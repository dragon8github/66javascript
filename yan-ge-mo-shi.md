ES5 引入另一种版本控制的考量 —— 严格模式

```js
"use strict";
```

当开启此模式时，会禁止使用一些JavaScript中问题较多或易于出错的特性。譬如:

```js
function f(x) {
    "use strict";
    var arguments = []; // Uncaught SyntaxError: Unexpected eval or arguments in strict mode
}
```

"use strict" 指令只有在脚本顶部或者函数顶部才能生效。这也导致一个敏感的问题。对于一些大型的应用开发，为了优化加载速度，减少http请求，通常会将js文件合并到同一个js文件中。如：

```js
"use strict";

// index.js
function f() {
}

// index2.js
function g () {
    var arguments = [] // Uncaught SyntaxError: Unexpected eval or arguments in strict mode
}
```

**（ps: 如果放置在不同的文件中加载，反而没有事。比如，先加载的具有严格模式的index.js文件，然后再加载index2.js文件，此时的index2.js是不会因为重复定义arguments变量而报错的）**

在真实的项目实战中，你可以坚持只使用"严格模式’或只使用“非严格模式”的代码风格。但通常这身不由己，毕竟在实战中你需要加载各式各样的第三方库，你无法保证这些库的代码风格也保持“严格模式”。为了你使你的代码更加健壮，满足各种代码的链接，你有两个可选方案：

**第一个解决方案是不要将进行严格模式的js文件和不进行严格模式的js文件合并。**这可能是最简单直接的解决方案，但它无疑会限制你对应用程序或库的文件结构的控制力。在最理想的情况下，你至少要部署两个独立的文件。一个是包含所有严格模式检查的文件，另一个则包含无须严格模式检查的文件。

**第二个解决方案是将自己开发的js代码，用IIFE（立即执行的函数表达式Immediately Invoked Function Expression）包裹起来：**

```js
// index.js
(function () {
    "use strict";
    function f() {
    }
})();

// index2.js
(function () {
    function g () {
        var arguments = []
    }
})();
```

由于每个文件的内容被放置在一个单独的作用域中，所以严格模式的检查只影响本文件的内容。

