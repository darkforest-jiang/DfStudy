安装 .netcore 部署linux
[toc]

# 安装.netcoresdk
## 微软官网下载linux 安装包

## ftp工具上传至linux服务器 

## linux系统下安装 .netcoresdk
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

# systemd方式部署启动服务
- https://docs.microsoft.com/zh-cn/aspnet/core/host-and-deploy/linux-nginx?view=aspnetcore-6.0#code-try-7
- cd /usr/lib/systemd/system 进入用户服务配置文件夹
- vi myApi.service 新建service文件
- 编辑service文件
  [Unit]
  #描述
  Description=myApiService runat 1004
  After=network.target remote-fs.target nss-lookup.target

  [Service]
  #工作目录
  WorkingDirectory=/app/api/LeagueService
  #启动时执行的命令
  ExecStart=/usr/bin/dotnet /app/api/LeagueService/CesaLeagueService.dll --urls http://*:1004
  #只有出错时重启，Restart=always表示无论什么原因造成服务停止都会重启
  Restart=on-failure #服务崩溃时，十秒钟重启一次
  #Restart service after 10 seconds if the dotnet service crashes:
  RestartSec=10
  KillSignal=SIGINT
  SyslogIdentifier=CesaLeagueService
  Environment=ASPNETCORE_ENVIRONMENT=Development
  Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

  [Install]
  #该服务所在的target
  #这里符号链接放在/usr/lib/systemd/system/multi-user.target.wants目录下
  WantedBy=multi-user.target
  
- [Esc] [:wq] 退出保存
- 服务启动
  - systemctl start myApi 启动服务
  - systemctl stop myApi 停止服务
  - systemctl restart myApi 重启服务
  - systemctl enable myApi 开机自启
  - systemctl disable myApi 开机不自启
  - systemctl status myApi 查看服务状态

# supervisord 部署启动.netcore
- cd /etc/supervisord.d 进入supervisord.d 目录
- vi myApi.ini 创建.netcore服务配置文件[注意：如何是复制进去的请务必使用ANSI编码的文本]
  #配置程序名称
  [program:MyApi]
  #运行程序的命令
  command=dotnet TestApi.dll --urls http://*:5000
  #命令执行的目录 可以不见，则上边命令必须使用相对路径
  directory=/root/testapi
  #进程环境变量
  environment=ASPNETCORE_ENVIRONMENT=Development
  #进程执行的用户身份
  user=root
  #进程停止信号，可以为TERM, HUP, INT, QUIT, KILL, USR1, or USR2等信号默认为TERM >。当用设定的信号去干掉进程，退出码会被认为是expected，非必须设置
  stopsignal=INT
  #如果是true的话，子进程将在supervisord启动后被自动启动，默认就是true，非必须设置
  autostart=true
  #这个是设置子进程挂掉后自动重启的情况，有三个选项，false,unexpected和true。如果>为false的时候，无论什么情况下，都不会被重新启动，如果为unexpected，只有当进程的>退出码不在下面的exitcodes里面定义的退出码的时候，才会被自动重启。当为true的时候>>，只要子进程挂掉，将会被无条件的重启
  autorestart=true
  #这个选项是子进程启动多少秒之后，此时状态如果是running，则我们认为启动成功了,默认值为1，非必须设置
  startsecs=1
  #错误日志文件
  stderr_logfile=/root/testapi/supervisor.err.log
  #输出日志文件
  stdout_logfile=/root/testapi/supervisor.out.log

- supervisord -c /etc/supervisord.conf 启动supervisor
  - 报错1：Error: File contains no section headers
    可能是上边创建的myApi.ini文件文本编码不是[ANSI]编码导致的，复制的话先保存成[ANSI]编码的再复制粘贴
  - 报错2：Error: Another program is already listening on a port that one of our HTTP servers is configured to use.  Shut this program down first before starting supervisord.
    有其它进程占用 查找初进程 kill掉即可
    ps -aux | grep supervisord
    kill 9
  - 报错3：Unlinking stale socket /var/run/supervisor/supervisor.sock
    unlink /var/run/supervisor/supervisor.sock
- 服务配置文件 myApi.ini 文件修改后需要重新加载 supervisorctl reload
- supervisorctl 管理命令
  - supervisorctl start all 启动所有进程
  - supervisorctl restart all 重启所有进程
  - supervisorctl start [服务名] 启动某个服务
  - supervisorctl stop [服务名] 停止某个服务
  - supervisorctl restart [服务名] 重启某个服务
  - supervisorctl status 查看supervisor所有进程运行状态 
- 配置supervisord为开机自启（详细见[supervisor.md]）

# windows系统下使用命令发布.net core 程序
- 启动 [dos]/[powershell]窗口
- [cd] 切换到要发布项目的文件夹下，也就是项目.csproj文件的所在目录
- 也可以在此目录下 [Shirft] + [鼠标右键] 启动powershell
- 使用命令 dotnet publish -c Release -r [运行时] --no-self-contained -o [指定发布目录]  发布
  例如：
   dotnet publish -c Release -r linux-x64 --no-self-contained -o E:\Code\SVN\Trunk\API\CesaLeagueService\CesaLeagueService\bin\Release\net6.0\publish
  - [-c] : 定义生成配置 Debug 或 Release 不加的话 默认Debug
  - [-f] : 指定目标框架，选项有netcoreapp2.2，netcoreapp3.0，netcoreapp3.1 net6.0。如果要发布与项目文件.csproj文件TargetFramework不一样的版本，这里就需显示指定一下
  - [-r] : 指定运行时 win-x86 win-x64 win-arm win-arm64 osx-x64 linux-x64 linux-arm
  - [--no-self-contained] ：不使用框架依赖 即不打包.netcore运行环境 则必须在部署机器上安装.netcore运行环境
    [--self-contained] ：使用框架依赖 即打包.netcore运行环境 部署机器上可以不安装运行环境直接运行
  - [-o] : 指定发布目录
    如果不指定位置，默认在当前目录的bin目录下。如果发布成Self-Contained ，则在 ./bin/[configuration]/[framework]/[runtime]/publish/ 。否则就在 /bin/[configuration]/[framework]/publish/
    [configuration] Debug Release
    [framework] net6.0 netcoreapp2.2
    [runtime] 运行时 win-x86 win-x64 win-arm win-arm64 osx-x64 linux-x64 linux-arm

# linux使用gdi+需要安装libgdiplus
官网：https://www.mono-project.com/docs/gui/libgdiplus/

# end


  