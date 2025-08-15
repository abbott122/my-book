#### 文件系统类型

| 文件系统     | 简介                                                         | 优点                                                         | 缺点                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **FAT32**    | 文件分配表（File Allocation Table）是一种较老的文件系统，广泛用于USB驱动器、SD卡等可移动存储设备。支持最大分区4TB，最大单文件4GB。 | - 跨平台兼容性强（Windows、Linux、macOS均支持）。 - 简单、轻量，适合小型存储设备。 - 低资源占用，适合嵌入式设备。 | - 单文件大小限制为4GB。 - 不支持权限管理，安全性低。 - 缺乏日志功能，断电易导致数据丢失。 - 性能较低，碎片化问题严重。 |
| **NTFS**     | 新技术文件系统（New Technology File System）是Windows默认文件系统，支持大容量存储和高级功能。 | - 支持大文件和分区（理论上16EB）。 - 日志功能，增强数据恢复能力。 - 支持文件权限、加密和压缩。 - 适合高性能和大型存储设备。 | - 非Windows系统兼容性较差（macOS只读，Linux需额外驱动）。 - 资源占用较高，不适合小型设备。 - 复杂性增加维护成本。 |
| **exFAT**    | 扩展文件分配表（Extended File Allocation Table）是为闪存设备设计的轻量文件系统，弥补FAT32单文件大小限制。 | - 支持超大文件和分区（理论上16EB）。 - 跨平台兼容性较好（Windows、macOS，Linux需支持）。 - 轻量，适合USB驱动器和SD卡。 | - 无日志功能，可靠性低于NTFS。 - 不支持权限管理和加密。 - 某些旧设备可能不支持。 |
| **ext4**     | 第四扩展文件系统（Extended File System 4）是Linux默认文件系统，广泛用于Linux服务器和桌面。 | - 高性能，适合大型文件和分区（最大1EB）。 - 支持日志，数据可靠性高。 - 支持文件权限和快速符号链接。 - 碎片化程度低，维护成本低。 | - Windows和macOS兼容性差（需第三方工具）。 - 对闪存设备支持不如exFAT。 - 不支持原生加密（需额外配置）。 |
| **Btrfs**    | B树文件系统（B-tree File System）是Linux的下一代文件系统，设计目标是取代ext4，提供高级功能。 | - 支持快照、数据压缩和子卷管理。 - 动态分配存储空间，灵活性高。 - 内置RAID支持，数据可靠性强。 - 最大支持16EB分区。 | - 性能可能不如ext4（尤其在高负载下）。 - 复杂性高，维护难度大。 - Windows和macOS不支持。 - 某些功能仍不稳定。 |
| **ZFS**      | Z文件系统（Zettabyte File System）是为企业级存储设计的高级文件系统，最初由Sun Microsystems开发。 | - 数据完整性校验，防止数据损坏。 - 支持快照、克隆和数据压缩。 - 内置RAID和动态扩展功能。 - 最大支持16EB，适合大规模存储。 | - 资源占用极高，不适合低端设备。 - 非Linux系统支持有限（Windows/macOS需额外配置）。 - 配置复杂，需专业知识。 - 许可证问题限制某些场景使用。 |
| **APFS**     | 苹果文件系统（Apple File System）是macOS、iOS等苹果设备的默认文件系统，优化了闪存和SSD存储。 | - 优化闪存/SSD性能，延迟低。 - 支持快照、加密和空间共享。 - 文件克隆功能节省存储空间。 - 高可靠性，适合移动设备。 | - 仅限苹果生态，Windows/Linux兼容性差。 - 对机械硬盘支持较弱。 - 复杂功能可能增加维护成本。 |
| **HFS+**     | 分层文件系统（Hierarchical File System Plus）是macOS的老文件系统，现已被APFS取代。 | - 与旧macOS设备兼容性好。 - 支持日志功能，数据可靠性较高。 - 简单易用，适合小型存储。 | - 性能不如APFS，延迟较高。 - 最大文件/分区大小限制（8EB）。 - Windows/Linux支持差（需第三方工具）。 - 已逐渐被淘汰。 |
| **XFS**      | X文件系统（X File System）是为高性能存储设计的文件系统，广泛用于Linux企业环境。 | - 极高性能，适合大文件和流媒体。 - 支持大分区（8EB）和动态扩展。 - 低延迟，适合高吞吐量场景。 - 日志功能增强可靠性。 | - 不适合小文件操作，性能较差。 - Windows/macOS不支持。 - 元数据操作（如删除大量文件）较慢。 - 恢复复杂，需专业工具。 |
| **ReiserFS** | 由Hans Reiser开发的文件系统，早期用于Linux，注重小文件性能。 | - 小文件操作性能优异。 - 支持动态扩展和日志功能。 - 存储效率高，适合小型存储。 | - 维护和开发已停止，逐渐淘汰。 - 可靠性低于ext4或XFS。 - Windows/macOS不支持。 - 不适合现代大容量存储。 |

##### 选择建议

- **桌面用户**：ext4（稳定、简单）或F2FS（SSD优化）。
- **服务器/NAS**：Btrfs（快照、RAID）或ZFS（高可靠性）。
- **大文件存储**：XFS（高性能）或ZFS（数据完整性）。
- **可移动存储**：exFAT（跨平台）或ext4（Linux专用）。
- **嵌入式/移动设备**：F2FS（闪存优化）或ext4（通用）。

