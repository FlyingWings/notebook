# Nginx的配置选项与功能

## nginx的配置结构：nginx.conf
- 块配置项：以大括号作为配置块的分割
- 块嵌套的格式进行模块化配置
- 配置项名是指定的，用空格作为分隔符；而配置项值一般是数字/字符串/正则表达式，以分号结尾；
- 配置项单位一般是K，M等，涉及时间时，可使用ms, s, m, h等
    
##  基本配置结构
分为以下几类：
- 调试定位问题用
- 正常运行，必备用
- 优化性能用
- 事件类（？）
    
### 调试+定位问题用
- 守护进程运行:  
daemon on|off;  
默认：daemon on;  

- 是否以m/w形式工作[多进程]  
master_process on|off;  
默认：master_process on  
关闭后单进程模式，master自己处理请求

- error_log设置  
error_log pathfile level;#多参数，文件位置，错误级别  
默认：error_log logs/error.log error; 
这里的pathfile可以是文件名，也可以是/dev/null（不输出）  
level可以为debug, info, notice, warn, error, crit, alert, emerg从左到右优先级变大，高于设定的优先级的日志会被输出，如warn级别，则warn及其右侧的都会输出

- 仅对指定客户端输出debug级别日志  
debug_connection IP|CIDR # IP地址|地址子网
需要放在events里面才有效，例如
```
events {
    debug_connection 192.168.2.10;
    debug_connection 192.168.1.0/24;
}
``` 
这样就会对指定地址的请求给出debug级别的报错，而其他的沿用原来的error_log设置

### 正常运行用

- 定义环境变量  
env VAR=VALUE  
设置操作系统的环境变量等，如`env TESTPATH=/tmp/;`

- 嵌入其他配置文件  
include pathfile;
将其他配置文件嵌入到当前的nginx.conf文件中(以相对路径配置时，根目录为nginx.conf所在目录)  
如
```
include mime.types;
include conf.d/*.conf
```
- pid文件的路径（保存master进程的process id的文件名）  
pid path/file;  
需要指定目录的读写权限  

- worker进程的用户及用户组  
user username[groupname];
默认：user nobody nobody或user nginx;  

- worker进程可以打开最大的文件描述符个数  
worker_rlimit_nofile limit;
例：worker_rlimit_nofile_limit 65535;

### 优化性能用
- worker进程个数  
worker_processes number;
默认：worker_processes 1; # 最好与CPU核的数量一致  

- 绑定Nginx worker进程到指定的CPU内核  
worker_cpu_affinity cpumask queue;
例如：`worker_cpu_affinity 0001 0010 0100 1000;`  
用于绑定进程到CPU核上，确保防止抢占

- worker_priority nginx进程优先级
worker_priority nice;
设置进程优先级，-20~+19之间，越小越高，需要大于-5(系统内核进程的优先级)

- worker进程优先级设置  
worker_priority nice;  
默认：worker_priority 0;
用于设置进程的nice优先级，-20~+19，从大到小，可以调整略小，但不要设置<-5(内核进程)

### 事件类配置项(在event配置块中的)
- 是否打开accept锁  
accept_mutex on|off
默认：accept_mutex on;  
worker子进程间的负载均衡锁，用于序列化地让所有子进程accept TCP连接  

- accept锁后到真正建立连接之间的延迟时间  
accept_mutex_delay Nms;  
默认：accept_mutex_delay 500ms;  
当某个worker进程试图取accept锁而没有取到时，需要延时以上的时间间隔才能再次取锁

- 批量建立新连接：  
multi_accept on|off  
默认multi_accept off;  

- 选择事件模型  
use kqueue|epoll|/dev/poll|select|poll|eventport]  
默认：不选，理论上用epoll最好  

- 每个worker最大连接数  
worker_connections number;(上面提过了)  
worker_connections 65535;


## 配置示例
- 见[Example](/nginx/Configuration/Example.md)