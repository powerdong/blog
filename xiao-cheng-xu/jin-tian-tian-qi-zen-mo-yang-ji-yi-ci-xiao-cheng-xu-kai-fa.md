# 今天天气怎么样——记一次小程序开发

在上周看了微信小程序的介绍后，就想着要开发一个服务于自己的小程序，也在另一方面可以有一次小程序的开发实践经验。

### 微信小程序

小程序是一种**不需要下载安装即可使用**的应用(其实是需要下载的,只不过是非常小的一个应用,加载时间可忽略)

应用将无处不在，随时可用，但又无需安装卸载。

* 小程序适用简单的，用完即走的应用
* 小程序适合低频的应用
* 小程序适合性能要求不高的应用

**小程序是单向数据绑定**

### 萌生想法

> 如果你想看实例源代码可以移步[天气小程序 git 仓库](https://github.com/powerdong/weatherProgram)

之前在学习网络请求时有过请求一个天气接口，获取到了天气的相关信息

由此，想利用现有的知识构建一个天气小程序。

### 开始实施

* 到[微信公众平台](https://mp.weixin.qq.com)注册一个账号
* 在左侧的开发选项中的**开发设置**中找到`AppID`
* 下载[微信 web 开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)
* 登陆工具后创建项目，将之前复制的`AppID`填写到对应的内容项中。

在新创建的文件夹中以`app`开头的文件都是全局配置文件，首先在`app.json`中的`pages`属性中增加`"pages/index/index"`保存后会自动生成相应的文件，`index`文件夹作为我们的首页。

```
{
  "pages": [
    "pages/index/index", //首页
    "pages/place/index" //搜索页
  ],
  "window": {
    "backgroundTextStyle": "light", //下拉 loading 的样式
    "navigationBarBackgroundColor": "#0078D7", //顶部导航的背景颜色
    "navigationBarTitleText": "通天气", //通用导航标题
    "navigationBarTextStyle": "white" // 导航标题颜色
  },
  "permission": {
    "scope.userLocation": {
      // 使用wx.getLocation时需要配置
      "desc": "你的位置信息将用于定位城市天气信息的展示" //请求位置信息时要展示的
    }
  },
  "sitemapLocation": "sitemap.json" //指明sitemap.json位置
}
```

首先查看了当前一些天气小程序(腾讯天气，彩云天气，墨迹天气等)，吸取该小程序的页面搭建样式，从华为手机的桌面天气应用中也有学习

#### 收集素材

> 本小程序计划天气样式使用字体图标

从 [iconfont](https://www.iconfont.cn) 网站上查询到了一些图标，然后下载到本地，配置到 `app.wxss` 用来页面引用

在一个[设计网站](http://bapaboli.lofter.com/post/910b8\_6499be)收集了一些天气背景图，裁剪后用于显示，美化页面

#### 开始构建页面

> 在请求天气接口时报错，查询问题后发现需要到开发设置中的**服务器域名**中配置合法的请求域名。

首先采用腾讯天气的页面布局，简约式布局方案，采用微信的 rpx 单位实现多设备适配实现，在该过程中引用本地图片是报错，微信建议使用 `base64` 或者配置 `download`

* 这里选择了使用 `base64` 格式

**当前天气情况**

**首页将要显示当前天气信息和未来几天的天气信息**

* 页面布局采用两屏布局格式，更好的展示信息
* 首屏展示当前天气信息和今明两天的天气信息
* 页面请求信息时默认请求当前位置的天气信息

首屏采用`flex`弹性盒子布局，内容横向居中，纵向排布，通过给各个模块设置`flex`值控制各模块的位置占比。 首页加入了暖心话语，采用`Math.random()`随机从数组中取出信息显示到页面中。

> 为了更好的展示页面，这里采用[腾讯地图](https://lbs.qq.com/qqmap\_wx\_jssdk/method-reverseGeocoder.html)中的逆地址解析，呈现在页面的位置信息可以更加精确。

**逐小时天气情况**

逐小时展示区域需要支持左右滑动，使用微信提供的`<scroll-view></scroll-view>`标签，在标签内加入属性`scroll-x`。设置当前样式

```
.weather-hours {
  background: #fff;
  padding: 0 0 0 20rpx;
  width: 100%;
  white-space: nowrap;
  box-sizing: border-box;
  box-shadow: 0 1px 8rpx #ccc;
}
```

* 天气情况图标只展示了`云、雷、雨、晴、阴、雪、雾、沙尘`的天气情况，所以配置了一个设置展示图标的方法函数。
* 在设置首页的天气背景图片使用了更换类名的形式更改背景图片，所以此函数可以实现多处调用。

在这个阶段多次用到了字符串编辑、删减、正则匹配等功能。以展示更好的界面。

**未来天气情况**

在设置未来几天的星期时使用了`Date()`

```
week: function(data) {
var item = data.split("-").join("/");
return item
},

let data = new Date(this.week(serverWeatherData[i].date)).getDay();
let weekday = new Array(7);
weekday = ["周日", "周一", "周二", "周三", "周四", "周五", "周六"]
serverWeatherData[i].day = weekday[data];
```

使用`new Date(2019/07/03).getDay()`可以获取到那天的星期几，将值作为数组下标，选取数组中对应的内容信息。

> 在首页底部添加了生活指数展示区。

**搜索页面**

**搜索页包含当前定位城市和热门城市列表信息展示。**

* 在首页点击地理位置展示区域添加了`bindtap`的点击事件
* 通过当前定位城市页面传值`wx.navigateTo({})`进行跳转到搜索页面
* 定义了一个全局变量`place`，将首页请求到的地理位置信息存放到`place`
* 当首页定位的城市传到搜索页展示到当前定位城市

> 热门城市列表采用列表渲染进行渲染展示。

为每一个列表项添加了点击事件，点击相应的城市可进行搜索，跳转至天气展示区域显示要查询的城市天气。

* 在列表项添加`bindtap`
* 在列表项中添加自定义属性`data-text`用来存取当前项的内容
* 在点击函数调用时获取到`data-text`存取的内容进行搜索

给搜索框添加`bindinput`事件，请求城市信息提交搜索

* 调用了[和风天气](https://dev.heweather.com/docs/search/)提供的城市列表接口
* 监听输入框内容如果为空则不显示搜索结果页
* 输入内容后如果没有检索到城市信息,显示**请输入正确的城市名**提示用户修改信息
* 为每个检索到的城市列表添加**点击搜索**事件
* 为**取消**按钮添加点击事件，点击后将输入框置为空
* 为输入框的`input`事件添加防抖函数
