# dyggy build log

### 4/2/2021 3:09PM
 - Registered domain dyggy.ml on freenom
 - Bought VPS with Ubuntu 20.04 and 20GB SSD
 
### 4/2/2021 3:21PM
 - Updated all packages
 - Installed `mariadb-server` and `mariadb-client`
 - Configured secure installation with `mysql_secure_installation`
 
### 4/2/2021 3:36PM
 - Configured users database
```
MariaDB [(none)]> create database dyggy;
Query OK, 1 row affected (0.000 sec)

MariaDB [(none)]> use dyggy;
Database changed
MariaDB [dyggy]> create table users (
    -> id int unsigned auto_increment primary key,
    -> nick varchar(30) not null,
    -> active bool,
    -> password char(64),
    -> salt char(64),
    -> regdate timestamp,
    -> cookie varchar(32)
    -> );
Query OK, 0 rows affected (0.022 sec)
```

 - Added MariaDB user `dyggy` with `INSERT` and `SELECT` privileges
```
MariaDB [dyggy]> create user dyggy;
Query OK, 0 rows affected (0.001 sec)

MariaDB [dyggy]> grant select, insert on dyggy.* to 'dyggy'@'%';
Query OK, 0 rows affected (0.001 sec)
```

### 4/2/2021 3:40PM
 - Installed package `python3-pip`
 - Installed pip package `pymysql`

### 4/2/2021 4:11PM
 - Tested python3-MariaDB connection
 - Installed nginx and fcgiwrap
 - Created catalog `/var/www/dyggy`
 - Configured nginx site
```
server {
        listen 80 default_server;
        listen [::]:80 default_server;
        index index index.html index.htm index.nginx-debian.html;
        server_name _;
        location = / {
                return 301 http://dyggy.ml/index;
        }

        location / {
                root /var/www/dyggy;
                include fastcgi_params;
                fastcgi_pass unix:/var/run/fcgiwrap.socket;
                fastcgi_param SCRIPT_FILENAME /var/www/dyggy$fastcgi_script_name;
        }
}
```

### 4/2/2021 7:26PM
 - Created registration and login
 - Note: `select * from tbl_name order by id desc limit N;`

### 5/2/2021 8:34AM
 - Created posts table
```
MariaDB [dyggy]> create table posts ( id int unsigned auto_increment primary key, nick varchar(30) not null, value varchar(300) not null, date timestamp, isreply bool, replyto int unsigned, replies int unsigned, hearts int unsigned );
```