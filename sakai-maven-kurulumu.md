# Maven Kurulumu (CentOS, RHEL, yada Fedora)

Maven Apache lisansı ile open source olarak proje yönetim aracı olarak kullanılmaktadır.

Packages dizinize geçtikten sonra maven dosylarını wget ile sunucumuza indiriyoruz.
```
cd /root/packages
```
```
wget http://mirror.idealhosting.net.tr/Apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
```
İndirdiğimiz maven dosyasını tar komutu ile açıp daha sonra opt dizininin altına maven ismiyle taşıyoruz.
```
tar xpfz apache-maven-3.3.9-bin.tar.gz
mv apache-maven-3.3.9 /opt
cd /opt
ln -s apache-maven-3.3.9/ maven
```
Maven kurulumu için çevresel değişkenleri .bashrc dosyasını nano ile açıp içine export ifadelerini ekliyoruz. Bu sayede sunucu her başladığında maven çalışması için maven in bulunduğu dizin ve maven için memory ayarları tanımlanmış olacaktır.

```
nano /root/.bashrc

export MAVEN_HOME=/opt/maven
export PATH=$PATH:$MAVEN_HOME/bin
export MAVEN_OPTS='-Xms512m -Xmx1024m -Djava.util.Arrays.useLegacyMergeSort=true'
```
Sisteme restart atmak yerine source komutu ile .bashrc dosyasının içeriğini sisteme tanıtmış oluyoruz.
```
source /root/.bashrc
```
Maven ın sürüm kontrolünü aşağıdaki komut ile yapabilirsiniz.
```
mvn --version
```
Maven ekran çıktısı aşağıdaki gibi olmalıdır.
```
Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-10T16:41:47+00:00)
Maven home: /opt/maven
Java home: /opt/jdk1.8.0_91/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-327.22.2.el7.x86_64", arch: "amd64", family: "unix"
```

Maven ayarları setting.xml dosyasının içinde bulunmaktadır. Projede kullanılacak repository dosyalarının nerede tutulacağına bu dosyada tanımlarız ve projede kullanılacak plugin respository vs bu dizine indirilecektir.

Maven repository lerinin indirileceği dizini oluşturuyoruz.
```
mkdir -p /root/.m2/repository/
```
Ve maven ayarlarını tanımlayacağımız ayar dosyanını oluşuruyoruz.
```
nano /root/.m2/setting.xml
```
Maven ayarlarımızı setting.xml dosyasına ekliyoruz.
```
<settings xmlns="http://maven.apache.org/POM/4.0.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <profiles>
    <profile>
      <id>tomcat</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <properties>
        <appserver.id>tomcat</appserver.id>
        <appserver.home>/opt/tomcat</appserver.home>
        <maven.tomcat.home>/opt/tomcat</maven.tomcat.home>
        <sakai.appserver.home>/opt/tomcat</sakai.appserver.home>
        <surefire.reportFormat>plain</surefire.reportFormat>
        <surefire.useFile>false</surefire.useFile>
      </properties>
    </profile>
  </profiles>
</settings>
```

Maven kurulumu tamanlandı. Bir sonraki aşama olan [Tomcat Kurulumu] (sakai-tomcat-kurulumu.md) kurulumuna geçebilirsiniz.
