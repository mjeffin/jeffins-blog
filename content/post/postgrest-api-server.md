---
title: "Postgrest - Instant API for postgres"
date: 2021-11-17T07:44:59+05:30
draft: true
tags: ["postgres"]
---

[Postgrest](https://postgrest.org/) is one of the most awesome tools I have had the pleasure of discovering. It's a single binary which can turn a postgres database 
into restful api server. 90% of the work involved in a typical webapp is simple CRUD(Create-Read-Update-Delete) operations, and postgrest automates most of it, without 
having to write any application code. The most important aspect of app development is getting the data model right, and often it's difficult to get it right the first time. 
Postgrest enables you to instantly reflect the data models changes in the rest api.

# Comparison

How does it compare to the typical pattern of using an ORM like [Django](https://www.djangoproject.com/) or RoR or Hibernate? Assume that you are already familiar with SQL in general 
and postgres in particular. Let's write down the steps involved in Django or DRF,

1. Read the framework documentation and learn how the field types maps to SQL. This probably takes few hours, if not days
2. Create models for each table.
3. Create serializers for each model
4. Create validators along with serializers to sanitize input data
5. Create viewsets for each model
6. Create url definitions for each model
7. Deploy it as proxy behind a server like nginx

Now, the steps involved in postgrest are,

1. Create the schema in plain sql, preferably using a migration tool like [golang-migrate](https://github.com/golang-migrate/migrate)
2. Create a jwt token to represent db user (one time)
3. Create a 5 line configuration file for postgres, including database url, token info etc(one time)
4. Download the postgres binary and run it as systemd service or just run the docker image and pass the configuration file
5. Deploy it as proxy behind a server like nginx

Any changes to the schema will be automatically picked up by postgrest. No need to add serializers or viewsets or new urls. Just pure magic !

# Examples

[Documentation](https://postgrest.org/en/v8.0/tutorials/tut0.html) has step-by-step instructions on getting it running. Let us a look an 
example scenario to demonstrate usefulness of postgrest beyond simple updating of data. Combining this with advanced sql features like
constraints,triggers, functions etc can take you very, very far, without having to write any code apart from SQL

## Scenario




