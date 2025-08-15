#### 用户与用户组管理概念

##### **1、理解 Linux 多用户、多任务的特性**

Linux 是一个真实的、完整的多用户多任务操作系统。多用户多任务就是可以在系统上建立多个用户，而多个用户可以在同一时间内登录同一个系统执行各自不同的任务，而互不影响。例如：某台Linux服务器上有4个用户，分别是root、www、ftp。在同一时间内root用户可能在查看日志，管理、维护系统。www用户可能在修改自己的网页程序，因此可知，不同用户具有不同的权限，每个用户在权限允许的范围内完成不同的任务，Linux正是通过这种权限的划分与管理，实现了多用户多任务的运行机制。

##### **2、Linux 下用户的角色分类**

在Linux下用户是根据角色定义的，具体分为三中角色。

①  超级用户：

​      拥有对系统的最高管理权限，默认是root用户；

②  普通用户：

​      只能对自己目录下的文件进行访问和修改，具有登录系统的权限；

③  虚拟用户：

​      也叫『伪』用户，这类用户最大的特点是不能登录系统，它们在存在主要是为了方便系统管理，满足相应的系统进程对文件属主的要求。

##### **3、用户和组的概念**

​        我们知道，Linux 是一个多用户多任务的分时操作系统，如果要使用系统资源，就必须向系统管理申请一个账户，然后通过这个账户进入系统。这个账户和用户是一个概念。通过建立不同属性的用户，一方面，可以合理利用和控制系统资源；另一方面，也可以帮助用户组织文件，提供对用户文件安全性保护。

每个用户都有一个唯一的用户名和用户密码，在登录系统时，只有正确输入了用户名和密码，才能进入系统和自己的主目录。

用户组是具有相同特征用户的逻辑集合，有时我们需要让多个用户具有相同的权限，比如：查看、修改某个文件的权限。

**用户和组的关系**

用户和用户都的对应的关系有：

①   一对一：一个用户可以存在一组中，也可以是组中的唯一成员。

②  一对多：一个用户可以存在多个用户组中，此用户具有多个组的共同权限。

③  多对一：多个用户可以存在一个组中，这些用户具有和组相同的权限。

④  多对多：多个用户可以存在多个组中。其实就是上面三个对应关系的扩展

#### 用户管理工具

##### 组管理

- 创建组

  命令： groupadd

  语法：`groupadd [options] GROUP 或者groupadd [options] gid GROUP`

  选项： -g      # 设置组ID

  ```shell
  Options:
    -f, --force                   exit successfully if the group already exists,
                                  and cancel -g if the GID is already used
    -g, --gid GID                 use GID for the new group
    -h, --help                    display this help message and exit
    -K, --key KEY=VALUE           override /etc/login.defs defaults
    -o, --non-unique              allow to create groups with duplicate
                                  (non-unique) GID
    -p, --password PASSWORD       use this encrypted password for the new group
    -r, --system                  create a system account
    -R, --root CHROOT_DIR         directory to chroot into
    -P, --prefix PREFIX_DIR       directory prefix
  
  ```

  示例:

  ```shell
  [root@server ~]# groupadd  tom
  [root@server ~]# groupadd -g 1005 abbottA
  ```

  验证：

  ```shell
  [root@server ~]# cat /etc/group
  tom:x:1001:
  abbottA:x:1005:
  ```

