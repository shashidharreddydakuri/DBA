**PG\_DBA\_SCRIPTS**

**DATABASE MANAGEMENT**  
Create a database in postgres

**\-- Below commands can be used to create database**

postgres=\# **create database DBATEST;**  
CREATE DATABASE

postgres=\# **create database DBATEST with tablespace ts\_postgres;**  
CREATE DATABASE

postgres\#**CREATE DATABASE "DBATEST"**  
**WITH TABLESPACE ts\_postgres**  
**OWNER "postgres"**  
**ENCODING 'UTF8'**  
**LC\_COLLATE \= 'en\_US.UTF-8'**  
**LC\_CTYPE \= 'en\_US.UTF-8'**  
**TEMPLATE template0;**

 

\-- View database information:

postgres=\# **\\l**

postgres\# **select \* from pg\_database;**

***\<\< Note \- alternatively database can be created using pgadmin GUI tool also \>\>\>***

How to connect to postgres db

**How to connect to postgres db:**

**set PATH if not done:**

postgres$ **export PATH=/Library/PostgreSQL/10/bin:$PATH**  
postgres$ **which psql**  
/Library/PostgreSQL/10/bin/psql

**connect to db using SYNTAX \- psql \-d \-U**

postgres$ **psql \-d edb \-U postgres**  
Password for user postgres:  
psql (10.13)  
Type "help" for help.

postgres=\#

**Find current connection info:**

postgres=\# **\\conninfo**  
You are connected to database "edb" as user "postgres" via socket in "/tmp" at port "5432".

 

postgres=\# **select current\_schema,current\_user,session\_user,current\_database();**  
current\_schema  | current\_user | session\_user | current\_database  
\----------------+--------------+--------------+------------------  
public         | postgres.     | postgres     | edb

 

**Switch to another database:**

postgres-\# **\\c dbaclass**  
You are now connected to database "dbaclass" as user "postgres".

   
Drop database in postgres  
**\- Drop database from psql**  
***Note** \- while dropping a database, you need to connect to a database other than the db you are trying to drop.*

postgres=\# **\\conninfo**  
You are connected to database "postgres" as user "postgres" on host "localhost" at port "5432".

postgres\#**drop database "DBACLASS";**

**\-- Drop database using dropdb os utility**

postgres$ pwd  
/Library/PostgreSQL/10/bin

postgres$ **./dropdb \-e "DBACLASS"**  
Password:  
SELECT pg\_catalog.set\_config('search\_path', '', false)  
DROP DATABASE "DBACLASS";

 

