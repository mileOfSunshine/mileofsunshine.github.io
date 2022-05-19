---
layout: post
title: 看keep-alive如何在项目中失效
date: 2022-05-11
categories: 前端 
tags: Vue3
---

![banner](https://p4.ssl.qhimg.com/t012fd27e9ca230afe2.jpg)

keep-alive 是vue内置的一个组件，其作用是缓存不活动的组件。众所周知，一般在组件进行切换时，默认会进行销毁。若想在组件切换时保存原有的状态，那就需要利用 keep-alive 来实现。

keep-alive 的用法也不复杂，[官网](https://v3.cn.vuejs.org/api/built-in-components.html#keep-alive)也做了说明，并给了示例。但有时规规矩矩的用了就是不生效。相信大家也会有这样的烦恼，接下来小编就列举下容易造成 keep-alive 失效的几个点。

## 案例

1. 关键词没拿捏好，一行注释都能让你忙活一上午。

`<keep-alive>` 是用在其**一个直属**的子组件被切换的情形。“**一个**”、“**直属**”这两点很好理解，却容易被忽视。有时为了提高代码可读性，我们会做个注释。而这个注释位置要是不对，违反了“**一个**”原则，就能造成 `keep-alive` 失效。

```html
<!--掉坑，失效-->
<keep-alive>
  <!-- 注释也会是keep-alive失效，删除本条注释功能正常 -->
  <component :is="view"></component>
</keep-alive>
```
关于“**直属**”，或许你能拍胸脯告诉我你不可能因为这点翻车。我承认在刚开始开发项目时，开发者都头脑清晰，加上有官方文档的辅助能很好的规避掉错误点。但随着项目战线拉长，快速的需求变更迭代，难保你在实现功能过程中会不小心掉入了“**直属**”的坑中。

```html
<!--掉坑，失效-->
<keep-alive>
  <div style="margin: 10px;">
    <component :is="view"></component>
  </div>
</keep-alive>
```

2. `<keep-alive>` 直属子组件中使用了 `v-for`。

```html
<!--失效-->
<keep-alive>
  <template v-for="tab in tabs" :key="tab">
    <component :is="`theme${tab}`" v-if="tab === curTab"/>
  </template>
</keep-alive>
<!--正常-->
<keep-alive>
  <component :is="`theme${curTab}`"/>
</keep-alive>
```

3. 你给我的，却不是我想要的。

这里就涉及到 keep-alive 的俩 prop（`include` 和 `exclude`）。简单介绍下这俩 prop 的类型：

>-   `include` - `string | RegExp | Array`。只有名称匹配的组件会被缓存。
>-   `exclude` - `string | RegExp | Array`。任何名称匹配的组件都不会被缓存。


『你给我的，却不是我想要的』大概有这三种情形：

- `include`（`exclude`）设置正确，使用了动态组件但并未在页面中引入相关组件；
- 找错对象了，易发在 `keep-alive` 结合 `router-view` 使用的场景，使用了 `vue-router`的 name 属性来设置`include`（`exclude`）；
- `include`（`exclude`）给的值与组件自身的 `name` 或是局部注册名称不匹配，定义时是小驼峰写法，使用时是大驼峰写法；

这三点中最容易犯错的就是第二点，所以重要的事情要强调下。

> vue-router 的 name 属性是用于路由跳转!!!

4. 用逗号分隔字符串来表示 `include` 和 `exclude`时，因多余空格导致失效。

为防止这种错误发生，推荐使用另外两种方式表示。正则表达式或一个数组。

```html

<!--逗号分隔字符串 失效-->
<keep-alive include="Home, About">
  <component :is="view"></component>
</keep-alive>

<!--逗号分隔字符串 正常-->
<keep-alive include="Home,About">
  <component :is="view"></component>
</keep-alive>

<!-- 使用正则表达式 -->
<keep-alive :include="/Home|About/">
 <component :is="view"></component>
</keep-alive>

<!-- 使用数组 -->
<keep-alive :include="['Home', 'About']">
 <component :is="view"></component>
</keep-alive>

```


5. 在 `Vue3` 中局部注册匿名组件后，缓存失效。

> `include` 和 `exclude` 匹配首先检查组件自身的 name 选项，如果 name 选项不可用，则匹配它的局部注册名称 (父组件 components 选项的键值)。

按照官方文档的意思，只有组件自身 name 不可用（我的理解是就未指定 name），才匹配它的局部注册名称。这里我在Vue2 和 Vue3都做了试验，发现两者表现不同。

Vue2写法：
```html
<template>
  <div id="app">
    <button @click="switchTheme">切换主题</button>
    <h4>直属的子组件切换，子组件被缓存</h4>
    <keep-alive :include="['a1', 'a2']">
      <component :is="`a${curTab}`"/>
    </keep-alive>
  </div>
</template>
<script>
const Theme1 = {
  // name: 'Theme1',
  template: `<div>主题1【{{ date }}】</div>`,
  data() {
    return {
      date: new Date()
    }
  }
}
const Theme2 = {
  // name: 'Theme2',
  template: `<div>主题2【{{ date }}】</div>`,
  data() {
    return {
      date: new Date()
    }
  }
}
export default {
  components: { 'a1': Theme1, 'a2': Theme2 },
  data() {
    return {
      curTab: 2,
    };
  },
  methods: {
    switchTheme: function() {
      this.curTab += 1
      if (this.curTab > 2) {
        this.curTab = 1
      }
    }
  }
};
</script>

```

![Video_2022-05-13_150716.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/901a1218ecb048939212f5a939a3ec16~tplv-k3u1fbpfcp-watermark.image?)

[点击查看效果](https://codepen.io/mileofsunshine/pen/jOZMQBr)



Vue3写法，相同部分的代码已省略
```html
<template>
<!--同上-->
</template>
<script>
import { ref } from 'vue'

const Theme1 = {} // 同上
const Theme2 = {} // 同上

export default {
  components: { 'a1': Theme1, 'a2': Theme2 },
  setup() {
    const curTab = ref(2)
    const switchTheme = () => {
      curTab.value += 1
      if (curTab.value > 2) {
        curTab.value = 1
      }
    }
    return {
      curTab,
      switchTheme
    }
  }
};
</script>
```

![2.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/01308958f9bf4fb79664636ff9f34a77~tplv-k3u1fbpfcp-watermark.image?)

[点击查看效果](https://codesandbox.io/s/vue3xia-de-keep-alivezhi-includeshi-li-iwocbh?file=/src/App.vue)


通过对比我们可以发现除了因 Vue 版本造成的写法不同外，二者并无差异。但运行后表现出在 Vue2 中`keep-alive` 功能正常，在 Vue3 中却失效。要是你的项目是用 Vue3 开发的，你就老老实实的给组件指定 name 吧。

6. 匿名组件造成 `keep-alive` 失效。

匿名组件就是未指定 name 的组件，但你要是通过局部（全局）组件注册后，它就是具名组件了。项目中容易出现匿名组件的就是Vue页面，`keep-alive` 结合 `router-view` 使用就容易让你困在vue-router 的 name 属性中，切记这个 name 属性只用于路由跳转。

vue2写法：
```html
<keep-alive>
  <router-view />
</keep-alive>
```

vue3写法：
```html
<router-view v-slot="{ Component }">
  <transition>
    <keep-alive>
      <component :is="Component" />
    </keep-alive>
  </transition>
</router-view>
```
7. 多级路由导致缓存失效

多级路由一般出现在功能繁杂的后台管理系统中，为提高系统性能和改善用户体验，产品可能会要求对页面进行缓存以达到快速地在页面间来回切换。keep-alive 进而在管理系统中崭露头角，但其表现极其不稳定，着实让人摸不着头脑。

Layout.vue
```html
<template>
  <router-view v-slot="{ Component }">
    <transition mode="out-in" name="fade-transform">
      <keep-alive :include="['theme1', 'theme2']">
        <component :is="Component"/>
      </keep-alive>
    </transition>
  </router-view>
</template>
```
NestRouterView.vue
```html
<template>
  <router-view />
</template>
```
router.js
```js
const routes = [
  {
    path: '/',
    component: () => import('@/views/Layout.vue'),
    children: [
      {
        name: 'theme',
        path: '',
        component: () => import('@/views/NestRouterView.vue'),
        children: [
          {
            name: 'theme1',
            path: 'theme/1',
            component: () => import('@/views/Theme1.vue'),
            meta: {
              title: '主题1',
              keepAlive: true,
            },
          },
        ]
      }
    ]
  }
]
```

这里多级路由缓存失效的原因是借助了『空组件』（NestRouterView.vue）。关于缓存的设置也仅限生效于二级路由中，三级路由中并未涉及到 keep-alive，也就无从谈及失效一说。如何解决多级路由缓存失效问题，请移步小编的另一篇文章[vue多级路由缓存(keep-alive)失效的解决方案](https://mileofsunshine.github.io/2022/05/19/2022-05-19-keep-alive/)。


8. `<keep-alive>` 不会在函数式组件中正常工作，因为它们没有缓存实例。

函数式组件只是函数，无状态 (没有[响应式数据](https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E6%95%B0%E6%8D%AE))，也无实例 (在vue2中，没有 `this` 上下文)。

以上是目前小编所知的8种失效场景。只有规避掉失效点，才能真正用好 `keep-alive`。


## 小结

- `include` 和 `exclude` 推荐使用正则表达式或一个数组来表示；
- 需要缓存的组件都写 `name` 属性，杜绝匿名组件；
- 认清 `keep-alive` 和 `vue-router`的关系，杜绝乱用 `name`；
- 面对多层 `router-view` 嵌套，找准 `keep-alive` 位置；