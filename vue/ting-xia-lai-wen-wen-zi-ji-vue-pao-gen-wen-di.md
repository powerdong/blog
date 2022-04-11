# 停下来，问问自己——Vue-刨根问底

### 为什么官方建议数据异步请求在`mounted`事件进行?

很多人觉得在`created` 事件里面把数据请求到，然后一起生成虚拟 `DOM`，再渲染会更好。实际上呢，请求是需要时间的，而且这个时间具有不稳定性，很可能 vue 的虚拟 DOM 准备好了，你的数据才请求到，然后又得更新一遍虚拟`DOM`，再渲染，极大地延长了白屏时间，用户体验很不好。**而在 mounted 事件请求数据呢，静态页面会先渲染好，等数据好了，再更新部分 DOM 即可。**

### 子组件修改父组件传过来的值

v-model 在使用的时候很像双向绑定的，但是 Vue 是单项数据流，`v-model` 只是语法糖而已：父组件用`v-bind`将值传给子组件，子组件通过`change/input`事件触发修改父组件的值。

```
<input v-model="inputValue" />
<!-- 等价于 -->
<input :value="inputValue" @change="inputValue" />
```

v-model 不仅仅能在`input`上用，在组件上也能使用`vue`组件间传递数据是单向的，即数据总是由父组件传递到子组件，子组件在其内部可以有自己维护的数据，但它无权修改父组件传递给它的数据，我们也可以参照`v-model`语法糖进行修改父组件的值，但是每次都这样写太麻烦了，vue 提供了一个修饰符`.sync`,用法如下：

```
<child :value.sync="inputValue"></child>

<!-- 子组件 -->
<script>
  export default{
    props:{
      //props可以设置值得类型，默认值，是否必传以及校验函
      value:{
        type: [String, Number],
        required: true
      }
    }
    //用一个变量中转，子组件中就用_value就不会直接修改父组件的值
    computed: {
      _value: {
        get() {
            return this.value;
        },
        set(val) {
            this.$emit('update:value', val);
        },
      }
    }
  }
</script>
```

### Vue 双向绑定的原理

`v-model` 是 Vue 2.2.0 添加的，在此之前 Vue 只支持单向数据流

