JavaScript 函数有一个非凡的特性，即将其源代码重现为字符串的能力。

```js
(function(x) {
    return x + 1;
}).toString();  // "function (x) {return x + 1; }"
```

聪明的黑客偶然会通过巧妙的方法用到它。

