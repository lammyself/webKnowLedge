# 事件```Event```

某些行为会触发事件, 事件被触发后可以使用```Event```接口中的方法进行监听.如: ```EventTarget.addEventListener()```... ... ;

开发者也可以自定义一些事件:  

```JavaScript
/**
 * typeArg: 事件名, DOMString(普通字符串就行).
 * eventOptions: {
    bubbles: 可选，Boolean类型，默认值为 false，表示该事件是否冒泡;
    cancelable: 可选，Boolean类型，默认值为 false， 表示该事件能否被取消;
    composed: 可选，Boolean类型，默认值为 false，指示事件是否会在影子DOM根节点之外触发侦听器。
    }
  */
const typeArg = "test";
const eventOptions = {
  bubbles: false,
  cancelable: false,
  composed: false
}
const myEvent = new Event(typeArg, eventOptions); // 定义事件
EventTarget.addEventListener(typeArg, e => {
  console.log(e);
}); // 监听事件
EventTarget.dispatchEvent(myEvent); // 触发事件
```

事件还可以通过一些方法主动触发, 例如:

```JavaScript
  const testDom = document.getElementById("test");
  testDom.addEventListener("click", (e) => {
    console.log("e");
    console.log(e);
  })
  testDom.click();
```

可以通过```EventTarget.removeEventListener();```移除事件监听;
