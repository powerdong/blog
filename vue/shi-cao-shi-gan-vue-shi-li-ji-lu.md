# 实操实干——vue实例记录

初出茅庐摸不着头脑

vue 的入门学习阶段告一段落了，刚刚学完的 vue 还热乎着，趁着这个热乎劲随即找了一个 vueDemo 进行练习。然而没有什么头绪，然后阅读了作者的源码，重新到官网上看了相关的例子和介绍，感觉自己目前立马需要做一个例子来练练手。

### 寻找资源

> 如果你想看实例源代码可以移步[我的第一个 Vue 实例 git 仓库](https://github.com/powerdong/vuePractice)

然后到慕课网找相关的 vue 项目教学，找到了一个基于 `Vue2.5 开发的去哪儿网 App`，看了项目介绍和评价，感觉适合现阶段的我去练习。

随机立马跟随老师的节奏，进行重学 vue，认真学习每一节的课程，记录相关的笔记，又重新看了相关的文档，第二次看和第一次看确实有些不同的见解。经过几天的学习，基础入门阶段已经完成，进入课程的第二阶段，就是项目实践阶段。

### 着手实践

首先进行项目环境搭建，这里采用 vue 官方提供的 `vue-cli` 这个脚手架工具，用的默认 `webpack` 打包工具，初始化了本地项目。

> 如果没有安装 `node` 的话需要到 `node` 官方下载安装。

成功安装完 node 后，执行下面命令进行全局安装这个包

> ```
> npm install -g vue
> ```

安装之后，你就可以在命令行中访问 `vue` 命令。你可以通过简单运行 `vue`，看看是否展示出了一份所有可用命令的帮助信息，来验证它是否安装成功。

> `vue -V` 可以查看当前 vue 版本号

打开你创建的项目文件夹，在命令行输入(意思是使用 webpack 打包工具初始化’demo’这个项目包)

> ```
> vue init webpack demo
> ```

初始化完成后我们就可以在 src 目录下进行编写相关的代码

之后建议先在 git 上面创建远程仓库，进行远程与本地连接，用来版本控制，也可以较安全的编辑代码，防止因为出错还可以使用 git 的回流功能返回之前的代码。

#### 文件夹的解释

配置完相关的配置后，老师进行了项目目录的解释，对不同文件的内容进行了相关的解释

* 项目的编写在 src 文件夹下面进行
* 项目由 `index.html` 进行显示
* `router` 文件夹用来配置路由相关的配置
* 在 `build` 文件夹中有一个 `webpack.base.conf.js` 文件里边 `alias` 属性可以自定义文件路径的简写形式

因为我自己使用的是 less 语法，在编译过程中报错，提醒我需要安装相关的配置 `loader`，所以我安装了 `less` 和 `less-loader` 两个个配置项

> ```
> npm install less less-loader --save
> ```

这次开发的是一个移动端的页面，在 `index.html` 中的 `<meta name="viewport">` 的标签中添加了`minimum-scale=1.0,maximum-scale=1.0,er-scalable=no"`配置，使**用户在访问页面时不能进行放大和缩小**。

* 引入了重置页面的`reset.css`文件
* 在移动端开发时出现的`1px border`问题引入了`border.css`文件
* 增加了项目所需要的字体文件。

#### 正式开发

> 接下来就是正式的开发了，项目采用页面分组件进行构建开发。

首先需要创建一个首页文件，因为这次项目的页面较多，所以在目录下创建了 `pages` 文件夹来存放各个页面。 在 `pages` 文件夹下创建了 `home` 文件夹，用来存放主页的相关组件，创建一个父组件 `Home.vue`。通过父组件来引入多个子组件来管理页面。 创建好了父组件后，在组件中定义组件的名字，然后需要在 `router` 文件夹下的 `index.js` 文件中配置。

1.  引入`vue`和`vue-router`

    ```
    import Vue from 'vue'
    import Router from 'vue-router'
    ```
2. 使用`router`(**`Vue.use(Router)`**)
3. 引入父组件(**`import Home from '@/pages/home/Home'`**)
4.  然后在配置中设置在根目录`'/'`下展示主页组件，定义要展示组件的名字

    ```
    path: '/',
    name: 'Home',
    component: Home
    ```

    测试成功后就开始进行页面的编写。

通过在主页父组件 `Home.vue` 中引入不同的组件然后再将子组件插到父组件的展示区域中进行展示。这里通过创建`components`文件夹用来管理使用自定义的组件。

* 父组件使用(**`import moduleName from 'module';`**)方式进行引入组件
* 在父组件要使用子组件，需要声明`components:{HomeHeader}`
* 然后就可以在展示区写入自定义组件的标签名了
  * 这里需要注意 `HTML` 中的特性名是大小写不敏感的，所以浏览器会把所有的大写字符解释为小写字符。
  * 这意味着在使用 `DOM` 中的模板时，`camelCase` 的 `prop` 名需要使用其等价的 `kebab-case` 命名。
* 引入子组件后在子组件中编写相关组件的页面和样式

> 在子组件中编写样式时，为了防止子组件样式对全局的污染，可以在`<style>`标签中加入`scoped`键值(**`<style lang="less" scoped></style>`**),可以防止污染情况的发生。

在编写页面的背景颜色时，由于想到页面的主题色可能多次使用，随即在样式文件夹下创建了一个全局样式。 由于样式文件夹引用需要跳到其他路径下，防止页面的阅读体验，所以在配置文件中设置了自定义的路径选择方式。

> 在引入外部的 less 文件时需要注意需要在 `url` 地址前面加上`~`就是这个样子(**`@import '~styles/global.less';`**)这样才能编译正确，不会报错。

#### Vue 的轮播组件

首页中的轮播图样式使用了`Vue-Awesome-Swiper`组件。

首先是进行安装

> ```
> npm install vue-awesome-swiper --save
> ```

使用的话需要进行引入，由于想到可能会多个页面都会这个组件，索性将组件引入到`main.js`文件中。

> ```
> import VueAwesomeSwiper from 'vue-awesome-swiper'`
> `import 'swiper/dist/css/swiper.css'`
> `Vue.use(VueAwesomeSwiper)
> ```

在需要轮播的位置使用`Vue-Awesome-Swiper`提供的模板

```
<swiper :options="swiperOption" ref="mySwiper" @someSwiperEvent="callback">
  <!-- slides -->
  <swiper-slide>I'm Slide 1</swiper-slide>
  <div class="swiper-pagination" slot="pagination"></div>
</swiper>
export default {
  name: 'carrousel',
  data() {
    return {
      swiperOption: {
        pagination: '.swiper-pagination',
        // 是否循环滚动
        loop: true
      }
    }
  }
}
```

在设置图片索引的小圆点样式时遇到的问题就是类名也已经加上了 ，但是样式就是没有生效，最后发现原因的产生竟然是`<style scoped></style>`中`scoped`属性。

> 该属性的作用是用来绑定当前样式不被污染，同时也就意味着在创建新的元素后，该元素样式是进入不到 style 总查找样式的。

了解到了 vue 中的**样式穿透**，也是解决样式问题的常用方式之一。sass/less 使用样式穿透的方式可能会有所不同。

```
/* stylus的样式穿透 使用>>> */

外层 >>> 第三方组件 

.wrapper >>> .swiper-pagination-bullet-active
background: #fff

/* sass和less的样式穿透 使用/deep/ */

外层 /deep/ 第三方组件 {
}
.wrapper /deep/ .swiper-pagination-bullet-active {
  background: #fff;
}
```

这样就可以正确的显示样式了。

#### 父子组件通信

结束了主页的编写后需要进行 ajax 访问服务器得到数据进行动态渲染。`Vue`建议使用`axios`

*   我们需要首先安装

    ```
    axios
    ```

    这个包。

    > `npm install axios`
*   安装完成后引入包。我们可以只在父组件使用 ajax 进行通信，得到数据后向子组件传值。

    > `import axios from 'axios';`

在调用`axios`建议使用`mounted`这个生命周期钩子函数。**它是在页面渲染完成时执行**

```
methods:{
    getInfo(){
        axios.get('url',{
            传入参数
            params:{
                key:value
            }
        })
        .then(this.successCallback)
    },
    successCallback(res){
        console.log(res);
    }
}

mounted(){
    this.getInfo();
}
```

通过`axios`成功获取到数据后将本地数据修改。

**父子组件之间传值**

进行父子间通信时父组件向子组件传值，子组件进行接收

```
//父组件传值
<children :childData="fatherData"></children>

data(){
    return{
        fatherData:[]
    }
}

// 子组件接收可定义类型 使用
props:{
    childData:{
        // 类型
        type:String,
        // 默认值
        default:"hello",
        // 是否必填
        required:true
    }
}
```

**父子组件传事件**

```
<!-- 子组件定义真实事件 -->
<button @click="childrenClick"></button>

methods:{ childrenClick(){
<!-- 通过$emit向外传事件，第一个参数是父级使用的名字，第二个参数可以传数据 -->
this.$emit("fatherHandle",value) } }
<!-- 父组件定义事件操作 -->
<children @fatherHandle="fatherClick"></children>

methods:{ fatherClick(){ console.log('触发子组件的点击事件') } }
```

#### 搜索框逻辑

> 通过`v-model`实现对`input`搜索框内数据的双向绑定。

在`Vue`中的`watch`监听数据的每次变化，这里使用`watch`侦听器，监听`input`搜索框内的数据变化。 但是由于输入内容太过频繁，搜索事件调用频繁，对页面性能也有一定的影响，这里使用了**函数防抖机制**。 这里对搜索的内容项注册了点击事件`@click="handleClick"`需要实现点击后跳传到首页。

```
handleClick(){
    this.$router.push("/");
}
```

#### Vuex 使用实现数据通信

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**

*   首先是安装 vuex

    > `npm install vuex`
* 为了更好的管理，我们选择在 src 文件夹下创建一个 store 文件夹存放 vuex 相关的文件。
*   创建好了之后我们在

    ```
    main.js
    ```

    文件中引入创建的 store，而且需要在 vue 实例中使用

    ```
    store
    ```

    > `import store from './store'`
* 这里在引入的 store 文件夹后会自动寻找该目录下的`index.js`文件
* 在`index.js`文件中配置 vuex。

```
import Vue from 'vue';
import Vuex from 'vuex';

export default new Vuex.Store({
    // 存放数据
    state:{
        city:"上海"
    },
    // 定义方法
    mutations:{
        // state存放的是数据  第二个参数是传过来的参数
        changeCity(state,city)
    }
})
```

在 `vue` 组件中通过 `map` 映射关系使用 `vuex`

```
import {mapState, mapMutations} from 'vuex';

computed: {
    ...mapState({
    // 映射 this.currentCity 为 store.state.city
    currentCity:'city'
    })
},

methods: {
      changeState(city){
          this.changeCity(city);
      },
      //将 `this.changeCity()` 映射为 `this.$store.commit('changeCity')`
      ...mapMutations(['changeCity'])
  },
```

#### 在动态组件上使用 keep-alive

在从主页跳转至城市列表页时需要**多次发送 ajax 请求**，这样对服务器是很不友好的，我们可以使用`keep-alive`对动态组件进行包裹，使那些标签的组件实例能够被在它们第一次被创建的时候缓存下来。

我们可以在`App.vue`中将`<router-view />`包裹起来

`<keep-alive>` 包裹动态组件时，**会缓存不活动的组件实例，而不是销毁它们。**

> 这里使用 `deactivated` 钩子函数，不能使用 `beforeDestroy` `destroy`

```
<keep-alive>
  <router-view />
</keep-alive>
```

当你使用了 `keep-alive`生命周期函数中会多出一组钩子函数`activated`、`deactivated`。

通过`activated` 实现在页面加载时时执行，`deactivated`在离开页面时执行，为防止缓存不必要的内容。

#### 动态路由绑定

```
{
    path:'/detail/:id',
    name:'Detail',
    component:Detail
}
```

通过`<router-link :to="'/detail/' + item.id">`实现动态跳转路由

```
{
    path:'/detail',
    name:'Detail',
    component:Detail
}
```

通过 `this.$router.push({ name: 'Detail', params: { id } })` 可以使得地址栏不显示 id 信息，并且可以通过 this.$route.params 获取到值

> 要注意这时候在 path 中不能再写 `/:id`

#### 巧妙使用递归组件

当你需要构建一个文件目录树，可以试着使用递归组件。

实现方法就是在组件内使用自己

```
<template>
  <detail-list :list="item.children"></detail-list>
</template>
```

#### vue 路由懒加载

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

* 首先，可以将异步组件定义为返回一个 Promise 的工厂函数 (该函数返回的 Promise 应该 resolve 组件本身)
  * `const Foo = () => Promise.resolve({ /* 组件定义对象 */ })`
* 第二，在 Webpack 2 中，我们可以使用动态 import 语法来定义代码分块点 (split point)
  * `import('./Foo.vue') // 返回 Promise`

结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件。

> ```
> const Foo = () => import('./Foo.vue')
> ```

在路由配置中什么都不需要改变，只需要像往常一样使用 Foo

```
const router = new VueRouter({
  routes: [{ path: '/foo', component: Foo }]
})
```

这里我尝试使用定义一个函数，通过传入参数方式定义引入组件的位置

> ```
> const _import_ = file => import('@/pages/' + file + 'index')
> ```

这里使用`file`变量，可以在`component`使用子组件时调用`_import_`函数，将文件夹名字传进去。类似这样

```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: _import_('home')
    }
  ]
})
```

#### 真机调试

* windows 用户可以在命令行内输入`ipconfig`
* Mac 用户输入`ifconfig`

找到**IPv4 地址**对应的 IP 地址。(例如 192.168.0.1) 在浏览器地址中输入`192.168.0.1:8080/`

**如果出现无法访问情况可以看下文设置**

默认情况下`npm run dev`并不能实现使用本地 ip 访问项目

可以在`package.json`中的`script`属性中的`dev`中增加`--host 0.0.0.0`类似这个样子:

> ```
> "dev": "webpack-dev-server --inline --host 0.0.0.0 --progress --config build/webpack.dev.conf.js",
> ```

设置后重新在浏览器中访问，正常后可在手机上访问该地址，**不过测试的手机需要和电脑连接在同一局域网下**

#### 低版本手机浏览器不支持 ES6 的 Promise

如果在手机浏览器中访问出现白屏情况，需要考虑是否是低版本浏览器情况下不支持 ES6 特性情况

**解决办法**

*   安装

    ```
    babel-polyfill
    ```

    包

    > `npm install babel-polyfill`
*   在

    ```
    main.js
    ```

    引入

    ```
    babel-polyfill
    ```

    > `import 'babel-polyfill'`
