上面代码中，函数`f()`有两个参数，且参数名都是`a`。取值的时候，以后面的`a`为准，即使后面的`a`没有值或被省略，也是以其为准。

```javascript
function f(a, a) {
  console.log(a);
}

f(1) // undefined
```

调用函数`f()`的时候，没有提供第二个参数，`a`的取值就变成了`undefined`。这时，如果要获得第一个`a`的值，可以使用`arguments`对象。

```javascript
function f(a, a) {
  console.log(arguments[0]);
}

f(1) // 1
```

### arguments 对象

**（1）定义**

由于 JavaScript 允许函数有不定数目的参数，所以需要一种机制，可以在函数体内部读取所有参数。这就是`arguments`对象的由来。

`arguments`对象包含了函数运行时的所有参数，`arguments[0]`就是第一个参数，`arguments[1]`就是第二个参数，以此类推。这个对象只有在函数体内部，才可以使用。

```javascript
var f = function (one) {
  console.log(arguments[0]);
  console.log(arguments[1]);
  console.log(arguments[2]);
}

f(1, 2, 3)
// 1
// 2
// 3
```

正常模式下，`arguments`对象可以在运行时修改。

```javascript
var f = function(a, b) {
  arguments[0] = 3;
  arguments[1] = 2;
  return a + b;
}

f(1, 1) // 5
```

上面代码中，函数`f()`调用时传入的参数，在函数内部被修改成`3`和`2`。

严格模式下，`arguments`对象与函数参数不具有联动关系。也就是说，修改`arguments`对象不会影响到实际的函数参数。

```javascript
var f = function(a, b) {
  'use strict'; // 开启严格模式
  arguments[0] = 3;
  arguments[1] = 2;
  return a + b;
}

f(1, 1) // 2
```

上面代码中，函数体内是严格模式，这时修改`arguments`对象，不会影响到真实参数`a`和`b`。

通过`arguments`对象的`length`属性，可以判断函数调用时到底带几个参数。

```javascript
function f() {
  return arguments.length;
}

f(1, 2, 3) // 3
f(1) // 1
f() // 0
```

**（2）与数组的关系**

需要注意的是，虽然`arguments`很像数组，但它是一个对象。数组专有的方法（比如`slice`和`forEach`），不能在`arguments`对象上直接使用。

如果要让`arguments`对象使用数组方法，真正的解决方法是将`arguments`转为真正的数组。下面是两种常用的转换方法：`slice`方法和逐一填入新数组。

```javascript
var args = Array.prototype.slice.call(arguments);

// 或者
var args = [];
for (var i = 0; i < arguments.length; i++) {
  args.push(arguments[i]);
}
```

**（3）callee 属性**

`arguments`对象带有一个`callee`属性，返回它所对应的原函数。

```javascript
var f = function () {
  console.log(arguments.callee === f);
}

f() // true
```

可以通过`arguments.callee`，达到调用函数自身的目的。这个属性在严格模式里面是禁用的，因此不建议使用。