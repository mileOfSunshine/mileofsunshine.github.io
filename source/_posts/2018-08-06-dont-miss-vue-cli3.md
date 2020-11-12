---
layout: post
title: 什么？错过了Vue CLI 2！你还要错过Vue CLI3？
date: 2018-08-06
categories: 前端
tag: vue
---


# Vue CLI 产生的背景

在`Vue CLI`出现之前，你可能要花费好几天的时间搭建项目的开发环境，如果你事先不了解`webpack`，你可能会又花费大把的时间熟悉`webpack`。就这样，一周过去了，你的项目还没有真正启动起来。
为了让开发者从纠结配置中解放出来，专注于撰写应用程序。`Vue CLI`也就因此而产生。它不仅确保了各种构建工具能够基于智能的默认配置即可平稳衔接，还提供了配置调整的灵活性。

# Vue CLI历史
`Vue CLI `到目前为止经历了两个大版本，`CLI 2` 和 `CLI 3`。很多人可能会好奇从 `CLI 2`升级到`CLI 3`会有哪些新的改变，接下来就一边回顾`CLI 2`，一边为大家解读`CLI 3`的新特性。

# 创建一个项目

`CLI 2`和`CLI 3`第一个区别是npm包的包名，`CLI 3`并没有沿用`CLI 2`的`vue-cli`，而是另起为`@vue/cli`。创建项目方面也发生了变化，`CLI 2` 可以选择根据模板初始化项目，而`CLI 3`并未重新开发模板，如果开发者想要像`CLI 2`一样使用模板初始化项目，可全局安装一个桥接工具`@vue/cli-init`。

1、`CLI 2` 全局安装并创建项目

``` bash
npm install -g vue-cli
# 或者
npm install -g vue-cli@x.x.x // 指定安装某一版本

vue init <template-name> <project-name>
# 例如：vue init webpack vue-test
```
*注意：这里的`CLI 2`是2.9.6。*

`<template-name>`：表示模板名称，可以通过vue list查看可用的模板，在这里官方提供了6种模板，分别为：
*   `browserify`——一个全面的`Browserify + vueify` 的模板，运行起来带有热重载，保存时 lint 校验，单元测试。
* `browserify-simple`——一个简单`Browserify + vueify`的模板，不包含其他功能，让你快速的搭建vue的开发环境。
* `pwa`——一个基于`webpack`模板的渐进式的网页应用程序模板。
* `simple`——一个最简单的单页应用模板。
* `webpack`——一个全面的`webpack + vue-loader`的模板，运行起来带有热重载，保存时 lint 校验和CSS扩展。
* `webpack-simple`——一个简单`webpack + vue-loader`的模板，能让你快速搭建一个vue的开发环境。

