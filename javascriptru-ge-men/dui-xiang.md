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

**Object.is\(\)**， 它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。

```js
Object.is('123', '123') //true
Object.is({}, {}) //false
//与===差异点在于+0与-0之间 和NaN之间

+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

**Object.assign\(target, source1\[, source2...\]\)**，方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。

```js
const target = { a: 1 };

const source1 = { b: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

```js
//如如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。
const target = { a: 1, b: 2, c: 3 };

const source1 = { b: 4, c: 5 };
const source2 = { c: 6 };

Object.assign(target, source1, source2);
target // {a:1, b:4, c:6}
```

```js
//同样可以处理数组，这时将数组视为对象
Object.assign([1, 2, 3], [4, 5]) // [4, 5, 3]
```

```js
//只能进行值的复制，如果要复制的值是一个取值函数，那么将求值后再复制。
const source = {
  get foo() { return 1 }
};
const target = {};

Object.assign(target, source)
```

```
作用
1.扩展对象属性方法
2.浅克隆
3.合并对象
4.为对象指定默认值（作为组件的options体现特别明显）
```

**Object.keys\(\)**， 只返回可枚举的属性

Object.values\(\)，返回对象自身的（不含继承的）所有可遍历（enumerable）属性的键值。

Object.getOwnPropertyNames\(\)，返回还返回不可枚举的属性名

Object.entries，方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组。

```js
const obj = { foo: 'bar', baz: 42 };
Object.entries(obj) // [ ["foo", "bar"], ["baz", 42] ]
```

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



