安装 postgres 数据库 

yum install postgresql-server postgresql-contrib

yum install https://download.postgresql.org/pub/repos/yum/9.6/redhat/rhel-7-x86_64/pgdg-redhat96-9.6-3.noarch.rpm

yum install postgresql96

yum install postgresql96-server

初始化数据库

/usr/pgsql-9.6/bin/postgresql96-setup initdb

systemctl enable postgresql-9.6

systemctl start postgresql-9.6



默认用户
postgres安装完成后，会自动在操作系统和postgres数据库中分别创建一个名为postgres的用户以及一个同样名为postgres的数据库。

登陆：

1指定方式登陆 psql -U username -d database_name -h host -W

说明：参数含义: -U指定用户 -d要连接的数据库 -h要连接的主机 -W提示输入密码。


yum 安装 postgresql ，默认会创建一个名为 postgres 的系统账号，用于执行 postgresql;

-bash-4.2$ psql -U postgres

修改用户postgres的密码

postgres=# alter user postgres with password 'startdt1234';
ALTER ROLE

修改配置文件

vim /var/lib/pgsql/9.6/data/postgresql.conf

##配置文件中，默认只能本机访问postgresql；
##修改listen_addresses = 'localhost'为listen_addresses = '*'，允许所有远程访问；

![image](https://github.com/starzjl/ops/blob/master/images/01.png)

vim /var/lib/pgsql/9.6/data/pg_hba.conf

第一个"all"代表 db，第二个"all"代表user，0.0.0.0/0代表client ip , 后面 trust , peer , ident , md5 , password  是认证方式

![image](https://github.com/starzjl/ops/blob/master/images/02.png)

查询最大连接数：

show max_connections;

查询当前连接数：

select count(1) from pg_stat_activity;

create role repl login replication encrypted password '51idc.com';



wal_level = hot_standby