在Linux系统中，如何有效地对磁盘存储空间加以使用和管理，是一项非常重要的技术，本章节从四个方面介绍磁盘的使用和管理技巧，首先讲述关于磁盘管理的基本概念以及如何合理划分磁盘分区

#### **磁盘的介绍**

磁盘（Disk）是计算机存储设备的核心组件，用于长期存储数据。以下是对磁盘的全面介绍，包括定义、类型、工作原理、优缺点及常见文件系统的适配性，结合您之前对Linux文件系统的关注，特别强调Linux环境下的适用性。内容以清晰的结构化形式呈现，包含表格总结关键信息。

##### 磁盘的定义
磁盘是一种非易失性存储设备，用于存储操作系统、应用程序和用户数据。它通过磁性、光学或固态存储技术记录数据，通常分为机械硬盘（HDD）、固态硬盘（SSD）和混合硬盘（SSHD）等类型。

##### 磁盘的类型
1. **机械硬盘（HDD, Hard Disk Drive）**：
   - 使用旋转的磁性盘片和机械读写头存储数据。
   - 容量大，价格低，广泛用于桌面、服务器和NAS。
2. **固态硬盘（SSD, Solid State Drive）**：
   - 使用NAND闪存存储数据，无机械部件。
   - 速度快，抗震性强，适合高性能需求。
3. **混合硬盘（SSHD, Solid State Hybrid Drive）**：
   - 结合HDD和少量SSD缓存，提升性能同时保持大容量。
4. **光盘（Optical Disk，如CD/DVD/Blu-ray）**：
   - 使用激光读写数据，主要用于只读或归档存储。
5. **虚拟磁盘（Virtual Disk）**：
   - 通过软件模拟的存储设备，常用于虚拟化环境。

##### 工作原理
- **HDD**：盘片高速旋转（5400-15000 RPM），读写头在盘片表面移动，记录或读取磁信号。数据以扇区和磁道组织。
- **SSD**：通过电荷存储在NAND闪存单元中，控制器管理数据读写，采用页面和块结构。
- **SSHD**：SSD部分缓存常用数据，HDD存储大容量数据，智能算法优化访问速度。
- **光盘**：激光束在盘片表面刻录或读取凹点（Pits）和平面（Lands），数据以螺旋轨道存储。

##### 磁盘的优缺点
| 磁盘类型 | 优点                                                         | 缺点                                                         |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **HDD**  | - 容量大（1TB-20TB+）。<br>- 每GB成本低。<br>- 适合大容量存储和归档。<br>- 技术成熟，兼容性强。 | - 读写速度慢（100-200MB/s）。<br>- 机械部件易受震动/摔落损坏。<br>- 功耗和噪音较高。<br>- 延迟高，随机访问慢。 |
| **SSD**  | - 速度快（500MB/s-7000MB/s）。<br>- 抗震性强，无机械部件。<br>- 低延迟，随机访问快。<br>- 低功耗，适合移动设备。 | - 每GB成本较高。<br>- 容量较小（通常128GB-4TB）。<br>- 闪存寿命有限（TBW限制）。<br>- 数据恢复难度大。 |
| **SSHD** | - 兼顾HDD容量和SSD速度。<br>- 成本低于纯SSD。<br>- 适合混合负载场景。<br>- 兼容现有HDD接口。 | - 性能不如纯SSD。<br>- 缓存命中率依赖使用模式。<br>- 复杂性增加维护难度。<br>- 市场选择较少。 |
| **光盘** | - 便携，适合分发和归档。<br>- 成本低，长期存储稳定。<br>- 兼容性广泛（带光驱设备）。<br>- 防篡改（只读盘）。 | - 容量小（CD: 700MB, DVD: 4.7GB, Blu-ray: 25-100GB）。<br>- 读写速度慢。<br>- 易受划伤损坏。<br>- 使用场景减少（云存储替代）。 |

##### 磁盘与Linux文件系统的适配性
基于您之前的提问，以下是磁盘类型与Linux常用文件系统的适配性分析，突出每种文件系统在不同磁盘上的表现。

| 磁盘类型 | 推荐文件系统                  | 适配性分析                                                   | 推荐场景                        |
| -------- | ----------------------------- | ------------------------------------------------------------ | ------------------------------- |
| **HDD**  | **ext4**, **XFS**, **Btrfs**  | - **ext4**：性能均衡，碎片化低，适合通用存储（如数据盘、归档）。<br>- **XFS**：优于大文件读写，适合媒体服务器或数据库。<br>- **Btrfs**：支持快照和RAID，适合NAS或需要数据保护的场景。 | 桌面、服务器、NAS、大容量存储。 |
| **SSD**  | **F2FS**, **ext4**, **Btrfs** | - **F2FS**：专为闪存优化，减少写放大，适合高性能SSD。<br>- **ext4**：TRIM支持良好，稳定且通用，适合系统盘。<br>- **Btrfs**：支持压缩和快照，但需优化配置以减少写操作。 | 系统盘、数据库、嵌入式设备。    |
| **SSHD** | **ext4**, **Btrfs**           | - **ext4**：兼容性强，适合混合负载，缓存加速常用数据。<br>- **Btrfs**：快照和压缩可提升性能，但需注意缓存管理。 | 笔记本、预算有限的高性能场景。  |
| **光盘** | **ISO9660**, **UDF**          | - **ISO9660**：标准光盘格式，Linux广泛支持，适合CD/DVD。<br>- **UDF**：适合DVD/Blu-ray，支持大文件，跨平台兼容。 | 归档、分发软件、只读存储。      |

