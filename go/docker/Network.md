# Network
自定义容器子网

###常用指令
- create
    - 用途：创建一个子网
    - --subnet: 子网IP段
    ```
    docker network create --subnet=172.18.0.0/16 alex-net
    指定子网段172.18.0.0-172.18.255.255 命名为alex-net
    ```


### 使用于
- 创建容器时自定义容器IP
```
    docker run -dtiP --net alex-net --ip 172.18.0.100 alex-container
    注意：自定义容器IP时，只能使用自定义的子网，不能用默认的（因为会用来自动分配IP段）
```
    