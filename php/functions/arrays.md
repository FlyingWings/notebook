# 常用的数组函数


- array_fill_keys(Array $keys, mixed $value)
    - 给定一个数组作为keys，生成一个以这些数组成员作为key，$value作为value的数组
    - 例：
    
    ```lang=php
    $keys = ['a','b','c'];
    $value = [];
    var_dump(array_fill_keys($keys, $value));
    
    //array(3){
        ['a']=>array(0){}
        ['b']=>array(0){}
        ['c']=>array(0){}
    }
    ```
- array_map(callable $callback, array $array1[, array $...])
    - 对后续所有的数组，均应用给定的$callback(可以是函数，也可以是回调)
    - 例：
    