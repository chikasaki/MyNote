1. Netty的IOBuffer:
    - ByteBuf: 对Jdk的`ByteBuffer`封装之后的产物
    - 可以同时进行读写
    - 常用方法:
        - buf.isReadable(): 是否可读(buffer空时不可读)
        - buf.readerIndex(): 当前读到的`ByteBuf`中的索引
        - buf.readbleBytes(): 当前剩余的可读的字节
        - buf.isWritable(): 是否可写(buffer满时不可写)
        - buf.writerIndex(): 当前写的话，开始写的位置
        - buf.writableBytes(): 剩余的可以写的字节位置
        - buf.capacity(): 当前字节数组的大小
        - buf.maxCapacity(): 字节数组可扩容到的最大大小
        - buf.isDirect(): 当前字节数组的空间，是否被分配在堆外
        
2. Netty client 的两种写法:
    - 基础写法:
        - group -> NioEventLoopGroup
        - client -> NioSocketChannel
        - group.register(client) 将fd丢入epoll的红黑树池子中
        - client.pipeline().addLast(handler) 注册事件逻辑
        - client.connet() //连接远程服务端
        - 该写数据写数据
        - 设置客户端阻塞
    - Bootstrap写法:
        - group -> NioEventLoopGroup
        - bs.group
        - .channel -> 设置要处理的socket类型，服务端为`NioServerSocketChannel`，
        这里是客户端，即`NioSocketChannel`
        - .handler() -> 传入注册事件逻辑
        - connect
        - 该写数据写数据
        - 设置客户端阻塞
   
3. Netty server 的两种写法:
    - 基础写法:
        - group -> NioEventLoopGroup
        - server -> NioServerSocketChannel
        - group.register(server) -> 将fd丢入epoll的红黑树池子中
        - server.pipeline().addLast(handler)
            - 作为服务端，一般会有多个客户端来连接
            - Netty提供了以下扩展:
                - 当对应客户端的`Handler`为单例时，必须加上`@Shrable`
                - 因为用户可能在`Handler`中写入各客户端之间相互独立的逻辑
        - server.bind() -> 绑定端口号
        - 设置服务端阻塞
    - Bootstrap 写法:
        - group -> NioEventLoopGroup 
        - group(group, group) -> 第一个group表示`BossGroup`，
        第二个表示`WorkerGroup`
        - channel() -> 这里传入`NioServerSocketChannel`
        - childHandler()
            - 这里，Bootstrap内部会调用自己的`AcceptHandler`
            - 需要自己提供`WorkerGroup`中的socket的事件处理逻辑
        - bind() -> 绑定端口号
        - 设置服务端阻塞