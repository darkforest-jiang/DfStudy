linux 学习
[toc]

# 目录结构

## 目录构成
- / 根目录
  - /bin 
    Binaries (二进制文件) 的缩写, 这个目录存放着最经常使用的命令
  - /boot 
    这里存放的是启动 Linux 时使用的一些核心文件，包括一些连接文件以及镜像文件
  - /dev 
    Device(设备) 的缩写, 该目录下存放的是 Linux 的外部设备，在 Linux 中访问设备的方式和访问文件的方式是相同的
  - /etc
    Etcetera(等等) 的缩写,这个目录用来存放所有的系统管理所需要的配置文件和子目录
  - /home 
    用户的主目录，在 Linux 中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的
    root用户主目录是 /root，其它是 /home/用户名
    每一个新建的用户的目录都在 /home 目录下新建一个与用户名名字相同的目录 
    来表示用户的主目录 用户登录后 会默认登录到该目录下 
    在命令行界面前边会显示 [用户名@地址 ~] 例如：[root@localhost ~] "~" 来表示用户主目录
    cd $home 切换到用户主目录
  - /root
    root用户主目录
  - /lib /lib64 
    Library(库) 的缩写这个目录里存放着系统最基本的动态连接共享库，其作用类似于 Windows 里的 DLL 文件。
    几乎所有的应用程序都需要用到这些共享库。
  - /media
    linux 系统会自动识别一些设备，例如U盘、光驱等等，当识别后，Linux 会把识别的设备挂载到这个目录下
  - /mnt 
    系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在 /mnt/ 上，
    然后进入该目录就可以查看光驱里的内容了。
  - /opt 
    optional(可选) 的缩写，这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。
    默认是空的。
  - /proc 
    Processes(进程) 的缩写，/proc 是一种伪文件系统（也即虚拟文件系统），存储的是当前内核运行状态的一系列特殊文件，这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。这个目录的内容不在硬盘上而是在内存里，我们也可以直接修改里面的某些文件，比如可以通过下面的命令来屏蔽主机的ping命令，使别人无法ping你的机器：
    echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all
  - /sbin
    s 就是 Super User 的意思，是 Superuser Binaries (超级用户的二进制文件) 的缩写，这里存放的是系统管理员使用的系统管理程序
  - /selinux
    这个目录是 Redhat/CentOS 所特有的目录，Selinux 是一个安全机制，类似于 windows 的防火墙，但是这套机制比较复杂，这个目录就是存放selinux相关的文件的
  - /srv
    该目录存放一些服务启动之后需要提取的数据(不用服务器就是空)
  - /sys
    这是 Linux2.6 内核的一个很大的变化。该目录下安装了 2.6 内核中新出现的一个文件系统 sysfs 。
    sysfs 文件系统集成了下面3种文件系统的信息：针对进程信息的 proc 文件系统、针对设备的 devfs 文件系统以及针对伪终端的 devpts 文件系统。
    该文件系统是内核设备树的一个直观反映。
    当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。
  - /tmp
    tmp 是 temporary(临时) 的缩写这个目录是用来存放一些临时文件的
  - /usr
    usr 是 unix shared resources(共享资源) 的缩写，这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于 windows 下的 program files 目录
  - /usr/bin
    系统用户使用的应用程序
  - /usr/sbin
    超级用户使用的比较高级的管理程序和系统守护程序
  - /usr/src
    内核源代码默认的放置目录
  - /var
    variable(变量) 的缩写，这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件
  - /run
    是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。如果你的系统上有 /var/run 目录，应该让它指向 run
  - /lost+found
    一般情况下为空的，系统非法关机后，这里就存放一些文件