For complete article visit \-  \> [https://dbaclass.com/article/drop-database-postgres/](https://dbaclass.com/article/drop-database-postgres/)

Find the database details in postgres  
postgres=\# **\\list+**  
List of databases  
Name | Owner | Encoding | Collate | Ctype | Access privileges | Size | Tablespace | Description  
\-----------+----------+----------+---------+-------+-----------------------+---------+------------+--------------------------------------------  
dbaclass | postgres | UTF8 | C | C | | 2268 MB | pg\_default |  
postgres | postgres | UTF8 | C | C | | 4132 MB | pg\_default | default administrative connection database  
template0 | postgres | UTF8 | C | C | \=c/postgres \+| 7601 kB | pg\_default | unmodifiable empty database  
| | | | | postgres=CTc/postgres | | |  
template1 | postgres | UTF8 | C | C | \=c/postgres \+| 7601 kB | pg\_default | default template for new databases  
| | | | | postgres=CTc/postgres | | |  
(4 rows)

postgres=\# **select datname from pg\_database;**  
datname  
\-----------  
postgres  
template1  
template0  
dbaclass  
(4 rows)

Find postgres db sizes  
**How to get postgres db size:**

postgres=\# ***SELECT pg\_database.datname as "database\_name", pg\_size\_pretty(pg\_database\_size(pg\_database.datname)) AS size\_in\_mb FROM pg\_database ORDER by size\_in\_mb DESC;***

 database\_name | size\_in\_mb  
\---------------+------------  
 DBACLASS      | 7767 kB  
 postgres      | 7735 kB  
 template1     | 7735 kB  
 template0     | 7601 kB  
(4 rows)

(or)

postgres=\# ***\\l+***  
                                                               List of databases  
   Name    |  Owner   | Encoding | Collate | Ctype |   Access privileges   |  Size   | Tablespace |                Description  
\-----------+----------+----------+---------+-------+-----------------------+---------+------------+--------------------------------------------  
 DBACLASS  | postgres | UTF8     | C       | C     |                       | 7767 kB | pg\_default | TESTING DB  
 postgres  | postgres | UTF8     | C       | C     |                       | 7735 kB | pg\_default | default administrative connection database  
 template0 | postgres | UTF8     | C       | C     | \=c/postgres          \+| 7601 kB | pg\_default | unmodifiable empty database  
           |          |          |         |       | postgres=CTc/postgres |         |            |  
 template1 | postgres | UTF8     | C       | C     | \=c/postgres          \+| 7735 kB | pg\_default | default template for new databases  
           |          |          |         |       | postgres=CTc/postgres |         |            |  
(4 rows)

How to find timezone info  
**Commands to find timezone information**

dbaclass=\# **show timezone**  
TimeZone  
\--------------  
Asia/Kolkata  
(1 row)

dbaclass=\# **SELECT current\_setting('TIMEZONE');**  
current\_setting  
\-----------------  
Asia/Kolkata

dbaclass=\# **select name,setting,short\_desc,boot\_val from pg\_settings where name='TimeZone';**  
name     | setting      | short\_desc                                                      | boot\_val  
\----------+--------------+-----------------------------------------------------------------+----------  
TimeZone | Asia/Kolkata | Sets the time zone for displaying and interpreting time stamps. | GMT  
(1 row)

How to find postgres version  
**Below commands can be used to find postgres version:**

dbaclass=\# **show server\_version;**  
server\_version  
\----------------  
10.13  
(1 row)

dbaclass=\# **select version ();**  
version  
\--------------------------------------------------------------------------------------------------------------------------------------------------------------------  
PostgreSQL 10.13 on x86\_64-apple-darwin, compiled by i686-apple-darwin11-llvm-gcc-4.2 (GCC) 4.2.1 (Based on Apple Inc. build 5658\) (LLVM build 2336.11.00), 64-bit  
(1 row)

postgres$ cd /Library/PostgreSQL/10/bin

postgres$ **./postgres \-V**  
postgres (PostgreSQL) 10.13

\-- PG\_VERSION file is under data directory  
postgres$ cd /Library/PostgreSQL/10/data

postgres$ **cat PG\_VERSION**  
10

How to enable archiving(wal) in postgres  
**STEPS FOR enabling archiving:**

**1\. Create directory for archiving:**

**mkdir \-p /Library/PostgreSQL/10/data/archive/**

**2\. Update the postgres.conf file with below values**

**wal\_level \= replica**  
**archive\_mode \= on**  
**max\_wal\_senders=1**  
**archive\_command= 'test \! \-f /Library/PostgreSQL/10/data/archive/%f && cp %p /Library/PostgreSQL/10/data/archive/%f'**

**3\. Restart the postgres servers**

**export PGDATA=/Library/PostgreSQL/10/data**  
**pg\_ctl stop**  
**pg\_ctl start**

**3\. Check archive status:**  
postgres=\# select name,setting from pg\_settings where name like 'archive%';  
name             | setting  
\-----------------+--------------------------------------------------------------------------------------------------  
archive\_command  | test \! \-f /Library/PostgreSQL/10/data/archive/%f && cp %p /Library/PostgreSQL/10/data/archive/%f  
archive\_mode     | on  
archive\_timeout  |  0

How to rotate server log in postgres  
Below command signals the log-file manager to switch to a new output file immediately.it is just like an alert log

postgres=\# **select pg\_rotate\_logfile() ;**  
pg\_rotate\_logfile  
\-------------------  
t

Find query execution time using pg\_stat\_statement  
**\-- Monitor query execution time**

**select substr(query,1,100) query,calls,min\_time/1000 "min\_time(in sec)" , max\_time/1000 "max\_time(in sec)", mean\_time/1000 "avg\_time(in sec)", rows from pg\_stat\_statements order by mean\_time desc;**

\-------------------------------------------------------------------------------------------------------------------------------------------------------

**NOTE:**

If pg\_stat\_statements is not available in your database, then activate using below:  
**\-- Add below parameters in postgres.conf file and restart the postgres cluster**

**shared\_preload\_libraries \= 'pg\_stat\_statements'**  
**pg\_stat\_statements.track \= all**

**sudo service postgresql restart**

**\-- Now create extension:**

dbaclass=\# **create extension pg\_stat\_statements;**  
CREATE EXTENSION

How to find data directory location  
**HOW TO FIND DATA DIRECTORY LOCATION** 

DATA\_DIRECTORY \- \> Specifies the directory to use for data storage.

dbaclass=\# **show data\_directory;**

data\_directory  
\-----------------------------  
/Library/PostgreSQL/10/data  
(1 row)

dbaclass=\# **select setting from pg\_settings where name \= 'data\_directory';**  
setting  
\-----------------------------  
/Library/PostgreSQL/10/data  
(1 row)

**\-- This will show location of important files in postgres**

dbaclass=\# **SELECT name, setting FROM pg\_settings WHERE category \= 'File Locations';**  
name                     | setting  
\-------------------+---------------------------------------------  
config\_file              | /Library/PostgreSQL/10/data/postgresql.conf  
data\_directory           | /Library/PostgreSQL/10/data  
external\_pid\_file        |  
hba\_file                 | /Library/PostgreSQL/10/data/pg\_hba.conf  
ident\_file               | /Library/PostgreSQL/10/data/pg\_ident.conf  
(5 rows)

Find current sessions in postgres  
**Find sessions in the postgres:**

select pid as process\_id,  
usename as username,  
datname as database\_name,  
client\_addr as client\_address,  
application\_name,  
backend\_start,  
state,  
state\_change,query  
from pg\_stat\_activity;

 

**\-- For specific database:**

select pid as process\_id,  
usename as username,  
datname as database\_name,  
client\_addr as client\_address,  
application\_name,  
backend\_start,  
state,  
state\_change,query   
from pg\_stat\_activity where datname='dbaclass';

process\_id | username | database\_name | client\_address | application\_name |          backend\_start           | state  |           state\_change           \------------+----------+---------------+----------------+------------------+----------------------------------+--------+----------------------------------

      18970 | postgres | dbaclass      |                | psql             | 2020-07-03 20:27:42.225987+05:30 | active | 2020-07-03 23:19:12.023416+05:30

(1 row)

Kill a session in postgres  
**\-- First find the pid of the session:**

dbaclass\#**SELECT datname as database, pid as pid, usename as username, application\_name , client\_addr , query FROM pg\_stat\_activity;**

***\<\< Lets say the pid=1124, in the below query pass the pid value to kill that particular session..\>\>***

dbaclass\#**select pg\_terminate\_backend(pid) from pg\_stat\_activity where pid='1123';**

Cancel a session in postgres  
**\-- First find the pid of the session:**

dbaclass\#**SELECT datname as database, pid as pid, usename as username, application\_name , client\_addr , query FROM pg\_stat\_activity;**

*\<\< Lets say the pid=1124, in the below query pass the pid value to cancel that particular session query\>\>*

dbaclass\#**select pg\_cancel\_backend(pid) from pg\_stat\_activity where pid='1124';**

Kill all session of a user in postgres  
**\-- Here we want to kill all session of the user postgres**

**\-- List all the session of that user.**  
dbaclass\#**select datname as database, pid as pid, usename as username, application\_name , client\_addr, query FROM pg\_stat\_activity where username='postgres';**

**\-- Kill all the session of user postgres.**

dbaclass\#**select pg\_terminate\_backend(pid) from pg\_stat\_activity where usename='postgres';**

Find locks present in postgres  
select t.relname,l.locktype,page,virtualtransaction,pid,mode,granted from pg\_locks l, pg\_stat\_all\_tables t where l.relation=t.relid order by relation asc;

Find blocking sessions in postgres  
**\-- QUERY TO FIND BLOCKING SESSION DETAILS**

dbaclass\#**select pid as blocked\_pid, usename, pg\_blocking\_pids(pid) as "blocked\_by(pid)", query as blocked\_query from pg\_stat\_activity where cardinality(pg\_blocking\_pids(pid)) \> 0;**

**output:**

 blocked\_pid | usename  | blocked\_by(pid) |        blocked\_query  
\-------------+----------+-------------+------------------------------  
        4206 | postgres | {3673}      | alter table test drop query;

Find location of postgres conf files  
**Find location of postgres related conf files**

dbaclass=\# **SELECT name, setting FROM pg\_settings WHERE category \= 'File Locations';**  
name                     | setting  
\-------------------+---------------------------------------------  
config\_file              | /Library/PostgreSQL/10/data/postgresql.conf  
data\_directory           | /Library/PostgreSQL/10/data  
external\_pid\_file        |  
hba\_file                 | /Library/PostgreSQL/10/data/pg\_hba.conf  
ident\_file               | /Library/PostgreSQL/10/data/pg\_ident.conf  
(5 rows)

alternatively:

postgres=\# **show config\_file**;

                 config\_file  
\--------------------------------------------

/Library/PostgreSQL/10/data/postgresql.conf

(1 row)

postgres=\# **show hba\_file;**

                hba\_file  
\-----------------------------------------  
/Library/PostgreSQL/10/data/pg\_hba.conf

(1 row)

postgres=\# **show ident\_file;**  
                ident\_file  
\-------------------------------------------

/Library/PostgreSQL/10/data/pg\_ident.conf

(1 row)

Find current data/time on postgres db  
**\-- Below commands can be used to find postgres db date/timestamp**

postgres=\# **SELECT CURRENT\_TIMESTAMP;**  
current\_timestamp  
\----------------------------------  
2020-07-06 17:45:24.929293+05:30  
(1 row)

postgres=\# **select current\_date;**  
current\_date  
\--------------  
2020-07-06  
(1 row)

postgres=\# **select statement\_timestamp() ;**  
statement\_timestamp  
\----------------------------------  
2020-07-06 17:46:16.825492+05:30  
(1 row)

postgres=\# **select timeofday() ;**  
timeofday  
\-------------------------------------  
Mon Jul 06 17:46:23.861551 2020 IST  
(1 row)

postgres=\# **select localtime(0);**  
localtime  
\-----------  
17:52:38  
(1 row)

postgres=\# **select localtimestamp(0);**  
localtimestamp  
\---------------------  
2020-07-06 17:52:48  
(1 row)

Find extension details  
**\-- Find list of installed extension:**

psql\# **\\dx**

(or)

psql\#**\\dx+**

(or)

psql\#**SELECT \* FROM pg\_extension;**

**\-- For finding available extension in server:**

psql\# **SELECT \* FROM pg\_available\_extensions;**

Find startup time and uptime postgres  
**\-- Uptime of server**

postgres\# **SELECT now() \- pg\_postmaster\_start\_time() "uptime";**  
uptime  
\------------------------  
9 days 04:54:56.774981  
(1 row)

**\-- Server startup time:**

postgres\# **SELECT pg\_postmaster\_start\_time();**  
pg\_postmaster\_start\_time  
\----------------------------------  
26-SEP-20 11:13:08.283105 \+03:00  
(1 row)

Find archiver process status  
postgres\# **select \* from pg\_stat\_archiver;**  
\-\[ RECORD 1 \]------+---------------------------------  
archived\_count     | 0  
last\_archived\_wal  |  
last\_archived\_time |  
failed\_count.      | 0  
last\_failed\_wal.   |  
last\_failed\_time   |  
stats\_reset        | 26-SEP-20 11:13:08.540237 \+03:00

Find postgres configuration values  
**1 . Get Config values from psql prompt.**

postgres=\# **select \* from pg\_settings;**

**\\x**  
postgres=\# **select \* from pg\_settings where name='port';**

**2\. Alternatively you can check postgresql.conf file**

postgres=\# **show config\_file;**  
config\_file  
\---------------------------------  
/pgdata/data/postgresql.conf  
(1 row)

cat /pgdata/data/postgresql.conf

Find the last pg config reload time  
**\-- Last pg config reload time**

postgres=\# **select pg\_conf\_load\_time() ;**  
pg\_conf\_load\_time  
\----------------------------------  
2020-07-06 13:20:18.048689+05:30  
(1 row)

**\-- Reload again and see whether reload time changed or not**

postgres=\# select pg\_reload\_conf();  
pg\_reload\_conf  
\----------------  
t  
(1 row)

postgres=\# **select pg\_conf\_load\_time() ;**

pg\_conf\_load\_time  
\----------------------------------  
2020-07-06 17:46:59.958056+05:30  
(1 row)

How to do wal switch manually  
postgres=\# **select pg\_switch\_wal();**  
pg\_switch\_wal  
\---------------  
0/1D392648  
(1 row)

Monitor archiving progress  
postgres\#**select pg\_walfile\_name(pg\_current\_wal\_lsn()),last\_archived\_wal,last\_failed\_wal,**  
**('x'||substring(pg\_walfile\_name(pg\_current\_wal\_lsn()),9,8))::bit(32)::int\*256 \+**  
**('x'||substring(pg\_walfile\_name(pg\_current\_wal\_lsn()),17))::bit(32)::int \-**  
**('x'||substring(last\_archived\_wal,9,8))::bit(32)::int\*256 \-**  
**('x'||substring(last\_archived\_wal,17))::bit(32)::int**  
**as diff from pg\_stat\_archiver;**

**![][image1]**

   
View/modify connection limit of database

**\---View existing connection limit setting:( datconnlimit )**

postgres=\# select datname,datallowconn,datconnlimit from pg\_database where datname='test\_dev';  
\-\[ RECORD 1 \]--+------------  
datname        | test\_dev  
datallowconn  | t.  
datconnlimit  | \-1.       \-- \>Means unlimited connections allowed

**\-- To set a specific limit for connection**

test\_dev=\# alter database test\_dev connection limit 100;  
ALTER DATABASE

**\-- To restrict all the connections to db**

test\_dev=\# alter database test\_dev connection limit 0;  
ALTER DATABASE

 

***NOTE \- \> Even if connection limit is set to 0 , the superuser will be able to connect to database.***

Find wal file details and its size

**\-- List down all the wal files present in pg\_wal**

postgres=\# **select \* from pg\_ls\_waldir();**  
name                     | size     | modification  
\------------------------------------------+----------+---------------------------  
0000000100000079000000D5 | 16777216 | 22-APR-22 20:51:26 \+03:00  
0000000100000079000000D8 | 16777216 | 22-APR-22 20:39:33 \+03:00  
0000000100000079000000D6 | 16777216 | 22-APR-22 20:07:40 \+03:00  
0000000100000079000000D9 | 16777216 | 22-APR-22 20:47:21 \+03:00  
0000000100000079000000D7 | 16777216 | 22-APR-22 20:21:45 \+03:00  
00000001000000790000005C.00005BC8.backup | 323 | 21-APR-22 10:14:40 \+03:00  
(6 rows)

**\-- Find total size of wal:**

postgres=\# **select sum(size) from pg\_ls\_waldir();**  
sum  
\----------  
83886403  
(1 row)

**\-- Find current wal file lsn:**

postgres=\# **select pg\_current\_wal\_insert\_lsn(),pg\_current\_wal\_lsn();**  
pg\_current\_wal\_insert\_lsn  | pg\_current\_wal\_lsn  
\---------------------------+--------------------  
79/D5980480                | 79/D5980480  
(1 row)

Find temp file usage of databases

postgres=\# **SELECT datname, temp\_files, temp\_bytes, stats\_reset FROM pg\_stat\_database;**

datname    | temp\_files | temp\_bytes | stats\_reset

\-----------+------------+------------+----------------------------------

| 0        | 0          | 18-APR-22.18:23:33.09366 \+03:00

postgres   | 2          | 28000000   | 18-APR-22 18:23:33.093639 \+03:00

edb        | 0          | 0          | 18-APR-22 18:23:37.095023 \+03:00

template1  | 0          | 0          |

template0  | 0          | 0          |

b2cplmdev  | 0          | 0          | 18-APR-22 18:23:35.093019 \+03:00

test\_dev   | 0          | 0          | 19-APR-22 10:17:28.826261 \+03:00

(7 rows)  

![][image2]  
**OBJECT MANAGEMENT**

Create/drop table in postgres

**\-- Create a simple table**  
postgres=\# **Create table member\_table ( mem\_id integer, member\_name varchar(100) , mobile integer not null);**  
CREATE TABLE

**\-- Create table with primary key**  
postgres=\# **Create table member\_table ( mem\_id integer primary key, member\_name varchar(100) , mobile integer not null);**  
CREATE TABLE  
**\-- Create table under particular tablespace**  
postgres=\# **Create table member\_table ( mem\_id integer primary key, member\_name varchar(100) , mobile integer not null) tablespace pg\_production\_ts;**  
CREATE TABLE

**\-- Create table with unique constraint**  
postgres=\# **Create table member\_table ( mem\_id integer, member\_name varchar(100) , mobile integer not null , constraint mem\_id\_cons unique(mem\_id));**  
CREATE TABLE

**\-- Create temporary table:**

postgres=\# **Create temporary table member\_table ( mem\_id integer primary key, member\_name varchar(100) , mobile integer not null) tablespace pg\_default;**  
CREATE TABLE

**\-- Drop table**

postgres=\# **drop table member\_table;**

Create/drop index commands in postgres

**\-- Simple create index:**  
postgres=\# **create index tab\_idx2 on scott.customer(emp\_name);**  
CREATE INDEX

**\-- Create index with tablespace:**

postgres\#**CREATE INDEX tab\_idx2 on scott.customer(emp\_name) TABLESPACE IND\_TS;**  
CREATE INDEX

**\-- Create index without causing blocking:**

postgres=\# **create index concurrently tab\_idx2 on scott.customer(emp\_name) TABLESPACE IND\_TS;**  
CREATE INDEX

**\-- Create unique index:**

postgres=\# **create unique index tab\_idx2 on scott.customer(emp\_name)**  
CREATE INDEX

**\-- Create functional index:**

postgres=\# **create index fun\_idx on scott.customer(lower(emp\_name));**  
CREATE INDEX

**\-- Create multi column index:**

postgres=\# **create index multi\_idx on scott.customer(emp\_name,emp\_id);**  
CREATE INDEX

**\-- drop an index:**

postgres=\# **drop index fun\_idx;**  
DROP INDEX

Find list of schemas in postgres

**\-- Below of any commands can be used to find the schema details:**

postgres=\# **select schema\_name,schema\_owner from information\_schema.schemata;**  
schema\_name         | schema\_owner  
\--------------------+--------------  
raj                | postgres  
information\_schema | postgres  
public             | postgres  
pg\_catalog         | postgres  
pg\_toast\_temp\_1    | postgres  
pg\_temp\_1.         | postgres  
pg\_toast           | postgres  
(7 rows)

postgres=\# **select nspname as schema\_name , pg\_get\_userbyid(nspowner) as schema\_owner from pg\_catalog.pg\_namespace;**  
schema\_name         | schema\_owner  
\--------------------+--------------  
pg\_toast            | postgres  
pg\_temp\_1           | postgres  
pg\_toast\_temp\_1     | postgres  
pg\_catalog          | postgres  
public              | postgres  
information\_schema  | postgres  
raj                 | postgres  
(7 rows)

postgres=\# **\\dn+**  
List of schemas  
Name    | Owner    | Access privileges    | Description  
\--------+----------+----------------------+------------------------  
public | postgres | postgres=UC/postgres+| standard public schema| | \=UC/postgres |  
raj    | postgres |                      |  
(2 rows)

list of objects presents in a schema

**\--Below is for finding objects under schema scott: Replace your schema\_name with scott**

SELECT n.nspname as "Schema",  
c.relname as "Name",  
CASE c.relkind WHEN 'r' THEN 'table' WHEN 'v' THEN 'view' WHEN 'm' THEN 'materialized view' WHEN 'i' THEN 'index' WHEN 'S' THEN 'sequence' WHEN 's' THEN 'special' WHEN 'f' THEN 'foreign table' WHEN 'p' THEN 'partitioned table' WHEN 'I' THEN 'partitioned index' END as "Type",  
pg\_catalog.pg\_get\_userbyid(c.relowner) as "Owner"  
FROM pg\_catalog.pg\_class c  
LEFT JOIN pg\_catalog.pg\_namespace n ON n.oid \= c.relnamespace  
where n.nspname \='**scott**'  
AND pg\_catalog.pg\_table\_is\_visible(c.oid)  
ORDER BY 1,2;

 

 

***NOTE: Make sure that, the schema\_name for which you are looking for objects, is present in the search\_path of that user. Otherwise it wont return any rows***

postgres=\# show search\_path;  
search\_path  
\-----------------------  
"$user", public, **scott**  
(1 row)

Find schema wise size in postgres db

**Below queries can be used to get schema wise size in postgres db**

postgres=\# **select schemaname,pg\_size\_pretty(sum(pg\_relation\_size(quote\_ident(schemaname) || '.' || quote\_ident(tablename)))::bigint) as schema\_size FROM pg\_tables group by schemaname;**  
schemaname          | pg\_size\_pretty  
\--------------------+----------------  
raj                 | 8192 bytes  
public              | 3651 MB  
pg\_catalog          | 2936 kB  
information\_schema  | 96 kB  
(4 rows)

postgres=\# **SELECT schemaname,**  
**pg\_size\_pretty(sum(table\_size)::bigint) as schema\_size,**  
**(sum(table\_size) / pg\_database\_size(current\_database())) \* 100 as percentage\_of\_total\_db**  
**FROM (**  
**SELECT pg\_catalog.pg\_namespace.nspname as schemaname,**  
**pg\_relation\_size(pg\_catalog.pg\_class.oid) as table\_size**  
**FROM pg\_catalog.pg\_class**  
**JOIN pg\_catalog.pg\_namespace ON relnamespace \= pg\_catalog.pg\_namespace.oid**  
**) t**  
**GROUP BY schemaname**  
**ORDER BY schemaname;**

schemaname          | schema\_size | percentage\_of\_total\_db  
\--------------------+-------------+----------------------------  
information\_schema  | 96 kB       | 0.002561568956316216939600  
pg\_catalog          | 6120 kB     | 0.16330002096515883000  
pg\_toast            | 648 kB      | 0.01729059045513446400  
public              | 3651 MB     | 99.76265110861169191100  
raj                 | 8192 bytes  | 0.000213464079693018078300  
(5 rows)

Find top 10 big tables in postgres

**\-- Top 10 big tables in postgres**

**select schemaname as schema\_owner,**  
**relname as table\_name,**  
**pg\_size\_pretty(pg\_total\_relation\_size(relid)) as total\_size,**  
**pg\_size\_pretty(pg\_relation\_size(relid)) as used\_size,**  
**pg\_size\_pretty(pg\_total\_relation\_size(relid) \- pg\_relation\_size(relid))**  
**as free\_space**  
**from pg\_catalog.pg\_statio\_user\_tables**  
**order by pg\_total\_relation\_size(relid) desc,**  
**pg\_relation\_size(relid) desc**  
**limit 10;**

 

**(or)**

**SELECT**  
**nspname as schema\_name,relname as table\_name,pg\_size\_pretty(pg\_relation\_size(c.oid)) as "table\_size"**  
**from pg\_class c left join pg\_namespace n on ( n.oid=c.relnamespace)**  
**where nspname not in ('pg\_catalog','information\_schema')**  
**order by pg\_relation\_size(c.oid) desc limit 10;**

 

Find tables and its index sizes

**\-- Find table sizes and its respective index sizes**

SELECT  
table\_name,  
pg\_size\_pretty(table\_size) AS table\_size,  
pg\_size\_pretty(indexes\_size) AS indexes\_size,  
pg\_size\_pretty(total\_size) AS total\_size  
FROM (  
SELECT  
table\_name,  
pg\_table\_size(table\_name) AS table\_size,  
pg\_indexes\_size(table\_name) AS indexes\_size,  
pg\_total\_relation\_size(table\_name) AS total\_size  
FROM (  
SELECT ('"' || table\_schema || '"."' || table\_name || '"') AS table\_name  
FROM information\_schema.tables  
) AS all\_tables  
ORDER BY total\_size DESC  
) AS pretty\_sizes limit 10;

List down index details in postgres

**\--- It wil find the indexes present on a table 'test'**

postgres=\# **select \* from pg\_indexes where tablename='test';**  
schemaname  | tablename | indexname | tablespace  | indexdef  
\------------+-----------+-----------+-------------+----------------------------------------------------------  
public.     | test      | tes\_idx1  | ts\_postgres | CREATE INDEX tes\_idx1 ON public.test USING btree (datid)  
(1 row)

**\-- All indexes present in database:**

postgres\#**select \* from pg\_indexes**

**\-- It will show all index details including size:**

postgres=\# **\\di+**  
List of relations  
Schema  | Name     | Type  | Owner    | Table  | Size   | Description  
\--------+----------+-------+----------+--------+--------+-------------  
public  | tes\_idx  | index | postgres | test56 | 64 kB |  
public  | tes\_idx1 | index | postgres | test   | 472 MB |  
(2 rows)

**\-- Find indexes with respective column name for table( here table name is test)**

***REFERENCE*** \- https://stackoverflow.com/questions/2204058/list-columns-with-indexes-in-postgresql  
**select**  
**t.relname as table\_name,**  
**i.relname as index\_name,**  
**array\_to\_string(array\_agg(a.attname), ', ') as column\_names**  
**from**  
**pg\_class t,**  
**pg\_class i,**  
**pg\_index ix,**  
**pg\_attribute a**  
**where**  
**t.oid \= ix.indrelid**  
**and i.oid \= ix.indexrelid**  
**and a.attrelid \= t.oid**  
**and a.attnum \= ANY(ix.indkey)**  
**and t.relkind \= 'r'**  
**and t.relname \='*test*'**  
**group by**  
**t.relname,**  
**i.relname**  
**order by**  
**t.relname,**  
**i.relname;**

Find the size of a column

**Describe the table:**

postgres=\# \\d test  
Table "public.test"  
Column.      | Type   | Collation | Nullable | Default  
\------------+---------+-----------+----------+---------  
sourcefile  | text.   | | |    \-- \>\>\>\> Will get size for this one   
sourceline  | integer  | | |.  \-- \>\>\>\> Will get size for this one also  
seqno.      | integer | |. |  
name.        | text  | | |  
setting.    | text  | | |  
applied     | boolean  | | |  
error       | text.  | | |  
Indexes:  
"test\_idx" btree (sourcefile)  
"test\_idx2" btree (sourceline)  
 

**\-- Find the column size ( for sourcefile and sourceline)**

postgres=\# **select pg\_size\_pretty(sum(pg\_column\_size(sourcefile))) as total\_size from test;**  
total\_size  
\------------  
12 MB  
(1 row)

postgres=\# **select pg\_size\_pretty(sum(pg\_column\_size(sourceline))) as total\_size from test;**  
total\_size  
\------------  
1152 kB  
(1 row)

Find respective physical file of a table/index

**\-- For getting the physical location of a table:**

postgres=\# **select pg\_relation\_filepath('test');**  
pg\_relation\_filepath  
\----------------------  
base/13635/17395  
(1 row)

**\-- For getting the physical location of an index:**

postgres=\# **select pg\_relation\_filepath('test\_idx');**  
pg\_relation\_filepath  
\----------------------  
base/13635/17638  
(1 row)

Find list of views present

postgres=\# **select \* from pg\_views where schemaname not in ('pg\_catalog','information\_schema','sys');**  
count  
\-------

postgres\#**\\dv**  
List of relations  
Schema | Name | Type | Owner  
\--------+--------------------+------+--------------  
public | pg\_stat\_statements | view | enterprisedb  
(1 row)

Manage sequences in postgres

**\-- Find the sequence details:**

**select \* from pg\_sequences;**

**(or)**

**\\ds+**

List of relations  
Schema  | Name      | Type     | Owner        | Size | Description  
\--------+-----------+----------+--------------+------------+-------------  
public  | class\_seq | sequence | enterprisedb | 8192 bytes |  
(1 row)

**\-- Create sequences:**

postgres\# **CREATE SEQUENCE class\_seq INCREMENT 1 MINVALUE 1 MAXVALUE 1000 START 1;**  
CREATE SEQUENCE

**\-- Create sequence in descending:**

postgres\# **CREATE SEQUENCE class\_seq INCREMENT \-1 MINVALUE 1 MAXVALUE 1000 START 1000;**  
CREATE SEQUENCE

**\-- Alter sequence to change maxvalue:**

postgres=\# **alter sequence class\_seq maxvalue 500;**  
ALTER SEQUENCE

**\-- Reset a sequence using alter command:**

postgres=\# **alter sequence class\_seq restart with 1;**  
ALTER SEQUENCE

**\-- Find next\_val and currval of a sequence:**

postgres=\# **select nextval('class\_seq');**  
nextval  
\---------  
1  
(1 row)

postgres=\# **select currval('class\_seq');**  
currval  
\---------  
1  
(1 row)

Create Partial index in postgres

Partial index, means index will be created on a specific subset of data of a table.

edbstore=\> **create index part\_emp\_idx on orders(tax) where tax \> 400;**  
CREATE INDEX

edbstore=\> **\\d part\_emp\_idx**  
Index "edbuser.part\_emp\_idx"  
Column  | Type          | Key? | Definition  
\--------+---------------+------+------------  
tax     | numeric(12,2) | yes  | tax  
btree, for table "edbuser.orders", predicate (tax \> 400::numeric)

Find foreign key details in postgres

**SELECT**  
**o.conname AS constraint\_name,**  
**(SELECT nspname FROM pg\_namespace WHERE oid=m.relnamespace) AS source\_schema,**  
**m.relname AS source\_table,**  
**(SELECT a.attname FROM pg\_attribute a WHERE a.attrelid \= m.oid AND a.attnum \= o.conkey\[1\] AND a.attisdropped \= false) AS source\_column,**  
**(SELECT nspname FROM pg\_namespace WHERE oid=f.relnamespace) AS target\_schema,**  
**f.relname AS target\_table,**  
**(SELECT a.attname FROM pg\_attribute a WHERE a.attrelid \= f.oid AND a.attnum \= o.confkey\[1\] AND a.attisdropped \= false) AS target\_column**  
**FROM**  
**pg\_constraint o LEFT JOIN pg\_class f ON f.oid \= o.confrelid LEFT JOIN pg\_class m ON m.oid \= o.conrelid**  
**WHERE**  
**o.contype \= 'f' AND o.conrelid IN (SELECT oid FROM pg\_class c WHERE c.relkind \= 'r');**

 

**REFERENCE** \- https://stackoverflow.com/questions/1152260/postgres-sql-to-list-table-foreign-keys

Find specific table/index size

postgres=\# \\d test  
Table "public.test"  
Column.     | Type    | Collation | Nullable | Default  
\------------+---------+-----------+----------+---------  
sourcefile | text     |           |          |  
sourceline | integer  |           |          |  
seqno.     | integer  |           |.         |  
name.      | text     |           |          |  
setting.   | text     |           |          |  
applied   | boolean   |           |          |  
error     | text.     |           |          |  
Indexes:  
"test\_idx" btree (sourcefile)  
"test\_idx2" btree (sourceline)

**\-- Find the table\_size ( excluding the index\_size)**

postgres=\# **SELECT pg\_size\_pretty (pg\_relation\_size('test'));**  
pg\_size\_pretty  
\----------------  
30 MB  
(1 row)

**\-- Find the total\_index size of the table**

postgres=\# **sELECT pg\_size\_pretty ( pg\_indexes\_size('test'));**  
pg\_size\_pretty  
\----------------  
26 MB  
(1 row)

**\-- Find particular index size:**

postgres=\# **select pg\_size\_pretty(pg\_total\_relation\_size('test\_idx'));**  
pg\_size\_pretty  
\----------------  
19 MB

postgres=\# **select pg\_size\_pretty(pg\_total\_relation\_size('test\_idx2'));**  
pg\_size\_pretty  
\----------------  
6496 kB

**Another method:**

postgres=\# **\\di+ "test\_idx"**

List of relations  
Schema  | Name     | Type  | Owner.  | Table | Size       | Description  
\--------+----------+-------+---------+-------+------------+-------------  
public  | test\_idx | index | dbaprod | test  | 8192 bytes |  
(1 row)

Find list of partitioned table details

**\-- List down all partitioned tables present in db**

SELECT  
nmsp\_parent.nspname AS parent\_schema,  
parent.relname AS parent,  
nmsp\_child.nspname AS child\_schema,  
child.relname AS child  
FROM pg\_inherits  
JOIN pg\_class parent ON pg\_inherits.inhparent \= parent.oid  
JOIN pg\_class child ON pg\_inherits.inhrelid \= child.oid  
JOIN pg\_namespace nmsp\_parent ON nmsp\_parent.oid \= parent.relnamespace  
JOIN pg\_namespace nmsp\_child ON nmsp\_child.oid \= child.relnamespace

**\-- List down all partitions of a single table:**

SELECT  
nmsp\_parent.nspname AS parent\_schema,  
parent.relname AS parent,  
nmsp\_child.nspname AS child\_schema,  
child.relname AS child  
FROM pg\_inherits  
JOIN pg\_class parent ON pg\_inherits.inhparent \= parent.oid  
JOIN pg\_class child ON pg\_inherits.inhrelid \= child.oid  
JOIN pg\_namespace nmsp\_parent ON nmsp\_parent.oid \= parent.relnamespace  
JOIN pg\_namespace nmsp\_child ON nmsp\_child.oid \= child.relnamespace  
WHERE parent.relname='parent\_table\_name';

**Ref link \-** \> https://dba.stackexchange.com/questions/40441/get-all-partition-names-for-a-table

![][image3]  
![][image4]  
**MAINTENANCE**  
Update statistics of a table using analyze

**\-- Analyze stats for a table testanalyze(schema is public)**

dbaclass=\# **analyze testanalyze;**  
ANALYZE

**\-- For analyzing selected columns for emptab table ( schema is dbatest)**

dbaclass=\# **analyze dbatest.emptab (datname,datdba);**  
ANALYZE

dbaclass=\# **select relname,reltuples from pg\_class where relname in ('testanalyze','emptab');**  
relname      | reltuples  
\-------------+-----------  
testanalyze  | 4  
emptab       | 4  
(2 rows)

dbaclass=\# **select schemaname,relname,analyze\_count,last\_analyze,last\_autoanalyze from pg\_stat\_user\_tables where relname in ('testanalyze','emptab');**  
schemaname  | relname     | analyze\_count | last\_analyze                     | last\_autoanalyze  
\------------+-------------+---------------+----------------------------------+------------------  
public      | testanalyze | 1             | 2020-07-21 17:00:49.687053+05:30 |  
dbatest     | emptab      | 1             | 2020-07-21 17:10:01.111517+05:30 |  
(2 rows)

**\---Analyze command with verbose command**

dbaclass=\# **analyze verbose dbatest.emptab (datname,datdba);**

INFO:  analyzing "dbatest.emptab"  
INFO:  "emptab": scanned 1 of 1 pages, containing 4 live rows and 0 dead rows; 4 rows in sample, 4 estimated total rows  
ANALYZE

**\---Analyze tables in the current schema that the user has access to.**

 

dbaclass=\# **analyze ;**  
ANALYZE

***NOTE***: ANALYZE requires only a read lock on the target table, so it can run in parallel with other activity on the table.

Reorg a table using VACUUM command

***VACUUM \- \>  REMOVES DEAD ROWS, AND MARK THEM FOR REUSE, BUT IT DOESN’T RETURN THE SPACE TO ORACLE,. IT DOESN'T NEED EXCLUSIVE LOCK ON THE TABLE.***

\-------------------------------------------------------------------------

**vacuum a table:**

dbaclass=\# **vacuum dbatest.emptab;**  
VACUUM

**both vacuum and analyze:**

dbaclass=\# v**acuum analyze dbatest.emptab;**  
VACUUM

**with verbose:**

dbaclass\# **vacuum verbose analyze dbatest.emptab;**

**Monitor vacuum process( if vacuum process runs for a long time)**

dbaclass\#**select \* from pg\_stat\_progress\_vacuum;**

**Check vacuum related information for the table**

dbaclass=\# **select schemaname,relname,last\_vacuum,vacuum\_count from pg\_stat\_user\_tables where relname='emptab';**  
schemaname  | relname | last\_vacuum                      | vacuum\_count  
\------------+---------+----------------------------------+--------------  
dbatest     | emptab  | 2020-07-21 18:35:34.801402+05:30 | 2  
(1 row)

Reorg a table using VACUUM FULL command

*VACUUM FULL \- \> JUST LIKE MOVE COMMAND IN ORACLE . IT TAKES MORE TIME, BUT IT RETURNS THE SPACE TO OS BECAUSE OF ITS COMPLEX ALGORITHM. IT also requires additional disk space , which can store the new copy of the table., until the activity is completed. Also it locks the table exclusively, which block all operations on the table .*

**\-- Command to run vacuum full command for table:**

dbaclass=\# **VACUUM FULL dbatest.emptab;**  
VACUUM

 

**DEMO TO CHECK HOW IT RECLAIMS SPACE:**

\-- Check existing space and delete some data:  
dbaclass=\# **select pg\_size\_pretty(pg\_relation\_size('dbatest.emptab'));**  
pg\_size\_pretty  
\----------------  
114 MB  
(1 row)

dbaclass=\# **delete from dbatest.emptab where oid=13634;**  
DELETE 131072

\-- We can observe size is still same:

dbaclass=\# **select pg\_size\_pretty(pg\_relation\_size('dbatest.emptab'));**  
pg\_size\_pretty  
\----------------  
114 MB  
(1 row)

\-- Run vacuum full and observe the space usage:

dbaclass=\# **VACUUM FULL dbatest.emptab;**  
VACUUM

dbaclass=\# **select pg\_size\_pretty(pg\_relation\_size('dbatest.emptab'));**  
pg\_size\_pretty  
\----------------  
39 MB \---- \> from 114MB it came down to 39 MB.  
(1 row)

Manage autovacuum process in postgres

**Autovacuum methods automates the executions vacuum,freeze and analyze commands.**

**\-- Find whether autovacuum is enabled or not:**

dbaclass=\# **select name,setting,short\_desc,boot\_val,pending\_restart from pg\_settings where name in ('autovacuum','track\_counts');**  
name.         | setting | short\_desc                                | boot\_val | pending\_restart  
\--------------+---------+-------------------------------------------+----------+-----------------  
autovacuum    | on      | Starts the autovacuum subprocess.         | on        | f  
track\_counts  | on      | Collects statistics on database activity. | on        | f  
(2 rows)

**\-- Find other autovacuum related parameter settings**  
dbaclass=\# **select name,setting,short\_desc,min\_val,max\_val,enumvals,boot\_val,pending\_restart from pg\_settings where category like 'Autovacuum';**

**![][image5]**  
**\- Change autovacuum settings:( they need restart)**

dbaclass=\# **alter system set autovacuum\_max\_workers=10 ;**  
ALTER SYSTEM

**Now restart :**

**pg\_ctl stop**  
**pg\_ctl start**

Rebuild indexes using REINDEX

REINDEX rebuilds an index using the data stored in the index's table, replacing the old copy of the index. There are several scenarios in which to use REINDEX:

**\- Rebuild particular index:**

postgres=\# **REINDEX INDEX TEST\_IDX2;**  
REINDEX

**\-- Rebuild all indexes on a table:**

postgres=\# **REINDEX TABLE TEST;**  
REINDEX

**\-- Rebuild all indexes of tables in a schema:**  
postgres=\# **reindex schema public;**  
REINDEX

**\-- Rebuild all indexes in a database :**

postgres=\# **reindex database dbaclass;**  
REINDEX

**\-- Reindex with verbose option:**

postgres=\# **reindex (verbose) table test;**  
INFO: index "test\_idx" was reindexed  
DETAIL: CPU: user: 5.44 s, system: 2.72 s, elapsed: 11.96 s  
INFO: index "test\_idx2" was reindexed  
DETAIL: CPU: user: 3.34 s, system: 1.01 s, elapsed: 5.49 s  
INFO: index "pg\_toast\_17395\_index" was reindexed  
DETAIL: CPU: user: 0.00 s, system: 0.00 s, elapsed: 0.00 s  
REINDEX

**Rebuild index without causing lock on the table:( using concurrently option)** 

postgres=\# **REINDEX ( verbose) table concurrently test;**  
INFO: index "public.test\_idx" was reindexed  
INFO: index "public.test\_idx2" was reindexed  
INFO: index "pg\_toast.pg\_toast\_17395\_index" was reindexed  
INFO: table "public.test" was reindexed  
DETAIL: CPU: user: 11.09 s, system: 6.23 s, elapsed: 24.63 s.  
REINDEX

Monitor index creation or rebuild

dbaclass=\# **SELECT a.query,p.phase, p.blocks\_total,p.blocks\_done,p.tuples\_total, p.tuples\_done FROM pg\_stat\_progress\_create\_index p JOIN pg\_stat\_activity a ON p.pid \= a.pid;**

\-\[ RECORD 1 \]+-------------------------------  
query        | reindex index test\_idx;  
phase        | building index: scanning table  
blocks\_total | 61281  
blocks\_done  | 15331  
tuples\_total | 0  
tuples\_done  | 0

(or)

dbaclass=\# **select pid,datname,command,phase,tuples\_total,tuples\_done,partitions\_total,partitions\_done from pg\_stat\_progress\_create\_index;**

\-\[ RECORD 1 \]----+-------------------------------  
pid.              | 14944  
datname           | postgres  
command           | REINDEX  
phase             | building index: scanning table  
tuples\_total      | 0  
tuples\_done       | 0  
partitions\_total  | 0  
partitions\_done   | 0

Monitor vacuum operation

postgres\# **select \* from pg\_stat\_progress\_vacuum;**

\-\[ RECORD 1 \]------+--------------------

pid                | 12540

datid              | 21192

datname            | b2cnsmst

relid              | 22402

phase              | cleaning up indexes

heap\_blks\_total.   | 624176

heap\_blks\_scanned  | 624176

heap\_blks\_vacuumed | 624176

index\_vacuum\_count | 0

max\_dead\_tuples    | 178956970

num\_dead\_tuples    | 0

find and change statistics level of a column

**\-- Finding statistics level of a column ( orders.orderdate)**  
*\-- statistics level range is 1-10000 ( where 100 means 1 percent,10000 means 100 percent)*

edbstore=\> **SELECT attname as column\_name , attstattarget as stats\_level FROM pg\_attribute WHERE attrelid \= (SELECT oid FROM pg\_class WHERE relname \= 'orders') and attname='orderdate';**

column\_name  | stats\_level  
\-------------+-------------  
orderdate    | 1000  
(1 row)

**\-- To change statistics level of a column:**

edbstore=\> **alter table orders alter column orderdate set statistics 1000;**  
ALTER TABLE

Find vaccum settings of tables

postgres=\# **SELECT n.nspname, c.relname,**  
**pg\_catalog.array\_to\_string(c.reloptions || array(**  
**select 'toast.' ||**  
**x from pg\_catalog.unnest(tc.reloptions) x),', ')**  
**as relopts**  
**FROM pg\_catalog.pg\_class c**  
**LEFT JOIN**  
**pg\_catalog.pg\_class tc ON (c.reltoastrelid \= tc.oid)**  
**JOIN**  
**pg\_namespace n ON c.relnamespace \= n.oid**  
**WHERE c.relkind \= 'r'**  
**AND nspname NOT IN ('pg\_catalog', 'information\_schema');**

nspname  | relname                             | relopts  
\---------+-------------------------------------+------------------------  
public   | test                                |  
public   | city\_id2                            |  
public   | test2                               | autovacuum\_enabled=off

 

Modify autovacuum setting of table/index

**\-- Disable autovacuum for a table:**

postgres=\# **alter table test2 set( autovacuum\_enabled \= off);**

**\-- Enable autovacuum for a table**

postgres=\# **alter table test2 set( autovacuum\_enabled \= on);**

   
Find last vaccum/analyze details of a table

**\-- Here the table\_name is test**

postgres=\# **select \* from pg\_stat\_user\_tables where relname='test';**  
\-\[ RECORD 1 \]-------+---------------------------------  
relid               | 914713  
schemaname          | public  
relname             | test  
seq\_scan            | 40  
seq\_tup\_read        | 12778861  
idx\_scan            |  
idx\_tup\_fetch       |  
n\_tup\_ins           | 4774377  
n\_tup\_upd           | 0  
n\_tup\_del           | 4774377  
n\_tup\_hot\_upd       | 0  
n\_live\_tup          | 0  
n\_dead\_tup          | 0  
n\_mod\_since\_analyze | 0  
**last\_vacuum         |**  
**last\_autovacuum     | 22-APR-22 21:27:10.863536 \+03:00**  
**last\_analyze        | 22-APR-22 21:05:05.874929 \+03:00**  
**last\_autoanalyze.   | 22-APR-22 21:27:10.865308 \+03:00**  
vacuum\_count        | 0  
autovacuum\_count    | 6  
analyze\_count       | 2  
autoanalyze\_count   | 11

Find how much bloating a table has

**\-- Create the pgstattuple extension:**

postgres=\# **create extension pgstattuple;**  
CREATE EXTENSION

**\-- bloating percentage of the table "test":**

postgres=\# **SELECT pg\_size\_pretty(pg\_relation\_size('test')) as table\_size,(pgstattuple('test')).dead\_tuple\_percent;**  
table\_size. | dead\_tuple\_percent  
\------------+--------------------  
1408 kB     | 0  
(1 row)

**\-- bloating percentage of index "test\_x\_idx":**

**select pg\_relation\_size('test\_x\_idx') as index\_size, 100-(pgstatindex('test\_x\_idx')).avg\_leaf\_density as bloat\_ratio;**

index\_size. | bloat\_ratio  
\------------+--------------------  
1008 kB     | 0  
(1 row)

![][image6]  
**USER MANAGEMENT**  
List users present in postgres

\--- List users present in postgres:

postgres=\# **select usename,usesuper,valuntil from pg\_user;**  
usename         | usesuper   | valuntil  
\---------------+----------+---------------------------  
postgres       | t           |  
test\_dbuser1   | f            | 2020-08-08 00:00:00+05:30

postgres\#**select usename,usesuper,valuntil from pg\_shadow;**

usename         | usesuper   | valuntil  
\---------------+----------+---------------------------  
postgres       | t           |  
test\_dbuser1   | f            | 2020-08-08 00:00:00+05:30

postgres\#select usename,usesuper,valuntil from pg\_shadow;

postgres=\# **\\du**  
List of roles  
Role name      | Attributes                                                 | Member of  
\---------------+------------------------------------------------------------+-----------  
postgres       | Superuser, Create role, Create DB, Replication, Bypass RLS | {}  
test\_dbuser1   | Password valid until 2020-08-08 00:00:00+05:30             | {}

 

***NOTE** \- \> \\du command output includes both user and roles(custom created roles only).*

*postgres users are bydefault role, but roles are not bydefault user.*

List roles present in postgres

**List roles :**

postgres=\# **select rolname,rolcanlogin,rolvaliduntil from pg\_roles;**  
rolname               | rolcanlogin | rolvaliduntil  
\----------------------+-------------+---------------------------  
pg\_monitor            | f           |  
pg\_read\_all\_settings  | f           |  
pg\_read\_all\_stats     | f           |  
pg\_stat\_scan\_tables   | f           |  
pg\_signal\_backend     | f           |  
postgres              | t           |  
test\_dbuser1          | f           | 2020-08-08 00:00:00+05:30

 

**rolcanlogin** \- \> If **true** mean they are role as well as user  
                If **false** mean they are only role( they cannot login)

***NOTE** \- \> In postgres users are bydefault role, but roles are not bydefault user. i.e*

*Bydefault user come with login privilege, where as roles don’t come with login privilege.*

create/drop user in postgres

**CREATE USER:**

dbaclass=\# **create user TEST\_DBACLASS with password 'test123';**  
CREATE ROLE

**CREATE USER WITH VALID UNTIL:**

dbaclass=\# **create user TEST\_dbuser1 with password 'test123' valid until '2020-08-08';**  
CREATE ROLE

**CREATE USER WITH SUPER USER PRIVILEGE**

dbaclass=\# **create user test\_dbuser3 with password 'test123' CREATEDB SUPERUSER;**

CREATE ROLE

**VIEW USERS:**

dbaclass=\# **select usename,valuntil,usecreatedb from pg\_shadow;**

dbaclass=\# **select usename,usesuper,valuntil from pg\_user;**

dbaclass=\# **\\du+**

**DROP USER:**

**drop user DB\_user1;**

Create/drop role in postgres

**\- Create role :**

dbaclass=\# **create role dev\_admin;**  
CREATE ROLE

dbaclass=\# **create role dev\_admin with valid until '10-oct-2020';**  
CREATE ROLE

\-- role with createdb and superuser privilege and login keyword mean it can login to db like a normal user

dbaclass=\# **create role dev\_admin with createdb createrole login ;**  
CREATE ROLE

DROP ROLE:

dbaclass=\# **drop role dev\_admin;**  
DROP ROLE

**select rolname,rolcanlogin,rolvaliduntil from pg\_roles;**

Alter an user in postgres

**\-- Rename a user:**

postgres=\# **alter user dbatest rename to dbaprod;**  
NOTICE: MD5 password cleared because of role rename  
ALTER ROLE  
postgres=\# **\\du**  
List of roles  
Role name  | Attributes                                                 | Member of  
\-----------+------------------------------------------------------------+-----------  
dbaprod   |                                                             | {}  
postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS  | {}

\<\< NOTE \- AFTER renaming the user, you need to reset the password to same old one.

i.e  
**\-- Change the password a user:**

postgres=\# **alter user dbaprod password 'test';**  
ALTER ROLE

**\--- Increase the validity of the user:**

postgres=\# **alter user dbaprod valid until 'Feb 10 2021';**  
ALTER ROLE

Convert a user to superuser

**\-- providing superuser role will make an user superuser.**

postgres=\# **select usename,usesuper from pg\_user where usename='dbatest';**  
usename  | usesuper  
\---------+----------  
dbatest  | f  
(1 row)

postgres=\#  
postgres=\# **alter user dbatest with superuser;**  
ALTER ROLE  
postgres=\# **select usename,usesuper from pg\_user where usename='dbatest';**  
usename  | usesuper  
\---------+----------  
dbatest  | t

**\-- How to revoke superuser:**

postgres=\# **alter user dbatest with nosuperuser;**  
ALTER ROLE  
postgres=\#  
postgres=\# **select usename,usesuper from pg\_user where usename='dbatest';**  
usename  | usesuper  
\---------+----------  
dbatest  | f  
(1 row)

Set password to original one without knowing

\--- Lets say you forgot the password of the user and u want to set the same password to that user in same or different db 

**1\. Set a password for the user dbaprod**  
postgres=\# **alter user dbaprod password 'old';**  
ALTER ROLE  
postgres=\#

**2.Note down the encrypted password**  
postgres=\# **SELECT rolname, rolpassword FROM pg\_catalog.pg\_authid where rolname='dbaprod';**  
rolname | rolpassword  
\---------+-------------------------------------  
dbaprod | md5bbb103edd695a83d45db75755e459a78 \-- \> NOTE DOWN THIS ONE  
(1 row)

**3\. Change the password and check the encrypted password**

postgres=\# **alter user dbaprod password 'new';**  
ALTER ROLE

postgres=\# **SELECT rolname, rolpassword FROM pg\_catalog.pg\_authid where rolname='dbaprod';**  
rolname | rolpassword  
\---------+-------------------------------------  
dbaprod | md5041382740aeba232404af81454f48d7f ( it has been changed)

**4.Now update this rolpassword with the value we got at step 2**

postgres=\# **update pg\_catalog.pg\_authid set rolpassword \= 'md5bbb103edd695a83d45db75755e459a78' where rolname='dbaprod';**  
UPDATE 1

Now try to connect to the database using the first password 'old'

postgres$ **PGPASSWORD=old ./psql \-d postgres \-U dbaprod**  
Password:  
psql (12.3)  
Type "help" for help.

GRANT privilege commands

**Examples on GRANT command**

**GRANT CONNECT ON DATABASE PRIMDB to DBAUSER1;**

**GRANT USAGE ON SCHEMA CRM to DBAUSER1;**

**GRANT INSERT,UPDATE,DELETE ON TABLE CRM.EMPTAB TO DBAUSER1;**

**GRANT ALL ON TABLE  CRM.EMPTAB TO DBAUSER1;**

**GRANT CREATE ALL ON DATABASE CRM to DBAUSER2;**

**GRANT CREATE ON TABLESPACE INV\_TS to DBAUSER2;**

**GRANT ALL ON TABLESPACE INV\_TS TO DBAUSER2;**

**GRANT CREATE ON TABLESPACE INV\_TS to DBAUSER2 with grant option:**

**GRANT EXECUTE ON PROCEDURE PRIM\_ID.TEST\_PROC;**

**GRANT EXECUTE ON FUNCTION PRIM\_ID.TEST\_FUNC;**

 

**for more commands: use the help command**

**\#\\h GRANT**

REVOKE privilege commands

**Examples on REVOKE command**

**REVOKE CONNECT ON DATABASE PRIMDB FROM DBAUSER1;**

**REVOKE USAGE ON SCHEMA CRM FROM DBAUSER1;**

**REVOKE INSERT,UPDATE,DELETE ON TABLE CRM.EMPTAB FROM DBAUSER1;**

**REVOKE ALL ON TABLE CRM.EMPTAB FROM DBAUSER1;**

**REVOKE CREATE ALL ON DATABASE CRM FROM DBAUSER2;**

**REVOKE CREATE ON TABLESPACE INV\_TS FROM DBAUSER2;**

**REVOKE ALL ON TABLESPACE INV\_TS FROM DBAUSER2;**

**REVOKE CREATE ON TABLESPACE INV\_TS FROM DBAUSER2 ;**

**REVOKE EXECUTE ON PROCEDURE PRIM\_ID.TEST\_PROC FROM DBAUSER2;**

**REVOKE EXECUTE ON FUNCTION PRIM\_ID.TEST\_FUNC FROM DBAUSER2;**

**for more commands: use the help command**

**\#\\h REVOKE**

Create user profile \- EDB Postgres

\--- in edb postgres advanced server we can create user profile  
\---- similar to that of oracle.

\-- Create profile:

\# **create profile REPORTING\_PROFILE limit FAILED\_LOGIN\_ATTEMPTS 3 PASSWORD\_LIFE\_TIME 90;**

\--- Alter profile:

\# **alter profile REPORTING\_PROFILE limit FAILED\_LOGIN\_ATTEMPTS 1;**

\-- view profile details:

\# **select \* from dba\_profiles;**

Create/Drop schema in postgres

**\-- Create schema:**

postgres=\# **create schema dba\_schema;**  
CREATE SCHEMA

**\-- Create schema with authorize particular user:**

postgres=\# **create schema dba\_schema authorization raj2;**  
CREATE SCHEMA

**\-- Drop schema**

postgres=\# **drop schema dba\_schema;**  
DROP SCHEMA

**\-- List down schemas present**

postgres=\# **\\dn+**  
List of schemas  
Name        | Owner.   | Access privileges    | Description  
\------------+----------+----------------------+------------------------  
dba\_schema | raj2      |                      |  
public     | postgres  | postgres=UC/postgres+| standard public schema| | \=UC/postgres |  
raj.       | postgres | |  
(3 rows)

Find search\_path setting of users

**\-- Find search\_path of users in a particular database ( replace your db\_name(EDB))**

**SELECT r.rolname, d.datname, drs.setconfig**  
**FROM pg\_db\_role\_setting drs**  
**LEFT JOIN pg\_roles r ON r.oid \= drs.setrole**  
**LEFT JOIN pg\_database d ON d.oid \= drs.setdatabase**  
**WHERE d.datname \= 'EDB';**

**\-- Find search\_path of users in postgres db cluster( all database)**

SELECT r.rolname, d.datname, drs.setconfig  
FROM pg\_db\_role\_setting drs  
LEFT JOIN pg\_roles r ON r.oid \= drs.setrole  
LEFT JOIN pg\_database d ON d.oid \= drs.setdatabase; 

Set search\_path of a user in postgres

**\-- set search\_path for a user in particular db:**

postgres\# **alter user prod\_user in database "EDB" set search\_path="$user", public, prim\_db;**

**\-- set search\_path for a user in postgres cluster( all dbs)**

postgres=\# **alter user prod\_user set search\_path="$user", public, prim\_db;**

Find privileges granted to a user postgres

**\-- List down table level privileges of user**  
**SELECT table\_catalog, table\_schema, table\_name, privilege\_type**  
**FROM information\_schema.table\_privileges**  
**WHERE grantee \= 'USER\_NAME';**

**\-- List down usage privileges of a user:**

**select \* from usage\_privileges where grantee='USER\_NAME';**

Find the roles granted to user/role

**SELECT**

**r.rolname,**

**ARRAY(SELECT b.rolname**

**FROM pg\_catalog.pg\_auth\_members m**

**JOIN pg\_catalog.pg\_roles b ON (m.roleid \= b.oid)**

**WHERE m.member \= r.oid) as memberof**

**FROM pg\_catalog.pg\_roles r**

**ORDER BY 1;**  
Find how much bloating a table has

**\-- Create the pgstattuple extension:**

postgres=\# **create extension pgstattuple;**  
CREATE EXTENSION

**\-- bloating percentage of the table "test":**

postgres=\# **SELECT pg\_size\_pretty(pg\_relation\_size('test')) as table\_size,(pgstattuple('test')).dead\_tuple\_percent;**  
table\_size. | dead\_tuple\_percent  
\------------+--------------------  
1408 kB     | 0  
(1 row)

**\-- bloating percentage of index "test\_x\_idx":**

**select pg\_relation\_size('test\_x\_idx') as index\_size, 100-(pgstatindex('test\_x\_idx')).avg\_leaf\_density as bloat\_ratio;**

index\_size. | bloat\_ratio  
\------------+--------------------  
1008 kB     | 0  
(1 row)

![][image7]  
![][image8]  
**TABLESPACE MANAGEMENT**  
View tablespace info in postgres

**VIEW TABLESPACE INFO IN POSTGRES:**

 

postgres=\# **select \* from pg\_tablespace;**

(OR)

postgres=\# **\\db+**

(or)

**\-- For getting size of specific tablespace:**

postgres=\# **select pg\_size\_pretty(pg\_tablespace\_size('ts\_dbaclass'));**

pg\_size\_pretty

\----------------

96 bytes

(1 row)

*Pre-configured tablespaces:( these are default tablespaces)*

*Pg\_global \- \> PGDATA/global \- \> used for cluster wide table and system catalog*  
*Pg\_default \- \> PGDATA/base directory \- \> it stores databases and relations*

create/drop/rename tablespace in postgres

**CREATE TABLESPACE:**  
postgres=\# **create tablespace ts\_postgres location '/Library/PostgreSQL/TEST/TS\_POSTGRES';**  
CREATE TABLESPACE

**RENAME TABLESPACE:**  
postgres=\# **alter tablespace ts\_postgres rename to ts\_dbaclass;**  
ALTER TABLESPACE

**DROP TABLESPACE:**

postgres=\# **drop tablespace ts\_dbaclass;**  
DROP TABLESPACE

 

\<\<Before dropping tablespace make sure it is emptry\>\>

find/change default tablespace

postgres=\# **show default\_tablespace;**  
default\_tablespace  
\--------------------

(1 row)  
\<\<\<\< If output is blank means default is **pg\_default** tablespace\>\>\>\>\>

**\--To change the default tablespace at database level:**

postgres=\# **alter system set default\_tablespace=ts\_postgres;**  
ALTER SYSTEM

postgres=\# **select pg\_reload\_conf();**  
pg\_reload\_conf  
\----------------  
t  
(1 row)

postgres=\# **show default\_tablespace;**  
default\_tablespace  
\--------------------  
ts\_postgres  
(1 row)

postgres=\# **SELECT name, setting FROM pg\_settings where name='default\_tablespace';**  
name | setting  
\--------------------+-------------  
default\_tablespace | ts\_postgres  
(1 row)

 

**Steps to change default tablespace at session level:**

postgres=\# **set default\_tablespace=ts\_postgres;**  
SET

find/change default temp tablespace

**VIEW DEFAULT TEMP TABLESPACE:**  
dbaclass=\# **SELECT name, setting FROM pg\_settings where name='temp\_tablespaces';**  
name | setting  
\------------------+---------  
temp\_tablespaces |  
(1 row)

dbaclass=\# **show temp\_tablespaces**  
dbaclass-\# ;  
temp\_tablespaces  
\------------------

(1 row)

**CHANGE DEFAULT TEMP TABLESPACE**

postgres=\# **alter system set temp\_tablespaces=TS\_TEMP;**  
ALTER SYSTEM

postgres=\# **select pg\_reload\_conf();**  
pg\_reload\_conf  
\----------------  
t  
(1 row)

postgres=\# **show temp\_tablespaces;**  
temp\_tablespaces  
\------------------  
ts\_temp  
(1 row)

postgres=\# **SELECT name, setting FROM pg\_settings where name='temp\_tablespaces';**  
name | setting  
\------------------+---------  
temp\_tablespaces | ts\_temp  
(1 row)

How to change tablespace owner

***\-- Change ownership of tablespace ts\_postgres to user dev\_admin***

postgres\# **alter tablespace ts\_postgres owner to dev\_admin;**

postgres\# **\\db+**

Move table/index to different tablespace

**Move table/index to different tablespace**

\-- move table to different tablespace  
prod\_crm=\# **alter table TEST8 set tablespace pg\_crm;**  
ALTER TABLE

\-- Move index to different tablespace

prod\_crm=\# **alter index TEST\_ind set tablespace pg\_crm;**  
ALTER TABLE

 

Move database to new tablespace in postgres

postgres=\# **alter database prod\_crm set tablespace crm\_tblspc;**

Before running this. make sure there are no active connections in the database.

You can kill the existing session using below query.

**postgres\# select pg\_terminate\_backend(pid) from pg\_stat\_activity where datname='DB\_NAME';**

![][image9]  
**BACKUP & RECOVERY**  
export table data to file using COPY

\-- export specific column data to text file:

**copy EMPLOYEE( EMP\_NAME,EMP\_ID) to '/tmp/emp.txt';**

\-- export complete table data to text file:

**copy EMPLOYEE to '/tmp/emp.txt';**

\-- export table data to csv file:

**copy EMPLOYEE to '/tmp/emp.csv' with csv headers;**

\-- export specific query output to csv file:

**copy ( select ename,depname from emp where depname='HR') to '/tmp/emp.csv' with csv headers;**

![][image10]  
![][image11]  
**AUDITING & SECURITY**  
Find pg\_hba.conf file content

**\-- This provides a summary of contents of client authentication config file pg\_hba.conf** 

postgres=\# **select \* from pg\_hba\_file\_rules;**  
line\_number | type | database | user\_name | address | netmask | auth\_method | options | error  
\-------------+-------+---------------+-----------+---------------+-----------------------------------------+-------------+---------+-------  
80 | local | {all} | {all} | | | md5 | |  
82 | host | {all} | {all} | 127.0.0.1 | 255.255.255.255 | ident | |  
84 | host | {all} | {all} | ::1 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff | ident | |  
85 | host | {all} | {all} | 172.21.19.148 | 255.255.255.255 | trust | |  
88 | local | {replication} | {all} | | | peer | |  
89 | host | {replication} | {all} | 127.0.0.1 | 255.255.255.255 | ident | |  
90 | host | {replication} | {all} | ::1 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff | ident | |  
(7 rows)

Enable auditing for ddl/dml statement

**\-- Check auditing setting :**

postgres=\# **show log\_statement;**  
log\_statement  
\---------------  
none

**\-- For logging all ddl activites:**

postgres=\# **alter system set log\_statement=ddl;**  
ALTER SYSTEM  
postgres=\# select pg\_reload\_conf();  
pg\_reload\_conf  
\----------------  
t

**\-- For logging all DDL DML activities:**

postgres=\# **alter system set log\_statement=mod;**  
ALTER SYSTEM  
postgres=\# select pg\_reload\_conf();  
pg\_reload\_conf  
\----------------  
t

**\-- For logging all statement( i.e ddl , dml and even select statements)**

postgres=\# **alter system set log\_statement='all';**  
ALTER SYSTEM  
postgres=\# select pg\_reload\_conf();  
pg\_reload\_conf  
\----------------  
t

Enable audit for log on/log off to postgres

**\- Enable audit for connection and disconnection to postgres.**

postgres=\# **select name,setting from pg\_settings where name in ('log\_disconnections','log\_connections');**  
name                | setting  
\--------------------+---------  
log\_connections     | off  
log\_disconnections  | off

postgres=\# **alter system set log\_disconnections=off;**  
ALTER SYSTEM

postgres=\# **alter system set log\_connections=on;**  
ALTER SYSTEM

postgres=\# **select pg\_reload\_conf();**  
pg\_reload\_conf  
\----------------  
t

Now all log on and log off will logged in the log file.

\<\<\<\<\<\<\<\<\< cd /Library/PostgreSQL/10/data/log/ \>\>\>\>\>\>\>  
2020-07-06 12:51:39.042 IST \[10212\] LOG: connection received: host=\[local\]  
2020-07-06 12:51:53.416 IST \[10215\] LOG: connection received: host=\[local\]  
2020-07-06 12:51:53.420 IST \[10215\] LOG: connection authorized: user=postgres database=postgres  
![][image12]  
**NETWORK**  
Find foreign server details

postgres=\# **\\des+**  
List of foreign servers  
Name  | Owner | Foreign-data wrapper | Access privileges | Type | Version | FDW options | Description  
\------+-------+----------------------+-------------------+------+---------+-------------+-------------  
(0 rows)

postgres=\# **select srvname,srvowner,srvoptions,fdwname,srvversion,srvtype from pg\_foreign\_server join pg\_foreign\_data\_wrapper b on b.oid=srvfdw;**  
srvname  | srvowner | srvoptions | fdwname | srvversion | srvtype  
\---------+----------+------------+---------+------------+---------  
(0 rows)

Find list of foreign tables

postgres=\# **\\det+**  
List of foreign tables  
Schema | Table | Server | FDW options | Description  
\--------+-------+--------+-------------+-------------  
(0 rows)

 

postgres=\# **SELECT n.nspname AS "Schema",**  
**c.relname AS "Table",**  
**s.srvname AS "Server",**  
**CASE WHEN ftoptions IS NULL THEN '' ELSE '(' || pg\_catalog.array\_to\_string(ARRAY(SELECT pg\_catalog.quote\_ident(option\_name) || ' ' || pg\_catalog.quote\_literal(option\_value) FROM pg\_catalog.pg\_options\_to\_table(ftoptions)), ', ') || ')' END AS "FDW options",**  
**d.description AS "Description"**  
**FROM pg\_catalog.pg\_foreign\_table ft**  
**INNER JOIN pg\_catalog.pg\_class c ON c.oid \= ft.ftrelid**  
**INNER JOIN pg\_catalog.pg\_namespace n ON n.oid \= c.relnamespace**  
**INNER JOIN pg\_catalog.pg\_foreign\_server s ON s.oid \= ft.ftserver**  
**LEFT JOIN pg\_catalog.pg\_description d**  
**ON d.classoid \= c.tableoid AND d.objoid \= c.oid AND d.objsubid \= 0**  
**WHERE pg\_catalog.pg\_table\_is\_visible(c.oid)**  
**ORDER BY 1, 2;**

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*  
List of foreign tables  
Schema  | Table | Server | FDW options | Description  
\--------+-------+--------+-------------+-------------  
(0 rows)

List foreign data wrapper

postgres=\# **\\dew**

List of foreign-data wrappers

Name.         | Owner        | Handler            | Validator

\--------------+--------------+--------------------+--------------------------

dblink\_fdw    | enterprisedb | \-                  | dblink\_fdw\_validator

libpq\_dblink  | enterprisedb | libpq\_fdw\_handler  | edb\_dblink\_fdw\_validator

oci\_dblink    | enterprisedb | oci\_fdw\_handler.   | edb\_dblink\_fdw\_validator

oracle\_fdw.   | enterprisedb | oracle\_fdw\_handler | oracle\_fdw\_validator

(4 rows)  
Find user mapping for FDW

postgres=\# \\deu+

List of user mappings  
Server.    | User name    | FDW options  
\-----------+--------------+----------------------------------------  
pg\_remote  | enterprisedb | ("user" 'dba\_raj', password 'dba\_raj')  
(1 row)

 

Create database link between postgres dbs

**1\. Create extension:**

postgres\# create extension dblink;

**2\. Create foreign server:( use the target postgres details.**

postgres=\# CREATE SERVER oracle\_dblink FOREIGN DATA WRAPPER dblink\_fdw OPTIONS ( host '10.21.120.131' ,dbname 'postgres' , port '5444');

**3\. Create user mapping details.**

postgres=\# CREATE USER MAPPING FOR enterprisedb SERVER oracle\_dblink OPTIONS ( user 'dba\_raj' ,password 'dba\_raj');

**4\. Test the database link:**

postgres=\# SELECT dblink\_connect('my\_new\_conn', 'oracle\_dblink');

dblink\_connect  
\----------------  
OK  
(1 row)

**5\. Fetch data using db\_link:**

postgres=\# select \* from dblink('oracle\_dblink','select object\_name from test') as test\_object(object\_name varchar );  
object\_name  
\---------------------------------------------------  
PG\_AGGREGATE\_FNOID\_INDEX  
PG\_AM\_NAME\_INDEX  
PG\_AM\_OID\_INDEX  
PG\_AMOP\_FAM\_STRAT\_INDEX

Create/Modify foreign server

**1\. Create foreign server:( use the target postgres details.**

postgres=\# **CREATE SERVER oracle\_dblink FOREIGN DATA WRAPPER dblink\_fdw OPTIONS ( host '10.21.120.131' ,dbname 'postgres' , port '5444');**

**2\. Add new parameter to existing foreign server:**

postgres=\# **ALTER SERVER oracle\_dblink options ( ADD port '5444');**

**3\. Modify parameters in foreign server:**

postgres=\# **ALTER SERVER oracle\_dblink options ( SET port '5432');**

**4\. Drop foreign server:**

postgres=\# **DROP SERVER oracle\_dblink CASCADE;**

![][image13]  
![][image14]  
**REPLICATION**  
Check streaming recovery status

**\-- Run this on hot standby server** 

**postgres=\# select pg\_is\_wal\_replay\_paused();**

**pg\_is\_wal\_replay\_paused**  
**\-------------------------**  
**f**

 

**\-- If the output is f then, streaming recovery is running, if t means not running.**

Check replication details on primary server

\-- Run on this primary server for outgoing replication details

postgres=\# **select \* from pg\_stat\_replication;**  
\-\[ RECORD 1 \]----+---------------------------------  
pid              | 18556  
usesysid.        | 10  
usename          | enterprisedb  
application\_name | walreceiver  
client\_addr      | 10.20.76.12  
client\_hostname  |  
client\_port      | 44244  
backend\_start    | 27-MAY-21 13:56:30.131681 \+03:00  
backend\_xmin     |  
state            | streaming  
sent\_lsn.        | 0/401F658  
write\_lsn        | 0/401F658  
flush\_lsn        | 0/401F658  
replay\_lsn.      | 0/401F658  
write\_lag        |  
flush\_lag        |  
replay\_lag.      |  
sync\_priority.   | 0  
sync\_state       | async

Get received /replayed WAL records on standby(replication)

***\-- Run on standby database***

postgres=\# **select pg\_last\_wal\_receive\_lsn(), pg\_last\_wal\_replay\_lsn(), pg\_last\_xact\_replay\_timestamp();**  
\-\[ RECORD 1 \]-----------------+---------------------------------  
pg\_last\_wal\_receive\_lsn       | 0/401F658  
pg\_last\_wal\_replay\_lsn        | 0/401F658  
pg\_last\_xact\_replay\_timestamp | 27-MAY-21 16:26:18.704299 \+03:00

postgres=\# **select \* from pg\_stat\_wal\_receiver;**  
\-\[ RECORD 1 \]---------+------------------------------------------------------------------------  
pid                   | 7933  
status                | streaming  
receive\_start\_lsn     | 0/4000000  
receive\_start\_tli     | 1  
received\_lsn          | 0/401F658  
received\_tli          | 1  
last\_msg\_send\_time.   | 27-MAY-21 20:29:39.599389 \+03:00  
last\_msg\_receipt\_time | 27-MAY-21 20:29:39.599599 \+03:00  
latest\_end\_lsn.       | 0/401F658  
latest\_end\_time       | 27-MAY-21 16:31:20.815183 \+03:00  
slot\_name             |  
sender\_host           | 10.20.30.40  
sender\_port           | 5444  
conninfo | user=enterprisedb passfile=/bgidata/enterprisedb/.pgpass dbname=replication host=10.20.30.40 port=5444 fallback\_application\_name=walreceiver sslmode=prefer sslcompression=0 krbsrvname=postgres target\_session\_attrs=any

How to stop /resume recovery in standy(replication)

***\-- Change ownership of tablespace ts\_postgres to user dev\_admin***

postgres\# **alter tablespace ts\_postgres owner to dev\_admin;**

postgres\# **\\db+**

Find lag in streaming replication

**\-- Find lag in bytes( run on standby)**

postgres\# **SELECT pg\_wal\_lsn\_diff(sent\_lsn, replay\_lsn) from pg\_stat\_replication;**

**\--- Find lag in seconds( run on standby)**

postgres\# **SELECT CASE WHEN pg\_last\_wal\_receive\_lsn() \=**  
**pg\_last\_wal\_replay\_lsn()**  
**THEN 0 ELSE**  
**EXTRACT (EPOCH FROM now() \- pg\_last\_xact\_replay\_timestamp()) END AS lag\_seconds;**

Manage replication slots

***\-- Check existing replication slot details***

postgres\# **SELECT redo\_lsn, slot\_name,restart\_lsn, active,**  
**round((redo\_lsn-restart\_lsn) / 1024 / 1024 / 1024, 2\) AS GB\_lag**  
**FROM pg\_control\_checkpoint(), pg\_replication\_slots;**

***\-- Create replication slots***

postgres\#**SELECT pg\_create\_physical\_replication\_slot('slot\_one');**

***\-- Drop unused replication slots***

postgres=\# **SELECT pg\_drop\_replication\_slot('slot\_one');**

Find subscription details in logic replication

select \* from pg\_stat\_subscription;

![][image15]  
**PERFORMANCE**  
Find foreign server details

postgres=\# **\\des+**  
List of foreign servers  
Name  | Owner | Foreign-data wrapper | Access privileges | Type | Version | FDW options | Description  
\------+-------+----------------------+-------------------+------+---------+-------------+-------------  
(0 rows)

postgres=\# **select srvname,srvowner,srvoptions,fdwname,srvversion,srvtype from pg\_foreign\_server join pg\_foreign\_data\_wrapper b on b.oid=srvfdw;**  
srvname  | srvowner | srvoptions | fdwname | srvversion | srvtype  
\---------+----------+------------+---------+------------+---------  
(0 rows)

![][image16]  
![][image17]  
**GENERIC**  
Find autocommit setting in postgres

**Bydefault autocommit is set to on in postgres. You can check the setting .**

postgres\# **\\echo :AUTOCOMMIT;**  
ON;

At session level you can change the autocommit setting :

postgres\# **\\set AUTOCOMMIT OFF**

Run a query repeatedly automatically

\-- You can use watch command to run a particular query repeatedly until you cancel it.  
\-- watch 3 , means for every 3 seconds, the previous query will be executed 

postgres=\# **select count(\*) from test;**  
count  
\-------  
4226  
(1 row)

postgres=\# **\\watch 3**  
Tue 19 Apr 2022 08:18:17 PM \+03 (every 3s)

count  
\-------  
4226  
(1 row)

Tue 19 Apr 2022 08:18:20 PM \+03 (every 3s)

count  
\-------  
4226  
(1 row)

# **The daily checklist of a PostgreSQL DBA**

---

We often get this question, What are the most important things a PostgreSQL DBA should do to guarantee optimal performance and reliability, Do we have checklist for PostgreSQL DBAs to follow daily ? Since we are getting this question too often, Thought let’s note it as blog post and share with community of PostgreSQL ecosystem. The only objective this post is to share the information, Please don’t consider this as a run-book or recommendation from MinervaDB PostgreSQL support. We at MinervaDB are not accountable of any negative performance in you PostgreSQL performance with running these scripts in production database infrastructure of your business, The following is a simple daily checklist for PostgreSQL DBA:

**Task 1:** Check that all the PostgreSQL instances are up and operational:

Shell

| 1 | pgrep \-u postgres \-fa \-- \-D |
| :---: | :---- |

What if you have several instances of PostgreSQL are running:

Shell

| 1 | pgrep \-fa \-- \-D |grep postgres |
| :---: | :---- |

**Task 2:** Monitoring PostgreSQL logsRecord PostgreSQL error logs: Open postgresql.conf configuration file, Under the **ERROR REPORTING AND LOGGING** section of the file, use following config parameters:

Shell

| 1 2 3 4 5 6 7 8 9 10 11 | log\_destination \= 'stderr' logging\_collector \= on log\_directory \= 'pg\_log' log\_filename \= 'postgresql-%Y-%m-%d\_%H%M%S.log' log\_truncate\_on\_rotation \= off log\_rotation\_age \= 1d log\_min\_duration\_statement \= 0 log\_connections \= on log\_duration \= on log\_hostname \= on log\_timezone \= 'UTC' |
| :---: | :---- |

Save the **postgresql.conf** file and restart the PostgreSQL server:

Shell

| 1 | sudo service postgresql restart |
| :---: | :---- |

**Task 3:** Confirm PostgreSQL backup completed successfully

Use backup logs (possible only with PostgreSQL logical backup) to audit backup quality:

Shell

| 1 | $ pg\_dumpall \> /backup\-path/pg\-backup\-dump.sql \> /var/log/postgres/pg\-backup\-dump.log |
| :---: | :---- |

**Task 4:** Monitoring PostgreSQL Database Size:

Shell

| 1 | select datname, pg\_size\_pretty(pg\_database\_size(datname)) from pg\_database order by pg\_database\_size(datname); |
| :---: | :---- |

**Task 5:** Monitor all PostgreSQL queries running (please repeat this task every 90 minutes during business / peak hours):

Shell

| 1 2 3 4 | SELECT pid, age(clock\_timestamp(), query\_start), usename, query FROM pg\_stat\_activity WHERE query \!= '\<IDLE\>' AND query NOT ILIKE '%pg\_stat\_activity%' ORDER BY query\_start desc; |
| :---: | :---- |

**Task 6:** Inventory of indexes in PostgreSQL database:

Shell

| 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 | select     t.relname as table\_name,     i.relname as index\_name,     string\_agg(a.attname, ',') as column\_name from     pg\_class t,     pg\_class i,     pg\_index ix,     pg\_attribute a where     t.oid \= ix.indrelid     and i.oid \= ix.indexrelid     and a.attrelid \= t.oid     and a.attnum \= ANY(ix.indkey)     and t.relkind \= 'r'     and t.relname not like 'pg\_%' group by       t.relname,     i.relname order by     t.relname,     i.relname; |
| :---: | :---- |

**Task 7:** Finding the largest databases in your PostgreSQL cluster:

| 1 2 3 4 5 6 7 8 9 10 11 12 | SELECT d.datname as Name,  pg\_catalog.pg\_get\_userbyid(d.datdba) as Owner,     CASE WHEN pg\_catalog.has\_database\_privilege(d.datname, 'CONNECT')         THEN pg\_catalog.pg\_size\_pretty(pg\_catalog.pg\_database\_size(d.datname))         ELSE 'No Access'     END as Size FROM pg\_catalog.pg\_database d     order by     CASE WHEN pg\_catalog.has\_database\_privilege(d.datname, 'CONNECT')         THEN pg\_catalog.pg\_database\_size(d.datname)         ELSE NULL     END desc \-- nulls first     LIMIT 20 |
| :---: | :---- |

**Task 8:** when you are suspecting some serious performance bottleneck in PostgreSQL ? Especially when you suspecting transactions blocking each other:

| 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 | WITH RECURSIVE l AS (   SELECT pid, locktype, mode, granted, ROW(locktype,database,relation,page,tuple,virtualxid,transactionid,classid,objid,objsubid) obj   FROM pg\_locks ), pairs AS (   SELECT w.pid waiter, l.pid locker, l.obj, l.mode   FROM l w   JOIN l ON l.obj IS NOT DISTINCT FROM w.obj AND l.locktype\=w.locktype AND NOT l.pid\=w.pid AND l.granted   WHERE NOT w.granted ), tree AS (   SELECT l.locker pid, l.locker root, NULL::record obj, NULL AS mode, 0 lvl, locker::text path, array\_agg(l.locker) OVER () all\_pids   FROM ( SELECT DISTINCT locker FROM pairs l WHERE NOT EXISTS (SELECT 1 FROM pairs WHERE waiter\=l.locker) ) l   UNION ALL   SELECT w.waiter pid, tree.root, w.obj, w.mode, tree.lvl\+1, tree.path||'.'||w.waiter, all\_pids || array\_agg(w.waiter) OVER ()   FROM tree JOIN pairs w ON tree.pid\=w.locker AND NOT w.waiter \= ANY ( all\_pids ) ) SELECT (clock\_timestamp() \- a.xact\_start)::interval(3) AS ts\_age,    	replace(a.state, 'idle in transaction', 'idletx') state,    	(clock\_timestamp() \- state\_change)::interval(3) AS change\_age,    	a.datname,tree.pid,a.usename,a.client\_addr,lvl,    	(SELECT count(\*) FROM tree p WHERE p.path \~ ('^'||tree.path) AND NOT p.path\=tree.path) blocked,    	repeat(' .', lvl)||' '||left(regexp\_replace(query, 's+', ' ', 'g'),100) query FROM tree JOIN pg\_stat\_activity a USING (pid) ORDER BY path; |
| :---: | :---- |

**Task 9:**  Identify bloated Tables in PostgreSQL :

| 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95 96 97 98 99 100 101 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137 138 | WITH constants AS (     \-- define some constants for sizes of things     \-- for reference down the query and easy maintenance     SELECT current\_setting('block\_size')::numeric AS bs, 23 AS hdr, 8 AS ma ), no\_stats AS (     \-- screen out table who have attributes     \-- which dont have stats, such as JSON     SELECT table\_schema, table\_name,         n\_live\_tup::numeric as est\_rows,         pg\_table\_size(relid)::numeric as table\_size     FROM information\_schema.columns         JOIN pg\_stat\_user\_tables as psut        	ON table\_schema \= psut.schemaname        	AND table\_name \= psut.relname         LEFT OUTER JOIN pg\_stats         ON table\_schema \= pg\_stats.schemaname             AND table\_name \= pg\_stats.tablename             AND column\_name \= attname     WHERE attname IS NULL         AND table\_schema NOT IN ('pg\_catalog', 'information\_schema')     GROUP BY table\_schema, table\_name, relid, n\_live\_tup ), null\_headers AS (     \-- calculate null header sizes     \-- omitting tables which dont have complete stats     \-- and attributes which aren't visible     SELECT         hdr\+1\+(sum(case when null\_frac \<\> 0 THEN 1 else 0 END)/8) as nullhdr,         SUM((1\-null\_frac)\*avg\_width) as datawidth,         MAX(null\_frac) as maxfracsum,         schemaname,         tablename,         hdr, ma, bs     FROM pg\_stats CROSS JOIN constants         LEFT OUTER JOIN no\_stats             ON schemaname \= no\_stats.table\_schema             AND tablename \= no\_stats.table\_name     WHERE schemaname NOT IN ('pg\_catalog', 'information\_schema')         AND no\_stats.table\_name IS NULL         AND EXISTS ( SELECT 1             FROM information\_schema.columns                 WHERE schemaname \= columns.table\_schema                     AND tablename \= columns.table\_name )     GROUP BY schemaname, tablename, hdr, ma, bs ), data\_headers AS (     \-- estimate header and row size     SELECT         ma, bs, hdr, schemaname, tablename,         (datawidth\+(hdr\+ma\-(case when hdr%ma\=0 THEN ma ELSE hdr%ma END)))::numeric AS datahdr,         (maxfracsum\*(nullhdr\+ma\-(case when nullhdr%ma\=0 THEN ma ELSE nullhdr%ma END))) AS nullhdr2     FROM null\_headers ), table\_estimates AS (     \-- make estimates of how large the table should be     \-- based on row and page size     SELECT schemaname, tablename, bs,         reltuples::numeric as est\_rows, relpages \* bs as table\_bytes,     CEIL((reltuples\*             (datahdr \+ nullhdr2 \+ 4 \+ ma \-                 (CASE WHEN datahdr%ma\=0                     THEN ma ELSE datahdr%ma END)                 )/(bs\-20))) \* bs AS expected\_bytes,         reltoastrelid     FROM data\_headers         JOIN pg\_class ON tablename \= relname         JOIN pg\_namespace ON relnamespace \= pg\_namespace.oid             AND schemaname \= nspname     WHERE pg\_class.relkind \= 'r' ), estimates\_with\_toast AS (     \-- add in estimated TOAST table sizes     \-- estimate based on 4 toast tuples per page because we dont have     \-- anything better.  also append the no\_data tables     SELECT schemaname, tablename,         TRUE as can\_estimate,         est\_rows,         table\_bytes \+ ( coalesce(toast.relpages, 0) \* bs ) as table\_bytes,         expected\_bytes \+ ( ceil( coalesce(toast.reltuples, 0) / 4 ) \* bs ) as expected\_bytes     FROM table\_estimates LEFT OUTER JOIN pg\_class as toast         ON table\_estimates.reltoastrelid \= toast.oid             AND toast.relkind \= 't' ), table\_estimates\_plus AS ( \-- add some extra metadata to the table data \-- and calculations to be reused \-- including whether we cant estimate it \-- or whether we think it might be compressed     SELECT current\_database() as databasename,             schemaname, tablename, can\_estimate,             est\_rows,             CASE WHEN table\_bytes \> 0                 THEN table\_bytes::NUMERIC                 ELSE NULL::NUMERIC END                 AS table\_bytes,             CASE WHEN expected\_bytes \> 0                 THEN expected\_bytes::NUMERIC                 ELSE NULL::NUMERIC END                     AS expected\_bytes,             CASE WHEN expected\_bytes \> 0 AND table\_bytes \> 0                 AND expected\_bytes \<= table\_bytes                 THEN (table\_bytes \- expected\_bytes)::NUMERIC                 ELSE 0::NUMERIC END AS bloat\_bytes     FROM estimates\_with\_toast     UNION ALL     SELECT current\_database() as databasename,         table\_schema, table\_name, FALSE,         est\_rows, table\_size,         NULL::NUMERIC, NULL::NUMERIC     FROM no\_stats ), bloat\_data AS (     \-- do final math calculations and formatting     select current\_database() as databasename,         schemaname, tablename, can\_estimate,         table\_bytes, round(table\_bytes/(1024^2)::NUMERIC,3) as table\_mb,         expected\_bytes, round(expected\_bytes/(1024^2)::NUMERIC,3) as expected\_mb,         round(bloat\_bytes\*100/table\_bytes) as pct\_bloat,         round(bloat\_bytes/(1024::NUMERIC^2),2) as mb\_bloat,         table\_bytes, expected\_bytes, est\_rows     FROM table\_estimates\_plus ) \-- filter output for bloated tables SELECT databasename, schemaname, tablename,     can\_estimate,     est\_rows,     pct\_bloat, mb\_bloat,     table\_mb FROM bloat\_data \-- this where clause defines which tables actually appear \-- in the bloat chart \-- example below filters for tables which are either 50% \-- bloated and more than 20mb in size, or more than 25% \-- bloated and more than 4GB in size WHERE ( pct\_bloat \>= 50 AND mb\_bloat \>= 10 )     OR ( pct\_bloat \>= 25 AND mb\_bloat \>= 1000 ) ORDER BY pct\_bloat DESC; |
| ----- | :---- |

**Task 10:**  Identify bloated indexes in PostgreSQL :

| 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95 96 97 98 99 100 101 | \-- btree index stats query \-- estimates bloat for btree indexes WITH btree\_index\_atts AS (     SELECT nspname,         indexclass.relname as index\_name,         indexclass.reltuples,         indexclass.relpages,         indrelid, indexrelid,         indexclass.relam,         tableclass.relname as tablename,         regexp\_split\_to\_table(indkey::text, ' ')::smallint AS attnum,         indexrelid as index\_oid     FROM pg\_index     JOIN pg\_class AS indexclass ON pg\_index.indexrelid \= indexclass.oid     JOIN pg\_class AS tableclass ON pg\_index.indrelid \= tableclass.oid     JOIN pg\_namespace ON pg\_namespace.oid \= indexclass.relnamespace     JOIN pg\_am ON indexclass.relam \= pg\_am.oid     WHERE pg\_am.amname \= 'btree' and indexclass.relpages \> 0      	AND nspname NOT IN ('pg\_catalog','information\_schema')     ), index\_item\_sizes AS (     SELECT     ind\_atts.nspname, ind\_atts.index\_name,     ind\_atts.reltuples, ind\_atts.relpages, ind\_atts.relam,     indrelid AS table\_oid, index\_oid,     current\_setting('block\_size')::numeric AS bs,     8 AS maxalign,     24 AS pagehdr,     CASE WHEN max(coalesce(pg\_stats.null\_frac,0)) \= 0         THEN 2         ELSE 6     END AS index\_tuple\_hdr,     sum( (1\-coalesce(pg\_stats.null\_frac, 0)) \* coalesce(pg\_stats.avg\_width, 1024) ) AS nulldatawidth     FROM pg\_attribute     JOIN btree\_index\_atts AS ind\_atts ON pg\_attribute.attrelid \= ind\_atts.indexrelid AND pg\_attribute.attnum \= ind\_atts.attnum     JOIN pg\_stats ON pg\_stats.schemaname \= ind\_atts.nspname           \-- stats for regular index columns           AND ( (pg\_stats.tablename \= ind\_atts.tablename AND pg\_stats.attname \= pg\_catalog.pg\_get\_indexdef(pg\_attribute.attrelid, pg\_attribute.attnum, TRUE))           \-- stats for functional indexes           OR   (pg\_stats.tablename \= ind\_atts.index\_name AND pg\_stats.attname \= pg\_attribute.attname))     WHERE pg\_attribute.attnum \> 0     GROUP BY 1, 2, 3, 4, 5, 6, 7, 8, 9 ), index\_aligned\_est AS (     SELECT maxalign, bs, nspname, index\_name, reltuples,         relpages, relam, table\_oid, index\_oid,         coalesce (             ceil (                 reltuples \* ( 6                     \+ maxalign                     \- CASE                         WHEN index\_tuple\_hdr%maxalign \= 0 THEN maxalign                         ELSE index\_tuple\_hdr%maxalign                       END                     \+ nulldatawidth                     \+ maxalign                     \- CASE /\* Add padding to the data to align on MAXALIGN \*/                         WHEN nulldatawidth::integer%maxalign \= 0 THEN maxalign                         ELSE nulldatawidth::integer%maxalign                       END                 )::numeric               / ( bs \- pagehdr::NUMERIC )               \+1 )      	, 0 )       as expected     FROM index\_item\_sizes ), raw\_bloat AS (     SELECT current\_database() as dbname, nspname, pg\_class.relname AS table\_name, index\_name,         bs\*(index\_aligned\_est.relpages)::bigint AS totalbytes, expected,         CASE             WHEN index\_aligned\_est.relpages \<= expected                 THEN 0                 ELSE bs\*(index\_aligned\_est.relpages\-expected)::bigint             END AS wastedbytes,         CASE             WHEN index\_aligned\_est.relpages \<= expected                 THEN 0                 ELSE bs\*(index\_aligned\_est.relpages\-expected)::bigint \* 100 / (bs\*(index\_aligned\_est.relpages)::bigint)             END AS realbloat,         pg\_relation\_size(index\_aligned\_est.table\_oid) as table\_bytes,         stat.idx\_scan as index\_scans     FROM index\_aligned\_est     JOIN pg\_class ON pg\_class.oid\=index\_aligned\_est.table\_oid     JOIN pg\_stat\_user\_indexes AS stat ON index\_aligned\_est.index\_oid \= stat.indexrelid ), format\_bloat AS ( SELECT dbname as database\_name, nspname as schema\_name, table\_name, index\_name,         round(realbloat) as bloat\_pct, round(wastedbytes/(1024^2)::NUMERIC) as bloat\_mb,         round(totalbytes/(1024^2)::NUMERIC,3) as index\_mb,         round(table\_bytes/(1024^2)::NUMERIC,3) as table\_mb,         index\_scans FROM raw\_bloat ) \-- final query outputting the bloated indexes \-- change the where and order by to change \-- what shows up as bloated SELECT \* FROM format\_bloat WHERE ( bloat\_pct \> 50 and bloat\_mb \> 10 ) ORDER BY bloat\_mb DESC; |
| :---: | :---- |

