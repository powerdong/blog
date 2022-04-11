---
cover: https://s2.ax1x.com/2019/11/24/ML33fs.png
coverY: 0
---

# 动起来，这样比较炫—— CSS3 动画

### Css3 动画

* transition
  * transition-property 规定设置过渡效果的 CSS 属性的名称。
  * transition-duration 规定完成过渡效果需要多少秒或毫秒。
  * transition-timing-function 规定速度效果的速度曲线。
  * transition-delay 定义过渡效果何时开始。
*   animation

    属性可以像 Flash 制作动画一样，通过控制关键帧来控制动画的每一步，实现更为复杂的动画效果。

    * animation 实现动画效果主要由两部分组成：
      * 通过类似 Flash 动画中的帧来声明一个动画；
      * 在 animation 属性中调用关键帧声明的动画。
* translate 3D 建模效果

#### transform 形状变换

> CSS3 转换可以可以对元素进行移动、缩放、转动、拉长或拉伸。

*   ```
    transform
    ```

    可以实现元素的形状，角度，位置等的变化。

    *   ```
        rotate(ndeg)
        ```

        以 x/y/z 为轴进行选装，默认为 z

        * `rotateX(ndeg)`
        * `rotateY(ndeg)`
        * `rotateZ(ndeg)`
        * `rotate3d(x, y, z, angle)`
    *   ```
        scale(n)
        ```

        以 x/y 为轴进行缩放

        * `scale(x, y)` 接受两个值，如果第二个参数未提供，则第二个参数使用第一个参数的值
        * `scaleX(n)`, `scaleY(n)` 值是数字表示倍数，不加任何单位
        * `scaleZ(n)`
        * `scale3d(x, y, z)`
    *   ```
        skew(ndeg)
        ```

        对元素进行倾斜扭曲

        * `skew(x, y)` 接受两个值，第一个参数对应 X 轴，第二个参数对应 Y 轴。如果第二个参数未提供，则默认值是 0
        * `skewX()`, `skewY()`
    *   ```
        translate()
        ```

        可以移动的距离，相对于自身位置

        * `translate(x,[y])`
        * `translateX()`
        * `translateY()`
        * `translateZ()`
        * `translate3d(x, y, z)`
    *   ```
        transform-origin
        ```

        变换原点

        * 任何一个元素都有一个中心点，默认情况下，其中心点是居于元素 X 轴和 Y 轴的 50% 处。
        * 值用百分比或关键字(top center)表示
    *   ```
        transform-style
        ```

        : 指定嵌套元素是怎样在三维空间中呈现

        * flat 2D 平面呈现
        * preserve-3d 3D 空间中呈现
        * **使用此属性必须先使用 `transform` 属性**
        * **需要设置在父元素中，高于任何嵌套的变形元素**
        * **设置了 `transform-style: preserve-3d` 的元素，就不能防止子元素溢出设置 `overflow: hidden`；否则会导致 preserve-3d 失效**

**代码示例**

```
<div></div>
div {
  width: 100px;
  height: 100px;
  margin: 100px auto;
  background: #f40;
  transform: rotate(-37deg);
  /* transform: skew(7deg); */
  /* transform: scale(1.5); */
  /* transform: translate(-32px); */
  /* transform-origin: bottom; */
}
```

