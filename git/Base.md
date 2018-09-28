# Git概念
## 分布式版本控制
- 保存完整的文件内容
- 每次提交增量更新（快照）
    - 每次更改时，对每个文件有变化的创建快照；没有变化的保存一个指向之前存储文件的链接
- 保证完整性
    - 检查Hash值计算完整性



## 基本状态
- Modified
- Staged
- Committed

## 基本目录区
- 工作目录

- 暂存区域

- Git仓库

## 基本流程
1. 工作目录修改文件
2. 暂存文件，将快照保存到暂存区域
3. 提交更新，将更改提交到Git仓库

## 基本配置
- 命令`git config`
    - 配置位置级别
        - /etc/gitconfig: 全局配置
        - ~/.gitconfig: 当前用户配置
        - 当前Git目录的.git/config: 当前目录配置
        
- 常用配置项
    - user.name
    - user.email
- 检查配置项
`git config --list`