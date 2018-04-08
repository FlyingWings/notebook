# 网络工具

- `netstat`: 统计网络信息

- `route`: 路由信息

- `traceroute`: 查看跳转路径

- `host`: DNS查询

- `ssh`: ssh访问+ssh隧道
    - -i: 利用密钥文件
    - -l: 远程用户名
    - -f: ssh后执行后台程序
    - -N: 不执行远程命令(端口转发专用)
    - -R: 端口转发用，格式如下（笑）
    - -D:
    - 常见用法：
        - 直接ssh访问: `ssh test.com`
        - 利用密钥访问: `ssh -i ~/.ssh/key_file test.com`
        - 端口转发（本地到远程）: `ssh -NfR $remote_port:localhost:$local_port $remote_addr` 
        - 搭建SSH SOCKS5通道: `ssh -N -f -D $local_port $remote_addr`

# 用户管理
#### 用户的增删改查

- `useradd`, 常用参数：  
-m: 创建用户同名目录  
-d: 用户根目录(Home Directory)  
-r: 创建一个系统账户

- `userdel`

- `passwd` 

- `su`: 切换用户

#### 用户组管理

- `groups`: 查看当前用户属于的组

- `usermod`: 更改用户信息，常用参数：  
-d: 更改主目录，默认/home/username



# 环境变量
/etc/profile: 全局环境变量  
~/.profile: 私人环境变量（登陆时执行一次）  
~/.bashrc: 私人设置文档，每次执行时都会更新（重启Terminal生效）