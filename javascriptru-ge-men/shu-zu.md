### 数组

数组（array）是按次序排列的一组值。每个值的位置都有编号（从0开始），整个数组用方括号表示。

```js
let arr = ['a', 'b', 'c'];
arr[0] // 'a'

//任何类型的数据，都可以放入数组。
let arr2 = [{}, true, null, undefined, function(){}, arr ]
```

#### 数组也是对象

```js
//数组实际上也是对象
let arr = ['a', 'b', 'c'];
typeof arr // object
Object.keys(arr)//["1", "2", "3"]
```

#### **.length**

数组的`length`属性，返回数组的成员数量。

```js
let arr = ['a', 'b', 'c'];
arr.length // 3

//length属性的值总是比最大的那个整数键大1
arr[100] = 1
arr.length // 101
//length值也是可写的，数组的成员数量会自动减少或增加到到length设置的值。
arr.length = 2 // 2 items
arr.length = 100 // 100 items
//数组本质上是对象，所以我们也可以为数组添加其他属相，但这不会影响length值
arr['a'] = 1;
arr.length // 100
```

#### for...in

循环和数组的遍历

```js
let arr = ['a', 'b', 'c'];
for (var i in a) {
  console.log(a[i]);
}
//还会遍历非数字键
arr['m'] = 1
//所以推荐for, while, for...of循环
```

#### 数组的空位

```js
//前面的空位的例子
let arr = ['a', 'b', 'c']
arr[100]//这是有97个空位

arr = ['a', ,'c']//中间也产生了一个空位
arr[2]//空位返回undefined


arr = ['a', 'b', ]//这不会产生空位，数组末位的逗号会被默认移除
```

```js
//空位虽然返回undefined但与赋值undefined还是不一样的
let arr = ['a', ,'c']
arr.forEach(function (x, i) {
  console.log(i + '. ' + x);//输出忽略空位
})

for (var i in arr) {
  console.log(i);//输出忽略空位

}

Object.keys(arr)//[]， 输出忽略空位
```

#### 扩展运算符

扩展运算符（spread）是三个点（`...`），将一个数组转为用逗号分隔的参数序列。

```
let arr = ['a', 'b', 'c']
console.log(...arr) //output， a b c
```

```js
function fn(v, w, x, y, z) { }
const args = [0, 1];
fn(-1, ...args, 2, ...[3]);
```

用途

```js
//数组潜复制
const a1 = [1, 2];
// 写法一
const a2 = [...a1];
// 写法二
const [...a2] = a1;
```

```js
//数组合并
let arr1 = ['a', 'b'];
let arr2 = ['c'];
let arr3 = ['d', 'e'];
[...arr1, ...arr2, ...arr3] //[ 'a', 'b', 'c', 'd', 'e' ]
```

```js
//结合解构赋值
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]
```

```js
//将字符串转为数组
[...'hello'] // [ "h", "e", "l", "l", "o" ]
```

```js
//实现了 Iterator 接口的对象
let nodeList = document.querySelectorAll('div');
let array = [...nodeList];
```

```js
//扩展运算符内部调用的是数据结构的 Iterator 接口，因此只要具有 Iterator 接口的对象，都可以使用扩展运算符，比如 Map 结构。
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

let arr = [...map.keys()]; // [1, 2, 3]

const go = function*(){
  yield 1;
  yield 2;
  yield 3;
};

[...go()] // [1, 2, 3]
```

#### Array.from\(\)

方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map）

```js
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
```

```js
let nodeList = document.querySelectorAll('div');
Array.from(nodeList)
```

Array.from 还提供第二个参数，作用类似于数组的`map`方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

```js
Array.from([1, , 2, , 3], (n) => n || 0)
// [1, 0, 2, 0, 3]
```

只要有一个原始的数据结构，你就可以先对它的值进行处理，然后转成规范的数组结构，进而就可以使用数量众多的数组方法。

```js
Array.from({ length: 2 }, () => 'xx') // ["xx", "xx"]
```

#### Array.of

方法用于将一组值，转换为数组

```js
Array.of(1, false, undefined) //[1, false, undefined]
```

#### find, findIndex

数组实例的`find`方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为`true`的成员，然后返回该成员。如果没有符合条件的成员，则返回`undefined`。

```js
[1, , 2, , 3].find(function(item){return item === 2;})//2， 返回第一个值等于2的元素
```

