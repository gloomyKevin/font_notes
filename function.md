### 函数名的提升

JavaScript 引擎将函数名视同变量名，所以采用`function`命令声明函数时，整个函数会像变量声明一样，被提升到代码头部。所以，下面的代码不会报错。

```javascript
f();

function f() {}
```

表面上，上面代码好像在声明之前就调用了函数`f`。但是实际上，由于“变量提升”，函数`f`被提升到了代码头部，也就是在调用之前已经声明了。但是，如果采用赋值语句定义函数，JavaScript 就会报错。

```javascript
f();
var f = function (){};
// TypeError: undefined is not a function
```

上面的代码等同于下面的形式。

```javascript
var f;
f();
f = function () {};
```

上面代码第二行，调用`f`的时候，`f`只是被声明了，还没有被赋值，等于`undefined`，所以会报错。

注意，如果像下面例子那样，采用`function`命令和`var`赋值语句声明同一个函数，由于存在函数提升，最后会采用`var`赋值语句的定义。

```javascript
var f = function () {
  console.log('1');
}

function f() {
  console.log('2');
}

f() // 1
```

上面例子中，表面上后面声明的函数`f`，应该覆盖前面的`var`赋值语句，但是由于存在函数提升，实际上正好反过来。