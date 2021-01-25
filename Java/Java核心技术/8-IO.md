## I/O



### 一、IO流概念

常见的数据源有：数据库、文件、其他程序、内存、网络连接、IO设备。



流是一个抽象、动态的概念，是一连串连续动态的数据集合。

![图10-2 流与源数据源和目标数据源之间的关系.png](D:\Notes\Java\Java核心技术\image\1495701094668697.png)



#### 1.1 典型的IO流

```java
package com.test.java;
import java.io.FileInputStream;
/**
 * 典型的IO流代码写法
 * @author 林
 *
 */
public class TestIO {
	public static void main(String[] args) {
		FileInputStream f1=null;
		try {
			f1=new FileInputStream("d:/a.txt");	//文件内容“ABC”
			StringBuilder s1=new StringBuilder();
			int temp=0;
			while((temp=f1.read())!=-1) {
				s1.append((char)temp);
			}
			System.out.println(s1);
		}catch(Exception e) {
			e.printStackTrace();
		}finally {
			try {
				if(f1!=null) {
					f1.close();
				}
			}catch(Exception e) {
				e.printStackTrace();
			}
		}
	}
}
```



#### 1.2 流的分类

**按流的方向分类：**

1. 输入流：数据流向是数据源到程序(以InputStream、Reader结尾的流)。

   2. 输出流：数据流向是程序到目的地(以OutPutStream、Writer结尾的流)。

![图10-5 输入输出流示意图.png](D:\Notes\Java\Java核心技术\image\1495702185421118.png)



**按处理的数据单元分类：**

1. 字节流：以字节为单位获取数据，命名上以Stream结尾的流一般是字节流，如FileInputStream、FileOutputStream。
2. 字符流：以字符为单位获取数据，命名上以Reader/Writer结尾的流一般是字符流，如FileReader、FileWriter。



**按处理对象不同分类：**

1. 节点流：可以直接从数据源或目的地读写数据，如FileInputStream、FileReader、DataInputStream等。
2. 处理流：不直接连接到数据源或目的地，是”处理流的流”。通过对其他流的处理提高程序的性能，如BufferedInputStream、BufferedReader等。处理流也叫包装流。

![图10-6 节点流处理流示意图.png](D:\Notes\Java\Java核心技术\image\1495702229684292.png)



#### 1.3 常用IO流

| 类                                         |                                                              |
| ------------------------------------------ | ------------------------------------------------------------ |
| InputStream/OutputStream                   | 字节流的抽象类。                                             |
| Reader/Writer                              | 字符流的抽象类。                                             |
| FileInputStream/FileOutputStream           | 节点流：以字节为单位直接操作“文件”。                         |
| ByteArrayInputStream/ByteArrayOutputStream | 节点流：以字节为单位直接操作“字节数组对象"。                 |
| ObjectInputStream/ObjectOutputStream       | 处理流：以字节为单位直接操作“对象”。                         |
| DataInputStream/DataOutputStream           | 处理流：以字节为单位直接操作“基本数据类型与字符串类型”。     |
| FileReader/FileWriter                      | 节点流：以字符为单位直接操作“文本文件"。                     |
| BufferedReader/BufferedWriter              | 处理流：将Reader/Writer对象进行包装，增加缓存功能，提高读写效率。 |
| BufferedInputStream/BufferedOutputStream   | 处理流：将InputStream/OutputStream对象进行包装，增加缓存功能，提高 读写效率。 |
| InputStreamReader/OutputStreamWriter       | 处理流：将字节流对象转化成字符流对象。                       |
| PrintStream                                | 处理流：将OutputStream进行包装，可以方便地输出字符，更加灵活。 |



#### 1.4 IO抽象类

 InputStream/OutputStream和Reader/writer类是所有IO流类的抽象父类。

