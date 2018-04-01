# 网络工具

- `netstat`: 统计网络信息

- `route`: 路由信息

- `traceroute`: 查看跳转路径

- `host`: DNS查询



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