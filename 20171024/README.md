http://mobile.51cto.com/ahot-442117.htm
http://www.linchaoqun.com/html/cms/content.jsp?menu=nodejs&id=1480081169735
http://www.cnblogs.com/xiezhengcai/p/3956401.html

服务端
io.on(‘connection’,function(socket));//监听客户端连接,回调函数会传递本次连接的socket

io.sockets.emit(‘String’,data);//给所有客户端广播消息

io.sockets.socket(socketid).emit(‘String’, data);//给指定的客户端发送消息

socket.on(‘String’,function(data));//监听客户端发送的信息

socket.emit(‘String’, data);//给该socket的客户端发送消息

广播消息

//给除了自己以外的客户端广播消息
socket.broadcast.emit("msg",{data:"hello,everyone"});
//给所有客户端广播消息
io.sockets.emit("msg",{data:"hello,all"});
分组

socket.on('group1', function (data) {
        socket.join('group1');
});
socket.on('group2',function(data){
        socket.join('group2');
 });
客户端发送

socket.emit(‘group1’)，就可以加入group1分组；
socket.emit(‘group2’)，就可以加入group2分组；

一个客户端可以存在多个分组（订阅模式）

踢出分组

socket.leave(data.room);

对分组中的用户发送信息

//不包括自己
socket.broadcast.to('group1').emit('event_name', data);
//包括自己
io.sockets.in('group1').emit('event_name', data);
broadcast方法允许当前socket client不在该分组内

获取连接的客户端socket

io.sockets.clients().forEach(function (socket) {
    //.....
})
获取分组信息

//获取所有房间（分组）信息
io.sockets.manager.rooms
//来获取此socketid进入的房间信息
io.sockets.manager.roomClients[socket.id]
//获取particular room中的客户端，返回所有在此房间的socket实例
io.sockets.clients('particular room')
另一种分组方式

io.of('/some').on('connection', function (socket) {
    socket.on('test', function (data) {
        socket.broadcast.emit('event_name',{});
    });
});
客户端

var socket = io.connect('ws://103.31.201.154:5555/some')
socket.on('even_name',function(data){
   console.log(data);
})
客户端都链接到ws://103.31.201.154:5555 但是服务端可以通过io.of(‘/some’)将其过滤出来。

另外，Socket.IO提供了4个配置的API：io.configure, io.set, io.enable, io.disable。其中io.set对单项进行设置，io.enable和io.disable用于单项设置布尔型的配置。io.configure可以让你对不同的生产环境（如devlopment，test等等）配置不同的参数。

客户端
建立一个socket连接

var socket = io(“ws://103.31.201.154:5555”);

监听服务消息

socket.on('msg',function(data){
    socket.emit('msg', {rp:"fine,thank you"}); //向服务器发送消息
    console.log(data);
});
socket.on(“String”,function(data)) 监听服务端发送的消息 Sting参数与服务端emit第一个参数相同

监听socket断开与重连。

socket.on('disconnect', function() {
    console.log("与服务其断开");
});


socket.on('reconnect', function() {
    console.log("重新连接到服务器");
});
客户端socket.on()监听的事件：
connect：连接成功
connecting：正在连接
disconnect：断开连接
connect_failed：连接失败
error：错误发生，并且无法被其他事件类型所处理
message：同服务器端message事件
anything：同服务器端anything事件
reconnect_failed：重连失败
reconnect：成功重连
reconnecting：正在重连
当第一次连接时，事件触发顺序为：connecting->connect；当失去连接时，事件触发顺序为：disconnect->reconnecting（可能进行多次）->connecting->reconnect->connect。
