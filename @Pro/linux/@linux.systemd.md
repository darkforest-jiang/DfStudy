systemd服务管理
[toc]

# 设置某个服务自启动
- cd /usr/lib/systemd/system 进入服务目录
- vi xxx.service 新建xxx服务配置文件，内容如下(按照各个服务作相应的配置 以下以.netcore api服务为例)，注意使用ANSI编码文字
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
- systemctl enable [service] 设置自启动

# systemctl服务管理命令
- systemctl status [service] 显示服务详细信息
- systemctl is-active [service] 仅显示是否Active
- systemctl is-enabled [service] 显示服务是否开机启动
- systemctl enable [service] 使服务自启动
- systemctl disable [service] 使服务不自动启动
- systemctl list-units --type=service 显示所有已启动服务
- systemctl start [service] 启动服务
- systemctl stop [service] 停止服务
- systemctl restart [service] 重启服务
- systemctl reload [service] 重载服务
- systemctl mask [service] 注销服务
- systemctl unmask [service] 取消注销服务

# end