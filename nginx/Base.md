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
ulimit -a: 查看所有配置项目

需要修改的是其中的open-files：   
/etc/security/limits.conf  
增加以下内容：  
@users soft nofile 100001  
@users hard nofile 100002  
@root soft nofile 100001  
@root hard nofile 100002  
设置最大打开文件数目
