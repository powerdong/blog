# 你在哪啊？我在这啊——HTML5-Geolocation

HTML5 提供了 **geolocation API**，用于提供位置相关服务。

地理位置 API 通过`window.navigator.geolocation`提供。在使用之前需要进行判断该对象是否存在，如果存在的话，那么位置信息可用

```
if ("geolocation" in navigator) {
  /* 地理位置信息可用 */
} else {
  /* 地理位置信息不可用 */
}
```

### geolocation 获取当前定位

我们可以通过使用`getCurrentPosition(success, error, options)`函数获取用户当前定位位置。这会**异步请求**获取用户位置，并查询定位硬件来获取最新信息。当定位被确定后，定义的回调函数就会被执行。

![nNYgyt.png](https://s2.ax1x.com/2019/09/10/nNYgyt.png)

* **success**:成功的回调函数(必须)
* **error**:错误的回调函数
* **options**:参数对象，通过该对象参数设置最长可接受的定位返回时间，等待请求的时间和是否精准定位。

#### success

```
// 特别注意：如果您使用的是谷歌浏览器，需要连接vpn才能获取到

let success = function(pos) {
  console.log(pos);
};

// 只传一个参数
window.navigator.geolocation.getCurrentPosition(success);
```

![nNNTrq.png](https://s2.ax1x.com/2019/09/10/nNNTrq.png)

* Geoposition 对象属性
  * accuracy 定位精准度，单位 m
  * altitude 海拔
  * altitudeAccuracy 海拔精准度，单位 m
  * heading 方向
  * latitude 纬度
  * longitude 经度
  * speed 速度

#### error

```
let error = function(err) {
  console.log(err);
};

//传入错误回调函数
window.navigator.geolocation.getCurrentPosition(success, error);
```

![nNU2O1.png](https://s2.ax1x.com/2019/09/10/nNU2O1.png)

* PositionError 对象属性
  * 用户拒绝 code === 1
  * 获取不到 code === 2
  * 连接超时 code === 3

#### config

```
const config = {
  // 是否需要高精度位置 默认为false
  enableHighAccuracy: true,
  // 请求超时时间，默认为 infinity，单位ms
  timeout: 1000,
  // 位置信息过期时间，设置为0就无条件获取新的地理位置信息，默认为 0，单位 ms
  maximumAge: 0
};

// 传入参数配置
window.navigator.geolocation.getCurrentPosition(success, error, config);
```

![nNaDBt.png](https://s2.ax1x.com/2019/09/10/nNaDBt.png)

> 这里的 code = 3，因为我们设置的请求超时时间为 1000ms，当浏览器在 1000ms 内没有成功的获取到信息的话，就会报连接超时错误

### watchPosition 监听位置变化

```
// 用于注册监听器，在设备的地理位置发生改变的时候自动被调用
// 参数与 getCurrentPosition 相同
let id = geolocation.watchPosition();
```

在 watchPosition 方法定义使用后，则会不停的获取用户的地理位置信息，不停的更新用户的位置信息。

#### clearWatch

使用`clearWatch(id)`对 watch 清除监听