- InputStream：此抽象类是表示字节输入流的所有类的父类。InputSteam是一个抽象类，它不可以实例化。 数据的读取需要由它的子类来实现。常用方法：int read() 和 void close()。数据的单位为字节(8 bit) 。
- OutputStream：此抽象类是表示字节输出流的所有类的父类。常用方法：void write(int n) 和 void close() 。
- Reader：用于读取的字符流抽象类，数据单位为字符。常用方法：int read() 和 void close() 。
- writer：用于写入的字符流抽象类，数据单位为字符。常用方法：void write(int n) 和 void close() 。





### 二、常用流

#### 2.1 文件字节流

| 类               | 描述                                               |
| ---------------- | -------------------------------------------------- |
| FileInputStream  | 通过字节的方式读取文件，适合读取所有类型的文件     |
| FileOutputStream | 通过字节的方式写数据到文件中，适合所有类型的文件。 |

**要点**：

1. 使用缓存数组，read(byte[] b)与write(byte[ ] b, int off, int length)
2. 程序中每个流都要单独关闭。



```java
package com.test.java;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
/**
 * 测试拷贝文件方法
 * @author 林
 *
 */
public class Test {
	public static void main(String[] args) {
		copyFile("d:/a.txt","d:/b.txt");
	}
	
	static void copyFile(String src,String dec) {
		FileInputStream f1=null;
		FileOutputStream f2=null;	
		byte[] b1=new byte[1024];	//设置缓存数组大小
		int temp=0;
		try {
			f1=new FileInputStream(src);
			f2=new FileOutputStream(dec);
			while((temp=f1.read(b1))!=-1) {		//temp等于-1时表示读取结束
				f2.write(b1,0,temp);
			}
		}catch(Exception e) {
			e.printStackTrace();
		}finally {
			try {
				if(f2!=null) {
					f2.close();
				}
			} catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(f1!=null) {
					f1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
		}
				
	}
}
```



#### 2.2 文件字符流

| 类         |                        |
| ---------- | ---------------------- |
| FileReader | 通过字符的方式读取文件 |
| FileWriter | 通过字符的方式写入文件 |

**要点**：

1. 字节流不能很好的处理Unicode字符，经常会出现“乱码”现象。
2. 处理文本文件，一般可以使用文件字符流，它以字符为单位进行操作。



```java
package com.test.java;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
/**
 * 测试使用FileReader与FileWriter实现文本文件的拷贝
 * @author 林
 *
 */
public class Test {
	public static void main(String[] args) {
		FileReader f1=null;
		FileWriter w1=null;
		int len=0;
		try {
			f1=new FileReader("d:/a.txt");
			w1=new FileWriter("d:/c.txt");
			char[] b1=new char[1024];	//字符数组缓存
			while((len=f1.read(b1))!=-1) {
				w1.write(b1,0,len);
			}
		}catch(FileNotFoundException e) {
			e.printStackTrace();
		}catch(IOException e) {
			e.printStackTrace();
		}finally {
			try {
				if(w1!=null) {
					w1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(f1!=null) {
					f1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
		}
	}
}
```



#### 2.3 缓存字节流

缓存字节流：适合读取所有类型的文件(图像、视频、文本文件等)。（效率高）

| 类                   | 方法    |          |
| -------------------- | ------- | -------- |
| BufferedInputStream  | read()  | 读取字节 |
| BufferedOutputStream | write() | 写入字节 |



```java
package com.test.java;
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
/**
 * 测试BufferedInputStream，BufferedOutputStream
 * @author 林
 *
 */
public class Test {
	public static void main(String[] args) {
		long time1=System.currentTimeMillis();
		copyFile("D:/Technology/PDF/mysql.pdf","d:/mysql1.pdf");
		long time2=System.currentTimeMillis();
		System.out.println("copyFile耗时:"+(time2-time1));
	}
	
	
	static void copyFile(String src,String dec) {	//缓冲字节流文件拷贝方法
		FileInputStream f1=null;
		BufferedInputStream b1=null;
		FileOutputStream f2=null;
		BufferedOutputStream b2=null;
		int temp=0;
		try {
			f1=new FileInputStream(src);
			f2=new FileOutputStream(dec);
			b1=new BufferedInputStream(f1);	//缓存区的大小默认是8192
			b2=new BufferedOutputStream(f2);
			while((temp=b1.read())!=-1) {
				b2.write(temp);
			}
		}catch(Exception e) {
			e.printStackTrace();
		}finally {	//使用缓冲字节流，后开的先关闭
			try {
				if(b2!=null) {
					b2.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(b1!=null) {
					b1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(f2!=null) {
					f2.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(f1!=null) {
					f1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
		}
	}

}
```





