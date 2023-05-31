# V2Ray / Trojan 传输方式哪个好？(原理对比)
来源： https://www.idleleo.com/02/4064.html
* [Trojan是什么？](https://www.idleleo.com/02/4064.html#title-0)
* [Trojan比V2Ray更安全？](https://www.idleleo.com/02/4064.html#title-1)
* [Trojan比V2Ray更快？](https://www.idleleo.com/02/4064.html#title-2)
* [应不应该改用Trojan？](https://www.idleleo.com/02/4064.html#title-3)
* [相关阅读推荐](https://www.idleleo.com/02/4064.html#title-4)

**目录**

前段时间， 有些社群爆出有论文研究V2Ray识别的方法。这让大家开始寻找一些V2Ray可能的替代品。这不，最近Trojan就十分火爆，笔者发现很多油管UP🐷开始着手测试Trojan的性能。笔者前段时间也发表了如何搭建的文章，《[替代V2Ray？- Trojan搭建教程](https://www.idleleo.com/02/3899.html)》（由于某些原因不得不使用密码，默认密码在输入密码的下面）。那么，我们从原理的角度来简单分析一下，V2Ray与Trojan究竟那个更好，更多详细的分析可见《[关于Trojan、CDN、V2Ray的种种问题 （原理分析）](https://www.idleleo.com/03/4320.html)》一文。

#### Trojan是什么？

根据官网给出的解释，Trojan的设计理念与传统协议的设计理念相反，Trojan不使用强加密和随机模糊，而是直接模仿互联网最常见的协议 `HTTPS` ，以此来达到设计的目的。

![](https://i.imgur.com/YlsejWy.png)

当然，这显然是胡扯。从笔者另一篇文章《[V2Ray / SSR 传输协议哪个好? (各种协议对比)](https://www.idleleo.com/05/2071.html)》中可以知道， `HTTPS` 协议本身就含有加密，而且 `TLS 1.3` 更是强加密，现今无法通过简单方式破解。Trojan本身使用了 `HTTPS` 的特性，自带了加密属性，只不过这种加密大家都在用而已，因此官网描述是不准确的。

#### Trojan比V2Ray更安全？

答案是：不见得。我们从上述描述中可以看出，Trojan本身已经是确定好了自身的协议类型，而V2Ray使用多种协议。笔者之前的文章特意探讨过V2Ray的各种协议区别。因此不能简单的把Trojan和V2Ray进行类比。为了公平起见，我们可以选取V2Ray“最安全”的协议WS+TLS与Trojan进行对比。

我们不难发现，若V2Ray使用了WS+TLS方式，其协议与Trojan本身差异并不大。两者建立连接的过程有区别，但是从流量本身，或者说第三者看流量是难以发现区别的。也就是说，对于第三者的监听，这两类协议与普通流量表现均一致，这是两者的共性。

![](https://i.imgur.com/kWlDwIQ.png)

根据Trojan的官网描述，Trojan在建立连接的过程中考虑了现行加密协议的不足，也参考了传统网站服务器的连接过程，Trojan开发者似乎有意识的将Trojan服务器伪造成只处理 `HTTPS` 流量的Nginx服务器，这点很好，Nginx服务器的高性能与广泛流行能够一定程度让Trojan进行伪装，而V2Ray并不具有这类特性。不过V2Ray本身在连接过程中较之Trojan增加了些许步骤。

![](https://i.imgur.com/Kxt6liI.png)

在笔者的文章《[V2Ray / SSR 加密方式哪个好? (加密方式对比)](https://www.idleleo.com/09/3058.html)》中，笔者介绍了V2Ray的加密形式，V2Ray的加密形式并不局限于一种协议， `Vmess` 加密在各个传输协议中均是存在的。大家可以这么理解，对于一个内容， `Vmess` 会根据客户端支持的加密方式进行加密，在服务端进行解密，这是基础的一层加密，如果使用WS+TLS的协议进行传输这些内容时，会在内容加密的基础上再进行一次 `TLS` 加密，也就是说， `Vmess` 会加密两次。因此，即便在传输过程中遭到中间人攻击，导致传输内容变成明文及 `TLS` 加密失效，V2Ray传输的内容依旧能一定程度保持安全。

![](https://i.imgur.com/RbUxnJ2.png)

从反向代理的角度看，在《[关于Trojan、CDN、V2Ray的种种问题 （原理分析）](https://www.idleleo.com/03/4320.html)》一文中，笔者提到，V2Ray是支持反向代理的，而Trojan却并不支持。这使得Trojan无法使用CDN这类使用反向代理技术的服务，从而无法隐藏真实IP。无法处理反向代理流量减少了Trojan的安全性，笔者相信这是Trojan团队未来需要解决的问题之一。

所以对于究竟哪个更安全的讨论是仁者见仁智者见智的。实际上，现在的 `TLS 1.3` 已经足够安全，像是笔者说的中间人攻击这类手段本身代价很大，笔者觉得不会这么为了普通人动用这类手段。而CDN隐藏IP对于普通使用的安全性影响亦有限，因此从安全性来说，两者协议差别不大。

#### Trojan比V2Ray更快？

从上述的笔者描述中，我们能知道，V2Ray在使用WS+TLS传输的过程中进行了两次加密，虽然 `TLS` 加密对于速度的影响并不大，但是 `Vmess` 加密的影响还是有的。尤其在使用一些并不是很友好的加密协议时，影响更大。当然，相信有童鞋会想到在使用V2Ray时把加密方式改成None，但这并不代表能与Trojan保持一致，笔者在《[V2Ray / SSR 加密方式哪个好? (加密方式对比)](https://www.idleleo.com/09/3058.html)》文章中提到过，V2Ray即便在加密协议设置成None的情况下还是会有加密，具体情况大家可以参考那篇文章。

而对于Trojan而言，在处理与Trojan有关的流量时，Trojan表现优越，但是在处理其他流量诸如正常的 `HTTPS` 或者 `Vmess` 时，由于多一个流量本身的判断，导致效能低下，笔者认为这是Trojan的致命伤，期望后期Trojan团队能有所改变。

因此，仅从真正使用的流量的传输速度看，Trojan在原理上是一定快于V2Ray的，倒不是Trojan有什么黑科技，而是Trojan比V2Ray更简单，我们可以认为Trojan就是一个只有WS+TLS协议的V2Ray的简化版本（不能简单等同）。这也许就是Trojan的设计理念吧，笔者认为把Trojan介绍成： **以简单的方法利用互联网最常见、最安全的形式完成连接的一种协议** ，这样的介绍或许会更好。

![](https://i.imgur.com/pb9GQ5r.png)

从各个UP🐷的测评看，Trojan的连接速度确实快于V2Ray的WS+TLS，当然这种提升不会非常明显。原因很简单，毕竟AES之类的加密如果对于性能影响很大，又怎么会是现在主流的加密方式呢？

#### 应不应该改用Trojan？

对于使用Trojan，根据Trojan的教程《[替代V2Ray？- Trojan搭建教程](https://www.idleleo.com/02/3899.html)》，普通人需要解决域名、解析的问题。这与V2Ray使用WS+TLS的门槛差不多了。如果仍有使用SSR的童鞋，笔者建议你赶紧改成Trojan或者V2Ray。

对于已经使用WS+TLS协议V2Ray用户，现在由于Trojan本身还算“年轻”，各个平台的客户端是远不及V2Ray来的完善，因此，为了提升一些些的传输速度，而选择改为Trojan是需要三思的。

那么是否需要从未来安全角度改用Trojan呢？笔者认为，若是V2Ray使用WS+TLS最终被封锁，那么其实离Trojan被封锁也不远了。毕竟Trojan不是一个具有革命性的新协议，两者本质上说均是采取了现在流行的 `TLS` 协议，因此没必要担心未来V2Ray是否安全而改用Trojan。