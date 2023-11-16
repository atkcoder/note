[TOC]

# 继承与多态

---

# 正则表达式

---

# IO流

- **什么是IO流？**

IO： Input, Output

Java虚拟机与硬盘通过01字节码的形式进行传输

**输出流**：Java程序向硬盘写入内容

**输入流**：Java程序从硬盘读取内容

## 文件

### 创建文件

在讲IO流之前我们先要了解一下文件的创建

文件的创建有**三种方法：**

```java
new File(String pathname);
new File(File Parent, String child);
new File(String parent, String child);
```

当然在这之前还要导入两个包：

```java
import java.io.File;
import java.io.IOException;
```

以第二个为例：

```java
    public void createFile2(){
        File parentpath = new File("D:\\temp");
        String child = "new2.txt";
        File file = new File(parentpath,child);
        try{
            file.createNewFile();
            System.out.println("创建成功2");
        }
        catch (IOException e){
            e.getStackTrace();
        }
    }
```

当然你可能要问了，既然有

```java
File file = new File(parentpath,child);
```

为什么还要

```java
file.createNewFile();
```

那是因为：**第一步只是在  Java  虚拟机的内存中创建了，我们还要将其写入到硬盘中去**

### 文件信息

File类有很多的方法，这里我们挑几个重要的讲：

```java
file.isFile()
file.exists()
file.getAbsolutePath()
file.getName()
file.getParent()
file.length()				//单位是字节
file.getPath()
```

### 操作文件

对文件（文件夹也被视为文件）的操作一般有：**创建，删除，写入，读取**

创建文件前面已经讲了，我们主要来讲后面几个：

- **文件夹的创建** 创建单个文件夹用 **mkdir()**方法   多个用 **mkdirs()**方法

```java
	File file = new File("D:\\temp\\new\\a\\d\\c");
        if(file.mkdirs()){
            System.out.println("目录创建成功");
        }
        else{
            System.out.println("目录创建失败");
        }
```

当然为了保险起见你也可以在创建目录前先判断一下它是否存在

- **文件的删除** 这里我们会用到**delete()**方法

```java
        File file = new File("D:\\temp\\new1.txt");
        if (file.exists()){							//先判断文件是否存在
            if (file.delete()){
                System.out.println("删除成功");
            }
            else{
                System.out.println("删除失败");
            }
        }
        else {
            System.out.println("文件不存在");
        }
```

## 操作IO流

-  按操作单位不同可分为：**字节流**（8 bit）**字节文件** 和 **字符流**（按字符）**文本文件** 
-  按数据流向可分为：**输入流 和 输出流** 
-  按流的角色的不同可分为：**节点流 和 处理流/包装流** 
| 抽象类 | 字节流 | 字符流 |
| --- | --- | --- |
| 输入流 | InputStream | Reader |
| 输出流 | OutputStream | Writer |


这里每一个类都是抽象类，并不能直接使用，我们需要使用他们的子类

<img src="https://raw.githubusercontent.com/atkcoder/typoraimg/main/img/202311161654377.png" style="zoom:67%;" />

### 节点流

#### InputStream

**InputStream**有三个常用的子类

- **FileInputStream**:    FileInputStream(String filepath / File filename) 
   - **read**  方法： 
      - int read()：读到末尾返回-1，正常情况返回编码值
      - int read(byte[] b):  读到末尾返回-1，正常情况返回读取的字节长度，且每次读取`b.length`长度的字节
   - **close**  方法： 
      - 读取完成后一定要关闭流
   - **注意**：read和close都需要使用抛出异常
- **BufferedInputStream**:节点流
- **ObjectInputStream**  对象流

#### OutputStream

常见子类：

- **FileOutputStream**: FileOutputStream(String filepath / File filename, boolean append) 
   - **wirte**  方法 
      - void write(int ch): 写入字符
      - void write(byte[] b):每次写入b.length长度的byte数组
      - void write(byte[] b, int off, int len)
   - **close**  方法 
      - 写完一定要关闭文件

#### 文件复制

通过IO流完成文件复制：

1. 通过 **输入流**   读取文件到 **Java虚拟机**
2. 通过 **输出流**   **Java虚拟机** 写入文件到文件夹
3. 方式为边读边写

```java
        String sourFile = "D:\\temp\\boy.jpeg";
        String destFile = "D:\\temp\\pic\\sea.jpeg";
        byte buf[] = new byte[1024];
        int dataLength = 0;

        FileOutputStream fileOutputStream = null;
        FileInputStream fileInputStream = null;

        try{
            fileInputStream = new FileInputStream(sourFile);
            fileOutputStream = new FileOutputStream(destFile);
            while (( dataLength = fileInputStream.read(buf)) != -1){
                fileOutputStream.write(buf,0,dataLength);
            }
        }
        catch (IOException e){
            e.printStackTrace();
        }
        finally {
            try{
                fileInputStream.close();
                fileOutputStream.close();
            }
            catch (IOException e){
                e.printStackTrace();
            }
        }
```

