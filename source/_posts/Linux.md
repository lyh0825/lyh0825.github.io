---
title: Linux
date: 2021-07-18 13:45:13
tags: Linux
---

## 安装

1. 官网上获得版本偶数版为稳定版，奇数版位测试版
2. 下载VMware14pro，安装码百度即可
3. 选择编辑添加新虚拟机
4. 其中虚拟机设置里将CD/DVD 2（IED）设置为`centOS-8-×86_64-1905-dcd.iso`的路径

如果开始时出现

**Pane is dead.窗格已死，解决方法为：**

​	1、把内存从1G升到2G，不管用。

​	2、按住 Ctrl+alt+F2， 然后输出 reboot， 依然不行。

​	3、更改ISO默认的映像文件，【step4】

安装时选择工作站版即可

##   Linux基础

## linux指令

### 切换用户

**用户标识**

- $表示普通用户
- #表示超级用户，也就是root用户

**0.创建common user$**

```shell
useradd username
```

**1.切换common user$**

```shell
su username
```

**2.切换到root#**

```shell
sudo su
```

**【用户操作】**

1. 在终端输入exit或logout或使用快捷方式ctrl+d，可以退回到原来用户
2. 在切换用户时，如果想在切换用户之后使用新用户的工作环境，可以在su和username之间加-，例如：【su - root】

### 查看进程

**【静态查看进程信息】**

ps命令

- ps -aux

- ps -elf

信息更详细些，包括 PPID (对应的父进程 的PID 号)

**【动态查看进程信息】**

top命令

`M: sorted by memory`

`N: sorted by time`

`p: sorted by GPU`

`q: 退出`

### **查看信息**

**查看系统公网ip**

```shell
# 返回详细信息, 服务器地址-client地址-数据来源
curl cip.cc

# 返回地址
curl ifconfig.io
curl ipconfig.io
curl ifconfig.me

# 从系统文件中查看地址
ifconfig -a
-etho0
```

**查看系统内核**

`uname -r`

**查看系统版本**

`cat /etc/os-release`

**用户与主机名**

```shell
# [root@localhost]
root: 当前登陆的用户
localhost: 当前的主机名

# 修改主机名
1.临时修改
sudo hostname <new-hostname>

2.永久修改 命令修改|配置修改
> sudo hostnamectl set-hostname <newhostname>
> vim手动修改/etc/hostname文件

# 查看当前主机名
hostnamectl

# 修改之后重启生效
reboot
```

**退出|关机|重启linux**

```bash
退出:
exit

关机命令：
1、halt 立刻关机
2、poweroff 立刻关机
3、shutdown -h now 立刻关机(root用户使用)
4、shutdown -h 10 10分钟后自动关机 如果是通过shutdown命令设置关机的话，可以用shutdown -c命令取消重启
5、init0 停机或者关机

重启命令：
1、reboot
2、shutdown -r now 立刻重启(root用户使用)
3、shutdown -r 10 过10分钟自动重启(root用户使用)
4、init6 重启

init一共分为7个级别，这7个级别的所代表的含义如下
0：停机或者关机（千万不能将initdefault设置为0）
1：单用户模式，只root用户进行维护
2：多用户模式，不能使用NFS(Net File System)
3：完全多用户模式（标准的运行级别）
4：安全模式
5：图形化（即图形界面）
6：重启（千万不要把initdefault设置为6）
```

### 查找文件

- [x] whereis

#### **whereis**

用于查找文件, 返回文件所在路径

- 文件应属于原始代码
- 二进制文件
- 帮助文件

该指令只能用于查找二进制文件、源代码文件和man手册页，一般文件的定位需使用locate命令。

### 文件操作

#### **cp**

> 复制文件

```
cp [options] source dest
```

- -a：此选项通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于dpR参数组合。
- -d：复制时保留链接。这里所说的链接相当于 Windows 系统中的快捷方式。
- -f：覆盖已经存在的目标文件而不给出提示。
- -i：与 **-f** 选项相反，在覆盖目标文件之前给出提示，要求用户确认是否覆盖，回答 **y** 时目标文件将被覆盖。
- -p：除复制文件的内容外，还把修改时间和访问权限也复制到新文件中。
- **-r：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。**
- -l：不复制文件，只是生成链接文件。

### yum命令

> yum（ Yellow dog Updater, Modified）是一个在 Fedora 和 RedHat 以及 SUSE 中的 Shell 前端软件包管理器。

基于 RPM 包管理，能够从指定的服务器自动下载 RPM 包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装。

### SSH(Secure Shell)

- 从CVM中获取`rsa_pub.pem`
- 将`rsa_pub.pem`保存在client的`./ssh`目录下
- 从client`./ssh`中获取`rsa_pub`
- 将`rsa_pub`中的内容放在CVM的`/root/.ssh/authorized_keys`中

```shell
# client 
ssh root@ip

# ori
ssh -i ~/.ssh/rsa_CVM.pub root@CVM.ip
```

-----

有关ssh的两个文件

- 一个是存放client_rsa.pub
- 一个是修改服务端和客户端ssh设置的
  - etc/ssh/ssh_config
  - etc/ssh/sshd_config

`/root/.ssh/authorized_keys`

**远程主机将用户的公钥，保存在此**

- 存放访问的client的pub_rsa

----

`/etc/ssh/sshd_config`

`/etc/ssh/ssh_config`

- ssh_config是针对客户端的配置文件

- sshd_config是针对服务器的配置文件

**ssh服务配置文件**

**文件修改后使用:**

`service sshd restart `保存

### systemctl命令

- [systemctl]**system controller** 

### sudo

super user do: 让普通用户执行只有root用户才可以执行的权限任务

### curl

就是模拟数据传输的一个库 可以抓取网页内容 也可传输内容给网页

