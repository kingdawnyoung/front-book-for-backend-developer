### 解构赋值

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

```js
//可以从数组中提取值，按照对应位置，对变量赋值。
let [a, b, c] = [1, 2, 3];
```

```js
//本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。
let [foo, [[bar], baz]] = [1, [[2], 3]];

let [ , , third] = ["foo", "bar", "baz"];

let [head, ...tail] = [1, 2, 3, 4];
//head: 1，tail: [2, 3, 4]

let [x, y, ...z] = ['a'];
//结构不成功返回undefined
//x: 'a', y: undefined, z: []
```

```js
//另一种情况是不完全解构，即等号左边的模式，只匹配一部分的等号右边的数组。这种情况下，解构依然可以成功。
let [x, y] = [1, 2, 3];
//x: 1
//y: 2

let [a, [b], d] = [1, [2, 3], 4];
//a: 1
//b: 2
//c: 4
```

```js
//事实上，只要某种数据结构具有 Iterator 接口，都可以采用数组形式的解构赋值。
let [x, y, z] = new Set(['a', 'b', 'c']);
//x:'a'

function* fibs() {
  let a = 0;
  let b = 1;
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

let [a, b, c, d, e, f] = fibs();
//f: 5
```

#### 默认值

解构赋值允许指定默认值。

```js
let [foo = true] = [];
//foo: true

let [x, y = 'b'] = ['a']; // x: 'a', y: 'b'
let [x, y = 'b'] = ['a', undefined]; // x: 'a', y: 'b'

//es6 内部使用严格相等运算符（===），判断一个位置是否有值。所以，如果一个数组成员不严格等于undefined，默认值是不会生效的。
let [x = 1] = [null];//x: null

```

```js
//默认值也可以是一个函数，但则函数是惰性的， 只用用到时才会执行
function fn(){
    console.log('call');
    return 2;
}
let [x = f()] = [1];
```

#### 对象的解构赋值

```js
let { foo, bar } = { foo: "aaa", bar: "bbb" };
//foo: "aaa"，bar: "bbb"

//不必考虑顺序
let { bar, foo } = { foo: "aaa", bar: "bbb" };
//foo: "aaa, bar: "bbb"


//不存在的键值返回undefined
let { baz } = { foo: "aaa", bar: "bbb" };
//baz: undefined
```

```js
//如果变量名与属性名不一致，必须写成下面这样。
let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
//baz: "aaa", 此时foo被称之为模式， baz擦拭变量
```

```js
//解构也可以用于嵌套结构的对象。
let obj = {
  p: [
    'Hello',
    { y: 'World' }
  ]
};

let { p: [x, { y }] } = obj;
//x: "Hello", y:"World"
```

```js
//对象的解构也可以指定默认值。
var {x, y = 5} = {x: 1};
//x: 1
//y: 5

var {x: y = 3} = {};
//y: 3

var {x: y = 3} = {x: 5};
//y: 5
```

```js
//对象的属性值严格等于undefined
var {x = 3} = {x: undefined};
//x：3

var {x = 3} = {x: null};
//x null
```

```js
//如果解构模式是嵌套的对象，而且子对象所在的父属性不存在，那么将会报错
let {foo: {bar}} = {baz: 'baz'};//error
//同下
let _tmp = {baz: 'baz'};
_tmp.foo.bar // 报错
```

```js
//如果要将一个已经声明的变量用于解构赋值，不要大括号写在行首
let x;
({x} = {x: 1});
```

#### 字符串的解构赋值

```js
const [a, b, c, d, e] = 'hello';

let {length : len} = 'hello';
//len: 5
```

#### 数值和布尔值的解构赋值 

```js
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
```

#### 函数参数的解构赋值

```js
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
```

```js
//函数参数的解构也可以使用默认值。
function move({x = 0, y = 0} = {}) {
  return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]
```

#### 用途

* 交换变量的值
* 从函数返回多个值
* 函数参数的定义
* 提取JSON数据
* 函数参数的默认值
* 遍历Map结构
* 输入模块的指定方法
