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

Kulurum için öncelikle işletim sistemini ve sürümünü kontrol ediyoruz. RHEL tabanlı olup olmadığına bakacağız.
```
cat /etc/redhat-release
```
Aşağıdaki gibi bir çıktı alıyorsanız kuruluma devam edebilirsiniz.
```
CentOS Linux release 7.1.1503 (Core)
```
Daha sonra işletim sisteminin kaç bit olduğunu kontrol ediyoruz. 64 bit işletim sistemi olması gerekmektedir.

```
uname -m
```
Aşağıdaki gibi bir çıktı almanız gerekiyor.

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
Sakai ana dizine geçiş yaptıktan sonra tüm projeyi derliyoruz.

```
cd ..
mvn clean install sakai:deploy -Dmaven.tomcat.home=/opt/tomcat -Dsakai.home=/opt/tomcat/sakai -Djava.net.preferIPv4Stack=true
```
