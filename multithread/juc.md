1. ReentrantLock
    - 可重入锁
        - 通过`lock`上锁
        - 一定要在`finally`语句块`unlock`
    - 与`synchronized`相比:
        - `tryLock`: 可以尝试获取锁，并指定时间，而不会一直阻塞
        - `lockInterruptibly`: 尝试获取锁时，可以响应中断
        - 可以设置公平与非公平

2. CountDownLatch
    - 一次性门闩，初始化时可以指定门闩数量
    - 通过`await`等待所有门闩打开
    - 通过`countdown`打开一个门闩

3. CyclicBarrier
    - 循环栅栏，
        - 初始化时可以指定推倒栅栏数量所需要的线程数
        - 可指定推倒栅栏之后，进一步执行的任务
        - 可以循环使用
    - await: 当前栅栏前线程数量 + 1，到达指定线程数可继续向下执行

4. Phaser
    - 循环栅栏只有一道栅栏多个线程反复推，而`Phaser`则有多道栅栏
        - 可以理解成循环栅栏多道栅栏都是一样的，而`Phaser`多道栅栏
        可以有不同的动作
    - 初始化
        - 可以指定`parties`
        - 可以通过`bulkRegister`指定`parties`
    - 通过`arriveAndAwaitAdvance`方法或`arriveAndDeregister`方法
    可以通知`phaser`当前线程可以参与推倒栅栏
    - 重写`onArrive`方法，可以指定各个阶段推倒栅栏之后的动作(相当于
    `CyclicBarrier`初始化时传入的`Runnable`)；
        - `phase`: 当前处于哪个阶段
        - `registeredParties`: 当前有多少个线程注册了该阶段的推倒
        - 返回值: `true`，整个`phaser`使命完成 

5. ReadWriteLock
    - 初始化
        - `ReentrantReadWriteLock`
        - 获取读锁: `lock.readLock()`
        - 获取写锁: `lock.writeLock()`
    - 读锁之间可以共享读，与写锁互斥
    - 写锁和读锁/写锁之间互斥
    - 默认非公平

6. Semaphore
    - 初始化: 指定总共令牌数
    - `acquire`: 获取令牌，可以获取多个令牌
    - `release`: 释放令牌，可以释放多个令牌
    - 典型应用场景:
        - 限流: 比如收费站一次只能通过`n`辆车

7. Exchanger
    - 类似一个容器，一般只用在两个线程之间交换数据
    - 初始化
        - `Exchanger<?>()`: 可以指定泛型
    - `exchange(?)` 
        - 调用此方法时，线程会被阻塞
        - 当两个线程都调用此方法之后，会将彼此之间传入的值交换返回，
        并继续执行