**Task 11:**  Monitor blocked and blocking activities in PostgreSQL:

| 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 | SELECT blocked\_locks.pid 	AS blocked\_pid,      	blocked\_activity.usename  AS blocked\_user,      	blocking\_locks.pid 	AS blocking\_pid,      	blocking\_activity.usename AS blocking\_user,      	blocked\_activity.query    AS blocked\_statement,      	blocking\_activity.query   AS current\_statement\_in\_blocking\_process    FROM  pg\_catalog.pg\_locks     	blocked\_locks     JOIN pg\_catalog.pg\_stat\_activity blocked\_activity  ON blocked\_activity.pid \= blocked\_locks.pid     JOIN pg\_catalog.pg\_locks     	blocking\_locks         ON blocking\_locks.locktype \= blocked\_locks.locktype         AND blocking\_locks.database IS NOT DISTINCT FROM blocked\_locks.database         AND blocking\_locks.relation IS NOT DISTINCT FROM blocked\_locks.relation         AND blocking\_locks.page IS NOT DISTINCT FROM blocked\_locks.page         AND blocking\_locks.tuple IS NOT DISTINCT FROM blocked\_locks.tuple         AND blocking\_locks.virtualxid IS NOT DISTINCT FROM blocked\_locks.virtualxid         AND blocking\_locks.transactionid IS NOT DISTINCT FROM blocked\_locks.transactionid         AND blocking\_locks.classid IS NOT DISTINCT FROM blocked\_locks.classid         AND blocking\_locks.objid IS NOT DISTINCT FROM blocked\_locks.objid         AND blocking\_locks.objsubid IS NOT DISTINCT FROM blocked\_locks.objsubid         AND blocking\_locks.pid \!= blocked\_locks.pid       JOIN pg\_catalog.pg\_stat\_activity blocking\_activity ON blocking\_activity.pid \= blocking\_locks.pid    WHERE NOT blocked\_locks.granted; |
| :---: | :---- |

