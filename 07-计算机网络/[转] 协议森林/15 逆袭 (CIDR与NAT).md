IPv4由于最初的设计原因，长度只有32位，所以只提供了大约40亿个地址。这造成了[地址耗尽危机（请戳我）](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247483795&idx=1&sn=17f78c3746016607b35782aaf4ed1611&chksm=972ad0e9a05d59ffa83fc04cd414b188983f4b422ff51c4c074e3ef4711ef7108367d870db7c&mpshare=1&srcid=1007lXMdILEYGgZM5BL4ofTb&scene=21#wechat_redirect)。随后，IPv6被设计出来，并可以提供足够多的IP地址。但是由于IPv4与IPv6并不兼容，IPv4向IPv6迁移并不容易。一些技术，比如说这里要说的CIDR和NAT，相继推广。这些技术可以缓解IPv4的稀缺状态，成就了IPv4一时的逆袭。

### CIDR

CIDR(Classless Inter Domain Routing)改进了传统的IPv4地址分类。传统的IP分类将IP地址直接对应为默认的分类，从而将Internet分割为网络。CIDR在路由表中增加了子网掩码(subnet masking)，从而可以更细分网络。利用CIDR，我们可以灵活的将某个范围的IP地址分配给某个网络。

**IP地址分类**

在[IP接力赛 （请戳我）](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247483787&idx=1&sn=c55dc4db56115623090e2d22bba4d61b&chksm=972ad0f1a05d59e727c0be3907b037b00e88e7180128603aac9ce9d158cdfbe9b906565c1b53&mpshare=1&srcid=10070SeZxnUr5q4ikdezqzkZ&scene=21#wechat_redirect)中，我提到，IP地址可以分为如下几类：

![img](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgIanJxzSOPicN5kZDOUjRBk7lA4czbicVWoeRxQwAvg484tQicoxCAMp4GDFS9kGClPjDhhYnXeewVbQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

这是最初的IPv4地址分类设计。一个IPv4地址总共有32位，可以分为网络(network)和主机(host)两部分。子网掩码(subnet mask)是用于表示哪些位代表了网络部分。比如如下subnet mask 255.0.0.0的二进制表示为：

`11111111 00000000 00000000 00000000`

它的前八位为1，所以表示IP地址的前八位为网络部分。而后面的24位代指该网络的各个主机。一个A类网络可以有224台主机，也就是16777216。由于IPv4地址已经分好了类，所以当我们拿到一个IP地址，我们就可以通过上面查到它的子网掩码。(B类，216; C类，28)

**传统路由表**

IP分类的方便了IP包的接力。IP包到达某个路由器后，会根据该路由器的路由表(routing table)，来决定接力的下一站。一个传统的路由表看起来是这样的：

![img](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgIanJxzSOPicN5kZDOUjRBk74cNAicBW8djjJ3SHMpBicBB854ymsMcYqtCUlCVibCU3fSU5XScmNbAmA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

该路由表代表的网络拓扑如下：

![img](http://mmbiz.qpic.cn/mmbiz_jpg/FWANMMXDrgIanJxzSOPicN5kZDOUjRBk72r3CL1EFJhBfMmsib6pALMf4AM0Jkl6oibXcylwCAISViaH1Ee5BMYhng/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

由于IP分类，我们不需要记录subnet mask。当我们要前往199.165.146.17时，我们已经知道这台主机位于一个C类地址，所以它的子网掩码是255.255.255.0，也就是说199.165.146代表了网络，17代表了主机。

**CIDR路由表**

然而，由于默认分类，造成了网络只能按照A、B、C的方式存在。假设一个网络(比如MIT的网络)分配了一个A类地址，那么该网络将容许16777216个主机。如果该网络无法用完这些IP地址，这些IP地址将无法被其他网络使用。再比如上面的网络，199.165.145必须作为一个整个的网络存在。如果我们只有10台主机，那么将会有200多个IP地址被浪费。CIDR的本质是在路由表中加入子网掩码，并根据该列信息对网络进行分割，而不是根据默认的A，B，C进行分割。比如：

![img](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgIanJxzSOPicN5kZDOUjRBk7ebzyRKKQh4iaewzvj36MfV79oWIBrstt7tiaayJCa7gId8aCQz1VeGvA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

根据路由表的第一条记录：

![img](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgIanJxzSOPicN5kZDOUjRBk7R0YicNIye5galRwLAs0fY1GfSBYDDhSEO52CtkAk0jy03DyicbfOPHmg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

通过子网掩码可以知道，前31位表示网络，最后一位表示主机。子网掩码总是有连续多个1组成，比如上面的31个1。所以也可记为199.165.145.254/31，来同时表示IP地址和子网掩码。

路由器将原来的199.165.145网络中的一部分分割出来。这一网络可以容纳两台电脑，也就是199.165.145.254和199.165.145.255。这个网络对应网卡是eth2。当有IP包通向这两个IP地址时，会前往eth2，而不是eth0。

网络拓扑如下：

![img](http://mmbiz.qpic.cn/mmbiz_jpg/FWANMMXDrgIanJxzSOPicN5kZDOUjRBk7ZAwcIsibFk17ficGKlFHk5ibiaynO9HZgicdYW7MJ0Nr0tX7osLVFll6iaOQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

利用CIDR，我们可以将IP地址根据需要进行分割，从而不浪费IP地址。

### NAT

CIDR虽然可以更加节约IP地址，但它并不能创造新的IP地址。IP地址的耗尽危机并不能因此得到解决。我们来看IPv4的第二袭，NAT(Network Address Translation)。

理论上，每个IP地址代表了Internet上的一个设备。但有一些IP地址被保留，用于一些特殊用途。下面三段IP地址被保留用作私有IP地址：

![img](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgIanJxzSOPicN5kZDOUjRBk7tvB67wtQuO242jYnWAbd4NdEzdbf0RqHvLrN2PFtmeUVHPKFTM3UHw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

私有IP地址只用于局域网内部。理论上，我们不应该在互联网上看到来自或者发往私有IP地址的IP包。与私有IP地址对应的是全球IP地址(global IP address)。

NAT是为私有网络(private network)服务的。该网络中的主机使用私有IP地址。当私有网络内部主机和外部Internet通信时，网关(gateway)路由器负责将私有IP地址转换为全球IP地址，这个地址转换过程就是Network Address Translation。网关路由器的NAT功能。最极端情况下，我们可以只分配一个全球IP地址给网关路由器，而私有网络中的设备都使用私有IP地址。由于私有IP地址可以在不同私有网络中重复使用，所以就大大减小了设备对IP地址的需求。

**基础NAT**

NAT的一种为基础NAT，也成为一对一(one-to-one)NAT。在基础NAT下，网关路由器一一转换一个外部IP地址和一个私有IP地址。网关路由器保存有IP的NAT对应关系，比如：

![img](http://mmbiz.qpic.cn/mmbiz_jpg/FWANMMXDrgIanJxzSOPicN5kZDOUjRBk7JsBzq0Oic6kz7BBPvzibSYdficOewBeLlsviaCEEfQUYicU5walohSmu6sQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

上面网络中，当有IP包要前往199.165.145.1时，网关路由器会将目的地改写为10.0.0.1，并接力给私有网络中的10.0.0.1的电脑。同样，当10.0.0.1的电脑向Internet发送IP包时，它的发送地为10.0.0.1。在到达网关路由器时，会将发送地更改为199.165.145.1。此外，IP头部的checksum，以及更高层协议(比如UDP和TCP)中的校验IP的checksum也会更改。

基础NAT尽管是一对一转换IP地址，它还是可以减小内部网络对IP地址的需求。通常来说，一个局域网中只有少数的设备处于开机状态，并不需要给每个设备对应一个全球IP地址。NAT可以动态的管理全球IP地址，并将全球IP地址对应到开机设备，从而减小内部网络对IP地址的需求。

**NAPT**

NAT还有一种，被成为NAPT (Network Address and Port Translation)。在基础NAT中，高层协议的端口号并不会改动。NAPT下，IP地址和端口号可能同时改动。

我们在UDP和TCP中提到端口(port)的概念。在建立UDP或者TCP通信时，我们实际上是用IP:Port来代表通信的一端(正如打电话时主机:分机号一样)。NAPT就是在网关路由器处建立两个通信通道，一个通往内部网络，一个通往外部网络，然后将网关处的通道端口连接，从而让内部和外部通信。比如：

![img](http://mmbiz.qpic.cn/mmbiz_jpg/FWANMMXDrgIanJxzSOPicN5kZDOUjRBk7fWvLQp6Q1Jia7oiaJhwLJBReRic1XC7THuD2X08o91YmeF4ogPbFHQeBg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

我们看到，通往IP 199.165.145.1建立了三个端口的连接：8888, 8889和8080。它们分别在NAPT处改为通往10.0.0.1:80, 10.0.0.1:8080和10.0.0.3:6000。NAPT记录有外部IP:端口和内部IP:端口的一一对应关系。在IP包经过时，网关路由器会更改IP地址，端口号以及相关的checksum。

利用NAPT我们可以使用一个(或者多个但少量的)外部IP和大量的端口号，来对应多个内部IP以及相应的端口号，从而大大减小了对全球IP地址的需求。

无论是基础NAT还是NAPT，它们的设置都比较复杂，并且从本质上违背了互联网最初的设计理念。但由于IPv4的使用惯性，NAT还是被广泛推广。由于NAT所处的网关服务器是理想的设置防火墙的位置，NAT还往往和防火墙共同建设，以提高私有网络的安全性。

### 总结

即使是CIDR和NAT广泛使用，IPv4还是在不可避免的耗尽。IPv6正在加紧部署。但上述的两种技术，CIDR和NAT在IPv6中同样被采用，所以了解它们依然是有意义的。



### 本文来源：

- 公众号：[逆袭 (CIDR与NAT)](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247484068&idx=1&sn=055e84db202c010b990177cd3fbce6e4&chksm=972ad3dea05d5ac8da3e7fa8df8d724ae61ccf346877ce990cc610fe6257abb5a3ab881a7b13&mpshare=1&scene=21&srcid=1222Q1Javd0ZnF4AgWuLjLSq#wechat_redirect)
- 作者原文：https://read.douban.com/reader/column/1788114/chapter/25058514/

