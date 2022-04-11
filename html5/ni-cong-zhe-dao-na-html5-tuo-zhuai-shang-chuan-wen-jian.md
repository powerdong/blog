# 你从这，到那——HTML5-拖拽上传文件

当我们学习了 HTML 提供的原生拖放(drag & drop)后,是时候想一想这个东西**可以用来作什么**，**可以在什么时候使用**，**使用的场景**等等

### 场景分析

当我们在注册成功一个账户时，一般网站会让我们上传我们的用户头像，或者在实名认证的时候会涉及到身份证图片上传到等，这时候我们可以使用`input`提供的`file`属性进行选择本地文件进行上传。

![实名认证](http://www-faq.stor.sinaapp.com/06ebdd14b6cccb47deb6.png)

我们再想一下，当在电脑端的情况下，当用户打开文件选择框时再寻找图片对应的文件夹，再进行选取文件的时候是不是会有点麻烦呢？我们可不可以让用户找到图片文件，直接引入实现上传呢？答案是可以的。

### 怎么做

经过这些分析后，我们可以尝试使用 HTML5 提供的拖拽，使得目标元素增加读取文件功能，然后使用 ajax 实现图片上传。

谈一谈我们需要使用到的技术:

* **Drag & Drop**: HTML5 基于拖拽的事件机制
* **File API**: 可以很方便的让 Web 应用访问文件对象，File API 包括 FileList、Blob、File、FileReader、URI scheme，本文主要讲解拖拽上传中用到的 FileList 和 FileReader 接口。
* **FormData**: FormData 是基于 XMLHttpRequest Level 2 的新接口，可以方便 web 应用模拟 Form 表单数据，最重要的是它支持文件的二进制流数据，这样我们就能够通过它来实现 AJAX 向后端发送文件数据了。

#### HTML5 拖拽事件

关于 Drag & Drop 拖拽事件，之前我写过一篇专门介绍的文章，[HTML5-拖拽](https://powerdong.github.io/myBlog/2019/09/07/HTML5-%E6%8B%96%E6%8B%BD/),大家有兴趣的话可以点击链接查看，我在这里就不在多啰嗦了\~下面直接出拖拽上传的简要代码示例

```
var oDragWrap = document.body;

//拖进
oDragWrap.addEventListener(
  "dragenter",
  function(e) {
    e.preventDefault();
  },
  false
);

//拖离
oDragWrap.addEventListener(
  "dragleave",
  function(e) {
    dragleaveHandler(e);
  },
  false
);

//拖来拖去 , 一定要注意dragover事件一定要清除默认事件
//不然会无法触发后面的drop事件
oDragWrap.addEventListener(
  "dragover",
  function(e) {
    e.preventDefault();
  },
  false
);

//扔
oDragWrap.addEventListener(
  "drop",
  function(e) {
    dropHandler(e);
  },
  false
);

var dropHandler = function(e) {
  //将本地图片拖拽到页面中后要进行的处理都在这
};
```

#### 获取文件数据 HTML5 File API

File API 中的 FileReader 接口，作为 File API 的一部分，FileReader 专门用来读取文件。我们在这里主要介绍一些 File API 中的 FileList 接口，它主要通过两个途径获取本地文件列表，一是`<input type="file"/>`的表单形式，另一种则是`e.dataTransfer.files`拖拽事件传递的文件信息。

```
var fileList = e.dataTransfer.files;
```

使用 files 方法将会获取到拖拽文件的数组形式的数据，每个文件占用一个数组的索引，如果索引不存在文件数据，将返回 Null。可以通过**length**属性获取文件的数量。

```
var fileNum = fileList.length;
```

拖拽上传需要注意的是需要判断两个条件

1. 拖拽的是文件而不是页面的元素
2. 拖拽的是图片而不是其他类型的文件，可以通过 `file.type` 属性获取文件的类型

```
// 检测是否是拖拽文件到页面的操作
if (fileList.length === 0) {
  return;
}

// 检测文件是不是图片
if (fileList[0].type.indexOf("image") === -1) {
  return;
}
```

下面我们看看结合之前的拖拽事件，来实现拖拽图片并在页面中预览

```
var dropHandler = function(e) {
  e.preventDefault(); //获取文件列表

  var fileList = e.dataTransfer.files;

  //检测是否是拖拽文件到页面的操作
  if (fileList.length == 0) {
    return;
  }

  //检测文件是不是图片
  if (fileList[0].type.indexOf("image") === -1) {
    return;
  }

  //实例化file reader对象
  var reader = new FileReader();
  var img = document.createElement("img");

  reader.onload = function(e) {
    img.src = this.result;
    oDragWrap.appendChild(img);
  };
  reader.readAsDataURL(fileList[0]);
};
```

![n34B6S.gif](https://s2.ax1x.com/2019/09/08/n34B6S.gif)

> 当完成以上操作后，相信你可以成功的完成了拖拽图片预览的操作。当你查看 img 标签时会发现，img的src属性是一个超长的文件二进制数据，当你需要很多这种的img元素时，建议将展示区域脱离文档流，让其绝对定位减少页面的 reflow

#### AJAX 上传图片

既然已经获取到拖拽到web页面中的图片数据了，下一步就是将其发送到服务器端。

### 总结

1. **监听拖拽**: 监听页面元素的拖拽事件，包括: `dragenter`、`dragover`、`dragleave` 和`drop`,**一定要将`dragover`的默认事件取消掉，不然无法触发`drop`事件**。如需拖拽页面里面的元素，需要给其添加属性`draggable="true"`
2. **获取拖拽文件**: 在 `drop` 事件触发后通过`e.dataTransfer.files`获取拖拽文件列表，**一定要将`drop`的默认事件取消掉，否则会默认打开文件**`length`属性获取文件数量，`type`属性获取文件类型
3. **读取图片数据并添加预览图**: 实例化`FileReader`对象，通过其`readAsDataURL(file)`方法获取文件二进制流，并监听其`onload`事件，将`e.result`赋值给img的src属性，最后将图片添加到DOM中
4. **发送图片数据**
