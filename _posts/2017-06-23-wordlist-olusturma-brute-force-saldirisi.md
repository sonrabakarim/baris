---
title: Wordlist Oluşturma Ve Brute Force Saldırısı
category: blog
last_modified_at: 2017-06-22
---
Yeni bir yazıdan herkese merhaba arkadaşlar. Bu yazımızda neler öğreneceğiz isterseniz öncelikle bunlardan bahsedelim. Öncelikle Linux işletim sisteminde nasıl kolay bir şekilde wordlist hazırlayabileceğimizi, daha sonra bu oluşturduğumuz wordlist yardımıyla Brute Force adını verdiğimiz saldırılar düzenlemeyi göreceğiz. Bonus olarak Python kullanarak kodladığım bir scripti sizlerle paylaşacağım. Bu script sayesinde md5 şifrelerini nasıl kırabileceğimizi göreceğiz.
<h5>Wordlist Oluşturma</h5>
Öncelikle wordlistimizi oluşturmak için crunch adlı pakete ihtiyacımız var. Kali gibi penetrasyon testi dağıtımlarıyla beraber zaten geliyor. Öncelikle sistemimizde crunch yüklümü bunu öğrenelim. Bash ekranımızı açıp

    crunch

&nbsp;

yazıyoruz. Eğer aldığımız çıktı



    crunch version 3.6
    
    Crunch can create a wordlist based on criteria you specify. The outout from crunch can be sent to the screen, file, or to another program.
    
    Usage: crunch &amp;amp;amp;amp;lt;min&amp;amp;amp;amp;gt; &amp;amp;amp;amp;lt;max&amp;amp;amp;amp;gt; [options]
    where min and max are numbers
    
    Please refer to the man page for instructions and examples on how to use crunch.



&nbsp;

bunun gibi ise crunch yüklü demektir devam edebiliriz. Eğer paketin yüklü olmadığı bilgisi veriliyorsa

    wget --no-check-certificate https://downloads.sourceforge.net/project/crunch-wordlist/crunch-wordlist/crunch-3.6.tgz

&nbsp;

komutunu vererek crunch paketini indiriyoruz. Daha sonra paketi açıp bash ekranında crunch klasörünün içine giriyoruz ve aşağıdaki komutu veriyoruz.

    sudo make

&nbsp;

ve daha sonra

    sudo make install

&nbsp;

komutunu veriyoruz. Bu işlemlerin sonunda crunch paketimizin kurulmuş olması gerekiyor.

Paket kurulumu tamamlandığına göre wordlist oluşturmaya geçebiliriz.

Bu adımı örneklerle inceleyelim. Aşağıdaki komutu veriyorum.

    crunch 1 4 0123456789 -o /home/barisayar/Desktop/wordlist.txt

&nbsp;

Bu komutta "1 4" yazdığımız bölüm kaç karakter aralılığında olacağını belirtiyor. Örneğin x sitedeki bir kullanıcının şifresini kırmak istiyorsunuz. Ama x sitesi kullanıcılarına şifren en az 6 karakter olmalıdır şartı koşuyor. Bu durumda bu komut benim işime yaramaz. O bölümü "6 6" veya "6 7" veya "6 8" olarak ayarlamanız gerekir. Bu aralığı isteğiniz doğrultusunda değiştirebilirsiniz fakat hane sayısı arttıkça dosya boyutuda giderek büyüyecek ve bilgisayarınızın işlemcisi bir noktadan sonra yetersiz kalacak ve yanıt vermeyecektir.

Yukarıdaki komutta sadece sayıların kombinasyonunu aldık şimdi işin içine harfleri de katalım. İndirdiğimiz crunch klasörünün içinde charset.lst adında bir yazı dosyası mevcut. Şimdi bu dosyadan yararlanacağız.

