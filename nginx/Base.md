# Nginx基础

- 特性
    - 静态文件处理
    - 高扩展性
    - 低内存消耗
    - 热部署
    - 反向代理+负载均衡
    - 支持fastcgi
    - 模块化的结构
    - 支持SSL

- M/S结构，减少I/O阻塞延迟

- 模块成熟，扩展容易

## 依赖：
- GCC
- PCRE库（用于支持正则）
- zlib（内置gzip压缩）
- openssl（https, sha1, md5）

## 相对应的内核参数优化
- fs.file-max = 999999 # 打开文件最大值（限制最大连接数）
- net.tcp_tw_reuse = 1 # 允许将TIME-WAIT状态的socket重用于新的连接
- net.tcp_keepalive_time = 600 # keepalive启用时，TCP 发送keepalive信息的频率，默认7200,小一点可以清理无效连接
- net.tcp_max_syn_backlog = 1024 #TCP三次握手时接受SYN队列的最大长度，大一些可以保证Nginx繁忙时，Linux不至于丢失客户端的连接请求
- net.ipv4.ip_local_port_range 9000 65535 # 本地端口的取值范围