## 文件I/O
一切皆文件

### 文件，文件描述符，文件表
- 文件：一切信息，包括文本文件，一个设备，一个socket都算文件
- 文件描述符：非负整数，用于标记文件的信息，实际上是一个对用户透明的句柄
- 文件表：每个进程独有的文件信息表，包括打开的文件个数，打开文件的偏移量（当前读的位置）

### 文件操作
#### 打开文件
- 基本函数原型：`int open(const char *pathname, int flags, mode_t mode);`
- 参数解释：
    - pathname：文件名
    - flags：打开文件的选项，只读(r, 0), 只写(w, 1), 读写(x, 2)
    - mode：权限，如0755之类
- 具体流程
    - 根据传入的flags进行检查，并通过mode来生成新的flags
    - 转入内核态（以下）
    - 申请新的文件描述符(fd)+ 文件管理结构(file)
    - 将fd+file结合起来(install)
    
#### 关闭文件
利用fd来实现关闭文件
- 流程分析：
    - 获取当前进程文件表
    - 判断传入的fd是否合法，若合法则根据fd得到对应的文件管理结构指针
    - 清空指定的file
    - 释放fd（置为unused）
- 存在隐患：始终使用最小的fd，会导致隐患
    - 最近关闭的fd有可能与最新打开的fd重合
    
- 不关闭文件会造成的问题：
    - 不释放fd
        - 一般进程，不释放，退出后内核会自动关闭文件+释放内存
        - 常驻进程，不释放，则每次运行都会申请新的描述符&&扩展文件表，这过程里如果打开文件的个数超过系统限制，就会返回错误
        
    - 不释放file
        - 一直申请file配额，不释放，用完时会报错
    -解决方法：
        - lsof，定位进程id，查找哪些地方的打开文件没有关闭
        
#### 文件偏移

