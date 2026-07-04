# Day002 Lesson

Topic: 打开一个网站时，家庭网关、DNS、NAT、ONT、BNG/BRAS 分别做什么

Status: Ready - Beginner Friendly

## 今天学习目标

Day001 你已经建立了固定宽带的大地图。今天只沿着一个动作学习：

```text
在电脑浏览器里输入 www.example.com，然后页面打开。
```

学完以后，你应该能用自己的话解释：

- DHCP 是在设备入网时拿地址，不是每次打开网页都发生。
- DNS 是把网站名字查成 IP 地址，查询通常由终端发起。
- 默认网关通常是家庭 CPE，不是普通桥接型 ONT。
- NAT 通常发生在家庭 CPE，作用是让多个家庭设备共享一个上级可路由地址。
- ONT 主要做光接入转换和 PON 接入，不等于家庭默认网关，除非它是光猫路由一体机。
- BNG/BRAS 管的是用户接入会话、地址、策略、计费等，不是每访问一个网站才临时认证一次。

今天不要求你会配置运营商设备。重点是把“谁在什么时候做什么”分清楚。

## 今天先懂这些词

| 术语 | 通俗解释 | 正式含义 | 家庭例子 | 工程意义 |
| --- | --- | --- | --- | --- |
| 终端 Host | 真正要上网的设备 | 发起或接收网络通信的端系统 | 你的 Mac、手机、电视 | 排障时要先看终端有没有正确 IP、网关、DNS |
| DHCP | 自动发门牌号 | Dynamic Host Configuration Protocol，自动下发 IP、网关、DNS 等参数 | 手机连 Wi-Fi 后自动拿到 `192.168.1.23` | 如果 DHCP 错了，设备可能连上 Wi-Fi 但不能正常出网 |
| 默认网关 Default Gateway | 出家门先找的大门 | 终端访问本地网络外目标时交给的下一跳 | Mac 要访问外网，先交给家用路由器 `192.168.1.1` | 判断问题在本机/家庭侧还是更上游的第一道分界线 |
| DNS | 网站名字电话簿 | Domain Name System，把域名解析为 IP 地址 | 把 `www.example.com` 查成一个服务器 IP | DNS 坏了会像“网页打不开”，但 IP 链路可能仍然通 |
| NAT | 家庭代表对外说话 | Network Address Translation，转换报文中的地址/端口信息 | 多台设备用私网 IP，对外共享一个出口地址 | 影响公网访问、端口映射、CGNAT、用户溯源和排障 |
| ONT | 光纤和家庭以太网之间的翻译器 | Optical Network Terminal，PON 用户侧光网络终端 | 常说的光猫 | 负责接入光链路，不应默认把所有三层功能都算到 ONT 上 |
| BNG/BRAS | 宽带用户的业务登记与放行点 | Broadband Network Gateway / Broadband Remote Access Server | 判断某条宽带线或会话是谁、用什么套餐、给什么策略 | 关联认证、地址、限速、会话、计费和运营商侧排障 |
| 会话 Session | 已经办好的入场手续 | 一段被网络设备识别、维护和计费的用户连接状态 | 宽带拨号或 IPoE 接入成功后形成的上网状态 | 会话通常先建立，之后访问多个网站都复用这个状态 |

## 1. 先把时间顺序分清楚

打开网页不是一个单独动作，而是一串动作。可以把它想成你开车去朋友家：

```text
1. 先拿到家庭住址和出门路线：DHCP
2. 查朋友家具体地址：DNS
3. 从家门出去：默认网关
4. 家庭代表对外通行：NAT
5. 经过小区出口和城市道路：ONT、OLT、接入网、汇聚网
6. 经过运营商用户入口管理点：BNG/BRAS
7. 到达互联网和网站服务器：Internet
```

注意：这些动作不是每次都从零开始。

例如，电脑连上 Wi-Fi 后可能已经通过 DHCP 拿到了 IP 地址；宽带用户会话也可能早就建立好了。你打开一个新网页时，通常不会重新走一遍“宽带认证”。

延伸学习：

