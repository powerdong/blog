# 拿个小本本记下——Cookie

### Cookie

#### 什么是 Cookie

Cookie 是由服务端生成，发送给 User-Agent(一般是浏览器)，(服务器告诉浏览器设置一下 Cookie)，浏览器会将 Cookie 以 key/value 保存到某个目录下的文本文件内，下次请求同一网站时就发送该 Cookie 给服务器(前提是浏览器设置为启用 Cookie)。

> Cookie 就是一个小型文件(浏览器对 Cookie 的内存大小是有限制的——用来记录一些信息)

#### 为什么会有 Cookie

Web 应用程序是使用 HTTP 协议传输数据的。 **HTTP 协议是无状态的协议。** 一旦数据交换完毕，客户端与服务端的连接就会关闭，再次交换数据需要建立新的连接。这就意味着服务器无法从连接上跟踪会话。

> 用户在登陆页面后，一段时间内重新打开页面不用输入自动登录(**Cookie 具有保质期**)

#### Cookie 的原理

第一次访问网站的时候，浏览器发出请求，服务器响应请求后，会将 Cookie 放到响应请求中，在浏览器第二次发请求时，会把 Cookie 带过去，服务端会辨别用户身份，当然**服务器也可以修改 Cookie 内容。**

#### Cookie 的属性

