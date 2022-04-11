# 那些好看的页面是怎么形成的——CSS 初识

**CSS** （Cascading Style Sheets，层叠样式表）是用来控制网页在浏览器中的显示外观的声明式语言。浏览器会根据 CSS 的样式定义将其选定的元素显示为恰当的形式。一条 CSS 的样式定义包括属性和属性值，它们共同决定网页的外观。

### 如何使用

* 行间样式
  * `<div style=" "></div>`
* 页面级 Css
  * ```
    <head>
      <style></style>
    </head>
    ```
* 外部 Css 文件
  * ```
    <head>
      <link rel="stylesheet" type="text/css" href="" />
    </head>
    ```

### 选择器

![MHbubq.png](https://s2.ax1x.com/2019/11/23/MHbubq.png)

#### 优先级是如何计算的？

优先级就是分配给指定的 CSS 声明的一个权重,它由匹配的选择器中的每一种选择器类型的数值决定。 而当优先级与多个 CSS 声明中任意一个声明的优先级相等的时候,CSS 中最后的那个声明会被应用到元素上。 当同一个元素有多个声明的时候,优先级才会有意义。因为每一个直接作用于元素的 CSS 规则总是会接管/覆盖(take over)该元素从祖先元素继承而来的规则。

#### 选择器的类型

1. **类型选择器**(type selectors) (例如`h1`)和**伪元素**(pseudo-elements) (例如`::before`)
2. **类选择器**(class selectors) (例如,`.example`),**属性选择器**(attributes selectors) (例如,`[type="radio"]`),**伪类**(pseudo-classes) (例如,:hover)
3. **ID 选择器**(例如,`#example`)
4. **子选择器**（Child selector）
5. **后代（包含）选择器**（Descendant selector）使用**空格**,用于选择指定标签元素下的**后辈元素**
6. **相邻兄弟选择器**（Adjacent sibling selector）使用加号`+`。**E+F,匹配 E 元素之后的同级元素 F(直接相邻)**
7. **通用兄弟选择器** 使用波浪号`~`,比如**E\~F,匹配 E 元素之后的所有同级元素 F(无论相邻与否)**
8. **通配符选择符**(universal selector) (\*),**关系选择符**(combinators) (`+`,`>`,`~`,’\`\`’)和**否定伪类**(negation pseudo-class) (`:not()`)对优先级没有影响。(但是,在\*\*:not()内部声明\*\*的选择器是会影响优先级的)。
9. 给元素添加的**内联样式**(例如,`style="font-weight:bold"`)总是会覆盖外部样式表的任何样式,因此可看作是具有最高优先级。

**例外的`！important`规则**

* 当在一个样式声明中使用一个`!important`规则时,此声明将覆盖任何其他声明。
* 使用`!important`是一个坏习惯,应该尽量避免,因为这破坏了样式表中固有的级联规则,使得调试找 bug 变得更加困难了。

#### 选择器的优先级

* **css 样式的简单权重级别由高到低排列:**
* 在属性后面使用 `!important` 会覆盖页面内任何位置定义的元素样式。
* 作为 style 属性写在元素标签上的**内联样式**
* id 选择器
* 类选择器、伪类选择器、属性选择器
* 标签选择器、伪元素选择器
* 通配符选择器
* 浏览器自定义

#### Css 权重

![MHb3PU.png](https://s2.ax1x.com/2019/11/23/MHb3PU.png)

| 选择器         | 权重       |
| ----------- | -------- |
| !important  | Infinity |
| 行间样式        | 1000     |
| id 选择器      | 100      |
| class/属性/伪类 | 10       |
| 标签/伪元素      | 1        |
| 通配符         | 0        |

#### 复杂场景下计算优先级

* 复杂场景下将选择器分为四个级别通过各级别的出现次数决定。
* 四个级别分别为：行内选择符、ID 选择符、类别选择符、元素选择符。
* 优先级的算法：
  * 每个规则对应一个初始”四位数”：0、0、0、0
  * 若是 行内选择符,则加 1、0、0、0
  * 若是 ID 选择符,则加 0、1、0、0
  * 若是 类选择符/属性选择符/伪类选择符,则分别加 0、0、1、0
  * 若是 元素选择符/伪元素选择符,则分别加 0、0、0、1
* 算法：将每条规则中,选择符对应的数相加后得到的“四位数”,从左到右进行比较,大的优先级更高。 优先级相同时,采用就近原则,即最后出现的样式。

#### 一些经验法则

* **一定**要优化考虑使用样式规则的优先级来解决问题而不是`!important`
* **只有**在需要覆盖全站或外部 CSS(例如引用 ExtJs 或者 YUI)的特定页面中使用`!important`
* **永远不要**在全站范围的 css 上使用`!important`
* **永远不要**在你的插件上使用`!important`

#### CSS 优化、提高性能的方法有哪些？

![MHbNrR.png](https://s2.ax1x.com/2019/11/23/MHbNrR.png)

* **关键选择器（key selector）**。选择器的最后面的部分为关键选择器（即用来匹配目标元素的部分）
* 如果规则拥有 ID 选择器作为其关键选择器，则不要为规则增加标签
* **过滤掉无关的规则**（这样样式系统就不会浪费时间去匹配它们了）
* **提取项目的通用公有样式**，增强可复用性，按模块编写组件
* 增强项目的协同开发性、可维护性和可扩展性
* 使用预处理工具或构建工具（gulp 对 css 进行语法检查、自动补前缀、打包压缩、自动优雅降级）

#### 类和 ID 选择器的使用场景

* class 重在样式的复用,重普遍性
* id 重在划分样式区域,可以作为样式表中的命名空间来管理样式
* id 也可以指定某一个特殊元素的样式,重特殊性

#### 哪些可以继承？

给父元素设置一些属性,子元素也可以使用,这个我们就称之为继承性

![MHbDPO.png](https://s2.ax1x.com/2019/11/23/MHbDPO.png)

**注意点:**

1. 并不是所有的属性都可以继承
2. 不可继承的:display、margin、border、padding、background、height、min-height、max-height、width、min-width、max-width、overflow、position、left、right、top、bottom、z-index、float、clear、table-layout、vertical-align、page-break-after、page-bread-before 和 unicode-bidi
3. **所有元素可继承:visibility 和 cursor**
4. 内联元素可继承:letter-spacing、word-spacing、white-space、line-height、color、font、font-family、font-size、font-style、font-variant、font-weight、text-decoration、text-transform、direction
5. 在 CSS 的继承中不仅仅是儿子可以继承,只要是后代都可以继承
6. 继承中的特殊性 **a 标签的文字颜色和下划线是不能继承的** **h 标签的文字大小是不能继承的**
7. 终端块状元素可继承:text-indent 和 text-align
8. 列表元素可继承:list-style、list-style-type、list-style-position、list-style-image
9. 表格元素可继承：border-collapse

**应用场景**

一般用于设置网页上的一些共性信息,例如网页的文字颜色,字体,文字大小等内容 body{}

### CSS 盒模型

> 盒模型分为标准盒模型和怪异盒模型(IE 模型)

```
box-sizing：content-box   //标准盒模型
box-sizing：border-box    //怪异盒模型
```

![MHboRg.png](https://s2.ax1x.com/2019/11/23/MHboRg.png)

#### 标准盒模型

W3C 中的盒子模型的宽：包括 margin + border + padding + width;

```
W = margin * 2 + border * 2 + padding * 2 + width;`
`H = margin * 2 + border * 2 + padding * 2 + hight;
/* 如下代码，整个宽高还是 120px */
div {
  box-sizing: content-box;
  margin: 10px;
  width: 100px;
  height: 100px;
  padding: 10px;
}
```

#### 怪异盒模型

IE 中的盒子模型的宽：包括 margin + border + padding + width;

```
W = margin * 2 + width;`
`H = margin * 2 + height;
/* 如下代码，整个宽高还是 100px */
div {
  box-sizing: border-box;
  margin: 10px;
  width: 100px;
  height: 100px;
  padding: 10px;
}
```

#### 两者的区别

* IE 盒子模型的 content 部分包含了 border 和 padding。

#### 选择哪种盒子模型

* 当然是“标准 w3c 盒子模型”了。
* 在网页顶部加上`doctype`声明,所有的浏览器都会采用标准的 w3c 盒子模型去解释你的盒子,网页就能在各个浏览器中显示一致了。

### 文档流(normal flow)

想像一下,什么是”流”？我们平常说的”水流”,”流体”,我们就可以把像河流那样长长的东西作为流。

![MHqSWF.png](https://s2.ax1x.com/2019/11/23/MHqSWF.png)

那这里所指的文档流指的是什么呢？由于这是显示在浏览器上面,显示在电脑屏幕前的。如果我们将屏幕的两侧想象成河道,将屏幕的上面作为流的源头,将屏幕的底部作为流的结尾的话,那我们就抽象出来的文档流。

那文档流是什么呢？那就是元素！可以将屏幕显示中的内容都可以一一对应文档中的一个元素,在这里就引出两个概念：**内联元素**和**块级元素**

#### 块级元素和内联元素

* 块级元素:四四方方的块,在文档中自己占一行。如`<div>` `<p>`
* 内联元素:(行内元素)多个内联元素,可以在一行显示。如`<span>` `<img>`

> **在内联元素中回车符会被显示成为一个空格**

#### 块级元素和行内元素的转换

块级元素(block)和行内元素(inline)只要加上 display:block;或者是 display:inline 就可以转换了！

#### 脱离文档流

CSS 中有三种方式脱离文档流

```
position:absolute
position:fixed
float
```
