---
title: H5(7) Audio && Video
date: 2018-01-21 21:41:34
categories:
- HTML5抄书笔记
tags:
- Audio
- Video
---

本文介绍HTML5中新增的两个元素——`video`元素与`audio`元素，它们分别用来处理视频数据和音频数据。需要说明的是，本文中的“媒体”一词是对音频和视频的总称。

<!-- More -->

### 一、元素属性

`audio`元素与`video`元素所具有的属性大致相同，所以我们看看这两个元素都具有哪些属性

#### 1. src

在该属性中指定媒体数据的URL地址

#### 2. autoplay

在该属性中指定媒体是否在页面加载后自动播放。该属性的使用方法如下所示：

```html
<video src="sample.mov" autoplay></video>
```

#### 3. preload

该属性指定视频或音频数据是否预加载。如果使用预加载的话，浏览器会预先将视频或音频数据进行缓冲，这样可以加快播放的速度，因为播放时数据已经预先缓冲完毕。

该属性由三个可选择的值：`none`、`metadata`与`auto`，默认为`auto`

* `none` —— 不进行预加载

* `metadata` —— 只预加载媒体的元数据（媒体字节数、第一帧、播放列表、持续时间等）

* `auto` —— 预加载全部视频或音频

```html
<video src="sample.mov" preload="auto"></video>
```

#### 4. poster（video元素独有属性）

当视频不可用时，可以使用该元素向用户展示一副替代用的图片。当视频不可用时，最好使用该属性，以免展示视频的区域内出现一片空白。

```html
<video src="sample.mov" poster="cannotuse.jpg"></video>
```

#### 5. loop

在该属性中指定是否循环播放视频或音频

```html
<video src="sample.mov" autoplay loop></video>
```

#### 6. controls

在该属性中指定是否为视频或音频添加浏览器自带的播放用的控制条。控制条中具有播放、暂停等按钮

```html
<video src="sample.mov" controls></video>
```

#### 7. width与height （video元素独有属性）

指定视频的宽度与高度（像素）

```html
<video src="sample.mov" width="500" height="500"></video>
```

#### 8. error属性

在读取、使用媒体数据的过程中，在正常情况下，`video`元素或`audio`元素的`error`属性为`null`，但是任何时候只要出现错误，`error`属性将会返回一个`MediaError`对象，该对象的`code`返回对应的错误状态，错误状态共有4个可能值。

* `MEDIA_ERR_ABORTED`（数字值为1）：媒体数据的下载过程由于用户的操作原因而被中止

* `MEDIA_ERR_NETWORK`（数字值为2）：确认媒体资源可用，但是在下载时出现网络错误，媒体数据的下载过程被中止

* `MEDIA_ERR_ECODE`（数字值为3）：确认媒体资源可用，但是解码时发生错误

* `MEDIA_ERR_SRC_NOT_SUPPORTED`（数字值为4）：媒体格式不被支持

#### 9. networkState属性

在媒体数据加载过程中可以使用`video`元素或`audio`元素的`networkState`属性读取当前网络状态，共有如下4种可能值。

* `NETWORK_EMPTY`（数字值为0）：元素处于初始状态

* `NETWORK_IDLE`（数字式为1）：浏览器已选择好用什么编码格式来播放媒体，但尚未建立网络连接

* `NETWORK_LOADING`（数字值为2）：媒体数据加载中

* `NETWORK_NO_SOURCE`（数字值为3）：没有支持的编码格式，不执行加载

#### 10. currentSrc属性

可以使用`video`元素或`audio`元素的`currentSrc`属性来读取播放中的媒体数据的url地址。`currentSrc`属性为只读属性。

#### 11. buffered属性

可以使用`video`元素或`audio`元素的`buffered`属性来返回一个对象，该对象实现`TimeRanges`接口，以确认浏览器是否已缓存媒体数据。`TimeRanges`对象表示一段时间范围，在大多数情况下，`TimeRanges`对象表示的时间范围时一个单一的以0开始的范围，但是如果浏览器发出`Range Requests`请求，这是`TimeRanges`对象表示的时间范围是多个时间范围。

