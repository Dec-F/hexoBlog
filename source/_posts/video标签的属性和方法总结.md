---
title: video标签的属性和方法总结
date: 2017-01-15 17:54:34
categories:
- HTML5

tags:
- video
- HTML5
---
最近想做一个弹幕插件，查了很多video标签的属性和方法

### error属性

   在正常读取时候，使用媒体数据的过程中，video元素或audio元素的error属性为null，但是任何时候只要出现错误，error属性将返回一个MediaError对象，该对象的code返回对应的错误状态，共有4个可能值：
   
* `MEDIA_ERR_ABORTED(数字值为1)`，媒体数据的下载过程由于用户的操作原因而被终止。
- `MEDIA_ERR_NETWORK(数字值为2)`，确认媒体资源可用，但是在下载出现网络错误，媒体数据的下载过程被中止。
<!-- more -->

- `MEDIA_ERR_DECODE(数字值为3)`，确认媒体资源可用，但是解码时发生错误。
- `MEDIA_ERR_SRC_NOT_SUPPORTED(数字值为4)`，媒体资源不可用或媒体格式不被支持。
代码如下：
```js
<video src="" id="videoElement"></video>
 var video = document.getElementById("videoElement");
 video.addEventListener("error",function(){
     var error = video.error;
     swicth(error.code) {
         case 1:
            alert("视频的下载过程被中止");
         break;
 
         case 2:
            alert("网络发生故障，视频的下载过程被中止");
         break;
 
         case 3:
            alert("解码失败");
         break;
 
         case 4:
            alert("媒体资源不可用或媒体格式不被支持");
         break; 
 
     }
 },false);
```
### networkState属性

在媒体数据加载过程中可以使用video元素或audio元素的networkState属性读取当前网络状态，共有4个可能的值；
1．  NETWORK_EMPTY（数字值为0）：元素处于初始状态。
2．  NETWORK_IDLE(数字值为1)，浏览器已选择好用什么编码格式来播放媒体，但是尚未建立网络连接。
3．  NETWORK_LOADING(数字值为2)：媒体数据加载中
4．  NETWORK_NO_SOURCE(数字值为3)，没有支持的编码格式，不执行加载。
### currentTime属性与duration属性

video元素或audio元素的currentTime属性来读取媒体的当前播放位置(单位是s)，也可以通过修改currentTime属性来修改当前播放位置，如果修改的位置上没有可用的媒体数据时，将抛出INVALID_STATE_ERR异常；
video元素或audio元素的duration属性来读取媒体文件总的播放时间。
我们可以来做demo如下：
HTML代码如下：

```html
<video id="video" controls width=640 height=360 autoplay>
    <source src="Wildlife/Wildlife.ogv" type="video/ogg; codecs="theora, vorbis""/>
    <source src="Wildlife/Wildlife.webm" type="video/webm" >
    <source src="Wildlife/Wildlife.mp4" type="video/mp4">
    <p>Video is not visible, most likely your browser does not support HTML5 video</p>
</video>
<button onclick="getCurTime()" type="button">获得当前时间的位置</button>
<button onclick="setCurTime()" type="button">把时间位置设置为 5 秒</button>
<button onclick="duration()" type="button">读取媒体文件总的播放时间</button>
```
 
JS代码如下：
```js
var myVid=document.getElementById("video");
function getCurTime(){ 
    // 获得当前视频或者音频时间的位置
    alert(myVid.currentTime);   
} 
function setCurTime(){ 
// 给当前的视频或者音频设置5s时间的位置
    myVid.currentTime=5;        
} 
function duration() {
    //读取媒体文件总的播放时间 单位为S
    alert(myVid.duration); 
}
```
大家可以复制代码，换一张视频地址就可以看到效果了~；