## 重要目录介绍
在 Linux 系统中，有几个目录是比较重要的，平时需要注意不要误删除或者随意更改内部文件。
/etc： 上边也提到了，这个是系统中的配置文件，如果你更改了该目录下的某个文件可能会导致系统不能启动。
/bin, /sbin, /usr/bin, /usr/sbin: 这是系统预设的执行文件的放置目录，比如 ls 就是在 /bin/ls 目录下的。
值得提出的是 /bin、/usr/bin 是给系统用户使用的指令（除 root 外的通用用户），而/sbin, /usr/sbin 则是给 root 使用的指令。
/var： 这是一个非常重要的目录，系统上跑了很多程序，那么每个程序都会有相应的日志产生，而这些日志就被记录到这个目录下，具体在 /var/log 目录下，另外 mail 的预设放置也是在这里

在Linux文件系统中有两个特殊的目录，一个用户所在的工作目录，也叫当前目录，可以使用一个点 . 来表示；另一个是当前目录的上一级目录，也叫父目录，可以使用两个点 .. 来表示


# 开启ssh 22端口
- 查看是否安装 openssh-server，执行命令：yum list installed | grep openssh-server
   如果有openssh-server，则是已安装
   如果没有则需要安装，执行安装命令：yum install openssh-server 需连网
