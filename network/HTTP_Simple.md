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

    - GET
    
    - POST
    
    - PUT
    
    - DELETE
    
    - OPTIONS
    
    - HEADER
    
- 保持连接（长连接）

- 利用Cookie进行状态管理