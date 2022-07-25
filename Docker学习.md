# Docker

> Docker学习

- Docker概述
- Docker安装
- Docker命令
  - 镜像命令
  - 容器命令
  - 操作命令
  - ...
- Docker镜像
- 容器数据卷
- DockerFile
- Docker网络原理
- Idea整合Docker
- Docker Compose
- Docker Swarm
- CI\CD jerkins



> 感谢狂神



# Docker 概述

## Docker为什么出现

项目跟着环境走！

以前：开发jar，运维做环境

现在：开发打包部署上线，一条龙



Docker的思想来自于集装箱！

**隔离** ：Docker的核心思想！打包装箱，每个箱子相互隔离！



## Docker历史

2010，美国dotCloud

做一下pass云计算服务！LXC有关的容器技术

他们将自己的技术（容器化技术）命名Docker！

因不受关注难以为继，选择  ==开源==

2013年**开源**

越来i越多人发现了docker的优点！

原因？轻巧！

> 之前基本是虚拟机

Docker是容器技术

但是两者都是虚拟化技术



> 关于Docker

Docker是用Go语言开发的开源项目！

Docker的文档超级详细！



## Docker能干嘛

> 之前的虚拟机技术

![image-20210414140504852](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418135044.png)

**虚拟机技术的缺点：**

1. 资源占用十分多
2. 冗余步骤多
3. 启动很慢！





> 容器化技术

虚拟化技术不是模拟一个完整的操作系统

![image-20210414140807209](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418135911.png)



比较Docker和虚拟机技术的不同

- 传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件
- 容器内的应用直接运行在宿主机的内容，容器没有自己的内核，也没有虚拟我们的硬件，所以轻便了
- 每个容器相互隔离，每个容器都有自己的文件系统，互不影响







> DevOps(开发、运维)

**应用更快捷的开发和部署**



传统：一堆帮助文档，安装程序

Docker：一键运行打包镜像发布测试，一键运行

**更便捷的升级和扩缩容**

使用Docker之后，部署应用和搭积木一样！

项目打包为一个镜像，扩展服务器A！



**更简单的系统运维**

容器化之后，开发测试环境高度一致



**更高效的计算机资源利用**

Docker是内核级别的虚拟化，可以在一个物理机上运行很多的容器实例！



# Docker安装

## Docker的基本组成

![image-20210414143550014](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418140345.png)



**镜像（Image）：**

docker镜像好比是模板，通过模板来创建容器服务，通过镜像创建多个容器（最终服务运行或者项目运行就是在容器中的）



**容器（container）**：

Docker利用容器技术，独立运行一个或者一个组应用，通过镜像来创建。

启动、停止、删除，基本命令！

目前可以把这个容器理解为就是一个建议的linux系统



**仓库（repository）：**

存放镜像的地方！

仓库分为公有仓库和私有仓库！

Docker Hub（默认国外）

国内云服务商都有容器服务器（配置镜像加速！）



## 安装Docker

> 环境准备

1. linux系统
2. ssh客户端
3. 系统

> 环境查看

```bash
# 系统内核是4.18以上
[hadoop@group01 ~]$ uname -r
4.18.0-240.15.1.el8_3.x86_64
```

```bash
# 系统版本
[hadoop@group01 ~]$ cat /etc/os-release 
NAME="CentOS Linux"
VERSION="8"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="8"
PLATFORM_ID="platform:el8"
PRETTY_NAME="CentOS Linux 8"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:8"
HOME_URL="https://centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"
CENTOS_MANTISBT_PROJECT="CentOS-8"
CENTOS_MANTISBT_PROJECT_VERSION="8"
```

> 安装

```shell
# 1. 卸载旧版本
yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-engine

# 2. 需要的安装包
yum install -y yum-utils

# 3. 设置镜像的仓库
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

# 更新yum软件包索引
yum makecache fast

# 4. 安装docker相关的 docker-ce社区
yum install docker-ce docker-ce-cli containerd.io

# 5. 启动docker
systemctl start docker

# 6. 查看是否安装成功
docker version
```



![image-20210414222505745](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418140407.png)