- 切换组

  **命令**：newgrp 

  **描述：**如果一个用户同时属于多个用户组，那么用户可以在用户组之间切换 ，以便具有其他用户的权限，newgrp主要用于在多个用户组之间进行切换。

  语法：`newgrp [-] [组]`

  示例：创建三个组和一个用户

  ```shell
  #创建组
  [root@server ~]# groupadd group1
  [root@server ~]# groupadd group2
  [root@server ~]# groupadd group3
  #创建用户user1 同时指定主为group1 附加用户组为group2和group3
  [root@server ~]# useradd  -g group1 -G group2,group3 user1
  #验证
  [root@server ~]# cat /etc/group | grep user1
  group2:x:1007:user1
  group3:x:1008:user1
  #切换用户
  [root@server ~]# su user1
  [user1@server root]$ cd ~
  #我是谁
  [user1@server ~]$ whoami 
  user1
  [user1@server ~]$ mkdir user1_doc
  [user1@server ~]$ ll
  总用量 0
  drwxr-xr-x. 2 user1 group1 6 3月  20 18:01 user1_doc
  #切换用户
  [user1@server ~]$ newgrp group2
  [user1@server ~]$ mkdir user2_doc
  [user1@server ~]$ newgrp group3
  [user1@server ~]$ mkdir user3_doc
  [user1@server ~]$ ll
  总用量 0
  drwxr-xr-x. 2 user1 group1 6 3月  20 18:01 user1_doc
  drwxr-xr-x. 2 user1 group2 6 3月  20 18:02 user2_doc
  drwxr-xr-x. 2 user1 group3 6 3月  20 18:02 user3_doc
  ```

- 删除组

  ```SHELL
  [root@server ~]# cat /etc/group | grep tom
  tom:x:1001:
  [root@server ~]# groupdel tom
  [root@server ~]# cat /etc/group | grep tom
  ```

- 修改组权限

  命令：groupmod

  语法：groupmod [选项] 组名

  选项：

  ```shell
  -g GID：修改组 ID；
  -n 新组名：修改组名；
  ```

  示例：

  ```shell
  [root@server ~]# cat /etc/group | grep tom 
  tom:x:1009:
  [root@server ~]# groupmod -n tom2 tom
  [root@server ~]# cat /etc/group | grep tom 
  tom2:x:1009
  ```


##### 用户管理

- 创建用户

  **命令：** useradd

  语法：useradd [选项] 用户

  选项：

  ```shell
  -c 设置账户信息，
  -d 设置账户的家目录
  -e 新账户的过期日期
  -g 新账户主组的名称或 ID
  -G 新账户的附加组列表
  -p 加密后的新账户密码
  -l 锁定用户  仅root使用
  -u 解锁用户
  ```

  示例：

  ```shell
  [root@server ~]# useradd zhansan
  [root@server ~]# cat /etc/passwd | grep zhansan
  zhansan:x:1002:1002::/home/zhansan:/bin/bash
  ```

- 修改用户属性

  命令： usermod

  语法： usermod [选项] 用户名称

  选项：

  ```shell
   -c, --comment 注释            GECOS 字段的新值
    -d, --home HOME_DIR           用户的新主目录
    -e, --expiredate EXPIRE_DATE  设定帐户过期的日期为 EXPIRE_DATE
    -f, --inactive INACTIVE       过期 INACTIVE 天数后，设定密码为失效状态
    -g, --gid GROUP               强制使用 GROUP 为新主组
    -G, --groups GROUPS           新的附加组列表 GROUPS
    -a, --append GROUP            将用户追加至上边 -G 中提到的附加组中，
                                  并不从其它组中删除此用户
    -h, --help                    显示此帮助信息并推出
    -l, --login LOGIN             新的登录名称
    -L, --lock                    锁定用户帐号
    -m, --move-home               将家目录内容移至新位置 (仅于 -d 一起使用)
    -o, --non-unique              允许使用重复的(非唯一的) UID
    -p, --password PASSWORD       将加密过的密码 (PASSWORD) 设为新密码
    -R, --root CHROOT_DIR         chroot 到的目录
    -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
    -s, --shell SHELL             该用户帐号的新登录 shell
    -u, --uid UID                 用户帐号的新 UID
    -U, --unlock                  解锁用户帐号
  
  ```

  示例：

  ```shell
  [root@server ~]# useradd tom
  [root@server ~]# usermod -d /home/tomcat tom 
  [root@server ~]# usermod -e 2022-10-01 tom 
  [root@server ~]# usermod -s /bin/bash tom
  [root@server ~]# usermod -u 1004 tom 
  ```

