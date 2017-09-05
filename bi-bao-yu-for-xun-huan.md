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



