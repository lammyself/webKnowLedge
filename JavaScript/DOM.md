# DOM

```DOM```(```Document Object Model```): 文档对象模型, 保存了文档树节点的信息.浏览器允许```JavaScript```通过```DOM```对象上的api(属性和方法)操作文档树节点、获取文档节点信息,或者注册事件.

[DOM接口](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model)

## 常用```DOM```接口

+ ```window```: 表示一个包含```DOM```文档的窗口, 暴露给```Javascript```代码.

  + ```window```对象中保存了所有的```WebApi```,并且是当前窗口的全局对象.
  
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

+ ```EventTarget```

+ ```MutationObserver```

+ ```MutationRecord```

+ ```NodeIterator```

+ ```URL```

+ ```Worker```

## ```Node```节点类型

### 元素节点```Element```

#### ```HTMLElement```

#### ```SVGElement```

### 文本节点```Text```
