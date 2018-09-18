下载路径

https://archive.cloudera.com/cdh5/parcels/5.14.0.24/

http://archive.cloudera.com/cm5/cm/5/

if test -f /sys/kernel/mm/transparent_hugepage/enabled; then
   echo never > /sys/kernel/mm/transparent_hugepage/enabled
fi

echo 0 > /proc/sys/vm/swappiness （最大化利用内存）

echo never > /sys/kernel/mm/transparent_hugepage/defrag

echo never > /sys/kernel/mm/transparent_hugepage/enabled

/usr/sbin/ntpdate -u ntp1.aliyun.com >> /var/log/ntpdate.log

yum install –y psmisc
yum install -y python-lxml

yum install httpd
yum install mod_ssl 

注意 mysql-connector-java-5.1.38.jar  要拷贝到好几个地方

第一个  /usr/share/java/ 下面要有 mysql-connector-java.jar

第二个 /opt/cloudera/parcels/CDH-5.14.2-1.cdh5.14.2.p0.3/lib/hive/lib/

第三个 /var/lib/oozie





遇到的问题：

对于此 Cloudera Manager 版本 (5.4.7) 太新的 CDH 版本不会显示
问题描述：Versions of CDH that are too new for this version of Cloudera Manager (5.4.7) will not be shown.
问题定位：PARCELS表fileName=CDH-5.4.7-1.cdh5.4.7.p0.3-el6.parcel的hash值为null，判断为parcel文件问题或scm数据库生成干扰问题
解决思路：判断为/opt/cloudera/parcel-repo/目录内的本地parcel应该在scm数据库初始化结束后再放入
问题解决：停止server>重置scm数据库>启动server>复制parcel到/opt/cloudera/parcel-repo/目录



 vim /opt/cm-5.12.1/lib64/cmf/service/client/deploy-cc.sh

JAVA_HOME=/usr/local/java
export JAVA_HOME=/usr/local/java
副本块不足参考 http://blog.xumingxiang.com/259.html https://blog.csdn.net/HFUTLXM/article/details/77605915?locationNum=8&fps=1
