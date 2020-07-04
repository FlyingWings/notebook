PHP扩展概念(以swoole为例)
---
1. 定义函数表: zend_function_entry
2. 定义扩展函数和函数参数
    - 工具：PHP_FE(函数名，参数类型)
    - 语法：
    ```shell script
    PHP_FE(swoole_strerror, arginfo_swoole_strerror)
    // swoole_strerror: 函数名
    // arginfo_swoole_strerror: 参数类型（需要额外定义）
   
    ```
3. 定义参数类型
    - 方法：ZEND_BEGIN_ARG_INFO_EX(参数名, 不用的, 是否返回引用, 必须参数数量)
    - 语法：
    ```shell script
    ZEND_BEGIN_ARG_INFO_EX(arginfo_swoole_strerror, 0, 0, 1)
       ZEND_ARG_INFO(0, errno)
       ZEND_ARG_INFO(0, error_type)
    ZEND_END_ARG_INFO()
    ```
    
4. 定义函数体：
    - 方法：PHP_FUNCTION(函数名)
    - 语法:
    ```shell script
    PHP_FUNCTION(swoole_strerror)
    {
       // do something
       RETURN_STRING("some thing")
    }
    ```