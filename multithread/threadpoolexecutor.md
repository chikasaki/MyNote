1. ThreadPoolExecutor:
    - `AbstractExecutorService`的子类，与`ForkJoinPool`号称两个线程池
    - 七个参数:
        - corePoolSize: 核心线程数，默认不会归还给OS的线程0
        - maximumPoolSize: 最大线程数，多出核心线程数的部分，在一定时间没有运行之后，会归还给OS
        - keepAliveTime: 规定的超时时间
        - unit: 超时时间单位
        - workQueue: 任务队列；
            - 核心线程有空闲/未创建满时，会将新来的任务直接交给核心线程
            - 当核心线程满了之后，会将新来的任务放入任务队列
            - 任务队列满了之后，且最大线程未满时，会新建线程并将新来任务将给新线程执行
            - 都满了之后，触发拒绝策略
        - threadFactory
            - `ThreadFactory`: 接口，可以通过自己实现来替代默认线程工厂
        - handler
            - 拒绝策略，jdk自带了四种，可以自己指定
                - AbortPolicy: 直接抛出异常，默认是这个
                - DiscardPolicy: 线程池直接抛弃该任务
                - DiscardOldestPolicy: 线程池抛弃任务队列中的最老任务
                - CallerRunsPolicy: 由提供任务的线程自己执行
                
2. 源码分析:
    - 基础变量:
        - ctl: 32位原子整形，高3位表示线程状态，低29位表示worker数量
    - 重要方法:
        - execute:
            - 当前已开启的线程数少于核心线程数，直接新建`worker`执行任务并返回
            - 当前线程池处于运行态并且任务队列未满，将当前任务放入任务队列中
            - 开启线程数 == 核心线程数，任务队列已满；或者线程池被关闭，
            尝试新建线程，创建失败则会采用拒绝策略拒绝该任务
        - addWorker:
            - cas改变workerCount
            - 检查状态，并将新的`worker`添加进`workers(HashSet)`
        - runWorker:
            - 加锁
            - 检测线程状态
            - 执行task
    - Worker类:
        - 继承AQS，实现Runnable接口
        - 任务单元，控制线程的执行