# 学会“套路”——设计模式（2）

### JS 中的设计模式

本文章通过介绍常见模式的简介，UML 类图展示，代码示例方式介绍几种常见的设计模式

**设计模式简介相关请访问**[**前端需要了解的设计模式(1)**](bu-yao-hua-li-hu-shao-yao-yi-tao-yi-tao-she-ji-mo-shi-1.md)****

#### 工厂模式

* 将 `new` 操作单独封装
* **遇到 `new` 时，就要考虑是否使用工厂模式**
* 这种设计模式可以大大降低程序模块之间的耦合度，便于更加灵活的扩展和维护

工厂模式是一种使用工厂方法创建对象的设计模式，而不是指定创建对象的确切的类或构造函数

> 示例：当你在购买汉堡时，直接点餐，取餐，不会自己亲手做。商店要”封装”做汉堡的工作，做好直接给买家

![uQPKjx.png](https://s2.ax1x.com/2019/09/28/uQPKjx.png)

```
/**
 * 工厂模式
 * 创建一个产品类，通过不断的 new 产品生产产品
 */
class Product {
  constructor(name) {
    this.name = name
  }
  init() {
    console.log(`创建一个：${this.name}`)
  }
  fn1() {
    console.log(`使用了fn1方法`)
  }
  fn2() {
    console.log(`使用了fn2方法`)
  }
}

class Creator {
  create(name) {
    return new Product(name)
  }
}

//
let creator = new Creator()
let product = creator.create('Hamburger')
product.init() //创建一个：Hamburger
product.fn1() //使用了fn1方法
```

如上述代码，函数`Product`能接受一个参数 name。通过函数`Creator`的`create`方法可以创建一个`Product`实例。可以无数次调用这个函数每次返回都会包含三个方法。

工厂模式是为了解决多个类似对象声明的问题，也是为了解决实例化对象产生重复的问题。

* \*\*优点：\*\*能解决多个相似的问题
* \*\*缺点：\*\*不能知道对象的识别问题(对象的类型不知道)

> 应用场景：jQuery 中的$、Vue.component 异步组件、React.createElement 等

#### 单例模式

* 系统中被唯一使用
* 一个类只有一个实例
* 单例模式提供了一种将代码组织为一个逻辑单元的手段，这个逻辑单元中的代码可以通过单一变量进行访问

实现方法：使用一个变量来标志当前是否已经为某个类创建过对象，如果创建了，则在下一次获取该类的实例时，直接返回之前创建的对象，否则就创建一个对象。

> 示例：登录框、购物车

```
class Singleton {
  login() {
    console.log('login...')
  }
}

Singleton.getInstance = (function() {
  let instance
  return function() {
    if (!instance) {
      instance = new Singleton()
    }
    return instance
  }
})()

let obj1 = Singleton.getInstance()
obj1.login()
let obj2 = Singleton.getInstance()
obj2.login()

console.log(`obj1 === obj2: ${obj1 === obj2}`) // true

// 这里无法完全控制
let obj3 = new Singleton()
obj3.login()

console.log(`obj1 === obj2: ${obj1 === obj3}`) // false
```

要实现一个单例模式的话，我们无非就是使用一个变量来标识该类是否被实例化，如果未被实例化的话，那么我们可以实例化一次，否则的话，直接返回已经被实例化的对象。

**单例模式的优点：**

* 可以用来划分命名空间，减少全局变量的数量
* 使用单例模式可以使代码组织更为一致，使代码容易阅读和维护
* 可以被实例化，且只能实例化一次

> 应用场景：jQuery 中的$、Vuex 中的 Store、Redux 中的 Store 等

#### 适配器模式

* 旧接口格式和使用者不兼容
* 中间加一个适配转化接口
* 用来解决两个接口不兼容问题，由一个对象来包装不兼容的对象，比如参数转换，允许直接访问

![uQVQKA.png](https://s2.ax1x.com/2019/09/28/uQVQKA.png)

```
Class Adoptee {
  specificRequest() {
    return '德国标准插头'
  }
}

class Target {
  constructor() {
    this.adoptee = new Adoptee()
  }
  request() {
    let info = this.adoptee.specificRequest()
    return `${info} --- 转换器 --- 中国标准接头`
  }
}

let target = new Target()
let res = target.request()
console.log(res) // 德国标准插头 -- 转换器 -- 中国标准接头
```

> 应用场景：Vue 的 computed、旧的 JSON 格式转换成新的格式等

#### 装饰器模式

* 为对象添加新功能
* 不改变其原有的结构和功能
* 可以动态的给某个对象添加额外的职责，而不会影响从这个类中派生的其他对象

> 比方说，汽车的成本取决于它的功能数量。 如果没有装饰者模式，我们必须为不同的功能组合创建不同的类，每个类都有一个成本方法来计算成本。

![uQMnoR.png](https://s2.ax1x.com/2019/09/28/uQMnoR.png)

```
class Circle {
  draw() {
    console.log('画一个圆形')
  }
}

class Decorator {
  constructor(circle) {
    this.circle = circle
  }
  draw() {
    console.log('画一个圆形')
    this.setRedBorder(this.circle)
  }
  setRedBorder(circle) {
    console.log('设置红色边框')
  }
}

let circle = new Circle()
circle.draw() // 画一个圆形
console.log('-------------')
let dec = new Decorator(circle)
dec.draw() // 画一个圆形 设置红的边框
```

#### 代理模式

* 使用者无权访问目标对象
* 中间加代理，通过代理做授权和控制
* 为其他对象提供一种代理，便以控制对这个对象的访问，不能直接访问目标对象

![uQJPHS.png](https://s2.ax1x.com/2019/09/28/uQJPHS.png)

```
class ReadImg {
  constructor(fileName) {
    this.fileName = fileName
    // 私有方法自己调用
    this.loadFormDisk()
  }
  display() {
    console.log(`display ... ${this.fileName}`)
  }
  loadFromDisk() {
    console.log(`loading ... ${this.fileName}`)
  }
}

class ProxyImg {
  constructor(fileName) {
    this.ReadImg = new ReadImg(fileName)
  }
  display() {
    this.ReadImg.display()
  }
}

let proxyImg = new ProxyImg('1.png')
proxyImg.display() // loading ...1.png  display ...1.png
```

代码中`ProxyImg`函数使用自己的特权方法`display`使用了目标对象的方法。

**代理模式的优点**

* 代理对象可以代替本体被实例化，并使其可以被远程访问
* 它还可以把本体实例化推迟到真正需要的时候，对于实例化比较费时的本体对象，或者因为尺寸比较大以至于不用时不适于保存在内存中的本体，我们可以推迟实例化该对象。

**使用代理模式实现预加载**

```
class MyImg {
  constructor() {
    this.imgNode = document.createElement('img')
  }
  append() {
    document.body.appendChild(this.imgNode)
  }
  setSrc(src) {
    this.imgNode.src = src
  }
}

class ProxyImg {
  constructor(src) {
    this.src = src
    this.img = new Image()
    this.imgNode = new MyImg()
    this.img.onload = this.onload()
  }
  onload() {
    this.imgNode.setSrc(this.src)
  }
  setSrc(src) {
    this.imgNode.setSrc('默认加载的图片')
    this.img.src = src
  }
}
```

> 应用场景：网页事件代理、jQuery 的$.proxy、ES6 的 Proxy

| 代理模式        | 适配器模式     |
| ----------- | --------- |
| 提供一个一摸一样的接口 | 提供一个不同的接口 |

| 代理模式                | 装饰器模式             |
| ------------------- | ----------------- |
| 显示原有功能但是经过限制或者阉割之后的 | 扩展功能，原有功能不变且可直接使用 |

#### 外观模式

* 为了子系统中的一组接口提供了一个高层接口
* 使用者使用这个高层接口
* 为一组复杂的子系统接口提供一个更高级的统一接口，通过这个接口使得对子系统接口的访问更容易，不符合单一职责原则和开放封闭原则

```
 class A {
    eat () {}
}
class  B {
    eat () {}
}
class C {
    eat () {
        const a = new A();
        const b = new B();
        a.eat();
        b.eat();
    }
}
// 跨浏览器事件侦听器
function addEvent(el, type, fn) {
    if (window.addEventListener) {
        el.addEventListener(type, fn, false);
    } else if (window.attachEvent) {
        el.attachEvent('on' + type, fn);
    } else {
        el['on' + type] = fn;
    }
}
```

> 应用场景：JS事件不同浏览器兼容处理，同一方法可以传入不同参数兼容处理

#### 观察者模式

* 观察者也可称为发布&订阅模式，它定义了对象间的一种一对多的关系，让多个观察者对象同时监听某一个主体对象，当一个对象发生改变时，所有依赖于它的对象都将得到通知。
* 观察者模式和发布&订阅者模式区别：本质上的区别是调度的地方不同
  * 观察者模式：目标和观察者是基类，目标提供维护观察者的一系列方法，观察者提供更新接口。具体观察者和具体目标继承各自的基类，然后具体观察自己注册到具体目标里。在具体目标发生变化的时候，调度观察者的更新方法。
    * 比如有个“天气中心”的具体目标A，专门监听天气变化，而有个显示天气的界面观察者B，B就把自己注册到A里，当A触发天气变化，就调度B的更新方法，并带上自己的上下文。
  * 发布&订阅模式：订阅者把自己想订阅的事件注册到调度中心，当该事件触发时候，发布者发布该事件到调度中心(顺带上下文)，由调度中心统一调度订阅者注册到调度中心处理代码。
    * 比如有个界面实时显示天气，它就订阅天气事件(注册到调度中心，包括处理程序)，当天气变化时(定时获取数据)，就作为发布者发布天气信息到调度中心，调度中心就调度订阅者的天气处理程序。

![u3FTy9.png](https://s2.ax1x.com/2019/09/29/u3FTy9.png)

```
// 主体，保存状态，状态变化后触发所有观察者对象
class Subject {
  constructor() {
    this.state = 0
    this.observer = []
  }
  getState() {
    return this.state
  }
  setState(state) {
    this.state = state
  }
  notifyAllObservers() {
    this.observers.forEach(observer => {
      observer.update()
    })
  }
  attach(observer) {
    this.observers.push(observer)
  }
}

class Observer {
  constructor(name, subject) {
    this.name = name
    this.subject = subject
    this.subject.attach(this)
  }
  update() {
    console.log(`${this.name} update, state:${this.subject.getState()}`)
  }
}

let s = new Subject()
let o1 = new Observer('o1',s)
let o2 = new Observer('o2',s)
let o3 = new Observer('o3',s)

s.setState(1)
// 依次输出
// o1 update, state: 1
// o2 update, state: 1
// o3 update, state: 1
s.setState(2)
// 依次输出
// o1 update, state: 2
// o2 update, state: 2
// o3 update, state: 2
s.setState(3)
// 依次输出
// o1 update, state: 3
// o2 update, state: 3
// o3 update, state: 3
```

> 应用场景：JS事件、JS Promise、JQuery.$CallBack、Vue watch、NodeJS自定义事件，文件流等

#### 迭代器模式

* 顺序访问一个集合
* 使用者无需知道集合的内部结构(封装)
* 提供一种方法顺序访问一个聚合对象中各个元素, 而又无须暴露该对象的内部表示

[![u3Ac5V.png](https://s2.ax1x.com/2019/09/29/u3Ac5V.png)](https://imgchr.com/i/u3Ac5V)

```
class Iterator {
  constructor(container) {
    this.list = container.list
    this.index = 0
  }
  next() {
    if(this.hasNext()) {
      return this.list[this.index++]
    }
    return null
  }
  hasNext() {
    if(this.index >= this.list.length) {
      return false
    }
    return true
  }
}

class Container {
  constructor(list) {
    this.list = list
  }
  getIterator() {
    return new Iterator(this)
  }
}

const arr = [1,2,3,4,5]
let container = new Container(arr)
let iterator = container.getIterator()
while(iterator.hasNext()) {
  console.log(iterator.next())
}
// 1
// 2
// 3
// 4
// 5
```

#### 状态模式

* 一个对象有状态变化
* 每次状态变化都会触发一个逻辑
* 不能总是用`if...else`来控制

> 示例：收藏、取关、交通灯

![u3EaIx.png](https://s2.ax1x.com/2019/09/29/u3EaIx.png)

```
// 交通灯的三色变化
class State{
  constructor(color) {
    this.color = color
  }
  handle(context) {
    console.log(`turn to ${this.color} light`)
    context.setState(this)
  }
}

// 主体
class Context {
  constructor() {
    this.state = null
  }
  // 获取状态
  getState() {
    return this.state
  }
  // 设置状态
  setState(state) {
    this.state = state
  }
}

let context = new Context()

let green = new State('green')
let yellow = new State('yellow')
let red = new State('red')

// 绿灯亮了
green.handle(context)
// 打印状态
console.log(context.getState())
yellow.handle(context)
console.log(context.getState());
red.handle(context)
console.log(context.getState());
```

到此我们了解了在前端开发中常见的一些设计模式

虽然了解各种设计模式很重要，但同样重要的是不要过度使用他们，在使用设计模式之前，你应该仔细考虑你所处的问题是否符合该设计模式。要了解模式是否符合你的问题，你应该研究设计模式的思想，以及该设计模式的应用。
