# Node

## Node第一弹

### 首先我们先了解一下 NodeJs 是什么

#### NodeJs 是什么

> Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行时。

* **什么是 V8？**
* V8 JavaScript 引擎是 Google 用于其 Chrome 浏览器的底层 JavaScript 引擎
* Google 使用 V8 创建了一个用 C++ 编写的超快解释器，该解释器拥有另一个独特特征；您可以下载该引擎并将其嵌入任何 应用程序。V8 JavaScript 引擎并不仅限于在一个浏览器中运行。因此，Node 实际上会使用 Google 编写的 V8 JavaScript 引擎，并将其重建为可在服务器上使用。

NodeJs 并不是一门语言，Js 才是语言，NodeJs 是一个 JS 的 runtime，是一个运行时。

#### Node 可以用来做什么

* **Node 非常适合以下情况**：
* 在响应客户端之前，您预计可能有很高的流量，但所需的服务器端逻辑和处理不一定很多。
* Node 是一个程序，作为一个可以将其作为基础进行构建的可扩展 JavaScript 平台，Node 还能完成更多的任务。
* Node 完成了它提供高度可伸缩服务器的目标。它使用了 Google 的一个非常快速的 JavaScript 引擎，即 V8 引擎。它使用一个事件驱动设计来保持代码最小且易于阅读。所有这些因素促成了 Node 的理想目标，即编写一个高度可伸缩的解决方案变得比较容易。

#### 阻塞

I/O 时进程休眠，等待进程完成后进入到进入一步

#### 非阻塞

I/O 时函数立即返回，进程不等待 I/O 完成

### NodeJs 好在哪里

* 前端职责范围变大，统一开发体验
* 在处理高并发，I/O 密集场景性能优势明显
* CPU 密集：压缩，解压，加密，解密 => CPU 运算
* I/O 密集：文件操作，网络操作，数据库

#### Web 常见场景

* 静态资源读取 => I/O 密集
* 数据库操作
* 渲染页面
* **这些场景用 JS 优势更加明显**

### 相关术语

#### 进程

进程是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位

#### 多进程

启动多个进程，多个进程可以一块执行多个任务

#### 线程

进程内一个相对独立，可调度的执行单元，与同属一个进程的线程共享进程的资源

#### 多线程

启动一个进程，在一个进程内启动多个线程，这样，多个线程也可以一块执行多个任务

### NodeJs 单线程

* 单线程只是针对主线程，I/O 操作系统底层多线程调度
* 单线程并不是单进程

#### 常用场景

* Web 高并发，I/O 密集
* **如果 CPU 密集，则不推荐使用 NodeJs**
* WebServer
* 本地代码构建
* 使用工具开发

### NodeJs 和 JavaScript 的区别

