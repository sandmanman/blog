---
title: 'HTML5 audio 跨页面持续播放'
date: 2017-01-17 11:48:40
categories: 项目总结
tags: ['audio','localStorage']
---

### 写在前面
继 [上个H5活动](http://bestcsser.cc/2016/12/26/h5-pimiao-game-project-summary/) 关于音乐跨页面持续播放的问题，没有在上个项目里解决掉，妥协使用每个页面重新播放。
这次的H5活动同样也需要用到背景音乐。提前去搜索相关的文章，看能不能很好的解决这个问题。


### 过程
关于跨页面持续播放音乐的场景，第一个想到的就是网易云音乐Web版的音乐播放，看了源码具体的DOM结构就是播放器和页面是分开的，页面是使用iFrame来展现，即使跳转页面，
也只是在iFrame内跳转，这就解决了问题（我说的很肤浅）。

手机端显然是不能够用到iFrame的（一直很抵触使用iFrame）。只能再去查了。

`currentTime` 设置或返回当前播放位置（以秒计）。既然有了 `currentTime` ，问题是不是就是好解决了...

使用 `localStorage` 在跳转页面前把 `currentTime` 存储起来，再打开新的页面中，为音乐设置最新的 `currentTime`，这样以来音乐就能够在新打开的页面接着在上次跳转的时候播放了。

由于该活动是在朋友圈传播，之前有听说微信内置浏览器对 `localStorage` 的支持不够友好，关于这个问题查了很多文章，说的云里雾里，最后还是没结果。那就直接写吧。


### 遇到的问题及想到的
最后代码的实现及在手机端的测试（小米4C，iphone6s/iphone7最新版微信），都能够实现在新打开的页面接着在上次跳转的时候播放。虽然问题是解决了，但在实际中体验还是不够好，
每次打开新的页面，音乐都要重新加载一次，网络状况不好的情况，就很尴尬，等了许久音乐才继续播放，也可能在用户要进去下一个新的页面了，音乐还是没加载好。网络状况理想的
情况下，表现还是不错的。

最后再想想这个事情，最简单直接的实现就是 单页面 来开发。就不用为音乐的事情犯愁了，当然音乐文件越小越理想。

这是地址 [Fast-PK](http://coding-living.coding.me/Fast-PK-H5)


### 关于audio/video
- [video标签在不同平台上的事件表现差异分析](http://imweb.io/topic/560a6015c2317a8c3e086207)
- [HTML5 Audio-使用 Media 事件添加进度栏](http://www.xuanfengge.com/html5-audio-using-a-media-event-to-add-a-progress-bar.html)
