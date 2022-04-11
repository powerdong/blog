# 一入前端深似海——BFC剖析

### BFC 实现原理,可以解决的问题,如何创建 BFC

#### BFC 的定义

BFC 全称为 block formatting context,中文为“块级格式化上下文”。 相对应的还有 IFC,也就是 inline formatting context,中文为“内联格式化上下”。 不过 IFC 作用和影响比较隐晦,我们就不介绍了,我们把学习重心放在 BFC 上。

如果一个元素具有 BFC,内部子元素再怎么翻江倒海、翻云覆雨,都不会影响外面的元素。所以 BFC 是不可能发生 margin 重叠的,因为 margin 重叠是会影响外面的元素的;BFC 元素也可以用来清除浮动的影响,因为如果不清除,子元素浮动则父元素高度塌陷,必然会影响后面元素的布局和定位。

那什么时候会触发 BFC 呢？常见的情况如下:

* `<html>`根元素
* `float` 的值不为 `none`
* `overflow` 的值为 `auto`、`scroll` 或 `hidden`
* `display` 的值为 `table-cell`、`table-caption` 和 `inline-block` 中的任何一个
* `position` 的值不为 `relative` 和 `static`

#### BFC 的应用场景

* 防止 margin 重叠（塌陷）
* 清除内部浮动
* 自适应多栏布局

```
<!--两栏布局-->
<style>
  body {
    width: 300px;
    position: relative;
  }

  .aside {
    width: 100px;
    height: 150px;
    float: left;
    background: #f66;
  }

  .main {
    height: 200px;
    background: #fcc;
    overflow: hidden;
  }
</style>
<body>
  <div class="aside"></div>
  <div class="main"></div>
</body>
<style>
  html,
  body {
    height: 100%;
    width: 100%;
    margin: 0;
    padding: 0;
  }
  .left {
    background: pink;
    float: left;
    width: 180px;
  }
  .center {
    background: lightyellow;
    overflow: hidden;
  }
  .right {
    background: lightblue;
    width: 180px;
    float: right;
  }
</style>
<div class="container">
  <div class="left"></div>
  <div class="right"></div>
  <div class="center"></div>
</div>
```

BFC 的特性最重要的用途其实不是去 margin 重叠或者是清除 float 影响,而是实现更健壮、更智能的自适应布局。

#### 创建 BFC

* float:left
  * 浮动元素本身 BFC 化
  * 然而浮动元素有破坏性和包裹性,失去了元素本身的流体自适应性,因此,无法用来实现自动填满容器的自适应布局
  * 其兼容性良好,上手简单
* position:absolute
  * 脱离文档流有些严重,过于清高,和非定位元素很难玩到一块去
*   overflow:hidden

    (这个超棒)

    * 不像浮动和绝对定位,玩的有点过,其本身还是一个很普通的元素
    * 块状元素的流体特性保存得相当完好
    * overflow:hidden 的 BFC 特性从 IE7 浏览器开始就支持,兼容性也不错
    * 唯一的问题就是容器盒子外的元素可能会被隐藏掉
* display:inline-block
  * 会让元素尺寸包裹收缩
  * 在 IE6 和 IE7 浏览器下,block 水平的元素设置 display:inline-block 元素还是 block 水平
  * 于是,对于 IE6 和 IE7 得到一个比 overflow:hidden 更强大的声明
* display:table-cell
  * 其让元素表现得像单元格一样,IE8 及以上版本浏览器才支持
  * 它会跟随内部元素的宽度显示

#### BFC 作用

* **BFC 最大的一个作用就是：在页面上有一个独立隔离容器，容器内的元素和容器外的元素布局不会相互影响**
* 解决上外边距重叠;重叠的两个 box 都开启 bfc;
* 解决浮动引起高度塌陷;容器盒子开启 bfc
* 解决文字环绕图片;左边图片 div,右边文字容器 p,将 p 容器开启 bfc
