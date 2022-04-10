---
cover: https://s2.ax1x.com/2019/12/02/Qn3uff.png
coverY: 0
---

# 左膀右臂一个不少——React初探

## React 初探

React 是一个用于构建用户界面的 JavaScript 库。

### React 家族介绍

* React.js
  * 用 React 的语法，编写一些网页的交互效果
* React.Native
  * 用 React 的语法，编写原生的 APP 应用
* React VR
  * 用 React 的语法，开发 VR 或全景应用

### React VS Vue

![Qn34je.png](https://s2.ax1x.com/2019/12/02/Qn34je.png)

| React                      | Vue                               |
| -------------------------- | --------------------------------- |
| 灵活性更大，处理一些非常复杂的业务时，会有更多的选择 | 提供了更丰富的 API，因为 API 多，它的灵活性有了一定的限制 |

### 从 Hello World 开始 React 之旅

```
ReactDOM.render(
  <h1>hello, world!</h1>
  document.getElementById('root')
)
```

它将在页面上展示一个 `Hello World` 的标题

### JSX

![Qn8PNq.png](https://s2.ax1x.com/2019/12/02/Qn8PNq.png)

```
const element = <h1>Hello,World!</h1>
```

这个语法他被称为**JSX，是一个 JavaScript 的语法扩展**。官方建议在 React 中配合使用 JSX，JSX 可以很好的描述 UI 应该呈现出它应有的交互的本质形式。JSX 可以生成 React 元素。

#### 为什么使用 JSX

React 人为渲染逻辑本质上与其他 UI 逻辑内在耦合，比如，在 UI 中需要绑定处理事件，在某些时刻状态发生变化时需要通知到 UI，以及需要在 UI 中展示准备好的数据。

React 不强制要求使用 JSX，但是大多数人发现，在 JavaScript 代码中将 JSX 和 UI 放在一起时，会在视觉上有辅助作用。他可以使 React 显示更多有用的错误和警告信息。

**JSX 中嵌入表达式**

```
const name = 'Lambda'
function hello(name) {
  return `hello, ${name}`
}

// 你可以在大括号中放置任何有效的JavaScript表达式
// 注意 这里是 {} 与 Vue 中的 {{}} 作区分
const element = <h1>{hello(name)}</h1>

ReactDOM.render(element, document.getElementById('root'))
```

在编译之后，JSX 表达式会被转换为普通的 JavaScript 函数调用，并且对其求值后得到 JavaScript 对象。

**JSX 特定属性**

```
// 通过引号，来将属性值指定为字符串字面量
const element = <div tabIndex="0"></div>
// 通过大括号，在属性值中插入一个JavaScript表达式
const element = <img src={user.avatarUrl}>
```

**因为 JSX 语法上更接近 JavaScript 而不是 HTML，所以 React DOM 使用 camelCase(小驼峰命名)来定义属性的名称，而不是使用 HTML 属性名称的命名约定，例如 JSX 中的`class`变成了`className`**

**JSX 防止注入攻击**

你可以安全的在 JSX 中插入用户输入内容

**React DOM 在渲染所有输入内容之前，默认会进行转义**，他可以确保在你的应用中，永远不会注入哪些并非自己明确编写的内容，**所有的内容在渲染之前都被转换成字符串**。这样可以有效的防止 XSS 攻击

### 元素渲染

> 元素是构成 React 应用的最小砖块

元素描述了你在屏幕上想看到的内容

与浏览器的 DOM 元素不同，React 元素是创建开销极小的普通对象。React DOM 会负责更新 DOM 来与 React 元素保持一致

#### 更新已渲染的元素

React 元素是不可变元素。一旦被创建，就无法更改他的子元素或者属性。一个元素就像电影的单帧：他代表某个特定时刻的 UI，根据我们已有的知识，更新 UI 唯一的方式就是创建一个全新的元素，并将其传入`ReactDOM.render()`

**React 只更新他需要更新的部分**

React DOM 会将元素和它的子元素与它们之前的状态进行比较，并只会进行必要的更新来使 DOM 达到预期的状态。

### 组件 & Props

![Qn8A3T.png](https://s2.ax1x.com/2019/12/02/Qn8A3T.png)

> 组件允许你将 UI 拆分为独立可复用的代码片段，并对每个片段进行独立构思。

组件，从概念上类似 JavaScript 函数。他接受任意的入参(即”props”)，并返回用于描述页面展示内容的 React 元素。

#### 函数组件与 class 组件

定义组件最简单的方式就是编写 JavaScript 函数

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>
}
```

函数接收唯一带有数据的”props”(代表属性)对象，这类组件被称为”函数组件”,因为它本质上就是 JavaScript 函数

使用 ES6 的 class 来定义组价

```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>
  }
}
```

#### 渲染组件

React 元素也可以是用户自定义的组件

```
// 组件名称必须以大写字母开头
const element = <Welcome name="Lambda" />
```

当 React 元素为用户自定义组件时，他会将 JSX 所接收的属性转换为单个对象传递给组件，这个对象被称之为”props”

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>
}

const element = <Welcome name="Lambda" />
ReactDOM.render(element, document.getElementById('root'))
```

