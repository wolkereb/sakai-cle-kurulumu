# Sakai Kurulumu?

####Kurulum CentOS 7.x 64bit işletim sistemi üzerinde gerçekleştirilmiştir. RHEL tabanlı işletim sistemlerinde aynı kurulumu kullanabilirsiniz. (Fedora, Redhat)

#####Sakai Hakkında:

Sakai İşbirliği ve Öğrenme Ortamı, ders yönetim sistemlerinin sahip olduğu birçok ortak özelliğin yanında bilgi\belge dağıtımı, ödev aktarma, çevrimiçi ölçme değerlendirme, not defteri ve canlı sohbet modüllerini içeren açık ve uzaktan eğitimi destekleyen birçok özelliği ile web tabanlı, platform bağımsız bir uygulamadır.

Öğrenme faaliyetlerini kolaylaştırmak ve daha sistematik, planlı bir şekilde gerçekleştirmeyi hedeflemektedir. Öğrenme materyali sunma, sunulan öğrenme materyalini paylaşma ve tartışma, dersleri yönetme, ödev alma, sınavlara girme, bu ödev ve sınavlara ilişkin geribildirim sağlama, öğrenme materyallerini düzenleme, öğrenci, öğretmen ve sistem kayıtlarını tutma, raporlar alma gibi işlevleri sağlamaktadır.

Türkiyede Sakai'yi kullanan üniversiteler;
* [Yaşar Üniversitesi] (http://e.yasar.edu.tr)
* [Dokuz Eylül Üniversitesi] (http://oys.deu.edu.tr/portal)
* [Gediz Üniversitesi] (http://oys.gediz.edu.tr/portal)
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
CentOS Linux release 7.1.1503 (Core)
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

* [Java Kurulumu] (java-kurulumu.md)
* [Maven Kurulumu] (maven-kurulumu.md)
* [Tomcat Kurulumu] (tomcat-kurulumu.md)
* [Subversion Kurulumu] (subversion-kurlumu.md)

Tomcat dizinine geçiş yapıp sakai kurulum dosyalarını subversion ile sunucumuza indiriyoruz. İndirme işlemi bağlantı hızına göre uzun sürebilirmektedir.
```
cd /opt/tomcat
svn co https://source.sakaiproject.org/svn/sakai/tags/sakai-10.5/ sakai
```
Ekran çıktısı aşağıdaki gibi olmalıdır.

```
A    sakai/web/web-impl/pack/src/webapp/WEB-INF
A    sakai/web/web-impl/pack/src/webapp/WEB-INF/components.xml
A    sakai/web/web-impl/pack/pom.xml
 U   sakai/web
Checked out external at revision 320234.

Checked out revision 320234.
```
Sakai'yi derlemeden önce master dizinine geçiş yapıp master'ı derliyoruz. 
```
cd sakai/master
mvn clean install
```
Ekran çıktısı aşağıdaki gibi olmalıdır.
```
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 28.722 s
[INFO] Finished at: 2015-07-22T14:26:09+03:00
[INFO] Final Memory: 11M/491M
[INFO] ------------------------------------------------------------------------
```
Şimdi ise Sakai ana dizine geçiş yaptıktan sonra tüm projeyi derliyoruz.
```
cd ..
mvn clean install sakai:deploy -Dmaven.tomcat.home=/opt/tomcat -Dsakai.home=/opt/tomcat/sakai -Djava.net.preferIPv4Stack=true
```
Ekran çıktısı aşağıdaki gibi olmalıdır.
```
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 01:01 h
[INFO] Finished at: 2015-07-22T15:29:02+03:00
[INFO] Final Memory: 436M/899M
[INFO] ------------------------------------------------------------------------
```
Kurulum tamamlandı şimdi Sakai'yi başladabiliriz.
```
/opt/tomcat/bin/startup.sh 
```
Komut satırına üstteki ifadeyi yazdığınızda aşağaki gibi çıktı almanız gerekmektedir.
```
Using CATALINA_BASE:   /opt/tomcat
Using CATALINA_HOME:   /opt/tomcat
Using CATALINA_TMPDIR: /opt/tomcat/temp
Using JRE_HOME:        /opt/jdk1.7.0_76/jre
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
