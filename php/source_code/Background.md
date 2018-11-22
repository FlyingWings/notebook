### PHP源码基础

#### 目录结构
- 根目录：设计方案等，如README之类的文档
- build： 源码编译，环境检查的文件
- ext：官方扩展目录，扩展的源码等
- main：核心文件
- Zend：Zend引擎的实现目录，脚本的词法语法解析，opcode的执行和扩展的实现机制等
- pear：PHP扩展与应用仓库
- sapi：服务器抽象层的代码，比如与mod_php, cgi, fastcgi和php-fpm的接口
- TSRM：线程安全资源管理器
- tests：测试脚本集合
- win32(windows的东西)


#### 常用代码（IN C)
- 宏定义
    - '##': 连接符，用于组合不同语言符号（可以是函数的参数，也可以是宏的变量，不过不能作为第一个或最后一格元素存在
    ```
    #define ZEND_FN(name) zif_##name
    // ZEND_FN(test) == zif_test
    ``` 
    用于作为代码生成器，以代码重用，减少代码密度
    
    - '#': 预处理运算符，将其后面的参数进行字符串化操作（转化成字符串）
    ```
    #define STR(x) #x
    // 将x变量转化为一个字符串型的x
    ```

- 宏定义中的`do-while`循环
    - 对复杂逻辑的整合(就是个花括号);防止某些if不带花括号的情况
    ```
    define TEST(a, b) do{ a++;b++;} while(0) 
    ```
    - 对于某些空操作，保留
    ```
    define VNC_FREE() do {} while(0)
    ```
    
- '#line' 预处理： 改变当前行号和文件名，编译器操作
    ```
    #line 838 "Zend/zend_language_scanner.c"
    ```
    
    
    
