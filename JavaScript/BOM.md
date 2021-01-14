# BOM

```BOM```(```Browser Object Model```), 浏览器对象模型, 提供了与浏览器窗口交互的接口;  
属于```WebApi```一部分;

## ```window```

  表示一个包含文档的窗口, 暴露给```Javascript```代码.  
  并保存了所有的```WebApi```,并且是当前窗口的全局对象.

## 常用```BOM```接口

+ [DOM](./DOM);
+ ```Console```: 控制台
+ ```History```: 历史记录;
+ ```Location```
+ ```WebGL```
+ ```OffscreenCanvas```: 提供了一个可以脱离屏幕渲染的```canvas```对象,它在窗口环境和```web worker```环境均有效.**实验中的功能**
+ ```OfflineAudioContext```: 不在硬件设备渲染音频;相反,它尽可能快地生成音频,输出一个 ```AudioBuffer``` 作为结果.
+ ```WaveShaperNode```: 表示一个非线性的畸变器, 是一个使用曲线来将一个波形畸变应用到一个声音信号中的```AudioNode```. 除了明显的失真效果之外, 它通常用来给信号添加一个暖调的感觉.
+ ```Performance```: 可以获取到一些和性能有关的信息;
+ ```Screen```: 表示一个屏幕窗口, 往往指的是当前正在被渲染的```window```对象, 可以使用 ```window.screen``` 获取它.
+ ```ScrollToOptions```: 元素或```window```滚动对象,可以获取当前滚动位置或者设置滚动到某个位置;
+ 数据缓存```Storage```
  + ```sessionStorage```
  + ```localStorage```
  + ```History.pushState()```
  + ```IndexDB```: 用于存储大量的结构化数据, 存储大量数据的方法还有```webSQL```;
  + ```cache```: 缓存请求及响应数据;
+ ```CSS```
+ ```Blob```
+ ```File```
+ ```Navigator```: 保存了一些浏览器的信息;
+ ```PublicKeyCredential```: 网络加密, 仅支持```HTTPS```协议;
+ 硬件:
  + 蓝牙
  + 各种传感器
  + USB: **实验中的功能**
