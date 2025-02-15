TLS名为传输层安全协议(Transport Layer Protocol)，这个协议是一套加密的通信协议。它的前身是SSL协议(安全套接层协议，Secure Sockets Layer)。这两个协议的工作方式类似，但TLS协议针对SSL协议进行了一些改善。SSL/TLS协议利用加密的方式，在开放的互联网环境中实现了加密通信，让通信的双方可以安心的说悄悄话。

### 加密

SSL协议的基础是加密技术。加密和解密是自古就有技术了。比如说古代的男女偷偷发生私情，不能被相互之间有血海深仇的两个家族知道。男孩问女孩要不要一起私奔。女孩第二天传来答复，上面写着：**K FQ**

男孩拿着这串字符翻来覆去想了半天，没明白女孩的意思，就以为女孩不愿放弃优渥的生活和他私奔。直到十年后，男孩忽然灵光一闪，发现如果把每个字母都替换成字母表上提前两个字母的话，这三个字符就变成了：**I DO**

这种加密方法是将原来的某种信息按照某个规律打乱。打乱的方式称为加密算法，而打乱过程中的参数就叫做密钥(cipher code)。上面女孩的加密方式是把原字母替换为字母表上后固定位的字母。而密钥就是固定的位数2了。发出信息的人根据密钥来给信息加密，而接收信息的人利用相同的密钥，来给信息解密。就好像一个带锁的盒子。发送信息的人将信息放到盒子里，用钥匙锁上。而接受信息的人则用相同的钥匙打开。加密和解密用的是同一个密钥，这种加密称为对称加密（symmetric encryption）。

如果一对一的话，那么两人需要交换一个密钥。理论上，如果密钥绝对安全，而且加密算法绝对复杂的话，对称加密是很难破解的。但通信双方很难绝对保证密钥的安全。一旦有其他人窃取到密钥，那么所有通信都变得不安全了。特别在一对多的话，如果共用同一套密钥，那么某一方通信的破解就意味着所有通信的破解。二战中盟军的情报战成果，很多都来自于破获这种对称加密的密钥。盟军破解了某个德国特工的加密手法，那么也就了解到纳粹总部的加密手法了。

对称加密的薄弱之处在于给了太多人的钥匙。如果换一种思路，只给特工锁，而总部保有钥匙，那就容易了。特工将信息用锁锁到盒子里，谁也打不开，除非到总部用唯一的一把钥匙打开。只是这样的话，特工每次出门都要带上许多锁，太容易被识破身份了。总部老大想了想，干脆就把造锁的技术公开了。特工，或者任何其它人，可以就地取材，按照图纸造锁，但无法根据图纸造出钥匙。钥匙只有总部的那一把。上面的关键是锁和钥匙工艺不同。知道了锁，并不能知道钥匙。这样，总部可以将“造锁”的方法公布给所有用户。每个用户可以用锁来加密自己的信用卡信息。即使被别人窃听到，也不用担心：只有总部才有钥匙呢！非对称加密中，给所有人用的锁被称为公钥(public key)，总部自己保留的钥匙被称为私钥(private key)。这样一种钥匙和锁分离的加密算法就叫做非对称加密(asymmetric encryption)。

### 非对称加密

对称加密的原理相对比较直观，而非对称加密听起来就有些神奇。经过非对称加密产生的密文，就算知道加密的方法，也无法获知原文。实现了非对称加密的经典算法是RSA算法。它来自于数论与计算机计数的奇妙结合。我们从下面的情境中体验一下RSA算法的妙处。

我是潜伏在龙凤大酒楼的卧底。想让下面信息以加密的方式发到总部：A CHEF HIDE A BED，厨子藏起来了一张床！这是如此的重要，需要立即通知总部。千万重要的是，不能让反革命的厨子知道。

第一步是转码，也就是将英文转换成某个对应的数字。这个对应很容易建立，比如：

