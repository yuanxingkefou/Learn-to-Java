#UDP通信

特点：以数据为中心，不安全，效率高

类:

DatagramSocket  DatagramPacket

1)客户端

* 创建客户端   DatagramSocket
* 准备数据
* 打包    DatagramPacket+服务器地址及端口
* 发送
* 释放资源

```

/**
 * 客户端
 * @author ASUS
 *
 */
public class MyClient {
	public static void main(String[] args) throws IOException {
		//创建服务端+端口
		DatagramSocket client=new DatagramSocket(6665);
		//准备数据
		String msg="udp编程";
		byte[] data=msg.getBytes();
		//打包	发送的地点及端口
		DatagramPacket packet=new DatagramPacket(data,data.length,new InetSocketAddress("localhost",8888));
		
		//发送
		client.send(packet);
		//释放
		client.close();
	}
}

```
2)服务器端

* 创建 服务器端     DatagramSocket类+指定端口
* 准备接收容器 字节数组 封装  DatagramPacket
* 包 接收数据
* 分析
* 释放资源

```
/**
 * 服务端
 * @author ASUS
 *
 */
public class MyServer {
	public static void main(String[] args) throws IOException {
		//创建服务器+端口
		DatagramSocket server=new DatagramSocket(8888);
		//准备接收容器
		byte[] container=new byte[1024];
		//封装成包
		DatagramPacket packet=new DatagramPacket(container,container.length);
		
		//接收数据
		server.receive(packet);
		
		//分析数据
		byte[] data=packet.getData();
		int len=packet.getLength();
		System.out.println(new String(data,0,len));
		//释放
	}
}

```
