与许多面向对象的语言一样，JavaScript 支持继承，既通过动态代理机制重用代码或数据。

但与传统面向对象语言不同的是，JavaScript 的继承机制基于**原型，而不是类。在JavaScript 中实际上没有类的概念**

**JavaScript 是第一个不需要类而实现面向对象的语言**

在许多语言中，每个对象是相关类的实例，类提供在其所有实例间共享代码。相反，JavaScript 并没有类的概念。对象是从其他对象中继承而来的。每个对象与其他一些对象是相关的，这些对象称之为原型。尽管依然使用了很多传统的面向对象语言的概念，但使用原型与使用类有很大差异。

Prototype、getPrototype和\_\_proto\_\_之间的关系

* User.prototype ： 由new User\(\)创建的**对象原型**
* Object.getPrototype\(obj\)： 是ES5用来**获取对象原型的标准方法**。
* obj.\_\_proto\_\_：** 获取对象原型的非标准方法，可作为不支持ES5环境的Object.getPrototype\(obj\)方法的权宜之计。**

```js
function User (name, passwordHash) {
    this.name = name;
    this.passwordHash = passwordHash
}

User.prototype.toString = function () {
    return "[User " + this.name + "]";
}

User.prototype.checkPassword = function (password) {
    return hash(password) === this.passwordHash
}

var u = new User('Lee', '123456')

Object.getPrototypeOf(u) === User.prototype; // true
u.__proto__ === User.prototype; // true
```



