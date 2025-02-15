近些年，随着域名劫持、信息泄漏等网络安全事件的频繁发生，网站安全也变得越来越重要，也促成了网络传输协议从 HTTP 到 HTTPS 再到 HSTS 的转变。

## 一、HTTP

HTTP（超文本传输协议） 是一种用于分布式、协作式和超媒体信息系统的应用层协议。HTTP 是互联网数据通信的基础。它是由万维网协会（W3C）和互联网工程任务组（IETF）进行协调制定了 HTTP 的标准，最终发布了一系列的 RFC，并且在1999年6月公布的 RFC 2616，定义了 HTTP 协议中现今广泛使用的一个版本——**HTTP 1.1。**

### HTTP 访问过程

HTTP 属于 TCP/IP 模型中的应用层协议，当浏览器与服务器进行互相通信时，需要先建立TCP 连接，之后服务器才会接收浏览器的请求信息，当接收到信息之后，服务器返回相应的信息。最后浏览器接受对服务器的信息应答后，对这些数据进行解释执行。http1.0 请求模式：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-5-34534979.jpg)



HTTP 1.0 时，浏览器每次访问都要单独建立连接，这会造成资源的浪费。后来 HTTP 1.1 可以在一次连接中处理多个请求，并且将多个请求重叠进行。http1.1 请求模式：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-5-49021234.jpg)

### HTTP 协议特点

1. 简单、快速、灵活：当用户想服务器发送请求时，只需传送请求方法和路径即可，HTTP 允许传输任意类型的数据对象。并且HTTP协议简单易用，HTTP 服务器规模小，保证了网络通信的速度；
2. 无连接、无状态：HTTP协议限制每次连接只处理单个请求，当服务器收到用户请求后就会断开连接，保证了传输时间的节省。同时HTTP协议对事务处理没有记忆能力，如果后续的请求需要使用前面的信息就必须重传数据；
3. 管线化和内容编码：随着管线化技术的出现，HTTP 请求比持久性连接速度更快，并且当某些报文的内容过大时，为了减少传输的时间，HTTP 会采取压缩文件的方式；
4. HTTP 支持客户/服务器模式

### 从HTTP到HTTPS

HTTP 协议由于其简单快速、占用资源少，一直被用于网站服务器和浏览器之间进行数据传输。但是在数据传输的过程中也存在很明显的问题，由于 HTTP 是明文协议，不会对数据进行任何方式的加密。当黑客攻击窃取了网站服务器和浏览器之间的传输报文的时，可以直接读取传输的信息，造成网站、用户资料的泄密。因此 HTTP 不适用于敏感信息的传播，这个时候需要引入 HTTPS（超文本传输安全协议）。

## 二、HTTPS

HTTPS（Hypertext Transfer Protocol Secure ）是一种以计算机网络安全通信为目的的传输协议。在 HTTP 下加入了 SSL 层，从而具有了保护交换数据隐私和完整性和提供对网站服务器身份认证的功能，简单来说它就是**安全版的 HTTP** 。HTTP、HTTPS 差异：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-5-52391331.jpg)



### HTTPS 访问过程

HTTPS 在进行数据传输之前会与网站服务器和 Web 浏览器进行一次握手，在握手时确定双方的**加密密码信息**。

具体过程如下：

1. Web 浏览器将支持的加密信息发送给网站服务器；
2. 网站服务器会选择出一套加密算法和哈希算法，将验证身份的信息以证书（证书发布CA机构、证书有效期、公钥、证书所有者、签名等）的形式发送给Web浏览器；
3. 当 Web 浏览器收到证书之后首先需要验证证书的合法性，如果证书受到浏览器信任则在浏览器地址栏会有标志显示，否则就会显示不受信的标识。当证书受信之后，Web 浏览器会随机生成一串密码，并使用证书中的公钥加密。之后就是使用约定好的哈希算法握手消息，并生成随机数对消息进行加密，再将之前生成的信息发送给网站；Chrome 浏览器 HTTPS安全标识：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-5-72315352.jpg)

4. 当网站服务器接收到浏览器发送过来的数据后，会使用网站本身的私钥将信息解密确定密码，然后通过密码解密 Web浏览器发送过来的握手信息，并验证哈希是否与Web浏览器一致。然后服务器会使用密码加密新的握手信息，发送给浏览器；
5. 最后浏览器解密并计算经过哈希算法加密的握手消息，如果与服务发送过来的哈希一致，则此握手过程结束后，服务器与浏览器会使用之前浏览器生成的随机密码和对称加密算法进行加密交换数据。HTTPS 握手过程：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-5-7363680.jpg)

