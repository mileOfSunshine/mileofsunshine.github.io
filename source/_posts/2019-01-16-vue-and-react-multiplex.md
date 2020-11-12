---
layout: post
title: Vue 和 React 的状态逻辑复用方案
date: 2019-01-16
categories: 前端
tag: vue
---


# Vue 五种逻辑复用方案
目前`Vue`实现代码逻辑复用主要5个途径： `Vuex`, `Mixins`(混入,  `Vue`推荐), `Hooks`, `HOC`(高阶组件, `React`推荐), 函数式组件

## Vuex
它采用集中式存储管理应用的所有组件的状态。对大型项目来说是个很好的状态管理模式，但存在对于引入太多抽象, 导致开发时总在不同的文件之间切换, 造成组件重用更困难等问题。

## Mixins
混合对象可以包含任意组件选项。以组件使用混合对象时，所有混合对象的选项将被混入该组件本身的选项。

- 同名钩子函数将混合为一个数组，都将被调用，混合对象的钩子将在组件自身钩子之前调用
- 当混合值为对象的选项，例如 methods 和 components 等将被混合为同一个对象。两个对象键名冲突时取组件对象的键值对。
- 请慎重使用全局注册混合对象，一旦使用，将会影响所有之后创建的Vue实例。

> 小剧场：起初`React`也是通过`mixins`实现代码复用，但后来React为了避免开发者在组件中总是要写一段相同的代码，一步步脱离`mixins`，他们认为`mixins`在`React`生态系统中不是一种好的模式。

## Hooks
Hooks遵循函数式编程的理念。是为了从组件中提取现有状态逻辑, 以便可以独立测试并重用。Hooks允许开发者在不更改组件层次结构的情况下重用有状态逻辑, 这样可以轻松地在许多组件之间或社区共享Hooks。

