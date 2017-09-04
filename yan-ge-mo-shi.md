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



