# Day003 Lab

Topic: 用 Linux 路由观察“IP 承载”的最小模型

Status: Ready - Beginner Friendly

## 实验目标

今天不模拟完整运营商网络，只搭一个最小模型：

```text
用户侧主机 -> 路由器 -> 远端服务器
```

你要观察：

1. 主机访问远端地址时，会先交给下一跳路由器。
2. 路由器根据路由表继续转发。
3. “IP 承载”的基础动作就是一跳一跳查表转发。

## 实验拓扑

```text
pc1 192.168.10.10/24
  |
  | 用户侧链路
  |
r1  192.168.10.1/24
r1  203.0.113.1/24
  |
  | 承载侧/远端链路
  |
srv 203.0.113.10/24
```

类比：

- `pc1`：用户电脑。
- `r1`：运营商 IP 承载网中的一个路由节点。今天用 Linux 模拟。
- `srv`：远端内容服务器。

## 实验环境

- Mac
- Docker Desktop
- containerlab
- Linux 容器镜像：`nicolaka/netshoot`

如果环境暂时没装好，先读拓扑和命令也可以。今天重点是理解“下一跳”和“路由表”。

## 步骤 1：准备临时目录

不要在仓库里新增实验工程目录。请在临时目录操作：

```sh
mkdir -p /tmp/fbb-day003-ip-bearer
cd /tmp/fbb-day003-ip-bearer
```

创建 `topo.yml`：

```sh
cat > topo.yml <<'EOF'
name: fbb-day003-ip-bearer

topology:
  nodes:
    pc1:
      kind: linux
      image: nicolaka/netshoot:latest
      exec:
        - ip addr add 192.168.10.10/24 dev eth1
        - ip route replace default via 192.168.10.1
    r1:
      kind: linux
      image: nicolaka/netshoot:latest
      exec:
        - ip addr add 192.168.10.1/24 dev eth1
        - ip addr add 203.0.113.1/24 dev eth2
        - sysctl -w net.ipv4.ip_forward=1
    srv:
      kind: linux
      image: nicolaka/netshoot:latest
      exec:
        - ip addr add 203.0.113.10/24 dev eth1
        - ip route replace default via 203.0.113.1
  links:
    - endpoints: ["pc1:eth1", "r1:eth1"]
    - endpoints: ["r1:eth2", "srv:eth1"]
EOF
```

## 步骤 2：启动实验

```sh
sudo containerlab deploy -t topo.yml
```

如果你的 Mac 不需要 `sudo`，可以去掉。

## 步骤 3：在 pc1 上看默认路由

```sh
docker exec -it clab-fbb-day003-ip-bearer-pc1 bash
ip addr
ip route
```

你应该看到类似：

```text
default via 192.168.10.1
192.168.10.0/24 dev eth1
```

理解：

```text
pc1 不知道所有远端网络怎么走。
pc1 只知道：不是本地网段的流量，先交给 192.168.10.1。
```

## 步骤 4：访问远端服务器

在 `pc1` 里执行：

```sh
ping -c 3 203.0.113.10
traceroute -n 203.0.113.10
```

你应该看到路径经过 `192.168.10.1`。

理解：

```text
pc1 -> r1 -> srv
```

这就是最小的“承载”动作：用户流量被路由节点继续送向远端。

## 步骤 5：在 r1 上看路由和转发开关

打开另一个终端：

```sh
docker exec -it clab-fbb-day003-ip-bearer-r1 bash
ip addr
ip route
sysctl net.ipv4.ip_forward
```

你应该看到：

```text
192.168.10.0/24 dev eth1
203.0.113.0/24 dev eth2
net.ipv4.ip_forward = 1
```

理解：

- `r1` 同时连接两个网段。
- `ip_forward = 1` 表示 Linux 允许它像路由器一样转发 IP 包。
- 真正运营商设备会用专业路由器和更复杂的协议，但基本逻辑仍然是查路由表转发。

## 步骤 6：抓包观察 IP 包经过 r1

在 `r1` 上执行：

```sh
tcpdump -ni eth1 icmp
```

回到 `pc1` 执行：

```sh
ping -c 1 203.0.113.10
```

你会看到 ICMP 请求从用户侧进来。

再在 `r1` 上抓承载侧接口：

```sh
tcpdump -ni eth2 icmp
```

再次从 `pc1` 执行：

```sh
ping -c 1 203.0.113.10
```

你会看到同一个访问继续从 `r1` 的另一侧发出去。

## 步骤 7：故意关掉转发，观察故障

在 `r1` 上执行：

```sh
sysctl -w net.ipv4.ip_forward=0
```

回到 `pc1`：

```sh
ping -c 3 203.0.113.10
```

现象：

```text
pc1 可能还能到达 192.168.10.1，
但到不了 203.0.113.10。
```

工程意义：

用户可能说“网关能 ping 通，但网站访问失败”。这时问题可能不在家庭 Wi-Fi，而在后续路由、策略、会话或承载路径。

恢复：

```sh
sysctl -w net.ipv4.ip_forward=1
```

## 步骤 8：清理实验

退出容器后运行：

```sh
cd /tmp/fbb-day003-ip-bearer
sudo containerlab destroy -t topo.yml
```

## 实验完成标准

你不需要把实验结果交给 AI 自动评分。

你只需要能用自己的话解释：

1. `pc1` 为什么把远端流量先交给 `192.168.10.1`。
2. `r1` 为什么能在两个网段之间转发。
3. 路由表在这个过程中起什么作用。
4. 为什么“能到默认网关”不等于“能访问远端网站”。
5. 这个最小拓扑和运营商 IP 承载网有什么相似点，哪里又被简化了。

## Lab Notes

由学习者填写。