#### FileReader

Reader的间接子类，常用方法：

- **FileReader:**  FileReader(String filePath  /  File fileName) 
   - **read:** 
      - int read():  读一个字符
      - int read(char buf[])
   - **close**

#### FileWriter

- **FileWriter:**  同上 
   - **write** 
      - public void write(int c)
      - public void write(char buf[])
      - public void write(char buf[], int off,int len)
   - **close**
   - **flush**

### 处理流

#### BufferedReader

**Reader**  的子类 同时可以往构造方法里面传**Reader**的子类，其为私有成员变量

- **readLine**  方法，读取一行字符，但不包括回车

其他方法具体看传进去的子类

#### BufferedWriter

- **newLine**  方法，写入一个回车

#### BufferedInputStream

同上

#### BufferedOutputStream

同上

#### 对象处理流

对象处理流，可以将对象保存起来，并 **不是单纯保存值** 而是 **保存数据类型和值**

例如：

​	我定义了一个 Dog 类，并声明了一个Dog的对象，但是我想要将**整个对象**保存起来，并不只是保存里面的值，而且还要保存方法，这时候只能通过对象处理流来操作

保存对象的过程也叫 **序列化**，读取保存的对象的过程叫 **反序列化**

**ObjectOutputStream**  ObjectOutputStream(OutputStream out)

- **writeInt**
- **writeChar**
- **WriteUTF**
- ......

**ObjectInputStream**     ObjectIntputStream(InputStream in)

- **readInt**
- **readChar**
- **readString**
- ......

**注意**：

-  通过 **ObjectOutputStream** 写入的对象一定要实现 **Serializable** 或者 **Externalizable**接口 
-  我们一般是实现**Serializable**接口，因为它里面没有方法，而 **Externalizable**接口中需要我们实现里面的方法 
-  被 **序列化** 的成员 也需要实现**Serializable**接口 
-  读写顺序要一致 
-  为提高兼容性可以在被 **序列化** 的类中加入 **SeriaVersionUID = 1L** 
-  用完记得关闭流，我们只需要关闭外层流即可 

---

# GUI编程

**Java的图形化处理常用的两个包——   ** **java.awt** 和 **javax.swing**

其中我们平时用的比较多的还是swing包

```java
package jju.soft.ui;

import javax.swing.*;
import java.awt.*;

public class ComponentInWindow extends JFrame {
    JTextField text;//文本框,只有一行
    JButton button;//按钮
    JCheckBox checkBox1, checkBox2, checkBox3;//选择框，可选可不选两种状态,也可多选

    ButtonGroup group;//用于将单选按钮组装起来
    JComboBox comBox;//下拉列表，通过  .addItem（）将选项加进去
    JRadioButton radio1, radio2;//单选框，需要与ButtonGroup的对象的 .add（）方法连用
    JTextArea area;//文本区

    public ComponentInWindow(){
        init();
        this.setVisible(true);
        this.setDefaultCloseOperation(3);
    }

    private void init(){
        this.setLayout(new FlowLayout());
        add (new JLabel("文本框："));
        text = new JTextField(10);
        add(text);
        add(new JLabel("按钮"));
        button = new JButton("确定");
        add(button);
        add(new JLabel("选择框："));
        checkBox1 = new JCheckBox("喜欢音乐");
        checkBox2 = new JCheckBox("喜欢旅游");
        checkBox3 = new JCheckBox("喜欢篮球");
        add(checkBox1);
        add(checkBox2);
        add(checkBox3);
        add(new JLabel("单选按钮："));

        group = new ButtonGroup();
        radio1 = new JRadioButton("男");
        radio2 = new JRadioButton("女");
        group.add(radio1);
        group.add(radio2);

        add(new JLabel("下拉列表"));
        comBox = new JComboBox();
        comBox.addItem("音乐天地");
        comBox.addItem("武术天地");
        comBox.addItem("象棋乐园");
        add(comBox);

        add(new JLabel("文本区"));
        area = new JTextArea(6,12);
        add(new JScrollPane(area));

    }

}
```

```java
import jju.soft.ui.ComponentInWindow;

public class Main {
    public static void main(String args[]){
        ComponentInWindow win = new ComponentInWindow();
        win.setBounds(100,100,310,260);
        win.setTitle("常用组件");
    }
}
```

