1. Redis对象:
    1) 基本结构
        ````
            typedef struct redisObject {
                unsigned type: 4; //redis对象类型
                unsigned encoding: 4; //redis对象底层的数据结构实现
                void *ptr; //指向实现数据结构对象的指针
            } robj;
        ````
    2) Redis新建键值对的过程:
        - 创建key对应的对象，总是字符串对象，即REDIS_STRING
        - 创建value对应的对象，可以是Redis的五大基本类型
            - REDIS_STRING
            - REDIS_LIST
            - REDIS_HASH
            - REDIS_SET
            - REDIS_ZSET
    3) RedisObject的encoding属性:
        - REDIS_ENCODING_INT => long类型整数
        - REDIS_ENCODING_EMBSTR => embstr编码简单动态字符串
        - REDIS_ENCODING_RAW => 简单动态字符串
        - REDIS_ENCODING_HT => 字典
        - REDIS_ENCODING_LINKEDLIST => 双端链表
        - REDIS_ENCODING_ZIPLIST => 压缩列表
        - REDIS_ENCODING_INTSET =>整数集合
        - REDIS_ENCODING_SKIPLIST => 跳表+字典
    
2. REDIS_STRING:
    1) 实现数据结构:
        - REDIS_ENCODING_INT:
            
          ````
            1. 字符串对象可以被转化为long整型时，会采用这种方式进行存储
            2. 超出long整型范围的数据，会被转化成raw
          ````
        - REDIS_ENCODING_EMBSTR
        - REDIS_ENCODING_RAW
        - EMBSTR与RAW的对比:
            - EMBSTR中的SDS对象与redisObject一起分配内存，
            所以一般EMBSTR在redis中只存储39B长的字符串
            - RAW则是一般的分配策略，先对redisObject分配
            内存，再对其中的ptr指针分配SDS对象内存
    2) long double型的存储: 为一般的EMBSTR或RAW，要进行
    算数运算时，会先将其转化为long double类型，计算完毕将
    结果转化为EMBSTR或RAW存储
    3) 编码转换:
        - 对int执行一些操作，使之不再是int类型，则会将其变为raw
        - 对embstr进行任意操作，都会将其变成raw；因为redis没有为
        embstr编码的字符串对象提供任何修改操作，其是只读的

3. REDIS_LIST:
    1) 实现数据结构:
        - REDIS_ENCODING_ZIPLIST
          ````
            1. 列表对象保存的所有字符串元素的长度都小于64字节
            2. 列表对象保存的元素数量小于512个
            3. 通过 list-max-ziplist-value, list-max-ziplist-entries
            修改以上两个限制
          ````
        - REDIS_ENCODING_LINKEDLIST
     
4. REDIS_HASH
    1) 实现数据结构:
        - REDIS_ENCODING_ZIPLIST:
          ````
            存储方式:
                使用两个ziplist的entry来分别存储HASH的键，值
                | key1 | value1 | key2 | value2 | ... |
            采用ZIP_LIST的时机:
                1. 所有键值对的键值对应字符串长度都小于64字节
                2. 哈希对象保存的键值对数量小于512个
          ````
        - REDIS_ENCODING_HT:
          ````
            1. 字典中每一个键和值都是一个字符串对象
          ````
       
5. REDIS_SET:
    1) 实现数据结构:
        - REDIS_ENCODING_INTSET
          ````
            1. 集合对象保存的所有元素都是整数值
            2. 集合对象保存的元素个数不超过512个
          ````
        - REDIS_ENCODING_HT
          ````
            1. 哈希所有键为字符串对象
            2. 哈希所有值为NULL
          ````
      
6. REDIS_ZSET:
    1) 实现数据结构:
        - REDIS_ENCODING_ZIPLIST
          ````
            1. 集合保存的元素个数少于128个
            2. 集合所有元素成员长度小于64字节
          ````
        - REDIS_ENCODING_SKIPLIST
          ````
            1. skiplist + hash
            2. skiplist: 确保集合的有序性
            3. hash: 可以快速通过键查询到对应的score
          ````
          
7. Redis对象的垃圾回收机制: 引用技术
   ````
    typedef struct redisObject {
        int refcount;
        ...
    } robj;
   ````
   
8. 对象的空转时长: EXPIRE，缓存回收LRU算法等的依据
   ````
    typedef struct redisObject {
        unsigned lru: 22; //对象最后一次被命令程序访问的时间
    } robj;
   ````