# 你会画小猪佩奇吗？—— canvas

### 简介

Canvas 是 HTML5 新增的一个标签属性，一个可以使用脚本在其中绘制图像的 HTML 元素。它可以用来制作照片或者制作简单的动画，甚至可以进行实时的视频处理和渲染。Canvas 是由 HTML 代码配合高度和宽度属性而定义出的可绘制区域。JavaScript 代码可以访问该区域，类似于其他通用的 API，通过一套完整的绘图函数来动态生成图形。

### 应用场景

1. 游戏
2. 图表
3. 动画
4. HTML5 动效(codepen.io)

### 如何使用 Canvas

* 添加 `canvas` 标签

```
<canvas id="myCanvas" width="500" height="500"></canvas>
```

* 获得 `canvas` 元素

```
const canvas = document.getElementById("myCanvas");
```

* 获得 `canvas` 上下文对象

```
const ctx = canvas.getContext("2d");
// 有两个参数getContext()可以采取。一个是渲染2D元素的标准2d上下文。另一种使用的webGL技术仍处于起步阶段。因此，大多数情况下，您可以在任何地方找到第二个画布的上下文（截至目前）
```

#### 两个对象

1. 元素对象(canvas 元素)和上下文对象(通过`getContext('2d)`方法获取到的 `CanvasRenderingContext2D` 对象)
2. 元素对象相对于我们的画布，上下文对象相当于画笔，我们接下来的所有操作是基于上下文对象的。

#### 线段

```
ctx.moveTo(x, y); // 移动到x, y坐标点
ctx.lineTO(x, y); // 从当前点绘制直线到x, y点
ctx.stroke(); // 描边
ctx.lineWidth() = 20; // 设置线段宽度
ctx.closePath(); // 闭合当前路径 和 回到起始点是有区别的
ctx.fill(); // 填充
```

* 注意:
  * `fill` 和 `stroke`方法都是作用在当前的所有子路径
  * 完成一条路径后要重新开始另一条路径时必须使用 `beginPath()` 方法，beginPath 开始子路径的一个新的集合

**代码示例**

```
<canvas id="myCanvas" width="500" height="500"></canvas>
const canvas = document.getElementById("myCanvas");
const ctx = canvas.getContext("2d");
ctx.lineWidth = 20;
ctx.moveTo(100, 50);
ctx.lineTo(100, 100);
ctx.lineTo(50, 100);
ctx.closePath();
ctx.strokeStyle = "red";
ctx.stroke();
ctx.moveTo(200, 200);
ctx.strokeStyle = "#000";
ctx.stroke();
```