- 删除用户

  命令： userdel
  
  语法： userdel  [选项] 用户名称
  
  选项：-r 
  
- 设置密码

  命令：passwd
  
  语法：passwd [选项] 用户名
  
  选项： -l  锁定用户，仅root可以使用
  
  ​            -stdin 从文件或管道符读取密码
  
  ​            -u  解锁用户
  
  ​            -d  快速清空用户密码，仅root用户可以使用
  
  示例：
  
  ```shell
  [root@server ~]# passwd tom3     #指定tom3密码
  更改用户 tom3 的密码 。
  新的 密码：
  无效的密码： 密码少于 8 个字符
  重新输入新的 密码：
  passwd：所有的身份验证令牌已经成功更新。
  
  [root@server ~]# echo 123456789 | passwd --stdin tom3    #设置tom3密码
  更改用户 tom3 的密码 。
  passwd：所有的身份验证令牌已经成功更新。
  c
  passwd: 操作成功
  ```
  ##### 用户和组配置文件

① 用户和组相关的配置文件

​    1） /etc/passwd 文件

​		系统用户配置文件，是用户管理中最终要的一个文件。这个文件记录了Linux系统中每个用户的一些基本属性，并且对所有用户可读。

```shell
用户名：密码：用户标识号：组标示号：注释性描述：主目录：默认shell
root:x:0:0:root:/root:/bin/bash
```

下面是每个字段的详细含义：

```shell
# 用户名：    
 是代表用户账号的字符串

##密码：    	 
存放着加密后的用户密码，虽然这个字段存放的只是用户密码的加密串，不是明文，但是由于/etc/passwd文件对所有用户都可读，因此着仍然是一个安全隐患。因此，现在许多Linux版本都使用了shadow技术，把真正加密后的用户密码存放在/etc/shadow文件中，而在/etc/shadow文件的密码字段中只存放一个特殊的字符。例如用"X"或者"*" 来表示。

# 用户标识号：
就是用户的UID，每个用户都有一个UID，并且是唯一的。通常UID号的取值范围：0~65535，0是超级用root的标识号，1~99由系统保留，作为管理账号，普通用户的标识号从100开始。而linux系统中，普通用户UID默认从500开始。因此多个用户公用一个UID是非常危险，会造成系统权限和管理的混乱。

#组标示号：
就是组的GID，与用户的UID类似，这个字段记录了用户所属的用户组。它对应/etc/group文件中的一条记录。

#注释性描述：

主目录：

默认shell
```

② /etc/shadow文件

由于/etc/passwd文件是所有用户都可读的，因此就导致了用户的密码容易出现泄露，于是，Linux将用户的密码信息重/etc/passwd 中分离出来，单独放到一个文件中，这个文件就是/etc/shadow。该文件只有root用户拥有读权限，从而保证了用户密码的安全性。

下面是/etc/shadow 文件的内容格式：

```shell
[root@huawei abbott]# cat /etc/shadow
root:$6$0ctItNBG$vPmSZBb8p3RKyjHI51:18963:0:99999:7:::
用户名:加密密码：最后一次修改时间：最小时间间隔：最大时间间隔：警告时间：不活动时间：失效时间：保留字段
```

③ /etc/group文件

```shell
tom:x:1003:
组名：密码 X为站位符：组ID：组内用户列表
```



#### 文件与权限的设定

所谓的文件权限，是指对文件的访问权限，包括对文件的读、写、删除、执行等。在Linux下，每个用户都具有不同的权限，普通用户只能在自己的主目录下进行写操作，而且在主目录之外，普通用户只能进行查找、读取操作。如何处理好文件权限和用户之间的关系，是本章讲述的重点



##### 1、查看文件的权限属性

使用ls命令就可以查看文件以及目录的权限信息，不带任何参数的ls命令只显示文件名称，通过『ls-al』可以显示文件或者目录的权限信息。看下输出：

