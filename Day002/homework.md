# Day002 Homework

Topic: 家庭网关、DNS、NAT、ONT、BNG/BRAS 职责边界

Status: Ready - Beginner Friendly

## Part 1：先用自己的话复述

请不要背定义。用自己的话回答下面问题：

1. 为什么说 DHCP 更像“入网时拿本地配置”，而不是“每打开一个网站都发生”？
2. 终端通过 DHCP 通常会拿到哪几类信息？
3. DNS 查询为什么更像“查电话簿”？查询动作通常是谁发起的？
4. 如果电脑能 ping 通 `8.8.8.8`，但打不开 `www.example.com`，你会优先怀疑什么？
5. 默认网关为什么像“出家门的大门”？
6. 家庭场景里，默认网关和 NAT 为什么通常在 CPE 上？
7. 普通桥接型 ONT 的主要职责是什么？
8. 光猫路由一体机为什么容易让人混淆 ONT 和 CPE 的职责？
9. BNG/BRAS 管的“用户会话”大概是什么意思？
10. 为什么不能说“每打开一个网站都要去 BNG/BRAS 重新认证一次”？

## Part 2：完成实验

完成 `Day002/lab.md`。

重点不是把命令背下来，而是看懂这条链：

```text
host1
  -> 默认网关 cpe
  -> cpe 做路由转发和 NAT
  -> isp 看到 cpe 的上游地址
```

请记录：

```text
host1 IP：
host1 默认网关：
cpe LAN 地址：
cpe WAN 地址：
isp 地址：
isp 抓包时看到的源地址：
DNS 查询是否成功：
```

如果实验环境暂时跑不起来，请至少阅读 lab 并手画拓扑。今天允许环境问题不阻塞学习。

## Part 3：重点复习

结合 Day001 的 Knowledge Review，今天重点复习这 3 点：

- Home Gateway vs ONT Role：默认网关/NAT/DNS relay 通常属于 CPE 功能；ONT 主要是光接入转换，除非是光猫路由一体机。
- DNS Resolution Path：终端发起 DNS 查询，可能先问 CPE，也可能问上游 DNS；DNS 失败不等于链路一定断。
- BNG/BRAS Session Timing：用户会话通常在上线阶段建立，访问不同网站时复用会话和策略。

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
- 动态出题，不使用固定题库
- 只有问答结束后才生成 oral exam 文件

本轮出题比例：

- 今天课程：40%
- 最近课程：20%
- 历史课程：20%
- Knowledge Review：20%

## Completion Checklist

- [ ] Lesson completed
- [ ] Lab read or completed
- [ ] Can explain DHCP/DNS/default gateway/NAT timing
- [ ] Can distinguish CPE and bridge ONT
- [ ] Can explain BNG/BRAS session timing
- [ ] Oral exam completed
- [ ] Journal completed