### played属性，paused属性与ended属性
- played：video元素或audio元素的played属性来返回一个TimeRanges对象，从该对象中可以读取媒体文件的已播放部分的时间段。开始时间为已播放部分的开始时间，结束时间为已播放部分的结束时间。
- paused属性：使用video元素或audio元素的paused属性来返回一个布尔值，表示是否处于暂停播放中，true表示媒体暂停播放，false表示媒体正在播放；
- ended属性：使用video元素或audio元素的ended属性来返回一个布尔值，表示是否播放完毕，true表示媒体播放完毕，false表示媒体还没有播放完毕。
HTML代码如下：
```html
    <video id="video" controls width=640 height=360 autoplay loop>
    <source src="Wildlife/Wildlife.ogv" type="video/ogg; codecs="theora, vorbis""/>
    <source src="Wildlife/Wildlife.webm" type="video/webm" >
    <source src="Wildlife/Wildlife.mp4" type="video/mp4">
    <p>Video is not visible, most likely your browser does not support HTML5 video</p>
    </video>
    <button onclick="played()" type="button">读取媒体文件的已播放部分的时间段</button>
    <button onclick="paused()" type="button">是否处于暂停播放</button>
    <button onclick="ended()" type="button">是否播放完毕</button>
```

JS代码如下：
```js
var myVid=document.getElementById("video");
function played() {
    //读取媒体文件的已播放部分的时间段
    console.log(myVid.played);
}
function paused() {
    // 是否处于暂停播放
    alert(myVid.paused);
}
function ended() {
    // 是否播放完毕
    alert(myVid.ended);
}

```
### defaultPlaybackRate属性与playbackRate属性
- defaultPlaybackRate属性：可以使用video元素或者audio元素的defaultPlaybackRate属性读取或修改媒体默认的播放速率。
- playbackRate属性：可以使用video元素或者audio元素的playbackRate属性读取或修改媒体当前的播放速率。
### volume属性与muted属性
- volume属性：使用video元素或audio元素的volume属性读取或修改媒体的播放音量，范围为0到1,0为静音，1为最大音量。
- muted属性：使用video元素或audio元素的muted属性读取或修改媒体的静音状态，该值为布尔值，true表示处于静音状态，false表示处于非静音状态。
HTML代码如下：
```html
<video id="video" controls width=640 height=360 autoplay loop>
    <source src="Wildlife/Wildlife.ogv" type="video/ogg; codecs="theora, vorbis""/>
    <source src="Wildlife/Wildlife.webm" type="video/webm" >
    <source src="Wildlife/Wildlife.mp4" type="video/mp4">
    <p>Video is not visible, most likely your browser does not support HTML5 video</p>
</video>
<button onclick="volume()" type="button">读取或修改媒体的播放音量</button>
<button onclick="muted()" type="button">读取或修改媒体当前的静音状态</button>
```

JS代码如下：
```js
var myVid=document.getElementById("video");
function volume(){
    // 读取或修改媒体的播放音量
    myVid.volume = 0.1;
}
function muted() {
    //读取或修改媒体当前的静音状态
    myVid.muted = true;
}
```
大家可以使用下，当我点击”读取或修改媒体的播放音量”按钮时候，声音会变的很小，当我点击”读取或修改媒体当前的静音状态”按钮时候，视频完全就没有声音。
###方法
video元素与audio元素有以下四种方法；
- play方法：使用play方法来播放媒体，自动将元素的paused属性的值变为false。
- pause方法：使用pause方法来暂停播放，自动将元素的paused属性的值变为true。
- load方法，使用load方法来重新载入媒体进行播放，自动将元素的playbackRate属性的值变为defaultPlaybackRate属性的值，自动将元素的error的值变为null。
下面是播放和暂停的demo如下：
```html
<video id="video" controls width=640 height=360>
    <source src="Wildlife/Wildlife.ogv" type="video/ogg; codecs="theora, vorbis""/>
    <source src="Wildlife/Wildlife.webm" type="video/webm" >
    <source src="Wildlife/Wildlife.mp4" type="video/mp4">
    <p>Video is not visible, most likely your browser does not support HTML5 video</p>
</video>
<button onclick="play()">播放</button>
<button onclick="pause()">暂停</button>
```
JS代码如下：
```js
var myVid=document.getElementById("video");
// 监听视频播放结束的事件
myVid.addEventListener("ended",function(){
    alert("播放结束");
},true);
// 发生错误
myVid.addEventListener("error",function(){
    switch(myVid.error.code) {
        case 1: 
          alert("视频的下载过程被中止");
        break;

        case 2:
              alert("网络发生故障，视频的下载过程被中止");
        break;

        case 3:
              alert("解码失败");
        break;

        case 4:
              alert("不支持播放的视频格式");
        break;

        default:
              alert("发生未知错误");
        }
},false);

function play() {
    // 播放视频
    myVid.play();
}
function pause() {
    // 暂停播放
    myVid.pause();
}
```
>如上代码，默认的情况下是不自动播放，当我点击播放按钮时候，就播放，当我点击暂停按钮时候就停止当前的播放。
- canPlayType方法：
使用canPlayType方法来测试浏览器是否支持指定的媒体类型，该方法定义如下：
`var support = videoElement.canPlayType(type);`
videoElement表示页面上的video元素或audio元素，该方法使用一个参数type，该参数的指定方法与source元素的type参数的指定方法相同，用播放文件的MIME类型来指定，可以在指定的字符串中加上表示媒体编码格式的codes参数。
该方法可能返回的值如下所示：
1. 空字符串：表示浏览器不支持此种媒体类型。
2. maybe: 表示浏览器可能支持此种媒体类型。
3. probably： 表示浏览器确定支持此种媒体类型。
HTML代码如下：
```html
<video id="video" controls width=640 height=360>
    <source src="Wildlife/Wildlife.ogv" type="video/ogg; codecs="theora, vorbis""/>
    <source src="Wildlife/Wildlife.webm" type="video/webm" >
    <source src="Wildlife/Wildlife.mp4" type="video/mp4">
    <p>Video is not visible, most likely your browser does not support HTML5 video</p>
</video>
<button onclick="canPlayType()">canPlayType方法</button>
```