```shell
[root@server ~]# ls -al /etc/
总用量 40
drwxr-xr-x.  6 root root      100 3月  19 00:37 yum
-rw-r--r--.  1 root root      970 10月  2 2020 yum.conf
drwxr-xr-x.  3 root root       36 3月  21 14:51 yum.repos.d
```

**详细介绍**

```shell
d    rwx   r-x     r-x.       6        root root               100        3月  19 00:37     yum
文件类型及权限     链接数    文件所属的用户和用户组     文件的大小    文档最后修改的日期   文件名称
```

下面通过具体的实例讲述每列代表的含义：

1） 第一列显示文档类型与执行权限，由10个字符组成，分为4部分。

- 文档类型部分： 

  ```shell
  d 表示目录
  l 表示软链接 
  - 表示文件
  c 表示串行端口字段设备文件
  b 表示可供存储的块设备文件
  ```

- user部分：

  ```shell
  对文档的所有者（user）权限的设定，rwx 表示用户对yum目录有读、写和执行的所有权限 7 7 7   rwx rwx rwx 
  ```

- group部分：

  ```shell
  对文档所属用户组（group）权限的设定，r-x 表用户组对 yum 目录有读和执行的权限
  ```

- others部分：

  ```shell
  对文档所有者之外的其他用户权限的设定，r-x 表示其他用户和用户组对 yum 目录读写权限
  ```

权限的对照表

| 数字 | 字符 | 文件                     | 文件夹                           |
| ---- | ---- | ------------------------ | -------------------------------- |
| 4    | r    | 查看文件内容             | 查看文件或者文件夹的名称         |
| 2    | w    | 修改文件内容             | 修改文件或者在文件夹内增、删、改 |
| 1    | x    | 可执行，一般为程序或脚本 | 可以用cd命令进入文件内           |

###### 什么是八进制值？ 

>  当 Linux 文件权限用数字表示时，这被称为数字模式。在数字模式下，一个三位数字值代表特定的文件权限（例如 744）。这些值被称为八进制值。第一位数字代表所有者权限，第二位数字代表组权限，第三位数字代表其他用户权限。每个权限都分配有一个数字值：

- r (read): 4 
- w (write): 2 
- x (execute): 1 

> 权限值744中，第一位对应用户，第二位对应组，第三位对应其他。将各个用户分类的值相加，即可得到文件的权限。

###### 权限计算

| 权限  | 二进制 | 八进制 |
| :---- | :----- | :----- |
| `r--` | `100`  | `4`    |
| `rw-` | `110`  | `6`    |
| `r-x` | `101`  | `5`    |
| `rwx` | `111`  | `7`    |

2） 第二列显示是文件及文件夹的链接数，这个链接就是硬链接的概念，即多少个文件指向同一inode。

举例：

```shell
[root@huawei ~]# ll
-rw-r--r-- 1 root root       139 Dec 13 20:47 test.py
[root@huawei ~]# ln test.py test.py2
[root@huawei ~]# ll
-rw-r--r-- 2 root root       139 Dec 13 20:47 test.py
-rw-r--r-- 2 root root       139 Dec 13 20:47 test.py2
[root@huawei ~]# ln test.py test.py3
-rw-r--r-- 3 root root       139 Dec 13 20:47 test.py
-rw-r--r-- 3 root root       139 Dec 13 20:47 test.py2
-rw-r--r-- 3 root root       139 Dec 13 20:47 test.py3
```

3）第三列显示文件及文件夹所属的用户和用户组，也就是文档属于哪个用户以及由那个用户组所有。

4）第四列显示的是文件及文件夹的大小，默认是以字节为单位，但是也可以同过命令的参数修改表示文档大小的单位。例如 `ls -sh`

5）第五列显示文档最后一次修改的日期，通常以月、日、时、分的方式显示。

6）第六列显示的是文件及文件夹的名称。

##### 2、利用chown改变属主和属组

