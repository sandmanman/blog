---
title: H5皮秒游戏总结
date: 2016-12-26 16:22:12
categories: 项目总结
---


### 写在前面

前段时间公司接的一个小项目，开发在微信上传播的小游戏。

游戏的流程就是提供三种瑕疵的美女图片斑点、皱纹、纹身，需要用户点击来擦除斑点、皱纹、纹身，并计时统计总的擦除用时，来进行排名。

接到项目的具体需求，心里想到的就是我们经常见到的刮刮乐，刮开图层显示中奖信息，这个原理跟要开发的小游戏基本一致，及擦除掉有斑点皱纹纹身的图层显示完美的一层即可。虽然这种效果经常遇见，具体的代码实现没有去看过。


### 过程

首先，使用Canvas是肯定的了。能力的不过关，自己从0开始去写这个东西不太现实，网上搜到一些大神们写好的Demo，基本都能匹配这次开发的需求点。擦除瑕疵图层到指定百分比及显示完美图层并停止当前的计时。

找了几个例子，并在本地实现简单的Demo，实现过程中，也慢慢的发现跟自己开发不能匹配的问题，例如我需要这块的Canvas能够实现：

- 擦除动作（开始，结束）回调
- 自定义笔触大小
- 笔触边缘要柔和（虚化）
- 能够锁定画布，在达到指定擦除范围的百分比后我需要锁定画布，防止误操作
- 能够指定动作，项目只需要点击的动作，禁止手指移动来擦除

通过在Github上寻找，锁定了插件 [wScratchPad](https://github.com/websanova/wScratchPad) 来完成这次的开发。

具体使用：
```javascript
// 设置
$('#elem').wScratchPad({
    size        : 5,          // 笔触尺寸
    bg          : '#cacaca',  // 背景可以是颜色和图片
    fg          : '#6699ff',  // 前景可以是颜色和图片（也就是需要擦掉的层）
    realtime    : true,       // 实时显示已擦除百分比
    scratchDown : function(e, percent){
        console.log(percent);
    },
    scratchMove : function(e, percent){
        console.log(percent);
    },
    scratchUp   : function(e, percent){
        console.log(percent);
    },
    cursor      : 'crosshair' // 鼠标样式（可以是图片）
});

// 方法
$('#elem').wScratchPad('reset'); // 重置画布
$('#elem').wScratchPad('clear'); // 清除画布
$('#elem').wScratchPad('enabled', <boolean>); // 是否开启 默认true
```


### 遇到的问题
- 插件本身笔触是生硬的，去查了下Canvas的API把笔触修改为边缘羽化效果，`createRadialGradient`
- 项目只需要点击擦除，禁止了手指移动擦除，插件基础上增加了 `disabledMove: false` 选项

**处理音频(audio)遇到的问题**
音频资源是客户提供的网易云音乐的资源，根据网上的方法，找到了该资源的真实mp3地址，尽然2.4MB，真实很大啊，先不管大了。

音频这块，我一开始的思路：
关于[Audio](http://www.w3school.com.cn/jsref/dom_obj_audio.asp)

需要实现的是进入页面自动播放音乐，点击开关停止播放，音乐ICON停止转动。然后需要知道，音频资源当前状态是否可以用了，判断 `readyState` 的状态。然有点问题，我因该先去知道音频资源是否已经加载成功，才可以去知道它的当前状态说否可以播放。关于怎么知道加载成功，我参考了两篇文章 [怎么判断 audio video 是否加载完](http://kaifage.com/notes/87/audio-ready.html)，[判断音乐是否加载怎么做？](https://segmentfault.com/q/1010000007183637)。其实最后上线，音频这块的优化我并没有做，找的理由就是催的及，懒得搞，我先给你上线再说吧。

关于自动播放，在iPhone7的微信浏览器中是不自动播放的，于是又去查到这个 [轻松化解iOS系统及微信中不支持audio自动播放问题](http://webexp.cn/dlsd2016.html)，根据这个方法，自动播放的问题解决了。

关于音乐持续播放的问题，因为跨页面了，貌似让一首音乐持续接着播放好像不容易实现，去看了下API， `currentTime` 设置或返回音频中的当前播放位置（以秒计）。我想法是在跳转也时候把 currentTime 的值通过 URL 传到下一页去，在下一页中去指定 `audio` 的 `currentTime`。在我去查这个问题的时候，好像没有看到关于 `currentTime` 的思路，想想也是，如果跳转挺多页的话岂不是要玩儿死，所以这个思路肯定是行不通的。

有空我要先Demo下 `currentTime` 是个什么情况，及一些网易云音乐Web版是怎么做的。


### CSS3动画性能
发现使用 `box-shadow` 来做闪烁的效果，几个同时闪烁，会有明显的卡顿现象（小米4C），如果 用边缘羽化过的图片 来替换box-shadow的话，卡顿现象消失了。
这里有两篇关于CSS3动画性能的文章：

- [高性能 CSS3 动画](https://www.qianduan.net/high-performance-css3-animations/)
- [CSS动画之硬件加速](https://www.w3cplus.com/css3/introduction-to-hardware-acceleration-css-animations.html)


### 还可以更好的地方
- 图片资源的预加载
- 音频资源的预加载
- JS文件合并
- 手机上如果快速点击的话，擦除的速度会有明显的慢（小米4C）
- 页面布局也需要花点时间再去调试

### 吐槽
- 能力不够就是累啊，加油吧。
- 开发前再三的确认设计是否定稿，结果还是因为设计返工，整个重新整。WTF!!!
- 致所有的UI设计师们，提供的PSD不分组命名是几个意思，量少就不说了。TM 一大堆形似图层副本的玩意儿，太浪费别人的时间了，当代码搬运工也很累的。

[线上地址](http://www.searchsport.cn:8015/)，因为需要用户提供姓名和手机才可以进入，程序这块有明显的体验问题，比如第二天再来还是需要重新输入，而且程序走的是手机号不能重复，然并没有任何提示，输入不同的昵称相同的手机号，排行显示的是首次使用的昵称，解决记住登录状态的问题就OK了。

**最后最后，希望自己下次能更好吧，至少不犯过去的Bugs。**
