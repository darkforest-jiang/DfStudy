linux软件安装
[toc]

# 安装目录
/usr：系统级的目录，可以理解为 C:/Windows/
/usr/lib：理解为 C:/Windows/System32
/usr/bin：几乎所有的系统可执行文件都会安装在这里
/usr/local/bin：则是可以存放一些系统用户自己特定的可执行文件，不用担心会被系统升级之类的行为覆盖，破坏，这个目录不是必须的
/usr/local：用户级的程序目录，可以理解为 C:/Progrem Files/。 用户自己编译的软件默认会安装到这个目录下。
/opt：用户级的程序目录 ，可以理解为D:/Software， opt有可选的意思， 这里可以用于放置第三方大型软件（或游戏），当你不需要时，直接 rm -rf 掉即可。在硬盘容量不够时，也可将/opt单独挂载到其他磁盘上使用。

源码目录：
/usr/src：系统级的源码目录。
/usr/local/src：用户级的源码目录。

# 安装方式
Linux下软件安装的方式主要有源码安装、rpm安装、yum安装，而常用的安装包主要有以下三种：
- tar包：例如software-1.2.3-1.tar.gz。它是使用UNIX系统的打包工具tar打包的。
- rpm包，如software-1.2.3-1.i386.rpm。它是Redhat Linux提供的一种包封装格式。(现在用的全称叫RPM Package Manager,以前叫Redhat Package Manager)
- dpkg包，如software-1.2.3-1.deb。它是Debain Linux提供的一种包封装格式。
  
而且，大多数Linux应用软件包的命名也有一定的规律，它遵循：
名称-版本-修正版-类型
例如：software-1.2.3-1.tar.gz
软件名称：software
版本号：1.2.3
修正版本：1
类型：tar.gz

## 源码安装
几乎所有的开源软件都支持在Linux下运行，而这些软件一般都以源码形式发放，只需要Linux安装了gcc、make、automake、autoconf都支持源码安装。
gcc安装详见[@Pro/linux/gcc.md]

安装方式：
- 下载tar包 上传到linux 解压出来/usr/local/src 其它目录都可以 不要放到/usr/local下 安装默认会放到/usr/local 文件名可能冲突
- cd 源码目录
- ./configure 执行配置
- make 编译
- make install 安装
- 通过软链接的方式设置环境变量 例如：
  ln -s /usr/local/dotnet/dotnet /usr/bin/dotnet

源码安装优点
- 文档齐全
- 因为可以定位到代码，所以debug方便
- 本机兼容性最好（由于是本机编译的，只要编译通过，就没有各种库的依赖的问题）
源码安装的缺点
- 编译麻烦
- 缺乏自动依赖管理，软件升级麻烦

## rpm包安装
rpm包安装几乎在所有Linux平台上都支持，它就像Windows下的exe安装文件一样，各种文件已经编译好，并打包，哪个文件在哪个文件夹里面都已经被指定好，所以很方便。

常用命令：
- rpm -ivh xxx.rpm 安装
- rpm -i xxx.rpm 安装
- rpm -e 包名 卸载
- rpm -e 包名 --nodeps 卸载 不检查依赖 可能哪些软件就不能正常工作了
- rpm -U 包名 更新
- rpm -Uvh *.rpm --nodeps --force 安装
- [opitons]
  - [-i] 显示套件的相关信息
  - [-v] 显示指令执行过程
  - [-h] 进度条
  - [-vv] 显示指令详细执行过程 便于排错
  - [-e] 删除指定的套件
  - [-U] 升级指定的套件
  - [--nodeps] 不验证套件档的关联性
  - [--force] 强行置换套件或文件

## yum安装
yum并不是一种包，它是安装包的一个软件，在CentOS中是软件包的管理器，yum也对依赖关系进行管理，但是必须要在联网的情况下完成。
- 基于 RPM 包管理，能够从指定的服务器自动下载 RPM 包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，
  无须繁琐地一次次下载、安装。
  yum 提供了查找、安装、删除某一个、一组甚至全部软件包的命令，而且命令简洁而又好记。
- 格式 yum [opitons] [command] [package]
- [opitons] -h:帮助 -y:当安装过程提示选择全部为 "yes" -q:不显示安装过程
- yum search [keyword] 查找软件包
- yum install [package] 安装软件包
- yum remove [package] 删除软件包
- yum check-update 列出所有可更新的软件清单命
- yum update 更新所有软件
- 清除缓存命令
  - yum clean packages: 清除缓存目录下的软件包
  - yum clean headers: 清除缓存目录下的 headers
  - yum clean oldheaders: 清除缓存目录下旧的 headers
  - yum clean, yum clean all (= yum clean packages; yum clean oldheaders) :清除缓存目录下的软件包及旧的 headers

# next

# end