# DOM

```DOM```(```Document Object Model```): 文档对象模型, 保存了文档树节点的信息.浏览器允许```JavaScript```通过```DOM```对象上的接口(属性和方法)操作文档树节点、获取文档节点信息,或者注册事件.  
属于```BOM```接口的一类;

[DOM接口](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model)

## 常用```DOM```接口

+ ```Document```: 当前窗口的文档, 保存了节点树模型、当前文档信息属性、控制文档的方法.

+ ```Node```: 文档树节点模型, 通常表现为一个```Element```;
  > 每一个```Element```都可以使用```Node```对象与```Element```对象的方法及属性, 因为```Element```继承自```Node```;
  > 每一个```Node```节点可以是```ParentNode```的同时也可以是```ChildNode```, 相对于父级节点就是```ChildNode```, 相对于子级节点就是```ParentNode```;
  + ```NodeList```: 一个节点模型列表, 但是不是数组.而是一个类似数组的对象, 而且可以使用```forEach```迭代;

  + ```ParentNode```: 一个拥有子节点的节点, 拥有一些父节点独有的方法和属性;如:
    + ```ParentNode.childElementCount```: 后代节点的数量;
    + ```ParentNode.children```: 包含所有后代元素节点的```ParentNode.children```;
    + ```ParentNode.append()```: 在最后一个后代后面添加**一组**节点;
    ... ...
  + ```ChildNode```: 一个拥有父级节点的节点, 拥有一些子节点独有的方法和属性;如:
    + ```ChildNode.remove()```: 将当前子节点从父节点中移除;
    ... ...

+ ```Event```: 事件;

  某些行为会触发事件, 事件被触发后可以使用```Event```接口中的方法进行监听.如: ```EventTarget.addEventListener()```... ... ;  
  [Event](./Event.md)

+ ```EventTarget```:是一个 DOM 接口，由可以接收事件、并且可以创建侦听器的对象实现;
  通常是指一个```ElementNode```(元素节点);
  也可使用```EventTarget```构造函数创建;

+ ```MutationObserver```: 接口提供了监视对```DOM```树所做更改的能力。它被设计为旧的```Mutation Events```功能的替代品，该功能是```DOM3 Events```规范的一部分。

  + ```MutationRecord```: 代表一个独立的 ```DOM``` 变化，在每次随 ```DOM``` 变化调用 MutationObserver 的回调函数时，一个相应的 ```MutationRecord``` 会被作为参数，传递给回调函数。

+ ```NodeIterator```: 元素迭代器, 遍历当前```DOM```节点的所有子节点的迭代器;

  通过```document.createNodeIterator()```方法创建;

+ ```URL```: 接口用于解析，构造，规范化和编码 ```URL```;

+ ```Worker```: ```Worker``` 接口是 ```Web Workers API``` 的一部分，指的是一种可由脚本创建的后台任务，任务执行中可以向其创建者收发信息。
+ ```ResizeObserver```: 可以监听到 ```Element``` 的内容区域或 ```SVGElement```的边界框改变, 内容区域则需要减去内边距padding。**实验中的功能**
+ ```Touch```: 获取在触控设备上操作的触摸点;

## ```Node```节点类型

### 元素节点```Element```

+ #### HTML元素节点```HTMLElement```

  所有的```HTML```元素节点;

+ #### SVG元素节点```SVGElement```

  所有的```SVG```元素;

### 属性节点```Attr```

元素的属性,可以通过```Element.getAttributeNode()```或者元素迭代器```NodeIterator```获取属性节点;

### 文本节点```Text```

所有元素或属性的文本内容,可以通过```Text()```构造函数创建一个```Text```对象;
