## Linux网络编程基础API
- socket地址API
- socket基础API
- 网络信息API

### socket
- 声明参数：`int socket(int domain, int type, int protocal);`
    - domain: 使用的底层协议族
        - TCP/IP: PF_INET(IPV4), PF_INET6(IPV6)
        - UNIX本地域：PF_UNIX
    - type: 服务类型
        - SOCK_STREAM: 流服务：用TCP
        - SOCK_UGRAM: 数据报：用UDP
    - protocol: 选择具体协议，一般不用选，默认0
    
    - 成功时返回socket fd, 失败返回-1
    
- 命名socket（绑定）
    - 方法：`int bind(int sockfd, const strct sockaddr* my_addr, socketlen_t addrlen);`
        - sockfd: socket的文件描述符
        - my_addr: 分配地址，并给定socket地址的长度
    - 返回值： 
        - 0: 成功
        - EACCESS： 被绑定的地址受保护（如绑定的是0~1023端口）
        - EADDRINUSE：地址被使用ing

- 监听socket
    - 方法：`int listen(int sockfd, int backlog);`
        - sockfd
        - backlog: 内核监听队列的最大长度
    