**渲染流程**

1. 我们调用`ReactDOM.render()` 函数，并传入`<Welcome name="Lambda" />` 作为参数
2. React 调用 `Welcome` 组件,并将`{name: 'Lambda'}` 作为 props 传入
3. `Welcome` 组件将`<h1>Hello, Lambda</h1>` 元素作为返回值
4. React DOM 将 DOM 高效的更新为`<h1>Hello, Lambda</h1>`

#### 组合组件

组件可以在其输出中引用其他组件。这就可以让我们用同一组件来抽象出任意层次的细节。

#### 提取组件

将组件拆分成更小的组件，由于组件的种种关系，难以维护，且很难复它的各个部分。因此，我们可以提起一些组件出来。

建议从组件自身的角度命名 props，而不是依赖于调用组件的上下文命名。

**最初看上去，提取组件可能是一件繁重的工作，但是，在大型应用中，构建可复用组件库是完全值得的。**

#### Props 的只读性

组件无论是使用函数声明还是通过 class 声明，都决**不能修改自身的 props**

### State & 生命周期

![QGan5q.png](https://s2.ax1x.com/2019/12/05/QGan5q.png)

在之前的了解中，我们了解可以通过调用 `ReactDOM.render()` 来修改我们想要渲染的元素。现在我们学习如何封装真正可复用的 `Clock` 组件。

```
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
    </div>
  )
}

function tick() {
  ReactDOM.render(
    <Clock />
    document.getElementById('root')
  )
}

setInterval(tick, 1000)
```

理想情况下，我们希望只编写一次代码，便可以让`Clock`组件自我更新，我们可以在`Clock`组件中添加`state`来实现这个功能。

> State 与 props 类似，但是 state 是私有的，并且完全受控于当前组件。

#### 将函数组件转换成 class 组件

通过下边的步骤将`Clock`的函数组件转换成 class 组件:

1. 创建同名的`ES6 class`，并且继承于`React.Component`
2. 添加一个空的`render()`方法
3. 将函数体移动到`render()`方法中
4. 在`render()`方法中使用`this.props`替换`props`
5. 删除剩余的空函数声明

```
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello,world!</h1>
        <h1>Hello,{this.props.name}</h1>
      </div>
    )
  }
}
```

现在的`Clock`组件被定义为 class，而不是函数

**每次组件更新时`render`方法都会被调用，但只要在相同的 DOM 节点中渲染`<Clock />`,就仅有一个`Clock`组件的 class 实例被创建使用。**

#### 向 class 组件中添加局部的 state

我们通过下边步骤将 date 从 props 移动到 state

1. 将`render()`方法中的`this.props.date`替换成`this.state.date`
2. 添加一个 class 构造函数，然后在该函数中为`this.state`赋初值
3. 移除`<Clock/>`元素中的`date`属性

```
class Clock extends React.Component {
  constructor(props) {
    // 添加构造函数
    super(props)
    // 定义组件状态
    this.state = {
      // 定义初始数据
      date: new Date()
    }
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        // 替换 this.props
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    )
  }
}

ReactDOM.render(<Clock />, document.getElementById('root'))
```

#### 将生命周期方法添加到 Class 中

> 在具有许多组件的应用程序中，当组件被销毁时释放所占用的资源是非常重要的。

当`Clock`组件在第一次被渲染到 DOM 中的时候，就为其设置一个计时器。**这在 React 中被称为”挂载(mount)”**

当 DOM 中的`Clock`组件被删除的时候，应该清除定时器。**这在 React 中被称为”卸载(unmount)”**

我们为 class 组件声明一些特殊的方法，当组件挂载或卸载时就会执行这些方法。

```
class Clock extends React.Component {
  constructor(props) {
    super(props)
    this.state = { date: new Date() }
  }
  // 在组件已经被渲染到DOM中后运行
  // 最好在这里设置计时器
  componentDidMount() {
    // 把计时器的ID保存在this之中
    this.timerID = setInterval(() => this.tick(), 1000)
  }
  // 在这个生命周期中清除计时器
  componentWillUnmount() {
    clearInterval(this.timerID)
  }
  // 实现一个tick方法
  tick() {
    // 通过 setState 改变数据
    this.setState({
      /**
       * React 中有 immutable 概念
       * State 不允许我们做任何修改
       * 当我们操作 state 中的值时，需要先进行深拷贝，修改拷贝的值后将拷贝的值赋值给 state，修改state
       */
      date: new Date()
    })
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h1>Hello,{this.state.date.toLocaleTimeString()}.</h1>
      </div>
    )
  }
}

// React 调用 Clock 组件的构造函数，this.state中有初始化的值
ReactDOM.render(<Clock />, document.getElementById('root'))
```

#### 正确使用 State

关于`setSate()`应该注意的三件事

* 不要直接修改 Sate
  * `this.state.comment = 'Hello'` **错误**
* State 的更新可能是异步的
  * 出于性能考虑，React 可能会把多个`setState()`调用合并成一个调用
  * **因为`this.props`和`this.state`可能会异步更新，所以不要依赖他们的值来更新下一个状态**

```
// 错误
this.setState({
  counter: this.state.counter + this.props.increment
})

// 要解决这个问题，可以让 setSate() 接受一个函数而不是一个对象
// 上一个 state 作为第一个参数，此次更新被应用时的 props 作为第二个参数
this.setState((state, props) => ({
  counter: state.counter + props.increment
}))
```

* State 的更新会被合并
  * 当调用`setState()`的时候，React 会把提供的对象合并到当前 state
* 数据是向下流动的
  * 任何的 state 总是所属于特定的组件，而且从该 state 派生的任何数据或 UI 只能影响树中“低于”它们的组件。
  * 如果你把一个以组件构成的树想象成一个 props 的数据瀑布的话，那么每一个组件的 state 就像是在任意一点上给瀑布增加额外的水源，但是它只能向下流动。

### 事件处理

* React 事件的命名采用小驼峰式命名，而不是纯小写
* 使用 JSX 语法时，你需要传入一个函数作为事件处理函数，而不是一个字符串
* 不能通过返回`false`的方式阻止默认行为。必须显式的使用`preventDefault`

```
function ActionLink() {
  function handleClick(e) {
    // 这里的 e 是一个合成事件，不需要关心跨浏览的兼容问题
    e.preventDefault()
    console.log('The link was clicked.')
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  )
}
```

当使用 ES6 class 语法定义一个组件的时候，通常的做法是将事件处理函数声明为 class 中的方法。

```
class Toggle extends React.Component {
  constructor(props) {
    super(props)
    this.state = { isToggleOn: true }

    // 为了在回调中使用 this，这个绑定是必不可少的
    this.handleClick = this.handleClick.bind(this)
  }

  handleClick() {
    this.setState((state) => ({
      isToggleOn: !state.isToggleOn
    }))
  }

  handleClick1() {
    console.log(`this is: ${this}`)
  }

  render() {
    return (
      // 如果你忘记绑定 this.handleClick 并把它传入了 onClick，当你调用这个函数的时候 this 的值为 undefined。
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
      // 使用箭头函数 确保 handleClick 内的 this 已被绑定
      <button onClick={(e) => this.handleClick1(e)}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    )
  }
}

ReactDOM.render(<Toggle />, document.getElementById('root'))
```

#### 向事件处理程序传递参数

在循环中，通常我们会为事件处理函数传递额外的参数。例如，若 id 是你要删除那一行的 ID，以下两种方式都可以向事件处理函数传递参数：

```
// React 的事件对象 e 会被作为第二个参数传递
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

### 条件渲染

> 在 React 中，你可以创建不同的组件来封装各种需要的行为。然后依据应用的不同状态，只渲染对应状态的内容。

React 中的条件渲染和 JavaScript 中的一样，使用 JavaScript 运算符`if`或`条件运算符(三目运算符)`去创建元素来表现当前的状态，然后让 React 根据它们来更新 UI

```
function UserGreeting(props) {
  return (<h1> Welcome to my blog</h1>)
}

function GuestGreeting(props) {
  return (<h1>Please sign up</h1>)
}

function Greeting(props) {
  const isLoggedIn = props.isLoggedIn
  if(isLoggedIn) {
    return <UserGreeting />
  }
  return <GuestGreeting />
}

ReactDOM.render(
  // 根据 isLoggedIn 的值来渲染不同的语句
  <Greeting isLoggedIn={false} />
  document.getElementById('root')
)
```

#### 元素变量

可以使用变量存储元素。可以帮助有条件的渲染组件的一部分，而其他渲染部分不会因此而改变

声明一个变量并使用`If`语句进行条件渲染是不错的方式

#### 与运算符 `&&`

通过花括号包裹代码，可以在 JSX 中嵌入任何表达式

在 JavaScript 中，`true && expression` 总会返回`expression`,而`false && expression` 总会返回`false`

因此，如果条件是 `true`，`&&` 右侧的元素就会被渲染，如果是`false`，React 会忽略跳过

#### 三目运算符

```
condition ? true : false
```

> **需要注意的是，如果条件变得过于复杂，那应该考虑如何提取组件**

#### 阻止组件渲染

在极少数情况下，如果希望隐藏组件，即使它已经被其他组件渲染。若要完成此操作，你可以让`render`方法直接返回`null`，而不进行任何渲染。

```
function WarningBanner(props) {
  if (!props.warn) {
    // 根据 warn 的值进行条件渲染
    return null
  }

  return <h1> Welcome my blog</h1>
}
```

在组件的 render 方法中返回 null 并不会影响组件的生命周期。

### 列表 & Key

#### 渲染多个组件

我们在 React 中可以通过 JavaScript 的 Map 函数渲染多个组件内容

```
function NumberList(props) {
  const numbers = props.numbers
  const listItems = numbers.map((number) => {
    ;<li>{number}</li>
  })
  return (
    // 我们把整个listItems插入到ul元素中
    <ul>{listItems}</ul>
  )
}

const numbers = [1, 2, 3, 4, 5, 6]
ReactDOM.render(
  <NumberList numbers={numbers}/>
  document.getElementById('root)
)
```

当我们在运行这段代码，将会看到一个警告，`a key should be provided for list items`,意思是当你创建一个元素时，必须包括一个特殊的 key 属性。

```
<li key={number.toString()}>{number}</li>
```

#### key

key 帮助 React 识别哪些元素改变了，比如被添加或删除。因此应当给数组中的每一个元素赋予一个确定的标识

> key 值用来在更改数据时，虚拟 DOM 可以更好的找到之前的数据作对比

为什么不推荐使用 index 作为 key 值?

> 当数组中的某一项删除后，会影响其他元素的 index(key)值，这样对后来的比对工作带来很大的压力，可以使用 item 作为 key 值【建议使用比较稳定的值作为 key 值】

**什么是虚拟 DOM**

* 一个 JS 对象，用它来描述真实 DOM
* 减少对真实 DOM 的创建，以及真实 DOM 的对比，取而代之，创建的是一个 JS 对象，对比的也是 JS 对象，性能会后很大的提升
* DOM 替换更新时新旧对比，只替换更改过的区域
* **性能提升**、**使得跨端应用得以实现**

#### 用 key 提取组件

> 元素的 key 只有放在就近的数组上下文中才有意义。

```
function ListItem(props) {
  // 这里不需要指定 key
  return <li>{props.value}</li>
}

function NumberList(props) {
  const numbers = props.numbers
  const listItems = numbers.map((number) => (
    <ListItem key={numbers.toString()} value={number} />
  ))

  return <ul>{listItems}</ul>
}

const numbers = [1, 2, 3, 4, 5]
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
)
```

**在 map()方法中的元素需要设置 key 属性**

#### key 只是在兄弟节点之间必须唯一

数组元素中使用的 key 在其兄弟节点之间应该是独一无二的。然而，他们不需要是全局唯一的。当我们生成两个不同的数组时，我们可以使用相同的 key 值

当在组件中需要时使用 key 值时，使用其他属性名显式传递这个值

> 如果一个`map()`嵌套了太多层级，可能需要提取组件

### 表单

在 React 中，HTML 表单元素的工作方式和其他的 DOM 元素有些不同，这是因为表单元素通常会保持一些内部的 state。

#### 受控组件

在 React 中，可变状态(mutable state)通常保存在组件的 state 属性中，并且只能通过使用`setState()`来更新

渲染表单的 React 组件还控制着用户输入过程中表单发生的操作，被 React 以这种方式控制取值的表单输入元素就叫做”受控组价”

```
class NameForm extends React.Component {
  constructor(props) {
    super(props)
    this.state = { value: '' }

    this.handleChange = this.handleChange.bind(this)
    this.handleSubmit = this.handleSubmit.bind(this)
  }

  handleChange(event) {
    this.setState({ value: event.target.value })
  }

  handleSubmit(event) {
    alert('提交的名字: ' + this.state.value)
    event.preventDefault()
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          名字:
          <input
            type="text"
            // 表单上设置value属性，使得React的state成为唯一的数据源
            value={this.state.value}
            // 每次变化会触发函数，因此显示的值将随着用户输入而更新
            onChange={this.handleChange}
          />
        </label>
        <input type="submit" value="提交" />
      </form>
    )
  }
}
```

#### textarea 标签

在 React 中，`<textarea>` 使用 value 属性代替。这样，可以使得使用 `<textarea>` 的表单和使用单行 input 的表单非常类似

#### select 标签

React 并不会使用 selected 属性，而是在根 select 标签上使用 value 属性。

```
class FlavorForm extends React.Component {
  constructor(props) {
    super(props)
    this.state = { value: 'coconut' }

    this.handleChange = this.handleChange.bind(this)
    this.handleSubmit = this.handleSubmit.bind(this)
  }

