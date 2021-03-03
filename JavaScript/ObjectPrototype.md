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
获取一个对象的某个属性时,如果对象没有这个属性会向它的原型(`__proto__`)中去找,然后逐层向上找到`Object`中;
每个对象的原型也都是对象, 原型与原型的原型相连接就形成了`原型链`.
+ `prototype(显式原型)`: `JavaScript`中函数`Function`可以拥有属性, 使用`function`关键字声明的函数都有一个`prototype`属性,但是箭头函数没有`prototype`属性;
+ `constructor(构造函数)`: 顾名思义,就是构造当前对象的函数.通过`obj.__proto__.constructor`可以访问到当前对象的构造函数, 函数的`prototype.constructor`指向函数本身;

## new

## 继承
