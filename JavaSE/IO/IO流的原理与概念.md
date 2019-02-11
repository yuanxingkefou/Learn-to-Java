#IO流的原理与概念

**概念**

程序与 文件|数组|网络连接|数据库 之间的流动，以程序为中心

**分类**

* 按流向分类：输入流和输出流

* 按数据分类：
    字节流： 二进制文件，可以处理一切文件，包括纯文本，doc，音频，视频等
    字符流： 文本文件，只能处理纯文本文件

* 功能：
  节点流：包裹源头的
  处理流： 增强功能，提供性能
  
**字符流与字节流**

**字节流**

输入流：InputStream

读取文件的操作：

* 1.建立联系  File对象
* 2.选择流    文件输入流 InputStream FileInputStream
* 3.操作：    byte[] arr=new byte[1024];+read()+读取大小
* 4.释放资源：关闭close()

```
package IOTest;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;

/**
 * 1.建立联系  File对象
 * 2.选择流    文件输入流 InputStream FileInputStream
 * 3.操作：    byte[] arr=new bute[1024];+read()+读取大小
 * 4.释放资源：关闭close()
 * @author ASUS
 *
 */
public class FileInputStreamDemo {
	public static void main(String[] args) {
		//1.建立联系  File对象
		File file=new File("E:/java/test/test1.txt");
		
		//选择流
		InputStream is=null;
		
		try {
			is=new FileInputStream(file);
			//3.操作不断读取	缓存数组
			byte[] buffer=new byte[10];
			int len=0;		//接收	实际读取大小
			//循环
			while(-1!=(len=is.read(buffer)))
			{
				//输出 字节数组转换为字符串
				String info=new String(buffer,0,len);
				System.out.println(info);
			}
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
			System.out.println("文件不存在");
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
			System.out.println("读取文件失败");
		}finally
		{
			try {
				is.close();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
				System.out.println("文件无法关闭");
			}
		}
		
	}

}
```

输出流：OutputStream

写入文件的操作：

* 1.建立联系  File对象
* 2.选择流    文件输出流 OutputStream FileOutputStream
* 3.操作：    write()+flush()
* 4.释放资源：关闭close()

```
package IOTest;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;

/**
 * 1.建立联系  File对象
 * 2.选择流    文件输入流 InputStream FileInputStream
 * 3.操作：    byte[] arr=new bute[1024];+read()+读取大小
 * 4.释放资源：关闭close()
 * @author ASUS
 *
 */
public class FileInputStreamDemo {
	public static void main(String[] args) {
		//1.建立联系  File对象
		File file=new File("E:/java/test/test1.txt");
		
		//选择流
		InputStream is=null;
		
		try {
			is=new FileInputStream(file);
			//3.操作不断读取	缓存数组
			byte[] buffer=new byte[10];
			int len=0;		//接收	实际读取大小
			//循环
			while(-1!=(len=is.read(buffer)))
			{
				//输出 字节数组转换为字符串
				String info=new String(buffer,0,len);
				System.out.println(info);
			}
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
			System.out.println("文件不存在");
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
			System.out.println("读取文件失败");
		}finally
		{
			try {
				is.close();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
				System.out.println("文件无法关闭");
			}
		}	
	}
}
```

**字符流**

输入流：Reader  FileReader

* 1.建立联系  File对象
* 2.选择流    文件输入流 Reader FileReader
* 3.操作：    char[] arr=new char[1024];+read()+读取大小
* 4.释放资源：关闭close()
```
package IOTest;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.io.Reader;

public class FileReaderDemo {
	public static void main(String[] args) {
		//1.建立联系
		File src=new File("E:/java/test/test1.txt");
		//2.选择流
		Reader reader=null;
		
		try {
			reader=new FileReader(src);
			//读取操作
			char[] buffer=new char[10];
			int len=0;
			while(-1!=(len=reader.read(buffer)))
			{
				String str=new String(buffer,0,len);
				System.out.println(str);
			}
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}finally
		{
			if(null!=reader)
				try {
					reader.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
		}
	}

}
```

输出流：Writer FileWriter

* 1.建立联系  File对象
* 2.选择流    文件输入流 Writer FileWriter
* 3.操作：    char[] arr=new char[1024];+write()+读取大小
* 4.释放资源：关闭close()
