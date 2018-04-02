# JavaScript 原型(Prototype)
JavaScript是一种面向对象的语言，区别于Java、C#等面向对象语言，JavaScript没有没有`class`关键字实现（在 ES2015/ES6 中引入了class关键字，但只是接下我们要讲的原型的语法糖）

每一个JavaScript对象都有一个私有属性，称之为[[Prototype]]，它指向他的原型对象(**prototype**)。该prototype又居右一个自己的`prototype`，层层向上直到一个对象的原型为`null`。`null`没有原型，作为了**原型链**中的最后一个环节。

几乎所有的JavaScript中的对象都是位于原型链顶端`Object`的实例。

原型机型经常被视为JavaScript的一个弱点，但事实上，原型链模型比经典的继承模式更加强大。

## 基于原型链的继承

### 继承属性

JavaScript对象有一个指向一个原型对象的链。当视图访问一个对象的属性时，它不仅仅在该对象上搜寻，还会搜寻该对象的原型，以及该对象原型的原型，一次层层向上搜索，直到找到一个名字匹配的属性或到达原型链的末尾

> 遵循ECMAScript标准，someObject.[[Prototype]] 符号是用于指向 someObject的原型。从 ECMAScript 6 开始，[[Prototype]] 可以通过Object.getPrototypeOf()和Object.setPrototypeOf()访问器来访问。这个等同于 JavaScript 的非标准但许多浏览器实现的属性 __proto__。

> 区别与构造函数的`prototype`，被构造函数创建的实例对象的 [[prototype]] 指向 函数 的 `prototype` 属性。

下面演示一个尝试访问属性时的例子

```js
//有个对象叫obj
let obj = {
  propA: 'obj-propA',
  propB: 'obj-propB',
}

//有个对象叫proto1
let proto1 = {
  propB: 'proto1-propB',
  propC: 'proto1-propC',
}

//将obj的原型指向proto1
obj.__proto__ = proto1;

//由有个对象叫proto2
let proto2 = {
  propD: 'proto2-propD',
}

//将proto1的原型指向proto2
proto1.__proto__ = proto2;

// obj --> proto1 --> proto2 --> {} --> null

obj.propA;
// propA是对象自身属性吗？ 是

obj.propB;
// propB是对象自身属性吗？ 是
// 对象原型上也有个属性propB，但是它不会被访问到，这种情况称之为“属性遮蔽”

obj.propC;
// propC是对象自身属性吗？ 不是，那就看看原型上有没有
// propC是obj.[[Prototype]]的属性吗？

obj.propD;
// propD是对象自身属性吗？ 不是，那就看看原型上有没有
// propD是obj.[[Prototype]]的属性吗？不是，那就看看obj.[[Prototype]].[[Prototype]]有没有
// propD是obj.[[Prototype]].[[Prototype]]的属性吗？是

obj.propE;
// propE是对象自身属性吗？ 不是，那就看看原型上有没有
// propE是obj.[[Prototype]]的属性吗？不是，那就看看obj.[[Prototype]].[[Prototype]]有没有
// propE是obj.[[Prototype]].[[Prototype]]的属性吗？不是，那就看看obj.[[Prototype]].[[Prototype]].[Prototype]]
// 知道原型为null，停止搜索
// 没有propE，返回undefined

```

### 继承方法

在JavaScript里，任何函数都可以添加到对象上作为对象的属性，函数的继承与其他的属性继承没有差别，包括上面的“属性遮蔽”(相当于其他语言的重写)

当继承的函数被调用时，`this`指向的当前继承的对象，而不是继承的函数所在的原型对象。

```js
// 原型proto1添加一个methodA方法
proto1.methodA = function() {
  console.log(this.propA);
};

obj.methodA(); //输出 obj-propA
```

## 使用不同的方法来创建对象和生成原型链

### 基本语法结构创建的对象

