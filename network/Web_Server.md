# Web服务器

# 虚拟主机
- 用于解决单台服务器挂载多个网站的问题
- 方式：在Host头部中知名访问的主机名或者域名的URI

# 代理，网关，隧道
- 代理服务器：  
    - 本质：处于服务器和客户端的中间，接受请求-》服务器，接受响应-》客户端
    - 特征：不改变请求URI，直接转发请求给目标服务器；可多个代理进行级联
    - 用途
        - 缓存代理：对请求的资源，如果在代理服务器上有合适的，未过期的缓存，则返回缓存，不再向目标服务器请求
        - 非透明代理：对请求/响应报文内容进行加工；不加工的叫透明代理
    

- 网关：  
    - 本质：转发其他服务器通信数据的服务器，本身会对请求进行处理
    - 类似于代理，但是网关可以提供非HTTP协议服务
    - 用途：
        - 连接数据库，加密查询(存疑@TODO)
- 隧道：  
    - 本质：对较远的客户端和服务器进行中转，并保持双方通信连接的应用程序[例如SSH隧道，建立在某个服务器和最终服务器之间]
    - 目的：建立私密痛到，进行安全通信；本身不解析HTTP请求