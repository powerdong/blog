# 你累吗？来来来，安利框架——Vue-初次见面

## 初探 Vue.js

### 渐进式的开发框架

通过不同的项目引用不同的模块

* 声明式渲染
* 组件系统
* 客户端路由
* 大规模状态管理
* 构建工具

### Vue 的特点

#### 响应的数据绑定

* 当数据发生改变，自动更新视图
* 利用`Object.definePrototype`中的`getter/setter`代理数据

#### 视图组件

* UI 页面映射组件树
* 组件可重用，可维护

#### 虚拟 DOM

1. 提供一种方便的的工具，使得开发效率得到保证
2. 保证最小化 DOM 操作，使得执行效率得到保证

```
/**
 * 构建虚拟DOM对象
 * @param {string} tagName  根节点
 * @param {string} prop     标签属性
 * @param {array} children  子节点
 */
function vElement(tagName, prop, children) {
  if (!(this instanceof vElement)) {
    return new vElement(tagName, prop, children);
  }
  if (Object.prototype.toString.call(prop) === "[object Array]") {
    children = prop;
    prop = {};
  }
  this.tagName = tagName;
  this.children = children;
  this.prop = prop;
  var count = 0;
  this.children.forEach((child, index) => {
    if (child instanceof vElement) {
      count += child.count;
    }
    count++;
  });
  this.count = count;
}
/**
 * 构建真实DOM树
 */
vElement.prototype.render = function() {
  var el = document.createElement(this.tagName);
  var children = this.children;
  var prop = this.prop;
  for (const item in prop) {
    const curProp = prop[item];
    el.setAttribute(item, curProp);
  }
  children.forEach((child, index) => {
    if (child instanceof vElement) {
      var childDom = child.render();
    } else {
      var childDom = document.createTextNode(child);
    }
    el.appendChild(childDom);
  });
  return el;
};
```

#### MVVM 模式

M: `model`数据模型 V: `View`视图模板 VM: `View-model`视图模型(中间层)

#### Vue 实例

```
<div id="demo">
  <!-- v-bind自定义属性 数组动态绑定多个class-->
  <span v-bind:data="message" v-bind:id="id" v-bind:class="[myClass1,myClass2]"
    >{{name}}</span
  >
  <!-- class通过对象设置  key为class名 value为bool true->设置  false->不设置 -->
  <!-- 通过对象   动态添加删除class -->
  <span v-bind:class="{class:ifClass}">{{age}}</span>
  <input v-model="input" />
  {{changeName}}
</div>
var vm = new Vue({
    el : "#demo",
    data:{
        name:'powerdong',
        message:'123',
        age:'18'
        id:'demo1',
        myClass1:'c234',
        myClass2:'c456',
        ifClass:true,
        input: 'hello'
    },
    computed:{
        //计算属性
        changeName:function(){
            return this.name + 666
        }
    },
    methods:{
      // 装方法
    }
})
```

*   ```
    v-show
    ```

    \=”bool”

    * 元素的显示与隐藏
    * display
*   ```
    v-if
    ```

    \=”bool”

    * 元素的显示与隐藏
    * dom 节点的存在与否
*   ```
    v-for
    ```

    \=”array”

    * 列表渲染
    * item in m
    * 通过数组的`push`方法可实现添加数据
*   ```
    v-on
    ```

    添加一个事件监听器

    *   ```
        v-on:click
        ```

        \=”fnName”

        * 在 methods 字段中设置方法
    *   ```
        v-on:keyup.enter
        ```

        \=”fnName”

        * 只有 enter 才触发
        * 事件修饰符
*   ```
    v-mode
    ```

    \=”value”

    * 实现表单输入和应用状态之间的双向数据绑定

### Vue 注册组件

```
Vue.component("todo-item", {
  props: ["todo"],
  template: "<li>{{todo.text}}</li>"
});
<ol>
  <!-- 创建一个 todo-item 实例 -->
  <todo-item></todo-item>
</ol>
```

### 生命周期

