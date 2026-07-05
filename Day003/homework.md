# Day003 Homework

Topic: 以太网、MAC、ARP 和默认网关选择

Status: Ready - Beginner Friendly

## Part 1：先用自己的话复述

请不要背定义。用自己的话回答下面问题：

1. 为什么说 MAC 地址更像“本地门牌”，不是公网地址？
2. 以太网帧和 IP 包有什么区别？今天只要求说出“本地投递”和“最终目标”的区别。
3. 家里电脑访问家里手机传文件，为什么不一定要经过互联网？
4. CPE 的交换功能在家庭内部互访时大概做什么？
5. ARP 解决什么问题？它和 DNS 的问题有什么不同？
6. 为什么 ARP 请求经常是广播？
7. 访问同一网段设备时，电脑 ARP 查询的是谁？
8. 访问外网时，电脑为什么 ARP 查询默认网关，而不是远端网站服务器？
9. 请解释这句话：以太网帧目的 MAC 是网关 MAC，但 IP 包目的 IP 仍是网站 IP。
10. 如果电脑能查到 DNS 结果，但 ARP 不到默认网关，可能出现什么现象？

## Part 2：完成实验

完成 `Day003/lab.md`。

重点不是背命令，而是看懂两条链：

```text
同网段：
pc1 -> ARP 查询 pc2 MAC -> 直接发给 pc2

跨网段：
pc1 -> ARP 查询默认网关 MAC -> 先发给 gw -> gw 再转发到 srv
```

请记录：

```text
pc1 eth1 IP：
pc1 eth2 IP：
pc1 默认网关：
pc2 IP：
srv IP：
访问 pc2 后 pc1 的 ARP/邻居表里出现了谁：
访问 srv 后 pc1 的 ARP/邻居表里出现了谁：
一句话解释 DNS 和 ARP 的区别：
```

如果实验环境暂时跑不起来，请至少阅读 lab 并手画拓扑。今天允许环境问题不阻塞学习。

## Part 3：重点复习

结合 Day002 的 Knowledge Review，今天重点复习这 3 点：

- DNS Resolution Path：DHCP 给 DNS 服务器地址；DNS 查询返回网站 IP；ARP 查询本地下一跳 MAC。
- Home Gateway vs ONT Role：默认网关、ARP 下一跳、NAT 常常落在 CPE/路由功能上；普通桥接 ONT 主要做光接入转换。
- BNG/BRAS Session Timing：访问网站时流量经过 BNG/BRAS 的会话和策略处理，但不是每个网站都重新认证一次。

## Part 4：口头问答说明

完成 lesson、lab 和自查后，对 Codex 说：

```text
开始问答
```

问答规则：

- 每轮 10 题
- 每题 10 分
- 总分 100 分
- 对话式回答
- 必须一题一题进行
- 第 10 题也必须先单独评分和点评，再进入总分总结
- 只有问答结束后才生成 oral exam 文件

本轮出题比例：

- 今天课程：40%
- 最近课程：20%
- 历史课程：20%
- Knowledge Review：20%

## Completion Checklist

- [ ] Lesson completed
- [ ] Lab read or completed
- [ ] Can explain MAC as local delivery address
- [ ] Can explain ARP as IP-to-next-hop-MAC resolution
- [ ] Can distinguish DNS result and ARP result
- [ ] Can explain why external traffic ARPs for the default gateway
- [ ] Oral exam completed
- [ ] Journal completed
