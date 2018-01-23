# Koa中间件之路由

## koa-router

RESTFul 资源路由器

```js
var Koa = require('koa');
var Router = require('koa-router');

var app = new Koa();
var router = new Router();

router.get('/', (ctx, next) => {
  ctx.body = "Hello World";
});

app
  .use(router.routes())
  .use(router.allowedMethods());

app.listen(3000);
```

### router.get|put|post|patch|delete|del ⇒ `Router`

HTTP verbs

```js
router
  .get('/', (ctx, next) => {
    ctx.body = 'Hello World!';
  })
  .post('/user', (ctx, next) => {
    // ...
  })
  .put('/user/:id', (ctx, next) => {
    // ...
  })
  .del('/user/:id', (ctx, next) => {
    // ...
  })
  .all('/user/:id', (ctx, next) => {
    // ...
  });
```

### 命名路由

```js
router.get('user', '/users/:id', (ctx, next) => {
 // ...
});

router.url('user', 3);
```

### 多个中间件

```js
router.get(
  '/users/:id',
  (ctx, next) => {
    ctx.state.user = {
      name: 'KingZ',
      id: ctx.params.id
    };
    next();
    ctx.body = ctx.params.id;
  },
  ctx => {
    console.log(ctx.state.user);
  }
);
```

### 路由前缀

```js
var router = new Router({
  prefix: '/users'
});

router.get('/', ...); // responds to "/users"
```

### URL参数

`ctx.params`

### `router.use([path], middleware) ⇒ Router`

使用中间件

```js
router
  .use(session())
  .use(authorize());

router.use('/users', userAuth());
router.use(['/users', '/admin'], userAuth());
```

### `router.prefix(prefix) ⇒ Router`

设置路由前缀

### `router.allowedMethods([options]) ⇒ function`

### `router.redirect(source, destination, [code]) ⇒ Router`

重定向路由

### `router.route(name) ⇒ Layer | false`

### `router.url(name, params, [options]) ⇒ String | Error`

生成一个路由URL

### `router.param(param, middleware) ⇒ Router`

识别命名路由中的参数来运行中间件

### `Router.url(path, params) ⇒ String`

生成一个路由URL
