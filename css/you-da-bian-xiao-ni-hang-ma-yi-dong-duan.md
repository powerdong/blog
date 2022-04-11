# 由大变小，你行吗 —— 移动端



响应式布局指的是同一个页面在不同屏幕尺寸下有不同的布局。传统的开发方式是 PC 端开发一套，手机端再开发一套，而使用响应式布局只要开发一套就够，缺点是`css`比较重。对于不同的屏幕大小，大致可分为`iPhone5/SE`,`iphone6/7/8`,`iphone 6/7/8 plus`,`ipad pro`,`dell台式宽屏(1440 X 900)`。

* 响应式设计与自适应设计的区别
  * 响应式开发一套界面，通过监测视口分辨率，针对不同客户端在客户端做代码处理，来展示不同的布局和内容
  * 自适应需要开发多套界面，通过检测视口分辨率，来判断当前访问的设备是 PC 端、平板、手机，从而请求服务层，返回不同的页面。

![ZqJWJH.jpg](https://s2.ax1x.com/2019/07/17/ZqJWJH.jpg)

### 媒体查询

> 为了更好的体验，是向不同设备提供不同的样式的一种方式，它为每种类型的用户提供了最佳的体验

#### 为什么会有媒体查询

移动设备的快速普及完全颠覆了 Web 设计领域

用户不再仅在用户不再仅在传统桌面系统上查看 Web 内容，他们越来越多地使用具有各种尺寸的智能电话、平板电脑和其他设备。

Web 设计人员的挑战是确保他们的网站不仅在大屏幕上看起来不错，在小型的电话以及介于它们之间的各种设备上看起来也不错。

### 物理像素和设备独立像素

![MLM7ee.png](https://s2.ax1x.com/2019/11/24/MLM7ee.png)

#### 物理像素

一个物理像素是显示器(手机屏幕)上最小的物理显示单元，在操作系统的调度下，每一个设备像素都有自己的颜色值和亮度值

#### 设备独立像素

设备独立像素(也叫目睹无关像素)，可以认为是计算机坐标系统中的一个点，这个点代表一个可以有程序使用的虚拟像素(Css 像素)，然后由相关系统转换为物理像素

#### 设备像素比

设备像素比(device pixel ratio) 设备像素比(简称 dpr)定义了物理像素和设备独立像素的对应关系，它的值可以按如下的公式得到

> 设备像素比 = 物理像素 / 设备独立像素

JavaScript 中可以通过`window.devicePixelRatio`得到

#### 例子

以 iPhone6 为例

* 设备宽高为 375 \* 667，可以理解为设备独立像素(Css 像素)
* dpr 为 2，根据上面的计算公式，其物理像素就应该 \_ 2，为 750 \_ 1334

1 Css 像素在 PC 端显示器需要用(`1*1`)个栅格点表示，在 iPhone6 中则需要 4(`2*2`)个

也就是在不同的屏幕上，**(普通屏幕 VS retina 屏幕)，Css 像素所呈现的大小(物理尺寸)是一致的，不同的是一个 Css 像素所对应不同的对象的物理像素个数是不一致的**

在普通的屏幕下，1 个 Css 像素对应的 1 个物理像素为 1:1。在 retina 屏幕下，1 个 Css 像素对应 4 个物理像素 1:4

#### 位图像素

1 个位图像素是栅格图像（如：png，jpg，gif 等）最小的数据单元。每一个位图像素都包含这一些资深的现实信息（如：显示位置，颜色值，透明度等）

在普通屏幕下是没问题的，但是在 retina 屏幕下就会出现位图像素点不够，从而导致图片模糊的情况

**对于 dpr = 2 的, 1 个位图像素对应于 4 个物理像素，由于单个位图像素不以再进一步分割，所以只能就近取色，从而导致图片模糊**

**解决办法使用分辨率大两倍的图片** 如 `200*300` img 标签，就需要提供 `400*600` 的图片.由此一来位图像素点的个数是原来的 4 倍，在 retina 屏幕下，位图像素点个数就可以物理像素点个数形成 1:1 的比例，图片自然就清晰了。

**这里还有另一个问题，如果如果普通屏幕下，也用了两倍图片，会怎样呢？** 很明显，在普通屏幕下，200×300(px) img 标签，所对应的物理像素个数就是 200×300 个，而两倍图片的位图像素个数则是 200×300\*4，所以就出现一个物理像素点对应 4 个位图像素点，所以它的取色也只能通过一定的算法(显示结果就是一张只有原图像素总数四分之一，我们称这个过程叫做 downsampling)，肉眼看上去虽然图片不会模糊，但是会觉得图片缺少一些锐利度，或者是有点色差(但还是可以接受的)。

### 响应式布局的实现步骤

#### 设置 Meta 标签

大多数移动浏览器将 HTML 页面放大为宽的视图(viewport)以符合屏幕分辨率。你可以使用视图的`meta`标签来进行重置。下面的视图标签告诉浏览器，使用的设备的宽度作为视图宽度，并禁止初始的缩放。在`<head>`标签里加入这个`meta`标签。

```
<meta
  name="viewport"
  content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no"
/>
<!-- user-scalable = no 属性能够解决 iPad 切换横屏之后触摸才能回到具体尺寸的问题。 -->
```

#### 设置媒体查询

`css3`媒体查询可以让我们针对不同的媒体类型定义不同的样式，当重置浏览器窗口大小的过程中，页面也会根据浏览器的宽度和高度重新渲染页面。

**如何选择屏幕大小的分割点**

如果我们选择`600px`,`900px`,`1200px`,`1800px`作为分割点，可以适配到常见的 14 个机型： ![ZqYLB6.png](https://s2.ax1x.com/2019/07/17/ZqYLB6.png) 当然这只是其中的一种分割方案，我们还可以这样划分：`480px`,`800px`,`1400px`。 ![ZqYjAO.jpg](https://s2.ax1x.com/2019/07/17/ZqYjAO.jpg) 而作为曾经典型的响应式布局框架，Bootstrap 是怎么进行断点的呢？ ![ZqYOHK.jpg](https://s2.ax1x.com/2019/07/17/ZqYOHK.jpg) Media Queries 是响应式设计的核心。

它根据条件告诉浏览器如何为指定视图宽度渲染页面。假如一个终端的分辨率小于`980px`,那么可以这样写

```
@media screen and (max-width: 980px) {
  #head {
    ...;
  }
  #content {
    ...;
  }
  #footer {
    ...;
  }
}
```

这里的样式就会覆盖上面已经定义好的样式。

**移动优先 OR PC 优先**

不管是移动优先还是 PC 优先，都是依据当随着屏幕宽度增大或减小的时候，后面的样式会覆盖前面的样式。因此，移动端优先首先使用的是`min-width`,PC 端优先使用的是`max-width`。

**移动优先：**

```
/* iphone6 7 8 */
body {
  background-color: yellow;
}
/* iphone 5 */
@media screen and (max-width: 320px) {
  body {
    background-color: red;
  }
}
/* iphoneX */
@media screen and (min-width: 375px) and (-webkit-device-pixel-ratio: 3) {
  body {
    background-color: #0ff000;
  }
}
/* iphone6 7 8 plus */
@media screen and (min-width: 414px) {
  body {
    background-color: blue;
  }
}
/* ipad */
@media screen and (min-width: 768px) {
  body {
    background-color: green;
  }
}
/* ipad pro */
@media screen and (min-width: 1024px) {
  body {
    background-color: #ff00ff;
  }
}
/* pc */
@media screen and (min-width: 1100px) {
  body {
    background-color: black;
  }
}
```

**PC 优先：**

```
/* pc width > 1024px */
body {
  background-color: yellow;
}
/* ipad pro */
@media screen and (max-width: 1024px) {
  body {
    background-color: #ff00ff;
  }
}
/* ipad */
@media screen and (max-width: 768px) {
  body {
    background-color: green;
  }
}
/* iphone6 7 8 plus */
@media screen and (max-width: 414px) {
  body {
    background-color: blue;
  }
}
/* iphoneX */
@media screen and (max-width: 375px) and (-webkit-device-pixel-ratio: 3) {
  body {
    background-color: #0ff000;
  }
}
/* iphone6 7 8 */
@media screen and (max-width: 375px) and (-webkit-device-pixel-ratio: 2) {
  body {
    background-color: #0ff000;
  }
}
/* iphone5 */
@media screen and (max-width: 320px) {
  body {
    background-color: #ff00ff;
  }
}
```

#### 百分比布局

通过百分比单位，可以使得浏览器中的宽和高随着浏览器的高度的变化而变化，从而实现响应式的效果。`CSS3`支持最大最小高，可以将百分比和`max(min)`一起结合使用来定义元素在不同设备下的宽高。

```
/* pc width > 1100px */
html,
body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}
aside {
  width: 10%;
  height: 100%;
  background-color: red;
  float: left;
}
main {
  height: 100%;
  background-color: blue;
  overflow: hidden;
}
/* ipad pro */
@media screen and (max-width: 1024px) {
  aside {
    width: 8%;
    background-color: yellow;
  }
}
/* ipad */
@media screen and (max-width: 768px) {
  aside {
    float: none;
    width: 100%;
    height: 10%;
    background-color: green;
  }
  main {
    height: calc(100vh - 10%);
    background-color: red;
  }
}
/* iphone6 7 8 plus */
@media screen and (max-width: 414px) {
  aside {
    float: none;
    width: 100%;
    height: 5%;
    background-color: yellow;
  }
  main {
    height: calc(100vh - 5%);
    background-color: red;
  }
}
/* iphoneX */
@media screen and (max-width: 375px) and (-webkit-device-pixel-ratio: 3) {
  aside {
    float: none;
    width: 100%;
    height: 10%;
    background-color: blue;
  }
  main {
    height: calc(100vh - 10%);
    background-color: red;
  }
}
/* iphone6 7 8 */
@media screen and (max-width: 375px) and (-webkit-device-pixel-ratio: 2) {
  aside {
    float: none;
    width: 100%;
    height: 3%;
    background-color: black;
  }
  main {
    height: calc(100vh - 3%);
    background-color: red;
  }
}
/* iphone5 */
@media screen and (max-width: 320px) {
  aside {
    float: none;
    width: 100%;
    height: 7%;
    background-color: green;
  }
  main {
    height: calc(100vh - 7%);
    background-color: red;
  }
}
```

我们需要弄清楚 css 中子元素的百分比到底是相对谁的百分比。

*   子元素的

    ```
    height
    ```

    或

    ```
    width
    ```

    中使用百分比，是相对于子元素的直接父元素，

    * `width`相对于父元素的`width`
    * `height`相对于父元素的`height`
* 子元素的`top`和`bottom`如果设置百分比，则相对于直接`非static`定位(默认定位)的父元素的高度
* 子元素的`left`和`right`如果设置百分比，则相对于直接`非static`定位(默认定位)的父元素的宽度
* 子元素的`padding`如果设置百分比，不论是垂直方向或是水平方向，都相对于直接父亲元素的`width`,与父元素的`height`无关。
* 子元素的`margin`如果设置百分比，不论是垂直方向或是水平方向，都相对于直接父亲元素的`width`,与父元素的`height`无关。
*   如果设置

    ```
    border-radius
    ```

    为百分比，则是相对于自身宽度

    * 还有`translate`、`background-size`等都是相对于自身的

使用百分比单位来实现响应式的布局，有明显的以下缺点

* 计算困难,如果我们要定义一个元素的宽度和高度，按照设计稿，必须换算成百分比单位。
* 各个属性中如果使用百分比，相对父元素的属性并不是唯一的。造成我们使用百分比单位容易使布局问题变得复杂。

#### rem 布局

`rem`是`css3`新增的单位，并且移动端的支持度很高，**Android2.x+,ios5+都支持。**`rem`单位都是相对根元素 html 的`font-size`来决定大小，根元素`font-size`相对于提供了一个基准，当页面的 size 发生变化时，只需要改变`font-size`的值，那么 rem 为固定单位的元素的大小也会发生响应的变化。**因此，如果通过`rem`来实现响应式的布局，只需要根据视图容器的大小，动态的改变`font-size`即可（而`em`是相对于父元素的）。**

**rem 响应式的布局思想：**

* 一般不要给元素设置具体的高度，但是对于一些小图标可以设定具体宽度值
* 高度值可以设置固定值，设计稿有多大，我们就严格多大
* 所有设置的固定值都用`rem`为单位

默认情况下我们的 html 标签的`font-size`为 16px;我们利用媒体查询，设置在不同设备下字体的大小。

```
/* pc width > 1100px */
html {
  font-size: 100%;
}
body {
  background-color: yellow;
  font-size: 1.5rem;
}
/* ipad pro */
@media screen and (max-width: 1024px) {
  body {
    background-color: #ff00ff;
    font-size: 1.4rem;
  }
}
/* ipad */
@media screen and (max-width: 768px) {
  body {
    background-color: green;
    font-size: 1.3rem;
  }
}
/* iphone6 7 8 plus */
@media screen and (max-width: 414px) {
  body {
    background-color: blue;
    font-size: 1.25rem;
  }
}
/* iphoneX */
@media screen and (max-width: 375px) and (-webkit-device-pixel-ratio: 3) {
  body {
    background-color: #0ff000;
    font-size: 1.125rem;
  }
}
/* iphone6 7 8 */
@media screen and (max-width: 375px) and (-webkit-device-pixel-ratio: 2) {
  body {
    background-color: #0ff000;
    font-size: 1rem;
  }
}
/* iphone5 */
@media screen and (max-width: 320px) {
  body {
    background-color: #0ff000;
    font-size: 0.75rem;
  }
}
```

#### 视口单位

`css3`中引入了另一个新的单位`vw/vh`，与视图窗口有关，`vw`表示相对于视图窗口的宽度，`vh`表示相对于视图窗口的高度，除了`vw`和`vh`外，还有`vmin`和`vmax`两个相关的单位

| 单位   | 含义                                   |
| ---- | ------------------------------------ |
| vw   | 相对于视窗的宽度，1vw 等于视口宽度的 1%，即视窗宽度是 100vw |
| vh   | 相对于视窗的高度，1vh 等于视口高度的 1%，即视窗高度是 100vh |
| vmin | vw 和 vh 中的较小值                        |
| vmax | vw 和 vh 中的较大值                        |

![ZqDLDJ.jpg](https://s2.ax1x.com/2019/07/17/ZqDLDJ.jpg) 用视口单位度量，视口宽度为 100vw，高度为 100vh（左侧为竖屏情况，右侧为横屏情况）。例如，在桌面端浏览器视口尺寸为 650px，那么 `1vw = 650 \* 1% = 6.5px`（这是理论推算的出，如果浏览器不支持 0.5px，那么实际渲染结果可能是 7px）。

> 任何层级元素，在使用单位`vw`单位的情况下，1vw 都等于视图宽度的百分之一。

**1. 仅使用 vw 作为 css 单位**

* 对于设计稿的尺寸转换为基准单位，我们使用`Sass`函数编译

```
/* iphone 6 尺寸作为设计稿基准 */
$vm_base: 375;
@function vw($px) {
  @return ($px / 375) * 100vw;
}
```

* 无论是文本还是布局宽度、间距等都使用 vw 作为单位

```
.mod_nav {
  background-color: #fff;
  &_list {
    display: flex;
    padding: vm(15) vm(10) vm(10); // 内间距
    &_item {
      flex: 1;
      text-align: center;
      font-size: vm(10); // 字体大小
      &_logo {
        display: block;
        margin: 0 auto;
        width: vm(40); // 宽度
        height: vm(40); // 高度
        img {
          display: block;
          margin: 0 auto;
          max-width: 100%;
        }
      }
      &_name {
        margin-top: vm(2);
      }
    }
  }
}
```

**2. 搭配 vw 和 rem**

虽然采用`vw`适配后的页面效果很好，但是它是利用视口单位实现的布局，依赖视口大小而自动缩放，无论视口过大还是过小，它也随着时候过大或者过小，失去了最大最小宽度的限制，此时我们可以结合`rem`来实现布局

* 给根元素大小设置随着视口变化而变化的 vw 单位，这样就可以实现动态改变其大小
* 限制根元素字体大小的最大最小值，配合 body 加上最大宽度和最小宽度

```
// rem 单位换算：定为 75px 只是方便运算，750px-75px、640-64px、1080px-108px，如此类推
$vm_fontsize: 75; // iPhone 6尺寸的根元素大小基准值
@function rem($px) {
     @return ($px / $vm_fontsize ) * 1rem;
}
// 根元素大小使用 vw 单位
$vm_design: 750;
html {
    font-size: ($vm_fontsize / ($vm_design / 2)) * 100vw;
    // 同时，通过Media Queries 限制根元素最大最小值
    @media screen and (max-width: 320px) {
        font-size: 64px;
    }
    @media screen and (min-width: 540px) {
        font-size: 108px;
    }
}
// body 也增加最大最小宽度限制，避免默认100%宽度的 block 元素跟随 body 而过大过小
body {
    max-width: 540px;
    min-width: 320px;
}
```

#### 图片自适应

```
html {
  /* 在320px 下 5vw == 16px  并且可以根据屏幕大小改变 */
  font-size: 5vw;
}

img {
  /* 在320px 下 8rem == 8 * 5vw = 128px 并且可以自适应 */
  width: 8rem;
  height: 8rem;
}
```

### 总结

* 响应式布局的实现可以通过
  * 媒体查询+px
  * 媒体查询+百分比
  * 媒体查询+rem+js
  * vm/vh +rem 这几种方式来实现
*   媒体查询需要

    选取主流设备宽度尺寸作为断点

    进行针对性写额外的样式进行适配

    * 这样做会比较麻烦，只能在选取的几个主流设备尺寸下呈现完美适配
    * 用户体验也不友好，布局在响应断点范围内的分辨率下维持不变
    * 在响应断点切换的瞬间，布局带来断层式的切换变化，如同卡带的唱机般“咔咔咔”地一下又一下。
*   通过百分比来适配首先是

    计算麻烦

    * 各个属性中如果使用百分比，其相对的元素的属性并不是唯一的
    * 造成我们使用百分比单位容易使布局问题变得复杂。
* 通过采用 rem 单位的动态计算的弹性布局
  * 则是需要在头部内嵌一段脚本来进行监听分辨率的变化来动态改变根元素字体大小
  * CSS 与 JS 耦合了在一起。
* 通过利用纯 css 视口单位实现适配的页面
  * 既能解决响应式断层问题
  * 又能解决脚本依赖的问题
  * 但是兼容性还没有完全能能够接受

> 安利一个可以自动转换 px 和 em 的网站[点我进入](https://pdf-lib.org/Tools/MediaQueries)
