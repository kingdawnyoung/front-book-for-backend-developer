# Generator（生成器）

Generator是ES6提供的一种异步编程解决方案

```js
function* gen() {
  yield 'Hello';
  yield 'World';
  return '!';
}

let g = gen();
console.log(g[Symbol.iterator]);// [Function: [Symbol.iterator]]
console.log(g.next()); // { value: 'Hello', done: false }
console.log(g.next()); // { value: 'World', done: false }
console.log(g.next()); // { value: '!', done: true }
console.log(g.next()); // { value: undefined, done: true }
```

1. 从语法上看，Generator 函数是一个状态机，封装了多个内部状态
2. 执行Generator 函数返回一个遍历器对象，可以依次遍历Generator函数内部的每一个状态
3. 形式上 Generator 是一个普通函数，它在`function`关键字和函数名之间有一个星号，并且函数体内使用`yield`表达式，定义不同的状态

## yield 表达式

Gernerator 函数返回的事一个遍历器对象，只用调用`next`方法才会遍历下一个内部状态，所以实际上提供了一种可以暂缓执行的函数。`yield`表达式就是暂停标志。

遍历器对象的`next`方法运行逻辑如下：
1. 遇到`yield`表达式，就暂停执行后面操作，并将紧跟在`yield`后面的那个表达式的值，作为返回的对象的`value`属性值。
2. 下一次调用`next`方法时，再继续往下执行，直到遇到下一个`yield`表达式。
3. 如果没有再遇到`yield`表达式，就一直运行函数结束，知道`return`语句为止，并将`return`语句后面的表达式的值，作为返回的对象的`value`属性值。
4. 如果改函数没有`return`语句，则返回的对象的`value`属性值为`undefined`。

`yield`表达式后面的表达式，只有当调用`next`方法、指针指向改语句时才会执行，因此等于为JavaScript提供了“惰性求值”的语法功能。

```js
function* gen() {
  yield 123 + 456
}
```

Generaotor 函数可以不用`yield`表达式，这时就变成了一个单纯的暂缓执行函数。

```js
function* gen() {
  return 'Hello World';
}

let g = gen();

setTimeout(() => {
  console.log(g.next())
}, 2000);
```

注意，`yield`表达式只能用在Generator函数里面，用在其它地方都会报错。

另外，`yield`表达式如果用在另一个表达式之中，必须放在圆括号里面。如果`yield`表达式用作函数参数或者放在赋值表达式右边，可以不加括号。

### 与Iterator接口的关系

前面章节我们说过任何一个对象的`Symbol.iterator`方法，等于该对象的迭代器函数，调用该函数会返回该对象的一个迭代器对象。

由于Generator函数就是迭代器生成器，因此可以把Generator赋值给对象的`Symbol.iterator`，从而使得对象居右Iterator接口。

```js
let obj = {};

obj[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};

[...obj] //[1, 2, 3]
```

Generator 函数执行后，返回一个遍历器对象。该对象本身也具有`Symbol.iterator`属性，执行后返回自身。

```js
function* gen(){
  ...
}

var g = gen();

g[Symbol.iterator]() === g // true
```