## Docker概述

Docker 是一个应用程序开发、部署、运行的平台，使用 go 语言开发。相较于传统的主机虚拟化，Docker 提供了轻量级的应用隔离方案，并且为我们提供了应用程序快速扩容、缩容的能力。

### Docker为什么会出现

### Docker历史

### 相关地址

官网：https://www.docker.com/

文档：https://docs.docker.com/

仓库：https://hub.docker.com/

### Docker能干嘛

### Docker镜像

#### 镜像是什么

#### 镜像加载原理

## Docker安装

### Docker的基本组成

#### 镜像（image）

Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。

#### 容器（container）

镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

#### 仓库（repository）

仓库可看成一个代码控制中心，用来保存镜像。

仓库分为公有仓库和私有仓库。

Docker Hub （默认仓库）

国内仓库 （阿里、华为...）,配置镜像加速。

### 安装Docker

> 参考地址：https://docs.docker.com/engine/install/

#### 使用官方安装脚本自动安装

安装命令如下：

```shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

也可以使用国内 daocloud 一键安装命令：

```shell
curl -sSL https://get.daocloud.io/docker | sh
```

如果要使用 Docker 作为非 root 用户，则应考虑使用类似以下方式将用户添加到 docker 组：

```shell
sudo usermod -aG docker your-user
```

#### Centos安装Docker

安装最新版docker

```shell
# 1.卸载旧版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
# 2.安装依赖
yum install -y yum-utils device-mapper-persistent-data lvm2

# 3.设置镜像仓库
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 4.安装docker
sudo yum makecache fast # 更新软件包索引
sudo yum install docker-ce docker-ce-cli containerd.io

# 5.启动docker
sudo systemctl start docker
docker version
sudo docker run hello-world
```

安装指定docker版本

```shell
yum list docker-ce --showduplicates | sort -r
docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable
docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable
docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable

# docker-ce-18.09.1
sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
```

#### Debian安装Docker

安装最新版docker

```shell
# 1.卸载旧版本
sudo apt-get remove docker docker-engine docker.io containerd runc docker-ce docker-ce-cli 
# 主机上的映像，容器，卷或自定义配置文件不会自动删除。要删除所有图像，容器和卷
rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd

# 设置存储库
sudo apt update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
    
# 添加Docker的官方GPG密钥
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
# 安装docker
#sudo add-apt-repository \
#   "deb [arch=amd64] https://download.docker.com/linux/debian \
#   $(lsb_release -cs) \
#   stable"
   
sudo add-apt-repository  "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/debian buster stable"
   
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io
 
 docker version
 sudo docker run hello-world
```

安装指定docker版本

```shell
apt-cache madison docker-ce

  docker-ce | 5:18.09.1~3-0~debian-stretch | https://download.docker.com/linux/debian stretch/stable amd64 Packages
  docker-ce | 5:18.09.0~3-0~debian-stretch | https://download.docker.com/linux/debian stretch/stable amd64 Packages
  docker-ce | 18.06.1~ce~3-0~debian        | https://download.docker.com/linux/debian stretch/stable amd64 Packages
  docker-ce | 18.06.0~ce~3-0~debian        | https://download.docker.com/linux/debian stretch/stable amd64 Packages
  ...
  
  sudo apt-get install docker-ce=5:19.03.13~3-0~debian-buster docker-ce-cli=5:19.03.13~3-0~debian-buster containerd.io
```



#### Ubuntu安装Docker



### 阿里云镜像加速

#### CentOS

您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://eb7r4uym.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

#### Ubuntu

您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器

```shell

sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://eb7r4uym.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

#### Windows

