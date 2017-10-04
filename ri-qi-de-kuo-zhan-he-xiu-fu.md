Date 构造器是 JavaScript 中传参形式最丰富的构造器，大致分为4种。

```js
new Date();
new Date(timestamp);
new Date(dateString);
new Date(year, month, day, hour, minute, second, millisecond);
```

其中第3种可以玩多种花样，个人建议只使用 "1990/01/01 12:00:00“ 这样的格式。后面的时分秒可省略。这个格式所有浏览器都支持。此构造器的兼容列表可见下文：http://dygraphs.com/date-formats.html



