## Nginx基本数据结构
#### 整型
- ngx_int_t: 有符号整型
- ngx_unit_t: 无符号整型
#### 字符串
- ngx_str_t
```
typedef struct {
    size_t len; # 长度
    u_char *data; # 字符串首成员地址
} ngx_str_t;
```
#### 链表
- ngx_list_t
```
struct ngx_list_part_t {
    void *elts; # 数组起始地址
    ngx_unit_t nels; # 已经使用了多少个元素（需要小于nalloc)
    ngx_list_part_t *next; # 下一个元素的地址
}
typedef struct {
    ngx_list_part_t *last; # 链表最后一个数组元素
    ngx_list_part_t part; # 链表首个数组元素
    size_t size; # 每个数组元素的空间大小
    ngx_unit_t nalloc; # 每个ngx_list_part_t的容量
    ngx_pool_t *pool; # 内存池对象
} ngx_list_t;
```


#### ngx_module_s: 模块的基本属性（基类）
- ctx_index: 给定该模块属于core, http, event, mail四者中的哪种
- index: 给定该模块在编译后的所有模块中的位置
- *ctx: 模块上下文，定义模块初始化过程中的阶段调用（里面都是函数指针）
- 回调函数