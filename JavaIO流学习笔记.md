# JavaIO流学习笔记

**写在前面**

**学习地址：**

[【韩顺平讲Java】Java IO流专题](https://www.bilibili.com/video/BV15B4y1u7Rn)

**感谢韩老师的讲解视频，十分感谢！！！**

**先上思维导图**

**流的分类一览**

![mind](http://qtwu22ub9.hn-bkt.clouddn.com/img/mind.jpg)

# 1. 文件的基本概念

## 1.1 文件流

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/JavaIO%E6%B5%81%E5%9B%BE%E7%89%87/Snipaste_2021-05-29_21-30-42.png)

# 2. 创建文件

## 2.1通过三个方法创建文件

```java
package com.zhuang;

import java.io.File;
import java.io.IOException;

/**
 * @Classname FileCreate
 * @Description 演示创建文件的3种方法
 * @Date 2021/5/29 19:40
 * @Created by dell
 */

public class FileCreate {
    public static void main(String[] args) {
      //  create01();
        create03();
      //  create02();
    }

    // 方式1 new File(String pathname)
    public static void create01(){
        String filePath = "f:\\a.txt";
        File file = new File(filePath);

        try {
            file.createNewFile();
            System.out.println("文件创建成功！");
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("文件创建失败！");
        }
    }

    ////方式 2 new File(File parent,String child) //根据父目录文件+子路径构建
    public static void create02(){
        File parentFile = new File("f:\\");
        String fileName="b.txt";

        File file = new File(parentFile, fileName);

        try {
            file.createNewFile();
            System.out.println("创建成功");
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("创建失败");
        }
    }

    //方式 3 new File(String parent,String child) //根据父目录+子路径构建
    public static void create03(){
        File parentPath = new File("f:\\");
        String fileName="c.txt";

        File file = new File(parentPath, fileName);

        try {
            file.createNewFile();
            System.out.println("创建成功");
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("创建失败");
        }
    }
}
```

## 2.2 获取文件的信息

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/JavaIO%E6%B5%81%E5%9B%BE%E7%89%87/Snipaste_2021-05-29_21-35-33.png)

```java
package com.zhuang;

import java.io.File;

/**
 * @Classname FileInformation
 * @Description 获取文件的相关信息
 * @Date 2021/5/29 19:57
 * @Created by dell
 */

public class FileInformation {
    public static void main(String[] args) {
        info();
    }

    private static void info() {
        //先创建文件对象
        File file = new File("f:\\a.txt");
        //调用相应的方法，得到对应信息
        System.out.println("文件名字=" + file.getName());
        //getName、getAbsolutePath、getParent、length、exists、isFile、isDirectory
        System.out.println("文件绝对路径=" + file.getAbsolutePath());
        System.out.println("文件父级目录=" + file.getParent());
        System.out.println("文件大小(字节)=" + file.length());
        System.out.println("文件是否存在=" + file.exists());//T
        System.out.println("是不是一个文件=" + file.isFile());//T
        System.out.println("是不是一个目录=" + file.isDirectory());//F
    }
}
```

# 3. IO流原理及流的分类

## 3.1 JavaIO流原理

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/JavaIO%E6%B5%81%E5%9B%BE%E7%89%87/Snipaste_2021-05-29_21-38-26.png)

![Snipaste_2021-05-29_21-38-42](https://gitee.com/zhuang-kang/note-picture-2/raw/master/JavaIO%E6%B5%81%E5%9B%BE%E7%89%87/Snipaste_2021-05-29_21-38-42.png)

## 3.2 流的分类

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/JavaIO%E6%B5%81%E5%9B%BE%E7%89%87/Snipaste_2021-05-29_21-39-06.png)



# 4. IO流体系图-常用的类

# 5.FileInputStream

FileInputStream 读取 a.txt 文件

