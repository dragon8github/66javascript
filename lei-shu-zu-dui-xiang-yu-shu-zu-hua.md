**何为类数组？**

> 拥有length属性却不具备数组的方法的对象

* 浏览器下存在许多类数组对象，如 function代码内的arguments变量也是一个类数组对象。

* 大部分通过JavaScript DOM操作获取的DOM集合也是类数组对象，如：document.forms、form.elements、document.link，select.opions、document.getElementsByName、document.getElemetnsByTagName、document.querySelectorAll、childNodes、chldren等方式获取的节点集合（HTMLCollection、NodeList）。

* 依照某些特殊写法的自定义对象：

```js
var obj = {
    0: "a",
    1: "b",
    2: "c",
    length: 3
}
```

尽管类数组对象是一个很好的存储结构，但它享受不了数组那些便捷的方法，所以为了方便通常会将它转化为数组。

优先考虑使用 Array.prototype.slice 来转换类数组对象。而唯一障碍就是旧版本IE（IE &lt; 9）下，HTMLCollection、NodeList不是 Object 的子类，采用该方法将导致IE执行异常。

```
Array.prototype.slice.call(arguments)
```

我们来看看各大库是怎么处理的

1、jQuery的MakeArray

```js
var makeArray = function (arg) {
    var ret = [];
    if (arg != null) {
        var i = arg.length;
        // typeof arg === 'function' 不总是有效的，譬如在IE 8 下
        // - typeof alert => 'object'
        // - typeof document.createElement('input').getAttribute => 'object'
        // 所以这里jquery是使用$.isFunction 来判断，我替换掉了
        if (i == null || typeof arg === 'string' || typeof arg === 'function' || arg.setInterval) {
            ret[0] = arg;
        } else {
            while (i)
                ret[--i] = arg[i]
        }
    }
    return ret;
}
```

2、Prototype.js的 $A

```js
function $A (arg) {
    if (!arg) 
        return [];
    if (arg.toArray) 
        return arg.toArray();
    var length  = arg.length || 0,
        results = new Array(length);
    while(length--) 
        results[length] = arg[length];
    return results;
}
```

3、

