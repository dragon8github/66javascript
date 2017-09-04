理解 JavaScript 变量提升行为的一个好方法是把变量声明看作由两部分组成，既**声明**和**赋值**。

JavaScript 隐式地将变量和函数的声明部分提升到封闭函数的顶部。而将赋值留在原地。

如果不明白 JavaScript 的这一特性将会导致一些奇妙的bug， 例如

```js
function isWinner (player, others) {
    var highest = 0;
    for (var i = 0, n = others.length; i < n; i++) {
        var player = others[i];
        if (player.score > highest) {
            highest = player.score;
        }
    }
    return player.score > highest;
}
```

该程序在for循环体内声明了一个局部变量 player ，** 但是！但是！！ 但是！！！ JavaScript 中变量是函数级作用域，而不是块级作用域。**所以for循环中的player实际上已经重写了参数player. 这就是变量提升的特性导致的。

因为重声明会导致截然不同的变量展现，一些程序员喜欢手动将函数中所有的变量放置在函数顶部定义（手动变量提升），从而避免歧义。

```js
var spa_chat = (function () {
    //---------------BEGIN MODULE SCOPE VARIABLES-------------------
    var 
        onClickToggle     = function () {},
        getEmSize         = function () {},
        setPxSizes        = function () {},
        setSliderPosition = function () {},
        setJqueryMap      = function () {},
        initModule        = function () {},
        configModule      = function () {}, 
        removeSlider      = function () {},
        handleResize      = function () {},
        jqueryMap = {},         
        stateMap  = {
            $append_target   : null,
            position_type    : 'closed',
            px_per_em        : 0,
            slider_hidden_px : 0,
            slider_closed_px : 0,
            slider_opened_px : 350
        }
    //---------------END MODULE SCOPE VARIABLES-------------------  
}();
```

无论你是否喜欢这种风格，重要的是，要理解 JavaScript 的作用域规则。

JavaScript 没有块级作用域，然而仅有的一个例外： 异常处理try/catch语句。

try/catch语句将捕获的异常绑定到一个变量，该变量的作用域只是catch语句块。





