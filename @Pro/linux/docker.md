docker
[toc]

# 简介
官网：https://www.docker.com/

Docker 分为 CE 和 EE 两大版本。CE 即社区版（免费，支持周期 7 个月）
- EE 即企业版，强调安全，付费使用，支持周期 24 个月。
- Docker CE 分为 stable test 和 nightly 三个更新频道。 官方网站上有各种环境下的安装指南，这里主要介绍
- Docker CE 在 CentOS上的安装。

- docker存储位置
默认情况下 Docker的存放位置为：/var/lib/docker
可以通过命令查看具体位置：docker info | grep “Docker Root Dir”
- x修改位置
首先停掉 Docker 服务：
systemctl stop docker
然后移动整个/var/lib/docker 目录到目的路径
mkdir -p /root/data
mv /var/lib/docker /root/data/docker
ln -s /root/data/docker /var/lib/docker --快捷方式

之前使用的CentOS8由于停止维护了，这意味着无法再使用新版本的软件包更新了，由于Docker 支持 64 位版本 CentOS 7，并且要求内核版本不低于 3.10， CentOS 7 满足最低内核的要求，所以我们在CentOS 7安装Docker。
查看内核版本号 Linux 3.10.0-327.el7.x86_64 x86_64

# 注意事项、安装问题
- docker默认使用iptables防火墙 
  linux请先安装 yum install iptables, yum install iptables-services
  安装后停止firewalld systemctl stop firewalld、 systemctl disable firewalld
- docker启动失败解决方法
  - systemctl status docker.service 查看原因
  - journalctl -xe 先输入启动命令 然后使用系统日志命令查看具体出错原因 
    

# 卸载旧版本的docker
yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-selinux \
    docker-engine-selinux \
    docker-engine \
    docker-ce

# 在线安装
- 安装yum工具
  yum install -y yum-utils \
           device-mapper-persistent-data \
           lvm2 --skip-broken
- 设置仓库
  - 官方源地址(比较慢 可以使用下边国内的)
    yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
  - 阿里云(可以)
    yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  - 清华大学源(有问题)
    yum-config-manager \
    --add-repo \
    https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo
- 安装 yum install -y docker-ce

# 二进制包离线安装
安装文档 https://docs.docker.com/engine/install/binaries/ 选择 Install daemon and client binaries on Linux
下载地址 https://download.docker.com/linux/static/stable/ 选择对应系统下载docker-20.10.18.tgz
- ftp上传至服务器 /usr/local/soft
- 解压缩 tar -zxvf  tar -zxvf docker-20.10.18.tgz
- 解压缩后有以下几个文件
  containerd  containerd-shim  containerd-shim-runc-v2  ctr  docker  dockerd  docker-init  docker-proxy  runc
