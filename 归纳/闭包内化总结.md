## 闭包原理

## 预备知识点

（变量）作用域`、`词法环境/执行环境`、`变量对象/活动对象`、`作用域链

![img](https://user-gold-cdn.xitu.io/2018/7/20/164b3513e183f890?imageslim)

### 变量作用域

### 词法环境

### 变量对象

### 作用域链

### 执行环境、变量对象和作用域链的关系及内部原理



## 闭包



## 闭包的使用场景

## 闭包与static

## 闭包问题处理

## 面试简答题分析

## 面试题编程题分析

### **匿名自执行函数（循环闭包、事件响应函数）**

> 题目：现在有个 HTML 片段，要求编写代码，点击编号为几的链接就`alert`弹出其编号

```html
<ul>
    <li>编号1，点击我请弹出1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>
```

一般不知道这个题目用闭包的话，会写出下面的代码：

```js
var list = document.getElementsByTagName('li');
for (var i = 0; i < list.length; i++) {
    list[i].addEventListener('click', function(){
        alert(i + 1)
    }, true)
}
```

实际上执行才会发现始终弹出的是`6`，这时候就应该通过闭包来解决：


```js
var list = document.getElementsByTagName('li');
for (var i = 0; i < list.length; i++) {
    list[i].addEventListener('click', function(i){
        return function(){
            alert(i + 1)
        }
    }(i), true)
}
```

分析：循环的时候使用**立即执行**的**匿名函数**包装起来，将每次循环的i值传到匿名函数里面，然后再到匿名函数里面去引用。这样当我们每次点击对应的li时都会有与之相对应的打印值。



经典面试题，循环中使用闭包解决 `var` 定义函数的问题

```js
for ( var i=1; i<=5; i++) {
	setTimeout( function timer() {
		console.log( i );
	}, i*1000 );
}
```

首先因为 `setTimeout` 是个异步函数，所有会先把循环全部执行完毕，这时候 `i` 就是 6 了，所以会输出一堆 6。

解决办法两种，第一种使用闭包

```js
for (var i = 1; i <= 5; i++) {
  (function(j) {
    setTimeout(function timer() {
      console.log(j);
    }, j * 1000);
  })(i);
}
```

第二种就是使用 `setTimeout` 的第三个参数

```js
for ( var i=1; i<=5; i++) {
	setTimeout( function timer(j) {
		console.log( j );
	}, i*1000, i);
}
```

第三种就是使用 `let` 定义 `i` 了

```js
for ( let i=1; i<=5; i++) {
	setTimeout( function timer() {
		console.log( i );
	}, i*1000 );
}
```

因为对于 `let` 来说，他会创建一个块级作用域，相当于

```js
{ // 形成块级作用域
  let i = 0
  {
    let ii = i
    setTimeout( function timer() {
        console.log( ii );
    }, i*1000 );
  }
  i++
  {
    let ii = i
  }
  i++
  {
    let ii = i
  }
  ...
}
```



### 闭包-封装变量

### 结果缓存

### 实现类和继承



## 闭包在实践中的应用

**闭包数据传播更灵活**

前端编程中闭包是非常常见的。

```javascript
!function(){
	var localData="localData here"
	document.addEventListener("click",function(){
		console.log(localData)
	})
}()
```

比如在浏览器中经常要处理事件的点击，比如我们有些自己的局部变量或者是嵌套的函数有些外函数的变量。在增加事件绑定的时候，比如增加点击事件的时候可能需要用到外部的局部变量，有了**闭包在数据传递上更为灵活**。

```javascript
!function(){
	var localData="localData here"
	var url="http://www.baidu.com/";
	$.ajax({
		url:url,
		success:function(){
			//do sth...
			console.log(localData)
		}
	})
}()
```

类似有异步的请求，我们可以在JQ中ajax的success的回调函数中用到外层函数的具体的变量，因为有了闭包的存在，那么在整个匿名函数调用结束后，成功的回调函数仍有能力访问到外层引用的具体变量

## 问题分析：思否上看见一个问题