##### Linux环境下的磁盘管理
- **分区工具**：`fdisk`, `parted`, `gparted`用于创建和管理分区。
- **文件系统格式化**：`mkfs.ext4`, `mkfs.xfs`, `mkfs.btrfs`等命令格式化磁盘。
- **挂载**：使用`mount`命令将磁盘挂载到文件系统，`/etc/fstab`配置自动挂载。
- **性能优化**：
  - SSD：启用TRIM（`fstrim`或`discard`选项）延长寿命。
  - HDD：定期检查碎片（`e2fsck`）和优化RAID配置。
  - Btrfs/ZFS：配置快照策略和数据校验以提高可靠性。
- **监控工具**：`df`, `du`, `iotop`, `smartctl`用于检查磁盘使用情况和健康状态。

##### 选择建议（Linux环境）
- **系统盘**：SSD + **F2FS**（高性能）或 **ext4**（稳定通用）。
- **数据盘**：HDD + **XFS**（大文件）或 **Btrfs**（快照/RAID）。
- **NAS/服务器**：HDD/SSD + **Btrfs**或 **ZFS**（数据保护）。
- **可移动存储**：SSD + **exFAT**（跨平台）或 **ext4**（Linux专用）。
- **归档**：光盘 + **UDF**或HDD + **ext4**。。



#### **磁盘的管理基本概念**

Linux系统中所有的硬件设备都是通过文件的方式来表现和使用的，我们将这些文件称为设备文件。在Linux下的/dev 目录中有大量的设备文件，根据设备文件的不同，又分为字符设备文件和块设备文件。

字符设备文件的存取是以字符流的方式进行的，一次传送一个字符。常见的有打印机、终端（TTY）、绘图仪和磁带设备等，字符设备文件有时也称为”raw“设备文件。

块设备文件是以数据块方式存取的，最常见的设备就是磁盘。

##### 1、 磁盘设备在Linux下写表示方法

现在常见的磁盘类型有IDE并口硬盘、SATA 串口硬盘以及SCSI硬盘。不同类型的硬盘在linux下对应的设备文件名称不相同。Linux下磁盘设备常见的表示方案如下两种。

**方案1：**

```shell
主设备好+次设备+磁盘分区编号
对于IDE硬盘：表示为hd[a~z]x
对于SCSI硬盘：表示为sd[a~z]x  sda   sda1 sda2   sdb sdc 
```

**方案2：**

```shell
主设备号+[0~n],y
对于IDE硬盘：表示为hd[0~n],y
对于SCSI硬盘：表示为sd[0~n],y
```

主设备号代表设备的类型，可以唯一地确定设备的驱动程序和界面。主设备号相同的设备是同类型设备，即，使用同一个驱动程序，比如：hd表示IDE硬盘，sd表示SCSI硬盘，TTY表示终端设备等。

次设备号代表同类设备中的序列号，”a~z“就表示设备的序列号。如：/dev/sda 表示第一块SCSI硬盘。/dev/sdb 则表示第二块SCSI硬盘。

##### 2、 磁盘分区的划分标准

磁盘的分区对于linux系统的稳定和安全非常重要，合理地划分磁盘分区有助于系统的稳定运行和保障数据的安全。

磁盘的分区由主分区、扩展分区和逻辑分区组成。在一块硬盘上，主分区的最大个数是4个，其中扩展分区也算一个主分区，在扩展分区下可以建立很多个逻辑分区，所以主分区（包括扩展分区）范围是1~4，逻辑分区从5开始。对于逻辑分区，linux规定必须建立在扩展分区上，而不是建立在主分区上。

主分区的作用是用来启动操作系统的，它主要存放操作系统的启动或引导程序，因此建议操作系统的引导程序都放在主分区上，比如Linux的/boot分区，最好放在主分区上。

通过fdsik -l 命令可以显示当前系统分区的所有信息

```shell
[root@localhost ~]# fdisk  -l

Disk /dev/sda: 107.4 GB, 107374182400 bytes, 209715200 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000d3ee0

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048    41945087    20971520   83  Linux
/dev/sda2        41945088   209715199    83885056   8e  Linux LVM


# 详情：
Device  #显示磁盘分区对应的设备文件名
Boot    #显示那个分区是引导分区
Start   #表示每个磁盘分区的起始位置，以柱面为计算单位
End     #表示每个磁盘分区的终止位置，以柱面为计算单位
Blocks  #显示磁盘分区的容量，以KB为单位
 Id     #显示磁盘分区对应的ID。根据不同的分区，则对应的ID也不同
system  #与ID基本相同，都表示不同分区类型
```

 

#### **磁盘的管理工具**

磁盘分区工具:

fdisk 是linux下一款功能强大的磁盘分区管理工具，可用于观察磁盘的使用情况，也可以对磁盘进行分割。Linux下类似于fdisk的工具还有cfdisk、parted等。他们都具有各自的优点，这里推荐fdisk，因为简单容易上手。

