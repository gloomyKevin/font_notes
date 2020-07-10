## eval 命令

### 基本用法

`eval`命令接受一个字符串作为参数，并将这个字符串当作语句执行。

```javascript
eval('var a = 1;');
a // 1
```

上面代码将字符串当作语句运行，生成了变量`a`。

如果参数字符串无法当作语句运行，那么就会报错。

```javascript
eval('3x') // Uncaught SyntaxError: Invalid or unexpected token
```

放在`eval`中的字符串，应该有独自存在的意义，不能用来与`eval`以外的命令配合使用。举例来说，下面的代码将会报错。

```javascript
eval('return;'); // Uncaught SyntaxError: Illegal return statement
```

上面代码会报错，因为`return`不能单独使用，必须在函数中使用。

如果`eval`的参数不是字符串，那么会原样返回。

```javascript
eval(123) // 123
```

`eval`没有自己的作用域，都在当前作用域内执行，因此可能会修改当前作用域的变量的值，造成安全问题。

```javascript
var a = 1;
eval('a = 2');

a // 2
```

上面代码中，`eval`命令修改了外部变量`a`的值。由于这个原因，`eval`有安全风险。

为了防止这种风险，JavaScript 规定，如果使用严格模式，`eval`内部声明的变量，不会影响到外部作用域。

```javascript
(function f() {
  'use strict';
  eval('var foo = 123');
  console.log(foo);  // ReferenceError: foo is not defined
})()
```

上面代码中，函数`f`内部是严格模式，这时`eval`内部声明的`foo`变量，就不会影响到外部。

不过，即使在严格模式下，`eval`依然可以读写当前作用域的变量。

```javascript
(function f() {
  'use strict';
  var foo = 1;
  eval('foo = 2');
  console.log(foo);  // 2
})()
```

上面代码中，严格模式下，`eval`内部还是改写了外部变量，可见安全风险依然存在。

总之，`eval`的本质是在当前作用域之中，注入代码。由于安全风险和不利于 JavaScript 引擎优化执行速度，所以一般不推荐使用。通常情况下，`eval`最常见的场合是解析 JSON 数据的字符串，不过正确的做法应该是使用原生的`JSON.parse`方法。

### eval 的别名调用

前面说过`eval`不利于引擎优化执行速度。更麻烦的是，还有下面这种情况，引擎在静态代码分析的阶段，根本无法分辨执行的是`eval`。

```javascript
var m = eval;
m('var x = 1');
x // 1
```

上面代码中，变量`m`是`eval`的别名。静态代码分析阶段，引擎分辨不出`m('var x = 1')`执行的是`eval`命令。

为了保证`eval`的别名不影响代码优化，JavaScript 的标准规定，凡是使用别名执行`eval`，`eval`内部一律是全局作用域。

```javascript
var a = 1;

function f() {
  var a = 2;
  var e = eval;
  e('console.log(a)');
}

f() // 1
```

上面代码中，`eval`是别名调用，所以即使它是在函数中，它的作用域还是全局作用域，因此输出的`a`为全局变量。这样的话，引擎就能确认`e()`不会对当前的函数作用域产生影响，优化的时候就可以把这一行排除掉。

`eval`的别名调用的形式五花八门，只要不是直接调用，都属于别名调用，因为引擎只能分辨`eval()`这一种形式是直接调用。

```javascript
eval.call(null, '...')
window.eval('...')
(1, eval)('...')
(eval, eval)('...')
```

上面这些形式都是`eval`的别名调用，作用域都是全局作用域。