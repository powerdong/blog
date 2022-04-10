---
cover: https://s1.ax1x.com/2020/05/24/YzA0oD.png
coverY: 0
---

# 页面路由——React-Router

## React-Router

### 简介

React Router 是一个基于 React 之上的强大路由库，它可以让你向应用中快速地添加视图和数据流，同时保持页面与 URL 之间的同步。

#### 使用 React Router

```jsx
import React from 'react'
import { render } from 'react-dom'

// 首先我们需要导入一些组件...
import { Router, Route, Link } from 'react-router'

// 然后我们从应用中删除一堆代码和
// 增加一些 <Link> 元素...
const App = React.createClass({
  render() {
    return (
      <div>
        <h1>App</h1>
        {/* 把 <a> 变成 <Link> */}
        <ul>
          <li><Link to="/about">About</Link></li>
          <li><Link to="/inbox">Inbox</Link></li>
        </ul>

        {/*
          接着用 `this.props.children` 替换 `<Child>`
          router 会帮我们找到这个 children
        */}
        {this.props.children}
      </div>
    )
  }
})

// 最后，我们用一些 <Route> 来渲染 <Router>。
// 这些就是路由提供的我们想要的东西。
React.render((
  <Router>
    <Route path="/" component={App}>
      <Route path="about" component={About} />
      <Route path="inbox" component={Inbox} />
    </Route>
  </Router>
), document.body)
```

如果你不熟悉 JSX，也可以用普通对象来替代。

```js
const routes = {
    path: '/',
    component: App,
    childRoutes: [
        {path: 'about', component: About},
        {path: 'inbox', component: Inbox}
    ]
}
React.render(<Router routes={routes} />, document.body)
```

#### 获取 URL 参数

为了从服务器获取 message 数据，我们首先需要知道它的信息。当渲染组件时，React Router 会自动向 Route 组件中注入一些有用的信息，尤其是路径中动态部分的参数。

```jsx
const Message = React.createClass({

  componentDidMount() {
    // 来自于路径 `/inbox/messages/:id`
    const id = this.props.params.id

    fetchMessage(id, function (err, message) {
      this.setState({ message: message })
    })
  },
})
```

也可以通过 query 字符串来访问参数。比如访问 `/foo?bar=baz`可以通过访问 `this.props.location.query.bar` 从 Route 组件中获取 `'baz'`的值

### 基础

