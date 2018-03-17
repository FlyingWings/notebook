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
        //基本内容[请求方法, URI, 协议版本]
        --------------------------
        Host: test.com
        Connection: keep-alive
        Content-Type: text/html
        Content-Length: 1024
        //请求头, header
        --------------------------
        key=value&test=1
        //请求内容
```
```
ii.响应  HTTP/1.1 200 OK
        //响应结果[协议版本，请求代号，解释]
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

    - GET: 获得信息
    
    - POST: 推送信息
    
    - PUT：上传文件
    
    - DELETE：删除文件
    
    - OPTIONS：查询支持的方法
    
    - HEADER：类似GET，返回响应头部信息
    
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