![KvaQ9s.png](https://s2.ax1x.com/2019/11/04/KvaQ9s.png)

* ECMAScript
  * 定义了语法，写 JavaScript 和 NodeJs 必须遵守
  * 变量定义，循环，判断，函数
  * 原型和原型链，作用域和闭包，异步
  * 不能操作 DOM，不能监听 click 事件，不能发送 ajax 请求
  * 不能处理 http 请求，不能操作文件
* JavaScript
  * 使用 ECMAScript 语法规范，外加 WebAPI，缺一不可
  * DOM 操作，BOM 操作，事件绑定，AJAX 等
  * 两者结合，即可完成浏览器端的任何操作
* NodeJs
  * 使用 ECMAScript 语法规范，外加 NodeJs API，缺一不可
  * 处理 Http，处理文件等
  * 两者结合，即可完成 server 端的任何操作

#### Node 的能力与应用

* 脱离浏览器运行 JS
* NodeJs Stream
* 服务端 API
* 作为中间层

#### 为什么要分前后端？

![KvaoKP.png](https://s2.ax1x.com/2019/11/04/KvaoKP.png)

因为前端无法操作数据库，需要服务器操作数据库，将数据返回给前端

#### server 开发和前端开发的区别

* 服务端稳定性
* 考虑内存和 CPU(优化，扩展)
* 日志记录
* 安全性
* 集群和服务拆分

### 创建第一个 server

![KvaqUg.png](https://s2.ax1x.com/2019/11/04/KvaqUg.png)

#### 安装 NodeJs

当你还没有安装 NodeJs 时我们可以通过两种方式来安装 Node

1. 使用 nvm
   * nvm 是 NodeJs 版本管理工具，可以切换多个 NodeJs 版本
   * [点击这里跳转到下载地址](https://github.com/coreybutler/nvm-windows/releases)建议下载`nvm-setup.zip`这个
   * 正常的程序安装流程，安装完成后在命令行通过 `nvm` 命令查看是否安装成功
   * 通过 `nvm list` 查看当前已安装所有的 node 版本
   * 通过 `nvm list available` 查看可下载的部分版本列表(建议下载 LTS 列下的版本)
   * 通过 `nvm install v10.15.3` 安装指定的 node 版本
   * 通过 `nvm use 10.15.3` 使用指定版本的 node
2. 通过官网下载安装
   * 访问 [node 官网](https://nodejs.org/en/)
   * 点击对应的 LTS 版本下载安装

#### 使用 Node 启动一个服务

首先我们创建一个`server.js`文件，输入下列的 js 代码

```
// http 为node内置模块
const http = require('http')
// 定义端口为1000
const port = 1000
// 定义主机名(本地多用127.0.0.1或localhost)
const hostname = '127.0.0.1'
// 创建http服务
const server = http.createServer((req, res) => {
  // 通过 req.method 获取当前请求的方法 GET/Post...
  console.log(req.method)
  // 当前请求的url
  console.log(req.url)
  // 要显示在页面中的内容
  res.end('hello server')
})

// 监听端口
server.listen(port, hostname, () => {
  // 成功执行的回调函数
  console.log(`Your application is running here: http://${hostname}:${port}`)
})
```

在命令号使用 `node server.js` 运行 js 文件，它会在命令行中打印出响应的内容

然后我们打开浏览器在地址栏输入 `127.0.0.1:1000` 你会惊奇的发现页面上显示着 `hello server`

![KvnutS.png](https://s2.ax1x.com/2019/11/04/KvnutS.png)

我们再回过头看控制台，上边打印着这次请求的方法和 url

![KvnBc9.png](https://s2.ax1x.com/2019/11/04/KvnBc9.png)

关于请求信息你也可以在 Network 工具中查看

### NodeJs 基础 API

![KvU50U.png](https://s2.ax1x.com/2019/11/04/KvU50U.png)

* path: 提供了一些工具函数，用于处理文件与目录的路径
  * normalize: 规范化路径
  * join: 组合路径
  * resolve: 将相对路径转换为绝对路径
  * basename: 基础名
  * dirname: 文件夹目录
  * extname: 扩展名
* `__dirname`、`__filename` 总是返回文件的绝对路径
* `process.cwd()` 总是返回执行 node 命令所在文件夹
* Buffer
  * Buffer 用于处理二进制数据流
  * Buffer 类在全局作用域中，因此无需使用 `require('buffer').Buffer`
  * `Buffer.alloc(num)` 创建一个长度为 num、且用 0 填充的 Buffer
  * `Buffer.form([1,2,3])` 创建长度为 3，内容值为 1,2,3 的 Buffer
  * `Buffer.alloc(5,1)` 创建长度为 5，用 1 填充
  * 静态方法 ↓
  * `Buffer.byteLength(string[, encoding])` 查看字节长度,encoding 默认为`utf8`
  * `Buffer.isBuffer(obj)` 查看是否是一个 Buffer 对象
  * `Buffer.concat(list[, totalLength])` 拼接 Buffer 对象
  * 实例方法 ↓
  * `Buffer.length` 返回内存中分配给 buf 的字节数。 不一定反映 buf 中可用数据的字节量。
  * `buf.toString([encoding[, start[, end]]])`
  * `buf.fill(value[, offset[, end]][, encoding])` 用指定的 value 填充 buf。 如果没有指定 offset 与 end，则填充整个 buf
  * `buf.equals(otherBuffer)` 如果 buf 与 otherBuffer 具有完全相同的字节，则返回 true，否则返回 false。
  * `buf.indexOf(value[, byteOffset][, encoding])`
  * `buf.copy(target[, targetStart[, sourceStart[, sourceEnd]]])` 拷贝 buf 中某个区域的数据到 target 中的某个区域，即使 target 的内存区域与 buf 的重叠。

#### 使用 string\_decode

```
const StringDecode = require('string_decode')
const decode = new StringDecode('utf-8)
// 使用decode.write() 打印中文，防止乱码
```

#### events 事件

大部分 NodeJs 核心 API 都采用惯用的异步事件驱动架构，其中某些类型的对象(触发器)会周期性地触发命名事件来调用函数对象(监听器)

`eventEmitter.on()` 方法用于注册监听器

`eventEmitter.emit()` 方法用于触发监事件

```
const EventEmitter = require('events')

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter()
myEmitter.on('event', () => {
  // 注册事件
  console.log('触发事件')
})
// 触发事件
myEmitter.emit('event')
```

`removeListener('事件名',方法名)`移出事件方法

`removeAllListeners('事件名')` 移出事件中所有方法

#### fs 文件系统

所有方法都有异步和同步的形式 异步方法的最后一个参数都会一个回调函数，传给回调函数的参数取决于具体方法，但回调函数的第一个参数会保留给异常

```
// =======================读文件
// 异步地读取文件的全部内容。
fs.readFile('文件地址', (err, data) => {
  if (err) {
    throw err
  }
  // data 是文件内容
  console.log(data)
})
// 有返回值，同步返回文件内容
fs.readFileSync('文件地址')
// ======================写文件
// 如果文件已存在则覆盖该文件。 data 可以是字符串或 buffer。
fs.writeFile('./text.txt', 'This is a content', {encoding: 'utf-8'}, err=> {
  if(err) {
    throw err
  }
  console.log('done')
})
// ======================查看文件是否存在
/**
 * 不建议在调用 fs.open()、 fs.readFile() 或 fs.writeFile() 之前使用 fs.stat() 检查文件是否存在。
 * 而是应该直接打开、读取或写入文件，如果文件不可用则处理引发的错误。
*/
fs.start('文件名', (err, stats) => {
  if(err) {
    throw err
  }
  console.log(stats)
  stats.isFile() // 是文件吗
  state.isDirectory() // 是文件夹吗
})
// =====================改名
fs.rename('旧文件名','新文件名', err => {
  if(err) {
    throw err
  }
  console.log('done')
})
// =====================删除文件
fs.unlink('文件名', err => {
  if(err) {
    throw err
  }
  console.log('done')
})
// =====================读文件夹
fs.readdir('文件目录', (err, files) => {
  if(err) {
    throw err
  }
  // 目录下的所有文件名
  console.log(files)
})
// =====================创建文件夹
fs.mkdir('文件夹名', err => {
  if(err) {
    throw err
  }
  console.log('done')
})
// =====================删除文件夹
// 在文件（而不是目录）上使用 fs.rmdir() 会导致在 Windows 上出现 ENOENT 错误、在 POSIX 上出现 ENOTDIR 错误。
fs.rmdir('文件夹名', err => {
  if(err) {
    throw err
  }
  console.log('done')
})
// =====================监听文件变化
fs.mkdir('文件目录', {recursive: true} (eventType, fileName) => {
  // code
})
```

**stream**

```
// 查看文件
const rs = fs.createReadStream('文件名')
rs.pipe(process.stdout)

const ws = fs.createWriteStream('文件名')
ws.write('内容') // 写内容
ws.end() // 写完之后
ws.on('finish', () => {
  console.log('done') // 写完之后触发
})
```

#### util 模块

可以使用 util 模块是函数 promise 化，通过 promise 解决回调地狱

```
const promisify = require('util').promisify
const read = promisify(fs.readFile) // 现在的read就是一个promise对象
read('文件名')
  .then((data) => {
    console.log(data)
  })
  .catch((err) => {
    console.log(err)
  })
```

### 如何判断当前脚本运行在浏览器还是 node 环境中？（阿里）

```
this === window ? 'browser' : 'node'
```

### 对 Node 的优点和缺点提出了自己的看法？

* **（优点）**
  * 因为 Node 是基于事件驱动和无阻塞的，所以非常适合处理并发请求， 因此构建在 Node 上的代理服务器相比其他技术实现（如 Ruby）的服务器表现要好得多。
  * 此外，与 Node 代理服务器交互的客户端代码是由 javascript 语言编写的， 因此客户端和服务器端都用同一种语言编写，这是非常美妙的事情。
* **（缺点）**
  * Node 是一个相对新的开源项目，所以不太稳定，它总是一直在变， 而且缺少足够多的第三方库支持。看起来，就像是 Ruby/Rails 当年的样子。
