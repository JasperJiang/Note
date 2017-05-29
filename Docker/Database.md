### Mysql

**Run mysql**

```shell
docker run --name mysql -v /my/own/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=test123 -d -p 3306:3306 mysql
```

### Sql Server


```shell
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=test123!' -p 1433:1433 -d microsoft/mssql-server-linux
```


```shell
docker exec -i <container_id> bash -c 'cat > /var/opt/mssql/backup.bak' < '/source/path/backup.bak’
```


```sql
RESTORE DATABASE AnalyticAppsUserManagement  
   FROM DISK = '/var/opt/mssql/backup.bak'
   WITH
   MOVE 'AnalyticAppsUserManagement' TO '/var/ops/mssql/AnalyticAppsUserManagement.mdf',
   MOVE 'AnalyticAppsUserManagement1' TO '/var/ops/mssql/AnalyticAppsUserManagement1.mdf',
   MOVE 'AnalyticAppsUserManagement2' TO '/var/ops/mssql/AnalyticAppsUserManagement2.mdf',
   MOVE 'AnalyticAppsUserManagement3' TO '/var/ops/mssql/AnalyticAppsUserManagement3.mdf',
   MOVE 'AnalyticAppsUserManagement_log' TO '/var/ops/mssql/AnalyticAppsUserManagement_log.ldf',
   MOVE 'AnalyticAppsUserManagement_log1' TO '/var/ops/mssql/AnalyticAppsUserManagement_log1.ldf',
   MOVE 'AnalyticAppsUserManagement_log2' TO '/var/ops/mssql/AnalyticAppsUserManagement_log2.ldf',
   MOVE 'AnalyticAppsUserManagement_log3' TO '/var/ops/mssql/AnalyticAppsUserManagement_log3.ldf'
```

