
# Java网络编程学习笔记

写在前面：感谢韩老师的讲解视频！

**学习地址：[【韩顺平讲Java】Java网络多线程专题 - TCP UDP Socket编程](https://www.bilibili.com/video/BV1j54y1b7qv)**

# 1. 网络的相关概念

## 1.1 网络通信

- 概念：两台设备之间通过网络实现数据传输
- 网络通信：将数据通过网络从一台设备传输到另以台设备
- java.net包下提供了一系列的类或接口，供程序员使用，完成网络通信

[![21wsEj.png](https://z3.ax1x.com/2021/06/03/21wsEj.png)](https://imgtu.com/i/21wsEj)

## 1.2 网络

- 概念：两台或多台设备通过一定物理设备连接来构成了网络
- 根据网络的覆盖范围不同，对网络进行分类
- 局域网：覆盖范围最小，仅仅覆盖一个教室或者一个机房
- 城域网：覆盖范围比较大，可以覆盖一个城市
- 广域网：覆盖范围最大，可以覆盖全国，甚至全球，万维网是广域网的代表

[![21wyUs.png](https://z3.ax1x.com/2021/06/03/21wyUs.png)](https://imgtu.com/i/21wyUs)

## 1.3 IP地址

- 概念：用于唯一标识网络中的每台主机/计算机
- 查看IP地址的命令：ipconfig
- ip地址的表示形式：点分十进制 xx.xx.xx.xx
- 每一个十进制的范围 0-255
- ip地址的组成=网络地址+主机地址  例如192.168.152.xxx
- ipv6是互联网工程任务组设计的用于替代IPv4的下一代IP协议
- 由于IPv4最大的问题在于网络地址资源有限，严重制约了互联网的应用和发展，IPv6的使用可以解决网络地址资源数量的问题，也解决了多种接入设备互联网的障碍

更多了解IPv6参考 [IPv6_百度百科 (baidu.com)](https://baike.baidu.com/item/IPv6/172297)



## 1.4 ipv4 地址分类

[![21w65n.png](https://z3.ax1x.com/2021/06/03/21w65n.png)](https://imgtu.com/i/21w65n)

![image-20210601211137383](http://qtwu22ub9.hn-bkt.clouddn.com/img/image-20210601211137383.png)



## 1.5 域名

- 好处：方便记忆，解决记ip的困难
- 概念：将ip地址映射成域名，HTTP协议

**端口号**

- 概念：用于标识计算机上某个特定的网络程序
- 表示形式：以整数形式，端口范围0-65535 [2个字节表示端口 0~2的16次-1]
- 常见网络程序端口号
  - tomcat：8080
  - mysql：3306
  - sqlserve：1433



## 1.6 网络通信协议

[![21wDbQ.png](https://z3.ax1x.com/2021/06/03/21wDbQ.png)](https://imgtu.com/i/21wDbQ)

- TCP/IP（Transmission Control Protocol/Internet Protocol，传输控制协议/网际协议）是指能够在多个不同网络间实现信息传输的协议簇

|  OSI模型   |   TCP/IP模型    | TCP/IP模型各层对应协议 |
| :--------: | :-------------: | :--------------------: |
|   应用层   |     应用层      |    HTTP FTP DNS...     |
|   表示层   |     应用层      |    HTTP FTP DNS...     |
|   会话层   |     应用层      |    HTTP FTP DNS...     |
|   传输层   |   传输层(TCP)   |       TCP UDP...       |
|   网络层   |   网络层(IP)    |      IP ICMP ARP       |
| 数据链路层 | 物理+数据链路层 |          Link          |
|   物理层   | 物理+数据链路层 |          Link          |

## 1.7 TCP和UDP

**TCP协议:传输控制协议**

- 1.使用TCP协议前，须先建立TCP连接，形成传输数据通道
- 2.传输前，采用"三次握手"方式，是可靠的
- 3.TCP协议进行通信的两个应用进程:客户端、服务端
- 4.在连接中可进行大数据量的传输
- 5.传输完毕，需释放已建立的连接，效率低

**UDP协议:用户数据协议**

- 1.将数据、源、目的封装成数据包，不需要建立连接
- 2.每个数据报的大小限制在64K内,不适合传输大量数据
- 3.因无需连接，故是不可靠的
- 4.发送数据结束时无需释放资源(因为不是面向连接的)，速度快
- 5.举例:厕所通知:发短信



# 2. InetAddress类

常用方法

[![21wBDg.png](https://z3.ax1x.com/2021/06/03/21wBDg.png)](https://imgtu.com/i/21wBDg)

```java
package com.zhuang.Intelnet;

import java.net.InetAddress;

/**
 * @Classname API_
 * @Description 网络编程常用api
 * @Date 2021/6/3 12:02
 * @Created by dell
 */

public class API_ {
    public static void main(String[] args) throws Exception {
        //获取本机 InetAddress 对象 getLocalHost
        InetAddress localHost = InetAddress.getLocalHost();
        System.out.println(localHost);
        //根据指定主机名/域名获取 ip 地址对象 getByName
        InetAddress host2 = InetAddress.getByName("DESKTOP-5H1AECH");
        System.out.println(host2);
        InetAddress host3 = InetAddress.getByName("itkxz.cn");
        System.out.println(host3);
        //获取 InetAddress 对象的主机名 getHostName
        String host3Name = host3.getHostName();
        System.out.println(host3Name);
        //获取 InetAddress 对象的地址 getHostAddress
        String host3Address = host3.getHostAddress();
        System.out.println(host3Address);
    }
}
```



# 3. Socket

基本介绍

- 套接字(Socket)开发网络应用程序被广泛使用
- 通信的两端都要有Socket，是两台机器通信的端点
- 网络通信其实就是Socket的通信
- Socket允许程序把网络连接当初一个流，数据在两个Socket间通过IO传输
- 一般主动发起通信的应用程序属于`客户端`，等待通信请求的为`服务端`

**示意图**

[![21wgCq.png](https://z3.ax1x.com/2021/06/03/21wgCq.png)](https://imgtu.com/i/21wgCq)



# 4. TCP 网络通信编程

基本介绍

- 基于客户端—服务端的网络通信
- 底层使用的是TCP/IP协议
- 场景举例：客户端发送数据，服务端接受并显示在控制台
- 基于Socket的TCP编程



## 4.1 练习

### 4.1.1 使用字节流(练习1)

[![21wR2V.png](https://z3.ax1x.com/2021/06/03/21wR2V.png)](https://imgtu.com/i/21wR2V)

**SocketTCP01Server**

```java
package com.zhuang.Intelnet;

import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * @Classname SocketTCP01Server
 * @Description 服务端
 * @Date 2021/6/3 12:20
 * @Created by dell
 */

public class SocketTCP01Server {
    public static void main(String[] args) throws Exception{
        ServerSocket serverSocket = new ServerSocket(9999);
        /*
        没有客户端连接9999端口会一直阻塞，等待连接
        如果有客户端连接，会返回Socket对象，程序继续
         */
        System.out.println("服务端在9999端口监听！");


        Socket socket = serverSocket.accept();
        System.out.println("服务端 socket="+socket.getClass());

        //通过socket.getInputStream()  读取客户端写入数据通信的数据，显示
        InputStream inputStream = socket.getInputStream();

        // IO读取
        byte[] buf=new byte[1024];
        int readLength = 0;
        while ((readLength=inputStream.read(buf))!=-1){
            //读取到的实际长度，显示内容
            System.out.println(new String(buf,0, readLength));
        }
//        关闭流和socket
        inputStream.close();
        socket.close();
        serverSocket.close();
    }
}
```

**SocketTCP01Client**

```java
package com.zhuang.Intelnet;

import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;

/**
 * @Classname SocketTCP01Client
 * @Description 客户端
 * @Date 2021/6/3 12:21
 * @Created by dell
 */

public class SocketTCP01Client {
    public static void main(String[] args) throws Exception{
        //思路
    //1. 连接服务端 (ip , 端口）
    //解读: 连接本机的 9999 端口, 如果连接成功，返回 Socket 对象
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
        System.out.println("客户端 socket 返回=" + socket.getClass());
    //2. 连接上后，生成 Socket, 通过 socket.getOutputStream()
    // 得到 和 socket 对象关联的输出流对象
        OutputStream outputStream = socket.getOutputStream();
    //3. 通过输出流，写入数据到 数据通道
        outputStream.write("hello, server".getBytes());
    //4. 关闭流对象和 socket, 必须关闭
        outputStream.close();
        socket.close();
        System.out.println("客户端退出.....");
    }
}
```



### 4.1.2 使用字节流(练习2)

[![21wWvT.png](https://z3.ax1x.com/2021/06/03/21wWvT.png)](https://imgtu.com/i/21wWvT)

**SocketTCP02Server**

```java
package com.zhuang.Intelnet;

import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * @Classname SocketTCP02Server
 * @Description 服务端
 * @Date 2021/6/3 12:20
 * @Created by dell
 */

public class SocketTCP02Server {
    public static void main(String[] args) throws Exception{
        ServerSocket serverSocket = new ServerSocket(9999);
        /*
        没有客户端连接9999端口会一直阻塞，等待连接
        如果有客户端连接，会返回Socket对象，程序继续
         */
        System.out.println("服务端在9999端口监听！");


        Socket socket = serverSocket.accept();
        System.out.println("服务端 socket="+socket.getClass());

        //通过socket.getInputStream()  读取客户端写入数据通信的数据，显示
        InputStream inputStream = socket.getInputStream();

        // IO读取
        byte[] buf=new byte[1024];
        int readLength = 0;
        while ((readLength=inputStream.read(buf))!=-1){
            //读取到的实际长度，显示内容
            System.out.println(new String(buf,0, readLength));
        }
        //5. 获取 socket 相关联的输出流
        OutputStream outputStream = socket.getOutputStream();
        outputStream.write("hello, client".getBytes());
        // 设置结束标记
        socket.shutdownOutput();


//        关闭流和socket
        inputStream.close();
        outputStream.close();
        socket.close();
        serverSocket.close();
    }
}
```

**SocketTCP02Client**

```java
package com.zhuang.Intelnet;

import java.io.InputStream;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;

/**
 * @Classname SocketTCP02Client
 * @Description 客户端
 * @Date 2021/6/3 12:21
 * @Created by dell
 */

public class SocketTCP02Client {
    public static void main(String[] args) throws Exception{
        //思路
    //1. 连接服务端 (ip , 端口）
    //解读: 连接本机的 9999 端口, 如果连接成功，返回 Socket 对象
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
        System.out.println("客户端 socket 返回=" + socket.getClass());
    //2. 连接上后，生成 Socket, 通过 socket.getOutputStream()
    // 得到 和 socket 对象关联的输出流对象
        OutputStream outputStream = socket.getOutputStream();
    //3. 通过输出流，写入数据到 数据通道
        outputStream.write("hello, server".getBytes());
        // 设置结束标记
        socket.shutdownOutput();
    //4. 获取和 socket 关联的输入流. 读取数据(字节)，并显示
        InputStream inputStream = socket.getInputStream();
        byte[] buf = new byte[1024];
        int readLen = 0;
        while ((readLen = inputStream.read(buf)) != -1) {
            System.out.println(new String(buf, 0, readLen));
        }
    //5. 关闭流对象和 socket, 必须关闭
        outputStream.close();
        inputStream.close();
        socket.close();
        System.out.println("客户端退出.....");
    }
}
```



### 4.1.3 使用字符流(练习3)

[![21whKU.png](https://z3.ax1x.com/2021/06/03/21whKU.png)](https://imgtu.com/i/21whKU)

**SocketTCP03Server**

```java
package com.zhuang.Intelnet;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * @Classname SocketTCP03Server
 * @Description 服务端
 * @Date 2021/6/3 12:47
 * @Created by dell
 */

public class SocketTCP03Server {
    public static void main(String[] args) throws Exception{
        ServerSocket serverSocket = new ServerSocket(9999);
        /*
        没有客户端连接9999端口会一直阻塞，等待连接
        如果有客户端连接，会返回Socket对象，程序继续
         */
        System.out.println("服务端在9999端口监听！");
        
        Socket socket = serverSocket.accept();
        System.out.println("服务端 socket="+socket.getClass());

        //2. 连接上后，生成 Socket, 通过 socket.getOutputStream()
        //3. 通过 socket.getInputStream() 读取客户端写入到数据通道的数据, 显示
        InputStream inputStream = socket.getInputStream();
        //4. IO 读取, 使用字符流, 老师使用 InputStreamReader 将 inputStream 转成字符流
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
        String s = bufferedReader.readLine();
        System.out.println(s);

        //5. 获取 socket 相关联的输出流
        OutputStream outputStream = socket.getOutputStream();

        // 使用字符输出流的方式回复信息
        BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(outputStream));
        bufferedWriter.write("hello client 字符流");
        bufferedWriter.newLine();// 插入一个换行符，表示回复内容的结束
        bufferedWriter.flush();//注意需要手动的 flush


        bufferedWriter.close();
        bufferedReader.close();
        socket.close();
        serverSocket.close();//关闭
    }
}
```

**SocketTCP03Client**

```java
package com.zhuang.Intelnet;

import java.io.*;
import java.net.InetAddress;
import java.net.Socket;

/**
 * @Classname SocketTCP03Client
 * @Description 客户端
 * @Date 2021/6/3 12:47
 * @Created by dell
 */

public class SocketTCP03Client {
    public static void main(String[] args) throws IOException{
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
        System.out.println("客户端 socket 返回=" + socket.getClass());
        //2. 连接上后，生成 Socket, 通过 socket.getOutputStream()
        // 得到 和 socket 对象关联的输出流对象
        OutputStream outputStream = socket.getOutputStream();
        //3. 通过输出流，写入数据到 数据通道, 使用字符流
        BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(outputStream));
        bufferedWriter.write("hello, server 字符流");
        bufferedWriter.newLine();//插入一个换行符，表示写入的内容结束, 注意，要求对方使用 readLine()!!!!
        bufferedWriter.flush();// 如果使用的字符流，需要手动刷新，否则数据不会写入数据通道
        //4. 获取和 socket 关联的输入流. 读取数据(字符)，并显示
        InputStream inputStream = socket.getInputStream();
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
        String s = bufferedReader.readLine();
        System.out.println(s);
        //5. 关闭流对象和 socket, 必须关闭
        bufferedReader.close();//关闭外层流
        bufferedWriter.close();
        socket.close();
        System.out.println("客户端退出.....");
    }
}
```



### 4.1.4 发送图片(练习4)

> - 编写一个服务端，和一个客户端
> - 在8888端口监听
> - 连接到服务端 发送一张图片
> - 服务端接收到 客户端发送的图片 保存到指定目录下，发送“收到图片”后退出
> - 客户端接受到服务端发送“收到图片” 再退出

**使用到一个工具类 StreamUtils**



```java
package com.zhuang.Intelnet;

import java.io.BufferedReader;
import java.io.ByteArrayOutputStream;
import java.io.InputStream;
import java.io.InputStreamReader;

/**
 * @Classname StreamUtils
 * @Description 用一句话描述类的作用
 * @Date 2021/6/3 13:03
 * @Created by dell
 */

/**
 * 此类用于演示关于流的读写方法
 *
 */
public class StreamUtils {
    /**
     * 功能：将输入流转换成byte[]
     * @param is
     * @return
     * @throws Exception
     */
    public static byte[] streamToByteArray(InputStream is) throws Exception{
        ByteArrayOutputStream bos = new ByteArrayOutputStream();//创建输出流对象
        byte[] b = new byte[1024];
        int len;
        while((len=is.read(b))!=-1){
            bos.write(b, 0, len);
        }
        byte[] array = bos.toByteArray();
        bos.close();
        return array;
    }
    /**
     * 功能：将InputStream转换成String
     * @param is
     * @return
     * @throws Exception
     */

    public static String streamToString(InputStream is) throws Exception{
        BufferedReader reader = new BufferedReader(new InputStreamReader(is));
        StringBuilder builder= new StringBuilder();
        String line;
        while((line=reader.readLine())!=null){
            builder.append(line+"\r\n");
        }
        return builder.toString();

    }

}
```

**TCPFileUploadServer**

```java
package com.zhuang.Intelnet;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * @Classname TCPFileUploadServer
 * @Description 文件上传服务端
 * @Date 2021/6/4 9:25
 * @Created by dell
 */

public class TCPFileUploadServer {
    public static void main(String[] args) throws Exception{
        // 在本机监听8888端口
        ServerSocket serverSocket = new ServerSocket(8888);
        System.out.println("服务端在8888端口监听......");
        //等待连接
        Socket socket = serverSocket.accept();
        //读取客户端的数据 通过Socket得到输入流
        BufferedInputStream bis = new BufferedInputStream(socket.getInputStream());
        byte[] bytes = StreamUtils.streamToByteArray(bis);
        //将得到的数组写入指定路径
        String desFilePath = "f:\\2.jpg";
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(desFilePath));
        bos.write(bytes);
        bos.close();

        //向客户端回复 收到图片
        //通过socket 获取到输入流
        BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
        writer.write("收到图片");
        // 把内容刷新到数据通道
        writer.flush();
        //设置写入结束标记
        socket.shutdownOutput();

        socket.close();
        serverSocket.close();
        bos.close();
        bis.close();
        writer.close();
    }
}
```

**TCPFileUploadClient**

```java
package com.zhuang.Intelnet;

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.InputStream;
import java.net.InetAddress;
import java.net.Socket;

/**
 * @Classname TCPFileUploadClient
 * @Description 文件上传客户端
 * @Date 2021/6/4 9:26
 * @Created by dell
 */

public class TCPFileUploadClient {
    public static void main(String[] args) throws Exception{
        //客户端连接服务端8888，得到Socket对象
        Socket socket = new Socket(InetAddress.getLocalHost(), 8888);
        //创建读取磁盘的文件
        String filePath="f:\\1.jpg";
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(filePath));
        //数组就是FilePath所对应的字节数组
        byte[] bytes = StreamUtils.streamToByteArray(bis);

//        通过socket获取到输入流，将bytes数据发送给服务端
        BufferedOutputStream bos = new BufferedOutputStream(socket.getOutputStream());
        bos.write(bytes);
        bis.close();
        socket.shutdownOutput();

        /*
        接受从服务端回复的消息

         */
        InputStream inputStream = socket.getInputStream();
        //转成字符串
        String s = StreamUtils.streamToString(inputStream);
        System.out.println(s);


        inputStream.close();
        bos.close();
        socket.close();

    }
}
```



# 5. netstat 指令

[![2GKweP.png](https://z3.ax1x.com/2021/06/04/2GKweP.png)](https://imgtu.com/i/2GKweP)

**示意图**

[![2GlBo6.png](https://z3.ax1x.com/2021/06/04/2GlBo6.png)](https://imgtu.com/i/2GlBo6)

- 当客户端连接到服务端后，实际上客户端也是通过一个端口和服务端来进行通讯的，这个端口是TVP/IP来分配的，是随机的



# 6. UDP 网络通信编程

**基本介绍**

[![2G1CpF.png](https://z3.ax1x.com/2021/06/04/2G1CpF.png)](https://imgtu.com/i/2G1CpF)

**基本流程**

[![2G1nfO.png](https://z3.ax1x.com/2021/06/04/2G1nfO.png)](https://imgtu.com/i/2G1nfO)



**案例**

[![2G1lXd.png](https://z3.ax1x.com/2021/06/04/2G1lXd.png)](https://imgtu.com/i/2G1lXd)

**UDPReceiverA**

```java
package com.zhuang.Intelnet;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

/**
 * @Classname UDPReceiverA
 * @Description UDP 接收端
 * @Date 2021/6/4 10:04
 * @Created by dell
 */

public class UDPReceiverA {
    public static void main(String[] args) throws Exception {

        //1. 创建一个 DatagramSocket 对象，准备在9999接收数据
        DatagramSocket socket = new DatagramSocket(9999);
        //2. 构建一个 DatagramPacket 对象，准备接收数据
        byte[] buf = new byte[1024];
        DatagramPacket packet = new DatagramPacket(buf, buf.length);
        //3. 调用 接收方法, 将通过网络传输的 DatagramPacket 对象
        //   填充到 packet对象
        //当有数据包发送到 本机的9999端口时，就会接收到数据
        //   如果没有数据包发送到 本机的9999端口, 就会阻塞等待.
        System.out.println("接收端A 等待接收数据..");
        socket.receive(packet);

        //4. 可以把packet 进行拆包，取出数据，并显示.
        //实际接收到的数据字节长度
        int length = packet.getLength();
        //接收到数据
        byte[] data = packet.getData();
        String s = new String(data, 0, length);
        System.out.println(s);


        //===回复信息给B端
        //将需要发送的数据，封装到 DatagramPacket对象
        data = "好的, 明天见".getBytes();
        //说明: 封装的 DatagramPacket对象 data 内容字节数组 , data.length , 主机(IP) , 端口
        packet =
                new DatagramPacket(data, data.length, InetAddress.getLocalHost(), 9998);

        socket.send(packet);

        //5. 关闭资源
        socket.close();
        System.out.println("A端退出...");



    }
}
```

**UDPSenderB**

```java
package com.zhuang.Intelnet;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

/**
 * @Classname UDPSenderB
 * @Description 发送端 B ====> 也可以接收数据
 * @Date 2021/6/4 10:04
 * @Created by dell
 */

public class UDPSenderB {
    public static void main(String[] args) throws Exception{
        //1.创建 DatagramSocket 对象，准备在9998端口 接收数据
        DatagramSocket socket = new DatagramSocket(9998);

        //2. 将需要发送的数据，封装到 DatagramPacket对象
        byte[] data = "hello 明天吃火锅~".getBytes();

        //说明: 封装的 DatagramPacket对象 data 内容字节数组 , data.length , 主机(IP) , 端口
        DatagramPacket packet =
                new DatagramPacket(data, data.length, InetAddress.getLocalHost(), 9999);

        socket.send(packet);

        //3.=== 接收从A端回复的信息
        //(1)   构建一个 DatagramPacket 对象，准备接收数据
        byte[] buf = new byte[1024];
        packet = new DatagramPacket(buf, buf.length);
        //(2)    调用 接收方法, 将通过网络传输的 DatagramPacket 对象
        //   填充到 packet对象
        //当有数据包发送到 本机的9998端口时，就会接收到数据
        //   如果没有数据包发送到 本机的9998端口, 就会阻塞等待.
        socket.receive(packet);

        //(3)  可以把packet 进行拆包，取出数据，并显示.
        //实际接收到的数据字节长度
        int length = packet.getLength();
        //接收到数据
        data = packet.getData();
        String s = new String(data, 0, length);
        System.out.println(s);

        //关闭资源
        socket.close();
        System.out.println("B端退出");
    }
}
```

## 6.1 练习

### 6.1.1 作业1

[![2G1R9U.png](https://z3.ax1x.com/2021/06/04/2G1R9U.png)](https://imgtu.com/i/2G1R9U)

**Homework01Server**

```java
package com.zhuang.Intelnet.HomeWork;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * @Classname Homework01Server
 * @Description 服务端, 使用字符流方式读写
 * @Date 2021/6/4 10:51
 * @Created by dell
 */

public class Homework01Server {
    public static void main(String[] args) throws IOException {
        //思路
        //1. 在本机 的9999端口监听, 等待连接
        //   细节: 要求在本机没有其它服务在监听9999
        //   细节：这个 ServerSocket 可以通过 accept() 返回多个Socket[多个客户端连接服务器的并发]
        ServerSocket serverSocket = new ServerSocket(9999);
        System.out.println("服务端，在9999端口监听，等待连接..");
        //2. 当没有客户端连接9999端口时，程序会 阻塞, 等待连接
        //   如果有客户端连接，则会返回Socket对象，程序继续

        Socket socket = serverSocket.accept();

        //
        //3. 通过socket.getInputStream() 读取客户端写入到数据通道的数据, 显示
        InputStream inputStream = socket.getInputStream();
        //4. IO读取, 使用字符流, 老师使用 InputStreamReader 将 inputStream 转成字符流
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
        String s = bufferedReader.readLine();
        String answer = "";
        if ("name".equals(s)) {
            answer = "我是zk";
        } else if("hobby".equals(s)) {
            answer = "编写java程序";
        } else {
            answer = "你说的啥子";
        }


        //5. 获取socket相关联的输出流
        OutputStream outputStream = socket.getOutputStream();
        //    使用字符输出流的方式回复信息
        BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(outputStream));
        bufferedWriter.write(answer);
        bufferedWriter.newLine();// 插入一个换行符，表示回复内容的结束
        bufferedWriter.flush();//注意需要手动的flush


        //6.关闭流和socket
        bufferedWriter.close();
        bufferedReader.close();
        socket.close();
        serverSocket.close();//关闭

    }
}
```

**Homework01Client**

```java
package com.zhuang.Intelnet.HomeWork;

import java.io.*;
import java.net.InetAddress;
import java.net.Socket;
import java.util.Scanner;

/**
 * @Classname Homework01Client
 * @Description 客户端，发送 "hello, server" 给服务端， 使用字符流
 * @Date 2021/6/4 10:50
 * @Created by dell
 */

public class Homework01Client {
    public static void main(String[] args) throws IOException {
        //思路
        //1. 连接服务端 (ip , 端口）
        //解读: 连接本机的 9999端口, 如果连接成功，返回Socket对象
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);

        //2. 连接上后，生成Socket, 通过socket.getOutputStream()
        //   得到 和 socket对象关联的输出流对象
        OutputStream outputStream = socket.getOutputStream();
        //3. 通过输出流，写入数据到 数据通道, 使用字符流
        BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(outputStream));

        //从键盘读取用户的问题
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入你的问题");
        String question = scanner.next();

        bufferedWriter.write(question);
        bufferedWriter.newLine();//插入一个换行符，表示写入的内容结束, 注意，要求对方使用readLine()!!!!
        bufferedWriter.flush();// 如果使用的字符流，需要手动刷新，否则数据不会写入数据通道


        //4. 获取和socket关联的输入流. 读取数据(字符)，并显示
        InputStream inputStream = socket.getInputStream();
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
        String s = bufferedReader.readLine();
        System.out.println(s);

        //5. 关闭流对象和socket, 必须关闭
        bufferedReader.close();//关闭外层流
        bufferedWriter.close();
        socket.close();
        System.out.println("客户端退出.....");
    }
}
```



### 6.1.2 作业2
[![2G1ghT.png](https://z3.ax1x.com/2021/06/04/2G1ghT.png)](https://imgtu.com/i/2G1ghT)

**Homework02ReceiverA**

```java
package com.zhuang.Intelnet.HomeWork;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

/**
 * @Classname Homework02ReceiverA
 * @Description UDP接收端
 * @Date 2021/6/4 10:59
 * @Created by dell
 */

public class Homework02ReceiverA {
    public static void main(String[] args) throws IOException {
        //1. 创建一个 DatagramSocket 对象，准备在8888接收数据
        DatagramSocket socket = new DatagramSocket(8888);
        //2. 构建一个 DatagramPacket 对象，准备接收数据
        byte[] buf = new byte[1024];
        DatagramPacket packet = new DatagramPacket(buf, buf.length);
        //3. 调用 接收方法, 将通过网络传输的 DatagramPacket 对象
        //   填充到 packet对象
        System.out.println("接收端 等待接收问题 ");
        socket.receive(packet);

        //4. 可以把packet 进行拆包，取出数据，并显示.
        //实际接收到的数据字节长度
        int length = packet.getLength();
        //接收到数据
        byte[] data = packet.getData();
        String s = new String(data, 0, length);
        //判断接收到的信息是什么
        String answer = "";
        if("四大名著是哪些".equals(s)) {
            answer = "四大名著 <<红楼梦>> <<三国演示>> <<西游记>> <<水浒传>>";
        } else {
            answer = "what?";
        }


        //===回复信息给B端
        //将需要发送的数据，封装到 DatagramPacket对象
        data = answer.getBytes();
        //说明: 封装的 DatagramPacket对象 data 内容字节数组 , data.length , 主机(IP) , 端口
        packet =
                new DatagramPacket(data, data.length, InetAddress.getLocalHost(), 9998);

        socket.send(packet);

        //5. 关闭资源
        socket.close();
        System.out.println("A端退出...");

    }
}
```

**Homework02SenderB**

```java
package com.zhuang.Intelnet.HomeWork;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.util.Scanner;

/**
 * @Classname Homework02SenderB
 * @Description 发送端B ====> 也可以接收数据
 * @Date 2021/6/4 10:59
 * @Created by dell
 */

public class Homework02SenderB {
    public static void main(String[] args) throws IOException {

        //1.创建 DatagramSocket 对象，准备在9998端口 接收数据
        DatagramSocket socket = new DatagramSocket(9998);

        //2. 将需要发送的数据，封装到 DatagramPacket对象
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入你的问题: ");
        String question = scanner.next();
        byte[] data = question.getBytes();

        //说明: 封装的 DatagramPacket对象 data 内容字节数组 , data.length , 主机(IP) , 端口
        DatagramPacket packet =
                new DatagramPacket(data, data.length, InetAddress.getLocalHost(), 8888);

        socket.send(packet);

        //3.=== 接收从A端回复的信息
        //(1)   构建一个 DatagramPacket 对象，准备接收数据
        byte[] buf = new byte[1024];
        packet = new DatagramPacket(buf, buf.length);
        //(2)    调用 接收方法, 将通过网络传输的 DatagramPacket 对象
        //   填充到 packet对象
        socket.receive(packet);

        //(3)  可以把packet 进行拆包，取出数据，并显示.
        //实际接收到的数据字节长度
        int length = packet.getLength();
        //接收到数据
        data = packet.getData();
        String s = new String(data, 0, length);
        System.out.println(s);

        //关闭资源
        socket.close();
        System.out.println("B端退出");
    }
}
```

# 7. 多用户即时通讯

此项目是用来总结 `多线程`，`IO流`，`网络编程`所学的重要知识点

**先看项目 目录一览**

[![2szH4f.png](https://z3.ax1x.com/2021/06/09/2szH4f.png)](https://imgtu.com/i/2szH4f)

所包含的功能

1. 用户登录
2. 拉取在线用户列表
3. 无异常退出（客户端，服务端）
4. 私聊
5. 群聊
6. 发文件
7. 服务器推送新闻



## 7.1 功能实现-用户登录

先看示意图，看懂图才能了解功能如何实现！

[![2ypoff.png](https://z3.ax1x.com/2021/06/09/2ypoff.png)](https://imgtu.com/i/2ypoff)

**图解读**

**客户端**

- 和服务端通信时，使用对象方式，可以使用`对象流`来读写
- 当客户端连接到服务端，也会得到`socket`
- 启动一个线程，该线程持有`socket`
- 方便管理线程，将线程放入到集合



**服务端**

- 有客户端连接到服务器，得到一个`socket`
- 启动一个线程，持有该`socket`,且包含该线程所有属性
- 方便管理线程，需要使用集合管理



## 7.2 功能实现-私聊

[![2ypoff.png](https://z3.ax1x.com/2021/06/09/2ypoff.png)](https://imgtu.com/i/2ypoff)

**客户端**

- 接受用户希望给其他在线用户聊天的内容
- 将消息构成`Message`对象，通过对应的`socket`发送给服务器
- 在通信线程中，读取到发送的`message`消息，显示



**服务端**

- 可以从客户端读取到消息
- 从管理线程的集合中，根据`message`对象的`id`获取对应的`socket`
- 最后将`message`对象转发给指定用户



## 7.3 功能实现-发文件

[![2yP7p6.png](https://z3.ax1x.com/2021/06/09/2yP7p6.png)](https://imgtu.com/i/2yP7p6)

**客户端**

- 先把文件读取到客户端，存放在字节数组中
- 把对应的文件字节数组封装到`message`对(文件内容，sender,getter)
- 将`message`对象发送给服务端
- 在接收到文件的消息后，将该文件保存到磁盘



**服务端**

- 接收`message`对象
- 拆解`message`对象的`id`，获取用户的通信线程
- 将`message`转发给指定用户



## 7.4 代码实现

分析完看代码实现，关键方法都有注释

### 7.4.1 QQServe

**Message**

- 实现序列化接口

```java
package com.zhuang.Intelnet.QQServe.qqcommon;

import java.io.Serializable;

/**
 * @Classname Message
 * @Description 表示客户端和服务端通信时的消息对象
 * @Date 2021/6/8 12:39
 * @Created by dell
 */

public class Message implements Serializable {
    private static final long serialVersionUID = 1L;
    //发送者
    private String sender;
    //接受者
    private String getter;
    //消息内容
    private String content;
    //发送时间
    private String sendTime;
    //消息类型
    private String mesType;

    //进行拓展 和文件相关的成员
    private byte[] fileBytes;
    private int fileLen=0;
    //文件传输的目的地
    private String dest;
    //源文件路径
    private String src;

    public byte[] getFileBytes() {
        return fileBytes;
    }

    public void setFileBytes(byte[] fileBytes) {
        this.fileBytes = fileBytes;
    }

    public int getFileLen() {
        return fileLen;
    }

    public void setFileLen(int fileLen) {
        this.fileLen = fileLen;
    }

    public String getDest() {
        return dest;
    }

    public void setDest(String dest) {
        this.dest = dest;
    }

    public String getSrc() {
        return src;
    }

    public void setSrc(String src) {
        this.src = src;
    }

    public String getMesType() {
        return mesType;
    }

    public void setMesType(String mesType) {
        this.mesType = mesType;
    }

    public String getSender() {
        return sender;
    }

    public void setSender(String sender) {
        this.sender = sender;
    }

    public String getGetter() {
        return getter;
    }

    public void setGetter(String getter) {
        this.getter = getter;
    }

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }

    public String getSendTime() {
        return sendTime;
    }

    public void setSendTime(String sendTime) {
        this.sendTime = sendTime;
    }
}
```

**MessageType**

```java
package com.zhuang.Intelnet.QQServe.qqcommon;

/**
 * @Classname MessageType
 * @Description 表示消息类型
 * @Date 2021/6/8 12:40
 * @Created by dell
 */

public interface MessageType {
    /*
    接口中定义一些常量
    不同常量表示不同的消息类型
     */
    String MESSAGE_LOGIN_SUCCEED = "1"; //表示登录成功
    String MESSAGE_LOGIN_FAIL = "2"; // 表示登录失败
    String MESSAGE_COMM_MES = "3"; //普通信息包
    String MESSAGE_GET_ONLINE_FRIEND = "4"; //要求返回在线用户列表
    String MESSAGE_RET_ONLINE_FRIEND = "5"; //返回在线用户列表
    String MESSAGE_CLIENT_EXIT = "6"; //客户端请求退出
    String MESSAGE_TO_ALL_MES = "7"; //群发消息报
    String MESSAGE_FILE_MES = "8"; //文件消息(发送文件)

}
```

**User**

```java
package com.zhuang.Intelnet.QQServe.qqcommon;

import java.io.Serializable;

/**
 * @Classname User
 * @Description 表示一个用户/客户信息
 * @Date 2021/6/8 12:39
 * @Created by dell
 */

public class User implements Serializable {
    private static final long serialVersionUID = 1L;
    private String userId;//用户Id/用户名
    private String passwd;//用户密码

    public User(String userId, String passwd) {
        this.userId = userId;
        this.passwd = passwd;
    }

    public User() {
    }

    public String getUserId() {
        return userId;
    }

    public void setUserId(String userId) {
        this.userId = userId;
    }

    public String getPasswd() {
        return passwd;
    }

    public void setPasswd(String passwd) {
        this.passwd = passwd;
    }
}
```

**工具类 方便管理输入**

**Utility**

```java
package com.zhuang.Intelnet.QQServe.utils;

/**
 * @Classname Utility
 * @Description 工具类
 * @Date 2021/6/8 12:48
 * @Created by dell
 */

/**
 工具类的作用:
 处理各种情况的用户输入，并且能够按照程序员的需求，得到用户的控制台输入。
 */
import java.util.Scanner;

public class Utility {
    //静态属性。。。
    private static Scanner scanner = new Scanner(System.in);


    /**
     * 功能：读取键盘输入的一个菜单选项，值：1——5的范围
     * @return 1——5
     */
    public static char readMenuSelection() {
        char c;
        for (; ; ) {
            String str = readKeyBoard(1, false);//包含一个字符的字符串
            c = str.charAt(0);//将字符串转换成字符char类型
            if (c != '1' && c != '2' &&
                    c != '3' && c != '4' && c != '5') {
                System.out.print("选择错误，请重新输入：");
            } else {
                break;
            }
        }
        return c;
    }

    /**
     * 功能：读取键盘输入的一个字符
     * @return 一个字符
     */
    public static char readChar() {
        String str = readKeyBoard(1, false);//就是一个字符
        return str.charAt(0);
    }
    /**
     * 功能：读取键盘输入的一个字符，如果直接按回车，则返回指定的默认值；否则返回输入的那个字符
     * @param defaultValue 指定的默认值
     * @return 默认值或输入的字符
     */

    public static char readChar(char defaultValue) {
        String str = readKeyBoard(1, true);//要么是空字符串，要么是一个字符
        return (str.length() == 0) ? defaultValue : str.charAt(0);
    }

    /**
     * 功能：读取键盘输入的整型，长度小于2位
     * @return 整数
     */
    public static int readInt() {
        int n;
        for (; ; ) {
            String str = readKeyBoard(10, false);//一个整数，长度<=10位
            try {
                n = Integer.parseInt(str);//将字符串转换成整数
                break;
            } catch (NumberFormatException e) {
                System.out.print("数字输入错误，请重新输入：");
            }
        }
        return n;
    }
    /**
     * 功能：读取键盘输入的 整数或默认值，如果直接回车，则返回默认值，否则返回输入的整数
     * @param defaultValue 指定的默认值
     * @return 整数或默认值
     */
    public static int readInt(int defaultValue) {
        int n;
        for (; ; ) {
            String str = readKeyBoard(10, true);
            if (str.equals("")) {
                return defaultValue;
            }

            //异常处理...
            try {
                n = Integer.parseInt(str);
                break;
            } catch (NumberFormatException e) {
                System.out.print("数字输入错误，请重新输入：");
            }
        }
        return n;
    }

    /**
     * 功能：读取键盘输入的指定长度的字符串
     * @param limit 限制的长度
     * @return 指定长度的字符串
     */

    public static String readString(int limit) {
        return readKeyBoard(limit, false);
    }

    /**
     * 功能：读取键盘输入的指定长度的字符串或默认值，如果直接回车，返回默认值，否则返回字符串
     * @param limit 限制的长度
     * @param defaultValue 指定的默认值
     * @return 指定长度的字符串
     */

    public static String readString(int limit, String defaultValue) {
        String str = readKeyBoard(limit, true);
        return str.equals("")? defaultValue : str;
    }


    /**
     * 功能：读取键盘输入的确认选项，Y或N
     * 将小的功能，封装到一个方法中.
     * @return Y或N
     */
    public static char readConfirmSelection() {
        System.out.println("请输入你的选择(Y/N): 请小心选择");
        char c;
        for (; ; ) {//无限循环
            //在这里，将接受到字符，转成了大写字母
            //y => Y n=>N
            String str = readKeyBoard(1, false).toUpperCase();
            c = str.charAt(0);
            if (c == 'Y' || c == 'N') {
                break;
            } else {
                System.out.print("选择错误，请重新输入：");
            }
        }
        return c;
    }

    /**
     * 功能： 读取一个字符串
     * @param limit 读取的长度
     * @param blankReturn 如果为true ,表示 可以读空字符串。
     *                   如果为false表示 不能读空字符串。
     *
     * 如果输入为空，或者输入大于limit的长度，就会提示重新输入。
     * @return
     */
    private static String readKeyBoard(int limit, boolean blankReturn) {

        //定义了字符串
        String line = "";

        //scanner.hasNextLine() 判断有没有下一行
        while (scanner.hasNextLine()) {
            line = scanner.nextLine();//读取这一行

            //如果line.length=0, 即用户没有输入任何内容，直接回车
            if (line.length() == 0) {
                if (blankReturn) {
                    return line;//如果blankReturn=true,可以返回空串
                } else {
                    continue; //如果blankReturn=false,不接受空串，必须输入内容
                }
            }

            //如果用户输入的内容大于了 limit，就提示重写输入
            //如果用户如的内容 >0 <= limit ,我就接受
            if (line.length() < 1 || line.length() > limit) {
                System.out.print("输入长度（不能大于" + limit + "）错误，请重新输入：");
                continue;
            }
            break;
        }

        return line;
    }
}
```



**ManageClientThreads**

```java
package com.zhuang.Intelnet.QQServe.qqserver;

import java.util.HashMap;
import java.util.Iterator;

/**
 * @Classname ManageClientThreads
 * @Description 该类用于管理和客户端通信的线程
 * @Date 2021/6/8 12:50
 * @Created by dell
 */

public class ManageClientThreads {
    private static HashMap<String,ServerConnectClientThread> hm=new HashMap<String, ServerConnectClientThread>();

    //返回hm
    public static HashMap<String, ServerConnectClientThread> getHm(){
        return hm;
    }
    //将线程添加对象到hm集合
    public static  void addClientThread(String userId,ServerConnectClientThread serverConnectClientThread){
        hm.put(userId, serverConnectClientThread);
    }

    //根据userId，返回ServerConnectClientThread线程
    public static  ServerConnectClientThread getServerConnectClientThread(String userId) {
        return hm.get(userId);
    }

    //增加一个方法，从集合中，移除某个线程对象
    public static void removeServerConnectClientThread(String userId) {
        hm.remove(userId);
    }

    /*
    返回在线用户列表
 */
    public static String getOnlineUser(){
//        遍历集合
        Iterator<String> iterator = hm.keySet().iterator();
        String onlineUserList="";
        while (iterator.hasNext()){
            onlineUserList+=iterator.next().toString()+" ";
        }
        return onlineUserList;
    }
}
```



**SendNewsToAllService**

```java
package com.zhuang.Intelnet.QQServe.qqserver;

import com.zhuang.Intelnet.QQServe.qqcommon.Message;
import com.zhuang.Intelnet.QQServe.qqcommon.MessageType;
import com.zhuang.Intelnet.QQServe.utils.Utility;

import java.io.ObjectOutputStream;
import java.util.Date;
import java.util.HashMap;
import java.util.Iterator;

/**
 * @Classname SendNewsToAllService
 * @Description 用一句话描述类的作用
 * @Date 2021/6/8 14:23
 * @Created by dell
 */

public class SendNewsToAllService implements Runnable{
    @Override
    public void run() {
        while (true) {
            //为了多次推送新闻可以使用while循环
            System.out.println("输入服务器要推送的新闻/消息[输入exit表示退出服务线程]");
            String news= Utility.readString(100);
            if ("exit".equals(news)){
                break;
            }
            //构建一个消息，群发信息
            Message message=new Message();
            message.setSender("服务器");
            message.setMesType(MessageType.MESSAGE_TO_ALL_MES);
            message.setContent(news);
            message.setSendTime(new Date().toString());
            System.out.println("服务器推送消息给所有人 说"+news);

            //遍历当前所有的通信线程，得到socket,并发送message
            HashMap<String, ServerConnectClientThread> hm = ManageClientThreads.getHm();
            Iterator<String> iterator = hm.keySet().iterator();
            while (iterator.hasNext()){
                String onLineUserId = iterator.next().toString();
                try {
                    ObjectOutputStream oos
                            = new ObjectOutputStream(hm.get(onLineUserId).getSocket().getOutputStream());
                    oos.writeObject(message);
                }catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



**ServerConnectClientThread**

```java
package com.zhuang.Intelnet.QQServe.qqserver;

import com.zhuang.Intelnet.QQServe.qqcommon.Message;
import com.zhuang.Intelnet.QQServe.qqcommon.MessageType;

import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.net.Socket;
import java.util.HashMap;
import java.util.Iterator;

/**
 * @Classname ServerConnectClientThread
 * @Description 该类的一个对象和某个客户端保持通信
 * @Date 2021/6/8 12:51
 * @Created by dell
 */

public class ServerConnectClientThread extends Thread{

    private Socket socket;
    //连接服务端的id
    private String userId;

    public ServerConnectClientThread(Socket socket, String userId){
        this.socket = socket;
        this.userId = userId;
    }

    public Socket getSocket() {
        return socket;
    }

    @Override
    public void run() {
        label:
        while (true) {
            try {
                System.out.println("服务端和客户端" + userId + " 保持通信，读取数据中！");
                ObjectInputStream ois = new ObjectInputStream(socket.getInputStream());
                Message message = (Message) ois.readObject();
                //后面根据MessageType的类型做相应的业务处理
                switch (message.getMesType()) {
                    case MessageType.MESSAGE_GET_ONLINE_FRIEND: {
                        //显示客户端在线列表
                        System.out.println(message.getSender() + " 要在线用户列表");
                        String onlineUser = ManageClientThreads.getOnlineUser();
                    /*
                    返回message
                    构建一个Message对象 返回客户端
                     */
                        Message message2 = new Message();
                        message2.setMesType(MessageType.MESSAGE_RET_ONLINE_FRIEND);
                        message2.setContent(onlineUser);
                        message2.setGetter(message.getSender());
                        //返回给客户端
                        ObjectOutputStream oos = new ObjectOutputStream(socket.getOutputStream());
                        oos.writeObject(message2);
                        break;
                    }
                    case MessageType.MESSAGE_COMM_MES: {
                        //根据message获取id 然后得到对应线程
                        ServerConnectClientThread serverConnectClientThread
                                = ManageClientThreads.getServerConnectClientThread(message.getGetter());
                        //得到对应Socket的对象输出流，将message对象转发给指定的客户端
                        ObjectOutputStream oos
                                = new ObjectOutputStream(serverConnectClientThread.getSocket().getOutputStream());
                        oos.writeObject(message);
                        break;
                    }
                    case MessageType.MESSAGE_TO_ALL_MES:
                        //需要遍历 管理线程的集合 把所有的线程的socket得到 得到把message进行转发即可
                        HashMap<String, ServerConnectClientThread> hm = ManageClientThreads.getHm();
                        Iterator<String> iterator = hm.keySet().iterator();
                        while (iterator.hasNext()) {
                            //取出在线用户id
                            String onLineUserId = iterator.next().toString();
                            if (!onLineUserId.equals(message.getSender())) {
                                //排除群发消息的用户
                                //进行转发消息message
                                ObjectOutputStream oos
                                        = new ObjectOutputStream(hm.get(onLineUserId).getSocket().getOutputStream());
                                oos.writeObject(message);
                            }
                        }
                        break;
                    case MessageType.MESSAGE_FILE_MES: {
                        //根据getter id 获取到对应的线程 ，将message对象转发
                        ObjectOutputStream oos = new ObjectOutputStream(ManageClientThreads
                                .getServerConnectClientThread(message.getGetter()).getSocket().getOutputStream());
                        //转发
                        oos.writeObject(message);
                        break;
                    }
                    case MessageType.MESSAGE_CLIENT_EXIT:
                        //客户端退出
                        System.out.println(message.getSender() + " 退出");
                        //这个客户端从对应线程删除
                        ManageClientThreads.removeServerConnectClientThread(message.getSender());
                        socket.close();
                        break label;
                    default:
                        System.out.println("其他类型的message,暂时不处理");
                        break;
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```



**QQServer**

```java
package com.zhuang.Intelnet.QQServe.qqserver;

import com.zhuang.Intelnet.QQServe.qqcommon.Message;
import com.zhuang.Intelnet.QQServe.qqcommon.MessageType;
import com.zhuang.Intelnet.QQServe.qqcommon.User;

import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.concurrent.ConcurrentHashMap;

/**
 * @Classname QQServer
 * @Description QQ服务端
 * @Date 2021/6/8 14:21
 * @Created by dell
 */

public class QQServer {
    private ServerSocket ss = null;
    //创建一个集合，存放多个用户，如果是这些用户登录，就认为是合法
    //这里我们也可以使用 ConcurrentHashMap, 可以处理并发的集合，没有线程安全
    //HashMap 没有处理线程安全，因此在多线程情况下是不安全
    //ConcurrentHashMap 处理的线程安全,即线程同步处理, 在多线程情况下是安全
    private static ConcurrentHashMap<String, User> validUsers = new ConcurrentHashMap<>();
    //private static ConcurrentHashMap<String, ArrayList<Message>> offLineDb = new ConcurrentHashMap<>();

    static { //在静态代码块，初始化 validUsers

        validUsers.put("100", new User("100", "123456"));
        validUsers.put("200", new User("200", "123456"));
        validUsers.put("300", new User("300", "123456"));
        validUsers.put("400", new User("400", "123456"));
        validUsers.put("500", new User("500", "123456"));
        validUsers.put("600", new User("600", "123456"));

    }

    //验证用户是否有效的方法
    private boolean checkUser(String userId, String passwd) {

        User user = validUsers.get(userId);
        //过关的验证方式
        //说明userId没有存在validUsers 的key中
        if(user == null) {
            return  false;
        }
        //userId正确，但是密码错误
        if(!user.getPasswd().equals(passwd)) {
            return false;
        }
        return  true;
    }

    public QQServer() {
        //注意：端口可以写在配置文件.
        try {
            System.out.println("服务端在9999端口监听...");
            //启动推送新闻的线程
            new Thread(new SendNewsToAllService()).start();
            ss = new ServerSocket(9999);
            //当和某个客户端连接后，会继续监听, 因此while
            while (true) {
                //如果没有客户端连接，就会阻塞在这里
                Socket socket = ss.accept();
                //得到socket关联的对象输入流
                ObjectInputStream ois =
                        new ObjectInputStream(socket.getInputStream());

                //得到socket关联的对象输出流
                ObjectOutputStream oos =
                        new ObjectOutputStream(socket.getOutputStream());

                //读取客户端发送的User对象
                User u = (User) ois.readObject();
                //创建一个Message对象，准备回复客户端
                Message message = new Message();
                //验证用户 方法
                //登录通过
                if (checkUser(u.getUserId(), u.getPasswd())) {
                    message.setMesType(MessageType.MESSAGE_LOGIN_SUCCEED);
                    //将message对象回复客户端
                    oos.writeObject(message);
                    //创建一个线程，和客户端保持通信, 该线程需要持有socket对象
                    ServerConnectClientThread serverConnectClientThread =
                            new ServerConnectClientThread(socket, u.getUserId());
                    //启动该线程
                    serverConnectClientThread.start();
                    //把该线程对象，放入到一个集合中，进行管理.
                    ManageClientThreads.addClientThread(u.getUserId(), serverConnectClientThread);

                } else { // 登录失败
                    System.out.println("用户 id=" + u.getUserId() + " pwd=" + u.getPasswd() + " 验证失败");
                    message.setMesType(MessageType.MESSAGE_LOGIN_FAIL);
                    oos.writeObject(message);
                    //关闭socket
                    socket.close();
                }
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //如果服务器退出了while，说明服务器端不在监听，因此关闭ServerSocket
            try {
                assert ss != null;
                ss.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

**QQFrame**

```java
package com.zhuang.Intelnet.QQServe.qqframe;

import com.zhuang.Intelnet.QQServe.qqserver.QQServer;

/**
 * @Classname QQFrame
 * @Description 该类创建QQServer ,启动后台的服务
 * @Date 2021/6/8 14:20
 * @Created by dell
 */

public class QQFrame {
    public static void main(String[] args)  {
        new QQServer();
    }
}
```



### 7.4.2 QQClient

**UserClientService**

```java
package com.zhuang.Intelnet.QQClient.service;

import com.zhuang.Intelnet.QQServe.qqcommon.Message;
import com.zhuang.Intelnet.QQServe.qqcommon.MessageType;
import com.zhuang.Intelnet.QQServe.qqcommon.User;

import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.net.InetAddress;
import java.net.Socket;

/**
 * @Classname UserClientService
 * @Description 该类完成用户登录验证和用户注册等功能
 * @Date 2021/6/8 14:46
 * @Created by dell
 */

public class UserClientService {

    //因为我们可能在其他地方用使用user信息, 因此作出成员属性
    private User u = new User();
    //因为Socket在其它地方也可能使用，因此作出属性
    private Socket socket;

    //根据userId 和 pwd 到服务器验证该用户是否合法
    public boolean checkUser(String userId, String pwd) {
        boolean b = false;
        //创建User对象
        u.setUserId(userId);
        u.setPasswd(pwd);


        try {
            //连接到服务端，发送u对象
            socket = new Socket(InetAddress.getByName("127.0.0.1"), 9999);
            //得到ObjectOutputStream对象
            ObjectOutputStream oos = new ObjectOutputStream(socket.getOutputStream());
            //发送User对象
            oos.writeObject(u);

            //读取从服务器回复的Message对象
            ObjectInputStream ois = new ObjectInputStream(socket.getInputStream());
            Message ms = (Message) ois.readObject();
            //登录OK
            if (ms.getMesType().equals(MessageType.MESSAGE_LOGIN_SUCCEED)) {


                //创建一个和服务器端保持通信的线程-> 创建一个类 ClientConnectServerThread
                ClientConnectServerThread clientConnectServerThread =
                        new ClientConnectServerThread(socket);
                //启动客户端的线程
                clientConnectServerThread.start();
                //这里为了后面客户端的扩展，我们将线程放入到集合管理
                ManageClientConnectServerThread.addClientConnectServerThread(userId, clientConnectServerThread);
                b = true;
            } else {
                //如果登录失败, 我们就不能启动和服务器通信的线程, 关闭socket
                socket.close();
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
        return b;

    }

    //向服务器端请求在线用户列表
    public void onlineFriendList() {

        //发送一个Message , 类型MESSAGE_GET_ONLINE_FRIEND
        Message message = new Message();
        message.setMesType(MessageType.MESSAGE_GET_ONLINE_FRIEND);
        message.setSender(u.getUserId());

        //发送给服务器

        try {
            //从管理线程的集合中，通过userId, 得到这个线程对象
            ClientConnectServerThread clientConnectServerThread =
                    ManageClientConnectServerThread.getClientConnectServerThread(u.getUserId());
            //通过这个线程得到关联的socket
            Socket socket = clientConnectServerThread.getSocket();
            //得到当前线程的Socket 对应的 ObjectOutputStream对象
            ObjectOutputStream oos = new ObjectOutputStream(socket.getOutputStream());
            //发送一个Message对象，向服务端要求在线用户列表
            oos.writeObject(message);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //编写方法，退出客户端，并给服务端发送一个退出系统的message对象
    public void logout() {
        Message message = new Message();
        message.setMesType(MessageType.MESSAGE_CLIENT_EXIT);
        //一定要指定我是哪个客户端id
        message.setSender(u.getUserId());

        //发送message
        try {
            ObjectOutputStream oos =
                    new ObjectOutputStream(ManageClientConnectServerThread.getClientConnectServerThread(u.getUserId()).getSocket().getOutputStream());
            oos.writeObject(message);
            System.out.println(u.getUserId() + " 退出系统 ");
            //结束进程
            System.exit(0);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

**MessageClientService**

```java
package com.zhuang.Intelnet.QQClient.service;

import java.io.IOException;
import java.io.ObjectOutputStream;
import java.util.Date;

import com.zhuang.Intelnet.QQServe.qqcommon.Message;
import com.zhuang.Intelnet.QQServe.qqcommon.MessageType;

/**
 * @Classname MessageClientService
 * @Description 私聊实现
 * @Date 2021/6/8 14:48
 * @Created by dell
 */

public class MessageClientService {
    /**
     * @param content  内容
     * @param senderId 发送者
     */
    public void sendMessageToAll(String content, String senderId) {
        //构建message
        Message message = new Message();
        message.setMesType(MessageType.MESSAGE_TO_ALL_MES);//群发消息这种类型
        message.setSender(senderId);
        message.setContent(content);
        message.setSendTime(new Date().toString());//发送时间设置到message对象
        System.out.println(senderId + " 对大家说 " + content);
        //发送给服务端

        try {
            ObjectOutputStream oos =
                    new ObjectOutputStream(ManageClientConnectServerThread.getClientConnectServerThread(senderId).getSocket().getOutputStream());
            oos.writeObject(message);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * @param content  内容
     * @param senderId 发送用户id
     * @param getterId 接收用户id
     */
    public void sendMessageToOne(String content, String senderId, String getterId) {
        //构建message
        Message message = new Message();
        //普通的聊天消息这种类型
        message.setMesType(MessageType.MESSAGE_COMM_MES);
        message.setSender(senderId);
        message.setGetter(getterId);
        message.setContent(content);
        //发送时间设置到message对象
        message.setSendTime(new Date().toString());
        System.out.println(senderId + " 对 " + getterId + " 说 " + content);
        //发送给服务端

        try {
            ObjectOutputStream oos =
                    new ObjectOutputStream(ManageClientConnectServerThread.getClientConnectServerThread(senderId).getSocket().getOutputStream());
            oos.writeObject(message);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```

**FileClientService**

```java
package com.zhuang.Intelnet.QQClient.service;

import com.zhuang.Intelnet.QQServe.qqcommon.Message;
import com.zhuang.Intelnet.QQServe.qqcommon.MessageType;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

/**
 * @Classname FileClientService
 * @Description 文件传输类
 * @Date 2021/6/8 14:48
 * @Created by dell
 */

public class FileClientService {
    /**
     * @param src      源文件
     * @param dest     把该文件传输到对方的哪个目录
     * @param senderId 发送用户id
     * @param getterId 接收用户id
     */
    public void sendFileToOne(String src, String dest, String senderId, String getterId) {

        //读取src文件  -->  message
        Message message = new Message();
        message.setMesType(MessageType.MESSAGE_FILE_MES);
        message.setSender(senderId);
        message.setGetter(getterId);
        message.setSrc(src);
        message.setDest(dest);

        //需要将文件读取
        FileInputStream fileInputStream = null;
        //获取文件的长度 写入字节数组中
        byte[] fileBytes = new byte[(int) new File(src).length()];

        try {
            fileInputStream = new FileInputStream(src);
            //将src文件读入到程序的字节数组
            fileInputStream.read(fileBytes);
            //将文件对应的字节数组设置message
            message.setFileBytes(fileBytes);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //关闭
            if (fileInputStream != null) {
                try {
                    fileInputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        //提示信息
        System.out.println("\n" + senderId + " 给 " + getterId + " 发送文件: " + src
                + " 到对方的电脑的目录 " + dest);
        //发送
        try {
            ObjectOutputStream oos =
                    new ObjectOutputStream(ManageClientConnectServerThread.getClientConnectServerThread(senderId).getSocket().getOutputStream());
            oos.writeObject(message);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

**ClientConnectServerThread**

```java
package com.zhuang.Intelnet.QQClient.service;

import java.io.FileOutputStream;
import java.io.ObjectInputStream;
import java.net.Socket;
import com.zhuang.Intelnet.QQServe.qqcommon.Message;
import com.zhuang.Intelnet.QQServe.qqcommon.MessageType;

/**
 * @Classname ClientConnectServerThread
 * @Description 用一句话描述类的作用
 * @Date 2021/6/8 14:47
 * @Created by dell
 */

public class ClientConnectServerThread extends Thread {
    //该线程需要持有Socket
    private Socket socket;

    //构造器可以接受一个Socket对象
    public ClientConnectServerThread(Socket socket) {
        this.socket = socket;
    }

    //
    @Override
    public void run() {
        //因为Thread需要在后台和服务器通信，因此我们while循环
        while (true) {

            try {
                System.out.println("客户端线程，等待从读取从服务器端发送的消息");
                ObjectInputStream ois = new ObjectInputStream(socket.getInputStream());
                //如果服务器没有发送Message对象,线程会阻塞在这里
                Message message = (Message) ois.readObject();
                //注意，后面我们需要去使用message
                //判断这个message类型，然后做相应的业务处理
                //如果是读取到的是 服务端返回的在线用户列表
                if (message.getMesType().equals(MessageType.MESSAGE_RET_ONLINE_FRIEND)) {
                    //取出在线列表信息，并显示
                    //规定
                    String[] onlineUsers = message.getContent().split(" ");
                    System.out.println("\n=======当前在线用户列表========");
                    for (int i = 0; i < onlineUsers.length; i++) {
                        System.out.println("用户: " + onlineUsers[i]);
                    }

                } else if (message.getMesType().equals(MessageType.MESSAGE_COMM_MES)) {//普通的聊天消息
                    //把从服务器转发的消息，显示到控制台即可
                    System.out.println("\n" + message.getSender()
                            + " 对 " + message.getGetter() + " 说: " + message.getContent());
                } else if (message.getMesType().equals(MessageType.MESSAGE_TO_ALL_MES)) {
                    //显示在客户端的控制台
                    System.out.println("\n" + message.getSender() + " 对大家说: " + message.getContent());
                } else if (message.getMesType().equals(MessageType.MESSAGE_FILE_MES)) {//如果是文件消息
                    //让用户指定保存路径。。。
                    System.out.println("\n" + message.getSender() + " 给 " + message.getGetter()
                            + " 发文件: " + message.getSrc() + " 到我的电脑的目录 " + message.getDest());

                    //取出message的文件字节数组，通过文件输出流写出到磁盘
                    FileOutputStream fileOutputStream = new FileOutputStream(message.getDest(), true);
                    fileOutputStream.write(message.getFileBytes());
                    fileOutputStream.close();
                    System.out.println("\n 保存文件成功~");

                } else {
                    System.out.println("是其他类型的message, 暂时不处理....");
                }

            } catch (Exception e) {
                e.printStackTrace();
            }
        }

    }

    //为了更方便的得到Socket
    public Socket getSocket() {
        return socket;
    }
}
```

**ManageClientConnectServerThread**

```java
package com.zhuang.Intelnet.QQClient.service;

import java.util.HashMap;

/**
 * @Classname ManageClientConnectServerThread
 * @Description 线程客户端管理
 * @Date 2021/6/8 14:48
 * @Created by dell
 */

public class ManageClientConnectServerThread {
    //我们把多个线程放入一个HashMap集合，key 就是用户id, value 就是线程
    private static HashMap<String, ClientConnectServerThread> hm = new HashMap<>();

    //将某个线程加入到集合
    public static void addClientConnectServerThread(String userId, ClientConnectServerThread clientConnectServerThread) {
        hm.put(userId, clientConnectServerThread);
    }
    //通过userId 可以得到对应线程
    public static ClientConnectServerThread getClientConnectServerThread(String userId) {
        return hm.get(userId);
    }

}
```

**QQView**

```java
package com.zhuang.Intelnet.QQClient.views;

import com.zhuang.Intelnet.QQClient.service.FileClientService;
import com.zhuang.Intelnet.QQClient.service.MessageClientService;
import com.zhuang.Intelnet.QQClient.service.UserClientService;
import com.zhuang.Intelnet.QQServe.utils.Utility;

/**
 * @Classname QQView
 * @Description QQ客户端启动类
 * @Date 2021/6/8 14:52
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class QQView {
     //控制是否显示菜单
     private boolean loop = true;
    // 接收用户的键盘输入
    private String key = "";
    //对象是用于登录服务/注册用户
    private UserClientService userClientService = new UserClientService();
    //对象用户私聊/群聊.
    private MessageClientService messageClientService = new MessageClientService();
    //该对象用户传输文件
    private FileClientService fileClientService = new FileClientService();
    public static void main(String[] args) {
        new QQView().mainMenu();
        System.out.println("客户端退出系统.....");
    }

    //显示主菜单
    private void mainMenu() {

        while (loop) {

            System.out.println("===========欢迎登录网络通信系统===========");
            System.out.println("\t\t 1 登录系统");
            System.out.println("\t\t 9 退出系统");
            System.out.print("请输入你的选择: ");
            key = Utility.readString(1);

            //根据用户的输入，来处理不同的逻辑
            switch (key) {
                case "1":
                    System.out.print("请输入用户号: ");
                    String userId = Utility.readString(50);
                    System.out.print("请输入密  码: ");
                    String pwd = Utility.readString(50);
                    //需要到服务端去验证该用户是否合法
                    if (userClientService.checkUser(userId, pwd)) {
                        System.out.println("===========欢迎 (用户 " + userId + " 登录成功) ===========");
                        //进入到二级菜单
                        while (loop) {
                            System.out.println("\n=========网络通信系统二级菜单(用户 " + userId + " )=======");
                            System.out.println("\t\t 1 显示在线用户列表");
                            System.out.println("\t\t 2 群发消息");
                            System.out.println("\t\t 3 私聊消息");
                            System.out.println("\t\t 4 发送文件");
                            System.out.println("\t\t 9 退出系统");
                            System.out.print("请输入你的选择: ");
                            key = Utility.readString(1);
                            switch (key) {
                                case "1":
                                    //写一个方法，来获取在线用户列表
                                    userClientService.onlineFriendList();
                                    break;
                                case "2":
                                    System.out.println("请输入想对大家说的话: ");
                                    String s = Utility.readString(100);
                                    messageClientService.sendMessageToAll(s, userId);
                                    break;
                                case "3":
                                    System.out.print("请输入想聊天的用户号(在线): ");
                                    String getterId = Utility.readString(50);
                                    System.out.print("请输入想说的话: ");
                                    String content = Utility.readString(100);
                                    //编写一个方法，将消息发送给服务器端
                                    messageClientService.sendMessageToOne(content, userId, getterId);
                                    break;
                                case "4":
                                    System.out.print("请输入你想把文件发送给的用户(在线用户): ");
                                    getterId = Utility.readString(50);
                                    System.out.print("请输入发送文件的路径(形式 d:\\xx.jpg)");
                                    String src = Utility.readString(100);
                                    System.out.print("请输入把文件发送到对应的路径(形式 d:\\yy.jpg)");
                                    String dest = Utility.readString(100);
                                    fileClientService.sendFileToOne(src,dest,userId,getterId);
                                    break;
                                case "9":
                                    //调用方法，给服务器发送一个退出系统的message
                                    userClientService.logout();
                                    loop = false;
                                    break;
                            }
                        }
                    } else { //登录服务器失败
                        System.out.println("=========登录失败=========");
                    }
                    break;
                case "9":
                    loop = false;
                    break;
            }

        }

    }
}
```

### 7.4.3 简单测试一下

[![2y3neA.png](https://z3.ax1x.com/2021/06/09/2y3neA.png)](https://imgtu.com/i/2y3neA)
[![2y3udI.png](https://z3.ax1x.com/2021/06/09/2y3udI.png)](https://imgtu.com/i/2y3udI)
[![2y3eLd.png](https://z3.ax1x.com/2021/06/09/2y3eLd.png)](https://imgtu.com/i/2y3eLd)



**总结**

- 网络编程有难度，还需要多反复巩固
- 基础要扎实，项目才能熟练

