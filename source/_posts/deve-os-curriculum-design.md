title: 操作系统课设
date: 2015-01-05 15:13:27
tags: [操作系统,文件管理系统,课程设计]
categories: 设计开发
toc: true
---


### 选题：文件管理系统
每个同学，必须完成两个题目，以下是我的两个选题。

1、银行家算法的模拟
掌握死锁相关的概念和解决方案，理解银行家算法的工作原理，设计合适的数据结构和算法，模拟实现银行家算法。

2、一个简单文件管理器的实现
掌握操作系统关于文件管理的各种原理，熟悉常用的文件操作，编写程序实现文件的常规操作。

对于选题1，找到了前辈的作品，改一改，验收！当然，被老师吐槽一顿是难免的。

对于选题2，文件管理系统，这个就有意思了，借鉴了大神[cattong](http://blog.csdn.net/cattong/article/details/1844916) 的思路，下面和大家讨论下。
<!--more-->
### 语言：Java
编程三年，个人感觉，最友好的语言就是Java了。本次课设，毫无争议选择了Java。

### 设计思路
#### 界面
![](http://voidking.qiniudn.com/@/imgs/界面.jpg)
很简洁的界面，上面输入命令，下面显示结果。废话不多说，上代码！
```java
package com.voidking.file;

import java.awt.BorderLayout;
import java.awt.FlowLayout;

import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;

public class MainFrame extends JFrame {

	public static void main(String[] args) {
		
		MainFrame thisClass = new MainFrame();
		thisClass.setVisible(true);
	}
	
	public MainFrame() {
		super();
		initialize();
	}
	
	private void initialize() {

		this.setTitle("资源管理");
		this.setSize(500, 400);
		this.setLocationRelativeTo(null);
		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		contentPane = new JPanel();
		contentPane.setLayout(new BorderLayout());
		contentPane.add(getCmdPane(), BorderLayout.NORTH);
		contentPane.add(getEchoArea(), BorderLayout.CENTER);
		this.add(contentPane);
		cmd = new Command(this);
		display("C:>");
	}

	private static final long serialVersionUID = 1L;

	private JPanel contentPane = null;

	private JPanel cmdPane = null;

	private JLabel jLabel = null;

	private JTextField cmdText = null;

	private JTextArea echoArea = null;

	private JScrollPane areaScroll = null;

	private Command cmd = null;

	private JPanel getCmdPane() {
		if (cmdPane == null) {
			jLabel = new JLabel();
			jLabel.setText("命 令 行:");
			cmdPane = new JPanel();
			cmdPane.setLayout(new FlowLayout());
			cmdPane.add(jLabel, null);
			cmdPane.add(getCmdText(), null);
		}
		return cmdPane;
	}

	private JTextField getCmdText() {
		if (cmdText == null) {
			cmdText = new JTextField();
			cmdText.setColumns(25);
			// 添加键盘监听事件;
			cmdText.addKeyListener(new java.awt.event.KeyAdapter() {
				public void keyTyped(java.awt.event.KeyEvent e) {

				}
				
				public void keyPressed(java.awt.event.KeyEvent e) {
					if (e.getKeyCode() == 10) {
						// 对文本进行处理;
						cmd.operate(cmdText.getText().trim());
					}

				}
			});
		}
		return cmdText;
	}

	private JScrollPane getEchoArea() {
		if (echoArea == null) {
			echoArea = new JTextArea();
			echoArea.setEditable(false);
			areaScroll = new JScrollPane();
			areaScroll.setViewportView(echoArea);
		}

		return areaScroll;
	}

	// 在回显框显示结果;
	public void display(String str) {
		echoArea.append(str + "\n");
		echoArea.setCaretPosition(echoArea.getText().length());
		cmdText.setText("");
	}

}

```
#### 命令处理
在界面初始化函数initialize()中，我们创建了一个Command类的实例：cmd对象。然后，在getCmdText()函数中监听文本框中的内容，调用cmd对象的函数进行处理。

Command类，封装了所有的命令处理函数。结构简单明了，不多解释，直接上代码！

```java
package com.voidking.file;

import java.awt.Desktop;
import java.io.File;


public class Command {
	private String currentPath;

	private MainFrame mainFrame = null;

	private final String cmd[] = { "cd", "dir", "mkdir", "rmdir", "mkfile", "rmfile",
			"exit" ,"mkpath","open"};

	private final int cmdInt[] = { 1, 2, 3, 4, 5, 6, 7 ,8,9};

	Command(MainFrame mainFrame) {
		currentPath = "C:";
		this.mainFrame = mainFrame;
	}


	public String[] separate(String operation) {
		String[] str = operation.split(" ");
		//主要解决文件夹或文件中含有空格的情况;
		if(str.length>2){
			String[] tempStr=new String[2];
			tempStr[0]=str[0];
			tempStr[1]=str[1];
			for(int i=2;i<str.length;i++)
				tempStr[1]+=" "+str[i];
			return tempStr;
		}
		return str;
	}

	/*
	 * 根据参数operation执行相应的操作;
	 */
	public void operate(String operation) {
		String[] str = separate(operation);
		String mycmd = "";

		int mycmdInt = 0;
		String path = "";
		if (str.length == 1) {
			mycmd = str[0];
			//切换盘符
			if (mycmd.indexOf(":") != -1) {
				File newFile = new File(mycmd);
				if (newFile.exists()) {
					currentPath = mycmd;
					mainFrame.display(getPath());
					return;
				}
			}
		}
		if (str.length >= 2) {
			mycmd = str[0];
			path = str[1];
		}

		for (int i = 0; i < cmd.length; i++) {
			if (mycmd.equalsIgnoreCase(cmd[i])) {
				mycmdInt = cmdInt[i];
			}
		}
		
		switch (mycmdInt) {
		case 1:
			cd(currentPath, path);
			break;
		case 2:
			dir(currentPath);
			break;
		case 3:
			mkdir(path);
			break;
		case 4:
			rmdir(path);
			break;
		case 5:
			mkfile(path);
			break;
		case 6:
			rmfile(path);
			break;
		case 7:
			exit();
			break;
		case 8:
			mkpath(path);
			break;
		case 9:
			open(path);
			break;
			
		default:
			mainFrame.display("无效的命令!");
		}
		mainFrame.display(getPath());
	}

	/*
	 * 获得当前所在目录;
	 */
	public String getPath() {
		return currentPath + ">";
	}

	/*
	 * 获得路径path下的文件;
	 */
	public String[] listAll(String path) {
		
		try {
			File f = new File(path);
			String[] fileName;
			if (f.isDirectory()) {
				fileName = f.list();
				mainFrame.display("共有" + fileName.length + "个文件");
				for (int i = 0; i < fileName.length; i++)
					mainFrame.display("    " + fileName[i]);
				return fileName;
			} else if (f.isFile()) {
				mainFrame.display("这是一个文件");
				return null;
			} else {
				//System.out.println(path);
				return null;
			}
		} catch (Exception e) {
			return null;
		}
	}
    public String[] listDirectory(String path){
    	File f = new File(path);
		String[] fileName;
		if (f.isDirectory()) {
			fileName = f.list();
			//for (int i = 0; i < fileName.length; i++)
				//System.out.println("/"+fileName[i]);
			return fileName;
		} else {
			//System.out.println(path+"是文件");
			return null;
		}
    }
	/*
	 * 判断这个路径是否正确;
	 */
	public boolean isRightPath(String path) {
		File file = new File(path);
		if (file.isDirectory() || file.isFile())
			return true;
		else
			return false;
	}

	/*
	 * 进行cd操作;cd<目录名> 功能:进入下一个目录;
	 */
	public void cd(String path, String file) {
		String temp = path + "\\" + file;
		if (!isRightPath(temp)) {
			mainFrame.display("没有找到这个文件夹");
		} else {
			if (!file.equals(""))
				currentPath += "\\" + file;
		}
	}

	/*
	 * 进行dir操作;dir [<目录名>] 功能: 显示目录下的所有文件;
	 */
	public void dir(String path) {
		if (path != null)
			listAll(path);
	}

	/*
	 * mkdir <目录名> 功能: 创建新目录
	 */
	public void mkdir(String directory) {
		if (!currentPath.equals("")) {
			String temp = currentPath + "\\" + directory;
			File newFile = new File(temp);
			if (!newFile.exists()) {
				try {
					if (newFile.isDirectory() == false) {
						newFile.mkdirs();
						mainFrame.display("文件夹创建成功!");
					} else {
						mainFrame.display("文件夹创建出错!");
					}
				} catch (Exception e) {
					mainFrame.display("出错信息:" + e.getMessage());
				}
			} else {
				mainFrame.display("文件夹已经存在");
			}
		}
	}

	/*
	 * rmdir <目录名> 功能: 删除目录;
	 */
	public void rmdir(String directory) {
		if (!currentPath.equals("")) {
			String temp = currentPath + "\\" + directory;
			File file = new File(temp);
			if (file.exists()) {
				if (file.delete()) {
					mainFrame.display("文件夹删除成功!");
				} else {
					mainFrame.display("文件夹删除操作出错!");
				}
			} else {
				mainFrame.display("文件夹不存在");
			}
		}
	}

	/*
	 * mkfile <文件名> 功能: 新建文件
	 */
	public void mkfile(String file) {
		if (!currentPath.equals("")) {
			String temp = currentPath + "\\" + file;
			File newFile = new File(temp);
			if (newFile.exists()) {
				mainFrame.display("文件已经存在!");
			} else {
				try {
					newFile.createNewFile();
					mainFrame.display("文件创建成功!");
				} catch (Exception e) {
					mainFrame.display("文件创建失败:" + e.getMessage());
				}
			}
		}
	}

	/*
	 * rmfile <文件名> 功能:删除文件;
	 */
	public void rmfile(String file) {
		if (!file.equals("")) {
			String temp = currentPath + "\\" + file;
			File dfile = new File(temp);
			if (dfile.exists()) {
				if (dfile.delete()) {
					mainFrame.display("文件删除成功!");
				} else {
					mainFrame.display("文件删除操作出错!");
				}
			} else {
				mainFrame.display("文件不存在");
			}
		}
	}

	/*
	 * 进行exit操作; 功能:退出文件系统;
	 */
	public void exit() {
		mainFrame.display("退出系统");
		System.exit(1);

	}
	
	/*
	 * 新建文件，使用绝对路径
	 */
	public void mkpath(String file){
		
		if (!currentPath.equals("")) {
			String temp = file;
			File newFile = new File(temp);
			if (newFile.exists()) {
				mainFrame.display("文件已经存在!");
			} else {
				try {
					newFile.createNewFile();
					mainFrame.display("文件创建成功!");
				} catch (Exception e) {
					mainFrame.display("文件创建失败:" + e.getMessage());
				}
			}
		}
	}
	
	/*
	 * 使用系统默认软件打开指定文件
	 */
	public void open(String file){
		
		if (!currentPath.equals("")) {
			String temp = currentPath + "\\" + file;
			File newFile = new File(temp);
			if (newFile.exists()) {
				try {
					Desktop.getDesktop().open(newFile);
				} catch (Exception e) {
					e.printStackTrace();
				}
				mainFrame.display("打开文件成功！");
			} else {
				mainFrame.display("文件不存在，无法打开");
			}
		}
	}
}

```

### 小结
怎么这么简单？没错，代码是很简单，但是，想到这个思路不容易，再次感谢cattong前辈。

### 源代码分享
http://yunpan.cn/cjqdEABf3dxkj  访问密码 7159
http://yunpan.cn/cjqdWLKJk4TQL  访问密码 63de

### 参考文档
http://www.oschina.net/project/tag/193?lang=19&show=hots
http://blog.csdn.net/cattong/article/details/1844916

