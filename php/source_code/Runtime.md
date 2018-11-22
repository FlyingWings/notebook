### PHP代码的执行

- 生命周期
    1. 词法分析
    2. 语法分析
    3. Zend Engine执行
    4. 输出

- SAPI运行流程
    - 流程：
        - 全局变量和常量初始化
        - Zend引擎和核心部件初始化
        - 解析`php.ini`
        - 全局操作函数的初始化
            - 对一些全局变量
        - 模块初始化：MINIT函数，一次执行中只进行一次
        - 激活Zend引擎：垃圾收集机制，异常和错误处理
        - 激活SAPI：对于不同类型的请求内容进行处理（不同的SAPI环境下会有不同处理）
        - 环境变量初始化：比如$_GET, $_POST等
        - 模块激活：RINIT函数，在请求阶段进行；每个请求过来都会激活一次；对每个扩展，模块等予以激活
        - 处理请求
        - DEACTIVATION： 
            - 执行析构函数
            - 刷出缓冲池
            - 发送response header
            - 销毁全局变量，关闭内存管理等
        - flush：清理缓冲区
        - 关闭Zend引擎
        - 模块停用：RSHUTDOWN函数
        - 模块关闭：MSHUTDOWN函数
    
    - 多进程环境：每个子进程分别开始结束MINIT-MSHUTDOWN
    - 多线程环境：一次MINIT，多次RINIT-执行-RSHUTDOWN，一次MSHUTDOWN

- SAPI: 服务器抽象层的实现约定，在sapi下有多种实现
    - 函数定义：
        - startup()
        - activate()
        - shutdown()
        - flush()
        - read_cookie()
        - send_header()
    - 实现接口
            - apache2handler
            - cgi
            - cli
            - fpm
@todo: 暂时还有一些需要后期理解的

- 脚本执行流程
    - 方式：运行时编译：在执行时进行编译（解释）
    - 流程：
        1. 词法分析：将代码切分为标记，并用语法分析器根据规则进行处理；
        2. 遇到对应的语法规则，Zend引擎进行编译，处理为opcode并执行；
        3. 2的过程中遇到的循环包含等，循环执行编译和opcode处理
    - 词法分析    
    - opcode: 字节码，运行于Zend虚拟机上；
        - 细节：@TODO