  handleChange(event) {
    this.setState({ value: event.target.value })
  }

  handleSubmit(event) {
    alert('你喜欢的风味是: ' + this.state.value)
    event.preventDefault()
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          选择你喜欢的风味: // 这里设置选中的值
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">葡萄柚</option>
            <option value="lime">酸橙</option>
            <option value="coconut">椰子</option>
            <option value="mango">芒果</option>
          </select>
        </label>
        <input type="submit" value="提交" />
      </form>
    )
  }
}
```

#### 受控输入空值

在受控组件上指定 value 的 prop 可以防止用户更改输入。如果指定了 value，但输入仍可编辑，则可能是意外地将 value 设置为 undefined 或 null。

```
ReactDOM.render(<input value="hi" />, mountNode)

setTimeout(function() {
  // 输入最初被锁定，但在短时间延迟后变为可编辑。
  ReactDOM.render(<input value={null} />, mountNode)
}, 1000)
```

### 状态提升

![QRjVG8.png](https://s2.ax1x.com/2019/12/14/QRjVG8.png)

通常，多个组件需要反映相同的变化数据，这时建议将共享状态提升到最近的共同父组件去。

在 React 中，将多个组件中需要共享的 state 向上移动到它们的最近共同父组件中，便可实现共享 state。**这就是所谓的状态提升**

```
// 将state替换为props
render() {
    // Before: const temperature = this.state.temperature;
    const temperature = this.props.temperature;
}

