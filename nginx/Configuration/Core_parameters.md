## 相对应的内核参数优化
ulimit -a: 查看所有配置项目

需要修改的是其中的open-files：   
/etc/security/limits.conf  
增加以下内容：  
@users soft nofile 100001  
@users hard nofile 100002  
@root soft nofile 100001  
@root hard nofile 100002  
设置最大打开文件数目