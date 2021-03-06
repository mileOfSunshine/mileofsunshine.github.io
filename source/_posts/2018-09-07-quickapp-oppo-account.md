---
layout: post
title: 快应用-开通oppo账号服务艰辛之路
date: 2018-09-07
categories: 前端
tag: 快应用
---
最近在开发一个快应用，其实不能说是一个快应用啦，是一个用户登录注册的功能。其中需要涉及到oppo账号授权。

这个事情还得从两个帐号说起，`快应用开发者帐号`和`OPPO开发者帐号`。

## 快应用开发者帐号

首先先说下这个`快应用开发者帐号`，其作用就是绑定厂商和上传快应用。对于快应用帐号如何申请，官网和各个厂商的开发平台都有相应的文档教程，这里笔者就不介绍了。但有有一点需要提醒一下，快应用开发者帐号必须**完善资料**，审核通过了才能在顶部有**开发者中心**这一项，上面说到的**厂商帐号绑定**就在这个栏目里。如下图所示

![开发者中心](https://user-images.githubusercontent.com/17926741/98777983-9e6c7100-242c-11eb-8c3e-4b8a53535168.png)


## OPPO开发者帐号

这个帐号与`快应用开发者帐号`是多对一的关系，一个`快应用开发者帐号`只能绑定一个`OPPO开发者帐号`，但一个`OPPO开发者帐号`可对应多个`快应用开发者帐号`。至于这个OPPO开发者帐号怎么用呢，这里先保留。

先说下原由，为何故事因这两个帐号而起。

-------------------------------------

事情是这样的......

有一天，产品给我这两个帐号，我当时还啥也不懂，你给我就收下咯。直到开发到oppo授权阶段，我愣住了。

在`快应用开发者帐号`里，绑定的是oppo帐号A（如下图），但是给我的是oppo帐号B。这两个有啥关系！！我着急了，赶紧告诉我们产品，这两个帐号没有本质的联系，是不对的。他又找了给这两个帐号的相关人员，历经波折......······.......嗯。终于找到了oppo帐号A，此时的我已经感动的老泪纵横【夸张了】。

![厂商帐号绑定](https://user-images.githubusercontent.com/17926741/98777984-9f050780-242c-11eb-9890-8df1d61559b5.png)

所以正确的流程应该是用与`快应用开发者帐号`绑定的`OPPO开发者帐号`登录到OPPO平台进行**开通帐号服务**。【也许这里有人会吐槽，很简单的流程为何还写了这么多。嗯，他说的也不无道理，但是记录的目的就是防止有人也跟我一样，迷糊了一圈】

![开通帐号服务流程图](https://user-images.githubusercontent.com/17926741/98777968-9a405380-242c-11eb-99cc-e81de88fc7d2.png)



你是不是以为找到正确的oppo帐号A就ok。当时的我也跟你一样的想法！事实上呢，好吧！是我（们）想太多，事情并没有这么简单。

我调用`account.authorize`还是获取不到授权信息。重点是获取服务提供商`account.getProvider()`都获取不到，这个数据应该不需要什么多余操作就能获取到的呀。我试了好几个安卓设备都是一样的结果，空空如也~~~



组里就我自己接触了快应用，也无人可解问。接着我就开始各种加群，QQ群（`oppo开发者交流群`，微信群（`快应用官方技术交流群`）。我就各种问呀问呀，虽然回答很慢（毕竟大家都有自己的工作嘛），但是都很实用。群里有各个厂商的技术大佬在，最为活跃的还属华为的技术大大，很感恩，因为他，我接触到了oppo快应用相关的技术大佬，为后期工作打下了深厚的基础。

> “三人行，必有我师焉”

没错，接下来还是继续加群。接下来的第一个群，解决了无法获取服务提供商的问题。你猜是怎么解决的？答案很简单就是换了个快应用调试器。。

![what?](https://user-images.githubusercontent.com/17926741/98777980-9dd3da80-242c-11eb-9cda-af8c34cf5237.png)

原因是这样的，我这个oppo手机内置的快应用运行平台是2.1的灰度版本暂时不能用联盟官网的调试器，后来是oppo技术人员单独发我一个调试器进行调试，不过他也表名，此举只是临时方案，后面会支持联盟官网的调试器。**系统设置 - 应用管理 - 快应用**可查看快应用版本。


![oppo手机快应用的版本](https://user-images.githubusercontent.com/17926741/98777993-a0cecb00-242c-11eb-917c-2411693e2c6a.png)

好了，有了能调试的调试器。那就来看看账号服务是否能走通吧~

无奈还是不行，好在不像之前，已经有相关错误提示啦~
```
fail:   code = 200, data = generic error
```
可行官网也没介绍这200是啥错。那我就只能厚着脸皮问oppo技术大大了，好在遇到了一群暖心的小哥哥小姐姐，被告知：要将快应用先提交（不用一定要上架），此举只是为了能申请账号服务，这里顺便提一句，原生应用和快应用申请账号服务是分开的。看到后面你就会明白这句话是啥意思啦~

那接下来就是提交快应用。这次提交你可别提交一个到处bug的rpk。至少是功能完整，就差账号服务这一功能，提交完成就是坐等审核通过啦。建议你加下`快应用官方客服`的微信，这样有问题你可以直接问他，或是你的快应用提交有误之处，他们也可能及时联系到你，方便你们沟通。

![提交快应用](https://user-images.githubusercontent.com/17926741/98777986-9f9d9e00-242c-11eb-9b9a-598861fb7256.png)

·········.........·········经过漫长的审核，终于通过啦~

接下来就是讲讲申请账号服务啦

这里，我又加入了另一个群，没错就是关于账号服务的群。

由于目前oppo关于快应用开通账号的页面还没开放，相关技术给了我线上地址，这里就不展示了。页面内容与原生应用开通账号服务的模样一般无二。如下图所示，我们申请的就是**第一项推送能力项openid**。

![快应用开通账号服务](https://user-images.githubusercontent.com/17926741/98777974-9ca2ad80-242c-11eb-920d-4205fc70acec.png)


点击`立即开通`，里头长这样。

![开通openid](https://user-images.githubusercontent.com/17926741/98777973-9c0a1700-242c-11eb-8ff9-3e250dac8fec.png)

那红框这一块如何填写呢? `随便填`
这里笔者觉得有些粗糙了，相信今后这一块会完善好。这一点我个人觉得还是华为做的好一些，开放文档里也写的很明了。

![](https://user-images.githubusercontent.com/17926741/98777990-a0363480-242c-11eb-84f9-46e8c7af6aa3.gif)

走到这一步就差不多了，就等审核通过啦。
···
审核通过啦，当我再次调试页面时提示我更新到最新版，可能是我刚好赶上新版调试器发布。时间总是如此的巧合。。

![v1020](https://user-images.githubusercontent.com/17926741/98777976-9d3b4400-242c-11eb-8d72-72d515648b20.png)


更新完调试器，在调试页面。我真的感动了，调用账号授权接口拿到code了。。。(当时心情无语言表)

（完）



*此文只是记录快应用账号接入的过程，仅供借鉴，随着快应用的发展，各家厂商产品的完善，大家可能不会像笔者这般纠结，如能帮助到大家，庆幸写下此文。*


> 本文作者： mileOfSunshine
> 本文链接： https://mileofsunshine.github.io/2018/09/07/2018-09-07-quickapp-oppo-account/
> 版权声明：文章是原创作品。转载请注明出处！