// props是只读的，将自定义的temperature组件接受onTemperature这两个来自父组件
handleChange(e) {
    // Before: this.setState({temperature: e.target.value});
    this.props.onTemperatureChange(e.target.value);
    // 通过修改父组件自身的内部 state 来处理数据的变化，进而使用新的数值重新渲染
}
```

#### 梳理流程

当你对输入框内容进行编辑时会发生些什么：

```
class Calculator extends React.Component {
  constructor(props) {
    super(props)
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this)
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this)
    this.state = { temperature: '', scale: 'c' }
  }

  handleCelsiusChange(temperature) {
    this.setState({ scale: 'c', temperature })
  }

  handleFahrenheitChange(temperature) {
    this.setState({ scale: 'f', temperature })
  }

  render() {
    const scale = this.state.scale
    const temperature = this.state.temperature
    const celsius =
      scale === 'f' ? tryConvert(temperature, toCelsius) : temperature
    const fahrenheit =
      scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature

    return (
      <div>
        <TemperatureInput
          scale="c"
          temperature={celsius}
          onTemperatureChange={this.handleCelsiusChange}
        />

        <TemperatureInput
          scale="f"
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheitChange}
        />

        <BoilingVerdict celsius={parseFloat(celsius)} />
      </div>
    )
  }
}
```

* React 会调用 DOM 中 `<input>` 的 `onChange` 方法。在本实例中，它是 `TemperatureInput` 组件的 `handleChange` 方法。
* `TemperatureInput` 组件中的 `handleChange` 方法会调用 `this.props.onTemperatureChange()`，并传入新输入的值作为参数。其 props 诸如 `onTemperatureChange` 之类，均由父组件 `Calculator` 提供。
* 起初渲染时，用于摄氏度输入的子组件 `TemperatureInput` 中 `onTemperatureChange` 方法为 `Calculator` 组件中的 `handleCelsiusChange` 方法，而，用于华氏度输入的子组件 `TemperatureInput` 中的 `onTemperatureChange` 方法为 `Calculator` 组件中的 `handleFahrenheitChange` 方法。因此，无论哪个输入框被编辑都会调用 `Calculator` 组件中对应的方法。
* 在这些方法内部，`Calculator` 组件通过使用新的输入值与当前输入框对应的温度计量单位来调用 `this.setState()` 进而请求 React 重新渲染自己本身。
* React 调用 `Calculator` 组件的 render 方法得到组件的 UI 呈现。温度转换在这时进行，两个输入框中的数值通过当前输入温度和其计量单位来重新计算获得。
* React 使用 `Calculator` 组件提供的新 props 分别调用两个 `TemperatureInput` 子组件的 `render` 方法来获取子组件的 UI 呈现。
* React 调用 `BoilingVerdict` 组件的 render 方法，并将摄氏温度值以组件 props 方式传入。
* React DOM 根据输入值匹配水是否沸腾，并将结果更新至 DOM。我们刚刚编辑的输入框接收其当前值，另一个输入框内容更新为转换后的温度值。

#### 学习小结

在 React 应用中，任何可变数据应当只有一个相对应的唯一“数据源”。通常，state 都是首先添加到需要渲染数据的组件中去。然后，如果其他组件也需要这个 state，可以将它提升至这些组件的最近共同父组件中。\*\*应当依靠自上而下的数据流，\*\*而不是尝试在不同组件同步 state

* 好处
  * 排查和隔离 bug 所需的工作量将会变少
  * 由于存在于组件中的任何 state，仅有组件自己能够修改它，因此 bug 的排查范围被大大缩减
  * 自定义逻辑来拒绝或转换用户的输入

### 组合 Vs 继承

![QRvnfK.png](https://s2.ax1x.com/2019/12/14/QRvnfK.png)

React 有十分强大的组合模式。推荐使用组合而非继承来实现组件间的代码重用

#### 包含关系

有些组件无法提前知晓他们子组件的具体内容。在 Sidebar(侧边栏)和 Dialog(对话框)等展现通用容器的组件特别容易遇到这种情况

建议这些组件使用一个特殊的`children` prop 来将他们的子组件传递到渲染结果中

```
function FancyBorder(props) {
  return (
    <div className={`FancyBorder FancyBorder-${props.color}`}>
      props.children
    </div>
  )
}
```

使得别的组件可以通过 JSX 嵌套，将任意组件作为子组件传递给它们

```
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">Welcome</h1>
      <p className="Dialog-message">Thank you for visiting our spacecraft!</p>
    </FancyBorder>
  )
}
```

少数情况下，可能需要在一个组建中预留出几个“洞”。这种情况下，我们可以不使用`children`，而是自行约定

```
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">{props.left}</div>
      <div className="SplitPane-right">{props.right}</div>
    </div>
  )
}