命令：fdisk

语法：fdisk [选项] -l <磁盘> 列出分区表

选项：

```shell
 -l  列出磁盘分区表
```

交互式；

```shell
[root@localhost ~]# fdisk /dev/sda 
欢迎使用 fdisk (util-linux 2.23.2)。

更改将停留在内存中，直到您决定将更改写入磁盘。
使用写入命令前请三思。


命令(输入 m 获取帮助)：m
命令操作
   a   toggle a bootable flag   # 设定硬盘启动区
   b   edit bsd disklabel       # 编辑BSD类型的分区
   c   toggle the dos compatibility flag   #编辑DOS兼容的分区
   d   delete a partition       #删除分区
   g   create a new empty GPT partition table  #创建一个GPT分区表
   G   create an IRIX (SGI) partition table    #创建一个 IRIX （SGI） 分区表
   l   list known partition types     #查看指定分区的分区类型信息
   m   print this menu                #显示帮助菜单
   n   add a new partition            #添加一个分区表
   o   create a new empty DOS partition table   #创建DOS分区表
   p   print the partition table          #输出分区表
   q   quit without saving changes        #退出交互操作，不保存任何操作
   s   create a new empty Sun disklabel   #创建一个新的sun磁盘标签
   t   change a partition's system id     #更改分区的系统 ID
   u   change display/entry units         #更改显示/输入单位
   v   verify the partition table         #验证分区表
   w   write table to disk and exit       #将表写入磁盘并退出
   x   extra functionality (experts only) #额外功能（仅限专家）
   
```

示例：

```shell

```

##### parted 工具规划磁盘分区

磁盘分区时使用比较多的工具一般都是fdsik。而fdisk工具对分区是有大小限制的，它只能划分2TB的磁盘。由于磁盘越来越廉价，而且磁盘空间越来越大，现在的磁盘空间很多是远远大于2TB，此时就需要另外一个磁盘管理工具parted来完成大于大于2TB的磁盘分区工作。

parted是有GNU组织开发的一款功能强大的磁盘分区和分区大小调整工具。它比fdisk更加灵活，功能也更丰富，它可以创建分区、调整分区大小、移动和复制分区、删除分区等。在使用上，parted与fdisk功能类似，它也分为两种模式：命令行模式和交互模式。在命令行模式下可以直接对磁盘进行分区操作，改模式比较适合编程应用。而交互模式比较方便和简单，适合在对parted命令不是很熟悉的情况下。

在介绍parted命令之前，先熟悉常用的两种分区表。目前最常见的两种分区有MBR与GPT。MBR分区表，既主引导分区表，这个比较常见，是在Windows系统上常用的分区表。它主要特点是支持最大2TB的磁盘分区，而且对分区有限制：最多支持4个主分区或3主分区加一个扩展分区。GPT是源自EFI标准的一种较新的磁盘分区表，是目前以及以后磁盘分区的主要形式。与MBR分区表相比，GPT突破MBR 4 个主分区的限制，每个磁盘最多支持128个分区，同时支持大于2TB的分区，最大卷可大18EB。

###### parted 使用方法

要使用parted名，需要安装parted工具包。安装方法：

```shell
[root@server ~]# yum -y install parted
```

语法：`parted [OPTION]... [DEVICE [COMMAND [PARAMETERS]...]...]`

选项：

```shell
  -h, --help                      显示此求助信息
  -l, --list                      列出所有块设备上的分区布局
  -m, --machine                   显示机器可解析输出
  -s, --script                    从不提示用户
  -v, --version                   显示版本
  -a, --align=[none|cyl|min|opt]  新分区的对齐方式
  [part-type|part-label] [fs-type] start end
```

交互式常用command：

| 命令    | 含义                                                         |
| ------- | ------------------------------------------------------------ |
| help    | 获取帮助                                                     |
| mklabel | 创建分区表，也就是设置使用msdos还是使用gpt格式。例如：mklabel gpt,表示设定分区表为gpt格式。 |
| mkpart  | 创建新分区。<br />使用格式为：mkpart  [part-type\| par-label] [fs-type] start end<br /><br />- part-type 表示分区类型，主要有primary(主分区)、extended(扩展分区)、logical（逻辑分区），其中，扩展分区和逻辑分区只针对msdos分区表。<br /><br />-  FS-type 表示文件系统类型，主要有fat32、NTFS、ext2、ext3等，可不填写。<br /><br />- start  表示分区的起始位置<br /><br />- end   表示分区的结束位置 |
| print   | 输出分区信息，可简写为p.该功能有3个选项：<br /><br />- free 显示该盘的所有信息，并显示磁盘剩余空间。<br /><br />- unmber 显示指定的分区的信息<br /><br />- all 或 list 显示所有磁盘信息 |
| rm      | 删除分区<br /><br />命令格式： rm unmber<br /><br />例如：rm 2 就表示将编号为3的分区删除 |
| select  | 选择设备<br /><br />当输入parted 命令后直接按Enter 键进入交互模式时，默认设置的是系统的第一块硬盘。如果系统由多块硬盘，需要用select 命令选择操作的硬盘。<br /><br />例如：select  /dev/sdb |

