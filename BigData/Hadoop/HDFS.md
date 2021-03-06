HDFS(Hadoop Distributed File System)
1. HDFS存储模型 --- 文件块
    - 块: 以字节为基本单位，根据硬件I/O速度可动态安排
    - 不同文件可设置不同的块大小，但某文件的块大小一旦
    被确定将不能再被修改
    - 块拥有副本概念，并且所有副本之间并无主从概念；不同
    块副本将会被存储在不同机器上
    - HDFS块是只读的，若支持修改会引发“泛洪”现象
    
2. HDFS角色设计:
    - Name Node: JVM进程，所有Node的统领者，负责记账功能
    - Data Node: JVM进程，实际存储数据的节点
    - Client: 文件系统的客户端，一般安装在服务器上

3. HDFS架构设计
    ![](./images/hdfs架构.png)

4. HDFS Name Node 元数据持久化策略:
    - Edits Log: 记录日志，所有对文件系统操作的命令都会追加
    到日志尾；当时间很长，日志体量会很大，恢复时间长
    - FsImage: 镜像/快照，对某个时间点的数据进行复制，并滚动
    更新时点；在复制时会消耗机器CPU，并且丢失的数据比较多
    - Edits Log + FsImage: HDFS采用的方式，某个时点的FsImage
    加上直到下一个时间点前的Edits Log作为恢复源
    
5. HDFS 副本放置策略:
    - 第一个副本: 放置在客户端安装的那台Data Node上(如果是)；
    否则随机选择一台CPU占用率不高的机器
    - 第二个副本: 放置在与第一个副本放置位置不同机架的机器上
    - 第三个副本: 放置在与第二个副本放置位置同一个机架上
 
6. HDFS读写策略:
    - 读:
    ![](./images/hdfsRead.png)
        - 客户端访问Name Node，判断所读文件是否存在
        - Name Node判断文件存在，并返回所有副本的位置列表
        - 客户端读取副本列表，选取最近的Data Node进行数据读取
        - 由于数据分块，支持随机访问块数据
        - 通过Data Node返回的数据及响应块的校验和，可判断文件
        数据是否正确
    - 写:
    ![](./images/hdfsWrite.png)
        - 客户端访问Name Node，判断相应完整文件名是否已存在
        - Name Node记录文件对应的元信息，并为文件块安排相应
        的副本放置策略(是某个文件块？还是整个文件的所有块？)，
        并返回给客户端
        - 客户端通过返回的副本安排位置，组建管道；并以小包packet
        为单位进行数据管道传输
        - 相应副本存储完毕后，对应的Data Node在向Name Node发送
        的心跳信息中夹带确认信息，供Name Node使用

7. Hadoop的安装过程:
    - jdk安装
    - ssh安装与免密配置
        - ssh免密涉及到的非对称加密算法
    - 配置hadoop-env.sh，修改JAVA_HOME
    - 配置core-site，新增网页浏览GUI的URL配置
    - 配置hdfs-site，新增name，data，secondary的存储位置，
    和secondary name node 的网页浏览URL
    - `hdfs namenode -format`初始化hdfs
    - `start-dfs.sh`启动namenode，datanode
    - 使用`hdfs dfs`相关命令操作文件系统
    
8. HDFS解决对单点问题的解决:
    1. 单点问题可能导致的两个结果:
        
       ````
        i. 单点故障，导致整个集群不可用
        ii. 主单点压力过大，内存受限
       ````
    2. 单点故障的解决方案: HA
        
       ````
        i. 采用主备的思路，当主机挂掉时，使用备机顶上
        ii. 可能产生的问题: 
            - 主备的数据一致性问题
            - 主挂时，如何快速切换到备机
        iii. 问题的解决:
            - 数据一致性: CAP定理(一致，可用，分区容忍)
               主备之间使用一个可靠的中间件，如Journal
            - 主备切换: 借助ZK，类似分布式锁，PAXOS定理
       ````
    3. 单点压力过大的解决方案: 分片
        
       ````
        可以使用哈希算法，将整个目录树分成若干片不想交叉的片；
        将各片独立存在于某个文件夹，每个文件夹对应于一个Name Node
        具体可参考一致性哈希算法
       ````