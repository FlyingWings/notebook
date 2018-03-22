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

# 状态码
提示服务器返回的信息的类型
- 基本类型

| |类别 |原因短语|
|-------|-------|-----|
|1xx|信息性|请求正在被处理[队列加入?]
|2xx|成功|正常处理
|3xx|重定向|请求地址被改到其他位置，需要额外操作
|4xx|客户端错误|服务器无法处理请求
|5xx|服务端错误|服务器处理出错

- 2xx 成功
    - 200 OK
    - 204 No Content  
   请求完成处理，返回的报文里不含实体的主题部分；比如204之后，浏览器显示的页面不发生更新  
   应用场景：记录访问请求(Web Tracking)
   
   - 206 Partial Content  
   请求完成处理，但只需要部分成分  
   应用场景：GET时加了Range，则返回中有Content-Range
   
- 3xx 重定向 ——浏览器对服务器的返回值需要额外处理
    - 301 Move Permanently  
    请求的URI所对应的资源已经被永远转移到了另一个位置，请去另一个位置访问  
    应用场景：域名重定向（对多个域名对应一个位置的情况）
    
    - 302 Found  
    临时性重定向，要求浏览器去新的URI访问
    
    - 303 See Other
    与上者类似，但是要求客户端要用GET
    
    - 304 Not Modified  
    允许访问，但不符合请求头中的条件，如If-Match, If-None-Match等
    
    - 307 Temporary Redirect（同302）
    
- 4xx 客户端错误 ——服务器无法处理请求
    - 400 Bad Request  
    请求报文中出现错误（请求的内容有问题）
    
    - 401 Unauthorized  
    请求未获授权
    
    - 403 Forbidden  
    请求访问拒绝，文件权限（例如非apache用户访问系统），访问权限等  
    实测Nginx在文件权限不允许时，给出403
    
    - 404 Not Found  
    请求的文件在路径上不存在，也可以作为服务器端拒绝请求的理由
    
- 5xx 服务器错误 ——服务器接受了请求，但本身出现问题
    - 500 Internal Server Error 
    服务器执行的时候出问题，可能是代码出错或临时故障
    - 502 Bad Gateway
    作为网关，接收到错误的上流服务器信息  
    
    - 503 Service Unavailable
    服务不可用，可能是某些暂时的问题，突发维护等
    - 504 Gateway Timeout 网关错误


# HTTP报文首部
- 格式`字段名: 字段值`，字段值可以有多个，以`,`分割，如`Keep-Alive: timeout=15, max=100`

- 基本类型
    - 通用首部（请求/响应均含有）
        - Cache-Control：控制缓存的行为
        - Date：报文创建时间
        - Via: 代理服务器的信息
        - Connection：连接的管理
        
    - 请求首部
        - Accept（请求的媒体类型）
        - Accept-Language（优先的语言）
        - Accept-Encoding（优先的编码）
        - Host（请求的资源域名）
        - If-Match（比较实体标记）
        - Range（请求的实体的字节范围）
        - User-Agent（客户端的相关信息）
        - Referer（请求的URI的来源者，比如进行3xx跳转的时候会标记）
    
    - 响应首部
        - Location（客户端重定向的位置）
        - Server（HTTP服务器的安装信息）
    
    - 实体首部（针对实体部分使用，补充了资源更新时间等）
        - Content-Length（实体长度）
        - Content-Type（资源类型）
        - Expires（资源过期日期）