# 基本数据结构类型

### Collection(Interface)
- List(Interface)
    - LinkedList(Class): 双向链表
    - ArrayList(Class): 动态递增数组（非同步）
    - Vector(Class): 动态扩容, 多线程同步
        ```
        当一个Iterator正开始遍历该Vector时，如果该Vector被另一个线程改变，则会跑出异常
        ```
        - Stack(栈，多线程同步)
- Set(里面的元素不重复)
    - SortedSet
    - HashSet
    - TreeSet

### Map
- HashTable
    - 用于Key的对象必须可以
        - hashCode()[计算存放的hash值, 默认是按位循环*31]
        - equals()[计算相等]
    - 线程安全（默认会维护线程锁）

- HashMap
    - k, v nullable(可为空)
    - 非线程安全（但是也可以通过语句来同步）
- TreeMap
- SortedMap
- WeakHashMap