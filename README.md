**要时常尝试有趣的东西，一切都是为了想使用百度平台的人脸检测的接口，因此来学习一下这个新的东西，例子百度一大把，主要是想把遇到的坑列一列，以便后来者更快速的查找错误。**

- 相信搜过javaCV的同学都会看到这篇文章[javacv开发详解之1：调用本机摄像头视频（建议使用javaCV最新版本）](https://blog.csdn.net/eguid_1/article/details/51659578)，我也一样，在学习这个知识之前，就搜集了一大堆资料，文档，文档看起来比较苦涩，所以，相信大多数人都希望能有个demo入门，因此，我的是基于上那个博主，在实践中，不断改进和完善。

#### 项目准备
1.我的是myeclipse，jdk1.8【要注意jdk的版本，低于1.7的会报错】
2.maven项目

#### 开始
1、引用依赖
```java
	<!-- javacv依赖包 -->
		<dependency>
		    <groupId>org.bytedeco</groupId>
		    <artifactId>javacv</artifactId>
		    <version>1.4.1</version>
		</dependency>
		<dependency>
			<groupId>org.bytedeco.javacpp-presets</groupId>
			<artifactId>opencv-platform</artifactId>
			<version>3.4.1-1.4.1</version>
		</dependency> 
```
创建java文件，复制粘贴
~~~java
/**
 * 
 */
package test;

import javax.swing.JFrame;

import org.bytedeco.javacv.CanvasFrame;
import org.bytedeco.javacv.FrameGrabber.Exception;
import org.bytedeco.javacv.OpenCVFrameGrabber;

/**
 * @author zwh_182
 * @email 
 * @createTime 2018-11-13上午9:33:51
 */
public class Test {

	/**
	 * 方法作用：调用windows平台的摄像头窗口视频 创建时间：2018-11-13 上午9:33:51
	 * 
	 * @throws Exception
	 * @throws InterruptedException
	 */
	public static void main(String[] args) throws Exception,
			InterruptedException {
		OpenCVFrameGrabber grabber = new OpenCVFrameGrabber(0);
		grabber.start(); // 开始获取摄像头数据
		CanvasFrame canvas = new CanvasFrame("摄像头");// 新建一个窗口
		canvas.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		canvas.setAlwaysOnTop(true);

		while (true) {
			if (!canvas.isDisplayable()) {// 窗口是否关闭
				grabber.stop();// 停止抓取
				System.exit(2);// 退出
			}
			canvas.showImage(grabber.grab());// 获取摄像头图像并放到窗口上显示， 这里的Frame
												// frame=grabber.grab();
												// frame是一帧视频图像

			Thread.sleep(50);// 50毫秒刷新一次图像
		}
	}

}

~~~


报这种错的，是因为依赖缺了
~~~error
Exception in thread "main" java.lang.UnsatisfiedLinkError: no jniopencv_core in java.library.path
	at java.lang.ClassLoader.loadLibrary(Unknown Source)
	at java.lang.Runtime.loadLibrary0(Unknown Source)
	at java.lang.System.loadLibrary(Unknown Source)
~~~

将以上两个依赖补上即可