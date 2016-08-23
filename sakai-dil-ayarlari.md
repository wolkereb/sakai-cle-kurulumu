## Sakai Dil Ayarları

Centos 7 üzerine pip kurulumu için aşağıdaki komutları kullanabilirsiniz.
```
yum -y install python-pip
pip -V
```
Version sorguladığınızda aşağıdaki gibi ifade ile karşılaşıyorsanız pip paket yöneticisini kurmuşsunuz demektedir.
```
pip 7.1.0 from /usr/lib/python2.7/site-packages (python 2.7)
```
Trasnsifex'ten sakai dil dosyalarını indirmek için transifex-client kurmamız gerekmektedir.
Kurulum için aşağıdaki komutu kullanabilirsiniz.
```
sudo pip install transifex-client
```
Dil dosyaları indirmek için aşağıdaki python scriptini indiriyoruz.
```
cd /opt/tomcat/sakai
git clone https://github.com/JaSakai/l10n
cd l10n
```

Dana sonra tmx.py dosyası içinden aşağıda bulunan ifadeyi sakai versiyonu göre değiştiriniz.

```
project_name = os.getenv('TRANSIFEX_SAKAI_PROJECTNAME','sakai-trunk')
```
sonrası
```
project_name = os.getenv('TRANSIFEX_SAKAI_PROJECTNAME','sakai11x')
```

Transifex kullanıcı adı ve şifrenizi tanımlıyoruz.
```
nano ~/.transifexrc

[https://www.transifex.com]
hostname = https://www.transifex.com
username = user
password = pass
token =
```
l10n klasöründeyken aşağıkidaki komutları sırasıyla çalıştırın.
```
python tmx.py init
python tmx.py download -u -l tr_TR
```

Sakai dil dosyaları indirildi şimdi sıra sakai yi derlemede.
