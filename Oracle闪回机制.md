#### Oracle开启flashback database功能

window系统下,启动flashback database的功能需要满足3个条件：

1. 要求db在archivelog
2. startup mount
3. 使用flash recovery area,因为flashback log 必须要求防止flash recovery area.

具体操作：

通过cmd命令，打开命令行。

输入`sqlplus / as sysdba`

​		`select flashback_on from v$database;`

​		`shutdown immediate;`

​		`startup mount;`

​		`archive log list;`

​		`alter database archivelog;`

​		`show parameter db_recovery;`

​		`alter system set db_recovery_file_dest='';`

​		`alter database flashback on;`

​		`alter system set db_recovery_file_dest='D:\oracle\flash_recovery_area';`

​		`alter database flashback on;`

​		`select flashback_on from v$database;`

​	    `alter database open;`

​		其实很简单，在开启FLASHBACK DATABASE之前必须保证数据库为归档模式

```
C:\Users\sky> sqlplus / as sysdba

SQL*Plus: Release 11.2.0.1.0 Production on 星期四 5月 21 10:59:46 2020
Copyright (c) 1982, 2010, Oracle.  All rights reserved.

连接到:
Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options

SQL> select flashback_on from v$database;

FLASHBACK_ON
------------------
NO
SQL> shutdown immediate;
数据库已经关闭。
已经卸载数据库。
ORACLE 例程已经关闭。
SQL> startup mount;
ORACLE 例程已经启动。

Total System Global Area 5010685952 bytes
Fixed Size                  2184352 bytes
Variable Size            2902461280 bytes
Database Buffers         2097152000 bytes
Redo Buffers                8888320 bytes
数据库装载完毕。

SQL> archive log list;
数据库日志模式             非存档模式
自动存档             禁用
存档终点            USE_DB_RECOVERY_FILE_DEST
最早的联机日志序列     3900
当前日志序列           3902

SQL> alter database archivelog;

数据库已更改。

SQL> show parameter db_recovery;

NAME                                 TYPE
------------------------------------ ----------------------
VALUE
------------------------------
db_recovery_file_dest                string
D:\oracle\flash_recovery_area
db_recovery_file_dest_size           big integer
3912M

SQL>  alter system set db_recovery_file_dest='';

系统已更改。

SQL>  alter database flashback on;
 alter database flashback on
*
第 1 行出现错误:
ORA-38706: 无法启用 FLASHBACK DATABASE 事件记录。
ORA-38707: 尚未启用介质恢复。

SQL> alter system set db_recovery_file_dest='D:\oracle\flash_recovery_area';

系统已更改。

SQL> alter database flashback on;

数据库已更改。

SQL> select flashback_on from v$database;

FLASHBACK_ON
------------------
YES

SQL> alter database open;

数据库已更改。

```

