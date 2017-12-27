# vue-router

我们使用vue创建单页面应用时往往需要将不同组件映射到不同地址，这时就需要`vue-router`。

```html
<h1>Hello Vue!</h1>
<p>
  <!-- 使用 router-link 组件来导航. -->
  <!-- 通过传入 `to` 属性指定链接. -->
  <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
  <router-link to="/a">Go to pageA</router-link>
  <router-link to="/b">Go to PageB</router-link>
</p>
<!-- 路由出口 -->
<!-- 路由匹配到的组件将渲染在这里 -->
<router-view></router-view>
```

```js
// 0. 如果使用模块化机制编程，導入Vue和VueRouter，要调用 Vue.use(VueRouter)

// 1. 定义（路由）组件。
// 可以从其他文件 import 进来
const pageA = {
  template: '<div>I am pageA</div>'
}
const PageB = {
  template: '<div>I am pageB</div>'
}

// 2. 定义路由
// 每个路由应该映射一个组件。
const routes = [{
    path: '/a',
    component: pageA
  },
  {
    path: '/b',
    component: PageB
  }
]

// 3. 创建 router 实例，然后传 `routes` 配置
// 你还可以传别的配置参数, 不过先这么简单着吧。
const router = new VueRouter({
  routes
})

// 4. 创建和挂载根实例。
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
  router
}).$mount('#example')
// 现在，应用已经启动了！
```

## 路由匹配

动态路径参数

```js
{
  path: '/a/:pageId',
  component: pageA
},
```

## 嵌套路由

实际生活中的应用界面，通常由多层嵌套的组件组合而成。同样地，URL 中各段动态路径也按某种结构对应嵌套的各层组件。

```js
const pageA = {
  template: `<div>
    I am pageA
    <router-view></router-view>
  </div>`
};

const pageAChild = {
  template: `<div>
      I am pageA's child
    </div>
  `
};

const routes = [{
    path: '/a',
    component: pageA,
    children:[{
      path: '/child',
      component: pageAChild,
    }]
  }
]
```

上面例子中`pageA`嵌套`<router-view>`，在对应的路由映射里添加`children`，`children`中的相应路径的组件将渲染到组件`pageA`的`<router-view>`位置。
> 以 / 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径

## 导航

前面例子中我们使用`<router-link>`创建`a`标签来定义导航链接，我们还可以借助`router`的实例方法，通过编写代码实现。

**`router.push(location, onComplete?, onAbort?)`**

想要导航到不同的 URL，则使用 `router.push` 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

`<router-link :to="...">`等同于`router.push(...)`

`router.push`用法

```js
router.push("a")

router.push({
  path: "a"
})

router.push({
  name: 'pageA',
  params: {
    pageId: 123
  }
})

router.push({
  path: 'a',
  query: {
    plan: 'private'
  }
})
```
> 如果提供了`path`，`params`会被忽略

**`router.replace(location, onComplete?, onAbort?)`**

`router.replace`和`router.push`很像，唯一的不同就是，它不会向history添加新记录，而是替换当前history记录

`<router-link :to="..." replace>` 等同于 `router.replace(...)`

**`router.go(n)`**

在 history 记录中向前或者后退多少步

```js
router.go(1)
router.go(-1)
```

## 命名视图

有事想同事暂时多个视图，而不是嵌套，比如创建一个布局有侧边栏和主内容两个视图，这时命名视图就派上用场。[示例代码](https://gitee.com/kingdawnyoung/codes/901qj8g7zoxltf3kns45w15)

```html
<router-view class="default"></router-view>
<router-view class="main" name="main"></router-view>
<router-view class="footer" name="footer"></router-view>
```

```js
//对应的组件会添加到属性name相匹配的<router-view>位置
//...
routes: [{
  path:'/nameView',
  components:{
    default: defaultComp,
    main: mainComp,
    footer: footerComp
  }
}]
```

## 重定向

```js
//...
routes: [{
  path: '/a', redirect: '/b',

  path: '/a', redirect: { name: 'foo' },

  path:'/a', redirect : to => {
    // 方法接收 目标路由 作为参数
    // return 重定向的 字符串路径/路径对象
  }
}]
```

## 别名

假如`/a` 的别名是 `/b`，意味着，当用户访问 `/b` 时，URL 会保持为 `/b`，但是路由匹配则为 `/a`，就像用户访问 `/a` 一样。

## 路由组件传参

在组件中使用$route会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的url上使用，限制了其灵活性。使用props将组件和路由解耦：

```js
//...
mainComp = {
  props: ["id"],
  template: `<div>
    {{id}}
  </div>`
},

//routes ...

//如果props被设置为true，route.params将会被设置为组件属性。
{
  path: '/nameView/:id',
  components: mainComp,
  props: true
}
// 如果props是一个对象，组件将会按照对象设置组件属性
{
  path: '/nameView/:id',
  components: mainComp,
  props: {
    id: 123
  }
}

//也可通过函数返回一个props，组件将会函数返回的对象设置属性
{
  path: '/nameView/:id',
  components: mainComp,
  props: (route) => ({
    id: route.query.id
  })
}

// 对于包含命名视图的路由，你必须分别为每个命名视图添加props选项：
{
  path: '/nameView/:id',
  components: {
    default: defaultComp,
    main: mainComp,
    footer: footerComp
  },
  props: {
    main: true
  }
}
```

## routes 完整参数

```js
{
  path: string;
  component?: Component;
  name?: string; // 命名路由
  components?: { [name: string]: Component }; // 命名视图组件
  redirect?: string | Location | Function;
  props?: boolean | string | Function;
  alias?: string | Array<string>;
  children?: Array<RouteConfig>; // 嵌套路由
  beforeEnter?: (to: Route, from: Route, next: Function) => void;
  meta?: any;

  // 2.6.0+
  caseSensitive?: boolean; // 匹配规则是否大小写敏感？(默认值：false)
  pathToRegexpOptions?: Object; // 编译正则的选项
}
```

## Router 参数

```js
{
  routes,
  mode,//模式, hash, history, abstract
  base,
  linkActiveClass,
  linkExactActiveClass,
  scrollBehavior,//当切换到新路由时滚动位置
  parseQuery/stringifyQuery,
  fallback
}
```

## Router实例

1. router.app
2. router.mode
3. router.currentRoute
4. router.beforeEach(guard)
5. router.beforeResolve(guard)
6. router.afterEach(hook)
7. router.push(location, onComplete?, onAbort?)
8. router.replace(location, onComplete?, onAbort?)
9. router.go(n)
10. router.back()
11. router.forward()
12. router.getMatchedComponents(location?)
13. router.resolve(location, current?, append?)
14. router.addRoutes(routes) 动态添加路由
15. router.onReady(callback, [errorCallback])
16. router.onError(callback)


## 路由信息对象

一个路由信息对象表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息，还有 URL 匹配到的 route records（路由记录）。

1. $route.path 对应当前路由的路径，总是解析为绝对路径
2. $route.params 一个 key/value 对象，包含了 动态片段 和 全匹配片段
3. $route.query 一个 key/value 对象，表示 URL 查询参数
4. $route.hash 当前路由的 hash 值 (带 #) ，如果没有 hash 值，则为空字符串
5. $route.fullPath 完成解析后的 URL，包含查询参数和 hash 的完整路径
6. $route.name 当前路由的名称，如果有的话
7. $route.matched 一个数组，包含当前路由的所有嵌套路径片段的 路由记录