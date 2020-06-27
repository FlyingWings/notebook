Makefile
---
### 定义
用于自动构建，编译源码并生成（实际上就是gcc流程的整合和简化）
### 要求
1. 文件名为Makefile
2. 语法里只认识Tab，不认识空格
### 结构(如例所示)
targets: prerequisites
    recipe
其中targets为任务对象，prerequisites为需要检查的成员，里面有变化就会触发recipe，recipe就是执行的脚本内容
### 语法
- 命令输出：默认会将recipe里面的内容都输出出来，而加@就会禁止输出，加-会忽略错误
- Phony(假的) Targets：定义的一些非文件名的targets，因为无法对比更新所以每次都会执行
    - 例如：
    ```
    all: echo 123
    maker:
        touch ex0.txt
    # 上述的all，maker就是targets
    ```
    - 可以用`.PHONY: all maker process` 来声明以下哪些targets是phony的，例如，clean，install，all
- 通配符语法
    - 对于依赖关系表：基本通配符都可用，*之类的
    - 对于命令
        - 可以用一些内置变量来指代依赖关系表中的内容
        - 变量 | 说明 | 解释
          --- | --- | ---
          $^ | 所有依赖关系  | 
          $@ | target，如果有多个就指代当前执行的那个
          $< | 第一个依赖关系
          $? | 需要更新的依赖关系（即不需要更新的不管）
          $+ | 所有以来，包括重复
              
    - 对于对象：
        - 通配某些格式，例如`%.png: %.svg`: 目标是所有png文件，监听svg变动
        - 例如: 
        ```
        a.%.b:
            @echo $* # %所匹配到的部分，带路径的话，路径会被前置
        ```
- 变量
    - 其实是宏定义
    - 所有类型都是string
        - name = 123
        - name ?= JEAN (setnx)
        - override name=DA (readonly)
        - name += Gre (字符串连接，默认空格分隔)
        - name3:=test; name4:= $(name3) contents # := 用于声明变量扩展（即该变量可在后面变量定义时自动扩展）
- 函数
    - 调用方式：$(函数名 参数1,参数2,参数3)
    - @echo $(wildcard *.md # 将参数变成文件路径
    - @echo $(patsubst %.markdown,%.md,$* $^) # 将.markdown转为.md格式
- 条件控制
    - 可以写在recipe里面，也可以写在外部做全局（比如外部流程控制等）
    - 例子：
    ```
    sport = tennis
    # 流程控制语句 (如if else 等等) 顶格写
    report:
    ifeq ($(sport),tennis)
        @echo 'game, set, match'
    else
        @echo "They think it's all over; it is now"
    endif
    # 还有 ifneq, ifdef, ifndef
    ```

### 实例
```
calc: main.c getch.c getop.c stack.c
    gcc -o calc main.c getch.c getop.c stack.c
``` 
1. 对象- calc: target（对象），该语句的目标，也是make的对象，同时一般也是输出对象（即命令需要生成的文件名）
2. 依赖关系表- 冒号后面的部分：依赖关系表（其中有一个发生变化就会触发第三部分）   
3. 命令- 下一行中以`tab`开始的gcc部分，执行对应的编译命令
