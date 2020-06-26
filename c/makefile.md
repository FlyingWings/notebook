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



### 实例
```
calc: main.c getch.c getop.c stack.c
    gcc -o calc main.c getch.c getop.c stack.c
``` 
1. 对象- calc: target（对象），该语句的目标，也是make的对象，同时一般也是输出对象（即命令需要生成的文件名）
2. 依赖关系表- 冒号后面的部分：依赖关系表（其中有一个发生变化就会触发第三部分）   
3. 命令- 下一行中以`tab`开始的gcc部分，执行对应的编译命令
