# 想炫吗？—— CSS3 属性

### 什么是 Css3.0

![ML8Cj0.png](https://s2.ax1x.com/2019/11/24/ML8Cj0.png)

Css3 是 Css2 的升级版本，3 只是版本号，它在 Css2.1 的基础上增加了很多强大的新功能。

### Css3 前缀

在编写 Css3 样式时，不同的浏览器可能需要不同的前缀。他表示该 Css 属性或规则尚未成为 W3C 标准的一部分，是浏览器的私有属性。

| 前缀       | 浏览器             |
| -------- | --------------- |
| -webkit- | Chrome 和 Safari |
| -moz-    | Firefox         |
| -ms-     | IE              |
| -o-      | Opera           |

### Css3 的功能

提供了更加强大且精准的选择器，提供了多种背景填充方案，可以实现渐变颜色，可以改变元素的形状、角度等，可以加阴影效果，报纸布局，弹性盒子，ie6 混杂模式的盒模型，新的计量单位，动画效果等等…

![ML8Qu6.png](https://s2.ax1x.com/2019/11/24/ML8Qu6.png)

#### CSS 新特性

* transition：过渡
* transform：旋转、缩放、移动或者倾斜
* animation：动画
* gradient：渐变
* shadow：阴影
* border-radius：圆角

#### border-radius 圆角

**代码示例**

```
<div class="border integrated"></div>
<div class="border individual"></div>
.border {
  width: 200px;
  height: 100px;
  background: red;
  margin: 30px;
}
.integrated {
  /*  整合属性设置  */
  /* 设置圆角 顺序为 左上 右上 右下 左下   */
  border-radius: 10px 100px 30px 50px;
}

.individual {
  /*  单个属性设置  */
  border-top-left-radius: 10px;
  border-top-right-radius: 100px 50px;
  border-bottom-right-radius: 30px 10px;
  border-bottom-left-radius: 50px;
}
```

![mE62B4.png](https://s2.ax1x.com/2019/08/15/mE62B4.png)

#### box-shadow 盒子阴影

以前没有 Css3 的时候，或者需要兼容低版本浏览器的时候，阴影只能用图片实现，但是现在不需要，Css3 就实现了

**代码示例**

```
<div class="shandow individual">individual</div>
<div class="shandow integrated">integrated</div>
.shandow {
  width: 200px;
  height: 100px;
  background: red;
  margin: 30px;
}

.individual {
  /*   X轴偏移量 Y轴偏移量 [阴影模糊半径] [阴影扩展半径] [阴影颜色] [投影方式 默认是从里往外，设置inset就是从外往里]; */
  box-shadow: 8px 10px 5px 1px #666;
}

.integrated {
  /*  同一盒子，可以同时加多个阴影，阴影之间用“,”隔开  */
  box-shadow: 4px 2px 6px #f00, -4px -2px 10px #000,
    0px 0px 12px 5px #33cc00 inset;
}
```

![mEcsIA.png](https://s2.ax1x.com/2019/08/15/mEcsIA.png)

#### text-shadow 文字阴影

**代码示例**

```
<div class="text">Text-Shadow</div>
.text {
  font-size: 20px;
  /*  text-shadow: X-Offset Y-Offset blur color */
  text-shadow: 10px 5px 2px #ccc;
}
```

![mEgmod.png](https://s2.ax1x.com/2019/08/15/mEgmod.png)

#### RGBA 颜色值

`RGB`是一种色彩标准，是由红(R)、绿(G)、蓝(B)的变化以及相互叠加来得到各式各样的颜色。`RGBA` 是在 `RGB` 的基础上增加了控制 `alpha` 的透明度的参数。

```
/* 语法 */
color: rgba(R, G, B, A);
```

以上 R、G、B 三个参数，正整数值的取值范围为:0-255。百分数值的取值范围为:0.0%-100.0%。超出范围的数值将被截至其最接近的取值极限。\*\*并非所有浏览器都支持使用百分数值。\*\*A 为透明度，取值在 0\~1 之间，不可为负值。

**代码示例**

```
<div class="rgba"></div>
.rgba {
  width: 100px;
  height: 100px;
  background-color: rgba(200, 50, 50, 0.5);
}
```

![mE2wAH.png](https://s2.ax1x.com/2019/08/15/mE2wAH.png)

#### gradient 背景渐变

Css 渐变分为两种:**线性渐变(`linear-to`)和径向渐变(radial-at)**

**线性渐变**

```
/* 语法 */
linear-gradient([direction], color [percent], color [percent],...)
[] 内为选填
direction 角度的单位为"deg"也可以用 'to bottom','to left','to top left'等的方式来表达
```

**代码示例**

```
<div class="linear"></div>
.linear {
  width: 100px;
  height: 100px;
  background: -webkit-linear-gradient(
    to bottom,
    red,
    blue
  ); /* Safari 5.1 - 6.0 */
  background: -o-linear-gradient(to bottom, red, blue); /* Opera 11.1 - 12.0 */
  background: -moz-linear-gradient(to bottom, red, blue); /* Firefox 3.6 - 15 */
  background: linear-gradient(to bottom, red, blue); /* 标准的语法 */
}
```

![mEo0pQ.png](https://s2.ax1x.com/2019/08/15/mEo0pQ.png)

**径向渐变**

```
/* 语法 */
radial-gradient(shape at position, color [percent] , color,...)
shape: 放射的形状，可以为圆形 circle，可以为椭圆 ellipse
position: 圆心的位置，可以两个值，也可以一个
          如果为一个时，第二个值默认为center，即 50%
          值类型可以为:百分数，距离像素，也可以是方位值(left，top..)
```

**代码示例**

```
<div class="radial"></div>
.radial {
  width: 100px;
  height: 100px;
  background: -webkit-radial-gradient(circle, pink, red); /* Safari 5.1 - 6.0 */
  background: -o-radial-gradient(circle, pink, red); /* Opera 11.1 - 12.0 */
  background: -moz-radial-gradient(circle, pink, red); /* Firefox 3.6 - 15 */
  background: radial-gradient(circle, pink, red); /* 标准的语法 */
}
```

![mEbiAx.png](https://s2.ax1x.com/2019/08/15/mEbiAx.png)

#### word-wrap 文字边界换行

```
/* 语法  break-word 允许长单词或url地址换行 
        normal(默认) 不允许长单词或url地址换行*/
word-wrap: normal | break-word;
```

**代码示例**

```
<div class="word">This is a long url : http://www.baidu.com.</div>
.word {
  width: 100px;
  border: 1px solid;
  /* break-word 允许长单词或url地址换行  */
  word-wrap: break-word;
}
```

![mnPOns.png](https://s2.ax1x.com/2019/08/17/mnPOns.png)

#### font-face 字体格式

**代码示例**

```
@font-face {
  font-family: 'myFirstFont';
  src: url('diyfont.eot'); /* IE9+ */
  src: url('diyfont.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */
      url('diyfont.woff') format('woff'),
    /* chrome、firefox */ url('diyfont.ttf') format('truetype'), /* chrome、firefox、opera、Safari, Android, iOS 4.2+*/
      url('diyfont.svg#fontname') format('svg'); /* iOS 4.1- */
  /* format 此值指的是你自定义的字体的格式
      主要用来帮助浏览器识别浏览器对 @font-face的兼容问题 
      因为不同的浏览器对字体格式支持的不一致 
      浏览器自身也无法通过路径来判断字体
    */
}
p {
  font-family: 'myFirstFont';
}
```

#### border-image 边框应用背景

**注意: Internet Explorer 不支持 border-image 属性。**

```
/* 语法 */
/* stretch 就是拉伸，有多长拉多长 (有多远"滚多远")*/
border-image:url(xxx.png) number stretch
/* repeat 和4角上同等大小的图片进行平铺
          当边框中间区域长度不是4角图片大小的整数倍时，会被切割 */
border-image:url(xxx.png) number repeat
/* round 4角上的图片，进行拉伸平铺，不会被切割 */
border-image:url(xxx.png) number round
```

number 为截取指定图片四周的宽度作为 `border` 的背景填充部分(截取图可按 `border-width` 大小伸缩) number 为一个数字时是复合写法 最后一个属性为 `border-image` 的展示策略

**代码示例**

```
<div class="round">这里，图像进行拉伸平铺来填充该区域。</div>
<br />
<div class="stretch">这里，图像被拉伸以填充该区域。</div>
<br />
<div class="repeat">
  这里，图像被平铺以填充该区域。当边框中间区域长度不是4角图片大小的整数倍时，会被切割
</div>
div {
  border: 15px solid transparent;
  width: 250px;
  padding: 10px 20px;
}
.round {
  border-image: url(https://www.runoob.com/images/border.png) 20 30 round;
}
.stretch {
  border-image: url(https://www.runoob.com/images/border.png) 10 20 stretch;
}
.repeat {
  border-image: url(https://www.runoob.com/images/border.png) 20 repeat;
}
```

![mnEvfP.png](https://s2.ax1x.com/2019/08/17/mnEvfP.png)

#### background-origin 背景图片起始位置

```
/* 语法 */
background-origin: border-box | padding-box | content-box;
/* 参数分别表示背景图片是从边框，   内边距(默认值)，  内容区域开始显示 */
```

**代码示例**

```
<div class="border-box">这里，表示背景图片是从边框</div>
<br />
<div class="padding-box">这里，表示背景图片是从内边距()默认</div>
<br />
<div class="content-box">这里，表示背景图片是从内容区域</div>
div {
  border: 10px solid;
  color: red;
  width: 100px;
  padding: 35px;
  background-image: url('https://s2.ax1x.com/2019/08/17/mnZK8P.th.png');
  background-size: 50px;
  background-repeat: no-repeat;
  background-position: left;
}

.border-box {
  background-origin: border-box;
}

.padding-box {
  background-origin: padding-box;
}

.content-box {
  background-origin: content-box;
}
```

![mnZORP.png](https://s2.ax1x.com/2019/08/17/mnZORP.png)

#### background-clip 裁剪背景

```
/* 语法 */
background-clip: border-box | padding-box | content-box | no-clip;
/* 参数分别表示从 边框 内填充 内容区域向外 裁剪背景
    no-clip 表示不裁剪，和参数 border-box 显示同样的效果
    background-clip 默认值为 border-box */
background-clip: text;
/* 从前景内容的形状(比如文字)作为裁剪区域向外裁剪，
    如此可实现使用背景作为填充色之类的遮罩效果 
*/
/* 注意：webkit 独有属性，且必须配合 text-fill-color 属性 */
```

**代码示例**

```
<div class="border-box">这里，表示是从边框开始裁剪</div>
<br />
<div class="padding-box">这里，表示是从内边距裁剪</div>
<br />
<div class="content-box">这里，表示是从内容区域裁剪</div>
<br />
<div class="text">这里，表示是从文字区域裁剪</div>
div {
  border: 10px dotted black;
  width: 100px;
  padding: 35px;
  background: skyblue;
}

.border-box {
  background-clip: border-box;
}

.padding-box {
  background-clip: padding-box;
}

.content-box {
  background-clip: content-box;
}

.text {
  font-weight: 900;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-fill-color: -webkit-background-clip;
  -webkit-background-clip: text;
}
```

![mnmH3t.png](https://s2.ax1x.com/2019/08/17/mnmH3t.png)

#### background-size 背景图片尺寸

设置背景图片的大小，以长度值或百分比显示，还可以通过 cover 和 contain 来对图片进行伸缩。

```
/* 语法 */
background-size: auto | 长度值 | 百分比 | cover | contain;
/* auto : 默认值，比改变背景图片的原始高度和宽度 */
/* 长度值 : 成对出现(200px 50px),将背景图片宽高依次设置为前面两个值
            当设置一个值时，将其作为图片宽度值来等比缩放 */
/* 百分比 : 0% ~ 100% 之间的任何值
            将背景图片宽高依次设置为所在元素宽高乘以前面百分比得出的数值 */
/* cover : 用一种张图片铺满整个背景
            如果比例不符，则截断图片 */
/* contain : 尽量让背景内，存在一张完整的图片 */
```

**代码示例**

```
<div class="size">这里，表示是使用长度值</div>
<br />
<div class="percentage">这里，表示使用百分比</div>
<br />
<div class="cover">这里，表示使用cover</div>
<br />
<div class="contain">这里，表示使用contain</div>
div {
  border: 1px dotted black;
  width: 200px;
  height: 100px;
  color: red;
  background: url('https://s2.ax1x.com/2019/06/23/ZCKwcV.th.jpg');
}

.size {
  background-size: 50px 50px;
}

.percentage {
  background-size: 80%;
}

.cover {
  background-size: cover;
}

.contain {
  background-size: contain;
}
```

![mnKxED.png](https://s2.ax1x.com/2019/08/17/mnKxED.png)

#### transparent 透明色颜色值

使用 `transparent` 属性画三角

**代码示例**

```
<div class="transparent"></div>
div {
  border: 100px solid transparent;
  width: 0;
  height: 0;
}
.transparent {
  border-top-color: #ccc;
}
```

![mnMWRA.png](https://s2.ax1x.com/2019/08/17/mnMWRA.png)

#### -webkit-box-reflect 反射倒影

```
/* 语法 */
-webkit-box-reflect: 方向[ above-上 | below-下 | right-右 | left-左
  ]，偏移量，遮罩图片;
```

**代码示例**

```
<div class="below"></div>
<div class="right"></div>
div {
  width: 160px;
  height: 160px;
  float: left;
  background: url('https://s2.ax1x.com/2019/06/23/ZCKwcV.th.jpg');
}
.below {
  /* 下倒影，渐变 */
  -webkit-box-reflect: below 0 linear-gradient(transparent, white);
}
.right {
  /* 右倒影 */
  -webkit-box-reflect: right;
}
```

![mn1ADx.png](https://s2.ax1x.com/2019/08/17/mn1ADx.png)

#### filter 滤镜

filter 属性定义了元素(通常是`<img>`)的可视效果(例如：模糊与饱和度)。

```
/* 语法 */
filter: none | blur() | brightness() | contrast() | drop-shadow() | grayscale() |
  hue-rotate() | invert() | opacity() | saturate() | sepia() | url();
```

| Filter                                            | 描述                                                                                |
| ------------------------------------------------- | --------------------------------------------------------------------------------- |
| none                                              | 默认值 没有效果                                                                          |
| blur(px)                                          | **给图像设置高斯模糊 ，值越大越模糊** 如果没有给定值，默认为 0 可以设置 css 长度值，不接受 百分比                          |
| brightness(%)                                     | 给图像设置亮度，值越小越黑，越大越亮，100% 无变化 如果没有给定值，默认是 1                                         |
| contrast(%)                                       | 调整图像的对比度。值是 0% 图像全黑；值是 100% 图像不变 可以超过 100% 如果没有给定值，默认是 1                          |
| drop-shadow (h-shadow v-shadow blur spread color) | 给图像设置一个阴影效果 **函数接收 类型的值，除了 “inset” 关键字是不允许的。** **通过滤镜，一些浏览器为了更好的性能会提供硬件加速**       |
| grayscale(%)                                      | 将图像转换为灰度图像 值为 100% 则完全转为灰度图像，值为 0% 图像无变化 若未设置，值默认是 0                              |
| hue-rotate(deg)                                   | 给图像应用色相旋转 值为 0deg，则图像无变化 若值未设置，默认值为 0deg                                          |
| invert(%)                                         | 反转输入图像，值定义转换的比例 100% 则完全反转，0% 则图像无变化 若值未设置，值默认是 0                                 |
| opacity(%)                                        | 转换图像的透明度 **该函数和已有的 opacity 属性很相似，不同之处在于通过 filter，一些浏览器为了提升性能会提供硬件加速**             |
| sepia(%)                                          | 将图像转为深褐色。 值为 100% 则完全是深褐色，值为 0% 图像无变化。 若未设置，默认为 0                                 |
| url()                                             | 接受一个 XML 文件，该文件设置了一个 SVG 滤镜，且可以包含一个锚点来指定一个具体的滤镜元素。 例如: `filter:url(svg-url#demo)` |

**代码示例**

最后，来欣赏一下 Css3.0 滤镜的强大

```
<p>原图</p>
<img src="https://s2.ax1x.com/2019/08/17/mnNFYV.png" />
<p>黑白色filter: grayscale(100%)</p>
<img
  src="https://s2.ax1x.com/2019/08/17/mnNFYV.png"
  style="filter: grayscale(100%);"
/>
<p>褐色filter:sepia(1)</p>
<img src="https://s2.ax1x.com/2019/08/17/mnNFYV.png" style="filter:sepia(1);" />
<p>饱和度saturate(2)</p>
<img
  src="https://s2.ax1x.com/2019/08/17/mnNFYV.png"
  style="filter:saturate(2);"
/>
<p>色相旋转hue-rotate(90deg)</p>
<img
  src="https://s2.ax1x.com/2019/08/17/mnNFYV.png"
  style="filter:hue-rotate(90deg);"
/>
<p>反色filter:invert(1)</p>
<img
  src="https://s2.ax1x.com/2019/08/17/mnNFYV.png"
  style="filter:invert(1);"
/>
<p>透明度opacity(.5)</p>
<img
  src="https://s2.ax1x.com/2019/08/17/mnNFYV.png"
  style="filter:opacity(.5);"
/>
<p>亮度brightness(.5)</p>
<img
  src="https://s2.ax1x.com/2019/08/17/mnNFYV.png"
  style="filter:brightness(.5);"
/>
<p>对比度contrast(2)</p>
<img
  src="https://s2.ax1x.com/2019/08/17/mnNFYV.png"
  style="filter:contrast(2);"
/>
<p>模糊blur(3px)</p>
<img
  src="https://s2.ax1x.com/2019/08/17/mnNFYV.png"
  style="filter:blur(3px);"
/>
<p>阴影drop-shadow(5px 5px 5px #000)</p>
<img
  src="https://s2.ax1x.com/2019/08/17/mnNFYV.png"
  style="filter:drop-shadow(5px 5px 5px #000);"
/>
```

[![mnafzT.md.png](https://s2.ax1x.com/2019/08/17/mnafzT.md.png)](https://imgchr.com/i/mnafzT)
