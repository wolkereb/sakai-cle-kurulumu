## Sakai Dil Ayarları

yum install python-pip
sudo pip install transifex-client

cd /root/packages
wget https://github.com/eemirtekin/sakai-tr/raw/master/l10n.zip
unzip l10n.zip
mv l10n /opt/tomcat/sakai/

nano ~/.transifexrc

[https://www.transifex.com]
hostname = https://www.transifex.com
password = password
token =
username = username

cd /opt/tomcat/sakai/l10n
python tmx.py init
python tmx.py download -u -l tr_TR

Bu aşamadan sonra sakai'yi tekrar derlemeniz gerekecek. Derlemeden önce yapılacak bir işlem aslında :)
