# 生命周期

class组件具有一些钩子函数,分别在组件运行的不同时期调用;

## `constructor`

开始创建组件时调用, 用于初始化内部状态.并且是唯一可以直接修改`state`的地方;

## `getDerivedStateFromProps`

16.3版本新特性. 获取到外部传入属性, 可以用来初始化一些状态;
也会在状态或属性发生改变时`shouldComponentUpdate`之前调用,或者主动调用`forceUpdate()`方法也会触发`getDerivedStateFromProps`;
每次`render`前都会调用, 通常用于从`props`初始化`state`, 比如: 初始化表单的默认值;

## `render`

渲染阶段,开始渲染`DOM`节点.
  也会在状态或属性更新时会在`shouldComponentUpdate`之后调用, 或者调用`forceUpdate()`触发`getDerivedStateFromProps`完成后调用;
  在更新阶段时触发还会计算一些视图的差异从而实现局部更新;

## `componentDidMount`

组件初始渲染完成后调用, 只执行一次,

## `shouldComponentUpdate`

组件应该更新的阶段, 在组件初始化完成后状态或属性发生改变时调用;
如果在`shouldComponentUpdate`中返回`false`, 组件将不会重新渲染.
可以让组件继承父类由`React.Component`改为`React.PureComponent`来实现参数的自动对比,用来决定是否渲染视图.但是`React.PureComponent`只能进行浅层对比, 数据类型比较复杂的时候不推荐使用;

## `getSnapshotBeforeUpdate`

16.3版本新特性, 在组件更新状态或属性时视图重新渲染之前调用.
通常用来在`DOM`更新之前获取一些信息, 而且返回值会被当做参数传递给`componentDidUpdate`;

## `componentDidUpdate`

状态或属性更新时视图渲染完成后调用;

## `componentWillUnmount`

组件卸载前调用, 通常用来释放一些资源,比如: 删除定时器... ...;

## 问题记录

+ 请求等行为放在`render`前面还是后面?