**Task 12:** Monitoring PostgreSQL Disk I/O performance:

| 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 | \-- perform a "select pg\_stat\_reset();" when you want to reset counter statistics with all\_tables as ( SELECT  \* FROM    (     SELECT  'all'::text as table\_name,         sum( (coalesce(heap\_blks\_read,0) \+ coalesce(idx\_blks\_read,0) \+ coalesce(toast\_blks\_read,0) \+ coalesce(tidx\_blks\_read,0)) ) as from\_disk,         sum( (coalesce(heap\_blks\_hit,0)  \+ coalesce(idx\_blks\_hit,0)  \+ coalesce(toast\_blks\_hit,0)  \+ coalesce(tidx\_blks\_hit,0))  ) as from\_cache         FROM    pg\_statio\_all\_tables  \--\> change to pg\_statio\_USER\_tables if you want to check only user tables (excluding postgres's own tables)     ) a WHERE   (from\_disk \+ from\_cache) \> 0 \-- discard tables without hits ), tables as ( SELECT  \* FROM    (     SELECT  relname as table\_name,         ( (coalesce(heap\_blks\_read,0) \+ coalesce(idx\_blks\_read,0) \+ coalesce(toast\_blks\_read,0) \+ coalesce(tidx\_blks\_read,0)) ) as from\_disk,         ( (coalesce(heap\_blks\_hit,0)  \+ coalesce(idx\_blks\_hit,0)  \+ coalesce(toast\_blks\_hit,0)  \+ coalesce(tidx\_blks\_hit,0))  ) as from\_cache         FROM    pg\_statio\_all\_tables \--\> change to pg\_statio\_USER\_tables if you want to check only user tables (excluding postgres's own tables)     ) a WHERE   (from\_disk \+ from\_cache) \> 0 \-- discard tables without hits ) SELECT  table\_name as "table name",     from\_disk as "disk hits",     round((from\_disk::numeric / (from\_disk \+ from\_cache)::numeric)\*100.0,2) as "% disk hits",     round((from\_cache::numeric / (from\_disk \+ from\_cache)::numeric)\*100.0,2) as "% cache hits",     (from\_disk \+ from\_cache) as "total hits" FROM    (SELECT \* FROM all\_tables UNION ALL SELECT \* FROM tables) a ORDER   BY (case when table\_name \= 'all' then 0 else 1 end), from\_disk desc; |
| :---: | :---- |