chown就是change owner 的意思，主要作用就是改变文件或者目录的所有者，而所有者的包含用户和用户组。其实chown 就是对文件所属的用户和用户组进行的一系列配置。

**chown 语法如下**

```shell
chown [选项]... [所有者][:[组]] 文件...
chown [选项] 用户名：用户组名称 文件或者目录
```

**选项：**

```shell
-R  进行递归式更改，也就是将目录下的所有文件、子目录都更新为指定的用户组权限。
```

<font color=red>注意：在执行操作前，确保指定的用户以及用户组在系统中是存在的</font>

示例1：

```shell
#修改文件或者文件夹的所属者和所属组
[root@server ~]# chown nansheng: install
#修改文件及文件夹的所属者
[root@server ~]# chown nvsheng install  
#修改文件或者文件夹所属组
[root@server ~]# chown :tom install/
```

##### 3、利用chmod改变访问权限

**chmod**用户改变文件或目录的访问权限。

命令：chmod

语法：`chmod [选项]... 模式[,模式]... 文件...`

选项： --reference  #根据参考文档设置权限

​            -R  #递归将权限设置所有的子目录及文件

命令中个选项的含义如下：

> - u 表示 “用户”（user），即文件或目录的所有者。
> - g 表示“用户组”（group），即文件或目录所属的用户组。
> - o 表示“其他用户”（others）
> - a  表示 “所有用户”（all）,他是系统默认值。

操作符号含义如下：

> - "+" 表示添加某个权限
> - “-” 表示取消某个权限
> - “=”表示赋予给定的权限，同时取消文档以前的所有权限

模式 表示可以执行的权限，可以使只读 “r”、可写 “w”、可执行“x"以及它们的组合。

文件可以使以空格分开的文件列表，支持通配符。

示例1：修改install文件，使其所有者具有所有权限，用户组和其他用户具有只读权限。

```shell
[root@server ~]# ls -ls
0 drwxr-xr-x. 2 root root   16 3月  23 21:05 install
[root@server ~]# chmod u=rwx,g=r,o=r install/
[root@server ~]# ll
drwxr--r--. 2 root root   16 3月  23 21:05 install
```

示例2：修改/etc/fastab文件的权限，使其所有者具有读写权限，用户组和其他用户没有任何权限。

```shell
[root@server ~]# ll /etc/fstab 
-rw-r--r--. 1 root root 501 3月  19 00:36 /etc/fstab
[root@server ~]# ll /etc/fstab 
-rw-------. 1 root root 501 3月  19 00:36 /etc/fstab
```

##### 4、用户管理案例

> 模拟公司的组织结构，EXAMPLE 公司是一个从事教育培训的组织，公司结构主要分为教研室（teach）、校长办公室（office）、财务部（finance）、行政部（admin）、市场部（market）。为了后期部署文件共享服务器，我们需要为每个部门创建各自独立的共享目标，根据要求为所有的普通员工创建各自独立的账户，这些账户仅需要读权限即可，并为每个部门的负责人创建一个管理账户（管理名称为Op_部门名称），这个账户要求可以进行读写操作。为了后期管理方便，需要为每个部门创建于部门名称同名的组账户。

##### 5、linux 特殊权限

linux基本权限只是规定了所有者、属组、其他人三种用户的权限，如果希望对文件或文件夹做一些特殊的权限设置呢？
比如：

- 设置属组继承权限
- 为执行文件设置临时超管执行权限
- 公共文件夹中的文件谁建立谁删除
  这些任务基本权限就解决不了，需要解决这个问题得靠特殊权限。

> 特殊权限是除**用户** 、 **组**和**其他权限**之外的第四个访问级别。特殊权限允许在标准权限集之外拥有额外的权限（顾名思义）。之前讨论过的每个访问级别都有一个特殊权限选项。让我们逐一了解一下，从“设置 UID”开始：

###### 特殊权限的介绍

