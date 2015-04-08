#### Java Kurulumu
Java kurulumunu kaynak koddan yağacağımız için aşağıdaki adımları sırasıyla takip ediniz. Kaynak koddan kurulumu tercih etmemin sebebi kurulum dosyalarına hakim olmaktır.
```
mkdir -p /root/packages  
cd /root/packages
```
Java dosyalarını indirin
```
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/7u76-b13/jdk-7u76-linux-x64.tar.gz"wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/7u76-b13/jdk-7u76-linux-x64.tar.gz"
```
İndirdiğimiz java doslarını tar komutu ile açıp opt dizini altına taşıyalım.
```
tar xpfz jdk-7u76-linux-x64.tar.gz
mv jdk1.7.0_76/ /opt/
```
Java'nın bulduğu dizinde aşağıdaki komutları çalıştıralım.
```
cd /opt/jdk1.7.0_76/

alternatives --install /usr/bin/java java /opt/jdk1.7.0_76/bin/java 2
alternatives --config java  // Sunucunuzda tek bir java kurulu iste 1 seçip enter'a tıklayınız. Birden fazla java sürümü varsa bize uygun olanı seçiniz.
alternatives --install /usr/bin/jar jar /opt/jdk1.7.0_76/bin/jar 2
alternatives --install /usr/bin/javac javac /opt/jdk1.7.0_76/bin/javac 2
alternatives --set jar /opt/jdk1.7.0_76/bin/jar
alternatives --set javac /opt/jdk1.7.0_76/bin/javac
```
Sunucu yeniden başladığında Java'nın çevresel değişkenlerini belirlememiz gerekiyor.

```
nano /root/.bashrc
export JAVA_HOME=/opt/jdk1.7.0_76
export JRE_HOME=/opt/jdk1.7.0_76/jre
export PATH=$PATH:/opt/jdk1.7.0_76/bin:/opt/jdk1.7.0_76/jre/bin
```
Aşağıdaki komutla bashrc dosyasını güncelleyiniz.
```
source /root/.bashrc
```

Java'nın yüklendiğini ve versiyonu kontrol etmek için aşağıdaki komutu kullanabilirsiniz.
```
java -version
```
Ekran Görüntüsü
```
java version "1.7.0_76"
Java(TM) SE Runtime Environment (build 1.7.0_76-b13)
Java HotSpot(TM) 64-Bit Server VM (build 24.76-b04, mixed mode)
```