- 设置环境变量
  - cp docker/* /usr/bin 将这几个文件赋值到/usr/bin目录下
  - 设置软链接方式
    mv docker /usr/local 移动亚萨出来的目录到/usr/local下
    ln -s /usr/local/docker/containerd /usr/bin/containerd
    ln -s /usr/local/docker/containerd-shim /usr/bin/containerd-shim
    ln -s /usr/local/docker/containerd-shim-runc-v2 /usr/bin/containerd-shim-runc-v2
    ln -s /usr/local/docker/ctr /usr/bin/ctr
    ln -s /usr/local/docker/docker /usr/bin/docker
    ln -s /usr/local/docker/dockerd /usr/bin/dockerd
    ln -s /usr/local/docker/docker-init /usr/bin/docker-init
    ln -s /usr/local/docker/docker-proxy /usr/bin/docker-proxy
    ln -s /usr/local/docker/runc /usr/bin/runc

- 创建docker服务文件
  - cd /usr/lib/systemd/system/docker.service
  - vi docker.service 
  - 这个是联网安装自动生成的
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target docker.socket firewalld.service containerd.service
Wants=network-online.target
Requires=docker.socket containerd.service

[Service]
Type=notify
#the default is not to use systemd for cgroups because the delegate issues still
#exists and systemd currently does not support the cgroup feature set required
#for containers run by docker
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
ExecReload=/bin/kill -s HUP $MAINPID
TimeoutSec=0
RestartSec=2
Restart=always

#Note that StartLimit* options were moved from "Service" to "Unit" in systemd 229.
#Both the old, and new location are accepted by systemd 229 and up, so using the old location
#to make them work for either version of systemd.
StartLimitBurst=3

#Note that StartLimitInterval was renamed to StartLimitIntervalSec in systemd 230.
#Both the old, and new name are accepted by systemd 230 and up, so using the old name to make
#this option work for either version of systemd.
StartLimitInterval=60s

#Having non-zero Limit*s causes performance problems due to accounting overhead
#in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity

#Comment TasksMax if your systemd version does not support it.
#Only systemd 226 and above support this option.
TasksMax=infinity

#set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes

#kill only the docker process, not all processes in the cgroup
KillMode=process
OOMScoreAdjust=-500

[Install]
WantedBy=multi-user.target

  - 这个是网上找的 貌似不太行
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target

[Service]
Type=notify
ExecStart=/usr/bin/dockerd
ExecReload=/bin/kill -s HUP $MAINPID
LimitNOFILE=infinity
LimitNPROC=infinity
TimeoutStartSec=0
Delegate=yes
KillMode=process
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s

[Install]
WantedBy=multi-user.target

# docker命令
- docker version 验证安装,docker启动后执行该命令会输出以下内容 Client、Servier信息：
  Client: Docker Engine - Community
 Version:           20.10.18
 API version:       1.41
 Go version:        go1.18.6
 
 Git commit:        b40c2f6
 Built:             Thu Sep  8 23:14:08 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.18
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.18.6
  Git commit:       e42327a
  Built:            Thu Sep  8 23:12:21 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.8
  GitCommit:        9cd3357b7fd7218e4aec3eae239db1f68a5a6ec6
 runc:
  Version:          1.1.4
  GitCommit:        v1.1.4-0-g5fd4c4d
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
  
- systemctl start docker 启动
  systemctl stop docker 停止

# 启动报错解决

# 构建docker镜像 容器中运行
- 在软件发布后的文件夹下编写Dockerfile文件 以.net6 webApi 发布docker为例
  #配置基本镜像
  FROM mcr.microsoft.com/dotnet/aspnet:6.0
  #配置工作目录
  WORKDIR /app
  #暴露端口(即监听宿主机端口转发至docker)
  EXPOSE 5000
  #复制发布文件
  COPY . /app
  #设置进入点
  ENTRYPOINT ["dotnet", "TestApi.dll"]
- cd 到发布文件夹
- docker build -t df.myapi.docker .     构建镜像 注意以 空格+. 结尾，镜像名称只能小写
- docker run --name=mydocker -p 5000:80 -d df.myapi.docker 指定在某个容器下运行 默认网桥模式
  前面是容器名称 -p指定容器运行的端口 最后是镜像名称
  容器会监听宿主机5000端口映射到docker容器80端口
- curl http://localhost:5000/weatherforecast 测试接口
  如果报错 curl: (56) Recv failure: Connection reset by peer 
  解决办法：
  (1)可以加上 --net=host 启动直接使用宿主机ip
  - docker run --name=mydocker -p 80:80 -d --net=host df.myapi.docker
  --net=host 容器直接使用宿主服务器的ip，这样开发用的物理机就能够访问这个容器的接口了
  (2) 重建docker0网桥 参考后边重建网桥
 
- 每次更新程序后需要依次执行以下命令(可以制作成shell脚本一键执行)
  1. 停止容器
  2. 删除容器
  3. 删除镜像
  4. 构建镜像
  5. 在指定容器中运行

# docker常用命令
- docker ps 列出所有正在运行的容器
- docker ps -a 列出所有容器（包括已停止的容器）
- docker start 容器名称或Id 启动某个容器
- docker stop 容器名称或Id 停止某个容器
- docker rm 容器名称或Id 删除某个容器
- docker images 列出所有镜像
- docker rmi 镜像名称或Id 删除某个镜像
- docker exec -it [容器] /bin/bash  进入容器内部

# curl报错 curl: (56) Recv failure: Connection reset by peer 解决： 重建网桥docker0
- 安装网桥包 yum install bridge-utils -y
- 重建网桥
  #停止docker
  systemctl stop docker
  pkill docker
  #停用网卡
  ip link set dev docker0 down
  #删除docker0网桥
  brctl delbr docker0
  #增加docker0网桥
  brctl addbr docker0
  #增加网卡
  ip addr add 172.16.10.1/24 dev docker0
  #启用网卡
  ip link set dev docker0 up
- 开启宿主机的ipv4转发功能
  - sysctl net.ipv4.ip_forward 查看 1-开启 0-未开启
  - vi /etc/sysctl.conf 添加一行 net.ipv4.ip_forward = 1
  - systemctl restart network
- systemctl restart docker 重启docker

# docker可视化 portainer
- docker search portainer |head -n -3
- docker pull portainer/portainer
- docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
- http://localhost:9000 创建用户 第一次创建完需重启portainer容器 第二次继续创建即可

## 重置密码
- 停止portainer容器
- find / -name portainer.key
- docker run --rm -v /var/lib/docker/volumes/portainer_data/_data:/data portainer/helper-reset-password
- 会出现以下提示文本 会提示用户名和密码
  2023/06/10 09:39:18 Password successfully updated for user: admin
  2023/06/10 09:39:18 Use the following password to login: 5tcL1h*^8wHl63X/OrW9s(JMG][~F40-
- 重启 portainer 容器
- 按提示的 用户名 和 密码 登录
- 登录后修改密码即可 新版最少12位 ...
- 我的默认账户： admin   adminadminadmin


# 删除
- 删除安装包 yum remove docker-ce
- 删除镜像、容器、配置文件等内容
  rm -rf /var/lib/docker docker默认位置 /var/log/docker

# docker compose 容器编排


# end