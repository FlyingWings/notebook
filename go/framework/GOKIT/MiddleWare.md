# 中间件
用于对指定的服务对象(struct)进行中间处理，并返回下层服务对象  
实际上就是对执行过程进行拦截和再处理的过程


- 结构：
    - 数据成员
        - 功能模块
        - 服务接口
        - 例子：
        ```
        type loggingMiddleware struct {
            logger log.Logger // 功能模块
            next StringService // 服务接口
        }
        ```
    - 函数成员
        - 对原先的服务中涉及的功能进行同名覆盖（相同参数和返回值）
        - 只是更改原先执行的顺序，在前/后加入触发器
        - 例：
        ```
        func (w loggingMiddleware) Count(s string) (n int) {
            defer func(begin time.Time) {
                _ = w.logger.Log(
                    "input", s,
                    "output", n,
                    "took", time.Since(begin)
                )
            }(time.Now())
            n = mw.next.Count(s)
            return n
        }
        ```
    