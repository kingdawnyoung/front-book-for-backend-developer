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

```js
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

```js
//字符串就是零个或多个排在一起的字符，放在单引号或双引号之中。
'0'
"123"
//单引号内可以放双引号， 双引号内可以放单引号
"a'b'c"
'a"b"c'
//如果要在单引号字符串的内部，使用单引号（或者在双引号字符串的内部，使用双引号）,需要使用反斜杠(\)转义
"a\"b\"c"
'a\'b\'c'
//一般字符串写在多行会报错，如果长字符串必须分成多行，可以在每一行的尾部使用反斜杠
"a\
b\
c"
```

转义

```js
\0 null（\u0000）
\b 后退键（\u0008）
\f 换页符（\u000C）
\n 换行符（\u000A）
\r 回车键（\u000D）
\t 制表符（\u0009）
\v 垂直制表符（\u000B）
\' 单引号（\u0027）
\" 双引号（\u0022）
\ 反斜杠（\u005C）
```

Unicode标识法

```js
//\HHH 8进制
'\172' === 'z' // true
//\xHH 16进制，只能输出256种字符
'\x7A' === 'z' // true
//\uXXXX 16进制
'\u007A' === 'z' // true
'\u{7A}' === 'z' //true，使用大括号形式会缺省前面的00
```

.length返回字符串长度

```
'123'.length//3
```



