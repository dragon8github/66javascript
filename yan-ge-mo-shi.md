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

