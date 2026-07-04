# Day002 Lab

Topic: 用 containerlab 观察默认网关、DNS、NAT 与上游路由的分工

Status: Ready - Beginner Friendly

## 实验目标

今天开始引入 Docker + containerlab + Linux 网络观察。

你不需要配置真实运营商设备，也不需要使用 eNSP。这个实验只做一件事：用一个很小的虚拟家庭网络，观察“终端 -> CPE -> ISP”路径中每个节点的职责。

完成后你应该能解释：

- 终端的默认网关是谁。
- DNS 查询是终端发起的，不是 ISP 路由器主动替终端查。
- CPE 开启转发和 NAT 后，家庭终端才能通过它访问上游。
- 上游 ISP 节点看到的是 CPE 的上游地址，而不是家庭终端的私网地址。

## 实验拓扑

```text
host1
  192.168.10.10/24
  default gateway: 192.168.10.1
    |
    | 家庭 LAN
    |
cpe
  LAN: 192.168.10.1/24
  WAN: 100.64.0.2/30
  NAT: enabled
    |
    | 上游链路
    |
isp
  100.64.0.1/30
```

类比：

- `host1`：你的 Mac 或手机。
- `cpe`：家用路由器，负责默认网关和 NAT。
- `isp`：运营商侧的上游节点，今天只模拟“更上游的网络”。

## 实验环境

- Mac
- Docker Desktop
- containerlab
- Linux 容器镜像：`nicolaka/netshoot`

如果 containerlab 还没有安装，今天先阅读步骤即可，不强制补装。后续课程会继续使用。

## 步骤 1：准备临时实验目录

不要在仓库里新增实验工程目录。请在临时目录操作：

```sh
mkdir -p /tmp/fbb-day002
cd /tmp/fbb-day002
```

创建 `topo.yml`：

```sh
cat > topo.yml <<'EOF'
name: fbb-day002

topology:
  nodes:
    host1:
      kind: linux
      image: nicolaka/netshoot:latest
      exec:
        - ip addr add 192.168.10.10/24 dev eth1
        - ip route replace default via 192.168.10.1
        - sh -c "echo 'nameserver 1.1.1.1' > /etc/resolv.conf"
    cpe:
      kind: linux
      image: nicolaka/netshoot:latest
      exec:
        - ip addr add 192.168.10.1/24 dev eth1
        - ip addr add 100.64.0.2/30 dev eth2
        - sysctl -w net.ipv4.ip_forward=1
        - iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -o eth2 -j MASQUERADE
        - ip route replace default via 100.64.0.1
    isp:
      kind: linux
      image: nicolaka/netshoot:latest
      exec:
        - ip addr add 100.64.0.1/30 dev eth1
        - sysctl -w net.ipv4.ip_forward=1
  links:
    - endpoints: ["host1:eth1", "cpe:eth1"]
    - endpoints: ["cpe:eth2", "isp:eth1"]
EOF
```

说明：这里使用临时文件是为了运行实验，不是给仓库增加新目录或脚本平台。

## 步骤 2：启动实验

```sh
sudo containerlab deploy -t topo.yml
```

如果你的 Mac 不需要 `sudo`，可以去掉。

## 步骤 3：观察终端的本地配置

进入 `host1`：

```sh
docker exec -it clab-fbb-day002-host1 bash
```

查看地址和路由：

```sh
ip addr show eth1
ip route
cat /etc/resolv.conf
```

你应该看到类似：

```text
192.168.10.10/24 dev eth1
default via 192.168.10.1
nameserver 1.1.1.1
```

理解：

- `192.168.10.10` 是终端自己的家庭侧地址。
- `192.168.10.1` 是默认网关，也就是 CPE。
- DNS 配置在终端上，终端会按这个配置发起 DNS 查询。

## 步骤 4：验证 host1 到默认网关

在 `host1` 内运行：

```sh
ping -c 3 192.168.10.1
```

参考结果：

```text
3 packets transmitted, 3 received, 0% packet loss
```

理解：

能 ping 通默认网关，只能说明 `host1 -> cpe` 这段正常；不能直接证明互联网一定正常。

## 步骤 5：验证 host1 到上游 ISP 节点

在 `host1` 内运行：

```sh
ping -c 3 100.64.0.1
```

如果成功，说明：

```text
host1 -> cpe -> isp
```

这条路径基本通了。

如果失败，优先检查：

- `host1` 默认路由是不是指向 `192.168.10.1`
- `cpe` 是否开启 `net.ipv4.ip_forward=1`
- `cpe` 到 `isp` 的 `100.64.0.0/30` 链路是否正常

## 步骤 6：在 CPE 上观察 NAT 规则

打开另一个终端：

```sh
docker exec -it clab-fbb-day002-cpe bash
```

查看 NAT 表：

```sh
iptables -t nat -L -n -v
```

参考输出里应该能看到类似：

```text
MASQUERADE  all  --  192.168.10.0/24  0.0.0.0/0
```

理解：

这表示家庭网段 `192.168.10.0/24` 出 `cpe` 上游口时，会被转换成 `cpe` 的上游地址。

## 步骤 7：抓包观察上游看到的源地址

在 `isp` 容器里抓包：

```sh
docker exec -it clab-fbb-day002-isp tcpdump -ni eth1 icmp
```

同时在 `host1` 里 ping：

```sh
ping -c 3 100.64.0.1
```

你在 `isp` 上可能看到类似：

```text
IP 100.64.0.2 > 100.64.0.1: ICMP echo request
```

注意这里源地址是 `100.64.0.2`，不是 `192.168.10.10`。

理解：

这就是 NAT 的工程效果：上游看到的是 CPE 的上游地址，家庭终端私网地址被隐藏在家庭网络内部。

## 步骤 8：观察 DNS 查询由谁发起

在 `host1` 内运行：

```sh
dig www.example.com
```

你会看到 DNS 查询结果。如果网络不能访问公网，也没关系；今天重点不是公网是否通，而是理解：

```text
DNS 查询动作由 host1 根据 /etc/resolv.conf 发起。
CPE/ONT/ISP 节点不会凭空知道你要访问哪个域名。
```

如果要观察 DNS 包，可以在 `cpe` 上抓：

```sh
tcpdump -ni eth1 port 53
```

然后在 `host1` 上再次执行：

```sh
dig www.example.com
```

## 步骤 9：清理实验

退出容器后运行：

```sh
cd /tmp/fbb-day002
sudo containerlab destroy -t topo.yml
```

## 实验完成标准

你不需要把实验结果交给 AI 自动评分。

你只需要能用自己的话解释：

1. `host1` 的默认网关为什么是 `cpe`。
2. `cpe` 为什么需要开启 IP 转发。
3. NAT 以后，`isp` 为什么看到 `100.64.0.2` 而不是 `192.168.10.10`。
4. DNS 查询为什么是终端根据 DNS 配置发起。
5. 这个实验里的 `cpe` 和真实家庭里的桥接型 ONT 有什么区别。

## Lab Notes

由学习者填写。
