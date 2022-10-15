 # Change Database info (修改数据库)

#### 还原数据库
```sql{.line-number}
/**
    restore database [database name]
    from disk='/var/opt/mssql/[back file name].bak'
    with move '[database logic name]'
    to '/opt/mssql/data/[database file name].mdf',
    move '[database log logic name]'
    to '/opt/mssql/data/[database logic name].ldf'
    Go
**/
restore database main_data
from disk='/var/opt/mssql/test.bak'
with move 'test_db'
to '/opt/mssql/data/test.mdf',
move 'test_db_log'
to '/opt/mssql/data/test_log.ldf'
Go
```


#### 修改数据库名称
```sql{.line-numbers}
 ALTER DATABASE DatabaseName 
 MODIFY FILE (NAME = OldLogicName, NEWNAME = NameLogicName)
```

#### 修改逻辑文件名
```sql{.line-numbers}
USE master;  
GO  
ALTER DATABASE MyTestDatabase SET SINGLE_USER WITH ROLLBACK IMMEDIATE;
GO
ALTER DATABASE MyTestDatabase MODIFY NAME = MyTestDatabaseCopy;
GO  
ALTER DATABASE MyTestDatabaseCopy SET MULTI_USER;
GO
```