```java
package com.zhuang;

import java.io.FileInputStream;
import java.io.IOException;

/**
 * @Classname FileInputStream_
 * @Description 演示 FileInputStream 的使用(字节输入流 文件--> 程序)
 * @Date 2021/5/29 20:01
 * @Created by dell
 */

public class FileInputStream_ {
    public static void main(String[] args) throws IOException {
        //  readFile01();
        readFile02();

    }


    private static void readFile01() throws IOException {
        String filePath = "f:\\a.txt";
        int readData = 0;

        FileInputStream fileInputStream = null;

        try {
            //创建fileInputStream对象 用于读取文件
            fileInputStream = new FileInputStream(filePath);
            /*
            从该输入流读取一个字节的数据，如果没有输入可用，返回-1 表示读取完毕
             */
            while ((readData = fileInputStream.read()) != -1) {
                System.out.print((char) readData);
            }
        } catch (Exception e) {
            e.printStackTrace();

        } finally {
            fileInputStream.close();
        }
    }


    /*
     * 使用 read(byte[] b) 读取文件，提高效率
     */


    private static void readFile02() {

        String filePath = "f:\\a.txt";
        byte[] buf = new byte[1024];
        int readData = 0;

        FileInputStream fileInputStream = null;

        try {
            fileInputStream = new FileInputStream(filePath);
            /*
            从输入流最多读取b.length字节的数据到字节数组  返回-1 读取完毕
            读取正常 返回实际读取的字节数
             */
            while ((readData = fileInputStream.read(buf)) != -1) {
                System.out.print(new String(buf, 0, readData));//显示
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                fileInputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```



# 6. FileOutputStream

## 6.1 用FileOutputStream读取文件

```java
package com.zhuang;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * @Classname FileOutputStream01
 * @Description 演示使用 FileOutputStream 将数据写到文件中, * 如果该文件不存在，则创建该文件
 * @Date 2021/5/29 20:17
 * @Created by dell
 */

public class FileOutputStream01 {
    public static void main(String[] args) {
        writeFile();
    }

    private static void writeFile() {
        String filePath = "f:\\d.txt";

        FileOutputStream fileOutputStream=null;

        try {
            //1. new FileOutputStream(filePath) 创建方式，当写入内容是，会覆盖原来的内容
           //2. new FileOutputStream(filePath, true) 创建方式，当写入内容是，是追加到文件后面
            fileOutputStream=new FileOutputStream(filePath,true);
            //写入一个字节
            fileOutputStream.write('K');
            //写入字符串
            String str="kangxiaozhuang";
            //转为字节数组
            byte[] bytes = str.getBytes("UTF-8");
            //按照编码的形式写入文件中
            fileOutputStream.write(str.getBytes("UTF-8"));

            /*
            write(byte[] b, int off, int len)
            将 len 字节从位于偏移量 off 的指定字节数组写入此文件输出流
             */
            fileOutputStream.write(str.getBytes(), 0, 3);
            System.out.println("写入文件成功");
        } catch (Exception e) {
            System.out.println("写入文件失败");
            e.printStackTrace();
        } finally {
            try {
                fileOutputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## 6.2 文件拷贝

```java
package com.zhuang;


import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * @Classname FileCopy
 * @Description 文件拷贝
 * @Date 2021/5/29 20:28
 * @Created by dell
 */

public class FileCopy {
    public static void main(String[] args) {
        copyFile();
    }

