# Sakai Kurulumu
Sakai CLE kurulumunu gerçekleştirmek için oluşturulmuştur.

İşletim sistemi: CentOS 7 64bit

Hostname

nmtui
systemctl restart systemd-hostnamed
cat /etc/hostname
cat /etc/sysconfig/network
hostnamectl status

Disk Yapısı

LVM disk yapısı oluşturulmuştur. / LVM yapısının tercih etmemin sebebi daha sonradan disk gemnişletme işlemi yapabilmektir. Daha esnek bir yapıya sahiptir LVM

8 GB Swap alanı
500MB boot
154 GB /root

Firewall Ayarları

Centos 7 üzerinde varsayılan olarak gelen firewall kapatalır. Üniversitenin firewall devrede olduğu için sunucu üzerine firewall yapılandırılmayacaktır.
systemctl disable firewalld
systemctl stop firewalld
systemctl status firewalld

Ssh Ayarları

Terminal ise sunucumuza ssh bağlantısı üzerinden erieşbilmek için ssh servisini başlatmamız gerekmektedir. Bu işlemden sonra fiziksel olarak sunucunun yanınanda bulunmamıza gerek kalmayacaktır.
systemctl enable sshd.service
systemclt start sshd.service

Kurulan Servisler 

Nano / dosyaları editlemek için
yum install -y nano

Wget / internetten dosya indirmek için
yum install -y wget

Zip / Unzip 

yum install -y zip unzip

NMAP /portaları dinlemek için.
yum install -y nmap
nmap localhost

Java

mkdir -p /root/packages
cd /root/packages

wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/7u71-b14/jdk-7u71-linux-x64.tar.gz"

tar xpfz jdk-7u71-linux-x64.tar.gz

mv jdk1.7.0_71/ /opt/

cd /opt/jdk1.7.0_71/

alternatives --install /usr/bin/java java /opt/jdk1.7.0_71/bin/java 2

alternatives --config java
alternatives --install /usr/bin/jar jar /opt/jdk1.7.0_71/bin/jar 1
alternatives --install /usr/bin/javac javac /opt/jdk1.7.0_71/bin/javac 1
alternatives --set jar /opt/jdk1.7.0_71/bin/jar 
alternatives --set javac /opt/jdk1.7.0_71/bin/javac

nano /root/.bashrc
export JAVA_HOME=/opt/jdk1.7.0_72 export JRE_HOME=/opt/jdk1.7.0_72/jre export PATH=$PATH:/opt/jdk1.7.0_72/bin:/opt/jdk1.7.0_72/jre/bin
source /root/.bashrc

java -version

java version "1.7.0_72"
Java(TM) SE Runtime Environment (build 1.7.0_72-b14)
Java HotSpot(TM) 64-Bit Server VM (build 24.72-b04, mixed mode)


#Maven Ayarları

cd /root/packages
wget http://ftp.itu.edu.tr/Mirror/Apache/maven/maven-3/3.2.5/binaries/apache-maven-3.2.5-bin.tar.gz
tar xpfz apache-maven-3.2.5-bin.tar.gz
mv apache-maven-3.2.5 /opt/maven

nano /root/.bashrc

export MAVEN_HOME=/opt/maven
export PATH=$PATH:$MAVEN_HOME/bin
export MAVEN_OPTS='-Xms512m -Xmx1024m -XX:PermSize=256m -XX:MaxPermSize=512m -Djava.util.Arrays.useLegacyMergeSort=true'

source /root/.bashrc

mvn --version

Apache Maven 3.2.5 (12a6b3acb947671f09b81f49094c53f426d8cea1; 2014-12-14T19:29:23+02:00)
Maven home: /opt/maven
Java version: 1.7.0_72, vendor: Oracle Corporation
Java home: /opt/jdk1.7.0_72/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-123.el7.x86_64", arch: "amd64", family: "unix"

mkdir -p /root/.m2/repository/
nano /root/.m2/setting.xml

