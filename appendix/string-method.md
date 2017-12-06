# String的方法

## String.prototype.charAt()

返回特定位置的字符。

## String.prototype.charCodeAt()
返回表示给定索引的字符的Unicode的值

## String.prototype.codePointAt()
返回使用UTF-16编码的给定位置的值的非负整数。

## String.prototype.concat()
连接两个字符串文本，并返回一个新的字符串。

## String.prototype.includes()
判断一个字符串里是否包含其他字符串。

## String.prototype.endsWith()
判断一个字符串的结尾是否包含其他字符串中的字符。

## String.prototype.indexOf()
从字符串对象中返回首个被发现的给定值的索引值，如果没有找到则返回-1。

## String.prototype.lastIndexOf()
从字符串对象中返回最后一个被发现的给定值的索引值，如果没有找到则返回-1。

## String.prototype.localeCompare()
返回一个数字表示是否引用字符串在排序中位于比较字符串的前面，后面，或者二者相同。

## String.prototype.match()
使用正则表达式与字符串相比较。

## String.prototype.normalize()
返回调用字符串值的Unicode标准化形式。

## String.prototype.padEnd()
Pads the current string from the end with a given string to create a new string from a given length.

## String.prototype.padStart()
Pads the current string from the start with a given string to create a new string from a given length.

## String.prototype.repeat()
返回指定重复次数的由元素组成的字符串对象。

## String.prototype.replace()
被用来在正则表达式和字符串直接比较，然后用新的子串来替换被匹配的子串。

## String.prototype.search()
对正则表达式和指定字符串进行匹配搜索，返回第一个出现的匹配项的下标。

## String.prototype.slice()
摘取一个字符串区域，返回一个新的字符串。

## String.prototype.split()
通过分离字符串成字串，将字符串对象分割成字符串数组。

## String.prototype.startsWith()
判断字符串的起始位置是否匹配其他字符串中的字符。

## String.prototype.substr()
通过指定字符数返回在指定位置开始的字符串中的字符。

## String.prototype.substring()
返回在字符串中指定两个下标之间的字符。

## String.prototype.toLocaleLowerCase()
根据当前区域设置，将符串中的字符转换成小写。对于大多数语言来说，toLowerCase的返回值是一致的。

## String.prototype.toLocaleUpperCase()
根据当前区域设置，将字符串中的字符转换成大写，对于大多数语言来说，toUpperCase的返回值是一致的。

## String.prototype.toLowerCase()
将字符串转换成小写并返回。

## String.prototype.toString()
返回用字符串表示的特定对象。重写 Object.prototype.toString 方法。

## String.prototype.toUpperCase()
将字符串转换成大写并返回。

## String.prototype.trim()
从字符串的开始和结尾去除空格。参照部分 ECMAScript 5 标准。

## String.prototype.valueOf()
返回特定对象的原始值。重写 Object.prototype.valueOf 方法。

## String.prototype\[@@iterator\]()
Returns a new Iterator object that iterates over the code points of a String value, returning each code point as a String value.