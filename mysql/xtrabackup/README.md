# 一、主库导出数据
```
#全库导出
innobackupex --default-file=/etc/my.cnf --user=root --password=***** --stream=tar  --tmpdir=/usr/local/mysql_bk/ /usr/local/mysql_bk/  |pigz -p 16 |ssh -i /root/.ssh/id_rsa_new root@192.168.56.100 "pigz -d | tar -xf - -C /data1/mysql"

#指定数据库导出
innobackupex --default-file=/etc/my.cnf --user=root --password=***** --databases=user_storage --stream=tar  --tmpdir=/usr/local/mysql_bk/ /usr/local/mysql_bk/  |pigz -p 16 |ssh -i /root/.ssh/id_rsa_new root@192.168.56.100 "pigz -d | tar -xf - -C /data1/mysql"
```

# 二、从库恢复

```
#查看binlog日志
[root@ mysql]# cat /data1/mysql/xtrabackup_binlog_info
mysql-bin.000002 499886662


#恢复数据
innobackupex --default-file=/etc/my.cnf --apply-log /data1/mysql/
```