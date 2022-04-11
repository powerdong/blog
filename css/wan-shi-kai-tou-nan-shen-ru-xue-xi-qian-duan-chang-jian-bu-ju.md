# 万事开头难？——深入学习前端常见布局

### Css3 布局

![MLlHPA.png](https://s2.ax1x.com/2019/11/24/MLlHPA.png)

#### display:flex 弹性盒子

flex 为复合属性，且必须配合父元素 `display: flex` 使用

> 弹性盒模型是 css3 的一种新的布局方式，是一种当前页面需要适应不同的屏幕大小及设备类型时确保拥有恰当的行为的布局方式。

![MLljr8.png](https://s2.ax1x.com/2019/11/24/MLljr8.png)

* **以下 6 个属性设置在项目(子元素)上**
  * flex-grow: 放大比例
    * 根据所设置的比例分配盒子所剩余的空间
    * 左右两栏布局
    * 默认值 0
  * flex-shrink: 缩小比例
    * 多出盒子的部分，按照比例的大小砍掉相应的大小
    * 即比例越大，被砍的越大
    * 默认值 1
  * flex-basis: 伸缩基准值(项目占据主轴的空间)
    * 该属性设置元素的宽度或高度，当然 width 也可以用来设置宽度
    * **如果元素上同时出现了 width 和 flex-basis，那么 flex-basis 会覆盖 width 的值**
  * flex: 是 flex-grow, flex-shrink 和 flex-basis 的简写
    * 常用简化写法: `flex: 1 => flex: 1 1 0%`
    * \*\*注意：\*\*flexbox 布局和原来的布局是两个概念，部分 Css 属性在 flexbox 盒子里面不起作用，(`float`、`clear`、`column`、`vertical-align`)等
  * order: 排列顺序
    * 数值越小，排列越靠前
    * 默认为 0
  * align-self: 单个项目对齐方式
    * 可覆盖 `align-items` 属性
    * 默认值为 auto，表示继承父元素的 `align-items` 属性，如果没有父元素，则等同于 `stretch`
    * align-self:auto | flex-start | flex-end | center | baseline | stretch
* 以下 6 个属性设置在容器上
  * flex-direction: 决定主轴的方向
    * row(默认值): 主轴为水平方向，起点在左端
    * row-revers: 主轴为水平方向，起点在右端
    * column: 主轴为垂直方向，起点在上沿
    * column-reverse: 主轴为垂直方向，起点在下沿
  * flex-wrap: 是否换行
    * 默认情况下，项目都排列在一条线(又称”轴线”)上
    * nowrap(默认): 不换行
    * wrap: 换行，第一行在上方
    * wrap-reverse: 换行，第一行在下方
  * flex-flow: flex-direction 和 flex-wrap 的简写
    * 默认值为 row nowrap
  * justify-content: 项目在主轴上的对齐方式(假设主轴为从左到右)
    * flex-start(默认值): 左对齐
    * flex-end: 右对齐
    * center: 居中
    * space-between: 两端对齐，项目之间的间隔都相等
    * space-around: 每个项目两侧的间隔相等。**项目之间的间隔比项目与边框的间隔大一倍**
  * align-items: 在侧轴上的对齐方式
    * flex-start: 交叉轴的起点对齐
    * flex-end: 交叉轴的终点对齐
    * center: 交叉轴的中点对齐
    * baseline: 项目第一行文字的基线对齐
    * stretch(默认值): **如果项目未设置高度或设置 auto**，将占满整个容器的高度
  *   align-content: 多跟轴线的对齐方式(

      当只有一根轴线时，不生效

      )

      * flex-start: 与交叉轴的起点对齐。
      * flex-end: 与交叉轴的终点对齐。
      * center: 与交叉轴的中点对齐。
      * space-between: 与交叉轴两端对齐，轴线之间的间隔平均分布。
      * space-around: 每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
      * stretch(默认值): 轴线占满整个交叉轴。

```
/* 溢出换行问题 */
flex-flow: flex-direction | flex-wrap flex-direction：row | row-reverse | column
  | column-reverse flex-wrap：nowrap（默认） | wrap | wrap-reverse;
```

当设置 flex-wrap 属性的时候 wrap 失效，flex-basis 尽可能按 basis 值往大了去从而达到折行的目的，flex-shrink 会失效(根据子元素实际的宽度判断是否折行)

**代码示例**

```
<div class="container">
  <div>1</div>
  <div>2</div>
  <div>3</div>
  <div>4</div>
  <div>5</div>
</div>
div.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 500px;
  /* flex-direction: row-reverse; */
  /* flex-wrap: wrap; */
  /* justify-content: space-around; */
  /* align-items: self-start; */
}
div.container > div {
  width: 200px;
  height: 200px;
  background-color: red;
  border: 1px solid #000;
  font-size: 20px;
  text-align: center;
  /* order: 0; */
  /* flex-grow: 0; */
  /* flex-basis: 200px; */
}
```

![mGFzIU.gif](https://s2.ax1x.com/2019/08/20/mGFzIU.gif)

#### columns 多列布局

为了能在 Web 页面中方便实现类似报纸、杂志那种多列排版的布局，W3C 特意给 Css3 增加了一个多列布局模块(Css Multi Column Layout Module)。它主要应用在文本的多列布局放面，这种布局在报纸和杂志上使用了几十年了，但要在 Web 页面上实现这样的效果还是有相当大的难度，庆幸的是，Css3 的多列布局可以轻松实现。

分栏布局 IE10+都可以使用，API 稳定，移动端兼容性比 flex 布局要好，虽然设计初衷不一样，但很多布局都可以实现。

**语法**

* columns: \[column-width]\[column-count]
  * column-width: 指每一列的宽度 根据容器宽度自适应(最小宽度)
  * column-count: 指规定的列数 唯一精准的列数
    * **注意：不要两个在一起使用 会乱！**
  * column-gap: 设置列与列之间的宽度，直接用数值表示即可 (10px)
    * 主要用来设置列与列之间的间距
    * 如果没有设置 gap 值，其值大小会根据浏览器默认的 font-size 来定
  * column-rule-: 不占用任何空间位置，在列与列之间改变其宽度不会改变任何列的位置
    * column-rule-width: 宽度
      * 类似于 `border-width`属性，主要用来定义列边框的宽度，其默认值为 “medium”
      * 接受任意浮点数，**但不接收负值**
      * 可以使用关键字: `medium`、 `thick`、 `thin`
    * column-rule-style: 样式
      * 类似于 `border-style` 属性，主要用来定义列边框样式，默认值为 `none`
      * 属性值包括`hidden`、`dotted`、`dashed`、`solid`、`double`、`groove`、`ridge`、`inset`、`outset`
    * column-rule-color: 颜色
      * 类似于 `border-color` 属性
  * column-span: 1/all: 设置多列布局元素内的子元素，可以跨列，类似标题效果。
    * **此属性要在子元素上设置**

**代码示例**

```
<div class="container">
  <div class="item">我是大标题</div>
  <div></div>
  <div></div>
  <div></div>
  <div></div>
  <div></div>
</div>
div.container {
  columns: 300px;
  column-gap: 10px;
  column-rule-width: 5px;
  column-rule-style: dashed;
  column-rule-color: green;
}
div.container > div {
  width: 200px;
  height: 200px;
  background-color: red;
  margin: 5px;
}
.item {
  column-span: all;
}
```

![m8ITaT.png](https://s2.ax1x.com/2019/08/20/m8ITaT.png)

#### 实现常用布局（三栏、圣杯、双飞翼、吸顶）

> 左右栏定宽 中间宽度自适应

**绝对定位 + `margin`**

```
<!-- html -->
<div class="container">
  <div class="left">这是左边的内容</div>
  <div class="middle">这是中间的内容</div>
  <div class="right">这是右边的内容</div>
</div>
<!-- css -->
<style type="text/css">
  .container .left,
  .container .right {
    position: absolute;
    top: 0;
    height: 300px;
    background-color: #f40;
    width: 300px;
  }
  .left {
    left: 0;
  }
  .right {
    right: 0;
  }
  .container .middle {
    height: 300px;
    background-color: #ccc;
    margin: 0 300px 0;
  }
</style>
```

![ZxI236.png](https://s2.ax1x.com/2019/07/20/ZxI236.png)

> 基于绝对定位的三栏布局：注意绝对定位的元素脱离文档流，相对于最近的已经定位的祖先元素进行定位。无需考虑 HTML 中结构的顺序

**缺点：有顶部对齐问题，需要进行调整，中间的高度为整个内容的高度**

**`float` + `margin`**

```
<!-- html -->
<div class="container">
  <div class="left">这是左边的内容</div>
  <div class="right">这是右边的内容</div>
  <div class="middle">这是中间的内容</div>
</div>
<!-- css -->
<style type="text/css">
  .container .left,
  .container .right {
    height: 300px;
    background-color: #f40;
    width: 300px;
  }
  .left {
    float: left;
  }
  .right {
    float: right;
  }
  .container .middle {
    height: 300px;
    background-color: #ccc;
    margin: 0 300px 0;
  }
</style>
```

![Zx7oE4.png](https://s2.ax1x.com/2019/07/20/Zx7oE4.png)

> 基于纯 float 实现的三栏布局需要将中间的内容放在 HTML 结构的最后，否则右侧会沉在中间内容的下侧
>
> 原理：元素浮动后，脱离文档流，后面的元素受浮动影响，设置受影响元素的 margin 值即可

**`flex` 布局**

```
<!-- html -->
<div class="container">
  <div class="left">这是左边的内容</div>
  <div class="middle">这是中间的内容</div>
  <div class="right">这是右边的内容</div>
</div>
<!-- css -->
<style type="text/css">
  .container {
    display: flex;
  }
  .container .left,
  .container .right {
    height: 300px;
    width: 300px;
    background-color: #f40;
  }
  .container .middle {
    background-color: #ccc;
    flex: 1;
  }
</style>
```

![ZxHJqU.png](https://s2.ax1x.com/2019/07/20/ZxHJqU.png)

> `flexbox`布局最简洁使用，并且没有明显的缺陷。

* 仅需将容器设置为`display:flex;`，两边的元素定宽定高，将中间的元素设置为`flex:1`即可填充空白。
* 并且中间元素的高度会与旁边的元素同高
* 如果两边元素不是等高，中间元素会与较高的元素等高 ![ZxLjWd.png](https://s2.ax1x.com/2019/07/20/ZxLjWd.png)
* 如果中间元素设置了高度，会使用自己设置的高度 ![ZxLxSA.png](https://s2.ax1x.com/2019/07/20/ZxLxSA.png)

**圣杯布局 利用 float 负边距**

```
<!-- html -->
<div class="container">
  <div class="middle">这是中间的内容</div>
  <div class="left">这是左边的内容</div>
  <div class="right">这是右边的内容</div>
</div>
<!-- css -->
<style type="text/css">
  .container {
    height: 300px;
    padding: 0 300px 0;
    margin: 0;
  }
  .container div {
    float: left;
    position: relative;
  }
  .container .left,
  .container .right {
    width: 300px;
    height: 300px;
    background-color: #f40;
  }
  .container .left {
    margin-left: -100%;
    left: -300px;
  }
  .container .right {
    margin-left: -300px;
    right: -300px;
  }
  .container .middle {
    width: 100%;
    height: 300px;
    background-color: #ccc;
  }
</style>
```

![ZzksVH.png](https://s2.ax1x.com/2019/07/20/ZzksVH.png)

> 必须将中间部分的 HTML 结构写在最前面，三个元素均设置向左浮动，相对定位。 两侧元素宽度固定，中间设置为 100% 然后利用左侧元素负的`margin`值进行偏移，覆盖在中间的宽度之上，右侧的元素同样利用负的`margin`值进行覆盖 优先加载主体

**存在问题：不能自适应高度**

**双飞翼布局**

```
<!-- html -->
<div class="container">
  <div class="middle">这是中间的内容</div>
</div>
<div class="left">这是左边的内容</div>
<div class="right">这是右边的内容</div>
<!-- css -->
<style type="text/css">
  .container {
    float: left;
    width: 100%;
  }
  .container .middle {
    height: 300px;
    margin-left: 300px;
    margin-right: 300px;
    background-color: #ccc;
  }
  .left {
    float: left;
    height: 300px;
    width: 300px;
    margin-left: -100%;
    background-color: #f40;
  }
  .right {
    float: right;
    height: 300px;
    width: 300px;
    margin-left: -300px;
    background-color: #f40;
  }
</style>
```

![ZxvbjA.png](https://s2.ax1x.com/2019/07/20/ZxvbjA.png)

> 利用的是浮动元素 `margin` 负值的应用

**主体内容可以优先加载，HTML 代码结构稍微复杂点**

**吸顶效果**

\*\*吸顶效果：\*\*顾名思义，就是当元素靠近顶部时，自动固定在顶部。

我们使用`position:sticky;`

> 该关键字指定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。此时 `top`, `right`, `bottom`, `left` 和 `z-index` 属性无效。 **粘性定位**可以被认为是相对定位和固定定位的混合。元素在跨越特定阈值前为相对定位，之后为固定定位

{% embed url="https://codepen.io/lhd990608/pen/gVpOaX" %}

在上面的例子中，我们给三个 div 都设置了`position: sticky`，但由于`top`值有所不同，产生的效果也有所不同。

* 相同点：
  * box1、box2、box3 在滚动之前，他们与相对定位一样
  * 当到达某一个位置时，表现的与绝对定位一样
* 不同点：
  * box1 的 top 值为 0，在靠近页面视口顶部时，固定在页面视口的顶部
  * 当 box2 的 top 值为 30px，在离页面视口 30px 的位置，它固定住
  * box3 没有设置 top，它的行为一直是相对定位

**注意：**

* **粘性定位**的固定定位并不一定是`position:fixed`，只有目标元素的任意父元素都没有设置`position:relative` | `absolute` | `fixed` | `sticky`的情况下，才与`position: fixed`表现一样。而当其任一父元素设置了`position:relative` | `absolute` | `fixed` | `sticky`时，目标元素是相对于父元素的固定。
