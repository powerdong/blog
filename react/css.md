---
cover: https://s1.ax1x.com/2020/05/02/Jv0xMQ.png
coverY: 0
---

# 管好你的状态——React-Redux

## React-Redux

Redux 是 JavaScript 状态容器，提供可预测化的状态管理。

可以让你构建一致化的应用，运行于不同的环境(客户端、服务器、原生应用)，并且易于测试。

### 开始之前

Redux 是负责组织 state 的工具，但你也要考虑它是否适合你的情况。不要因为有人告诉你要用 Redux 就去用

在下面的场景中，引入 Redux 是比较明智的

* 你有着大量的、随时间变化的数据
* 你的 state 需要有一个单一可靠数据来源
* 你觉得把所有 state 放在最顶层组件中已经无法满足需要了

> Redux 的目标是创建一个**状态管理库**，来提高工最简化 API，但同时做到行为的完全可预测，因此才得以实现日志打印，热加载，时间旅行，同构应用，录制和重放，而不需要任何开发参与。

#### 要点

应用中所有的 state 都以一个对象树的形式存储在一个单一的 _store_ 中，唯一改变 _state_ 的办法是触发 _action_ ，一个描述发生什么的对象。为了描述 _action_ 如何改变 _state_ 树，需要编写 _reducers_

```jsx
import { createStore } from 'redux';
// 这是一个 reducer 形式为 (state, action) => state 的纯函数
// 描述了 action 如何把 state 转变成下一个 state
function countter(state = 0, action) {
  switch (action.type) {
    case: 'CASE':
      return state + 1
    default:
      return state
  }
}
// 创建 Redux store 来存放应用的状态
// API 是 { subscribe, dispatch, getState }
let store = createStore(counter)
// 可以手动订阅更新
store.subscribe(() => console.log(store.getState()))
// 改变内部 state 唯一的方法是 dispatch 一个 action
store.dispatch({ type: 'CASE' })
```

应该把要做的修改变成一个普通对象，这个对象被叫做 _action_ 而不是直接修改 state。

然而编写专门的函数来决定每个 action 如何改变应用的 state 这个函数被叫做 reducer

### 介绍