#### 2.4 缓冲字符流

缓存字符流：适合读取文本文件。（效率高）

| 类             | 方法       |                |
| -------------- | ---------- | -------------- |
| BufferedReader | readLine() | 读取一行字符串 |
| BufferedWriter | write()    | 写入字符串     |
| BufferedWriter | newLine()  | 换行           |



```java
package com.test.java;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

/**
 * 测试BufferedReader，BufferedWriter
 * @author 林
 *
 */
public class Test {
	public static void main(String[] args) {
		FileReader f1=null;
		FileWriter f2=null;
		BufferedReader b1=null;
		BufferedWriter b2=null;
		String temp="";
		try {
			f1=new FileReader("D:/a.txt");
			f2=new FileWriter("D:/b.txt");
			b1=new BufferedReader(f1);
			b2=new BufferedWriter(f2);
			while((temp=b1.readLine())!=null) {
				b2.write(temp);  //读取的一行字符串写入文件中
				b2.newLine();  //换行
			}
		}catch(FileNotFoundException e) {
			e.printStackTrace();
		}catch(IOException e) {
			e.printStackTrace();
		}finally {
			try {
				if(b2!=null) {
					b2.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(b1!=null) {
					b1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(f2!=null) {
					f2.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(f1!=null) {
					f1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
		}
	}
}
```



### 三、字节数组流

ByteArrayInputStream和ByteArrayOutputStream经常用在需要流和数组之间转化的情况。

ByteArrayInputStream是把内存中的”某个字节数组对象”当做数据源。

```java
package com.test.java;
import java.io.ByteArrayInputStream;
import java.io.IOException;
/**
 * 测试字节数组流
 * @author 林
 *
 */
public class Test {
	public static void main(String[] args) {
		byte[] b="ABCDE".getBytes();
		test(b);
	}
	
	public static void test(byte[] b) {
		ByteArrayInputStream b1=null;
		StringBuilder s1=new StringBuilder();
		int temp=0;
		int num=0;
		try {
			b1=new ByteArrayInputStream(b);
			while((temp=b1.read())!=-1) {
				s1.append((char)temp);
				num++;
			}
			System.out.println(s1);
			System.out.println("读取字节数:"+num);
		}catch(Exception e) {
			e.printStackTrace();
		}finally {
			try {
				if(b1!=null) {
					b1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
		}
	}
}
```



### 四、数据流

数据流将“基本数据类型与字符串类型”作为数据源。

DataInputStream和DataOutputStream提供了可以存取与机器无关的所有Java基础类型数据(如：int、double、String等)的方法。

DataInputStream和DataOutputStream是处理流。

```java
package com.test.java;

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * 测试基本数据类型与字符串类型数据量
 * @author 林
 *
 */
public class Test {
	public static void main(String[] args) {
		FileInputStream f1=null;
		FileOutputStream f2=null;
		DataInputStream d1=null;
		DataOutputStream d2=null;
		try {
			f2=new FileOutputStream("D:/data.txt");
			f1=new FileInputStream("D:/data.txt");
			d2=new DataOutputStream(new BufferedOutputStream(f2));
			d1=new DataInputStream(new BufferedInputStream(f1));
			d2.writeChar('A');	//写数据到文件中
			d2.writeInt(10);
			d2.writeDouble(Math.random());
			d2.writeBoolean(true);
			d2.writeUTF("Hello Java");
			d2.flush();		//手动刷新缓冲区：将流中数据写入到文件中
			System.out.println(d1.readChar());
			System.out.println(d1.readInt());
			System.out.println(d1.readDouble());
			System.out.println(d1.readBoolean());
			System.out.println(d1.readUTF());
		}catch(IOException e) {
			e.printStackTrace();
		}finally {
			try {
				if(d2!=null) {
					d2.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(d1!=null) {
					d1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(f2!=null) {
					f2.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(f1!=null) {
					f1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
		}
	}
}
```



