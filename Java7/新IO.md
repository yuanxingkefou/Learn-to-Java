#新I/O————NIO.2

**##文件I/O的基石：Path**

Path通常代表文件系统中的位置，Path类中的方法可以用来获取路径信息，访问该路径中的各元素，将路径转换为其他形式等

创建一个Path：`Path listing=Paths.get("/usr/bin/zip"); `

移除冗余项：需要处理的Path中可能会有一个或两个点等，有toRealPath(),toAbsolutePath()和normalize()三个方法

转换Path：

* 合并Path：
```
Path a=Paths.get("");
Path b=Paths.get("");
Path completePath=a.resolve(b);
如果b不是绝对路径，则将a和b的路径连起来；否则返回b
```

调用p.relativize(r)将产生路径q，对q进行解析会产生r。如：p为"/home/cay"，r为"/home/fred/myprog",则r为"../fred/myprog"

**##处理目录和目录树**

新加入的java.nio.file.DirectoryStream<T>接口和它的实现类提供了很多功能：

* 在目录中查找文件：
```
查找dir目录中所有的.properties文件
Path dir=Paths.get("...");
try{DirectoryStream<Path> stream=Files.newDirectoryStream(dir,"*.properties"))
    ...
    }
```

* 遍历目录树：

`Files.walkFileTree(Path startingDir,FileVisitor<? super Path> visitor);`

给出FileVisitor接口的实现类需要至少实现5个方法，但提供了一个默认实现类SimpleFileVisitor，可以实现大多数用法

**##NIO.2的文件系统I/O**

* 创建和删除文件
```
//创建文件
Path target=Paths.get("");
Path file=Files.createFile(target);
//删除文件
Files.delete(target);
```

* 文件的复制和移动
```
//文件复制
Files.copy(source,target,参数);
参数有REPLACE_EXISTING（覆盖即替换已有文件）,COPY_ATTRIBUTES（复制文件属性）,ATOMIC_MOVE（确保在两边的操作都成功，否则回滚）
//文件移动
Files.move(Path source,Path target)
```

* 文件的属性

*文件修改通知

在Java7中可以用java.nio.file.WatchService类监测文件或目录的变化

*SeekableByteChannerl接口

**##异步I/O操作**

AsynchronousFileChannel————用于文件I/O

AsynchronousSocketChannel————用于套接字I/O，支持超时

AsynchronousServerSocketChannel————用于套接字接受异步连接

```
将来式：
try
{
    Path file=Paths.get("");
    
    AsynchronousFileChannel channel=AsynchronousFileChannel.open(file);
    
    //读取100 000字节
    ByteBuffer buffer=ByteBuffer.allocate(100_000);
    Future<Integer> result=channel.read(buffer,0);
    
    while(!result.isDone())
    {
        do something other 
    }
    
    Integer bytesRead=result.get();
    System.out.println("Bytes read ["+bytesRead+"]");
}
```

```
回调式：
try
{
    Path file=Paths.get("");
    
    AsynchronousFileChannel channel=AsynchronousFileChannel.open(file);
    
    //读取100 000字节
    ByteBuffer buffer=ByteBuffer.allocate(100_000);
    
    channel.read(buffer,0,buffer,
          new CompletionHandler<Integer,ByteBuffer>()
          {
              public void completed(Integer result,ByteBuffer attachment)
              {
                  System.out.println("Bytes read ["+result+"]");
              }
              public void failed(Throwable exception ,ByteBuffer attachment)
              {
                  System.out.println(exception.getMessage());
              }
           });
 }
 ```
 
