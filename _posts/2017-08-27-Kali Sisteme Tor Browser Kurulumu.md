---
title: Kali Sisteme Tor Browser Kurulumu
category: blog
last_modified_at: 2017-08-27
---
Merhaba arkadaşlar bu yazımızda Kali işletim sistemimize Tor Browser kurulumu yapacağız. Aslında bakarsanız diğer Linux işletim sistemleri içinde yapılması gerekenler tamamen aynı. Sadece Kali'de diğer birçok dağıtımın aksine root olarak işlem yaptığımızdan bir kaç ek işlem uygulamamız gerekiyor. Malum yasaklardan dolayı kullanımı bir hayli zor hale getirilen bu browsera bazı alanlarda ihtiyaç duyabiliyoruz. Tor Browser'ın resmi sitesine erişim ülkemizde yasaklanmış durumda. Bu nedenle paket dosyasını yine bir başka resmi kaynak olan github sayfasından indiriyoruz. Tor Browser'in resmi github sayfası aşağıdaki gibidir. Paket dosyasını buradan edinmeniz güvenliğiniz için oldukça önemlidir.

    https://github.com/TheTorProject/gettorbrowser

&nbsp;

Yukarıdaki bağlantıdan sistemimiz için uygun olan paketi bilgisayarımıza indiriyoruz. İndirme işlemi tamamlandıktan sonra sıkıştırılmış dosyayı açıyoruz. Çıkan dosyalarda Tor Browser Setup simgesine tıkladığımız zaman eğer "<em><strong>The Tor Browser Bundle should not be run as root. Exiting.</strong></em>" hatası alıyorsanız (bir çok linux dağıtımında bu hatayla karşılaşmazsınız. Eğer hata almıyorsanız bu adımları geçebilirsiniz. Bu hatayı sadece Kali gibi root kullanıcı haklarıyla işlem yapanlar almaktadır.) Browser dizinin içindeki "<em><strong>start-tor-browser</strong></em>" dosyasını bir editör yardımıyla düzenlememiz gerekiyor.

    leafpad Tor/Browser/start-tor-browser

&nbsp;

komutuyla dosyayı açıyorum ve 94. satırda bulunan

    if [ "`id -u`" -eq 0 ]; then
     complain "The Tor Browser Bundle should not be run as root. Exiting."
     exit 1

&nbsp;

"-eq 0" değerini "-eq 1" olarak değiştirip, kaydediyoruz. Bu değişiklikler sayesinde şimdi Tor Browser'i root olarakta kullanabileceğiz. Fakat bu pek güvenli bir yol değidir. Bu programı root olarak kullanmanızı engellemelerinin bir sebebi var. Bu hatayı çözmenin bir başka yolu daha var. Bu daha güvenli bir yol Tor Browseri kullanmak için standart kullanıcı tanımlamak. Bunun için sırasıyla aşağıdaki komutları yürütelim.

    useradd toruser

&nbsp;

    chown -R toruser tor-browser_en-US # Replace with your TBB folder
&nbsp;

    gksu -u toruser tor-browser_en-US/Browser/start-tor-browser

&nbsp;

Şimdi verilen hataları çözdüğümüze göre köprü(bridge) ayarlarını yapılandırabiliriz. Tor Browser Setup uygulamasını çalıştırıyoruz. İlk olarak Configure'ye tıklayalım

![tor1](https://www.hubeybi.com/wp-content/uploads/2017/08/tor1.png)

Bu bölümde bize internet sağlayıcımızın Tor ağına sansür uygulayıp uygulamadığını soruyor. "Evet" seçeneğini seçip devam ediyoruz.

![tor2](https://www.hubeybi.com/wp-content/uploads/2017/08/tor2.png)

Bu bölümde köprü yapılandırma ayarlamasını önerilen şekilde işaretleyip devam ediyoruz.

![tor3](https://www.hubeybi.com/wp-content/uploads/2017/08/tor3.png)

Bu ekranda ise bize yerel bir erişim yasağının olup olmadığı soruluyor. Eğer işyeri veya okul gibi kısıtlı erişime sahipseniz evet'i; herhangi bir yerel kısıtlama söz konusu değil ise hayır'ı seçin.

![tor4](https://www.hubeybi.com/wp-content/uploads/2017/08/tor4.png)

Hayır'ı işaretleyip bağlantının kurulmasını bekliyorum.

![tor5](https://www.hubeybi.com/wp-content/uploads/2017/08/tor5.png)

Bağlantı kurulduktan sonra artık Tor Browser kullanıma hazır.

![tor6](https://www.hubeybi.com/wp-content/uploads/2017/08/tor6.png)