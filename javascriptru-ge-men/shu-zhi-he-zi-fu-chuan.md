### 数值\(number\)

#### 整数和浮点数

```js
1 === 1.0//true
35//整数，35
0xfff//16进制，255
0o377//8进制，255
0b11//二进制，3

//科学计数法
35e2//3500
35e-2//0.35
35E+2
35E-2

//最大值
Number.MAX_VALUE
//最小值
Number.MIN_VALUE
```

pasrInt、parseFloat或者Number.parseInt、Number.parseFloat

```js
parseInt('3.14')//3
parseFloat('3.14#')//3
Number.parseInt('0b11')
```

Number.isInterger是否为整数

```js
Number.isInteger(25) // true
Number.isInteger(25.0) // true, ps：js整数和浮点数是同样的储存方法
```

.toFixed\(n\)将数值转化为字符串并保留n位小数

```
3.1415926532653589932384626.toFixed(2)//'3.14'
(3).toFixed(2)//'3.00'
```

#### NaN

表示非数值

```js
5 - 'x'
0 / 0
Math.sqrt(-1)
//但NaN依然是数值类型
typeof NaN; // output, 'number'
```

运算规则

```js
NaN === NaN //false
[NaN].indexOf(NaN) //false, ps:数组indexOf判断逻辑使用的是===运算符
Boolean[NaN] //false
//NaN与任何数运算都得到NaN
NaN + 32//NaN
```

判断NaN的方法isNaN或者Number.isNaN

```js
isNaN(NaN)//true
isNaN(123)//false
Number.isNaN('123')//false
Number.isNaN(true)//false
```

#### **Infinity**

表示无穷大

```js
Infinity + 1//Infinity
```

判断Infinity的方法isFinity或者Number.isFinity

```js
Number.isFinite(Infinity - 1)//true
```

### 字符串