之前我们提到了特殊权限有三个，这三个特殊权限是在可执行程序运行时影响操作权限的，它们分别是SUID,SGID,sticky-bit位

| 特殊权限  | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| SUID      | 当一个设置了SUID 位的可执行文件被执行时，该文件将以所有者的身份运行，也就是说无论谁来执行这个文件，他都有文件所有者的特权。任意存取该文件拥有者能使用的全部系统资源。如果所有者是 root 的话，那么执行人就有超级用户的特权了。 |
| SGID      | 当一个设置了SGID 位的可执行文件运行时，该文件将具有所属组的特权，任意存取整个组所能使用的系统资源；若一个目录设置了SGID，则所有被复制到这个目录下的文件，其所属的组都会被重设为和这个目录一样，除非在复制文件时保留文件属性，才能保留原来所属的群组设置。 |
| stickybit | 对一个文件设置了sticky-bit之后，尽管其他用户有写权限，也必须由属主执行删除、移动等操作；对一个目录设置了sticky-bit之后，存放在该目录的文件仅准许其属主执行删除、移动等操作。 |

- user + s (特殊)

通常记为 **SUID** ，这是用户访问级别的特殊权限，它只有一个作用：具有 **SUID** 的文件始终以文件所有者的用户身份执行，无论传递命令的用户是谁。如果文件所有者没有执行权限，则在此处使用大写字母 **S。**

现在，为了从实际角度理解这一点，我们来看一下 `/usr/bin/passwd` 命令。默认情况下，此命令具有 SUID 权限设置：

```shell
[root@server ~]# ls -l /usr/bin/passwd 
-rwsr-xr-x. 1 root root 33544 Dec 13  2019 /usr/bin/passwd
```

- 组 + s（特殊）

此特殊权限通常称为 **SGID** ，它具有以下几个功能：	

> 如果在文件上设置，则允许以拥有该文件的**组**身份执行该文件（类似于 SUID）
>
> 如果在目录中设置，则在该目录中创建的任何文件的**组**所有权都将设置为目录所有者的组所有权

```shell
[root@server]# ls -l 
total 0
drwxrws---. 2 root root  69 Apr  7 11:31 my_file
```

> 此权限集以小写字母 **s** 表示，其中 **x** 通常表示**组**的**执行**权限。它对于组成员之间经常协作使用的目录也特别有用。组中的任何成员都可以访问任何新文件。这也适用于文件的执行。如果使用得当， **SGID** 非常强大。
>
> 如前所述，对于 **SUID** ，如果拥有组没有执行权限，则使用大写 **S。**

- other + t (粘性)

最后一项特殊权限被称为“粘滞位”。此权限不会影响单个文件。但是，在目录级别，它限制文件的删除。只有文件的**所有者** （即 **root** ）才能删除该目录中的文件。一个常见的例子是 `/tmp` 目录：

```shell
[root@server]$ ls -ld /tmp/
drwxrwxrwt. 15 root root 4096 Sep 22 15:28 /tmp/
```

权限集用小写字母 **t** 表示，其中 **x** 通常表示执行权限。

###### 特殊权限的设置和查看

设置特殊权限

要对文件或目录设置特殊权限，您可以使用上面概述的标准权限的两种方法之一：符号或数字

- 假设我们想要在目录 `file` 上设置 **SGID**

  > 为了使用符号方法做到这一点，我们执行以下操作：

  ```shell
  [root@server ]$ chmod g+s file/
  ```

使用数值方法时，我们需要在 `chmod` 命令中传递第四位数字。该数字的计算方式与标准权限数字类似：

- Start at 0 
- SUID = 4 
- SGID = 2
- Sticky = 1

语法：

```shell
[root@server ~]$ chmod X### file | directory
```

> 其中 **X** 是特殊权限数字

以下是使用数值方法在 `file` 上设置 **SGID 的**命令：

```shell
[root@server]$ chmod 2770 file/
[root@server]$ ls -ld file/
drwxrws---. 2 root root 113 Apr  7 11:32 file/
```



