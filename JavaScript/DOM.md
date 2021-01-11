# DOM

```DOM```(```Document Object Model```): 文档对象模型, 保存了文档树节点的信息.浏览器允许```JavaScript```通过```DOM```对象上的api(属性和方法)操作文档树节点、获取文档节点信息,或者注册事件.

[DOM接口](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model)

## 常用```DOM```接口

+ ```window```: 表示一个包含```DOM```文档的窗口, 暴露给```Javascript```代码.

  + ```window```对象中保存了所有的```WebApi```,并且是当前窗口的全局对象.
  
+ ```Document```: 当前窗口的文档, 保存了元素节点树模型、当前文档信息属性、控制文档的方法.

+ ```Node```: 文档树节点模型;

  + ```NodeList```

  + ```ParentNode```

  + ```ChildNode```

+ ```Text```

+ ```Attr```

+ ```Event```

+ ```EventTarget```

+ ```MutationObserver```

+ ```MutationRecord```

+ ```NodeIterator```

+ ```URL```

+ ```Worker```