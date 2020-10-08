1. Disruptor原理:
    - 使用环形数组作为基础数据结构，构建生产消费队列
    - 维护`sequence`来判断当前使用的`slot`，多线程使用时
    只需要对`sequence`加锁即可
    
2. Disruptor使用:
    - 队列装载对象: Event
    - Event对象工厂: Disruptor在新创建时，会默认生成所有槽位的`Event`对象，
    保证效率
    - EventHandler: 事件处理器，即消费者；
        - 实现`onEvent`方法
        - 可提供多个`EventHandler`给`Disruptor`，默认一个`EventHandler`
        对应一个线程
    - EventTranslator:
        - 事件转换器，用来给生产者生产消息用的函数式接口
    - publishEvent(EventTranslator)
    