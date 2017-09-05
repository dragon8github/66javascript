# 什么是闭包？

**闭包实际上也是一个函数**，如果一个函数同时具备以下三个表征（缺一不可），就可以判断该函数是一个闭包。

1、只是它必须作为函数中的函数，既内部函数，包裹它的函数我们称为外部函数。

```js
// 外部函数
function warpFunc() {
    // 内部函数（闭包）
    function innerFunc() {
        return 'fuck'
    }
}
```

2、 他必须引用外部函数定义的变量

```js
function warpFunc() {
    let foo = 'Fuck'
    function innerFunc() {
        return foo
    }
}
```

3、 外部函数必须把内部函数以某种形式返回， 通常有三种返回形式：

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

