# Tomcat Kurulumu
```
cd /root/packages/
```
```
wget http://ftp.mku.edu.tr/apache-dist/tomcat/tomcat-7/v7.0.63/bin/apache-tomcat-7.0.63.tar.gz
```
```
tar xpfz apache-tomcat-7.0.63.tar.gz
mv apache-tomcat-7.0.63 /opt/tomcat
```
```
nano /root/.bashrc
export CATALINA_HOME=/opt/tomcat
export PATH=$PATH:$CATALINA_HOME/bin
```
```
source /root/.bashrc
```

```
cd /opt/tomcat/bin
nano setenv.sh
```

```
export JAVA_OPTS='-server -Xms512m -Xmx1024m -XX:PermSize=128m -XX:MaxPermSize=512m -XX:NewSize=192m -XX:MaxNewSize=384m -Djava.awt.headless=true -Dhttp.agent=Sakai -Dorg.apache.jasper.compiler.Parser.STRICT_QUOTE_ESCAPING=false -Dsun.lang.ClassLoader.allowArraySyntax=true'
```

```
chmod a+x *.sh
cd ../conf
nano server.xml
```


```
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```
to

```
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" URIEncoding="UTF-8"/>
```

```
cd ..
rm -rf webapps/*
nano conf/catalina.properties
```
```
common.loader=${catalina.base}/lib,${catalina.base}/lib/*.jar,${catalina.home}/lib,${catalina.home}/lib/*.jar
```
to
```
common.loader=${catalina.base}/lib,${catalina.base}/lib/*.jar,${catalina.home}/lib,${catalina.home}/lib/*.jar,${catalina.base}/common/classes/,${catalina.base}/common/lib/*.jar
```
```
shared.loader=${catalina.base}/shared/classes/,${catalina.base}/shared/lib/*.jar
```

```
server.loader=${catalina.base}/server/classes/,${catalina.base}/server/lib/*.jar
```
```
mkdir -p shared/classes shared/lib common/classes common/lib server/classes server/lib
```

Maven kurulumu tamanlandı. Bir sonraki aşama olan [Subversion Kurulumu] (subversion-kurulumu.md) kurulumuna geçebilirsiniz.