### 五、对象流

ObjectInputStream/ObjectOutputStream是以“对象”为数据源，但是必须将传输的对象进行序列化与反序列化操作。

```java
package com.test.java;

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.OutputStream;
import java.util.Date;

/**
 * 测试
 * @author 林
 *
 */
public class Test {
	public static void main(String[] args) {
		write();
		read();
	}
	
	public static void write() {	//自定义对象流，写入文件
		OutputStream o1=null;
		BufferedOutputStream b1=null;
		ObjectOutputStream o2=null;
		try {
			o1=new FileOutputStream(new File("d:/obj.txt"));
			b1=new BufferedOutputStream(o1);
			o2=new ObjectOutputStream(b1);
			o2.writeInt(12);
			o2.writeDouble(3.14);
			o2.writeChar('A');
			o2.writeBoolean(true);
			o2.writeUTF("Hello Java");
			o2.writeObject(new Date());
		}catch(IOException e) {
			e.printStackTrace();
		}finally {
			try {
				if(o2!=null) {
					o2.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(b1!=null) {
					b1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(o1!=null) {
					o1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
		}
	}
	
	public static void read() {	//自定义对象流，写入文件
		InputStream i1=null;
		BufferedInputStream b1=null;
		ObjectInputStream o1=null;
		try {
			i1=new FileInputStream(new File("d:/obj.txt"));
			b1=new BufferedInputStream(i1);
			o1=new ObjectInputStream(b1);
			System.out.println(o1.readInt());
			System.out.println(o1.readDouble());
			System.out.println(o1.readChar());
			System.out.println(o1.readBoolean());
			System.out.println(o1.readUTF());
			//System.out.println(o1.readObject().toString());
		}catch(IOException e) {
			e.printStackTrace();
		}finally {
			try {
				if(o1!=null) {
					o1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(b1!=null) {
					b1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(i1!=null) {
					i1.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
		}
	}
}
```



### 六、转换流

InputStreamReader/OutputStreamWriter用来实现将字节流转化成字符流。

```java
package com.test.java;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

/**
 * 测试
 * @author 林
 *
 */
public class Test {
	public static void main(String[] args) {
		BufferedReader br=null;
		BufferedWriter bw=null;
		try {
			br=new BufferedReader(new InputStreamReader(System.in));
			bw=new BufferedWriter(new OutputStreamWriter(System.out));
			String str=br.readLine();
			while(!"exit".equals(str)) {
				bw.write(str);
				bw.newLine();
				bw.flush();
				str=br.readLine();
			}
		}catch(IOException e) {
			e.printStackTrace();
		}finally {
			try {
				if(br!=null) {
					br.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
			try {
				if(bw!=null) {
					bw.close();
				}
			}catch(IOException e) {
				e.printStackTrace();
			}
		}
	}
}
```



### 七、序列化与反序列化

把Java对象转换为字节序列的过程称为对象的**序列化**。

把字节序列恢复为Java对象的过程称为对象的**反序列化**。

|                                            |                                                              |
| ------------------------------------------ | ------------------------------------------------------------ |
| ObjectOutputStream.writeObject(Object obj) | 对参数指定的obj对象进行序列化，把得到的字节序列写到一个目标输出流中。 |
| ObjectInputStream.readObject()             | 从一个源输入流中读取字节序列，再把它们反序列化为一个对象，并将其返回。 |

只有实现了Serializable接口的类的对象才能被序列化。 Serializable接口是一个空接口，只起到标记作用。

