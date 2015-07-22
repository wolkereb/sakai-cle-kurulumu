##Subversion Kurulumu (CentOS, RHEL, yada Fedora)

Subversion'u Sakai'nin kurulum dosyalarını indirmek için kullanacaktır.
Şimdi subversion'u kurmak için aşağıdaki ifadeyi komut satırına yapıştırınız.

```
yum install -y subversion 
```

Subversion'ın sürüm kontrolünü aşağıdaki komut ile yapabilirsiniz.
```
svn --version
```
Subversiion ekran çıktısı aşağıdaki gibi olmalıdır.
```
svn, version 1.7.14 (r1542130)
   compiled Feb 10 2015, 23:03:21

Copyright (C) 2013 The Apache Software Foundation.
This software consists of contributions made by many people; see the NOTICE
file for more information.
Subversion is open source software, see http://subversion.apache.org/

The following repository access (RA) modules are available:

* ra_neon : Module for accessing a repository via WebDAV protocol using Neon.
  - handles 'http' scheme
  - handles 'https' scheme
* ra_svn : Module for accessing a repository using the svn network protocol.
  - with Cyrus SASL authentication
  - handles 'svn' scheme
* ra_local : Module for accessing a repository on local disk.
  - handles 'file' scheme
```
