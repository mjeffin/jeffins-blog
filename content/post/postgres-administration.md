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

## Find disk space used by a database
It's important to keep track of the disk space used by a db to ensure that you don't run out of disk space. Here's the command to get the size for a single database.

```sql
SELECT pg_size_pretty( pg_database_size(‘<db-name>’) );
```
This post in [postgreswiki](https://wiki.postgresql.org/wiki/Disk_Usage) has got more queries
