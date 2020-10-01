## 万能命令: `java -XX:+PrintFlagsFinal -version | grep/findstr xxx`
1. 常见垃圾回收期组合参数设定:
    - -XX:+UseSerialGC = Serial New + Serial Old
    - -XX:+UseParNewGC = ParNew + SerialOld(很少用，有些版本已被废弃)
    - -XX:+UseConc(urrent)MarkSweepGC = ParNew + CMS + Serial Old
    - -XX:+UseParallelGC = Parallel Scavenge + Parallel Old
    - -XX:+UseParallelOldGC = Parallel Scavenge + Parallel Old
    - -XX:+UseG1GC = G1
    
2. 常用内存区域设置指令:
    - -Xmn: 新生代大小设置 
    - -Xms: 堆最小大小
    - -Xmx: 堆最大大小
    
3. GC相关指令:
    - -XX:+PrintGC: 打印GC相关消息
    - -XX:+PrintGCDetails: 打印GC详细消息
    - -XX:+PrintGCTimeStamps: 打印GC时间戳
    - -XX:+PrintGCCause:
    
4. 测试时使用的远程连接指令(JMX):
    > java -Djava.rmi.server.hostname=192.168.17.11 
     > -Dcom.sun.management.jmxremote 
     > -Dcom.sun.management.jmxremote.port=11111 
     > -Dcom.sun.management.jmxremote.authenticate=false 
     > -Dcom.sun.management.jmxremote.ssl=false 
     > XXX

5. 其它查看指令:
    - -XX:+PrintCommandLineFlags: 打印出当前命令的所有设置
    - -XX:+PrintFlagsInitial: 默认参数值
    - -XX:+PrintFlagsFinal: 最终参数值
    