```shell
1. 安装／升级Docker客户端
对于Windows 10以下的用户，推荐使用Docker Toolbox

Windows安装文件：http://mirrors.aliyun.com/docker-toolbox/windows/docker-toolbox/

对于Windows 10以上的用户 推荐使用Docker for Windows

Windows安装文件：http://mirrors.aliyun.com/docker-toolbox/windows/docker-for-windows/

2. 配置镜像加速器
针对安装了Docker Toolbox的用户，您可以参考以下配置步骤：
创建一台安装有Docker环境的Linux虚拟机，指定机器名称为default，同时配置Docker加速器地址。

docker-machine create --engine-registry-mirror=https://eb7r4uym.mirror.aliyuncs.com -d virtualbox default
查看机器的环境配置，并配置到本地，并通过Docker客户端访问Docker服务。

docker-machine env default
eval "$(docker-machine env default)"
docker info
针对安装了Docker for Windows的用户，您可以参考以下配置步骤：
在系统右下角托盘图标内右键菜单选择 Settings，打开配置窗口后左侧导航菜单选择 Docker Daemon。编辑窗口内的JSON串，填写下方加速器地址：
{
  "registry-mirrors": ["https://eb7r4uym.mirror.aliyuncs.com"]
}
编辑完成后点击 Apply 保存按钮，等待Docker重启并应用配置的镜像加速器。

注意
Docker for Windows 和 Docker Toolbox互不兼容，如果同时安装两者的话，需要使用hyperv的参数启动。

docker-machine create --engine-registry-mirror=https://eb7r4uym.mirror.aliyuncs.com -d hyperv default
Docker for Windows 有两种运行模式，一种运行Windows相关容器，一种运行传统的Linux容器。同一时间只能选择一种模式运行。xxxxxxxxxx 1. 安装／升级Docker客户端对于Windows 10以下的用户，推荐使用Docker ToolboxWindows安装文件：http://mirrors.aliyun.com/docker-toolbox/windows/docker-toolbox/对于Windows 10以上的用户 推荐使用Docker for WindowsWindows安装文件：http://mirrors.aliyun.com/docker-toolbox/windows/docker-for-windows/2. 配置镜像加速器针对安装了Docker Toolbox的用户，您可以参考以下配置步骤：创建一台安装有Docker环境的Linux虚拟机，指定机器名称为default，同时配置Docker加速器地址。docker-machine create --engine-registry-mirror=https://eb7r4uym.mirror.aliyuncs.com -d virtualbox default查看机器的环境配置，并配置到本地，并通过Docker客户端访问Docker服务。docker-machine env defaulteval "$(docker-machine env default)"docker info针对安装了Docker for Windows的用户，您可以参考以下配置步骤：在系统右下角托盘图标内右键菜单选择 Settings，打开配置窗口后左侧导航菜单选择 Docker Daemon。编辑窗口内的JSON串，填写下方加速器地址：{  "registry-mirrors": ["https://eb7r4uym.mirror.aliyuncs.com"]}编辑完成后点击 Apply 保存按钮，等待Docker重启并应用配置的镜像加速器。注意Docker for Windows 和 Docker Toolbox互不兼容，如果同时安装两者的话，需要使用hyperv的参数启动。docker-machine create --engine-registry-mirror=https://eb7r4uym.mirror.aliyuncs.com -d hyperv defaultDocker for Windows 有两种运行模式，一种运行Windows相关容器，一种运行传统的Linux容器。同一时间只能选择一种模式运行。shell
```



### Docker底层原理

#### Docker是怎么工作的

#### Docker为什么比虚拟机快

- docker悠着比虚拟机更少的抽象层
- docker利用的是宿主机的内核，vm需要Guest OS。



## Docker常用命令

### 帮助命令

> 官网参考：https://docs.docker.com/engine/reference/commandline/config/

```shell
docker version    # 版本信息
docker info       # 详细信息 ，包括镜像和容器
docker 命令 --help
```

### 镜像命令

#### images 

docker images 查看本地上的所有镜像

```shell
docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              bf756fb1ae65        9 months ago        13.3kB

# 解释
REPOSITORY
TAG 
IMAGE ID
CREATED
SIZE

# 可选项
名称，速记	默认	  描述
--all , -a		   显示所有image（默认隐藏中间image）
--digests		   显示摘要
--filter , -f	   根据提供的条件过滤输出
--format		   使用Go模板打印漂亮的图像
--no-trunc		   不要截断输出
--quiet , -q	   仅显示数字ID
```

#### search

docker search 搜索镜像

> 仓库地址：https://hub.docker.com/search?type=image

```shell
docker serach mysql
NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10031               [OK]                
mariadb                           MariaDB is a community-developed fork of MyS…   3676                [OK]                

# 可选项
--filter , -f		Filter output based on conditions provided
--format		Pretty-print search using a Go template
--limit	25	Max number of search results
--no-trunc		Don’t truncate output

docker search --filter stars=300 mysql
NAME                 DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql                MySQL is a widely used, open-source relation…   10031               [OK]                
mariadb              MariaDB is a community-developed fork of MyS…   3676                [OK]                
mysql/mysql-server   Optimized MySQL Server Docker images. Create…   735                                     [OK]
percona              Percona Server is a fork of the MySQL relati…   511                 [OK]           
```

 pull

