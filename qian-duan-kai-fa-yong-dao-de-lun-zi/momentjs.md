# moment.js

JavaScript 日期处理类库

## 解析

Moment.js只是创建了一个封装`Date`的对象，并没有对`Date.prototype`进行修改。要获取M的封装对象只要简单的调用`moment()`即可。

获取当前时间：

```js
moment()
moment([])
moment({})

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
//值也可以传入字符串
moment({ years:"2017", months:"12", days:"25", hours:"15", minutes:"10", seconds:"3", milliseconds:"123"});
```

可以通过偏移量(毫秒)创建一个Moment对象，偏移量是相对于`1970-1-1 00:00`

```js
moment(1318781876406)

//通过Unix时间戳(秒)
moment.unix(1318781876);
```

通过一个Date对象创建一个Moment对象

```
moment(new Date(2017, 12, 25))
```

通过一个数组创建一个Moment对象，顺序和`new Date()`一致

```js
moment([2017, 11, 25])

//utc时间
moment.utc([2017, 11, 25])
```

通过`.clone()`可以克隆一个Moment对象

```js
moment().clone();
```

通过`moment.utc()`可以创建一个协调世界时间

```js
moment.utc("2017-12-15")
//其他方法
```

创建Moment对象时，可以缺省某些值

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

毫秒

```js
moment().millisecond(Number);
moment().millisecond(); // Number
moment().milliseconds(Number);
moment().milliseconds(); // Number
```

秒

```js
moment().second(Number);
moment().second(); // Number
moment().seconds(Number);
moment().seconds(); // Number
```

分钟

```js
moment().minute(Number);
moment().minute(); // Number
moment().minutes(Number);
moment().minutes(); // Number
```

小时

```js
moment().hour(Number);
moment().hour(); // Number
moment().hours(Number);
moment().hours(); // Number
```

日期

```js
moment().date(Number);
moment().date(); // Number
```

星期

```js
moment().day(Number|String);
moment().day(); // Number
moment().days(Number|String);
moment().days(); // Number
```

月

```js
moment().month(Number|String);
moment().month(); // Number
moment().months(Number|String);
moment().months(); // Number
```

天，一年中的第几天

```js
moment().dayOfYear(Number);
moment().dayOfYear(); // Number
```

周，一年中的第几周

```js
moment().week(Number);
moment().week(); // Number
moment().weeks(Number);
moment().weeks(); // Number
```

季度

```js
moment().quarter(); // Number
moment().quarter(Number);
```

年

```js
moment().year(Number);
moment().year(); // Number
moment().years(Number);
moment().years(); // Number
```

通过get取值

```js
moment().get('year');
moment().get('month');  // 0 to 11
moment().get('date');
moment().get('hour');
moment().get('minute');
moment().get('second');
moment().get('millisecond');
```

通过set赋值

```js
moment().set('year', 2017);
moment().set('month', 11);  // April
moment().set('date', 25);
moment().set('hour', 5);
moment().set('minute', 10);
moment().set('second', 30);
moment().set('millisecond', 300);

moment().set({'year': 2017, 'month': 12});
```

返回给定Moment对象中的日期最大的

```js
moment.max(Moment[,Moment...]);
```

返回给定Moment类的日期最小的

```js
moment.min(Moment[,Moment...]);
```

## 操作

加法

```js
moment().add(Number, String);
moment().add(Duration);
moment().add(Object);
```

```js
moment().add(7, 'days').add(1, 'months');
moment().add({days: 7, months: 1});
```

减法

```js
moment().subtract(Number, String);
moment().subtract(Duration);
moment().subtract(Object);
```

```js
moment().subtract(1, 'days').subtract(1, "months");
```

开始时间，更改日期到对应日期单元的开始时间

```js
moment().startOf(String);

moment().startOf('year');
moment().startOf('month');
moment().startOf('quarter');
moment().startOf('week');
moment().startOf('isoWeek');
moment().startOf('day');
moment().startOf('hour');
moment().startOf('minute');
moment().startOf('second');
```

结束时间，更改日期到对应时间单元的结束时间

```js
moment().endOf(String);
```

## 显示

