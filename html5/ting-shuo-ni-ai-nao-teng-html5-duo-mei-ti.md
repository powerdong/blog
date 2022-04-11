# 听说你爱闹腾——HTML5-多媒体

### Web 中的音频和视频

> 自 21 世纪初以来，我们的带宽开始能够支持任意类型的视频在早些时候，传统的 web 技术（如 HTML ）不能够在 Web 中嵌入音频和视频，所以一些像 Flash 的专利技术在处理这些内容上变得很受欢迎。这些技术能够正常的工作，但是却有着一系列的问题，包括无法很好的支持 HTML/CSS 特性、安全问题，以及可行性问题。

传统的解决方案能够解决许多这样的问题，前提是它能够正确的工作,幸运的是，几年之后 HTML5 标准提出，其中有许多的新特性，包括 `<video>` 和 `<audio>` 标签，以及一些 JavaScript 和 APIs 用于对其进行控制。

### `<audio>` 音频标签

我们可以在 html 文件中使用 `<audio>` 标签来插入一个音频文件。

```
<!-- src 属性指向你想要嵌入网页当中的资源 -->
<audio id="" src=""></audio>
```

#### 属性

**`autoplay` 自动播放**

```
<!-- 这个属性会使音频和视频内容立即播放，即使页面的其他部分还没有加载完全。 -->
<audio src="" autoplay></audio>
```

**`controls` 设置控件**

```
<!-- 用户必须能够控制视频和音频的回放功能。
    可以使用浏览器提供的控制接口，同时你也可以使用合适的 JavaScript API构建控制接口
    至少，这些媒体应该包括开始和停止，以及调整音量的功能。
 -->
<audio src="" autoplay controls></audio>
```

**`preload` 预加载**

规定是在否在页面加载后载入资源，这个属性有三个值可选

* `"none"`:不缓冲，不需要加载数据
* `"metadata"`: 仅缓存文件的元数据，诸如时长，比特率，帧大小这样的元数据，而不是媒体内容徐亚加载的
* `"auto"`: 页面加载后缓存媒体文件，浏览器应当加载它认为适量的媒体内容

```
<audio src="" preload="auto"></audio>
```

**loop 循环播放**

是否可以让音频或者视频文件

```
<audio src="" loop></audio >
```

**muted**

会导致媒体播放时，默认关闭声音。

```
<audio src="" muted></audio >
```

#### 多格式支持

对于不同的浏览器对视频或者音频格式的支持不同

像 MP3、MP4、WebM这些术语叫做**容器格式**,他们是用不同的方式来播放音频或视频的。也就是说这些容器是用不同的音频轨道、视频轨道、元数据来呈现媒体文件的。

视频和音频都有不同的格式

* WebM 容器通常包括了 Ogg Vorbis 音频和 VP8/VP9 视频
  * 主要在 FireFox 和 Chrome 当中支持
* MP4 容器通常包括 AAC 以及 MP3 音频和 H.264 视频
  * 主要在 Internet Explorer 和 Safari 当中支持
* 老式的 Ogg 容器往往支持 Ogg Vorbis 音频和 Ogg Theora 视频
  * 主要在 Firefox 和 Chrome 当中支持，**不过这个容器已经被更强大的 WebM 容器所取代**

```
<audio id="music">  
  <source src="music.mp3" type="audio/mpeg">
  <source src="music.ogg" type='audio/ogg"'>
</audio>
```

现在我们将 src 属性从 `<audio>` 标签中移除，转而将它放在几个单独的标签 `<source>` 当中。在这个例子当中，浏览器将会检查 `<source>` 标签，并且播放第一个与其自身 `codec` 相匹配的媒体

视频应当包括 WebM 和 MP4 两种格式，这两种在目前已经足够支持大多数平台和浏览器。

每个 `<source>` 标签页含有一个 `type` 属性，这个属性是可选的,但是建议添加上这个属性——它包含了`<source>` 标签文件的 `MIME types`,同时在浏览器检索的时候也会迅速的跳过那些不支持的格式，找到能正确播放的格式。

### `<video>` 视频标签

标签与 标签的使用方式几乎完全相同,有一些细微的差别我们下边一一介绍

### 属性

#### width/height

控制视频的尺寸，可以使用属性，也可以使用 Css 来控制视频的尺寸。无论哪种方式，视频都会保持它原始的**纵横比**。如果你设置的尺寸没有保持视频原始的长宽比，那么视频的边框将会拉伸，而未被视频内容填充的部分，将会显示默认的背景颜色。

#### poster

当时视频不可用时，使用一张图片替代，否则是空白

指向了一个图像的URL，这个图像会在视频播放前显示。通常用于粗略的预览或者广告。

```
<video id="" poster="封面.jpg" controls></video>>
```

> 当我们使用了 `autoplay` 属性时，当页面一加载就播放视频的话，就不会看到 `poster` 的效果了。

### 脚本化

当我们在元素上设置了 `controls` 属性时，浏览器会显示原生的浏览器控件，当你想要修改这些样式的时候，可以通过隐藏本地控件， JavaScript API 来进行相应的设置。

#### 获取 Dom

```
// 获取到对应的 `dom` 元素
let audio = document.getElementById('audio')
// 使用 js 创建标签
// audio 可以使用 new 操作符
let audio = new Audio('音频文件')
let video = document.createElement('video');
```

#### 自定义控件

为了设置当前的控件样式，需要将默认的控件删除。

```
audio.removeAttribute('controls');
// 使用属性 Boolean 值
audio.controls = false
// 其他的属性设置
audio.loop = 'loop'
audio.preload = 'auto'
audio.autoplay = true
```

