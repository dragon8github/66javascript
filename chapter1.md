# 

闭包实际上也是一个函数，只是它必须作为函数中的函数，既内部函数，包裹它的函数我们称为外部函数。

而且，必须通过外部函数返回内部函数（或者包含一个、多个内部函数的对象），才算完整有意义的闭包，我们以一个简单的例子说明：

```js
function warpFunc () {
    let foo = 'Fuck'
    function innerFunc () {
        return foo
    }
    return innerFunc
}
var f = warpFunc(); // 返回innerFunc
f(); // => "Fuck"
```

问题是，这样一个闭包（内部函数）有什么意义和作用？何必把一个函数搞得如此复杂？

确实对于不了解闭包特性的程序员来说，闭包是个陌生的概念，但是，闭包是 JavaScript 最优雅、最有表现力的特性之一，也是许多惯用法的核心。付出努力掌握闭包将会给你带来超值的回报。理解闭包只需要学会三个基本的事实。

---

**第一个事实： JavaScript 允许你引用在当前函数以外定义的变量。**

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

---

**第二个事实： 即使外部函数已经返回，当前函数仍然可以引用在外部函数所定义的变量。（就如吸血鬼一样存活在阳光底下暗中行动）。这意味着，你可以返回一个内部函数，并在稍后调用它。这是闭包最重要的特性**

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

函数可以引用在其作用域内任何变量，包括参数和外部变量。

值得一提的是，闭包（内部函数）对变量存储的方式是**引用。**什么是引用？ 这就如引用类型 与 值类型的概念一样。请自行扩展了解。

---

**第三个事实：闭包可以更新外部变量的值。**

```js
function box () {
    var val = undefined;
    return {
        set: function (newVal) {val = newVal; },
        get: function () { return val; },
        type: function () { return typeof val; }
    }
}

var b = box();
b.type(); // => 'undefined'
b.set(98.6);
b.get(); // => 98.6
b.type(); // => number
```

该例子产生一个包含三个闭包的对象。这三个闭包是set、get 和 type属性。它们都共享访问 val 变量。

