# Query Table Info (查询表信息)

```sql {.line-numbers}
--表名!!!
DECLARE @tableName NVARCHAR(MAX);
SET @tableName = N'table name here!';

SELECT CASE WHEN col.colorder = 1 THEN obj.name
ELSE ''
END AS 表名 ,
    col.colorder AS 序号 ,
    t.name AS 数据类型,
    col.name AS 列名 ,
    ISNULL(ep.[value], '') AS 列说明 ,
    t.name AS 数据类型 ,
    col.length AS 长度 ,
    ISNULL(COLUMNPROPERTY(col.id, col.name, 'Scale'), 0) AS 小数位数 ,
    CASE WHEN COLUMNPROPERTY(col.id, col.name, 'IsIdentity') = 1 THEN '√'
ELSE ''
END AS 标识 ,
    CASE WHEN EXISTS ( SELECT 1
    FROM dbo.sysindexes si
        INNER JOIN dbo.sysindexkeys sik ON si.id = sik.id
            AND si.indid = sik.indid
        INNER JOIN dbo.syscolumns sc ON sc.id = sik.id
            AND sc.colid = sik.colid
        INNER JOIN dbo.sysobjects so ON so.name = si.name
            AND so.xtype = 'PK'
    WHERE sc.id = col.id
        AND sc.colid = col.colid ) THEN '√'
ELSE ''
END AS 主键 ,
    CASE WHEN col.isnullable = 1 THEN '√'
ELSE ''
END AS 允许空 ,
    ISNULL(comm.text, '') AS 默认值
FROM dbo.syscolumns col
    LEFT JOIN dbo.systypes t ON col.xtype = t.xusertype
    INNER JOIN dbo.sysobjects obj ON col.id = obj.id
        AND obj.xtype = 'U'
        AND obj.status >= 0
    LEFT JOIN dbo.syscomments comm ON col.cdefault = comm.id
    LEFT JOIN sys.extended_properties ep ON col.id = ep.major_id
        AND col.colid = ep.minor_id
        AND ep.name = 'MS_Description'
    LEFT JOIN sys.extended_properties epTwo ON obj.id = epTwo.major_id
        AND epTwo.minor_id = 0
        AND epTwo.name = 'MS_Description'
WHERE obj.name = @tableName --表名
ORDER BY col.colorder;--col.colorder
```
# List tables by their size in SQL Server database (查询表数据量)
```sql {.line-numbers}
select schema_name(tab.schema_id) + '.' + tab.name as [table],
   cast(sum(spc.used_pages * 8)/1024.00 as numeric(36, 2)) as used_mb,
   cast(sum(spc.total_pages * 8)/1024.00 as numeric(36, 2)) as allocated_mb
from sys.tables tab
   inner join sys.indexes ind
   on tab.object_id = ind.object_id
   inner join sys.partitions part
   on ind.object_id = part.object_id and ind.index_id = part.index_id
   inner join sys.allocation_units spc
   on part.partition_id = spc.container_id
group by schema_name(tab.schema_id) + '.' + tab.name
order by sum(spc.used_pages) desc
```
# SQL Server – Query to list table size and row counts (查询表大小和行数)
```sql {.line-numbers}
/********************
    objects:
    sys.tables
    sys.partitions
    sys.allocation_units
    sys.schemas
********************/
USE [Testdb]
GO
SELECT
   s.Name AS SchemaName,
   t.Name AS TableName,
   p.rows AS RowCounts,
   CAST(ROUND((SUM(a.used_pages) / 128.00), 2) AS NUMERIC(36, 2)) AS Used_MB,
   CAST(ROUND((SUM(a.total_pages) - SUM(a.used_pages)) / 128.00, 2) AS NUMERIC(36, 2)) AS Unused_MB,
   CAST(ROUND((SUM(a.total_pages) / 128.00), 2) AS NUMERIC(36, 2)) AS Total_MB
FROM sys.tables t
   INNER JOIN sys.indexes i ON t.OBJECT_ID = i.object_id
   INNER JOIN sys.partitions p ON i.object_id = p.OBJECT_ID AND i.index_id = p.index_id
   INNER JOIN sys.allocation_units a ON p.partition_id = a.container_id
   INNER JOIN sys.schemas s ON t.schema_id = s.schema_id
GROUP BY t.Name, s.Name, p.Rows
ORDER BY p.Rows desc
GO
```