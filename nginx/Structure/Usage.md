## 基本用途

#### 重定向

- upstream 配置块，与http，server同级，作用为将请求转发给配置块内的服务器
    - 语法格式: `upstream name { ... }`
    - 子配置块：server，用于设置组内服务器的信息，`server address [parameters];`
        - address: 服务器地址，IP +端口，或者是linux的socket，如`unix:/run/socket`
        - parameters: 配置参数，常见类型：
            - weight=number，负载均衡的权重，默认是1，越高就越重要
            - max_fails=number，设置一定时间范围内，最大错误次数，如果超过这个最大次数，则将其设为失效；默认为1;
            - fail_timeout=time，用途：1. 给定max_fails里的那个“一定时间范围” 2. 对已经失效的服务器，经过这段时间后恢复成有效；默认是10s
            - backup，标记为备用服务器，当上述服务器中出现失效时，启用这台服务器
            - down： 永久标记为失效服务器
            
    - 子配置参数
        - ip_hash: 实现会话保持功能：对于来源相同的请求，限定转发给一台服务器处理；除非服务器失效(down)
            - 用途： 保持客户端请求的会话(不用session共享，单台服务器独享session)
            - 限制：
                1. 不能当负载均衡用(上节的weight失效) 
                2. 当本台服务器不是处于最上游时，得到的请求都是上游服务器转发过来的，则所有请求都会由一台服务器处理（会崩）
        - keepalive number: number为该服务器组允许保持的空闲连接的上限(HTTP keepalive)
        - least_conn: 配置负载均衡用，首先考虑连接数最少的服务器，其次再根据权重来
        
    - 实例：
    ```
        upstream backend {
            keepalive 1024;
            # ip_hash; # 设置ip_hash;
            least_conn; # 设置最少连接
            server a.test.com weight=3;
            server b.test.com fail_timeout=10s max_fails=3;
            server c.test.com;
        }
    ```
#### 重写
- Rewrite配置项，用于