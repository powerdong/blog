---
cover: https://s1.ax1x.com/2020/04/25/JsS5Hf.png
coverY: 0
---

# 一个哪行——React详解

### 刚开始

定义一个组件

```jsx
import React, { Component, Fragment } form 'react'
```

创建一个组件

```jsx
class TodoList extends Component {
  render() {
    return (
      <Fragment>
        <div>
          <input
            value={this.state.inputValue}
            onChange={this.handleChange.bind(this)}
          />
          <button onClick={this.handleBtnCLick.bind(this)}>提交</button>
        </div>
        <ul>
          <li></li>
        </ul>
      </Fragment>
    )
  }
}

export default TodoList
```

使用 Fragment 替代最外层的 `<div>` 使得最外层标签隐藏

#### React 设计思想和事件绑定

![JsSoE8.png](https://s1.ax1x.com/2020/04/25/JsSoE8.png)

* state 里面存储组件中使用的数据
* JSX 中如果想使用 JS 表达式，需要使用{}包裹
* 事件绑定的时候需要使用 bind 对函数 this 指向进行变更
* 如果想改变数据项的内容，不能直接去改它，需要通过 setState 函数，通过传入一个对象的形式来对 state 进行更新

```jsx
constructor (props) {
    super(props);
    // 定义数据
    this.state = {
        inputValue: '',
        list: []
    }
}
handleChange (e) {
    this.setState({
        inputValue: e.target.value
    })
}
```

#### 列表渲染

```jsx
<ul>
  {this.state.list.map((item, index) => {
    return (
      <li key={index} onClick={this.handleDelItem.bind(this, index)}>
        {item}
      </li>
    )
  })}
</ul>
```

#### 操作数组

```jsx
// 添加
handleBtnCLick () {
    this.setState({
        // 更新数组
        list: [...this.state.list, this.state.inputValue],
        inputValue: ''
    })
}

// 删除
handleDelItem (index) {
    // immutable state 不允许我们做任何改变
    // 拷贝数组
    const list = [...this.state.list]
    list.splice(index, 1) // 删除数据
    this.setState ({
        list: list
    })
}
```

#### 冷门小知识

* JSX 中 使用样式的 `class` 时不要使用 `class` 应该使用 `className`
* 当你想在页面中展示未转义的 html 内容使用
  *   ```jsx
      dangerouslySetInnerHTML={{__html: item}}
      ```

      > 这种的未转义会造成 XSS 攻击 慎用！
* 在`<label>`标签上使用`for`时，应使用 `htmlFor`

#### 父子传值

**父传子**

```jsx
// Parent
// 父组件通过属性的方式传值
;<ul>
  {this.state.list.map((item) => (
    <TodoItem
      content={item}
      index={index}
      handleDelItem={this.handleDelItem.bind(this)}
    />
  ))}
</ul>

// Children
// 子组件通过 this.props. 的方式接受使用
class TodoItem extends Component {
  render() {
    return <div>{this.props.content}</div>
  }
}
```

**子传父**

```jsx
// Children
// 子组件通过 this.props. 的方式接受使用
class TodoItem extends Component {
  constructor(props) {
    super(props)
    this.handleClick = this.handleClick.bind(this)
  }
  render() {
    return <div onClick={this.handleClick}>{this.props.content}</div>
  }
  handleClick() {
    const index = this.props.index
    // 子组件调用父组件传过来的方法，通过调用该方法实现删除操作
    this.props.handleDelItem(index)
  }
}
```

#### 代码优化

1. 在 constructor 中进行 this 绑定
2. 通过解构赋值的方式提取 props state 的值
3. JSX 部分不要出现逻辑代码，如果出现建议拆分出来成为一个函数
4. setState 时使用函数返回的方式修改

```jsx
const value = e.target.value
this.setState((prevState) => ({
  inputValue: value,
}))
```

#### React 思考

![JsSTUS.png](https://s1.ax1x.com/2020/04/25/JsSTUS.png)

* 声明式开发
* 可以与其他框架一起开发
* 组件化
* 单向数据流
* 视图层框架
* 函数式编程

### 继续冲

PropTypes

![JsSLgs.png](https://s1.ax1x.com/2020/04/25/JsSLgs.png)

[查看文档](https://zh-hans.reactjs.org/docs/typechecking-with-proptypes.html)

**类型校验**

在子组件中通过 `Children.propTypes = {}`的形式来声明 props 传过来的类型

```jsx
TodoItem.propTypes = {
  content: PropTypes.string,
  handleDelItem: PropTypes.func,
  index: PropTypes.number,
  text: PropTypes.string.isRequired, // 必传
}
```

#### DefaultProps

**定义默认值**

在子组件中通过 `Children.defaultProps = {}`的形式来设置默认值，当父组件没有向子组件传值的情况下会使用默认值

```jsx
TodoItem.defaultProps = {
  text: 'defalut content',
}
```

Props，State 与 render 函数关系

> 当组件的 state 或者 props 发生改变的时候，render 函数就会重新执行

1. 在页面初始化的时候，render 函数会自动的执行一次
2. State 发生改变，render 再次执行
3. 当父组件的 render 运行时，它的子组件的 render 都将被重新运行一次
   * 因为 props 改变了
   * 因为父组件的 render 重新执行了

> 父组件 render -> 子组件 render

#### 什么是虚拟 DOM

![JsSvD0.png](https://s1.ax1x.com/2020/04/25/JsSvD0.png)

1. State 数据
2. JSX 模板
3. 数据 + 模板 结合，生成真实 DOM，来显示
4. State 发生改变
5. 数据 + 模板 结合，生成真实 DOM，替换原始的 DOM

* 缺陷：
  * 第一次生成了完成的 DOM 片段
  * 第二次生成了一个完成的 DOM 片段
  * 第二次生成的 DOM 他替换第一次的 DOM，非常耗性能

***

1. State 数据
2. JSX 模板
3. 数据 + 模板 结合，生成真实 DOM，来显示
4. State 发生改变
5. 数据 + 模板 结合，生成真实 DOM，并不直接替换原始的 DOM
6. 新的 DOM 和原始的 DOM 作比对，找差异
7. 找出 `input` 框发生的变化
8. 只用新的 DOM 中的 input 元素，替换掉老的 DOM 中的 `input` 元素

* 缺陷：
  * 性能提升并不明显

***

1. State 数据
2. JSX 模板
3. 生成虚拟 DOM (虚拟 DOM 就是一个 JS 对象，用它来描述真实 DOM) \[**损耗了性能**]
4. 用虚拟 DOM 的结构生成真实 DOM，来显示
5. State 发生变化
6. 数据 + 模板 生成新的虚拟 DOM \[**极大的提升了性能**]
7. 比较两次的虚拟 DOM，找到区别 \[**极大的提升了性能**]
8. 直接操作 DOM，改变内容

* 减少了对真实 DOM 的创建，和真实 DOM 的对比，创建的和对比的都是 JS 对象

#### 深入理解虚拟 DOM

> JSX –> createElement –> JS 对象 –> 真实 DOM

* 好处
  * 性能提升了
  * 它使得跨端应用得以实现。

#### DIFF 算法

![JsSOvn.png](https://s1.ax1x.com/2020/04/25/JsSOvn.png)

* **同级比较**
  * 会对 DOM 树进行同层比对，如果发现某一层改变，会将它子节点全部替换掉(不管子节点是否改变)，替换成新的 DOM 节点，虽然这样会造成性能上的损耗，子节点未改变也会被替换，但是这样的算法实现起来比较简单，从算法的角度看性能上还是比较优的。
* **Key 值比对**

#### React 的 ref 使用

可以通过 `e.target` 的方式获取到触发事件的那个 DOM 节点

可以通过`ref`的方式获取 DOM

```jsx
;<input
  ref={(input) => {
    this.input = input
  }}
/>
// this.input 指向的就是input这个DOM

// 之前使用 e.target 来获取
// 现在可以直接使用 this.input 获取DOM
console.log(this.input.value)
this.setState(
  (prev) => ({
    // 这里用来更新 state 数据
  }),
  () => {
    // 这里是 setState 执行完成后执行的回调，因为setState是异步执行的
  }
)
```

#### 生命周期函数

![JspVDx.png](https://s1.ax1x.com/2020/04/25/JspVDx.png)

> 生命周期函数指在某一时刻自动调用执行的函数
>
> 父组件 render –> 子组件的 render

* `Initialization` 初始化 props 和 state
* `Mounting`
  * `componentWillMount` 在组件即将被**挂载**到页面的时刻自动执行
  * `render`
  * `componentDidMount` 在组件被**挂载**到页面的时刻自动执行
* `Updation`
  * `Props`
    * `componentWillReceiveProps`
      * 当一个组件从父组件接受了参数，只要**父组件的 render 函数重新执行了**，子组件的这个函数就会被执行
      * 如果这个组件第一次存在于父组件中，这个函数不执行
      * 如果这个组件之前已经存在于父组件中，才会执行
    * `shouldComponentUpdate`
      * 组件被更新之前，他会自动执行，\*\*需要返回一个 Boolean，\*\*控制组件是否更新
    * `componentWillUpdate`
      * 组件被更新之前，它会自动执行，但是他在 shouldComponentUpdate 之后执行，如果 shouldComponentUpdate 返回 true 它会执行，否则它将不执行
    * `render`
    * `componentDidUpdate`
      * 组件更新完成之后，它会被执行
  * `States`
    * `shouldComponentUpdate`
      * 组件被更新之前，他会自动执行，\*\*需要返回一个 Boolean，\*\*控制组件是否更新
    * `componentWillUpdate`
    * `render`
    * `componentDidUpdate`
* `Unmounting`
  * `componentWillUnmount`
    * 当这个组件即将被从页面剔除的时候会被执行

#### 使用场景

通过 `shouldComponentUpdate` 控制子组件的 render

```jsx
shouldComponentUpdate (nextProps, nextState) {
    if(nextProps.content !== this.props.content) {
        // 发生了改变
        return true
    } else {
        return false
    }
}
```

#### React Css 动画

可以使用 CSS 自己编写动画，通过控制切换类名的方式实现动画

使用 [react-transition-group](https://reactcommunity.org/react-transition-group/)

### Redux

就是将组件中用到的数据放到一个公用的区域去存储。

> Redux = Reducer + Flux

#### Redux Flow

![JrxbdS.png](https://s1.ax1x.com/2020/04/25/JrxbdS.png)

#### 创建 store

```jsx
// index.js
import { createStore } from 'redux'
import reducer from './reducer'

const store createStore(reducer)

export default store

// reducer.js
constt defaultState = {
    inputValue: '',
    list: []
}

// reducer 可以接受 state 但是绝不能修改 state
export default (state = defaultState, action) => {
    // store 接收到 组件的 dispatch 后委托给 reducer
    if (action.type === 'change_input_value') {
        // 这里获取到相关的数据进行处理返回新的value，改变 store
        const newState = JSON.parse(JSON.stringify(state))
        newState.inputValue = action.value
        return newState;
    }
    return state
}
```

#### 组件中使用 store

```jsx
// 获取 store
import store form './store'
// 从 store 中获取数据
this.state = store.getState()
// 订阅 store 的改变，只要 store 发生改变，this.handleStoreChange 就会被执行
store.subscribe(this.handleStoreChange)
handleStoreChange () {
  // 数据与 store 联动的效果
  this.setState(store.getState())
}
```

#### 组件中提交更新 store 数据

```jsx
handleInputChange (e) {
    const action = {
        type: 'change_input_value',
        value: e.target.value
    }
    // 组件将Action 派发给store
    store.dispatch(action)
}
```

> 整体的流程：通过创建 store 和 reducer 管理数据，组件中获取 store 数据，如果想更新数据，向 store 派发出一个 action，action 通过 dispatch 传递给 store，store 传给 reducer，reducer 接收后做一些处理再将新的 State 返回，store 接收到以后进行更新，store 收到更新，组件监听 store 改变后自己也进行改变。

#### 通过 actionCreators 管理 action

```jsx
// actionTypes.js
const CHANGG_INPUT_VALUE = 'change_input_value'

export default {
  CHANGG_INPUT_VALUE
}

// actionCreators.js
// 只是用来接受数据返回一个对象
import { CHANGG_INPUT_VALUE } from './actionTypes'
export const getInputChangeAction = (value) => ({
    type: CHANGG_INPUT_VALUE,
    value
})

// 组件中使用
import getInputChangeAction from './actionCreators'
handleInputChange (e) {
    const action = getInputChangeAction(e.target.value)
    // 组件将Action 派发给store
    store.dispatch(action)
}
```

#### Redux 使用原则

![JspnUO.png](https://s1.ax1x.com/2020/04/25/JspnUO.png)

1. Store 是唯一的
2. 只有 Store 能够改变自己的内容 (reducer 只是处理后将新的 state 返回而已，更新还是 store 自己更新)
3. Reducer 必须是纯函数 (给定固定的输入，就一定会有固定的输出，而且不会有任何副作用)

#### Redux 中核心 API

* createStore 创建 store
* store.dispatch 向 store 派发 action
* store.getState 获取 store 中的所有数据内容
* store.subscribe 订阅 store 的改变，只要 store 发生改变，这个订阅的函数就会被执行，我们就可以获取到最新的 store 数据

### Redux ++

#### 普通组件

```jsx
class TodoList extends Component {
  render() {
    return <div>普通组件</div>
  }
}
```

#### UI 组件

负责页面的渲染，不关心逻辑怎么走。

#### 容器组件

负责页面的逻辑，并不关心页面长什么样子。

父组件通过 `props` 向 UI 组件传递数据和事件，UI 组件的所有涉及到逻辑部分都通过事件派发出来，UI 组件内不做逻辑操作

#### 无状态组件

当一个组件中只有一个 render 函数时，我们就可以用一个无状态组件定义这个组件

```jsx
const TodoListUI = (props) => {
  return <div>无状态组件</div>
}
```

性能比较高，因为他只是一个函数，普通的组件，是一个 JS”类”，这个里边还有 React 的一些生命周期函数，和 render， 所要执行的东西较多。

**当在做一个 UI 组件的时候，他只负责页面渲染的时候，不做任何页面操作的时候，一般可以通过无状态组件定义。**

#### 发送异步数据

```jsx
componentDidMount () {
    axios.get('/data').then((res) => {
        if (res.code === 200) {
            const {data} = res
            const action = initListAction(data)
            store.dispatch(action)
        }
    })
}
```

#### Redux-thunk

![Jspl2d.png](https://s1.ax1x.com/2020/04/25/Jspl2d.png)

使得在 action 可以使用异步的代码了

```jsx
// actionCreators.js
// 返回一个请求函数

const initListAction = (data) => ({
    type: 'get_ttodo_list',
    data
})

export const getTodoList = () => {
    // 接受到 dispatch
    return (dispatch) => {
        axios.get('/data').then((res) => {
            if (res.code === 200) {
                const {data} = res
                const action = initListAction(data)
                // 派发出去
                dispatch(action)
           }
        })
    }
}

// 组件中使用
componentDidMount () {
    const action = getTodoList()
    store.dispatch(action)
}
```

到底什么是 Redux 中间件

![JrzCZT.png](https://s1.ax1x.com/2020/04/25/JrzCZT.png)

#### Redux-saga

[Redux-Saga 仓库](https://github.com/redux-saga/redux-saga)

[Redux-Saga 文档](https://redux-saga-in-chinese.js.org)

![Jsp1xA.png](https://s1.ax1x.com/2020/04/25/Jsp1xA.png)

把异步代码放到 action 中去，这样有利于做自动化测试和代码拆分管理

```jsx
// actionTypes
export const GET_INIT_LIST = 'get_init_List'

// actionCreators.js
export const getInitList = () => ({
    type: GET_INIT_LIST
})

// 组件中
componentDidMount () {
    const action = getInitList()
    // 这里派发出去以后，不仅在 reducers 文件中接收到 action 在 saga 文件中也可以接收到
    store.dispatch(action)
}

// saga.js
// 将所有的异步代码都整合到一个文件里
import { takeEvery, put} from 'redux-saga/effects'
import { GET_INIT_LIST } from './actionTypes'

function* getInitList () {
    try {
        const res = yield axios.get('/data')
        const { data } = res
        const action = initListAction(data)
        // 这里使用 put 向 store 派出 action
        yield put(action)
    }catch(e) {
        console.log(e)
    }
}

function* mySage () {
    // 通过 takeEvery 捕获到每次派发的 action 的 type，然后调用 getInitList 方法
    yield takeEvery(GET_INIT_LIST, getInitList);
}

export default mySage
```

#### React-Redux 的使用

![Jsp8KI.png](https://s1.ax1x.com/2020/04/25/Jsp8KI.png)

```jsx
// index.js
import { Provider } from 'react-redux'
import store from './store'

// Provider 连接了 Store Provide 里面所有的组件都有能力获取到 store 中的数据
const App = (
  <Provider store={store}>
    <TodoList />
  </Provider>
)

// TodoList.js 组件
import { connect } from 'react-redux'

// 组件中使用 this.props.inputValue
// 组件中事件调用 this.props.changeValue

// 把 store 中的数据映射到组件变成组件的 props
const mapStateToProps = (state) => {
  return {
    inputValue: state.inputValue,
  }
}

// 我们把 store 的dispatch方法挂载到props上
const mapDispatchToProps = (dispatch) => {
  return {
    changeValue() {
      dispatch()
    },
  }
}

// 让 TodoList 这个组件和 Store 连接
export default connect(mapStateToProps, mapDispatchToProps)(TodoList)
```

#### ActionCreator

> 相当于 Vue 中的 actions

```jsx
export function clearInput(state) {
  return { type: 'clear_input', state }
}
```

#### Reducer

> 相当于 Vue 中的 mutation

```jsx
const defaultState = {
  inputValue: '',
}

export default (state = defaultState, action) => {
  if (action.type === 'clear_input') {
    const newState = JSON.parse(JSON.stringify(state))
    newState.inputValue = ''
    return newState
  }
  return state
}
```

#### Store

```jsx
import { createStore } from 'redux'
import todoApp from './reducer'
let store = createStore(todoApp)
```

### Immutable

为了防止 `state` 在无意中被修改，`Immutable` 帮助我们生成 `immutable` 对象，让我们的数据变成 `Immutable` 对象

因为他已经变成了 `Immutable` 对象，那么当我们再用这个对象的时候就不能直接使用，需要通过`get()`的形式去获取，同理，在更新的时候也需要通过 set 的形式去更新。这时候会结合之前的 `Immutable` 对象的值和设置的值返回一个全新的对象，并不会改变原始的数据。

这样通过这个库来解决我们在 `reducer` 中无意修改 `state` 的错误。

```jsx
// 使用 fromJS 将 JS 对象转换为 immutable 对象
import { fromJS } from 'immutable'

const defaultValue = fromJS({
  inputValue: '',
})

// 更新数据
// 会结合之前 immutable 对象的值和设置的值返回一个全新的对像，并不会改原始的数据
state.set('inputValue', '')

// 获取数据
state.get('inputValue')
// 获取 header 下边的 inputValue 值
state.getIn(['header', 'inputValue'])
```

#### redux-immutable

```jsx
import { combineReducers } from 'redux-immutable'

// 通过 combineReducers 包裹的 reducer 内容就是 immutable 对象类型
const todoApp = combineReducers({
  header: HeaderRuducer,
})

export default todoApp
```