![mhxZM4.png](https://s2.ax1x.com/2019/08/27/mhxZM4.png)

#### 矩形

```
// 绘制矩形路径 左上角坐标为(x, y)，宽 高为dx dy。
ctx.rect(x, y, dx, dy);
// 填充向一个矩形区域填充颜色。 左上角坐标为(x, y)，宽 高为dx dy。
ctx.fillRect(x, y, dx, dy);
// 绘制一个矩形区域的边框。 左上角坐标为(x, y)，宽 高为w h。
ctx.strokeRect(x, y, w, h);
// 擦除指定矩形区域的像素颜色，等同于把早先的绘制效果都去除。
ctx.clearRect(x, y, dx, dy);
```

**代码示例**

```
<canvas id="myCanvas" width="500" height="500"></canvas>
const canvas = document.getElementById("myCanvas");
const ctx = canvas.getContext("2d");

let y = 100;
function draw(y) {
  ctx.fillRect(100, y, 20, 20);
}
const timer = setInterval(() => {
  ctx.clearRect(0, 0, 500, 500);
  draw(y);
  y += 10;
  if (y > 480) {
    clearInterval(timer);
  }
}, 50);
```

![mhzstx.gif](https://s2.ax1x.com/2019/08/27/mhzstx.gif)

#### 弧形

```
arc(x, y, r, 起始弧度, 结束弧度, 弧形的方向); // 弧形方向为 true 逆时针， false 顺时针
// 角以弧度计
Math.PI / 180 * angle ==> 弧度
```

**代码示例**

```
<canvas id="myCanvas" width="500" height="500"></canvas>
const canvas = document.getElementById("myCanvas");
const ctx = canvas.getContext("2d");
// 圆形
ctx.beginPath();
ctx.arc(100, 100, 50, 0, (Math.PI / 180) * 360, true);
ctx.stroke();
ctx.closePath();
// 小扇形
ctx.beginPath();
ctx.moveTo(250, 100);
ctx.arc(250, 100, 50, 0, (Math.PI / 180) * 45, false);
ctx.lineTo(250, 100);
ctx.stroke();
ctx.closePath();
// 大扇形
ctx.beginPath();
ctx.moveTo(200, 200);
ctx.arc(200, 200, 50, 0, (Math.PI / 180) * 45, true);
ctx.lineTo(200, 200);
ctx.stroke();
ctx.closePath();
```

![m4pJQU.png](https://s2.ax1x.com/2019/08/27/m4pJQU.png)

#### 圆角

```
ctx.arcTo(x1, y1, x2, y2, r); // 绘制的弧线与当前点和 x1, y1 和 x2, y2连接都相切
```

**代码示例**

```
<canvas id="myCanvas" width="500" height="500"></canvas>
const canvas = document.getElementById("myCanvas");
const ctx = canvas.getContext("2d");
// 绘制圆角矩形
ctx.beginPath();
ctx.moveTo(150, 50);
ctx.arcTo(200, 50, 200, 150, 30);
ctx.arcTo(200, 150, 100, 150, 30);
ctx.arcTo(100, 150, 100, 50, 30);
ctx.arcTo(100, 50, 200, 50, 30);
ctx.closePath();
ctx.stroke();
```

![m4PJQf.png](https://s2.ax1x.com/2019/08/27/m4PJQf.png)

#### 贝塞尔曲线

```
// 二次贝塞尔曲线
// x1, y1 控制点 ex, ey 结束点
quadraticCurveTo(x1, y1, ex, ey);
// 三次贝塞尔曲线
// x1, y1, x2, y2 控制点 ex, ey 结束点
bezierCurveTo(x1, y1, x2, y2, ex, ey);
```

#### 坐标轴转换

```
translate(dx, dy); // 重新映射画布上的(0, 0)位置
scale(sx, sy); // 缩放当前绘图
rotate(Math.PI); // 旋转当前的绘图
save(); // 保存当前图像状态的一份拷贝
restore(); // 从栈中弹出存储的图像状态并恢复
setTransform(a, b, c, d, e, f); // 先重置再变换
// 参数：水平旋转 水平倾斜 垂直倾斜 垂直缩放 水平移动 垂直移动
transform(a, b, c, d, e, f); // 在之前的基础上变换
```

#### 填充图案

```
createPattern(image, "repeat | repeat-x | repeat-y | no-repeat");
```

#### 渐变

```
// 必须在填充渐变的区域里定义渐变，否则没效果
// 线性渐变
createLinearGradient(x1, y1, x2, y2);
// 径向渐变
createRedialGradient(x1, y1, x2, y2);
bg.addColorStop(p, color);
```

**代码示例**

```
<canvas id="myCanvas" width="500" height="500"></canvas>
const canvas = document.getElementById("myCanvas");
const ctx = canvas.getContext("2d");
// 线性渐变
let gradient = ctx.createLinearGradient(0, 0, 200, 0);
gradient.addColorStop(0, "green");
gradient.addColorStop(1, "white");
ctx.fillStyle = gradient;
ctx.fillRect(10, 10, 200, 100);
// 径向渐变
let gradient1 = ctx.createRadialGradient(280, 280, 50, 300, 300, 200);
gradient1.addColorStop(0, "white");
gradient1.addColorStop(1, "green");
ctx.fillStyle = gradient1;
ctx.fillRect(200, 200, 300, 300);
```

![m4mVSJ.png](https://s2.ax1x.com/2019/08/27/m4mVSJ.png)

#### 阴影

```
ctx.shadowColor; // 阴影的颜色，默认为black
ctx.shadowOffsetX; // 阴影的水平位移，默认为0
ctx.shadowOffsetY; // 阴影的垂直位移，默认为0
ctx.shadowBlur; // 阴影的模糊程度，默认为0
// 这里的阴影偏移量不受坐标系变换的影响
```

#### 文本

```
fillText(); // 在指定位置绘制实心字符
strokeText(); // 在指定位置绘制空心字符
ctx.font; // 指定字型大小和字体，默认值为 10px sans-serif
ctx.textAlign; // 文本的对齐方式，默认值为start
ctx.direction; // 文本的方向，默认值为inherit
ctx.textBaseline; // 文本的垂直位置，默认值为alphabetic
```

![m4ng8e.png](https://s2.ax1x.com/2019/08/27/m4ng8e.png)

**代码示例**

```
<canvas id="myCanvas" width="500" height="500"></canvas>
const canvas = document.getElementById("myCanvas");
const ctx = canvas.getContext("2d");
ctx.font = "Bold 20px Arial";
ctx.strokeText("Hello world", 50, 50);
ctx.fillText("Hello world", 100, 100);
```

![m4KZlQ.png](https://s2.ax1x.com/2019/08/27/m4KZlQ.png)

#### 线段样式

* lineGap 指定线条末端的样式
  * butt（默认值，末端为矩形）
  * round（末端为圆形）
  * square（末端为突出的矩形，矩形宽度不变，高度为线条宽度的一半）。
* lineJoin 指定线段交点的样式
  * round（交点为扇形）
  * bevel（交点为三角形底边）
  * miter（默认值，交点为菱形)
* ctx.miterLimit 指定交点菱形的长度，默认为 10

当 lineJoin 是 miter 时，用于控制斜接部分的长度，如果斜接长度超过 miterLimit 的值，变成 bevel

**代码示例**

```
<canvas id="myCanvas" width="500" height="500"></canvas>
const canvas = document.getElementById("myCanvas");
const ctx = canvas.getContext("2d");
ctx.lineWidth = 20;
ctx.lineJoin = "miter";
ctx.miterLimit = 20;
ctx.moveTo(100, 100);
ctx.lineTo(200, 100);
ctx.lineTo(100, 110);
ctx.closePath();
ctx.stroke();
```

![m411b9.png](https://s2.ax1x.com/2019/08/27/m411b9.png)

#### 裁剪

```
// 当前路径外的区域不在绘制
ctx.clip();
// 可在clip() 前用save()方法保存，后续通过restore()方法恢复
```

**代码示例**

```
<canvas id="myCanvas" width="500" height="500"></canvas>
const canvas = document.getElementById("myCanvas");
const ctx = canvas.getContext("2d");
ctx.beginPath();
ctx.arc(200, 200, 50, 0, Math.PI * 2, 0);
ctx.closePath();
ctx.clip();
ctx.fillRect(100, 100, 300, 300);
ctx.strokeRect(100, 100, 400, 400);
```

![m48o90.png](https://s2.ax1x.com/2019/08/27/m48o90.png)

#### 全局透明度

```
ctx.globalAlpha = "0.5";
```

#### 绘制图片

```
// 第一个参数是img(image,canvas,video)
// 要在 onload 后再绘制
ctx.drawImage();
// 3 个参数
// 起始点坐标
x, y;
// 5个参数
// 起始点坐标及图片所存区域的宽高
x, y, dx, dy;
// 9个参数
// 前四个为所绘制目标元素的起始点和宽高，后四个为canvas绘制的起始点和大小
x1, y1, dx1, dy1, x2, y2, w2, h2;
let img = new Image();
img.onload = function() {
  ctx.drawImage();
};
img.src = "";
```

[Canvas 最佳实践（性能篇）](http://taobaofed.org/blog/2016/02/22/canvas-performance/)

### 如何解决 Canvas 高分屏模糊问题

在分辨率比较高的屏幕，例如 ip6/6s/mac 等机器上，因为 canvas 绘制的是位图，所以会导致模糊，解决方法是根据屏幕分辨率修改 canvas 样式代码中的宽和高与 canvas 的 width 和 height 属性的比例
