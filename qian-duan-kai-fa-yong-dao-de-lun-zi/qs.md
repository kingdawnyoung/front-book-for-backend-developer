{% raw %}
# qs

安全地解析查询字符串、字符化查询字符串的库

```js
var qs = require('qs');

var obj = qs.parse('a=c');//{b:'c'}
var str = qs.stringify(obj);//"a=c"
```

## 转化为对象

```js
qs.parse(string, [options]);
```

```js
qs.parse('foo[bar]=baz')
//可以转化编码后的查询字符串
qs.parse('a%5Bb%5D=c')
//限制转化深度
qs.parse('foo[bar][baz]=foobarbaz',{ depth: 1 })
//限制转化个数
qs.parse('a=b&c=d', { parameterLimit: 1 })
//忽略查询字符串前缀
qs.parse('?a=b&c=d', { ignoreQueryPrefix: true })
//指定分割符
qs.parse('a=b;c=d', { delimiter: ';' })
//分隔符可以是一个正则
qs.parse('a=b;c=d,e=f', { delimiter: /[;,]/ });
//查询字符串对象除了可以使用方括号还可以使用半角句号(.)
qs.parse('a.b=c', { allowDots: true })
```

## 转化为数组

```js
//在连接中传递数据一般在键后跟使用空的方括号
qs.parse('a[]=b&a[]=c')
//同样可以在方括号内加上索引
qs.parse('a[1]=c&a[0]=b') //{a : ['b', 'c']}
//索引仅标识顺序
qs.parse('a[1]=b&a[15]=c') //{a: ['b', 'c']}
//如果某个键的值为空则转化为空字符串
qs.parse('a[]=&a[]=b')
//qs中限制数组默认最大长度为20
qs.parse('a[100]=b') //{ a: { '100': 'b' } }
//可以通过arrayLimit限制其长度
qs.parse('a[1]=b', { arrayLimit: 0 }) //{ a: { '1': 'b' } }
//可以组织数组的转化
qs.parse('a[]=b', { parseArrays: false }) //{ a: { '0': 'b' } }
//查询字符串可以混合数组和对象, 默认整合到对象
qs.parse('a[0]=b&a[b]=c') //{ a: { '0': 'b', b: 'c' } }
//可以创建一个对象数组
qs.parse('a[][b]=c') //{ a: [{ b: 'c' }] }
```

## 转化为字符串

```js
qs.stringify(object, [options]);
```

```js
qs.stringify({ a: 'b' })
//默认会对方括号进行编码
qs.stringify({ a: { b: 'c' } }) //'a%5Bb%5D=c'
//可以指定不编码
qs.stringify({ a: { b: 'c' } }, { encode: false }) //'a[b]=c'
//可以指定仅对值进行编码
qs.stringify(
    { a: 'b', c: ['d', 'e=f'], f: [['g'], ['h']] },
    { encodeValuesOnly: true }
) // 'a=b&c[0]=d&c[1]=e%3Df&f[0][0]=g&f[1][0]=h'
//可以自定义编码格式
qs.stringify({ a: { b: 'c' } }, { encoder: function (str) {
    // Passed in values `a`, `b`, `c`
    return // Return encoded string
}})

//转化数组时默认会添加索引
qs.stringify({ a: ['b', 'c', 'd'] }) // 'a[0]=b&a[1]=c&a[2]=d'
//当然也可以设置不添加
qs.stringify({ a: ['b', 'c', 'd'] }, { indices: false }) // 'a=b&a=c&a=d'
//你可以手动转化的数组形式
qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'indices' }) // 'a[0]=b&a[1]=c'
qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'brackets' }) // 'a[]=b&a[]=c'
qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'repeat' }) // 'a=b&a=c'

//转化对象时默认使用方括号键值的形式
qs.stringify({ a: { b: { c: 'd', e: 'f' } } }) // 'a[b][c]=d&a[b][e]=f'
//也可以指定为半角句号(.)
qs.stringify({ a: { b: { c: 'd', e: 'f' } } }, { allowDots: true }) // 'a.b.c=d&a.b.e=f'

//空字符串和null会省略值
qs.stringify({ a: '' }) // 'a='
//对于空数组和空对象不会被转化
qs.stringify({ a: [] })
qs.stringify({ a: {} })
qs.stringify({ a: [{}] })
qs.stringify({ a: { b: []} })
qs.stringify({ a: { b: {}} })
//如果值为undefined也不会转化
qs.stringify({ a: null, b: undefined }) // 'a='

//有时我们需要在转化的字符串前加问号(?)
qs.stringify({ a: 'b', c: 'd' }, { addQueryPrefix: true }) // '?a=b&c=d'
//我们同样可以自定义风格符
qs.stringify({ a: 'b', c: 'd' }, { delimiter: ';' }) // 'a=b;c=d'

//可以转化日期
qs.stringify({ a: new Date() }) //a=2018-01-04T07%3A05%3A30.489Z
//可以自定义日期的转化形式
qs.stringify({ a: new Date() }, { serializeDate: function (d) { return d.getTime(); } }) // a=1515049621472

//可以指定查询字符串键的顺序
qs.stringify({ a: 'c', z: 'y', b : 'f' }, { sort: alphabeticalSort })

//通过filter过滤出需要的键
qs.stringify({ a: 'b', c: 'd', e: 'f' }, { filter: ['a', 'e'] }) // 'a=b&e=f'
qs.stringify({ a: ['b', 'c', 'd'], e: 'f' }, { filter: ['a', 0, 2] }) // 'a[0]=b&a[2]=d'

```

## 处理null值

```js
qs.stringify({ a: null, b: '' }) //'a=&b='
qs.parse('a&b=') //{ a: '', b: '' }

//null值严格模式
qs.stringify({ a: null, b: '' }, { strictNullHandling: true } //'a&b='
qs.parse('a&b=', { strictNullHandling: true } //{ a: null, b: '' }

//通过可以指定忽略null
qs.stringify({ a: 'b', c: null}, { skipNulls: true }) //'a=b'
```

{% endraw %}
