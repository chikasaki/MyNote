1. Redis-value字符串类型:
    - 可以使用的函数:
        - set: 设置key对应的value值；带`nx`表示`nonexist`, 即只有当key不存在时才会set，存在时不会覆盖
        - get: 获取key对应的value值
        - mget: 批量获取key对应的value值
        - mset: 批量设置key对应的value值
        - msetnx: 批量设置key对应的value值，并且这些key都不能存在；原子性操作
        - setrange(key offset value): 
            从key的offset开始，将key的后面部分换为value
        - getrange(key start end): 从key的start位置一直读到end位置，两者都包含
        - strlen
        - incr
            - incrby
            - incrfloat
        - decr
            - decrby
        - bitmap相关:
            - setbit
            - bitop
            - bitcount
            - bitpos
    - 函数直接搜就行，一些参数的理解:
        - start/end: 表示字节的索引
        - offset: setrange中的表示字节索引，在setbit中表示位索引(从左到右，从0开始)
    - redis-string的二进制安全:
        - 为了支持不同语言，redis的string统一将字符串看作字节流
        - `oject encoding key`的类型是用来加速计算的；一些整型在进行
        `incr`等操作时，需要检验，使用内部类型计算时不需要反复校验