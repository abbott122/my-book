
# Ethernet 与 ARP/RARP 

## 一、Ethernet（以太网）

以太网是局域网中最常见的数据链路层技术。
在 Linux 网络中，网卡接口（eth0, ens33, eno1 等）就是 Ethernet 设备。

1. Ethernet 帧结构

+-----------------+-----------------+-----------+---------+---------+
| 目标MAC地址(6B) | 源MAC地址(6B)   | 类型(2B)  | 数据    | CRC校验 |
+-----------------+-----------------+-----------+---------+---------+

	•	目标 MAC 地址：接收方硬件地址
	•	源 MAC 地址：发送方硬件地址
	•	类型字段：
	•	0x0800 → IPv4
	•	0x0806 → ARP
	•	0x8035 → RARP
	•	0x86DD → IPv6

2. Linux 下常用 Ethernet 管理命令
	•	查看网卡信息

ip link show
ethtool eth0


	•	修改网卡参数

ip link set eth0 up              # 启用网卡
ip link set eth0 down            # 禁用网卡
ip link set eth0 address XX:XX:XX:XX:XX:XX   # 修改MAC


	•	抓包查看 Ethernet 帧

tcpdump -i eth0 -nn -e



⸻

## 二、ARP（Address Resolution Protocol）

ARP 用于 已知 IP → 查询 MAC 地址。
在二层网络通信中，IP 报文必须先封装进 Ethernet 帧，因此主机需要知道对方的 MAC。

1. ARP 工作过程

举例：主机 A (192.168.1.10) 想访问 B (192.168.1.20)：
	1.	A 查 ARP 缓存表，若无记录则广播 ARP 请求。
	2.	B 收到广播后，回复自己的 MAC 地址（单播）。
	3.	A 将 IP 与 MAC 绑定关系存入 ARP 缓存表。

2. Linux 下的 ARP 管理
	•	查看 ARP 缓存表

ip neigh show
arp -n


	•	手动添加/删除 ARP

ip neigh add 192.168.1.20 lladdr 00:11:22:33:44:55 dev eth0
ip neigh del 192.168.1.20 dev eth0


	•	防止 ARP 欺骗
	•	使用静态 ARP 表：

arp -s 192.168.1.20 00:11:22:33:44:55


	•	开启 arp_ignore 和 arp_announce 优化内核参数。

3. 抓包分析 ARP

tcpdump -i eth0 arp


⸻

## 三、RARP（Reverse ARP，反向 ARP）
	•	作用：已知 MAC → 查询 IP 地址
	•	背景：主要用于早期的无盘工作站（diskless workstation）启动，通过 RARP 向服务器请求 IP。
	•	现状：现代网络已由 DHCP 替代 RARP，Linux 内核已基本不再使用 RARP。

Linux 下的 RARP
	•	老版本支持 rarp 命令，现在已废弃。
	•	取而代之的是：
	•	BOOTP
	•	DHCP

⸻

四、Ethernet & ARP 在 Linux 网络管理中的结合
	1.	以太网层 (MAC) → 定义数据帧传输方式。
	2.	ARP 层 → 建立 IP 和 MAC 的映射，保证三层 IP 协议能在二层传输。
	3.	RARP → 过时，但理解它有助于掌握网络发展史。

⸻

## 五、实验案例（Linux 环境）

实验1：手动配置并查看 ARP 表

ip addr add 192.168.1.10/24 dev eth0
ping -c 1 192.168.1.20
ip neigh show

实验2：静态绑定 ARP

arp -s 192.168.1.20 00:11:22:33:44:55
arp -n

实验3：抓取 ARP 数据包

tcpdump -i eth0 arp -n -e

