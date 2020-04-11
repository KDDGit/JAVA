# Java IO

### Java中的流（IO/BIO、NIO、AIO）

**BIO** （Blocking I/O）：**同步阻塞**I/O模式，数据的读取写入必须阻塞在一个线程内等待其完成。**jdk1.0**开始，在java.io包。   
**NIO** （New I/O）：同时支持阻塞与非阻塞模式，但主要是使用**同步非阻塞**IO。**jdk1.4**开始，在java.nio包。    
**AIO** （Asynchronous I/O）：**异步非阻塞**I/O模型。**jdk1.7**开始，在java.nio包。   

#### 适用场景
**BIO**：适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，但程序直观简单易理解。   
**NIO**：适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。    
**AIO**：适用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持。    

注：    
**NIO** 加入了Buffer、Channel、Selector等概念。基于事件驱动的，采用了Reactor模式，它使用一个线程管理所有的socket通道，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O请求时才启动一个线程进行处理。它的特点是要不断主动地去询问数据有没有处理完，一般只适用于连接数目较大但连接时间短的应用，如聊天应用等。

**AIO** 引入异常通道的概念，采用了Proactor模式，简化了程序编写，一个有效的请求才启动一个线程，它的特点是先由操作系统完成后才通知服务端程序启动线程去处理，一般适用于连接数较多且连接时间长的应用。

### IO中的阻塞、非阻塞、同步、异步

注：**同步／异步讲的是被调用方；阻塞／非阻塞讲的是调用方**

同步和异步   
烧水问题    
１、传统水壶。同步。烧水时需根据水的沸腾程度来判断水有没有烧开   
２、电热水壶。异步。水烧开之后会通过声音提醒我们    
同步请求，A调用B，B的处理是同步的，在处理完之前他不会通知A，只有处理完之后才会明确的通知A。    
异步请求，A调用B，B的处理是异步的，B在接到请求后先告诉A我已经接到请求了，然后异步去处理，处理完之后通过回调等方式再通知A。    

阻塞和非阻塞    
１、阻塞：坐在水壶前面，等着水烧好。    
２、非阻塞：到客厅看电视等着水开。   
阻塞请求，A调用B，A一直等着B的返回，别的事情什么也不干。    
非阻塞请求，A调用B，A不用一直等着B的返回，先去忙别的事情了。    


### NIO的工作流程步骤：

１、首先是先创建ServerSocketChannel 对象，和真正处理业务的线程池
２、然后给刚刚创建的ServerSocketChannel 对象进行绑定一个对应的端口，然后设置为非阻塞
３、然后创建Selector对象并打开，然后把这Selector对象注册到ServerSocketChannel 中，并设置好监听的事件，监听 SelectionKey.OP_ACCEPT
４、接着就是Selector对象进行死循环监听每一个Channel通道的事件，循环执行 Selector.select() 方法，轮询就绪的 Channel
５、从Selector中获取所有的SelectorKey（这个就可以看成是不同的事件），如果SelectorKey是处于 OP_ACCEPT 状态，说明是新的客户端接入，调用 ServerSocketChannel.accept 接收新的客户端。
６、然后对这个把这个接受的新客户端的Channel通道注册到ServerSocketChannel上，并且把之前的OP_ACCEPT 状态改为SelectionKey.OP_READ读取事件状态，并且设置为非阻塞的，然后把当前的这个SelectorKey给移除掉，说明这个事件完成了
７、如果第5步的时候过来的事件不是OP_ACCEPT 状态，那就是OP_READ读取数据的事件状态，然后调用本文章的上面的那个读取数据的机制就可以了

### Netty的工作流程步骤：

１、创建 NIO 线程组 EventLoopGroup 和 ServerBootstrap。
２、设置 ServerBootstrap 的属性：线程组、SO_BACKLOG 选项，设置 NioServerSocketChannel 为 Channel，设置业务处理 Handler
３、绑定端口，启动服务器程序。
４、在业务处理 TimeServerHandler 中，读取客户端发送的数据，并给出响应

### NIO和Netty的工作模型对比

１、OP_ACCEPT 的处理被简化，因为对于 accept 操作的处理在不同业务上都是一致的。
２、在 NIO 中需要自己构建 ByteBuffer 从 Channel 中读取数据，而 Netty 中数据是直接读取完成存放在 ByteBuf 中的。相当于省略了用户进程从内核中复制数据的过程。
３、在 Netty 中，我们看到有使用一个解码器 FixedLengthFrameDecoder，可以用于处理定长消息的问题，能够解决 TCP 粘包读半包问题，十分方便。



