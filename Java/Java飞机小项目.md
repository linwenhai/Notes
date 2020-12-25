## Java飞机小项目

### 一、创建飞机游戏的主窗口

新建MyGameFrame.java

```java
package com.test.game;

import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import javax.swing.JFrame;

public class MyGameFrame extends JFrame {
    /*初始化窗口*/
	public void launchFrame() {
		setTitle("飞机游戏");	//标题
		setVisible(true);		//窗口默认不可见，设为可见
		setSize(500,500);		//窗口大小，宽500，高500
		setLocation(300,300);	//窗口左上角顶点的坐标位置
		
		/*增加关闭窗口监听，用户可点击右上角关闭图标*/
		addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}	
		});
	}
	
	public static void main(String[] args) {
		MyGameFrame f=new MyGameFrame();
		f.launchFrame();
	}
}
```



### 二、创建加载图片的工具类

创建GameUtil.java

```java
package com.test.game;

import java.awt.Image;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.net.URL;
import javax.imageio.ImageIO;

public class GameUtil {		//加载图片工具类
	private GameUtil() {}
	
	public static Image getImage(String path) {
		BufferedImage bi =null;
		try {
			URL u=GameUtil.class.getClassLoader().getResource(path);
			bi=ImageIO.read(u);
		} catch(IOException e) {
			e.printStackTrace();
		}
		return bi;
	}
}
```