charset.lst

    hex-lower = [0123456789abcdef]
    hex-upper = [0123456789ABCDEF]
    
    numeric = [0123456789]
    numeric-space = [0123456789 ]
    
    symbols14 = [!@#$%^&amp;amp;amp;amp;*()-_+=]
    symbols14-space = [!@#$%^&amp;amp;amp;amp;*()-_+= ]
    
    symbols-all = [!@#$%^&amp;amp;amp;amp;*()-_+=~`[]{}|\:;"'&amp;amp;amp;lt;&amp;amp;amp;gt;,.?/]
    symbols-all-space = [!@#$%^&amp;amp;amp;amp;*()-_+=~`[]{}|\:;"'&amp;amp;amp;lt;&amp;amp;amp;gt;,.?/ ]
    
    ualpha = [ABCDEFGHIJKLMNOPQRSTUVWXYZ]
    ualpha-space = [ABCDEFGHIJKLMNOPQRSTUVWXYZ ]
    ualpha-numeric = [ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789]
    ualpha-numeric-space = [ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789 ]
    ualpha-numeric-symbol14 = [ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&amp;amp;amp;amp;*()-_+=]
    ualpha-numeric-symbol14-space = [ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&amp;amp;amp;amp;*()-_+= ]
    ualpha-numeric-all = [ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&amp;amp;amp;amp;*()-_+=~`[]{}|\:;"'&amp;amp;amp;lt;&amp;amp;amp;gt;,.?/]
    ualpha-numeric-all-space = [ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&amp;amp;amp;amp;*()-_+=~`[]{}|\:;"'&amp;amp;amp;lt;&amp;amp;amp;gt;,.?/ ]
    
    lalpha = [abcdefghijklmnopqrstuvwxyz]
    lalpha-space = [abcdefghijklmnopqrstuvwxyz ]
    lalpha-numeric = [abcdefghijklmnopqrstuvwxyz0123456789]
    lalpha-numeric-space = [abcdefghijklmnopqrstuvwxyz0123456789 ]
    lalpha-numeric-symbol14 = [abcdefghijklmnopqrstuvwxyz0123456789!@#$%^&amp;amp;amp;amp;*()-_+=]
    lalpha-numeric-symbol14-space = [abcdefghijklmnopqrstuvwxyz0123456789!@#$%^&amp;amp;amp;amp;*()-_+= ]
    lalpha-numeric-all = [abcdefghijklmnopqrstuvwxyz0123456789!@#$%^&amp;amp;amp;amp;*()-_+=~`[]{}|\:;"'&amp;amp;amp;lt;&amp;amp;amp;gt;,.?/]
    lalpha-numeric-all-space = [abcdefghijklmnopqrstuvwxyz0123456789!@#$%^&amp;amp;amp;amp;*()-_+=~`[]{}|\:;"'&amp;amp;amp;lt;&amp;amp;amp;gt;,.?/ ]

burada sadace harflerin kombinasyonunu kullanacaksak "ualpha" ya da harf sayı karışık bir kombinasyon hazırlamak istiyorsanız "ualpha-numeric" komutunu eklemeliyiz. Şimdi ben 6 ile 8 hane arası bütün sayı ve harf kombinasyonlarını txt dosyasına dökmek istiyorum o halde vermem gereken komut:

    crunch 6 8 -f charset.lst alpha-numeric -o /home/barisayar/desktop/output.txt

&nbsp;

bu komutu verdiğimde Masaüstümde output.txt isminde bir dosya oluşturup içine 6, 7 ve 8 haneli bütün sayı ve harflerin kombinasyonunu dökmüş oldum.
<h5>Brute Force Saldırısı</h5>
Wordlistimizi hazırladık peki ne işe yarayacak bu? Şimdi hydra adlı paketle bu wordlistteki her kombinasyonu şifre olarak denettireceğiz hesaba. Öncelikle hydra paketini indirmek için aşağıdaki komutu veriyoruz.

    git clone https://github.com/vanhauser-thc/thc-hydra.git

&nbsp;

Daha sonra gerekli yazma izinlerini veriyoruz.

    sudo chmod -R 755 thc-hydra

&nbsp;

Şimdi eksik bağımlılıklarımızı tamamlayalım.

    sudo apt-get install zlib1g-dev libssl-dev libidn11-dev libcurses-ocaml-dev libpcre3-dev libpq-dev libsvn-dev libafpclient-dev libssh-dev

Şimdi hydra paketini yapılandırabiliriz. thc-hydra dizinine giderek aşağıdaki komutları yürütelim.

    make -jX

&nbsp;

buradaki x bilgisayarınızdaki işlemci sayısıdır.

    sudo make install

&nbsp;

Böylelikle hydra programına da kullanıma hazır hale getirmiş olduk. Şimdi saldırıya geçebiliriz. hydra dizinindeyken aşağıdaki komutu kendimize göre düzenleyip çalıştırabiliriz.

    -l admin -P wordlist.txt -f -s 22 -t 4 -e ns 127.0.0.1 ssh

&nbsp;

Bonus

Bu scriptin mantığıda brute force'a dayanmaktadır. Elimizde 87f69271c0c6e1884df583fca7daa75d gibi bir md5 kodu olsun. Bu script sayesinde yine wordlistteki tüm kombinasyonların md5 değerlerini alıp elimizdeki md5 ile karşılaştırma yapacağız.

Python script kodumuz


    #-*- coding: utf-8 -*-
    import hashlib
     
    print """
    ###########################
    # Md5 Cracker #
    # www.hubeybi.com #
    ###########################
    """
     
    md5hash = raw_input("Kırılacak Hash: ")
    wordlist = open("wordlist.txt", "r").readlines()
     
    for kelime in wordlist:
     kelime = kelime.strip()
     kir = hashlib.md5(kelime).hexdigest()
     if kir == md5hash:
     print "MD5 Cracklendi: " + kelime
     break

Scripti <a href="https://github.com/hubeybi/md5-crack">Github sayfamdan</a> indirebilirsiniz.