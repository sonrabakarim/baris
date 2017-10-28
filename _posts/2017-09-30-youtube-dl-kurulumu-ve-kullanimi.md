---
title: Youtube-Dl Kurulumu ve Kullanımı
category: blog
last_modified_at: 2017-09-30
---
Bu yazıda Youtube-Dl kurulumuna ve bu yazılımın kullanımını ele alacağız. Bu yazılımı; Youtube, Dailymotion, Google Video, Facebook ve benzeri sitelerden video indirmek için kullanacağız. Kurulumu ve kullanımı oldukça basit. Kurulumunu Repolardan veya kendi sitesinden gerçekleştirebilirsiniz. Ben aşağıdaki komutu girerek kurulumu repo üzerinden gerçekleştiriyorum.

    sudo apt-get install youtube-dl

&nbsp;

Eğer repoları kullanmak istemiyorsanız sırasıyla aşağıdaki komutları yürüterek de kurulumu tamamlayabilirsiniz.

    sudo wget https://yt-dl.org/downloads/latest/youtube-dl -O/usr/local/bin/youtube-dl

&nbsp;

    sudo chmod a+rx /usr/local/bin/youtube-dl

&nbsp;

Yükleme gerçekleşti. Şimdi gelelim bu yazılımı nasıl kullanacağımıza. Yazılımı ihtiyaçlarım doğrultusunda kullanabilmem için yazılımın neler yapabildiğini bilmem gerekiyor. Bunun için youtube-dl yardım sayfasından yararlanacağım.

    youtube-dl --help

&nbsp;

Bu işlemden sonra ekrana kullanabileceğim opsiyonların dökülmüş olması gerekiyor. Bu opsiyonları kullanarak isteğim doğrultusunda indirmeler gerçekleştireceğim. Basit olarak video indirmek istiyorsam aşağıdaki komutu vermem yeterli olacaktır.

    youtube-dl video linki

&nbsp;

Diyelim ki indireceğiniz videoda format veya kalite seçimi yapmak istiyorsunuz. Bunu yapabilmek için -F parametresini ekliyoruz. Örnek olarak son zamanlarda moda olan Aleyna Baba'nın O Sen Olsan Bari klibini ele alalım. :)

&nbsp;

    youtube-dl -F https://www.youtube.com/watch?v=yJpJCZYTL74

aşağıdaki resimde görüldüğü gibi indirebileceğim seçenekler sıralanacak.

   
![aleyna-baba](https://www.hubeybi.com/wp-content/uploads/2017/08/aleyna-baba.png)

Şimdi ben bu video klibini mp4 formatında ve 1920x1080 çözünürlüğünde indirmek istiyorum. Bunun için aşağıdaki komutu yürütüyorum.

    youtube-dl -f 137 https://www.youtube.com/watch?v=yJpJCZYTL74

&nbsp;

Eğer mp4 formatında ve 1280x720 çözünürlüğünde indirmek isteseydim -f 22 parametresini kullanacaktım.

> <strong><span style="color: #800000;">Uyarı:</span></strong> Burada
> "-F" ve "-f" kullanımına dikkat etmeniz gerekiyor. İndirebileceğim
> türleri listelemek için "-F" parametresini kullanıyorum, indirmeyi
> başlatmak için ise "-f" parametresini. Yani büyük küçük harf
> kullanımını gözardı etmeyin.

&nbsp;

Diyelim ki güzel bir playlist'e denk geldiniz ama bu listede yer alan her video için tek tek uğraşmak istemiyorsunuz. Playlist'i toplu bir şekilde indirmek için ise aşağıdaki komutu kullanabilirsiniz.

    youtube-dl -cit playlist_adresi

&nbsp;


İndireceğiniz videoda altyazı olduğunu varsayalım. Bu videoyu indirdiğinizde -eğer yabancı diliniz yoksa- altyazısız bir işinize yaramayacaktır. Peki altyazılı bir şekilde videoyu nasıl indirebiliriz. Öncelikle videodaki altyazı dosylarını listeleyelim.

    youtube-dl --list-subs https://www.youtube.com/watch?...

&nbsp;

Video indirmesini geçip altyazıyı dosyasını indirmek için alttaki komutu yürütüyoruz.

    youtube-dl --all-subs --skip-download https://www.youtube.com/watch?...

&nbsp;

Son olarak her ne kadar yasal olmasada müzik arşivinizi genişletmek istediğinizi varsayalım. Ama videoyu indir, formatını mp3'e döndür vs. işlemlerle uğraşmak istemiyorsanız aşağıdaki komutu kullanarak videoyu otomatik mp3 formatında kaydedebilirsiniz.

    youtube-dl --extract-audio --audio-format mp3 video_linki

&nbsp;

Youtube-Dl yardım sayfasında belirtilen parametreleri kullanarak ihtiyaçlarınıza göre bu komutları çoğaltabilirisiniz.

> Eğer "ERROR: ffprobe or avprobe not found. Please install one."
> şeklinde bir hatayla karşılaşırsanız aşağıdaki komutu yürüterek bu
> sorunu çözebilirsiniz.

&nbsp;

    sudo apt-get install -y libav-tools