![YzAfw8.png](https://s1.ax1x.com/2020/05/24/YzAfw8.png)

#### 添加首页

当URL为`/`时，我们想渲染一个在 `App`中的组件。不过此时，App 的 render中的`this.prop.children`还是undefined。这种情况下我们可以使用 `IndexRoute`来设置一个默认页面。

```jsx
import { IndexRoute } from 'react-router'

const Dashboard = React.createClass({
  render() {
    return <div>Welcome to the app!</div>
  }
})

React.render((
  <Router>
    <Route path="/" component={App}>
      {/* 当 url 为 / 时渲染 Dashboard */}
      <IndexRoute component={Dashboard} />
      <Route path="about" component={About} />
      <Route path="inbox" component={Inbox}>
        <Route path="messages/:id" component={Message} />
      </Route>
    </Route>
  </Router>
), document.body)
```

#### 让 UI 从 URL 中解耦出来

通过在定义 path 中使用 `/messages/:id` 替换 `messages/:id` 在多层前套路由中使用绝对路径的能力让我们对 URL 拥有绝对的掌握。我们无需在 URL 中添加更多的层级。

#### 兼容旧的 URL

使用 `<Redirect>` 使URL重定向。

```jsx
import { Redirect } from 'react-router'

React.render((
  <Router>
    <Route path="/" component={App}>
      <IndexRoute component={Dashboard} />
      <Route path="about" component={About} />
      <Route path="inbox" component={Inbox}>
        <Route path="/messages/:id" component={Message} />

        {/* 跳转 /inbox/messages/:id 到 /messages/:id */}
        <Redirect from="messages/:id" to="/messages/:id" />
      </Route>
    </Route>
  </Router>
), document.body)
```

#### 进入和离开 Hook

Route 可以定义 `onEnter` 和 `onLeave` 两个 hook，这些 hook 会在页面跳转确认时触发一次。这些 hook 对于一些情况非常的有用，例如权限验证或者在路由跳转前将一些数据持久化保存起来。

在路由跳转过程中

* `onLeave` 会在所有将离开的路由中触发，从最下层的路由开始直到最外层路由结束。
* `onEnter` 会在将进入的路由时触发。

#### 替换配置方式

如果你不想使用 JSX，也可以直接使用原生的 route 数组对象。

```jsx
const routeConfig = [
  { path: '/',
    component: App,
    indexRoute: { component: Dashboard },
    childRoutes: [
      { path: 'about', component: About },
      { path: 'inbox',
        component: Inbox,
        childRoutes: [
          { path: '/messages/:id', component: Message },
          { path: 'messages/:id',
            onEnter: function (nextState, replaceState) {
              replaceState(null, '/messages/' + nextState.params.id)
            }
          }
        ]
      }
    ]
  }
]

React.render(<Router routes={routeConfig} />, document.body)
```

### 路由匹配原理

路由拥有三个属性来决定是否匹配一个URL

* 嵌套关系
* 路径语法
* 优先级

#### 嵌套关系

当一个给定的 URL 被调用时，整个集合中(命中的部分)都会被渲染。嵌套路由被描述成一种树形结构

#### 路径语法

匹配一个(或一部分)URL的一个字符串模式。大部分的路由路径都可以直接按照字面量理解，除了下边的特殊符号

* `:paramName` - 匹配一段位于 `/`、`?`或`#`之后的URL
* `()` - 在它内的内容被认为是可选的
* `*` - 匹配任意字符直到命中下一个字符或者整个 URL 的末尾

#### 优先级

路由算法会根据定义的顺序自顶向下匹配路由

### Histories

React Router是建立在 history 上的。简而言之，一个 history 知道如何去监听浏览器地址栏的变化，并解析这个 URL 转化为 location 对象，然后 router 使用它匹配到路由，最后正确的渲染页面。

* browserHistory
* hashHistory
* createMemoryHistory

#### 使用

```jsx
import { browserHistory } from 'react-router'

render(
  <Router history={browserHistory} routes={routes} />,
  document.getElementById('app')
)
```

#### browserHistory

他使用浏览器中 History API 用于处理 URL

[**服务器配置**](https://react-guide.github.io/react-router-cn/docs/guides/basics/Histories.html)

服务器需要做好处理 URL 的准备。处理应用启动最初的/这样的请求是没问题的，但当用户来回跳转并在`/account/123`刷新时，服务器就会收到来自 `account/123`的请求，这时需要处理这个 URL 并响应对应的 JavaScript 代码。

#### hashHistory

Hash History 使用 URL 中的 hash `#` 部分去创建形如`example.com/#/some/path` 的路由。

**对比**

Hash History 不需要服务器任何配置就可以运行，但是我们不推荐在实际线上环境中使用，因为每一个 web 应用都应该渴望使用 browserHistory。

**传递参数**

当一个 History 通过应用程序中的 push 或 replace跳转时，他可以在新的 location中存储 location state而不显示在 URL 中，这就像是在一个 HTML 中 post 的表单数据。

#### createMemoryHistory

Memory History 不会在地址栏被操作或读取。同时它也非常适合测试和其他渲染环境

和另外两种 history 的一点不同是必须创建它，这种方式便于测试。

```jsx
const history = createMemoryHistory(location)
```

#### 使用

```jsx
import React from 'react'
import { render } from 'react-dom'
import { browserHistory, Router, Route, IndexRoute } from 'react-router'

import App from '../components/App'
import Home from '../components/Home'
import About from '../components/About'
import Features from '../components/Features'

render(
  <Router history={browserHistory}>
    <Route path='/' component={App}>
      <IndexRoute component={Home} />
      <Route path='about' component={About} />
      <Route path='features' component={Features} />
    </Route>
  </Router>,
  document.getElementById('app')
)
```

#### 默认路由 `(IndexRoute)` 与 `IndexLink`

**默认路由 `(IndexRoute)`**

```jsx
<Router>
  <Route path="/" component={App}>
    <IndexRoute component={Home}/>
    <Route path="accounts" component={Accounts}/>
    <Route path="statements" component={Statements}/>
  </Route>
</Router>
```

**Index Links**

如果你在这个 app 中使用 `<Link to="/">Home</Link>` , 它会一直处于激活状态，因为所有的 URL 的开头都是 `/` 。 这确实是个问题，因为我们仅仅希望在 `Home` 被渲染后，激活并链接到它。

如果需要在 `Home` 路由被渲染后才激活的指向 `/` 的链接，请使用 `<IndexLink to="/">Home</IndexLink>`

### 高级用法

![YzAqO0.png](https://s1.ax1x.com/2020/05/24/YzAqO0.png)

#### 动态路由

React Router 适用于小型网站，也可以支持大型网站。

对于大型应用来说，一个首当其冲的问题就是所需加载的 JavaScript 的大小。程序应当只加载渲染页所需的 JavaScript。

路由是个非常适于做代码分拆的地方：他的责任就是配置好每个 view

React Router 里的路径匹配以及组件加载都是异步完成，不仅允许你延迟加载组件，并且可以延迟加载路由配置。在首次加载包中你只需要有一个路径定义，路由会自动解析剩下的路径。

Route 可以定义 `getChildRoutes`，`getIndexRoute`和`getComponents`这几个函数。他们都是异步执行，并且只有在需要的时候才被调用。称之为“逐渐匹配”。

匹配 URL 并只加载该 URL 对应页面所需的路径配置和组件。

```jsx
const CourseRoute = {
  path: 'course/:courseId',

  getChildRoutes(location, callback) {
    require.ensure([], function (require) {
      callback(null, [
        require('./routes/Announcements'),
        require('./routes/Assignments'),
        require('./routes/Grades'),
      ])
    })
  },

  getIndexRoute(location, callback) {
    require.ensure([], function (require) {
      callback(null, require('./components/Index'))
    })
  },

  getComponents(location, callback) {
    require.ensure([], function (require) {
      callback(null, require('./components/Course'))
    })
  }
}
```

#### 跳转前确认

React Router 它提供一个 `routerWillLeave` 钩子，使得组件可以拦截正在发生的跳转，或在离开 route 前提示用户。

1. `return false` 取消此次跳转
2. `return`返回提示信息，在离开 route前提示用户进行确认。

通过在 route 组件中引入 `Lifecycle` mixin 来安装这钩子

```jsx
import { Lifecycle } from 'react-router'

const Home = React.createClass({

  // 假设 Home 是一个 route 组件，它可能会使用
  // Lifecycle mixin 去获得一个 routerWillLeave 方法。
  mixins: [ Lifecycle ],

  routerWillLeave(nextLocation) {
    if (!this.state.isSaved)
      return 'Your work is not saved! Are you sure you want to leave?'
  },
})
```

#### 服务端渲染

服务端渲染与客户端渲染有些许不同

* 发生错误时发送一个 500 的响应
* 需要重定向时发送一个 30x 的响应
* 在渲染之前获得数据（用 router 帮你完成这点）

为了迎合这一需求，需要在 `<Router />`API 下一层使用

* 使用 `match` 在渲染之前根据 location 匹配 route
* 使用 `RoutingContext` 同步渲染route组件

```jsx
import { renderToString } from 'react-dom/server'
import { match, RoutingContext } from 'react-router'
import routes from './routes'

serve((req, res) => {
  // 注意！这里的 req.url 应该是从初始请求中获得的
  // 完整的 URL 路径，包括查询字符串。
  match({ routes, location: req.url }, (error, redirectLocation, renderProps) => {
    if (error) {
      res.send(500, error.message)
    } else if (redirectLocation) {
      res.redirect(302, redirectLocation.pathname + redirectLocation.search)
    } else if (renderProps) {
      res.send(200, renderToString(<RoutingContext {...renderProps} />))
    } else {
      res.send(404, 'Not found')
    }
  })
})
```

#### 组件生命周期

```jsx
<Route path="/" component={App}>
  <IndexRoute component={Home}/>
  <Route path="invoices/:invoiceId" component={Invoice}/>
  <Route path="accounts/:accountId" component={Account}/>
</Route>
```

路由切换时，组件生命周期的变化情况

1.当用户打开应用 `'/'` 页面

| 组件      | 生命周期              |
| ------- | ----------------- |
| APP     | componentDidMount |
| Home    | componentDidMount |
| Invoice | N/A               |
| Account | N/A               |

2.当用户从 `'/'`跳转到 `'/invoice/123'`

| 组件      | 生命周期                                          |
| ------- | --------------------------------------------- |
| App     | componentWillReceiveProps, componentDidUpdate |
| Home    | componentWillUnmount                          |
| Invoice | componentDidMount                             |
| Account | N/A                                           |

* App 从 router 中接收到新的 props，所以触发了两个生命周期方法
* Home 不再被渲染，所以它将被移除
* Invoice 首次被挂载

3.当用户从 `/invoice/123` 跳转到 `/invoice/789`

| 组件      | 生命周期                                          |
| ------- | --------------------------------------------- |
| App     | componentWillReceiveProps, componentDidUpdate |
| Home    | N/A                                           |
| Invoice | componentWillReceiveProps, componentDidUpdate |
| Account | N/A                                           |

4.当从 `/invoice/789` 跳转到 `/accounts/123`

| 组件      | 生命周期                                          |
| ------- | --------------------------------------------- |
| App     | componentWillReceiveProps, componentDidUpdate |
| Home    | N/A                                           |
| Invoice | componentWillUnmount                          |
| Account | componentDidMount                             |

**获取数据**

我们在 `Invoice` 组件里实现一个简单的数据获取功能

```jsx
let Invoice = React.createClass({

  getInitialState () {
    return {
      invoice: null
    }
  },

  componentDidMount () {
    // 上面的步骤2，在此初始化数据
    this.fetchInvoice()
  },

  componentDidUpdate (prevProps) {
    // 上面步骤3，通过参数更新数据
    let oldId = prevProps.params.invoiceId
    let newId = this.props.params.invoiceId
    if (newId !== oldId)
      this.fetchInvoice()
  },

  componentWillUnmount () {
    // 上面步骤四，在组件移除前忽略正在进行中的请求
    this.ignoreLastFetch = true
  },

  fetchInvoice () {
    let url = `/api/invoices/${this.props.params.invoiceId}`
    this.request = fetch(url, (err, data) => {
      if (!this.ignoreLastFetch)
        this.setState({ invoice: data.invoice })
    })
  },

  render () {
    return <InvoiceView invoice={this.state.invoice}/>
  }

})
```
