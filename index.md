---
layout: default
---


### 引言
随着网页功能变得愈发精细和复杂，以及手机端的硬件性能瓶颈，网页的运行时性能问题变得越来越突出。而用户对于网页运行时性能最直观的感受，莫过于UI操作的流畅程度。流畅或卡顿，爽或不爽，皆取决于每个UI动画细节之间。本文旨在帮助理解动画卡顿与流畅的原因，卡顿问题的调试方法，以及从实践中总结出实现流畅动画的规律。为构建操作流畅的网页提供参考。


### 索引
* [量化动画性能](#section-2)
* [调优策略](#section-8)
* [实践经验](#tbd)


## 量化动画性能
[动画](http://zh.wikipedia.org/wiki/动画)的实现原理，是利用了人眼的“[视觉暂留](http://zh.wikipedia.org/wiki/視覺暫留)”现象，在短时间内连续播放数幅静止的画面，使肉眼因视觉残象产生错觉，而误以为画面在“动”。

##### 动画相关的几个概念
* **帧**：在动画过程中，每一幅静止画面即为一“帧”。
* **[帧率](http://zh.wikipedia.org/wiki/帧率)**：即每秒钟播放的静止画面的数量，单位是fps(Frame per second)。
* **帧时长**：即每一幅静止画面的停留时间，单位一般是ms(毫秒)。
* **跳帧(掉帧/丢帧)**：在帧率固定的动画中，某一帧的时长远高于平均帧时长，导致其后续数帧被挤压而丢失的现象。

##### 生活中常见的帧率(频率)：
* 10 FPS		达成基本视觉暂留
* 25～30 FPS		传统广播电视信号
* 60～85 HZ		显示器刷新频率
* 100 HZ		日光灯管闪烁频率

##### 帧率能反映动画的流畅程度
* 在网页中，帧率能够达到50~60fps的动画将会相当流畅，让人倍感舒适。
* 帧率在30～50fps之间的动画，因各人敏感程度不同，舒适度因人而异。
* 帧率在30fps以下的动画，让人感觉到明显的卡顿和不适感。

##### 用帧率量化流畅度的几个例子
* 帧率稳定在60fps左右的流畅动画

<div style="width: 300px; /* margin: 0 auto; */">
    <iframe width="100%" height="200" src="http://fiddle.jshell.net/Alexorz/KqzeV/show/light/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
</div>


* 帧率稳定但低于30fps的卡顿动画

<div style="width: 300px; /* margin: 0 auto; */">
    <iframe width="100%" height="200" src="http://fiddle.jshell.net/Alexorz/KqzeV/show/light/?mode=lowfps" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
</div>


* 平均帧率高，但存在跳帧现象的卡顿动画

<div style="width: 300px; /* margin: 0 auto; */">
    <iframe width="100%" height="200" src="http://fiddle.jshell.net/Alexorz/KqzeV/show/light/?mode=jumpframe" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
</div>


##### 几款帧率监测工具

* [Stats.js](https://github.com/mrdoob/stats.js)，侦听全局或指定位置的帧率，JS实现，所有浏览器可用

<p style="padding-left: 30px; vertical-align: top;">
	<img src="images/stats-panel.png"  height="48" width="160" alt="Stats.js panel">
</p>


* Chrome自带的帧率监测工具，用于侦听全局帧率，以及页面重绘耗时


<p style="padding-left: 30px; vertical-align: top;">
	<img src="images/chrome-fps-panel.png" style="vertical-align: top; margin-right: 20px;" height="71" width="185" alt="Chrome fps panel">
	<img src="images/chrome-ms-panel.png"  style="vertical-align: top;" height="85" width="185" alt="Chrome ms panel">
	<!-- ![Chrome fps panel](images/chrome-fps-panel.png =185x71)
	![Chrome ms panel](images/chrome-ms-panel.png =185x85) -->
</p>

![Chrome Console](images/rendering-panel-skitched.png)


* Chrome Timeline，杀手级监测 & 调试工具

![Chrome Console](images/chrome-timeline-skitched.png)


## 动画调优策略
* **提升每一帧性能（缩短帧时长，提高帧率）**
	* 避免重排。
	* 合理规划“层”，动静分离，避免大面积重绘。
	* 推荐使用`transform: perspective(px) translate3d(x, y, z);`实现移动、缩放等动画，3D变换效果不发生重绘与重排，性能更好。
* **保证帧率平稳（避免跳帧）**
	* 不在连续的动画过程中做高耗时的操作（如大面积重绘、重排、复杂JS执行），避免发生跳帧。
	* 若高耗时操作无法避免，可以尝试以下用二种做法化解：
		1. 将高耗时操作放在动画开始和结尾处
		2. 将高耗时操作分摊至动画的每一帧中处理（保持帧率平稳）
* **移动端低性能设备优先调试**
	* Android设备优先调试：移动端设备中，iOS设备普遍比Android设备的硬件性能高，因此Andorid设备下更容易出现性能问题，而且Android设备可以借助Chrome的远程调试工具方便调试动画性能，所以优先调试Android设备可以更早地发现并解决问题。
* **避开疑难杂症**
	* 不建议异步载入CSS：Webkit系浏览器中，异步载入CSS文件会使页面渲染停顿（页面中存在持续pending的CSS请求时十分明显）。

<!-- 
## 卡顿问题调试 -->

## 实践经验（TBD）



<script src="http://0.0.0.0:4444/livereload.js"></script>
