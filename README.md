# Sakai Kurulumu?

####Kurulum CentOS 7 64bit işletim sistemi üzerinde gerçekleştirilmiştir. RHEL tabanlı işletim sistemlerinde çalışacaktır.(Fedora, Redhat)

Sakai İşbirliği ve Öğrenme Ortamı, ders yönetim sistemlerinin sahip olduğu birçok ortak özelliğin yanında bilgi\belge dağıtımı, ödev aktarma, çevrimiçi ölçme değerlendirme, not defteri ve canlı sohbet modüllerini içeren açık ve uzaktan eğitimi destekleyen birçok özelliği ile web tabanlı, platform bağımsız bir uygulamadır.

Öğrenme faaliyetlerini kolaylaştırmak ve daha sistematik, planlı bir şekilde gerçekleştirmeyi hedeflemektedir. Öğrenme materyali sunma, sunulan öğrenme materyalini paylaşma ve tartışma, dersleri yönetme, ödev alma, sınavlara girme, bu ödev ve sınavlara ilişkin geribildirim sağlama, öğrenme materyallerini düzenleme, öğrenci, öğretmen ve sistem kayıtlarını tutma, raporlar alma gibi işlevleri sağlamaktadır.

Türkiyede Sakai'yi kullanan üniversiteler;

* [Yaşar Üniversitesi] (http://e.yasar.edu.tr)
* [Dokuz Eylül Üniversitesi] (http://oys.deu.edu.tr/portal)
* [Gediz Üniversitesi] (http://oys.gediz.edu.tr/portal)
* [İstanbul Kültür Üniversitesi] (http://cats.iku.edu.tr/portal)
* [Sabacı Üniversitesi] (https://sucourse.sabanciuniv.edu/portal)
* [Anadolu Üniversitesi] (http://sakai.anadolu.edu.tr)

#### Java Kurulumu
```
mkdir -p /root/packages  

cd /root/packages

wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/7u76-b13/jdk-7u76-linux-x64.tar.gz"wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/7u76-b13/jdk-7u76-linux-x64.tar.gz"

tar xpfz jdk-7u76-linux-x64.tar.gz
mv jdk1.7.0_76/ /opt/
cd /opt/jdk1.7.0_76/

alternatives --install /usr/bin/java java /opt/jdk1.7.0_76/bin/java 2
alternatives --config java
alternatives --install /usr/bin/jar jar /opt/jdk1.7.0_76/bin/jar 2
alternatives --install /usr/bin/javac javac /opt/jdk1.7.0_76/bin/javac 2
alternatives --set jar /opt/jdk1.7.0_76/bin/jar
alternatives --set javac /opt/jdk1.7.0_76/bin/javac


```
