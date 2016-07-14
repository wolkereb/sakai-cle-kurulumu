## Sakai Dil Ayarları

Centos 7 üzerine pip kurulumu için aşağıdaki komutları kullanabilirsiniz.
```
yum -y install python-pip
pip -V
```
Version sorguladığınızda aşağıdaki gibi ifade ile karşılaşıyorsanız pip pakat yöneticisini kurmuşsunuz demektedir.
```
pip 7.1.0 from /usr/lib/python2.7/site-packages (python 2.7)
```
Trasnsifex'ten sakai dil dosyalarını indirmek için transifex-client kurmamız gerekmektedir.
Kurulum için aşağıdaki komutu kullanabilirsiniz.
```
sudo pip install transifex-client
```
l10n klasörünü sakai'yi derlemeden önce veya sonra sakai dosyalarının bulunduğu dizine atmanız gerekmektedir.

l10n klasöründeyken aşağıkidaki komutları çalıştırın.
Ama öncesinde sakainin hangi versiyonun dil dosyalarını indireceksiniz bunu tmx.py dosyasından belirlemeniz gerekemektedir. Ben sakai10x için dil dosyalarını indirmiş olacağım.
```
#TMX Project Name on Transifex
#project_name = 'sakai-trunk'
project_name = 'sakai10x'
```

python tmx.py init


nano ~/.transifexrc

[https://www.transifex.com]
hostname = https://www.transifex.com
username = user
password = pass
token =
