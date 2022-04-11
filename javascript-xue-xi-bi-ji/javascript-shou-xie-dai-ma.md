# JavaScript手写代码

### 1. 实现一个`new`操作符

* **过程**
  1. 创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。
  2. 属性和方法被加入到 this 引用的对象中。
  3. 新创建的对象由 this 所引用，并且最后隐式的返回 this 。
* new 操作符做了这些事:
  * 它创建了一个全新的对象
  * 它会被执行`[[Prototype]]`(也就是`__proto__`)链接
  * 它使`this`指向新创建的对象
  * 通过 new 创建的每个对象将最终被`[[Prototype]]`链接到这个函数的 prototype 对象上
  * 如果函数没有返回对象类型 Object(包含 Function、Array、Date、RegExp、Error)，那么 new 表达式中的函数调用将返回该对象引用

```
function New(func) {
  // 创建一个全新的对象
  var res = {};
  if (func.prototype !== null) {
    // 通过new创建的每个对象将链接到这个函数的prototype对象上
    res.__proto__ = func.prototype;
  }
  // 使this指向新创建的对象
  var ret = func.apply(res, Array.prototype.slice.call(arguments, 1));
  if ((typeof ret === "object" || typeof ret === "function") && ret !== null) {
    // 返回该对象引用
    return ret;
  }
  return res;
}

var obj = New(A, 1, 2);
// equals to
var obj = new A(1, 2);
```

### 2. 实现一个`JSON.stringify`

> JSON.stringify(value\[, replacer \[, space]]);

* Boolean|Number|String 类型会自动转换对应的原始值
* undefined、任意函数以及 Symbol，会被忽略(出现在非数组对象的属性值中时)，或者被转换成 null(出现在数组中时)
* 不可枚举的属性会被忽略
* 如果一个对象的属性值通过某种间接的方式指回该对象本身，及循环引用，属性也会被忽略

```
function jsonStringify(obj) {
    let type = typeof obj;
    if(type !== 'object'){
        // Boolean|Number|String 类型会自动转换对应的原始值
        if(/string|undefined|function/.test(type)){
            obj = '"' + obj + '"';
        }
        return String(obj);
    }else{
        let json = [];
        let arr = Array.isArray(obj);
        for (let k in obj) {
            let v = obj[k];
            let type = typeof v;
            if(/string|undefined|function/.test(type)){
                v = '"' + v + '"';
            }else if(type === "object"){
                v = jsonStringify(v);
            }
            json.push((arr ? "" : '"' + k + '":') + String(v));
        }
        return (arr ? "[" : "{") + String(json) + (arr ? "]" : "}")
    }
}

jsonStringify({x : 5}) // "{"x":5}"
jsonStringify([1, "false", false]) // "[1,"false",false]"
jsonStringify({b: undefined}) // "{"b":"undefined"}"
```

### 3. 实现一个`JSON.parse`

> JSON.parse(text,reviver)

用来解析 JSON 字符串，构造由字符串描述的 JavaScript 值或对象。提供可选的 reviver 函数用以在返回之前对得到的对象执行变换(操作)

#### 3.1 第一种：直接调用 `eval`

```
function jsonParse(opt) {
    return eval('(' + opt +')');
}

jsonParse(jsonStringify({x : 5}))
// Object {x:5}
```

> 避免在不必要的情况下使用`eval`，`eval()`是一个危险的函数，他执行的代码拥有着执行者的权利

#### 3.2 第二种：Function

核心:`Function`与`eval`有相同的字符串参数特性。

> ```
> var func = new Function(arg1,arg2,..., functionBody);
> ```

在转换 JSON 的实际应用中，只需要这么做

```
var jsonStr = '{"age":20,"name" : "PowerDong"}';
var json = (new Function('return' + jsonStr))();
```

> `eval`和`Function`都有着动态编译 js 代码的作用，但是在实际的编程中并不推荐使用

### 4. 实现一个`call`或`apply`

`call`语法:

