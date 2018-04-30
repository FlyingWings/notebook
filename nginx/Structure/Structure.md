# Nginx架构

## 启动过程

1. daemon守护进程的形式启动（ /etc/init.d/nginx 或 service)
2. 启动单个master进程和多个worker进程（进程个数取决于配置文件中worker_processes，一般与服务器CPU核的数量相同）
3. master用于管理worker进程
    - 验证配置
    - 接收外界信号
    - 向worker发送信号
    - 监控worker状态
    - 调度不同worker进程的启动/关闭

4. worker用于处理请求+发送响应  
优点：单进程处理，不需要加锁，且独立不受影响，即使出问题也会仅仅影响一个进程，而服务不中断  
    - 启动时，master进程fork出N个worker进程，各自拥有listenfd（socket）
    - 在新连接到来时，listenfd进入可读状态，多个worker抢accept_mutex(互斥锁)
    - 抢到的worker进程，注册listenfd读事件，调用accept接受该连接
    - 读取+解析+处理+返回响应


5. Nginx平滑重启
- 重新加载配置文件
- 启动新的worker进程
- 结束老的worker进程（处理完请求后再退出）

6. Nginx并发机制——异步非阻塞

- 进程接受请求后，判断读写事件是否Ready
- 当Ready时，开始处理；没有，则将其放入event poll中


## 事件驱动模型
用于每个worker进程内部对请求的调度（实际上就是I/O复用）
- 组成结构：
    - 事件收集器：
        - 收集所有事件（用户操作，系统中断）
    - 事件发送器：
        - 将事件发送给事件处理器
    - 事件处理器：
        - 响应具体事件，处理
- Nginx中的处理形式：接受请求——放入请求列表——非阻塞I/O调用事件处理器

- 常用处理模型：
    - select：核心为轮询
        - 为所有的fd创建读，写，异常三个集合，每个集合里分别监控对应操作的fd状态
        - 对所有的集合里的fd轮询，如果有读/写/异常事件发生，则处理
    - poll
        - 类似select，只是不拆成三个集合，而是只对所有的fd建立一个集合，并且设置读/写/异常监控
        - 轮询唯一的集合，三个操作任一发生，则处理
    - epoll（最好）
        - 结构类似poll
        - 不主动维护集合，而是当有三个操作发生时，内核将这一操作通知进程，并处理
        - 1. 创建描述符事件列表，长度为N，并设置三个事件
        - 2. 将这些添加到内核的事件列表中
        - 3. 发生事件时，内核通知进程进行处理