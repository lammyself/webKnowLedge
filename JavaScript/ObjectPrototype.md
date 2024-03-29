# 对象原型

通过原型这种机制,`JavaScript`中的对象从其他对象继承功能特性;

## `__proto__` & `prototype`

对象通过`__proto__`获取到原型对象,在最新版的标准中`__proto__`已被废弃,通过`[["prototype"]]`与`Object.getPrototypeOf()`代替.
`[["prototype"]]`会影响性能,并且还会影响所有继承自这个`[["prototype"]]`的对象,所以推荐使用`Object.getPrototypeOf()`.
现在部分浏览器对`[["prototype"]]`与`Object.getPrototypeOf()`并不支持,所以并不推荐使用.

`__proto__`指向它构造函数的`prototype`,并且每个对象都有一个`__proto__`,每个函数都有一个`prototype`属性.
箭头函数没有`prototype`属性;
`prototype`中的`constructor`指向构造函数本身, 具体关系参考一张经典的图:

![img](./img/prototype.webp)

+ `__proto__(隐式原型)`: 指向对象的原型, 每个对象都会有这个属性.但是这个属性并不是`ECMA`标准,但是现代浏览器都实现了它.
每个对象的原型也都是对象, 原型与原型的原型相连接就形成了`原型链`.
获取一个对象的某个属性时,如果对象没有这个属性会向它的原型(`__proto__`)中去找,然后顺着原型链逐层向上找到`Object.prototype`中, 原型链的终点也就是`Object.prototype.__proto__`是`null`;
+ `prototype(显式原型)`: `JavaScript`中函数`Function`可以拥有属性, 使用`function`关键字声明的函数都有一个`prototype`属性,但是箭头函数没有`prototype`属性;
+ `constructor(构造函数)`: 顾名思义,就是构造当前对象的函数.通过`obj.__proto__.constructor`可以访问到当前对象的构造函数, 函数的`prototype.constructor`指向函数本身;

## new

`new` 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例.

+ new做了什么?

```mermaid
graph 
B(创建一个空对象)-->b(将空对象的原型指向构造函数的prototype) -->  C(将创建的对象和this绑定)-->
A(调用构造函数) --> D(返回对象) --> F(返回这个对象作为结果)
A(调用构造函数) --> E(返回的值不是对象) --> G(返回this对象作为结果)
```

+ 怎样实现一个new

实现一个`new`就是把`new`做的事做一遍,如:

```javaScript
  /**
   * 因为new是一个关键字,而js不能创建关键字.所以,通过函数的方式创建;
   * 并且
   */
  const createObj = function (cur, ...arg) {
    const obj = {}; // 创建一个空对象
    obj.__proto__ = cur.prototype; // 将空对象的原型指向构造函数的prototype
    const result = cur.apply(obj, arg); // 将创建的对象和this绑定,并调用构造函数
    if(typeof result === "object" || typeof result === "function") {// 判断构造函数返回值
      return result; // 如果是对象,就返回这个对象作为结果
    }else {
      return obj; // 如果不是对象, 返回this作为结果.因为this指向了obj,所以返回obj;
    }
  }
```

+ 构造函数

指使用`new`关键字创建对象的函数;
通常构造函数命名使用大驼峰命名,即所有单词首字母大写.
`ES6`中推出了构造函数的语法糖`class`;

+ class
  [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/class)上`class`的定义: `class` 声明创建一个基于原型继承的具有给定名称的新类.
  然而`class`只是创建了一个函数, 比如我们可以使用全等运算符判断一个`class`的原型`__proto__`是否与`Function.prototype`相等.
  用法示例:  

```JavaScript
const People = class Person { // class关键字后面的Person会成为当前类的名字,可以通过在class内部通过People.name或Person.name调用.
  // 当没有将class赋值给一个变量的时候, class会将name的值作为变量名.如果赋值给一个变量后,只能通过这个变量获取当前class;
  constructor(...args) { // 构造函数, 如果没有显式声明js会自动给这个类添加一个空的构造函数
    console.log(People.name);
  }
  test() { // 构造函数外声明的属性和方法会被挂载到原型上
    console.log("pass");
  }
  static ex() { // 静态方法,可以直接通过类来调用.this指向class本身,不会被实例对象继承, 但是可以被子类继承.
    console.log(this);
  }
  static text = "text";
  getName() {
    return Person.name;
  }
  get age() { // 访问age属性时,会获取这个方法的返回值;
    return "18";
  }
  set age(val) { // 修改age属性时可以通过这个方法的第一个参数获取要修改的值;
    console.log(val);
  }
}
console.log(People.text);
class Man extends People { // class 可以使用 extends实现继承
  constructor() {
    this.sex = "男人";
    // 通过super可以访问到父类,包括父类的静态方法;
    console.log(super.age);
    super.ex();
  }
}
```

## 继承

父类构造函数:

```JavaScript
function Parent(name) {
  this.sex = "男";
  this.info = {
      job: "PE",
      age: 35,
      name
  };
};
Parent.prototype.recommend = function() {
  console.log(this.sex, this.info);
};
```

+ 原型链继承:
  + 优点: 父类方法可以共用;
  + 缺点: 父类引用类型的属性会被共享, 子类示例也不能给父类传参;

```JavaScript
function Child (){};
Child.prototype = new Parent();
```

将父类挂载到子类的原型链上

+ 构造函数继承
  + 优点: 可以在子类中向父类构造函数传参, 父类的引用类型不会被子类共享;
  + 缺点: 子类不能访问父类的原型;  

```JavaScript
function Child(name) {
  Parent.call(this, name)
};
```

使用`call`或者`apply`方法更改父类构造函数中`this`指向为子类实例;

+ 组合继承
  + 优点:  
    + 父类的方法啊可以复用;
    + 可以在子类上向父类传参;
    + 构造函数中的引用类型不会被共享;
  + 缺点:  
    + 父类构造函数被调用两次;
    + 父类原型上的引用类型会被共享;

```JavaScript
function Child(name) {
  Parent.call(this, name)
};
Child.prototype = new Parent();
```

将原型链和构造函数继承的优点结合起来;

+ 寄生式继承:  
  + 优点: 不调用父类构造函数,父类原型上的方法可以共享;
  + 缺点: 无法继承构造函数内属性;

```JavaScript
function inheritPrototype(child, parent) {
  let prototype = Object.create(parent.prototype); 
  prototype.constructor = child;
  Child.prototype = prototype;
}
function Child(name) {
  this.info = {
    job: "student",
    age: 18,
    name
  };
};
inheritPrototype(Child, Parent);
```

在将父类的原型赋值给一个空对象的原型,并将这个对象赋值给`Child`的原型;

+ 寄生式组合继承: 终极解决方案;

```JavaScript
function inheritPrototype(child, parent) {
  let prototype = Object.create(parent.prototype); 
  prototype.constructor = child;
  Child.prototype = prototype;
};
function Child(name) {
  Parent.call(this, name);
};
inheritPrototype(Child, Parent);
```

寄生式继承结合构造函数继承, 将两者优点结合;

+ `class`继承语法糖
通过`extends`关键字可以实现`class`的继承;

```JavaScript
class Parent {
  this.sex = "男";
  this.info = {
      job: "PE",
      age: 35,
      name
  };
};
Parent.prototype.recommend = function() {
  console.log(this.sex, this.info);
};
class Child extends Parent {
  this.info = {
    job: "student",
    age: 18,
    name
  };
}
```

`extends`实现继承效果与寄生组合式继承类似;
