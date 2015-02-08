# Sakai Kurulumu
Sakai CLE kurulumunu gerçekleştirmek için oluşturulmuştur. İşletim sistemi olarak CentOS 7 64bit tercih edilmiştir. Dilerseniz bu kurulumu RedHat ve Fedaora içinde de gerçekletirebilirsiniz.

## Firewall için gerekli ayarlar

Üniversitenin firewall'u olduğu için sunucu tarafında firewall yapılandırılmayacaktır. CentOS 7 ile beraber gelen firewall hizmetini ise aşağıdaki komut satırları ile durdurup tekrar başlamaması için ayarlayabilirsiniz.

``` javasccript
iptables -F
service iptables stop
chkconfig iptables off ```