这里仅列出了parted最经常使用的一些交互命令。parted 主要的功能是进行磁盘分区，虽然parted 也具有创建文件系统的功能，但是这方面的功能较弱。因此最常见的做法是用parted进行磁盘分区，然后退出parted交互模式，用其他命令（mkfs.ext4、mkfs.xfs）创建文件系统。

parted 实验示例：

```shell
[root@server ~]# parted 
GNU Parted 3.1
使用 /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.
#选择磁盘
(parted) select /dev/sdb
使用 /dev/sdb

(parted) p    #显示磁盘分区信息                                                             
错误: /dev/sdb: unrecognised disk label
Model: VMware, VMware Virtual S (scsi)                                    
Disk /dev/sdb: 21.5GB      #显示磁盘大小
Sector size (logical/physical): 512B/512B
Partition Table: unknown   #目前没有分区信息
Disk Flags: 

#将磁盘分区表类型修改为gpt格式  
(parted) mklabel gpt   
(parted) p                                                                
Model: VMware, VMware Virtual S (scsi)
Disk /dev/sdb: 21.5GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start  End  Size  File system  Name  标志
#通过mkpart命令创建磁盘分区，primary表示创建一个主分区， 0gb是分区起始位置，10gb是分区结束位置
(parted) mkpart primary 0gb 10gb
#显示分区信息
(parted) p                                                                
Model: VMware, VMware Virtual S (scsi)
Disk /dev/sdb: 21.5GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name     标志
 1      1049kB  10.0GB  9999MB               primary  #这里可以看到，磁盘分区已经创建成功


#继续创建分区
(parted) mkpart primary 10gb 15gb 
(parted) p                                                                
Model: VMware, VMware Virtual S (scsi)
Disk /dev/sdb: 21.5GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name     标志
 1      1049kB  10.0GB  9999MB               primary
 2      10.0GB  15.0GB  5000MB               primary
 
 #print 命令通过加free 参数，可以查看剩余空闲的磁盘空间
 (parted) p free
Model: VMware, VMware Virtual S (scsi)
Disk /dev/sdb: 21.5GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name     标志
        17.4kB  1049kB  1031kB  Free Space
 1      1049kB  10.0GB  9999MB               primary
 2      10.0GB  15.0GB  5000MB               primary
        15.0GB  21.5GB  6475MB  Free Space   #磁盘空闲空间

#把剩余空间都分出去
(parted) mkpart primary  15gb 21.5gb 
(parted) p
Model: VMware, VMware Virtual S (scsi)
Disk /dev/sdb: 21.5GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name     标志
 1      1049kB  10.0GB  9999MB               primary
 2      10.0GB  15.0GB  5000MB               primary
 3      15.0GB  21.5GB  6474MB               primary
 
 #删除一个磁盘分区
 (parted) rm 3                                                             
(parted) p                                                                
Model: VMware, VMware Virtual S (scsi)
Disk /dev/sdb: 21.5GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name     标志
 1      1049kB  10.0GB  9999MB               primary
 2      10.0GB  15.0GB  5000MB               primary
 

```

分区完成后，还需要分区进行格式化，创建文件系统，然后挂载才能使用。

```shell
[root@server ~]# mkfs.xfs  /dev/sdb1

[root@server ~]# mkfs.xfs  /dev/sdb2

```

挂载（mount）

```shell
[root@server ~]# mkdir /data
[root@server ~]# mkdir /data2
[root@server ~]# mount /dev/sdb1  /data/
[root@server ~]# mount /dev/sdb2  /data2/
```

验证：

```shell
[root@server ~]# df -h
```



##### 3、 设备的挂载与使用

在Linux系统用什么方式来实现使用需要满足几个条件？

>  1、添加磁盘
>   2、磁盘分区
>   3、磁盘格式化成为文件系统
>  挂载磁盘分区的文件系统
>  挂载分区对应的设备文件
>  挂载到在咱们创建挂载点      命令：  mount  umount 

命令：

```shell
mount  umount 
```



#### **逻辑卷管理**

##### LVM

##### 一、 LVM 的基本概念

在对磁盘进行分区大小规划时，有时往往不能确定这个分区要使用的总空间大小，而用fdisk对磁盘分区后，每个分区的大小已经固定了，如果分区设置得过大，就白白浪费了磁盘空间，而分区设置得过小，就会导致空间不够用的情形。此时最常见的方法是重新规划磁盘分区，或者通过软链接的方式将此分区的目录链接到另一个分区，虽然这能临时解决问题，但是给管理带来了麻烦。如何解决这写问题呢？LVM是一个错的方法。

LVM（Logical Volume Manager,逻辑卷管理器）是Linux下对磁盘分区进行管理的一种机制。LVM建立在磁盘分区和文件系统之间的一个逻辑层，管理员利用LVM可以在磁盘不用重新分区的情况下动态调整分区的大小。如果系统新增一块硬盘。通过LVM就可以将新增的硬盘空间直接扩展到原来的磁盘分区上。

##### 二、LVM中的术语

通过LVM，屏蔽了磁盘分区的底层差异，在逻辑上给文件系统提供了一个卷的概念，然后再这些卷上建立相应的文件系统。在认识LVM之前，先熟悉LVM中几个常用的术语，即在LVM中涉及的几个概念。