> `fun.call(thisArg,arg1,arg2,...)`,调用一个函数，其具有一个指定的`this`值和分别的提供的参数(参数列表)。

`apply`语法:

> `fun.apply(thisArg,[argsArray]`,调用一个函数，以及作为一个数组(或类似数组的对象)提供平的参数。

#### 4.1 `Function.call`按套路实现

**`call`核心**:

* 将函数设为对象的属性
* 执行并删除这个函数
* 指定`this`到函数并传入给定参数执行函数
* 如果不传入参数，默认指向为 window

**4.1.1 简单版**

```
var foo = {
  value: 1,
  bar: function() {
    console.log(this.value);
  }
};

foo.bar(); //1
```

**4.1.2 完善版**

```
//默认指向window
Function.prototype.myCall = function(content = window){
    //指定this到函数
    content.fn = this;
    let args = [...arguments].slice(1);
    //执行函数
    let result = content.fn(...args);
    //删除函数
    delete content.fn;
    return result;
}
let foo = {
    value : 1;
}
function bar(name,age){
    console.log(name);
    console.log(age);
    console.log(this.value);
}
bar.myCall(foo,'dong','18');// dong  18  1
```

#### 4.2 `Function.apply`的模拟实现

`apply()`的实现和`call()`类似，只是参数形式不同。

```
//默认指向window
Function.prototype.myApply = function(content = window) {
  //指定this到函数
  content.fn = this;
  let result;
  // 判断是否有第二个参数
  if (arguments[1]) {
    result = content.fn(...arguments[1]);
  } else {
    result = content.fn();
  }
  delete content.fn;
  return result;
};
```

### 5. 实现一个`Function.bind()`

`bind()`方法:

> 会创建一个新函数。当这个新函数被调用时，bind()的第一个参数将作为他运行时的`this`，之后的一序列参数将会在传递的实参前传入作为他的参数。

此外，`bind`实现需要考虑实例化后对原型链的影响。

```
Function.prototype.myBind = function(content) {
  if (typeof this != "function") {
    throw Error("not a function");
  }
  // 若没问参数类型则从这开始写
  let fn = this;
  let args = [...arguments].slice(1);

  let resFn = function() {
    return fn.apply(
      this instanceof resFn ? this : content,
      args.concat(...arguments)
    );
  };
  function tmp() {}
  tmp.prototype = this.prototype;
  resFn.prototype = new tmp();

  return resFn;
};
```

### 6. 实现一个继承

**寄生组合式继承:**

核心实现是:**用一个`F`空的构造函数去取代执行了`Parent`这个构造函数**

```
function Parent(name) {
  this.name = name;
}
Parent.prototype.sayName = function() {
  console.log("parent name:", this.name);
};
function Child(name, parentName) {
  Parent.call(this, parentName);
  this.name = name;
}
function create(proto) {
  function F() {}
  F.prototype = proto;
  return new F();
}
Child.prototype = create(Parent.prototype);
Child.prototype.sayName = function() {
  console.log("child name:", this.name);
};
Child.prototype.constructor = Child;

var parent = new Parent("father");
parent.sayName(); // parent name: father

var child = new Child("son", "father");
//圣杯模式(继承)
function inherit(Target, Origin) {
  function F() {}
  F.prototype = Origin.prototype;
  Target.prototype = new F();
  Target.prototype.constuctor = Target;
  Target.prototype.uber = Origin.prototype;
}
```

### 7. 实现一个 JS 函数柯里化

**什么是柯里化?**

> 在计算机科学中，柯里化(Currying)是把接受多个参数的函数变换成接受一个单一参数(最初函数的第一个参数)的函数，并且返回接受余下的参数且返回结果的新函数的技术。

#### 7.1 通用版

