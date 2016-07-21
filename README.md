# Sakai 11.x CLE Kurulumu ve Yapılandırması
12.x için de aynı kurulum adımları geçerlidir. Git master sürümünde çalışmanız yeterlidir.

![alt text](https://raw.githubusercontent.com/eemirtekin/sakai-tr/master/sakai-ekran-goruntusu.png "Sakai 11.x Ekran görüntüsü")
####Kurulum CentOS 7.x 64bit işletim sistemi üzerinde gerçekleştirilmiştir. RHEL tabanlı işletim sistemlerinde aynı kurulumu kullanabilirsiniz. (Fedora, Redhat)

#####Sakai Hakkında:

Sakai İşbirliği ve Öğrenme Ortamı, ders yönetim sistemlerinin sahip olduğu birçok ortak özelliğin yanında bilgi\belge dağıtımı, ödev aktarma, çevrimiçi ölçme değerlendirme, not defteri ve canlı sohbet modüllerini içeren açık ve uzaktan eğitimi destekleyen birçok özelliği ile web tabanlı, platform bağımsız bir uygulamadır.

Öğrenme faaliyetlerini kolaylaştırmak ve daha sistematik, planlı bir şekilde gerçekleştirmeyi hedeflemektedir. Öğrenme materyali sunma, sunulan öğrenme materyalini paylaşma ve tartışma, dersleri yönetme, ödev alma, sınavlara girme, bu ödev ve sınavlara ilişkin geribildirim sağlama, öğrenme materyallerini düzenleme, öğrenci, öğretmen ve sistem kayıtlarını tutma, raporlar alma gibi işlevleri sağlamaktadır.

Türkiyede Sakai'yi kullanan üniversiteler;
* [Yaşar Üniversitesi] (http://e.yasar.edu.tr)
* [Dokuz Eylül Üniversitesi] (http://oys.deu.edu.tr/portal)
* [İstanbul Kültür Üniversitesi] (http://cats.iku.edu.tr/portal)
* [Sabacı Üniversitesi] (https://sucourse.sabanciuniv.edu/portal)
* [Anadolu Üniversitesi] (http://sakai.anadolu.edu.tr)

###Kurulumu Aşamaları

Kuruluma başlamadan önce CentOs, Fedora veya RHEL tabanlı bir işletim sistemi kurduğunuzu var sayıyoruz. Sakai Java ile geliştirildiği için platform bağımsız çalışmaktadır. Hakim olduğunuz bir sunucu tipini kurup kuruluma başlayabilirsiniz. Bizim CentOs'useçme nedenimiz daha kararlı yapıda olmasıdır.

Terminal ile sunucuya bağlandıktan sonra aşağıda sunucunun işletim sistemini ve sunucuda 32bit mi 64bit işletim sistemi kurulmuş bunu  kontrol edeceğiniz. Ram kullanımını doğru ayarlayabilmek için sunucunun 32bit veya 64bit olduğunu bilmemiz şart.

```
cat /etc/redhat-release
```
Kurduğumuz sunucu CentOS 7.x işletim sistemidir.
```
CentOS Linux release 7.2.1511 (Core)
```
```
uname -m
```
64bit işletim sistemi olduğunu görüyoruz.
```
x86_64
```
Şimdi kurulum esnasında gerekli olan paketleri kurmalıyız.
```
yum install nano unzip wget -y
```
Sırası ile Java, Maven, Tomcat ve Subversion'ı sunucumuza kuruyoruz.

* [Java Kurulumu] (sakai-java-kurulumu.md)
* [Maven Kurulumu] (sakai-maven-kurulumu.md)
* [Tomcat Kurulumu] (sakai-tomcat-kurulumu.md)

Tomcat dizinine geçiş yapıp sakai kurulum dosyalarını subversion ile sunucumuza indiriyoruz. İndirme işlemi bağlantı hızına göre uzun sürebilirmektedir.
```
cd /opt/tomcat
git clone https://github.com/sakaiproject/sakai.git
cd sakai
git tag -l
git checkout 11.0-rc02
```
Ekran çıktısı aşağıdaki gibi olmalıdır.

```
[root@sakai11 tomcat]# git clone https://github.com/sakaiproject/sakai.git
Cloning into 'sakai'...
remote: Counting objects: 718240, done.
remote: Compressing objects: 100% (93/93), done.
remote: Total 718240 (delta 21), reused 0 (delta 0), pack-reused 718138
Receiving objects: 100% (718240/718240), 282.08 MiB | 26.85 MiB/s, done.
Resolving deltas: 100% (313459/313459), done.
Checking connectivity... done.
Checking out files: 100% (22198/22198), done.
```
Sakai'yi derlemeden önce master dizinine geçiş yapıp master'ı derliyoruz.
```
cd /opt/tomcat/sakai/master
mvn clean install
```
Ekran çıktısı aşağıdaki gibi olmalıdır.
```
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 7.481 s
[INFO] Finished at: 2016-07-21T07:26:53+00:00
[INFO] Final Memory: 14M/607M
[INFO] ------------------------------------------------------------------------
```
Şimdi ise Sakai ana dizine geçiş yaptıktan sonra tüm projeyi derliyoruz.
```
cd ..
mvn clean install sakai:deploy -Dmaven.tomcat.home=/opt/tomcat -Dsakai.home=/opt/tomcat/sakai -Djava.net.preferIPv4Stack=true -Dmaven.test.skip=true
```
Ekran çıktısı aşağıdaki gibi olmalıdır.
```
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 06:00 min
[INFO] Finished at: 2016-07-21T07:34:44+00:00
[INFO] Final Memory: 615M/968M
[INFO] ------------------------------------------------------------------------
```

## Sakai MySql Ayarları
* Sakai'de MySql kurulumu ve ayarları için [tıklanıyız] (sakai-mysql-kurulumu.md).

Kurulum tamamlandı şimdi Sakai'yi başladabiliriz.
```
/opt/tomcat/bin/startup.sh
```
Komut satırına üstteki ifadeyi yazdığınızda aşağaki gibi çıktı almanız gerekmektedir.
```
Using CATALINA_BASE:   /opt/tomcat
Using CATALINA_HOME:   /opt/tomcat
Using CATALINA_TMPDIR: /opt/tomcat/temp
Using JRE_HOME:        /opt/jdk1.8.0_91/jre
Using CLASSPATH:       /opt/tomcat/bin/bootstrap.jar:/opt/tomcat/bin/tomcat-juli.jar
Tomcat started.
```
Sakai'yi kurdunuz fakar sayfa gelmiyor ise muhtemelen firewall!a takılıyor olabilirsiniz.
CentOS 7.x için firewall'u aşağıdaki gibi kapatabilirsiniz.
```
systemctl stop firewalld.service
systemctl disable firewalld.service
```
Sakai'nin başlayıp, başlamadığını yada başlarken ne gibi hatalar oluştuğunu kontrol etmek için /opt/tomcat/logs/catalina.out dosyasını inceleyenilirsiniz.
```
tail -f /opt/tomcat/logs/catalina.out
```
Eğer aşağıdaki ifadeyi görüyorsunuz. Sakai'yi iyi kötü başlamış demekdir.
```
INFO: Deployment of web application archive /opt/tomcat/webapps/sakai-ws.war has finished in 2,578 ms
Jul 22, 2015 3:53:51 PM org.apache.coyote.AbstractProtocol start
INFO: Starting ProtocolHandler ["http-bio-8080"]
Jul 22, 2015 3:53:51 PM org.apache.coyote.AbstractProtocol start
INFO: Starting ProtocolHandler ["ajp-bio-8009"]
Jul 22, 2015 3:53:51 PM org.apache.catalina.startup.Catalina start
INFO: Server startup in 256159 ms
```
Herhangi bir internet tarayıcısını açıp sisteme gişi yapabilirsiniz. Tomcat varsayılan olarak 8080 portunu kullanmaktadır. Bu yüzden sakai'de 8080 portundan çalışmaktadır. Şimdi kurduğumuz sakai'ye bir göz atabilirsiniz.
```
http://ip-adres:8080/portal
```
Varsayılan yönetici için kullanıcı adı ve şifre aşağıdadır.
```
Kullanıcı Adı: admin
Şifre: admin
```

## Sakai Ldap Ayarları
* Sakai Ldap ayarları için [tıklanıyız] (sakai-ldap-ayarlari.md).