数组实例的`findIndex`方法的用法与`find`方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回`-1`。

#### fill

填充一个数组

### 数组

数组（array）是按次序排列的一组值。每个值的位置都有编号（从0开始），整个数组用方括号表示。

```js
let arr = ['a', 'b', 'c'];
arr[0] // 'a'

//任何类型的数据，都可以放入数组。
let arr2 = [{}, true, null, undefined, function(){}, arr ]
```

#### 数组也是对象

```js
//数组实际上也是对象
let arr = ['a', 'b', 'c'];
typeof arr // object
Object.keys(arr)//["1", "2", "3"]
```

#### **.length**

数组的`length`属性，返回数组的成员数量。

```js
let arr = ['a', 'b', 'c'];
arr.length // 3

//length属性的值总是比最大的那个整数键大1
arr[100] = 1
arr.length // 101
//length值也是可写的，数组的成员数量会自动减少或增加到到length设置的值。
arr.length = 2 // 2 items
arr.length = 100 // 100 items
//数组本质上是对象，所以我们也可以为数组添加其他属相，但这不会影响length值
arr['a'] = 1;
arr.length // 100
```

#### for...in

循环和数组的遍历

```js
let arr = ['a', 'b', 'c'];
for (var i in a) {
  console.log(a[i]);
}
//还会遍历非数字键
arr['m'] = 1
//所以推荐for, while, for...of循环
```

#### 数组的空位

```js
//前面的空位的例子
let arr = ['a', 'b', 'c']
arr[100]//这是有97个空位

arr = ['a', ,'c']//中间也产生了一个空位
arr[2]//空位返回undefined


arr = ['a', 'b', ]//这不会产生空位，数组末位的逗号会被默认移除
```

```js
//空位虽然返回undefined但与赋值undefined还是不一样的
let arr = ['a', ,'c']
arr.forEach(function (x, i) {
  console.log(i + '. ' + x);//输出忽略空位
})

for (var i in arr) {
  console.log(i);//输出忽略空位

}

Object.keys(arr)//[]， 输出忽略空位
```

#### 扩展运算符

扩展运算符（spread）是三个点（`...`），将一个数组转为用逗号分隔的参数序列。

```
let arr = ['a', 'b', 'c']
console.log(...arr) //output， a b c
```

```js
function fn(v, w, x, y, z) { }
const args = [0, 1];
fn(-1, ...args, 2, ...[3]);
```

用途

```js
//数组潜复制
const a1 = [1, 2];
// 写法一
const a2 = [...a1];
// 写法二
const [...a2] = a1;
```

```js
//数组合并
let arr1 = ['a', 'b'];
let arr2 = ['c'];
let arr3 = ['d', 'e'];
[...arr1, ...arr2, ...arr3] //[ 'a', 'b', 'c', 'd', 'e' ]
```

```js
//结合解构赋值
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]
```

```js
//将字符串转为数组
[...'hello'] // [ "h", "e", "l", "l", "o" ]
```

```js
//实现了 Iterator 接口的对象
let nodeList = document.querySelectorAll('div');
let array = [...nodeList];
```

```js
//扩展运算符内部调用的是数据结构的 Iterator 接口，因此只要具有 Iterator 接口的对象，都可以使用扩展运算符，比如 Map 结构。
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

let arr = [...map.keys()]; // [1, 2, 3]

const go = function*(){
  yield 1;
  yield 2;
  yield 3;
};

[...go()] // [1, 2, 3]
```

#### Array.from\(\)

方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map）

```js
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
```

```js
let nodeList = document.querySelectorAll('div');
Array.from(nodeList)
```

Array.from 还提供第二个参数，作用类似于数组的`map`方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

```js
Array.from([1, , 2, , 3], (n) => n || 0)
// [1, 0, 2, 0, 3]
```

只要有一个原始的数据结构，你就可以先对它的值进行处理，然后转成规范的数组结构，进而就可以使用数量众多的数组方法。

```js
Array.from({ length: 2 }, () => 'xx') // ["xx", "xx"]
```

#### Array.of

方法用于将一组值，转换为数组

```js
Array.of(1, false, undefined) //[1, false, undefined]
```

#### find和findIndex

数组实例的`find`方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为`true`的成员，然后返回该成员。如果没有符合条件的成员，则返回`undefined`。