`TimeRanges`对象具有一个`length`属性，表示有多少个时间范围，大多数情况下存在时间范围时，该值为1；不存在时间范围时，该值为0。`TimeRanges`对象还具有两个方法，`TimeRanges.start(index)`与`TimeRanges.end(index)`，大多数情况下将`index`值设为0就可以了。

当用`videoElement.buffered`语句来实现`TimeRanges`接口时，`TimeRanges.start(0)`表示当前缓存区内从媒体数据的什么时间开始进行缓存，`TimeRanges.end(0)`表示当前缓存区内的结束时间。

#### 12. readyState属性

可以使用`video`元素或`audio`元素的`readyState`属性返回媒体当前播放位置的就绪状态，共有5个可能值。

* `HAVE_NOTHING`（数字值为0）：没有获取到媒体的任何信息，当前播放位置没有可播放数据

* `HAVE_METADATA`（数字值为1）：已经获取到了足够的媒体数据，但是当前播放位置没有有效的媒体数据（也就是说，获取到的媒体数据无效，不能播放）

* `HAVE_CURRENT_DATA`（数字值为2）：当前播放位置已经有数据可以播放，但没有获取到可以让播放器前进的数据。当媒体为视频时，意思是当前帧的数据已获得，但还没有获取到下一帧的数据，或者当前帧已经是播放的最后一帧。

* `HAVE_FURURE_DATA`（数字值为3）：当前播放位置已经有数据可以播放，而且也获取到了可以让播放器前进的数据。当媒体为视频时，意思是当前帧的数据已获得，而且也获取到了下一帧的数据，当前帧时播放的最后一帧时，`readyState`属性不可能为`HAVE_FUTURE_DATA`

* `HAVE_ENOUGH_DATA`（数字值为4）：当前播放位置已经有苏杭距可以播放，同时也获取到了可以让播放器前进的数据，而且浏览器确认媒体数据以某一种速度进行加载，可以保证有足够的后续数据进行播放。

#### 13. seeking属性与seekable属性

可以使用`video`元素或`audio`元素的`seeking`属性返回一个布尔值，表示浏览器是否正在请求某一特定播放位置的元素，`true`表示浏览器正在请求数据，`false`表示浏览器已停止请求。

可以使用`video`元素或`audio`元素的`seekable`属性来返回一个`TimeRanges`对象，该对象表示请求到的数据的时间范围。当媒体为视频时，开始时间为请求到的视频数据第一帧的事件，结束时间为请求到的视频数据最后一帧的事件。

#### 14. currentTime属性、startTime属性和duration属性

可以使用`video`元素或`audio`元素的`currentTime`属性来读取媒体的当前播放位置，也可以通过修改`currentTime`属性来修改当前播放位置。如果修改的位置上没有可用的媒体数据时，将抛出`INVALID_STATE_ERR`异常；如果修改的位置超出了浏览器在一次请求中可以请求的数据范围，将抛出`INDEX_SIZE_ERR`异常。

可以使用`video`元素或`audio`元素的`startTime`属性来读取媒体播放的开始时间，通常为0

可以使用`video`元素或`audio`元素的`duration`属性来读取媒体文件总的播放时间。

三项属性均为时间，单位为秒。

#### 15. played属性、paused属性、ended属性

可以使用`video`元素或`audio`元素的`played`属性来返回一个`TimeRanges`对象，从该对象中可以读取媒体文件的已播放部分的时间段。开始时间为已播放部分的开始时间，结束时间为已播放部分的结束时间。

可以使用`video`元素或`audio`元素的`paused`属性来返回一个布尔值，表示是否处于暂停播放中，`true`表示媒体餐厅播放，`false`表示媒体正在播放。

