# Day001 Homework

Topic: Fixed Broadband Overview

Status: Ready - Beginner Friendly

## Part 1：先用自己的话复述

请不要背定义。先用自己的话回答下面问题。

1. 固定宽带和手机流量最大的不同是什么？
2. 为什么说家用路由器不是只负责发 Wi-Fi？
3. 如果家里电脑要访问家里手机，为什么不一定需要访问互联网？
4. DHCP 像不像“自动发门牌号”？为什么？
5. 私网 IP 为什么不能直接代表互联网上的唯一设备？
6. NAT 为什么像“家庭代表对外说话”？
7. 默认网关为什么像“出小区的大门”？
8. DNS 为什么像“电话簿”？
9. ONT 和 OLT 分别更靠近用户还是运营商？
10. 用户说“不能上网”时，为什么要先问影响范围？

## Part 2：完成实验

完成 `Day001/lab.md`。

实验不评分，目标是看懂自己的 Mac 当前网络状态。

重点不是命令本身，而是理解命令结果：

- `ifconfig`：我是谁，我的本地 IP 是什么。
- `route -n get default`：我要出门时先找谁。
- `scutil --dns`：我查域名时问谁。
- `ping -c 4 <gateway>`：我到家庭网关这段通不通。
- `arp -a`：我在本地网络里是否知道网关的 MAC 地址。

## Part 3：推荐阅读

如果今天时间有限，优先读前 3 个链接。

1. [Cloudflare：How does the Internet work?](https://www.cloudflare.com/learning/network-layer/how-does-the-internet-work/)
2. [Cloudflare：What is a LAN?](https://www.cloudflare.com/learning/network-layer/what-is-a-lan/)
3. [Cloudflare：What is a router?](https://www.cloudflare.com/learning/network-layer/what-is-a-router/)
4. [Cloudflare：What is DNS?](https://www.cloudflare.com/learning/dns/what-is-dns/)
5. [WIRED：DHCP](https://www.wired.com/2010/02/dhcp/)
6. [Wikipedia：Network address translation](https://en.wikipedia.org/wiki/Network_address_translation)
7. [Wikipedia：Passive optical network](https://en.wikipedia.org/wiki/Passive_optical_network)

阅读时不要追求全部看懂。今天只需要把它们和课程里的家庭例子对应起来。

## Part 4：口头问答

完成 lesson、lab 和自查后，对 Codex 说：

```text
开始问答
```

问答规则：

- 每轮 10 题
- 每题 10 分
- 总分 100 分
- 对话式回答
- 动态出题，不使用固定题库
- 只有问答结束后才生成 oral exam 文件

Day001 还没有历史课程和错题，所以第一轮会主要围绕今天的固网架构、家庭网络术语和故障域判断。

## Completion Checklist

- [ ] Lesson completed
- [ ] Lab completed
- [ ] Beginner self-check completed
- [ ] Recommended reading reviewed
- [ ] Oral exam completed
- [ ] Journal completed
