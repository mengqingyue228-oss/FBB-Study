# PROJECT_GUIDE.md

# FBB-Study Project Guide (V1.0)

Version: 1.0

---

# 一、项目简介

FBB-Study（Fixed Broadband Study）是一套由 AI 辅助完成的长期固网学习项目。

本项目不是学习平台，也不是软件项目。

目标只有一个：

> 帮助一位具有软件背景、没有通信网络基础的学习者，在 120 天左右系统掌握运营商固网知识，并最终具备独立分析、设计、排障和自动化运维固网的能力。

本项目坚持：

* 简单
* 长期可维护
* 低维护成本
* 不增加额外平台

整个项目仅依赖：

* ChatGPT / Codex
* GitHub
* 本地 Mac
* 邮件

除此之外，不增加任何系统。

---

# 二、学习目标

整个学习路线遵循：

**先广度，再深度。**

先建立完整知识体系，再逐步深入。

重点学习：

* AI 时代长期有价值的知识
* 网络原理
* 固网架构
* 运营商工程实践
* 自动化能力

不花大量时间学习已经逐渐淘汰的技术。

---

# 三、学习背景

学习者：

* 软件工程背景
* 几乎没有通信网络基础
* 希望未来能够成为运营商固网专家
* 工作中主要接触华为设备

因此：

课程设计遵循：

理论：

遵循 RFC 和标准协议。

实践：

优先采用华为运营商场景。

配置：

优先采用华为 VRP 风格。

但是：

不要依赖 eNSP。

实验统一采用现代虚拟化环境。

---

# 四、实验环境

实验平台：

Mac

Git

VS Code

Wireshark

Docker Desktop

containerlab

FRRouting

draw.io

以后课程中的所有实验都基于上述环境。

---

# 五、仓库目录

仓库保持尽可能简单。

```
FBB-Study

README.md

PROJECT_GUIDE.md

Roadmap.md

Progress.md

Mistakes.md

Day001/

lesson.md

lab.md

homework.md

journal.md
```

以后：

Day002

Day003

......

保持相同结构。

不要增加其它目录。

不要修改目录结构。

---

# 六、文件说明

README.md

项目首页。

展示：

* 当前学习阶段
* 当前主题
* 当前学习进度
* 最近一次学习
* 最近一次问答成绩
* 下一步计划

不要放课程内容。

---

Roadmap.md

120 天学习路线。

这里只维护学习路线。

不要维护课程内容。

---

Progress.md

维护：

Current Day

Current Topic

Current Stage

Status

Completed Days

Skipped Days

Latest Oral Score

Next Topic

这里只维护状态。

---

Mistakes.md

维护：

Knowledge Mastery。

不是传统错题本。

每条记录包括：

Knowledge Point

Wrong Count

Correct Count

Consecutive Correct

Mastery Level

Status

Next Review

这里记录的是：

知识点。

不是题目。

例如：

ARP Broadcast Domain

而不是：

为什么 ARP 不跨三层。

---

DayXXX/

每一天固定包含：

lesson.md

lab.md

homework.md

journal.md

oral_exam 文件不要预创建。

完成问答以后生成。

例如：

oral_exam_001_2026-07-03.md

oral_exam_002_2026-07-03.md

允许一天进行多轮问答。

---

# 七、课程设计原则

课程必须：

循序渐进。

由浅入深。

第一阶段：

建立整体知识体系。

第二阶段：

逐步深入。

课程优先讲：

为什么。

而不是：

如何配置。

所有知识都要解释：

原理。

工程意义。

运营商中的实际应用。

课程设计优先考虑：

AI 时代长期有价值的能力。

减少已经逐渐淘汰内容的学习时间。

## 当前课程主线

整体课程顺序调整为：

1. IP 承载网
2. 光传送网
3. 光接入网

讲每一个部分时：

如果涉及其他部分知识，只做必要铺垫。

例如：

讲 IP 承载网时，可以说“底层可能跑在光传送网络上”，但不要展开 OTN。

讲光传送网时，可以说“上层承载 IP 业务”，但不要展开 BNG/BRAS。

讲光接入网时，可以说“流量最终进入 IP 承载网”，但不要把 IP 路由重新讲一遍。

专项内容到对应阶段再完整打开。

## 小白课程规则

课程起步必须把学习者当作完全不懂网络的小白。

所有专业名词、英文缩写和协议名都要：

1. 先用通俗类比解释。
2. 再给正式含义。
3. 再给一个家庭或运营商案例。
4. 最后说明工程意义。

例如：

CPE：

先解释为“用户家里的网络入口设备”。

再说明 Customer Premises Equipment。

再举家庭路由器、光猫路由一体机、企业专线 CPE 等例子。

## 每日主题规则

每天只打开一个主主题。

不要把几个概念来回讲。

前面错题或低分知识点只放在短小的 Knowledge Review 里复习。

除非学习者明显卡住，否则不要让错题复习覆盖当天新主题。

每天课程必须和前一天不一样。

## 协议讲解规则

涉及协议时，不能只讲概念。

至少说明：

* 这个协议解决什么问题
* 协议发生在什么阶段
* 谁向谁发起
* 关键报文或关键字段是什么
* 成功以后网络状态发生什么变化
* 失败时用户或工程师看到什么现象

例如：

讲 DHCP 时，要讲 Discover、Offer、Request、Ack。

讲 PPPoE 时，要讲发现阶段、会话阶段和认证关系。

讲 OSPF 时，要讲邻居、LSA、路由计算的基本含义。

## 设备与物理工程规则

