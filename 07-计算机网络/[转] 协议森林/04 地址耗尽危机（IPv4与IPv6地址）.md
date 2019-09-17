IP 地址是 IP 协议的重要组成部分，它可以识别接入互联网中的任意一台设备。在 IP 接力赛中，我们已经看到，IP 包的头部写有出发地和目的地的IP地址。IP 包上携带的IP地址和路由器相配合，最终允许 IP 包从互联网的一台电脑传送到另一台。

在 [IP 接力赛](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247483787&idx=1&sn=c55dc4db56115623090e2d22bba4d61b&chksm=972ad0f1a05d59e727c0be3907b037b00e88e7180128603aac9ce9d158cdfbe9b906565c1b53&mpshare=1&scene=21&srcid=0903GADvZTwVybIPH6rnDla6#wechat_redirect)中，我们以 IPv4 为例说明IP包格式，其第一个4bit字段是用来说明当前所用的IP协议版本。IPv4 和IPv6 是先后出现的两个 IP 协议版本。**IPv4的地址就是一个32位的0/1序列**，比如11000000 00000000 0000000 00000011。

为了方便人类记录和阅读，我们通常将32位0/1分成4段8位序列，并用**10进制来表示每一段**(这样，一段的范围就是0到255)，段与段之间以.分隔。比如上面的地址可以表示成为192.0.0.3。IPv6地址是128位0/1序列，它也按照8位分割，以16进制来记录每一段(使用16进制而不是10进制，这能让写出来的IPv6地址短一些)，段与段之间以:分隔。

### IP地址的分配

IP 地址的分配是一个政策性的问题。**ICANN(**the Internet Corporation for Assigned Names and Numbers)是Internet的中心管理机构。ICANN 的 IANA(Internet Assigned Numbers Authourity)部门负责将 IP 地址分配给 5 个区域性的**互联网注册机构(RIR，Reginal Internet Registry)**，比如 APNIC，它负责亚太地区的 IP 分配。

然后RIR将地址进一步分配给当地的 **ISP(Internet Service Provider)**，比如中国电信和中国网通。ISP再根据自己的情况，将IP地址分配给机构或者直接分配给用户，比如将A类地址分配给一个超大型机构，而将C类地址分配给一个网吧。机构可以进一步在局域网内部分配IP地址给各个主机。(A/B/C类地址请参阅[IP接力赛](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247483787&idx=1&sn=c55dc4db56115623090e2d22bba4d61b&chksm=972ad0f1a05d59e727c0be3907b037b00e88e7180128603aac9ce9d158cdfbe9b906565c1b53&mpshare=1&scene=21&srcid=0903GADvZTwVybIPH6rnDla6#wechat_redirect))

![](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgLpoUxYwbZrzy4ajjfTR1D0NUf0ibMd1pjYic6Qn2iaQYicuQj4E5iabIoFyXYwtc36KdyIcySYfgAATicQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

​									`**5个RIR的分管区域**`

并不是所有的地址都会被分配。一些地址被预留，用于广播、测试、私有网络使用等。这些地址被称为专用地址(special-use address)。你可以查询 RFC5735 来了解哪些地址是专用地址。

(**RFC**，Request For Comments。RFC 是一系列的技术文档，用于记录Internet相关的技术和协议规定。每一个RFC 文件都有一个固定的编号。它们是互联网的一个重要财产。你可以通过 http://www.rfc-editor.org/ 来查找RFC 文件)

### IPv4地址耗尽

由于IPv4协议的地址为32位，所以它可以提供232, 也就是大约40亿个地址。如果地球人每人一个IP地址的话，IPv4地址已经远远不够。更何况，人均持有的入网设备可能要远多于一个，下图中显示了一个家庭对IP地址的需求，这种需求量已经相当常见了：

![](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgLpoUxYwbZrzy4ajjfTR1D0Daq9N573uWGHQzcFK6QcqSabVUFUfuChXVjZt6yzjw6o2EgAtELlGg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

下图显示了各大洲RIR的IPv4地址耗尽日期 (IANA已经将所有的IP分配给各个RIR)：

![](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgLpoUxYwbZrzy4ajjfTR1D0Lryq11pdC8lIq1ISfjxKSibqibhZiaRIOAeeenpib4sgiakStJLmHS6cNicg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

尽管一些技术措施(比如 NAT 技术，我会在其他文章中深入NAT)减缓了情况的紧急程度，但 IPv4 地址耗尽的一天终究还是会很快到来。很明显，我们需要更多的 IP 地址，以满足爆炸式增长的互联网设备对 IP 地址的需求。

### 更长=更好

IPv6 协议的地址最重要的改进就是：**加长**。IPv6 的地址为**128位**。准确的说，IPv4 有4,294,967,296个地址，而IPv6 有以下这么多个地址：**340,282,366,920,938,463,374,607,431,768,211,456**

这是怎样一个概念呢？如果您对数字不敏感我们可以大概计算一下，地球表面积大约为 510,067,866,000,000 平方米。在一平方厘米(大约是指甲盖大小)的面积内，我们可以有 6.67x1016 个 IP 地址！所以在短期的时间内，我们应该不会看到 IPv6 被用尽的尴尬。(不排除在未来计算机以分子尺寸出现，那么我们就会有 IPv6 耗尽危机了)

所以，为了解决 IPv4 地址耗尽危机，这就是结论：

![](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgLpoUxYwbZrzy4ajjfTR1D0ZHj0hbbUZDO4sute5ICsz1ba64QfBYeibaLnuDoqpCKedhT1Yy932XA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

### 总结

IPv4 地址正在耗尽，而 IPv6 通过更长的序列提供了更多的 IP 地址。IPv4 向 IPv6 的迁移正在发生。

阻碍迁移的过程的主要是 IPv4 和 IPv6 格式的不兼容性。老的路由器支持 IPv4 格式的 IP 包，但它们无法理解 IPv6 格式的 IP 包。所以这一迁移过程必然要伴随者设备的更新。然而，我们的许多互联网资产都是建立在 IPv4 网络上的，不可能一夜之间停止 IPv4 网络的服务而整体迁移到 IPv6 网络中。这一迁移过程注定充满坎坷。



### 本文来源：

- 公众号：[地址耗尽危机（IPv4与IPv6地址）](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247483795&idx=1&sn=17f78c3746016607b35782aaf4ed1611&chksm=972ad0e9a05d59ffa83fc04cd414b188983f4b422ff51c4c074e3ef4711ef7108367d870db7c&mpshare=1&srcid=1007lXMdILEYGgZM5BL4ofTb&scene=21#wechat_redirect)
- 作者原文：https://read.douban.com/reader/column/1788114/chapter/21060938/











