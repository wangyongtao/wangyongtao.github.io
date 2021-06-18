---
title: 消息队列 RabbitMQ 的安装与配置(Docker)
date: 2018-02-08T17:00:00+08:00
# description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: WYT
authorEmoji: 🧑
tags:
- Golang
categories:
- themes
- QuickStart
series:
- Themes Guide
image: images/common/golang-1.jpeg
---

消息队列 RabbitMQ 的安装与配置(Docker)


打开 [RabbitMQ](http://www.rabbitmq.com/) 官网, 点击进入 [Docs] 菜单，我们可以看到有多个平台的不同安装文档介绍，我们只要选择和我们系统平台对应的即可。

在本文中，我们选择使用 [Docker image](https://hub.docker.com/_/rabbitmq/) 来安装。

下面我们来走一遍流程。

## 环境说明

虚拟机软件使用的 VirtualBox，使用的是名为 vbox-test 的一个 Docker 虚拟机， IP是 192.168.99.100:

查看Docker 虚拟机信息: 

``` 
// 使用“docker-machine ls”列出虚拟机
$ docker-machine ls
NAME       ACTIVE DRIVER      STATE   URL                        SWARM   DOCKER       ERRORS
vbox-test  -      virtualbox  Running tcp://192.168.99.100:2376          v18.03.1-ce

// 查看我的虚拟机ip: 
$ docker-machine ip vbox-test
192.168.99.100
```

## 登录虚拟机

通过 `docker-machine ssh` 命令登录到 `vbox-test` 虚拟机中:

```sh
$ docker-machine ssh vbox-test
                        ##         .
                  ## ## ##        ==
               ## ## ## ## ##    ===
           /"""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
           \______ o           __/
             \    \         __/
              \____\_______/
 _                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_|
Boot2Docker version 18.03.1-ce, build HEAD : cb77972 - Thu Apr 26 16:40:36 UTC 2018
Docker version 18.03.1-ce, build 9ee9f40
```

查看目前运行中的容器，我这里还有没运行任何容器:

```
docker@vbox-test:~$ docker ps
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
```

查看镜像列表, 我之前已经拉取过了，已经有 rabbitmq 镜像了:

```
docker@vbox-test:~$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
base/archlinux      latest              6f30e92a4162        3 days ago          541MB
redis               alpine              d3117424aaee        4 days ago          27.1MB
rabbitmq            3-management        a15563f1e21c        9 days ago          151MB
rabbitmq            3                   72cee1616e73        9 days ago          127MB
```
使用 docker pull rabbitmq 命令来拉取或更新 rabbitmq 镜像:

docker@vbox-test:~$ docker pull rabbitmq
Using default tag: latest
latest: Pulling from library/rabbitmq
Digest: sha256:2cd00c31d2e443ab170dac2b3b37ef85fc09effb0c2528f8e81a806ccc3d9455
Status: Image is up to date for rabbitmq:latest
使用 docker run 命令运行 rabbitmq 镜像:

使用 -d 参数来指定后台运行rabbitmq, 并打印出容器的ID (container ID)
使用 --hostname 参数来指定 主机名为： my-rabbit
使用 --name 参数来指定 容器名为：some-rabbit
docker@vbox-test:~$ docker run -d --hostname my-rabbit --name some-rabbit rabbitmq
9f555cba9297c165e1376cfeec5396236376bfeb7858f108d8119c81e5061fac
docker@vbox-test:~$ docker ps
CONTAINER ID  IMAGE     COMMAND                 CREATED         STATUS         PORTS                               NAMES
9f555cba9297  rabbitmq  "docker-entrypoint.s…"  18 seconds ago  Up 18 seconds  4369/tcp, 5671-5672/tcp, 25672/tcp  some-rabbit
查看启动日志 docker logs some-rabbit

$ docker logs some-rabbit
2018-02-09 08:55:33.500 [info] <0.33.0> Application lager started on node 'rabbit@my-rabbit'
... ...
2018-02-09 08:55:34.265 [info] <0.183.0> 
 Starting RabbitMQ 3.7.3 on Erlang 20.1.7
 Copyright (C) 2007-2018 Pivotal Software, Inc.
 Licensed under the MPL.  See http://www.rabbitmq.com/
  ##  ##      RabbitMQ 3.7.3. Copyright (C) 2007-2018 Pivotal Software, Inc.
  ##########  Licensed under the MPL.  See http://www.rabbitmq.com/
  ##########  Logs: <stdout>
Starting broker...
2018-02-09 08:55:34.306 [info] <0.183.0> 
 node           : rabbit@my-rabbit
 home dir       : /var/lib/rabbitmq
 config file(s) : /etc/rabbitmq/rabbitmq.conf
 cookie hash    : YZ+Mu7/Mufu5lw/I9DXYBw==
 log(s)         : <stdout>
 database dir   : /var/lib/rabbitmq/mnesia/rabbit@my-rabbit
... ... 
可以看到节点(node)的名称: rabbit@my-rabbit, 配置文件路径: /etc/rabbitmq/rabbitmq.conf 等信息。

停止容器运行，删除容器:

docker@vbox-test:~$ docker stop some-rabbit
some-rabbit
docker@vbox-test:~$ docker rm some-rabbit
some-rabbit
拉取 rabbitmq:3-management 镜像，这个镜像带管理后台:

docker@vbox-test:~$ docker pull rabbitmq:3-management
3-management: Pulling from library/rabbitmq
Digest: sha256:ce1f6f597861a0e11942ad30415b192379a69b491a5899eead98abcf88430e31
Status: Image is up to date for rabbitmq:3-management
使用docker run命令运行 rabbitmq:3-management 镜像:

使用 -p 参考来映射当前端口号和docker容器的端口号, 我们启动的名为 some-rabbit 容器的管理后台默认登录端口是 15672， 我们给映射成 8080， 这样就可以通过 8080 来访问了。

// 运行容器
docker@vbox-test:~$ docker run -d --hostname my-rabbit --name some-rabbit -p 8080:15672 rabbitmq:3-management
add2359445d8c38053ca9635ac75fb08f18e229c487ac30356d99802ebcff699

// 查看运行的容器 
docker@vbox-test:~$ docker ps
CONTAINER ID IMAGE                 COMMAND                CREATED       STATUS                  PORTS                               NAMES
add2359445d8 rabbitmq:3-management "docker-entrypoint.s…" About an hour ago   Up About an hour  4369/tcp, 5671-5672/tcp, 15671/tcp, 25672/tcp, 0.0.0.0:8080->15672/tcp  some-rabbit
打开浏览器，输入地址: http://localhost:8080 或者 http://host-ip:8080

由于我是在虚拟机(vbox-test)中安装的，虚拟机ip是 192.168.99.100, 打开地址: http://192.168.99.100:8080/ 输入账号密码: guest / guest， 即可登录管理后台了。

使用 docker exec 命令，登录到 some-rabbit 容器中:

docker@vbox-test:~$ docker exec -it some-rabbit /bin/bash
root@my-rabbit:/# ls
bin   docker-entrypoint.sh  lib    mnt      proc  sbin  tmp
boot  etc                   lib64  opt      root  srv   usr
dev   home                  media  plugins  run   sys   var
使用 rabbitmqctl 命令，对队列进行管理：

列出所有的用户: /usr/lib/rabbitmq/bin/rabbitmqctl -n rabbit@my-rabbit list_users

rabbitmqctl 使用管理 RabbitMQ 的命令行工具, 默认路径为: /usr/lib/rabbitmq/bin/rabbitmqctl
my-rabbit 使我们启动容器指定的 hostname
list_users 是列出所有用户的命令，详见rabbitmqctl

// 已经进入到容器里了，列出所有的用户
root@my-rabbit:/# 
root@my-rabbit:~# /usr/lib/rabbitmq/bin/rabbitmqctl -n rabbit@my-rabbit list_users
Listing users ...
guest   [administrator]

// 添加新用户
root@my-rabbit:~# /usr/lib/rabbitmq/bin/rabbitmqctl -n rabbit@my-rabbit add_user mq-admin mq-admin123456
Adding user "mq-admin" ...

// 列出所有的用户
root@my-rabbit:~# /usr/lib/rabbitmq/bin/rabbitmqctl -n rabbit@my-rabbit list_users
Listing users ...
mq-admin    []
guest   [administrator]
查看队列的状态信息:

root@my-rabbit:/# /usr/lib/rabbitmq/bin/rabbitmqctl status
Status of node rabbit@my-rabbit ...
[{pid,315},
 {running_applications,
     [{rabbitmq_management,"RabbitMQ Management Console","3.7.3"},
      {rabbitmq_management_agent,"RabbitMQ Management Agent","3.7.3"},
      {rabbitmq_web_dispatch,"RabbitMQ Web Dispatcher","3.7.3"},
      {amqp_client,"RabbitMQ AMQP Client","3.7.3"},
      {rabbit,"RabbitMQ","3.7.3"},
      {mnesia,"MNESIA  CXC 138 12","4.15.1"},
      {rabbit_common,"Modules shared by rabbitmq-server and rabbitmq-erlang-client","3.7.3"},
      {cowboy,"Small, fast, modern HTTP server.","2.0.0"},
      {ranch_proxy_protocol,"Ranch Proxy Protocol Transport","1.4.4"},
      {ranch,"Socket acceptor pool for TCP protocols.","1.4.0"},
      {ssl,"Erlang/OTP SSL application","8.2.2"},
      {public_key,"Public key infrastructure","1.5.1"},
      {asn1,"The Erlang ASN1 compiler version 5.0.3","5.0.3"},
      {xmerl,"XML parser","1.3.15"},
      {jsx,"a streaming, evented json parsing toolkit","2.8.2"},
      {cowlib,"Support library for manipulating Web protocols.","2.0.0"},
      {crypto,"CRYPTO","4.1"},
      {os_mon,"CPO  CXC 138 46","2.4.3"},
      {recon,"Diagnostic tools for production use","2.3.2"},
      {inets,"INETS  CXC 138 49","6.4.4"},
      {lager,"Erlang logging framework","3.5.1"},
      {goldrush,"Erlang event stream processor","0.1.9"},
      {compiler,"ERTS  CXC 138 10","7.1.3"},
      {syntax_tools,"Syntax tools","2.1.3"},
      {sasl,"SASL  CXC 138 11","3.1"},
      {stdlib,"ERTS  CXC 138 10","3.4.2"},
      {kernel,"ERTS  CXC 138 10","5.4"}]},
 {os,{unix,linux}},
 {erlang_version,"Erlang/OTP 20 [erts-9.1.5] [source] [64-bit] [smp:1:1] [ds:1:1:10] [async-threads:64] [hipe] [kernel-poll:true]\n"},
 {memory,
     [{connection_readers,0},
      {connection_writers,0},
      {connection_channels,0},
      {connection_other,24536},
      {queue_procs,0},
      {queue_slave_procs,0},
      {plugins,1121680},
      {other_proc,22007712},
      {metrics,194848},
      {mgmt_db,194440},
      {mnesia,73648},
      {other_ets,2168952},
      {binary,114224},
      {msg_index,28912},
      {code,28209092},
      {atom,1123529},
      {other_system,21360675},
      {allocated_unused,21689944},
      {reserved_unallocated,57344},
      {strategy,rss},
      {total,[{erlang,76622248},{rss,98369536},{allocated,98312192}]}]},
 {alarms,[]},
 {listeners,[{clustering,25672,"::"},{amqp,5672,"::"},{http,15672,"::"}]},
 {vm_memory_calculation_strategy,rss},
 {vm_memory_high_watermark,0.4},
 {vm_memory_limit,417692057},
 {disk_free_limit,50000000},
 {disk_free,14999191552},
 {file_descriptors,
     [{total_limit,1048476},
      {total_used,2},
      {sockets_limit,943626},
      {sockets_used,0}]},
 {processes,[{limit,1048576},{used,367}]},
 {run_queue,0},
 {uptime,3364},
 {kernel,{net_ticktime,60}}]
可以看到： {listeners,[{clustering,25672,"::"},{amqp,5672,"::"},{http,15672,"::"}]}

默认rabbitmq监听了三个端口号:

clustering: 25672
amqp: 5672
http: 15672

### 参考内容

http://www.rabbitmq.com/download.html
https://hub.docker.com/r/_/rabbitmq/

### 更新记录

2018-02-08 新增本文内容
2018-02-10 修改完善本文内容及格式
[END]