![mlskMd.gif](https://s2.ax1x.com/2019/08/19/mlskMd.gif)

#### transition 过渡

![ML3BtJ.png](https://s2.ax1x.com/2019/11/24/ML3BtJ.png)

> CSS3 中，我们为了添加某种效果可以从一种样式转变到另一个的时候，无需使用 Flash 动画或 JavaScript。

`transition` 属性是 Css3 的一个复合属性，主要包括以下几个子属性

* `transition-property`: 指定过渡或动态模拟的 Css 属性，默认为 all
* `transition-duration` : 指定过渡所需要的时间
* `transition-timing-function` : 指定过渡函数
* `transition-delay` : 指定开始出现的延迟时间

**`transition` 过度动画可以参与的过度的属性** ![mlc8Ld.png](https://s2.ax1x.com/2019/08/19/mlc8Ld.png) **`transition` 过渡函数可以选择的值** ![mlctot.png](https://s2.ax1x.com/2019/08/19/mlctot.png)

**代码示例**

```
<div></div>
div {
  width: 100px;
  height: 101px;
  margin: 10px auto;
  background: #f40;
  transform: translateX(100px);
  transition: transform 1s 1s;
}
```

![ml2Fb9.gif](https://s2.ax1x.com/2019/08/19/ml2Fb9.gif)

#### animation 动画

![ML3T1I.png](https://s2.ax1x.com/2019/11/24/ML3T1I.png)

> CSS3 可以创建动画，它可以取代许多网页动画图像、Flash 动画和 JavaScript 实现的效果。

*   ```
    @keyframes
    ```

    name {0% ; 100% ;}

    * 创建动画
    * 指定一个 CSS 样式和动画将逐步从目前的样式更改为新的样式
*   ```
    animation
    ```

    动画，为 Css3 的复合属性，主要包括以下属性

    *   ```
        animation-name
        ```

        此属性未执行动画的 keyframes 名

        * 当在 `@keyframes` 创建动画，把它绑定到一个选择器，否则动画不会有任何效果。
    *   ```
        animation-duration
        ```

        动画执行时间

        * 必须定义动画的名称和动画的持续时间。如果省略的持续时间，动画将无法运行，因为默认值是 0。
    * `animation-time-function` 过渡函数速率(默认是 `ease`)
    * `animation-delay` 动画延迟时间 默认是 0
    *   ```
        animation-direction
        ```

        :

        * normal 默认值
        * reverse 反向播放
        * alternate 动画在奇数次正向播放，在偶数次反向播放
        * alternate-reverse 动画在奇数次反向播放，在偶数次正向播放
    *   ```
        animation-iteration-count
        ```

        : 规定动画播放次数，默认是 1

        * number 一个数字，定义应该播放多少次动画
        * infinity 动画无限次播放
    *   ```
        animation-fill-mode
        ```

        : 当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。

        * none 默认值
        * forwards 表示动画在结束后继续应用最后的关键帧的位置
        * backwards 会在向元素应用动画样式时迅速应用动画的初始帧（结合延迟 1s 来看）
        * both 动画遵循 forwards 和 backwards 的规则。
    *   ```
        animation-play-state
        ```

        :动画是否正在运行或已暂停。

        * paused 暂停动画
        * running 正在运行的动画

**代码示例**

```
<div></div>
div {
  width: 100px;
  height: 100px;
  margin: 10px auto;
  background: #f40;
  transform: translateX(100px);
  /* animation: name duration timing-function delay iteration-count direction fill-mode; */
  animation: transform 2s ease-in 1s 3 normal none;
}
@keyframes transform {
  0% {
    transform: translateX(100px);
  }
  100% {
    transform: translateX(0px);
  }
}
```

![ml4C7Q.gif](https://s2.ax1x.com/2019/08/19/ml4C7Q.gif)

#### perspective 景深

![ML3qnf.png](https://s2.ax1x.com/2019/11/24/ML3qnf.png)

> perspective 指定了观察者与 z=0 平面的距离，使具有三维位置变换的元素产生透视效果。 z>0 的三维元素比正常大，而 z<0 时则比正常小，大小程度由该属性的值决定。

可以简单的把 `perspective` 理解成人距离显示器的距离，此值越大的效果越差，越小效果越好。

假设你距离 100 米和 1 米的距离去看一个边长一米的正方体。

**`perspective` 的值要大于 3d 物体的值**

* number 元素距离视图的距离，以像素计
* none 默认值。与 0 相同，不设置透视

在 3D 变形中，除了 `perspective` 属性可以激活一个 3D 空间之外，在 3D 变形的函数中的 `perspective()` 也可以激活 3D 空间。

* 它们不同的地方是
  * perspective 用在舞台元素(变形元素们的共同父级)
  * perspective() 就是用在当前变形元素上，并且可以与其他的 transform 函数一起使用

```
.stage {
  perspective: 600px;
}

.stage .box {
  transform: perspective(600px);
}
```

\*\*注意:\*\*当有多个变形元素时，第一种只有一个透视点，第二种每一个变形元素都有一个透视点。

在元素运动过程中，`backface-visibility: visible | hidden` 控制能否展示元素的背面

3d 旋转 —— 我们转的不只是元素本身，元素在转，坐标轴其实也在转

> 两个相同的元素，其中一个设置了 `rotateY(180deg)`，然后同时设置 `translateZ(100px)`；这时，他们在空间的距离是 200px，而不是 0

[CSS3 动画专题](http://www.aseoe.com/special/webstart/css3\_animation/)

#### Css 动画性能优化

**尽可能多的利用硬件能力，如使用 3D 变形来开启** [**GPU**](https://baike.baidu.com/item/%E5%9B%BE%E5%BD%A2%E5%A4%84%E7%90%86%E5%99%A8/8694767?fromtitle=gpu\&fromid=105524) **加速**

```
-webkit-transform: translate3d(0, 0, 0);
-moz-transform: translate3d(0, 0, 0);
-ms-transform: translate3d(0, 0, 0);
transform: translate3d(0, 0, 0);
```

\*\*注意：\*\*如果动画过程有闪烁(通常发生在动画开始的时候)，可以尝试

```
-webkit-backface-visibility: hidden;
-moz-backface-visibility: hidden;
-ms-backface-visibility: hidden;
backface-visibility: hidden;
-webkit-perspective: 1000;
-moz-perspective: 1000;
-ms-perspective: 1000;
perspective: 1000;
```

一个元素通过 `transform3d` 右移 500px 的动画流畅度会明显优于使用 left 属性

> 3D 变形会消耗更多的内存与功耗，应确实有性能问题时才去使用它，兼在权衡

**尽可能少的使用 `box-shadows` 与 `gradients`**

这两个都是页面性能杀手，能避免尽量避免

**尽可能的让动画元素不在文档流中，以减少重排**

```
position: fixed;
position: absolute;
```

**优化 DOM reflow**
