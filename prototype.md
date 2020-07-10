知识点梳理

#### 实例成员

实例成员就是构造函数内部**通过this添加**的成员 如下列代码中uname age sing 就是实例成员，实例成员**只能通过实例化的对象来访问**

```js
 function Star(uname, age) {
     this.uname = uname;
     this.age = age;
     this.sing = function() {
     console.log('我会唱歌');
    }
}
var ldh = new Star('刘德华', 18);
console.log(ldh.uname);//实例成员只能通过实例化的对象来访问
```



```javascript
function Cat (name, color) {
  this.name = name;
  this.color = color;
  this.meow = function () {
    console.log('喵喵');
  };
}

var cat1 = new Cat('大毛', '白色');
var cat2 = new Cat('二毛', '黑色');

cat1.meow === cat2.meow
// false
```

#### 静态成员

静态成员 在构造函数本身上添加的成员 如下列代码中 sex 就是静态成员,静态成员只能通过构造函数来访问

```js
 function Star(uname, age) {
     this.uname = uname;
     this.age = age;
     this.sing = function() {
     console.log('我会唱歌');
    }
}
Star.sex = '男';
var ldh = new Star('刘德华', 18);
console.log(Star.sex);//静态成员只能通过构造函数来访问
```



```javascript
function Animal(name) {
  this.name = name;
}
Animal.color = 'white';

var cat1 = new Animal('大毛');
var cat2 = new Animal('二毛');

console.log(cat1.color);// 无法访问
console.log(cat2.color);// 无法访问
```

上面代码

### 构造函数的缺点

JavaScript 通过构造函数生成新对象，因此构造函数可以视为对象的模板。实例对象的属性和方法，可以定义在构造函数内部。

```javascript
function Cat (name, color) {
  this.name = name;
  this.color = color;
}

var cat1 = new Cat('大毛', '白色');

cat1.name // '大毛'
cat1.color // '白色'
```

上面代码中，`Cat`函数是一个构造函数，函数内部定义了`name`属性和`color`属性，所有实例对象（上例是`cat1`）都会生成这两个属性，即这两个属性会定义在实例对象上面。

通过构造函数为实例对象定义属性，虽然很方便，但是有一个缺点。同一个构造函数的多个实例之间，无法共享属性，从而造成对系统资源的浪费。

```javascript
function Cat(name, color) {
  this.name = name;
  this.color = color;
  this.meow = function () {
    console.log('喵喵');
  };
}

var cat1 = new Cat('大毛', '白色');
var cat2 = new Cat('二毛', '黑色');

cat1.meow === cat2.meow
// false
```

上面代码中，`cat1`和`cat2`是同一个构造函数的两个实例，它们都具有`meow`方法。由于`meow`方法是生成在每个实例对象上面，所以两个实例就生成了两次。也就是说，每新建一个实例，就会新建一个`meow`方法。这既没有必要，又浪费系统资源，因为所有`meow`方法都是同样的行为，完全应该共享。

![img](https://upload-images.jianshu.io/upload_images/16910815-3fb968046459b6ae.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

这个问题的解决方法，就是 JavaScript 的原型对象（prototype）。

### prototype 属性的作用

JavaScript 规定，每个函数都有一个`prototype`属性，指向一个对象。这个prototype就是一个对象，这个对象的所有属性和方法，都会被构造函数所拥有。

我们可以把那些不变的方法，直接定义在 prototype 对象上，这样所有对象的实例就可以共享这些方法。

改写

```javascript
function Animal(name) {
  this.name = name;
}
Animal.prototype.meow = '喵喵';

var cat1 = new Animal('大毛');
var cat2 = new Animal('二毛');

cat1.meow // 'white',cat1实例调用构造函数原型的方法
cat2.meow // 'white',cat2实例调用构造函数原型的方法
```

构造函数`Animal`的`prototype`属性，就是实例对象`cat1`和`cat2`的原型对象。原型对象上添加一个`color`属性，结果，实例对象都共享了该属性。

同样的，还可以为原型对象添加方法，此方法可以在所有Animal实例对象上调用

```javascript
function Animal(name) {
  this.name = name;
}

//探究：此函数写在function内部和外部的区别
//写在内部，即成为开头所说的实例成员，只能通过this添加，并只能通过实例化对象访问
Animal.prototype.walk = function () {
  console.log(this.name + ' is walking');
};

var cat1 = new Animal('大毛');
var cat2 = new Animal('二毛');

cat1.walk // '大毛 is walking'
cat2.walk // '二毛 is walking'
```

### `prototype`属性特征

原型对象的属性不是实例对象自身的属性。只要修改原型对象，变动就立刻会体现在**所有实例对象**上。

```javascript
Animal.prototype.color = 'yellow';

cat1.color // "yellow"
cat2.color // "yellow"
```

上面代码中，原型对象的`color`属性的值变为`yellow`，两个实例对象的`color`属性立刻跟着变了。这是因为实例对象其实没有`color`属性，都是读取原型对象的`color`属性。也就是说，**当实例对象本身没有某个属性或方法的时候，它会到原型对象去寻找该属性或方法。**这就是原型对象的特殊之处。

如果实例对象自身就有某个属性或方法，它就不会再去原型对象寻找这个属性或方法。

```javascript
cat1.color = 'black';

cat1.color // 'black'
cat2.color // 'yellow'
Animal.prototype.color // 'yellow';
```

上面代码中，实例对象`cat1`的`color`属性改为`black`，就使得它不再去原型对象读取`color`属性，后者的值依然为`yellow`。