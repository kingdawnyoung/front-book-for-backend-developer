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
  getOptions(state){
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
  incrementAsync (context) {
    context.commit('increment')
  }
}
```

分发Action

```js
store.dispatch('incrementAsync');

// 以对象形式分发
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

## Module

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。为了解决以上问题，Vuex允许我们将store风格层**模块(module)**。每个模块都拥有自己的state、mutation、action、getter甚至是嵌套子模块。

```js
const moduleA = {
  state: {...},
  mutations: {...},
  actions: {...},
  getters: {...}
};

const moduleB = {
  state: {...},
  mutations: {...},
  actions: {...},
  getters: {...}
};

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
});
```

```js
store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

对于模块内部的 mutation 和 getter，接收的第一个参数是模块的局部状态对象。

```js
const moduleA = {
  state: {
    count: 0
  },
  //mutation
  mutations: {
    increment (state) {
      // 这里的 `state` 对象是模块的局部状态
      state.count++
    }
  },
  //getter
  getters: {
    doubleCount (state) {
      return state.count * 2
    }
  }
}
```

同样在模块内部的action，局部状态通过`context.state`暴露出来，根节点状态则为`context.rootState`

```js
const moduleA = {
  actions: {
    incrementAction({state, commit, rootState}){
      //...
    }
  }
};
```

对于模块内部的 getter，根节点状态会作为第三个参数暴露出来：

```js
const moduleA = {
  // ...
  getters: {
    sumWithRootCount (state, getters, rootState) {
      return state.count + rootState.count
    }
  }
}
```

默认情况下action、mutation 和 getter是注册在全局命名空间的，这样多个模块可以对同一mutation和action做出响应。如果希望模块具有更高的封装性和复用性，可以通过添加 `namespaced: true` 的方式使其成为命名空间模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名。[实例代码](https://gitee.com/kingdawnyoung/codes/o3wj62rd1ihyxt4ncpubk39)
<a class="jsbin-embed" href="http://jsbin.com/zucagoz/embed">例子</a>

如果你希望使用全局 state 和 getter，rootState 和 rootGetter 会作为第三和第四参数传入 getter，也会通过 context 对象的属性传入 action。

若需要在全局命名空间内分发 action 或提交 mutation，将 { root: true } 作为第三参数传给 dispatch 或 commit 即可。

```js
modules: {
  foo: {
    namespaced: true,

    getters: {
      // 在这个模块的 getter 中，`getters` 被局部化了
      // 你可以使用 getter 的第四个参数来调用 `rootGetters`
      someGetter (state, getters, rootState, rootGetters) {
        getters.someOtherGetter // -> 'foo/someOtherGetter'
        rootGetters.someOtherGetter // -> 'someOtherGetter'
      },
      someOtherGetter: state => { ... }
    },

    actions: {
      // 在这个模块中， dispatch 和 commit 也被局部化了
      // 他们可以接受 `root` 属性以访问根 dispatch 或 commit
      someAction ({ dispatch, commit, getters, rootGetters }) {
        getters.someGetter // -> 'foo/someGetter'
        rootGetters.someGetter // -> 'someGetter'

        dispatch('someOtherAction') // -> 'foo/someOtherAction'
        dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'

        commit('someMutation') // -> 'foo/someMutation'
        commit('someMutation', null, { root: true }) // -> 'someMutation'
      },
      someOtherAction (ctx, payload) { ... }
    }
  }
}
```

## 辅助函数

state的辅助函数`mapState`

```js
computed(){
  ...Vuex.mapState({
    optionsAlias: 'options',
    optionsFn: state => state.options,
    optionsComp(state) {
      return state.options.length + this.num;
    }
  })
}
```

当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 mapState 传一个字符串数组。

```js
computed: mapState([
  // 映射 this.count 为 store.state.count
  'options'
])
```

getter的辅助函数`mapGetters`

```js
computed: {
  ...mapGetters([
    'getOptions',
    // ...
  ])
}
```

getter 属性另取一个名字

```js
mapGetters({
  // 映射 `this.doneCount` 为 `store.getters.doneTodosCount`
  getCustomOptions: 'getOptions'
})
```

mutation的辅助函数`mapMutations`

```js
//...
methods:{
  ...mapMutations([
    'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`
  ]),
  ...mapMutations({
    add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
  })
}
```

action的辅助函数`mapActions`

```js
// ...
methods: {
  ...mapActions([
    'incrementAsync', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`
  ]),
  ...mapActions({
    addAsync: 'incrementAsync' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
  })
}
```

<script src="https://static.jsbin.com/js/embed.min.js?4.1.1"></script>