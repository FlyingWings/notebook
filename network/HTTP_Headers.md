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