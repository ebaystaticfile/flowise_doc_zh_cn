---
description: 学习如何将您的 Flowise 实例连接到数据库
---

# 数据库

***

Flowise 支持 4 种数据库类型：

* SQLite
* MySQL
* PostgreSQL
* MariaDB

## SQLite（默认）

SQLite 将作为默认数据库。这些数据库可以使用以下环境变量进行配置：

代码块 0sh
DATABASE_TYPE=sqlite
DATABASE_PATH=/root/.flowise # 您首选的位置
代码块 1

一个 `database.sqlite` 文件将被创建并保存到 `DATABASE_PATH` 指定的路径中。如果未指定，则默认存储路径将位于您的主目录 -> .flowise

**注意：**如果没有指定任何环境变量，SQLite 将作为备用数据库选择。

## MySQL

代码块 2sh
DATABASE_TYPE=mysql
DATABASE_PORT=3306
DATABASE_HOST=localhost
DATABASE_NAME=flowise
DATABASE_USER=user
DATABASE_PASSWORD=123
代码块 3

## PostgreSQL

```sh
DATABASE_TYPE=postgres
DATABASE_PORT=5432
DATABASE_HOST=localhost
DATABASE_NAME=flowise
DATABASE_USER=user
DATABASE_PASSWORD=123
PGSSLMODE=require
```

## MariaDB

```bash
DATABASE_TYPE="mariadb"
DATABASE_PORT="3306"
DATABASE_HOST="localhost"
DATABASE_NAME="flowise"
DATABASE_USER="flowise"
DATABASE_PASSWORD="mypassword"
```

## 如何使用 Flowise 数据库 SQLite 和 MySQL/MariaDB

{% embed url="https://youtu.be/R-6uV1Cb8I8" %}
