数组

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

```
//前面的空位的例子
let arr = ['a', 'b', 'c']
arr[100]//这是有97个空位

arr = ['a', ,'c']//中间也产生了一个空位
arr[2]//空位返回undefined


arr = ['a', 'b', ]//这不会产生空位，数组末位的逗号会被默认移除
```

```
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



