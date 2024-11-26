---
title: Database Installation
type: docs
prev: /_index
next: docs/install
---



To install you will first need the database and queue manager running. Every instance of Glympse you run will need to connect to these containers. 

First create your docker network. `docker network create glympse_net`

Then create a a directory and compose file:
```
mkdir /opt/glympse_database

cd /opt/glympse_database

nano /opt/glympse_database/compose.yml
```

Paste the following, be sure to change the passwords and any other personal details before saving. 

```yaml {filename="compose.yml"}
services:
  redis:
    image: redis:alpine
    container_name: redis
    restart: always
    ports:
      - 6379:6379
    volumes:
      - /opt/glympse_database/redis_data:/data
    healthcheck:
      test:
        - CMD
        - redis-cli
        - ping
      interval: 30s
      timeout: 10s
      retries: 3
    networks:
      glympse_net: null      
  rabbitmq:
    image: rabbitmq:3.13.4-management
    container_name: rabbitmq
    volumes:
      - type: bind
        source: /opt/glympse_database/rabbitmq.conf
        target: /etc/rabbitmq/conf.d/10-defaults.conf
    ports:
      - 15672:15672
      - 5672:5672
    networks:
      glympse_net: null      
  mysql:
    image: mysql:8.0
    container_name: mysql
    environment:
      MYSQL_ROOT_PASSWORD: <root password>
      MYSQL_DATABASE: glympse
      MYSQL_USER: glympse
      MYSQL_PASSWORD: <glympse password>
    volumes:
      - /opt/glympse/mysql:/var/lib/mysql
      - type: bind
        source: /opt/glympse_database/my.cnf
        target: /etc/my.cnf
    ports:
      - 6033:6033
      - 3306:3306
    networks:
      glympse_net: null
networks:
  glympse_net:
    external: true
```
There are two more files that you need to create and save before running the compose file. 

First is the mysql config file. 

Create a file called my.cnf in the glympse_database directory: `nano my.cnf` and paste the following:

```conf {filename="my.cnf"}
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/8.0/en/server-configuration-defaults.html

[mysqld]
#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M
#
# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin
#
# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M

# Remove leading # to revert to previous value for default_authentication_plugin,
# this will increase compatibility with older clients. For background, see:
# https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_default_authentication_plugin
# default-authentication-plugin=mysql_native_password
skip-host-cache
skip-name-resolve
datadir=/var/lib/mysql
socket=/var/run/mysqld/mysqld.sock
secure-file-priv=/var/lib/mysql-files
user=mysql
connect_timeout = 600
net_read_timeout = 30
wait_timeout = 28800
interactive_timeout = 28800

pid-file=/var/run/mysqld/mysqld.pid
[client]
socket=/var/run/mysqld/mysqld.sock

!includedir /etc/mysql/conf.d/

```

Finally create a config file for rabbitmq: `nano rabbitmq.conf` and paste the following:

be sure to change the password for something secure and keep note of it as you will need it for your compose env files. 

```conf {filename="rabbitmq.conf"}
default_vhost = glympse
default_user = glympse
default_pass = <enter password>
default_permissions.configure = .*
default_permissions.read = .*
default_permissions.write = .*
default_user_tags.administrator = true
default_user_tags.management = true
default_user_tags.glympse = true
consumer_timeout = 31622400000
```