### HTTPS 加密算法

为了保护数据的安全，HTTPS 运用了诸多加密算法：

1. 对称加密：有流式、分组两种，加密和解密都是使用的同一个密钥。例如：DES、AES-GCM、ChaCha20-Poly1305 等。
2. 非对称加密：加密使用的密钥和解密使用的密钥是不相同的，分别称为：公钥、私钥，公钥和算法都是公开的，私钥是保密的。非对称加密算法性能较低，但是安全性超强，由于其加密特性，非对称加密算法能加密的数据长度也是有限的。例如：RSA、DSA、ECDSA、 DH、ECDHE 等。
3. 哈希算法：将任意长度的信息转换为较短的固定长度的值，通常其长度要比信息小得多，且算法不可逆。例如：MD5、SHA-1、SHA-2、SHA-256 等。
4. 数字签名：签名就是在信息的后面再加上一段内容（信息经过 hash 后的值），可以证明信息没有被修改过。hash 值一般都会加密后（也就是签名）再和信息一起发送，以保证这个 hash 值不被修改。

### 从 HTTPS 到 HSTS

但是当网站传输协议从 HTTP 到 HTTPS 之后，数据传输真的安全了吗？

由于用户习惯，通常准备访问某个网站时，在浏览器中只会输入一个域名，而不会在域名前面加上 http:// 或者 https://，而是由浏览器自动填充，当前所有浏览器默认填充的都是http://。一般情况网站管理员会采用了301/302 跳转的方式由 HTTP 跳转到 HTTPS，但是这个过程总使用到 HTTP 因此容易发生劫持，受到第三方的攻击。***这个时候就需要用到 HSTS（HTTP 严格安全传输）**。如下为 HTTP 请求劫持H：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-5-84247742.jpg)

## 三、HSTS

HSTS 是国际互联网工程组织 IETF 正在推行一种新的 Web 安全协议，网站采用 HSTS 后，用户访问时无需手动在地址栏中输入 HTTPS，浏览器会自动采用 HTTPS 访问网站地址，从而保证用户始终访问到网站的加密链接，保护数据传输安全。

### HSTS原理

HSTS 主要是通过服务器发送响应头的方式来控制浏览器操作：

1. 首先在服务器响应头中添加 HSTS 响应头：

   ``` html
   Strict-Transport-Security: max-age=expireTime [; includeSubDomains] [; preload]
   ```

   此响应头只有在 https 访问返回时才生效，其中[ ]中的参数表示可选；

2. 设置 max-age 参数，时间设置不宜过长，建议设置时间为 6 个月；

3. 当用户下次使用 HTTP 访问，客户端就会进行内部跳转，并且能够看到 307 Redirect Internel 的响应码；

4. 网站服务器变成了 HTTPS 访问源服务器。

开启 HSTS 后网站可以有效防范中间人的攻击，同时也会省去网站 301/302 跳转花费的时间，大大提升安全系数和用户体验。

### 开启 HSTS 后网站安全系数检测测评

开启 HSTS 以后，可以到 ssllabs 进行测试，网站的安全等级会进一步提升。

开启前等级为：A

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-5-17015705.jpg)

开启后等级变为：A+

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-5-61519678.jpg)

## 总结

从 HTTP 到 HTTPS 再到 HSTS，网站的安全系数一直在上升，防止 DNS 劫持、数据泄密的力度也再加大。国内公有云服务商比如又拍云提供了完整的 HTTPS和HSTS的解决方案，不仅支持 SSL 证书快速申请，HTTPS 一键部署，还支持一键开启 HSTS，感兴趣的同学可以前往[又拍云官网](https://www.upyun.com/https)了解。



## 参考来源：

- [从HTTP到HTTPS再到HSTS](http://support.upyun.com/hc/kb/article/1079017/?comefrom=http://blogread.cn/news/)



## 延伸阅读

- [HTTPS系列干货（一）：HTTPS 原理详解](http://support.upyun.com/hc/kb/article/1031843/)
- [5分钟让你明白HTTP协议](https://juejin.im/post/5ad4465d6fb9a028da7d0117)
- [HTTP常见面试题](https://segmentfault.com/a/1190000013271378)
- [HTTP和HTTPS详解](https://juejin.im/post/5a030e326fb9a0450a66c8ea)