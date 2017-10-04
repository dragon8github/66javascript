### JavaScript 对类与继承的支持

JavaScript 中并没有真正的类，但作为一个刻意模仿 java 的语言，JavaScript 也存在 new 操作符。

在其它语言中，实例对象都是通过类 new 出来的，而 JavaScirpt 则是通过构造器。任何 JavaScript 函数都可以作为构造器。

```js
function Dog () {}
Dog.prototype = {
    aa: "aa",
    method: function () {}
};
var a = new Dog;
var b = new Dog;
console.log(a.aa == b.aa)         // => true
console.log(a.method == b.method) // => true
```

我们把定义在原型上的方法叫原型方法，它为所有实例所共享。

---

### 原型模式和基于原型继承的 JavaScript对象系统

JavaScript 原型编程的思维和规则：

1. 在 JavaScript 中没有类的概念，要得到一个对象，不是通过实例化类，而是找到一个对象作为原型并克隆它；
2. 所有的数据都是对象；
3. 对象会记住它的原型；

4. 如果对象无法响应某个请求，它会把这个请求委托给它自己的原型。

> 1、在 JavaScript 中没有类的概念，要得到一个对象，不是通过实例化类，而是找到一个对象作为原型并克隆它。

```js
function Person( name ){
     this.name = name;
};
Person.prototype.getName = function(){
     return this.name;
};
var a = new Person( 'sven' );
console.log( a.name ); // 输出：sven
console.log( a.getName() ); // 输出：sven
console.log( Object.getPrototypeOf( a ) === Person.prototype ); // 输出：true
```

在这里 new Person\(\) 并不是类，而是函数构造器。

任何 JavaScript 函数都可以作为构造器。当使用 new 运算符来调用函数时，此时的函数就是一个构造器。

用 new 运算符来创建对象的过程，实际上也只是先克隆`Object.prototype`对象，再进行一些“额外操作”的过程。

我们可以通过下面这段代码来理解 new 的运算过程：

```js
var objectFactory = function() {
     // 从 Object.prototype 上克隆一个空的对象
     var obj = new Object();
     // 取得外部传入的构造器，此例是 Person
     var Constructor = [].shift.call( arguments ); 
     // 指向正确的原型
     obj.__proto__ = Constructor.prototype; 
     // 借用外部传入的构造器给 obj 设置属性
     var ret = Constructor.apply( obj, arguments ); 
     // 确保构造器总是会返回一个对象
     return typeof ret === 'object' ? ret : obj; 
};

function Person( name ){
     this.name = name;
};
Person.prototype.getName = function() {
     return this.name;
};
var a = objectFactory( Person, 'sven' );
console.log( a.name );                                          // 输出：sven
console.log( a.getName() );                                     // 输出：sven
console.log( Object.getPrototypeOf( a ) === Person.prototype ); // 输出：true
```

> 2、所有的数据都是对象

JavaScript在设计的时候，模仿 Java 引入了两套类型机制：基本类型和对象类型。

基本类型包括：undefined 、 null、 string 、 number 、 boolean 、function 、 object 。

我们不能说在 JavaScript中所有的数据都是对象，但可以说绝大部分数据都是对象。那么相信在 JavaScript中也一定会有一个根对象存在，这些对象追根溯源都来源于这个根对象。

事实上，JavaScript 中的根对象是 Object.prototype 对象。 Object.prototype 对象是一个空的对象。我们在 JavaScript 遇到的每个对象，实际上都是从 Object.prototype 对象克隆而来的，Object.prototype 对象就是它们的原型。

可以利用 ECMAScript 5提供的 Object.getPrototypeOf 来查看这两个对象的原型：

```js
console.log( Object.getPrototypeOf( obj1 ) === Object.prototype ); // 输出：true
console.log( Object.getPrototypeOf( obj2 ) === Object.prototype ); // 输出：true
```

> 3、对象会记住它的原型

目前我们一直在讨论"对象的原型"，但就JavaScript的真正实现来说，其实并不能说对象有原型，而只能说对象的构造器有原型。

“对象把请求委托给它自己的原型”这句话，更好的说法是对象把请求委托给它的构造器的原型。那么对象如何把请求顺利地转交给它的构造器的原型呢？

JavaScript给对象提供了一个名为 \_\_proto\_\_ 的隐藏属性，某个对象的 \_\_proto\_\_ 属性默认会指向它的构造器的原型对象，即{Constructor}.prototype。在一些浏览器中， \_\_proto\_\_ 被公开出来，我们可以在 Chrome 或者 Firefox 上用这段代码来验证：

```
var a = new Object();
console.log ( a.__proto__=== Object.prototype ); // 输出：true
```

实际上， \_\_proto\_\_ 就是对象跟“对象构造器的原型”联系起来的纽带。

> 4、如果对象无法响应某个请求，它会把这个请求委托给它自己的原型。

下面的代码是我们最常用的原型继承方式：

```js
var A = function(){};
A.prototype =  { name: 'sven' };
var a = new A();
console.log( a.name ); // 输出：sven
```

1. 首先，尝试遍历对象 a 中的所有属性，但没有找到 name 这个属性。
2. 查找 name 属性的这个请求被委托给对象 a 的构造器的原型A.prototype， 它被 a. \_\_proto\_\_ 记录着，于是 name 属性会从  a.\_\_proto\_\_ 中查找
3. 最后，在对象 a.\_\_proto\_\_\(A.prototype\) 中找到了 name 属性，并返回它的值。

---

### 使用克隆的原型模式： Object.create

```js
var Plane = function(){
  this.blood = 100;
  this.attackLevel = 1;
  this.defenseLevel = 1;
};
var plane = new Plane();
plane.blood = 500;
plane.attackLevel = 10;
plane.defenseLevel = 7;
var clonePlane = Object.create( plane );
console.log( clonePlane );  // => Object {blood: 500, attackLevel: 10, defenseLevel: 7}
```

不支持Object.create方法的浏览器

```js
Object.create = Object.create || function( obj ){
     var F = function(){};
     F.prototype = obj;
     return new F();
}
```

---

### “类” 继承

```js
var A = function(){};
var B = function(){};
A.prototype = { name: 'sven' };
B.prototype = new A();
var b = new B();
console.log( b.name ); // 输出：sven
```

1. 首先，尝试遍历对象 b 中的所有属性，但没有找到 name 这个属性。
2. 查找 name 属性的请求被委托给对象 b 的构造器的原型，它被 b. \_\_proto\_\_ 记录着并且指向 B.prototype ，而 B.prototype 被设置为一个通过 new A\(\) 创建出来的对象。
3. 在该对象中依然没有找到 name 属性，于是请求被继续委托给这个对象构造器的原型 A.prototype 。
4. 在 A.prototype 中找到了 name 属性，并返回它的值。



