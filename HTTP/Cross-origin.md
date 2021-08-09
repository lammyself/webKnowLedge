# 跨域

跨域是指一个域下的文档`(HTML)`、脚本`(js)`去请求另一个域下的资源, 受浏览器**同源策略**影响部分跨域行为受到限制.

## 跨域类型

+ 前端跨域: 页面在浏览器上不能对跨域的其他页面进行操作.
  + `Cookie`、`LocalStorage` 和 `IndexDB` 无法读取。
  + 无法获取`DOM`.
+ 请求跨域: 部分请求发送后浏览器不会接收响应.

## 解决跨域的方法

1. **CORS**: 跨域资源共享(**CORS**) 是一种机制，它使用额外的 `HTTP` 头来告诉浏览器允许接收的响应类型:
    1. **Access-Control-Allow-Origin**: 允许接收的域名;
    2. **Access-Control-Allow-Credentials**: 允许请求携带`Cookie`;
    3. **Access-Control-Request-Method**: 允许的请求方法;
    4. **Access-Control-Allow-Headers**: 允许的自定义请求头;
使用**CORS**来解决跨域问题还需要注意请求类型:

    + **简单请求**: 不会触发 `CORS` 预检请求:请求前不会发送`(OPTINS)`。
    + **复杂请求**: 会触发 `CORS` 预检请求: 请求前会发送`(OPTINS)`.

      简单请求需要满足以下条件, 如果不满足就是复杂请求:

    + 请求方法必须是 `GET`, `HEAD`, `POST`;
    + 主动设置`Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, `DPR`, `Downlink`, `Save-Data`, `Viewport-Width`, `Width`**之外**的请求头.
    + **Content-Type**只能是`text/plain`, `multipart/form-data`, `application/x-www-form-urlencoded`**中**的一种.
    + 请求中的任意 `XMLHttpRequestUpload` 对象均没有注册任何事件监听器；`XMLHttpRequestUpload` 对象可以使用 `XMLHttpRequest.upload` 属性访问。
    + 请求中没有使用 `ReadableStream` 对象。

2. **正向代理**: 一个位于客户端和原始服务器之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。
利用服务端请求不受**同源策略**限制的特性,就可以实现跨域访问了.
前端常用在开发模式下利用`webpack`服务配置正向代理.
3. **反向代理**: 实际运行方式是指以代理服务器来接受请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给客户端，此时代理服务器对外就表现为一个服务器。
可以通过`Nginx`配置反向代理服务器, 将资源`(或接口)`和页面代理到同一域名的不同路径下.
4. **JSONP**: 主要就是利用了 `script` 标签没有跨域限制的这个特性来完成的。
5. **Websocket**: 客户端和服务器之间可以建立持久的连接，而且双方都可以随时开始发送数据.
`Websocket`本质上没有使用 `HTTP` 的响应头, 因此也没有跨域的限制;
6. **window.postMessage**: 浏览器提供的跨域通讯的方法, 允许两个正在运行的页面相互发送消息.
