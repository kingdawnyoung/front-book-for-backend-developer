# Vuex

Vuex是一个准尉Vue.js应用策划稿女婿开发的**状态管理模式**。它采用集中式存储管理应用的所有组件的转台，并以相应的规则保证状态以一种可预测的方式发生改变。

![](https://vuex.vuejs.org/zh-cn/images/flow.png)

* state，驱动应用的数据源；
* view，以声明方式将 state 映射到视图；
* actions，响应在 view 上的用户输入导致的状态变化。

当我们的应用遇到多个组件共享状态是，单向数据流的简洁性很容易破坏。

* 多个视图依赖同一个状态
* 来自不同的视图需要变更同一状态

所以为什么不把组件的共享状态抽离出来，以一个全局单例模式管理呢。在这种模式下，我们的组件树构成了一个巨大的“视图”，不管在树的哪个位置，任何组件都能获取状态或者触发行为！

另外，通过定义和隔离状态管理中的各种概念并强制遵守一定的规则，我们的代码将会变得更结构化且易维护。

![](https://vuex.vuejs.org/zh-cn/images/vuex.png)

每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的状态 (state)。

1. Vuex 的状态存储是响应式的。
2. 你不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation。

```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

```js
store.commit('increment')

console.log(store.state.count) // -> 1

```
## State

Vuex 使用单一状态树，用一个对象就包含了全部的应用层级状态。这也意味着，每个应用将仅仅包含一个 store 实例。

从 store 实例中读取状态最简单的方法就是在计算属性中返回某个状态

```js
//计算属性
computed: {
  count () {
    return store.state.count
  }
}
```

Vuex通过`store`选项，提供一种机制将状态从根组件注入到每一个子组件

```js
new Vue({
  el:"#example",
  store
})
```

## Getter

有时我们需要从store中的state中派生出一些状态。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

```js
//state
state: {
  options:[{
    label: "A",
    value: "A"
  },{
    label: "B",
    value: "B"
  }]
},
//getter
getters:{
  options(state){
    return [{
      label: "全部",
      value: "-1"
    }].concat(state.options)
  }
}
```

Getter 会暴露为 store.getters 对象：

```js
store.getters.options
```

getter亦可以返回一个函数，来实现给getter传参

```js
getter:{
  getOption(state){
    return (value)=>{
      return state.options.find((item)=>{
        return item.value === value
      })
    }
  }
}
```

```js
store.getters.getOption('B') // -> { label: "B", value: "B" }
```

## Mutation

更改Vuex的store中的状态唯一的方法是提交mutation。Vuex中的mutation非常类似与事件每个 mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler)。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

```js
//mutation
mutations: {
  increment (state) {
    state.count++
  }
}
```

要调用一个mutation handler时，需要使用调用`staore.commint`方法

```js
store.commit('increment');
```

你可以向 store.commit 传入额外的参数，即 mutation 的 载荷（payload）:

```js
//mutation
mutations: {
  increment (state, n) {
    state.count += n
  }
}
```

```js
store.commit('increment', 10)

//type 属性的对象
store.commit({
  type: 'increment',
  amount: 10
})
```

## Action

Action 类似于 mutation，不同在于：

* Action 提交的是 mutation，而不是直接变更状态。
* Action 可以包含任意异步操作。

```js
//actions
actions: {
  increment (context) {
    context.commit('increment')
  }
}
```

分发Action

```js
store.dispatch('increment');

// 以对象形式分发
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```