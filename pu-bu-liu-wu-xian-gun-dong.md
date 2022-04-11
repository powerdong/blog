# 瀑布流(无限滚动)

### 瀑布流

> 瀑布流:又称瀑布流式布局。是比较流行的一种网站页面布局，视觉表现为参差不齐的多栏布局，随着页面滚动条向下滚动，这种布局还会不断加载数据块并附加至当前尾部。

![瀑布流](https://www.w3cplus.com/sites/default/files/blogs/2017/1704/pinterest-masonry.png)

我们要创建一个瀑布流插件，尽量的可以再多处使用。

#### 开始布局

首先先创建好一个外层标签，用来包裹我们的图片元素

```
<div id="water"></div>
```

所有的瀑布流元素都会存放到这个`div`标签中 在添加图片时需要动态设置新图片的位置，由于图片高度不定，这里采用绝对定位方式布局。

```
#water {
  position: relative;
}
```

#### 插件创建

我们的想法是要创建一个插件，这里给插件起一个名字`createWaterFall`，这里想要传入参数，首先需要传入外层包裹元素`areaDom`，还需要传入图片组的路径`urls`，还有每张图片的宽度`everyWidth`。

```
function createWaterFall(areaDom, urls, everyWidth) {}
```

**创建图片 dom 对象**

这里我首先想到的是通过遍历传入的图片组路径`urls`来创建图片 dom

```
function createImgDoms() {
  // 循环遍历urls
  for (var i = 0; i < urls.length; i++) {
    var url = urls[i];
    // 动态创建img dom元素
    var img = document.createElement("img");
    // 将图片url赋值给dom元素
    img.src = url;
    // 图片的宽度是固定的
    img.style.width = everyWidth + "px";
    // 图片需要绝对定位
    img.style.position = "absolute";
    // 创建完成后将图片添加到包裹元素中
    areaDom.appendChild(img);
  }
}
```

经过上边这样折腾，我们成功的将一批图片添加到页面中，不过位置有些不对，都跑到了左上角，接下来我们需要再写一个函数用来设置图片的位置。

```
function setImgPosition() {}
```

在写这个函数时我又发现我需要得到图片要展示几列，于是我又写一个函数用来计算当前窗口可以展示几列图片。

**计算图片展示列数**

```
function cal() {
  // 我们要计算有多少列
  // colNumber = 容器的宽度 / 图片的宽度
  var containerWidth = parseInt(areaDom.clientWidth);
  var colNumber = parseInt(containerWidth / everyWidth);
  // 我们将剩余空间平分为每列的间距
  var space = containerWidth - colNumber * everyWidth;
  var gap = space / (colNumber + 1);
  // 这个函数中我们成功得到了当前窗口可以展示多少列，也得到了每列的间隙
}
```

**设置图片坐标**

```
function setImgPosition() {
  cal();
  // 这里需要用到列数和间隙，所以我选择将 colNumber 和 gap 提升
  // 这里存放的是每一列的下一个图片的Y坐标
  var colY = new Array(colNumber);
  // 初始时将他们置为0
  colY.fill(0);

  // 遍历所有的图片元素
  for (var i = 0; i < areaDom.children.length; i++) {
    var img = areaDom.children[i];
    // 找到每列中的最小值  也就是最短的那一列
    var y = Math.min(...colY); //y坐标
    // 第几列
    var index = colY.indexOf(y);
    // x坐标 = 列数 + 1 * 间隙 + 列数 * 图片的宽度
    var x = (index + 1) * gap + index * everyWidth;
    // 图片设置位置
    img.style.left = x + "px";
    img.style.top = y + "px";
    // 更新数组 将这一列的高度更新
    colY[index] += parseInt(img.height) + gap;
  }
  // 找到数组中最大的数字
  var height = Math.max(...colY);
  // 外层Dom需要更新为最长的那个长度
  areaDom.style.height = height + "px";
}
```

### 无限滚动

> 这里继续做了无限滚动的功能，通过不断的获取数据，将图片渲染到页面

#### 获取图片

```
function fetchDatas(callback) {
  $.ajax({
    url: "",
    success: function(data) {
      // 定义一个变量  表示要显示几页数据
      page++;
      if (page === 6) {
        // 没有数据了
        callback([]);
        return;
      }
      callback(data);
    }
  });
}
```

#### 请求数据

```
function getImgUrls() {
  // 刚开始请求时判断是否还有更多的数据
  // 判断是否正在请求数据，如果正在请求，则不多次请求
  // 该函数某个时间只能运行一次
  if (!hasMoreData || isLoading) {
    // 已经没数据了 或 正在获取数据
    return;
  }
  isLoading = true;
  // 显示刷新圈  利用CSS动画实现一个刷新圈元素
  loading.style.display = "block";
  // 从这里获取数据
  fetchDatas(function(urls) {
    // 这里创建数据
    createImgDoms(urls);
    // 隐藏刷新圈
    loading.style.display = "none";
    // 如果没有拿到数据
    if (urls.length === 0) {
      // 标记没有数据
      hasMoreData = false;
      // 显示没有数据的 div  这个div内有一行文本 没有数据了...
      noData.style.display = "block";
    }
    // 结束后将锁关闭
    isLoading = false;
  });
}
```

### 完整的代码

```
// 还有没有更多的数据
var hasMoreData = true;
// 是否正在获取数据
var isLoading = false;
//当前获取的是第几页的数据
var page = 0;
var div = document.getElementById("water");
/**
 * 创建一个瀑布流插件
 * @param {*} areaDom 容器
 * @param {*} urls 图片的url数组
 * @param {*} everyWidth 每张图片的宽度
 */

function createWaterFall(areaDom, urls, everyWidth) {
  // 有多少列
  var colNumber;
  // 每列间隙
  var gap;

  /**
   * 创建图片的dom对象
   */
  function createImgDoms() {
    for (var i = 0; i < urls.length; i++) {
      var url = urls[i];
      var img = document.createElement("img");
      img.src = url;
      img.style.width = everyWidth + "px";
      img.style.position = "absolute";
      // 图片加载完成后设置位置 显示
      img.onload = function() {
        setImgPosition();
      };
      areaDom.appendChild(img);
    }
  }

  /**
   * 计算
   */
  function cal() {
    // 有多少列？
    // colNumber = 容器的宽度 / 图片的宽度
    var containerWidth = parseInt(areaDom.clientWidth);
    colNumber = parseInt(containerWidth / everyWidth);
    // 剩余空间  列间距
    var space = containerWidth - colNumber * everyWidth;
    gap = space / (colNumber + 1);
  }

  /**
   * 设置每张图片的坐标
   */
  function setImgPosition() {
    cal();
    // 存放的是，每一列的下一个图片的Y坐标
    var colY = new Array(colNumber);
    // 将数组的每一项填充为0
    colY.fill(0);

    for (var i = 0; i < areaDom.children.length; i++) {
      var img = areaDom.children[i];
      // 找到colY中的最小值
      var y = Math.min(...colY); //y坐标
      // x坐标
      // 第几列
      var index = colY.indexOf(y);
      var x = (index + 1) * gap + index * everyWidth;
      img.style.left = x + "px";
      img.style.top = y + "px";

      // 更新数组
      colY[index] += parseInt(img.height) + gap;
    }
    // 找到数组中最大的数字
    var height = Math.max(...colY);
    areaDom.style.height = height + "px";
  }

  /**
   * 改变窗口大小
   */
  var timer = null;
  window.onresize = function() {
    if (timer) {
      clearTimeout(timer);
    }
    timer = setTimeout(function() {
      setImgPosition();
    }, 500);
  };
  setImgPosition();
  createImgDoms();
}

function fetchDatas(callback) {
  $.ajax({
    url: "",
    success: function(data) {
      // 定义一个变量  表示要显示几页数据
      page++;
      if (page === 6) {
        // 没有数据了
        callback([]);
        return;
      }
      callback(data);
    }
  });
}

function getImgUrls() {
  // 刚开始请求时判断是否还有更多的数据
  // 判断是否正在请求数据，如果正在请求，则不多次请求
  // 该函数某个时间只能运行一次
  if (!hasMoreData || isLoading) {
    // 已经没数据了 或 正在获取数据
    return;
  }
  isLoading = true;
  // 显示刷新圈  利用CSS动画实现一个刷新圈元素
  loading.style.display = "block";
  // 从这里获取数据
  fetchDatas(function(newUrls) {
    // 这里创建数据
    createWaterFall(areaDom, newUrls, evertWidth);
    // 隐藏刷新圈
    loading.style.display = "none";
    // 如果没有拿到数据
    if (urls.length === 0) {
      // 标记没有数据
      hasMoreData = false;
      // 显示没有数据的 div  这个div内有一行文本 没有数据了...
      noData.style.display = "block";
    }
    // 结束后将锁关闭
    isLoading = false;
  });
}

window.onscroll = function() {
  // 距离底部的距离
  var bottom =
    document.documentElement.scrollHeight -
    document.documentElement.scrollTop -
    document.documentElement.clientHeight;

  if (bottom <= 60) {
    getImgUrls();
  }
};

// 开局调用 获取图片
getImgUrls();
```
