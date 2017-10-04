对象的扩展机制，通常我们是做成一个方法，叫做extend 或 mixin。

javascript 对象在es5的 [属性描述符 （Object.defineProperty / Object.defineProperties）](https://segmentfault.com/a/1190000007290020)没有诞生之前，是可以随意增加、更改、删除其成员的，因此扩展一个对象非常便捷：

```js
function extend (destination, source) {
    // 通过for in 循环遍历出对象所有的属性名、方法名
    for (var property in source) {
        destination[property] = source[property];
    }
    return destination;
}
var obj = {};
extend(obj, {a: "a", b: "b"});
```

但在低版本的IE有个问题，for in 循环是无法遍历名为 valueOf、toString 的属性名。IE认为这些原型方法不应该被遍历出来。后来人们模拟 Object.keys 方法实现：

```js
Object.keys = Object.keys || function () {
    var a = [];
    // 请注意这个 for in 与数组的赋值技巧
    for(a[a.length] in obj);
    return a;
}
```