**效果如下**


      - 写完一定要关闭文件[基础Java.md](https://github.com/atkcoder/note/files/13374512/Java.md)


#### 文件复制

通过IO流完成文件复制：

1. 通过 **输入流**   读取文件到 **Java虚拟机**
2. 通过 **输出流**   **Java虚拟机** 写入文件到文件夹
3. 方式为边读边写

```java
        String sourFile = "D:\\temp\\boy.jpeg";
        String destFile = "D:\\temp\\pic\\sea.jpeg";
        byte buf[] = new byte[1024];
        int dataLength = 0;

        FileOutputStream fileOutputStream = null;
        FileInputStream fileInputStream = null;

        try{
            fileInputStream = new FileInputStream(sourFile);
            fileOutputStream = new FileOutputStream(destFile);
            while (( dataLength = fileInputStream.read(buf)) != -1){
                fileOutputStream.write(buf,0,dataLength);
            }
        }
        catch (IOException e){
            e.printStackTrace();
        }
        finally {
            try{
                fileInputStream.close();
                fileOutputStream.close();
            }
            catch (IOException e){
                e.printStackTrace();
            }
        }
```

#### FileReader

Reader的间接子类，常用方法：

- **FileReader:**  FileReader(String filePath  /  File fileName) 
   - **read:** 
      - int read():  读一个字符
      - int read(char buf[])
   - **close**

#### FileWriter

- **FileWriter:**  同上 
   - **write** 
      - public void write(int c)
      - public void write(char buf[])
      - public void write(char buf[], int off,int len)
   - **close**
   - **flush**

### 处理流

#### BufferedReader

**Reader**  的子类 同时可以往构造方法里面传**Reader**的子类，其为私有成员变量

- **readLine**  方法，读取一行字符，但不包括回车

其他方法具体看传进去的子类

#### BufferedWriter

- **newLine**  方法，写入一个回车

#### BufferedInputStream

同上

#### BufferedOutputStream

同上

#### 对象处理流

对象处理流，可以将对象保存起来，并 **不是单纯保存值** 而是 **保存数据类型和值**

例如：

​	我定义了一个 Dog 类，并声明了一个Dog的对象，但是我想要将**整个对象**保存起来，并不只是保存里面的值，而且还要保存方法，这时候只能通过对象处理流来操作

保存对象的过程也叫 **序列化**，读取保存的对象的过程叫 **反序列化**

**ObjectOutputStream**  ObjectOutputStream(OutputStream out)

- **writeInt**
- **writeChar**
- **WriteUTF**
- ......

**ObjectInputStream**     ObjectIntputStream(InputStream in)

- **readInt**
- **readChar**
- **readString**
- ......

**注意**：

-  通过 **ObjectOutputStream** 写入的对象一定要实现 **Serializable** 或者 **Externalizable**接口 
-  我们一般是实现**Serializable**接口，因为它里面没有方法，而 **Externalizable**接口中需要我们实现里面的方法 
-  被 **序列化** 的成员 也需要实现**Serializable**接口 
-  读写顺序要一致 
-  为提高兼容性可以在被 **序列化** 的类中加入 **SeriaVersionUID = 1L** 
-  用完记得关闭流，我们只需要关闭外层流即可 

---

# GUI编程

**Java的图形化处理常用的两个包——   ** **java.awt** 和 **javax.swing**

其中我们平时用的比较多的还是swing包

```java
package jju.soft.ui;

import javax.swing.*;
import java.awt.*;

public class ComponentInWindow extends JFrame {
    JTextField text;//文本框,只有一行
    JButton button;//按钮
    JCheckBox checkBox1, checkBox2, checkBox3;//选择框，可选可不选两种状态,也可多选

    ButtonGroup group;//用于将单选按钮组装起来
    JComboBox comBox;//下拉列表，通过  .addItem（）将选项加进去
    JRadioButton radio1, radio2;//单选框，需要与ButtonGroup的对象的 .add（）方法连用
    JTextArea area;//文本区

    public ComponentInWindow(){
        init();
        this.setVisible(true);
        this.setDefaultCloseOperation(3);
    }

    private void init(){
        this.setLayout(new FlowLayout());
        add (new JLabel("文本框："));
        text = new JTextField(10);
        add(text);
        add(new JLabel("按钮"));
        button = new JButton("确定");
        add(button);
        add(new JLabel("选择框："));
        checkBox1 = new JCheckBox("喜欢音乐");
        checkBox2 = new JCheckBox("喜欢旅游");
        checkBox3 = new JCheckBox("喜欢篮球");
        add(checkBox1);
        add(checkBox2);
        add(checkBox3);
        add(new JLabel("单选按钮："));

        group = new ButtonGroup();
        radio1 = new JRadioButton("男");
        radio2 = new JRadioButton("女");
        group.add(radio1);
        group.add(radio2);

        add(new JLabel("下拉列表"));
        comBox = new JComboBox();
        comBox.addItem("音乐天地");
        comBox.addItem("武术天地");
        comBox.addItem("象棋乐园");
        add(comBox);

        add(new JLabel("文本区"));
        area = new JTextArea(6,12);
        add(new JScrollPane(area));

    }

}
```

```java
import jju.soft.ui.ComponentInWindow;

public class Main {
    public static void main(String args[]){
        ComponentInWindow win = new ComponentInWindow();
        win.setBounds(100,100,310,260);
        win.setTitle("常用组件");
    }
}
```

**效果如下**

