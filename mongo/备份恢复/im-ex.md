# Mongo备份恢复

#### 操作方法
>备份：
> 
    mongodump -h 172.16.0.114 -d local -o ~/dbbackup
>恢复：
> 
    mongorestore -h 172.16.0.114 -d local --drop ~/dbbackup/local
    