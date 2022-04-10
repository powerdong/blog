# 多图片加载——懒加载详解

### 懒加载

由于过多的图片会严重影响网页的加载速度，并且移动网络下的流量消耗巨大，所以说延迟加载几乎是标配了。

#### 懒加载原理

图片懒加载的原理很简单，就是我们先设置图片的`data-set`属性（当然也可以是其他任意的，只要不会发送 http 请求就行了，作用就是为了存取值）值为其图片路径，由于不是`src`，所以不会发送 http 请求。 然后我们**计算出页面 scrollTop 的高度和浏览器的高度之和， 如果图片举例页面顶端的坐标 Y（相对于整个页面，而不是浏览器窗口）小于前两者之和，就说明图片就要显示出来了**（合适的时机，当然也可以是其他情况），这时候我们再将 `data-set` 属性替换为 `src` 属性即可。

#### 手动实现一个懒加载

```javascript
<div class="top">上边</div>
<img class="lazyload" data-src="https://s2.ax1x.com/2019/11/18/MsHSZ8.png" />
<img class="lazyload" data-src="https://s2.ax1x.com/2019/11/18/MsHSZ8.png" />
...
<div class="bottom">下边</div>
class StartLazy {
  constructor(defaultImg, timeout) {
    this.defaultImg = defaultImg
    this.timeout = timeout || 500
  }
  /**
   * 初始化
   * 或得到全部需要懒加载的图片资源
   * 设置上默认要显示的图片
   * 进行懒加载
   */
  init() {
    this.imgs = [...document.querySelectorAll('img[data-src]')]
    // 设置默认图片
    this.setDefaultImgs()
    // 懒加载所有
    this.loadAllImgs()
    const self = this
    let timer = null
    document.body.onscroll = function (params) {
      if (timer) {
        clearTimeout(timer)
      }
      timer = setTimeout(() => {
        self.loadAllImgs()
      }, self.timeout);
    }
  }

  /**
   * 设置上默认要显示图片
   */
  setDefaultImgs() {
    if (!this.defaultImg) {
      // 如果没有设置默认图片
      return
    }
    const imgs = this.imgs
    for (let i = 0; i < imgs.len; i++) {
      const img = imgs[i];
      img.src = defaultImg
    }
  }

  /**
   * 加载所有的图片
   */
  loadAllImgs() {
    const imgs = this.imgs
    for (let i = 0; i < imgs.length; i++) {
      const img = imgs[i]
      // 判断当前图片是否需要加载
      if (this.loadImg(img)) {
        // 去除掉已经加载的图片
        imgs.splice(i, 1)
        i--
      }

    }
  }

  /**
   * 懒加载一张图片
   * 判断是否应该加载
   */
  loadImg(img) {
    // 判断该图片是否能够加载，判断图片是否在可视区域内
    const rect = img.getBoundingClientRect()
    if (rect.bottom <= 0) {
      return false
    }
    if (rect.top >= document.documentElement.clientHeight) {
      return false
    }

    img.src = img.dataset.src

    // 判断是否有原图(大图)  进行预加载
    if (img.dataset.original) {
      // 等待图片加载完成
      img.onload = () => {
        img.src = img.dataset.original
        // 加载完成后清除事件
        img.onload - null
      }
    }
    return true
  }
}

// 使用
const lazyLoad = new StartLazy('loading.gif', 300)
lazyLoad.init()
```
