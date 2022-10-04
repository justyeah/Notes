# System Information Schema Views (Transact-SQL) 系统信息架构视图
> #### Applies to:  
> * SQL Server (all supported versions) 
> * Azure SQL Database  
> * Azure SQL Managed Instance (托管实例)

>An information schema view is one of several methods SQL Server provides for obtaining metadata. Information schema views provide an internal, system table-independent view of the SQL Server metadata. Information schema views enable applications to work correctly although significant changes have been made to the underlying system tables. The information schema views included in SQL Server comply with the ISO standard definition for the INFORMATION_SCHEMA.
信息架构视图是SQL Server用于获取元数据的几种方法之一。 信息架构视图提供与SQL Server元数据无关的内部系统表视图。 尽管已经对基础系统表进行了重要的修改，信息架构视图仍然可使应用程序正常工作。 SQL Server中包含的信息架构视图符合INFORMATION_SCHEMA的 ISO 标准定义。

>SQL Server supports a three-part naming convention when you refer to the current server. The ISO standard also supports a three-part naming convention. However, the names used in both naming conventions are different. The information schema views are defined in a special schema named INFORMATION_SCHEMA. This schema is contained in each database. Each information schema view contains metadata for all data objects stored in that particular database. The following table shows the relationships between the SQL Server names and the SQL standard names.
引用当前服务器时，SQL Server支持由三部分构成的命名约定。 ISO 标准也支持三部分命名约定。 但是，两种命名约定中使用的名称并不相同。 信息架构视图是在名为 INFORMATION_SCHEMA 的特殊架构中定义的。 此架构包含在每个数据库中。 每个信息架构视图包含特定数据库中存储的所有数据对象的元数据。 下表显示了SQL Server名称和 SQL 标准名称之间的关系。

|  SQL Server name   | Maps to this equivalent SQL standard name  |
|  ----  | ----  |
| Database	  | Catalog |
| Schema  | Schema |
| Object  | Object |
| user-defined data type  | Domain |

|  SQL Server 名称   | 对应的 SQL 标准等价名称  |
|  ----  | ----  |
| 数据库	  | 目录 |
| 架构  | 架构 |
| 对象  | 对象 |
| 用户定义的数据类型  | 域 |


> ##### This name-mapping convention applies to the following SQL Server ISO-compatible views.

~~~sql {.line-numbers}
CHECK_CONSTRAINTS

COLUMN_DOMAIN_USAGE

COLUMN_PRIVILEGES

COLUMNS

CONSTRAINT_COLUMN_USAGE

CONSTRAINT_TABLE_USAGE

DOMAIN_CONSTRAINTS

DOMAINS

KEY_COLUMN_USAGE

PARAMETERS

REFERENTIAL_CONSTRAINTS

ROUTINES

ROUTINE_COLUMNS

SCHEMATA

TABLE_CONSTRAINTS

TABLE_PRIVILEGES

TABLES

VIEW_COLUMN_USAGE

VIEW_TABLE_USAGE

VIEWS
~~~