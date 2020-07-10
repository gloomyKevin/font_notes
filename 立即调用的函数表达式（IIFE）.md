### 立即调用的函数表达式（IIFE）

在 JavaScript 中，圆括号`()`是一种运算符，跟在函数名之后，表示调用该函数。比如，`print()`就表示调用`print`函数。

有时，我们需要在定义函数之后，立即调用该函数。这时，你不能在函数的定义之后加上圆括号，这会产生语法错误。

```javascript
function(){ /* code */ }();
// SyntaxError: Unexpected token (
```

产生这个错误的原因是，`function`这个关键字即可以当作语句，也可以当作表达式。

```javascript
// 语句
function f() {}

// 表达式
var f = function f() {}
```

为了避免解析上的歧义，JavaScript 引擎规定，如果`function`关键字出现在行首，一律解释成语句。因此，JavaScript 引擎看到行首是`function`关键字之后，认为这一段都是函数的定义，不应该以圆括号结尾，所以就报错了。

解决方法就是不要让`function`出现在行首，让引擎将其理解成一个表达式。最简单的处理，就是将其放在一个圆括号里面。

```javascript
(function(){ /* code */ }());
// 或者
(function(){ /* code */ })();
```

上面两种写法都是以圆括号开头，引擎就会认为后面跟的是一个表示式，而不是函数定义语句，所以就避免了错误。这就叫做“立即调用的函数表达式”（Immediately-Invoked Function Expression），简称 IIFE。

注意，上面两种写法最后的分号都是必须的。如果省略分号，遇到连着两个 IIFE，可能就会报错。

```javascript
// 报错
(function(){ /* code */ }())
(function(){ /* code */ }())
```

上面代码的两行之间没有分号，JavaScript 会将它们连在一起解释，将第二行解释为第一行的参数。

推而广之，任何让解释器以表达式来处理函数定义的方法，都能产生同样的效果，比如下面三种写法。

```javascript
var i = function(){ return 10; }();
true && function(){ /* code */ }();
0, function(){ /* code */ }();
```

甚至像下面这样写，也是可以的。

```javascript
!function () { /* code */ }();
~function () { /* code */ }();
-function () { /* code */ }();
+function () { /* code */ }();
```

通常情况下，只对匿名函数使用这种“立即执行的函数表达式”。它的目的有两个：一是不必为函数命名，避免了污染全局变量；二是 IIFE 内部形成了一个单独的作用域，可以封装一些外部无法读取的私有变量。

```javascript
// 写法一
var tmp = newData;
processData(tmp);
storeData(tmp);

// 写法二
(function () {
  var tmp = newData;
  processData(tmp);
  storeData(tmp);
}());
```

上面代码中，写法二比写法一更好，因为完全避免了污染全局变量。