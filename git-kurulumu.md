## Centos 7 Üzerine Git Kurulumu
Git kurulumu için lütfen aşağıdaki adımları takip ediniz.
```

yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
yum install gcc perl-ExtUtils-MakeMaker
cd /root/packages/

wget https://www.kernel.org/pub/software/scm/git/git-2.9.2.tar.gztar
tar xzf git-2.9.2.tar.gz
cd git-2.9.2
make prefix=/usr/local/git all
make prefix=/usr/local/git install
nano /root/.bashrc
export PATH=$PATH:/usr/local/git/bin
source /root/.bashrc

git --version

git version 2.9.2
```
