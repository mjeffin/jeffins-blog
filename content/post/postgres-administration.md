---
title: "Postgres Administration"
date: 2021-05-24T20:00:00+05:30
draft: false
tags: ["postgres","snippets"]

---

This post is a collection of notes & nippets related to administering a postgres database.

## Securing connections via SSL

This [post](https://loganmarchione.com/2020/10/securing-postgres-connections-using-lets-encrypt-certificates/) gives detailed instructions on how to enable
SSL for postgres using Letencrypt certificates for internet facing databases. The post is written for postgres 11, so if you are using a different version, make sure 
to update 11 in the postgres path to your version. It works like a charm and thank you [ Logan Marchione,](https://loganmarchione.com/) 🙏


## Setting up read only users

Login as the superuser to the default database which ususally is postgres/postgres most of the time. 
#### See list of users
```sql
SELECT usename AS role_name,
  CASE
     WHEN usesuper AND usecreatedb THEN
	   CAST('superuser, create database' AS pg_catalog.text)
     WHEN usesuper THEN
	    CAST('superuser' AS pg_catalog.text)
     WHEN usecreatedb THEN
	    CAST('create database' AS pg_catalog.text)
     ELSE
	    CAST('' AS pg_catalog.text)
  END role_attributes,
       *
FROM pg_catalog.pg_user
ORDER BY role_name desc;
```

#### Set up read only role(group)
This needs to done only once.
```sql
create role read_only_group nologin;
GRANT USAGE ON SCHEMA public to read_only_group;
```

#### Create the acutal user and assign to role
Do this every time a new read only user needs to be created
```sql
create user newuser  WITH  PASSWORD 'xxxxxxxx';
grant read_only_group to newuser;
Alter user newuser NOCREATEDB;
Alter user newuser NOCREATEROLE;
Alter user newuser INHERIT;

```
#### Assign Read only privileges to all databases
This step needs to be done only once, unless a database itself is added in the cluster. Privileges can be up in all databases in the cluster dynamically 
as below. Leave the dbname= part blank in the template and fill the rest.

```sql


--- automated way
create  extension dblink;

DO
$$
DECLARE database_name_client TEXT;
DECLARE template_connection TEXT;
DECLARE string_conn TEXT;
DECLARE create_group_name TEXT;
BEGIN
template_connection = 'user=<superuser> password=<superuser_password> port=<port> host=<host> dbname=';
create_group_name = 'read_only_group';
FOR database_name_client IN
  SELECT datname FROM pg_database
  WHERE datistemplate = false and datname not in ('postgres')
LOOP
  string_conn = template_connection ||  database_name_client;
  perform dblink_exec(string_conn, 'GRANT CONNECT ON DATABASE "' || database_name_client || '" TO ' || create_group_name);
  perform dblink_exec(string_conn, 'GRANT SELECT ON ALL TABLES IN SCHEMA public TO ' || create_group_name);
  perform dblink_exec(string_conn, 'GRANT SELECT ON ALL SEQUENCES IN SCHEMA public TO ' || create_group_name);
  perform dblink_exec(string_conn, 'ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO ' || create_group_name);
END LOOP;
END
$$;
```
Thanks to Timescale cloud support for sharing these steps. 

## Find disk space used by a database
It's important to keep track of the disk space used by a db to ensure that you don't run out of disk space. Here's the command to get the size for a single database.

```sql
SELECT pg_size_pretty( pg_database_size(‘<db-name>’) );
```
This post in [postgreswiki](https://wiki.postgresql.org/wiki/Disk_Usage) has got more queries
