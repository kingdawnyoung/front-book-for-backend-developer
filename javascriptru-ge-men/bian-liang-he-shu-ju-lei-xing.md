### 变量

变量是对“值”的引用，使用变量等同于引用一个值。每一个变量都有一个变量名。

js里三种定义变量方法， var, let，const

#### var

```js
var foo;
var bar = 1;
console.log(foo, bar);//undefined, 1
```

#### let

```js
let foo;
let bar = 1;
console.log(foo, bar);//undefined, 1
```

#### const

```js
const foo;//error，const必须要有初始值
const bar = 1;
console.log(foo, bar);//output, undefined, 1
bar = 2//error, constant不可以改变值
```

#### var VS. let & const

let和const是块级作用域

```js
//var example
{
    var a = 1;
}
console.log(a);//output, 1

{
    let b = 1;
    const c = 2;
}
console.log(b, c);//error, b 没有定义
```

let和const不可以重复定义

```js
var a = 1;
var a = 2;
console.log(a);//output, 2

let b = 1;
let b = 2;//error, 变量b已经存在

let a = 3;//error, 变量a已经存在
const a = 3;//error, 变量a已经存在

```



