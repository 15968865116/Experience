
# 手动安装

## 卸载旧版本

Docker 的旧版本被称为 docker，docker.io 或 docker-engine 。如果已安装，请卸载它们：

```bash
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

当前称为 Docker Engine-Community 软件包 docker-ce 。

安装 Docker Engine-Community，以下介绍两种方式。

使用 Docker 仓库进行安装
在新主机上首次安装 Docker Engine-Community 之前，需要设置 Docker 仓库。之后，您可以从仓库安装和更新 Docker 。

## 设置仓库

更新 apt 包索引。

```bash
$ sudo apt-get update
```

安装 apt 依赖包，用于通过HTTPS来获取仓库:

```bash
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

添加 Docker 的官方 GPG 密钥：

```bash
$ curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -

9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88
```

通过搜索指纹的后8个字符，验证您现在是否拥有带有指纹的密钥。

```bash
$ sudo apt-key fingerprint 0EBFCD88

pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]  
```

使用以下指令设置稳定版仓库 添加Docker软件源

  ```bash
$ add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"
  ```

安装 Docker Engine-Community
更新 apt 包索引。

```bash
$ sudo apt-get update
```

安装最新版本的 Docker Engine-Community 和 containerd ，或者转到下一步安装特定版本：

```bash
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

要安装特定版本的 Docker Engine-Community，请在仓库中列出可用版本，然后选择一种安装。列出您的仓库中可用的版本：

```bash
$ apt-cache madison docker-ce

  docker-ce | 5:18.09.1~3-0~ubuntu-xenial | https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu  xenial/stable amd64 Packages
  docker-ce | 5:18.09.0~3-0~ubuntu-xenial | https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu  xenial/stable amd64 Packages
  docker-ce | 18.06.1~ce~3-0~ubuntu       | https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu  xenial/stable amd64 Packages
  docker-ce | 18.06.0~ce~3-0~ubuntu       | https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu  xenial/stable amd64 Packages
  ```
  
使用第二列中的版本字符串安装特定版本，例如 5:18.09.1~ 3-0~ ubuntu-xenial。

```bash
$ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
```

测试 Docker 是否安装成功，输入以下指令，打印出以下信息则安装成功:

```bash
$ sudo docker run hello-world

Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete                                                                                                                                  Digest: sha256:c3b4ada4687bbaa170745b3e4dd8ac3f194ca95b2d0518b417fb47e5879d9b5f
Status: Downloaded newer image for hello-world:latest


Hello from Docker!
This message shows that your installation appears to be working correctly.


To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.


To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash


Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/


For more examples and ideas, visit:
 https://docs.docker.com/get-started/
 ```

# 使用镜像

## 列出镜像列表

```bash
~$ docker image
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              14.04               90d5884b1ee0        5 days ago          188 MB
php                 5.6                 f40e9e0f10c8        9 days ago          444.8 MB
nginx               latest              6f8d099c3adc        12 days ago         182.7 MB
mysql               5.6                 f2e8d6c772c0        3 weeks ago         324.6 MB
httpd               latest              02ef73cf1bc0        3 weeks ago         194.4 MB
ubuntu              15.10               4e3b13c8a266        4 weeks ago         136.3 MB
hello-world         latest              690ed74de00f        6 months ago        960 B
training/webapp     latest              6fae60ef3446        11 months ago       348.8 MB
```

内容说明

* REPOSITORY：表示镜像的仓库源

* TAG：镜像的标签

* IMAGE ID：镜像ID

* CREATED：镜像创建时间

* SIZE：镜像大小

## 获取一个新的镜像

当我们在本地主机上使用一个不存在的镜像时 Docker 就会自动下载这个镜像。如果我们想预先下载这个镜像，我们可以使用 docker pull 命令来下载它。

```bash
~$ docker pull 你需要的镜像
```

## 查找镜像

```bash
~$ docker search 你需要查找的镜像
```

## 运行镜像

```bash
~$ docker run 你已有的镜像
```

## 命令行进入镜像

```bash
docker run -it ubuntu /bin/bash
```

## 查看正在运行的容器

```bash
docker ps
```

## 查看已停止运行的容器

```bash
docker ps -a
```

## 停止运行容器

```bash
docker stop <容器 ID>
```

## 删除镜像

```bash
~$ docker rmi
```

## 复制文件到容器

```bash
docker cp 本地文件路径 容器id:容器内路径
```

# docker登录到仓库

```bash
docker login -u 账号 -p 密码 具体网址
```

# 生成镜像

通过 docker commit
-a auther -m 备注
gochatting:v2 这是镜像名字和tag
```bash
sudo docker commit -a "zjj" -m "my goproject" c001465e24c2  gochatting:v2 
```

通过Dockerfile
编写Dockerfile
在Dockerfile文件目录下
```bash
sudo docker build -t="gochatting:v3" .
```

# 打包镜像

```bash
docker save -o gochatting.tar gochatting:v2
```

# 上传至dockerhub

需要注册 然后docker login
上传的镜像必须要以自己的名字开头
比如zhujunjiechina/xxx:v3这样
