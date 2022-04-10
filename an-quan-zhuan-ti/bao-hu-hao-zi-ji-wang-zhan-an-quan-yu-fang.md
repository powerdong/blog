---
cover: https://s1.ax1x.com/2020/04/06/GsKK00.png
coverY: 0
---

# 保护好自己——网站安全，预防

## 网站安全问题预防

关于前端的安全问题，我们不能归之避之，安全，是一个很大的话题，各种安全问题的类型也种类繁多。如果我们把安全问题按照所发生的的区域来进行分类的话，那么 **所有发生在后端服务器、应用、服务当中的安全问题就是“后端安全问题”，所有发生在浏览器、单页面应用、Web 页面当中的安全问题则算是“前端安全问题”**

![GsKe6s.png](https://s1.ax1x.com/2020/04/06/GsKe6s.png)

> 比如说：SQL 注入漏洞发生在后端应用中，是后端安全问题 跨站脚本攻击(XSS)则是前端安全问题，因为它发生在用户的浏览器里

### 八大前端安全问题

* 八大前端安全问题
  * 老生常谈的 XSS
    * [如何防御](https://powerdong.github.io/myBlog/2019/11/13/http-%E7%BD%91%E7%AB%99%E5%AE%89%E5%85%A8/#%E5%A6%82%E4%BD%95%E9%98%B2%E5%BE%A1)
    * [其他防御措施](https://powerdong.github.io/myBlog/2019/11/13/http-%E7%BD%91%E7%AB%99%E5%AE%89%E5%85%A8/#%E5%85%B6%E4%BB%96%E9%98%B2%E5%BE%A1%E6%8E%AA%E6%96%BD)
  * 警惕 iframe 带来的风险
    * [如何防御](https://powerdong.github.io/myBlog/2019/11/13/http-%E7%BD%91%E7%AB%99%E5%AE%89%E5%85%A8/#%E5%A6%82%E4%BD%95%E9%98%B2%E5%BE%A1-1)
  * 别被点击劫持了
    * [如何防御](https://powerdong.github.io/myBlog/2019/11/13/http-%E7%BD%91%E7%AB%99%E5%AE%89%E5%85%A8/#%E5%A6%82%E4%BD%95%E9%98%B2%E5%BE%A1-2)
  * 错误的内容推断
    * [如何防御](https://powerdong.github.io/myBlog/2019/11/13/http-%E7%BD%91%E7%AB%99%E5%AE%89%E5%85%A8/#%E5%A6%82%E4%BD%95%E9%98%B2%E5%BE%A1-3)
  * [防火防盗防猪队友：不安全的第三方依赖包](https://powerdong.github.io/myBlog/2019/11/13/http-%E7%BD%91%E7%AB%99%E5%AE%89%E5%85%A8/#%E9%98%B2%E7%81%AB%E9%98%B2%E7%9B%97%E9%98%B2%E7%8C%AA%E9%98%9F%E5%8F%8B%E4%B8%8D%E5%AE%89%E5%85%A8%E7%9A%84%E7%AC%AC%E4%B8%89%E6%96%B9%E4%BE%9D%E8%B5%96%E5%8C%85)
  * 用了 HTTPS 也可能掉坑里
    * [如何防御](https://powerdong.github.io/myBlog/2019/11/13/http-%E7%BD%91%E7%AB%99%E5%AE%89%E5%85%A8/#%E5%A6%82%E4%BD%95%E9%98%B2%E5%BE%A1-4)
  * 本地存储数据泄露
    * [如何防御](https://powerdong.github.io/myBlog/2019/11/13/http-%E7%BD%91%E7%AB%99%E5%AE%89%E5%85%A8/#%E5%A6%82%E4%BD%95%E9%98%B2%E5%BE%A1-5)
  * 缺失静态资源完整性校验
    * [如何防御](https://powerdong.github.io/myBlog/2019/11/13/http-%E7%BD%91%E7%AB%99%E5%AE%89%E5%85%A8/#%E5%A6%82%E4%BD%95%E9%98%B2%E5%BE%A1-6)
  * CSRF 跨站请求伪造
    * [如何防御](https://powerdong.github.io/myBlog/2019/11/13/http-%E7%BD%91%E7%AB%99%E5%AE%89%E5%85%A8/#%E5%A6%82%E4%BD%95%E9%98%B2%E5%BE%A1-7)
  * [时刻谨记](https://powerdong.github.io/myBlog/2019/11/13/http-%E7%BD%91%E7%AB%99%E5%AE%89%E5%85%A8/#%E6%97%B6%E5%88%BB%E8%B0%A8%E8%AE%B0)

#### 老生常谈的 XSS

XSS 是跨站脚本攻击 (Cross-Site Scripting) 的简称，XSS 这类安全问题发生的本质原因在于，浏览器错误的将攻击者提供的用户输入数据当做 JavaScript 脚本执行

* XSS 有几种不同的分类方法
  * 按照恶意输入的脚本是否在应用中存储
    * XSS 被划分为 **存储型 XSS** 和 **反射型 XSS**
  * 按照是否和服务器有交互
    * XSS 被划分为 **Server Side XSS** 和 **DOM based XSS**

> 无论怎么分类，XSS 漏洞始终是威胁用户的一个安全隐患。攻击者可以利用 XSS 漏洞来窃取包括用户身份信息在内的各种敏感信息，修改 Web 页面以欺骗用户，甚至控制受害者浏览器，或者和其他漏洞结合起来形成蠕虫攻击，等等。

**如何防御**

防御 XSS 最佳的做法就是对数据进行严格的输出编码，使得攻击者提供的数据不再被浏览器认为是脚本而被误执行。 例如`<script>`进行 HTML 编码后会变成`<script>`,而这段数据就会被浏览器认为只是一段普通的字符串，而不会被当做脚本执行了

> 需要根据输出数据所在的上下文来进行相应的编码

* 如果放在 HTML 元素中，因此进行的是 HTML 编码
* 如果放置在 URL 中，则需要进行 URL 编码
* 此外还有 JavaScript 编码、CSS 编码、HTML 属性编码、JSON 编码等等

**其他防御措施**

* 设置 CSP HTTP Header
  * 需要配置你的网络服务器返回 `Content-Security-Policy` HTTP 头部
  * `<meta http-equiv="Content-Security-Policy" content="default-src 'self'; img-src https://*; child-src 'none';">`
* 输入验证
* 开启浏览器 XSS 防御
  * 浏览器默认有开启
  * 关闭 Chrome XSS 防御 `"浏览器安装路径\chrome.exe" --args --disable-xss-auditor`

#### 警惕 iframe 带来的风险

![GsK3hF.png](https://s1.ax1x.com/2020/04/06/GsK3hF.png)

有时候我们前端页面需要用到第三方提供的页面组件，通常会以 iframe 的方式引入。

> 使用 iframe 在页面上添加第三方提供的广告、天气预报、社交分享插件等等

iframe 在给我们的页面带来更多丰富的内容和能力的同时，也带来了不少的安全隐患。

因为 iframe 中的内容是由第三方提供的，默认情况下他们不受我们的控制，**他们可以在 iframe 中运行 JavaScript 脚本、Flash 插件、弹出对话框等等，这可能会破坏前端用户体验**

如果说 iframe 中的域名因为过期而被恶意攻击者攻击，或者第三方被黑客攻击，iframe 中的内容被替换掉，从而利用用户浏览器中的安全漏洞下载安装木马、恶意勒索软件等等，这问题可就大了

**如何防御**

在 HTML5 中，iframe 有了一个叫做 `sandbox` 的安全属性，通过它可以对 iframe 的行为进行各种限制，充分实现”最小权限”原则。

```
<iframe sandbox src="..."> ... </iframe>
```

如果只是添加上这个属性而保持属性值为空，那么浏览器将会对 iframe 实施最严厉的限制，\*\*基本上就是除了允许显示静态资源之外，其他什么都做不了。\*\*比如不准提交表单、不准弹窗、不准执行脚本等。

* sandbox 丰富的配置参数
  * `allow-forms`: 允许 iframe 中提交 from 表单
  * `allow-popups`: 允许 iframe 中弹出新的窗口或者标签页
  * `allow-scripts`: 允许 iframe 中执行 JavaScript
  * `allow-same-origin`: 允许 iframe 中的网页开启同源策略

#### 别被点击劫持了

![GsKJ1J.png](https://s1.ax1x.com/2020/04/06/GsKJ1J.png)

这是一种欺骗性比较强，同时也需要用户高度参与才能完成的一种攻击

1. 攻击者精心构造了一个诱导用户点击的内容
2. 将我们的页面放到 iframe 当中
3. 利用 `z-index` 等 CSS 样式将这个 iframe 叠加到小游戏的垂直方向的正上方
4. 把 iframe 设置为 100% 透明度
5. 受害者访问到这个页面后，肉眼看到的是一个小游戏，如果收到诱导进行了点击的话，实际上点击到的却是 iframe 中的我们的页面

**点击劫持的危害在于，攻击利用了受害者的用户身份，在其不知情的情况下进行一些操作。**

**如何防御**

* 使用 `X-Frame-Options: DENY` 这个 HTTP header 来明确告知浏览器，不要把当前 HTTP 响应中的内容在 HTML Frame 中显示出来

#### 错误的内容推断

> 某网站允许用户在评论里上传图片，攻击者在上传图片的时候，看似提交的是个图片文件，实则是个含有 JavaScript 的脚本文件。
>
> 接下来，受害者在访问这段评论的时候，浏览器会去请求这个伪装成图片的 JavaScript 脚本，那么就会把这个图片文件当做 JavaScript 脚本执行，于是攻击也就成功了

**问题的关键在于，后端服务器在返回的响应中设置的 `Content-Type-Header` 仅仅只是给浏览器提供当前响应内容类型的建议，而浏览器有可能会自作主张的根据响应中的实际内容去推断内容的类型**

**如何防御**

通过设置 `X-Content-Type-Options` 这个 HTTP Header 明确禁止浏览器去推断响应类型

#### 防火防盗防猪队友：不安全的第三方依赖包

![GsKLBq.png](https://s1.ax1x.com/2020/04/06/GsKLBq.png)

据统计，一个应用有将近 80% 的代码其实来自第三方组件，依赖的类库等

这样做的好处显而易见，但是与此同时安全风险也在不断累积——应用使用了如此多的第三方代码，不论应用自己的代码安全性有多高，一旦这些来自第三方的代码有安全漏洞，那么对应用整体的安全性依然会造成严峻的挑战

#### 用了 HTTPS 也可能掉坑里

![GsKs9e.png](https://s1.ax1x.com/2020/04/06/GsKs9e.png)

为了保证信息在传输过程中不被泄露，保证传输安全，使用 TLS 或者通俗的讲，使用 HTTPS 已经是当前的标准配置了。然而，即使是服务端开启了 HTTPS，也还是存在安全隐患，**黑客可以利用 `SSL Stripping` 这种攻击手段，强制让 HTTPS 降级回 HTTP，从而继续进行中间人攻击**

问题的本质来自于浏览器发出第一次请求就被攻击者拦截了下来并做了修改，根本不给浏览器和服务器进行 HTTPS 通信的机会。

* 用户在浏览器里输入 URL 的时候往往不是从 `https://` 开始的，而是直接从域名开始输入
* 然后浏览器向服务器发起 HTTP 通信
* 由于攻击者的存在，他把服务器端返回的跳转到 HTTPS 页面的响应拦截了
* 并且代替客户端和服务端进行后续的通信
* 由于这一切都是暗中进行的，所以使用前端应用的用户对此毫无察觉

**如何防御**

解决这个安全问题的办法是使用 `HSTS (HTTP Strict Transport Security)`，通过`Strict-Transport-Security: max-age=<seconds>; includeSubDomains; preload` 头部设置来告知浏览器在和网站进行通信的时候强制性使用 HTTPS，而不是通过明文的 HTTP 通信

这里的“强制性”表现为浏览器无论在何种情况下都直接向服务器发起 HTTPS 请求，而不再像以往那样从 HTTP 跳转到 HTTPS。

另外，当遇到证书或者链接不安全的时候，则首先警告用户，并且不再让用户选择是否继续进行不安全的通信

#### 本地存储数据泄露

![GsMSCF.png](https://s1.ax1x.com/2020/04/06/GsMSCF.png)

随着单页面应用的大量出现，存储在前端的数据量也在逐渐增多

前端应用是完全暴露在用户以及攻击者面前的，在前端存储任何敏感，机密的数据，都会面临泄露的风险，就算在前端通过 JS 脚本对数据进行加密基本也无济于事

**如何防御**

推荐的做法是**尽可能不在前端存这些与用户相关的私密数据**

#### 缺失静态资源完整性校验

![GsMCv9.png](https://s1.ax1x.com/2020/04/06/GsMCv9.png)

出于性能考虑，前端应用通常会把一些静态资源存放在 CDN(Content Delivery Networks) 上面，例如 JavaScript 脚本和 Stylesheet 文件，这么做可以显著提高前端应用的访问速度，但同时却隐含了一个新的安全风险

如果攻击者劫持了 CDN，或者对 CDN 中的资源进行了污染，那么我们的前端应用拿到的就是有问题的 JS 脚本或者 Stylesheet 文件，使得攻击者可以肆意篡改我们的前端页面，对用户实施攻击。

**如何防御**

防御这种攻击的办法是使用浏览器提供的 SRI (Sub Resource Integrity) 功能。

HTML 页面中通过 `<script>` 和 `<link>` 元素所指定的资源文件，每个资源文件都可以有一个 SRI 值， 它由两部分组成，减号(-) 左侧是生成 SRI 值用到的哈希算法名，右侧是经过 Base64 编码后的该资源文件的 Hash 值

```
<script src=“https://example.js” integrity=“sha384-eivAQsRgJIi2KsTdSnfoEGIRTo25NCAqjNJNZalV63WKX3Y51adIzLT4So1pk5tX”></script>
```

浏览器在处理这个 script 元素的时候，就会检查对应的 JS 脚本文件的完整性，看其是否和 script 元素中的 integrity 属性指定的 SRI 值一致，如果不匹配，浏览器则会终止对这个 JS 脚本的处理

#### CSRF 跨站请求伪造

![GsMmCD.png](https://s1.ax1x.com/2020/04/06/GsMmCD.png)

> CSRF（Cross-site request forgery 跨站请求伪造，也被称为“One Click Attack”或者 Session Riding，通常缩写为 CSRF 或者 XSRF，是一种对网站的恶意利用。

会造成用户的登陆状态被盗用，完成业务的请求等

**如何防御**

* 合理使用 post 和 get
  * 日常开发中，遵循提交业务严格按照 post 请求，更不要使用 jsonp 去做提交型接口
* 最简单的办法就是加验证码，这样除了用户，黑客的网站是获取不到用户本次的 session 的验证码的
* cookies 设置 sameSite 属性的值为 strict，这样只有同源网站的请求才会带上 cookies **此方案有浏览器兼容问题**
* 另一种方式，就是在用访问的页面中，都种下验证用的 token，用户所有的提交都必须带上本次页面中生成的 token
  * 服务端随机生成 token，保存在服务端 session 中，同时保存到客户端中，客户端发送请求时，把 token 带到 HTTP 请求头或参数中，服务端接收到请求中，验证请求中的 token 与 session 中的是否一致

#### 时刻谨记

1. 开发时要提防用户产生的内容，要对用户输入信息进行层层层检测
2. 注意对用户的输出内容进行过滤
3. 重要的内容记得要加密传输
4. get 请求和 post 请求，严格遵守规范
5. 对于 URL 上携带的信息，要谨慎使用
6. **心中时刻记着，自己的网站哪里可能有危险**