这是我做的一个使用 Cookie 记录小方块的位置信息的 demo, 图中显示了众多 Cookie 属性 ![mpb24A.png](https://s2.ax1x.com/2019/08/13/mpb24A.png)

**name**

这个显而易见，就是代表 Cookie 的名字，一个域名下绑定的 Cookie， **name 不能相同，相同的 name 对应的值会被覆盖掉。**

**value**

这个就是每个 Cookie 拥有的一个属性，他**表示 Cookie 的值**

**domain**

这个是指的域名，**代表的是 Cookie 绑定的域名**，如果没有设置，就会自动绑定到执行语句的当前域。

**path**

path 这个属性默认就是 ‘/‘,这个值**匹配的是 web 的路由**

```
// 默认路径
www.baidu.com
// blog 路径
www.baidu.com/blog
```

**当你路径设置成 `/blog` 的时候，其实他会给 `/blog`、`/blogaaa` 等等的绑定 Cookie**

**Expires / Max-Age**

这个属性是 Cookie 的有效期，**一般浏览器的 Cookie 都是默认存储的，当关闭浏览器结束这个会话的时候，这个 Cookie 也就会被删除。** 如果你想要 Cookie 存在一段时间，可以通过**设置 Max-Age 属性以秒为单位**。当值为正数时，Cookie 会在 Max-Age 秒之后被删除，**当 Max-Age 为负数时，表示的是临时存储**，不会生出 Cookie 文件，只会存在浏览器内存中，且只会在打开的浏览器窗口或者子窗口有效。一旦浏览器关闭，Cookie 就会消失，**当 Max-Age 为 0 时，删除 Cookie。**

**Secure**

这个属性译为安全，HTTP 不仅是无状态的，还是不安全的协议，容易被劫持。当这个 **Secure 属性设置为 true 时，此 Cookie 只会在 HTTPS 和 SSL 等安全协议下传输。**

> 这个属性并不能对客户端 Cookie 进行加密，不能保证绝对的安全性

**HttpOnly**

如果这个 **HttpOnly 属性设置为 true，就不能通过 js 脚本来获取 Cookie 的值**，能有效的防止 XSS 攻击

#### Cookie 内存大小受限制

Cookie 有个数和大小的限制，**大小一般是 4k**

> Cookie 在本地，可以被更改文件，敏感数据不要放在 Cookie 里

#### Cookie 的使用

```
// 检测是否启用了cookie
navigator.cookieEnabled
//读取浏览器中的cookie
console.log(document.cookie);
//写入cookie
document.cookie = "name=lambda;path=/;domain=.baidu.com";
```

**获取 Cookie 函数封装**

```
// 获取Cookie
function getCookie (name) {
  let name = name + '='
  let Cookies = document.cookie.split(';')
  for(let i = 0;i < Cookies.length; i++) {
    let Cookie = Cookies[i]
    while(Cookie.charAt(0) == '') {
      Cookie = Cookie.substring(1)
    }
    if(Cookie.indexOf(name) != -1) {
      return Cookie.substring(name.length, Cookie.length)
    }
  }
  return ""
}
// 设置 Cookie
function setCookie (name, value, expires, path, domain, secure) {
  let cookieText = encodeURIComponent(name) + '=' + encodeURIComponent(value)

  if (expires instanceof Date) {
      cookieText += "; expires=" + expires.toGMTString();
  }

  if (path) {
      cookieText += "; path=" + path;
  }

  if (domain) {
      cookieText += "; domain=" + domain;
  }

  if (secure) {
      cookieText += "; secure";
  }

  document.cookie = cookieText;
}
// 删除 Cookie
function deleteCookie (name, path, domain, secure) {
  setCookie(name, "", new Date(0), path, domain, secure)
}
```

### Session

#### 什么是 Session

在计算机中，尤其是在网络应用中，称为“会话控制”。 [Session](https://baike.baidu.com/item/Session/479100) 对象存储特定用户会话所需的属性及配置信息。这样，当用户在应用程序的 Web 页之间跳转时，存储在 [Session](https://baike.baidu.com/item/Session/479100) 对象中的变量将不会丢失，而是在整个用户会话中一直存在下去

### Cookie 和 Session 有什么不同

* 作用范围不同
  * Cookie 保存在客户端(浏览器)
  * Session 保存在服务器端
* 存取方式的不同
  * Cookie 只能保存 ASCII
  * Session 可以存任何数据类型，一般情况下我们可以在 Session 中保存一些常用变量信息，比如说 UserId 等
* 有效期不同
  * Cookie 可设置为长时间保持，比如我们经常使用的默认登陆功能
  * Session 一般失效时间较短，客户端关闭或者 Session 超时都会失效
* 隐私策略不同
  * Cookie 存储在客户端，比较容易遭到不法获取，比如将用户名密码存放在 Cookie 导致信息被窃取
  * Session 存储在服务端，安全性相对 Cookie 要好一些
* 存储大小不同
  * 单个 Cookie 保存的数据不能超过 4K
  * Session 可存储数据远高于 Cookie

### 为什么需要 Cookie 和 Session，他们有什么关联

#### 为什么需要 Cookie

我们都知道浏览器是没有状态的(HTTP 协议无状态)，这意味着浏览器并不知道是张三还是李四在和服务端打交道。这个时候就需要有一个机制来告诉服务端，本次操作用户是否登录，是哪个用户在执行的操作，那这套机制的实现就需要 Cookie 和 Session 的配合。

#### Cookie 和 Session 如何配合

![m9kgL4.png](https://s2.ax1x.com/2019/08/13/m9kgL4.png)

用户第一次请求服务器的时候，服务器根据用户提交的相关信息，创建对应的 Session,请求返回时将此 Session 的唯一标识信息 SessionID 返回给浏览器，浏览器接收到服务器返回的 SessionID 信息后，会将此信息存入到 Cookie 中，同时 Cookie 记录此 SessionID 属于哪个域名

当用户第二次访问服务器的时候，请求会自动判断此域名下是否存在 Cookie 信息，如果存在自动将 Cookie 信息也发送给服务端，服务端会从 Cookie 中获取 SessionID，在根据 SessionID 查找对应的 Session 信息，如果没有找到，说明用户没有登陆或者说登录失效，如果找到 Session 证明用户已经登录可执行后面操作。

### 如果浏览器中禁止了 Cookie，如何保障整个机制的正常运转

1. 每次请求中都携带一个 SessionID 的参数，也可以 POST 的方式提交，也可以在请求的地址后面拼接`?SessionID=123...`
2. Token 机制。Token 机制多用于 APP 客户端和服务器交互的模式，也可用于 Web 端做用户状态管理

### 如何考虑分布式 Session 问题

在互联网公司为了可以支撑更大的流量，后端往往需要多台服务器共同来支撑前端用户请求，那如果用户在 A 服务器登录了，第二次请求跑到服务 B 就会出现登录失效问题。

分布式 Session 一般会有以下几种解决方案：

* Nginx ip\_hash 策略，服务端使用 Nginx 代理，每个请求按访问 IP 的 hash 分配，这样来自同一 IP 固定访问一个后台服务器，避免了在服务器 A 创建 Session，第二次分发到服务器 B 的现象。
* Session 复制，任何一个服务器上的 Session 发生改变（增删改），该节点会把这个 Session 的所有内容序列化，然后广播给所有其它节点。
* 共享 Session，服务端无状态话，将用户的 Session 等信息使用缓存中间件来统一管理，保障分发到每一个服务器的响应结果都一致。

**建议采用第三种方案。**

在前端开发过程中，为了方便服务器更方便的交互或者提升用户体验，我们会在客户端(用户)的本地保存一部分数据，比如`cookie/Storage`。在设备不能上网的情况下，我们可以通过使用本地存储是的程序正常运行。

开发离线 Web 应用需要几个步骤，首先是确保应用知道设备能否上网，再做出下一步操作。最好有一块本地存储的数据，无论设备能否上网，都不妨碍读写。

HTML5 及其相关的 API 让离线开发应用成为了现实。

### 离线检测

要知道设备是否在连网状态，HTML5 定义了一个 `navigator.onLine` 属性，这个属性返回一个 Boolean 值。

* 当为 true 时，表示设备处于在线状态(能上网)
* 当为 false 表示当前设备处于离线状态(不能上网)

```
if (navigator.onLine) {
  // 在线 正常工作
} else {
  // 离线 执行离线任务
}
```

由于 `navigator.onLine` 存在一定的兼容性问题，因此除了 `navigator.onLine` 属性之外，为了更好的确定网络是否可用，HTML5 还定义了两个事件 **`online`** 和 **`offline`**

```
window.addEventListener("online", function() {
  // 在线 正常工作
});

window.addEventListener("offline", function() {
  // 离线 执行离线任务
});
```

> 在实际应用中，最好在页面加载后，先取得应用的状态，然后通过上述两个事件执行后续的函数。

### 客户端存储

在常见的前端开发过程中，大家常用的就是 **`Cookie`**、**`LocalStorage`**、**`SessionStorage`**。这里就为大家来总结一下这三种存储方式的异同点，如有错误，欢迎指正。

#### 三者异同点

* 相同点
  * 三者都是通过键值对的集合进行存储
  * 三者都会在浏览器中进行保存，并且有大小、同源策略的限制
* 不同点
  * 主要用途不同
    * Cookie 主要用于标识用户 ID
    * Web Storage 主要用于浏览器存储数据
  * 操作频率不同
    * 一般不会频繁的操作 Cookie
    * 但是会频繁操作 Web Storage
  * 存储容量不同
    * Cookie 大小一般为 4k 左右
    * Web Storage 每个源支持 5MB 左右
  * 请求方式不同
    * Cookie 通过 HTTP 请求
    * Web Storage 由脚本提交
  * 有效期不同
    * Cookie 可以设置有效期，在当前有效期内有效
    * SessionStorage 在当前页面关闭前有效
    * LocalStorage 永久有效，直到通过脚本或者清除缓存来清除它
  * 页面能否共享
    * SessionStorage 不能共享
    * LocalStorage 在同源文档中可以共享
    * Cookie 在同源，且在 path 规则下文档共享
  * 拥有的 API 不同
    * Cookie 通过 `document.cookie` 设置和读取 cookie
    *   Storage 通过

        ```
        localStorage.name = 'lambda'
        ```

        存储单个值

        * 通过 `localStorage.info = JSON.stringify({name:'lambda',gender: 'man'})` 来存储一个对象

#### Cookie

### Cookie 可以用来做什么

> Cookie 是一种存储在用户电脑上用来记录一些有状态的信息(小数据)

* 记录购物车中商品的 ID
* 记录用户登陆的账号密码(记住密码功能)

由于 Cookie 的大小限制，对于较复杂的需求 Cookie 是满足不了的

如果您需要更多的了解 Cookie 可以查看 [Cookie 和 Session](https://powerdong.github.io/myBlog/2019/08/13/Cookie-%E5%92%8C-Session/)

#### Web Storage

Web Storage 的目的时客服由 Cookie 带来的限制，当数据需要被严格控制在客户端上时，无须持续地将数据发回服务器，Web Storage 的两个主要目标是：

* 提供一种在 Cookie 之外存储会话数据的路径
* 提供一种存储大量可以跨会话存在的数据的机制

Web Storage 中定义了两种对象: SessionStorage 和 LocalStorage，是 Storage 对象的示例。

**Storage 对象的 API**

* Storage.length: 返回存储的数据项数量
* Storage.key(keyIndex)：取得对应索引的键名
* Storage.getItem(keyName)：取得对应键名的值
* Storage.setItem(keyName,keyValue)：增加或更新一个数据项
* Storage.removeItem(keyName)：删除对应键名的数据项
* Storage.clear()：清空所有存储的数据项

### 总结

| 特性      | Cookie                                       | LocalStorage                      | SessionStorage        |
| ------- | -------------------------------------------- | --------------------------------- | --------------------- |
| 数据的生命周期 | 一般由服务器生成，可设置失效时间 如果在浏览器端生成Cookie，默认是关闭浏览器后失效 | 除非被清除，否则永久保存                      | 仅在当前会话下有效，关闭页面或浏览器后清除 |
| 存放数据大小  | 4k左右                                         | 一般为5MB                            |                       |
| 与服务器端通信 | 每次都会携带在HTTP请求头中，如果Cookie保存过多的数据会带来性能问题       | 仅在客户端(即浏览器中保存，不参与和服务器的通信)         |                       |
| 易用性     | 需要程序员自己封装，原生的Cookie接口不好用                     | 原生接口可以接受，亦可再次封装Object和Array有更好的支持 |                       |

### 请描述一下 cookies，sessionStorage 和 localStorage 的区别？

* cookie 是网站为了标示用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密）。 cookie 数据始终在同源的 http 请求中携带（即使不需要），这会在浏览器和服务器间来回传递。
* sessionStorage 和 localStorage 不会自动把数据发给服务器，仅在本地保存。
* **存储大小：**
  * cookie 数据大小不能超过 4k
  * sessionStorage 和 localStorage 虽然也有存储大小的限制，但比 cookie 大得多，可以达到 5M 或更大
* **有期时间：**
  * localStorage 存储持久数据，浏览器关闭后数据不丢失除非主动删除数据；
  * sessionStorage 数据在当前浏览器窗口关闭后自动删除。
  * cookie 设置的 cookie 过期时间之前一直有效，即使窗口或浏览器关闭
