
---
date: '2025-06-23T16:26:02+07:00'
draft: false
title: 'Cassandra on Mac'
summary: 'Quick guide to installing and using Cassandra on macOS'
categories:

- Code
tags:
- cassandra
- nosql
- cqlsh
- database

---

# Cassandra on Mac

This guide covers installing Apache Cassandra on macOS, connecting with `cqlsh`, and basic usage.

## Installation

Install Cassandra using Homebrew:

```sh
brew install cassandra
```

## Connecting with cqlsh

Connect to your Cassandra instance:

```sh
cqlsh <ip_address> 9042 \
    -u <username> \
    -p <password> \
    --cqlversion="3.4.7"
```

Switch to your keyspace:

```sql
DESCRIBE KEYSPACES;
USE <keyspace_name>;
```

## Securing cqlsh

To enable authentication, follow the [official guide](https://cassandra.apache.org/_/blog/Apache-Cassandra-4.1-Features-Authentication-Plugin-Support-for-CQLSH.html).

Create or edit `~/.cassandra/credentials`:

```ini
[PlainTextAuthProvider]
username = user1
password = pass1
```

## Basic Queries

Show all tables in the current keyspace:

```sql
DESCRIBE TABLES;
-- or --
DESC TABLES;
```

Show the schema for a specific table:

```sql
DESCRIBE TABLE <table_name>;
-- or --
DESC <table_name>;
```
