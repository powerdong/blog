# 事件循环

### 事件循环和Promise

> JS 运行的环境称之为宿主环境。

宿主环境有浏览器，服务器，桌面等…

> 执行栈: (call stack),一个数据结构，用于存放各种函数的执行环境，没一个函数执行之前，它的相关信息会加入到执行栈。

函数调用之前，创建执行环境，然后加入到执行栈；函数调用之后，销毁执行环境。

```
function A() {
  console.log("a"); //  3  创建console函数环境
  function B() {
    console.log("b"); // 5  创建console函数环境
  }
  B(); //  4  创建B函数环境
}

console.log("global"); // 1 函数环境

A(); // 2  创建A函数环境

console.log("global over"); //  6   创建console环境
```

**JS 引擎永远执行的是执行栈的最顶部。**

> **异步函数**:某些函数不会立即执行，需要等到某个时机才会执行，这样的函数称之为异步函数。 比如事件处理函数。异步函数的执行时机，会被宿主环境控制。

#### 浏览器宿主环境中包含 5 个线程

1. **JS 引擎**：负责执行执行栈的最顶部代码
2. **GUI 线程**：负责渲染页面 (`DOMContentLoaded`,`onLoad`)
3. **事件监听线程**：负责监听各种事件 (鼠标，键盘)
4. **计时线程**：负责计时 (`setTimeout`)
5. **网络线程**：负责网络通信 (http 请求)

当上面的线程发生了某些事情，如果该线程发现这件事情有处理程序，它会将该处理程序加入到一个叫做事件队列的内存。当 `JS`引擎发现，执行栈中已经没有了任何内容后，会将事件队列中第一个函数加入到执行队列。

```
setTimeout(() => {
  console.log("1"); //   2
  setTimeout(() => {
    console.log("3"); //    4
  }, 0);
}, 0);

setTimeout(() => {
  console.log("2"); //    3
}, 0);

for (let i = 0; i < 1000; i++) {
  console.log(i); //  1 --- 999
}
```

> JS 引擎对事件队列的取出执行方式，以及与宿主环境的配合，称之为**事件循环**。

事件队列在不同的宿主环境中有所差异，大部分宿主环境会将事件队列进行细分。在浏览器中，事件队列分为两种

*   宏任务

    (队列)

    * 计时器结束的回调、事件回调、`http` 回调等大部分异步函数进入宏队列
*   微任务

    (队列)

    * `Promise` 产生的回调进入微队列

当执行栈清空时，`JS` 引擎首先会将微任务中的所有任务依次执行结束，如果没有微任务，则执行宏任务。

### `Promise`

`Promise` 对象表示任务，它用一种标准格式处理异步任务。

`Promise` 构造函数中传入的任务函数会被立即执行(同步)。`resolve` 和 `reject` 是异步执行。

`resolve` 调用后，把任务标记为成功，可以通过 `Promise` 对象的 `then` 函数注册处理程序，该程序会在任务标记为成功后执行，是异步的。

```
setTimeout(function() {
  console.log("A");
}, 0);
var obj = {
  func: function() {
    setTimeout(function() {
      console.log("B");
    }, 0);
    return new Promise(resolve => {
      console.log("C");
      resolve();
      console.log("D");
    });
  }
};
obj.func().then(function() {
  console.log("E");
});
console.log("F");

// C  D  F  E  A  B
```
