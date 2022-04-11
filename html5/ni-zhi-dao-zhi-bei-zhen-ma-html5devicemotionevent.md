# 你知道指北针吗?——HTML5-DeviceMotionEvent

### 事件介绍

1. `deviceorientation` 提供设备的物理方向信息，表示为一系列本地坐标系的旋角。
2. `devicemotion` 提供设备的加速信息，表示为定义在设备上的坐标系中的卡尔迪坐标。其还提供了设备在坐标系中的自转速率。若可行的话，事件应该提供设备重心处的加速信息。
3. `compassneedscalibration` 用于通知 Web 站点使用罗盘信息校准上述事件。

### 用法简介

#### deviceorientation

注册一个 **deviceorientation** 事件的接收器

```
window.addEventListener("deviceorientation", function(e) {
  console.log(e);
  // 处理event.alpha、event.beta及event.gamma
  document.querySelector("#item").innerHTML = `
  <p> 设备沿z轴上的旋转角度${e.alpha} </p>
  <p> 设备在x轴上的旋转角度，描述设备由前向后旋转的情况 ${e.beta}  </p>
  <p>表示设备在y轴上的旋转角度，描述设备由左向右旋转的情况${e.gamma} </p>
  <p>与北方向的角度差值，正北为0度，正东为90度，正南为180度，正西为270度${e.webkitCompassHeading}</p>
  <p>指北针的精确度，表示偏差为正负多少度${e.webkitCompassAccuracy} </p>
  `;
});
```

> 需要在手机端使用

![nNsGMF.png](https://s2.ax1x.com/2019/09/10/nNsGMF.png)

> 安卓端不支持显示 `webkitCompassHeading` 和 `webkitCompassAccuracy` 小伙伴可以通过苹果手机访问获取

* deviceorientation 事件包含的属性
  * alpha 表示设备沿 z 轴上的旋转角度，范围为 0\~360
  * beta 表示设备在 x 轴上的旋转角度，范围为 -180\~180,，它描述的是设备由前向后的旋转的情况
  * gamma 表示设备在 y 轴上的旋转角度，范围为-90\~90，它描述的是设备在由左向右旋转的情况
  * webkitCompassHeading 与正北方向的角度差值，正北为 0 度，正东为 90 度，正南为 180 度，正西为 270 度，因为 0 表示正北，所以叫指北针
  * webkitCompassAccuracy 指北针的精确度，表示偏差为正负多少度，一般为 10

#### compassneedscalibration

使用自定义界面通知用户校准罗盘

```
window.addEventListener("compassneedscalibration", function(event) {
  alert("您的罗盘需要校准，请将设备沿数字8方向移动。");
  event.preventDefault();
});
```

#### devicemotion

注册一个 **devicemotion** 时间的接收器

```
window.addEventListener("devicemotion", function(event) {
  console.log(event);
});
```

* deviceorientation 事件包含的属性
  * accelerationIncludingGravity 包括重心引力，z 轴方向加了 9.8，在 x，y 方向上的值两者相同
  * acceleration 重力加速度(需要陀螺仪支持)
  * rotationRate (alpha, beta, gamma) 旋转速率
  * interval 获取时间间隔

> 均为只读属性

### 代码示例

我们尝试使用 HTML 的这个对象方法实现一个摇一摇功能

```
let SHAKE_THRESHOLD = 800
let last_update = 0
let x, y, z, last_x = 0, last_y = 0, last_z = 0
function deviceMotionHandler (eventData) {
  let acceleration = eventData.accelerationIncludingGravity
  let curTime = +new Date()
  if ((curTime - last_update) > 300) {
    let diffTime = curTime - last_update
    last_update = curTime
    x = acceleration.x
    y = acceleration.y
    z = acceleration.z
    let speed = Math.abs(x + y + z -last_x - last_y - last_z) / diffTime * 10000

    if (speed > SHAKE_THRESHOLD) {
      // 这里写要执行的代码
    }
    last_x = x
    last_y = y
    last_z = z
  }
}

if (window.DeviceMotionEvent) {
  window.addEventListener('devicemotion', deviceMotionHandler, false)
} else {
  alert('Not supported on your device')
}
```
