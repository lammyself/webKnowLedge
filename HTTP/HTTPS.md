# HTTPS

因为HTTP是明文传输的,所以在传输过程中可能会被窃取或篡改.这种攻击方式被称为`中间人攻击`;
为了信息的安全, 引入了`HTTPS`加密数据;
`HTTPS`相对于`HTTP`增加了一个安全层, 数据在发送接受的过程中都会经过安全层.

![img](./img/HTTP.png)

> 从图中我们可以看出 HTTPS 并非是一个新的协议，通常 HTTP 直接和 TCP 通信，HTTPS 则先和安全层通信，然后安全层再和 TCP 层通信。也就是说 HTTPS 所有的安全核心都在安全层，它不会影响到上面的 HTTP 协议，也不会影响到下面的 TCP/IP.
安全层有两个主要的职责：对发起 HTTP 请求的数据进行加密操作和对接收到 HTTP 的内容进行解密操作。

## 加密方法

+ 使用对称加密传输数据: 因为加密套件和秘钥也经过`HTTP`传输.所以单独使用对称加密也是不安全的;

![img](./img/HTTP_SymmetricEncryption.png)

+ 使用非对称加密传输数据: 解决了黑客获取到客户端发送数据的安全问题, 但是无法保证服务端数据的安全, 并且非对称加密效率较低影响了加密解密速度.

  > 第一个是非对称加密的效率太低。这会严重影响到加解密数据的速度，进而影响到用户打开页面的速度。
  第二个是无法保证服务器发送给浏览器的数据安全。虽然浏览器端可以使用公钥来加密，但是服务器端只能采用私钥来加密，私钥加密只有公钥能解密，但黑客也是可以获取得到公钥的，这样就不能保证服务器端数据的安全了。

  ![img](./img/HTTP_AsymmetricEncryption.png)

+ 使用对称加密与非对称加密结合: 在传输数据阶段依然使用对称加密，但是对称加密的密钥我们采用非对称加密来传输。

  >首先浏览器向服务器发送对称加密套件列表、非对称加密套件列表和随机数 client-random；
  服务器保存随机数 client-random，选择对称加密和非对称加密的套件，然后生成随机数 service-random，向浏览器发送选择的加密套件、service-random 和公钥；
  浏览器保存公钥，并生成随机数 pre-master，然后利用公钥对 pre-master 加密，并向服务器发送加密后的数据；
  最后服务器拿出自己的私钥，解密出 pre-master 数据，并返回确认消息。
  \
  pre-master 是经过公钥加密之后传输的，所以黑客无法获取到 pre-master，这样黑客就无法生成密钥，也就保证了黑客无法破解传输过程中的数据了。
  \
  到此为止，服务器和浏览器就有了共同的 client-random、service-random 和 pre-master，然后服务器和浏览器会使用这三组随机数生成对称密钥，因为服务器和浏览器使用同一套方法来生成密钥，所以最终生成的密钥也是相同的

  ![img](./img/HTTP_both.png)

  但是这种方法无法防范`DNS`劫持, 如果域名被劫持后访问的其实是黑客的服务器了, 黑客就可以在自己的服务器上实现公钥和私钥.

+ 使用对称加密与非对称加密结合并添加数字证书: 通过数字证书验证服务器的真实性;

  > 有了 CA 签名过的数字证书，当浏览器向极客时间服务器发出请求时，服务器会返回数字证书给浏览器。
  浏览器接收到数字证书之后，会对数字证书进行验证。
  首先浏览器读取证书中相关的明文信息，采用 CA 签名时相同的 Hash 函数来计算并得到信息摘要 A；
  然后再利用对应 CA 的公钥解密签名数据，得到信息摘要 B；
  对比信息摘要 A 和信息摘要 B，如果一致，则可以确认证书是合法的，即证明了这个服务器是合法服务器的；
  同时浏览器还会验证证书相关的域名信息、有效时间等信息。这时候相当于验证了 CA 是谁，但是这个 CA 可能比较小众，浏览器不知道该不该信任它，然后浏览器会继续查找给这个 CA 颁发证书的 CA，再以同样的方式验证它上级 CA 的可靠性。
  通常情况下，操作系统中会内置信任的顶级 CA 的证书信息（包含公钥），如果这个 CA 链中没有找到浏览器内置的顶级的 CA，证书也会被判定非法。
  