![JvBFiV.png](https://s1.ax1x.com/2020/05/02/JvBFiV.png)

#### 动机

随着 JavaScript 单页应用开发日趋复杂，JavaScript 需要管理比任何事都都要多的 state 。

管理不断变化的 state 非常困难。多个 model 的变化会让你搞不清到底发生了什么，所以 state 在什么时候，由于什么原因，如何变化已然不受控制。当系统变得错综复杂的时候，想重现问题或者添加新功能就会变得举步维艰。

一些库如 React 试图在 视图层禁止异步和直接操作 DOM 来解决这个问题。美中不足的是，React 依旧把处理 state 中数据的问题留给了你。Redux 就是为了帮助你解决这个问题。

**Redux 试图让 state 的变化变得可预测。**

> React 负责视图层 Redux 负责管理视图层用到的一些 state

#### 核心概念

> Redux 本身很简单

强制使用 action 来描述所有变化带来的好处是可以清晰地知道应用中到底发生了什么。action 就像是描述发生了什么的指示器。最终，**为了把 action 和 state 串起来，开发一些函数，这就是 Reducer。**

```
function visibilityFilter(state = 'SHOW_ALL', action) {
  if (action.type === 'SET_VISIBILITY_FILTER') {
    return action.filter
  } else {
    return state
  }
}
function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([{ text: action.text, completed: false }])
    case 'TOGGLE_TODO':
      return state.map((todo, index) =>
        action.index === index
          ? { text: todo.text, completed: !todo.completed }
          : todo
      )
    default:
      return state
  }
}
```

再开发一个 reducer 调用这两个 reducer，进而来管理整个应用的 state：

```
function todoApp(state = {}, action) {
  return {
    todos: todos(state.todos, action),
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
  }
}
```

> 这差不多就是 Redux 思想的全部。 主要的想法是如何根据这些 action 对象来更新 state

#### 三大原则

* 单一数据源
* State 是只读的
* 使用纯函数来修改

**单一数据源**

> 整个应用的 state 被存储在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中。

来自服务端的 state 可以在无需编写更多代码的情况下被序列化并注入到客户端中。由于是单一的 state tree，调试也变得非常容易。

**State 是只读的**

> 唯一改变 state 的方法就是触发 action，action 是一个用于描述已经发生事件的普通对象。

这样确保了视图和网络请求都不能直接修改 state，相反它们只能表达想要修改的意图。因为所有的修改都被集中化管理，且严格按照一个接一个的顺序执行，因此不用担心竞态条件的出现。

```
store.dispatch({
  type: 'COMPLETE_TODO',
  index: 1,
})
```

**使用纯函数来执行修改**

> 为了描述 `action` 如何改变 `state tree`，需要编写 `reducers`

reducer 只是一些纯函数，它接收先前的 state 和 action，并返回新的 state。

因为 reducer 只是函数，可以控制他们的调用顺序，传入附加数据，甚至编写可复用的 reducer 来处理一些通用任务。

```
function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      return action.filter
    default:
      return state
  }
}
function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        {
          text: action.text,
          completed: false,
        },
      ]
    case 'COMPLETE_TODO':
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: true,
          })
        }
        return todo
      })
    default:
      return state
  }
}
import { combineReducers, createStore } from 'redux'
// 组合多个 reducer
let reducer = combineReducers({ visibilityFilter, todos })
let store = createStore(reducer)
```

> Redux 规定，将模型的更新逻辑全部集中于一个特定的层（Redux 里的 reducer）

### 基础

![JvDn1S.png](https://s1.ax1x.com/2020/05/02/JvDn1S.png)

#### Action

Action 是把数据从应用传到 store 的有效载荷。它是 store 数据的唯一来源。一般可以通过 `store.dispatch()`将 action 传到 store

Action 本质上是 JavaScript 普通对象。我们约定，action 内必须使用一个字符串类型的 type 字段来表示将要执行的动作。

除了 type 字段外，action 对象的结构完全由你自己决定。

> 我们应该尽量减少在 action 中传递数据

#### Action 创建函数 (actionCreator)

Action 创建函数 就是生成 action 的方法。**“action”和“action 创建函数”** 这两个个概念使用时最好注意区分。

```
function addTodo(text) {
  return {
    type: ADD_TODO,
    text,
  }
}
```

Redux 中只需把 action 创建函数的结果传给 dispatch 方法即可发起一次 dispatch 过程。

```
dispatch(addTodo(text))
```

或者创建一个 **被绑定的 action 创建函数** 来自动 dispatch

```
const boundAddTodo = (text) => dispatch(addTodo(text))
boundAddTodo(text)
```

store 里能直接通过 store.dispatch() 调用 dispatch() 方法，但是多数情况下你会使用 react-redux 提供的 connect() 帮助器来调用。bindActionCreators() 可以自动把多个 action 创建函数 绑定到 dispatch() 方法上。

#### Reducer

Reducers 指定了应用状态的变化如何响应 actions 并发送到 store 的，记住 actions 知识描述了 有事情发生了这一事实，并没有描述应用如何更新 state

保持 reducer 纯洁非常重要。永远不要在 reducer 里做这些操作。

* 修改传入参数
* 执行有副作用的操作，如 API 请求和路由跳转
* 调用非纯函数，如 Data.now()

> 只要传入参数相同，返回计算得到的下一个 state 就一定相同。没有特殊情况、没有副作用，没有 API 请求、没有变量修改，单纯的执行计算。

```
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter,
      })
    default:
      return state
  }
}
```

注意：

1. 不要修改 state。 使用 Object.assign()新建了一个副本。
2. 在 default 情况下返回旧的 state。遇到未知的 action 时，一定要返回旧的 state。

> 时刻谨记永远不要在克隆 state 前修改它。

最后，Redux 提供了 combineReducers()来整合多个 reducer

```
import { combineReducers } from 'redux'
const todoApp = combineReducers({
  visibilityFilter,
  todos,
})
export default todoApp
```

combineReducers()所做的只是生成一个函数，这个函数来调用一系列 reducer，每个 reducer 根据它们的 key 来筛选出 state 中的一部分数据并处理，然而这个生成的函数再将所有 reducer 的结果合并成一个大的对象。

```
// 可以把所有顶级的 reducer 放到一个独立的文件中，通过 export 暴露出每个 reducer 函数
import { combineReducers } from 'redux'
import * as reducers from './reducers'
const todoApp = combineReducers(reducers)
```

#### Store

前面我们描述了 使用 action 来描述 “发生了什么”，和使用 reducers 来根据 action 更新 state 的用法。

Store 就是把他们联系到一起的对象。

* 维持应用的 state
* 提供 getState()方法获取 state
* 提供 dispatch(action) 方法更新 state
* 通过 subscribe(listener) 注册监听器
* 通过 subscribe(listener) 返回函数注销监听器

> 再次强调一下 **Redux 应用只有一个单一的 store**。当需要拆分数据处理逻辑时，你应该使用 reducer 组合 而不是创建多个 store。

```
import { addTodo } from './actions'
// 打印初始状态
console.log(store.getState())
// 每次 state 更新时，打印日志
// 注意 subscribe() 返回一个函数用来注销监听器
const unsubscribe = store.subscribe(() => console.log(store.getState()))
// 发起一系列 action
store.dispatch(addTodo('Learn about actions'))
// 停止监听 state 更新
unsubscribe()
```

#### 数据流

> 严格的单向数据流是 Redux 架构的设计核心

Redux 生命周期

* 调用 store.dispatch(action)
  * Action 就是一个描述发生了什么的普通对象 {type:’ADD\_TODO’, id: 1}
* Redux store 调用传入的 reducer 函数
  * Store 会把两个参数传入 reducer，当前的 state 和 action
* Reducer 应该把多个子 reducer 输出合并成一个单一的 state 树
* Redux store 保存了根 reducer 返回的完整的 state 树
  * 这个新的树就是应用的下一个 state！所有订阅 store.subscribe(listener) 的监听器都将被调用；监听器里可以调用 store.getState() 获得当前 state。

#### 搭配 React

> Redux 和 React 之间没有关系。React 支持 React、Angular、Ember、jQuery 甚至纯 JavaScript

这里需要使用到 react-redux 库

**容器组件和展示组件**

|                | **展示组件**       | **容器组件**           |
| -------------- | -------------- | ------------------ |
| **作用**         | 描述如何展示(骨架、样式)  | 描述如何运行（数据获取、状态更新）  |
| **直接使用 Redux** | 否              | 是                  |
| **数据来源**       | props          | 监听 Redux state     |
| **数据修改**       | 从 props 调用回调函数 | 向 Redux 派发 actions |
| **调用方式**       | 手动             | 通常由 React Redux 生成 |

### 高级

#### 异步 Action

当调用异步 API 时，有两个非常关键的时刻：发起请求的时刻，和接收到响应时刻(也可能是超时)

这两个时刻都可能会更改应用的 state；为此，需要 dispatch 普通的同步 action。一般情况下，每个 API 请求都需要 dispatch 至少三种 action

* 一种通知 reducer 请求开始的 action
  * 以此来告诉 UI 显示加载界面
* 一种通知 reducer 请求成功的 action
  * Reducer 可能会把接收到的数据合并到 state 中，UI 隐藏加载界面，并显示接收到的数据
* 一种通知 reducer 请求失败的 action
  * Reducer 会保存这些失败的信息，并在 UI 中显示

```
// 在 action 中添加专门的 status 字段作为标记
{ type: 'FETCH_POSTS' }
{ type: 'FETCH_POSTS', status: 'error', error: 'Oops' }
{ type: 'FETCH_POSTS', status: 'success', response: { ... } }
// 或者使用不同的 type
{ type: 'FETCH_POSTS_REQUEST' }
{ type: 'FETCH_POSTS_FAILURE', error: 'Oops' }
{ type: 'FETCH_POSTS_SUCCESS', response: { ... } }
```

> 使用多个 type 会降低犯错误的机率

#### 同步 Action 创建函数 (actionCreator)

**action.js**

```
export const SELECT_SUBREDDIT = 'SELECT_SUBREDDIT'
export function selectSubreddit(subreddit) {
  return {
    type: SELECT_SUBREDDIT,
    subreddit,
  }
}
```

#### 处理 Action

**reducer.js**

> reducer 只是函数而已，所以你可以尽情使用函数组合和高阶函数这些特性。

```
import { combineReducers } from 'redux'
import {
  SELECT_SUBREDDIT,
  INVALIDATE_SUBREDDIT,
  REQUEST_POSTS,
  RECEIVE_POSTS,
} from '../actions'
function selectedsubreddit(state = 'reactjs', action) {
  switch (action.type) {
    case SELECT_SUBREDDIT:
      return action.subreddit
    default:
      return state
  }
}
const rootReducer = combineReducers({
  selectedsubreddit,
})
export default rootReducer
```

#### 异步 action 创建函数

通过使用指定的 `middleware`，`action` 创建函数除了返回 `action` 对象外，还可以返回函数。这时，这个 action 创建的函数就成为了 thunk

当 action 创建函数返回函数时，这个函数会被 Redux Thunk middleware 执行。这个函数并不需要保持纯净；他带有副作用，包括执行异步 API 请求。这个函数还可以 dispatch(action)

我们也可以在 actions.js 里定义这些特殊的 thunk action 创建函数

```
// 虽然内部操作不同，可以像其他 action 创建函数 一样使用它
// store.dispatch(fetchPosts('reactjs'))
export function fetchPosts(subreddit) {
  // 这里把 dispatch 方法通过参数的形式传给函数，
  // 以此来让它自己也能 dispatch action。
  // 也可以接收了 getState() 方法
  return function (dispatch, getState) {
    // 首次 dispatch：更新应用的 state 来通知
    // API 请求发起了。
    dispatch(requestPosts(subreddit))
    return fetch(`http://www.subreddit.com/r/${subreddit}.json`)
      .then(
        (response) => response.json(),
        // 不要使用 catch，因为会捕获在 dispatch 和渲染中出现的任何错误，
        (error) => console.log('An error occurred.', error)
      )
      .then((json) =>
        // 可以多次 dispatch！
        // 这里，使用 API 请求结果来更新应用的 state。
        dispatch(receivePosts(subreddit, json))
      )
  }
}
```

#### 异步数据流

默认情况下，createStore 所创建的 Redux store 没有使用 middleware，所以只支持 同步数据流。

像 redux-thunk 或 redux-promise 这样支持异步的 middleware 都包装了 store 的 dispatch() 方法，以此来让你 dispatch 一些除了 action 以外的其他内容，例如：函数或者 Promise。

#### Middleware

Middleware 是指可以被嵌入在框架接收请求到产生响应过程之中的代码。middleware 最优秀特性就是可以被链式组合。你可以在一个项目中使用多个独立的第三方 middleware。

Redux middleware 被用于解决不同的问题，但其中的概念是类似的。**它提供的是位于 action 被发起之后，到达 reducer 之前的扩展点。**

可以利用 redux middleware 来进行日志记录、创建奔溃报告等。

#### 理解 Middleware

使用 Redux 的一个益处就是它让 state 的变化过程变得可预知和透明。每当一个 action 发起完成后，新的 state 就会被计算并保存下来。State 不能被自身修改，只能由特定 action 引起变化。

**封装 Dispatch**

```
function dispatchAndLog(store, action) {
  console.log('dispatching', action)
  store.dispatch(action)
  console.log('next state', store.getState())
}
// 替换 store.dispatch()
dispatchAndLog(store, addTodo('Use Redux'))
```

#### 搭配 React Router

![JvBvY6.png](https://s1.ax1x.com/2020/05/02/JvBvY6.png)

Redux 和 React Router 将分别成为 数据 和 URL 的事实来源。

```
import { BrowserRouter as Router, Route } from 'react-router-dom'
// 用 path 指定 URL，用 component 指定路由命中 URL 后需要渲染的那个组件。
const Root = () => (
  <Router>
    <Route path="/" component={App} />
  </Router>
)
```

在我们的 Redux 应用中，仍将使用 将 Redux 绑定到 React

```
import { Provider } from 'react-redux'
// 用 <Provider /> 包裹 <Router />，以便于路由处理器可以访问 store
const Root = ({ store }) => (
  <Provider store={store}>
    <Router>
      {/* URL 匹配到 '/' 将会渲染 `<App />` 组件 */}
      <Route path="/" component={App} />
      <Route path="/:filter?" component={App} />
    </Router>
  </Provider>
)
```

在 React Router 中还有另一种的路由模式 browerHistory 实现从 URL 中移出 #

```
import { Router, Route, browserHistory } from 'react-router'
```

#### 搭配 TypeScript

![JvBuZR.png](https://s1.ax1x.com/2020/05/02/JvBuZR.png)

TypeScript 是 JavaScript 类型的超集。由于其带来的好处，它最近在应用中变得流行。

TypeScript 可以为 Redux 应用程序带来以下好处：

1. 为 reducer、state 和 action creator 带来类型安全
2. 轻松重构 type 代码
3. 在团队协作环境中获得愉悦的开发体验

**为 State 增加类型检查**

```
export interface Message {
  user: string
  message: string
  timestamp: number
}
export interface ChatState {
  message: Messagge[]
}
```

导出这些 interface，以便稍后在 reducer 和 action creators 中重用它们。

**为 Action & Action Creator 增加类型检查**

使用字符串文字并使用 typeof 来声明我们的 action 常量和推断类型。将我们的类型分成单独的文件，可以让我们的其他文件更专注于它们的目的。

```
export const DELETE_MESSAGE = 'DELETE_MESSAGE'
interface SendMessageAction {
  type: typeof SEND_MESSAGE
  payload: Message
}
export type ChatActionTypes = SendMessageAction
```

**为 Reducer 增加类型检查**

Reducer 只是纯函数，它输入先前的 state 和 action 然后返回下一个 state。在此示例中，我们显式声明此 reducer 将接收的 action 类型以及它应返回的内容。

```
import { ChatState, ChatActions, ChatActionTypes, SEND_MESSAGE } from './types'
const initialState: ChatState = {
  messages: [],
}
export function chatReducer(
  state = initialState,
  action: ChatActionTypes
): ChatState {
  switch (action.type) {
    case SEND_MESSAGE:
      return {
        messages: [...state.messages, action.payload],
      }
    default:
      return state
  }
}
// src/store/index.ts
import { systemReducer } from './system/reducers'
import { chatReducer } from './chat/reducers'
const rootReducer = combineReducers({
  system: systemReducer,
  chat: chatReducer,
})
export type AppState = ReturnType
```

搭配 React Redux

虽然 React Redux 是一个独立于 redux 本身的库，但它通常与 react 一起使用。出于这个原因。

```
// 对 mapStateProps 接收的参数添加类型检查。
import { AppState } from './store'
const mapStateToProps = (state: AppState) => ({
  system: state.system,
  chat: state.chat,
})
```

**搭配 Redux Thunk**

![JvB6yQ.png](https://s1.ax1x.com/2020/05/02/JvB6yQ.png)

Redux Thunk 是常用的异步请求中间件。thunk 是一个返回另一个参数为 dispatch 和 getState 的函数的函数。Redux Thunk 有一个内置类型 ThunkAction

```
// src/thunks.ts
import { Action } from 'redux'
import { sendMessage } from './store/chat/actions'
import { AppState } from './store'
import { ThunkAction } from 'redux-thunk'
export const thunkSendMessage = (
 message: string
): ThunkAction> => async dispatch => {
 const asyncResp = await exampleAPI()
 dispatch(
  sendMessage({
   message,
   user: asyncResp,
   timestamp: new Date().getTime()
  })
 )
}
function exampleAPI() {
 return Promise.resolve('Async Chat Bot')
}
```

建议在的 dispatch 中使用 action creator，因为我们可以重用对这些函数的类型检查。
