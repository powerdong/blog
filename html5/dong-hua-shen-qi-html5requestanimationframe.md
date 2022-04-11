# 动画神器——HTML5-requestAnimationFrame

在 Web 应用中，实现动画效果的方法比较多，JavaScript 中可以通过定时器 setTimeout 来实现，Css3 可以使用 transition 和 animation 来实现，HTML5 中的 canvas 也可以实现。除此之外，HTML5 还提供了一个专门用于请求动画的 API，即 `requestAnimationFrame(rAF)` 顾名思义就是请求动画帧。

### 屏幕绘制频率

**屏幕绘制频率**即图像在屏幕上更新的速度，也是屏幕上图像每秒钟出现的次数，它的单位是赫兹(HZ),对于一般的笔记本电脑，这个频率大概是60HZ。因此，**当你对着电脑屏幕什么也不做的情况下，显示器也会以以每秒60次的频率在不断地更新屏幕上的图像**，那么我们为什么感觉不到这个变化呢？那是因为人的眼睛有 [**视觉停留效应**](https://baike.baidu.com/item/%E8%A7%86%E8%A7%89%E6%9A%82%E7%95%99/5125149?fr=aladdin) 即前一刻画面留在大脑印象还没消失，紧接着后幅画面就跟上来了，这中间只隔了16.7ms(1000/60≈16.7)，所以会让你误以为屏幕上的图像是静止不动的。

### 原理

`window.requestAnimationFrame()`告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。该方法需要传入一个毁掉函数作为参数，该回调函数会在浏览器下次重绘之前执行。

该函数有返回值，建议使用一个ID进行标识，没有别的意义，可以传给`window.cancelAnimationFrame()`取消回调函数

#### 总结

1. 页面刷新前执行一次
2. 1000ms 60fps -> 16ms
3. `cancelAnimationFrame` 用来取消
4. 用法和 setTimeout 类似
5. 兼容性查看[caniuse](https://caniuse.com/#search=requestAnimationFrame)

> 若你想在浏览器下次重绘之前继续更新下一帧动画，那么回调函数自身必须再次调用window.requestAnimationFrame()

### 兼容

对于不同的浏览器，requestAnimationFrame 这个api的名字也略有差异，针对低版本浏览器，这里写出对应的兼容性写法封装。

```
// requestAnimationFrame 的封装
/**
 * 针对不同内核的浏览器的API写法不同进行兼容
 */
window.requestAnimaFrame = (function(){
  return window.requestAnimationFrame ||
        window.webkitRequestAnimationFrame ||
        window.mozRequestAnimationFrame ||
        function (callback) {
          window.setTimeout(callback, 1000 / 60)
        }
})();
// cancelAnimationFrame
/**
 * 针对不同内核的浏览器的API写法不同进行兼容
 */
window.cancelAnimFrame = (function(){
  return window.cancelAnimationFrame ||
        window.webkitCancelAnimationFrame ||
        window.mozCancelAnimationFrame ||
        function (id) {
          window.clearTimeout(id)
        }
})
```

#### 代码示例

```
<div id="a" style="width: 100px; 
            height: 100px;
            background-color: red;
            position: absolute;"
            >
</div>

<script>
/**
 * 定义一个div盒子，动画设置向右平移 500px
 * 
 * 注：这里使用的requestAnimaFrame和cancelAnimFrame方法名为自己封装的方法
 */
let start = 0,
    end = 500,
    ele = document.querySelector('#a'),
    req;
let f = function () {
  start += 10
  ele.style.left = start + 'px'
  // 距离页面左边的距离
  let left = ele.getBoundingClientRect().left
  if(left < end) {
    req = requestAnimaFrame(f)
  } else {
    cancelAnimFrame(req)
  }
}
</script>
```

![nF8UJS.gi](https://s2.ax1x.com/2019/09/03/nF8UJS.gif)
