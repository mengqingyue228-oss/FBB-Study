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