**Login to Database using psql**

$ psql "service=myservice sslmode=require"  
$ psql postgresql://dbmaster:5433/mydb?sslmode=require  
$ psql \-U username mydatabase

**Basic Commands**

| \\c dbname | Switch connection to a new database |
| :---- | :---- |
| \\l | List available databases |
| \\dt | List available tables |
| \\d table\_name | Describe a table such as a column, type, modifiers of columns, etc. |
| \\dn | List all schemas of the currently connected database |
| \\df | List available functions in the current database |
| \\dv | List available views in the current database |
| \\du | List all users and their assign roles |
| SELECT version(); | Retrieve the current version of PostgreSQL server |
| \\g | Exexute the last command again |
| \\s | Display command history |
| \\s filename | Save the command history to a file |
| \\i filename | Execute psql commands from a file |
| \\? | Know all available psql commands |
| \\h | Get help |
| \\e | Edit command in your own editor |
| \\a | Switch from aligned to non-aligned column output |
| \\H | Switch the output to HTML format |
| \\q | Exit psql shell |

**Turn on Query Execution Time**

dvdrental=\# \\timing  
Timing is on.  
dvdrental=\# select count(\*) from film;  
 count  
\-------  
  1000  
(1 row)

Time: 1.495 ms

**Find a view**

