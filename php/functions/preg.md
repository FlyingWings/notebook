# 常用的正则函数

- preg_replace:
    - 给定一个正则表达式（以字符组的形式）+一个需要替换的文本，返回结果
    - 例：
    ```lang=php
    $regex = "/(?<=: )([\w^*].+)/";
    $content = '
    1) 完全匹配: alexfu.cc
    2) 通配符: *.alexfu.cc
    3) 通配符在后: alexfu.*
    4) 正则表达式: ^alex.+$';
    
    $ma= preg_replace($regex, "`$1`", $content);
    var_dump($ma);
    
    结果：
    string(118) "
      1) 完全匹配: `alexfu.cc`
      2) 通配符: `*.alexfu.cc`
      3) 通配符在后: `alexfu.*`
      4) 正则表达式: `^alex.+$`"
    ```
    - 使用过程中，一般不涉及原形式的替换，直接进行即可；
    - 涉及原表达式中给定的字符组的，如样例中的`([\w^*].+)`, 需要对它两侧加括号的，则需要用()组成字符组，并依次用$1, $2来指定匹配出来的字符组
    
- preg_match_all:
    - 对给定的字符串进行正则匹配
    - 例：
    ```lang=php
    $regex = "/([a-z]+)(\.+)(\d+)/";
    $content = "aaa..123
    basfdsaio.1123e;"
    preg_match_all($regex, $content, $ma);
    var_dump($ma);
    
    结果：
    array(4) {
    [0]=>
    array(2) {
        [0]=>"aaa...123"
        [1]=>"ffff.123"
    }
    [1]=>
    array(2) {
        [0]=>
        string(3) "aaa"
        [1]=>
        string(4) "ffff"
    }
    ```
    - 匹配的结果数组的第一个成员为匹配得到的字符串
    - 第二个，第三个依次为每个字符串中，符合对应格式的子串, 类似于\1, \2的结果
    