```js
[1, , 2, , 3].find(function(item){return item === 2;})//2， 返回第一个值等于2的元素
```

数组实例的`findIndex`方法的用法与`find`方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回`-1`。

#### fill(item, startIndex, endIndex)

填充一个数组

```js
['a', 'b', 'c'].fill(7, 1, 2) //['a', 7, 'c']
```

#### entries，keys

它们都返回一个遍历器对象, 可以用for...of循环进行遍历

```js
['a', 'b'].keys()
['a', 'b'].entries()
```

#### includes

`Array.prototype.includes`方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似

```js
['a', 'b'].includes('a') //true
```

#### push, pop, shift, unshift

```js
[1, 2].push(3, 4) //[1, 2, 3， 4]，在数组末位插入元素, 返回数组长度
[1, 2].pop() //[1], 删除数组的最后一个元素， 返回删除的元素
[1, 2].shift() //[2], 删除第一个元素， 返回删除的元素
[1, 2].unshift(-1, 0)//[-1, 0, 1, 2 ], 在数组首位插入元素，返回数组长度
```

#### join

`join`方法以参数作为分隔符，将所有数组成员组成一个字符串返回。如果不提供参数，默认用逗号分隔。

#### concat

方法用于多个数组的合并。它将新数组的成员，添加到原数组的尾部，然后返回一个新数组，原数组不变。

#### reverse()

方法用于颠倒数组中元素的顺序，返回改变后的数组。注意，该方法将改变原数组。

#### slice(startIndex, uptoIndex)

方法用于提取原数组的一部分，返回一个新数组，原数组不变

它的第一个参数为起始位置，第二个参数为终止位置（但该位置的元素本身不包括在内）。如果省略第二个参数，则一直返回到原数组的最后一个成员。

```js
var a = ['a', 'b', 'c'];
a.slice(1, 2) // ["b"]
```

```js
//方法的参数是负数，则表示倒数计算的位置。
var a = ['a', 'b', 'c'];
a.slice(-2, -1) // ["b"]
```
#### splice(index, countToRemove, addElement1, addElement2, ...)

方法用于删除原数组的一部分成员，并可以在被删除的位置添加入新的数组成员，返回值是被删除的元素。注意，该方法会改变原数组。第一个参数是删除的起始位置，第二个参数是被删除的元素个数。如果后面还有更多的参数，则表示这些就是要被插入数组的新元素。

#### sort

对数组成员进行排序。默认按照字典排序，也可以传入函数按照自定义方法排序。函数有两个参数，如果返回值大于0，表示第一个元素排在第二个元素后面；其他情况下，都是第一个元素排在第二个元素前面。

```js
[
  { name: "张三", age: 30 },
  { name: "李四", age: 24 },
  { name: "王五", age: 28  }
].sort(function (o1, o2) {
  return o1.age - o2.age;
})
// [
//   { name: "李四", age: 24 },
//   { name: "王五", age: 28  },
//   { name: "张三", age: 30 }
// ]
```

#### map

对数组的所有成员依次调用一个函数，根据函数结果返回一个新数组。

```js
var numbers = [1, 2, 3];

numbers.map(function (item, index, array) {
  return item + 1;
});
// [2, 3, 4]
```

#### forEach

也是遍历数组的所有成员，执行某种操作

```js
[2, 5, 9].forEach(function(item, index, array){
  console.log(item);
});
```

#### filter

`filter`参数是一个函数，所有数组成员依次执行该函数，返回结果为true的成员组成一个新数组返回。该方法不会改变原数组。

``` js
[1, 2, 3, 4, 5].filter(function (item) {
  return item > 3;
});
//[4, 5]
```

#### some，every

`some`方法是只要有一个数组成员的返回值是`true`就返回`true`

`every`方法则是所有数组成员的返回值都是`true`才返回`true`

#### reduce，reduceRight

方法依次处理数组的每个成员，最终累计为一个值。

```bash
1.累积变量，默认为数组的第一个成员
2.当前变量，默认为数组的第二个成员
3.当前位置（从0开始）
4.原数组
```

#### indexOf，lastIndexOf
方法返回给定元素在数组中第一次出现的位置，如果没有出现则返回-1; 第二个参数表示开始位置

```js
['a', 'b', 'c'].indexOf('a', 1) // -1
```
lastIndexOf方法返回给定元素在数组中最后一次出现的位置，如果没有出现则返回-1。