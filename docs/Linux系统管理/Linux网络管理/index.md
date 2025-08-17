
Linux 网络通过强大的管理工具实现跨平台的系统连接和资源共享。网络配置存储在 /etc/network/interfaces 中。关键命令包括用于接口管理的 ifconfig （已弃用）和 ip 。支持各种协议，并具有出色的可扩展性。这对于系统连接和网络故障排除至关重要。Linux 采用基于文件的网络配置方法，将网络相关设置和配置存储在标准文件中，例如 /etc/network/interfaces 或 /etc/sysconfig/network-scripts/ ，具体取决于 Linux 发行版。



## 1. TCP/IP Stack
[TCP/IP](../Linux网络管理/协议栈/index.md)

## 2. subnetting 

子网划分将网络划分为更小的子网，以提高 Linux 网络的性能和安全性。它按照 IP 寻址方案组织 IP 地址，从而防止冲突并高效利用地址范围。使用 `route -n` 查看路由表， `route add -net xxx.xxx.xxx.x/xx gw yyy.yyy.yyy.y` 添加子网。这对于复杂的网络环境至关重要。

[字网](../Linux网络管理/子网/index.md)
## 3. Ethernet & arp/rarp


## 4. DHCP 

## 5. IP  Routing

## 6. DNS  Resolution

## 7. Netfilter 

## 8. SSH

## 9. File Transfer