安装 .netcore sdk
[toc]

# 微软官网下载linux 安装包

# ftp工具上传至linux服务器 

# linux系统下安装 .netcoresdk
- mkdir -p /usr/local/dotnet 创建dotnet安装目录 
  [-p] 指创建目录时 没有则创建 有了不报错
  /usr 目录是用户应用程序安装目录
  /usr/local 一般放到这个目录下
- tar zxf dotnet-sdk-6.0.302-linux-x64.tar.gz -C /usr/local/dotnet 解压安装包到此目录 
  [zxf] 压缩或解压缩(.gz)  解包  指定文件名称, 必须放到所有选项后面
  [-C] 解压缩到指定目录
- ln -s /usr/local/dotnet/dotnet /usr/bin/dotnet 通过软链接的方式设置环境变量
  [/usr/bin]
- dotnet --version 或 dotnet --info 查看是否安装成功
  
# 启动.netcore服务
- dotnet [dest] --urls="http://*:5000" --environment=Development

# 防火墙添加端口
- firewall-cmd --list-ports 查看以开放端口
- systemctl status firewalld 查看防火墙状态
- systemctl start firewalld 启动防火墙
- systemctl stop firewalld 关闭防火墙
- firewall-cmd --zone=public --add-port=5000/tcp --permanent 添加端口 permanent表示永远存在 否则重启后就没有了
- firewall-cmd --reload 重启防火墙 添加端口号后需重启