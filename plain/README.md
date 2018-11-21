1. app used gateway and logicserver and dbserver.

2. gateway（网关）的作用主要是账号验证和一些无效连接拦截
   logicserver 作为逻辑处理器，负责游戏世界的所有处理
   dbserver 作为数据库查询接入处理的服务器（暂放到逻辑服务器中处理）

   在logicserver中使用redis为玩家数据作为缓存和共享的方式，提供单个模块的缓存接口
   需要指定某个关键字作为更新（通用模块使用玩家ID），现在的缓存暂放在脚本层处理

   网络，统一使用protobuf的协议
   考虑动态加载协议进行注册的方式（包括C++层） -- 暂定

3. 关于框架加载环境变量文件的改进
   可以让配置来开启任意的服务，比如一个监听端口的服务器和连接到指定服务器的客户端
   连接，当然数据库也可以扩展多个长连接服务
   服务器和客户端连接中设置了加密连接，那么客户端连接的时候需要握手（发送指定
   的握手包）获得监听服务器的认可，才可以正常处理该连接的其他消息，否则在未握手
   成功前发送任意消息将被服务器断开连接（在客户端第一次连接到服务器的时候，就将
   该连接加入检测列表，如果该连接在指定时间内未握手成功，也将断开连接，暂时使用
   简单的验证监听端和连接端的加密字符串来实现，后期可以考虑动态加密）