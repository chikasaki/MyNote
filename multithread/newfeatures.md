1. VarHandle:
    - 1.9版本之后新加的类，可直接操作二进制码
    - 本质是对一个对象/一块内存空间新增一个引用，这个
    引用可以直接操作这块区域，并使用CPU级别的原语
    - 使用:
        ````java
          VarHandle handle = MethodHandles.lookup()
                              .findVarHandle(
                              xxx.class, filedName, filed.class
                             )
        ````