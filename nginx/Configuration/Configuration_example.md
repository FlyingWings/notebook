# Nginx实例配置


```lang=c

user nginx nginx; # 用户名+用户组
worker_processes 2; # worker进程数量
worker_cpu_affinity 0001 0010; # worker进程绑定的CPU核ID
error_log /var/log/nginx/error.log; # 错误日志位置

pid /run/nginx.pid; # master进程ID的存放位置（里面只有一个ID）
worker_rlimit_nofile 65535; # worker进程最大打开文件描述符数量

# 加载额外模块，例如IP解析-》地理位置
load_module "/usr/lib64/nginx/modules/ngx_http_geoip_module.so";

events {
    use epoll; # 事件模型：epoll
    worker_connections 20480; # 单个worker最大接受连接数
    accept_mutex on; # 开启accept锁
    multi_accept on;# 对给到的请求均建立accept
}


http {
    log_format  main '$remote_addr';
    access_log /var/log/nginx/access.log    main;
    
    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    
    server {
        listen 80;
        server_name localhost;
        root /var/www/html; # 默认服务器ROOT
        index index.php;
        
        
        location /test {
            index index.html;
            root /var/www/html/use; # 访问的默认路径是/var/www/html/use/test
        }
        
        location /test-alias {
            root /var/www/html/test-alias;
        }
        
        
        location / {
            index index.php;
        }
        error_page 404 /404.html;
            location = /40x.html {
        }
        
        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
        
}


{"id":"fdsfs-2cvwefs-dsafs","subnets":[{"id":"fdfdsfas-23fds"
