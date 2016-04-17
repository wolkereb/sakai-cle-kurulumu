#Sakai Ldap Ayarları

Sakai'yi üniversitede kullanacaksanız mutlaka Ldap, Cas veya Kerberos gibi Sakai'nin desteklediği bir yapıyı kullanmalısınız. Binlerce öğrenci olduğundan hesapları manuel açamanız yorucu ve mantıklı olmayacaktır.
Sakai için ldap yapılandırması nasıl yapılır?

Öncelikle sakai dizinine gidiyoruz. Bu dizinde düzenlememiz gereken bazı dosyalar. Aşağıdaki işlemleri sırası ile uygulayın.

```
cd /opt/tomcat/sakai/
nano providers/component/pom.xml
```
Bu dosyadaki Jldap bölümünü aktif ediyoruz.
```
<!-- Needed for the JLDAP Provider
                <dependency>
                        <groupId>org.sakaiproject</groupId>
                        <artifactId>sakai-jldap-provider</artifactId>
                </dependency>
            -->
```
Aşağıdaki gibi olması gerekiyor.
```
<!-- Needed for the JLDAP Provider  -->
                <dependency>
                          <groupId>org.sakaiproject</groupId>
                          <artifactId>sakai-jldap-provider</artifactId>
                </dependency>
```

Şimdi Jldap bileşenini aktif edelim.
```           
nano providers/component/src/webapp/WEB-INF/components.xml
```

```
<!-- Uncomment and configure to use the JLDAPDirectoryProvider -->
        <!--  import resource="jldap-beans.xml" / -->
```
Aşağıdaki gibi olması gerekiyor.

```
<!-- Uncomment and configure to use the JLDAPDirectoryProvider -->
        <import resource="jldap-beans.xml" />

```
Buraya kadar jldap bileşenini aktif ettiş olduk. Şimdi ldap bilgilerini gireceğimiz bölüme geldik.

```
nano providers/component/src/webapp/WEB-INF/jldap-beans.xml
```
Ldap sunucusunun hostname i yada ip adresini girebilirsiniz.
```
<property name="ldapHost">
    <value>ldap.yasar.edu.trm</value>
</property>
```
Ldap kullanıcılarını sorgulayacağınız bilgileri girmelisiniz. Bu bilgileri bilgi işlemden alabilirsiniz.
```
<property name="basePath">
    <value>OU=Genel,dc=emirtekin,dc=local</value>
</property>
```
Ldap kullanıcı adınızı giriniz.
```
<property name="ldapUser">
    <value>eemirtekin</value>
</property>
```
Ldap şifrenizi giriniz.
```
<property name="ldapPassword">
    <value>şifre</value>
</property>
```
```
<property name="autoBind">
       <value>true</value>
</property>
```
Alt Organization Unit (OU) larda arama yapmak istiyorsanız. searchScope u 2 yapmanız gerekmektedir.
```
<property name="searchScope">
  <value>2</value>
</property>
```
Buradaki eşleştirme kısmınıda bilgi işleme sormalısınız. Onlardan gelecek bilgiye göre aşağıdaki alanları düzenleyin.
```
<property name="attributeMappings">
    <map>
    <entry key="login"><value>sAMAccountName</value></entry>         
    <entry key="firstName"><value>name</value></entry>
    <entry key="preferredFirstName"><value>displayName</value></entry>
    <entry key="lastName"><value>lastName</value></entry>
    <entry key="email"><value>mail</value></entry>
        <!--
        <entry key="groupMembership"><value>groupMembership</value></entry>
        -->
    </map>
</property>
```

Tüm ayarları tamamladık provider bileşenini tekrar derlemeliyiz.
```
cd /opt/tomcat/sakai/providers
mvn clean install sakai:deploy -Dmaven.tomcat.home=/opt/tomcat -Dsakai.home=/opt/tomcat/sakai -Djava.net.preferIPv4Stack=true -Dmaven.test.skip=true

```

Aşağıdaki gibi bir çıktı alırsanız derlemenin başarılı olduğunu düşünebilirsiniz. Ldap ayarlarınız doğru ise artık öğrenci veya eğitmenleri ldap kullanarak sisteme ekleyebilirsiniz.

```
[INFO] Sakai Providers Project ............................ SUCCESS [  2.593 s]
[INFO] sakai-allhands-provider ............................ SUCCESS [  6.104 s]
[INFO] sakai-coursemanagement-authz-provider-impl ......... SUCCESS [  1.141 s]
[INFO] sakai-coursemanagement-authz-provider-impl ......... SUCCESS [  1.171 s]
[INFO] sakai-sample-provider .............................. SUCCESS [  0.377 s]
[INFO] sakai-jldap-provider-mock .......................... SUCCESS [  0.793 s]
[INFO] sakai-jldap-provider ............................... SUCCESS [01:49 min]
[INFO] sakai-provider-pack ................................ SUCCESS [  0.310 s]
[INFO] sakai-federating-provider .......................... SUCCESS [  0.183 s]
[INFO] sakai-imsent-provider .............................. SUCCESS [  0.121 s]
[INFO] sakai-kerberos-provider ............................ SUCCESS [  0.660 s]
[INFO] sakai-openldap-provider ............................ SUCCESS [  0.251 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:07 min
[INFO] Finished at: 2015-09-11T09:50:41+03:00
[INFO] Final Memory: 28M/491M
```
