#### mysql开启binlog模式

```
--docker环境 mysql容器
docker exec -it mysql /bin/bash
cd /etc/mysql/mysql.conf.d
vi mysqld.cnf

补充：
	进入docker容器后，无法使用vi编辑命令

解决方法：
    1.apt-get update 
    2.apt-get install vim
```

添加配置

```
log-bin=/var/lib/mysql/mysql-bin
server-id=12345
```

使用root账号创建用户并授予权限

```properties
create user canal@'%' IDENTIFIED by 'canal';
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT,SUPER ON *.* TO 'canal'@'%';
FLUSH PRIVILEGES;
```

重启mysql容器

```properties
docker restart mysql
```

#### mysql修改时区

```
--docker环境  （mysql容器）
-- 进入mysql容器
docker exec -it mysql /bin/bash
```

```
cd /etc/mysql/mysql.conf.d
vi mysqld.cnf
datadir 下面另加一行 default-time_zone = '+8:00' 

⚠️ default-time_zone = '+8:00' 比较简单，如果想更容易理解的话也可以：default-time_zone = 'Asia/Shanghai' ，都是一样的效果。
```

