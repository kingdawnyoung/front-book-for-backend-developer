### Iterator（迭代器）

目前表示数据集合的的数据结构有数组、对象、Map和Set，再加上彼此可以组合，我们就需要一种统一的接口机制，来处理不同的数据结构。

迭代器（Iterator）就是这么一种机制，他是一种接口，为各种数据接口提供统一的访问机制。任何数据结构只要实现Iterator接口，就可以完成遍历操作。

它的作用有三个：一是为各种数据结构提供统一简便的访问方式；二是使数据结构的成员能够按照某个次序排列；三是创造了一种新的遍历命令`for...of`，Iterator主要共`for...of`消费

Iterator的遍历过程是这样

1. 创建一个指针对象，指向当前数据结构的起始位置
2. 第一次调用指正对象的`next`方法，指针指向数据结构的第一个成员
3. 第二次调用指针对象的`next`方法，指针指向数据结构的第二个成员
4. 不断调用指针对象的`next`方法，直到指向数据结构的结束位置

每一次调用`next`方法，都会返回数据结构的当前成员信息。具体来说，就是返回一个包含`value`和`done`两个属性的对象。其中，`value`属性是当前成员的值，`done`属性是一个布尔值，标识遍历是否结束。

用代码描述如下

```js
let it = makeIterator([1, 2]);
it.next(); // { value: 1, done: false }
it.next(); // { value: 2, done: false }
it.next(); // { value: undefined, done: true }

function makeIterator(array) {
    var nextIndex = 0;
    return {
        next: function() {
            return nextIndex < array.length ?
                {value: array[nextIndex++], done: false} :
                {value: undefined, done: true};
        }
    };
}
```

#### 默认 Iterator 接口

Iteraotr接口目的就是为所有的数据结构提供统一的访问机制，即`for...of`循环。当使用`fo...of`循环时，会自动去寻找Iterator接口。只要数据结构部署了Iterator结构，我们就称这种数据结构是“可遍历的”（iterable）。

默认的Iterator接口部署在数据结构的`Symbol.iterator`属性上，或者说，一个数据结构只要有`Sysbol.iterator`属性，就可以认为是“可遍历的”（iterable）.

```js
const obj = {
  [Symbol.iterator]: function(){
    return {
      next: function(){
        return {
          value: 1,
          done: true
        }
      }
  }}
}
```
原生具备 Iterator 接口的数据结构如下。

- Array
- Map
- Set
- String
- TypedArray
- 函数的 arguments 对象
- NodeList 对象

以数组举例

```js
let arr = ['a', 'b', 'c'];
let iter = arr[Symbol.iterator]();

iter.next() // { value: 'a', done: false }
```

结合数组的Iterator我们可以为类数组对象添加Iterator

```js
const obj = {
  0:1,
  1:2,
  2:3,
  length:3,
  [Symbol.iterator]: Array.prototype[Symbol.iterator]
}

for(let item of obj){
  console.log(item)
}
```

#### 调用 Iterator 接口的场合

* 结构赋值

```js
let set = new Set().add('a').add('b').add('c');

let [x,y] = set;
// x='a'; y='b'

let [first, ...rest] = set;
// first='a'; rest=['b','c'];
```

* 扩展运算符

```js
let arr = ['b', 'c'];
['a', ...arr, 'd']
// ['a', 'b', 'c', 'd']
```
实际上，这提供了一种简便机制，可以将任何部署了 Iterator 接口的数据结构，转为数组。也就是说，只要某个数据结构部署了 Iterator 接口，就可以对它使用扩展运算符，将其转为数组。

* yield*
* Iterator接口与Generator函数 


* 其他场合
1. for...of
2. Array.from()
3. Map(), Set(), WeakMap(), WeakSet()（比如new Map([['a',1],['b',2]])）
4. Promise.all()
5. Promise.race()

#### 字符串的 Iterator 接口
字符串是一个类似数组的对象，也原生具有 Iterator 接口。

```js
var someString = "hi";
typeof someString[Symbol.iterator]
// "function"

var iterator = someString[Symbol.iterator]();

iterator.next()  // { value: "h", done: false }
iterator.next()  // { value: "i", done: false }
iterator.next()  // { value: undefined, done: true }
```

#### for...of循环

数组原生具备`iterator`接口（即默认部署了`Symbol.iterator`属性），`for...of`循环本质上就是调用这个接口产生的遍历器，可以用下面的代码证明。

```js
const arr = ['red', 'green', 'blue'];

for(let v of arr) {
  console.log(v); // red green blue
}

const obj = {};
obj[Symbol.iterator] = arr[Symbol.iterator].bind(arr);

for(let v of obj) {
  console.log(v); // red green blue
}
```