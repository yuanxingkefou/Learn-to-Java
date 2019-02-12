#InetAddress

封装了IP和DNS

方法
```
getHostAddress()          返回IP地址
getHostName()             返回域名（本地为计算名）
InetAddress.getLocalHost();
InetAddress.getByName("IP地址|域名");
```

#InetSocketAddress  

封装了端口

* 创建对象

 `InetSocketAddress(String hostname,int port)`
 
* 方法
```
getAddress()
getHostName()
getPort()
```
