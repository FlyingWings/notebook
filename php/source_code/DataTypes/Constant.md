### 常量
- 结构：
```
typedef struct _zend_constant {
    zval value;         // 值
    int flags;          // 内部标记，比如case_sensitive, 
    char *name;         // 常量名称
    uint name_len;  
    int module_number; // 模块号-》属于不同模块的编号
} zend_constant;
```
- 定义流程：
    - 判断大小写是否敏感
    - 值的复制和常量命名
    - 将常量注册到常量列表中
- flag:
    - 定义：内部标记
    - 类型：
        - CONST_CS：常量大小写敏感
        - CONST_PERSISTENT: 常量是否持久化，持久化常量只会在MSHUTDOWN阶段被释放；诸如内置常量，一些字符串，数字等
        - CONST_CT_SUBST： 在编译时可被替换，如TRUE，FALSE，NULL等
        
- 模块编号
    - 定义：常量的级别
    - 类型：
        - PHP_USER_CONSTANT： 用户定义常量-通过define()定义的
        - 标准常量：E_ALL, E_WARNING等，在Zend引擎注册时就会执行
        
- 魔术常量
    - 定义：一些需要在不同php文件里，随着当时情况会动态变化的常量
    - 类型
        - "__ FILE __"
        - "__ LINE __"
        - "__ DIR __"
        - "__ FUNCTION __"
        - "__ CLASS __"
        - "__ METHOD __"
        - "__ NAMESPACE __"
    - 特性：做词法解析时会自动将这些常量予以替换