select \* from INFORMATION\_SCHEMA.views where table\_name='pg\_stat\_activity\_dd';    

**Execute a .sql script**

C:\\Users\\jptdhee\>psql \-h localhost \-d postgres \-U postgres \-p 5432 \-a \-f C:/Users/jptdhee/poi\_qa.sql

**Find Table from a Particular Schema**  

select table\_schema, 

       table\_name 

from information\_schema.tables 

where table\_name like 'event\_of\_interest\_attribute%' 

      and table\_schema not in ('information\_schema', 'pg\_catalog') 

      and table\_type \= 'BASE TABLE' 

order by table\_name, 

         table\_schema; 

SELECT \* FROM information\_schema.tables WHERE table\_schema \= '\<schema\_name\>' ; 

 

SELECT table\_schema||'.'||table\_name AS full\_rel\_name 

  FROM information\_schema.tables 

 WHERE table\_schema \= '\<schema\_name\>';

**Privilege for user’s in a particular table** 

SELECT grantee, privilege\_type  

FROM information\_schema.role\_table\_grants  

WHERE table\_name='\<table\_name\>' ;

**List Schema details**

\\c database ; 

 

select s.nspname as table\_schema, s.oid as schema\_id, u.usename as owner from pg\_catalog.pg\_namespace s join pg\_catalog.pg\_user u on u.usesysid \= s.nspowner where nspname not in ('information\_schema', 'pg\_catalog', 'public') and nspname not like 'pg\_toast%' and nspname not like 'pg\_temp\_%' order by table\_schema; 

 

