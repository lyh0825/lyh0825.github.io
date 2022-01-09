---
title: Question
date: 2021-10-07 19:39:08
tags: Drink
---

## Q7Docker run后自动stop

### -Info

>在docker run xxxcontainer后, docker ps查看容器发现已经停止

### -Analysis

1. docker容器运行必须有一个前台进程， 如果没有前台进程执行，容器认为空闲，就会自行退出
2. 容器运行的命令如果不是那些一直挂起的命令（ 运行top，tail、循环等），就是会自动退出
3. 这个是 docker 的机制问题

### -Solution

1. 在run命令时添加挂起命令tail

```docker
docker run -d -it --name mysql0 mysql tail -f dev/null
```

2. 在dockerfile中添加命令

```dockerfile
FROM centos
MAINTAINER <DRINK 1982164667@QQ.COM>
...
ENTRYPOINT ["tail"]
CMD ["-f", "dev/null"]
```

### -Review

- dev/null 文件通常用于丢弃不需要的数据输出， 或用于输入流的空文件

## Q6Docker链接失败

### -Info

使用`docker ps`出现

```shell
docker Cannot connect to the Docker daemon at unix:///var/run/docker.sock.
```

### -Analysis

此时已确定Docker本身已经安装正常。 问题原因是因为docker服务没有启动，所以在相应的/var/run/ 路径下找不到docker的进程。 

### -Solution

执行`service docker start `命令，启动docker服务，返回`docker start/running, process 2662` 此时进程启动成功，再执行docker ps，问题解决

## Q5SSH连接VCM断开

### -Question

**报错信息**

**info1**

client_loop: send disconnect: Broken pipe

**info2**

yum-config-manager client_loop: send disconnect: Broken pipe

### -Analysis

**info1**

终端连接服务器无响应且没用自动间隔连接配置**VCM的问题**

### -Solution

**info1**

修改`/etc/ssh/sshd_config`文件, 修改`ClientAliveInterval 120`

```bash
# 通过每 300 秒（每 5 分钟）进行响应确认来保持连接。 我认为这个部分的数字可以是60或120。
>/etc/ssh/sshd_config
#PermitUserEnvironment no
#Compression delayed
ClientAliveInterval 60
#ClientAliveCountMax 3
```

重新启动ssh服务

```bash
systemctl restart sshd
```

或者在客户端进行更改`/etc/ssh/ssh_config`

```shell
# 添加参数
TCPKeepAlive yes
ServerAliveInterval 60
```

**info2**

配置`/etc/ssh/ssh_config`文件，增加以下内容即可：

```bash
Host *
		IPQoS=throughput
        # 断开时重试连接的次数
        ServerAliveCountMax 5

        # 每隔5秒自动发送一个空的请求以保持连接
        ServerAliveInterval 5
```

重新启动ssh服务

```bash
systemctl restart sshd
```

## -Review

**1.在服务端进行修改**

在服务器端， 可以让服务器发送“心跳”信号测试提醒客户端进行保持连接

通过修改 sshd 的配置文件，能够让 SSH Server 发送“心跳”信号来维持持续连接，下面是设置的内容

打开服务器 `/etc/ssh/sshd_config`，我在最后增加一行

```
ClientAliveInterval 60
ClientAliveCountMax 1
```

这样，SSH Server 每 60 秒就会自动发送一个信号给 Client，而等待 Client 回应，（注意：是服务器发心跳信号，不是客户端，这个有别于一些 FTP Client 发送的 KeepAlives 信号），如果客户端没有回应，会记录下来直到记录数超过 ClientAliveCountMax 的值时，才会断开连接。

**2.在客户端进行修改**

只要在/etc/ssh/ssh_config文件里加两个参数

```
TCPKeepAlive yes
ServerAliveInterval 60
```

前一个参数是说要保持连接，后一个参数表示每过1分钟发一个数据包到服务器表示“我还活着”

如果你没有root权限，修改或者创建~/.ssh/ssh_config也是可以的

## Q4vim安装失败

### -Question

1. 在使用`vim`时发现报错`bash: vim: command not found`

### -Analysis

使用`cat /proc/version`查看系统版本

![image-20210928202753365](/Users/edy/Library/Application Support/typora-user-images/image-20210928202753365.png)

查看系统镜像中是否有`vim`, 发现`red hat 5`才有`vim`

### -Solution

- 使用`apt install vim `安装`vim`
- 发现`Unable to locate package vim`

![image-20210928203059115](/Users/edy/Library/Application Support/typora-user-images/image-20210928203059115.png)

