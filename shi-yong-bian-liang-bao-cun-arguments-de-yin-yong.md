迭代器（iterator）是一个可以顺序存取数据集合的对象。其一个典型的 API 是 next方法。该方法获取序列中的下一个值。

假设我们编写一个便利的函数，它可以接受任意数量的参数，并为这些值建立一个迭代器。我们期待结果如下：

```js
var it = values(1, 4, 1, 4, 2, 1, 3, 5, 6);
it.next() // => 1
it.next() // => 4
it.next() // => 1
```

由于 values 函数必须能够接受任意数量的参数， 所以我们可以构造迭代器对象来遍历 arguments 对象的元素。

```js
function values () {
    var i = 0, n = arguments.length;

    return {
        hasNext: function () {
            return i < n;
        },
        next: function () {
            if (i > n) {
                throw new Error('end of iteration');
            } 
            return arguments[i++];
        }
    }
}
```

当我们试图使用迭代器对象时问题立马就暴漏出来了：

```js
var it = values(1, 4, 1, 4, 2, 1, 3, 5, 6);
it.next() // => undefined
it.next() // => undefined
it.next() // => undefined
```

为什么会出现这个问题？

那是因为我们忽略了。当我们调用 it.next\(\) 函数的时候，next\(\) 函数本身也存在 arguments 变量，这就可以理解为什么打印出undefined了

解决这个问题的方式也非常简单，在外层函数中定义一个新变量接受 arguments 即可

```js
function values () {
    var i = 0, n = arguments.length, a = arguments;

    return {
        hasNext: function () {
            return i < n;
        },
        next: function () {
            if (i > n) {
                throw new Error('end of iteration');
            } 
            return a[i++];
        }
    }
}

var it = values(1, 4, 1, 4, 2, 1, 3, 5, 6);
it.next() // => 1
it.next() // => 4
it.next() // => 1
```