##### ACL 访问控制权限

由于系统的基本权限是针对文档所有者、所属组或其他账户进行控制的，无法针对某个单独的账户进行控制，所以就有了ACL（Account Control List）访问控制列表的概念，使用ACL，我们可以针对单一用户设置文档的访问权限。

linux系统使用getfacl查看文档ACL权限，使用setfacl来设置文档的ACL权限。

###### ACL权限的设置和查看

如果要使用acl权限，首先要确定你的文件系统支持acl权限,如果再Default mount options字段出现acl字样就意味着你的文件系统支持acl，不过在CentOS8中默认是都支持的。

- **setfacl命令：设置文件或文件夹的ACL权限**

  ```shell
  命令选项：
  -m: #设置acl
  -x: #删除指定的acl
  -b: #删除所有的acl
  -k: #删除默认的ACL
  -R: #递归处理所有的子文件与子目录
  ```

- **getfacl命令：用来查看文件的acl权限**
  现在我们来看下如何设置

  ```shell
  [root@server test]# ll
  总用量 0
  drwxr-xr-x. 2 root root 6 3月  23 23:03 file3
  [root@server test]# setfacl -m u:tom:rwx file3
  [root@server test]# ll
  总用量 0
  drwxrwxr-x+ 2 root root 6 3月  23 23:03 file3
  
  ```

  你会发现使用ll（等同于ls -l）命令查看时会发现多了一个+号，这只是提醒我们此文件被设置了acl权限，但是具体是什么样的，我们还需要使用getfacl来查看

  ```shell
  [root@server test]# getfacl file3/
  # file: file3/
  # owner: root    所有者
  # group: root    所有者组
  user::rwx        user:后面是空的，代表是是所有者的权限
  user:tom:rwx     我们之前给额为用户设置的权限
  group::r-x       所有者组的权限
  mask::rwx        默认的有效权限
  other::r-x       其他人的权限
  ```

  以上是我们针对一个额外的用户设置的权限，同理可以设置针对组和其他人的acl权限

- **为不同用户或组设置不同权限**

  ```shell
  [root@server test]# setfacl -m g:tom:rw file3
  [root@server test]# setfacl -m o:rw file3
  [root@server test]# setfacl -m u:tom:rw file3/
  
  [root@server test]# getfacl file3
  # file: file3
  # owner: root
  # group: root
  user::rwx
  user:tom:rw-
  group::r-x
  group:tom:rw-
  mask::rwx
  other::rw-
  ```

- **删除指定的acl**

  ```shell
  #删除acl用户
  [root@server test]# setfacl -x u:tom file3
  [root@server test]# getfacl file3/
  # file: file3/
  # owner: root
  # group: root
  user::rwx
  group::r-x
  group:tom:rw-
  mask::rwx
  other::rw-
  
  #删除ACL组
  [root@server test]# setfacl -x g:tom file3/
  [root@server test]# getfacl file3/
  # file: file3/
  # owner: root
  # group: root
  user::rwx
  group::r-x
  mask::r-x
  other::rw-
  
  
  #删除其他人的权限直接使用chmod就可以
  [root@server test]# chmod o=r file3/
  [root@server test]# getfacl file3/
  # file: file3/
  # owner: root
  # group: root
  user::rwx
  group::r-x
  mask::r-x
  other::r--
  ```

- **删除所有acl**

  ```shell
  [root@server test]# setfacl -m u:tom:rw,g:tom:rw,o:rwx file3/
  [root@server test]# getfacl file3
  # file: file3
  # owner: root
  # group: root
  user::rwx
  user:tom:rw-
  group::r-x
  group:tom:rw-
  mask::rwx
  other::rwx
  
  [root@server test]# setfacl -b file3/
  [root@server test]# getfacl file3
  # file: file3
  # owner: root
  # group: root
  user::rwx
  group::r-x
  other::rwx
  ```