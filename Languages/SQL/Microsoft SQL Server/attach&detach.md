# Attach and Detach DataBase (分离和附加数据库)
## sp_attach_db (Transact-SQL) 附加数据库
### Applies to: yesSQL Server (all supported versions)
 
### Important   
    1.This feature will be removed in a future version of Microsoft SQL Server. Avoid using this feature in new development work, and plan to modify applications that currently use this feature. We recommend that you use CREATE DATABASE database_name FOR ATTACH instead
    2.We recommend that you do not attach or restore databases from unknown or untrusted sources. Such databases could contain malicious code that might execute unintended Transact-SQL code or cause errors by modifying the schema or the physical database structure. Before you use a database from an unknown or untrusted source, run DBCC CHECKDB on the database on a nonproduction server and also examine the code, such as stored procedures or other user-defined code, in the database.

### Note 
    To rebuild multiple log files when one or more have a new location, use CREATE DATABASE database_name FOR ATTACH_REBUILD_LOG

> Syntax 
```sql {.line-numbers}
exec sp_attach_db [ @dbname= ] 'dbname'  
    , [ @filename1= ] 'filename_n' [ ,...16 ]
```
> e.g
```sql {.line-numbers}
    exec sp_attach_db @dbname='Test_db',
    @filename1='D:\Test_db.mdf',@filename2='D:\Test_db_log.ldf'
    GO
```

---
## sp_detach_db (Transact-SQL) 分离数据库
### Applies to: SQL Server (all supported versions)

### Important
    For a replicated database to be detached, it must be unpublished. For more information, see the "Remarks" section later in this topic.
> Syntax
```sql {.line-numbers}
exec sp_detach_db [ @dbname= ] 'database_name'   
        [ , [ @skipchecks= ] 'skipchecks' ]   
        [ , [ @keepfulltextindexfile = ] 'KeepFulltextIndexFile' ] 
```
> e.g
```sql {.line-numbers}
    exec sp_detach_db 'Test_db'
    GO
```