function App() {
  return <SplitPane left={<Contacts />} right={<Chat />} />
}
```

这种方法可能会使你想起 Vue 中的槽`slot`概念，但在 React 中，没有槽这个概念的限制，你可以将任何东西作为 props 进行传递

使用 class 实现

```
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">{props.title}</h1>
      <p className="Dialog-message">{props.message}</p>
      {props.children}
    </FancyBorder>
  )
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props)
    this.handleChange = this.handleChange.bind(this)
    this.handleSignUp = this.handleSignUp.bind(this)
    this.state = { login: '' }
  }

  render() {
    return (
      <Dialog
        title="Mars Exploration Program"
        message="How should we refer to you?"
      >
        <input value={this.state.login} onChange={this.handleChange} />

        <button onClick={this.handleSignUp}>Sign Me Up!</button>
      </Dialog>
    )
  }

  handleChange(e) {
    this.setState({ login: e.target.value })
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`)
  }
}
```

#### 继承

Props 和 组合已经提供了清晰而安全的定制组件外观和行为的灵活方式。**注意：组件可以接受任意 props，包括基本数据类型，React 元素以及函数**

如果想在组件间复用非 UI 的功能，我们建议将其提取为一个单独的 JavaScript 模块，如函数，对象或者类。组件中可以直接引入(import)而无需通过 extend 继承它们
