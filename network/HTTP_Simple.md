# HTTP基础

- 用途：
    服务器和客户端之间的通信
    
- 特性：
    - 通信方式：请求-响应
    - 不保存状态（Stateless）[当然可以通过cookie]

- 请求/响应格式
    1. 请求格式
    2. 响应格式
```
i.请求   GET /index.html HTTP/1.1
        //请求行[请求方法, URI, 协议版本]
        --------------------------
        Host: test.com
        Connection: keep-alive
        Content-Type: text/html
        Content-Length: 1024
        //请求头,首部字段, header
        --------------------------
        key=value&test=1
        //请求内容
```
```
ii.响应  HTTP/1.1 200 OK
        //状态行[协议版本，状态代号，原因]
        --------------------------
        Date: 2018-01-xx xx:xx:xx GMT
        Content-Type: text/html
        //响应头，header
        --------------------------
        <html>
        ...
        //响应内容
```

- 基本动词(请求方法)

    - GET: 获得资源
    
    - POST: 推送资源
    
    - PUT：上传文件
    
    - DELETE：删除文件
    
    - OPTIONS：查询支持的方法
    
    - HEADER：类似GET，返回响应头部信息
    
- 连接机制
    - 建立连接-三次握手
        - SYN     ---->
        - SYN/ACK <----
        - ACK     ---->
    - 断开连接-四次握手
        - FIN <----
        - ACK ---->
        - FIN ---->
        - ACK <----

- 保持连接（长连接）
    - Connection: keep-alive
    - 用途：在三次握手建立连接之后，可以长久进行通信，  
    多次通信的过程中不需要断开-握手，仅需要一次握手-断开
    
    - 管线化：建立在长连接之上
        - 类似于请求队列化，可以一次性顺序发送多个请求，然后等待服务器逐个响应
        - 不需要发送之后等待，然后再发送（提高效率）
    
   

- 利用Cookie进行状态管理

    - 实现过程：
        1. 客户端首次请求
        2. 服务端响应头中加上Set-cookie(内容+地址+过期时间)
        3. 客户端保存Cookie-Id，并在第二次请求时将Cookie作为请求头加入
        
- 编码管理
    - 压缩内容
        - gzip
        - compress
        - deflate
        - identity(不编码)
    - 分块编码
        - 将实体主体分割，分块传输
    - 内容格式管理MIME(Discuss Later)
    - 返回部分内容：头部关键字Range, Status Code:206, Partial Content
    
- 内容协商机制
    - 根据头部某些字段，返回不同版本（语言）的内容
        - Accept
        - Accept-Encoding
        - Accept-Charset
    - 类型
        - 服务器驱动协商：根据header进行服务器处理
        - 客户端驱动协商：根据客户手动选择进行处理
        - 透明协商：上面两个加起来
