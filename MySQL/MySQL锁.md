1. MyISAM锁:
    - 只有表锁
    - 分类:
        - 写锁: 阻塞其它任何读写
            - lock table tab_name write
            - 当前session可以进行读写，其它session不能进行任何读写
        - 读锁: 阻塞其它写，允许读
            - lock table tab_name read
            - 当前session可以进行读，不可以进行写；其它session可以读，不能写
    - MyISAM:
        - 查询前，自动给涉及的所有表加读锁
        - 更新前，自动给涉及的表加写锁
    - lock table tab_name read local:
        - 当前session获取读锁，并允许其它session进行写操作；
        - 其它session的修改结果，不会影响当前session的事务查询
        
2. InnoDB锁: 既支持表锁，也支持行锁
    - 共享锁: 与MyISAM的读锁基本一样
        - lock in share mode
    - 排他锁: 与MyISAM的写锁基本一样
        - for update
    - InnoDB默认给(update, delete, insert等)写操作
    自动加排他锁
    - 行锁具体实现:
        - 在带数据的索引上，加锁
        - 只有涉及到索引的查询，才会上行锁
        - 其它查询，默认都是上表锁