```
function curry(fn, args) {
  var length = fn.length;
  var args = args || [];
  return function() {
    newArgs = args.concat(Array.prototype.slice.call(arguments));
    if (newArgs.length < length) {
      return curry.call(this, fn, newArgs);
    } else {
      return fn.apply(this, newArgs);
    }
  };
}

function multiFn(a, b, c) {
  return a * b * c;
}

var multi = curry(multiFn);

multi(2)(3)(4);
multi(2, 3, 4);
multi(2)(3, 4);
multi(2, 3)(4);
```

#### 7.2 `ES6写法`

```
const curry = (fn, arr = [])
            => (...args)
            => (arg
            => arg.length === fn.length? fn(...arg): curry(fn, arg))
            ([...arr, ...args])

let curryTest=curry((a,b,c,d)=>a+b+c+d)
curryTest(1,2,3)(4) //返回10
curryTest(1,2)(4)(3) //返回10
curryTest(1,2)(3,4) //返回10
```

### 8. 手写一个`Promise`

`Promise`规范:

* 三种状态`pending`|`resolved`|`rejected`
* 当处于`pending`状态的时候，可以转移到`resolved`或者`rejected`状态
* 当处于`resolved`状态或者`rejected`状态的时候，就不可变

#### 8.1 `Promise`流程图分析

