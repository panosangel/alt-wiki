# Various - Postgres Database Management

## Manage databases
List all dbs:
```
\l
```
List tables, views, and sequences:
```
\d
```
Create a new db:
```postgresql
CREATE DATABASE dbname;
```
Connect to a db:
```
\c dbname
```
Delete a db:
```postgresql
DROP DATABASE IF EXISTS dbname;
```

## Manage users
List all users:
```
\du
```
Create a user:
```postgresql
CREATE USER dbuser WITH PASSWORD 'password';
```
Note:  More options exist such as SUPERUSER.  
Add an existing user to db:
```sql
GRANT ALL PRIVILEGES ON DATABASE dbname TO dbuser;
```
Altering Existing User Permissions:
```sql
ALTER USER dbuser WITH OPTION1 OPTION2 OPTION3;
```
Delete user:
```postgresql
DROP USER dbuser;
```
Note: A user cannot be dropped if he is the owner of a db.

## Manage tables
List all tables:
```
\dt
```
Create a table
```sql
CREATE TABLE IF NOT EXISTS dbtable (...);
```
Delete a table:
```sql
DROP TABLE dbtable;
```

## Manage sequences
List all sequences:
```
\ds
```
Create a sequence:
```sql
CREATE SEQUENCE seqName START WITH startNumber INCREMENT BY incrementNumber;
```
Show current value:
```postgresql
SELECT currval('seqName');
```
Get next value:
```postgresql
SELECT nextval('seqName');
```
Set value:
```postgresql
SELECT setval('seqName',10);
```
Delete a sequence:
```sql
DROP SEQUENCE seqName;
```

---

## Back up a single database
We are using `pg_dump` which is the official tool to extract a PostgreSQL database into a script file or other archive file.  
The synopsis of the command us is:
`pg_dump [connection-option...] [option...] [dbname]`  
For example:
```bash
pg_dump -h <hostname> -p <port> -U <dbuser> -d <dbname> -v -Fc > <outputFilename>.dump
```

## Restore a single database
We are using `pg_restore` which is the official tool to restore a PostgreSQL database from an archive file created by pg_dump.  
The synopsis of the command us is:
`pg_restore [connection-option...] [option...] [filename]`    
For example:
```bash
pg_restore -h <hostname> -p <port> -U <dbuser> -C -d postgres -v <dumpFilename>
```
Note:
- With `-C`, data is always restored into the database name that appears in the dump file.  

In case we have an .sql file:
```bash
psql -U <username> -d <dbname> -1 -f <filename>.sql
```

## Back up all databases
```bash
pg_dumpall [...] > pg_backup.bak
```

## Appendix A - Sources
- [Postgres Tutorial - 17 Practical psql Commands That You Don’t Want To Miss](http://www.postgresqltutorial.com/psql-commands/)
- [Postgresguide - psql](http://postgresguide.com/utilities/psql.html)
- [PostgreSQL Tutorial: Learn in 3 Days](https://www.guru99.com/postgresql-tutorial.html)
- [What is the purpose of the sql script file in a tar dump?](https://stackoverflow.com/questions/50911400/what-is-the-purpose-of-the-sql-script-file-in-a-tar-dump)
- [FAQ: Using Sequences in PostgreSQL](http://www.neilconway.org/docs/sequences/)
- [w3resource - PostgreSQL: Sequence](https://www.w3resource.com/PostgreSQL/postgresql-sequence.php)
- [PostgreSQL Sequences](http://www.postgresqltutorial.com/postgresql-sequences/)
- [Reset a PostgreSQL sequence and update column values](https://fle.github.io/reset-a-postgresql-sequence-and-recompute-column-values.html)
