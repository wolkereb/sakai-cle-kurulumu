## Java Kurulumu (CentOS, RHEL, yada Fedora)
Java kurulumunu kaynak koddan yağacağımız için aşağıdaki adımları sırasıyla takip ediniz. Kaynak koddan kurulumu tercih etmemin sebebi kurulum dosyalarına hakim olmaktır.

Sakai 10.x Java 1.7 ile uyumlu çalışmaktadır. Bu yüzden Oracle JDK 1.7.0_79 sürümünü tercih ediyoruz. 1.7 sürümleri arasında uyumsuzluklar olabilmektedir bu yüzden Sakai CLE ana sitesindenki kurulum dökümanlarını incelemenizi tavsiye ederim. İlgili dökümana ulaşmak için [tıklayınız.](https://confluence.sakaiproject.org/display/DOC/Sakai+10+Install+Guides)
Java'nın diğer versiyonları indirmek için [tıklayınız](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

SSH ile sunucunuza bağlandıktan sonra aşağıdaki adımları sırası ile gerçekleştiriniz.
```
mkdir -p /root/packages  
cd /root/packages
```
Java dosyalarını indirin.

wget komutunu --no-cookies --no-check-certificate komutları ile kullanırsanız cookieleri ve sertifikayı devre dışı bırakmış oluyorsunuz böylece java dosyasını sunucuza direk indirebilirsiniz.
```
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/7u80-b15/jdk-7u80-linux-x64.tar.gz"
```
İndirdiğimiz java doslarını tar komutu ile açıp opt dizini altına taşıyalım.
```
tar xpfz jdk-7u80-linux-x64.tar.gz
mv jdk1.7.0_80/ /opt/
```
Java'nın bulduğu dizinde aşağıdaki komutları çalıştıralım.
```
cd /opt/jdk1.7.0_80
alternatives --install /usr/bin/java java /opt/jdk1.7.0_80/bin/java 2
alternatives --config java
```
Sunucunuza daha önce java kurmadıysanız karşınıza çıkan ekrandan 1'i seçiniz. Sunucunuzda birden fazla java sürümü varsa size uygun olan java sürümünü tercih ediniz.
```
alternatives --install /usr/bin/jar jar /opt/jdk1.7.0_80/bin/jar 2
alternatives --install /usr/bin/javac javac /opt/jdk1.7.0_80/bin/javac 2
alternatives --set jar /opt/jdk1.7.0_80/bin/jar
alternatives --set javac /opt/jdk1.7.0_80/bin/javac
```
Java kaynak koddan kurduğumuz için sunucu yeniden başladığında Java'nın çevresel değişkenlerini belirlememiz gerekiyor.
```
nano /root/.bashrc
```
Nano ile .bashrc dosyasını açtıktan sonra aşağıdaki komutları ekleyiniz.
```
export JAVA_HOME=/opt/jdk1.7.0_80
export JRE_HOME=/opt/jdk1.7.0_80/jre
export PATH=$PATH:/opt/jdk1.7.0_80/bin:/opt/jdk1.7.0_80/jre/bin
```
Aşağıdaki komutla .bashrc dosyasını güncelleyin.
```
source /root/.bashrc
```
Java'nın yüklendiğini ve javanı versiyonu kontrol etmek için aşağıdaki komutu kullanabilirsiniz.
```
java -version
```
Alacağınız ekran çıktısı aşağıdaki gibi olabilir.
```
java version "1.7.0_80"
Java(TM) SE Runtime Environment (build 1.7.0_80-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.80-b11, mixed mode)
```

Sunucumuzda java kurulmuş durumdadır. Bir sonraki aşama olan [Maven Kurulumu] (sakai-maven-kurulumu.md) kurulumuna geçebilirsiniz.
