Bash基础
---
1. 用途：bash shell，用于编写命令行脚本
2. 基础语法
- 首行: `#!/bin/bash`，定义执行该脚本的程序
- 分割指令的方式：分号 / 换行
- 声明变量方式：`Variable="SomeThing"` (不能有空格)
- 使用变量：
    - 赋值或导出时，不用加`$`
    - 要使用变量的值，加`$`, 如 `echo $Variable` 或 `echo "$Variable"`
- 控制结构：
    - 判断
        - 实例
        ```bash
            if [[ $name == "Hi" ]]
            then 
                echo "Hi"
            else
                echo "Hello"
            fi
        ```
        - if 判断条件
            - 字符串相等: `if [[ "$s1" == "$s2" ]]`
            - 字符串不相等: `if [[ "$s1" != "$s2" ]]`
            - 字符串包含: `if [[ $s1 == *"$s2"* ]]`
            - 字符串正则匹配: `if [[ $s1 =~ [0-9]+ ]]` : 正则表达式不需要引号
        - 链式调用和取否调用
            - `&&`: 用于链式调用，前者成功则执行后者
            - `||`: 用于取否调用，前者不成功执行后者
        - 逻辑与 和 逻辑或
            - `if [ $name == "A" ] && [ $test == "B" ]` : 逻辑与之间需要方括号并列
    - 分支
        - 实例：
        ```
        case "$Variable" in 
            0) echo 123;;
            1) echo 234;;
            *) echo "else";;
        esac
        ```
    - 循环
        - 实例1：循环变量
        ```shell script
        for ((a=1; a <= 3; a++))
        do 
          echo $a
        done
        ```
        - 实例2：遍历指令结果
       ```shell script
        for Output in $(ls)
        do
            cat "$Output"
        done
        ```
    - 函数
        - 实例:
        ```shell script
        # 简单声明
        foo()
        {
            echo "Hello World"
            return 0
        }
        # 复杂声明
        function bar()
        {
            echo "bar"
            return 0
        }
        ``` 
        - 获取返回值：先调用该函数，然后用$?获取结果，例如：
        ```shell script
        bar() {
          return 123;
        }
        bar
        echo $?
        # 输出为123
        ```
        - 传参：函数体内部用$1 $2 ${11} 获取第X个参数
        ```
        bar() {
            echo "$1 $2 ${10}"
        }
        bar 1 alex
        # 输出传入的元素
        ``` 
- 上下文依赖：
    - 定义：bash运行时依赖其中指令的上下文（即可以或者某个指令的中间结果作为输入或输出）
    - 关键词：
        - 管道： ` A|B`: 将A指令的结果给B指令作为输入
        - 重定向: `>`，`<` 等，将执行结果重定向给目标指令，或者位置
        - 指令嵌套：通过`$()`的方式将指令进行嵌套执行，例如
        ```shell script
         echo "There $(ls -l | wc -l) Lines "
        ```
