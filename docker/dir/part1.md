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