<settings xmlns="http://maven.apache.org/POM/4.0.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <profiles>
    <profile>
      <id>tomcat7</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <properties>
        <appserver.id>tomcat7</appserver.id>
        <appserver.home>/opt/tomcat</appserver.home>
        <maven.tomcat.home>/opt/tomcat</maven.tomcat.home>
        <sakai.appserver.home>/opt/tomcat</sakai.appserver.home>
        <surefire.reportFormat>plain</surefire.reportFormat>
        <surefire.useFile>false</surefire.useFile>
      </properties>
    </profile>
  </profiles>
</settings>



Tomcat Ayarları

cd /root/packages/

wget http://ftp.itu.edu.tr/Mirror/Apache/tomcat/tomcat-7/v7.0.57/bin/apache-tomcat-7.0.57.tar.gz

tar xpfz apache-tomcat-7.0.57.tar.gz
mv apache-tomcat-7.0.57 /opt/tomcat

nano /root/.bashrc
export CATALINA_HOME=/opt/tomcat
export PATH=$PATH:$CATALINA_HOME/bin
source /root/.bashrc


cd /opt/tomcat/bin

nano setenv.sh

export JAVA_OPTS='-server -d64 -Xms14336m -Xmx14336m -XX:PermSize=256m -XX:MaxPermSize=512m -XX:NewSize=384m -XX:MaxNewSize=512m -Djava.awt.headless=true -XX:+UseParallelGC -Dhttp.agent=Sakai -Dorg.apache.jasper.compiler.Parser.STRICT_QUOTE_ESCAPING=false -Dsun.lang.ClassLoader.allowArraySyntax=true -verbose:gc -Xloggc:/opt/tomcat/logs/sakai_gc.log -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -Duser.language=tr -Duser.region=TR'

chmod a+x *.sh

cd ../conf

nano server.xml

<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />

to

<Connector port="8080" maxHttpHeaderSize="8192"
               compression="on" compressableMimeTypes="text/html,text/xml,text/javascript,text/css,text/plain"
               maxThreads="1000" minSpareThreads="25" enableLookups="false" redirectPort="8443" acceptCount="250"
               connectionTimeout="20000" disableUploadTimeout="true" maxPostSize="0" URIEncoding="UTF-8"/>
cd ..

