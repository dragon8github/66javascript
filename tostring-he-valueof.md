当字符串与对象进行 '+' 运算时，对象通过隐式地调用自身的 toString 方法转换为字符串，类似的，Number 和 对象进行运算时，也可以通过其valueOf方法转换为数字。

```js
"J" + {toString: function () { return 'S' }}; // => "JS"
2 * {toString: function () { return 3 }}; // => 6
```

特别的，当一个对象同时包含toString 和 valueOf方法时，运算符 '+' 会调用哪个方法呢？

JavaScript 通过盲目地选择 valueOf 方法而不是 toString 方法来解决这种含糊的情况。

```js
var obj = {
    toString: function () {
        return "[object FuckObject]";
    },
    valueOf: function () {
        return 17;
    }
}

"object:" + obj // => object:17
```



