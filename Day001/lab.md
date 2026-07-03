# Day001 Lab

Topic: 观察你自己的家庭网络

Status: Ready - Beginner Friendly

## 实验目标

今天不做复杂实验，也不要求你配置 Docker、containerlab 或 FRRouting。

原因是：Day001 的目标是先把“我自己的电脑是怎么接入家庭网络的”看清楚。你先能看懂本机 IP、默认网关、DNS，再进入后续虚拟化实验会稳很多。

完成后你应该能解释：

- 我的 Mac 现在拿到了什么 IP 地址。
- 我的默认网关是谁。
- 我的 DNS 服务器是谁。
- 我的 Mac 到家用路由器这一段是否通。
- ARP 表里能不能看到网关的 MAC 地址。

## 实验环境

- Mac
- Terminal
- Wireshark，可选

## 实验前先理解

你可以把今天的实验想成“查户口”：

- IP 地址：设备在家庭网络里的门牌号。
- 默认网关：离开家庭网络时先找谁。
- DNS：把网站名字翻译成 IP 地址的电话簿。
- ARP：在同一个家庭网络里，根据 IP 找到对方网卡地址的方法。

## 步骤 1：查看本机 IP 地址

运行：

```sh
ifconfig
```

你要找当前正在使用的接口。

常见情况：

- Wi-Fi：可能是 `en0` 或 `en1`
- 有线网卡或扩展坞：可能是其他 `enX`

找 `inet` 后面的地址，比如：

```text
inet 192.168.1.23
```

记录：

```text
本机 IP：
使用的接口：
```

理解一下：这个 IP 大概率是家用路由器通过 DHCP 自动发给你的。

## 步骤 2：查看默认网关

运行：

```sh
route -n get default
```

找：

```text
gateway:
interface:
```

记录：

```text
默认网关：
出口接口：
```

理解一下：默认网关通常就是你家用路由器的家庭侧地址，比如 `192.168.1.1`。

## 步骤 3：查看 DNS 服务器

运行：

```sh
scutil --dns
```

找 `nameserver` 字段。

记录：

```text
DNS Server 1：
DNS Server 2：
```

如果你看到 DNS 是 `192.168.1.1`，不要惊讶。这通常表示你的 Mac 先问家用路由器，路由器再帮你转问上游 DNS。

## 步骤 4：测试能不能到默认网关

运行：

```sh
ping -c 4 <default-gateway-ip>
```

例如：

```sh
ping -c 4 192.168.1.1
```

记录：

```text
能否 ping 通默认网关：Yes / No
平均时延：
```

理解一下：

- 如果能 ping 通，说明你的 Mac 到家用路由器这一段大概率正常。
- 如果 ping 不通，先不要怀疑运营商网络，优先检查 Wi-Fi、网线、本机 IP、CPE。

## 步骤 5：查看 ARP 表

运行：

```sh
arp -a
```

找默认网关对应的条目。

记录：

```text
网关 IP：
网关 MAC：
接口：
```

理解一下：ARP 是本地网络里的“找人方式”。你的 Mac 知道网关 IP 后，还需要知道网关的 MAC 地址，才能在同一个局域网里把数据发给它。

## 步骤 6：可选 Wireshark 抓包

打开 Wireshark，选择当前活跃接口。

然后运行：

```sh
ping -c 2 <default-gateway-ip>
```

尝试观察：

- ARP request：谁是这个 IP？
- ARP reply：这个 IP 的 MAC 是我。
- ICMP echo request：ping 请求。
- ICMP echo reply：ping 响应。

今天不要求一定抓到完整过程。能理解这些包在做什么就够了。

## 实验完成标准

你不需要把结果发给 AI 自动评分。

你只需要能用自己的话解释：

1. 我的 Mac 在家庭网络里的 IP 是什么。
2. 我的默认网关是谁。
3. 我的 DNS 服务器是谁。
4. 能 ping 通默认网关说明什么。
5. 不能 ping 通默认网关时，为什么不应该先怀疑核心网。

## 参考输出格式

```text
本机 IP：192.168.1.23
使用的接口：en0
默认网关：192.168.1.1
DNS Server 1：192.168.1.1
网关 MAC：aa:bb:cc:dd:ee:ff
能否 ping 通默认网关：Yes
```

你的实际数值会不同。

## Lab Notes

由学习者填写。
