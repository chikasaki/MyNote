1. Java四种引用:
    - 强引用
        - 一般情况下使用的引用都是强引用，在任何情况下，只要
        该引用不是垃圾，都不会被GC
    - 软引用
        - `SoftReference<?>`来将我们想要变成软引用的引用放入其中
        - 通过`get`方法获取我们需要的引用
        - 在即将发生内存溢出时，会清理软引用
            - 注意，只是将`SoftReference`中的引用清除，而`SoftReference`这个
            类对象是一个强引用
    - 弱引用
        - `WeakReference<?>`来将想要变成弱引用的引用放入其中
        - 通过`get`方法获取我们需要的引用
        - 只要没有强引用指向其中对象，`WeakReference`中的引用就会被垃圾收集器
        当作垃圾回收
        - 使用:
            - ThreadLocal
            - WeakHashMap
    - 虚引用
        - `PhantomReference<?>`来将想要变成虚引用的引用放入其中
        - 放入`PhantomReference`中的引用不可以再次获取，使用`get`方法始终返回null
        - 作用: 在虚引用被回收时，给出一个信号通知；具体信号通知放在引用队列中
        - 使用场景:
            - 编写JVM时使用
            - 操作直接内存NIO的`DirectByteBuffer`一般会使用虚引用来声明
            - 直接内存不会被垃圾收集器回收，所以需要在`DirectByteBuffer`指为null时
            给出一个信号，让系统编程人员可以手动回收直接内存