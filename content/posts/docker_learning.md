---
title: "Docker_learning"
date: 2021-03-07T18:39:47+09:00
draft: true
---

# Section3 
## 执行启动容器命令之后会发生什么
执行`docker container run --publish 80:80 --name webhost --detach nginx`之后发生的：

1. 在本地的image cache中寻找
2. 如果本地找不到匹配的image，则在远程的image repo中寻找（默认为Docker Hub）
3. 下载寻找到的image最新版本
4. 以下载好的image为基础创建一个容器container
5. 在docker engine的private network中创建一个虚拟IP
6. 打开主机（host）的80端口连接转发至容器的80端口
	- 如果不用`--publish`选项则不会打开任何端口
7. 使用image Dockerfile中的CMD启动容器
8. `--detach`使得container在后台运行，不接收输入与输出。
  - 重新进入交互模式使用`docker attach`(需要有正在运行的bash，退出container也停止)，或者`docker exec -it`(退出container不停止)
  - [参考1](https://qiita.com/RyoMa_0923/items/9b5d2c4a97205692a560)
  - [参考2](https://stackoverflow.com/questions/30960686/difference-between-docker-attach-and-docker-exec)

## Container VS. VM
### container不是mini VM（与VM基本没有相似之处）
- 只是进程（process）
- 被限制在其能访问的资源中
- 进程停止则退出

#### 相关命令解说
`docker top`列出在特定的容器中执行的进程
`ps aux`可以发现同样的进程可以在主机系统中查询到
所以容器是一个进程

## 监视
- `docker container top` 同上
- `docker container inspect`列出启动容器时所用的设置（JSON格式）等元数据
- `docker container stats`监视容器运行状态（CPU使用和内存等）

## 进入容器内部
- `docker container run -it`以交互的方式启动容器
	- `-t`创建一个虚拟的tty
	- `-i`使得会话开放来接收终端输入（keep session open to receive terminal input）
	- 末尾可以加入想运行的命令，比如`bash`
	- e.g. `docker container run --name nginx -it nginx bash`直接进入容器的终端
		- 退出后容器停止
- `docker container exec -it <command>`在既存的运行中的容器中以交互方式执行命令
	- 不影响容器运行
	- 需要启动容器来执行否则会报错
- Alpine是一个很小的linux发行版本，只有`sh`，可以通过`apk`来安装`bash`

## 容器的网络
### 网络默认设置
- 每个容器都连接到一个虚拟私有网络`bridge`
- 每个虚拟网络都通过NAT防火墙路由至主机IP
	- 每个容器可以通过这个路径连接到外面的网络再连接回来
- 容器之间的通讯不需要`-p`的设置也可以实现
	- 如一个由mysql, php/apache组成的`my_web_app`网络
	- 和一个由mongo, nodejs组成的`my_api`网络
	- 但是不进行设置的话不同网络之间没法互通
### 定制化
- "Batteris included, But Removable" 
	- 大部分情景使用默认设置即可但是可以随意定制
- 可以将容器连接至多个或0个虚拟网络
- 可使用`--net=host`来跳过虚拟网络直接使用主机IP
- 在主机级别的端口，一个端口只能被一个容器所监听
### 代码
- `-p`命令后的格式是`HOST:CONTAINER`
- `docker container port <container>`显示端口映射情况
- 查看容器地址`docker container inspect --format "{{ .NetworkSettings.IPAddress}}" <container>`

### 命令行管理网络
- `docker network ls`列出所有虚拟网络
	- bridge(docker0)是通过NAT防火墙连接至主机物理网络的默认网络
	- host是跳过docker的直接与主机网络接口连接的网络，牺牲了容器模式下的安全性换来了更高的网络性能
	- none:不与任何外部接口相连（eth0），只留有容器内部的localhost接口
- `docker network inspect bridge`查看bridge网络的详情
	- 可以看到有哪些容器连接在这个网络上
	- `IPAM`可以看到网络设置，最后通过网关连接到主机物理网络
- `docker network create <name>`使用bridge的驱动（默认的）创建一个虚拟网络
- 启动（run）容器时可以用`--network`来指定网络
- `docker network connect/disconnect <network> <container>`将容器连接至/断开网络连接

### DNS
dcoker自带一个DNS用来解析域名（容器名）从而访问到正确的容器（使用相当于hostname的container name来实现容器之间的通讯）
- 新建一个虚拟网络，当使用`--network`来将不通的容器连接至虚拟网络时，或自动以容器名为key来建立DNS服务，这样同一个虚拟网络之间的容器就可以互相连通
- 默认的`bridge`虚拟网络没有这种服务，每次添加新的容器如果需要与其他容器连接则需要手动使用`--link <list of container>`来设置

# Section4 Image
## 什么是Image （包括）
- 应用的二进制和依赖文件数据
- image data的元数据和如何运行image的设定参数

## Image layers
- `docker image history` 显示Image layer的历史
	- 除了第一个履历有Image ID之外其他都为<missing>, 因为其他的layer只是构成Image的一部分并不是Image本身，所以不配拥有ID。
- 每个Image都从成为scratch的空layer开始，在这之后发生在Image的文件系统中的改变都是一个layer
![例图](https://github.com/ACGNM/pics/raw/master/docker/image_layers.png)
![例图](https://github.com/ACGNM/pics/raw/master/docker/custom_image.png)
![pic](https://github.com/ACGNM/pics/raw/master/docker/copy_on_write.png)

- `docker image inspect` 取得Image的metadata (Image如何运行，Image的JSON格式的详细信息)

## image tag
- `dokcer image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]`
	- `docker image tag --help`来查看用法
	- 根据SOURCE\_IMAGE(with tag `TAG`)来创建一个TARGET\_IMAGE（默认tag为latest）
	- 总是创建一个新记录（对于执行 `docker image ls`的结果）
- 只是一个用来区分的名称。如nginx的latest和mainline是拥有同一个image ID的image。都pull下来时，pull第二个会显示已经存在。但是在`docker image ls`中分开显示（同时可以发现两者image ID相同）
- 默认的tag是`latest`。但并不代表就是最新的
- `docker image push`可以向Docker Hub上传变更过layer的image
	- 这之前需要先登陆。使用`docker login`
	- 在`./docker/config.json`中存储docker Hub的认证信息
如果想上传一个私人（private）的image，则先需要建立一个私人仓库然后再上传

## Dockerfile基础
### 参考dockerfile-sample-1注释
- [dockerfile-sample-1](https://github.com/BretFisher/udemy-docker-mastery/blob/main/dockerfile-sample-1/Dockerfile)
- [官方相关文档](https://docs.docker.com/engine/reference/builder/#usage)

### 建立Image (building Image)
- 首先改变当前目录至存在`Dockerfile`的目录中
	- 如果想使用非默认名称 (Dockerfile) 的dockerfile, 使用`-f`选项 (`docker image build -f <dockerfile_name>`) 
- `docker image build -t IMAGE_NAME[:TAG] <path>`
	- 如果只是本地使用，则不需要在前面加上Docker Hub的account name
- 接下来的输出对应Dockerfile的每一个stanza的命令
	- 每一步会对应一个存在build Cache中的Hash用来标记每一步
	- 如果下一次build时Dockerfile的对应那一行没有变化则不会再执行一边相同的stanza
	- 如果有所该表则重新执行有变化的stanza及其之后的所有stanza
	- Docker的build快速是因为docker将每一步都放入缓存中
- 扩展官方image
	- 使用  [dockerfile-sample-2](https://github.com/BretFisher/udemy-docker-mastery/blob/main/dockerfile-sample-2)
	- 可以直接`FROM`官方的Image来加入自定义设置
	- 可以继承来`FROM`里的所有stanza，所以不需要加入`CMD`等必要的stanza
	- `WORKDIR`相当于执行了`cd`命令
	- `docker conainer run`的`-rm`flag用来告诉docker，当container退出后clean up container和其file system