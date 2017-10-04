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

（ps: 不明白引用类型和值类型区别的同学需要自行扩展学习。简而言之，值类型是变量的副本，就如我们Ctrl + C、V产生的内容一样。当副本变化的时候，不会影响源的变化。引用类型则相反，当你对引用的值进行修改时，源的内容会被修改。所有其他引用的值一样会发生变化。）

知道这一点后就很容易找出问题了：每当 “i” 发生变化的时候（**i++**），哪怕是已经躺在 "result“  数组的函数，也会跟着发生变化！

所以，当循环结束时，所有函数的"i" 都为 "a.length + 1"  \(该场景中为 **"7"**\)。

那么当我们调用warp\[0\]的函数时，实际等于执行 "return a\[7\]", 由于超出了数组索引范围，所以显然是返回 **undefined**

---

解决方法是通过创建一个**立即执行函数（IIFE） —— 局部作用域。**

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

~~这一次闭包存储了 “j” 的引用，但为什么这次不会导致上述的问题呢？~~

~~答案就在于**立即执行函数（IIFE）。**尽管闭包确实引用了变量 “j”， 但 "j" 由于每次都被重新创建和赋值 "i" , 从而保证了 "j" 的值就是函数创建时的 "i"。 于是我们的问题得到解决。~~

