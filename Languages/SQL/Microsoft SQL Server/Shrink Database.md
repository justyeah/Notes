# Use Transact-SQL Shrink a database
> SHRINKDATABASE to decrease the size of the data and log files in the UserDB database, and to allow for 10 percent free space in the database.
```sql
DBCC SHRINKDATABASE (UserDB, 10);
GO
```
# Using Transact-SQL To shrink a data or log file
>SHRINKFILE to shrink the size of a data file named DataFile1 in the UserDB database to 7 MB.
```sql
USE UserDB;
GO
DBCC SHRINKFILE (DataFile1, 7);
GO
```