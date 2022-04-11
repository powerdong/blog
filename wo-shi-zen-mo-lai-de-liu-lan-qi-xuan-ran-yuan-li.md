# 我是怎么来的？——浏览器渲染原理

### 引入

提到页面渲染，最重要的就是关键渲染路径

通常我们在编写页面时，只需要编写 HTML、Css、JavaScript 屏幕上就会显示出漂亮的页面，但浏览器是如何使用我们的代码在屏幕上渲染像素的呢? 浏览器将 HTML、Css、JavaScript 转换为屏幕上所呈现的实际像素，这期间所经历的一系列步骤，叫做**关键渲染路径(Critical Rendering Path)**

### 浏览器渲染页面的过程

从耗时的角度，浏览器请求，加载，渲染一个页面，时间花在下面五项

* DNS 查询
* TCP 连接
* HTTP 请求及响应
* 服务器响应
* 客户端渲染

本文讨论第五个部分，即**浏览器对内容的渲染**

1. 处理 HTML 生成 DOM 树
2. 处理 Css 生成 CssOM 树
3. 将两树合并成 render 树
4. 对 render 树进行布局计算
5. 将 render 树中的每个节点绘制到屏幕上

> 如果 DOM 或 CssDOM 被修改，以上过程需要重复执行，这样才能计算出那些像素在屏幕上进行重新渲染。

#### 细化分析

1. 浏览器把获取到的 HTML 代码解析成一个 DOM 树，**HTML 中的每个 tag 都是 DOM 树中的 一个节点，根节点就是我们常用的 `document` 对象**，当然这里包含使用 js 动态创建的 DOM 节点
2. 浏览器把所有样式(主要包括 Css 和浏览器的默认样式设置)解析成样式结构体，**在解析的过程中会去掉浏览器不能识别的样式**，生成 CssDOM 树
3. DOM tree 和 CssDOM tree 合并成 render tree， render tree 中每个 node 都有自己的 style，而且 render tree 不包含隐藏的节点(比如`display: none`的节点，还有无样式的`<head>`节点)，因为这些节点不会用于呈现，而且不会影响呈现的。**注意：`visibility: hidden`隐藏元素还是会包含到 render tree 中，因为`visibility: hidden`会影响布局(layout),会占有空间**
4. render tree 构建完毕之后根据样式计算布局，布局阶段的输出结果都称为”盒模型”(box-model)。盒模型精准表达了窗口每个元素的位置和大小，而且所有的相对度量单位都被转换成了屏幕上的绝对像素位置(根据 Css2 的标准，render tree 中的每个节点都称为 box),box 所有属性:width,height,margin,padding,left,border…
5. 将这些信息渲染为屏幕上每个真实的像素点，这个阶段称为”绘制”，或者”栅格化”。

![mymf3T.png](https://s2.ax1x.com/2019/08/24/mymf3T.png)

#### 渲染阻塞

> 现代的浏览器总是并行加载资源。

当 HTML 解析器被脚本阻塞时，解析器虽然会停止构建 DOM，但仍会识别该脚本后面的资源，并进行预加载。

1. 默认情况下，Css 被视为阻塞渲染的资源，这意味着浏览器将不会渲染任何已处理的内容，直到 CssDOM 构建完毕
   * DOM 树和 CssOM 解析是互不影响的，因此，Css 不会阻塞 DOM 的解析
   * 当 Css 还有构建完成时，不会渲染到页面的，因此Css会阻塞DOM的渲染
   * 这可能是浏览器的一种机制，如果在加载Css的时候，可能会修改下面的DOM节点样式，如果Css加载不阻塞DOM的话，那么当Css加载完成之后，DOM树又得重绘或重排了，这就造成不必要的损耗。
2. JavaScript 不仅可以读取和修改 DOM 属性，还可以读取和修改 CssDOM 属性
   *   故在JS脚本执行前，浏览器必须保证到Css文件完全加载并解析完成。这就导致了JS执行的延迟，也因此导致HTML解析和渲染延迟

       > 存在阻塞的 Css 资源时，浏览器会延迟 JavaScript 的执行和 DOM 构建

**总结**

* **Css 加载不会阻塞 DOM 树的解析**
* **Css 加载会阻塞 DOM 树的解析**
* **Css 加载会阻塞后面 JS 语句的执行**

1. **当浏览器遇到一个 script 标记时，DOM 构建将暂停，直至脚本完成执行**
2. JavaScript 可以查询和修改 DOM 与 CssDOM
3. **CssDOM 构建时，JavaScript 执行将暂停，直至 CssDOM 就绪**

> script 标签的位置很重要，实际使用时应将资源放在页面底部

#### defer 和 async

> **注意:async 与 defer 属性对于 inline-script 都是无效的**

**defer**

defer 属性表示延迟执行引入的 JavaScript，在这段代码加载时 HTML 并未停止解析，这两个过程是并行的。**当整个 DOM 解析完毕且 defer-script 也加载完成后，执行 defer-script 里的 JavaScript 代码。**

> 执行结果放在 HTML 标签解析完成之后

**async**

async 属于异步加载 JavaScript，在这段代码加载时 HTML 并未停止解析，这两个过程是并行的。**当 async-script 加载完成后，会立即执行 async-script 里的 JavaScript 代码。**

> 执行结果可能在 HTML 标签解析完成之前或之后

#### 动态创建标签

当使用`document.createElement('script')`创建的 script 默认是异步的

```
console.log(document.createElement("script").async); //true
```

### 重绘重排

> 重排重绘会影响性能

1. 我们计算它们在当前设备中的准确位置和尺寸。这正是布局阶段要做的工作，该阶段在英语中也被称为 **“回流”(reflow)**,当 render tree 中的一部分(或全部)因为元素的规模尺寸，布局，隐藏等给便需要重新构建。也会回流(其实我觉得叫重新布局简单明了些)。每个页面至少需要一次回流，就是在页面第一次加载的时候。
2. \*\*重绘(repaints)\*\*当 render tree 中的一些元素需要更新属性，而这些属性只是影响元素的外观，风格，而不会影响布局，比如`background-color`,则就称为重绘

#### 触发重排的方法

以下这些属性和方法需要返回最新的布局信息，重新计算渲染树，就会造成回流，触发重排以返回正确的值。建议将他们合并到一起操作，可以减少回流的次数。这些属性包括：`offsetTop`、`offsetLeft`、 `offsetWidth`、`offsetHeight`；`scrollTop`、`scrollLeft`、`scrollWidth`、`scrollHeight`；`clientTop`、`clientLeft`、`clientWidth`、`clientHeight`；`getComputedStyle()` 、`currentStyle()`

> 提高网页性能，就是要降低”重排”和”重绘”的频率和成本，尽量少触发重新渲染。

DOM 变动和样式变动，都会触发重新渲染。但是，浏览器已经很智能了，会尽量把所有的变动集中在一起，排成一个队列，然后一次性执行，尽量避免多次重新渲染.

#### 优化办法

> 理论上的优化办法

1. DOM 的多个读操作，(或多个写操作)，应该放在一起，不要两个读操作之间插入一个写操作
2. 离线操作 DOM。如使用隐藏元素 `document.createDocumentFragment()`,`cloneNode()`
3. 修改样式的时候添加类名，或一次性添加到 `dom.style.cssText`上等
