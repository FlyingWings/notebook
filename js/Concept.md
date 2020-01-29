 基础概念
====

### 变量
1. 变量以var开头，类型不限，可以是下划线或者$开头（尽量避免）
2. ECMAScript 的解释程序遇到未声明过的标识符时，用该变量名创建一个全局变量，并将其初始化为指定的值。【对于不用var 声明的变量而言，等价于全局变量，故而对于函数内部使用变量而言，默认为全局】
    ```
    var a =3 ;
    function p(){
        console.log(a);
    }
    ```
3. 变量类型：
    - 原始值：存在栈中，变量直接访问内存中位置【如果是原始类型一般直接存在栈中】
        - 原始类型：
            - null: typeof null === "object"
            - undefined
            - boolean
            - number
            - string
            - object: 对象或者是null
            - symbol
    - 引用值：存在堆中，变量保存的是指针
    
    