rm -rf webapps/*

nano conf/catalina.properties

common.loader=.... ekle

,${catalina.base}/common/classes/,${catalina.base}/common/lib/*.jar

shared.loader=...  ekle

${catalina.base}/shared/classes/,${catalina.base}/shared/lib/*.jar

server.loader=... ekle

${catalina.base}/server/classes/,${catalina.base}/server/lib/*.jar

mkdir -p shared/classes shared/lib common/classes common/lib server/classes server/lib


Subversion Ayarları

Sakai dosyalarını sunucuya indirme için subversion u kurmamız gerekiyor

yum install -u subversion

svn --version

svn, version 1.7.14 (r1542130)
   compiled Jun  9 2014, 18:54:44

Copyright (C) 2013 The Apache Software Foundation.
This software consists of contributions made by many people; see the NOTICE
file for more information.
Subversion is open source software, see http://subversion.apache.org/

The following repository access (RA) modules are available:

* ra_neon : Module for accessing a repository via WebDAV protocol using Neon.
  - handles 'http' scheme
  - handles 'https' scheme
* ra_svn : Module for accessing a repository using the svn network protocol.
  - with Cyrus SASL authentication
  - handles 'svn' scheme
* ra_local : Module for accessing a repository on local disk.
  - handles 'file' scheme

cd /opt/tomcat

svn co https://source.sakaiproject.org/svn/sakai/tags/sakai-10.3/ sakai

A    sakai/web/web-impl/pack/src/webapp/WEB-INF
A    sakai/web/web-impl/pack/src/webapp/WEB-INF/components.xml
A    sakai/web/web-impl/pack/pom.xml
 U   sakai/web
Checked out external at revision 316732.

Checked out revision 316732.

cd sakai/master
mvn clean install


[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.078 s
[INFO] Finished at: 2015-01-13T17:29:37+02:00
[INFO] Final Memory: 17M/491M

cd ..

mvn clean install sakai:deploy -Dmaven.tomcat.home=/opt/tomcat -Dsakai.home=/opt/tomcat/sakai -Djava.net.preferIPv4Stack=true

[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 26:52 min
[INFO] Finished at: 2015-01-13T17:58:26+02:00
[INFO] Final Memory: 438M/874M





Dil Dosyalarının Yüklenmesi

cd /root/packages
wget https://crowdin.com/download/project/sakai-cle.zip
unzip sakai-cle.zip
cd tr

find . -name '*.properties' -exec sh -c 'mv "$0" "${0%.properties}_tr_TR.properties"' {} \;  // for centos

find . -name "*.properties" -exec rename 's/\.properties$/_tr_TR.properties/' '{}' \; // for debian

/bin/cp -rf * /opt/tomcat/sakai/

/opt/tomcat/bin/shutdown.sh
tail -f /opt/tomcat/logs/catalina.out  // kapandığından emin olunuz.

rm -rf /root/.m2/repository/*
cd /opt/tomcat/sakai

mvn clean install sakai:deploy -Dmaven.tomcat.home=/opt/tomcat -Dsakai.home=/opt/tomcat/sakai -Djava.net.preferIPv4Stack=true

nano /opt/tomcat/bin/setenv.sh  / aşağıdaki 2 parametreyi ekle

-Duser.language=tr -Duser.region=TR

Sunucu Saati

timedatectl list-timezones
timedatectl set-timezone Europe/Istanbul

yum install -y ntpdate
systemctl enable ntpdate
systemctl start ntpdate

ntpdate 0.tr.pool.ntp.org
hwclock --systohc


Bigbluebutton Tool Kurulumu

cd /root/packages

wget https://github.com/blindsidenetworks/bigbluebutton-sakai/archive/1.0.7-SNAPSHOT.zip
unzip 1.0.7-SNAPSHOT.zip
cd bigbluebutton-sakai-1.0.7-SNAPSHOT
cp -R bbb-tool/ /opt/tomcat/sakai/
cd /opt/tomcat/sakai/bbb-tool
mvn clean install sakai:deploy -Dmaven.tomcat.home=/opt/tomcat -Dsakai.home=/opt/tomcat/sakai -Djava.net.preferIPv4Stack=true

[INFO] BigBlueButton ...................................... SUCCESS [  0.255 s]
[INFO] BigBlueButton API .................................. SUCCESS [ 14.075 s]
[INFO] BigBlueButton Bundles .............................. SUCCESS [  0.408 s]
[INFO] BigBlueButton Implementation ....................... SUCCESS [ 18.008 s]
[INFO] BigBlueButton Component Pack ....................... SUCCESS [  7.581 s]
[INFO] BigBlueButton Tool ................................. SUCCESS [ 28.290 s]
[INFO] BigBlueButton Assembly ............................. SUCCESS [ 13.472 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 01:38 min
[INFO] Finished at: 2015-01-14T16:21:06+02:00
[INFO] Final Memory: 42M/501M
[INFO] --------------------------------------------------

NFS Ayarları

yum install -y nfs-utils nfs-utils-lib
systemctl enable rpcbind
systemctl enable nfs-server
systemctl enable nfs-lock
systemctl enable nfs-idmap

mkdir /data
mount -t nfs 172.16.250.2:/vol/volFeyasar/  /data/
nano /etc/fstab
172.16.250.2:/vol/volFeyasar    /data   nfs     defaults 0 0
rpcinfo -p

Sakai Güvenlik

cd /opt/tomcat/sakai
mkdir antisamy
cd antisamy