- 物理存储设备（physical media）: 指系统的存储设备文件，如/dev/sda、/dev/hdb等；
- 物理卷（Physical Volume, PV ）：指硬盘分区或者从逻辑上看和硬盘分区类似的设备（如：RAID设备）
- 卷组（Volume Group， VG）：类似于非LVM 系统中的物理硬盘，一个LVM卷组由一个或多个物理卷组成。
- 逻辑卷（Logical Volume，LV1）：类似于LVM系统中的硬盘分区，LV建立在VG上，可以在LV上创建文件系统。
- PE （Physical Extent ,PE )：PV中可以分配的最小存储单元称为PE。PE的大小是可以指定的，默认为4MB。
- LE （Logical Extent， LE）：LV 中可以分配的最小单元称为LE。在同一卷组中，LE的大小和PE是一样的，且一一对应。

下面是LVM各个组成部分之间的对应关系

![LVM](/Users/abbott/Documents/LVM.jpg)

##### 三、LVM的创建与管理

1、创建物理分区

在使用LVM之前，需要首先划分磁盘分区，也就是用fdisk命令划分磁盘分区，划分的方法在之前已经讲解过，稍微不同的是，在创建分区的时候，需要指定分区类型为 Linux LVM，对应的ID为8e（其实LVM也能识别Linux默认的分区类型83）。分区如下。

```shell

```

2、创建物理卷

创建物理卷（PV）的命令是pvcreate

语法：

```shell
pvcreate 磁盘分区或整个磁盘
```

示例：将划分的所有磁盘分区创建为物理卷

```shell
pvcreate /dev/sdb1 /dev/sdb2
```

3、创建卷组

创建卷组（VG）的命令是vgcreate

语法：

```shell
vgcreate myvgl /dev/sdb1 /dev/sdb2
```

4、激活卷组

卷组（VG）创建完毕后，可以通过vgchange命令激活卷组，而无线重启系统。

- 命令：

  ```shell
  vgchange
  ```

- 语法：

  ```shell
  vgchange -a y 卷组名   #激活卷组
  vgchange -a n 卷组名   #停用卷组
  ```

- 激活卷组myvg1

  ```shell
  vgchang -a y myvg1
  ```

5、显示卷组、物理卷属性信息

vgdisplay 用户显示创建的卷组信息，同理pvdisplay 命令用户显示物理卷组信息。

- 命令语法：

  ```shell
  vgdisplay 卷组名
  pvdisplay 物理卷名
  ```

- 示例：查看创建的卷组

  ```shell
  vgdisplay myvg1
  ```

6、创建逻辑卷

创建逻辑卷（LV）命令是lvcreate

- 命令语法：

  ```shell
  lvcreate [-L 逻辑卷大小 | -l PE数] -n 逻辑卷名称   所属的卷组名
  
  -L 后面接逻辑卷的大小，可以用K、M、G表示。
  ```

- 示例：在myvg1下创建两个逻辑卷mylv1 和mylv2

  ```shell
  lvcreate -L 4G -n mylv1 myvgl
  #检查卷组myvg1 中可用的卷组空间
  vgdisplay myvg1 | grep "Free PE"   
  #再次分
  ```

7、格式化逻辑卷，创建文件系统

文件系统是创建在逻辑卷上的。这里假定使用linux默认的xfs文件系统。

- 命令语法：

  ```shell
  mkfs.xfs /dev/myvg1/mylv1
  ```

8、添加新的物理卷到卷组

添加新的物理卷到卷组的命令为vgextend.

- 命令语法：

  ```shell
  vgextend 卷组名 新加入的物理卷
  ```

- 示例：

  ```shell
  pvcreate /dev/sd3    #先将分区转换为物理卷
  vgextend   myvg1 /dev/sdb3           #将新增物理卷添加到卷组中
  ```

- 

9、修改逻辑卷的大小

LVM 最主要的功能就是能动态调整分区的大小，其实也就是修改逻辑卷的大小。

- 命令语法：

  ```shell
  lvextend     #扩展逻辑卷
  lvreduce     #缩减逻辑卷
  ext2resize   #修改文件系统大小
  
  #语法
  lvextend [-L (+size) -l [+PE数]] 逻辑卷名称
  lvreduce [-L (-size) -l [-PE数]] 逻辑卷名称
  ```

  > 修改逻辑卷分为扩充逻辑卷大小和减少逻辑卷大小两种情况。

- 选项：

  ```shell
  -l:与-L 用法相同，不同的是 -l 选项用PE数来表示逻辑卷的大小
  ```

- 示例1：以文件系统是ext2/ext3/ext4，可以通过resize2fs来扩充文件系统

  1. 扩充逻辑卷 

     - 命令：

       ```shell
       利用扩展逻辑卷命令lvextend扩展逻辑空间
       利用resize2fs命令扩展文件系统
       ```

     - 示例：

       ```shell
       lvextend  -L +2G /dev/myvg1/myvg
       resiza2fs /dev/myvg1/myvg  #更新逻辑卷大小
       ```

     - 验证:

       ```shell
       df -h
       ```

  2. 减少逻辑卷

     > 1. 卸载已经挂载的逻辑卷分区
     > 1. 利用resize2fs指令修改文件系统大小以实现空间缩减。
     > 1. 利用减少逻辑卷命令lvreduce减少逻辑卷空间。
     > 1. 挂载缩减后的逻辑分区。

     - 命令语法：

       ```shell
       resize2fs /dev/myvg1/myvg 5G    #缩减逻辑卷大小
       e2fsck -f /dev/myvg1/myvg       #检查逻辑卷
       resize2fs /dev/myvg1/myvg 5G    #再次执行缩减命令
       lvreduce -L -2G  /dev/myvg1/myvg
       ```

