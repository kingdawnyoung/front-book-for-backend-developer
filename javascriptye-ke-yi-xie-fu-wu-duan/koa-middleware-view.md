# Koa中间件之模版引擎

## koa-ejs

针对koa开发的ejs模版引擎中间间，具备[ejs](/template-engine/ejs.md)的所有功能

```js
const Koa = require('koa');
const render = require('koa-ejs');
const path = require('path');

const app = new Koa();
render(app, {
  root: path.join(__dirname, 'view'),
  layout: 'template',
  viewExt: 'html',
  cache: false,
  debug: true
});

app.use(async function (ctx) {
  await ctx.render('home');
});

app.listen(3000);
```

### 配置

* root: 视图的根目录
* layout: 全局布局文件，默认layout文件，设为`false`禁止使用布局
* viewExt: 视图文件扩展名，默认`html`
* cache: 是否缓存视图编译结果
* debug: 调试标识，默认`false`
* delimiter: 用于闭合尖括号的分界符，默认百分号(%)

### 布局

```html
<html>
<head>
  <title>Hello</title>
</head>

<body>
  <h3>Hello World</h3>
  <%- body %>
</body>
</html>
```

### include

支持ejs的includes

```html
<div>
  <% include user.html %>
</div>
```