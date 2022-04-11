# 隔壁老王想篡位？门都没有——同源策略

### 同源策略

浏览器有一个很重要的概念————同源策略

> **同源策略**限制了从同一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的重要安全机制。

同源是指：**域名**、**协议**、**端口**相同。不同源的客户端脚本(`js`、`ActionScript`)在没明确授权的情况下，不能读写对方的资源。

![eCseMQ.png](https://s2.ax1x.com/2019/07/22/eCseMQ.png)

* `http` 与 `https` 下的默认端口可省略
  * `http` => 80
  * `https` => 443

**域名是倒着解析的**

1. `.com`顶级域名
2. `baidu.com` (一)二级域名
3. `zhidao.baidu.com` (二)三级域名
4. `www` 二级域名前缀，表示万维网维护的
5. `www.baidu.com` 属于特殊的三级域名

> `zhidao.baidu.com` 属于百度自己维护的网络地址

* com、org、net 属于顶级域名，是在全世界范围内解析的，cn、hk 是在一个地区解析的。
  * cn 中国
  * .com (商业机构)
  * .net (从事互联网服务的机构)
  * .org (非盈利性组织)
  * .com.cn (国内商业机构)
  * .net.cn (国内互联网机构)
  * .org.cn (国内非盈利性组织)

> 如果把 IP 地址比作一间房子，端口就是出入这间房子的门。真正的房子只有几个门，但是一个 IP 地址的端口可以有多个。
>
> 浏览网页服务默认的[端口号](https://baike.baidu.com/item/%E7%AB%AF%E5%8F%A3%E5%8F%B7)都是 80,因此只需输入网址即可，不用输入”:80”

### 如何解决跨域问题?

`jsonp`、 `iframe`、`window.name`、`window.postMessage`、服务器上设置代理页面

[jsonp](https://powerdong.github.io/myBlog/2019/08/11/%E7%BD%91%E7%BB%9C-JSONP/) [iframe](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe) [window.name](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/name) [window.postMessage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)

说到 AJAX 就会不可避免的面临两个问题。

* AJAX 以何种格式来交换数据?
* 跨域的需求如何解决?

在这里总结了**解决跨域问题的几种方法**

1. Flash
2. 服务器代理中转
3. JSONP
4. document.domain(针对基础域名相同的情况)

但到目前为止最推崇或者说是首选的方案还是用**JSON 来传数据，靠 JSONP 来跨域。**

JSON 和 JSONP 虽然只有一个字母的差别，但其实他们根本不是一回事。

* [**JSON**](https://baike.baidu.com/item/JSON)**是一种轻量级的数据交换格式**
* [**JSONP**](https://baike.baidu.com/item/JSONP)**是一种依靠开发人员的聪明才智创造出的一种非官方跨域数据交互协议,是**[**JSON**](https://baike.baidu.com/item/JSON)**的一种“使用模式”，可用于解决主流浏览器的跨域数据访问的问题**
* 一个是描述信息的格式，一个是信息传递双方约定的方法

### 什么是 JSON

JSON 是 JavaScript Object Notation 的缩写，它是一种数据交换格式。在 JSON 中，一共有 6 种数据类型：**Number**，**Boolean**，**String**，**Null**，**Array**，**Object**以及这些数据类型的任意组合。

由于 JSON 非常简单，很快就风靡 Web 世界，并且成为 ECMA 标准。几乎所有编程语言都有解析 JSON 的库，而在 JavaScript 中，我们可以直接使用 JSON,因为 JavaScript 内置了 JSON 的解析。

### 什么是 JSONP

#### JSONP 原理

1. Web 页面上用 `<script>`引入 js 文件时不受是否跨域的影响(不仅如此，我们还发现凡是拥有 “`src`“ 这个属性的标签都拥有跨域的能力，比如 `<script>`、`<img>`、`<iframe>`)
2. 于是我们把数据放在服务器上，并且数据为 JSON 格式(因为 JS 可以轻松处理 JSON 数据)
3. 因为我们无法监控通过`<script>`的 `src` 属性是否把数据获取完成，所以我们需要做一个处理。
4. 实现定义好处理跨域获取数据的函数
5. 用 `src` 获取数据的时候添加一个参数 `cb = 'callback'`(服务端或根据参数 `cb` 的值返回对应的内容)，此内容以 `cb` 对应的值 `callback` 为函数真实要传递的数据为函数的参数的一串字符

#### JSONP 的客户端具体实现

**1>** 我们知道，哪怕跨域 js 文件中的代码，web 页面也是可以无条件执行的。 远程服务器`server.com`根目录下有个`server.js`文件代码如下:

```
alert("我是远程文件");
```

本地服务器`local.com`下有个`jsonp.html`页面代码如下:

```
<!DOCTYPE html>
<html>
  <head>
    <title></title>
    <script type="text/script" src="http://server.com/server.js"></script>
  </head>
  <body></body>
</html>
```

毫无疑问，页面将会弹出一个提示窗体，显示跨域调用成功

**2>** 现在我们在 `jsonp.html`页面定义一个函数，然后在远程 `server.js` 中传入数据进行调用 `jsonp.html`页面代码如下:

```
<!-- jsonp.html -->
<!DOCTYPE html>
<html>
  <head>
    <title></title>
    <script type="text/javascript">
      var localHandler = function(data) {
        alert(
          "我是本地函数，可以被跨域的 server.js 文件调用，远程js带来的数据是:" +
            data.result
        );
      };
    </script>
    <script type="text/javascript" src="http://server.com/server.js"></script>
  </head>
  <body></body>
</html>
```

`server.js` 文件代码如下:

```
// server.js
localHandler({
  result: "我是远程js带来的数据"
});
```

运行之后查看结果，页面成功弹出提示窗口，显示本地函数被跨域的远程 js 调用成功，并且还接受到了远程 js 带来的数据。但是又一个问题出现了，\*\*怎么让远程 js 知道他应该调用的本地函数叫什么名字呢?\*\*毕竟是 JSONP 的服务者都要面对很多服务对象，而这些服务对象各自的本地函数都不相同啊

**3>** 我们可以这样想，只要服务端提供的 js 脚本是动态生成的就行了呗，这样调用者可以传一个参数过去告诉服务端”我想要一段调用 XXX 函数的代码，请你返回给我”，于是服务器就可以按照客户端的需求来生成 js 脚本并响应了。

`jsonp.html`页面的代码

```
<!-- jsonp.html -->
<!DOCTYPE html>
<html>
  <head>
    <title></title>
    <script type="text/javascript">
      // 得到航班信息查询结果后的回调函数
      var flightHandler = function(data) {
        alert(
          "你查询的航班结果是：票价 " +
            data.price +
            " 元，" +
            "余票 " +
            data.tickets +
            " 张。"
        );
      };
      // 提供jsonp服务的url地址（不管是什么类型的地址，最终生成的返回值都是一段javascript代码）
      var url =
        "http://flightQuery.com/jsonp/flightResult.aspx?code=CA1998&callback=flightHandler";
      // 创建script标签，设置其属性
      var script = document.createElement("script");
      script.setAttribute("src", url);
      // 把script标签加入head，此时调用开始
      document.getElementsByTagName("head")[0].appendChild(script);
    </script>
  </head>
  <body></body>
</html>
```

这次的代码变化比较大，不再直接把远程 js 文件写死，而是编码实现**动态查询**，而**这也正是 JSONP 客户端实现的核心部分**

```
flightHandler({
  code: "CA1998",
  price: 1780,
  tickets: 5
});
```

我们看到，传递给 `flightHandler` 函数的是一个 `json` ，它描述了航班的基本信息。运行一下页面，成功的弹出提示窗口，JSONP 的执行全过程顺利完成。

#### 原生 JS 实现 JSONP 的步骤

**客户端**

1. 定义获取数据后调用的回调函数
2. 动态生成对服务端 JS 进行引用的代码
   * 设置`url`为提供`jsonp`服务的`url`地址，并在该`url`中设置相关`callback`参数
   * 创建`script`标签，并设置其`src`属性
   * 把`script`标签加入`head`中，此时调用开始

**服务端**

将客户端发送的`callback`参数作为函数名来包裹住 JSON 数据，返回数据至客户端

### JSONP 与 GET/POST 请求

我们知道`script`,`link`,`img`等标签引入外部资源，都是`GET`请求，那么就决定了**JSONP 一定是 GET 请求**，即便在`$.ajax()`中使用`POST`请求也能成功。一旦我们指定了`dataType: 'jsonp'`，不管 `type`的值是什么(`GET`/`POST`/甚至不写)，都会进行`GET`请求。 **这是 jquery 在封装 JSONP 跨域时就已经写死了的**

```
// 对应的源码
jQuery.ajaxPrefilter("script", function(s) {
  if (s.cache === undefined) {
    s.cache = false;
  }
  if (s.crossDomain) {
    s.type = "GET";
    s.global = false;
  }
});
```

### AJAX 与 JSONP 的异同

* AJAX 和 JSONP 这两种技术在调用方式上”看起来”很像，目的也一样，都是请求一个`url`，然后把服务器返回的数据进行处理，因此有的框架都把 JSONP 作为 AJAX 的一种形式进行了封装
* AJAX 和 JSONP 其本质上是不同的东西
  * AJAX 的核心是通过 `XmlHttpRequest` 获取非本页内容
  * JSONP 的核心则是动态添加 `<script>`标签来调用服务器提供的 js 脚本
* 两者区别联系
  * 区别不在于是否跨域
  * AJAX 通过服务端代理一样可以实现跨域
  * JSONP 本身也不排斥同域的数据的获取
* JSONP 是一种方式或者说是非强制性的协议
* JSONP 只支持 GET 请求，AJAX 支持 GET 和 POST 请求
