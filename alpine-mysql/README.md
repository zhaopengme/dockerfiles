# alpine-mysql
a docker image base on alpine with mysql

# build image
```
docker build -t zhaopengme/alpine-mysql .
docker run -it --rm -v $(pwd)/data:/var/lib/mysql -p 3306:3306 zhaopengme/alpine-mysql
```


# Usage
```
docker run -it --name mysql -p 3306:3306 -e MYSQL_DATABASE=admin -e MYSQL_USER=tom -e MYSQL_PASSWORD=tom -e MYSQL_ROOT_PASSWORD=111111 zhaopengme/alpine-mysql

docker run -it --name mysql -p 3306:3306 -v $(pwd)/data:/var/lib/mysql -e MYSQL_DATABASE=admin -e MYSQL_USER=tom -e MYSQL_PASSWORD=tom -e MYSQL_ROOT_PASSWORD=111111 zhaopengme/alpine-mysql
```

It will create a new db, and set mysql root password(default is 111111)

# Issue

```
[ERROR] InnoDB: The Auto-extending innodb_system data file './ibdata1' is of a different size 0 pages than specified in the .cnf file: initial 768 pages, max 0 (relevant if non-zero) pages!
```

这个错误按照下面的方式依旧无法解决,所以在 macos 上面,数据只能存储在容器中.

```
#!/bin/bash
set -e

# Script to workaround docker-machine/boot2docker OSX host volume issues: https://github.com/docker-library/mysql/issues/99

echo '* Working around permission errors locally by making sure that "mysql" uses the same uid and gid as the host volume'
TARGET_UID=$(stat -c "%u" /var/lib/mysql)
echo '-- Setting mysql user to use uid '$TARGET_UID
usermod -o -u $TARGET_UID mysql || true
TARGET_GID=$(stat -c "%g" /var/lib/mysql)
echo '-- Setting mysql group to use gid '$TARGET_GID
groupmod -o -g $TARGET_GID mysql || true
echo
echo '* Starting MySQL'
chown -R mysql:root /var/run/mysqld/
/entrypoint.sh mysqld --user=mysql --console
```