```js
let obj = { a: 1 };
// obj --> Object.prototype --> null
// Object.prototype.hasOwnProperty('toString')
// obj.toString();

let arr = [1, 2, 3];
// arr --> Array.prototype --> Object.prototype ---> null
// Array.prototype.hasOwnProperty('slice')
// arr.slice()

function fn(){
  return 2;
}
// fn --> Function.prototype --> Object.prototype --> null
// Function.prototype.hasOwnProperty('bind')
// fn.bind()

```

### 构造器创建的对象

在 JavaScript 中，构造器其实就是一个普通的函数。当使用 `new 操作符` 来作用这个函数时，它就可以被称为构造方法（构造函数）。

```js
function Fn() {
  this.foo = 'foo';
  this.bar = 'bar';
}

Fn.prototype.methodA = function() {
  console.log('methodA');
}

let fn = new Fn();
fn.foo;
fn.bar;
fn.methodA();

// fn是生成的对象，它有自身属性"foo"和 "bar"。
// g.[[Prototype]]指向了Fn.prototype
```

### 通过Object.create创建的对象

ECMAScript 5 中引入了一个新方法：`Object.create()`。可以调用这个方法来创建一个新对象。新对象的原型就是调用 create 方法时传入的第一个参数。

```js
let foo = { a: 1};
// foo --> Object.prototype --> null

let bar = Object.create(a);
// bar --> foo --> Object.prototype --> null

let baz = Object.create(null);
//baz --> baz

```

### class关键字创建的对象

ECMAScript6 引入了一套新的关键字用来实现 `class`。使用基于类语言的开发人员会对这些结构感到熟悉，但它们是不同的。JavaScript 仍然基于原型。这些新的关键字包括 `class`, `constructor`，`static`，`extends` 和 `super`。

```js
class Parent {
  constructor() {
    console.log('parent');
  }
}

class Child extends Parent{
  constructor() {
    super()
  }

  get method() {
    console.log('child-method');
  }
}

let child = new Child();
```

### 性能

在原型链上查找属性是比较耗时的，另外视图访问不存在的属性时会遍历整个原型链。

遍历对象的属性时，原型链上的每个可枚举属性都会被枚举处理啊。要检查对线是否居右自己定义的属性，而不是其原型链上的某个属性，则必须使用所有对象从`Object.prototype`继承的`hasOwnProperty`方法。

```js
obj.hasOwnProperty('propA'); // true
obj.hasOwnProperty('propC'); // false

obj.__proto__.haOwnProperty('propC') // true
```

> `hasOwnProperty` 是 JavaScript 中唯一处理属性并且不会遍历原型链的方法。

## prototype和Object.getPrototypeOf

在前面的例子中我们定义的函数`function Fn`中有个特殊属性`prototype`，该属性可以与`new操作符`一起使用。对原型对象的引用被复制到了新实例的内部`[[Prototype]]`属性。当执行`let fn = new Fn()`时设置`fn.[[Prototype]] = Fn.prototype`。然后当访问实例的属性时，JavaScript首先检查他们是否直接存在于该对象上，如果不存在则会`[[Prototype]]`中查找。这意味着在`prototype`中定义的所有内容都可以与实例有效共享，我们甚至可以更改部分`prototype`，并在所有势力中显示更改。

如果我们执行`let fn1 = new Fn(); let fn2 = new Fn()`；那么`fn1.methodA`事实上会指向`Object.getPrototypeOf(fn1).methodA`，它就是我们在`Fn.prototype.methodA`中定义的内容。也就是说`Object.getPrototypeOf(fn1).methodA == Object.getPrototypeOf(fn2).doSomething == Fn.prototype.doSomething。`

```js
let fn = new Fn();

// 实际上
let fn = new Object();
fn.__proto__ = Fn.prototype;
Fn.call(fn);

// 然后执行methodA
fn.methodA();

```
![](/assets/JavaScript/prototype.png)

## 总结

在用原型继承编写复杂代码之前，了解原型继承模型非常重要。同时，要注意代码中的原型链的长度，并在必要时将其分解，以避免潜在的性能问题。