![img](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgJTpvIEVicXyj8jy3XK8wsqCk8vQjoWPZ1R75VbPJ7KcMe4iaRHLXNib5dbm07NIK2JrwqzmVpkDlXlw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

将上面的信息转码，获得下面的数字序列：

![img](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgJTpvIEVicXyj8jy3XK8wsqCUnicCS1121Z913HedWLJzmT8N8XpuGx4tkS119NgfUlmmwI8vbWBWGA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

这串数字完全没有什么秘密可言。厨子发现了这串数字之后，很容易根据数字顺序，对应字母表猜出来。

为了和狡猾的厨子斗智斗勇，我们需要对这串数字进一步加密。使用总部发给我们的锁，两个数字：3和10。我们分为两步处理。第一步是求乘方。第一个数字是3，也就是说，总部指示我们，求上面数字串的3次方：

![img](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgJTpvIEVicXyj8jy3XK8wsqC1LwXpf6niaERCXUqpX7OKNxr2xzkcH7icDzQ5jHGV87N4q8JjfcIhdrg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

将这串数字发回总部。中途被厨子偷看到，但一时不能了解其中的意思。如果还是像刚才一样对应字母表的话，信息是：AGBEFBIDEAHED，这串字母完全不包含正常的单词。

信息到了总部。总部开始用神奇的钥匙来解读。这个钥匙是3。在这个简单的粒子里，钥匙不小心和之前锁中的一个数字相同。但这只是巧合。复杂的情况下很容易让锁和钥匙不同。解锁过程也是两步。第一步求钥匙次的乘方，即3次方。第二步求它们除以10（锁之一）的余数。

![img](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgJTpvIEVicXyj8jy3XK8wsqCbcDl1zXNNzdTEHWl32sypjAwFeEmkyctlo97cLDgr8ZRbOqgsqkAwg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

这正是我们发送的信息。对应字母表，总部可以立即知道原来的信息。就此，我们简单的体验了RSA算法的使用过程。鉴于这里篇幅有限，这里不再详细解释RSA算法的原理。

### SSL协议

可以看到，非对称加密从安全性上要强过对称加密。但天下没有免费的午餐。非对称加密的运算成本同样也比较高。为了兼顾效率和安全，SSL协议同时使用了非对称和对称加密。它用对称加密算法来加密信息本身。但对于安全性比较脆弱的对称加密密钥，则采用非对称加密的方式来传输。

SSL协议分为客户端和服务器端。通信的核心步骤很简单：

（1）双方利用明文通信的方式确立使用的加密算法。

（2）利用非对称算法通信，交换一个密钥。

（3）该密钥用于对称加密算法，加密接下来的通信正文。

可以看到，SSL协议的关键是用一个非常安全的方式来交换一个对称密钥。交换的过程会比上面的描述更加复杂一些。

（1）客户发起请求时，除了说明自己支持的非对称加密算法，还会附加一个客户端随机数(client random)。

（2）服务器回复请求时，会确定非对称加密算法和哈希函数，并附上公钥。此外，服务器端还会在此次通信中附加一个服务器端随机数(server random)。

（3）客户端会产生第三个随机数(Premaster secret)，然后利用服务器确定的非对称加密算法和公钥来加密这个随机数，再发送给服务器端。

（4）客户端用自己的私钥解密第三个随机数。

（5）这样，客户端和服务器端都知道了三个随机数。双方各自用商量好的哈希函数从三个随机数获得对称加密的密钥。

即使明文通信的时候，某些信息被窃听，但第三步的非对称加密通信部分可以保证窃听者无法完整的获得三个随机数。这样，窃听者还是不知道对称加密的密钥是什么。这样，对称加密的密钥就在一个安全的环境中获得了。为了进一步安全，服务器的公钥会包含在一个数字证书中发送给客户。这样，客户还可以通过数字证书来验证服务器的身份，以免服务器本身出现问题。 

今年来使用越来越广泛的HTTPS协议就是在SSL/TLS协议的基础上进行通信。HTTP协议在通信过程中要经过多重路由，很容易被窃听。经过SSL协议加密的信息就算被窃听，也只能被通信目的地的人解读，从而保证了信息的安全。所以，如果所访问的网站没有使用HTTPS协议，那么在输入银行账号和密码之类的敏感信息时，就要三思而后行了。

![img](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgJTpvIEVicXyj8jy3XK8wsqC5VDpQIacOKGhf4JPc86h5y6w12KMiaReiayAUHZBtHU6XZBlE3TB5c3A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
					`当浏览器出现锁的符号时，说明访问的资源使用了HTTPS通信`



### 本文来源：

- 公众号：[我和你的悄悄话 (SSL/TLS协议)](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247484075&idx=1&sn=04d34c3758535a7c6e1d2c9f7b9e4f9c&chksm=972ad3d1a05d5ac7c307216e5342f5ed53d44349a0fd25e0d5b1723ddea386ab431cc8328c65&mpshare=1&scene=21&srcid=1222BTWxi2HnoSBCDcsG3weC#wechat_redirect)
- 作者原文：https://read.douban.com/reader/column/1788114/chapter/26130137/

