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
        char *key;
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
        - hash_lookup
        - hash_insert()
        - hash_remove()
        - hash_destroy()