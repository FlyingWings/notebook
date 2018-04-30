# 模块化

## 模块类型
- 核心模块：进程管理，事件驱动等
- 标准HTTP：身份认证，处理网页编码
- 可选HTTP：gzip，ssl等
- 邮件服务
- 自定义

## Gzip
最常用的压缩模块
- 配置项
    - gzip on; 是否开启gzip(on|off)
    - gzip_buffers number size; 缓冲区的数量和大小(16, 64k)
    - gzip_comp_level level; 压缩级别(1-9, 从低到高, 一般6)
    - gzip_min_length length; 对小于该长度的，不进行压缩(1024, 1k)
    - gzip_types mime-type; gzip的文档类型，如text/plain image/* application/json
    - gzip_vary on; 是否发送Vary: Accept-Encoding的响应头，从而会在Content-Encoding头部里加上gzip

