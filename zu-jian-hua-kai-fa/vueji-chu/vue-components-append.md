# vue组件(补)

## 动态组件

通过保留的`<component>`元素，动态的绑定到它的`is`特性，我们让多个组件可以使用同一个挂载点，并动态切换。

```js
let compA = {
    template: "<span>compA</span>"
  },
  compB = {
    template: "<span>compB</span>"
  },
  compC = {
    template: "<span>compC-{{ Date.now() }}</span>"
  };
new Vue({
  el: "#example",
  data() {
    return {
      currentView: compB
    }
  },
  components: {
    compA,
    compB,
    compC
  }
})
```

```html
<button @click="currentView='compA'">compA</button>
<button @click="currentView='compB'">compB</button>
<button @click="currentView='compC'">compC</button>
<div>
  <component :is="currentView"></component>
</div>
```

使用`keep-alive`可以把切换出去的组件保存在内存中，保留它的状态或者避免重新渲染

```html
<!--挂载点-->
<div>
  <keep-alive>
    <component :is="currentView"></component>
  </keep-alive>
</div>
```

## 编写可复用组件

编写组件时，最好考虑号以后是否进行复用。如果仅仅是一次性的组件内部有紧密的耦合没有，但是可复用的组件应当定义一个清晰的公开接口，同时也不要对其使用的外层数据做任何假设。

Vue组件的API来自三个部分——prop、事件、插槽

* Prop 允许外部环境传递数据给组件；
* 事件允许从组件内触发外部环境的副作用；
* 插槽允许外部环境将额外的内容组合在组件中。

## 异步组件

在大型应用中，我们可能需要将应用拆分为多个小的模块，按需从服务器下载。为了进一步简化，Vue.js允许将组件定义为工厂函数，异步地解析组件的定义。Vue只在组件需要渲染的时候出发工厂函数，并把结果缓存起来，用于后面的再次渲染。

```js
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // 将组件定义传入 resolve 回调函数
    resolve({
      template: '<div>async component!</div>'
    })
  }, 1000)
})
```

工厂行数接受一个`resolve`的回调，在收到服务器下载的组件定义时调用。

使用webpack代码分割功能
```js
Vue.component('async-example', function (resolve, reject) {
  require(["./async-component"], resolve)
})
```

工厂函数也可以返回一个`Promise`

```js
Vue.component("async-component", () => 
  Promise.resolve({
    template: "<span>async-component</span>"
  })
);
```

当使用局部注册时，也可以直接提供一个`Promise`的函数

```js
components: {
  'async-component': () =>
    Promise.resolve({
      template: "<span>async-component</span>"
    })
}
```

## 组件命名约定

当注册组件 (或者 prop) 时，可以使用 kebab-case (短横线分隔命名，烤串命名)、camelCase (驼峰式命名) 或 PascalCase (单词首字母大写命名)。

```js
// 在组件定义中
components: {
  // 使用 kebab-case 注册
  'kebab-cased-component': { /* ... */ },
  // 使用 camelCase 注册
  'camelCasedComponent': { /* ... */ },
  // 使用 PascalCase 注册
  'PascalCasedComponent': { /* ... */ }
}
```

在 HTML 模板中，请使用 `kebab-case`：

```html
<kebab-cased-component></kebab-cased-component>
<camel-cased-component></camel-cased-component>
<pascal-cased-component></pascal-cased-component>
```

如果组件未经 slot 元素传入内容，你甚至可以在组件名后使用 / 使其自闭合：

```html
<kebab-cased-component />
<camel-cased-component />
<pascal-cased-component />
```

## 对低开销的静态组件使用 `v-once`

尽管在 Vue 中渲染 HTML 很快，不过当组件中包含大量静态内容时，可以考虑使用 v-once 将渲染结果缓存起来

```js
Vue.component('once-component', {
  template: `
    <div v-once>
      <h1>Terms of Service</h1>
      ...很多静态内容...
    </div>
  `
})
```

## 单文件组件

前面我们使用`Vue.component`和`new Vue()`的形式定义组件，这种方式在小规模的项目中运行很好，但遇到复杂的项目时缺点就会很明显：

* 全局定义的强制命名不能重复
* 字符串模版缺乏语法高亮，且不易维护
* 不支持CSS
* 没有构建步骤不能使用预处理，如ES2015的新增特性

为了解决此类问题Vue提供了`.vue`为扩展名的单文件组件，并且还亦可以使用webpack或者Browserify等构建工具

一个.vue的例子

```html
<template>
  <p>Hello, {{ name }}</p>
</template>

<script>
  export default = {
    data(){
      return {
        name: "KingZ"
      }
    }
  }
</script>

<style scoped>
P{
  font-size: 20px
}
</style>
```

以前开发页面注重`结构``样式``行为`分离，但其实在一个组件里，其模版、逻辑和样式是内部耦合的，并且把他们搭配在一起实际上使得组件更加内聚更可维护。

如果仍然不喜欢单文件组件，你仍然把JavaScipt和CSS分离，然后由构建工具热重载和预编译

```html
<template>
  <p>Hello, {{ name }}</p>
</template>
<script src="./component.js"></script>
<style src="./component.css"></style>
```