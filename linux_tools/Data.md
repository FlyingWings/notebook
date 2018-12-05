# 数据转移和恢复

### MySQL
- 备份：`mysqldump -u root db_name > db.sql && gzip db.sql`
- 还原：
    - 压缩的：`zcat db.sql.gz | mysql -u root db_name`
    - 原生的：`mysql - u root < db.sql`
### Clickhouse
- 备份：`clickhouse-client --query="SELECT * FROM xxx format (NATIVE|CSV)" > result.csv`
- 还原：`clickhouse-client --query="INSERT INTO xxx FORMAT (NATIVE|CSV)" < result.csv`
### MongoDB
- 备份: `mongoexport -d db_name -c collection_name -o output -f "fields1,fields2""`
- 还原: `mongoimport -d db_name -c collection_name file_name`

