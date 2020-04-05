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
- bind: 绑定端口/地址对，如127.0.0.1:8080.同个端口监听多个地址时生效
- ssl：显式声明基于SSL协议

### server_name: 主机名
```
server_name alexfu.cc www.alexfu.cc 
```
在处理HTTP请求时，Nginx会取Header中的Host字段，与server_name进行匹配，若出现多个，则优先级如下：
1) 完全匹配: `alexfu.cc`
2) 通配符: `*.alexfu.cc`
3) 通配符在后: `alexfu.*`
4) 正则表达式: `^alex.+$`

若均不匹配，则会如上，找default的块，以及匹配listen端口的第一个server块

### location
```
location[=|~|~*|^~|@]/uri/
```
对给定，符合location中规则的URI进行针对性请求
- `=`:  将URI 作为字符串，对参数中的URI做完全匹配
- `~`:  匹配时大小写敏感
- `~*`: 匹配时大小写不敏感
- `^~`: 只要指定模式的前半部分与URI参数匹配（类似正则）
- `@`:  Nginx内部服务间的重定向，不直接处理用户请求
（URI模式可为正则表达式类型）  
location顺序处理，location /作为最低部，囊括所有未被匹配的请求
例子：
```
location / #所有
location = /test #直接等于test 
location ~ \.php($|/) # 区分大小写，以php结尾或者.php/参数的形式
location ^~ images #以images开头的
```
对于location中，若指定root, 则以其中的root为准

### root: 给定指定配置块对应的根目录
对于server块，指定的是/目录对应的根目录，如果本服务器对应的URI格式全部位于本目录下，则不用在location块中指定root;  
反之则需要在location中指定自己的root位置
```
server {
    root /var/www;
    location / {} # 这里默认的就是/var/www目录下
    location /index {
        root /var/test; # 这里默认就是/var/test/index下[重新指定自己ROOT]
    }
```
### alias: 对给定的位置赋予别名
仅用于location块中，用于设置指定位置的绝对路径别名，如
```
location /index {
    alias /var/www/index; # 对指定的pattern，给一个真正的目录别名
}
```
### index: 首页文件名（按顺序访问)

### error_page 错误页面
根据错误码，重定向到指定的URI中
```
error_page 404 /404.html;
error_page 500 502 503 504 /50x.html;
error_page 403  http://test.com/forbidden.html;
```

### try_files: 顺序访问path，找不到则重定向到最后的参数URI
```
try_files test.html $uri $uri/index.html $uri.html @other;
location @other {
    proxy_pass  http://test.html;
}
```


