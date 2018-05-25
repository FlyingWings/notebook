# MYSQL

### 基础架构——三层分层
- 应用层
    - 与客户端和中间层交互，建立连接，响应请求
- 逻辑层
    - 负责查询处理，事务，存储管理等
    - 查询优化器
    - 计划执行器
    - 事务管理器（确保原子化，执行事务）
    - 锁管理器（并发控制）
    - 日志管理器（数据持久化）
- 物理层
    - 实际存储的文件，日志等
    
### 查询执行过程分析
1. 连接MySQL
2. 鉴权
    - 判断是否允许连接
    - 若能连接，检查所发送的请求是否能被执行
3. 若有缓存，返回缓存
4. 查询优化器，优化语句，生成执行计划（如何执行）
5. 执行计划通过API从存储引擎处获取数据
6. 返回数据


### 连接管理
- 短连接
    - 每次通信时都需要建立连接，一系列操作执行完毕后，关闭连接
- 长连接
    - 节约开销
    - 一定时间不操作，server会主动关闭连接，此时需要重新连接）

- 连接池

