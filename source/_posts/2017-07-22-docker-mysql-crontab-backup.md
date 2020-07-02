---
layout: post
title: docker mysql crontab backup
date: 2017-07-22 17:18:07
tags:
- docker
---

在既有的 mysql 中加入 crontab 的方式去定期備份 data。


## Step 1. init database

希望一開始就幫我們把資料加入，因此 mount 一個 docker-entrypoint-initdb.d 到 container 內的。可參考[[Mysql Docker Hub](https://hub.docker.com/_/mysql/)]

> Initializing a fresh instance
>
> When a container is started for the first time, a new database with the specified name will be created and initialized with the provided configuration variables. Furthermore, it will execute files with extensions .sh, .sql and .sql.gz that are found in /docker-entrypoint-initdb.d. Files will be executed in alphabetical order. You can easily populate your mysql services by mounting a SQL dump into that directory and provide custom images with contributed data. SQL files will be imported by default to the database specified by the MYSQL_DATABASE variable.


```
version: '2'
services:
    mysql:
        image: mysql:latest
	volumes:
		- ./docker-entrypoint-initdb.d:/docker-entrypoint-initdb.d
	environment:
		MYSQL_ROOT_PASSWORD: password
		MYSQL_DATABASE: develop
		MYSQL_USER: test
		MYSQL_PASSWORD: testpassword
```

而在 docker-entrypoint-initdb.d 內放入一小段 sql。

**init.sql**

```
CREATE DATABASE IF NOT EXISTS develop;
USE develop;
CREATE TABLE IF NOT EXISTS tasks (
    task_id INT(11) NOT NULL AUTO_INCREMENT,
    subject VARCHAR(45) DEFAULT NULL,
    PRIMARY KEY (task_id)
) ENGINE=InnoDB;

INSERT INTO tasks (subject)
VALUES ('task 1'),
       ('task 2');
```

## Step 2. Add crontab


特地把 mysql_backup mount 到 local 的 backup_sql。


```
 crontab:
        build: ./crontab/
        volumes:
            - ./mysql_backup:/backup_sql
        links:
            - mysql
```

調整你希望備份的時間區間，可修改 `crontab.txt`。

```
*/1 * * * * /script.sh
```

調整你的執行 mysqldump 的 sh，可修改 `script.sh`

```
#!/bin/sh

now=$(date +%Y-%m-%d_%H-%M-%S)

mysqldump -u test -ptestpassword -h mysql develop > /backup_sql/$now.sql
```

全部的東西可在 [Github](https://github.com/lighter/docker-mysql-backup) 找到。


## Ref

* [How to run a cron job inside a docker container](https://stackoverflow.com/questions/37015624/how-to-run-a-cron-job-inside-a-docker-container)
* [Docker Hub - Mysql](https://hub.docker.com/_/mysql/)

