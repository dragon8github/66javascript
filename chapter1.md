# 

付出努力掌握闭包将会给你带来超值的回报。理解闭包只需要学会三个基本的事实。

第一个事实： JavaScript 允许你引用在当前函数以外定义的变量。

```js
function makeSandwith () {
    var magicIngRedient = 'Peanut butter';

    function make (filling) {
        return magicIngRedient + 'and' + filling
    }

    return make('jelly');
}

makeSandwith() // => "Peanut butterandjelly"
```

请注意内部的make函数是如何引用定义在外部的 makeSandwith 函数内的 magicIngRedient 变量的。



第二个事实： 即使外部函数已经返回，当前函数仍然可以**引用**在外部函数所定义的变量。（就如吸血鬼一样存活在阳光底下暗中行动）。这意味着，你可以返回一个内部函数，并在稍后调用它。这是闭包最重要的特性

```js
function sendwichMaker () {
    var magicIngredient = 'panut butter';

    function make (fillling) {
        return magicIngredient + ' and ' + fillling;
    }

    return make
}

var f = sendwichMaker();

f('jelly'); // => "panut butter and jelly"
f('bananas'); // => "panut butter and bananas"
f('marshmallows'); // => "panut butter and marshmallows"
```

值得一提的是，闭包（内部函数）对变量存储的方式是**引用。**什么是引用？ 这就和引用类型 与 值类型的概念一样。请自行扩展了解。

