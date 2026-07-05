# Day003 Lab

Topic: 用 containerlab 观察 ARP、本地转发和默认网关

Status: Ready - Beginner Friendly

## 实验目标

今天继续使用 Docker + containerlab + Linux，并引入一个 FRRouting 容器作为网关角色。

实验只观察两件事：

1. 访问同一本地网段设备时，主机 ARP 查询的是对方设备。
2. 访问非本地网段设备时，主机 ARP 查询的是默认网关，不是远端服务器。

你不需要配置真实运营商设备，也不需要使用 eNSP。

## 实验拓扑

```text
同一链路观察：

pc1 192.168.30.10/24
  |
  | 本地链路 A
  |
pc2 192.168.30.20/24


默认网关观察：

pc1 192.168.40.10/24
  |
  | 本地链路 B
  |
gw  192.168.40.1/24
gw  203.0.113.1/24
  |
  | 上游链路
  |
srv 203.0.113.10/24
```

类比：

- `pc1`：你的电脑。
- `pc2`：同一个家庭网络里的另一台设备。
- `gw`：家庭 CPE/默认网关角色，这里用 FRRouting 容器承载 Linux 路由能力。
- `srv`：远端网络里的服务器。

## 实验环境

- Mac
- Docker Desktop
- containerlab
- Linux 容器镜像：`nicolaka/netshoot`
- FRRouting 容器镜像：`frrouting/frr:latest`

如果环境暂时没装好，今天可以先阅读拓扑和命令。重点是理解 ARP 目标是谁。

## 步骤 1：准备临时实验目录

不要在仓库里新增实验工程目录。请在临时目录操作：

```sh
mkdir -p /tmp/fbb-day003
cd /tmp/fbb-day003
```

创建 `topo.yml`：

```sh
cat > topo.yml <<'EOF'
name: fbb-day003

topology:
  nodes:
    pc1:
      kind: linux
      image: nicolaka/netshoot:latest
      exec:
        - ip addr add 192.168.30.10/24 dev eth1
        - ip addr add 192.168.40.10/24 dev eth2
        - ip route replace default via 192.168.40.1
    pc2:
      kind: linux
      image: nicolaka/netshoot:latest
      exec:
        - ip addr add 192.168.30.20/24 dev eth1
    gw:
      kind: linux
      image: frrouting/frr:latest
      exec:
        - ip addr add 192.168.40.1/24 dev eth1
        - ip addr add 203.0.113.1/24 dev eth2
        - sysctl -w net.ipv4.ip_forward=1
    srv:
      kind: linux
      image: nicolaka/netshoot:latest
      exec:
        - ip addr add 203.0.113.10/24 dev eth1
        - ip route replace default via 203.0.113.1
  links:
    - endpoints: ["pc1:eth1", "pc2:eth1"]
    - endpoints: ["pc1:eth2", "gw:eth1"]
    - endpoints: ["gw:eth2", "srv:eth1"]
EOF
```

说明：`gw` 使用 FRRouting 镜像，但今天不启用动态路由协议；先用 Linux 静态地址和转发观察基础 ARP 行为。

## 步骤 2：启动实验

```sh
sudo containerlab deploy -t topo.yml
```

如果你的 Mac 不需要 `sudo`，可以去掉。

## 步骤 3：查看 pc1 的地址和路由

进入 `pc1`：

```sh
docker exec -it clab-fbb-day003-pc1 bash
```

查看地址：

```sh
ip addr show eth1
ip addr show eth2
ip route
```

你应该看到类似：

```text
192.168.30.10/24 dev eth1
192.168.40.10/24 dev eth2
default via 192.168.40.1
```

理解：

- `192.168.30.0/24` 是 pc1 和 pc2 的本地链路。
- `192.168.40.0/24` 是 pc1 和 gw 的本地链路。
- 默认网关是 `192.168.40.1`，也就是 gw。

## 步骤 4：清空并观察 ARP 缓存

在 `pc1` 内执行：

