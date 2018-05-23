## 基本用途

### 地址转发
- 概念：不改变地址栏的显示；只产生一次网络请求；比较快
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
### 地址重写
- 概念：类似于3xx，重定向会产生两次请求；比较慢
- Rewrite配置项，用于指定虚拟服务器的路由重置
    - 子配置参数
        - if：对条件判断选择不同的配置，语法格式`if (condition) { ... }`
            - 逻辑判断
                ```
                if ($request_method = POST) {
                    return 404;
                }
                
                if ( $type ) { # 变量存在且不为0
                    # do something
                }
                ```
            - 正则判断
                - Nginx里面正则的条件：`~：大小写敏感；~*：大小写不敏感`
                ```
                if ( $request_uri ~* ^\/.*) {
                    rewrite /index.php/$1;
                }
                ```
            - 请求的文件是否存在
                - `-f`, `-d`,文件，文件夹是否存在
                - `-e`：文件或目录是否存在
                    ```
                    if ( !-e $request_name) {
                        rewrite ^(.*)$ /index.php/$1;
                        break;
                    }
                    ```
                - `-x`：文件是否可执行
        - break：跳出当前配置块
        - return：直接返回响应内容，无视后续的所有配置
            ```
            if ( !-e $request_name) {
                return 404;
            }
            ```
        - rewrite：用于对符合正则格式的请求，替换为另一种格式，结构：`rewrite regex replacement [flag]`
            - regex：相对URI对应的格式，不包括host地址
            - replacement：需要重定向的地址
            - flag：用于设置rewrite后对URI的处理方式
                - last：本location块中处理这一次，处理结果跳出给其他location块处理
                - break：本块中处理一次，并且跳出（不在其他块中处理）
                - redirect：302重定向，暂时重定向
                - permanent：301重定向，永久重定向

    - Rewrite全局变量
        - `$args`：所有的查询参数
        - `$host`：请求的主机名
        - `$request_method`：请求方式（GET, POST）
        - `$request_filename`：请求文件名
        - `$request_uri`：请求URI（文件名+参数）
        - `$scheme`：请求协议，http，https等
       
    - 应用：
        - 多域名
        - 防盗链：用Nginx自带的valid_referers指令对Referer头里面的值进行验证
            - `valid_referers none | blocked | server_names string ...;`
            - 参数释义：以上都是可以并存的
                - `none`: 头部不存在情况，加入屏蔽器
                - `blocked`: 头部不存在或者被删除/伪装，此时不以http://或者https://开头
                - `server_names`: 后面给定的string以外的，屏蔽
            - 例子：
                ```
                location ~* ^.+\.(jpg|png|gif) {
                    valid_referers none blocked server_names *.test.com;
                    if ($invalid_referer) {
                        rewrite ^/ http://test.com/assets/img/forbidden.jpg;
                    }
                ```
                
            
            