- 示例2：以xfs 文件系统为例

  1. 扩充逻辑卷空间

     > 利用扩展逻辑卷命令lvextend扩展逻辑卷空间
     >
     > 利用xfs_growfs指令修改xfs文件系统大小以实现空间扩充

     - 命令语法：

       ```shell
       lvextend -l +1023 /dev/myvg1/myvg     # -l:选项是扩充PE数， 或者使用 -L 选项 +4G
       xfs_growfs /myvg      #执行xfs_growfs命令，是扩充的大小生效
       ```

10、如何删除物理卷、卷组和逻辑卷

- 删除物理卷

  命令：pvremove

- 将物理卷从卷组移出

  命令：vgreduce

- 删除卷组

  命令：vgremove

- 删除逻辑卷

  命令： lvremove 

删除的步骤：

> 卸载逻辑卷分区---> 删除卷组上所有的逻辑卷—>删除卷组

命令语法：

```shell
umount     #卸载逻辑卷对应的分区
lvremove   /dev/myvg1/myvg   #删除逻辑卷
vgreduce   myvg1 /dev/sdb1   #从卷组中移出一个物理卷
pvgremove  /dev/sdb1         #删除物理卷
vgremove   myvg1 #删除卷组
```

#### RAID磁盘阵列

早在1978年，美国加州大学伯克利分校就提出了RAID（Redundant Array of Independent DIsk）虚拟存储系统。RAID即独立冗余磁盘阵列；其思想是将多块独立的磁盘按不同的方式组合为一个逻辑磁盘，从而提高存储容量，提升存储性能或提供数据备份功能。RAID存储系统的组合方式根据RAID基本定义。

RAID分为软件RAID和硬件RAID两类。在现有的操作系统中。如windows、Linux、Unix等已经集成了软RAID的功能。软件RAID可以实现与硬件RAID相同的功能，当由于其没有独立的硬件控制设备，所以性能不如硬件RAID，但软件RAID实现简单，不需要额外的硬件设备。硬件RAID通常需要有RAID卡，RAID卡本身会有独立控制部件和内存，所以不会占有系统资源，效率高、性能强。当然目前市面的大部分主板已经继承了RAID卡，具体使用方式可以参考硬件说明书

##### RAID 级别

###### 1、 RAID 0 （不含校验与冗余的条带存储）

在 RAID 0 系统中，数据被分成块，这些块被写入阵列中的所有驱动器。通过同时使用多个磁盘（至少 2 个），这提供了卓越的 I/O 性能。通过使用多个控制器（理想情况下每个磁盘一个控制器）可以进一步增强这种性能。

**RAID 0 的优点**

- RAID 0 在读取和写入操作方面都提供了出色的性能。奇偶校验控制不会导致开销。
- 使用所有存储容量，没有开销。
- 该技术易于实施。

**RAID 0 的缺点**

- RAID 0 不是容错的。如果一个驱动器发生故障，RAID 0 阵列中的所有数据都将丢失。它不应用于关键任务系统。

###### 2、 RAID 1 （不含校验的镜像存储）

通过将数据写入数据驱动器（或数据驱动器组）和镜像驱动器（或驱动器组），数据被存储两次。如果驱动器发生故障，控制器将使用数据驱动器或镜像驱动器进行数据恢复和连续操作。RAID 1 阵列至少需要 2 个驱动器。

**RAID 1 的优点**

- RAID 1 提供出色的读取速度和与单个驱动器相当的写入速度。
- 如果驱动器发生故障，则不必重建数据，只需将它们复制到替换驱动器即可。
- RAID 1 是一种非常简单的技术。

**RAID 1 的缺点**

- 主要缺点是有效存储容量仅为驱动器总容量的一半，因为所有数据都被写入两次。
- 软件RAID 1解决方案并不总是允许失败的驱动器的热插拔。这意味着只有在关闭计算机上的计算机下电后才只能替换失败的驱动器。对于许多人同时使用的服务器，这可能是不可接受的。这些系统通常使用支持热插拔的硬件控制器。

###### 3、 RAID 3 （字节级别的校验式条带存储）

该级别的磁盘阵列可以参考RAID 2 的读写原理，仅数据分割的单位为字节。å

###### 4、 RAID 4 （数据块级别的校验式条带存储）

该级别的磁盘阵列与RAID 2 类似，仅数据分割单位不同。

###### 5、 RAID 5（数据库级别的分布式校验条带存储）

