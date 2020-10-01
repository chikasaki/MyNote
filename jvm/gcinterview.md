1. 系统CPU经常100%，如何调优?
    - 找出哪个进程cpu高(top)
    - 找出进程中的哪个线程占用高(top -Hp)
    - 导出该线程的堆栈信息(jstack)
    - 查找各个方法(栈帧)的消耗时间(jstack)
    - 工作线程占比高/GC线程占用高
    
2. 系统内存飙高，如何查找问题?
    - 到处堆内存(jmap)
    - 分析(jhat jvisualvm ...)
    
3. 实际工作的问题如何检测出OOM?
    - top指令检查进程
    - top -Hp 检查线程
    - jstack打印堆栈
        - 线程安全问题: waiting on <>，找持有锁线程
    - 使用`jmap -histo` 指令打印出堆的对象分配详细信息
        - 实际生产中一般不能使用`jamp -dump:format=b,file=xxx pid"指令，
        因为其执行占用CPU，会对用户线程的执行造成很大的影响