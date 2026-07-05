# Day003 Homework

Topic: IP 承载网入门

Status: Ready - Beginner Friendly

## Part 1：先用自己的话复述

请不要背定义。用自己的话回答：

1. 为什么说 IP 承载网像运营商内部的“数据公路网”？
2. “业务”和“承载”有什么区别？请用电商和快递的例子解释。
3. IP 包里今天最需要先关注哪两个地址？
4. 路由器收到一个 IP 包后，为什么要查路由表？
5. 下一跳是什么意思？它为什么不一定是最终目标？
6. BNG/BRAS 在家庭宽带用户进入运营商网络时大概做什么？
7. 汇聚路由器和核心路由器的角色有什么不同？
8. IP 承载网和光传送网、光接入网分别解决什么问题？
9. 为什么规划 IP 承载网时要考虑用户规模、带宽、地址和故障域？
10. 如果用户能 ping 通家庭网关，但访问远端服务器失败，为什么问题可能出现在后续承载路径？

## Part 2：完成实验

完成 `Day003/lab.md`。

记录：

```text
pc1 IP：
pc1 默认网关：
r1 用户侧 IP：
r1 远端侧 IP：
srv IP：
pc1 的默认路由：
r1 的路由表里有哪些直连网段：
关掉 ip_forward 后的现象：
一句话解释 IP 承载：
```

如果实验环境暂时跑不起来，请至少阅读 lab 并手画拓扑。

## Part 3：Knowledge Review

结合 Day002 的 Knowledge Review，只复习下面 3 点：

- DNS Resolution Path：DNS 查域名对应 IP，不负责决定 IP 承载网路径。
- Home Gateway vs ONT Role：家庭默认网关/NAT 可能在 CPE 或路由型 ONT；桥接 ONT 主要做光接入转换。
- BNG/BRAS Session Timing：用户认证和会话通常在接入阶段建立，不是每访问一个网站都重新认证。

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
- [ ] Can explain IP bearer network in plain language
- [ ] Can explain packet, route, next hop, and router
- [ ] Can describe BNG/BRAS as a broadband service edge
- [ ] Can distinguish IP bearer, optical transport, and optical access at a high level
- [ ] Oral exam completed
- [ ] Journal completed
