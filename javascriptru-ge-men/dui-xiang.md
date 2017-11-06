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



