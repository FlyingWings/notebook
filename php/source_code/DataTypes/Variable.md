### 变量及数据类型
#### 变量(V5.x)
- 结构：
    - 名称：一般命名标准
    - 类型：
    - 值
- 存储结构
    - zval: 
    ```
    struct _zval_struct {
    	/* Variable information */
    	zvalue_value value;		/* value */
    	zend_uint refcount__gc; // 引用计数，为0时会被GC清除
    	zend_uchar type;	/* 变量的具体类型 */
    	zend_uchar is_ref__gc; // 是否为引用变量
    };
    ```
    - value: 存储的值
        - zend_value:
        ```
        typedef union _zvalue_value {
        	long lval;					// 整型
        	double dval;				// 浮点数
        	struct {
        		char *val;
        		int len;
        	} str;                      // 字符串
        	HashTable *ht;				// hash表类型，比如array
        	zend_object_value obj;      // 对象类型, obj
        	zend_ast *ast;              // @todo
        } zvalue_value;
        ```
        为一个union，由于变量只能为一个类型，为了节约资源
    - type: 变量类型，如ZVAL_BOOL, ZVAL_LONG, ZVAL_NULL等
    
- HashTable： 核心存储结构
    - 常用概念：
        - key: 索引
        - slot： 存放数据的一个单元
        - hash function：用于计算hash位置的函数
        - hash collision：hash碰撞，需要解决
    - 冲突解决方法：
        - 链接法：链表保存冲突的数据，导致hash退化成链表；PHP中限制提交字段数量来解决（但是这一方法避免不了json_decode这种，来自于外源json数据）
        - 开放寻址法：遇到冲突时，顺位找到下一个槽；查找同上
        
    - 数据结构：
    ```
    typdef struct _Bucket
    {
        char *key; // 对应的索引值，一个HashTable中唯一
        void *value;
        struct _Bucket *next;
    } Bucket;
    typedef struct _HashTable
    {
        int size;           // 容量，可扩容
        int elem_num;       // 元素数量
        Bucket** buckets;   // 一层指针保存hashtable结构，二层指针保存链接表
    } HashTable; 
    ```
    
    - 操作接口
        - hash_init()
            - 初始容量，元素数量，分配buckets空间
        - hash_lookup()
            - 计算hash值，找到对应的hash位置
            - 在指定hash位置对应的链表中依次往后查找，最快O(1), 最慢O(n) [hash冲突的碰撞]
        - hash_insert() 复杂度O(1)
            - 检查hash后的位置是否为空，不为空则继续；
            - 为空则新建一个元素，将原来的链表加到这个元素的尾部
            - 发生碰撞时，新元素永远最前(Stack)
        - hash_resize() 复杂度[O(n)]
            - 当元素数量达到上限时（size容量），将原来的HashTable扩充为原来的两倍
            - 对原来所有的元素（按hash值遍历），每个链表里拆出来依次进行插入操作
            - 等价于重新hash计算（原来同个链表的元素可能就分散到不同的hash值里面）
        - hash_remove() 复杂度O(1) - O(n)
        - hash_destroy() 

- PHP中的哈希表（以上的优化）
    - 定义：双向链表+哈希表
    - 结构：
    ```
    typedef struct _hashtable { 
        uint nTableSize;        // hash Bucket的大小，最小为8，以2x增长。
        uint nTableMask;        // nTableSize-1 ， 索引取值的优化
        uint nNumOfElements;    // hash Bucket中当前存在的元素个数，count()函数会直接返回此值 
        ulong nNextFreeElement; // 下一个数字索引的位置
        Bucket *pInternalPointer;   // 当前遍历的指针（foreach比for快的原因之一）
        Bucket *pListHead;          
        Bucket *pListTail;          
        Bucket **arBuckets;         // 存储hash数组
        dtor_func_t pDestructor;    // 在删除元素时执行的回调函数，用于资源的释放
        zend_bool persistent;       // 如果是持久化的，该数组可以在多个请求时间访问【使用OS的内存分配方法】
        unsigned char nApplyCount; // 标记当前hash Bucket被递归访问的次数（防止多次递归）
        zend_bool bApplyProtection;// 标记当前hash桶允许不允许多次访问，不允许时，最多只能递归3次
    #if ZEND_DEBUG
        int inconsistent;
    #endif
    } HashTable;
    ```
    - 方法：
        - init：设置初始HashTable容量大小，默认为8（根据初始元素的数量，给出合适的容量[2的X次方]
           - 例：
           ```
           $array = ['1'x10]; 则$array的size为16
           ``` 
        -    
    - 基本单元: bucket
        - 结构:
        ```
        typedef struct bucket {
            ulong h;            // 对char *key进行hash后的值，或者是用户指定的数字索引值
            uint nKeyLength;    // hash关键字的长度，如果数组索引为数字，此值为0
            void *pData;        // 指向value，一般是用户数据的副本，如果是指针数据，则指向pDataPtr
            void *pDataPtr;     //如果是指针数据，此值会指向真正的value，同时上面pData会指向此值
            struct bucket *pListNext;   // 整个hash表的下一元素
            struct bucket *pListLast;   // 整个哈希表该元素的上一个元素
            struct bucket *pNext;       // 存放在同一个hash Bucket内的下一个元素
            struct bucket *pLast;       // 同一个哈希bucket的上一个元素
            // 保存当前值所对于的key字符串，这个字段只能定义在最后，实现变长结构体
            char arKey[1];              
        } Bucket;
        ```
        - 解释：
            - 可以被转换成数组的索引key，会当做数字处理
            - pListNext, pListLast 指向的是整个Hash表里面的上下(按hash值), 故而每个元素都会保存自己的直接上下级（不同链表中），以及同hash上下级
            - pNext, pLast 指向的是同一个hash值链表里面的上下级
            
- PHP中的链表
    - 定义：双向链表
    - 略