select s.nspname as table\_schema, 

       s.oid as schema\_id, 

       u.usename as owner 

from pg\_catalog.pg\_namespace s 

join pg\_catalog.pg\_user u on u.usesysid \= s.nspowner 

order by table\_schema;

**List of all tables in a database**

\\c database ;

SELECT \* FROM pg\_catalog.pg\_tables WHERE schemaname \!= 'pg\_catalog' AND schemaname \!= 'information\_schema';

**Drop User**

DROP OWNED BY st\_datapipeline; 

DROP USER st\_datapipeline;

**Check Number of Connection in use**

SELECT count(distinct(numbackends)) FROM pg\_stat\_database ;

**Show Connection Limit**

SHOW max\_connections

**Reloading Config without restart**

SELECT pg\_reload\_config()

**Alter Connection Limit of a User**

ALTER ROLE jirauser CONNECTION LIMIT \-1;    \-- infinite   
ALTER ROLE data\_analytics CONNECTION LIMIT 600 ; 

**Search User**

SELECT rolname, rolconnlimit from pg\_roles; 

SELECT rolname, rolconnlimit   
FROM pg\_roles   
WHERE rolconnlimit \<\> \-1;

**How to determine the size of PostgreSQL databases and tables**

SELECT pg\_size\_pretty( pg\_database\_size('dbname') );

SELECT pg\_size\_pretty( pg\_total\_relation\_size('tablename') );

select t1.datname AS db\_name,     
       pg\_size\_pretty(pg\_database\_size(t1.datname)) as db\_size   
from pg\_database t1   
order by pg\_database\_size(t1.datname) desc; 

SELECT database\_name, pg\_size\_pretty(size) from (SELECT pg\_database.datname as "database\_name", pg\_database\_size(pg\_database.datname) AS size FROM pg\_database ORDER by size DESC) as ordered;

\---Databases to which the user cannot connect are sorted as if they were infinite size  
SELECT    
    datname AS DatabaseName   
    ,pg\_catalog.pg\_get\_userbyid(datdba) AS OwnerName   
    ,CASE    
        WHEN pg\_catalog.has\_database\_privilege(datname, 'CONNECT')   
        THEN pg\_catalog.pg\_size\_pretty(pg\_catalog.pg\_database\_size(datname))   
        ELSE 'No Access For You'   
    END AS DatabaseSize   
FROM pg\_catalog.pg\_database   
ORDER BY    
    CASE    
        WHEN pg\_catalog.has\_database\_privilege(datname, 'CONNECT')   
        THEN pg\_catalog.pg\_database\_size(datname)   
        ELSE NULL   
    END DESC;

**Find the Status of Database connections**

select state, count(\*) from pg\_catalog.pg\_stat\_activity psa where datname \= '\<database\>' group by state;

**Show all Blocked Queries**

select pid, 

datname, 

usename, 

pg\_blocking\_pids(pid) as blocked\_by, 

query as blocked\_query 

from pg\_stat\_activity 

where cardinality(pg\_blocking\_pids(pid)) \> 0;

**View Query of Blocking ID**

select pid, 

datname, 

usename,  

query 

from pg\_stat\_activity 

where pid \= '\<some\_query\_id\>' ;

**Terminate Query that is Blocking**

select pg\_terminate\_backend(pg\_stat\_activity.pid) 

from pg\_stat\_activity 

where pid \= '

select pid, 

datname, 

usename,  

query 

from pg\_stat\_activity 

where pid \= '\<some\_query\_id\>' ;

**Show the parent pid and query that is causing the blocking and child queries that are being blocked by the parent**

SELECT 

activity.pid, 

activity.usename, 

activity.query, 

blocking.pid AS blocking\_id, 

blocking.query AS blocking\_query 

FROM pg\_stat\_activity AS activity 

JOIN pg\_stat\_activity AS blocking ON blocking.pid \= ANY(pg\_blocking\_pids(activity.pid));

**List all the tables and data size of tables in a database**

\\c database ;

select schemaname as table\_schema, relname as table\_name, pg\_size\_pretty(pg\_relation\_size(relid)) as data\_size from pg\_catalog.pg\_statio\_user\_tables order by pg\_relation\_size(relid) desc;

**Select count(\*) of tables in a database**

\\c database ;

SELECT schemaname as table\_schema, relname as table\_name, n\_live\_tup as row\_count FROM pg\_stat\_user\_tables ORDER BY n\_live\_tup DESC;

select n.nspname as table\_schema, c.relname as table\_name, c.reltuples as rows from pg\_class c join pg\_namespace n on n.oid \= c.relnamespace where c.relkind \= 'r' and n.nspname not in ('information\_schema','pg\_catalog') order by c.reltuples desc;

**Find the PostgreSQL Activities**

select pid as process\_id, usename as username, datname as database\_name, client\_addr as client\_address, application\_name, backend\_start, state, state\_change from pg\_stat\_activity;

**Copy the Output to .csv file**

\\copy (select \* from mobile\_settings)To '/tmp/output.csv' With CSV DELIMITER ',' HEADER

psql \-d dbname \-t \-A \-F"," \-c "select \* from users" \> output.csv 

psql \-d my\_db\_name \-t \-A \-F"," \-f input-file.sql \-o output-file.csv 

 

$ ssh psqlserver.example.com 'psql \-d mydb "COPY (select id, name from groups) TO STDOUT WITH CSV HEADER"' \> groups.csv

**Verify the Master/Slave Replication**

**MASTER :** 

   
select usename,application\_name,client\_addr,backend\_start,state,sync\_state from pg\_stat\_replication ; 

**SLAVE:** 

   
select pg\_is\_in\_recovery(); 

select pg\_last\_xlog\_receive\_location(), pg\_last\_xlog\_replay\_location(), pg\_last\_xact\_replay\_timestamp(), CASE WHEN pg\_last\_xlog\_receive\_location() \= pg\_last\_xlog\_replay\_location() THEN 0 ELSE EXTRACT(EPOCH FROM now() \- pg\_last\_xact\_replay\_timestamp()) END AS log\_delay;

**List table columns in a database**

select table\_schema,  
       table\_name,  
       ordinal\_position as position,  
       column\_name,  
       data\_type,  
       case when character\_maximum\_length is not null  
            then character\_maximum\_length  
            else numeric\_precision end as max\_length,  
       is\_nullable,  
       column\_default as default\_value  
from information\_schema.columns  
where table\_schema not in ('information\_schema', 'pg\_catalog')  
order by table\_schema,   
         table\_name,  
         ordinal\_position;

**List of all column’s in a specific tables**

select ordinal\_position as position,  
       column\_name,  
       data\_type,  
       case when character\_maximum\_length is not null  
            then character\_maximum\_length  
            else numeric\_precision end as max\_length,  
       is\_nullable,  
       column\_default as default\_value  
from information\_schema.columns  
where table\_name \= 'Table name' \-- enter table name here  
      \-- and table\_schema= 'Schema name'  
order by ordinal\_position;

**List Views in a database**

select table\_schema as schema\_name,  
       table\_name as view\_name  
from information\_schema.views  
where table\_schema not in ('information\_schema', 'pg\_catalog')  
order by schema\_name,  
         view\_name;

**List Views with their Scripts**

select table\_schema as schema\_name,  
       table\_name as view\_name,  
       view\_definition  
from information\_schema.views  
where table\_schema not in ('information\_schema', 'pg\_catalog')  
order by schema\_name,  
         view\_name;

**Lists views in a database with their definition**

select u.view\_schema as schema\_name,  
       u.view\_name,  
       u.table\_schema as referenced\_table\_schema,  
       u.table\_name as referenced\_table\_name,  
       v.view\_definition  
from information\_schema.view\_table\_usage u  
join information\_schema.views v   
     on u.view\_schema \= v.table\_schema  
     and u.view\_name \= v.table\_name  
where u.table\_schema not in ('information\_schema', 'pg\_catalog')  
order by u.view\_schema,  
         u.view\_name;

**Lists all materialized views, with their definition**

select schemaname as schema\_name,  
       matviewname as view\_name,  
       matviewowner as owner,  
       ispopulated as is\_populated,  
       definition  
from pg\_matviews  
order by schema\_name,  
         view\_name;

**Lists all primary keys constraints (PK) in the database**

select kcu.table\_schema,  
       kcu.table\_name,  
       tco.constraint\_name,  
       string\_agg(kcu.column\_name,', ') as key\_columns  
from information\_schema.table\_constraints tco  
join information\_schema.key\_column\_usage kcu   
     on kcu.constraint\_name \= tco.constraint\_name  
     and kcu.constraint\_schema \= tco.constraint\_schema  
     and kcu.constraint\_name \= tco.constraint\_name  
where tco.constraint\_type \= 'PRIMARY KEY'  
group by tco.constraint\_name,  
       kcu.table\_schema,  
       kcu.table\_name  
order by kcu.table\_schema,  
         kcu.table\_name;

**Lists tables and their primary key (PK) constraint names**

select tab.table\_schema,  
       tab.table\_name,  
       tco.constraint\_name,  
       string\_agg(kcu.column\_name, ', ') as key\_columns  
from information\_schema.tables tab  
left join information\_schema.table\_constraints tco  
          on tco.table\_schema \= tab.table\_schema  
          and tco.table\_name \= tab.table\_name  
          and tco.constraint\_type \= 'PRIMARY KEY'  
left join information\_schema.key\_column\_usage kcu   
          on kcu.constraint\_name \= tco.constraint\_name  
          and kcu.constraint\_schema \= tco.constraint\_schema  
          and kcu.constraint\_name \= tco.constraint\_name  
where tab.table\_schema not in ('pg\_catalog', 'information\_schema')  
      and tab.table\_type \= 'BASE TABLE'  
group by tab.table\_schema,  
         tab.table\_name,  
         tco.constraint\_name  
order by tab.table\_schema,  
         tab.table\_name

 **Lists tables in a database without primary keys**

select tab.table\_schema,  
       tab.table\_name  
from information\_schema.tables tab  
left join information\_schema.table\_constraints tco   
          on tab.table\_schema \= tco.table\_schema  
          and tab.table\_name \= tco.table\_name   
          and tco.constraint\_type \= 'PRIMARY KEY'  
where tab.table\_type \= 'BASE TABLE'  
      and tab.table\_schema not in ('pg\_catalog', 'information\_schema')  
      and tco.constraint\_name is null  
