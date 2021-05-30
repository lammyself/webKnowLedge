# vue原理简介

## 变化侦测

应用在运行过程中, 内部状态是在不断的改变的.所以需要侦测到状态的变化, 并及时根据状态的修改及时更新;
通常变化侦测有两种方案:

+ 推: 状态发生改变时, 会主动通知依赖他的方法.`推`的方案会告诉依赖更多的信息, 所以可以进行更细粒度的更新;
  但是,细粒度的监听会占用大量内存;
+ 拉: 状态发生改变时依赖它的方法只能获取到有状态发生改变, 但是并不知道具体是哪一个状态发生了改变;
  监听到状态改变后再进行对比, 找到两次状态间的差异并进行更新;

`vue`中采用的是`推`的方法, 因为细粒度的监听会占用内存.所以`vue`的状态不再绑定`DOM`节点, 而是组件;

`vue`实例监听到状态改变后, 会通知到组件.组件内部再进行一次比对, 这样可以降低侦测的力度从而节省大量内存;

## vue侦测变化的方法

通过`Object.defineProperty`侦测到对象的变化,然后在`setter`中更新依赖;
每个状态都可能会有很多地方去依赖, 所以需要在`getter`中收集依赖,然后在`setter`中去更新;
`vue`中的依赖就是-监听器`watcher`, 依赖更新时会去通知`watcher`, 然后它再去通知其他地方;
`vue`中定义了一个`Observe`类,用来将一个对象转为可以被侦测的`object`;
`vue`会调用`Observe`转换`data`中所有的属性;

因为变化侦测是通过`Object.defineProperty`来实现的, 所以当对象的属性值发生改变的时候`vue`是侦测不到这种变化的.
可通过显式的使用`vm.$set()`来通知vue实例, 或者通过`vm.$forceUpdate()`强制`vue实例`重新渲染;

### 侦听数组的变化

因为无法侦测到数组内部的增加或删除, 所以`vue`使用一个拦截器来覆盖`Array.prototype`.每当使用`Array`原型上的方法修改数组时, 执行的都是拦截器中的方法;
拦截器中的方法执行时会通知这个数组的依赖, 而`vue`中数组的依赖收集同样是在`getter`中收集;
所有方法中数组的变化都是通过拦截器来进行监听的;

### 变化侦测相关的方法实现

+ vm.$watch: 是一种对监听器`watcher`的封装;
+ vm.$set: `Observe`类抛出的set方法;

## 虚拟DOM(Virtual DOM)

因为频繁操作`DOM`修改的代价很大, 所以采用`虚拟DOM`来比对更新前后的差异, 从而减少`DOM`的操作;
`Angular`和`React`都采用了全部比对的方式, 而`vue`在状态更新时会检测到组件级的变化, 所以`vue`只需要比对单个组件的`虚拟DOM`;
`虚拟DOM`只是一种用来描述`DOM`的`JavaScript`对象, 操作对象会比操作`DOM`更节省资源;

### patching算法

`vue`在更新`DOM`的时候并不会直接覆盖原有`DOM`节点, `patching算法`会通过`虚拟DOM`来比对新旧节点的差异从而找出需要更新的节点;
对比出新旧节点之间的差异后, 除删除、新增之外需要更新的节点不会被删除, 而是根据比对出的差异来修改原节点.

## 模板编译原理

`vue`提供了模板语法, 可以声明式的描述`DOM`与状态之间的关系.
但是模板最终也要生成`虚拟DOM`, 并渲染为真实`DOM`呈现给用户;

`vue`会通过模板生成渲染函数, 再将渲染函数生成为`VNode`(虚拟节点),通过`VNode`组合成`虚拟DOM`, 最终渲染到页面上;

![img](./img/CompilationProcess.png)

+ 模板: 用来描述了`DOM`与状态之间关系;
+ 渲染函数: 用来在状态更新后生成`VNode`;

模板编译过程为渲染函数分为两步:

+ 将模板编译为`AST`;
+ 使用`AST`生成渲染函数;

因为有些节点没有与状态绑定, 不需要重复渲染.所以在生成渲染函数时会先遍历一次`AST`, 将那些节点标记为静态节点;

## 自定义事件原理

`vm.$on`会将事件存储到`vm._events`中, 存储时用`事件名`做`key`,`事件回调`会被`push`到一个数组中作为属性值`value`;
`vm.$emit`调用时会根据对应的`事件名`执行`vm._events`中的方法;