JS代码如下：
```js
var myVid=document.getElementById("video");
function canPlayType() {
    var support = myVid.canPlayType("video/webm");
    console.log(support); // maybe

    var support = myVid.canPlayType("video/ogg; codecs="theora, vorbis");
    console.log(support); // probably
}
```
### 事件
1. 事件处理方式

使用video元素或audio元素的事件方式有2种，第一种是监听的方式，使用video元素或audio元素的addEventListener方法来对事件的发生进行监听，该方法定义如下：
`videoElement.addEventListener(type,listener,userCapture);`

如上的监听方式和我们的JS的监听的方式一样，不做详细介绍，我们来看看第二种事件监听方式是为javascript脚本中常见的获取事件句柄的方式，如下所示：
```js
<video id=”” src=””  onplay=”begin()”></video>
function begin(){}
```

| 事件   |  描述   |
| :---: | :---: |
|   loadstart	  |    浏览器开始在网上寻找媒体数据   |
|progress| 浏览器正在获取媒体数据|
| suspend|	 浏览器暂停获取媒体数据，但是下载过程中并没有正常结束|
 |abort	| 浏览器在下载完全部媒体数据之前中止获取媒体数据，但是并不是由错误引起的|
 |error|	 获取媒体数据过程中出错|
 |emptied|	 暂不考虑，可以网上看|
 |stalled|	 浏览器尝试获取媒体数据失败|
 |play	| 即将开始播放，当执行了play方法时触发，或数据下载后元素被设为autoplay|
 |pause	| 播放暂停，当执行了pause方法触发|
 |loadedmetadata|	 浏览器获取完媒体的时长和字节数|
 |loadeddata	| 浏览器已加载完毕当前播放位置的媒体数据，准备播放|
 |waiting|	 可以暂不看|
 |playing|	 正在播放|
 |canplay|	 浏览器能够播放媒体，但估计以当前播放速率不能直接将媒体播完，播放期间需要缓冲|
 |canplaythrough|	 浏览器能够播放媒体，但估计以当前播放速率能将媒体播完，不再需要缓冲|
 |seeking|	 Seeking变为true，表示浏览器正在请求数据|
 |seeked|	 Seeking属性变为false，表面浏览器停止请求数据|
 |timeupdate|	 当前播放位置发生改变，可能是播放过程中的自然改变，也可能是人为改变，或由于播放不能连续而发生的跳变。|
 |ended|	 播放结束后停止播放|
 |ratechange	| DefaultplaybackRate属性或playbackRate属性发生改变|
 |durationchange|	 播放时长发生改变|
 |volumechange|	 volume属性或muted属性发生改变|
 	 