格式化，对应[占位符](http://momentjs.cn/docs/#/displaying/format/)

```js
moment().format();
moment().format(String);
```

```js
moment.format("YYYY-MM-DD")
```

时差(毫秒)

```js
moment().diff(Moment|String|Number|Date|Array);
moment().diff(Moment|String|Number|Date|Array, String);
moment().diff(Moment|String|Number|Date|Array, String, Boolean);
```

```js
//默认返回毫秒
var a = moment([2018, 0, 3]);
var b = moment([2018, 0, 2]);
a.diff(b) // 86400000
```

```js
//可以指定比较时间单元
var a = moment([2018, 0, 3]);
var b = moment([2018, 0, 2]);
a.diff(b, "days") // 1
```

```js
//置顶时间单元，精确到小数
var a = moment([2018, 0, 3, 12]);
var b = moment([2018, 0, 2]);
a.diff(b, "days", true) // 1.5
```

返回一个月的总天数

```js
moment().daysInMonth();
```

获取原生的Date对象

```js
moment().toDate();
```

获取一个日期单元的数组

```js
moment().toArray();
```

返回一个json序列化时的字符串

```js
moment().toJSON();

//可以功过moment.fn-toJSON改些格式
moment.fn.toJSON = function() { return this.format("YYYY-MM-DD"); }
```

返回一个日期单元的对象

```js
moment().toObject()
```

## 查询

日期是否在指定日期之前

```js
moment().isBefore(Moment|String|Number|Date|Array);
moment().isBefore(Moment|String|Number|Date|Array, String);
```

```js
var a = moment([2018, 0, 3, 12]);
var b = moment([2018, 0, 2]);

a.isBefore(b); //false

//可以指定到比较的时间单元
a.isBefore(b, "year"); //false
```

日期是否与指定日期相同

```js
moment().isSame(Moment|String|Number|Date|Array);
moment().isSame(Moment|String|Number|Date|Array, String);
```

```js
var a = moment([2018, 0, 3, 12]);
var b = moment([2018, 0, 2]);

a.isSame(b); //fals
//可以指定到比较的时间单元
a.isSame(b, "month"); //true
```

日期是否在指定日期之后

```js
moment().isAfter(Moment|String|Number|Date|Array);
moment().isAfter(Moment|String|Number|Date|Array, String);
```

```js
var a = moment([2018, 0, 3, 12]);
var b = moment([2018, 0, 2]);

a.isAfter(b); //true
//可以指定到比较的时间单元
a.isAfter(b, "month"); //false
```

日期是否在指定日期之前或与之相同

```js
moment().isSameOrBefore(Moment|String|Number|Date|Array);
moment().isSameOrBefore(Moment|String|Number|Date|Array, String);
```


日期是否在指定日期之后或与之相同

```js
moment().isSameOrAfter(Moment|String|Number|Date|Array);
moment().isSameOrAfter(Moment|String|Number|Date|Array, String);
```

是否在指定时间之间

```js
moment().isBetween(moment-like, moment-like);
moment().isBetween(moment-like, moment-like, String);
// where moment-like is Moment|String|Number|Date|Array
```

```js
moment('2010-10-20').isBetween('2010-10-19', '2010-10-25'); // true
```


是否闰年

```js
moment().isLeapYear();
```

是否是Moment对象

```js
moment.isMoment(obj);
```

是否是Date对象

```js
moment.isDate();
```

## 时间段

创建一个时间段

```js
moment.duration(Number, String);
moment.duration(Number);
moment.duration(Object);
moment.duration(String);
```

```js
moment.duration(2, 'seconds');
moment.duration(2, 'minutes');
moment.duration(2, 'hours');
moment.duration(2, 'days');
moment.duration(2, 'weeks');
moment.duration(2, 'months');
moment.duration(2, 'years');
```

```js
moment.duration(2);
```

```js
moment.duration({
  seconds: 2,
  minutes: 2,
  hours: 2,
  days: 2,
  weeks: 2,
  months: 2,
  years: 2
});
```

```js
moment.duration('7.23:59:59.999');
```

克隆

```js
moment.duration().clone();
```

获取时间段的毫秒数

```js
//毫秒
moment.duration().milliseconds();
//总毫秒数
moment.duration().asMilliseconds();
```

获取时间段的秒数

```js
//秒数
moment.duration().seconds();
//总秒数
moment.duration().asSeconds();
```

获取时间段天数

```js
//天数
moment.duration().days();
//总天数
moment.duration().asDays();
```

获取时间段的月数

```js
//月数
moment.duration().months();
//总月数
moment.duration().asMonths();
```

获取时间段的年数

```js
//年数
moment.duration().years();
//总年数
moment.duration().asYears();
```

加法

```js
moment.duration().add(Number, String);
moment.duration().add(Number);
moment.duration().add(Duration);
moment.duration().add(Object);
```

减法

```js
moment.duration().subtract(Number, String);
moment.duration().subtract(Number);
moment.duration().subtract(Duration);
moment.duration().subtract(Object);
```

转换单位

```js
moment.duration().as(String);
```

取值

```js
moment.duration().get(String);
```

返回一个json序列化时的字符串

```js
moment.duration().toJSON();
```

是否是时间段

```js
moment.isDuration(obj);
```

## 工具

创建一个无效的Moment对象

```js
moment.invalid(Object);
```