![官方提供的6种模板](https://user-images.githubusercontent.com/17926741/98778990-1f783800-242e-11eb-8681-f33344d7a0d5.png)

初始化过程中会确认项目的项目名、作者等信息，大家可根据需求自行修改。

2、`CLI 3`全局安装并创建一个项目
``` bash
npm install -g @vue/cli
# 或者
yarn global add @vue/cli
# 创建项目
vue create <project-name>
# 例如：vue create vue-3.0-demo
# 或
# 使用init 初始化项目
npm install -g @vue/cli-init
# `vue init` 的运行效果将会跟 `vue-cli@2.x` 相同
vue init webpack vue-3.0-demo
```

当我们用`CLI 3`的方式创建项目，输入`vue create vue-3.0-demo`命令后，你会发现在创建项目的路上总是有位“记者大哥”横路拦截，问你这问你那，你还必须做出选择。

--------------------------------------------

记者大哥：“欢迎进入`CLI 3`的世界，首先你得选取一个 preset。选择默认的设置可以快速创建一个新项目的原型，而手动设置则提供了更多的选择。你是选择默认配置，还是手动选择特性呢？”

你：（心里活动：`“来都来了，为何不看看记者大哥到底搞什么鬼”`）“我选择了手动选择属性”

![手动选取特性](https://user-images.githubusercontent.com/17926741/98778969-1a1aed80-242e-11eb-9d8c-d042ec636c42.png)

你：什么鬼？还给我来个多选题！首先`Babel`是必要的，不然拿什么来转换`ES6`语法，`TypeScript`？不会，略过。渐进式的程序应用，暂时也不涉及这个。`Router`勾上，作为一个移动M站，得有人来管理路由呀。`Vuex`一个状态管理器，后期要用再加上吧，反正也跑不了。css 预处理器，习惯使用`Less`，也加一。`Linter / Formatter`也加一，作为一个团队，没有人统一代码风格可不行。最后两个分别是单元测试和端对端测试，这里我就不加上了，没用过，期待今后有大神分享。

选择完特性后，你以为就结束了，没想到，一步选错步步要你选。

对于css预处理器方面，你毅然决然选择了`Less`；
但`linter / formatter` 配置，你懵逼了。这都是什么？？记者大哥介绍了一下：

[ESLint](https://github.com/eslint/eslint) 是一个语法规则和代码风格的检查工具，可以检测出你代码中潜在的问题，可以保证写出语法正确和风格统一的代码。
![选择linter配置.png](https://user-images.githubusercontent.com/17926741/98778993-1f783800-242e-11eb-8d17-c1f3e8c235a6.png)

* `ESLint with error prevention only`——只检测错误。
* `ESLint + Airbnb config`——独角兽公司的[Airbnb](https://github.com/airbnb/javascript)，有人评价说“这是一份最合理的JavaScript编码规范”，它几乎涵盖了JavaScript的各个方面。
* `ESLint + Standard config`——[standardJs](https://github.com/standard/standard)一份强大的JavaScript编码规范，自带linter和自动代码纠正。没有配置。自动格式化代码。可以在编码早期发现规范问题和低级错误。
* `ESLint + Prettier`—— [Prettier](https://github.com/prettier/prettier) 作为代码格式化工具，能够统一整个团队的代码风格。

等他介绍完，你心里大概有点谱了，这里你选择了 `ESLint + Standard config`。

lint有两种检查时机，一是用户保存文件的时候，二是用户提交文件到git的时候。你就选了` Lint on save`，有错及时解决嘛。

终于“记者大哥”告诉你接下来这个问题是最后一个问题咯。

![](https://user-images.githubusercontent.com/17926741/98778974-1be4b100-242e-11eb-9132-05d0209ea4c7.png)

记者大哥：你是喜欢把Babel、ESLint等配置信息全放在package.json文件里呢，还是单独文件管理？

你：一个一个文件比较好，根据文件名就知道这是谁的配置，方便维护。

记者大哥：那你是否想把今天你手动选择的preset保存为未来项目的preset呢？

你：
![说好的最后一个呢！！](https://user-images.githubusercontent.com/17926741/98778976-1c7d4780-242e-11eb-9142-ba662c760b2c.png)

......保存！

---------------------------------------------------

**温馨提示**：*如果你是用windows，在进行创建项目的时候，最好使用cmd，在cmd里你可以通过箭头上下选择和空格选中。如果你用git bash 可能会出现箭头和空格都没有请选择和选中作用。*

这里通过一个漫长的对话我们自定义的一个preset，此时如果你需要创建新工程，这时候你就会发现多了一个preset，就是最初你自己设置的。你可以选择自己之前保存preset的，也可以再次开启“采访模式”。

![新添的preset](https://user-images.githubusercontent.com/17926741/98778971-1ab38400-242e-11eb-883f-257ca1a322c1.png)

## CLI 2 的项目结构
![vue cli 2.9.6项目结构.png](https://user-images.githubusercontent.com/17926741/98778966-18e9c080-242e-11eb-87df-eec44d66af28.png)
对于`CLI 2`这个项目结构，主要的也是最重要的在于bulid和config者两个目录。bulid是项目构建的相关代码，config是项目开发环境配置。

接下来就先从`webpack.base.conf.js`开始依次介绍build和config两个目录下的相关功能。

### webpack.base.conf.js
`webpack.base.conf.js`是webpack的基础配置，是dev和prod的公共配置文件。

``` js
const path = require('path')
const utils = require('./utils')
const config = require('../config')
const vueLoaderConfig = require('./vue-loader.conf')
```

* path——该模块提供了一些用于处理文件路径的小工具
* utils——给整个CLI提供方法
* config——开发环境的配置
* vueLoaderConfig——分析是否是生产环境，然后将根据不同的环境来加载配置功能

在这个文件里一共实现了两个方法。一是合并path路径的，另一个是创建Eslint的Rules。而剩余部分就是webpack的基础配置，这里简化了webpack结构，简化的结果其实就是webpack的一个骨架，如果在配置上遇到问题，可去[webpack](http://webpack.github.io/)查证。
``` js
...
module.exports = {
  entry: {}, // 编译入口文件
  output: {}, // 编译输出路径
  resolve: {}, // 一些解决方案配置
  module: {
    // 不同类型文件加载器配置
    rules: []
  },
  ...
  // 这些选项用于配置polyfill或mock某些node.js全局变量和模块。
  // 这可以使最初为nodejs编写的代码可以在浏览器端运行
  node: {...}
}
```
关于path有兴趣的可前往[node](https://nodejs.org/api/path.html)学习，接下来重点介绍下`utils.js`，`config`和`vue-loader.conf`。

### utils.js
utils.js文件中总共实现了4个方法：`assetsPath`、`cssLoaders`、`styleLoaders`、`createNotifierCallback`。

* assetsPath——返回不同环境下的static目录位置
* cssLoaders——为不同的css预处理器提供一个统一的生成方式
* styleLoaders——为那些独立的style文件创建加载器配置
* createNotifierCallback——以类似浏览器的通知的形式展示信息

### config
config关键文件是`index.js`。这个文件是开发环境和生产环境的基本配置。在这个文件里开发者可在dev设置开发环境的静态路径、本地服务器配置项、Eslint、SourceMaps和代理，也可在build设置生产环境是否开启gzip压缩，以及压缩后缀名的设置等。
``` js
...
module.exports = {
  dev: {...},
  build: {...}
}
```
### vue-loader.conf
这个文件的内容相对比较少。首先，vue文件中的css loader将在生产环境下把css文件抽取到一个独立的文件中；其次是根据不同的环境，引入不同的source map配置文件；最后设置是否开启缓存破坏。
``` js
'use strict'
const utils = require('./utils')
const config = require('../config')
const isProduction = process.env.NODE_ENV === 'production'
const sourceMapEnabled = isProduction
  ? config.build.productionSourceMap
  : config.dev.cssSourceMap

module.exports = {
  loaders: utils.cssLoaders({
    sourceMap: sourceMapEnabled,
    extract: isProduction
  }),
  cssSourceMap: sourceMapEnabled,
  cacheBusting: config.dev.cacheBusting,
  transformToRequire: {
    video: ['src', 'poster'],
    source: 'src',
    img: 'src',
    image: 'xlink:href'
  }
}
```
关于webpack公共配置讲完了，接下来我们就一起学习下在dev和prod环境各自的配置吧。

### webpack.dev.conf.js

这个文件引入了`webpack-merge`，意在将公共配置文件和dev配置合并。从代码里我们可以发现，dev环境又新增了一些配置项。
* 给独立的style文件添加了sourceMap功能，有了它，出错的时候，除错工具将直接显示原始代码，而不是转换后的代码。
* 引入devtool。
* 配置devServer，包括热部署、代理、启动程序的时候自动在浏览器打开主页面等。
* 新增一些插件，包括热替换、`webpack.NamedModulesPlugin`在热加载时直接返回更新文件名、`html-webpack-plugin`生成html文件等。

最后一个函数是为了确保启动程序时，如果端口被占用时，会通过portfinder来发布新的端口。

``` js
...
const devWebpackConfig = merge(baseWebpackConfig, {
  module: { ... },
  devtool: config.dev.devtool,
  devServer: { ... },
  plugins: [ ... ]
})

module.exports = new Promise((resolve, reject) => { ... })
```

### webpack.prod.conf.js
相比 `webpack.dev.conf.js`，这个文件多引入了几个依赖，主要是为了压缩CSS和JS。在文件配置上多了一个output，将js文件打包成多个chuck，用hash值命名，来解决缓存策略。

到这里`CLI 2`的整个配置也就接近尾声了。剩下的还有`check-version.js`和`bulid.js`两个文件。

### check-version.js
这个文件主要是用来检测当前环境中的node和npm版本和我们需要的是否一致的。

### bulid.js
这个文件刚开始通过`check-versions`判断当前的node和npm版本号，如果现有的npm或者node的版本比定义的版本低，则生成一段警告。接下来，先删除打包目标目录下的文件，再进行打包，直至打包完成。

我们走马观花的学习了`CLI 2`的配置，估计大家也都累了。那接下来就来一段采访吧~~期待不，哈哈。

## CLI 3的项目结构

![CLI 3项目结构.png](https://user-images.githubusercontent.com/17926741/98778997-20a96500-242e-11eb-9165-96230508123b.png)

从`CLI 3`的整个项目结构我们可以发现，这个结构很简单，没有相关的配置文件或复杂的目录结构。`CLI 3`仅生成构建应用程序所需的文件，让使用者不用关心这些工具的具体配置，从而降低了工具的使用难度。

其实通过阅读`CLI 3`的官方文档，你可能已经知道，官方内置了一个CLI服务（`@vue/cli-service`），作为一个开发环境的依赖，局部安装在`@vue/cli`创建的项目中。如果你真想修改webpack的相关配置，可在项目的根目录下（和`package.json同级`）创建一个vue.config.js配置文件，这个文件一旦存在就会被`@vue/cli-service`自动加载。也可直接使用`package.json`中的`vue`字段。

> 一个没有好奇心的程序猿，不是一个更好的程序猿。

如果你已经满足于官方的介绍，那也就到此结束漫长的阅读之旅啦（偷偷告诉你后面还有新特性的精彩内容）。如果你也像我一样，充满了好奇心，就跟我再去探索一番。

从`CLI 2`到`CLI 3`，初期可能没有官方文档。如果你真想探个究竟，可以从启动项目入手。

`CLI 2`启动方式是`webpack-dev-server --inline --progress --config build/webpack.dev.conf.js`
这里用`webpack-dev-server`搭一个服务。
* --inline：启动inline模式来自动刷新页面
* --progress：显示打包的进度
* --config build/webpack.dev.conf.js：指定要用的是哪个配置文件

`CLI 3`启动方式是`vue-cli-service serve`

`vue-cli-service`就是CLI服务，你可全局搜索一下，位于`node_modules\@vue\cli-service\bin`

### vue-cli-service.js
``` js
#!/usr/bin/env node

const semver = require('semver')
const { error } = require('@vue/cli-shared-utils')
const requiredVersion = require('../package.json').engines.node

...
const Service = require('../lib/Service')
const service = new Service(process.env.VUE_CLI_CONTEXT || process.cwd())

const rawArgv = process.argv.slice(2)
const args = require('minimist')(rawArgv)
const command = args._[0]

service.run(command, args, rawArgv).catch(err => {
  error(err)
  process.exit(1)
})
```
这个文件首先是判断了当前node的版本和`vue-cli-service`要求的版本是否一致，如果版本太低就得升级node版本。

紧接着就起了个服务，这个服务是位于`lib/Service`。

### Service.js

``` js
...
loadUserOptions () {
    // vue.config.js
    let fileConfig, pkgConfig, resolved, resovledFrom
    const configPath = (
      process.env.VUE_CLI_SERVICE_CONFIG_PATH ||
      path.resolve(this.context, 'vue.config.js')
    )
    ...

    // package.vue
    pkgConfig = this.pkg.vue
    ...

    if (fileConfig) {
      if (pkgConfig) {
        ...
      }
      resolved = fileConfig
      resovledFrom = 'vue.config.js'
    } else if (pkgConfig) {
      resolved = pkgConfig
      resovledFrom = '"vue" field in package.json'
    } else {
      resolved = this.inlineOptions || {}
      resovledFrom = 'inline options'
    }
    ...
    return resolved
  }
}
...
```
在`loadUserOptions`这个函数中，你可以看到官方提到的`vue.config.js`。
这个函数主要是加载用户的配置。如果`vue.config.js`和`package.json`的vue字段同时存在，会忽略`package.json`的vue字段配置，而选取`vue.config.js`的配置。

这里粗略介绍了何处加载了  vue.confg.js  文件，有兴趣可以继续深究。经过安装CLI、创建项目到整个项目结构介绍，我们可以大致了解了两者的区别。接下来大家一起围观一下`CLI 3`给我们带来的哪些新特性吧~~~


## 新特新

### CLI插件的出现
据我所知，在`CLI 3`之前是没有CLI插件这个概念的，人们在开发Vue项目时，若是需要实现功能都是引用npm的相关包。`CLI 3`的出现，带来了CLI插件这个概念，也带来了统一的命名方式：`@vue/cli-plugin-`（内建插件）/ `vue-cli-plugin-`（社区插件）开头。

![CLI 3出现前包名](https://user-images.githubusercontent.com/17926741/98779806-45eaa300-242f-11eb-9452-1145566cbd20.png)

![CLI插件](https://user-images.githubusercontent.com/17926741/98779816-46833980-242f-11eb-89ad-743d43889e14.png)

### 即刻创建原型

有时候你想快速创建一个原型，不需要添加一大堆样板。Vue CLI就提供了一个运行原型的开发服务器。

要想使用这个开发服务器，前提是安装`@vue/cli-service-global`

``` bash
npm install -g @vue/cli-service-global
```
你可以用IDE创建.vue文件，并添加vue代码。如果你对命令行掌握良好，也能轻松创建。

``` bash
echo 'hello world' > src/views/HelloWorld.vue
```
然后将HelloWorld.vue 修改为标准的vue文件结构就行。

``` html
<template>
  <div>hello world!</div>
</template>

```

紧接着你就可以运行`vue serve src/views/HelloWorld.vue` 就能看效果啦~

![快速原型开发](https://user-images.githubusercontent.com/17926741/98778982-1dae7480-242e-11eb-9923-5b8b51a69878.png)


###  配置时无需Eject

如果你曾经是一位React的忠实用户，或许使用过[create-react-app](https://github.com/facebookincubator/create-react-app)（react的脚手架），那你对eject的理解可能就很深刻了。可惜小女不才，早期与React只有一面之缘，也就没此机会接触`create-react-app`。为了理解eject到底是何物，我查看了react的相关文档，终于明白了。
在react中，使用CRA（ `create-react-app`简称）创建完项目，我们可以在package.json看到这里一个script命令。
``` json
"scripts": {
  "eject": "react-scripts eject"
}
```
执行完`npm run eject`会将封装在CRA里的配置全部*反编译*到当前项目，换句话就是把之前好不容易藏好了config文件暴露出来了，用户也就获取到了控制权，想怎么改随你。这样`react-scripts`就以文件的形式存在于项目中，就无法升级啦。
```
# eject 后项目根目录下会出现 config 文件夹，里面就包含了 webpack 配置
config
├── env.js
├── jest
│   ├── cssTransform.js
│   └── fileTransform.js
├── paths.js
├── polyfills.js
├── webpack.config.dev.js // 开发环境配置
├── webpack.config.prod.js // 生产环境配置
└── webpackDevServer.config.js
```
**好在`CLI 3`并没有像CRA一样，开发者你要是想自己修改配置，也是可以的，我不需要你eject，你想改就去vue.config.js里改吧。**
![](https://user-images.githubusercontent.com/17926741/98778972-1b4c1a80-242e-11eb-9249-4a803f012815.png)

如果你想看看默认的webpack配置，可执行`vue inspect`查看，默认情况下，会将配置输出到控制台，你也可以将结果指向一个文件，例如：`vue inspect > webpack.config.js`。

### 使用图形化界面

图形界面化能快速引导用户创建和管理项目，这对于习惯了图形界面化的开发者来说简直就是福音。操作方式很简单，在你安装好了`CLI 3`的前提下，执行`vue ui`命令 ，会自动在浏览器打开图形界面。

![Vue CLI图形界面](https://user-images.githubusercontent.com/17926741/98778984-1e470b00-242e-11eb-9d43-2094ec4ed4f3.png)

创建项目的整个流程跟命令创建项目大同小异。这里介绍下一个**“导入”**功能，正常情况下不是用这个图形界面创建的项目是不会出现在项目列表中，如果你想把已有的项目也放在Vue CLI 图形界面管理，可将项目导入进来。导入项目到底有什么好处呢？这里就厉害了！一个项目导入进来，会按四个部分进行管理项目，分别为插件、依赖、配置和任务

##### 插件
在插件这一部分罗列了项目已安装的CLI插件，除此之外，用户还可以随时添加插件，默认推荐给你添加vue-router和vuex。更棒的是还未每个插件提供了官方文档，是不是很贴心~

##### 依赖
我们都知道依赖分为开发依赖和运行依赖，在图形界面未出现时，我们要移除不用的依赖还得根据环境进行移除，移除开发依赖执行`npm uninstall xx --save-dev`，移除运行依赖`npm uninstall xx --save`。移除之前你可能都要去package.json里查看相关依赖名，很是费劲。还在图形化界面解决了这个问题，轻轻松松你就可以移除不需要得依赖。

##### 配置
关于vue.config.js文件的配置，在这也提供了入口。这里它分为基础配置和CSS配置，别觉得少，但这却是最基本、最经常被修改的配置。如果无法满足你根本需求，你也可以打开vue.config.js文件并参考官方配置。除此之外也提供了ESlint的相关配置。

##### 任务
笔者觉得，厉害的还是任务这一栏。这一栏目包含了`serve`、`build`、`lint`和`inspect`。项目的运行情况、数据分析、编译进度、检查webpack配置等。真的很厉害有木有~
![所有信息尽收眼底](https://user-images.githubusercontent.com/17926741/98778978-1d15de00-242e-11eb-8fe3-bedb153157de.png)


新特性到此就介绍完毕了。初次写一篇这么长的文章，如有不足，请多见谅。感谢查阅~~
![](https://user-images.githubusercontent.com/17926741/98778988-1edfa180-242e-11eb-8bd1-4d078318ac8c.png)


参考资料：

https://cli.vuejs.org/zh/guide/
https://npmjs.com/package/vue-cli
http://zhaozhiming.github.io/blog/2018/01/08/create-react-app-override-webpack-config/
https://segmentfault.com/a/1190000012581869#articleHeader5
http://www.css88.com/archives/8405