docker pull 下载镜像

docker pull [OPTIONS] NAME[:TAG|@DIGEST]

```shell
docker pull mysql
Using default tag: latest # 默认为latest，等价于 docker pull mysql:latest
latest: Pulling from library/mysql
d121f8d1c412: Pull complete  # 分层下载，联合文件系统，节省空间
f3cebc0b4691: Pull complete 
1862755a0b37: Pull complete 
489b44f3dbb4: Pull complete 
690874f836db: Pull complete 
baa8be383ffb: Pull complete 
55356608b4ac: Pull complete 
dd35ceccb6eb: Pull complete 
429b35712b19: Pull complete 
162d8291095c: Pull complete 
5e500ef7181b: Pull complete 
af7528e958b6: Pull complete 
Digest: sha256:e1bfe11693ed2052cb3b4e5fa356c65381129e87e38551c6cd6ec532ebe0e808 # 签名
Status: Downloaded newer image for mysql:latest 
docker.io/library/mysql:latest # 真实地址 ，等价于 docker pull docker.io/library/mysql:latest


# 可选项
--all-tags , -a		下载存储库中所有标记的图像
--disable-content-trust	true	跳过图像验证
--platform		实验（守护程序）API 1.32+
如果服务器支持多平台，则设置平台
--quiet , -q		禁止详细输出
```

#### rmi 

docker rmi 删除镜像

```shell
# 删除指定镜像
docker rmi 镜像id
docker rmi mysql:5.7
# 删除多个镜像
docker rmi 镜像id1 镜像id2 镜像id3...
docker rmi mysql:5.7 nginx:latest
# 删除所有镜像
docker rmi -f $(docker images -aq)  

# 可选项
--force , -f		强制删除镜像
--no-prune		不要删除未加标签的父母
```



### 容器命令

#### run

创建一个新的容器并运行一个命令.

##### 语法

```shell
docker run [可选参数] image
```

##### 常用可选参数：

```shell
--name="nginx-lb"       # 为容器指定一个名称；
-d                      # 后台运行
-it                     # -i以交互模式运行容器，-t 为容器重新分配一个伪输入终端,docker run -it centos /bin/bash
-p                      # 指定端口映射，格式为：主机(宿主)端口:容器端口
    -p ip:主机端口：容器端口
    -p 主机端口：容器端口 # 最常用
    -p 容器端口
    容器端口
-P                      # 随机端口映射，容器内部端口随机映射到主机的端口
-v                      # 绑定一个卷
-e                      # 设置环境变量
-h                      # 指定容器的hostnam
```

##### 帮助

