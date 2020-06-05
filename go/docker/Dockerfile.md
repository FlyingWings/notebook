Dockerfile用途
---
## 定义：
用于从其他镜像中创建自定义镜像

## 用途：
- 构建自定义镜像
- 自动执行脚本
- 自动开放网络
- 自动绑定volume

## 指令
- EXPOSE：暴露端口，用于部署容器时使用指定端口
    - `EXPOSE 80 443`
- RUN：执行一定指令
- COPY：复制当前文件夹下的文件到镜像中指定目录
    - `COPY nginx.conf /etc/nginx.conf`
    