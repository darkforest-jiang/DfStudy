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
  linux请先安装 yum install iptables,
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
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
ExecReload=/bin/kill -s HUP $MAINPID
TimeoutSec=0
RestartSec=2
Restart=always

# Note that StartLimit* options were moved from "Service" to "Unit" in systemd 229.
# Both the old, and new location are accepted by systemd 229 and up, so using the old location
# to make them work for either version of systemd.
StartLimitBurst=3

# Note that StartLimitInterval was renamed to StartLimitIntervalSec in systemd 230.
# Both the old, and new name are accepted by systemd 230 and up, so using the old name to make
# this option work for either version of systemd.
StartLimitInterval=60s

# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity

# Comment TasksMax if your systemd version does not support it.
# Only systemd 226 and above support this option.
TasksMax=infinity

# set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes

# kill only the docker process, not all processes in the cgroup
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
- 启动、停止docker
  systemctl start docker
  systemctl stop docker
- docker version 验证安装

# 启动报错解决
- 
  SELINUX=enforcing

# docker常用命令
- docker ps 查询容器
- docker

# 删除
- 删除安装包 yum remove docker-ce
- 删除镜像、容器、配置文件等内容
  rm -rf /var/lib/docker docker默认位置 /var/log/docker

# end