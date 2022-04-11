# 还有哪些好玩的东西——CSS提升

CSS 页面布局技术允许我们拾取网页中的元素，并且控制它们相对正常布局流、周边元素、父容器或者主视口/窗口的位置。

![MHqke1.png](https://s2.ax1x.com/2019/11/23/MHqke1.png)

### CSS 中的定位

* 静态定位
  * 静态定位是每个元素获取的默认值————他只是意味着”将元素放入它在文档布局流中的正常位置”。
  * `position:static`
* 相对定位(参照物)
  * 它与静态定位非常相似,占据在正常的文档流中,可以修改他的最终位置(通过`top`,`left`,`right`,`bottom`),也可以与页面上的其他元素重叠。
  * `position:relative`
* 绝对定位(做定位)
  * 绝对定位的元素不再存在正常的文档布局流中。相反,它独立于一切坐在它自己的层。
  * 这意味着我们可以创建不干扰页面上其他元素的位置的隔离 UI 功能。
  * `position:absolute`
* 定位上下文
  * 哪个元素是绝对定位元素的”包含元素”？这取决于绝对定位元素的父元素 position 属性。
  * 我们可以改变**定位上下文**————绝对定位的元素的相对位置元素。通过设置其中一个父元素的定位属性————也就是包含绝对定位元素的那个元素(如果要设置绝对定位元素的相对元素,那么这个元素一定要包含绝对定位元素)。给包含元素设置`position:relative`。
* 固定定位
  * 不为元素预留空间，而是通过指定元素相对于屏幕视口（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变。fixed 属性会创建新的层叠上下文。
  * `position:fixed`

#### position 的值 relative 和 absolute 定位原点是？

![MHqmWD.png](https://s2.ax1x.com/2019/11/23/MHqmWD.png)

* `absolute` 生成绝对定位的元素，相对于值不为 static 的第一个父元素进行定位。
* `fixed` （老 IE 不支持） 生成绝对定位的元素，相对于浏览器窗口进行定位。
* `relative` 生成相对定位的元素，相对于其正常位置进行定位。 static 默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right z-index 声明）。
* `inherit` 规定从父元素继承 position 属性的值。

```
/* 页面居中 */
position: absolute;
left: 50%;
top: 50%;
margin-left: -(width/2) px;
margin-right: -(height/2) px;
/* 两栏布局 */
position: absolute;
margin-left: ;
```

#### float 浮动模型

![MHq8TP.png](https://s2.ax1x.com/2019/11/23/MHq8TP.png)

```
float: left/right;
```

* 浮动元素产生了浮动流
* 所有产生了浮动流的元素，块级元素看不到他们
* 产生了 BFC 的元素和文本类属性的元素以及未被能看到浮动元素

**解决浮动流**

* **clear 清除浮动**
  *   （

      添加空 div 法

      ）在浮动元素下方添加空 div,并给该元素写 css 样式

      ```
       {
        clear: both;
        height: 0;
        overflow: hidden;
      }
      ```
* **给父级添加 overflow:hidden 清除浮动方法**
*   **万能清除法 after 伪类 清浮动（现在主流方法，推荐使用）**

    ```
    float_div:after {
      content: '.';
      clear: both;
      display: block;
      height: 0;
      overflow: hidden;
      visibility: hidden;
    }
    .float_div {
      zoom: 1;
    }
    ```

### CSS 伪类和伪元素有哪些,它们的区别和实际应用

![MHqtfS.png](https://s2.ax1x.com/2019/11/23/MHqtfS.png)

### 常用的 Css 技巧

![MHq4mR.png](https://s2.ax1x.com/2019/11/23/MHq4mR.png)

#### 溢出容器，打点显示

```
/* 单行 */
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;
/* 多行 */
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 2; // 定义行数
/*! autoprefixer: off */
-webkit-box-orient: vertical;
```

#### position 和 float

```
position: absolute;
float: left/right;
/* 将内部元素转换成inline-block */
```

#### 禁止复制

```
user-select: none;
```

#### 水平垂直居中

```
/* Flex 布局 */
display: flex //设置 Flex 模式
flex-direction: column //决定元素是横排还是竖着排
flex-wrap: wrap //决定元素换行格式
justify-content: space-between //同一排下对齐方式，空格如何隔开各个元素
align-items: center //同一排下元素如何对齐
align-content: space-between //多行对齐方式
```

#### 水平居中

```
/* 行内元素 */
display: inline-block;
/* 块级元素 */
margin: 0 auto;
/* Flex  */
display: flex;
justify-content: center;
```

#### 垂直居中

```
/* 行高 = 元素高： */
line-height: height
/* flex:  */
display: flex;
align-item: center
```

#### CSS 创建一个三角形

```
 <style>
    div {
        width: 0;
        height: 0;
        border-top: 40px solid transparent;
        border-left: 40px solid transparent;
        border-right: 40px solid transparent;
        border-bottom: 40px solid #ff0000;
    }
    </style>
</head>
<body>
  <div></div>
</body>
```

#### 消除 translation 闪屏

```
/* 消除 transition 闪屏 */
.css {
  -webkit-transform-style: preserve-3d;
  -webkit-backface-visibility: hidden;
  -webkit-perspective: 1000;
}
```

#### 改变输入框内提示文字颜色

```
::-webkit-input-placeholder {
  /*webkitbrowsers*/
  color: #999;
}
:-moz-placeholder {
  /*mozillafirefox4to18*/
  color: #999;
}
::-moz-placeholder {
  /*mozillafirefox19+*/
  color: #999;
}
:-ms-input-placeholder {
  /*internetexplorer10+*/
  color: #999;
}
input:focus::-webkit-input-placeholder {
  color: #999;
}
```

#### 使元素消失的方法

```
visibility:hidden
display:none
z-index:-1
opacity：0
```

1. `opacity：0`,该元素隐藏起来了，但不会改变页面布局，并且，如果该元素已经绑定了一些事件，如 **`click` 事件也能触发**
2. `visibility:hidden`,该元素隐藏起来了，但不会改变页面布局，但是**不会触发该元素已经绑定的事件**
3. `display:none`, 把元素隐藏起来，并且会改变页面布局，可以理解成在页面中把该元素删掉

#### 开启硬件加速

```
/* 目前，像 Chrome/Firefox/Safari/IE9+以及最新版本 Opera 都支持硬件加速 */
/* 当检测到某个 DOM 元素应用了某些 CSS 规则时就会自动开启，从而解决页面闪白，保证动画流畅。 */
.css {
  -webkit-transform: translate3d(0, 0, 0);
  -moz-transform: translate3d(0, 0, 0);
  -ms-transform: translate3d(0, 0, 0);
  transform: translate3d(0, 0, 0);
}
```

### Css 注意

* 行级元素只能嵌套行级元素
  * `<p><div></div></p>` **错误**
  * `<a><a></a></a>` **错误**
* 跨级元素可以嵌套任何元素
* button 元素一定要写上 type 属性不然会默认提交表单，出现想不到的 bug