可以使用`video`元素或`audio`元素的`ended`属性来返回一个布尔值，表示是否播放完毕，`true`表示媒体播放完毕，`false`表示还没有播放完毕。

#### 16. defaultPlaybackRate属性与playbackRate属性

可以使用`video`元素或`audio`元素的`defaultPlaybackRate`属性读取或修改媒体默认的播放速率

可以使用`vidio`元素或`audio`元素的`playbackRate`属性读取或修改媒体当前的播放速率

#### 17. volumn属性与muted属性

可以使用`video`元素或`audio`元素的`volumn`属性读取或修改媒体的播放音量，范围为0到1，0为静音，1为最大音量

可以使用`vidio`元素或`audio`元素的`muted`属性读取或修改媒体的静音状态，该值为布尔值，`true`表示处于静音状态，`false`表示处于非静音状态

---

### 二、方法

#### 1. play方法

使用`play`方法来播放媒体，自动将元素的`paused`属性值变为`false`

#### 2. pause方法

使用`pause`方法来暂停播放，自动将元素的`paused`属性值变为`true`

#### 3. load方法

使用`load`方法来重新载入媒体进行播放，自动将元素的`playbackRate`属性值变为`defaultPlaybackRate`属性的值，自动将元素的`error`值变为`null`

#### 4. canPlayType方法

使用`canPlayType`方法来测试浏览器是否支持指定的媒体类型，该方法定义如下：

```javascript
var support = videoElement.canPlayType(type);
```

该方法返回3个可能值：

* 空字符串：表示浏览器不支持此种媒体类型

* `maybe`：表示浏览器可能支持此种媒体类型

* `probably`：表示浏览器确定支持此种媒体类型

---

### 三、事件

浏览器在请求媒体数据、下载媒体数据、播放媒体数据中的事件

| 事件 | 描述
| - | -
| `loadstart` | 浏览器开始在网上寻找媒体数据
| `progress` | 浏览器正在获取媒体数据
| `suspend` | 浏览器暂停获取媒体数据，但是下载过程并没有正常结束
| `abort` | 浏览器在下载完全部媒体数据之前中止获取媒体数据，但是并不是由错误引起的
| `error` | 获取媒体数据过程中出错
| `emptied` | video元素或audio元素所在的网络突然变为未初始化状态（可能原因：1. 载入媒体过程中突然发生一个致命错误；2. 在浏览器正在选择支持的播放格式时，又调用了`load`方法重新载入媒体）
| `stalled` | 浏览器尝试获取媒体数据失败
| `play` | 即将开始播放，当执行了`play`方法时触发，或数据下载后元素被设为`autoplay`属性
| `pause` | 播放暂停，当执行了`pause`方法时触发
| `loadedmetadata` | 浏览器获取完毕媒体的时间长和字节数
| `loadeddata` | 浏览器已加载完毕当前播放位置的媒体数据，准备播放
| `waiting` | 播放过程由于得不到下一帧而暂停播放（例如下一帧尚未加载完毕），但很快就能够得到下一帧
| `playing` | 正在播放
| `canplay` | 浏览器能够播放媒体，但估计以当前播放速率不能直接将媒体播放完毕，播放期间需要缓冲
| `canplaythrough` | 浏览器能够播放媒体，而且以当前播放速率能够将媒体播放完毕，不再需要进行缓冲
| `seeking` | `seeking`属性变为true，浏览器正在请求数据
| `seeked` | `seeking`属性变为false，浏览器停止请求数据
| `timeupdate` | 当前播放位置被改变，可能是播放过程中的自然改变，也可能使被认为的改变，或由于播放不能连续而发生的跳变
| `ended` | 播放结束后停止播放
| `ratechange` | `defaultplaybackRate`属性或`playbackRate`属性被改变
| `durationchange` | 播放时长被改变
| `volumnchange` | `volumn`属性被改变或`muted`属性被改变

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()

