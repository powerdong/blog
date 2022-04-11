# 喜怒哀乐多状态——HTTP状态码

### HTTP

> [超文本传输协议(HTTP)](https://baike.baidu.com/item/HTTP/243074)(HyperText Transfer Protocol)是用于传输诸如 HTML 的超媒体文档的应用层协议，是互联网上应用最为广泛的一种网络协议。

* **超文本传输协议（HTTP）的设计目的是保证客户机与服务器之间的通信。** HTTP 的工作方式是客户机与服务器之间的请求-应答协议。web 浏览器可能是客户端，而计算机上的网络应用程序也可能作为服务器端

浏览器作为 HTTP 客户端通过 URL 向 HTTP 服务端发送所有请求。Web 服务器根据接收到的请求后，向客户端发送响应信息

#### 主要特点

1. 简单：客户端向服务器请求服务时，只需要传送请求方法和路径
2. 灵活：HTTP 允许传输任何类型的数据对象。由 Content-Type 加以标记 (text/html)
3. 无连接：限制每次连接只处理一个请求。服务器处理完客户端的请求，并收到客户端的应答后，即断开连接。(采用这种方式可以节省传输时间)
4. 无状态：HTTP 协议是无状态协议。这种情况下造成后续请求需要前面的信息，必须重传，导致每次连接的数据量大。

#### HTTP 方法

* **HEAD**: 与 GET 相同，但只返回 HTTP 报头，不返回文档主体
* **PUT**:上传指定的 URI 表示
* **DELETE**: 删除指定资源
* **OPTIONS**: 返回服务器支持的 HTTP 方法
* **CONNECT**: 把请求连接转换到透明的 TCP/IP 通道
* **POST**: 向指定的资源提交要被处理的数

#### HTTP 工作原理

HTTP 协议定义 Web 客户端如何从 Web 服务器请求 Web 页面，以及服务器如何把 Web 页面传送给客户端。HTTP 协议采用了请求/响应模型。客户端向服务器发送一个请求报文，请求报文包含请求的方法、URL、协议版本。请求头部和请求数据。服务器以一个状态行作为响应，响应的头部和响应数据。

### HTTPS

> [HTTPS](https://baike.baidu.com/item/https/285356?fr=aladdin)(Hypertext Transfer Protocol Secure)(安全的 HTTP)是 HTTP 协议加密版本。它通常使用 SSL 或者 TLS 来加密客户端和服务器之间的所有通信。是以安全为目标的 HTTP 通道。

HTTPS 是最流行的 HTTP 安全形势。使用 HTTPS 时，所有的 HTTP 请求和响应数据在发送到网络之前，都要进行加密。

#### SSL

不使用 SSL 的 HTTP 通信，就是不加密的通信，所有信息明文传输，带来了三大风险

* 窃听风险：第三方可以获知通信内容
* 篡改风险：第三方可以修改通信内容
* 冒充风险：第三方可以冒充他人身份参与通信

SSL 协议就是为了解决这三大风险而设计的，希望达到

* 所有信息都是加密传播，第三方无法窃听
* 具有校验机制，一旦被篡改，通信双方会立刻发现
* 配备身份证书，防止身份被冒充

#### HTTPS 方案

现在安全的 HTTP 是可选的。请求一个资源时，会去检查 URL 的方案

* 如果 URL 的方案为 HTTP，客户端就会打开一条到服务器端口默认 80 的连接，并向其发送老的 HTTP 命令。
* 如果 URL 的方案为 HTTPS，客户端就会打开一条到服务器端口默认 443 的连接，然后与服务器“握手”，以二进制格式与服务器交换一些 SSL 安全参数，附上加密的 HTTP 命令

### HTTP(请求报文，响应报文)

HTTP 通过报文进行沟通

#### 请求报文

> 客户端向服务器发起请求时会生成一段请求报文，请求报文是由**请求方法**，**URL**，**协议版本**，可选的请求首部字段和内容实体构成。

一旦连接建立，用户代理可以发送请求(用户代理通常是 Web 浏览器，但也可以是其他的(例如爬虫))。客户端请求由一系列文本指令组成，并使用 CRLF 分隔，它们被划分为三个块:

* 第一行包括请求方法及请求参数:
  * 文档路径，不包括协议和域名的绝对路径 URL
  * 使用的 HTTP 协议版本
* 接下来的行每一行都表示一个 HTTP 首部，为服务器提供关于所需数据的信息(例如语言，或 MIME 类型)，或是一些改变请求行为的数据(例如当数据已经被缓存，就不再应答)。这些 HTTP 首部组成一个空行结束的一个块。
* 最后一行是可选数据块，包含更多数据，主要被 POST 方法所使用

```
<!-- 发送表单的结果 -->
POST / contact_form.php HTTP/1.1
HOST: developer.mozilla.org
Content-length:64
Content-Type: application/x-www-form-urlencoded

name=lambda&age=18
```

![e1O3rD.png](https://user-gold-cdn.xitu.io/2019/8/17/16c9da3039654154?w=875\&h=513\&f=png\&s=419945)

#### 响应报文

> 接收到请求的服务器，会将请求内容的处理结构以响应的形式返回。响应报文基本上由协议版本，状态码，用以解释状态的原因短语，可选的响应首部字段以及实体主体构成。

当收到用户代理发送的请求后，Web 服务器就会处理它，并最终送回一个响应。与客户端请求很类似，服务器响应由一系列文本指令组成，并使用 CRLF 分隔，它们被划分为三个不同的块

* 第一行是状态行，包括使用的 HTTP 协议版本，状态码和一个状态描述(可读描述文本)
* 接下来每一行都表示一个 HTTP 首部，为客户端提供关于所发送数据的一些信息(如类型，数据大小，使用的压缩算法，缓存指示)。与客户端请求的头部块类似，这些 HTTP 首部组成一个块，并以一个空行结束。
* 最后一块是数据块，包含了响应的数据(如果有的话)

```
<!-- 成功的网页响应 -->
HTTP/1.1 200 OK
Date: Sat, 09 Oct 2010 14:28:02 GMT
Server: Apache
Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
ETag: "51142bc1-7449-479b075b2891b"
Accept-Ranges: bytes
Content-Length: 29769
Content-Type: text/html
<!-- 请求资源已被永久移动的网页响应 -->
HTTP/1.1 301 Moved Permanently
Server: Apache/2.2.3 (Red Hat)
Content-Type: text/html; charset=iso-8859-1
Date: Sat, 09 Oct 2010 14:30:24 GMT
Location: https://developer.mozilla.org/ (目标资源的新地址, 服务器期望用户代理去访问它)
Keep-Alive: timeout=15, max=98
Accept-Ranges: bytes
Via: Moz-Cache-zlb05
Connection: Keep-Alive
X-Cache-Info: caching
X-Cache-Info: caching
Content-Length: 325 (如果用户代理无法转到新地址，就显示一个默认页面)
<!-- 请求资源不存在的网页响应 -->
HTTP/1.1 404 Not Found
Date: Sat, 09 Oct 2010 14:33:02 GMT
Server: Apache
Last-Modified: Tue, 01 May 2007 14:24:39 GMT
ETag: "499fd34e-29ec-42f695ca96761;48fe7523cfcc1"
Accept-Ranges: bytes
Content-Length: 10732
Content-Type: text/html
```

![e1OYad.png](https://user-gold-cdn.xitu.io/2019/8/17/16c9da302dbe7d5d?w=880\&h=520\&f=png\&s=191932)

### HTTP 响应代码

> HTTP 响应状态代码指示特定 HTTP 请求是否已成功完成。

响应分为五类：**信息响应(1xx)**，**成功响应(2xx)**，**重定向(3xx)**，**客户端错误(4xx)和服务器错误(5xx)**。

| 状态码 | 定义    | 说明                                       |
| --- | ----- | ---------------------------------------- |
| 1xx | 信息    | 接到请求继续处理                                 |
| 2xx | 成功    | 成功的收到，理解，接收                              |
| 3xx | 重定向   | 为了完成请求需要进行另一步措施(如直接从浏览器缓存中获取资源，或跳转到其它页面) |
| 4xx | 客户端错误 | 请求语法有错误，不能完全符合要求                         |
| 5xx | 服务端错误 | 服务器无法完成明显有效的请求                           |

#### 常见的 HTTP 状态码

* 成功状态码
  * 200 服务器成功返回内容
  * 301/2 永久/临时重定向
  * 304 资源未被修改过
* 失败的状态码
  * 404 请求内容不存在
  * 500 服务器暂时不可用
  * 503 服务器内部错误

### GET 请求和 POST 请求的异同

* 相同点
  * 都将数据提交到远程服务器
* 不同点
  * 提交的位置不同
    * GET 会将数据放在 URL 后面
    * POST 会将数据放到请求头中
  * 提交数据大小限制不同
    * GET 请求对数据有大小限制
    * POST 请求对数据没有大小限制
  * 是否可被缓存
    * GET 请求可被缓存
    * POST 请求不会被缓存
  * 是否会保留历史记录
    * GET 请求保留在浏览器历史记录中
    * POST 请求不会保留在浏览器历史记录中
  * 是否可被收藏
    * GET 请求可被收藏为书签
    * POST 不能被收藏为书签
  * 其他注意
    * GET 请求不应在处理敏感数据时使用
    * GET 请求只应当用于取回数据

#### GET/POST 请求应用场景

* GET 请求用于提交非敏感数据和小数据
* POST 请求用于提交敏感数据和大数据

**注意**

1. 上传文件一般使用 POST 提交
2. 上传文件必须设置 `enctype="multipart/form-data"`

### 浏览器缓存机制(HTTP)

Date ：服务器响应的内容日期 Cache-control ：内容缓存时间 no-cache ：不被缓存的，只不过每次在向客户端（浏览器）提供响应数据时，缓存都要向服务器评估缓存响应的有效性。 no-store ：用于防止重要的信息被无意的发布。在请求消息中发送将使得请求和响应消息都不使用缓存。 根据缓存超时 max-age ：指示客户机可以接收生存期不大于指定时间（以秒为单位）的响应。 min-fresh ：指示客户机可以接收响应时间小于当前时间加上指定时间的响应。 max-stale ：指示客户机可以接收超出超时期间的响应消息。如果指定 max-stale 消息的值，那么客户机可以 接收超出超时期指定值之内的响应消息。 Expires ：内容保质期，表示存在时间，允许客户端在这个时间之前不去检查（发请求），等同 max-age 的效果。但是如果同时存在，则被 cache-control 的 max-age 覆盖。

> **网站如何统计用户从何点击而来?** Referer: 如果从浏览器地址栏里直接输入地址请求头没有 Referer

### Ajax 解决浏览器缓存问题

1. 在 ajax 发送请求前加上 `anyAjaxObj.setRequestHeader("If-Modified-Since","0")`
2. 在 ajax 发送请求前加上 `anyAjaxObj.setRequestHeader("Cache-Control","no-cache")`
3. 在 URL 后面加上一个随机数： `"fresh=" + Math.random();`
4. 在 URL 后面加上时间戳：`"nowtime=" + new Date().getTime();`
5. 如果是使用 jQuery，直接这样就可以了 `$.ajaxSetup({cache:false})`这样页面的所有 ajax 都会执行这条语句就是不需要保存缓存记录。

### ajax 实现原理及方法使用

* readyState 属性有五个状态值。
  * 0：是 uninitialized，未初始化。已经创建了 XMLHttpRequest 对象但是未初始化。
  * 1：是 loading.已经开始准备好要发送了。
  * 2：已经发送，但是还没有收到响应。
  * 3：正在接受响应，但是还不完整。
  * 4：接受响应完毕。
* responseText：服务器返回的响应文本。只有当 readyState>=3 的时候才有值，根据
* readyState 的状态值，可以知道，当 readyState=3，返回的响应文本不完整，只有
  * readyState=4，完全返回，才能接受全部的响应文本。
* responseXML：response as Dom Document object。响应信息是 xml，可以解析为 Dom 对象。
* status：服务器的 Http 状态码，若是 200，则表示 OK，404，表示为未找到。
* statusText：服务器 http 状态码的文本。比如 OK，Not Found。