`vm.$off`可以根据传参的不同移除不同的事件监听或者清空所有事件;

## use

`Vue.use`用于注册安装vue插件, 如果插件是一个对象, 那么它必须提供`install`方法.如果插件是一个函数,那么它自己会被视为`install`方法.
调用`install`方法时,会将`Vue`作为参数传入.
多次调用`install`, 插件也只会安装一次.

## mixin

`Vue.mixin`方法注册后会影响创建的每个`vue`实例, 因为这个方法会更改`vue.options`.
会将用户传入的对象和`vue.options`混合到一起, 然后用混合后的对象代替`vue.options`.

## 生命周期函数

生命周期钩子函数是在`vue`运行过程中不同阶段被`callHook`函数调用的`vue`实例上的方法.

`vue.mixin`和`new Vue()`都可以设置生命周期钩子, 所以同名的生命周期钩子函数会被放入一个数组中;
`callHook`函数接受两个参数`vue`组件实例和钩子函数名, 然后`callHook`内部会遍历调用生命周期.

## vuex

> Vuex实现了一个单向数据流，在全局拥有一个State存放数据，所有修改State的操作必须通过Mutation进行，Mutation的同时提供了订阅者模式供外部插件调用获取State数据的更新。所有异步接口需要走Action，常见于调用后端接口异步获取更新数据，而Action也是无法直接修改State的，还是需要通过Mutation来修改State的数据。最后，根据State的变化，渲染到视图上。Vuex运行依赖Vue内部数据双向绑定机制，需要new一个Vue对象来实现“响应式化”，所以Vuex是一个专门为Vue.js设计的状态管理库。

`vuex`被注册时`install`方法会通过`vue.mixin`将`vuexInit`混入进`beforeCreate`钩子函数中执行;

`vuexInit`会尝试从`option`中获取`vuex`实例`store`.
如果当前组件是根节点,则`options`中会存在`store`,可以直接获取赋值给`$store`.
如果当前组件不是根组件, 会通过`options`中的`parent`获取父组件`$store`的引用.
这样所有组件都会获取到一份相同的`store`的引用;
`vuex`数据更新借助了`vue`的响应式`data`来实现的, 设计思路与`event bus`类似.

## Vue router

+ install:
`VueRouter`注册时会通过`install`方法会通过`vue.mixin`在Vue实例的`beforeCreate`中初始化.

```javaScript
export function install (Vue) {
 // ...
  // 混入 beforeCreate 钩子
  Vue.mixin({
    beforeCreate () {
      // 在option上面存在router则代表是根组件 
      if (isDef(this.$options.router)) {
        this._routerRoot = this
        this._router = this.$options.router
        // 执行_router实例的 init 方法
        this._router.init(this)
        // 为 vue 实例定义数据劫持
        Vue.util.defineReactive(this, '_route', this._router.history.current)
      } else {
        // 非根组件则直接从父组件中获取
        this._routerRoot = (this.$parent && this.$parent._routerRoot) || this
      }
      registerInstance(this, this)
    },
    destroyed () {
      registerInstance(this)
    }
  })
 
  // 设置代理，当访问 this.$router 的时候，代理到 this._routerRoot._router
  Object.defineProperty(Vue.prototype, '$router', {
    get () { return this._routerRoot._router }
  })
  // 设置代理，当访问 this.$route 的时候，代理到 this._routerRoot._route
  Object.defineProperty(Vue.prototype, '$route', {
    get () { return this._routerRoot._route }
  })
 
  // 注册 router-view 和 router-link 组件
  Vue.component('RouterView', View)
  Vue.component('RouterLink', Link)

  // Vue钩子合并策略
  const strats = Vue.config.optionMergeStrategies
  // use the same hook merging strategy for route hooks
  strats.beforeRouteEnter = strats.beforeRouteLeave = strats.beforeRouteUpdate = strats.created
  // ...
}
```

+ 路由实现:
  主要是通过事件监听来获取路由的变化.
  + hash: 通过监听`hashchange`事件获取路由变化.
  + history: 通过监听`popstate`事件获取路由变化.
  其中`history`模式的路由更改是通过`history.replaceState()`和`history.pushState`来实现的,而`hash`模式则是通过`hash`值变化不触发浏览器更新来实现的.
  