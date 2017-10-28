---
title: Lamp Kurulumu ve Karşılaşılan Sorunlar
category: blog
last_modified_at: 2017-06-24
---
Linux dağıtımlarında açık kaynak kodlu olarak kullanılan bir web geliştirme platformu olarak tanımlayabiliriz. LAMP; Apache web sunucusu, MySQL veritabanı ve PHP programlama dili kullanmaktadır. LAMP'ın ne olduğundan kısaca bahsettiğimize göre şimdi kurulumuna geçebiliriz.

Aşağıdaki komutu çalıştırıp, şifremizi girerek bash ekranında yönetici oluyoruz.

    sudo su

&nbsp;

Ardından aşağıdaki komutu yürütüp MySQL Server kurulumunu tamamlıyoruz.

    apt-get install mysql-server mysql-client

![dvwa1](https://www.hubeybi.com/wp-content/uploads/2017/06/dvwa1.png)

ve gelen uyarıya evet diyoruz.

Paketler indikten sonra bizden MySQL root şifresi koymamızı isteyecek. Şifremizi girip "Tamam" dedikten sonra şifreyi tekrar girmemizi isteyecek, aynı şifreyi girip devam ediyoruz.

![dvwa2](https://www.hubeybi.com/wp-content/uploads/2017/06/dvwa2.png)

Not: Bu şifreyi unutmuyoruz ileride lazım olacak.

Şimdi de aşağıdaki komutu çalıştırarak Apache Server kurulumumuzu tamamlayalım.

    apt-get install apache2

&nbsp;

Apache Server kurulumunu da tamamladık, PHP7 kurulumunuda yapalım hemen

    apt-get install php libapache2-mod-php php-mcrypt php-mysql

&nbsp;

şimdi aşağıdaki komutla serverimizi yeniden başlatalım.

    service apache2 restart

&nbsp;

Serverimizi yeniden başlattığımıza göre kontrol edelim bakalım başarılı bir şekilde yüklenmiş mi?

    vim /var/www/html/info.php

Bu komutla dizinime info.php dosyası oluşturdum ve içine aşağıdaki kodları yazıyorum ve kaydediyorum.

&nbsp;

![dvwa3](https://www.hubeybi.com/wp-content/uploads/2017/06/dvwa3.png)

&nbsp;

    <?php
    
    phpinfo();
    
    ?>


&nbsp;

bu işlemden sonra localhost/info.php adresine tıkladığımda böyle bir ekranla karşılaşmış olmam gerekiyor.

&nbsp;

![dvwa6](https://www.hubeybi.com/wp-content/uploads/2017/06/dvwa6.png)

&nbsp;

PhpMyAdmin paketimizi yükleyebiliriz, aşağıdaki komutu çalıştıralım

    apt-get install phpmyadmin

&nbsp;

paket indirildikten sonra çıkan ekranda apache2 seçeneğini işaretleyip devam ediyoruz.

![dvwa4](https://www.hubeybi.com/wp-content/uploads/2017/06/dvwa4.png)

ardından gelen ekranda hayırı işaretleyip devam ediyoruz.

![dvwa5](https://www.hubeybi.com/wp-content/uploads/2017/06/dvwa5.png)

Şimdi burası önemli localhost/phpmyadmin sayfasına gittiğinizde muhtemelen çoğunuz 404 Page Not Found hatası alacaksınız. Bunu şu şekilde düzelteceğiz. Ben editör olarak vim'i kullanıyorum siz farklı editörle açabilirsiniz.

    gksu vim /etc/apache2/apache2.conf

&nbsp;

komutuyla dosyayı açıyorum ve en alt satırına aşağıdaki kodu ekliyorum.

    Include /etc/phpmyadmin/apache.conf

&nbsp;

kaydedip çıkıyorum ve ardından apache'yi tekrar başlatıyorum.

    service apache2 restart

&nbsp;

tekrar localhost/phpmyadmin sayfasına gidiyorum ve işte oldu.

![dvwa7](https://www.hubeybi.com/wp-content/uploads/2017/06/dvwa7.png)

Gelen ekranda kullanıcı adı bölümüne root, şifre bölümüne de mysql'ü kurarken oluşturduğumuz, önemli diye not düştüğüm şifreyi girerek giriş yapabilirsiniz.

Dosyalarınızı /var/www/html/ dizinine yükleyip çalıştırabilirsiniz.

Sağlıcakla kalın.
