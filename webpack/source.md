# webpack原理

## 编写一个loader

一个loader就是一个函数, webpack调用loader时会重新绑定这个函数的`this`,并将源码字符串作为参数传入loader中;

在loader函数中:

+ `this`提供了webpack的方法;
+ 输出的内容:
  + 返回值: 将返回值作为输出的内容;
  + `this.callback`: 通过这个函数输出内容;
  + `this.async`: 异步的输出内容;
+ 为loader传入参数: 在使用loader时通过`options`属性可以将参数传递给loader函数;

在调用自定义loader时需要指定loader文件的绝对路径, 这个文件需要使用`module.exports`导出loader函数.
也可以通过`resolveLoader.modules`为webpack指定查找loader的目录;

## 编写plugin

webpack中plugin是一个类,调用的时候再进行实例化;
plugin类中需要有一个`apply`方法, 这个方法默认接受一个参数`compiler`;
`compiler`是当前webpack实例, `compiler.hooks`属性保存了webpack实例的生命周期钩子;

编写plugin时可以在这些钩子中通过webpack实例对构建过程进行一些操作:

`compiler.hooks`中提供的生命周期钩子都会提供一个Tapable的方法,每种Tapable方法都会接收一个回调函数.
这个回调函数的第一个参数是一个`compilation`实例, `compilation`实例能够访问所有的模块和它们的依赖.
在构建过程中具体进行的操作通常都是在这个回调函数中进行的.
