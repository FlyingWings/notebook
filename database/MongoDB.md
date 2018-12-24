MongoDB
=======

### 查询优化
- 方法：`cursor().explain("executionStats")` [这里的cursor指的是一个查询语句的结果游标]
- 结果
    - queryPlanner: 查询策略
        - winningPlan: 所使用的查询策略（索引等）
            - stage: 具体策略：
                - COLLSCAN：collection scan-全表扫描
                - IXSCAN： index scan-索引扫描
                - FETCH: 
    - executionStats: 运行统计
    - serverInfo: 服务器信息