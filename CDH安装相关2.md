第一步  修改Hostname:

1)systemctl set-hostname master01(master02,manager,slave01-06)

2)vim /etc/hosts

10.32.168.86 master01
10.32.168.87 master02 
10.32.168.88 manager 
10.32.168.119 slave01 
10.32.168.120 slave02 
10.32.168.122 slave03 
10.32.168.123 slave04 
10.32.168.124 slave05
10.32.168.125 slave06 
10.32.168.89 datadev01 
10.32.168.90 datadev02 
10.32.168.91 datadev03

3)vim /etc/sysconfig/network

# Created by anaconda 
PEERNTP=no 
NETWORKING_IPV6=no 
GATEWAY=10.32.168.247 
HOSTNAME=master01

第二步：ssh 免密码登陆   ssh-copy-i root@master01

第三步：防火墙和SELinux 关闭

systemctl stop firewalld.service

systemctl disable firewalld.service

selinux disable

内存策略配置：

vim /etc/rc.local

########################################

if test -f /sys/kernel/mm/transparent_hugepage/enabled; then
echo never > /sys/kernel/mm/transparent_hugepage/enabled
fi

if test -f /sys/kernel/mm/transparent_hugepage/defrag; then
echo never > /sys/kernel/mm/transparent_hugepage/defrag
fi

########################################

chmod +x /etc/rc.d/rc.local

vim /etc/security/limits.conf

* soft nofile 65535
* hard nofile 65535

第四步：配置所有服务器时间同步

1）查看时区 timedatectl

2)  安装 ntp 服务器

yum install ntp 

systemctl start ntpd.service

3) 这个默认已经设置了，等安装完毕以后更新



第五步：安装相关的软件 所有节点

yum install psmisc

yum install python-lxml

yum install httpd (很重要)
yum install mod_ssl  （很重要）

第六步：主节点安装 mysql

rpm -qa|grep mysql*

rpm -qa|grep mari*

yum remove mariadb-libs-5.5.44-2.el7.centos.x86_64

rpm -ivh mysql-community-release-el6.rpm

yum install mysql-community-server

systemctl start mysqld

/sbin/chkconfig mysqld on

mysqladmin -u root password '123456'

授权root用户使用密码从任意主机连接到mysql服务器

GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY '123456' WITH GRANT OPTION;

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;

flush privileges;



创建CM库

create database hive DEFAULT CHARSET utf8 COLLATE utf8_general_ci;

create database amon DEFAULT CHARSET utf8 COLLATE utf8_general_ci;

create database hue DEFAULT CHARSET utf8 COLLATE utf8_general_ci;

create database oozie DEFAULT CHARSET utf8 COLLATE utf8_general_ci;

cp /usr/src/mysql-connector-java-5.1.38.jar /opt/cm-5.15.0/share/cmf/lib/

useradd --system --home=/opt/cm-5.15.0/run/cloudera-scm-server/ --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm

/opt/cm-5.15.0/share/cmf/schema/scm_prepare_database.sh mysql cm -hmanager -uroot -pyhdsj123456 --scm-host manager scm scm scm
