---
title: "Postgres Administration"
date: 2021-05-24T20:00:00+05:30
draft: false
tags: ["postgres","snippets"]

---

This post is a collection of notes & nippets related to administering a postgres database.

## Find disk space used by a database

```sql
SELECT pg_size_pretty( pg_database_size(‘<db-name>’) );

```