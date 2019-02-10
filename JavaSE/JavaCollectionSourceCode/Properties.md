#Properties

作用：读写资源配置文件

要求：键与值只能为字符串

方法:

setProperty(String key,String value);
//如果不存在，返回null
getProperty(String key);
//如果不存在，返回defaultValue
getProperty(String key,String defaultValue);

存储为文件：

* 后缀为.properties
  store(Writer writer, String comments)
  store(OutputStream out, String comments)

* 后缀为.xml
  storeToXML(OutputStream os, String comment)
  storeToXML(OutputStream os, String comment, String encoding)

```
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.Properties;

/**
 * 1.Properties资源配置文件的读写
 * 2.使用Properties输出到文件(资源配置文件)
 * 3.使用类的相对路径获取配置文件
 * @author ASUS
 *
 */
public class PropertiesDemo {
	public static void main(String[] args) throws FileNotFoundException, IOException {
		Properties pro=new Properties();
		
		//存储
		pro.setProperty("driver", "oracle.jdbc.driver.OracleDriver");
		pro.setProperty("url", "jdbc:oracle:thin:@localhost:1521:orcl");
		pro.setProperty("user", "maqi");
		pro.setProperty("pwd", "067931");
		
		//获取		
		String url=pro.getProperty("url","test");
		System.out.println(url);
		
		//存储到文件
		pro.store(new FileOutputStream(new File("src/db.properties")),"db配置");
		pro.storeToXML(new FileOutputStream(new File("src/db.properties")),"db配置");
		/*
		 * <?xml version="1.0" encoding="UTF-8" standalone="no"?>
			<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
			<properties>
			<comment>dbéç½®</comment>
			<entry key="user">maqi</entry>
			<entry key="url">jdbc:oracle:thin:@localhost:1521:orcl</entry>
			<entry key="driver">oracle.jdbc.driver.OracleDriver</entry>
			<entry key="pwd">067931</entry>
			</properties>
		 */
		
		//获取类的配置文件
		//""表示bin
		Properties pro1=new Properties();
		pro1.load(Thread.currentThread().getContextClassLoader().getResourceAsStream(""));
		System.out.println(pro1.getProperty("user"));
	}

}

```
