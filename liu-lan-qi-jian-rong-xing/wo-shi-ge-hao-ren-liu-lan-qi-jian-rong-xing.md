# 我是个好人——浏览器兼容性

## 浏览器兼容性处理

### 经常遇到的浏览器的兼容性有哪些？原因，解决方法是什么，常用 hack 的技巧 ？

* png24 位的图片在 iE6 浏览器上出现背景，解决方案是做成 png8.
* 浏览器默认的 margin 和 padding 不同。解决方案是加一个全局的`*{margin:0padding:0;}`来统一。
* IE6 双边距 bug:块属性标签 float 后，又有横行的 margin 情况下，在 ie6 显示 margin 比设置的大。
* 浮动 ie 产生的双倍距离 `#box{ float:left; width:10px; margin:0 0 0 100px;}`
  * 这种情况之下 IE 会产生 20px 的距离，解决方案是在 float 的标签样式控制中加入 `*display:inline;`将其转化为行内属性。(`*`这个符号只有 ie6 会识别)
* 渐进识别的方式，从总体中逐渐排除局部。
  * 首先，巧妙的使用“\9”这一标记，将 IE 游览器从所有情况中分离出来
  * 接着，再次使用“+”将 IE8 和 IE7、IE6 分离开来，这样 IE8 已经独立识别。
  * ```
    .bb{ background-color:red;/所有识别/
    background-color:#00deff\9; /IE6、7、8识别/
    +background-color:#a200ff;/IE6、7识别/
    _background-color:#1e0bd1;/IE6识别/ }
    ```
* IE 下,可以使用获取常规属性的方法来获取自定义属性, 也可以使用 getAttribute()获取自定义属性;
* Firefox 下,只能使用 getAttribute()获取自定义属性。 解决方法:统一通过 getAttribute()获取自定义属性。
* IE 下,even 对象有 x,y 属性,但是没有 pageX,pageY 属性; Firefox 下,event 对象有 pageX,pageY 属性,但是没有 x,y 属性。
  * **解决方法：（条件注释）缺点是在 IE 浏览器下可能会增加额外的 HTTP 请求数。**
* **Chrome 中文界面下默认会将小于 12px 的文本强制按照 12px 显示, 可通过加入 CSS 属性 -webkit-text-size-adjust: none;**
* 解决超链接访问过后 hover 样式就不出现了 被点击访问过的超链接样式不在具有 hover 和 active 了
  * 解决方法是改变 CSS 属性的排列顺序: `L-V-H-A : a:link {} a:visited {} a:hover {} a:active {}`

### 如何解决浏览器的兼容性

* 因为历史原因，不同的浏览器样式存在差异，使用自己的`reset.css`
* 在 CSS3 还没有成为真正的标准时，浏览器厂商就开始支持这些属性的使用了。CSS3 样式语法还存在波动时，浏览器厂商提供了针对浏览器的前缀，直到现在还是有部分的属性需要加上浏览器前缀。前端自动化构建工程帮我们处理。
*   透明属性，解决 IE9 以下浏览器不能使用 opacity。

    ```
    opacity: 0.5;
    filter: alpha(opacity = 50); /*IE6-IE8我们习惯使用filter滤镜属性来进行实现 */
    filter: progid:DXImageTransform.Microsoft.Alpha(style = 0, opacity = 50); /* IE4-IE9都支持滤镜写法progid:DXImageTransform.Microsoft.Alpha(Opacity=xx)*/
    ```
* 封装一个适配器的方法，过滤事件句柄绑定、移除、冒泡阻止以及默认事件行为处理
* 获取 scrollTop 通过 `document.documentElement.scrollTop` 兼容非 chrome 浏览器

```javascript
var scrollTop = document.documentElement.scrollTop || document.body.scrollTop
```

### 列举 IE 与其他浏览器不一样的特性 <a href="#lie-ju-ie-yu-qi-ta-liu-lan-qi-bu-yi-yang-de-te-xing" id="lie-ju-ie-yu-qi-ta-liu-lan-qi-bu-yi-yang-de-te-xing"></a>

**事件不同之处：**

触发事件的元素被认为是目标（target）。而在 IE 中，目标包含在 event 对象的 srcElement 属性；

获取字符代码、如果按键代表一个字符（shift、ctrl、alt 除外），IE 的 keyCode 会返回字符代码（Unicode），DOM 中按键的代码和字符是分离的，要获取字符代码，需要使用 charCode 属性；

阻止某个事件的默认行为，IE 中阻止某个事件的默认行为，必须将 returnValue 属性设置为 false，Mozilla 中，需要调用 preventDefault() 方法；

停止事件冒泡，IE 中阻止事件进一步冒泡，需要设置 cancelBubble 为 true，Mozilla 中，需要调用 stopPropagation()；