```shell
Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Run a command in a new container

Options:
      --add-host list                  Add a custom host-to-IP mapping (host:ip)
  -a, --attach list                    Attach to STDIN, STDOUT or STDERR
      --blkio-weight uint16            Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)
      --blkio-weight-device list       Block IO weight (relative device weight) (default [])
      --cap-add list                   Add Linux capabilities
      --cap-drop list                  Drop Linux capabilities
      --cgroup-parent string           Optional parent cgroup for the container
      --cidfile string                 Write the container ID to the file
      --cpu-period int                 Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int                  Limit CPU CFS (Completely Fair Scheduler) quota
      --cpu-rt-period int              Limit CPU real-time period in microseconds
      --cpu-rt-runtime int             Limit CPU real-time runtime in microseconds
  -c, --cpu-shares int                 CPU shares (relative weight)
      --cpus decimal                   Number of CPUs
      --cpuset-cpus string             CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string             MEMs in which to allow execution (0-3, 0,1)
  -d, --detach                         Run container in background and print container ID
      --detach-keys string             Override the key sequence for detaching a container
      --device list                    Add a host device to the container
      --device-cgroup-rule list        Add a rule to the cgroup allowed devices list
      --device-read-bps list           Limit read rate (bytes per second) from a device (default [])
      --device-read-iops list          Limit read rate (IO per second) from a device (default [])
      --device-write-bps list          Limit write rate (bytes per second) to a device (default [])
      --device-write-iops list         Limit write rate (IO per second) to a device (default [])
      --disable-content-trust          Skip image verification (default true)
      --dns list                       Set custom DNS servers
      --dns-option list                Set DNS options
      --dns-search list                Set custom DNS search domains
      --domainname string              Container NIS domain name
      --entrypoint string              Overwrite the default ENTRYPOINT of the image
  -e, --env list                       Set environment variables
      --env-file list                  Read in a file of environment variables
      --expose list                    Expose a port or a range of ports
      --gpus gpu-request               GPU devices to add to the container ('all' to pass all GPUs)
      --group-add list                 Add additional groups to join
      --health-cmd string              Command to run to check health
      --health-interval duration       Time between running the check (ms|s|m|h) (default 0s)
      --health-retries int             Consecutive failures needed to report unhealthy
      --health-start-period duration   Start period for the container to initialize before starting health-retries countdown (ms|s|m|h)
                                       (default 0s)
      --health-timeout duration        Maximum time to allow one check to run (ms|s|m|h) (default 0s)
      --help                           Print usage
  -h, --hostname string                Container host name
      --init                           Run an init inside the container that forwards signals and reaps processes
  -i, --interactive                    Keep STDIN open even if not attached
      --ip string                      IPv4 address (e.g., 172.30.100.104)
      --ip6 string                     IPv6 address (e.g., 2001:db8::33)
      --ipc string                     IPC mode to use
      --isolation string               Container isolation technology
      --kernel-memory bytes            Kernel memory limit
  -l, --label list                     Set meta data on a container
      --label-file list                Read in a line delimited file of labels
      --link list                      Add link to another container
      --link-local-ip list             Container IPv4/IPv6 link-local addresses
      --log-driver string              Logging driver for the container
      --log-opt list                   Log driver options
      --mac-address string             Container MAC address (e.g., 92:d0:c6:0a:29:33)
  -m, --memory bytes                   Memory limit
      --memory-reservation bytes       Memory soft limit
      --memory-swap bytes              Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --memory-swappiness int          Tune container memory swappiness (0 to 100) (default -1)
      --mount mount                    Attach a filesystem mount to the container
      --name string                    Assign a name to the container
      --network network                Connect a container to a network
      --network-alias list             Add network-scoped alias for the container
      --no-healthcheck                 Disable any container-specified HEALTHCHECK
      --oom-kill-disable               Disable OOM Killer
      --oom-score-adj int              Tune host's OOM preferences (-1000 to 1000)
      --pid string                     PID namespace to use
      --pids-limit int                 Tune container pids limit (set -1 for unlimited)
      --platform string                Set platform if server is multi-platform capable
      --privileged                     Give extended privileges to this container
  -p, --publish list                   Publish a container's port(s) to the host
  -P, --publish-all                    Publish all exposed ports to random ports
      --read-only                      Mount the container's root filesystem as read only
      --restart string                 Restart policy to apply when a container exits (default "no")
      --rm                             Automatically remove the container when it exits
      --runtime string                 Runtime to use for this container
      --security-opt list              Security Options
      --shm-size bytes                 Size of /dev/shm
      --sig-proxy                      Proxy received signals to the process (default true)
      --stop-signal string             Signal to stop a container (default "SIGTERM")
      --stop-timeout int               Timeout (in seconds) to stop a container
      --storage-opt list               Storage driver options for the container
      --sysctl map                     Sysctl options (default map[])
      --tmpfs list                     Mount a tmpfs directory
  -t, --tty                            Allocate a pseudo-TTY
      --ulimit ulimit                  Ulimit options (default [])
  -u, --user string                    Username or UID (format: <name|uid>[:<group|gid>])
      --userns string                  User namespace to use
      --uts string                     UTS namespace to use
  -v, --volume list                    Bind mount a volume
      --volume-driver string           Optional volume driver for the container
      --volumes-from list              Mount volumes from the specified container(s)
  -w, --workdir string                 Working directory inside the container
```

#### ps

列出容器。

##### 语法

```shell
docker ps [可选参数]
```

##### 常用可选参数

```shell
--all , -a		显示所有容器（默认显示为正在运行）
--filter , -f	根据提供的条件过滤输出
--format		使用Go模板打印漂亮的容器
--last , -n	-1	显示n个最后创建的容器（包括所有状态）
--latest , -l   显示最新创建的容器（包括所有状态）
--no-trunc		不要截断输出
--quiet , -q	仅显示数字ID
--size , -s		显示文件总大小
```

##### 帮助

```shell
Usage:  docker ps [OPTIONS]

List containers

Options:
  -a, --all             Show all containers (default shows just running)
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print containers using a Go template
  -n, --last int        Show n last created containers (includes all states) (default -1)
  -l, --latest          Show the latest created container (includes all states)
      --no-trunc        Don't truncate output
  -q, --quiet           Only display numeric IDs
  -s, --size            Display total file sizes
```