```shell
# 7. 启动HelloWorld
docker run hello-world
```

![image-20210414222717872](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418140414.png)

```shell
# 8. 查看一下下载的Helloworld镜像
docker image
```

卸载Docker

```shell
#1. 删除Docker
yum remove docker-ce docker-ce-cli containerd.io

#2. 删除镜像等资源
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```





## Docker原理

**Docker是什么工作的？**

Docker是一个Client - Server 结构的系统，Docker的守护进程运行在主机上，通过Socket从客户端访问。

DockerServer接收到Docker - Client的指令，就会执行这个命令。

**Docker为什么比VM快？**

![image-20210414225127090](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418140418.png)

1. Docker有比虚拟机更少的抽象层
2. Docker利用的是宿主机的内核，vm需要Guest OS

新建容器的时候docker不需要像虚拟机一样新建一个操作系统内核，避免引导。

虚拟机加载的是GuestOS，分钟级别，而docker利用宿主机的操作系统，秒级！





# Docker常用命令

## 帮助命令

```shell
docker version 	  #显示docker的版本信息
docker info		  #显示docker的系统信息（system级别）包括镜像容器和数量
docker 命令 --help #万能
```

帮助文档的地址:[客户端相关命令](https://docs.docker.com/reference)



## 镜像命令

**docker images**

```shell
[root@group01 hadoop]# docker images
#镜像的仓库源   #镜像的标签 #镜像的id       #镜像的创建时间  #镜像的大小
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    d1165f221234   5 weeks ago   13.3kB


# 参数
-a, --all   #列出所有的镜像
-q, --quiet #只列出镜像的id
...
```

**docker search 搜索镜像**

```shell
[root@group01 hadoop]# docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10748     [OK]       
mariadb                           MariaDB Server is a high performing open sou…   4047      [OK]       
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   791                  [OK]


# 参数
# 可选项，通过收藏过滤
--filter=STARS=3000 #搜索出来的大于3000
```



**docker pull** 下载

```shell
# 下载镜像
docker pull 镜像名[:tag]
# 如果不写tag，默认最新

[root@group01 hadoop]# docker pull mysql
Using default tag: latest
latest: Pulling from library/mysql
f7ec5a41d630: Pull complete  #分层下载，docker image的核心 联合文件系统
9444bb562699: Pull complete 
6a4207b96940: Pull complete 
181cefd361ce: Pull complete 
8a2090759d8a: Pull complete 
15f235e0d7ee: Pull complete 
d870539cd9db: Pull complete 
5726073179b6: Pull complete 
eadfac8b2520: Pull complete 
f5936a8c3f2b: Pull complete 
cca8ee89e625: Pull complete 
6c79df02586a: Pull complete 
Digest: 
#签名 
sha256:6e0014cdd88092545557dee5e9eb7e1a3c84c9a14ad2418d5f2231e930967a38
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest # 真实地址


# 下面两者等价
docker pull mysql
docker pull docker.io/library/mysql:latest

# 指定版本下载
[root@group01 hadoop]# docker pull mysql:5
5: Pulling from library/mysql
f7ec5a41d630: Already exists 
9444bb562699: Already exists 
6a4207b96940: Already exists 
181cefd361ce: Already exists 
8a2090759d8a: Already exists 
15f235e0d7ee: Already exists 
d870539cd9db: Already exists 
7310c448ab4f: Pull complete 
4a72aac2e800: Pull complete 
b1ab932f17c4: Pull complete 
1a985de740ee: Pull complete 
Digest: sha256:e42a18d0bd0aa746a734a49cbbcc079ccdf6681c474a238d38e79dc0884e0ecc
Status: Downloaded newer image for mysql:5
docker.io/library/mysql:5
```

![image-20210415102636347](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418140423.png)



**docker rmi** 删除镜像

```shell
docker rmi -f 镜像id		 # 删除指定镜像
docker rmi -f 镜像id 镜像id # 删除多个惊喜那个
docker rmi -f $(docker images -aq) #删除全部镜像
```



## 容器命令

**说明：有了镜像才可以创建容器，Linux，下载一个CentOS镜像测试学习**

```shell
docker pull centos
```

**新建容器并启动**

```shell
docker run [可选参数] image

# 参数
--name="Name"	#容器名字 tomcat tomcat02, 用来区分容器
-d				#后台方式运行
-it				#使用交互方式运行，进入容器查看内容
-p				#指定容器端口 -p 8080:8080
	-p ip:主机端口:容器端口
	-p 主机端口：容器端口 （常用）
	-p 容器端口
	容器端口
-P				# 随机指定端口

# 测试，启动并进入容器
[root@group01 hadoop]# docker run -it centos /bin/bash
[root@0ac0238d498c /]# ls # 查看容器内的centos，基础版本，很多命令不全
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr


# 退出
exit

```

**列出所有的运行的容器**

```shell
# docker ps 命令
-a # 列出当前正在运行的容器，顺带带出历史运行过的容器
-n=? #显示最近创建的容器
-q #只显示容器的编号

[root@group01 hadoop]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

[root@group01 hadoop]# docker ps -a
CONTAINER ID   IMAGE         COMMAND       CREATED              STATUS                      PORTS     NAMES
0ac0238d498c   centos        "/bin/bash"   About a minute ago   Exited (0) 19 seconds ago             relaxed_sanderson
99aa136ba239   hello-world   "/hello"      12 hours ago         Exited (0) 12 hours ago               nostalgic_banzai

[root@group01 hadoop]# 

```

**退出容器**

```shell
exit #直接容器停止并退出
Ctrl + P + Q  # 不停止容器退出
```

**删除容器**

```shell
docker rm 容器id					 #删除指定的容器，不能删除正在运行的容器，-f 强制删除（force）
docker rm -rf $(docker ps -aq)	  #删除所有的容器（递归强制删除）
docker ps -a -q | xargs docker rm #删除所有的容器（管道删除）
```

**启动和停止容器的操作**

```shell
docker start 容器id	#开启
docker restart 容器id	#重启
docker stop 容器id	#停止
docker kill 容器id 	#杀死容器
```





## 常用其他命令

后台启动容器

```shell
# 命令 docker run -d 镜像名
docker run -d centos

#问题：centos停止了

#docker容器使用后台运行，必须要有一个前台进程，docker发现没有应用，就会自动停止
#nginx， 容器启动后，发现自己没有提供服务，就会自动停止
```

**查看日志**

```shell
docker logs -f -t --tail 镜像id

# 自己写脚本
docker run -d centos /bin/sh -c "while true;do echo hong;sleep 1; done"

#显示日志
-tf # 显示日志
--tail number #要显示日志条数

docker logs -tf --tail 10 镜像id
```

**查看容器中的进程信息**

```shell
docker top 镜像id
```

**查看镜像的元数据**

```shell
docker inspect 镜像id
```

**进入当前正在运行的容器**

```shell
# 我们通常容器都是使用后台方式运行的，需要进入容器，修改一些配置

# 命令1，进入容器后，开启一个新的终端
docker exec -it 容器id bashshell

#命令2，进入容器正在执行的终端
docker attach 容器id
```



**从容器拷贝文件到主机上**

```shell
docker cp 容器id：容器内路径 目的路径
# 拷贝是一个手动过程，未来我们使用-v卷的技术，可以实现自动同步
```



## 小结

![image-20210415190254282](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418140430.png)

```shell
attach      Attach local standard input, output, and error streams to a running container
build       Build an image from a Dockerfile
commit      Create a new image from a container's changes
cp          Copy files/folders between a container and the local filesystem
create      Create a new container
diff        Inspect changes to files or directories on a container's filesystem
events      Get real time events from the server
exec        Run a command in a running container
export      Export a container's filesystem as a tar archive
history     Show the history of an image
images      List images
import      Import the contents from a tarball to create a filesystem image
info        Display system-wide information
inspect     Return low-level information on Docker objects
kill        Kill one or more running containers
load        Load an image from a tar archive or STDIN
login       Log in to a Docker registry
logout      Log out from a Docker registry
logs        Fetch the logs of a container
pause       Pause all processes within one or more containers
port        List port mappings or a specific mapping for the container
ps          List containers
pull        Pull an image or a repository from a registry
push        Push an image or a repository to a registry
rename      Rename a container
restart     Restart one or more containers
rm          Remove one or more containers
rmi         Remove one or more images
run         Run a command in a new container
save        Save one or more images to a tar archive (streamed to STDOUT by default)
search      Search the Docker Hub for images
start       Start one or more stopped containers
stats       Display a live stream of container(s) resource usage statistics
stop        Stop one or more running containers
tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
top         Display the running processes of a container
unpause     Unpause all processes within one or more containers
update      Update configuration of one or more containers
version     Show the Docker version information
wait        Block until one or more containers stop, then print their exit codes

```







## 作业练习

> Docker 部署Ngnix

```shell
# 1、搜索镜像
docker search nginx

# 2、下载镜像
docker pull nginx

# 3、运行测试
docker run -d --name nginx01 -p 3344:80 nginx

[root@group01 hadoop]# docker run -d --name nginx01 -p 3344:80 nginx
48a4f3fd8b76e676bb3e97fba180eeec8135534a30ddd93b6626ecb11fabd710
[root@group01 hadoop]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                   NAMES
48a4f3fd8b76   nginx     "/docker-entrypoint.…"   6 seconds ago   Up 5 seconds   0.0.0.0:3344->80/tcp, :::3344->80/tcp   nginx01
[root@group01 hadoop]# docker exec -it nginx01 /bin/bash
root@48a4f3fd8b76:/# ls
bin   docker-entrypoint.d   home   media  proc	sbin  tmp
boot  docker-entrypoint.sh  lib    mnt	  root	srv   usr
dev   etc		    lib64  opt	  run	sys   var
root@48a4f3fd8b76:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@48a4f3fd8b76:/# 
```



> Tomcat 部署

```shell
#1、官方的使用
docker run -it --rm tomcat:9.0

#--rm 用完即删，一般用于测试使用

#2、下载
docker pull tomcat

#3、启动运行
docker run -d --name tomcat01 -p 3355:8080 tomcat

#4、进入容器
docker exec -it tomcat01 /bin/bash

#问题：webapps下没有文件，文件在webapps.dist下
#将其复制到webapps下访问成功
```





> ES + Kibana

```shell
# es 暴露的端口很多！
# es 十分的耗内存
# es的数据一般放置在安全目录！挂载
# 官方：
# --net somenetwork 网络配置
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:tag

#1、启动
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.12.0

#问题：十分耗内存
#查看docker资源状态
docker stats

# 测试：
{
  "name" : "824bc92ee6df",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "L3e4-kpAS4WnMh-fbLXLYA",
  "version" : {
    "number" : "7.12.0",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "78722783c38caa25a70982b5b042074cde5d3b3a",
    "build_date" : "2021-03-18T06:17:15.410153305Z",
    "build_snapshot" : false,
    "lucene_version" : "8.8.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

#增加内存限制，修改配置文件，-e 环境配置修改
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e ES_JAVA_OPTS="-Xms64m -Xmx512m" -e "discovery.type=single-node" elasticsearch:7.12.0
```



> kibana连接Es





## 可视化

- Portainer

```shell
docker run -d -p 8088:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock \
--privileged=true portainer/portainer
```



- Rancher

什么是**protainer**

Docker图形化界面管理工具！提供一个后台面板供我们操作！

```shell
docker run -d -p 8088:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock \
--privileged=true portainer/portainer
```

![image-20210415202017974](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418140437.png)





# Docker镜像讲解

## 镜像是什么

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件。

所有的应用，直接打包docker镜像，就可以直接跑起来！

如何得到镜像

- 从远程仓库下载
- 朋友拷贝给你
- 自己制作一个镜像DockerFile



## Docker镜像加载原理

> UnionFS（联合文件系统）

UnionFS（联合文件系统）：Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件的修改作为一次提交来一层层的叠加，同时可以将不通的目录挂载到同一个虚拟文件系统下（unite serveral derecotries into a single virtual filesystem）。Union文件系统时Docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统包含所有底层的文件和目录。



> Docker镜像加载原理

docker的镜像实际上是由一层一层的文件系统组成，这种层级的文件系统UnionFS。

Boots（boot file system）主要是包含BootLoader和Kernel，BootLoader主要是引导加载Kernel，Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层时bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核，当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

RootFS（Root File system），在BootFS之上，包含的就是典型的Linux系统中的/dev，/proc，/etc等标准目录和文件，rootFS就是各种不通的操作系统的发行版，比如Ubuntu、CentOS等等。

![image-20210415204513330](C:\Users\12451\AppData\Roaming\Typora\typora-user-images\image-20210415204513330.png)

对于一个精简的OS，RootFS可以很小，只需要包含最基本的命令，工具和程序库就可以了，因为底层直接使用Host和Kernel，自己只需要提供RootFS就可以了。由此可见对于不通的Linux发行版，BootFS基本是一致的，RootFS会有差别，因此不同的发型板可以共用BootFS。



## 分层理解

> 分层的镜像

为什么Docker镜像要采用分层结构？





>  特点

Docker镜像都是只读的，当容器启动时，一个新的可写操作被加载到镜像顶部。

这一层叫容器曾，容器之下镜像层！





## Comiit镜像

> 如何提交一个自己的镜像

```shell
docker commit 提交容器成为一个新的副本

# 和git类似
docker commit -m="提交的描述信息"， -a="作者" 容器id 目标镜像名:[TAG]
```

实例测试：

```shell
#1. 启动tomcat，把webapps.dist的文件拷贝到webapps

#2. 提交新的tomcat命名为tomcat_hong
```

![image-20210416104655300](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418140445.png)



> 想要保存当前容器的的状态，就可以通过提交当前容器生成新的镜像，相当于VM的快照。





# 容器数据卷

## **什么是容器数据卷**

为了数据持久化，希望有一种容器间的数据共享技术。将容器的数据同步到本地。

这就是卷技术。本质上是目录的挂载，将容器内的目录挂载到Linux上



**容器的持久化和同步操作，容器间可以数据共享！**



## 使用数据卷

> 方法一：直接使用命令来挂载：-v

```shell
docker run -it -v

# 测试：
docker run -it -v /home/test:/home centos /bin/bash
```

好处：以后只需要修改本地文件，就可以自动同步到容器中。



## 实战：安装Mysql

思考：MySQL的数据持久化问题

```shell
# 获取镜像
docker pull mysql

# 运行镜像
docker run --name mysql01 -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=12345678 mysql

# 创建新的数据库、新的表
# 删除镜像
# 运行新的镜像，映射同样的路径
# 数据库仍然存在
```





## 具名和匿名挂载

```shell
# 匿名挂载
-v 容器内路径 （不写主机路径）
docker run -d -P --name nginx01 -v /etc/nginx nginx

#查看所有的volume的情况
docker volume ls
[root@group01 data]# docker volume ls
DRIVER    VOLUME NAME
local     38bcacb8afc4b66556150dd685c5a10887e0a64ccaff79ebdf3601f8d57b6ff9
local     d1ca26526ad1ac26845830c85ae035f0e683f6912bf4ad1fc6b5ef68d1f1a417

#这种就是匿名挂载


#具名挂载
docker run -d -P --name nginx01 -v juming-nginx:/etc/nginx nginx
[root@group01 data]# docker volume ls
DRIVER    VOLUME NAME
local     juming-nginx

# 通过 -v 卷名：容器内路径
# 查看一下这个卷的位置
docker volume inspect juming-nginx

```

所有的docker容器内卷，没有指定目录的情况下都是在`/var/lib/docker/volume/`下

```shell
# 如何确定是具名还是匿名挂载
-v 容器内路径 		#匿名挂载
-v 卷名:容器内路径    #具名挂载
-v /宿主机路径:容器内路径 #指定路径挂载

#拓展
# 改变读写权限
# ro readonly	#只读
# rw readwrite	#可读写
# 一旦设定了容器权限，容器对我们挂在出来的内容就有权限了
-v juming-nginx:/etc/nginx:ro
-v juming-nginx:/etc/nginx:rw

# ro 只能通过宿主机对文件进行操作，容器内不能才做
```



## 初识DockerFile

DockerFile是用来构建docker镜像的构建文件！是一个命令脚本！

通过这个脚本可以生成镜像。

镜像是一层一层的。

```shell
# 创建一个dockerfile文件
# 文件中的内容，指令（大写），参数
FORM centos

VOLUME ["volume01", "volume02"]

CMD echo "----end----"
CMD /bin/bash

# 这里的命令都是一层

#启动
[root@group01 docker-volume-test]# docker run -it hong/centos /bin/bash
[root@7d6f8041ccfe /]# ls
bin  home   lost+found	opt   run   sys  var
dev  lib    media	proc  sbin  tmp  volume01
etc  lib64  mnt		root  srv   usr  volume02

# 在宿主机通过docker inspect 容器查看Mounts信息
# 从而知道对应的挂载路径
```





## 数据卷容器

两个Mysql同步？



```shell
# 1. 启动一个镜像
docker run -it -n docker01 hong/centos

# 2. 启动第二个镜像
docker run -it -n docker02 --volumes-from docker01 hong/centos
# 这样1 和2 就同步了与volumes

--volumes-from 
# 只要使用，就是对目标的volumes的同步挂载

# 不因01的删除而结束同步
# 不因容器的删除而消失
# 文件是在宿主机的物理地址！！
```





**结论**：

容器之间的配置信息的传递

数据卷容器的生命周期一直持续到没有容器使用为止











# DockerFile

DockerFile 是用来构建docker镜像的文件！命令参数脚本！

构建步骤：

1、 编写一个dockerfile文件

2、docker build 构建成为一个镜像

3、docker run 运行镜像

4、docker push 发布镜像（DockerHub、阿里云镜像仓库！）

![image-20210416153030769](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418140453.png)

```dockerfile
FROM scratch
ADD centos-8-x86_64.tar.xz /
LABEL org.label-schema.schema-version="1.0"     org.label-schema.name="CentOS Base Image"     org.label-schema.vendor="CentOS"     org.label-schema.license="GPLv2"     org.label-schema.build-date="20201204"
CMD ["/bin/bash"]
```



## DockerFile构建过程

**基础**：

1、每个保留关键字，必须是大写字母

2、指令从上到下顺序执行

3、#号注释

4、每一个指令都会创建提交一个新的镜像层，并提交

![image-20210416153547691](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418140456.png)



DockerFile是面向开发的，我们以后发布项目，做镜像，需要编写DockerFile文件。

Docker镜像逐渐成为企业的交付标准。

DockerFile：构建文件，定义了一切的步骤，类似源代码。

DockerImages：通过DockerFile构建生成的镜像。，最终发布和运行的产品。

Docker容器：容器就是镜像运行起来提供服务器。



## DockerFile的指令

```dockerfile
FROM 		# 基础镜像
MAINTAINER 	# 维护者信息，姓名、邮箱
RUN			# Docker镜像构建的时候需要运行的命令
ADD			# 步骤，向镜像里面添加内容
WORKDIR		# 当前工作目录
VOLUME		# 设置卷，挂载主机目录
EXPOSE		# 指定对外的端口
RUN			# 
CMD			# 指定容器启动的时候运行的命令
			# 写法： CMD ["可执行程序", "参数1", "参数2"]
			# 例如： CMD ["nginx", "-g", "daemon off;"] #直接执行nginx并要求前台形式运行
ENTRYPOINT	# 指定容器入口点，可以直接追加命令
			# 使用入口点，可以直接载docker run后面跟上cmd，这些cmd会追加到entrypoint
ONBUILD		# 

```



## 实战

```shell
# CentOS 8 DockerHub
FROM scratch 
	#scratch DockerHub中大部分的镜像都是从这个开始的
	#然后再配置需要的软件和配置来进行配置
ADD centos-8-x86_64.tar.xz /
LABEL  \
	org.label-schema.schema-version="1.0" \
	org.label-schema.name="CentOS Base Image" \
    org.label-schema.vendor="CentOS"     \
    org.label-schema.license="GPLv2"     \
    org.label-schema.build-date="20201204" 
CMD ["/bin/bash"]
```



> 创建一个自己的CentOS

```shell
# 目的：增加vim
FROM centos
MAINTAINER hong<rocaelfaller@gmail.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH
RUN yum install -y vim; \
	yum install -y net-tools; 
EXPOSE 80
CMD ["echo", "$MYPATH"]
CMD ["echo", "---end---"]
CMD /bin/bash

# 构建
docker build -f mycentos-dockerfile -t mycentos:latest .

# 构建完成
Successfully built ffdda7ddaa9c
Successfully tagged mycentos:latest

# 测试
```

我们可以列出镜像的变更历史

`docker history 容器id`





## 实战：Tomcat镜像

1、准备镜像文件：tomcat压缩包，jdk压缩包

![image-20210416165022820](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418140500.png)

2、编写dockerfile文件

```shell
FROM centos
MAINTAINER hong<rocaelfaller@gmail.com>

COPY readme.txt /usr/local/readme.txt
ADD jdk-8u281-linux-x64.tar.gz /usr/local/
ADD	apache-tomcat-9.0.45.tar.gz /usr/local/ 
RUN yum -y install vim 
ENV MYPATH /usr/local
WORKDIR $MYPATH
ENV JAVA_HOME /usr/local/jdk1.8.0_281
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.45
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.45
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

EXPOSE 8080

CMD /usr/local/apache-tomcat-9.0.45/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.45/logs/catalina.out

```

3、构建镜像

```shell
docker build diytomcat .
```

4、启动镜像

```shell
docker run -d -p 9090:8080 --name mydiytomcat01 -v /home/tomcat/test:/usr/local/apache-tomcat-9.0.45/webapps/test -v /home/tomcat/tomcatlogs:/usr/local/apache-tomcat-9.0.45/logs diytomcat
```

5、访问测试

6、发布项目（直接本地编写即可）







成功。

 只有的项目都用docker来发布运行





## 发布镜像

> Docker Hub

1、注册账号

2、登录账号

3、服务器上提交自己的镜像

```shell
# Docker 命令行登录
docker login -u username
password: 

# 新增一个i额tag
docker tag 容器名	容器名:tag

# 推送
docker push 容器名：tag 
```

![image-20210416181351470](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418140504.png)

提交的时候也是按照层级来进行





## 小结

![image-20210416181543267](C:\Users\12451\AppData\Roaming\Typora\typora-user-images\image-20210416181543267.png)

```shell
docker save 会保存为tar文件
```









# Docker网络

## 理解Docker0

清空环境

> 初始测试

思考：容器能否ping通外部

```shell
# 启动tomcat
docker run -d -P --name tomcat01 tomcat

#查看容器内部网络地址
# eth0@ifxx 是docker分配的
docker exec -it tomcat01 ip addr

[root@group01 tomcatlogs]# docker exec -it tomcat01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
40: eth0@if41: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
[root@group01 tomcatlogs]# 

# ping 容器
ping 172.17.0.2
# 可以ping通
```



> 原理

1. 我们每启动一个docker容器，docker就会给docker容器分配一个ip，只要安装了docker，就会有个docker网卡 `docker0`。

   桥接模式：使用的技术是evth-pair技术

2. 再启动新的容器测试

   又多了已对网卡



> 我们发现这个容器带来的网卡都是一对一对的。
>
> 这就是evth-pair技术，就是一对的虚拟设备接口，成对出现。
>
> 一段连接协议，一段连接彼此
>
> 正因为有这个技术，evth-pair充当一个桥梁，连接各种虚拟设备。
>
> OpenStac, Docker容器之间的连接，OVS连接，都是使用的这个技术



3、 同一网段下，容器可以ping通容器。







> 小结

Docker中的所有网络接口都是虚拟的。



> 思考场景
>
> 我们编写了一个微服务，database url=ip:port ，数据库IP变换了，我们希望可以通过名字访问服务。
>
> （高可用）



## **--link**

```shell
docker run -d -P --name tomcat03 --link tomcat02 tomcat
#这样tomcat03 可以ping tomcat02 通
# 但是tomcat02 不能ping tomcat03 通

docker network inspect # 可以查看网络信息

# 原理：
docker在容器内的hosts文件里直接写死被连接容器的ip 主机名 容器id
```

本质只是hosts的修改，但是如果容器重启导致ip变更，则会失效。

我们需要的是最定义网络，link不适用docker0！

docker0问题：它不支持容器名连接访问



## 自定义网络（容器互联）

> 查看所有的docker网络

**![image-20210416221559262](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210418140510.png)**



**网络模式**

bridge：桥接，docker默认

none：不配置网络

host：和宿主机共享网络

container：容器网络连通（用的少，有局限性）

**测试**

```shell
# 我们直接启动的命令有一个默认参数 --net bridge 
docker run -d -P --name tomcat01 tomcat
docker run -d -P --name tomcat01 --net bridge tomcat

# docker0 特点，默认域名不能访问，--link可以打通连接
docker network create --driver bridge --subnet 192.168.244.0/24 --gateway 192.168.244.1 mynet

# 查看mynet
cbaf02bb0e55   none      null      local
[root@group01 tomcatlogs]# docker network inspect mynet 
[
    {
        "Name": "mynet",
        "Id": "0bff61f0dcfa42739f210a2336f3db137780c96ae9d10ef1765fd2a79dc3ab4e",
        "Created": "2021-04-16T22:22:50.138416262+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.244.0/24",
                    "Gateway": "192.168.244.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]


# 创建依赖于mynet的tomcat
docker run -d -P --name tomcat-net-01 --net mynet tomcat
docker run -d -P --name tomcat-net-02 --net mynet tomcat

# 测试，通过tomcat01 ping tomcat02
[root@group01 tomcatlogs]# docker exec -it tomcat-net-01 ping tomcat-net-02
PING tomcat-net-02 (192.168.244.3) 56(84) bytes of data.
64 bytes from tomcat-net-02.mynet (192.168.244.3): icmp_seq=1 ttl=64 time=0.070 ms
64 bytes from tomcat-net-02.mynet (192.168.244.3): icmp_seq=2 ttl=64 time=0.050 ms
64 bytes from tomcat-net-02.mynet (192.168.244.3): icmp_seq=3 ttl=64 time=0.106 ms
^C
--- tomcat-net-02 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 50ms
rtt min/avg/max/mdev = 0.050/0.075/0.106/0.024 ms

# Ping通，完成
```

自定义的网络docker都直接把国内我们维护好了对应的关系。



**容器连接另外的网卡**

```shell
# 例如基于bridge网卡的容器tomcat01 连接mynet的tomcat-net-01
# 那么需要将tomcat01添加到mynet的路由表之中.
docker network connect mynet tomcat01

# 呈现的结果就是tomcat01一个容器两个虚拟IP
# 配置之后,tomcat02依然无法连接tomcat-net-01
```





# Docker Compose

目的: 定义运行多个容器

使用YAML文件进行配置!

特点:轻松高效.

作用:批量容器编排



Compose: 重要的概念

- 服务services, 容器. 应用 (web, redis, mysql...)
- 项目project, 一组关联的容器





## 安装

1. 下载

   ```shell
   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   # 很慢
   # 用下面的连接可以加速
   sudo curl -L "https://get.daocloud.io/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   
   
   # 可以直接用python安装
   pip install docker-compose
   
   # 提升权限
   chmod +x /usr/local/bin/docker-compose
   
   # 连接到/usr/bin下方便使用命令
   ln /usr/local/bin/docker-compose /usr/bin/docker-compose
   
   ```



## 体验

地址：

Python 应用，计数器，redis！



1. 应用 app.py
2. DockerFile 应用打包为镜像
3. docker-compse yaml文件（定义整个服务，需要的环境、web、redis）完整的上线服务！
4. 启动compose项目（docker-compose up）



## yaml规则



```yaml
version: ''
service:
	服务1：web
		images
		build
		network


volumes:
network:
configs

```

