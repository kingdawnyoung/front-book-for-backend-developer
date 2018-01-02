# moment.js

JavaScript 日期处理类库

## 解析

Moment.js只是创建了一个封装`Date`的对象，并没有对`Date.prototype`进行修改。要获取M的封装对象只要简单的调用`moment()`即可。

获取当前时间：

```js
moment()

//或者
moment(new Date())
```

根据字符串返回一个时间

```js
moment("2017-12-25")
```

上面的例子会因为浏览器的日期格式标准会有所差异，所以推荐使用支付串+格式的形式

```js
moment("2017-12-25", 'YYYY-MM-DD')
```

涉及到格式就会有对应的[占位符](http://momentjs.cn/docs/#/parsing/string-format/)

你也可以通过传入一个对象，指定每个日期中的每个元素

```js
//2017-12-25 15:10:03.123
moment({ years:2017, months:12, days:25, hours:15, minutes:10, seconds:3, milliseconds:123});
moment({ year :2017, month :12, day :25, hour :15, minute :10, second :3, millisecond :123});
moment({ y    :2017, M     :12, d   :25, h    :15, m      :10, s      :3, ms          :123});

//days和day 可以用date替换
moment({ year :2017, month :12, date :25, hour :15, minute :10, second :3, millisecond :123});
```

可以通过偏移量(毫秒)创建一个包装对象，偏移量是相对于`1970-1-1 00:00`

```js
moment(1318781876406)

//通过Unix时间戳(秒)
moment.unix(1318781876);
```

通过一个Date对象创建一个包装对象

```
moment(new Date(2017, 12, 25))
```

通过一个数组创建一个包装对象，顺序和`new Date()`一致

```js
moment([2017, 11, 25])

//utc时间
moment.utc([2017, 11, 25])
```

通过`.clone()`可以克隆一个包装对象

```js
moment().clone();
```

通过`moment.utc()`可以创建一个协调世界时间

```js
moment.utc("2017-12-15")
//其他方法
```

创建包装对象时，可以缺省某些值

```js
moment();// 当前日期时间
moment(5, "HH");  // 今天, 5:00:00.000
moment({hour: 5});  // 今天, 5:00:00.000
moment({hour: 5, minute: 10});  // 今天, 5:10.00.000
moment({hour: 5, minute: 10, seconds: 20});  // 今天, 5:10.20.000
moment({hour: 5, minute: 10, seconds: 20, milliseconds: 300});  // 今天, 5:10.20.300

moment(5, "DD");  // 当前月, 第五天
moment("4 05:06:07", "DD hh:mm:ss");  // 当前月, 第四天, 05:06:07.000
```

## 取值/赋值