---
title: 记一次 RocketMQ broker 因内存不足导致的启动失败
date: 2021-01-12
updated: 2021-01-12
categories:
- RocketMQ
tags:
- 消息中间件
- RocketMQ
---

## 背景

**该小节交代问题发生的背景，急需解决问题的小伙伴，可以跳过本节，直接看下一小节**。

因为项目提测，需要搭建一套测试环境。所以呢，是时候展示真正的技术啦！在搞定了容器、中间件、项目镜像后，小西登录系统对各大模块的功能进行测试。事情到了这里，小西本来应该会就这样愉快地完成了部署任务，可是生活总是会给你带来意想不到的“惊喜”。

- 在测试一类预警事件消息时，忽然发现压根没有消息，就去 RocketMQ 的控制台界面查看，发现控制台原本应该乖乖被监控的 broker 一个都不在了。

- 在不考虑 broker 不会自己罢工跑掉的情况下，登录服务器查看 broker 服务，发现服务没有启动成功。

- 再查看 broker 的启动日志，发现启动报错了。

于是，就有了这篇分享。

---

<!--more-->

---

## 部署环境

操作系统：centos7 linux 系统

部署方式：docker 容器 + docker-compose 容器编排

部署版本：RocketMQ 4.4.0

## 问题描述

开发环境访问 RocketMQ 控制台，发现 broker 服务宕机。登录服务器查看日志发现以下报错：

```
Java HotSpot(TM) 64-Bit Server VM warning: INFO: os::commit_memory(0x00000000c0000000, 7163871232, 0) failed; error=
 ...
#
# There is insufficient memory for the Java Runtime Environment to continue.
# Native memory allocation (mmap) failed to map 7163871232 bytes for Failed to commit area from 0x00000000c0000000 to
 ...
```

提示内存分配无法满足 7163871232 字节的需求。那为什么会出现这个问题呢？

## 问题定位

### 重启broker

刚开始没有排查日志时，以为环境被人停掉了，所以对 broker 进行了重启。

```shell
[root@172-30-1-135 nginx]# docker-compose restart
```

发现 broker 启动依旧失败，而 namesrv 和 console 启动正常。

### 分析启动脚本

登录 RocketMQ 的 docker 容器。

注意：**因为 broker 无法启动，使用 docker exec 是无法进入容器的，需要使用 docker run 命令进入容器**。

```shell
[root@37-128-28-177 nginx]# docker run -it rocketmqinc/rocketmq:4.4.0 bash
```

查看启动脚本 broker.sh

```shell
[rocketmq@38bc66dd72c3 bin]$ vi runbroker.sh
```

发现 runbroker.sh 启动脚本中有最大允许堆内存的配置项 `MAX_POSSIBLE_HEAP` 。

```shell
...
# Get the max heap used by a jvm, which used all the ram available to the container.
if [ -z "$MAX_POSSIBLE_HEAP" ]
then
        MAX_POSSIBLE_RAM_STR=$(java -XX:+UnlockExperimentalVMOptions -XX:MaxRAMFraction=1 -XshowSettings:vm -version |& awk '/Max\. Heap Size \(Estimated\): [0-9KMG]+/{ print $5}')
        MAX_POSSIBLE_RAM=$MAX_POSSIBLE_RAM_STR
        CAL_UNIT=${MAX_POSSIBLE_RAM_STR: -1}
        if [ "$CAL_UNIT" == "G" -o "$CAL_UNIT" == "g" ]; then
                MAX_POSSIBLE_RAM=$(echo ${MAX_POSSIBLE_RAM_STR:0:${#MAX_POSSIBLE_RAM_STR}-1} `expr 1 \* 1024 \* 1024 \* 1024` | awk '{printf "%d",$1*$2}')
        elif [ "$CAL_UNIT" == "M" -o "$CAL_UNIT" == "m" ]; then
                MAX_POSSIBLE_RAM=$(echo ${MAX_POSSIBLE_RAM_STR:0:${#MAX_POSSIBLE_RAM_STR}-1} `expr 1 \* 1024 \* 1024` | awk '{printf "%d",$1*$2}')
        elif [ "$CAL_UNIT" == "K" -o "$CAL_UNIT" == "k" ]; then
                MAX_POSSIBLE_RAM=$(echo ${MAX_POSSIBLE_RAM_STR:0:${#MAX_POSSIBLE_RAM_STR}-1} `expr 1 \* 1024` | awk '{printf "%d",$1*$2}')
        fi
        MAX_POSSIBLE_HEAP=$[MAX_POSSIBLE_RAM/4]
fi

# Dynamically calculate parameters, for reference.
Xms=$MAX_POSSIBLE_HEAP
Xmx=$MAX_POSSIBLE_HEAP
Xmn=$[MAX_POSSIBLE_HEAP/2]
...
```

