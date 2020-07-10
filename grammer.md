### 变量提升

JavaScript 引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）。

```javascript
console.log(a);
var a = 1;
```

上面代码首先使用`console.log`方法，在控制台（console）显示变量`a`的值。这时变量`a`还没有声明和赋值，所以这是一种错误的做法，但是实际上不会报错。因为存在变量提升，真正运行的是下面的代码。

```javascript
var a;
console.log(a);
a = 1;
```

最后的结果是显示`undefined`，表示变量`a`已声明，但还未赋值。

中文是合法的标识符，可以用作变量名。

```javascript
var 临时变量 = 1;
```

## 区块

JavaScript 使用大括号，将多个相关的语句组合在一起，称为“区块”（block）。

对于`var`命令来说，JavaScript 的区块不构成单独的作用域（scope）。

```javascript
{
  var a = 1;
}

a // 1
```

上面代码在区块内部，使用`var`命令声明并赋值了变量`a`，然后在区块外部，变量`a`依然有效，区块对于`var`命令不构成单独的作用域，与不使用区块的情况没有任何区别。在 JavaScript 语言中，单独使用区块并不常见，区块往往用来构成其他更复杂的语法结构，比如`for`、`if`、`while`、`function`等。

赋值表达式（`=`）、严格相等运算符（`===`）和相等运算符（`==`）

将常量写在运算符的左边，这样的话，一旦不小心将相等运算符写成赋值运算符，就会报错，因为常量不能被赋值。

```javascript
if (x = 2) { // 不报错
if (2 = x) { // 报错
```

至于为什么优先采用“严格相等运算符”（`===`），而不是“相等运算符”（`==`），请参考《运算符》章节

### if...else 结构

`else`代码块总是与离自己最近的那个`if`语句配对。

```javascript
var m = 1;
var n = 2;

if (m !== 1)
if (n === 2) console.log('hello');
else console.log('world');
```

上面代码不会有任何输出，`else`代码块不会得到执行，因为它跟着的是最近的那个`if`语句，相当于下面这样。

```javascript
if (m !== 1) {
  if (n === 2) {
    console.log('hello');
  } else {
    console.log('world');
  }
}
```

如果想让`else`代码块跟随最上面的那个`if`语句，就要改变大括号的位置。

```javascript
if (m !== 1) {
  if (n === 2) {
    console.log('hello');
  }
} else {
  console.log('world');
}
// world
```

### switch 结构

switch`语句部分和`case`语句部分，都可以使用表达式。

```javascript
switch (1 + 3) {
  case 2 + 2:
    f();
    break;
  default:
    neverHappens();
}
```

上面代码的`default`部分，是永远不会执行到的。

需要注意的是，`switch`语句后面的表达式，与`case`语句后面的表示式比较运行结果时，采用的是严格相等运算符（`===`），而不是相等运算符（`==`），这意味着比较时不会发生类型转换。

```javascript
var x = 1;

switch (x) {
  case true:
    console.log('x 发生类型转换');
    break;
  default:
    console.log('x 没有发生类型转换');
}
// x 没有发生类型转换
```

上面代码中，由于变量`x`没有发生类型转换，所以不会执行`case true`的情况。这表明，`switch`语句内部采用的是“严格相等运算符”，详细解释请参考《运算符》一节。

所有`for`循环，都可以改写成`while`循环。

`break`语句用于跳出代码块或循环。

`continue`语句用于立即终止本轮循环，返回循环结构的头部，开始下一轮循环。

### 标签（label）

JavaScript 语言允许，语句的前面有标签（label），相当于定位符，用于跳转到程序的任意位置，标签的格式如下。

```javascript
label:
  语句
```

标签可以是任意的标识符，但不能是保留字，语句部分可以是任意语句。

标签通常与`break`语句和`continue`语句配合使用，跳出特定的循环。

```javascript
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) break top;
      console.log('i=' + i + ', j=' + j);
    }
  }
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
```

上面代码为一个双重循环区块，`break`命令后面加上了`top`标签（注意，`top`不用加引号），满足条件时，直接跳出双层循环。如果`break`语句后面不使用标签，则只能跳出内层循环，进入下一次的外层循环。

标签也可以用于跳出代码块。

```javascript
foo: {
  console.log(1);
  break foo;
  console.log('本行不会输出');
}
console.log(2);
// 1
// 2
```

上面代码执行到`break foo`，就会跳出区块。

`continue`语句也可以与标签配合使用。

```javascript
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) continue top;
      console.log('i=' + i + ', j=' + j);
    }
  }
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
// i=2, j=0
// i=2, j=1
// i=2, j=2
```

上面代码中，`continue`命令后面有一个标签名，满足条件时，会跳过当前循环，直接进入下一轮外层循环。如果`continue`语句后面不使用标签，则只能进入下一轮的内层循环。

## 