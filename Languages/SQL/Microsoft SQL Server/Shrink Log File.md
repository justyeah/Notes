# Shrink SQL Server Log File (收缩日志文件)

> SQL script
```sql {.line-numbers}
--收缩2008 数据库日志
0
USE [master]  
GO  
ALTER DATABASE Testdb SET RECOVERY SIMPLE WITH NO_WAIT  
GO  
ALTER DATABASE Testdb SET RECOVERY SIMPLE  
GO  
USE Testdb  
GO  
DBCC SHRINKFILE (N'Testdb_log' , 0,TRUNCATEONLY)  
GO  
USE [master]  
GO  
ALTER DATABASE Testdb SET RECOVERY FULL WITH NO_WAIT  
GO  
ALTER DATABASE Testdb SET RECOVERY FULL  
GO  
  
--查询指定数据库的日志文件名称  
--USE Testdb  
--GO
--Query log file name!  
--SELECT name FROM SYS.database_files WHERE type_desc='LOG'  
```