涉及物理层面或真实网络设备时，不能只讲泛泛概念。

必须说明：

* 设备具体叫什么
* 常见类别
* 常见部署位置
* 在运营商网络中的作用
* 可以举华为设备族作为例子，但不要变成产品广告

例如：

家庭网络：

桥接 ONT、路由型 ONT、Wi-Fi ONT、独立家庭路由器、Mesh 路由器、企业 CPE。

IP 承载：

接入交换机、汇聚路由器、核心路由器、BNG/BRAS、业务边界路由器，可举华为 NetEngine 系列作为运营商路由器例子。

光传送：

OTN、WDM、ROADM、光放大器、站点机柜、尾纤、ODF，可举华为 OptiX OSN/OptiXtrans 场景作为例子。

光接入：

OLT、ODN、分光器、接头盒、光交箱、分纤箱、入户皮线光缆、ONT/CPE，可举华为 MA5800 OLT、EchoLife/OptiXstar ONT/CPE 场景作为例子。

## 工程场景规则

课程不能只瞄准运维。

不同阶段都要穿插：

* 规划
* 建设
* 开通
* 维护
* 运营
* 优化
* 营销

例如：

讲接入网时，除了 OLT/ONT 协议，还要讲 ODN、分光器、接头盒、光缆敷设、标签、验收、OTDR 测试、装维、套餐带宽体验和覆盖营销。

讲 IP 承载网时，除了路由协议，还要讲地址规划、带宽规划、BRAS/BNG 用户容量、故障域、扩容、QoS、业务开通和用户体验指标。

---

# 八、华为原则

课程默认：

理论：

标准协议。

实践：

华为运营商场景。

配置：

华为 VRP 风格。

排障：

华为 display 思路。

但是：

实验平台：

统一采用：

containerlab

FRRouting

Linux

不要采用 eNSP。

---

# 九、实验设计

每天都有一个 Lab。

Lab 不要求 AI 自动检查。

Lab 只需要：

说明：

实验目标。

实验步骤。

最终应该达到什么效果。

必要时提供：

参考输出。

学习者自行完成实验。

---

# 十、作业设计

每天包含两类作业。

第一类：

问答。

第二类：

实验。

---

## 问答

问答采用对话形式。

不是 Markdown。

不是选择题。

不是填空题。

学习者完成课程以后：

通过 AI 开始问答。

每轮固定：

10 题。

---

题目组成：

40%

今天课程

20%

最近课程

20%

历史课程

20%

Knowledge Review

---

题目必须动态生成。

不要机械重复。

例如：

第一次：

为什么 ARP 不跨三层？

第二次：

路由器为什么不转发 ARP？

第三次：

ARP 广播可以经过网关吗？

虽然问题不同。

但是：

考察的是：

同一个知识点。

---

每题：

10 分。

总分：

100 分。

问答结束以后：

生成：

oral_exam_xxx.md

记录：

问题

学习者回答

点评

评分

是否加入 Knowledge Review

总评

建议

一天允许多次问答。

文件编号：

001

002

003

......

---

## 实验

实验不评分。

AI 不检查配置。

实验只要求：

达到目标。

例如：

完成：

OSPF 邻居建立。

完成：

VLAN 隔离。

完成：

Wireshark 抓到 ARP。

即可。

---

# 十一、Journal

Journal 由学习者填写。

AI 不自动生成。

可以提供模板。

例如：

今天最大的收获：

今天最大的疑问：

今天一句总结：

今天学习时长：

---

# 十二、Knowledge Review

这里维护：

知识点。

不是：

题目。

例如：

Knowledge：

ARP Broadcast Domain

Wrong：

3

Correct：

5

Consecutive Correct：

2

Mastery：

72%

Status：

Learning

Next Review：

After Day022

以后：

抽查题围绕：

Knowledge Point

动态生成。

不是：

围绕：

题目。

---

# 十三、学习推进

学习者以后不会说：

Day018 完成。

统一采用自然语言。

例如：

今天已完成

今天没空

开始问答

实验完成

作业完成

AI 根据：

Progress.md

自动维护学习状态。

学习者不需要记住：

Day 编号。

如果：

今天没空。

整体顺延一天。

不要跳课。

---

# 十四、Git 规范

Commit Message 推荐：

Initialize Project

Day001 Materials

Day001 Oral Exam #001

Day001 Journal

Day001 Completed

保持统一风格。

---

# 十五、Codex 的职责

Codex 是项目维护者。

负责：

初始化仓库。

创建目录。

维护 Markdown。

更新 README。

更新 Progress。

更新 Mistakes。

创建新的 DayXXX。

维护 Git。

Commit。

Push。

Codex 不负责：

设计课程。

修改学习路线。

增加学习内容。

调整教学策略。

这些由 AI 导师负责。

---

# 十六、初始化任务

请完成以下工作：

1. 初始化整个仓库。

2. 创建所有基础文件。

3. 初始化 README。

4. 初始化 Roadmap。

5. 初始化 Progress。

6. 初始化 Mistakes。

7. 创建 Day001。

8. 为 lesson、lab、homework、journal 创建模板。

9. 保持目录结构固定。

10. 初始化完成后提交 Git。

---

# 十七、最高原则

本项目的目标：

永远只有一个。

帮助学习者真正掌握固网。

任何新增功能。

都必须满足：

不增加新的平台。

不增加新的数据库。

不增加新的维护成本。

如果某个设计会让系统变复杂。

不要实现。

保持：

简单。

稳定。

长期可维护。

如果课程设计与上述原则冲突，以帮助学习者长期坚持学习为最高优先级。
