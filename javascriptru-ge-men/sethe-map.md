### Set

它类似于数组，但是成员的值都是唯一的，没有重复的值。

```js
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

```js
//Set 函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。
// 例一
const set = new Set([1, 2, 3, 4, 4]);
[...set] // [1, 2, 3, 4]
set.size //4
```

#### 操作方法

* `add(value)`：添加某个值，返回Set结构本身。
* `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
* `has(value)`：返回一个布尔值，表示该值是否为Set的成员。
* `clear()`：清除所有成员，没有返回值。

```js
const s = new Set();

s.add(1).add(2).add(2);
// 注意2被加入了两次

s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2);//true
s.has(2) // false

s.clear()
```

#### 遍历方法

* `keys()`：返回键名的遍历器
* `values()`：返回键值的遍历器
* `entries()`：返回键值对的遍历器
* `forEach()`：使用回调函数遍历每个成员

```js
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]

//实际上Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的values方法。
for(let x of set){
    console.log(x);
}
//red
//green
//blue
```

```js
//forEach和数组一样，用于对每个成员执行某种操作
let set = new Set(['red', 'green', 'blue']);
set.forEach((val, key)=>console.log(key, val))
```

#### 遍历的应用

扩展运算符（...）内部使用for...of循环，所以也可以用于 Set 结构。

```js
let set = new Set(['red', 'green', 'blue']);
let arr = [...set];
Array.isArray(arr) //true
```

```js
//扩展运算符和 Set 结构相结合，就可以去除数组的重复成员。
let arr = [3, 5, 2, 2, 5, 5];
let unique = [...new Set(arr)];
```

```js
//整合数组的 filter很容易实现， 并集(union)， 交集(intersect)和差集(difference)
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```

### WeakSet

WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别。

1. WeakSet 的成员只能是对象，而不能是其他类型的值。
2. 弱引用可能随时被回收掉

WeakSet没有size属性， 不可遍历

### Map

它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

```js
const map = new Map();
const objKey = {p: 'Hello World'};

m.set(objKey, 'content')
m.get(objKey) // "content"

m.has(objKey) // true
m.delete(objKey) // true
m.has(objKey) // false
```

```js
//Map 也可以接受一个数组作为参数。该数组的成员是一个个表示键值对的数组。
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);
map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"
```

```js
//事实上，不仅仅是数组，任何具有 Iterator 接口、且每个成员都是一个双元素的数组的数据结构都可以当作Map构造函数的参数。这就是说，Set和Map都可以用来生成新的 Map。
const set = new Set([
  ['foo', 1],
  ['bar', 2]
]);
const m1 = new Map(set);
m1.get('foo') // 1

const m2 = new Map([['baz', 3]]);
const m3 = new Map(m2);
m3.get('baz') // 3

```

```js
//如果对同一个键多次赋值，后面的值将覆盖前面的值。
const map = new Map();

map
.set(1, 'aaa')
.set(1, 'bbb');

map.get(1) // "bbb"
```

#### 属性和操作方法

* `size`属性返回 Map 结构的成员总数。
* `set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。
* `get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。
* `has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中
* `delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。
* `clear`方法清除所有成员，没有返回值

#### 遍历方法

* `keys()`：返回键名的遍历器。
* `values()`：返回键值的遍历器。
* `entries()`：返回所有成员的遍历器。
* `forEach()`：遍历 Map 的所有成员。

```js
const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()，Map 结构的默认遍历器接口（Symbol.iterator属性），就是entries方法。
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"
```

```js
//Map 的forEach方法，与数组的forEach方法类似，也可以实现遍历。
map.forEach(function(value, key, map) {
  console.log("Key: %s, Value: %s", key, value);
});
```

### WeakMap

WeakMap结构与Map结构类似，也是用于生成键值对的集合。区别有两点。

1. `WeakMap`只接受对象作为键名（`null`除外），不接受其他类型的值作为键名。
2. 其次，`WeakMap`的键名所指向的对象，不计入垃圾回收机制。

WeakMap没有size属性， 不可遍历