- 打开sshd配置文件sshd_config ，执行命令：vi /etc/ssh/sshd_config 
   - 去掉监听端口、地址前的注释 [#]：
     Port 22 [√]
     #AddressFamily any
     ListenAddress 0.0.0.0
     ListenAddress ::
   - 开启远程登录
     #Authentication:
     #LoginGraceTime 2m
     PermitRootLogin yes [√]
     #StrictModes yes
     #MaxAuthTries 6
     #MaxSessions 10
   - 开启用户密码作为连接验证，保存退出
     PasswordAuthentication yes
     #PermitEmptyPasswords no
     PasswordAuthentication yes [√]
- 开启 sshd 服务，执行命令：sudo service sshd start
  停止SSH服务命令（service sshd stop）
  重启SSH服务命令（service sshd restart）
- 查看22端口是否被监听，执行命令：netstat -nltp|grep 22
- 设置开机自启动 systemctl enable sshd
  
# 登录
- ssh登录
  ssh root@192.168.1.1 回车后输入密码即可登录 linux ssh端口默认22

## 登录后主机名显示为 bogon
hostname root

# 基础命令
## su 切换用户

## sudo 以管理员身份权限运行命令
在运行其他命令前加上 sudo 即可

## clear 清屏

## --help、man 查看命令帮助
- --help 查看命令帮助 格式 命令 + --help
  会在终端显示命令的详细信息
- man 查看命令帮助 格式 man + 命令
  显示命令的详细详细信息 终端显示 就像进入到了一个文件中 里边可以操作
  操作命令如下
  - 空格 显示下一屏信息
  - 回车 显示下一行信息
  - b 显示上一屏信息
  - f 显示下一屏信息
  - q 退出

## history 显示历史输入命令

# 用户和组

# 目录/文件操作
## pwd 显示当前目录

## ls 列出目录及文件名
- ls 默认会以文件名排序
  仅显示目录和文件名 不显示 . 和 .. 及以.开头的隐藏目录
- ls -a
  显示所有目录和文件
- ls -A
  显示所有目录和文件 不显示 . 和 ..
- ls -l
  以较长格式列表显示目录及文件 共 9 列
  例如：
  -rwxr-xr-x. 1 root root 141760 6月  22 19:03 dotnet
  drwxrwxr-x. 3 root root     16 6月  22 19:14 host
  - 第1列 文件属性 11位 -rwxr-xr-x. drwxrwxr-x.
    - 第1位 文件类型
      - - 普通文件
      - d 目录
      - l 链接文件
      - p 管理文件
      - b 块设备文件
      - c 字符设备文件
      - s 套接字文件
    - 第2-10位 文件权限
      - r（Read，读取权限）：对文件而言，具有读取文件内容的权限；对目录来说，具有浏览目录的权限。
      - w（Write，写入权限）：对文件而言，具有新增、修改文件内容的权限；对目录来说，具有删除、移动目录内文件的权限。
      - x（eXecute，执行权限）：对文件而言，具有执行文件的权限；对目录来说，该用户具有进入目录的权限。
      - - 无权限
      - 第2-4位 文件创建者/所有者对该文件所具有的权限
      - 第5-7位 创建者/所有者所在的组的其他用户所具有的权限
      - 第8-10位 其他组的其他用户所具有的权限
  - 第二列 显示了个数字 目录/链接个数
    - 对于普通文件：链接数
    - 对于目录文件：第一级目录数 注意 显示个数-2=实际子目录数
  - 第三列 用户名
  - 第四列 组名
  - 第五列 文件大小/byte
  - 第六、七、八列 最后修改时间
  - 第九列 目录/文件名
- ls -h 将文件容量以人类较易读的方式（如GB，KB等）列出来
- ls -t 按时间排序
- ls -S 按文件容量排序

## cd 切换目录
- cd [dirname] 进入要切换的目录
- cd、cd ~、cd $home 进入用户主目录
- cd / 进入根目录
- cd .. 进入上级目录
- cd ../.. 进入上两级目录
- cd - 返回并进入此目录之前所在目录

## mkdir 创建目录
- mkdir [dirname] 创建目录
- mkdir -m [dirname] 创建目录并设定权限
- mkdir -p [dirname] 创建目录(文件存在 不报错) 直接将所需要的目录(包含上一级目录)递归创建起来
- mkdir [dirname1] [dirname1] [dirname1] 一次创建多个目录
- mkdir -p [dirname1]/[dirname2]/[driname3] 创建多级目录
- mkdir -p [dirname1]/{[dirname2],[dirname3]}/[dirname4]
  创建目录dirname1，其中包含2个目录dirname2,dirname3，这2个目录下都有一个目录dirname4

## rmdir 删除空目录
- rmdir [dirname] 删除空目录
- rmdir -p [dirnmae] 删除空目录 如果子目录有空 也会删除

## cp 复制目录/文件
- cp -a [src] [dest] 等同于 cp -pdr 该选项通常在拷贝目录时使用。它保留链接、文件属性，并递归地拷贝目录，其作用等于dpR选项的组合
- cp -p [src] [dest] 此时cp除复制源文件的内容外，还将把其修改时间和访问权限也复制到新文件中
  连同文件的属性一起复制过去，而非使用默认属性(备份常用)
- cp -d [src] [dest] 拷贝时保留链接 若来源档为连结档的属性(link file)，则复制连结档属性而非文件本身
- cp -r [src] [dest] 递归持续复制，用於目录的复制行为；(常用)
- cp -f [src] [dest] 删除已经存在的目标文件而不提示。为强制 (force) 的意思，若有重复或其它疑问时，不会询问使用者，而强制复制
- cp -i [src] [dest] 和f选项相反，在覆盖目标文件之前将给出提示要求用户确认。回答y时目标文件将被覆盖，是交互式拷贝
- cp -l [src] [dest] 进行硬式连结 (hard link) 的连结档建立，而非复制档案本身
- cp -s [src] [dest] 复制成为符号连结文件 (symbolic link)，亦即『快捷方式』档案

## mv 移动目录/文件，或修改名称
- mv -f [src] [dest] force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；
- mv -i [src] [dest] 若目标文件 (destination) 已经存在时，就会询问是否覆盖

## rm 删除目录/文件
- rm -i [fn] 删除的时候会提示是否确认删除，一次删除多个文件则每一个文件都会提醒
- rm -I [fn] 一次删除多个文件（大于三个），提示消息只提示一次
- rm -r [fn] 递归删除，用于删除目录
- rm -d [fn] 用于删除空目录，如果目录不为空，则无法删除
- rm -f [fn] 强制删除，不弹出任何提示，慎用

Linux系统没有回收站，rm删除就永远找不到了，特别是不要用 rm -rf 这个指令，删除的时候，最好用绝对路径，同时带-i的参数进行提醒，比较保险。那么如果真的删除了，可以用下面的步骤去尝试恢复文件
- 使用lsof命令查看当前是否有进程打开删除的文件
  lsof | grep /root/selenium/Spider/MySql.Data.dll       //查看进程中调用删除的dll文件
  lsof | grep diamon      //查看进程中调用删除的diamon文件
  sh         8455      root  255r      REG              253,0        173               764298 /tmp/diamon.sh (deleted)
  //从上面的输出可以看到，进程8455正在以只读的方式打开这个文件，打开的文件描述符为255，同时文件diamon.sh被标记删除,然后查看文件" /proc/8455/fd/255"
- 查看是否存在恢复数据
  cat /proc/13067/fd/86       // 13067 打开MySql.Data.dll进程ID，86文件描述符
  cat /proc/8455/fd/255         //查看只读的diamon数据
- 使用I/O重定向恢复文件
  cat /proc/23778/fd/86 > /root/selenium/Spider/MySql.Data.dll         //恢复删除的MySql.Data.dll
  cat /proc/8455/fd/255 > /tmp/diamon.sh       //恢复删除的diamon.sh

## vi 命令
- vi [fn] 进入编辑文件
- [I] 键 进入编辑模式
- [Esc] 键退出编辑模式
- [:w] 保存文件
- [:q] 退出vi编辑
- [:wq] 保存并退出vi编辑
- [:q!] 不保存退出vi编辑
- vi编辑显示 行头显示[~] 表示没有任何东西

# 链接
- ln -s [src] [dest] 建立软链接，类似windowns快捷方式
  可以跨文件系统

# 网络

## ip a 显示网卡及ip信息

## ifconfig  显示网络配置信息 网卡 ip dns mask等信息

# 网络配置
- 实体机
  - ip a 命令查看网卡信息
  - 修改网路设置配置文件
    ifcfg-eth0 是配置文件名称 eth0 是你网卡的名称 即 ip a 输出的env开头的
    vi /etc/sysconfig/network-scripts/ifcfg-eth0
    按 [i] 键进入编辑模式
    以下字段没有则回车添加：
    BOOTPROTO=static
    DNS1=本机首选DNS
    DNS2=本机备用DNS
    ONBOOT=yes
    IPADDR=设置的IP
    NETMASK=子网掩码
    GATEWAT=网关

    按 ESC 键退出编辑模式
    输入  :wq 退出保存
    输入  :q! 退出不保存
  - systemctl restart network 重启网卡
  - ifconfig 查看网络配置信息

- 虚拟机
  - 桥接模式 默认使用网卡Vmnet0 不提供 DHCP 服务
    虚拟机与外部主机在同一个网段上，相当于一个主机 既能与局域网内的主机通讯，也能与外部网络通信
    需要设置ip dns等网络配置信息 跟主机一样 ip换一个即可 配置方式参照上边[实体机]设置
  - NAT模式 默认使用 VMnet8，提供 DHCP 服务
    可以与物理机互相访问，也可访问外部网络 不能访问局域内其他机器 不会与局域网内其他 ip 地址发生冲突
    相当于虚拟机自己建了个局域网 可以与主机通信
    一班情况虚拟机可以ping通主机 主机可能ping不同虚拟机
    主机ping不同虚拟机解决办法：
    - 控制面板->网络和Internet->网络和共享中心->更改适配器设置->VMnet8网卡右键 属性->点 [配置]->
      高级->属性->Jumbo Packet->右边选择 Enable 启用
    - 网上有人说是不在一个网段造成的 需要把VMnet8网卡右键属性中的ipv4设置跟虚拟机一个网段即可
      但是自己试了 貌似是不行的 没准是自己设置错了

# 查看软件包是否安装
- rpm：rpm -qa | grep dotnet
- den：dpkg -l | grep dotnet
- yum：yum list installed | grep dotnet
- 其它方式安装的：whereis dotnet ，whereis 命令用于查找文件

# systemctl 服务管理
- systemctl status [service] 显示服务详细信息
- systemctl is-active [service] 仅显示是否Active
- systemctl is-enable [service] 显示服务是否开机启动
- systemctl enable [service] 使服务自启动
- systemctl disable [service] 使服务不自动启动
- systemctl list-units --type=service 显示所有已启动服务
- systemctl start [service] 启动服务
- systemctl stop [service] 停止服务
- systemctl restart [service] 重启服务
- systemctl reload [service] 重载服务
- systemctl mask [service] 注销服务
- systemctl unmask [service] 取消注销服务
  
# 防火墙
## firewalld防火墙
- systemctl status firewalld 查看防火墙状态
- systemctl start firewalld 启动防火墙
- systemctl stop firewalld 关闭防火墙
- systemctl mask firewalld 禁用防火墙
- firewall-cmd --list-ports 查看已开放端口
- firewall-cmd --zone=public --add-port=5000/tcp --permanent 添加端口 permanent表示永远存在 否则重启后就没有了
- firewall-cmd --zone=public --remove-port=5000/tcp --permanent 移除端口
- firewall-cmd --list-services 查看开放的服务
- firewall-cmd --add-service=https --permanent 添加https服务
- firewall-cmd --remove-service=https --permanent 移除https服务
- firewall-cmd --reload 重启防火墙 修改规则后需重启

## iptables防火墙
### 介绍
iptables 是集成在 Linux 内核中的包过滤防火墙系统。使用 iptables 可以添加、删除具体的过滤规则，iptables 默认维护着 4 个表和 5 个链，所有的防火墙策略规则都被分别写入这些表与链中。

“四表”是指 iptables 的功能，默认的 iptables规则表有 filter 表（过滤规则表）、nat 表（地址转换规则表）、mangle（修改数据标记位规则表）、raw（跟踪数据表规则表）：
  filter 表：控制数据包是否允许进出及转发，可以控制的链路有 INPUT、FORWARD 和 OUTPUT。
  nat 表：控制数据包中地址转换，可以控制的链路有 PREROUTING、INPUT、OUTPUT 和 POSTROUTING。
  mangle：修改数据包中的原数据，可以控制的链路有 PREROUTING、INPUT、OUTPUT、FORWARD 和 POSTROUTING。
  raw：控制 nat 表中连接追踪机制的启用状况，可以控制的链路有 PREROUTING、OUTPUT。
“五链”是指内核中控制网络的 NetFilter 定义的 5 个规则链。每个规则表中包含多个数据链：
  INPUT（入站数据过滤）： 进来的数据包应用此规则链中的策略
  OUTPUT（出站数据过滤）：外出的数据包应用此规则链中的策略
  FORWARD（转发数据过滤）：转发数据包时应用此规则链中的策略
  PREROUTING（路由前过滤）：对数据包作路由选择前应用此链中的规则（所有的数据包进来的时侯都先由这个链处理）
  POSTROUTING（路由后过滤）：对数据包作路由选择后应用此链中的规则（所有的数据包出来的时侯都先由这个链处理）
防火墙规则需要写入到这些具体的数据链中。

防火墙过滤框架:
  -> [PREROUTING] -> [路由决策] -> [FORWARD] -> [POSTROUTING]
                         ↓                           ↑
                     [INPUT] -> [LocalProcess] -> [OUTPUT]
可以看出，如果是外部主机发送数据包给防火墙本机，数据将会经过 PREROUTING 链与 INPUT 链；如果是防火墙本机发送数据包到外部主机，数据将会经过 OUTPUT 链与 POSTROUTING 链；如果防火墙作为路由负责转发数据，则数据将经过 PREROUTING 链、FORWARD 链以及 POSTROUTING 链。
————————————————
版权声明：本文为CSDN博主「一口Linux」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/daocaokafei/article/details/115091313

### 命令
- yum install -y iptables 在线安装
- yum install iptables-services 安装服务
- 离线安装 在centos安装包下packages下找rpm包安装即可
- iptables -V 验证安装
- iptables -nL --line-number 查看现有规则(列出序号)
- 链管理
  iptables -P [INPUT] ACCEPT/DROP 设置默认策略打开/关闭
  [INPUT] 链名，共五链 INPUT OUTPUT FORWARD PREROUTING POSTROUTING
- iptables -F 清空所有规则，只留下默认规则
- iptables -X 清空所有自定义规则
- 规则管理
  iptables [-A] [INPUT] [-s] 192.18.1.100/16 [-p] tcp --dport 22 [-j] Accept
  - [-A] 在当前链最后追加一条规则
    [-l num] 从位置num处插入规则，从1开始，num即为 -nL --line-number显示的序号
    [-R num] 修改第几条规则
    [-D num] 删除第几条规则
  - [INPUT] 链名，共五链 INPUT OUTPUT FORWARD PREROUTING POSTROUTING 
  - [-s] 来源IP网段或IP地址，这里指来源于网络号为192.18.1.100掩码为255.255.0.0的地址。
  - [-p] 规则应用在那个协议上，这里“-p tcp”即应用规则在tcp协议上，还有其他协议tcp等
  - [-j] 代表将要应用的规则，这里“-j DROP”即将要丢弃数据。其他的动作还有ACCEPT接受，REJECT拒绝等

# 进程

## ps 显示当前进程的状态，类似于 windows 的任务管理器。
- ps  -aux | grep dotnet 显示所有包含其他使用者的进程 |grep + 检索 检索关键字
  输出格式 USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
  USER: 行程拥有者
  PID: pid
  %CPU: 占用的 CPU 使用率
  %MEM: 占用的记忆体使用率
  VSZ: 占用的虚拟记忆体大小
  RSS: 占用的记忆体大小
  TTY: 终端的次要装置号码 (minor device number of tty)
  STAT: 该行程的状态:
    D: 无法中断的休眠状态 (通常 IO 的进程)
    R: 正在执行中
    S: 静止状态
    T: 暂停执行
    Z: 不存在但暂时无法消除
    W: 没有足够的记忆体分页可分配
    <: 高优先序的行程
    N: 低优先序的行程
    L: 有记忆体分页分配并锁在记忆体内 (实时系统或捱A I/O)
  START: 行程开始时间
  TIME: 执行的时间
  COMMAND:所执行的指令
- ps -ef | grep 进程关键字   查找指定进程 跟 ps -aux|grep 进程关键字   差不多

## kill 杀死进程
- kill [pid] 
  标准的kill命令，默认采用信号(signal)号是15，通常都能达到目的，终止有问题的进程，并把进程的资源释放给系统。然而，如果进程启动了子进程，只杀死父进程，子进程仍在运行，因此仍消耗资源。为了防止这些所谓的“僵尸进程”，应确保在杀死父进程之前，先杀死其所有的子进程。
- kill -9 [pid] 强制结束进程 [-9]是传递给进程的信号是[9] 即强制、尽快终止进程 
  这个强大和危险的命令迫使进程在运行时突然终止，进程在结束后不能自我清理。危害是导致系统资源无法正常释放，一般不推荐使用，除非其他办法都无效。

# tar/zip 压缩/解压缩
  linux 默认支持格式 [.gz] [.bz2] [.zip]
  - [.gz] 压缩率低 时间快 相对[.bz2]
  - [.bz2] 压缩率高 时间长 相对[.gz]
  - [.gz] [.bz2] 需要使用[tar]命令压缩/解压缩
  - [.zip] 需要使用[zip]压缩 [unzip]解压缩
## tar
- 格式：tar [必要参数] [选择参数] [文件]
- 命令选项
  - [-c] 创建打包文件
  - [-v] 显示打包或者解包的纤细信息
  - [-f] 指定文件名称 必须放到所有选项后面
  - [-z] 压缩或解压缩(.gz)
  - [-i] 压缩或解压缩(.bz2)
  - [-x] 解包
  - [-C] 解压缩到指定目录
- 压缩[.gz] tar -zcvf test.tar.gz *.txt
- 解压缩[.gz]  tar -zxvf test.tar.gz -C /root/testtar
  [-C]指定解压目录 如果不指定则默认当前目录
- 压缩[.bz2] tar -icvf test.bz2 *.txt
- 解压缩[.bz2] tar -ixvf test.bz2 -C /root/testtar
  [-C]指定解压目录 如果不指定则默认当前目录

## zip/unzip
- 压缩[zip] zip test.zip *.txt
- 解压缩[zip] unzip test.zip -d /root/testtar
  [-d] 解压缩到指定目录 不指定默认当前目录

# 日期时间

## date 日期
格式：date [option] [+FORMAT]
- date 显示当前时间
- date +"%Y-%m-%d" 格式化输出当前日期
- date -s 设置当前时间，只有root权限才能设置，其他只能查看
  date -s 20120523                # 设置成20120523，这样会把具体时间设置成00:00:00
  date -s 01:01:01                # 设置具体时间，不会对日期做更改
  date -s "01:01:01 2012-05-23"   # 这样可以设置全部时间
  date -s "01:01:01 20120523"     # 这样可以设置全部时间
  date -s "2012-05-23 01:01:01"   # 这样可以设置全部时间
  date -s "20120523 01:01:01"     # 这样可以设置全部时间
- hwclock -w 将当前时间和日期写入BIOS，避免重启后失效
- [FORMAT]参数  格式设定为一个[+]后接数个标记，其中可用的标记列表如下：
  %%   输出字符 %
  %a   星期几的缩写 (Sun..Sat)
  %A   星期的完整名称(Sunday..Saturday)。 
  %b   缩写的月份名称（例如，Jan）
  %B   完整的月份名称（例如，January）
  %c   本地日期和时间（例如，Thu Mar  3 23:05:25 2005）
  %C   世纪，和%Y类似，但是省略后两位（例如，20）
  %d   日 (01..31)
  %D   日期，等价于%m/%d/%y
  %e   一月中的一天，格式使用空格填充，等价于%_d
  %F   完整的日期；等价于 %Y-%m-%d
  %g   ISO 标准计数周的年份的最后两位数字
  %G   ISO 标准计数周的年份，通常只对%V有用
  %h   等价于 %b
  %H   小时 (00..23)
  %I   小时 (01..12)
  %j   一年中的第几天 (001..366)
  %k   小时，使用空格填充 ( 0..23); 等价于 %_H
  %l   小时, 使用空格填充 ( 1..12); 等价于 %_I
  %m   月份 (01..12)
  %M   分钟 (00..59)
  %n   新的一行，换行符
  %N   纳秒 (000000000..999999999)
  %p   用于表示当地的AM或PM，如果未知则为空白
  %P   类似 %p, 但是是小写的
  %r   本地的 12 小时制时间(例如 11:11:04 PM)
  %R   24 小时制 的小时与分钟; 等价于 %H:%M
  %s   自 1970-01-01 00:00:00 UTC 到现在的秒数
  %S   秒 (00..60)
  %t   插入水平制表符 tab
  %T   时间; 等价于 %H:%M:%S
  %u   一周中的一天 (1..7); 1 表示星期一
  %U   一年中的第几周，周日作为一周的起始 (00..53)
  %V   ISO 标准计数周，该方法将周一作为一周的起始 (01..53)
  %w   一周中的一天（0..6），0代表星期天
  %W   一年中的第几周，周一作为一周的起始（00..53）
  %x   本地的日期格式（例如，12/31/99）
  %X   本地的日期格式（例如，23:13:48）
  %y   年份后两位数字 (00..99)
  %Y   年
  %z   +hhmm 格式的数值化时区格式（例如，-0400）
  %:z  +hh:mm 格式的数值化时区格式（例如，-04:00）
  %::z  +hh:mm:ss格式的数值化时区格式（例如，-04:00:00）
  %:::z  数值化时区格式，相比上一个格式增加':'以显示必要的精度（例如，-04，+05:30）
  %Z  时区缩写 （如 EDT）

## cal 日历
格式 cal [options] [[[日] 月] 年]
- [options]
  -1 只显示当前月份
  -3 显示上个月、当月、下个月
  -s 周日作为一周第一天(默认)
  -m 周一作为一周第一天
  -j 显示每一天是本年的第几天
  -y 输出整年
- cal、cal -1、cal -s 输出当前月份
- cal [日] [月] [年] 显示几几年几月几日日历 中间可加[option]参数
  cal 1993
  cal 4 1993
  cal 5 4 1993

# end