RAID-5具有和RAID-0相近似的数据读取速度，同时磁盘空间利用率也要比RAID1高。属于是RAID-0和RAID-1的折中方案，是一种存储性能、数据安全和存储成本兼顾的存储解决方案。由于存储成本相对较低，是目前运用较多的一种解决方案。
      RAID-5阵列中数据的分布与RAID-0类似，数据也是分布到每块硬盘上，但Raid-5没有独立的奇偶校验盘，他通过将校验的数据循环存储并分散放在所有磁盘上，其中任意N-1块磁盘上都存储完整的数据，也就是说有相当于一块磁盘容量的空间用于存储奇偶校验信息。因此当RAID5能够支持在一块盘离线的情况下保证数据的正常访问，不会影响数据的完整性，从而保证了数据安全。当损坏的磁盘被替换后，RAID还会自动利用剩下奇偶校验信息去重建此磁盘上的数据，来保持RAID5的高可靠性。
      RAID-5的组建，至少需要三块或更多的磁盘，并可以使用0块或更多的备用磁盘，其数据安全保障程度要比RAID1低，写入数据的速度比对单个磁盘进行写入速度稍慢。如果有两块或两块以上硬盘同时离线，或者RAID信息出错等原因，阵列便会失效，这时就需要对数据进行重组。并且做raid 5阵列所有磁盘容量必须一样大，当容量不同时，会以最小的容量为准。 同时，最好硬盘转速一样，否则会影响性能。

###### 6、 RAID 10 （镜像与条带存储）

RAID 1+0也被称为RAID-10标准，实际是将RAID-0和RAID-1标准结合的产物，是先对磁盘先进行RAID-1，然后RAID-0，这种方法即提供了冗余，而且也提升了速度。
      在连续地以位或字节为单位分割数据并且并行读/写多个磁盘的同时，为每一块磁盘作磁盘镜像进行冗余。它的优点是同时拥有RAID-0的超凡速度和RAID-1的数据高可靠性，但是CPU占用率同样也更高，而且磁盘的利用率比较低。由于利用了RAID-0极高的读写效率和RAID-1较高的数据保护、恢复能力，使RAID-10成为了一种性价比较高的等级，目前几乎所有的RAID控制卡都支持这一等级。





| RAID等级 | 最少硬盘 | 最大容错 | 可用容量 | 读取效能 | 写入效能 |             安全性             |              目的              |       应用产业       |
| :------: | :------: | :------: | :------: | :------: | -------: | :----------------------------: | :----------------------------: | :------------------: |
| 单一硬盘 |  (参考)  |    0     |    1     |    1     |        1 |               无               |                                |                      |
|   JBOD   |    1     |    0     |    n     |    1     |        1 |         无（同RAID 0）         |            增加容量            | 个人（暂时）储存备份 |
|    0     |    2     |    0     |    n     |    n     |        n | 一个硬盘异常，全部硬盘都会异常 |       追求最大容量、速度       |   影片剪接快取用途   |
|    1     |    2     |   n-1    |    1     |    n     |        1 |        高，一个正常即可        |         追求最大安全性         |    个人、企业备份    |
|    5     |    3     |    1     |   n-1    |   n-1    |      n-1 |            中下至中            |     追求最大容量、最小预算     |  个人、小型企业备份  |
|    6     |    4     |    2     |   n-2    |   n-2    |      n-2 |  中至中高,仅安全性较RAID 5高   |       同RAID 5，但较安全       |    个人、企业备份    |
|    10    |    4     |          |          |          |          |               高               | 综合RAID 0/1优点，理论速度较快 |  大型资料库、伺服器  |
|    50    |    6     |          |          |          |          |               高               |          提升资料安全          |                      |
|    60    |    8     |          |          |          |          |               高               |          提升资料安全          |                      |

##### 创建于管理软RIAD

1、查看磁盘信息

```shell
fdisk -l
```

2、创建RAID 模式md0 

- 命令：mdadm

- 描述：管理Linux软件RAID设备

- 用法：mdadm [模式] 选项 <raid 设备>

- 选项：

  ```shell
  —C， # 创建软件RAID
  -n, --raid-devices=   #指定空闲磁盘
  -l  --level=          #指定级别 （raid {0, 1 , 5  10 }）
  -X, --examine-bitmap  #指定位图
  ```

- 示例 ：创建RAID 模式md0 

  ```shell
  mdadm -C /dev/md0 -n3 -l5 -x1 /dev/*d{d,e,f,g}
  ```

  查看raid状态：

  ```shell
  cat /proc/mdstat
  
  mdadm -D /dev/md0
  ```

  格式文件系统

  ```shell
  mkfs.ext4 /dev/md0
  ```

  挂载使用：

  ```shell
  mount /dev/md0 /****
  #开机自动挂载
  vim /etc/fstab  
  /dev/md0   /****    ext4   defaulte     0   0
  ```

RAID 性能测试：

- 普通磁盘：写入模拟

  ```shell
  time dd if=/dev/zero  of=txt bs=1M count=1000
  if=文件名：输入文件名，默认为标准输入。即指定源文件。
  of=文件名：输出文件名，默认为标准输出。即指定目的文件。
  count=blocks：仅拷贝blocks个块，块大小等于ibs指定的字节数。
  ibs=bytes：一次读入bytes个字节，即指定一个块大小为bytes个字节。
  obs=bytes：一次输出bytes个字节，即指定一个块大小为bytes个字节。
  bs=bytes：同时设置读入/输出的块大小为bytes个字节。
  ```

RAID 模拟故障

```shell
mdadm  /dev/md0   -f /dev/sda    #  模拟
mdadm -D /dev/md127  --detail 
```