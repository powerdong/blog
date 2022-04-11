# 你有喘息的机会吗?——Vue,逐步了解

### 声明式渲染

* 命令式编程:命令”机器”如何去做事情(how),这样不管你想要的是什么(what),它都会按照你的命令实现
* 声明式编程:告诉”机器”你想要的是什么(what)，让机器想出如何去做(how)

### 三大模板

#### html 模板

v-html=”标签”

#### 字符串模板

template:”

hello world

“ 使用模板字符串拼接

```
<script type="x/template" id="demo">
  <div>
      hello world{{id}}
  </div>
</script>
var vm = new Vue({
    template: "#demo"
})
```

#### render 函数(效率更高)

```
var vm = new Vue = ({
    render(createElement){
        var dom = createElement("div",{
            attrs:{
                // 添加属性
                id:"demo"
            },
            class:{
                // 添加class
                vue1:true,
                vue2:false
            },
            domPros:{
                // 会覆盖子元素
                innerHTML:"<div>HTML</div>"
            }
        },["hello",createElement("p",["world"])]);
        // 渲染dom
        return dom
    }
})
```

### 组件基础

#### 基本实例

```
//定义一个名为button-counter的新组件
Vue.component('button-counter',{
    data:function(){
        return {
            count : 0
        }
    }
    template:'<button v-on:click="count++">clicked me {{count}} +1</button>'
})
```

组件是可复用的 Vue 实例，且带有一个名字。我们可以在一个通过`new Vue`创建的 Vue 根实例中，把这个组件作为自定义元素来使用。

```
<div id="demo">
  <button-counter></button-counter>
</div>
new Vue({
  el: '#demo'
})
```

#### 组件的复用

可以将组件进行任意次数的复用

*   ```
    data
    ```

    必须是一个函数

    * 在定义组件时，`data`并不是像之前提供一个对象
    * 一个组件的`data`选项必须是一个函数，每个实例维护一份被返回对象的独立的拷贝

#### 通过`Prop`向子组件传递数据

`Prop`是你可以在组件上注册的一些自定义特性。当一个值传递给一个`prop`特性的时候，他就变成了那个组件实例的一个属性。我们可以用一个`props`选项将其包含在该组件可接受的`prop`列表中。

```
Vue.component('blog-post',{
    props:['title],
    template:'<h3>{{title}}</h3>
})
```

### 组件注册

#### 组件名

在注册一个组件的时候，我们始终需要给它一个名字。

```
Vue.component('my-component-name', {
  // template: "<div>我是一个组件</div>",
  // template:`<div>我是一个组件  </div>`
})
```

该组件名就是`Vue.component`的第一个参数(推荐字母全小写且必须包含一个连字符)

#### 全局注册

只使用`Vue.component`来创建组件。这些组件是**全局注册的**。在它们注册后可以用在任何新创建的 Vue 根实例(`new Vue`)的模板中。在组件内部也可以相互使用。

#### 局部注册

全局注册往往是不够理想的。全局注册所有的组件意味着即便你已经不再使用一个组件了，它仍然会包含在最终的构建结果中。这造成用户下载的 JavaScript 的无谓的增加。

可以通过普通的 JavaScript 对象来定义组件。

```
var ComponentA = {
  template:`<div>我是局部组件</div>`
};
var ComponentB = {
  /* ... */
};
new Vue({
  el:"#demo,
  components:{
    "component":{
      template:`<div>我是局部组件</div>`
    }
  }
})
```

然后在 `components` 选项中定义你想要使用的组件：

```
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

### Prop

#### Prop 的大小写

HTML 中的特性名是大小写不敏感的，所以浏览器会把所有的大写字符解释为小写字符。这意味着在使用 DOM 中的模板时，camelCase 的 prop 名需要使用其等价的 kebab-case 命名。

#### Prop 类型

```
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```

如果希望每个 prop 都有指定的值类型，可以用对象的形式列出 prop。

```
props: {
  title: {
    // 类型
    type:String,
    // 默认值
    default:"hello",
    // 是否必填
    required:true
  },
  likes: {
    //多个可能的类型
    type:[String,Number]
  },
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}
```

#### 传递静态或动态的 Prop

可以给 prop 传入一个静态的值，也可以通过`v-bind`动态赋值

```
<!-- 静态 -->
<blog-post title="My journey with Vue"></blog-post>
<!-- 动态 -->
<blog-post v-bind:title="post.title"></blog-post>
```
