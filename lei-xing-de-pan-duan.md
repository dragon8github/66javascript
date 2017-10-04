JavaScript 存在两套类型系统：

* 基本数据类型，分别是undefined、null、string、number、boolean、object 和 function。后五种可以通过 typeof 来检测；

* 对象类型系统，通过 instanceof 来检测。

然而，这两套识别机制非常不靠谱，存在各种各样的坑和陷阱：

```js
typeof null                             // => "object"
typeof document.childNodes              // => safari "function"
typeof document.createElement('embed')  // => ff3-10 "function"
typeof document.createElement('object') // => ff3-10 "function"
typeof document.createElement('applet') // => ff3-10 "function"
typeof /\d/i                            // => 在实现了ECMA262v4的浏览器返回 "function"
typeof window.alert                     // => IE678 "object"
```

判断是否为数组

```js
function isArray (arr) {
    return arr instanceof Array;
}
```

null、undefined、NaN的判断

```js
// NaN - JavaScript 中唯一自己不等于自己的值
function isNaN (obj) {
    return obj !== obj
}

function isNull (obj) {
    return obj === null;
}

// 使用void 0 来判断undefined是避免undefined被重写导致判断不正确。
function isUndefined (obj) {
    return obj === void 0;
}
```



