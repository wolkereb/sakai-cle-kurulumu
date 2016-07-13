## MySql 5.6 Kurulumu (CentOS, RHEL, yada Fedora)
Sakai resmi olarak MySql 5.5.x versiyonu desteklesede nightly2.sakaiproject.org serverlarının tümünde MySql 5.6.x sümünü kullanmaktadır.  Bu yüzden bende MySql 5.5.x yerine MySql 5.6.x sürümünü kullanacağım.

MySql sunucusunu sakainin kurulu olduğu sunucu da yapacağım sırası ile aşağıdaki adımları gerçekleştirebilirsiniz.
Packages dizinine girerek mysql repolarını sunucumuza indiriyoruz. İndirdikten sonra rpm i sunucumuza kuruyoruz.

```
cd /root/packages
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
```

Mysql repoları sunucumuza yüklenip yüklenmediğini aşağıdaki dizini listeleyerek kontrol ediyoruz.

```
ls -l /etc/yum.repos.d/mysql-community*
```
Ekran çıktısı aşağıdaki gibi olmalıdır.
```
[root@sakai packages]# ls -l /etc/yum.repos.d/mysql-community*
-rw-r--r--. 1 root root 1209 Jan 29  2014 /etc/yum.repos.d/mysql-community.repo
-rw-r--r--. 1 root root 1060 Jan 29  2014 /etc/yum.repos.d/mysql-community-source.repo
```

Mysql server aşağıdaki komut ile kuruyoruz.
```
yum install mysql-server
```

Sırasıyla aşağıdaki komutları çalıştırıyoruz. İlk komut mysql i başlatacak, ikinci komut ile mysql in çalışıp çalışmadığını öğreneceğiz, üçüncü komut ile mysql in daha güvenli hale gelmesi için bir root şifresi belirleyip bazı özellikleriyle oynayacağız. mysql_secure_installation komutunu çalıştırdıktan sonra sırasıyla bir root şifresi belirleyip size sorunlan sorulara Yes diyerek geçebilirsiniz.

```
systemctl start mysqld
systemctl status mysqld
mysql_secure_installation
```

Sürümünü kontrol etmek için "mysql --version" komutunu kullanabilirsiniz.
Ekran çıktısı aşağıdaki gibi olmalıdır.

```
mysql  Ver 14.14 Distrib 5.6.29, for Linux (x86_64) using  EditLine wrapper
```

MySql sunucumuz hazır sırada sakai için bir veritabanı ve bir kullanıcı oluşturup bu kullanıcıya veri tabanı üzerinden gerekli yetkileri veriyoruz.
```
[root@sakai packages]# mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 15
Server version: 5.6.29 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> create database sakai default character set utf8;
Query OK, 1 row affected (0.00 sec)

mysql> grant all on sakai.* to sakaiuser@'localhost' identified by 'sakaipassword';
Query OK, 0 rows affected (0.00 sec)

mysql> grant all on sakai.* to sakaiuser@'127.0.0.1' identified by 'sakaipassword';
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> quit
Bye
```

Mysql için varsayılan mysql motorunu InnoDB, lower_case_table_names ifadeside 1 yapıyoruz ve daha sonra mysql sunucusunu yeniden başlatıyoruz.
```
nano /etc/my.cnf

[mysqld]
default-storage-engine=InnoDB
lower_case_table_names=1
```

```
systemctl restart mysqld
```

Tomcatin mysql ile haberleşmesi için gerekli mysql Connector ü /opt/tomcat/common/lib/ dizini altına kopyalıyoruz.
```
cd /root/packages/
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.38.tar.gz
tar xpfz mysql-connector-java-5.1.38.tar.gz 
cd mysql-connector-java-5.1.38
cp mysql-connector-java-5.1.38-bin.jar /opt/tomcat/common/lib/
```

sakai.properties dosyasına aşağıdaki ifadeleri ekleyip sakaiyi başlattığımızda sakai tüm sql dosyaları çalıştırarak tablolarını sakai veritabanına taşıyacaktır.

auto.ddl ifadesini tablolar oluştuktan sonra kapatmayı unutmayınız, açık kaldığı zaman sakaiyi her yeniden başlattığınızda tabloları yeniden oluşturmaya başlayacaktır.

Yeni bir modül vs kurmaya çalıştığınızda auto.ddl ifadesini true yapmalısınız.


Mysql Ayarları
```
auto.ddl=true
username@javax.sql.BaseDataSource=sakaiuser
password@javax.sql.BaseDataSource=sakaipassword
vendor@org.sakaiproject.db.api.SqlService=mysql
driverClassName@javax.sql.BaseDataSource=com.mysql.jdbc.Driver
hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
url@javax.sql.BaseDataSource=jdbc:mysql://127.0.0.1:3306/sakai?useUnicode=true&characterEncoding=UTF-8&useServerPrepStmts=false&cachePrepStmts=true&prepStmtCacheSize=4096&prepStmtCacheSqlLimit=4096
validationQuery@javax.sql.BaseDataSource=select 1 from DUAL
defaultTransactionIsolationString@javax.sql.BaseDataSource=TRANSACTION_READ_COMMITTED
```
