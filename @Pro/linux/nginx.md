nginx安装部署使用
[toc]

# 所需包
- gcc/g++  https://mirrors.tuna.tsinghua.edu.cn/gnu/gcc/gcc-12.2.0/
  官网：http://gcc.gnu.org/
  镜像站点：https://gcc.gnu.org/mirrors.html
  各版本下载地址：ftp://ftp.mirrorservice.org/sites/sourceware.org/pub/gcc/releases/gcc-4.7.2/
  glibc官网：ftp://ftp.gnu.org/gnu/glibc/
- pcre https://netix.dl.sourceforge.net/project/pcre/pcre/8.40/pcre-8.40.tar.gz
- libtool https://www.gnu.org/software/libtool/
- zlib http://zlib.net/zlib-1.2.12.tar.gz
- nginx http://nginx.org/en/download.html
  
# gcc/g++ 安装 参考[@pro/linux/gcc.md]

# pcre 安装
- 解压后进入目录
- ./configure 配置
- make 编译
- make install 安装

# libtool 安装
- 解压后进入目录
- ./configure 配置
- make 编译
- make install 安装

# zlib 安装
- 解压后进入目录
- ./configure 配置
- make 编译
- make install 安装

# nginx 安装
- 解压 /usr/local/soft 不要解压到/usr/local目录 因为安装的默认目录时用户目录
- 进入解压目录
- ./configure 配置
- make 编译
- make install 安装
  安装目录时 /usr/local/nginx

# nginx 命令
- 启动 /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
- 停止 /usr/local/nginx/sbin/nginx -s stop (quit)
- 重启 /usr/local/nginx/sbin/nginx -s reload
- 查看端口占用 netstat -tunlp

# nginx配置
配置文件 /usr/local/nginx/conf/nginx.conf
server配置
http{
  server
}


# 默认启动端口是80 注意端口占用 访问地址：192.168.43.6:80

# end