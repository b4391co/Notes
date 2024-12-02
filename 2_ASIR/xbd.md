<h1 style="text-align:center;font-size:40px;">XBD</h1>

## Configuracion

### CMD

```bash
mysql -u root -p
select X ; # X = version() , current_date , current_time , user() , now()...
create database X;
show databases; # ver todas las bases de datos
describe T ; # ver características de tabla
```

### PODER IMPORTAR TABLAS

```sql
show global variables like 'local_infile';
set global local_infile=true;
exit
mysql -u USER -p --local-infile # iniciar con local infile
LOAD DATA LOCAL INFILE 'RUTA' INTO TABLE T;
```

### MODIFICAR VARIABLES

```sql
show global variables like 'x';
set global x=X;
```

### permitir conexion a la bdd para web php

#### php

```bash
apt install php-mysql
apt install php-fpm
iptables -A INPUT -i eth0 -p tcp --destination-port 3306 -j ACCEPT
```

#### mysql / mariadb

```sql
CREATE DATABASE IF NOT EXISTS `namedb`;
CREATE USER username@IP identified by 'abc123.';
GRANT ALL PRIVILEGES ON namedb.* TO username@localhost
FLUSH PRIVILEGES;
SELECT User, Host FROM mysql.user;
```