![Promise](https://user-gold-cdn.xitu.io/2019/3/28/169c500344dfe50a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**`Promise`用法:**

```
var promise = new Promise((resolve, reject) => {
  if (操作成功) {
    resolve(value);
  } else {
    reject(error);
  }
});
promise.then(
  function(value) {
    // resolve
  },
  function(value) {
    // rejected
  }
);
```

#### 8.2 手写

```
// pending状态 => resolved / rejected
// 如果不作处理一直是pending
// Promise 对象 调用then (fn1, fn2)
// 调用then  返回 Promise对象，可以链式调用

function myPromise(fn) {
  if (typeof fn !== "function") {
    throw Error(`Promise resolver ${fn} is not a function`);
  }
  var self = this;
  this.status = "pending";
  this.data = null;
  this.resolvedArr = [];
  this.rejectedArr = [];

  function resolved(data) {
    setTimeout(() => {
      if (self.status === "pending") {
        self.status = "resolved";
        self.data = data;
        self.resolvedArr.forEach(fn => fn());
      }
    }, 0);
  }

  function rejected(err) {
    setTimeout(() => {
      if (self.status === "pending") {
        self.status = "rejected";
        self.data = err;
      }
    }, 0);
  }
  fn(resolved, rejected);
}

myPromise.prototype.then = function(onResolved, onRejected) {
  var self = this;
  if (this.status === "resolved") {
    return new myPromise(function(resolved, rejected) {
      var res = onResolved(self.data);
      if (res instanceof myPromise) {
        res.then(resolved, rejected);
      } else {
        resolved(res);
      }
    });
  }

  if (this.status === "rejected") {
    return new myPromise(function(resolved, rejected) {
      var res = onRejected(self.data);
      if (res instanceof myPromise) {
        res.then(resolved, rejected);
      } else {
        resolved();
      }
    });
  }

  if (this.status === "pending") {
    // 如果现在是pending状态 属于异步
    // 把要做的事情用立即函数存起来  让它执行对应状态下的函数
    return new myPromise(function(resolved, rejected) {
      self.resolvedArr.push(
        (function(onResolved) {
          return function() {
            var res = onResolved(self.data);
            if (res instanceof myPromise) {
              res.then(resolved, rejected);
            } else {
              resolved(res);
            }
          };
        })(onResolved)
      );

      self.rejectedArr.push(
        (function(onRejected) {
          return function() {
            var res = onRejected(self.data);
            if (res instanceof myPromise) {
              res.then(resolved, rejected);
            } else {
              resolved(res);
            }
          };
        })(onRejected)
      );
    });
  }
};
//测试
var mp = new myPromise(function(resolved, rejected) {
  console.log("1");
  resolved("2");
});

mp.then(data => console.log(data));
console.log("3");

//输出 1 3 2
```

#### 8.3 大厂专供版

```
const PENDING = "pending";
const FULFILLED = "fulfilled";
const REJECTED = "rejected";

function Promise(excutor) {
  let that = this; // 缓存当前promise实例对象
  that.status = PENDING; // 初始状态
  that.value = undefined; // fulfilled状态时 返回的信息
  that.reason = undefined; // rejected状态时 拒绝的原因
  that.onFulfilledCallbacks = []; // 存储fulfilled状态对应的onFulfilled函数
  that.onRejectedCallbacks = []; // 存储rejected状态对应的onRejected函数

  function resolve(value) {
    // value成功态时接收的终值
    if (value instanceof Promise) {
      return value.then(resolve, reject);
    }
    // 实践中要确保 onFulfilled 和 onRejected 方法异步执行，且应该在 then 方法被调用的那一轮事件循环之后的新执行栈中执行。
    setTimeout(() => {
      // 调用resolve 回调对应onFulfilled函数
      if (that.status === PENDING) {
        // 只能由pending状态 => fulfilled状态 (避免调用多次resolve reject)
        that.status = FULFILLED;
        that.value = value;
        that.onFulfilledCallbacks.forEach(cb => cb(that.value));
      }
    });
  }
  function reject(reason) {
    // reason失败态时接收的拒因
    setTimeout(() => {
      // 调用reject 回调对应onRejected函数
      if (that.status === PENDING) {
        // 只能由pending状态 => rejected状态 (避免调用多次resolve reject)
        that.status = REJECTED;
        that.reason = reason;
        that.onRejectedCallbacks.forEach(cb => cb(that.reason));
      }
    });
  }

  // 捕获在excutor执行器中抛出的异常
  // new Promise((resolve, reject) => {
  //     throw new Error('error in excutor')
  // })
  try {
    excutor(resolve, reject);
  } catch (e) {
    reject(e);
  }
}

Promise.prototype.then = function(onFulfilled, onRejected) {
  const that = this;
  let newPromise;
  // 处理参数默认值 保证参数后续能够继续执行
  onFulfilled =
    typeof onFulfilled === "function" ? onFulfilled : value => value;
  onRejected =
    typeof onRejected === "function"
      ? onRejected
      : reason => {
          throw reason;
        };
  if (that.status === FULFILLED) {
    // 成功态
    return (newPromise = new Promise((resolve, reject) => {
      setTimeout(() => {
        try {
          let x = onFulfilled(that.value);
          resolvePromise(newPromise, x, resolve, reject); // 新的promise resolve 上一个onFulfilled的返回值
        } catch (e) {
          reject(e); // 捕获前面onFulfilled中抛出的异常 then(onFulfilled, onRejected);
        }
      });
    }));
  }

  if (that.status === REJECTED) {
    // 失败态
    return (newPromise = new Promise((resolve, reject) => {
      setTimeout(() => {
        try {
          let x = onRejected(that.reason);
          resolvePromise(newPromise, x, resolve, reject);
        } catch (e) {
          reject(e);
        }
      });
    }));
  }

  if (that.status === PENDING) {
    // 等待态
    // 当异步调用resolve/rejected时 将onFulfilled/onRejected收集暂存到集合中
    return (newPromise = new Promise((resolve, reject) => {
      that.onFulfilledCallbacks.push(value => {
        try {
          let x = onFulfilled(value);
          resolvePromise(newPromise, x, resolve, reject);
        } catch (e) {
          reject(e);
        }
      });
      that.onRejectedCallbacks.push(reason => {
        try {
          let x = onRejected(reason);
          resolvePromise(newPromise, x, resolve, reject);
        } catch (e) {
          reject(e);
        }
      });
    }));
  }
};
```

### 9 防抖 (`Debounce`) 和 节流 (Throttle)

#### 9.1 防抖 (`Debounce`) 实现

典型例子: 限制 鼠标连击 触发。

一个比较好的解释是:

> 当一个事件发生后，事件处理器要等一定阈值的时间，如果这段时间过去后，再也没有事情发生，就处理最后一次发生的事情。假设还差`0.01`秒就到达指定之间，这时又来了一个事件，那么之前的等待作废，需要重新再等待指定时间

> 举一个小例子：假定在做公交车时，司机需等待最后一个人进入后再关门，每次新进一个人，司机就会把计时器清零并重新开始计时，重新等待 1 分钟再关门，如果后续 1 分钟内都没有乘客上车，司机会认为乘客都上来了，将关门发车。 此时「上车的乘客」就是我们频繁操作事件而不断涌入的回调任务；「1 分钟」就是计时器，它是司机决定「关门」的依据，如果有新的「乘客」上车，将清零并重新计时；「关门」就是最后需要执行的函数。

**原理及实现**

实现原理就是利用定时器，函数第一次执行时设定一个定时器，之后调用时发现已经设定过定时器就清空之前的定时器，并重新设定一个新的定时器，如果存在没有被清空的定时器，当定时器计时结束后触发函数执行。

```
function debounce(fn, wait = 50, immediate) {
  let timer = null;
  return function(...args) {
    if (timer) clearTimeout(timer);
    // immediate 为 true 表示第一次触发后执行
    // timer 为空表示首次触发
    if (immediate && !timer) {
      fn.apply(this, args);
    }

    timer = setTimeout(() => {
      fn.apply(this, args);
    }, wait);
  };
}
//Demo
// 简单的防抖动函数
// 实际想绑定在 scroll 事件上的 handler
function realFunc() {
  console.log("Success");
}

// 采用了防抖动
window.addEventListener("scroll", debounce(realFunc, 500));
// 没采用防抖动
window.addEventListener("scroll", realFunc);
```

#### 9.2 节流 (`Throttle`) 实现

> 可以理解为事件在一个管道中传输，加上这个节流阀以后，事件的流速就会减慢。实际上这个函数的作用就是如此，它可以将一个函数的调用频率限制在一定阈值内，例如 1s，那么 1s 内这个函数一定不会被调用两次。

> 举一个小例子，不知道大家小时候有没有养过小金鱼啥的，养金鱼肯定少不了接水，刚开始接水时管道中水流很大，水到半满时开始拧紧水龙头，减少水流的速度变成 3 秒一滴，通过滴水给小金鱼增加氧气。 此时「管道中的水」就是我们频繁操作事件而不断涌入的回调任务，它需要接受「水龙头」安排；「水龙头」就是节流阀，控制水的流速，过滤无效的回调任务；「滴水」就是每隔一段时间执行一次函数，「3 秒」就是间隔时间，它是「水龙头」决定「滴水」的依据。

**原理及实现**

实现方案有两种

* 第一种是用时间戳来判断是否已到执行时间，记录上次执行的时间戳，然后每次触发事件执行回调，回调中判断当前时间戳距离上次执行时间戳的间隔时候已经到达时间差，如果则执行，并更新上次执行的时间戳，如此循环。
* 第二种就是使用定时器，比如当`scroll`时间刚触发时，打印一个`hello world`，然后设置一个`1000ms`的定时器，此后每次触发`scroll`事件触发回调，如果已经存在定时器，则回调不执行方法，直到定时器触发，`handler`被清除，然后重新设置定时器。

```
// fn 是需要执行的函数
// wait 是时间间隔
const throttle = (fn, wait = 50) => {
  // 上一次执行 fn 的时间
  let previous = 0;
  // 将 throttle 处理结果当作函数返回
  return function(...args) {
    // 获取当前时间，转换成时间戳，单位毫秒
    let now = +new Date();
    // 将当前时间和上一次执行函数的时间进行对比
    // 大于等待时间就把 previous 设置为当前时间并执行函数 fn
    if (now - previous > wait) {
      previous = now;
      fn.apply(this, args);
    }
  };
};
// DEMO
// 执行 throttle 函数返回新函数
const betterFn = throttle(() => console.log("fn 函数执行了"), 1000);
// 每 10 秒执行一次 betterFn 函数，但是只有时间差大于 1000 时才会执行 fn
setInterval(betterFn, 10);
```

### 10. 手写一个深拷贝

有一个最著名的乞丐版实现，在《你不知道的 JavaScript(上)》里也有提及

#### 10.1 乞丐版

```
var newObj = JSON.parse(JSON.stringify(somObj));
```

#### 10.2 网传圣杯模式

```
// 圣杯模式
function deepClone(Target, Origin) {
  var Origin = Origin || {};
  for (var prop in Target) {
    if (Target.hasOwnProperty(prop)) {
      if (Target[prop] !== "null" && typeof Target[prop] === "object") {
        if (Object.prototype.toString.call(Target[prop]) === "[object Array]") {
          Origin[prop] = [];
        } else {
          Origin[prop] = {};
        }
        //Origin[prop] = Object.prototype.toString.call(Target[prop]) === '[object Array]' ? [] :{}//
        deepClone(Target[prop], Origin[prop]);
      } else {
        Origin[prop] = Target[prop];
      }
    }
  }
  return Origin;
}
```

#### 简约版实现

```
// 简约版
function deepCopy(obj) {
  // 判断是否是简单数据类型
  if (typeof obj === "object") {
    // 复杂数据类型
    var result = obj.constructor === Array ? [] : {};
    for (let i in obj) {
      result[i] = typeof obj[i] == "object" ? deepCopy(obj[i]) : obj[i];
    }
  } else {
    // 简单数据类型，直接赋值
    var result = obj;
  }
  return result;
}
```

### 11. 实现一个`instanceOf`

```
function myInstanceOf(left, right) {
  let proto = left.__proto__;
  let prototype = right.prototype;
  while (true) {
    if (proto === null) {
      return false;
    }
    if (proto === prototype) {
      return true;
    }
    proto = proto.__proto__;
  }
}
```

### 12. 数组去重

```
// indexOf实现
var array = [1, 1, '1']

function unique(array) {
  var res = []
  for (var i = 0, len = array.length; i < len; i++) {
    var current = array[i]
    if (res.indexOf(current) === -1) {
      res.push(current)
    }
  }
  return res
}

console.log(unique(array))

// 排序后去重
var array = [1, 1, '1']

function unique(array) {
  var res = []
  var sortedArray = array.concat().sort()
  var seen
  for (var i = 0, len = sortedArray.length; i < len; i++) {
    // 如果是第一个元素或者相邻的元素不相同
    if (!i || seen !== sortedArray[i]) {
      res.push(sortedArray[i])
    }
    seen = sortedArray[i]
  }
  return res
}

console.log(unique(array))

// filter实现
var array = [1, 2, 1, 1, '1']
function unique(array) {
  var res = array.filter(function(item, index, array) {
    return array.indexOf(item) === index
  })
  return res
}
console.log(unique(array))

// 排序去重
var array = [1, 2, 1, 1, '1']
function unique(array) {
  return array
    .concat()
    .sort()
    .filter(function(item, index, array) {
      return !index || item !== array[index - 1]
    })
}
console.log(unique(array))

// Object键值对
var array = [{ value: 1 }, { value: 1 }, { value: 2 }]

function unique(array) {
  var obj = {}
  return array.filter(function(item, index, array) {
    console.log(typeof item + JSON.stringify(item))
    return obj.hasOwnProperty(typeof item + JSON.stringify(item))
      ? false
      : (obj[typeof item + JSON.stringify(item)] = true)
  })
}

console.log(unique(array)) // [{value: 1}, {value: 2}]

// ES6 Set实现
var unique = (a) => [...new Set(a)]
```

### 13. 将浮点数点左边的数每三位添加一个逗号

```
// 如 12000000.11 转化为 12,000,000.11?
function commafy(num) {
  return (
    num &&
    num.toString().replace(/(\d)(?=(\d{3})+\.)/g, function($1, $2) {
      return $2 + ','
    })
  )
}
```

### 如何判断一个对象是否属于某个类？

```
Object.prototype.toString.call()
```
