# Container

### 常用指令
- run
    - --rm: 容器退出后自动删除（用于执行client类型）
    - -t: 分配一个terminal
    - -i: 进入交互模式（允许输入输出指令）
    - --link 名称： 将此容器连接到目标容器（一般为server）
    - --host 指定连接的host，一般为子网IP或容器名
    - -d 以后台模式运行一个容器（一般为server类型）
    