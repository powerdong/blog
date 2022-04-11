# 你一块，我一块——Web-modules 前端模块化

模块化的开发方式可以提高代码的服用效率，方便进行代码的管理。模块化主要体现的是一种分而治之的思想。[分而治之](https://baike.baidu.com/item/%E5%88%86%E8%80%8C%E6%B2%BB%E4%B9%8B%E7%AE%97%E6%B3%95)是复杂系统开发和维护的基石，模块化则是前段最流行的分治阶段。

![n4PnU0.jpg](https://s2.ax1x.com/2019/09/17/n4PnU0.jpg)

### 模块化简介

具有相同属性和行为的事物的集合，在前端中，将一些属性比较类似和行为比较类似的内容放在同一个 JS 文件里面，把这个 JS 文件称为一个模块，为了每个 JS 文件只关注自身有关的事情，让每个 JS 文件各行其职

模块化要有几个特点：**独立**，**完整**，**依赖关系**

在最开始的阶段，JS 并没有模块化机制，各种 JS 到处飞，也就是所谓的野生代码，得不到妥善的管理。后来前端圈开始制定规范，最耳熟能详的是`CommonJS`和`AMD`。\*\*`node.js`\*\*就是`CommonJS`规范的产物。但是由于`CommonJS`同步加载更适合服务端，所以迫切需要一个关于客户端的规范，既又出现了很多客户端，耳熟能详的`AMD`

#### 模块化的实现

* 函数
* 对象的写法
* 匿名函数、返回对象
* 以来传入实参
* **以上缺点依赖关系不好处理，需要按顺序加载，可能会阻塞页面**

### 模块化开发怎么做

立即执行函数,不暴露私有成员

```
var module1 = (function() {
  var _count = 0
  var m1 = function() {
    //...
  }
  var m2 = function() {
    //...
  }
  return {
    m1: m1,
    m2: m2
  }
})()
```

#### 模块化的好处

* 避免命名冲突
* 更好的分离，按需加载
* 更高复用性
* 高可维护性

### 同步加载 CommonJS

![n4CJtf.png](https://s2.ax1x.com/2019/09/17/n4CJtf.png)

根据`CommonJS`规范，每一个文件就是一个模块，其内部定义的变量是属于这个模块的，不会对外暴露，也就是说不会污染全局变量。

> 该规范最初是用来服务器端的 node，前端的 Webpack 也是对 CommonJS 原生支持

`CommonJS` 的核心思想就是通过 `require` 方法来同步加载所要依赖的其他模块，然后通过 `exports` 或者 `mode.exports`来导出需要暴露的接口

#### 使用

```
// index.js
var module = require('module.js)
module.aa('hello')

// module.js
module.exports = {
  aa: function (str) {
    console.log(str)
  }
}
```

#### 特点

* 所有代码都运行在模块作用域，不会污染全局作用域
* 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。**要想让模块再次运行，必须清除缓存**
* 模块加载的顺序，按照其在代码中出现的顺序

**`require`命令的基本功能是，读取并执行一个 JavaScript 文件，然后返回该模块的 exports 对象。如果没有发现指定模块，会报错！**

#### 模块加载机制

**`CommonJS`模块的加载机制是，输入的是被输出的值的拷贝。也就是说，一旦输出一个值，模块的内部变化就影响不到这个值**

```
// module.js
let counter = 3;
function addCounter() {
  counter++;
}
module.exports = {
  counter: counter,
  addCounter: addCounter
};

// index.js
let counter = require("module.js").counter;
let addCounter = require("module.js").addCounter;

console.log(counter); // 3
addCounter();
console.log(counter); // 3
```

上面的代码说明，当`module.js`中的值被输出之后，模块内部的变化已经就影响不到这个值了。**这是因为 counter 是一个原始类型值，会被缓存。除非写成一个函数，才能得到内部变动的值**

**注意:** 浏览器不兼容 `CommonJS`，原因是浏览器缺少 `module`、`exports`、`require`、`global` 四个环境变量。如要使用需要工具转换。

> CommonJS 采用同步加载不同模块文件，适用于服务端 因为模块文件都存放在服务器的各个硬盘上，读取加载时间快，适合服务端，不适应浏览器

### 异步加载 AMD

![nhq259.png](https://s2.ax1x.com/2019/09/17/nhq259.png)

CommonJS 为服务器端而生，采用的同步加载方式。因此不适用与浏览器。因为浏览器需要到服务器加载文件，请求时间远大于本机读取时间，倘若文件较多，网络迟缓就会导致页面瘫痪，所以浏览器更希望能够实现异步加载的方式。

AMD 规范是实现异步加载模块，允许指定回调函数，等模块异步加载完成后即可调用回调函数。

AMD 的核心思想就是通过 `define` 来定义一个模块，然后使用 `require` 来加载一个模块

#### 使用

```
// main.js
// 使用模块
require(["jquery", "math", "module"], function($, math) {
  // your code
});

// math.js
// 定义没有依赖的模块
define(function() {
  // your code
  return 模块;
});

// module.js
// 定义有依赖的模块
define(["module"], function() {
  return 模块;
});
```

AMD 模块定义的方法非常清晰，不污染全局环境，能够清除的显示依赖关系。AMD 模式可以用于浏览器环境，并且允许非同步加载模块，也可以根据需要动态加载模块。

### 异步加载 CMD

![nhj1iQ.png](https://s2.ax1x.com/2019/09/17/nhj1iQ.png)

CMD 异步加载，跟 AMD 的主要区别在于，AMD 依赖前置，提前加载依赖。而 CMD 就近加载，按需加载。

CMD 的核心思想就是通过 define 来定义一个模块，然后使用 require 来加载一个模块。

#### 使用

```
// module1.js
// 定义没有依赖的模块
define(function(require, exports, module) {
  exports.xxx = val;
  module.exports = value;
});

// module2.js
// 定义有依赖的模块
define(function(require, exports, module) {
  // 引入依赖模块(同步)
  let module1 = require("./module1.js");
  // 引入依赖模块(异步)
  require.async("./module1.js", function(m1) {
    // your code
  });
  // 暴露模块
  exports.xxx = value;
});

// index.js
define(function(require) {
  let m1 = require("./module1");
  let m2 = require("./module2");
  m1.show();
  m2.show();
});
```

### ES6 模块化

![n4CnpD.png](https://s2.ax1x.com/2019/09/17/n4CnpD.png)

ES6 自带模块化，可以使用 `import` 关键字引入模块，通过 export 关键字导出模块，**功能较之前的几个方案更为强大，也是我们所推崇的**，但是 ES6 目前无法在浏览器中执行，所以，我们只能通过 babel 将不被支持的 import 编译为当前受到广泛支持的 require

#### 使用

```
// index.js
import tool from "tool.js";

// tool.js
export class show {
  // 导出
  constructor() {
    this.age = 18;
  }
  showName() {
    return this.age;
  }
}
```

#### ES6 模块与 CommonJS 模块的差异

1. CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用
2. CommonJS 模块是运行时加载，ES6 模块是编译时输出接口
3. ES6 模块是动态引用的，并且不会缓存值，模块里边的变量绑定其所在的模块

### 总结

* CommonJS 规范**主要用于服务端编程，加载模块是同步的**，这并不适合在浏览器环境中使用，因为同步意味着阻塞加载，浏览器资源是异步加载的，因此有了 AMD 和 CMD 解决方案
* AMD 规范在浏览器环境中**异步加载模块，而且可以并行加载多个模块**。不过 AMD 规范开发成本高，代码的阅读和书写比较困难，模块定义的方式语义不顺畅
* CMD 规范与 AMD 规范很相似，都用于浏览器编程，**依赖就近，延迟执行**，可以很容易在 Node.js 中运行。不过，依赖 SPM 打包，模块的加载逻辑偏重
* ES6 在语言标准层面上，**实现模块功能，而且实现的相当简单**，完全可以取代 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案
