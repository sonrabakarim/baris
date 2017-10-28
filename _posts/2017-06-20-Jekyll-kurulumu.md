---
title: Jekyll Nedir? Jekyll Kurulumu Rehberi
category: blog
last_modified_at: 2017-06-21
---

Jekyll nedir sorusunun cevabı kısaca; markdown ile düz metinleri  statik websitelerine ve bloglara dönüştürmeye yarayan bir araçtır. Jekyll sayesinde blog gönderilerinizi ve sayfalarınızı bir veritabanına ihtiyaç duymadan görüntüleyebilirsiniz. Ayrıca bu oluşturulan içerik Github Pages sayesinde Github üzerindeki bir repository’den kolaylıkla yayına alınabilir.
<h5>Jekyll Kurulumu Yapma</h5>
Şimdi gelelim Jekyll kurulumuna. Bilgisayarımda şuan Ubuntu kurulu olduğu için Linux ortamında anlatacağım. Jekyll'ı kurabilmemiz için bir kaç yazılıma ihtiyacımız var, bunlardan biri de Ruby.

İlk önce her zaman ki gibi ilk iş olarak update yapalım.

    $ sudo apt-get update

&nbsp;

Şimdi Ruby kuruluma geçebiliriz

$ sudo apt-get install ruby ruby-dev make gcc

&nbsp;

Ve son olarakta Jekyll Bundler kurulumunu yapıp diğer aşamalara geçebiliriz.

    $ sudo gem install jekyll bundler

&nbsp;

Blogumuzu oluşturmak için herşeyi böylelikle hazırlamış olduk.

Şimdi 'cd' komutuyla oluşturmak istediğiniz dizine gidin. Bu işlemden sonra

    $ jekyll new site_ismi

&nbsp;

yazıyoruz.

İşlemleri başarıyla yaptıysanız klasörünüz bu şekilde hazır durumda olacaktır.

![resim 1](https://www.hubeybi.com/wp-content/uploads/2017/06/jekyll1.png)

&nbsp;

Şimdi yapmamız gereken işlem yine 'cd' komutuyla dosyaların bulunduğu klasörün için girmek. Örneğin benim dosyalarım "barisayar" adlı klasörde olduğu için

    $ cd ./barisayar

&nbsp;

komutunu verip bu klasörün içine girdim ve daha sonra

    $ jekyll serve

&nbsp;

komutunu veriyorum. Daha sonra terminalde böyle bir ekranla karşılacaksınız.

![enter image description here](https://www.hubeybi.com/wp-content/uploads/2017/06/jekyll2.png)

Jekyll Serve işlemini yaptıktan sonra klasörümüzün içine baktığımızda dosyalarımızda değişiklikler olduğunu göreceğiz.

![enter image description here](https://www.hubeybi.com/wp-content/uploads/2017/06/jekyll3.png)

Şimdi bizim için önemli olan _site klasörümüzü böylelikle oluşturmuş olduk. Eğer bir yerlerde bir yanlış yapmadıysak http://localhost:4000 adresine tıkladığımızda böyle bir sayfa karşımıza çıkması gerekiyor.

![jekyll](https://www.hubeybi.com/wp-content/uploads/2017/06/jekyll4.png)

Bundan sonra internetten beğeneceğiniz bir Jekyll temasını indirebilirsiniz. İndirdiğiniz tema dosyalarını bu klasör içine atın ve tekrar

    $ jekyll serve

&nbsp;
komutunu yürütün. Tebrikler artık blogunuz hazır. :)
<h5>Jekyll Blogumuzu İnternette Yayınlama</h5>
Eee! Blogumuz hazırda buna bir tek biz erişebiliyoruz, olmaz ki böyle. Şimdi sıra geldi bu güzel blogu bütün dünyayla paylaşmaya. Eğer Github'a üye değilseniz öncelikle bir üyelik almanız gerekiyor. Üyeliğinizi tamamladıktan sonra, sağ üst bölümde bulunan profil resminizin yanındaki "+" işaretine tıklayın ve "New repository" seçeneğini seçin. İlgili alanları kendinize göre doldurun. Bu işlem sonucunda kullanıcıadi.github.io gibi bir adres oluşacaktır. Bu alanı oluşturduktan sonra yine 'cd' komutuyla blogunuzu yönetmek istediğiniz bir dizine gidin ve

    $ git clone alanadınız.github.io

&nbsp;

komutunu verin. Bu komutu verdikten sonra seçtiğiniz dizine alanadınız.github.io adlı bir klasör gelmiş olması lazım. Şimdi az önce indirdiğimiz tema dosylarını kopyalayıp bu klasör içine yapıştırıyoruz. Buraya kadar tamamsa son kısıma geçebiliriz.

Şimdi terminalimizi açıp alanadınız.github.io klasörüne girin. Örneğin benim hubeybi ve

    $ cd hubeybi.github.io

&nbsp;

yazıyorum bu işlemden sonra

    $ git config –global user.name “githubadınız” $ git config –global user.email “emailadresiniz”

&nbsp;

İlgili yerleri kendinize göre düzenledikten sonra komutunu yürüterek bağlantımızı gerçekleştiriyoruz.

Daha sonra

    $ git init

&nbsp;

komutuyla repository'imizi başlatabiliriz.

&nbsp;

    $ git status

&nbsp;

komutu ile de repository'imizin durumunu, hangi dosyaların eklenip hangi dosyaların kaldırıldığı gibi bilgilere ulaşabiliriz.

Devamında ise

    $ git add . $ git commit -m “bu bölüme karışıklıkları gidermek ve işlemlerinizi kolaylaştırmak içim düzenleme nedeninizi yazın” $ git push origin master

&nbsp;

tüm bu işlemlerin sonunda github kullanıcı adınız ve şifreniz istenecektir. Eğer sorunsuz olarak bu aşamaları tamamladıysanız eğer sizinde bütün dünyayla paylaşabilidiğiniz bir blogunuz yayında demektir. Güle güle kullanınız. :)