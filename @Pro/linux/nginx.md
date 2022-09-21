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

# nginx 设置开机启动
- cd /usr/lib/systemd/system 进入服务配置文件路径
- vi nginx.service 创建nginx服务配置文件
  [Unit]
  Description=nginx service
  After=network.target

  [Service]
  Type=forking
  ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
  ExecReload=/usr/local/nginx/sbin/nginx -s reload
  ExecStop=/usr/local/nginx/sbin/nginx -s stop
  PrivateTmp=true

  [Install]
  WantedBy=multi-user.target
- [esc]键退出编辑 [:wq]保存退出
- systemctl enable nginx 设置开机启动
- systemctl disable nginx 取消开机启动

# nginx配置
配置文件 /usr/local/nginx/conf/nginx.conf
server配置
http{
  listen       5000;
  server_name  myApiGateWay;
  
  location / {
    root   html;
    index  index.html index.htm;
  }

  # 反向代理
  # 一个地址转发
  location /weatherforecast {
    proxy_pass http://localhost:5000;
  }
  # 匹配多个路径转发到同一个地址写法
  location ~ ^/(路径1|路径2|路径3|路径4|路径5) {
        proxy_pass  跳转的地址;
        proxy_set_header $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header x-tif-uid $http_x_tif_uid;
        proxy_connect_timeout 600;
        proxy_read_timeout 600;
        proxy_send_timeout 600;
        proxy_ignore_client_abort on;
        proxy_next_upstream timeout;
  }

  # 负载均衡配置
  # 轮询方式
  upstream webServer {
    server 192.168.233.80:80; //服务器Aip
    server 192.168.233.90:80; //服务器Bip
  }
  # 权重方式
  upstream webServer {
    server 192.168.233.80:80 weight=3; //服务器Aip
    server 192.168.233.90:80 weight=7; //服务器Bip
  }
  server{
    listen 5000;
    server_name myapi;
    location / {
      index  index.html index.htm;
      proxy_pass http://webServer; //【webServer】和upstream 【webServer】名字一致
    }
  }

}


# 默认启动端口是80 注意端口占用 访问地址：192.168.43.6:80

# end