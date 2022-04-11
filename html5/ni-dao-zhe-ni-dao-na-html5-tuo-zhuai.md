# 你到这，你到那——HTML5-拖拽

拖放（drag && drop）在我们平时的工作中，经常遇到。它表示我们日常的抓取和拖放到另一个位置的过程。**常用于各种拖动操作中**

### 创建元素

首先我们需要创建可拖动元素

```
<div id="drag" draggable="true"></div>
```

### 拖拽相关的事件

在元素进行拖拽中会触发各种事件，

* 被拖拽元素拖拽开始时，会触发`dragstart`事件
* 被拖拽元素拖拽完成时，会触发`dragend`事件
* 当被拖拽元素进入到目标元素时时，会触发`dragenter`事件
* 被拖拽元素在目标元素上移动时，会触发`dragover`事件
*   被拖拽元素在目标元素上同时鼠标放开时，会触发

    ```
    drop
    ```

    事件

    * **需要阻止 `dragover` 的默认行为才会触发 `drop` 事件**

### DragEvent 事件对象

* 传值
  * `e.dataTransfer.setData('name',e.target.id)`
* 取值
  * `e.dataTransfer.getData('name')`

### 代码示例

```
<style>
  .drop{
    width: 100px;
    height: 100px;
    background-color: #8c38f2;
    margin-bottom: 50px;
  }
  .target{
    width: 200px;
    height: 200px;
    background-color: rgb(128, 131, 128)
  }
</style>

<p class="info">这里显示开始和结束信息</p>
<p class="going">这里显示正在进行的信息</p>

<div class="drop" draggable="true">我是可以拖动的元素</div>
<div class="target">我是目标元素</div>
  
<script>
  const drop = document.querySelector('.drop')
  const target = document.querySelector('.target')
  const info = document.querySelector('.info')
  const going = document.querySelector('.going')

  drop.addEventListener('dragstart',function (e) {
    // 传值
    e.dataTransfer.setData('name','这是我给你传的值')
    info.innerHTML = '被拖拽元素开始拖动'
  })

  drop.addEventListener('dragend',function () {
    info.innerHTML = '被拖拽元素拖动完成'
    console.log('被拖拽元素拖动完成');
  })

  drop.addEventListener('drag',function () {
    going.innerHTML = '被拖拽元素正在被拖动'
  })

  target.addEventListener('dragenter',function () {
    info.innerHTML = '被拖拽元素进入目标元素'
  })

  target.addEventListener('dragover',function (e) {
    going.innerHTML = '被拖拽元素在目标元素上移动'
    e.preventDefault()
  })

  target.addEventListener('dragleave',function () {
    info.innerHTML = '被拖拽元素离开目标元素'
  })

  target.addEventListener('drop',function (e) {
    e.preventDefault()    
    // 获取到传过来的值
    let returnInfo = e.dataTransfer.getData('name')
    console.log(returnInfo)
    info.innerHTML = '被拖拽元素在目标元素上放下'
    // 通过控制台显示信息判断拖放完成和在目标元素上放下两个事件的先后顺序
    console.log('被拖拽元素在目标元素上放下'); 
  })
</script>
```

[![nQb1o9.gif](https://s2.ax1x.com/2019/09/07/nQb1o9.gif)](https://imgchr.com/i/nQb1o9)

### 总结

原生 HTML5 拖拽API，drag & drop 在实际开发中会在很多情况下都会遇到，比如我们常见的**拖拽上传文件**，**商品拖拽添加购物车等**。这些案例我们下次再进行讨论。
