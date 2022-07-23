# `docker`

[Docker入门教程-阮一峰 - 掘金](https://juejin.cn/post/6844903561432662023)

Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会  生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了 Docker，就不用担心环境问题。😄
Docker 是一个开源的商业产品，有两个版本：社区版（Community Edition，缩写为 CE）和企业版（Enterprise Edition，缩写为 EE）

## 安装

1. Docker CE 的安装请参考官方文档
2. 安装完成后，运行下面的命令，验证是否安装成功

> [root@VM-4-11-centos ~]# **docker info**
> Client:
> Context:    default
> Debug Mode: false
> Plugins:
> app: Docker App (Docker Inc., v0.9.1-beta3)
> buildx: Build with BuildKit (Docker Inc., v0.5.1-docker)
> 
> Server:
> Containers: 0
> Running: 0
> Paused: 0
> Stopped: 0
> Images: 0
> Server Version: 20.10.5
> Storage Driver: overlay2
> Backing Filesystem: extfs
> Supports d_type: true
> Native Overlay Diff: true
> Logging Driver: json-file
> Cgroup Driver: cgroupfs
> Cgroup Version: 1
> Plugins:
> Volume: local
> Network: bridge host ipvlan macvlan null overlay
> Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
> Swarm: inactive
> Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
> Default Runtime: runc
> Init Binary: docker-init
> containerd version: 05f951a3781f4f2c1911b05e61c160e9c30eaa8e
> runc version: 12644e614e25b05da6fd08a38ffa0cfe1903fdec
> init version: de40ad0
> Security Options:
> seccomp
> Profile: default
> Kernel Version: 4.18.0-193.28.1.el8_2.x86_64
> Operating System: CentOS Linux 8 (Core)
> OSType: linux
> Architecture: x86_64
> CPUs: 2
> Total Memory: 1.784GiB
> Name: VM-4-11-centos
> ID: 2WXB:YI4L:UN2T:HIZ2:FW6K:4UOZ:N34V:VH2P:5PY6:NNGY:BIDM:BZBG
> Docker Root Dir: /var/lib/docker
> Debug Mode: false
> Registry: https://index.docker.io/v1/
> Labels:
> Experimental: false
> Insecure Registries:
> 127.0.0.0/8
> Registry Mirrors:
> https://mirror.ccs.tencentyun.com/
> Live Restore Enabled: false

---

> **[root@VM-4-11-centos ~]# docker version**
> Client: Docker Engine - Community
> Version:           20.10.5
> API version:       1.41
> Go version:        go1.13.15
> Git commit:        55c4c88
> Built:             Tue Mar  2 20:17:04 2021
> OS/Arch:           linux/amd64
> Context:           default
> Experimental:      true
> 
> Server: Docker Engine - Community
> Engine:
> Version:          20.10.5
> API version:      1.41 (minimum version 1.12)
> Go version:       go1.13.15
> Git commit:       363e9a8
> Built:            Tue Mar  2 20:15:27 2021
> OS/Arch:          linux/amd64
> Experimental:     false
> containerd:
> Version:          1.4.4
> GitCommit:        05f951a3781f4f2c1911b05e61c160e9c30eaa8e
> runc:
> Version:          1.0.0-rc93
> GitCommit:        12644e614e25b05da6fd08a38ffa0cfe1903fdec
> docker-init:
> Version:          0.19.0
> GitCommit:        de40ad0

## image文件

**Docker 把应用程序及其依赖，打包在 image 文件里面**

```
# 列出本机的所有 image 文件。
$ docker image ls

# 删除 image 文件
$ docker image rm [imageName]
```

```
[root@VM-4-11-centos ~]# docker image ls
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
[root@VM-4-11-centos ~]#
```

## hello world

`docker image pull`是抓取 image 文件的命令。`library/hello-world`是 image 文件在仓库里面的位置，其中`library`是 image 文件所在的组，`hello-world`是 image 文件的名字。

由于 Docker 官方提供的 image 文件，都放library组里面，所以它的是默认组，可以省略

```
$ docker image pull library/hello-world
$ docker image pull hello-world
```

**获取镜像**

```
[root@VM-4-11-centos ~]# docker image pull library/hello-world
Using default tag: latest
latest: Pulling from library/hello-world
2db29710123e: Pull complete
Digest: sha256:13e367d31ae85359f42d637adf6da428f76d75dc9afeb3c21faea0d976f5c651
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest
```

**查看列表**

```[root@VM-4-11-centos ~]# docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    feb5d9fea6a5   9 months ago   13.3kB
[root@VM-4-11-centos ~]#
```

**运行image文件，运行后就停止，因为不是提供的服务**
`$ docker container run hello-world`

```
[root@VM-4-11-centos ~]# docker container run hello-world
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

[root@VM-4-11-centos ~]#
```

**有些容器不会自动终止，因为提供的是服务。比如，安装运行 Ubuntu 的 image，就可以在命令行体验 Ubuntu 系统**

```
[root@VM-4-11-centos ~]# docker container run -it ubuntu bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
405f018f9d1d: Pull complete
Digest: sha256:b6b83d3c331794420340093eb706a6f152d9c1fa51b262d9bf34594887c2c7ac
Status: Downloaded newer image for ubuntu:latest
root@bf3801821804:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@bf3801821804:/#
```

**对于那些不会自动终止的容器，必须使用`docker container kill`命令手动终止**

`$ docker container kill [containID]`

```
[root@VM-4-11-centos ~]# docker container ls
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS     NAMES
bf3801821804   ubuntu    "bash"    2 minutes ago   Up 2 minutes             cool_tereshkova
[root@VM-4-11-centos ~]# docker container kill bf3801821804
bf3801821804
[root@VM-4-11-centos ~]#
```

## 容器文件

**image 文件生成的容器实例，本身也是一个文件，称为容器文件。** 也就是说，一旦容器生成，就会同时存在两个文件： image 文件和容器文件。而且关闭容器并不会删除容器文件，只是容器停止运行而已

```
# 列出本机正在运行的容器
$ docker container ls

# 列出本机所有容器，包括终止运行的容器
$ docker container ls --all
```

```
[root@VM-4-11-centos ~]# docker container ls --all
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                       PORTS     NAMES
bf3801821804   ubuntu        "bash"     5 minutes ago   Exited (137) 2 minutes ago             cool_tereshkova
fe7cf01224ac   hello-world   "/hello"   7 minutes ago   Exited (0) 7 minutes ago               reverent_ride
[root@VM-4-11-centos ~]#
```

终止运行的容器文件，依然会占据硬盘空间，可以使用[`docker container rm`]命令删除。
`$ docker container rm [containerID]`

```
[root@VM-4-11-centos ~]# docker container ls --all
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                       PORTS     NAMES
bf3801821804   ubuntu        "bash"     5 minutes ago   Exited (137) 2 minutes ago             cool_tereshkova
fe7cf01224ac   hello-world   "/hello"   7 minutes ago   Exited (0) 7 minutes ago               reverent_ride
[root@VM-4-11-centos ~]# docker container rm bf3801821804
bf3801821804
[root@VM-4-11-centos ~]# docker container ls --all
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
fe7cf01224ac   hello-world   "/hello"   8 minutes ago   Exited (0) 8 minutes ago             reverent_ride
[root@VM-4-11-centos ~]#
```

## Dockerfile 文件

它是一个文本文件，用来配置 image。Docker 根据 该文件生成二进制的 image 文件

## 实例：制作自己的 Docker 容器

[Git 安装配置 | 菜鸟教程 (runoob.com)](https://www.runoob.com/git/git-install-setup.html)

```
[root@VM-4-11-centos software]# git --version
git version 2.27.0
```

**下载源码**

```
$ git clone https://github.com/ruanyf/koa-demos.git
$ cd koa-demos
```

```
[root@VM-4-11-centos code]# ll
total 36
drwxr-xr-x 3 root root  4096 Feb  4  2018 koa-demos-master
-rw-r--r-- 1 root root 31763 Jul  8 22:45 koa-demos-master.zip
[root@VM-4-11-centos code]# cd koa-demos-master/
[root@VM-4-11-centos koa-demos-master]# ll
total 64
drwxr-xr-x 2 root root  4096 Feb  4  2018 demos
-rw-r--r-- 1 root root    78 Feb  4  2018 Dockerfile
-rw-r--r-- 1 root root 16950 Feb  4  2018 logo.png
-rw-r--r-- 1 root root   444 Feb  4  2018 package.json
-rw-r--r-- 1 root root 14633 Feb  4  2018 package-lock.json
-rw-r--r-- 1 root root 15539 Feb  4  2018 README.md
[root@VM-4-11-centos koa-demos-master]#
```

**编写 Dockerfile 文件**
首先，在项目的根目录下，新建一个文本文件`.dockerignore`，写入下面的内容。表示，这三个路径要排除，不要打包进入 image 文件。如果你没有路径要排除，这个文件可以不新建

```
.git
node_modules
npm-debug.log
```

**然后，在项目的根目录下，新建一个文本文件 Dockerfile**

```
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000
```

```
FROM node:8.4：该 image 文件继承官方的 node image，冒号表示标签，这里标签是8.4，即8.4版本的 node。
COPY . /app：将当前目录下的所有文件（除了.dockerignore排除的路径），都拷贝进入 image 文件的/app目录。
WORKDIR /app：指定接下来的工作路径为/app。
RUN npm install：在/app目录下，运行npm install命令安装依赖。注意，安装后所有的依赖，都将打包进入 image 文件。
EXPOSE 3000：将容器 3000 端口暴露出来， 允许外部连接这个端口。
```

**创建 image 文件**

```
$ docker image build -t koa-demo .
$ docker image build -t koa-demo:0.0.1 .
```

```
上面代码中，-t参数用来指定 image 文件的名字，后面还可以用冒号指定标签。如果不指定，默认的标签就是latest。最后的那个点表示 Dockerfile 文件所在的路径，上例是当前路径，所以是一个点。
如果运行成功，就可以看到新生成的 image 文件koa-demo了
```

```
[root@VM-4-11-centos koa-demos-master]# docker image build -t koa-demo .
Sending build context to Docker daemon  79.36kB
Step 1/5 : FROM node:8.4
8.4: Pulling from library/node
aa18ad1a0d33: Pull complete
90f6d19ae388: Pull complete
94273a890192: Pull complete
c9110c904324: Pull complete
788d73c0fb6b: Pull complete
f221bb562f24: Pull complete
14414a6a768d: Pull complete
af6d2b2ad991: Pull complete
Digest: sha256:080488acfe59bae32331ce28373b752f3f848be8b76c2c2d8fdce28205336504
Status: Downloaded newer image for node:8.4
---> 386940f92d24
Step 2/5 : COPY . /app
---> 7b1604ce5662
Step 3/5 : WORKDIR /app
---> Running in 32444f525c88
Removing intermediate container 32444f525c88
---> 779a938d510d
Step 4/5 : RUN ["npm", "install"]
---> Running in fc216071469e
npm info it worked if it ends with ok
npm info using npm@5.3.0
npm info using node@v8.4.0
...
...
added 61 packages in 4.086s
npm info ok
Removing intermediate container fc216071469e
---> 3677c907666f
Step 5/5 : EXPOSE 3000/tcp
---> Running in ec2728ce3152
Removing intermediate container ec2728ce3152
---> d6f90814c3d4
Successfully built d6f90814c3d4
Successfully tagged koa-demo:latest
```

```
[root@VM-4-11-centos koa-demos-master]# docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
koa-demo      latest    d6f90814c3d4   2 minutes ago   675MB
ubuntu        latest    27941809078c   4 weeks ago     77.8MB
hello-world   latest    feb5d9fea6a5   9 months ago    13.3kB
node          8.4       386940f92d24   4 years ago     673MB
[root@VM-4-11-centos koa-demos-master]#
```

**生成容器**

`docker container run`命令会从 image 文件生成容器

```
$ docker container run -p 8000:3000 -it koa-demo /bin/bash
或者
$ docker container run -p 8000:3000 -it koa-demo:0.0.1 /bin/bash
```

```
上面命令的各个参数含义如下：
-p参数：容器的 3000 端口映射到本机的 8000 端口。
-it参数：容器的 Shell 映射到当前的 Shell，然后你在本机窗口输入的命令，就会传入容器。
koa-demo:0.0.1：image 文件的名字（如果有标签，还需要提供标签，默认是 latest 标签）。
/bin/bash：容器启动以后，内部第一个执行的命令。这里是启动 Bash，保证用户可以使用 Shell。
```

*如果一切正常，运行上面的命令以后，就会返回一个命令行提示符。*

```
[root@VM-4-11-centos koa-demos-master]# docker container run -p 8000:3000 -it koa-demo /bin/bash
root@fdb155aee557:/app#
```

*这表示你已经在容器里面了，返回的提示符就是容器内部的 Shell 提示符。执行下面的命令。*
`root@66d80f4aaf1e:/app# node demos/01.js`

```
[root@VM-4-11-centos koa-demos-master]# docker container run -p 8000:3000 -it koa-demo /bin/bash
root@fdb155aee557:/app# node demos/01.js
```

*这时，Koa 框架已经运行起来了。打开本机的浏览器，访问 http://127.0.0.1:8000，网页显示"Not Found"，这是因为这个 demo 没有写路由*


```
[root@VM-4-11-centos ~]# curl http://127.0.0.1:8000
Not Found[root@VM-4-11-centos ~]#
```

*现在，在容器的命令行，按下 Ctrl + c 停止 Node 进程，然后按下 Ctrl + d （或者输入 exit）退出容器。此外，也可以用`docker container kill`终止容器运行**现在，在容器的命令行，按下 Ctrl + c 停止 Node 进程，然后按下 Ctrl + d （或者输入 exit）退出容器。此外，也可以用`docker container kill`终止容器运行*
**CMD 命令****上一节的例子里面，容器启动以后，需要手动输入命令`node demos/01.js`。我们可以把这个命令写在 Dockerfile 里面，这样容器启动以后，这个命令就已经执行了，不用再手动输入了**

```
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000
CMD node demos/01.js
```

**发布 image 文件**