从脚本中可以看出，在 runborker.sh 脚本中， `MAX_POSSIBLE_HEAP` 参数值会通过参数进行设置，而如果没有任何设置就会走下面这个判断：

```shell
MAX_POSSIBLE_HEAP=$[MAX_POSSIBLE_RAM/4]
```

也就是说 `MAX_POSSIBLE_HEAP` 参数如果没有指定，它会使用四分之一的最大可用内存 `MAX_POSSIBLE_RAM` ，这一机制可以保护服务器的操作系统不会因为被服务占据全部内存而无法正常运行。但当服务器的可用内存较小时，这个四分之一对于 RocketMQ 来说就有些“捉襟见肘”了。所以，也就导致了 RocketMQ 因内存不足而无法启动。

分析出原因以后，就可以考虑通过**显式指定参数**的方式解决这个问题。

## 解决方案

### 方案一：修改最大堆内存

退出 docker 容器，修改 RocketMQ 服务 `docker-compose.yml` 文件，给 broker 指定 `MAX_POSSIBLE_HEAP` 参数，指定为 `1024m`

```shell
broker:
    image: rocketmqinc/rocketmq:4.4.0
    container_name: rmqbroker
    ports:
      - 10909:10909
      - 10911:10911
      - 10912:10912
    volumes:
      - /data/admin/app/yunying/mq/logs/broker:/home/rocketmq/logs
      - /data/admin/app/yunying/mq/broker:/home/rocketmq/store
      - /data/admin/app/yunying/mq/broker.conf:/opt/rocketmq-4.4.0/conf/broker.conf
    command: sh  mqbroker -n 172.30.1.135:9876 -c /opt/rocketmq-4.4.0/conf/broker.conf
    depends_on:
      - namesrv
    environment:
      - "autoCreateTopicEnable=true"
      - "JAVA_HOME=/usr/lib/jvm/jre"
      # 指定堆内存大小
      - "MAX_POSSIBLE_HEAP=1024m"
      - TZ=Asia/Shanghai
```

重启 broker。查看日志，发现以下报错。

```
/opt/rocketmq-4.4.0/bin/runbroker.sh: line 58: 1024m: value too great for base (error token is "1024m")
1
```

由于原始问题报错信息中的单位是 bytes，考虑到参数单位可能与 JVM 内存设置参数不同，再次修改堆内存配置。

重启 broker，启动成功。

```shell
[admin@zw-yunying-172.30.1.135 mq]$ docker logs -f --tail 10 rmqbroker
The broker[broker-a, 172.30.1.135:10911] boot success. serializeType=JSON and name server is 172.30.1.135:9876
```

至此，问题解决。

### 方案二：修改JVM元空间大小

本方案是网上查找资料发现的解决方案，报错问题类似但不完全一致。该方案没有做验证，不确定是否能够解决该问题。

感兴趣的小伙伴可以验证一下，下面是问题描述和解决方案。

问题描述为：

```
JRE version: (8.0_172-b11) (build )
Java VM: Java HotSpot(TM) 64-Bit Server VM (25.172-b11 mixed mode linux-amd64 compressed oops)
Java运行时环境的内存不足，无法继续，本机内存分配（mmap）未能映射8589934592字节，用于提交保留内存
```

解决方案如下：

找到 runserver.sh 和 runbroker.sh，编辑

```shell
JAVA_OPT=”${JAVA_OPT} -server -Xms256m -Xmx1024m -Xmn125m -XX:MetaspaceSize=1024m -XX:MaxMetaspaceSize=1024m”
1
```



## 参考资料

- [搭建RocketMQ踩的坑-内存不足](https://blog.csdn.net/u014362882/article/details/80422136) 