测试NaN值是异常困难的。这有两个原因。第一个是JavaScript遵循了 IEEE 浮点数标准令人头疼的要求 —— NaN不等于其本身。因此，测试一个值是否等于NaN根本行不通。

```js
var x = NaN;
x == NaN; // => false
```

另外，内置标准库 isNaN也不是很靠谱。

```js
isNaN(NaN);                // => truew
isNaN('foo');              // => true
isNaN(undefined);          // => true
isNaN({});                 // => true
isNaN({ valueOf: 'foo' }); // => true
```

幸运的是，我们可以反过来利用NaN的特性来测试是否为NaN —— NaN 是 JavaScrit 中唯一一个不等于自身的值，因此，你可以通过检查一个值是否等于其自身的方式来测试该值是否是NaN:

```js
var a = NaN;
a !== a; // true

var b = 'foo';
b !== b; // false

var c = undefined;
c !== c; // false

var d = {};
d !== d; // false

var e = { valueOf: 'foo' };
e !== e;  // false
```