```sh
ip neigh flush all
ip neigh
```

如果没有输出，表示当前 ARP/邻居缓存基本为空。

## 步骤 5：访问同一本地网段的 pc2

在 `pc1` 内执行：

```sh
ping -c 2 192.168.30.20
ip neigh
```

你应该看到类似：

```text
192.168.30.20 dev eth1 lladdr xx:xx:xx:xx:xx:xx REACHABLE
```

理解：

```text
pc1 访问 192.168.30.20
目标在本地网段
pc1 ARP 查询 pc2 的 MAC
以太网帧目的 MAC 是 pc2 的 MAC
```

## 步骤 6：抓包看 ARP 请求

重新清空缓存：

```sh
ip neigh flush all
```

打开另一个终端抓包：

```sh
docker exec -it clab-fbb-day003-pc1 tcpdump -ni eth1 arp
```

回到 `pc1` 终端执行：

```sh
ping -c 1 192.168.30.20
```

你应该看到类似含义的包：

```text
ARP, Request who-has 192.168.30.20 tell 192.168.30.10
ARP, Reply 192.168.30.20 is-at xx:xx:xx:xx:xx:xx
```

理解：

访问同一本地网段设备时，ARP 问的是目标设备 `pc2`。

## 步骤 7：访问远端服务器 srv

在 `pc1` 内执行：

```sh
ip neigh flush all
ping -c 2 203.0.113.10
ip neigh
```

你应该看到类似：

```text
192.168.40.1 dev eth2 lladdr yy:yy:yy:yy:yy:yy REACHABLE
```

注意：这里缓存里出现的是 `192.168.40.1`，也就是默认网关，不是 `203.0.113.10`。

理解：

```text
pc1 访问 203.0.113.10
目标不在 pc1 的本地网段
pc1 按默认路由交给 192.168.40.1
pc1 ARP 查询默认网关 gw 的 MAC
以太网帧目的 MAC 是 gw 的 MAC
IP 包目的 IP 仍是 203.0.113.10
```

## 步骤 8：抓包看“外网访问时 ARP 问的是网关”

重新清空缓存：

```sh
ip neigh flush all
```

抓 `pc1` 到 `gw` 的链路：

```sh
docker exec -it clab-fbb-day003-pc1 tcpdump -ni eth2 arp
```

回到 `pc1` 终端执行：

```sh
ping -c 1 203.0.113.10
```

你应该看到类似含义：

```text
ARP, Request who-has 192.168.40.1 tell 192.168.40.10
ARP, Reply 192.168.40.1 is-at yy:yy:yy:yy:yy:yy
```

这就是今天的核心观察。

## 步骤 9：在网关上看 Linux 路由转发

进入 `gw`：

```sh
docker exec -it clab-fbb-day003-gw sh
```

查看接口和路由：

```sh
ip addr
ip route
sysctl net.ipv4.ip_forward
```

参考理解：

```text
gw 在 192.168.40.0/24 和 203.0.113.0/24 两个网段之间转发。
pc1 不需要知道 srv 的 MAC。
pc1 只需要知道本地第一跳 gw 的 MAC。
```

## 步骤 10：清理实验

退出容器后运行：

```sh
cd /tmp/fbb-day003
sudo containerlab destroy -t topo.yml
```

## 实验完成标准

你不需要把实验结果交给 AI 自动评分。

你只需要能用自己的话解释：

1. 为什么 `pc1` 访问 `192.168.30.20` 时，ARP 查询的是 `pc2`。
2. 为什么 `pc1` 访问 `203.0.113.10` 时，ARP 查询的是 `192.168.40.1`。
3. 为什么“DNS 查询网站 IP”和“ARP 查询下一跳 MAC”不是一回事。
4. 为什么访问外网时，以太网帧目的 MAC 可以是网关 MAC，但 IP 包目的 IP 仍是远端服务器 IP。
5. 华为排障里为什么会同时看 `display arp`、`display mac-address`、`display ip routing-table`。

## Lab Notes

由学习者填写。
