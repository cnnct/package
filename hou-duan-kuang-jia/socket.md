##### 流程原理

1、在applicationContext-interfservice.xml中配置SocketServer启动类bean：

![](/assets/socket2.png)

2、在SocketServer启动类中读取para.properties配置文件，启动socket服务，创建初始化类SocketServerInitializer的bean（处理字符编码等）：



3、在初始化类SocketServerInitializer中创建接收返回数据类的bean进行数据处理：