![QdCN9I.png](https://s2.ax1x.com/2019/12/08/QdCN9I.png)

Vue 是采用数据劫持结合发布者-订阅者模式的方式，通过`Object.defineProperty()`来劫持各个属性的`setter`,`getter`，在数据变动时发布消息给订阅者，触发相应的监听回调。

#### 双向绑定实现

如果想实现显示文字根据输入 input 变化，实现一个简单版的

```
<input type="text" id="a" />
<span id="b"></span>

<script>
  const obj = {}
  Object.defineProperty(obj, 'hello', {
    get() {
      console.log(`方法被调用`)
    },
    set(newVal) {
      document.getElementById('a').value = newVal
      document.getElementById('b').innerHTML = newVal
    }
  })
  document.addEventListener('keyup', function(e) {
    obj.hello = e.target.value
  })
</script>
```

#### 拆解任务，实现 vue 的双向数据绑定

我们最终实现下面 vue 的效果

```
<div id="app">
  <input type="text" v-model="text" />
</div>
<script>
  const vm = new Vue({
    id: 'app',
    data: {
      text: 'hello world'
    }
  })
</script>
```

1. 输入框的文本与文本节点的 data 数据绑定
2. 输入框的内容发生变化时，data 中的数据也发生变化，实现 view->model 的变化
3. data 中的数据发生变化时，文本节点的内容同步发生变化，实现 model->view 的变化

要实现 1 的要求，则又涉及到了 dom 的编译，其中有一个 DocumentFragment 的知识点。

#### DocumentFragment

众所周知，vue 吸收了 react 虚拟 DOM 的优点，使用 DocumentFragment 处理节点，其性能和速度远胜于直接操作 dom。vue 进行编译时，就是将所有挂载在 dom 上的子节点进行劫持到使用 DocumentFragment 处理节点，等到所有操作都执行完毕，将 DocumentFragment 再一模一样返回到挂载的目标上。

先实现一段劫持函数，将要操作的 dom 全部劫持到 DocumentFragment 中，然后再 append 会原位置。

```
<div id="app">
  <input type="text" v-model="text" />
</div>
<script>
  const app = document.getElementById('app')
  const nodeToFragment = (node) => {
    const flag = document.createDocumentFragment()
    let child
    while ((child = node.firstChild)) {
      flag.appendChild(child) //不断劫持挂载元素下的所有dom节点到创建的DocumentFragment
    }
    return flag
  }
  const dom = nodeToFragment(app)
</script>
```

#### 数据初始化绑定

当已经获取到所有的 dom 元素之后，则需要对数据进行初始化绑定，这里简单涉及到了模板的编译。

```
// 编译 HTML 模板
const compile = (node, vm) => {
  const regex = /\{\{(.\*)\}\}/ //为临时正则表达式，为 demo 而生
  //如果节点类型为元素的话
  if (node.nodeType === 1) {
    const attrs = node.attributes
    for (let i = 0; i < attrs.length; i++) {
      let attr = attrs[i]
      if (attr.nodeName === 'v-model') {
        let name = attr.nodeValue
        node.addEventListener('input', function(e) {
          vm.data[name] = e.target.value
        })
        node.value = vm.data[name]
        node.removeAttribute('v-model')
      }
    }
  }
  //如果节点类型为文本的话
  if (node.nodeType === 3) {
    if (regex.test(node.nodeValue)) {
      let name = RegExp.$1 //获取匹配的字符串，又学到了。。。
      name = name.trim()
      node.nodeValue = vm.data[name]
    }
  }
}

//劫持挂载元素到虚拟dom
let nodeToFragment = (node, vm) => {
  const flag = document.createDocumentFragment()
  let child
  while ((child = node.firstChild)) {
    compile(child, vm) //绑定数据，插入到虚拟DOM中
    flag.appendChild(child)
  }
  return flag
}

//初始化
class Vue {
  constructor(option) {
    this.data = option.data
    let id = option.el
    let dom = nodeToFragment(document.getElementById(id), this)
    document.getElementById(id).appendChild(dom)
  }
}

const vm = new Vue({
  el: 'app',
  data: {
    text: 'hello world'
  }
})
```

通过以上代码先实现了第一个要求，文本框和文本节点已经出现了 hello world 了

#### 响应式的数据绑定

接下来我们要实现数据双向绑定的第一步，即 view->model 的绑定。根据之前那个简单的例子看到，我们可以通过 input 事件实时获取 input 中的值，接着将通过 Object.defineProperty 这个方法将 data 中的 text 设置为 vm 的访问器属性。当我们将获取到的 input 值设置 vm.data.text 时，通过 set 方法，实现了数据层的绑定。在这一步，set 中要做的操作是更新属性的值。

```
let defineReactive = (obj,key,val) => {
   Object.defineProperty(obj,key,{
      get(val){
         return val;
      }
      set(newVal,oldVal){
         if(newVal === oldVal) return;
         val = newVal;
         console.log(`我获取到了新${val},并成功设置`);
      }
   })
};

//监听所有 data 传递进来的数据，将他们绑定到原型 vm 上面
let observe = (obj,vm) => {
   Object.keys(obj).forEach((key)=>{
      defineReactive(vm.data,key,obj[key]);
   })
};
```

#### 订阅/发布模式（subscribe\&publish）

text 属性变化了，set 方法触发了，可以通过 view 层的改变实时改变数据，可是并没有改变文本节点的数据。一个新的知识点：订阅发布模式。

订阅发布模式定义了一种一对多的关系，让多个观察者同时监听一个主题对象，这个主体对象的改变会通知所有观察者对象。

发布者发出通知=>主题对象收到通知并推送给订阅者=>订阅者执行操作

```
// 三个订阅者
let sub1 = {
  update() {
    console.log(1)
  }
}
let sub2 = {
  update() {
    console.log(2)
  }
}
let sub3 = {
  update() {
    console.log(3)
  }
}

// 一个主题发布器
class Dep {
  constructor() {
    this.subs = [sub1, sub2, sub3]
  }
  notify() {
    subs.forEach((sub) => {
      sub.update()
    })
  }
}
const dep = new Dep()
// 一个发布者
const pub = {
  publish() {
    dep.notify()
  }
}
pub.publish()
```

上图为一个简单实例，发布者执行发布命令，所有这个主题的订阅者执行更新操作。接下去我们要做的就是，当 set 方法触发后，input 作为发布者，改变了 text 属性；而文本节点作为订阅者，在收到消息后执行更新操作。

#### 双向绑定的实现

1. 每次 new 一个新的 vue 对象时，主要是做了两件事，一件是监听数据：observer(监听数据)，第二个是编译 HTML，HTML，nodeToFragment(id)。
2. 在监听数据的过程中，会为 data 中的每一个属性生成一个主题对象。
3. 而在编译 HTML 的过程中，会为每个与数据绑定的相关节点生成一个订阅者 watcher，订阅者 watcher 会将自己订阅到相应属性的 dep 中。
4. 在前面的方法中已经实现了：修改输入框内容=>再时间回调中修改属性值=>触发属性的 set 方法。
5. 接下来要做的是发出通知 dep.notify=>发出订阅者的 update 方法=>更新视图。

那么如何将 watcher 添加到关联属性的 dep 中呢。

编译 HTML 过程中，为每一个与 data 关联的节点生成一个 watcher，那么 watcher 中又发生了什么？

```
// 每一个属性节点的 watcher
class Watcher{
   constructor(vm,node,name){
      Dep.target = this;
      this.name = name;
      this.node = node;
      this.vm = vm;
      this.update();
      Dep.target = null;
   }
   update(){
      //获得最新值，然后更新视图
      this.get();
      this.node.nodeValue = this.value;
   }
   get(){
      this.value = this.vm.data[this.name];
   }
}
在编译 HTML 的过程中，生成 watcher

let compile = (node,vm){
   // ......
   //如果节点类型为文本的话
   if(node.nodeType === 3){
      if(regex.test(node.nodeValue)){
         let name = RegExp.$1;
         name = name.trim();
         node.nodeValue = vm.data[name];
         //在编译过程中，每发现一个属性，则新建一个 watcher
         new Watcher(vm,node,name);//在此处添加订阅者
      }
   }
}
```

* 首先将自己赋给了一个全局变量 Dep.target;然后执行了 update 方法，进而执行了 get 方法，读取了 vm 的访问器属性，从而触发了访问器属性的 get 方法，get 方法将相应的 watcher 添加到对应访问器属性的 dep 中
* 再次，获取属性的值，然后更新视图
* 最后将 dep.target 设置为空，是因为这是个全局变量也是 watcher 与 dep 之间唯一的桥梁，任何时间都只能保证只有一个值。

```
// 一个主题发布器
class Dep(){
   constructor(){
      this.subs = [];
   }
   notify(){
      this.subs.forEach((sub) => {
         sub.update();
      }
   }
   addSub(sub){
      this.subs.push(sub);
      }
}
let defineReactive = (obj,key,val) => {
   let dep = new Dep();
   Object.defineProperty(obk,key,{
      get(){
         //在此处将所有的监测器 watcher 添加进发布器，每一个属性都有自己的发布器
         if(dep.target) dep.addSub(dep.target);
      }
      set(newVal,oldVal){
         if(newVal === oldVal) return;
         val = newVal;
         dep.notify();
      }
   })
}
```

至此，hello world 双向绑定就基本实现了。文本内容会随输入框内容同步变化，在控制器中修改 vm.text 的值，会同步反映到文本内容中。
