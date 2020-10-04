1. Callable
    - 可以带返回值的`Runnable`
    
2. Future
    - 装载着任务返回值的容器
    - 一般由线程池的`submit`方法返回，可以异步执行并使用其`get`
    方法获取任务执行结果
    
3. FutureTask
    - 实现了`RunnableFuture`接口，而`RunnableFuture`接口
    继承了`Runnable`接口和`Future`接口
    - 可以同时担任`Future`和`Runnable`
    - 将其作为`Runnable`传入线程或线程池时，运行其`run`方法
    时，会运行`FutureTask`内部经过封装的`Callable`的`call`方法，
    并将结果放到`FutureTask`中承载返回值的容器中
    
4. CompletableFuture
    - 一个管理任务的工具类
    - 通过`CompletableFuture.supplyAsync(Consumer)`提交一个
    带返回值的任务
    - 支持链式调用`thenApply`, `thenAccept`, ...
    - 可以对多个任务进行同步操作:
        - `CompletableFuture.allof(cf1, cf2, ...).join()`