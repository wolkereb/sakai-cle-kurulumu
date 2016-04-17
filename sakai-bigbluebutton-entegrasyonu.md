## Sakai Bigbluebutton Entegrasyonu

cd /root/packages
wget https://github.com/blindsidenetworks/bigbluebutton-sakai/archive/1.0.8-SNAPSHOT.zip
unzip 1.0.8-SNAPSHOT.zip
cd bigbluebutton-sakai-1.0.8-SNAPSHOT/
mv bbb-tool/ /opt/tomcat/sakai/
cd /opt/tomcat/sakai/bbb-tool/

mvn clean install sakai:deploy -Dmaven.tomcat.home=/opt/tomcat -Dsakai.home=/opt/tomcat/sakai -Djava.net.preferIPv4Stack=true -Dmaven.test.skip=true

[INFO] Reactor Summary:
[INFO]
[INFO] BigBlueButton ...................................... SUCCESS [  0.265 s]
[INFO] BigBlueButton API .................................. SUCCESS [ 13.604 s]
[INFO] BigBlueButton Bundles .............................. SUCCESS [  0.298 s]
[INFO] BigBlueButton Implementation ....................... FAILURE [  6.653 s]
[INFO] BigBlueButton Component Pack ....................... SKIPPED
[INFO] BigBlueButton Tool ................................. SKIPPED
[INFO] BigBlueButton Assembly ............................. SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 35.767 s
[INFO] Finished at: 2016-02-25T10:42:46+02:00
[INFO] Final Memory: 20M/491M
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal on project bbb-impl: Could not resolve dependencies for project org.sakaiproject.bbb:bbb-impl:jar:1.0.8: Failed to collect dependencies at net.fortuna.ical4j:ical4j:jar:1.0.2: Failed to read artifact descriptor for net.fortuna.ical4j:ical4j:jar:1.0.2: Could not transfer artifact net.fortuna.ical4j:ical4j:pom:1.0.2 from/to modularity-releases (http://m2.modularity.net.au/releases): m2.modularity.net.au: Unknown host m2.modularity.net.au -> [Help 1]


ÜStteki hatayı alıyorsanız aşağıdaki iki dosyayı düzenlemeniz gerekecektir.


bbb-tool/pom.xml
bbb-tool/impl/pom.xml

        <dependency>
            <groupId>org.mnode.ical4j</groupId>
            <artifactId>ical4j</artifactId>
        </dependency>

Bu dosyaları değiştirdikten sonra tekrar derleyebilirsiniz. sakai.properties dosyasındaki auto.ddl true yapmayı unutmayın eğer mysql veya oracle veri tabanı kullanıyorsanız aksi takdirde bbb için gerekli tablolar oluşmayacaktır.

mvn clean install sakai:deploy -Dmaven.tomcat.home=/opt/tomcat -Dsakai.home=/opt/tomcat/sakai -Djava.net.preferIPv4Stack=true -Dmaven.test.skip=true


Son olarak sakai'nin bbb sunucunuz ile iletişim kurabilmesi için aşağıdaki parametreleri sakai.properties eklemeniz gerekecektir.

Bu parametreleri bbb yi kurduktan sonra bbb-conf --salt komutunu kullanarak alabilirsiniz.

Aşağıdaki parametreler bbb nin test sunucularına aittir ve kullanabilirsiniz.

bbb.url=http://test-install.blindsidenetworks.com/bigbluebutton
bbb.salt=8cd8ef52e8e101574e400365b55e11a6