#### 播放与暂停

通过使用`play()`和`pause()`方法来控制音频视频的播放和暂停,通过`paused`属性查询媒体播放状态

```
if (audio.paused) {
  audio.play()
} else {
  audio.pause()
}
```

我们使用if语句来检查视频是否暂停。如果视频已暂停，`audio.paused`属性返回true，包括在第一次加载时处于0的时间段都是视频的暂停状态。

当我们在想实现视频停止功能时，点击停止，视频停止播放，播放时间返回0。无奈的是在 API 中并没有停止的方法，我们可以通过设置`currentTime`属性来设置当前的播放位置,将它设置为0，将会立刻跳到该位置。

```
function stop(){
  audio.pause()
  audio.currentTime = 0
}
```

#### 播放速率

我们可以通过设置播放速率，进而实现快退快进的功能。 playbackRate设置媒体文件播放时的速率。这用于实现让用户控制快放、慢放等。 正常播放速率乘以该值表示当前的播放速率，所以1.0表示一个正常的播放速率。

```
// 浮点数1.0 是 "正常速度"， 比 1.0 小的值使媒体文件播放的慢于正常速度，比1.0大的值使播放变得快于正常速度.
audio.playbackRate = 1.0
```

也可以通过定时器，通过设置当前播放时间来实现快进快退

```
function backWard() {
  // 快退
  if(audio.currentTime <= 3) {
    // 当资源已经回退到最初了
    clearInterval(intervalRwd)
    stop()
  } else {
    audio.currentTime -= 3
  }
}

function forWard() {
  // 快进
  if(audio.currentTime >= audio.duration -3) {
    // 当资源已经快进到最后了
    clearInterval(intervalFwd)
    stop()
  } else {
      audio.currentTime += 3
    }
}
```

#### 更新当前播放时间

当需要设置播放器的显示时间时，我们将运行一个函数，以便在元素播放过程中`timeupdate`事件触发时更新当前播放时间和播放进度。

```
function setTime() {
  // 首先我们计算到当前的播放时间
  let minutes = Math.floor(audio.currentTime / 60);
  let seconds = Math.floor(audio.currentTime - minutes * 60);
  let minuteValue;
  let secondValue;
  // 进行格式转换，当分钟小于10的时候，在前面加0
  if (minutes < 10) {
    minuteValue = '0' + minutes;
  } else {
    minuteValue = minutes;
  }
  // 秒数的格式设置
  if (seconds < 10) {
    secondValue = '0' + seconds;
  } else {
    secondValue = seconds;
  }
  // 进行时间值拼接，展示到页面
  let audioTime = minuteValue + ':' + secondValue;
  timer.textContent = audioTime;
  // 进度条的长度计算
  let barLength = timerWrapper.clientWidth * (audio.currentTime/audio.duration);
  timerBar.style.width = barLength + 'px';
}
```

#### 音量

可以通过 `volume` 属性设置播放音量，介于0(静音)\~1(最大音量)之间，默认为1。将 `muted` 属性设置为true则会进入静音属性(模式)，设置为false则会恢复之前指定的音量继续播放。

```
audio.volume=0.2;
```

#### 缓冲 TimeRanges对象

played 属性返回 TimeRanges 对象，表示已经播放(看过)的时间段 已播范围指的是被播放音频/视频的时间范围。如果用户在音频/视频中跳跃，则会获得多个播放范围。

buffered 属性返回 TimeRanges 对象，表示已经缓存的时间段 缓冲范围指的是已缓冲音视频的时间范围。如果用户在音视频中跳跃播放，会得到多个缓冲范围。

seekable 属性返回 TimeRanges 对象，表示用户可以调转的时间段 可寻址范围指的是用户在音频/视频中可寻址（移动播放位置）的时间范围。

```
// 返回起始时间点和结束时间点，单位是秒，其实参数为0
audio.played.start(0)
audio.played.end(0)
audio.buffered.start(0)
audio.buffered.end(0)
audio.seekable.start(0)
audio.seekable.end(0)
```

#### 媒体播放状态

* paused 为 true 时，表示播放器暂停
* seeking 为 true 时，表示播放器正在跳到一个新的播放点
* ended 为 true 时，播放器播放完媒体会停下来

下面的代码确定当前缓存内容的百分比

```
let percent_loaded = Math.floor(song.buffered.end(0) / song.duration * 100)
```

### 事件

* 当音频/视频处于加载过程中时，会依次发生以下事件：
  * loadstart 当浏览器开始查找资源时触发
  * durationchange 当资源时长已更改时
  * loadedmetadata 当浏览器获取完媒体的元数据时触发
  * loadeddata 浏览器已经加载完当前帧数据，准备播放时触发
  * progress 当浏览器正在下载资源时
  * canplay 当浏览器可以播放资源时
  * canplaythrough 当浏览器可在不因缓冲而停顿的情况下进行播放时
* play 当资源已经开始播放时
* pause 当资源暂停时
* retechange 当资源播放速度已更改时
* seeked 当用户已经移动/跳跃到音频/视频中的新位置时
* stalled 当浏览器尝试获取媒体数据，但数据不可用时
* volumechange 当音量发生改变时
* error 当在资源加载期间发生错误时
* ended 当播放结束时触发

### 兼容文件

在使用资源文件时，需要考虑到当前浏览器是否支持当前的文件格式，我们可以使用`canPlayType()`方法来做兼容

```
let a = new Audio()
if(a.canPlayType('audio/mp3')){
  a.src = './audio.mp3';
  a.play();
}
```