现状：[React-hooks](https://reactjs.org/docs/hooks-intro.html)比较成熟, vue也有一个[vue-hooks](https://github.com/yyx990803/vue-hooks), 但只是处于实验阶段。关于vue-hooks如何使用，尤大大在GitHub上也给出了相关示例，可以参考学习。

> 推荐文章
[在 React 和 Vue 中尝鲜 Hooks](https://juejin.im/entry/5bda51576fb9a022824c0d27)

## HOC
组件工厂，接受原始组件作为参数，添加完功能与业务后，返回新的组件。 React 在代码中实现复用的主要方式就是使用高阶函数。

**现状**：在React实现组件其实就是在写函数, 函数拥有的功能组件都有，故实现高阶组件也是比较简单。而vue更像是一个高度封装的函数, 缺少了一定灵活性, 写高阶组件也比较复杂，关于这块Vue官网也是简单的介绍，笔者也是搜罗阅读了一些文章，写了个简单的demo, 仅供参考。[Live Demo](https://codesandbox.io/s/5xp39rx4kn)

 ```js
// Wrapper.js
export default function Wrapper(WrappedComponent) {
  return {
    props: {
      list: {
        type: Array,
        default: () => []
      },
      title: String
    },
    render(h) {
      return h("div", [
        h("h3", {}, this.title),
        h(
          "div",
          {
            class: "flex-row main-align-space-around"
          },
          this.list.map(item => {
            return h(WrappedComponent, { props: item });
          })
        )
      ]);
    }
  };
}
```

```html
<!-- app.vue -->
<template>
  <div id="app">
    <anime-list title="今日漫画" :list="animeList"></anime-list>
    <book-list title="今日推荐书籍" :list="bookList"></book-list>
  </div>
</template>

<script>
import Item from "./components/Item";
import Wrapper from "./components/Wrapper";

const AnimeList = Wrapper(Item);
const BookList = Wrapper(Item);

export default {
  name: "App",
  components: {
    AnimeList,
    BookList
  },
  data() {
    return {
      animeList: [],
      bookList: []
    };
  }
};
</script>
```

![高阶组件效果图](https://user-images.githubusercontent.com/17926741/98777650-19815780-242c-11eb-8a35-ddb3399cb1c5.png)




> 推荐文章
> [探索Vue高阶组件](http://hcysun.me/2018/01/05/%E6%8E%A2%E7%B4%A2Vue%E9%AB%98%E9%98%B6%E7%BB%84%E4%BB%B6/)

## 函数式组件 (vue)
标记组件为functional，无状态(没有响应式数据)，无实例(没有this上下文)，**组件需要的一切都是通过上下文传递**，在一定程度上达到了复用。有两种方式可以定义函数式组件，一是通过`render function`形式，二是通过使用`单文件组件`。 目前对于单文件形式的函数式组件在模板编译、scoped css 和 热重载方面都得到了良好支持。 模板中的表达式会在函数式渲染上下文中求值，这就意味着在模板中，prop需要以`props.xx`的形式访问。

```js
// render function
export default {
  functional: true,
  props: {},
  render(h, { props, children, slots, data, parent, listeners, injections }) {}
}
```

```html
<!-- 单文件形式 -->
<template functional>
  <div v-bind="data.attrs" v-on="listener">{{ parent.$someProperty }}</div>
</template>

```

[Live Demo](https://codesandbox.io/s/yv4l2rnm4x)


> 推荐文章：
> - [Functional Components in Vue.js](https://alligator.io/vuejs/functional-components/)
> - [What’s the deal with functional components in Vue.js?](https://itnext.io/whats-the-deal-with-functional-components-in-vue-js-513a31eb72b0) 
>   - [demo](https://github.com/daprahamian/vue-render-functions-example)
>   - [文章快照](https://upload-images.jianshu.io/upload_images/3152495-58990312b685ea50.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




## 模板复用
Vuex, Mixins, hooks, HOC 都是在二级上进行高度封装，考验开发者的编程功底。目前还有一个解决复用的方法，例如阿里的[飞冰](https://alibaba.github.io/ice/iceworks), 通过穷举模板，进行复用。


# React 三种状态共享方案

1. [渲染属性（Render Props）](https://reactjs.org/docs/render-props.html): 使用一个值为函数的prop来传递需要动态渲染的nodes或组件

2. [高阶组件（Higher-Order Components）](https://reactjs.org/docs/higher-order-components.html)： 一个函数接受一个组件作为参数，经过一系列加工后，最后返回一个新的组件。

3. [React Hooks](https://reactjs.org/docs/hooks-intro.html#___gatsby) **状态逻辑复用**，只共享数据处理逻辑，不会共享数据本身。

    **a. useState**
它的作用就是用来声明状态变量。useState这个函数接收的参数是我们的状态初始值（initial state），它返回了一个数组，这个数组的第[0]项是当前当前的状态值，第[1]项是可以改变状态值的方法函数。

    **b. useEffect**
        
      - 解绑副作用：让我们传给useEffect的副作用函数返回一个新的函数即可。这个新的函数将会在组件下一次重新渲染之后执行。

      - 跳过不必要的副作用： 只需要给useEffect传第二个参数即可。用第二个参数来告诉react只有当这个参数的值发生改变时，才执行我们传的副作用函数（第一个参数）。当我们第二个参数传一个空数组[]时，其实就相当于只在首次渲染的时候执行。不过这种用法可能带来bug，少用。


![](https://user-images.githubusercontent.com/17926741/98777658-1b4b1b00-242c-11eb-9eaa-de0e0ea2ccf1.png)



> 推荐文章
[精读《怎么用 React Hooks 造轮子》](https://github.com/dt-fe/weekly/blob/master/80.%E7%B2%BE%E8%AF%BB%E3%80%8A%E6%80%8E%E4%B9%88%E7%94%A8%20React%20Hooks%20%E9%80%A0%E8%BD%AE%E5%AD%90%E3%80%8B.md)
[30分钟精通React Hooks](https://juejin.im/post/5be3ea136fb9a049f9121014)