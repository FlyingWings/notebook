# 网络基础

## Web基础协议：HTTP
超文本传输协议
- 进化过程
    - 0.9
    - 1.0: 使用广泛
    - 1.1：主流，加入新操作等
    - 2.0

## 互联网基础：TCP/IP
核心：分层管理
- 应用层：即时通信提供的服务，FTP | DNS | SSH
    - DNS：域名解析

- 传输层：提供数据传输：TCP | UDP : TLS

    - TCP： 确保可靠性
    - UDP： 广播机制，不确保可靠

- 网络层：处理数据包，规定传输路线(traceroute之类的)：
    - IP： 负责IP寻址
    - ARP：解析IP为MAC地址（可能是直接地址也可能是路由中介）

- 数据链路层：  硬件层面的传输过程
![](../pics/protocols_tcp_ip.jpg)

## URI

标准格式： http://user:pass@example.com:80/index.html?ab=1#href=rpc
    
    http-> 协议名（可选）
    user-> 用户名（可选）
    pass-> 密码（可选）
    example.com-> 域名
    80->端口号（可选）
    index.html->请求路径
    ab=1->请求参数(GET)
    href=rpc->片段标识符，锚点
    
一般情况下，协议会自动给定访问端口，例如:
    
    http->80
    https->443
    ftp->21
    ssh->22