#### rm

删除一个或多个容器。

##### 语法

```shell
docker rm [可选参数] 
```

##### 常用可选参数：

```shell
--force , -f		强制删除正在运行的容器（使用SIGKILL）
--link , -l		删除指定的链接
--volumes , -v		删除与容器关联的匿名卷
```

##### 参考：

```shell
docker rm 00c60ba784a5 # 删除指定容器
docker rm 00c60ba784a5 e5c6d72e6f1e  # 删除多个容器 
docker rm -f $(docker ps -aq) # 强制删除所有容器
docker ps -aq | xargs dcoker rm -f # 强制删除所有容器
```

#### 容器的启停

```sh
docker start 容器id # 启动容器
docker restart 容器id # 重启容器
docker stop 容器id # 停止容器
docker kill 容器id # 强制停止容器

```

### 常用其他命令

#### 后台启动

```shell
docker run -d centos

# docker ps ,发现停止了
# 容器使用后台运行就必须要有一个前台进程，docker发现没有前台进程会自动停止容器
# nginx发现自己没有提供服务就会停止
```

#### 查看日志

```shell
docker logs -f -t --tail 10 容器id 

# 测试
docker run -d centos /bin/sh -c "while true;do echo 测试;sleep 1;done"

docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
39c63c733c1c        centos              "/bin/sh -c 'while t…"   3 seconds ago       Up 2 seconds                            unruffled_lamport


# 显示日志
--tf # 显示日志
--tail number # 显示日志条数

docker logs -tf --tail 10 39c63c733c1c
```

#### 查看容器中的进程信息

```shell
docker top 39c63c733c1c                                                                                                                                                                            
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD                                                                      
root                11505               11487               0                   17:30               ?                   00:00:00            /bin/sh -c while true;do echo 测试;sleep 1;done                          
root                13944               11505               0                   17:35               ?                   00:00:00            /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1
```

#### 查看镜像的元数据

```shell
docker inspect 39c63c733c1c

[                                                                                                                                                                                                                    
    {                                                                                                                                                                                                                
        "Id": "39c63c733c1c1cee8350090a0cc737a9b8780e9d6f3fcf890e49ab6317209820",                                                                                                                                    
        "Created": "2020-10-09T09:30:41.092331444Z",                                                                                                                                                                 
        "Path": "/bin/sh",                                                                                                                                                                                           
        "Args": [                                                                                                                                                                                                    
            "-c",                                                                                                                                                                                                    
            "while true;do echo 测试;sleep 1;done"                                                                                                                                                                   
        ],                                                                                                                                                                                                           
        "State": {                                                                                                                                                                                                   
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 11505,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-10-09T09:30:41.467369752Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:0d120b6ccaa8c5e149176798b3501d4dd1885f961922497cd0abef155c869566",
        "ResolvConfPath": "/var/lib/docker/containers/39c63c733c1c1cee8350090a0cc737a9b8780e9d6f3fcf890e49ab6317209820/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/39c63c733c1c1cee8350090a0cc737a9b8780e9d6f3fcf890e49ab6317209820/hostname",
        "HostsPath": "/var/lib/docker/containers/39c63c733c1c1cee8350090a0cc737a9b8780e9d6f3fcf890e49ab6317209820/hosts",
        "LogPath": "/var/lib/docker/containers/39c63c733c1c1cee8350090a0cc737a9b8780e9d6f3fcf890e49ab6317209820/39c63c733c1c1cee8350090a0cc737a9b8780e9d6f3fcf890e49ab6317209820-json.log",
        "Name": "/unruffled_lamport",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/333806365b194f4ff1202ea9e949bafc70f7c9adb1f265252ff5dc4ce7800f58-init/diff:/var/lib/docker/overlay2/ce2756ab6f53b7ebf9dc182a1583eb2b6baf6ba60ca633ff10aeb5f4e037dfc9/diff",
                "MergedDir": "/var/lib/docker/overlay2/333806365b194f4ff1202ea9e949bafc70f7c9adb1f265252ff5dc4ce7800f58/merged",
                "UpperDir": "/var/lib/docker/overlay2/333806365b194f4ff1202ea9e949bafc70f7c9adb1f265252ff5dc4ce7800f58/diff",
                "WorkDir": "/var/lib/docker/overlay2/333806365b194f4ff1202ea9e949bafc70f7c9adb1f265252ff5dc4ce7800f58/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "39c63c733c1c",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true;do echo 测试;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20200809",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "6e8ce89a34c483b68c177b84426627ce2964774e78b404eeb8604d04d88aee06",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/6e8ce89a34c4",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "44778335592ff1b4f48c8892bc897b6e00837b636207edc9009f85fb274db49b",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "d9ce2f7e2b000d11f386895aec90e4118b76f853bfac18411a5c0e379a7a04ed",
                    "EndpointID": "44778335592ff1b4f48c8892bc897b6e00837b636207edc9009f85fb274db49b",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]

```

