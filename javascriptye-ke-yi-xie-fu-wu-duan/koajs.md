# Koa

为web应用和API提供更小，根据表现力和更强大的基础Web框架。

```bash
# 初始化package.json
npm init -y

# 安装koa2
npm install koa --save
```

Hello World

```js
//index.js
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Hello World'
})

app.listen(3000);
```

## 中间件

koa对网络请求采用了中间件的形式处理，中间件可以介入请求和响应的处理，是一个轻量级的模块。每个中间负责完成某个特定的功能。中间件的通过next函数联系，执行next()后会将控制权交给下一个中间件，如果没有有中间件没有执行next后将会沿路折返，将控制权交换给前一个中间件。

![](https://raw.github.com/fengmk2/koa-guide/master/onion.png)

一个中间件就是一个普通函数，包含两个参数，一个是`ctx`，一个是`next`。`ctx`即为上下文，包含两个基本属性`ctx.request`和`ctx.response`，分别表示'请求'和'响应'。`next`的作用是将处理的控制权交给下一个中间件,`next()`后面的代码会在后面中间件执行结束后继续执行。

```js
//用于验证权限
async function auth(ctx, next) {
  console.log('验证权限');
  await next();
  console.log('验证权限中间件继续执行');
}

//用于记录请求耗时的中间件
async function logger(ctx, next) {
  console.log('开始记录日志');
  const startDate = new Date();
  await next();
  console.log(`method: ${ctx.method} status: ${ctx.status} spend:${new Date() -startDate}ms`);
}
```

```js
//使用中间间
app.use(auth).use(logger);
```

```bash
验证权限
开始记录日志
method: GET status: 200 spend:2ms
验证权限中间件继续执行
```

## Context

Koa Context将Node的`request`和`response`对象包含进一个对象对象中，并且提供一些有用的方法来写web应用和API。

每一次请求都会创建一个`Context`，作为中间件的接收方用`ctx`表示。

为了方便起见，很多`Context`的属性和方法直接关联到`ctx.request`和`ctx.response`的相同的成员并且在其他方面也是相同的。

**ctx.req**

Node的`request`对象

**ctx.res**

Node的`response`对象

**ctx.request**

Koa `Request` 对象

**ctx.response**

koa `Response` 对象.

**ctx.app**

关联`Application`实例

**ctx.state**

推荐的命名空间用于在中间件之间传值或者将值传给前端视图

**ctx.cookies.get(name, [options])**

**ctx.cookies.set(name, value, [options])**

**ctx.throw([status], [msg], [properties])**

抛出一个默认转代码(`.status`)为500的异常，Koa做出响应响应。

**Request aliases**

```bash
ctx.header
ctx.headers
ctx.method
ctx.method=
ctx.url
ctx.url=
ctx.originalUrl
ctx.origin
ctx.href
ctx.path
ctx.path=
ctx.query
ctx.query=
ctx.querystring
ctx.querystring=
ctx.host
ctx.hostname
ctx.fresh
ctx.stale
ctx.socket
ctx.protocol
ctx.secure
ctx.ip
ctx.ips
ctx.subdomains
ctx.is()
ctx.accepts()
ctx.acceptsEncodings()
ctx.acceptsCharsets()
ctx.acceptsLanguages()
ctx.get()
```

**Response aliases**

```bash
ctx.body
ctx.body=
ctx.status
ctx.status=
ctx.message
ctx.message=
ctx.length=
ctx.length
ctx.type=
ctx.type
ctx.headerSent
ctx.redirect()
ctx.attachment()
ctx.set()
ctx.append()
ctx.remove()
ctx.lastModified=
ctx.etag=
```

## Request

Koa `Request` 对象是对node `request`的更高层次抽象并提供一些额外的功能用于每一个HTTP服务请求

**request.header**

**request.header=**

**request.headers**

`request.header`的别名

**request.headers=**

`request.header=`的别名

**request.method**

**request.method=**

**request.length**

请求的`Content-Length`

**request.url**

**request.url=**

设置请求URL，用于url重定向

**request.originalUrl**

**request.origin**

**request.href**

**request.path**

**request.path=**

**request.querystring**

获取原始的查询字符串不包括问号(?)

**request.querystring=**

**request.search**

获取原始的查询字符串包括问号(?)

**request.search=**

**request.host**

**request.hostname**

**request.URL**

获取转化后的URL对象

**request.type**

获取`Content-Type`不包括`charset`

**request.charset**

**request.query**

获取转化后的query-string，如果没有query-string返回一个空对象

**request.query=**

**request.fresh**

**request.stale**

**request.protocol**

**request.secure**

`ctx.protocol === 'https'`的简写

**request.ip**

**request.ips**

**request.subdomains**

**request.is(types...)**

**request.accepts(types)**

**request.acceptsEncodings(types)**

**request.acceptsCharsets(charsets)**

**request.acceptsLanguages(langs)**

**request.idempotent**

**request.socket**

**request.get(field)**

返回请求头

## Response

Koa `Response`对象时对node `response`对象的更高层次的抽象并提供一些额外的功能用于每一个HTTP服务响应。

**response.header**

**response.headers**

`response.header`的别名

**response.socket**

`request.socket`

**response.status**

**response.status=**

**response.message**

**response.message=**

**response.length=**

**response.length**

**response.body**

**response.body=**
设置响应主体

1. string written
2. Buffer written
3. Stream piped
4. Object || Array json-stringified
5. null no content response

**response.get(field)**

获取响应头，不区分大小写

**response.set(field, value)**

**response.append(field, value)**

在header追加额外信息

**response.set(fields)**

以对象的形式设置请求头信息

**response.remove(field)**

**response.type**

获取响应`Content-Type`不包括`chartset`

**response.type=**

**response.is(types...)**

**response.redirect(url, [alt])**

**response.attachment([filename])**

**response.headerSent**

检查响应头是否已发送

**response.lastModified**

Return the `Last-Modified` header as a Date, if it exists.

**response.lastModified=**

**response.etag=**

**response.vary(field)**

**response.flushHeaders()**

