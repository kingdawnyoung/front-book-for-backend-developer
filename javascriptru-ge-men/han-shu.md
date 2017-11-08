### 函数
函数就是一段可以反复调用的代码块。

#### 函数声明

```js
//声明式
function print(s) {
  console.log(s);
  return s;
}

//表达式
var print = function(s){
  console.log(s)
  return s;
}

//采用表达式声明的函数，如果加上函数名，该函数名只在函数体内部有效，在函数体外部无效。
var print = function x(){
  console.log(typeof x);
};
x//error, x is not defined
print()//function
```

##### 函数的重复声明

```js
//如果同一个函数被多次声明，后面的声明就会覆盖前面的声明。由于函数名的提升，前一次声明在任何时候都是无效的
function f() {
  console.log(1);
}
f() // 2

function f() {
  console.log(2);
}
f() // 2
```

#### 参数

函数运行的时候，有时需要提供外部数据，不同的外部数据会得到不同的结果，这种外部数据就叫参数。

```js
function twice(x){
    return x * 2;
}

twice(2) //4
twice(3) //6
```

##### 参数省略

函数参数不是必需的，Javascript允许省略参数。

```js
function fn(a, b){
    return a;
}

fn() // undefined
fn(1) // 1
fn(1, 2, 3) // 1
fn.length //2

//只能缺省靠后的参数
fn(, 1) // error
fn(undefined, 1) //undefined
```

##### 默认值

```js
function fn(a = 1, b = 2){
    console.log(a, b);
}
fn(4) // output，4 2
```

```js
//默认值可以传一个表达式，但这个值是惰性的
let x = 99;
function fn(p= x + 1){
  console.log(p)
}

foo() //100

x = 100;
foo() //101
```

##### 配合解构函数使用

```js
function fn({x, y = 5}) {
  console.log(x, y);
}
fn({}) // undefined 5
fn({x: 1}) //1 5
foo() //error， 无法兑取undefined的属性‘x’
//上面代码只使用了对象的解构赋值默认值，没有使用函数参数的默认值。只有当函数foo的参数是一个对象时，变量x和y才会通过解构赋值生成。
```

``` js
//如果函数foo调用时没提供参数，变量x和y就不会生成，从而报错。通过提供函数参数的默认值，就可以避免这种情况。
function fn({x, y = 5} = {}){
  console.log(x, y);
}

foo() // undefined 5
```

```js
//在看一个我们异步亲亲的例子
function fetch(url, { body = '', method = 'GET', headers = {} } = {}) {
  console.log(method);
}

fetch('http://example.com')
// "GET"
```

##### 参数默认位置

通常情况下，定义了默认值的参数，应该是函数的尾参数。

```js
function fn(x, y = 2, z) {
  return [x, y, z];
}
fn() // [undefined, 2, undefined]
fn(1) // [1, 2, undefined]
fn(1, ,3) // 报错
fn(1, undefined, 3) // [1, 2, 3]
```

##### rest参数
`rest` 参数（形式为`...变量名`），用于获取函数的多余参数。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

```js
function add(...values) {
  let sum = 0;

  for (var val of values) {
    sum += val;
  }

  return sum;
}

add(1, 2, 3) // 6
```

```js
//rest参数之后不能再有其他参数（即只能是最后一个参数）
// 报错
function f(a, ...b, c) {
  // ...
}
```

##### 同名参数

如果有同名的参数，则取最后出现的那个值。

```js
function f(a, a) {
  console.log(a);
}

f(1, 2) // 2
f(1) // undefined
```

#### 箭头函数

```js
var fn = arg => arg;

//等价于
var fn = function(arg) {
  return arg;
};

//如果箭头函数不需要参数或多个参数，就使用一个圆括号代表参数部分
var fn = () => arg;
var add = (num1, num2) => num1 + num2;

//如果代码块多余一条语句，就需要用大括号把他们括起来
var add = (num1, num2) => {return num1 + num2;}

//由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。
var getObj = id => ({ id: id, name: "Temp" });

//如果箭头函数只有一句话，且不需要返回值，可以采用下面的写法，就不用写大括号了。
var fn = () => void 1;

//箭头函数同样支持默认值， 结构赋值， rest
var fn = (arg = 1)=> arg;
var fn = ({arg = 1} = {}) => arg;
var fn = (...rest) => rest

//简化回调
[1, 2, 3].forEach(item => void console.log(item))
[1, 2, 3].map(item => ++item)

```

#### 箭头函数使用注意点
* 函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象
* 不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误
* 不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
* 不可以使用`yield`命令，因此箭头函数不能用作 `Generator` 函数