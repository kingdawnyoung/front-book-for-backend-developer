### 对象

JavaScript 原生提供`Object`对象（注意起首的`O`是大写），所有其他对象都继承自这个对象。`Object`本身也是一个构造函数，可以直接通过它来生成新对象。

```js
var obj = new Object();

var o1 = {a: 1};
var o2 = new Object(o1);
o1 === o2 // true

new Object(123) instanceof Number
// true
```

#### 属性简洁表示法

```js
//属性简写
const foo = 'bar';
const baz = {foo};
baz // {foo: "bar"}

// 等同于
const baz = {foo: foo};
```

```js
//方法的简写
const o = {
  method() {
    return "Hello!";
  }
};
//等同于
const o = {
  method: function() {
    return "Hello!";
  }
};
```

#### 属性名表达式

```js
//通常对象字面量的表达方式
var obj = {
  foo: true,
  bar: 123
};

//我们现在可以使用表达式作为对象的属性名
let propKey = 'foo';

let obj = {
  [propKey]: true,
  ['a' + 'bc']: 123
};
```

#### Object\(\)函数

Object本身也是也是一个函数， 可以将任何值转化为对象

```js
Object() // 返回一个空对象
Object() instanceof Object // true

Object(undefined) // 返回一个空对象
Object(undefined) instanceof Object // true

Object(null) // 返回一个空对象
Object(null) instanceof Object // true

Object(1) // 等同于 new Number(1)
Object(1) instanceof Object // true
Object(1) instanceof Number // true

Object('foo') // 等同于 new String('foo')
Object('foo') instanceof Object // true
Object('foo') instanceof String // true

Object(true) // 等同于 new Boolean(true)
Object(true) instanceof Object // true
Object(true) instanceof Boolean // true

//如果Object参数也是个对象， 他总是返回原来对象
var arr = [];
Object(arr) // 返回原数组
Object(arr) === arr // true

var obj = {};
Object(obj) // 返回原对象
Object(obj) === obj // true

var fn = function () {};
Object(fn) // 返回原函数
Object(fn) === fn // true
```

#### Object静态方法

Object.is\(\)， 它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。

```js
Object.is('123', '123') //true
Object.is({}, {}) //false
//与===差异点在于+0与-0之间 和NaN之间

+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

Object.keys\(\)， 只返回可枚举的属性

Object.getOwnPropertyNames\(\)，返回还返回不可枚举的属性名

**对象属性模型的相关方法**

Object.getOwnPropertyDescriptor\(\)

Object.defineProperty\(\)

Object.defineProperties\(\)

Object.getOwnPropertyNames\(\)

**控制对象状态的方法**

Object.preventExtensions\(\)

Object.isExtendible\(\)

Object.seal\(\)

Object.isSealed\(\)

Object.freeze\(\)

Object.isFrozen\(\)

**原型链相关方法**

Object.create\(\)

Object.getPrototypeOf\(\)

#### Object对象实例方法

valueOf\(\) 返回当前对象的值

toString\(\) 返回当前对象的字符串形式

toLocaleString 返回当前对象的本地字符串形式

hasOwnProperty 判断某个属性是否为当前对象自身的属性，还是继承自原型对象的属性。

isPrototypeOf 判断当前对象是否为另一个对象的原型。

propertyIsEnumerable 判断某个属性是否可枚举

### 数组



