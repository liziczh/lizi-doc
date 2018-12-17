# IO&NIO

## NIO流

NIO面向缓冲区的，基于通道的IO操作；（JDK1.4已产生）

| 缓冲区（Buffer）：存储数据； |
| ---------------------------- |
| 通道（Channel）：传输数据；  |

NIO与IO的区别：

| IO：基于流；阻塞式（每次只能操作一种流）；    |
| --------------------------------------------- |
| NIO：面向缓冲区，基于通道，选择器；非阻塞式； |

NIO将以更加高效的方式进行文件的读写操作；

 

### 缓冲区（Buffer）

缓冲区：一个用于特定基本类型数据的容器；

缓冲区作用：保存数据；进行数据读写；

Buffer常见实现：

   ByteBuffer，ShortBuffer，IntBuffer，LongBuffer，   FloatBuffer，DoubleBuffer，CharBuffer；无布尔型缓冲区。   

 

①建立缓冲区，分配容量：

ByteBuffer b = ByteBuffer.[allocate](mk:@MSITStore:C:\Users\admin\Desktop\JDK_API_1.6_zh_中文.CHM::/java/nio/ByteBuffer.html#allocate(int))(1024);

清空缓冲区：Clear()

②缓冲区属性

位置：position()；

限制：limit()；

容量：capacity()；

③读/写：get()；put()；

④ 读写模式切换：filp();

⑤标记：mark()；reset()；

 

非直接缓冲区：[allocate](mk:@MSITStore:C:\Users\admin\Desktop\JDK_API_1.6_zh_中文.CHM::/java/nio/ByteBuffer.html#allocate(int))(capacity)；

传输方式：拷贝方式；

内存位置：位于堆区；

特点：占用资源较少，容易被释放；But效率低；

直接缓冲区：[allocate](mk:@MSITStore:C:\Users\admin\Desktop\JDK_API_1.6_zh_中文.CHM::/java/nio/ByteBuffer.html#allocate(int))Direct(capacity)；

传输方式：内存映射；

内存位置：直接位于内存页；

特点：效率高；But分配资源消耗大，不易被回收；

应用：一般分配给易受基础系统的本机IO操作的大型；

 

### 通道（Channel）

通道作用：传输数据；

1.Java 为 Channel 接口提供的最主要实现类如下：

•FileChannel：用于读取、写入、映射和操作文件的通道。

•DatagramChannel：通过 UDP 读写网络中的数据通道。

•SocketChannel：通过 TCP 读写网络中的数据。

•ServerSocketChannel：可以监听新进来的 TCP 连接，对每一个新进来的连接都会创建一个 SocketChannel。

 

2.获取通道：

本地IO：调用getChannel()；

FileInputStream/FileOutPutStream 

RandomAccessFile

网络IO：

Socket

ServerSocket

DatagramSocket

获取通道的其他方式是：

Files类静态方法：newByteChannel() 获取字节通道（JDK1.7）

Channel类静态方法：open(Path path,OpenOpertion ... oo)（JDK1.7）

 

获取通道：

①本地IO获取通道：本地IO.getChannel()；

②打开通道FileChannle.open(Path path,OpenOpertion ... oo)

 

使用通道进行数据传输：

①使用通道+非直接缓冲区完成文件复制；

②使用直接缓冲区完成文件复制（内存映射文件）

③直接使用通道完成数据传输；

 

分散读取和聚集写入

①分散读取：read(ByteBuffer[] bufs)；

②聚集写入：write(Bytebuffer buf1)；

   **while**((inChannel.read(bufs)) != -1) {       **for**(ByteBuffer b : bufs) {           b.flip();           outChannel.write(b);           b.clear();       }   }   

 

NIO的非阻塞式网络通信：

空闲通道：多路复用；

网络通信的三要素：

```
          IP地址：可以唯一的定位到一台计算机

          端口号：可以唯一的定位到一个程序

          通信协议：TCP/IP  UDP

   阻塞式：

          客户端：

                 \1. 获取通道

                 \2. 分配指定大小的缓冲区

                 \3. 读取本地文件，并发送到服务端

                 \4. 关闭通道

          服务端：

                 \1. 获取通道

                 \2. 绑定连接

                 \3. 获取客户端连接的通道

                 \4. 分配指定大小的缓冲区

                 \5. 接收客户端的数据，并保存到本地

                 \6. 关闭通道

   非阻塞式：

          客户端

                 \1. 获取通道

                 \2. 切换非阻塞模式

                 \3. 分配指定大小的缓冲区

                 \4. 发送数据给服务端

                 \5. 关闭通道
```

                     

```
          服务端

            \1. 获取通道

                 \2. 切换非阻塞模式

                 \3. 绑定连接

                 \4. 获取选择器

                 \5. 将通道注册到选择器上, 并且指定“监听接收事件”

                 \6. 轮询式的获取选择器上已经“准备就绪”的事件

                 \7. 获取当前选择器中所有注册的“选择键(已就绪的监听事件)”

                 \8. 获取准备“就绪”的是事件

                 \9. 判断具体是什么事件准备就绪

                 \10. 若“接收就绪”，获取客户端连接

                 \11. 切换非阻塞模式

                 \12. 将该通道注册到选择器上

                 \13. 获取当前选择器上“读就绪”状态的通道

                 \14. 读取数据
```

 

选择器（Selector）

作用：针对非阻塞式IO通信；

打开选择器：Selector.open()；

将通道注册到选择器：

Selector.select()：查看选择器注册的通道数；

键：SelectionKey，状态值

值：Channel，通道