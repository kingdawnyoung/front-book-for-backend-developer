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

变量指向的那个内存地址不得改动

```js
const foo;//error，const必须要有初始值
const bar = 1;
console.log(foo, bar);//output, undefined, 1
bar = 2//error, constant不可以改变值
```

#### var VS. let & const

let和const是块级作用域， 称之为`暂时性死区`（temporal dead zone），简称TDZ

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

let和const不可以重复声明

```js
var a = 1;
var a = 2;
console.log(a);//output, 2

let b = 1;
let b = 2;//error, 变量b已经存在

let a = 3;//error, 变量a已经存在
const a = 3;//error, 变量a已经存在

//在TDZ可以声明重复变量
{
    let a = 3;
    console.log(a);
}
{
    const a = 3;
    console.log(a);
}
```

#### 声明变量的其他三种方式

`function`，`class`， `import`

### 数据类型

#### 数值\(number\)

整数和小数（比如1和3.14）

#### 字符串\(String\)

字符组成的文本（比如”Hello World”）

#### 布尔值\(Boolean\)

`true`（真）和`false`（假）两个特定值

#### undefined

标识未定义或不存在

#### null

表示无值

#### 对象\(Object\)

各种值的组成的集合

#### Symbol

表示独一无二的值，由Symbol函数生成

