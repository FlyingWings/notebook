# Nginx架构

## 启动过程

1. daemon守护进程的形式启动（ /etc/init.d/nginx 或 service)
2. 启动单个master进程和多个worker进程（进程个数取决于配置文件中worker_processes，一般与服务器CPU核的数量相同）
3. master用于管理worker进程
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


