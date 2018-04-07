# server块的结构

### listen: 用于声明端口监听的方式   
```lang=c
listen 80;
listen 127.0.0.1:80;
listen 443 ssl;
listen 80 default_server;
```
参数意义：
- default: 将给定的server块作为Web服务的默认server，不设置时以第一个server块作为默认
- default_server: 同上
- bgacklog=num: 建立连接的过程中，还没有建立fd，于是在backlog队列中存放新连接；默认为-1
- deferred: 当请求建立，三次握手完成，且真的发送请求时，才唤醒worker进程处理该连接；在请求建立但未发送数据时，不唤醒；（用于大并发优化）
- bind: 未知

### server_name server_name_list;
```
server_name alexfu.cc www.alexfu.cc 
```
在处理HTTP请求时，Nginx会取Header中的Host字段，与server_name进行匹配，若出现多个，则优先级如下：
1) 完全匹配: `alexfu.cc`
2) 通配符: `*.alexfu.cc`
3) 通配符在后: `alexfu.*`
4) 正则表达式: `^alex.+$`