![vue生命实例](https://cn.vuejs.org/images/lifecycle.png)

### Style 和 Class

`v-bind:style`是一个 `JavaScript` 对象。`CSS` 属性名可以用驼峰式或短横线分割来命名(这时需要用引号括起来)

```
<div v-bind:style="{color:activeColor, fontSize: fontSize + 'px'}"></div>
<div v-bind:style="styleObject"></div>
data: {
  activeColor: 'red',
  fontSize: 30
}

data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

### 条件渲染

```
v-if`和`v-else
<div v-if="Math.random() > 0.5">
  Now you see me
</div>
<div v-else>
  Now you don't
</div>
```

**`v-else` 元素必须紧跟在带 `v-if` 或者`v-else-if` 的元素的后面，否则它将不会被识别。**

`v-else-if`(2.1.0新增)

**类似于 `v-else`，`v-else-if` 也必须紧跟在带 `v-if` 或者 `v-else-if` 的元素之后。**

#### `v-if` VS `v-show`

* `v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
* `v-if` 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
* 相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 `CSS` 进行切换。
* 一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。
* **不推荐**同时使用`v-if`和`v-for`
* 当`v-if`与`v-for`一起使用时，`v-for`具有比`v-if`更高的优先级

### 列表渲染

#### 用`v-for`把一个数组对应为一组元素

*   在

    ```
    v-for
    ```

    块中，我们可以访问所有父作用域的属性。

    ```
    v-for
    ```

    还支持一个可选的第二个参数，即当前项的索引。

    * `v-for="(item, index) in items"`
    * 也可以用`of`替代`in`作为分隔符。
*   ```
    v-for
    ```

    可以来遍历一个对象的属性

    * `v-for="(value, name, index) in object">`

> 在遍历对象时，会按`Object.keys()`的结果遍历，但是**不能**保证它的结果在不同的`JavaScript`引擎下都一致

*   建议尽可能在使用

    ```
    v-for
    ```

    时提供

    ```
    key
    ```

    attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性

    能上的提升。

    * `v-for="item in items" v-bind:key="item.id">`

> 不要使用对象或数组之类的非基本类型值作为`v-for`的`key`。请用字符串或数值类型的值。

**变异方法**

* `push()`
* `pop()`
* `shift()`
* `unshift()`
* `splice()`
* `sort()`
* `reverse()`

在使用数组的这些方法时，它们会触发视图的更新

**替换数组**

变异方法，会改变调用了这些方法的原始数组。 想比之下，也有非变异方法 例如`filter()`、`concat()`和`slice()`。它们不会改变原始数组，而**总是返回一个新数组**。当使用非变异方法时，可以用新数组替换旧数组。

**注意事项**

```
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```

#### 对象变更检测注意事项

对于已经创建的实例，`Vue` 不允许动态添加根级别的响应式属性。但是，可以使用 `Vue.set(object, propertyName, value)` 方法向嵌套对象添加响应式属性。

### 事件处理

#### 监听事件

可以用`v-on`指令监听DOM事件，并在触发时运行一下`JavaScript`代码。

#### 事件修饰符

在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()`是非常常见的需求。

* `.stop`
* `.prevent`
* `.capture`
* `.self`
* `.once`
* `.passive`

```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次  2.1.4新增-->
<a v-on:click.once="doThis"></a>

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 2.3.0 新增-->
<div v-on:scroll.passive="onScroll">...</div>
```

> 使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止所有的点击，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。

> 不要把 `.passive` 和 `.prevent` 一起使用，因为 `.prevent` 将会被忽略，同时浏览器可能会向你展示一个警告。请记住，`.passive` 会告诉浏览器你不想阻止事件的默认行为。

#### 按键修饰符

```
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">
```

**为什么在 HTML 中监听事件?**

*   使用

    ```
    v-on
    ```

    有几个好处

    1. 扫一眼 `HTML` 模板便能轻松定位在 `JavaScript` 代码里对应的方法。
    2. 因为你无须在 `JavaScript` 里手动绑定事件，你的 `ViewModel` 代码可以是非常纯粹的逻辑，和 DOM 完全解耦，更易于测试。
    3. 当一个 `ViewModel` 被销毁时，所有的事件处理器都会自动被删除。你无须担心如何清理它们。

### 表单输入绑定

#### 基础用法

可以使用`v-model`指令在表单`<input>`、`<textarea>`及`<select>`元素上创建双向数据绑定。

v-model 在内部为不同的输入元素使用不同的属性并抛出不同的事件：

* `text` 和 `textarea` 元素使用 `value` 属性和 `input` 事件；
* `checkbox` 和 `radio` 使用 `checked` 属性和 `change` 事件；
* `select` 字段将 `value` 作为 `prop` 并将 `change` 作为事件。

**复选框**

单个复选框，绑定到布尔值 多个复选框，绑定到同一个数组

```
<div id='example-3'>
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Checked names: {{ checkedNames }}</span>
</div>
new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
```

**单选框**

单选框绑定到字符串上

**选择框**

单选时使用字符串 多选时绑定到一个数组

#### 修饰符

**`.lazy`**

再默认情况下，v-model 在每次 `input` 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 `lazy` 修饰符，从而转变为使用 `change` 事件进行同步：

```
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg" >
```

**`.number`**

如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符：

```
<input v-model.number="age" type="number">
```

如果这个值无法被 `parseFloat()` 解析，则会返回原始的值。

**`.trim`**

如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符：

```
<input v-model.trim="msg">
```
