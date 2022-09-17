gcc 安装
[toc]

# rpm包方式安装
- centos的iso镜像解压出来的Packages文件夹下取出以下依赖包
  - gcc
    mpfr-3.1.1-4.el7.x86_64.rpm
    libmpc-1.0.1-3.el7.x86_64.rpm
    kernel-headers-3.10.0-123.el7.x86_64.rpm
    glibc-headers-2.17-55.el7.x86_64.rpm
    glibc-devel-2.17-55.el7.x86_64.rpm
    cpp-4.8.2-16.el7.x86_64.rpm
    gcc-4.8.2-16.el7.x86_64.rpm
  - g++
    libstdc++-devel-4.4.6-4.el6.x86_64.rpm
    gcc-c++-4.4.6-4.el6.x86_64.rpm
- ftp上传到linux
- 安装命令
  - 方式1 按如下顺序安装
    [gcc]
    rpm -ivh mpfr-3.1.1-4.el7.x86_64.rpm
    rpm -ivh libmpc-1.0.1-3.el7.x86_64.rpm
    rpm -ivh kernel-headers-3.10.0-123.el7.x86_64.rpm
    rpm -ivh glibc-headers-2.17-55.el7.x86_64.rpm
    rpm -ivh glibc-devel-2.17-55.el7.x86_64.rpm
    rpm -ivh cpp-4.8.2-16.el7.x86_64.rpm
    rpm -ivh gcc-4.8.2-16.el7.x86_64.rpm
    [g++]
    rpm -ivh libstdc++-devel-4.4.6-4.el6.x86_64.rpm
    rpm -ivh gcc-c++-4.4.6-4.el6.x86_64.rpm 
  - 方式2 统一安装 rpm -Uvh *.rpm --nodeps --force
- 验证安装 gcc -v   g++ -v

# 手动编译安装
- 参考 
  https://blog.csdn.net/weixin_38184741/article/details/107681479
  https://blog.csdn.net/qq_41054313/article/details/119453611
- 下载tar包 gcc-12.2.0.tar.gz https://mirrors.tuna.tsinghua.edu.cn/gnu/gcc/gcc-12.2.0/
- ftp上传至服务器
- 解压 tar -zxvf gcc-12.2.0.tar.gz -C /usr/local/src
- cd到解压目录 /usr/local/src/gcc-12.2.0
- 创建编译目录 mkdir build
- 进入build目录 cd build
- 配置
  执行以下命令：
  ../configure -enable-checking=release -enable-languages=c,c++ -disable-multilib
  释义：
  #–enable-languages表示你要让你的gcc支持那些语言，
  #–disable-multilib不生成编译为其他平台可执行代码的交叉编译器。
  #–disable-checking生成的编译器在编译过程中不做额外检查，
  #也可以使用*–enable-checking=xxx*来增加一些检查
- 编译 make
  编译耗时很长 可以使用 make -j4
  这一步需要时间非常久 可以使用 make -j 4 让make最多运行四个编译命令同时运行，加快编译速度（建议不要超过CPU核心数量的2倍）
- 安装 make install
- 验证安装 gcc -v   g++ -v
- 使用软链接方式设置环境变量 使全局可用
  ln -s /usr/local/bin/gcc /usr/bin/gcc

# yum命令轻松升级到高版本gcc的方法！简单粗暴！(请实验后再用)
背景：
直接通过yum install gcc安装的版本4.8.5太老了，很多新的库的用不起，没办法，只有升级了。
手动编译安装太过于麻烦，于是乎网上找到了这个方法。

方法:
sudo yum install centos-release-scl
sudo yum install devtoolset-7-gcc*
scl enable devtoolset-7 bash
which gcc
gcc --version
————————————————
版权声明：本文为CSDN博主「宇龍_」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/Think88666/article/details/109362132

# end
  