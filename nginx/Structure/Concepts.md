基础概念
---
1. connection
    - 概念：对tcp连接概念的一个抽象（OO）
    - 成员: socket（属性）, read/write(方法)
    - 每个子进程都有对应的connection_poll，通过nofile限定可创建的socket数量（也就是最大维持的连接数）
    - 对应结构:`ngx_connection_t`
2. request
    - 概念：对http请求的抽象
    - 对应结构:`ngx_http_request_t`
3. keepalive
4. pipe
5. lingering_close
