# 蛋糕分割整合工具——Webpack-前端工程化

### 历史状况

Web 前端开发这几年发展非常迅速，非常多的开发框架和构建工具的涌现，敬爱的前端攻城狮，可能你昨天还在用的工具、插件，到了今天可能就过时了。到了今天，外面的世界已经变了。

> 当你在遇到一个 Bug 需要修复时，没有比工程化推广更迫切的事情了

### 需要解决的问题

工具和语言虽然差异大，但是解决的都是相似的问题，大概有以下几点

* 扩展 JavaScript、HTML、Css 本身的语言能力
* 解决重复工作
* 模板化、模块化
* 解决功能复用和变更的问题
* 解决开发和产品环境差异问题
* 解决发布流程问题

### 什么是前端工程化

所谓工程化，可以简单认为是将框架的职责拓宽再拓宽，主旨是帮业务团队更好的完成需求，工程化会预测一些常碰到的问题，将之扼杀在摇篮，而这种路径是可重用的，是具有可持续性。

> 比如第一个优化去除冗余，是在多次去除冗余代码，思考冗余出现的原因而最终思考得出一个避免冗余的方案。

![nOI3pF.jpg](https://s2.ax1x.com/2019/09/20/nOI3pF.jpg)

### 自动化构建工具

说到构建工具，我们往往会在前面加上(**自动化**)三个字，因为构建工具就是用来让我们不再做机械重复的事情，解放我们的双手。

要完成前端工程化，少不了工程构建工具，`requireJS`与`grunt`的出现，改变了业界前端代码的编写习惯，同时他们也是推动前端工程化的一个基础

`requireJS`是一伟大的模块加载器，他的出现让 JavaScript 制作多人维护的大型项目成为可能。 `grunt`是一款 JavaScript 构建工具，主要完成编译、压缩、合并等一系列工作，后续又出了`Gulp`、`WebPack`等构建工具。

**`WebPack` 具有`Gulp`、`grunt`对于静态资源自动化构建的能力，但更重要的是，`WebPack` 弥补了`requireJS`在模块化方面的缺陷，同时兼容 AMD 与 CMD 的模块加载规范，具有更强大的 JS 模块化功能。**

### 两种模式

#### 开发模式

> 开发模式比较简单，主要就是监听文件变化，自动进行打包、合并的操作

#### 生产模式

参考我们得技术栈与需求，我们的静态文件都要发布到 cdn 上，而且**必须有 md5 版本号**，方便快速发布(**cdn 更新极其缓慢，所以更新必须使用新的文件名**)

> 生产模式主要增加了文件压缩、文件 md5 名修改、替换 HTML 等操作

这样的好处是上线非常方便，一个命令即可更新线上，而且不存在缓存问题

### 什么是 CDN

CDN 是构建在网络之上的内容分发网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN 的关键技术主要有内容存储和分发技术。

CDN 的基本原理是广泛采用各种缓存服务器，将这些缓存服务器分布到用户访问相对集中的地区或网络中，在用户访问网站时，利用全局负载技术将用户的访问指向距离最近的工作正常的缓存服务器上，由缓存服务器直接响应用户请求。

CDN 网络是在用户和服务器之间增加 Cache 层，如何将用户的请求引导到 Cache 上获得源服务器的数据，主要是通过接管 DNS 实现，这就是 CDN 的最基本的原理

#### 请求流程

![nOoauj.png](https://s2.ax1x.com/2019/09/20/nOoauj.png)

1. 用户向浏览器输入`www.web.com`这个域名，浏览器第一次发现本地没有 dns 缓存，则向网站的 dns 服务器请求
2. 网站的 dns 域名解析设置了 Cname，指向了`wwww.web.51cdn.com`，请求指向了 CDN 网络中的智能 DNS 负载均衡系统
3. 智能 DNS 负载均衡系统解析域名，把对用户响应速度最快的 IP 节点返回给用户
4. 用户向该节点(CDN 服务器)发出请求
5. 由于是第一次访问，CDN 服务器会向原 Web 站点请求，并缓存内容
6. 请求结果发送给用户

### 为什么要用 WebPack

![nOT1sJ.png](https://s2.ax1x.com/2019/09/20/nOT1sJ.png)

前端需要模块化:JS 模块化不仅仅是为了提高代码复用性，多人协作开发项目，更是为了让资源文件更合理的进行缓存

当在 WebPack 处理后，输出的静态文件只剩下 JS、png、Css、jpg

#### Webpack 是什么，有什么好处

* WebPack 可以看做是模块打包机：它做的事情是，分析你的项目结构，找到 JavaScript 模块以及其它的一些浏览器不能直接运行的拓展语言（Sass，TypeScript 等），并将其打包为合适的格式以供浏览器使用。
* 为了简化开发的复杂度
* 模块化，让我们可以把复杂的程序细化为小的文件;
* CSS modules
* sass，less 等 CSS 预处理器
* webpack-plugins
* 插件（Plugins）是用来拓展 Webpack 功能的，它们会在整个构建过程中生效，执行相关的任务。

#### WebPack 功能特性

* 支持 CommonJS 和 AMD 模块，意思就是我们基本可以无痛迁移旧项目
* 支持模块加载器和插件机制，可对模块灵活定制
* 可以通过配置打包多个文件，有利于浏览器的缓存功能提升性能
* 将样式文件和图片等静态资源也可视为模块进行打包
* 内置有`source map`，即使打包在一起依旧方便调试

> **任何静态资源都可以视作模块，然后模块之间也可以相互依赖，通过 WebPack 对模块进行处理后，可以打包我们想要的静态资源**

#### 使用 WebPack

* 新建 WebPackDemo 文件夹，使用`npm init`命令生成`package.json`文件。
* 使用`npm install webpack -D`安装 `webpack`

![nOqPkn.png](https://s2.ax1x.com/2019/09/20/nOqPkn.png)

*   在文件夹下创建

    ```
    webpack.config.js
    ```

    文件，编写配置文件

    * entry: 是页面入口文件配置(HTML 文件引入的唯一的 js 文件)
    * output: 对应输出项配置(path: 入口文件最终要输出到哪里，filename: 输出文件的名称，publicPath: 公共资源路径)

![nOXj4s.png](https://s2.ax1x.com/2019/09/20/nOXj4s.png)

项目结构

```
webpack-demo
|-- node_modules
|-- src
  |-- index.js
| index.html
| package.json
| webpack.config.js
```

* webpack 打包当前项目操作指令 `npx webpack`

会在项目文件夹中生成一个 out 文件夹，里边会有编译后的 index.js。

**Webpack loader 加载器**

**loader 意义** 这些加载器主要做一些预处理的工作，比如 less 等，这里我们以 css 和 babel 编译 E2015 为例

**安装 loader** 我们第一步就是先要安装好各个需要使用的 loader。 运行以下命令进行安装`npm install babel-loader babel babel-core css-loader style-loader -D`

添加 css 文件，在 js 文件中引用之后使用 WebPack 打包

![nXS9JJ.png](https://s2.ax1x.com/2019/09/20/nXS9JJ.png)

> 如果编译过程中出现报错 Error: Cannot find module ‘@babel/core’ babel-loader@8 requires Babel 7.x (the package ‘@babel/core’). If you’d like to use Babel 6.x (‘babel-core’), you should install ‘babel-loader@7’. 使用命令 `npm install babel-loader@7`

编译完成后查看`index.html`，查看源码后发现`head`标签里多出了`style`标签，里面正是我们想要的样式。 ![nXpZXq.png](https://s2.ax1x.com/2019/09/20/nXpZXq.png)

**图片打包**

我们之前也说过，WebPack 对于静态资源来说，也是看做模块来加载的，Css 我们是已经看过了。那么图片是怎么作为模块打包加载进来的呢？这里我们可以想到，图片我们是`url-loader`加载的，我们在 Css 文件里的 url 属性，其实就是一种封装处理过的 require 操作，当然我们还有一种方式就是直接对元素的 src 属性进行 require 赋值。

```
npm install url-loader file-loader
div {
  background-image: url("./1.jpg");
}
let img = document.createElement("img");
img.src = require("./image/xxx.png");
document.body.appendChild(img);
```

上述两种方法都会对符合要求的图片进行处理。而要求就是在 url-loader 后面通过 query 参数的方式实现的，这里就是说只有不大于**8kb**的图片才会打包处理成 Base64 格式的图片。

```
{
  test: /.jpg|png|gif|svg$/,
  use: ['url-loader?limit=8192&name=./[name].[ext]']
}
```

`publicPath` 是像图片这种静态资源打包后的根路径，当浏览器需要引用输入静态资源文件时，这个配置项指定输入文件的公共 URL 地址。在 loader 中它被嵌入到 script 或者 link 标签或者对静态资源的引用里。当文件的 href 或者 url()与它在磁盘上的路径不一致时就应当用 publicPath，当你想定义一些或者所有文件放在不同的主机或 CDN 上时会有用。

![nXm2b4.png](https://s2.ax1x.com/2019/09/20/nXm2b4.png)

编译后实现效果

![nXMHfA.png](https://s2.ax1x.com/2019/09/20/nXMHfA.png)

**打包多个文件**

WebPack 不仅适用于 SPA 开发，对于多页面，多站点，WebPack 支持的很好，通过更改配置文件为多入口

output 设置里面`[name]`代表 `entry` 的每一个键值，因此运行 WebPack 时候会输出对应多个文件，`out/page1.js`,`out/page2.js`,在`index.html`和`index2.html`两个页面分别引用。

![nvT4bQ.png](https://s2.ax1x.com/2019/09/21/nvT4bQ.png)

**插件**

代码压缩插件：uglifyjs-webpack-plugin

插件下载 `npm install uglifyjs-webpack-plugin --save-dev`

插件使用

```
const UglifyJsPlugin = require("uglifyjs-webpack-plugin");

module.exports = {
  optimization: {
    minimizer: [new UglifyJsPlugin()]
  }
};
```

具体配置请访问:https://github.com/webpack-contrib/uglifyjs-webpack-plugin

\*\*页面分离：\*\*前端工程一项就是减少 http 请求，这表示需要把多个 js 合并成一个，但是，单个 JS 文件过大会影响浏览器下载文件的速度，由于现在浏览器并发 http 请求多达 6 个，可以利用这个特性，将复用第三方资源库奋力加载。

模块分离插件：SplitChunksPlugin

> Originally, chunks (and modules imported inside them) were connected by a parent-child relationship in the internal webpack graph. The CommonsChunkPlugin was used to avoid duplicated dependencies across them, but further optimizations where not possible
>
> Since version 4 the CommonsChunkPlugin was removed in favor of optimization.splitChunks and optimization.runtimeChunk options.

通过将公共模块拆出来，最终合成的文件能够在最开始的时候加载一次，便存起来到缓存中供后续使用。

我们需要在`webpack.config.js`文件中添加下面的代码

```
splitChunks: {
    chunks: "async", // initial、async和all
    minSize: 30000, // 形成一个新代码块最小的体积 默认是 3000
    minChunks: 1, // 在分割之前，这个代码块最小应该被引用的次数
    maxAsyncRequests: 5, // 按需加载时候最大的并行请求数。
    maxInitialRequests: 3, // 一个入口最大的并行请求数
    automaticNameDelimiter: '~',
    name: true, // 打包的chunks的名字 字符串或者函数(函数可以根据条件自定义名字)
    cacheGroups: { // 这里开始设置缓存的 chunks
        vendors: {
            test: /[\\/]node_modules[\\/]/,
            priority: -10
        },
    default: {
            minChunks: 2,
            priority: -20,
            reuseExistingChunk: true
        }
    }
}
```

具体配置请访问:https://www.webpackjs.com/plugins/split-chunks-plugin/

**Css独立打包**

独立出Css样式，如果我们希望样式通过`<link>`引入，而不是放在`<style>`标签中，即使这样做会多一个请求。

安装 `npm install mini-css-extract-plugin --save-dev`

![nvbIVe.png](https://s2.ax1x.com/2019/09/21/nvbIVe.png)

为了区分开`<link>`链接和用`<style>`，我们这里以Css后缀结尾的模块用插件。

**WebPack 服务器**

> Use webpack with a development server that provides live reloading. This should be used for development only.

安装：`npm install webpack-dev-server --save-dev`

#### Webpack 是如何打包的

WebPack 根据 `webPack.config.js`中的入口文件，在入口文件里识别模块依赖，不管这里的模块依赖是用CommonJS写的，还是用 ES6 Module 规范写的，WebPack会自动进行分析，并通过转换、编译代码、打包成最终的文件。(**最终的文件是基于Webpack自己实现的ES5代码，所以可以跑在浏览器**)

[![nvqCPs.png](https://s2.ax1x.com/2019/09/21/nvqCPs.png)](https://imgchr.com/i/nvqCPs)

#### Webpack 热更新实现原理?

* Webpack 编译期，为需要热更新的 entry 注入热更新代码(EventSource 通信)
* 页面首次打开后，服务端与客户端通过 EventSource 建立通信渠道，把下一次的 hash 返回前端
* 客户端获取到 hash，这个 hash 将作为下一次请求服务端 hot-update.js 和 hot-update.json 的 hash
* 修改页面代码后，Webpack 监听到文件修改后，开始编译，编译完成后，发送 build 消息给客户端
* 客户端获取到 hash，成功后客户端构造 hot-update.js script 链接，然后插入主文档
* hot-update.js 插入成功后，执行 hotAPI 的 createRecord 和 reload 方法，获取到 Vue 组件的 render 方法，重新 render 组件， 继而实现 UI 无刷新更新。
