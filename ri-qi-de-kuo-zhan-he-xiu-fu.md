Date 构造器是 JavaScript 中传参形式最丰富的构造器，大致分为4种。

```js
new Date();
new Date(timestamp);
new Date(dateString);
new Date(year, month, day, hour, minute, second, millisecond);
```

其中第3种可以玩多种花样，个人建议只使用 "1990/01/01 12:00:00“ 这样的格式。后面的时分秒可省略。这个格式所有浏览器都支持。此构造器的兼容列表可见下文：[http://dygraphs.com/date-formats.html](http://dygraphs.com/date-formats.html)

### Date的扩展

```js
// 传入两个Date类型的日期，求出它们相隔多少天。
function getDatePeriod (start, finish) {
    return Math.abs(start * 1 - finish * 1) / 60 / 60 / 1000 / 24;
}

// 传入一个 Date 类型的日期，求出它所在月的第一天。
function getFristDateInMonth (date) {
    return new Date(date.getFullYear(), date.getMonth(), 1);
}

// 传入一个 Date 类型的日期，求出它所在月最后一天。
function getLastDateInMonth (date) {
    return new Date(date.getFullYear(), date.getMonth() + 1, 0);
}

// 传入一个 Date 类型的日期，求出它所在季度的第一天。
function getFirstDateInQuarter (date) {
    return new Date(date.getFullYear(), ~~(date.getMonth() / 3) * 3, 1);
}

// 传入一个 Date 类型的日期，求出它所在季度的最后一天。
function getLastDateInQuarter (date) {
    return new Date(date.getFullYear(), ~~(date.getMonth() / 3) * 3 + 3, 0);
}

// 判断是否为闰年
function isLeapYear (date) {
    return new Date(this.getFullYear(), 2, 0).getDate() == 29;
}

// 取得当前月份的天数
// 取得当前月份的天数
function getDatasInMonth (date) {
   return new Date(date.getFullYear(), date.getMonth() + 1, 0).getDate();
}
```



