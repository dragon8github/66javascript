# 什么是闭包？

**闭包实际上也是一个函数**，如果一个函数同时具备以下三个表征（缺一不可），就可以判断该函数是一个闭包。

* 它必须作为函数中的函数
* 他必须引用外部函数定义的变量
* 外部函数必须把内部函数以某种形式返回
* 外部函数必须被执行，闭包才会产生。

> 1、它必须作为函数中的函数，既内部函数，包裹它的函数我们称为外部函数。

```js
// 外部函数
function warpFunc() {
    // 内部函数（闭包）
    function innerFunc() {
        return 'fuck'
    }
}

// 比较复杂的情况
// 外部函数
function warpElements(a) {
   var result = [], i, n;
   for (i = 0, n = a.length; i < n; i++) {
        // 内部函数
        result[i] = function () { return a[i]; }
   }
   return result;
}
```

> 2、 他必须引用外部函数定义的变量

```js
function warpFunc() {
    let foo = 'Fuck'
    function innerFunc() {
        return foo
    }
}
```

> 3、 外部函数必须把内部函数以某种形式返回， 通常有三种返回形式：

* 直接返回函数\(常用\)

```js
function warpFunc() {
    let foo = 'Fuck'
    function innerFunc() {
        return foo
    }
    return innerFunc
}


// 通常会利用匿名函数，来书写一种更简洁的表达式：
function warpFunc() {
    let foo = 'Fuck'
    return function() {
        return foo
    }
}
```

* 通过对象返回多个函数\(最常用\)

```js
function warpFunc() {
    let foo = 'Fuck'
    function innerFunc() {
        return foo
    }
    return {
        innerFunc: innerFunc,
        bar: function () {
            return 'bar'
        }
    }
}
```

* 通过数组返回多个函数\(极少用\)

```js
function warpFunc() {
    let foo = 'Fuck'
    function innerFunc() {
        return foo
    }
    function bar () {
        return 'bar'
    }
    return [innerFunc, bar];
}
```

综上所述，一个完整有意义的闭包，基本表现形式大致如下：

```js
function warpFunc() {
    let foo = 'Fuck'
    return function innerFunc() {
        return foo;
    }
}
```

是不是特别简单？

---

# 闭包的意义

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

