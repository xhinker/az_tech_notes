# PostgreSQL tablespace and database 

Tablespace in postgres is logical database layer. A tablespace is a location on disk where PostgreSQL stores data files containing database,indexes,and tables.

Tablespace enable you putting data on disks at your will. 

Take care the tablespace folder permission first. make sure to set **postgres** as the owner of the targeting folder. 

```
drwx------  3 postgres  root       4096 Sep 25 19:51 pg_dbs/
```

Create a new tablespace with sql. 
```sql
create      tablespace my_tablespace 
owner       owner_user
location    '/mnt/postgresql-db-dir';
```

List all tablespaces 
```sql 
select  spcname
        ,pg_tablespace_location(oid) 
from    pg_tablespace;
```

or with pg command
```
\db
```

To set default tablespace 
```
set default_tablespace = new_tablespace;
```


Drop a tablespace with sql
```sql 
drop        tablespace 
if exists   azlog_tablespace;
```

Create new database with assigned tablespace.  
```sql
create database     new_database
tablespace          new_tablespace;
```
