---
layout: post
title: 不为人知的Chrome devtools!
date: 2018-12-11
categories: 前端
tag: Chrome
---

说到chrome devtools 使用技巧，今天想和大家分享两个，网页截屏和快速引入并引用npm库。除此之外，还有一个大惊喜~

## 网页截屏

如何实现网页截屏，大家可能会想使用微信的截图工具就可以呀！是的，笔者往常也是这样截图的。那如何截取长页面的内容呢? 我想微信截图恐怕就无法实现了，这时候你可能会借用‘外力’，没错，我们可以使用软件[FastStone Capture](https://faststone-capture.en.softonic.com/)。那有没有不借助‘外力’也能截图长图呢，答案是肯定的。 

![FastStone Capture 滚动截图](https://user-images.githubusercontent.com/17926741/98777152-323d3d80-242b-11eb-8e8a-9466db06b41c.png)

那如何利用devtools实现截图呢？

<!--more-->

1、`F12`打开开发者工具;
2、利用快捷键 `ctrl + shift + p`调出命令行;
3、输入screen，选择 `capture full size screenshot`(捕捉全尺寸屏幕截图)

锵锵~~ 生成当前页面的长图。是不是很惊喜，执行一个命令就完成了。
![全尺寸效果图](https://user-images.githubusercontent.com/17926741/98777151-310c1080-242b-11eb-94e0-09efc99cc84c.png)


有时你可能只想截取一部分内容，但内容又有些长。这时`capture full size screenshot` 和 `capture screenshot` 都不满足需求，别忘了，我们还有`capture node screenshot`。

![捕捉节点屏幕截屏](https://user-images.githubusercontent.com/17926741/98777133-2c475c80-242b-11eb-8ff7-0d1ffecb1be4.gif)


*温馨提示*：
`capture full size screenshot` 捕捉整个网页屏幕截图
`capture node screenshot` 捕捉节点网页屏幕截图
`capture screenshot` 捕捉当前屏幕截图


## 快速引入并应用npm库

有时候我们想学习一个库( 很有兴致的那种 )，可能往往就在环境搭建的过程中放弃了学习( 苦笑 )。今天介绍的这个想必能解决你的痛点！

1、获取并添加Chrome扩展程序：[Console Importer](https://chrome.google.com/webstore/detail/console-importer/hgajpakhafplebkdljleajgbpdmplhie/related)
2、如果你想练习下 `lodash` 里的方法，你可以执行 `$i('lodash')` 将 `lodash` 库引入，接下来就可以愉快把玩工具库啦。对于学习使用某个工具库真的很方便，有木有~

![lodash练习](https://user-images.githubusercontent.com/17926741/98777128-2b162f80-242b-11eb-910d-8598adfc306d.png)

### 你可能会遇到的问题?

**Q1: `$i` 不像预期的那样起作用**
一些网站可能已经把 `$i` 作为一个全局变量，这个扩展程序就无法重写覆盖它，这时你可以 `console.$i` 这样调用，也是一样的效果。

**Q2: `$i` 无法导入资源**

![lodash导入报错](https://user-images.githubusercontent.com/17926741/98777131-2baec600-242b-11eb-9ee3-1bf736bf440f.png)

出错的原因是这些网站采用了严格的内容安全政策(`Content Security Policy`)。你可以打开一个本地html，或者是用一个没有进行严格内容安全政策的网站进行 `$i('lodash')` 练习

## 彩蛋

想必大家在网上也看到过很多介绍console方法的文章，笔者觉得有些碎片化了。故借此机会，向大家推荐阅读[devtools的开发者手册](https://developers.google.com/web/tools/chrome-devtools/)（感觉是宝藏, 虽然有一定的学习门槛）

![如何找到devtools开发者手册](https://user-images.githubusercontent.com/17926741/98777158-336e6a80-242b-11eb-90a4-ed3fc61a7f9e.png)

> 使用 Console API 可以向控制台写入信息、创建 JavaScript 配置文件，以及启动调试会话。

- [Console API Reference](https://developers.google.com/web/tools/chrome-devtools/console/console-reference)介绍了`assert, clear, debug, dir, dirxml, error, group, groupCollapsed, groupEnd, info, log, profile, profileEnd, time, timeEnd, timeStamp, trace, warn`的使用，也提供了相关实例。

> Command Line API 包含一个用于执行以下常见任务的便捷函数集合：选择和检查 DOM 元素，以可读格式显示数据，停止和启动分析器，以及监控 DOM 事件。

- [Command Line Reference](https://developers.google.com/web/tools/chrome-devtools/console/command-line-reference)一共介绍了21个方法: 
`$_, $0 - $4, $, $$, $x, clear, copy, debug, dir, dirxml, inspect, getEventListeners, keys, monitor, monitorEvents, profile and profileEnd, table, undebug, unmonitor, unmonitorEvents, values`

我猜有人犯懒了，链接都懒得打开啦...
![别跑，快回来学习！](https://user-images.githubusercontent.com/17926741/98777123-29e50280-242b-11eb-9ddb-a4d60c3318a2.gif)

那我就把 `devtools` 手册目录贴出来啦，说不好一看又感兴趣了呢~

![开发者手册目录](https://user-images.githubusercontent.com/17926741/98777154-323d3d80-242b-11eb-911a-6b66ea0d2805.png)

单词我们都认识，能不能学好就靠你自己啦...前面推荐的`Console API Reference`和`Command Line Reference` 已经是中文文档了哈，莫慌莫慌...

![加油！](https://user-images.githubusercontent.com/17926741/98777155-32d5d400-242b-11eb-9a0d-91a3854d0450.jpg)
