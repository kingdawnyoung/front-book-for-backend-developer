# 开始使用Vue

vue是一个**渐进式**JavaScript框架

## 最重要的事

vue可以简化数据绑定，允许你简单的复用组件。

你不需要创建特殊的模型或集合，并在其中注册事件对象，你也不需要遵循特定的语法，你更不需要安装无休止的依赖。

你的model就是就是普通的js对象，它们可以绑定到你任何想要绑定的地方。

你可以可以很简单的vue.js加入到你的项目中使用。或者，你也可以使用vue-cli构建你的开发。

你可以从你的视图层分离出样式和脚本或者把他们放在一个Vue文件里。

你可以预处理是所有你想要的，你也可以使用ES2016。你将它和你喜欢的框架一起使用，或者单独使用。你可以使用他的部分功能，或者使用它的整个生态系统构建复杂的项目。

## 安装

你可以直接通过`script`引入

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

或者通过npm整合webpack模块大宝漆使用

```bash
> npm install vue
```

官方也提供了命令行工具，用来快速构建大型单页面应用。该工具开箱即用，自带热重载、静态检查，还可以构建生产环境级别代码。所以我们更推荐使用命令行工具开发vue项目。

```bash
# 全局安装vue-cli
> npm install -g ve-cli
# 创建一个基于webpack模版的新项目
> vue init webpack project-name
# 安装依赖
>cd project-name
>npm install
>npm run dev
```
## Vue实例

```js
var vm = new Vue({
  // 选项
});
```

## 生命周期

![](https://cn.vuejs.org/images/lifecycle.png)

## 开发工具

为`VS Code`提供了[Vetur](https://vuejs.github.io/vetur/)

为浏览器提供了[Vue.js devtools](https://github.com/vuejs/vue-devtools)

## 部署

如果你是通过引入`script`方式开发代码，直接上传文件到服务器即可

如果你是使用了构建工具，在部署前你需要预编译处理好，然后把编译好的数据传到服务器或者CDN。预编译可以直接将代码编译成渲染函数而不是原始的模版字符串，这样我们保证在性能上就有了初步保证。

```bash
# 与编辑代码
>npm run build
# 加report可以看到每个文件的都有哪些某块组成, 以及体积等信息
>npm run build --report
```
