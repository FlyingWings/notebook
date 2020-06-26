# Container

### 常用指令
- run
    - --rm: 容器退出后自动删除（用于执行client类型）
    - -t: 分配一个terminal
    - -i: 进入交互模式（允许输入输出指令）
    - --link 名称： 将此容器连接到目标容器（一般为server）
    - --host 指定连接的host，一般为子网IP或容器名
    - -d 以后台模式运行一个容器（一般为server类型）
    - -P 开放所有容器端口，随机绑定到主机
    - --mount: 挂载文件，可以是volume或者是主机目录
        - 主机目录，语法如下：`--mount type=bind,src=//c/org,dst=/data`,将宿主的C盘/org目录绑定给容器的/data目录
        
