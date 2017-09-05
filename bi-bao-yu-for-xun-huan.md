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
w(); // => ????
```

我们可能希望这段程序输出的是 1 ， 但实际上它输出的是 undefined

---

问题就出在变量 “i” 上，程序员似乎期待内部函数使用 “a\[i\]” 时的 “i” ， 是创建时变量 “i” 的值。这种想法是完全符合逻辑的。

只是忽略了闭包的一个特性： **闭包存储外部变量的引用而不是值。**

不明白引用类型和值类型区别的同学需要自行扩展学习。

知道这一点后就很容易找出问题了。每当 “i” 发生变化的时候（i++），哪怕是已经生成并且放入result数组的函数也会发送变化，当循环结束时，i的值为 "a.length + 1"  该场景中为 "7" 

