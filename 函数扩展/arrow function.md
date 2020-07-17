饥人谷笔记

箭头函数基本用法，直接见阮一峰es6



深入解析

this

**this是call的第一个参数**

function()中被隐藏的第一个参数this，相当于python中的self（function设计中的潜规则）

需要使用call(),使this暴露

因而

初级写法

object.hi(1, 2)

高级写法

object.ji.call(object, 1, 2)



因为this的指向只有在----调用的时候才能确定

对var that = this的深入理解



箭头函数中的this

函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。

方方：箭头函数没有this的概念

两组代码的对比

普通函数



箭头函数





箭头函数不接受call传入的this值（自己打自己的脸）

```javascript
this;
        console.log('this === window');//true

        function f1() {
            console.log(this);
        }

        f1.call('name: kevin')

        let f2 = () => { console.log('this'); }

        f2.call('name:kevin');//Window
var controller = {
        el: "app",
    init: function() {
            $(this.el)on('click', this.onclick)}
}
controller.init();
```

使用箭头函数之后可以不用this(使箭头函数变得简单干净)

手动将形参传入，像python的self一样,可以将自己定义的this或者self当成普通变量使用



JavaScript设计箭头函数的原因

借鉴语言为coffeescript

```javascript
// 箭头函数的易用性展示
        var array = [1, 2, 3, 4];

        for (let i = 0; i < array.length; i++) {
            array[i] = array[i] * array[i];
        }

        array.map(function (number) {
            number = number * number;
        })

        array.map(number => number * number)
        array.map(n => n * n)
```

vue为什么不使用箭头函数

因为vue依赖this的使用