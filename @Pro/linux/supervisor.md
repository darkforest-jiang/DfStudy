安装[supervisor]
[toc]

# 在线安装
- yum install python-setuptools 安装python包管理巩固
- easy_install supervisor 安装supervisor

# 离线安装

## 所需包
- setuptools python包管理工具 
- elementtree
- meld3
- supervisor 支持支python2.x
  
## install setuptools
- 官网下载压缩包 https://pypi.org/project/setuptools/#files
- ftp上传到linux服务器 /usr/local/soft 并解压
- cd 进入解压文件夹
- python setup.py install 安装setuptools工具

## install meld3
- 官网下载压缩包 https://pypi.org/project/meld3/#files
- ftp上传到linux服务器 /usr/local/soft 并解压
- cd 进入解压文件夹
- python setup.py install 安装

## install supervisor
- 官网下载压缩包 https://pypi.org/project/supervisor/#files
- ftp上传到linux服务器 /usr/local/soft 并解压
- cd 进入解压文件夹
- python setup.py install 安装
- supervisor -v 查看版本号

# supervisorctl 管理命令
- supervisorctl reload 重新加载配置文件并重启服务
- supervisorctl start all 启动所有进程
- supervisorctl restart all 重启所有进程
- supervisorctl start [服务名] 启动某个服务
- supervisorctl stop [服务名] 停止某个服务
- supervisorctl restart [服务名] 重启某个服务
- supervisorctl status 查看supervisor所有进程运行状态 

# supervisor 设置自启动
- cd /usr/lib/systemd/system 进入服务目录
