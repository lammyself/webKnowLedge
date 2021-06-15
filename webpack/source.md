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