    private static void copyFile() {
        /*
        创建输入流和输出流
         */
        String srcFilePath = "f:\\1.jpg";
        String desFilePath = "f:\\2.jpg";

        FileInputStream fis = null;
        FileOutputStream fos = null;

        try {
            fis = new FileInputStream(srcFilePath);
            fos = new FileOutputStream(desFilePath);
            //定义字节数组 提高读取效果
            byte[] buf = new byte[1024];
            int readLen=0;

            while ((readLen=fis.read(buf))!=-1){
                //一边读一边写
                fos.write(buf, 0, readLen);
            }
            System.out.println("文件拷贝成功！！！");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                fis.close();
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }

}
```

# 7. FileReader 和 FileWriter

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/JavaIO%E6%B5%81%E5%9B%BE%E7%89%87/Snipaste_2021-05-29_21-55-47.png)

## 7.1 FileReader方法

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/JavaIO%E6%B5%81%E5%9B%BE%E7%89%87/Snipaste_2021-05-29_21-57-54.png)

## 7.2 FileWriter方法 

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/JavaIO%E6%B5%81%E5%9B%BE%E7%89%87/Snipaste_2021-05-29_21-58-01.png)

## 7.3 FileReader读取文件

```java
package com.zhuang;

import java.io.FileReader;
import java.io.IOException;

/**
 * @Classname FileReader_
 * @Description FileReader
 * @Date 2021/5/29 21:48
 * @Created by dell
 */

public class FileReader_ {
    public static void main(String[] args) throws Exception {
       // readFile01();
        readFile02();
    }
    public static void readFile01() {
        String filePath = "f:\\d.txt";
        FileReader fileReader = null;
        int data = 0;
        //1. 创建 FileReader 对象
        try {
            fileReader = new FileReader(filePath);
        //循环读取 使用 read, 单个字符读取
            while ((data = fileReader.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (fileReader != null) {
                    fileReader.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 字符数组读取文件
     */
    public static void readFile02() throws Exception {
        System.out.println("===readFile02===");
        String filePath = "f:\\d.txt";
        FileReader fileReader = null;
        int readLen = 0;
        char[] buf = new char[8];
        //1. 创建 FileReader 对象
        try {
            fileReader = new FileReader(filePath);
        //循环读取 使用 read(buf), 返回的是实际读取到的字符数
        //如果返回-1, 说明到文件结束
            while ((readLen = fileReader.read(buf)) != -1) {
                System.out.print(new String(buf, 0, readLen));
            }
        } catch(IOException e){
            e.printStackTrace();
        } finally{
            try {
                if (fileReader != null) {
                    fileReader.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}

````

## 7.4 FileWriter写入文件

```java
package com.zhuang;

import java.io.FileWriter;
import java.io.IOException;

/**
 * @Classname FileWriter_
 * @Description 用一句话描述类的作用
 * @Date 2021/5/29 20:44
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class FileWriter_ {
    public static void main(String[] args) {
        String filePath = "f:\\note.txt";
        //创建 FileWriter 对象
        FileWriter fileWriter = null;
        char[] chars = {'a', 'b', 'c'};
        try {
            fileWriter = new FileWriter(filePath);//默认是覆盖写入
        // 3) write(int):写入单个字符
            fileWriter.write('H');
        // 4) write(char[]):写入指定数组
            fileWriter.write(chars);
        // 5) write(char[],off,len):写入指定数组的指定部分
            fileWriter.write("康小庄2333".toCharArray(), 0, 3);
        // 6) write（string）：写入整个字符串
            fileWriter.write(" 你好北京~");
            fileWriter.write("风雨之后，定见彩虹");
            // 7) write(string,off,len):写入字符串的指定部分
            fileWriter.write("上海天津", 0, 2);
        //在数据量大的情况下，可以使用循环操作
            System.out.println("写入文件成功");
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                //一定要关闭流 才能写入数据
                fileWriter.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```



# 8. 节点流和处理流

## 8.1 基本介绍

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/JavaIO%E6%B5%81%E5%9B%BE%E7%89%87/Snipaste_2021-05-29_22-03-47.png)



## 8.2 节点流和处理流一览图

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/JavaIO%E6%B5%81%E5%9B%BE%E7%89%87/Snipaste_2021-05-29_22-04-45.png)

## 8.3 节点流和处理流的区别和联系

- 节点流是底层流/低级流，直接跟数据源相接
- 处理流(包装流)包装节点流，既可以消除不同节点流的实现差异，也可以提供更方便的方法来完成输入输出



## 8.4 处理流的功能

- 1．性能的提高:主要以增加缓冲的方式来提高输入输出的效率。

- 2．操作的便捷:处理流可能提供了一系列便捷的方法来一次输入输出大批量的数据，使用更加灵活方便



# 9. 处理流-BufferedReader 和 BufferedWriter

## 9.1 BufferedReader的使用

```java
package com.zhuang;

import java.io.*;

/**
 * @Classname BufferedReader_
 * @Description * 演示 bufferedReader 使用
 * @Date 2021/5/29 20:52
 * @Created by dell
 */

public class BufferedReader_ {
    public static void main(String[] args) throws Exception {
        String filePath = "f:\\note.txt";

        //创建BufferedReader
        BufferedReader  bufferedReader=new BufferedReader(new FileReader(filePath));
        try {
            //按行读取 效率高
            String line;
            //1. bufferedReader.readLine() 是按行读取文件
            //2. 当返回 null 时，表示文件读取完毕
            while ((line = bufferedReader.readLine())!=null){
                System.out.println(line);
            }
           // 关闭流, 这里注意，只需要关闭 BufferedReader ，因为底层会自动的去关闭 节点流
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            bufferedReader.close();
        }
    }
}
```

## 9.2 BufferedWriter的使用

```java
package com.zhuang;

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

/**
 * @Classname BufferedWriter_
 * @Description 演示 BufferedWriter 的使用
 * @Date 2021/5/29 21:00
 * @Created by dell
 */

public class BufferedWriter_ {
    public static void main(String[] args) throws Exception {
        String filePath = "f:\\ok.txt";
        //创建 BufferedWriter
        //说明:
        //1. new FileWriter(filePath, true) 表示以追加的方式写入
        //2. new FileWriter(filePath) , 表示以覆盖的方式写入
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(filePath));
        bufferedWriter.write("hello, 康小庄!");
        bufferedWriter.newLine();//插入一个和系统相关的换行
        bufferedWriter.write("hello2, 康小庄!");
        bufferedWriter.newLine();
        bufferedWriter.write("hello3, 康小庄!");
        bufferedWriter.newLine();
        System.out.println("写入文件成功！");
        //关闭流
        bufferedWriter.close();

    }
}
```

## 9.3 拷贝文件

```java
package com.zhuang;

import java.io.*;

/**
 * @Classname BufferedCopy_
 * @Description 用一句话描述类的作用
 * @Date 2021/5/29 21:07
 * @Created by dell
 */

public class BufferedCopy_ {
    public static void main(String[] args) {
        String srcFilePath = "f:\\1.jpg";
        String desFilePath="f:\\3.jpg";
        BufferedReader br=null;
        BufferedWriter bw=null;
        //定义行
        String line;

        try {
            br=new BufferedReader(new FileReader(srcFilePath));
            bw=new BufferedWriter(new FileWriter(desFilePath));
            while ((line= br.readLine())!=null){
                bw.write(line);
                //插入新的换行
                bw.newLine();
            }
            System.out.println("拷贝成功！");
        }catch (Exception e) {
            e.printStackTrace();
        }finally {
            try {
                br.close();
                bw.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

**使用BufferedOutputStream写入文件**

```java
package com.zhuang;

import java.io.*;

/**
 * @Classname BufferedCopy2_
 * @Description  演示使用 BufferedOutputStream 和 BufferedInputStream 使用 完成文件的拷贝
 * @Date 2021/5/31 14:50
 * @Created by dell
 */

public class BufferedCopy2_ {
    public static void main(String[] args) throws IOException {
        String srcFilePath = "f:\\a.txt";
        String destFilePath = "f:\\a2.txt";
        //创建 BufferedOutputStream 对象 BufferedInputStream 对象
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;

        try {
        //因为 FileInputStream 是 InputStream 子类
            bis = new BufferedInputStream(new FileInputStream(srcFilePath));
         bos = new BufferedOutputStream(new FileOutputStream(destFilePath));
        //循环的读取文件，并写入到 destFilePath
        byte[] buff = new byte[1024];
        int readLen = 0;
        //当返回 -1 时，就表示文件读取完毕
        while ((readLen = bis.read(buff)) != -1) {
            bos.write(buff, 0, readLen);
        }
        System.out.println("文件拷贝完毕~~~");
        } catch (IOException ioException) {
            ioException.printStackTrace();
        }finally {
            bis.close();
            bos.close();
        }
    }
}
```

# 10. 对象流-ObjectInputStream 和 ObjectOutputStream

![image-20210531170403674](http://qtwu22ub9.hn-bkt.clouddn.com/img/image-20210531170403674.png)

## 10.1 对象流的介绍

功能：提供了对基本类型或对象类型的序列化和反序列化的方法 

- ObjectOutputStream 提供 序列化功能 

- ObjectInputStream 提供 反序列化功能

![image-20210531170512989](http://qtwu22ub9.hn-bkt.clouddn.com/img/image-20210531170512989.png)

**实现序列化的操作**

```java
package com.zhuang;

import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

/**
 * @Classname ObjectOutStream_
 * @Description ObjectOutStream实现序列化操作
 * @Date 2021/5/31 14:56
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class ObjectOutStream_{
    public static void main(String[] args) throws IOException {
        //序列化后，保存的文件格式，不是存文本，而是按照他的格式来保存
        String filePath = "f:\\data.dat";
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filePath));
        //序列化数据到 f:\data.dat
        oos.writeInt(100);// int -> Integer (实现了 Serializable)
        oos.writeBoolean(true);// boolean -> Boolean (实现了 Serializable)
        oos.writeChar('a');// char -> Character (实现了 Serializable)
        oos.writeDouble(9.5);// double -> Double (实现了 Serializable)
        oos.writeUTF("康小庄");//String
        //保存一个 dog 对象
        oos.writeObject(new Dog("旺财", 10, "日本", "白色"));
        oos.close();
        System.out.println("数据保存完毕(序列化形式)");
    }
}
```

**实现反序列化的操作**

```java
package com.zhuang;

import java.io.FileInputStream;
import java.io.ObjectInputStream;

/**
 * @Classname ObjectInputStream_
 * @Description ObjectInputStream实现反序列化
 * @Date 2021/5/31 15:00
 * @Created by dell
 */

public class ObjectInputStream_ {
    public static void main(String[] args) throws Exception {
        //1.创建流对象
        ObjectInputStream ois=new ObjectInputStream(new FileInputStream("f:\\data.dat"));
        // 2.读取， 注意顺序
        System.out.println(ois.readInt());
        System.out.println(ois.readBoolean());
        System.out.println(ois.readChar());
        System.out.println(ois.readDouble());
        System.out.println(ois.readUTF());
        // 3.关闭
        ois.close();
        System.out.println("以反序列化的方式读取(恢复)ok~");
    }
}
```

**操作注意事项**

![image-20210531170735820](http://qtwu22ub9.hn-bkt.clouddn.com/img/image-20210531170735820.png)



# 11. 转换流-InputStreamReader 和 OutputStreamWriter

![image-20210531170838279](http://qtwu22ub9.hn-bkt.clouddn.com/img/image-20210531170838279.png)

**使用InputStreamReader读取内容 并指定编码**

```java
package com.zhuang;

import java.io.*;

/**
 * @Classname InputStreamReader_
 * @Description InputStreamReader
 * @Date 2021/5/31 15:09
 * @Created by dell
 */

public class InputStreamReader_ {
    public static void main(String[] args) throws Exception{
        String filePath = "f:\\a.txt";
        //1. 把 FileInputStream 转成 InputStreamReader
        //2. 指定编码 gbk
        //InputStreamReader isr = new InputStreamReader(new FileInputStream(filePath), "gbk");
        //3. 把 InputStreamReader 传入 BufferedReader
        //BufferedReader br = new BufferedReader(isr);
        //将 2 和 3 合在一起
        BufferedReader br = new BufferedReader(new InputStreamReader(
                new FileInputStream(filePath), "gbk"));
        //4. 读取
        String s = br.readLine();
        System.out.println("读取内容=" + s);
        //5. 关闭外层流
        br.close();
    }
}
```



# 12. 打印流-PrintStream 和 PrintWriter

![image-20210531171119302](http://qtwu22ub9.hn-bkt.clouddn.com/img/image-20210531171119302.png)

```java
package com.zhuang;

import java.io.IOException;
import java.io.PrintStream;

/**
 * @Classname PrintStream_
 * @Description 演示 PrintStream （字节打印流/输出流）
 * @Date 2021/5/31 15:27
 * @Created by dell
 */

public class PrintStream_ {
    public static void main(String[] args) throws IOException {
        PrintStream out = System.out;
        //在默认情况下，PrintStream 输出数据的位置是 标准输出，即显示器
        out.println("zk666");
        out.close();
    }
}
```



# 13. Properties类

**我们一般用xxx.properties用作配置文件 是以K-V键值对的形式**

**常用方法**
![image-20210531171526982](http://qtwu22ub9.hn-bkt.clouddn.com/img/image-20210531171526982.png)

```java
package com.zhuang;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

/**
 * @Classname Properties01
 * @Description Properties类
 * @Date 2021/5/31 16:23
 * @Created by dell
 */

public class Properties01 {
    public static void main(String[] args) throws IOException {
        //读取 mysql.properties 文件，并得到 ip, user 和 pwd
        BufferedReader br = new BufferedReader(new FileReader("f:\\mysql.properties"));
        String line = "";
        while ((line = br.readLine()) != null) { //循环读取
            String[] split = line.split("=");
        //如果我们要求指定的 ip 值
            if("ip".equals(split[0])) {
                System.out.println(split[0] + "值是: " + split[1]);
            }
        }
        br.close();
    }
}
```

**来读取文件并显示出来**

```java
package com.zhuang;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.util.*;

/**
 * @Classname Properties02
 * @Description Properties02
 * @Date 2021/5/31 16:29
 * @Created by dell
 */

public class Properties02 {
    public static void main(String[] args) throws IOException {
        //使用 Properties 类来读取 mysql.properties 文件
//1. 创建 Properties 对象
        Properties properties = new Properties();
//2. 加载指定配置文件
        properties.load(new FileReader("f:\\mysql.properties"));
//3. 把 k-v 显示控制台
        properties.list(System.out);
//4. 根据 key 获取对应的值
        String user = properties.getProperty("user");
        String pwd = properties.getProperty("pwd");
        String ip = properties.getProperty("ip");
        System.out.println("用户名=" + user);
        System.out.println("密码是=" + pwd);
        System.out.println("IP是=" + ip);
    }
}
```

# 14. 练习

## 14.1 练习1

> 判断是否存在目录 不存在就创建 并在目录中创建文件 ，写入一点数据

```java
package com.zhuang.HomeWork;

import java.io.*;

/**
 * @Classname HomeWork01
 * @Description 用一句话描述类的作用
 * @Date 2021/5/31 16:35
 * @Created by dell
 */

public class HomeWork01 {
    public static void main(String[] args) throws IOException {
        String dirPath = "f:\\mytemp";
        File file = new File(dirPath);
        if (!file.exists()) {
            if (file.mkdirs()) {
                System.out.println("创建" + dirPath + "成功");
            } else {
                System.out.println("创建" + dirPath + "失败");
            }
        }

        String filePath = dirPath + "\\hello.txt";
        file = new File(filePath);
        if (!file.exists()) {
            if (file.createNewFile()) {
                System.out.println(filePath + "创建成功");
                BufferedWriter bw=new BufferedWriter(new FileWriter(file));
                bw.write("哈哈哈！！！");
                bw.close();
            } else {
                System.out.println(filePath + "创建失败");
            }
        } else {
            System.out.println("文件已经存在");
        }
    }
}
```



## 14.2 练习2

> 写一个properties文件 ，读取文件的内容，并将文件的内容序列化到任意文件中

```java
package com.zhuang.HomeWork;

import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.ObjectOutputStream;
import java.io.Serializable;
import java.util.Properties;

/**
 * @Classname HomeWork02
 * @Description HomeWork02
 * @Date 2021/5/31 16:48
 * @Created by dell
 */

public class HomeWork02 {
    public static void main(String[] args) throws Exception {
        String filePath = "f:\\dog.properties";
        Properties properties = new Properties();
        properties.load(new FileReader(filePath));

        String name = (String) properties.get("name");
        int age = Integer.parseInt(properties.get("age")+"");
        String color = (String) properties.get("color");

        Dog dog = new Dog(name, age, color);
        System.out.println(dog);

        //将信息序列化进文件中
        String filePath2="f:\\a2.txt";
        ObjectOutputStream oos=new ObjectOutputStream(new FileOutputStream(filePath2));
        oos.writeObject(dog);

        oos.close();
        System.out.println("写入文件成功");
    }
}
class Dog implements Serializable {
        private String name;
        private int age;
        private String color;

        public Dog(String name, int age, String color) {
            this.name = name;
            this.age = age;
            this.color = color;
        }

        @Override
        public String toString() {
            return "Dog{" +
                    "name='" + name + '\'' +
                    ", age=" + age +
                    ", color='" + color + '\'' +
                    '}';
        }
    }
```

**纸上得来终觉浅，绝知此事要躬行**



**若有错误，还请各位指出错误，及时更改！**