order by table\_schema,  
         table\_name;

**Lists check constraints defined in the database ordered by constraint name**

select pgc.conname as constraint\_name,  
       ccu.table\_schema as table\_schema,  
       ccu.table\_name,  
       ccu.column\_name,  
       pgc.consrc as definition  
from pg\_constraint pgc  
join pg\_namespace nsp on nsp.oid \= pgc.connamespace  
join pg\_class  cls on pgc.conrelid \= cls.oid  
left join information\_schema.constraint\_column\_usage ccu  
          on pgc.conname \= ccu.constraint\_name  
          and nsp.nspname \= ccu.constraint\_schema  
where contype \='c'  
order by pgc.conname;

**Lists table triggers in a database with their details**

select event\_object\_schema as table\_schema,  
       event\_object\_table as table\_name,  
       trigger\_schema,  
       trigger\_name,  
       string\_agg(event\_manipulation, ',') as event,  
       action\_timing as activation,  
       action\_condition as condition,  
       action\_statement as definition  
from information\_schema.triggers  
group by 1,2,3,4,6,7,8  
order by table\_schema,  
         table\_name;

**List stored procedures and information about it in PostgreSQL database**

select n.nspname as schema\_name,  
       p.proname as specific\_name,  
       l.lanname as language,  
       case when l.lanname \= 'internal' then p.prosrc  
            else pg\_get\_functiondef(p.oid)  
            end as definition,  
       pg\_get\_function\_arguments(p.oid) as arguments  
from pg\_proc p  
left join pg\_namespace n on p.pronamespace \= n.oid  
left join pg\_language l on p.prolang \= l.oid  
left join pg\_type t on t.oid \= p.prorettype   
where n.nspname not in ('pg\_catalog', 'information\_schema')  
      and p.prokind \= 'p'  
order by schema\_name,  
         specific\_name;

**Finds tables which names start with specific prefix**

select table\_schema,  
       table\_name  
from information\_schema.tables  
where table\_name like 'payment%'  
      and table\_schema not in ('information\_schema', 'pg\_catalog')  
      and table\_type \= 'BASE TABLE'  
order by table\_name,  
         table\_schema;

**Find total number of tables in current database**

select count(\*) as tables  
from information\_schema.tables  
where table\_type \= 'BASE TABLE';

**List of users in current database**

select usesysid as user\_id,  
       usename as username,  
       usesuper as is\_superuser,  
       passwd as password\_md5,  
       valuntil as password\_expiration  
from pg\_shadow  
order by usename;

**List Database Sessions**

select pid as process\_id,   
       usename as username,   
       datname as database\_name,   
       client\_addr as client\_address,   
       application\_name,  
       backend\_start,  
       state,  
       state\_change  
from pg\_stat\_activity;

**Kill a Session**

select pg\_terminate\_backend(pid)   
from pg\_stat\_activity  
where pid \= '18765';

**Find empty tables in PostgreSQL database**

select n.nspname as table\_schema,  
       c.relname as table\_name  
from pg\_class c  
join pg\_namespace n on n.oid \= c.relnamespace  
where c.relkind \= 'r'  
      and n.nspname not in ('information\_schema','pg\_catalog')  
      and c.reltuples \= 0  
order by table\_schema,  
         table\_name;

**List of tables by the size of data and indexes**

select schemaname as table\_schema,  
       relname as table\_name,  
       pg\_size\_pretty(pg\_total\_relation\_size(relid)) as total\_size,  
       pg\_size\_pretty(pg\_relation\_size(relid)) as data\_size,  
       pg\_size\_pretty(pg\_total\_relation\_size(relid) \- pg\_relation\_size(relid))   
        as external\_size  
from pg\_catalog.pg\_statio\_user\_tables  
order by pg\_total\_relation\_size(relid) desc,  
         pg\_relation\_size(relid) desc;

table\_schema – table’s schema name  
table\_name – table name  
total\_size – Total disk space used by the specified table, including all indexes and TOAST data  
data\_size – Disk space used by specified table or index  
external\_size – Disk space used by realted object to specified table

Advertisement

**List table which uses 50% of summary space used by all database tables.**

select schemaname as table\_schema,   
       relname as table\_name,  
       pg\_size\_pretty(pg\_relation\_size(relid))  
from pg\_catalog.pg\_statio\_user\_tables  
where pg\_relation\_size(relid) \> 0.5 \* (  
        select sum((pg\_relation\_size(relid)))  
        from pg\_catalog.pg\_statio\_user\_tables);

**Backup using pg\_dump**

Backup one database

pg\_dump \-U postgres \-W \-F t dvdrental \> c:\\pgbackup\\dvdrental.tar

Backup all databases

pg\_dumpall \-U postgres \> c:\\pgbackup\\all.sql

Backup database object definitions

pg\_dumpall \--schema-only \> c:\\pgdump\\definitiononly.sql

pg\_dumpall \--roles-only \> c:\\pgdump\\allroles.sql

pg\_dumpall \--tablespaces-only \> c:\\pgdump\\allroles.sql

To dump a single table named mytab:

$ pg\_dump \-t mytab mydb \> db.sql

To dump all tables whose names start with emp in the detroit schema, except for the table named employee\_log:

$ pg\_dump \-t 'detroit.emp\*' \-T detroit.employee\_log mydb \> db.sql

To dump all schemas whose names start with east or west and end in gsm, excluding any schemas whose names contain the word test:

$ pg\_dump \-n 'east\*gsm' \-n 'west\*gsm' \-N '\*test\*' mydb \> db.sql

[image1]: <data:image/png;base64,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>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image5]: <data:image/png;base64,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6NGjn3nmGQTiyiuvfOedd7bccsv77rvvscceA5fXXHNNfr366qup5+WXXx4/fjwDufjiix3q8sc//vGRRx5ZdtllBwwYwAfv1jz77LMvvPCCydhH8c98/vDDDyu6TCOVj4JMXHfddQVdZueFI3PcEmNE9ucoHHM888wzczr8UAozMPOIam+44YY+irzl80YbbbTZZptVwg2avPK73/3OrEzCCVQ+nHLKKWWFaSTS7VQl0ymsRkgWWNNeiNUeOtidqgnoK5LJqux9QWkZPGuk584FYT5apHk40+QvlTHO6T64itxtG4yyDnu4e9Rw/vnnWwIN7swwDjjggD46g7HeeuvdeuutUVjAt2Bbfvhy6NCh7rxHlIULhk3YcsjMZTGIw32ckfxXG/JEsMgQ/GJNE+tLLrlk/fXXj3Tcc4EFFqgrOPbII4+0a2JVz6tYgzp0xMhDtqmra9qaqSykc1M8gHXso/BF2t13333zWkaz+xsHt9K0imSnEcJPP/104MCBe+6557hx4/70pz9ZiSzYRZnhnLbDW3TaJNa1bpEwvaEjZ+amBawhR8Ei5E6aTbzbWwG3bpQvd9ttNwbrd6FM+f+HGWopXFrHZyAup8lfRbBJueqqq1588UVIveuuu06dOhXp4tcJEyZMmTIlVmalBx98cIkllnj88ccfffRR6jnwwAMnTZq0//7726qVVCryGiHjiiuuOHbs2FQeZyK/+cYbb3zggQfeeOMNnqeqJ554Yumllx4zZgy1Qfm99toLSIHjMBQ+oq1w3JEu8GuKkqvD/V122eW555479NBDQX5+2mqrrRKlDbrrrrsSTdSSMHdiDtcEhCYFZgubUrmekRwp6md67emT2+L7yy677N133916661vueWWp59+GhhkpCDnpZdeCs0hGl9CIh5jXJtuuukdd9zB2JdbbjnsBZ+hAEQDVJ966inE8owzzgBXd999d6tkImv61ltvWa0gTk374qANCLz33ntXZJJmaFAzzefo+gknnOBAg2HDhtH2zTffDIh88sknw4cPp1W4Qu9XW221U0899dhjj11llVV4AGnADAwaNMhbU0suuSStgj78+vvf//7oo49Gel555RWYAfqj0ravjAp5evLJJ5daaqmJEyfCeNgM/6BIXhu6GAkGDF2gAuYEAEKqTj/99IV0VRyGYfnll0d6eAzSbLvttk6C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bKNNawVwzguSfZU6Py9LXJHhrOsg4LLLLsu7y+r60rLcDT4YKK1I1OnVM/jNh6WXXtoNtek8+7nnntuqw5F9lIvcZolGqZDH6K1rTgWCrvafysccLdafkgzY+++/38THdgUVH3roodddd52tiJ+Z9WKeeqQQ7auvvmrpskN8zTXXXHDBBa+88kqi21KhEq0PHTr09NNP76P1hpVWWsm2xJheU2gM5HWes+WVKp1R2IftVEBEQx4ibSE8hjyLeFVnz3mSms2UqlahF9b1f6vptks+91Ms22K6o9GS4A7zSrv88aJWHQYopTs13HHHHfSnrkx4TiWfKX02Ndj49VYENf3hX+qnVw8++KBNIPVbG/l+UaV6KGhTFspg7ejbokqbUNS1nQvp6hK4ZpOwzz779NPlPE76geDx/IorrhjJiC6pLGX8XUL3yHYqXU5/ndxdRtfr9tWJL6jhJdxe2uaw/egIsVGL64Ti0rq2waLeT3kbqIHhV3RTN3ruOR89XFK5fP2hLC+nGG6YWk6XFrhyPtDJm2+++cADD6wpboCe0w3bOSvsWmutVVTsIvXwl7YYCE0zwFV096eJ7ODEX/7ylw05N1VZa4+0rkuOF1xwwYV1cwBuypAhQ2699VbaqiixBq936JYFi0RDh7us5nzJA1TFZwZb0jI7XGakUIMa7C310z1UjAVCLa0M/oaCvrq6ADgy8iKr9hTTcPDX0tjUgjlX3ERVntZdd90Vy20th4WKF154wenQ+ex9BFvWn1RMAQbOMJ966qk+Otpb0wnm11577eijj0basambbrqpF5acXJqGrrjiimuvvZZv6MCoUaMiufjMw5588kl0JA7n5i0VXUv3HoQFs0MOOcT+WfefZ7m4J1HI0rCi7s3tpU2lWMcNJk2a9Oijj5533nk9dPlgt37OFoP6+eefl+QH2z2iTgR4o402qmltL/8DyfENiPT++uuvN7D+/ve/R6xHjx69++67v/322ygGlV522WVIA6KMDXjggQdQLdhz1VVXXXrppWecccYtt9xy0kkndWh3bauttuJXDJ6zbMPg8ePHw7kWnQKk0b322mvKlCnUA/OA+D/84Q889tBDDx1zzDFG5KqmSieffDINMQZUy3nYUdR3330XaqJjdHXs2LHDhg2jG8jKeuuth1jw05133rntttviB1x++eXMruraojMVZiQf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1DN69OhRo0YtqTt9H3nkEcRjn332efnllw866CBk4/XXX99oo41okZ+eeeYZY1dFB9kh6ZG6GrL+0wOVf7RE4eiRBRWa1BXVXw9TL8SMgfDh5ptvHjduHNKI7vABWaVLdHXSpEkM+aSTTmIIv/3tb2+//fa77757JRWnDAOCAEzoTCXg6ttvv73mmmsaW2Ip1DfffNOqVSi79UgjugYxd9lll5JmmDM0qDwN21ADCPTEE09Qo3OsX3fddcjExx9/fOqpp4J0o3X5CW7X8ccff9hhh9Ez0P+BBx5YddVVkWM+r7766vADFvINHNprr72OOuooSAA7MQBob03WuqZLv++7775HH32UF6nt8MMPd9w/3/Aro/W2OS/yDVxHo7AZRx999HLLLXfFFVc8/vjj9Hm11Va74447Nt10U6rCVm2zzTYleYJYaMb1pgqd5EXM0vjx4zfeeGNIab+Vf4877ritt94aQXn44YcZMt1DoBmC4QbagR0jRozAXNE3GoUrVL7hhhuut956l19+uWlq1+HOO++EkaA//UFneP3cc8+95557mlOuSOXVV1+1ZY0FHJFM7JZbbgmp/0vZmXmL/oALgwcPdiJThsCTjHG//faDsN9++y1KctdddzkSElmnk1AbcwjgbrfddsOHD/fKdl7TQQZrS2wso6vTp0+Hm3/729/wVKAzZhXNAaTQNHR+++23h7bQAbOKtBWUKwMDjDxQrUM/ECya5td9990XC4dEQo1FdDAjlueESbv44osjBeUjf/ARcUeWvO8OTegqH/bcc09eTLSLA2WKCv03Vg4ZMuSQQw7heUb0wQcfnHPOOcgVlLn++ushONXCFMb+6aefXnPNNUhgi9YnbajeeuutV3UJAY3SOgztpYhKmwHUD54yFjqfKT0sQ4DICDZjB1moYcUVV+ThQw89tKQYLjALYtIoqgEjoD8ifeaZZ9oODRs27MQTT5wwYQLd6KvURT21qEvnkQo4a0yHaDvvvDPsRhF4HWoArAwKtWdcyytXMArfXwWNOPDAAxPNtxgv/blRt4jQ1tChQxHd5lGrina8qJORQkC0le7xGVkFH3lm7bXXZozoF/1h7H/6059oHaI5sgkt++ijjzbZZBOTHbFBU4BalI7nb7jhBtTHqcCRah5AbRGAkSNH0g2egT6wElgH2vAI4RrVGhDpJCTaf//933vv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yy23NLUGXpay4b877bTT22+/TROL645GaxTPDlZITeaeFVhmo3idsFuhrV60NNnwCeRx1VVXIeAKWlPl81VXXbUl56krLMAyClQg/4UQH3jggS233DKSIbWkMtFbr5jqUh1+H6MkwBDDWC1IHnjggWuttVaflppNHltssQVd6lV+QdqymC7LnyvpFrluQY8WVFq6rW8p3WIUS0sx4zSBz2pZ36WA5KOOOmr27NmmfxMD+EFVHHTQQS0dPsuGv5TubyjoLNqALu2pysOzuKfyVPpmn332iUO8ookQPExQRIOHDDZsPFGndYB7W5HGcqzTZz7zGa+vmvJLCmtvaVPAdmGsZZixYRW6Jke/oIUva0o+cRanPkWgNMMFc27LIqNbC1T9cta3226722677V+UssOfR1rCoXufUx7/iqxM0BKLWTbffHNnYzfqKDZOl5S52m6l9ahL3Ta0LvKFL3yB+nFuzM7Obv0pHdtNZaYncp6YjtVXX512mWsr7KZ0f5/gQx/6UFNwzDHHQBsFhToaRf2K0mrJia/L1LAKKchcDpzxfkE1pPihAyeddFJTxo0lDB2477774HenwC3JAxmFerNEipWu8rLLLjNV9GlfDCvksccemzx5MqiGK6+44gqwfccdd/zyl7+0yW6xlq9xhGB6YFyYjDVZmen7o1D9AD6XWWYZ85TJm4awJF577bWVV145DhfEMvzrrrsOLZix0oigHTMey0033dSvY7seI7+vsMIKt99++8knn9ylE3TzVKjmQygAeco0JNowh9zPOuss5Purr7565JFHwmmzZs06++yzofJdd92VAgwSp/jyyy9fd91111xzzQsvvBB+o07+fu1rX8Or2HnnneEEqr3mmmtmzJiBX2K5wMgpQ0c322wzWvnWt77l6Imjjz76Bz/4AQ/7778/Jsnxxx8/c+ZMCGW55ZajnksvvfT0009fb731EtnOsc6EQDFUC2HxFhEA4V599dW77757UdeG33DDDZAaPYGyqcS0iySl52+++eYFF1yAXPjOd77jw6lQHp1EA9FbiHKDDTYAm3PmzKEAThUPF1988frrrz9WBwmGOWeehkyhgslacBoqcv0d+Qk2UklwZNC5555Lh/Gexykiup1GR8EJWA9PP/10pD0zM96NN9744osvMvvgDfyATzwMjI8f/ehHGF+oup/85CdoXEQ8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>

[image6]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image7]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image8]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image9]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image10]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image11]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image12]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image13]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image14]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image15]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image16]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>

[image17]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABAQMAAAAl21bKAAAABlBMVEUAAAAAAAClZ7nPAAAAAXRSTlMAQObYZgAAAApJREFUeF5jYAAAAAIAAd6ej78AAAAASUVORK5CYII=>