- [Practical Networking: Packet Traveling](https://www.practicalnetworking.net/series/packet-traveling/packet-traveling/)  
  为什么推荐：它用分步骤方式讲一个包如何穿过网络，适合软件背景学习者建立路径感。  
  今天读到什么程度：只看开头和目录，知道后续会分主机、交换机、路由器来讲即可。

## 2. DHCP：上网前先拿到自己的本地配置

家庭生活类比：

你搬进一个小区，物业给你：

```text
门牌号：你住哪一户
小区大门：你出门先走哪
电话簿服务：你查别人地址时问谁
```

网络里也是类似。终端连上家庭网络后，通常会通过 DHCP 从 CPE 获得：

```text
IP 地址：192.168.1.23
子网掩码：255.255.255.0
默认网关：192.168.1.1
DNS 服务器：192.168.1.1 或运营商/公共 DNS
```

正式定义：DHCP 是动态主机配置协议，用于自动向终端分配 IP 地址和相关网络参数。

工程意义：

- 没有 IP，终端不知道自己是谁。
- 没有默认网关，终端不知道出本地网络该交给谁。
- DNS 配错，网页可能打不开，但这不一定是链路断了。
- 在运营商网络里，家庭侧 DHCP、运营商侧 IPoE DHCP、PPPoE 地址分配是不同层面的事情，不能混为一谈。

延伸学习：

- [RFC 2131: Dynamic Host Configuration Protocol](https://www.rfc-editor.org/rfc/rfc2131)  
  为什么推荐：这是 DHCP 的标准文档，适合建立“这是标准协议，不是厂商私有技巧”的意识。  
  今天读到什么程度：只读 Abstract、Introduction，看到 DHCP 用来传递配置参数和分配地址即可。

## 3. DNS：终端发起查询，不是 ONT 主动替你查网站

家庭生活类比：

你知道朋友叫“张三”，但快递系统需要具体地址。于是你查通讯录：

```text
张三 -> 某小区某栋某户
```

浏览器也是这样。你输入：

```text
www.example.com
```

终端需要先知道它对应的 IP 地址。这个查询一般由终端或终端上的系统解析器发起，可能先问家庭 CPE，也可能直接问运营商 DNS、企业 DNS 或公共 DNS。

常见家庭路径：

```text
Mac/手机
  -> 先问 CPE 上的 DNS relay，比如 192.168.1.1
  -> CPE 再问上游 DNS resolver
  -> 返回网站 IP
```

正式定义：DNS 是域名系统，用来把人容易记住的域名解析为机器通信所需的 IP 地址。DNS 是分布式系统，不是一台服务器保存全世界所有域名。

工程意义：

- 能访问 `8.8.8.8` 但打不开 `www.example.com`，优先怀疑 DNS。
- DNS 解析慢会表现为“打开网页前卡一下”。
- 家用路由器显示为 DNS，不代表它知道全部答案；它可能只是 DNS relay。
- 普通桥接型 ONT 不应该被描述成“负责 DNS 查询”的设备。

延伸学习：

- [Cloudflare: What is DNS?](https://www.cloudflare.com/learning/dns/what-is-dns/)  
  为什么推荐：解释清楚 DNS 为什么存在、递归解析器和权威服务器的大致关系。  
  今天读到什么程度：读到 “What are the steps in a DNS lookup?”，不需要背 8 个步骤。

## 4. 默认网关和 NAT：通常在 CPE 上，不要默认放到 ONT 上

家庭生活类比：

你在家里有很多成员：

```text
手机：192.168.1.23
电脑：192.168.1.24
电视：192.168.1.25
```

这些是家庭内部编号。大家要出门办事时，先从家门出去。这个“家门”就是默认网关，通常是 CPE 的 LAN 地址：

```text
192.168.1.1
```

出门后，如果外部世界不能直接识别每个家庭内部编号，CPE 会做 NAT。它像家庭代表一样，把内部成员的请求统一用家庭出口身份发出去，并记录“哪个回包该交给哪个成员”。

```text
手机 192.168.1.23:51500
  -> CPE NAT
  -> 上级网络看到 CPE 出口地址:某端口
```

正式定义：

- 默认网关是终端访问非本地网络目的地址时默认交给的下一跳。
- NAT 是网络地址转换，会在报文经过转换设备时修改地址或端口信息，并维护映射关系。

工程意义：

- 单用户不能上网时，先看终端能不能到默认网关。
- NAT 位置影响端口映射、家庭服务器访问、游戏联机、VoIP、VPN。
- 如果运营商侧还有 CGNAT，用户家里做了一次 NAT，运营商侧可能又做一次 NAT。
- 普通桥接 ONT 更像光接入转换器；CPE 才常做默认网关和 NAT。光猫路由一体机是例外：ONT 和 CPE 功能在同一台设备里。

延伸学习：

- [Cloudflare: What is a router?](https://www.cloudflare.com/learning/network-layer/what-is-a-router/)  
  为什么推荐：能帮助区分 router 和 modem/ONT 的角色，适合纠正“ONT 就是网关”的混淆。  
  今天读到什么程度：读 “What is the difference between a router and a modem?”。
- [Practical Networking: Network Address Translation](https://www.practicalnetworking.net/series/nat/nat/)  
  为什么推荐：用网络工程视角讲 NAT，后续理解端口映射和 CGNAT 会用到。  
  今天读到什么程度：只看系列目录和开头，不需要深入 NAT 类型。
- [RFC 3022: Traditional IP Network Address Translator](https://www.rfc-editor.org/rfc/rfc3022)  
  为什么推荐：这是传统 NAT 的标准参考之一。  
  今天读到什么程度：只看标题和 Introduction，知道 NAT 是标准化讨论过的地址转换机制即可。

## 5. ONT：先把它当作光接入转换点，而不是万能网络盒子

家庭生活类比：

运营商光纤像一条“光语言”的专线，你家的电脑和路由器说的是以太网/Wi-Fi 语言。ONT 就像翻译口：

```text
运营商光纤/PON
  <-> ONT
  <-> 家庭以太网/CPE
```

正式定义：ONT 是光网络终端，位于用户侧，接入 OLT 所在的 PON 网络，把光接入侧和家庭/用户侧网络连接起来。

工程意义：

- 查单用户故障时，ONT 在线状态、光功率、LOS、PON 注册状态非常关键。
- 但“网页 DNS 查询”“默认网关”“NAT”不应默认算在普通桥接 ONT 上。
- 如果设备是光猫路由一体机，需要在脑中拆成两个功能：ONT 功能 + CPE 路由功能。

```text
桥接型场景：
Mac -> CPE(网关/NAT/DNS relay) -> ONT(光接入转换) -> OLT

一体机场景：
Mac -> 光猫路由一体机(ONT + CPE 功能) -> OLT
```

延伸学习：

- [Wikipedia: Passive optical network](https://en.wikipedia.org/wiki/Passive_optical_network)  
  为什么推荐：适合先认识 PON、OLT、ONU/ONT、分光这些基本词。  
  今天读到什么程度：只看开头定义和基本组成，不需要看协议细节。

## 6. BNG/BRAS：会话先建立，之后网页访问复用这个状态

家庭生活类比：

你进入一个会员制图书馆时，门口先查：

```text
你是谁
会员是否有效
能进哪些区域
有什么速度/权限限制
```

查完以后，你在图书馆里读不同的书，不会每翻一本书都重新办一次会员认证。

BNG/BRAS 类似。它通常在运营商网络边缘，负责宽带用户接入相关功能，例如：

- 识别用户或线路
- 建立 PPPoE 或 IPoE 用户会话
- 分配或管理地址
- 应用速率、QoS、访问策略
- 对接 AAA/RADIUS、计费、运营支撑系统

正式定义：BNG/BRAS 是运营商宽带网络中聚合和管理用户接入会话的网关/服务器，位于接入/汇聚网络与运营商 IP 网络之间。

工程意义：

- 用户正常上网前，会话通常已经建立。
- 打开一个网站时，流量会经过 BNG/BRAS 转发和策略处理，但不是每个网站访问都重新认证一次。
- 排障时可以看用户是否在线、会话是否存在、地址是否分配、策略是否正确。
- 华为运营商场景中，排障思路常围绕 `display` 查看用户会话、接口、路由、地址池、AAA 等证据。

示意路径：

```text
用户上线阶段：
CPE/ONT/OLT -> 接入/汇聚 -> BNG/BRAS -> AAA/RADIUS -> 会话建立

访问网站阶段：
Mac -> CPE -> ONT -> OLT -> 汇聚 -> BNG/BRAS -> 核心网/Internet -> 网站
```

延伸学习：

- [Broadband Forum TR-101 technical library](https://www.broadband-forum.org/technical-library/?number=TR-101)  
  为什么推荐：Broadband Forum 是宽带架构的重要行业组织，TR-101 是理解以太网宽带汇聚架构的经典材料之一。  
  今天读到什么程度：只知道这是运营商宽带汇聚架构参考，不需要下载或通读。
- [Wikipedia: Broadband remote access server](https://en.wikipedia.org/wiki/Broadband_remote_access_server)  
  为什么推荐：用较短篇幅说明 BRAS/BNG 站在 ISP 网络边缘并聚合用户会话。  
  今天读到什么程度：只看开头和主要功能列表。

## 7. 一张纠错表：把 Day001 最后一题修正过来

| 容易混淆的说法 | 更准确的说法 |
| --- | --- |
| ONT 是默认网关 | 普通桥接 ONT 主要做光接入转换；默认网关通常是 CPE。光猫路由一体机是功能集成例外。 |
| ONT 去查 DNS | DNS 查询通常由终端发起，可能先问 CPE 的 DNS relay 或上游 DNS。 |
| NAT 发生在 ONT | 家庭场景里 NAT 通常在 CPE；如果一体机做路由，则 NAT 在一体机的路由功能中。 |
| 每打开一个网站都去 BNG/BRAS 认证 | 用户会话通常在上线时建立；访问不同网站时复用已建立的会话和策略。 |
| DNS 失败就是运营商链路断 | DNS 失败可能只是解析失败，IP 连通性仍可能正常。 |

## 8. 华为运营商排障思路：先问证据，不急着下结论

今天不需要背命令，只要建立 `display` 思维。

排障时不要说：

```text
用户打不开网页，所以核心网坏了。
```

应该按证据问：

```text
终端有没有 IP、网关、DNS？
终端能不能 ping 默认网关？
CPE 是否拨号或获取上级地址成功？
ONT 是否在线，光功率是否正常？
OLT PON 口和用户侧状态是否正常？
BNG/BRAS 上有没有用户会话？
DNS 查询是否成功？
到公网 IP 是否可达？
```

华为 VRP 风格的排障表达可以先长这样：

```text
display interface brief
display ip routing-table
display arp
display access-user
display aaa online-fail-record
display ip pool
```

这些命令今天只作为“证据意识”出现，不要求你在真实设备上执行。

## 今日链接清单

- [Practical Networking: Packet Traveling](https://www.practicalnetworking.net/series/packet-traveling/packet-traveling/)
- [RFC 2131: Dynamic Host Configuration Protocol](https://www.rfc-editor.org/rfc/rfc2131)
- [Cloudflare: What is DNS?](https://www.cloudflare.com/learning/dns/what-is-dns/)
- [Cloudflare: What is a router?](https://www.cloudflare.com/learning/network-layer/what-is-a-router/)
- [Practical Networking: Network Address Translation](https://www.practicalnetworking.net/series/nat/nat/)
- [RFC 3022: Traditional IP Network Address Translator](https://www.rfc-editor.org/rfc/rfc3022)
- [Wikipedia: Passive optical network](https://en.wikipedia.org/wiki/Passive_optical_network)
- [Broadband Forum TR-101 technical library](https://www.broadband-forum.org/technical-library/?number=TR-101)
- [Wikipedia: Broadband remote access server](https://en.wikipedia.org/wiki/Broadband_remote_access_server)

## 今天必须掌握

请把下面这句话背后的逻辑讲清楚：

```text
电脑先通过 DHCP 拿到本地配置；
打开网页时先做 DNS 解析；
访问外部 IP 时把包交给默认网关 CPE；
CPE 常做 NAT；
ONT 主要负责光接入转换；
BNG/BRAS 管用户会话和策略，但不是每打开一个网页才认证一次。
```