#### 进入当前正在运行的容器

```shell
# 进入容器后开启新的终端
docker exec -it 39c63c733c1c /bin/bash
# 进入到容器当前的终端
docker attach 39c63c733c1c
```

#### 文件拷贝

```shell
# 将容器内文件拷贝出来
docker cp 39c63c733c1c:/home/test.txt  /home/
```



### 分层理解

## 容器数据卷

#### 什么是容器数据卷

#### 使用数据卷

##### Mysql

```shell
# 下载镜像
docker pull mysql:5.7
# 运行 挂载
docker run --name mysql-test -e MYSQL_ROOT_PASSWORD=admin123 -d -p 3307:3306 -v /home/lemon/test/mysql/conf:/etc/mysql/conf.d -v /home/lemon/test/mysql/data:/var/lib/mysql mysql:5.7
```

#### 具名挂载、匿名挂载

所有的docker容器内的卷，在没有指定目录的情况下都是在`/var/lib/docker/volumes/卷名`

推荐使用具名挂载。

具名挂载：

```shell
# 指定卷名
docker run -d -P --name nginx-test1 -v test-nginx:/etc/nginx nginx

# 查看所有volume的情况
docker volume ls
DRIVER              VOLUME NAME
local               test-nginx
```

匿名挂载：只指定了容器内的路径，没有定义容器外的路径。

```shell
# 匿名挂载
docker run -d -P --name nginx-test -v /etc/nginx nginx

# 查看所有volume的情况
docker vloume ls

DRIVER              VOLUME NAME
local               3995dca83386a359e7bda0cf33d3b595d4646407aec72e8e3a2724a81f7358db
```

拓展：

```shell
# ro/rw 指定容器对挂载卷的权限，默认为rw
# ro read only 
# rw read write
docker run -d -P --name nginx-test1 -v test-nginx:/etc/nginx:ro nginx
docker run -d -P --name nginx-test2 -v test-nginx:/etc/nginx:ro nginx
```

#### 数据卷容器

容器间的数据同步

```
--volums-from
```



## DockerFile

### 什么是 Dockerfile

Dockerfile 是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明。

简单的dockerfile：

```dockerfile
FROM centos

VOLUME ["volume1","volume2"]

CMD echo "-----end--------"
CMD /bin/bash
```

构建镜像：

```shell
docker build -f test-dockerfile -t lemon/centos:1.0 .

Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM centos
 ---> 0d120b6ccaa8
Step 2/4 : VOLUME ["volume1","volume2"]
 ---> Running in 24f505f0b7a8
Removing intermediate container 24f505f0b7a8
 ---> 7d9d019f61ef
Step 3/4 : CMD echo "-----end--------"
 ---> Running in f71eda7b3c75
Removing intermediate container f71eda7b3c75
 ---> 86fbc4d2beec
Step 4/4 : CMD /bin/bash
 ---> Running in 63eae0ac4089
Removing intermediate container 63eae0ac4089
 ---> a23cc496831e
Successfully built a23cc496831e
Successfully tagged lemon/centos:1.0
```

查看镜像：

```shell
docker images 
REPOSITORY                          TAG                 IMAGE ID            CREATED             SIZE
lemon/centos                        1.0                 a23cc496831e        7 seconds ago       215MB
```

运行：

```shell
docker run -it  a23cc496831e bash
```

### DockerFile构建过程

基础知识：

- 关键字都必须是大写
- 执行由上而下
- 使用#注释
- 每个指令都会创建提交一个新的镜像层并提交

![image-20201010154547333](https://gitee.com/SweetMcdull/img/raw/master/image-20201010154547333.png)

### 指令

```shell
FROM             # 基础镜像
MAINTAINER       # 作者，姓名+邮箱
RUN              # 用于执行后面跟着的命令行命令
```



## 网络原理

## Docker Compose

## Docker Swarm

## CI/CD Jenkins