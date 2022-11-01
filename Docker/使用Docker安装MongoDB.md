# 使用Docker 安装MongoDB

>  注：非分布式

> 此教程所安装的内容仅适合学习Mongo使用。
>
> 实际使用的话务必学习Docker网络以及Docker挂载卷技术。
>
> 自定义MongoDB请务必掌握Dockerfile、Docker Compose、Docker Swarm。



## 0、环境

1. Linux 系统
2. Linux能联网



## 1、安装Docker

### Ubuntu

[官方文档](https://docs.docker.com/engine/install/ubuntu/)



**卸载命令**

> 鲁迅说过，先学会卸载，再学习使用

```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```



**安装命令**

```shell
# 1、更新
sudo apt-get update
sudo apt-get upgrade

# 2、安装一些基本库和依赖库
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
# 3、增加Docker源
# x86/amd64
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

#其他内核可以访问官网文档查看

# 4、安装Docker及Docker社区
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io


# 5、执行官方样例，验证安装
sudo docker run hello-world
```

![image-20210419193746280](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210419193750.png)



运行出来显示完全，就算是没问题了。





### CentOS

> 大同小异

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





## 2、部署MongoDB



### 一键启动

**安装docker-compose**

```shell
#下载到/usr/local
sudo curl -L "https://get.daocloud.io/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/docker-compose

#也可以使用下面的官方链接下载，但是国内可能很慢
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/docker-compose

#给刚下载完的docker-compose增加执行权限
sudo chmod +x /usr/local/docker-compose

#移动下载完成的docker-compose到/usr/bin （主要是这样不用去写环境变量了）
sudo mv /usr/local/docker-compose /usr/bin/

#官方说使用pip install docker-compose也可以，但是我没试过
```



**创建docker-compose启动文件**

```shell
#随便找个路径新建docker-compose.yml文件，我这里选择/homg/mongo
cd /home/mongo
nano docker-compose.yml
```

然后复制下面内容到文件内（密码请随意）

```yml
version: '3'

services:
  mongo:
    image: mongo
    restart: always
    ports:
      - 27017:27017
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: 123456

  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - 8081:8081
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: root
      ME_CONFIG_MONGODB_ADMINPASSWORD: 123456
```



> 文件内容来自官方发布于hub.docker上的内容，有兴趣可以查看：
>
> [DockerHub Mongo](https://hub.docker.com/_/mongo)



复制完后`Ctrl + O`，`Ctrl + X`保存并退出nano面板

> 使用nano进行复制的原因是因为vim会出现缩进混乱的问题。

执行命令启动MongoDB

```shell
# 正常启动
docker-compose up

# 带文件名启动，在文件名不是docker-compose.yml时使用
docker-compose -f xxx.yml up

# 后台启动
docker-compose up -d
```



启动后，打开浏览器，输入linux机器的ip:8081查看。

出现如下界面算是成功启动

![image-20210419195442037](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210419195442.png)





## 3、进入Mongo

确认启动成功以后，在命令行输入`docker ps -a`查看容器列表，获取到容器的id和容器名字

比如我的是如下的内容：

![image-20210419202740696](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210419202740.png)

> （我没有-a是因为我知道他们在运行，-a可以显示不在运行的容器，不加则不显示不在运行的容器）

从而知道我的`mongo-express`容器id是`da28fc1ab352`，名字是`mongo_mongo-express_1`

这里我们用的是Mongo容器的ID和名字，我的`mongo`容器id是`71ac14731807`，名字是`mongo_mongo_1`



首先确定mongo正常运行，如没有运行则运行它。

执行命令`docker exec -it 容器id mongo root`

> 命令中的-it是交互运行的意思，exec是进入`docker`容器的意思，容器id的地方填上mongo容器id或者容器名，id后面的mongo表示镜像名，一般都是`mongo`，按照`docker ps`的内容显示`Image`写，最后的`root`是`mongo`用户名的意思，如果你修改过上面的`docker-compose.yml`的话，按照你修改的用户名来。



这样一来我自己的进入命令就是：

```shell
docker exec -ti 71ac14731807 mongo root

#与之同效：
docker exec -ti mongo_mongo_1 mongo root
```



这样就进来了

![image-20210419203548290](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210419203548.png)





## 4、其他指令

> 这里主要是防止使用中出现一些问题，在这里留一手

如果重新执行docker-compose.yml文件（在已经执行过一次之后无论关闭与否又执行），可能会出现启动异常，网页打开缺少内容。

这时可以通过删除原先的容器，再执行compose。

```shell
# 容器删除指令
docker rm -f 容器id

# 查看容器id
docker ps -a
# 查看mongo和mongo express的容器id，复制并执行docker rm -f xxxx来删除它。
# 嫌麻烦也可以使用这个命令
# 删除全部容器
docker rm -f $(docker ps -aq)
# 谨慎操作！
```

