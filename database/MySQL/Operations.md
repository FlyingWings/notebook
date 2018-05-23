### 常用指令和功能

#### 权限
- GRANT
    - 例子：`grant select, insert, update, delete on db_name.*(table name) to user_name@'remote_addr' identified by 'password';`
    - 解释： 为从`remote_addr`上访问的用户`user_name`赋予数据库`db_name`上的CRUD权限，以密码`password`作为验证
    - 用途：
        - 增加用户
        - 修改用户密码
        - 修改用户权限
    - 可以理解为INSERT/UPDATE mysql.user，同时对用户名，密码和
- REVOKE
    - 例子：`revoke select, insert, update, delete on db_name.*(table_name) to user_name@'remote_addr'`
    - 解释：收回CRUD权限（不改密码，不删除用户）
    - 用户：
        - 修改权限（ONLY）
   
#### 连接
- 短连接
    - 

- 长连接

- 连接池