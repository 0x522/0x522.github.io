# Docker原理及其应用


# Docker

## 什么是 docker

Docker 是一个开放源代码软件，是一个开放平台，用于开发应用、交付（shipping）应用、运行应用。 Docker 允许用户将基础设施（Infrastructure）中的应用单独分割出来，形成更小的颗粒（容器），从而提高交付软件的速度。<br>
Docker 容器与虚拟机类似，但二者在原理上不同。容器是将操作系统层虚拟化，虚拟机则是虚拟化硬件，因此容器更具有便携性、高效地利用服务器。 容器更多的用于表示 软件的一个标准化单元。由于容器的标准化，因此它可以无视基础设施（Infrastructure）的差异，部署到任何一个地方。另外，Docker 也为容器提供更强的业界的隔离兼容。<br>
**Docker 利用 Linux 核心中的资源分离机制，例如 cgroups，以及 Linux 核心名字空间（namespaces），来创建独立的容器（containers）。**

## Docker 引擎

Docker 引擎(Docker Engine)是一个服务端-客户端结构的应用，主要有这些部分：Docker 守护进程、Docker Engine AP 页面存档备份，存于互联网档案馆、Docker 客户端。<br>

- Docker 守护进程(Docker daemons)，也叫 dockerd ，是一个持久化的进程，用户管理容器。守护进程会监听 Docker Engine API 页面存档备份，存于互联网档案馆 的请求。
- Docker Engine API 页面存档备份，存于互联网档案馆是用于与 Docker 守护进程交互用的的 API。它是一个 RESTful API，因此它不仅可以被 Docker 客户端调用，也可以被 wget 和 curl 等命令调用。
- **Docker 客户端，也叫 docker，是大部分用户与 Docker 交互的主要方式。用户通过客户端将命令发送给守护进程。命令会遵循 Docker Engine API 页面存档备份，存于互联网档案馆。**

## Docker 对象

Docker 的对象是指 Images、Containers、Networks、Volumes、Plugins 等等。

- 容器（Containers）是镜像的可运行的实例。容器可通过 API 或 CLI（命令行）进行操控。
- 镜像（Images）是一个只读模板，用于指示创建容器。 镜像分层(layers)构建的，而定义这些层次的文件叫 Dockerfile。
- 服务（Services）允许用户跨越不同的 Docker 守护进程（Docker daemons）的情况下增加容器，并将这些容器分为管理者（managers）和工作者（workers），让他们为 swarm 共同工作。

## Docker 能做什么

- 保证开发、测试、交付、部署的环境完全一致
- 抱枕资源的隔离
- 启动临时的、用完及弃的环境，例如测试
- 秒级的超大规模部署和扩容

## Docker 的基本概念

- 镜像 images
  一个预先定义好的模板文件，Docker 引擎可以按照这个模板文件启动无数个一模一样互不干扰的容器。
- 容器 container
  是一台虚拟的计算机，它拥有独立的

  - 网络
  - 文件系统
  - 进程

    它默认和宿主机不发生任何交互，这意味着数据是没有持久化的。

### docker 对比 VM

![](/vm%20versus%20docker.jpg)

### docker pull/images

- `docker pull`是下载一个指定的镜像，方便随时启动。例如：docker pull mysql：6.7.28 下载一个 mysql 版本为 5.7.28。
- `docker images`产看本地已经有的镜像。

### docker run/ps

- `docker run`装载镜像成为一个容器。
  - 就好像从蛋糕模子做出来一个蛋糕
  - 在这个容器看来，自己就是一台独立的计算机
  - 每个容器有一个 ID，支持缩写，例如容器 id 是 234sdhfsakjhf，就可以简写为 234s
- `docker run -it <镜像名> <镜像中要运行的命令和参数>`
  - 交互式命令行，当前 shell 中运行，CTRL+c 退出
- `docker run -d <镜像名> <镜像中要运行的命令和参数>`
  - daemon 模式，在后台运行容器

#### docker run

- `--name`为容器指定一个名字
- `--restart=always`遇到错误自动重启
- `-v <本地文件>:<容器文件>`
- `-p <本地端口>:<容器端口>`
- `-e NAME=VALUE`

### docker start/stop

- 启动/停止一个容器
  - 可以想象为开关机

### docker rm

- 删除一个容器
  - 想象成将电脑丢掉
- `docker rm -f <容器名或id>` 强制删除某个容器

### docker exec

- 指定目标容器，进入容器执行命令
  - `docker run -it <目标容器ID> <目标命令(通常为bash)>`
- 调试、解决问题必备命令

### docker logs

- `docker logs<容器ID或容器名>`
  - 查看目标容器的输出
- `docker logs -f<容器ID或容器名>`

### docker inspect

- 高级命令：查看容器的详细状态(json 格式)

## Docker 分层的镜像

![](/mirror.jpg)

**分层的镜像实现了 docker 容器的复用。**

### Dockerfile

- 指定镜像如何生成
- 编写一个 Dockerfile

  - 在 Docker 中创建镜像最常用的方式，就是使用 Dockerfile。Dockerfile 是一个 Docker 镜像的描述文件，我们可以理解成火箭发射的 A、B、C、D…的步骤。Dockerfile 其内部包含了一条条的指令，每一条指令构建一层，因此每一条指令的内容，就是描述该层应当如何构建。

  ```dockerfile
    #基于centos镜像
    FROM centos

    #维护人的信息
    MAINTAINER The CentOS Project <xxxx@xxx.com>

    #安装httpd软件包
    RUN yum -y update
    RUN yum -y install httpd

    #开启80端口
    EXPOSE 80

    #复制网站首页文件至镜像中web站点下
    ADD index.html /var/www/html/index.html

    #复制该脚本至镜像中，并修改其权限
    ADD run.sh /run.sh
    RUN chmod 775 /run.sh

    #当启动容器时执行的脚本文件
    CMD ["/run.sh"]
  ```

- `docker build .` 以当前目录建立一个镜像 image
- 每个镜像都会有一个唯一的 id

### Docker 的镜像仓库与 tag

- 可以任意对镜像进⾏ tag 操作
  - 决定了未来这个镜像会被 push 到哪⾥
  - 决定了未来从哪⾥下载镜像
- 可以⽅便的创建镜像仓库的私服
  - `--registry-mirror`
  - `--insecure-registry`

## Docker 与 Kubernetes(k8s)

![](/dockerk8s.jpg)<br>
业界有一个形象的比喻

- 传统服务被比作宠物 pets，意思是当对外的服务出了问题，宠物意味着你要尽心尽力的维护它，直到它健康没有任何问题。
- 当基于 docker 的 k8s 出现后，服务就被比作 cattle，意思就是牲畜，当牲畜出现了问题，就把他杀掉再来一只顶替(杀掉容器&&重新启动容器)，而且成本很低。

### k8s

![](/docker%20versus%20kubernetes.jpg)

- kubernetes master 主节点 用于管理 Node
- Node 子节点，里面存放
  - kubelet 当前节点的管理员
  - Kube-proxy 提供网络服务
  - Pod 就可以理解为上面所说的 cattle，里面运行一个或多个 docker 容器，当他们其中有个别出了问题，就杀掉重启。

**k8s 提供自动滚动更新(rolling update) 服务，可以保证你的服务在更新的过程中不被更新中断**

