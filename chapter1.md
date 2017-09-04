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



