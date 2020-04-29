ES6语法
---
1. 前置工具
    - Babel：用于转换ES6-> ES5
2. 基础指令
    - let： 声明局部变量（代码块内有效）；不存在变量提升（使用时报Reference错误）；创造块级作用域
        - 变量区绑定【暂时性死区】：如果区块内有let指令声明变量，那么在声明前的调用都会报错
        - 
    - 块级作用域函数（函数闭包）
      ```
        (function(){
            if( false){
                function f() { // do xxx
                }
            }
        }());
      ```
3. 解构赋值
    - 数组： let [a, b, c] = [1, 2, 3]; // php中的list($a, $b) = [1,2];
    - 对象： let { foo, bar } = {foo :'aaa', bar: 'bbb'};
4. 字符串扩展
    - "\uxxxx": unicode字符
    - 