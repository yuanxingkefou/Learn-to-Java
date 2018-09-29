#Coin项目

Coin是Java7中一个开源的子项目，反映了Java语言中的微小变动

**Java7的新特性**：

**##switch语句中的String**

在Java6及以前，case语句中的常量只能是byte,char,short和int或枚举常量。Java7增加了String，如：

`case "Sunday" : System.out.println("Sunday");`

**##更强的数值文本表示法**

* 二进制文本

之前用位模式表示十进制102，这样写
` int x=Integer.parseInt("1100110",2);

现在`int x=0b1100110;`

* 数字中的下划线

为了在处理长串数字时不搞混，可以在代码中将100000000写成100_000_000

**##改善后的异常处理**

* multicatch

之前：
```
try{
}catch(FileNotFoundException){}
catch(ParseException){}
catch(IOExcetion){}
```
现在：
```
try{
}catch(FileNotFoundException|ParseException){}
catch(IOException){}
```

* final重抛
```
try {
  doSomethingWhichMightThrowIOException();
  doSomethingWhichMightThrowSQLException();
  }catch(final Exception e)
  {
      throw e;
  }
```
关键字final表明实际抛出的异常就是运行时遇到的异常，而不是一个笼统的Exception类型

**##try-with-resources(TWR)**

```
try(OutputStream out=new FileOutputStream(file); InputStream is=url.openStream() )
{
    byte[] buf=new byte[4096];
    int len;
    while((len=is.read(buf))>0)
    {
        out.write(buf,0,len);
    }
 }
 ```
 把资源放在try的圆括号内，在这段代码块中使用的资源在处理完成后会自动关闭
 
 **##钻石语法**
 
 简化泛型定义或实例化太过繁琐的问题
 
 ```
 Map<Integer,String> users=new HashMap<Integer,String>();
 简化为：
 Map<Integer,String> users=new HashMap<>();
 ```
 
 **##简化变参方法调用**
 
 

