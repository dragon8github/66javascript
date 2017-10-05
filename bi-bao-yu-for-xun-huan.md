下面这段（Bug）程序将会输出什么？

```js
function warpElements(a) {
   var result = [], i, n;
   for (i = 0, n = a.length; i < n; i++) {
        result[i] = function () { return a[i]; }
   }
   return result;
}
var warp =  warpElements([1, 2, 3, 4, 5, 6]);
var w = warp[0];
w(); // => ?
```

我们可能希望这段程序输出的是 1 ， 但实际上它输出的却是 undefined

---

问题就出在变量 “i” 上，程序员似乎期待内部函数使用 “a\[i\]” 时的 “i” ， 是创建时变量 “i” 的值。这本是理所当然的事。

只是我们忽略了闭包的一个特性： **闭包存储外部变量的引用而不是值。**

（ps: 不明白引用类型和值类型区别的同学需要自行扩展学习。简而言之，值类型是变量的副本，就如我们Ctrl + C、V产生的副本一样。当副本变化的时候，不会影响源的变化。引用类型则相反，当你对引用的值进行修改时，源的内容会被修改。所有其他引用的值一样会发生变化。）

知道这一点后就很容易找出问题了：每当 “i” 发生变化的时候（**i++**），哪怕是已经躺在 "result“  数组的函数，也会跟着发生变化！

所以，当循环结束时，所有函数的"i" 都为 “6”。

那么当我们调用warp\[0\]的函数时，实际等于执行 "return a\[6\]", 由于超出了数组索引范围，所以显然是返回 **undefined**

---

解决方法是通过**立即执行函数（IIFE）创建一个局部作用域 **

```js
function warpElements(a) {
    var result = [];
    for (i = 0, n = a.length; i < n; i++) {
        (function () {
            var j = i;
            result[i] = function() { return a[j]; }
        })() 
    }
    return result;
}
var warp = warpElements([1, 2, 3, 4, 5, 6]);
var w = warp[0];
w(); // => 1
```

---

为什么这次不会导致上述的问题呢？

在回答这个问题之前，我们先看三个Demo

```js
// Demo1： 外部函数与立即执行函数（IIFE）
function warpElements(a) {
    var result = [];
    for (i = 0, n = a.length; i < n; i++) {
        (function() {
            result[i] = i; 
        })()
    }
    return result;
}
var warp = warpElements([1, 2, 3, 4, 5, 6]);
var w = warp[0];
console.log(w) // => 0
```

我们先重温一下闭包的两个特征：

1、闭包就是函数中的函数。既外部函数和内部函数的关系。而内部函数就是我们说的闭包；

2、外部函数定义的变量被内部函数使用时，那么这个变量就会变成引用类型，所以变量一旦变化，也会影响内部函数中引用变量的变化。

通过上述的例子，我们需要分别对这两个问题进行思考：

如果内部函数是一个立即执行函数（IIFE）呢？这个立即函数还会是一个闭包么？它还会产生引用关系吗？

带着这两个问题，我们继续看下面两个demo：

```js
function warpElements(a) {
    var result = [];
    for (i = 0, n = a.length; i < n; i++) {
        (function() {
            result[i] = function() { console.log(i); }
        })()
    }
    return result;
}
var warp = warpElements([1, 2, 3, 4, 5, 6]);
var w = warp[0];
w() // => 6
```

这次的demo显示，立即执行函数IIFE中的（内部）函数，依然和外部函数存在引用关系。

那么我们看最后一个Demo:

```js
function warpElements(a) {
    var result = [];
    for (i = 0, n = a.length; i < n; i++) {
        (function() {
            result[i] = (function() { return i; })();
        })()
    }
    return result;
}
var warp = warpElements([1, 2, 3, 4, 5, 6]);
var w = warp[0];
w() // => 0
```

~~由此得出，局部作用域、立即执行函数IIFE确实有阻断外部引用的作用。~~

并非阻碍了引用，而是产生不了闭包罢了。所以，解决方案之所以成立，是因为利用了“没有引用”的特性？