- 需要输入：`apt-get update`（作用是：同步 /etc/apt/sources.list 和 /etc/apt/sources.list.d 中列出的源的索引，这样才能获取到最新的软件包）
- 之后再输入`apt-get install vim`安装（非root用户登录，`root apt-get intall vim`）

### -Review

**docker常用命令**

```shell
# 修改了image内的文件后重启image
docker restart image_id

# 启动container中的所有image
docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)

# 停止container中的所有image
docker stop $(docker ps -a | awk '{ print $1}' | tail -n +2)

# 删除container中的所有image
docker rm $(docker ps -a | awk '{ print $1}' | tail -n +2)
```

## Q3镜像部署失败

### -Question

1. 部署对应的tag的时候出现`error`

![image-20210928155639667](/Users/edy/Library/Application Support/typora-user-images/image-20210928155639667.png)

2. 有正在运行的container, 无法部署

### -Analysis

- 在部署的过程中存在正在运行的container
- 需要将所有的正在运行的container停下来或者删除所有正在运行的container中的image

### -Solution

```bash
# 启动所有imagedocker start $(docker ps -a | awk '{ print $1}' | tail -n +2)# 停止运行所有imagedocker stop $(docker ps -a | awk '{ print $1}' | tail -n +2)# 删除container中的所有imagedocker rm $(docker ps -a | awk '{ print $1}' | tail -n +2)# 重新部署sh deploy-debug.sh 1.0.0.65
```

### -Review

- 使用`docker ps -a`container中的所有image包括正在运行和未运行的
- `docker rmi -f 'docker iamges' `删除的已经存在但未运行的image
- `docker rm image_id`删除正在运行的image

## Q2镜像重启

### -Question

1. Docker ps 查看镜像状态显示`restart`

![image-20210928095953991](/Users/edy/Library/Application Support/typora-user-images/image-20210928095953991.png)

2. RabbitMQ只有生产者没有消费者

### -Analysis

1. 部分服务器没起来

### -Solution

1. `docker logs -f image_name `查看对应的image日志
2. 找到报错信息

![image-20210928095931103](/Users/edy/Library/Application Support/typora-user-images/image-20210928095931103.png)

`cerely can't find module name 'component.task.member_change_task.component'`

**cerely配置错误**

### -Review

- docker logs -f 
- command r 搜索已经使用的order

## Q1磁盘溢出

### -**Question**

1. mongodb连接失败报错
   - `failed to read 4 bytes from socket within 300000 milliseconds`
2. 紫豆超管后台登陆失败
   - `login 无响应`
3. 机器人正常但不可用
   - `机器人上报heartbeat给服务器但服务器不callback`

### -Analysis

1. 登陆紫豆服务器`ssh root@ip`
   - 报错显示disk overflower
2. `df -h`查看disk存储情况

![image-20210926170428181](file:///Users/edy/Library/Application%20Support/typora-user-images/image-20210926170428181.png?lastModify=1632625187)

### **-Solution**

1. 删除不用的container|容器修剪

   - PRO环境具有一定的风险
   - `docker system prune`  `docker container prune`

   ```dockerfile
   docker container pruneWARNING! This will remove all stopped containers.Are you sure you want to continue? [y/N] yDeleted Containers:4a7f7eebae0f63178aff7eb0aa39cd3f0627a203ab2df258c1a00b456cf20063f98f9c2aa1eaf727e4ec9c0283bc7d4aa4762fbdba7f26191f26c97f64090360Total reclaimed space: 212 B
   ```

2. **删除镜像[优先选择]**

   - ```bahs
     docker rmi -f `docker images`
     ```

   - docker image需`

   - **删除已经存在的镜像「不包括正在运行的」**

### -Review

**docker rmi :** 删除本地一个或多个镜像。

```docker
docker rmi [OPTIONS] IMAGE [IMAGE...]
```

**OPTIONS说明：**

- **-f :**强制删除；
- **--no-prune :**不移除该镜像的过程镜像，默认移除；

----

**docker prune 命令**

prune 命令用来删除不再使用的 docker 对象。

删除所有未被 tag 标记和未被容器使用的镜像:

```bash
$ docker image pruneWARNING! This will remove all dangling images.Are you sure you want to continue? [y/N] y
```

删除所有未被容器使用的镜像:

```bash
$ docker image prune -a
```

删除所有停止运行的容器:

```bash
$ docker container prune
```

删除所有未被挂载的卷:

```bash
$ docker volume prune
```

删除所有网络:

```bash
$ docker network prune
```

删除 docker 所有资源:

```bash
$ docker system prune
```

**docker system prune** 命令：

```
This will remove:        
- all stopped containers        
- all networks not used by at least one container        
- all dangling images        
- all dangling build cache
```

删除停止的容器、删除所有未被容器使用的网络、删除所有none的镜像。

