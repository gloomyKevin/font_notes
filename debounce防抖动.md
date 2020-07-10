## debounce 函数

> from 阮一峰

有时，我们不希望回调函数被频繁调用。比如，用户填入网页输入框的内容，希望通过 Ajax 方法传回服务器，jQuery 的写法如下。

```javascript
$('textarea').on('keydown', ajaxAction);
```

这样写有一个很大的缺点，就是如果用户连续击键，就会连续触发`keydown`事件，造成大量的 Ajax 通信。这是不必要的，而且很可能产生性能问题。正确的做法应该是，设置一个门槛值，表示两次 Ajax 通信的最小间隔时间。如果在间隔时间内，发生新的`keydown`事件，则不触发 Ajax 通信，并且重新开始计时。如果过了指定时间，没有发生新的`keydown`事件，再将数据发送出去。

这种做法叫做 debounce（防抖动）。假定两次 Ajax 通信的间隔不得小于2500毫秒，上面的代码可以改写成下面这样。

```javascript
$('textarea').on('keydown', debounce(ajaxAction, 2500));

function debounce(fn, delay){
  var timer = null; // 声明计时器
  return function() {
    var context = this;
    var args = arguments;
    clearTimeout(timer);
    timer = setTimeout(function () {
      fn.apply(context, args);
    }, delay);
  };
}
```

上面代码中，只要在2500毫秒之内，用户再次击键，就会取消上一次的定时器，然后再新建一个定时器。这样就保证了回调函数之间的调用间隔，至少是2500毫秒。

>from AlloyTeam

第一步，先把闭包的架子搭起来，因为我们已经分析了闭包生成函数内部一定有的两部分内容。

```javascript
function debunce(func, timeout) {
    // 闭包函数执行时依赖的变量，每次执行闭包函数时都能访问和修改
    return function() {
        // 这个函数最终会被赋值给一个变量
    }
}
```

第二步： 把闭包第一次执行的情况写出来

```javascript
function debunce(func, timeout) {
    timeout = timeout || 300;
    return  function(...args)  {
        var _this = this;
        setTimeout(function () {
            func.apply(_this, args);
        }, timeout);
    }
}
```

第三步： 加上一些判断条件。就像我们最开始写计数器的 `index` 一样，不过这一次你不是把变量写在全局下，而是写在闭包生成器的内部。

```javascript
function debunce(func, timeout) {
    timeout = timeout || 300;
    var timer = null;   // 被闭包函数使用
    return  function(...args)  {
        var _this = this;
        clearTimeout(timer);    // 做一些逻辑让每次执行效果可不一致
        timer  = setTimeout(function () {
            func.apply(_this, args);
        }, timeout);
    }
}

// 测试：
function log(...args) {
    console.log('log:', args);
}

var d_log = debunce(log, 1000);
d_log(1);   // 预期：不输出
d_log(2);   // 预期：1s 后输出

setTimeout( function () {
    d_log(3);   // 预期：不输出
    d_log(4);   // 预期：1s 后输出
}, 1500)
```

### 