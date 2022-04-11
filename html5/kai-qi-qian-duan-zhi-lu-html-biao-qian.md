---
cover: https://s2.ax1x.com/2019/11/23/MHgDXD.png
coverY: 0
---

# 开启前端之路——HTML 标签

**HTML**（超文本标记语言——HyperText Markup Language）是构成 Web 世界的一砖一瓦。它定义了网页内容的含义和结构。除 HTML 以外的其它技术则通常用来描述一个网页的表现与展示效果（如 `CSS`），或功能与行为（如 `JavaScript`）。

为了创建网站，你应该了解 **HTML —— 用于定义一个网页结构的基本技术。** HTML 用于表示你的网页内容是应该被理解为段落、列表、头部、链接、图像、多媒体播放器、表单或是其他众多可用的元素之一亦或是你定义的新元素。

### 从规范的角度理解 HTML,从分类和语义的角度使用标签

![MHggAA.png](https://s2.ax1x.com/2019/11/23/MHggAA.png)

#### HTML 语义化是什么?

语义化是指根据内容的结构化(内容语义化),选择合适的标签(代码语义化),便于开发者阅读和写出更优雅的代码的同时,让浏览器的爬虫和机器很好地解析。

#### 为什么要语义化?

* 有利于 **SEO(搜索引擎优化(Search Engine Optimization))**,有助于爬虫爬取更多有效的信息,爬虫是依赖于标签来确定上下文和各个关键字的权重
* **语义化的 HTML 在没有 CSS 的情况下也能呈现较好的内容结构和代码结构**
* 方便其他设备的解析
* 便于团队开发和维护

### 常用页面标签的默认样式、自带属性、不同浏览器的差异、处理浏览器兼容问题的方式

* `li`：默认以列表显示
* `head`：默认不显示
* `table`：默认为表格显示
* `tr`：默认为表格行显示
* `thead`：默认为表格头部分组显示
* `tbody`：默认为表格行分组显示
* `tfoot`：默认为表格底部分组显示
* `col`：默认为表格列显示
* `colgroup`：默认为表格列分组显示
* `td,th`：默认为单元格显示
* `caption`：默认为表格标题显示,呈现居中状态
* `th`：默认为表格标题显示,呈现加粗居中状态
* `body`：默认 8px 的外边距,行高默认 1.12
* `h1`：默认字体大小 2em,上下外边距 0.67em
* `h2`：默认字体大小 1.5em,上下外边距 0.75em
* `h3`：默认字体大小为 1.17em,上下外边距 0.83em
* `h4`, `p`,`blockquote`, `ul`, `fieldset`, `form`, `ol`, `dl`, `dir`, `menu`：默认上下外边距 1.12em
* `h5`：默认字体大小为 0.83em,上下外边距 1.5em
* `h6`：默认字体大小为 0.75em,上下外边距 1.67em
* `h1`, `h2`,`h3`, `h4`, `h5`, `h6`, `b`,`strong`：默认字体加粗效果
* `blockquote` ：默认左右边距 40px
* `i`, `cite`,`em`,`var`, `address`：默认字体类型为 italic
* `pre`, `tt`,`code`, `kbd`, `samp`:默认字体为 monospace
* `pre`：默认支持显示空格 {white-space: pre }
* `button`,`textarea`, `input`, `object`, `select`：行级块元素
* `big`：默认字体大小为 1.17em
* `small`,`sub`, `sup`：默认字体大小为 0.83em
* `sub`：定义 sub 元素默认为下标显示
* `sup`：定义 sup 元素默认为上标显示
* `table`:设置相邻的表格单元格的边界之间的距离 2px
* `thead`,`tbody`, `tfoot`：定义表头、主体表、表脚元素默认为垂直对齐
* `td`, `th`：定义单元格、列标题默认为垂直对齐默认为继承
* `s`, `strike`,`del`：定义这些元素默认为删除线显示
* `hr`：定义分割线默认为 1px 宽的 3D 凹边效果
* `ol`, `ul`,`dir`, `menu`, `dd`：默认左外边距为 40px
* `ol`：默认以数字排序
* `u`, `ins`：默认带下划线

#### 易混淆的 HTML 标签

![MHgh1f.png](https://s2.ax1x.com/2019/11/23/MHgh1f.png)

* i 标签

_i 标签效果_,i 标签通常表示因为某种原因和正常文本不同的文本,例如专业术语,外语短语或排版用的文字。通常表现为斜体。

* em 标签

_em 标签效果_,em 表示强调的文本,视觉上也是斜体效果。

* strong 标签

**strong 效果**,以加粗的形式展现。表示这个文本的重要性,在 HTMl4 中表示特别强调,HTML5 中描述为“strong importance for its contents”。

* b 标签

**b 标签效果**,表示的风格不同于正常的文本,没有表达任何特殊的重要性和相关性。通常用于关键回顾,回顾中的产品名称,或者是其他需要表现为粗体的文本。另一个例子就是标志每个段落的 lead sentence。

* mark 标签

mark 标签效果,表现为高亮文本。例如我们在网页查找关键字时,找到的结果就会以高亮的形式展现。 mark 元素通常是表现跨越不同的上下文种的相关文本。

#### 常见的行/块级元素

* inline 行级元素
  * 内容决定元素所占位置
  * 不可以通过 Css 改变宽高
  * 都有文字特性
    * `<span>`、`<strong>`、`<em>`、`<a>`、`<del>`、`<i>`
* block 块级元素
  * 独占一行
  * 可以通过 Css 改变宽高
    * `<div>`、`<p>`、`<ul>`、`<li>`、`<ol>`、`<form>`
* inline-block 行块级元素
  * 内容决定大小
  * 可以改变宽高
    * `<img>`

#### HTML5 新标签

![MH2pB4.png](https://s2.ax1x.com/2019/11/23/MH2pB4.png)

* section

section标签效果

,表示文档中的一个区域(或节),比如,内容中的一个专辑组,一般来说会有包含一个标题(heading)。一般通过是否包含一个标签(h1-h6)作为子节点来辨识每一个section 。一般来说,一个section 应该出现在文档大纲中。

* article

article标签效果

,article元素表示文档、页面、应用或网站中的独立结构,其意在成为可独立分配的或可复用的结构,如在发布中,他可能是论坛帖子、杂志新闻、博客、用户提交的评论、交互式组件、或者其他独立的内容项目。

[HTML5 新增了哪些语义标签](http://www.html5jscss.com/html5-semantics-section.html);

#### 浏览器默认样式

[浏览器默认样式对照表](http://developer.doyoe.com/default-style/)

### 元信息类标签(head、title、meta)的使用目的和配置方法

#### Head 中的全局属性

**base**

base 标签指定了一个 url 地址作为基准,那么当前文档中的所有超链接都遵循这一规则,在 a 中设置访问目标的相对地址,浏览器会自动解析出一个完成的链接地址,若 a 中的 href 为空,浏览器也会根据 base 所给的 url 进行访问。 **注意:** base 标签需放在包含 url 地址的语句前面。

```
<head>
  <base href="https://www.baidu.com/" target="_blank" />
</head>
```

**link**

link 标签:用于引入外部文档,常用到引用 css 样式或 icon 图标 其中:rel 属性是 link 标签的核心 注意:在 HTML 中,`<link>`标签没有结束标签 在 XHTML 中,`<link>`标签必须被正确的关闭

**charset 属性规定被链接文档的字符编码方式** **href 属性规定被链接文档的位置(URL)**

```
<head>
  <link href="index.css" rel="stylesheet" type="text/css" />
</head>
```

**media 属性规定被链接文档将显示在什么设备上**

media 属性有:

* ```
  screen
  ```
  * 计算机屏幕(默认)
* ```
  tty
  ```
  * 电传打字机以及类似的使用等宽字符网格的媒介。
* ```
  tv
  ```
  * 电视机类型设备（低分辨率、有限的滚屏能力）。
* ```
  projection
  ```
  * 放映机
* ```
  handheld
  ```
  * 手持设备(小屏幕、有限带宽)
* ```
  print
  ```
  * 打印预览模式/打印页面
* ```
  braille
  ```
  * 盲人点字法反馈设备
* ```
  aural
  ```
  * 语音合成器
* ```
  all
  ```
  * 适用于所有设备

**rel 属性规定当前文档与被链接文档之间的关系**

只有 rel 属性的”`stylesheet`“值得到了所有浏览器的支持。其他值只得到了部分地支持

**title**

浏览器会议特殊的方式来使用标题,当把文档加入链接列表或收藏夹时,文档标题将成为该链接的默认名称 **`<title>`标签是`<head>`标签中唯一要求包含的东西。**

**1. dir** 属性为 rtl 和 ltr 规定元素中内容的文本方向-正向和反向 **2. lang** 规定元素中内容的语言代码 **3. xml:lang** 规定 XHTML 文档中元素内容的语言代码

#### meta

主要用于描述网页,与之对应的属性值为 content,content 中的内容主要是便于搜索引擎机器人查找信息和分类信息用的

```
<meta name="参数" content="具体的参数值" />
```

1. `keywords` 关键字
2. `description` 网站内容描述
3. `robots` 机器人向导(content 的参数值有 all,none,index,noindex,follow,nofollow)
4. `author` 作者
5. `renderer` 渲染模式
6. `viewport`(视图模式)【对手机端而言很重要可把 `content="width-device=width"`,则宽度为使用手机的宽度】
7. `http-equiv` 属性【http-equiv 顾名思义,相当于 http 的文件头作用,它可以向浏览器传回一些有用的信息,以帮助正确和精确地显示网页内容,与之对应的属性值为 content,content 中的内容其实就是各个参数的变量值。】

### 主流浏览器内核

目前最为主流浏览器有五大款，分别是 IE、Firefox、Google Chrome、Safari、Opera。

![MHgGm4.png](https://s2.ax1x.com/2019/11/23/MHgGm4.png)

> 浏览器最重要的部分是浏览器的内核。浏览器内核是浏览器的核心，也称“渲染引擎”，用来解释网页语法并渲染到网页上。浏览器内核决定了浏览器该如何显示网页内容以及页面的格式信息。不同的浏览器内核对网页的语法解释也不同，因此网页开发者需要在不同内核的浏览器中测试网页的渲染效果。

| 主流浏览器         | 内核           |
| ------------- | ------------ |
| IE            | Trident      |
| Firefox       | Gecko        |
| Google Chrome | Webkit/blink |
| Safari        | Webkit       |
| Opera         | presto/blink |

### 写 HTML 代码时应注意什么?

![MH2ZjO.png](https://s2.ax1x.com/2019/11/23/MH2ZjO.png)

* 尽可能少的使用无语义的标签 `<div>` 和 `<span>`
* 在语义不明显时既可以使用 `<div>` 或者 `<p>` 时,尽量使用 `<p>`,因为 `<p>` 在默认情况下有上下间距,对兼容特殊终端有利
* 不要使用纯样式标签,改用 css 设置
* 需要强调的文本,可以包含在 `<strong>` 或者 `<em>` 标签中(浏览器预设样式,能用 css 指定就不用他们),`<strong>` 默认样式是加粗(不要用 `<b>`),`<em>` 是斜体(不用 `<i>`)
* 使用表格时,标题要用 `caption`,表头用 `<thead>`,主体部分用 `<tbody>` 包围,尾部用 `<tfoot>` 包围。表头和一般单元格要区分开,表头用 `<th>`,单元格用 `<td>`
* 表单域要用 `<fieldset>` 标签包起来,并用 `<legend>` 标签说明表单的用途
* 每个 `<input>` 标签对应的说明文本都需要使用 `label` 标签,并且通过 `<input>` 标签设置 id 属性,在 `label` 标签中设置 for 来让说明文本和相对应的 `<input>` 关联起来。
