SignalR
[toc]

# .net 6以上自带库

# 使用
1. 新建一个处理某个特定业务的类 继承 Hub
  public class SignalRHub : Hub
    {
        public override async Task OnConnectedAsync()
        {
            var cid = Context.ConnectionId;
            var client = Clients.Client(cid);

            await client.SendAsync("hello", $"你{cid}连上了");

            await Clients.All.SendAsync("hello", $"他 {cid} 连上了");
        }
    }
2. 启动注册
  builder.Services.AddSignalR();//注入服务

  app.MapHub<SignalRHub>("/signalRHub");//添加端点是客户端连接
3. Controller中使用
   private readonly IHubContext<SignalRHub> _hub;
     public HomeController(IHubContext<SignalRHub> hub)
        {
            _hub = hub;
        }

    前端调用了某个接口后触发 给客户端发送消息: 方法 参数
   [HttpPost]
        public async Task InfoChanged()
        {
            await _hub.Clients.All.SendAsync("Referesh", $"请刷新");
        } 
4. 客户端使用
 HubConnection conn;
        public void Test()
        {
            conn = new HubConnectionBuilder().WithUrl("https://localhost:7213/signalRHub").Build();//创建连接
            //接收到消息处理 方法名 参数 需更服务端定义一致
            conn.On<string>("hello", (msg) =>
            {
                Console.WriteLine($"hello - {msg}");
            });
            conn.On<string>("Referesh", (msg) =>
            {
                Console.WriteLine($"Referesh - {msg}");
            });
            conn.StartAsync();//启动连接
        }