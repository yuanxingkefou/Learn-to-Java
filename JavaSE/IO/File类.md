#File类

**两个常量及File的创建**
```
package IOTest;

import java.io.File;
/**
 * 两个常量：
 * 1.路径分割符	;
 * 2.文件分割符	Windows下： \	linux下： /
 * 
 * 相对路径和绝对路径构件File
 * 1.相对路径
 *  File(String parent, String child)
 *  --> File("E:/java/test","test1.txt");
 *  File(File parent, String child)
 *  --> File(new File("E:/java/test"),"test2.txt");
 * 2.绝对路径
 *  File(String pathname)
 *  --> File("E:/java/test/test3.txt");
 * @author ASUS
 *
 */
public class FileDemo {
	public static void main(String[] args) {
		//路径分割符	;
		System.out.println(File.pathSeparator);
		//文件分割符	Windows下： \	linux下： /
		System.out.println(File.separator);
		
		//创建File对象
		File test1=new File("E:/java/test","test1.txt");
		System.out.println(test1.getPath());
		System.out.println(test1.getName());
		
		File test2=new File(new File("E:/java/test"),"test2.txt");
		System.out.println(test2.getPath());
		System.out.println(test2.getName());
		
		File test3=new File("E:/java/test/test3.txt");
		System.out.println(test3.getPath());
		System.out.println(test3.getName());
		
		//没有盘符	以user.dir（当前项目的路径）构建
		File test4=new File("test4.txt");
		System.out.println(test4.getAbsolutePath());
		System.out.println(test4.getName());		
	}
}
输出为：
;
\
E:\java\test\test1.txt
test1.txt
E:\java\test\test2.txt
test2.txt
E:\java\test\test3.txt
test3.txt
D:\ecplise\java\Practice\test4.txt
test4.txt

```

**File类的常用方法**

看API



























