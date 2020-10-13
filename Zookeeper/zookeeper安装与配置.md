1. 安装jdk
2. 下载解压zookeeper
3. 修改配置文件
    - zoo_sample.cfg -> zoo.cfg
    - 修改datadir为自定义路径，并在路径中添加`myid`文件
    - 添加server: server.1 = ip:port1:port2
        - port1: leader与follower的通信连接
        - port2: 建立连接时/选举leader时用的通信连接
4. 修改PATH变量
5. 运行 `zkServer.sh`，使用`start-foreground`可在前台执行
6. 使用`zkServer.sh status`检查机器所属角色(`leader or follower`)
7. 使用`zkCli.sh`连接进入客户端
    - 字段说明:
        - cZxid: 64位，前32位为版本号(选举), 后32位位事务id(写操作id)
        - mZxid: 某个节点的最后修改时刻，定义同cZxid
        - pZxid: 某个节点的最后创建的子节点的时刻，定义同mZxid
        - ephemeralOwner: 创建该临时节点的集群结点对应id号；如果创建
        的是永久节点，则值位0x0
        