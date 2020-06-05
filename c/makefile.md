Makefile
---
### 定义
用于自动构建，编译源码并生成（实际上就是gcc流程的整合和简化）
### 结构(如例所示)
```
calc: main.c getch.c getop.c stack.c
    gcc -o calc main.c getch.c getop.c stack.c
``` 
1. 对象- calc: target（对象），该语句的目标
2. 依赖关系表- 冒号后面的部分：依赖关系表（其中有一个发生变化就会触发第三部分）   
3. 命令- 下一行中以`tab`开始的gcc部分，执行对应的编译命令
