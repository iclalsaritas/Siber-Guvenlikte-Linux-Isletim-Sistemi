###### Herkese merhaba. Siber Güvenlikte Linux İşletim Sistemi başlığı altında Linux güvenliği ve sıkılaştırması ile ilgileneceğiz. Linux güvenliğine temel atmaya karar verdiysen doğru yerdesin. Ubuntu üzerinden anlatımımı gerçekleştireceğim. Halihazırda Linux komutlarını bildiğinizi varsayarak anlatımımı yapacağım. İlerlememiz şu başlıklar altında olacaktır : 

#### 1) Kullanıcı Hesap Yönetimi

##### - Komut kullanımı
##### - Parola yönetimi
##### - Hesap açma
##### - Hesap süresi yönetimi
##### - Kilitli hesapların yönetimi

#### 2) Dosya Yönetimi

##### - Dosya sahiplikleri
##### - Önemli kavramlar

#### 3) Şifreleme İşlemleri

##### - Ecryptfs kullanımı
##### - GPG uygulamaları

#### 4) Erişim Denetimi

##### - IPTables kullanımı
##### - SSH servis kurulumu
##### - SSH parolasız erişim yönetimi

----------------------------------------------------------------------------------------------------------------------------------------------------------------------
#### 1) Kullanıcı Hesap Yönetimi

##### - Komut Kullanımı :

###### sudo komutu, komutları yönetici yetkisiyle çalıştırmamıza olanak sağlayan komuttur. Peki neden böyle bir komuta ihtiyaç duyarız? Bütün komutları root olarak yönetmek aslında tehlikelidir. Çünkü farkında olmadan çok güçlü bir komutu çalıştırıp sisteme zarar verebiliriz ya da daha da önemlisi farkında olmadan zararlı bir dosyayı çalıştırıp sistemin saldırgan tarafından ele geçirilmesine sebep olabiliriz. whoami komutunu yazdığımızda defne11 olduğumu söyledi : 

![whoami](https://user-images.githubusercontent.com/97543719/225611237-8d27d6dd-6747-4e33-8264-abe52fcedb1a.png)


###### id komutunu yazdığımda :

![id](https://user-images.githubusercontent.com/97543719/225611319-684b5618-4cf5-428f-82ac-f7194336e818.png)


###### groups komutunu yazdığımda :

![groups](https://user-images.githubusercontent.com/97543719/225611386-77369ae2-f038-4167-b482-e96a158ccbe3.PNG)


###### yerlerinin üyesi olduğumu söyledi. Sudo üyesi olmak ne anlama geliyor mesela? Bunu öğrenmek için sudo -l komutunu yazalım. Şifreyi girelim. Bu komut hem belli bir süreliğine tekrardan şifre istemeyecek hem de tüm yetkilere sahip olduğumuzu gösterecek. sudo -k dersem sudo -l özelliğini iptal etmiş olurum. Tekrar sudo -l yazmaya kalkışsam bu sefer şifre girmemi isteyecektir : 

![sudo l ve k](https://user-images.githubusercontent.com/97543719/225612160-6d1966c3-0997-4e14-abc4-c9d6dd4fa97d.PNG)

###### sudo -i beni root yapacak :

![sudo -i](https://user-images.githubusercontent.com/97543719/225613071-d01c5986-dda2-443d-95e7-f025a88ed772.png)

###### exit komutu ile normal kullanıcı olmaya devam edebilirsin : 

![exit](https://user-images.githubusercontent.com/97543719/225613831-3826b3db-c279-4a96-bce4-e21b9a469c06.png)


###### Peki kullanıcı eklemek istersem ? Bunu yapmak için sudo useradd komutunu kullanırız ve eklemek istediğimiz kişinin adını yazarız :

![ahmet](https://user-images.githubusercontent.com/97543719/225614403-009e83a0-7e47-4416-9e12-0f10e48db8f6.PNG)


###### Bu kişiye parola oluşturmak istersek sudo passwd komutunu kullanırız : 

![passwd](https://user-images.githubusercontent.com/97543719/225614696-26331ca3-ae83-45fc-90e7-010c30778398.PNG)

###### Artık yeni bir kullanıcımız var. Bu kişiye gerekli yetkinlikleri ve izinleri verebiliriz ama dikkat et her şeye erişim yetkisi olmasın :)

###### su ahmet komutunu yazalım. Parolasını girelim ve sudo -i komutunu yazalım. Ahmet için parola isteyecek, parolasını girdiğimizde bir uyarıyla karşılaşıyoruz :

![su ahmet](https://user-images.githubusercontent.com/97543719/225615919-383b0488-8060-402f-954b-d864d7988cca.PNG)

###### Linux ortamlarında genelde sıradan bir kullanıcı cat /etc/passwd dosyasını görür. Kullanıcı izinleri, id değerleri, yetkilendirilmeleri ve aynı zamanda ev dizinleri gibi bilgilendirmeler yer alır ama bu kullanıcı cat /etc/shadow komutunu çalıştıramaz. Burada şifreler, hash özet değerleri vs. yer alır. Root olmak gerekir. sudo usermod -aG sudo ahmet yazdığımızda ahmet'i sudo grubuna almış oluruz : 

![sudo](https://user-images.githubusercontent.com/97543719/225617371-6ac8d036-f28d-498b-bbc5-4236535eb631.png)

###### sudo grubuna kabul edilen Ahmet'in cat /etc/shadow dosyasını nasıl okuyabildiğini görelim :

![ahm](https://user-images.githubusercontent.com/97543719/225617878-9e2207f4-3c8b-4a6b-b406-9a187c54f954.PNG)

###### sudo ile ilgili şimdi özel bir dosyayı tanıyalım. sudo cat /etc/sudoers komutuyla sudoers isimli bir dosyayı açmış oldum ve ben bu sudo yetkisini kimler hangi niteliklerle kullanabiliyor bunu hem okuyabiliyorum hem de düzenleyebiliyorum. elbette nano diyip düzenleyebiliriz lakin bu geçici bir çözümdür. Daha verimli bir yoldan bunu gerçekleştirelim, sudo visudo komutunu kullanalım ve bu düzenlemeye hadi yeni kullanıcımız olan Ahmet üzerinden gerçekleştirelim : 

![visudo](https://user-images.githubusercontent.com/97543719/225619564-06467240-5709-4a03-8b1a-6ae4b0517512.png)

###### Birkaç değişiklik yapacağız : 

![viii](https://user-images.githubusercontent.com/97543719/225620411-e5f7cf95-3d5b-4d45-8bad-4a14594974a2.PNG)

###### Ahmet ALL = (ALL)  ALL yazıp kaydedelim ve çıkalım. Bunu yaptıktan sonra su ahmet yazıp parolasını girip ardından sudo -i dediğimizde artık önümüze engeller çıkmayacak ve root olarak devam edebileceğiz. 

###### Bazı durumlarda, bir kullanıcının belli başlı komutları çalıştırmasını istiyor olabilirim. Burada kendi ubuntumuzda bir senaryo gerçekleştiriyor olsak da, iş hayatında büyük bir sistemi yönetiyor olabiliriz. Bizim yardımcımız olabilir ve onun sadece mesela disklere ulaşmasını istiyor olabilirim. Senaryomuzda Ahmet benim yardımcm olsun ve onun nelere yetkisinin olduğunu düzenlemek için tekrar sudo visudo yazalım ve en alta gidelim : 

![stat](https://user-images.githubusercontent.com/97543719/225622881-d30e0594-0896-476d-b48a-0ce90737dcc9.png)

###### Kaydedip çıkalım. su ahmet diyelim ve parolasını girelim. sudo -l dediğimde bana Ahmet'in nelere yetkisinin olduğunu gösteriyor. sudo komutunuzla bu yetkileri test edebilirsiniz.

![komu](https://user-images.githubusercontent.com/97543719/225624602-8c260fc4-fe51-436e-9341-df73ea1b132d.png)

###### Peki, siber güvenlik anlamında bu konuyu irdeleyecek olursak sizce ahmet ALL = (ALL) /bin/bash doğru bir hareket mi olurdu? Hayır. Çünkü  kabuğu root yetkisiyle açtıktan sonra zaten istediğim komutu çalıştırabilirim. Bu konuyla alakalı yeri gelmişken 2 tane hak yükseltme saldırısı deneyelim mi ?

###### ahmet ALL = (ALL) /home/ahmet/callme.sh iznini verelim ve ardından nano callme.sh diyelim ve şunları yazıp çıkalım :

![safs](https://user-images.githubusercontent.com/97543719/225629863-ff422f22-7fcd-479c-95c5-8eadcd462e8c.png)

###### chmod +x callme.sh diyerek yazma yetkisini de verelim. ./callme.sh :

![hak](https://user-images.githubusercontent.com/97543719/225631166-a1466d1d-5e1a-49d5-a75d-cfa63c705ada.png)

###### Gördüğünüz üzere hak yükseltme saldırısı başarılı oldu. Ahmet üzerinden komutu çalıştırabildim. Bir başka senaryoyu inceleyelim :

![adsjlk](https://user-images.githubusercontent.com/97543719/225632534-fa1ef20c-838c-42c9-8bd6-358fb3d9bf2c.png)

###### gibi kısıtlı bir yetki verelim. su ahmet yazıp parolayı girelim. sudo more /etc/ssh/ssh_config diyelim. Burada özellikle dikkat edin. En alta !bash yazalım. Bakın root olduk : 

![bu kadar](https://user-images.githubusercontent.com/97543719/225636387-830e03fd-1744-4390-8d4d-55696ad75924.png)

###### Bu da shell escape ya da bash escape denilen bir saldırı yöntemidir. more gibi editörler kendi içlerinde bash komutlarını çalıştırmaya müsade etmekteler. Bu da hak saldırısı örneklerindendi.

##### - Güçlü Parola Yönetimi :

###### Sıra geldi güçlü parola yönetim politikasına. Bu konuya başlamadan önce bize yardımcı olacak bir paketi indireceğiz. Terminalinize sudo apt install libpam-pwdqudity yazın. Siber güvenlikte almamız gereken en önemli güvenlik önlemi güçlü parola politikasıdır. Bir hacker ya da bir pentester bize saldırmak istediğinde kullanacağı şey sözlük saldırısıdır. Paket indiğine göre :

![aaaaaaaaaaa](https://user-images.githubusercontent.com/97543719/225638861-00aef348-5cb0-455e-85cb-84a578a03f81.png)

###### sudo gedit /etc/security/pwquality.conf komutunu yazalım. Açıldığında göreceksiniz ki aslında bütün değerler gördüğünüz gibi kapalı durumda. Biz bunları açtıkça etkinleşmiş hale gelecek. Değerleri değiştirip çıkalım.

![dosya](https://user-images.githubusercontent.com/97543719/225639841-88ed1bee-948a-4624-a30d-9dd5ab2a1f43.png)

###### minlen = 12, minclass = 3 olarak değiştirdim. Uygulamalı olarak görmek için bir kullanıcı üzerinden deneme gerçekleştirelim ama bu sefer yeni bir kullanıcı ekleyelim ve onun üzerinden deneyelim : 

![adjksa](https://user-images.githubusercontent.com/97543719/225641574-ecb4ee3f-1039-41b6-93a2-8607fcbcb077.png)

###### Ayşe için oluşturmuş olduğum 123ayşe şifresini kabul etmedi çünkü 12 haneli şartı koymuştum. Şimdi sağlam bir şifre girelim :

![sssssssssss](https://user-images.githubusercontent.com/97543719/225642318-91cc3229-2752-4d53-9db5-c3003c77ba72.png)

###### Koşulları sağlayan bir parola hazırladığımda bunu kabul etti gördüğümüz gibi. Dolayısıyla herhangi bir sistemi ayağa kaldırmadan önce gerçekten sistemi yönetiyorsak güçlü parola politikası uygulamaktan erinmemeli ve kurumsal bir yaklaşımla uygulama yapmalıyız. Mümkünse kurumumuza ait güvenlik yapılandırma dosyasında bütün bunlar tanımlanmış olmalı. Daha derin güçlendirmeler için  gedit /etc/security/pwquality.conf komutuyla dosyayı açıp orada farklı farklı değişiklikler yapmaya devam edebilirsiniz.

##### - Yeni Hesap Açma :

###### Bu bölümde user@ komutuyla yeni bir kullanıcı yaratma, o kullanıcı için bir ev dizini oluşturma ve varsayılan kabuk tanımlama gibi özellikleri inceleyeceğiz. sudo useradd ali -m -d /home/ayşe -s /bin/bash komutunu yazıyoruzç Ardından sudo passwd ali diyoruz ve Ali'ye yeni bir şifre tanımlayabiliyoruz. su ali yazıp şifresini girelim. cd /home yazıp ls -l dediğimde klasörlere girip dolaşabilme yetkilerini görüyorum. Diğer kullanıcıların dosyalarının içerisinde gezinebiliyorum. cd defne11 yazıp ls dediğimde mesela defne11'in dosyalarına ulaştım. Bunu önlemek lazım :

![ali](https://user-images.githubusercontent.com/97543719/225645936-a5c95357-486a-4c6a-831e-f697c42dc754.png)

###### cd.. yapıp iki kere exit yapalım. sudo chmod 700* komutunu yazalım ve bu sefer ls -l dediğimizde klasör yetkilendirmeleri daha kontrollü oldu. Önceki işlemleri tekrarladığında kafana göre başkasının dosyalarında gezemediğini göreceksin. Bu ayarlamaları konfigurasyon dosyası üzerinden yapmak mümkündür. sudo nano /etc/login.defs dediğimiz bir dosya var. Onu açıyoruz. En altta UMASK denen bir değer değer var. O değeri 077 yapıp çıkıyoruz. Bu sayede artık yeni bir kullanıcı oluşturduğumuzda otomatik olarak kilitlenmiş olacaktır.

![umask](https://user-images.githubusercontent.com/97543719/225648515-1f411617-52b4-488b-b122-4ee4863a829c.png)

##### - Hesap Geçerlilik Sürelerinin Yönetimi :

###### Bu kısmı yeni bir kullanıcı üzerinden irdeleyelim . sudo useradd mehmet - m -d /home/mehmet -s /bin/bash -e 2023-06-30 koumutunu yazıyoruz:

![memet](https://user-images.githubusercontent.com/97543719/225653576-9f6d6452-106d-4318-b978-814ebec07775.png)

###### Kullanıcının hesabı kullanma süresi bu tarihte bitecek. Ama belki de bu adam işten daha erken ayrılacak ve ben bunun süresini kısayım biraz dersek eğer, sudo usermod mehmet -e 2023-04-28 diyerek tarihi değiştirmiş oluyorum. Mehmet'e parola da ayarlayalım. sudo passwd mehmet. sudo passwd -e mehmet komutunu yazdığımızda ise Mehmet'in parolasının son kullanma tarihini değiştirmiş oluyoruz:

![mmmm](https://user-images.githubusercontent.com/97543719/225654891-94c0d57b-c34c-4bc1-a67c-d5c63b70b965.png)

###### su mehmet dediğimizde ve parolayı girdiğimizde, parolanızı en kısa sürede değiştirmeniz gerekiyor! uyarısı gelecektir :

![eeeeeeeeee](https://user-images.githubusercontent.com/97543719/225655440-e7ef2c8a-dc48-409f-9299-9ab5bdbde21b.png)

###### Geçerli paroladan sonra yeni parolamızı oluşturunca kullanıcımıza yeni hesabını güven içerisinde teslim etmiş oluyoruz.

##### - Kilitli Hesapların Yönetilmesi :

###### Bu bölümde kullanıcı hesaplarını kilitleme özelliğini inceleyeceğiz. Eğer kullanıcı hesabına oturum açma anlamında bizim belirlediğimiz bir sayı üzerinde deneme gerçekleşiyorsa hesabın otomatik olarak kitlenmesini sağlayabiliriz. sudo gedit /etc/pam.d/common-auth isimli dosyayı açıyorum :

![zzzzzzz](https://user-images.githubusercontent.com/97543719/225657608-193d0a33-fc88-4201-9016-b8f214f79d01.png)

###### here are the per-package modules (the "primary" block) adlı yazının altına : 

![Ekran görüntüsü 2023-03-16 180255](https://user-images.githubusercontent.com/97543719/225658935-cef04df8-1b17-4771-a18f-d5b5540c8a34.png)

###### diye bir yazı ekliyorum. Kaydedip çıkıyoruz. Oturumu kapatıp tekrar girelim. Mesela defne11 diye bir kullanıcımız vardı. Onun üstünden örneğimizi uygulayalım. Rastgele 3 kez parola girelim ve hesabın kilitlendiğine şahit olalım :

![lol](https://user-images.githubusercontent.com/97543719/225662028-bea48d87-8c9e-427c-b99f-5c46c4bb6595.png)

###### İnsanlar bazen parolalarını unutabiliyorlar. O yüzden 3 kez deneme hakkı az olabiliyor ama bir sözlük saldırısı yapılıyorsa mesela 60-200 gibi değer vererek hem saldırıyı tespit edebiliriz hem de hesap sahibini mağdur etmemiş oluruz. sudo pam_tally2 komutunu çalıştıralım. Burada giriş denemelerinin not düşüldüğünü görüyorum. Kilitli hesabı geri açmak isterseniz sudo usermod -U defne11 dersiniz ve açılır. -U = Unlock, -L = Lock anlamlarına gelir. Aynı şekilde sudo passwd -l defne11 derseniz kilitlenir, sudo passwd -u defne11 derseniz açarsınız. Bu da başka bir yöntemdir.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### 2) Dosya Yönetimi

##### - Dosya Sahiplikleri :

###### Biz herhangi bir dosya oluşturduğumuzda sistem tarafından o dosyaya bir kullanıcı ve bir grup sahip olarak atanacaktır. Örneğin echo "yeni dosya" > yeni.txt diye bir dosya oluşturalım. ls -l dediğimizde bu dosyanın sahipliğini görebiliriz. Sahip kullanıcısı defne11 ve grubu defne11 :

![lk](https://user-images.githubusercontent.com/97543719/225676690-576c9b50-756f-47a6-814f-e1aa1af03b55.PNG)

###### Dosyanın sahipliğini değiştirmek istersek chown ahmet:ahmet yeni.txt komutunu yazarız. İşleme izin vermezse başına sudo yaz :

![xcv](https://user-images.githubusercontent.com/97543719/225678111-75a50bba-1102-4ab3-9695-75bd4150097f.PNG)

###### Eğer sahibini tekrar defne11 yapmak istersek chown defne11 yeni.txt komutunu yazmamız yeterli olur. Bu arada grubuna da defne11 demediğimiz için Ahmet olarak kalır. Grubunu da defne11 yapmak istersek defne11 'in başına : koymamız yeterli olacaktır.

###### Bir de shell scripti hazırlayalım :

![asdfg](https://user-images.githubusercontent.com/97543719/225679777-950a3e28-f2ee-4334-8bf4-724812342bf9.PNG)

###### Bu dosyaların belli bir gruba ait olmasını istiyorsam :

![kn](https://user-images.githubusercontent.com/97543719/225681025-f492b87f-1f30-42b5-8e62-fccce9203b1b.PNG)

###### sudo chown :hacker -R lesson/ yazıp cd lesson komutunu uyguluyoruz ardından. ls -l yaptığınızda göreceksiniz ki iki dosyanın grubu da hackera dönüştü.

###### Bildiğimiz üzere shell dosyası kabuk programlamada çalıştırılabilir komutlar demek. Hadi bi' çalıştıralım :
###### ./echo.sh yazdım ama erişime izin vermedi. Peki neden? Çünkü sahibi olarak benim okuma ve yazma yetkim var ama çalıştırma yetkim yok. Bu yetkiyi şöyle ekleyebiliriz. chmod u+x echo.sh diyoruz. Bu sefer ./echo.sh yazarsanız çalıştığını göreceksiniz. chmod g+t echo.sh deseydik, gruba da çalıştırma yetkisi vermiş olacaktık. chmod o+x echo.sh deseydik, others yani diğerlerine de çalıştırma yetkisi vermiş olacaktık. 

###### Bir dosya ilk oluşturulduğunda dikkat ederseniz -rw-r--r-r yetkileri verildiğini görürüz. Bunun aynı zamanda sayısal karşılığı vardır :
###### r = 4, w = 2, x = 1, r+w+x = 7 . Bunların toplamı olarak bizler değer atayabiliyoruz. chmod 777, chmod 700 gibi!

![ASASFD](https://user-images.githubusercontent.com/97543719/225684459-f11f25ea-b2c3-4c8f-87f7-cf48da244adc.PNG)

###### Sayısal ifadelerini stat -c '%n %a' * komutunu kullanarak görmüş olduk. 

##### - Suid, Guid ve Sticky Bit Kavramları :

###### Herhangi bir dosya açıldığında o anda açılmış kullanıcının yetkileriyle açılır. Ancak bazı özel durumlarda o dosyanın sahibinin yetkisiyle özellikle de root yetkisiyle çalıştırmak isteyebiliriz. Örneğin, ls -l /usr/bin/passwd komutunu inceleyelim. which sudo komutuna bakalım. ls- la /usr/bin/sudo'ya bakalım. ls -l /usr/bin/bsd-write komutuna bakalım :

![qqqqqqqqqqqqqq](https://user-images.githubusercontent.com/97543719/225687116-abd82eaa-da97-4e47-b0ce-e90abe439963.PNG)

###### Gördüğünüz gibi, aslında herhangi bir dosyayı çok güçlü bir yetkiyle kullanılmasını sağlayan bir özellik suid bit. Dolayısıyla bunu manipüle etmek de mümkün olabilir diye düşünebiliriz ki eskiden bu çok kolaymış. Şöyle bir örnek verelim :

![kkkkkkkkkkkkkkk](https://user-images.githubusercontent.com/97543719/225688276-ac235c5a-62b8-4962-8638-2784319d6be7.PNG)

###### sudo nano echo.sh girip id ve cat /etc/shadow yazıp çıkalım. ls -l diyelim. 644 değerinde olduğunu görürüz. Çalıştırma yetkisi verelim : chmod +x echo.sh
###### ls -l yaptığımızda çalıştığını görürüz. Suid bit eklemek için daha fazla beklemeye gerek var mı?

![iiiiii](https://user-images.githubusercontent.com/97543719/225689193-2d7a7de2-16d8-4fa3-9e9f-e6b269ebf7a3.PNG)

###### İşin özeti Suid Bit'i mecbur olmadıkça hatta hiç kullanmamaya çalışalım. Bu konuda alınabilecek önlemlerden bir tanesi de disk bölümlenmesini ayağa kaldırdığımızda no dev No suid parametrelerini kullanmaktır.
######  Ayrıca bir de Sticky Bit var. Bazen biz bir klasör ortamında örneğin herkesin ortak şekilde erişim sağlamasını isteyebiliriz. Kendi dosyalarını yaratıp üzerinde değişiklik yapmalarını isteyebiliriz. Ama oturum açan başka bir kullanıcının o klasör içindeki o dosyayı silmesşnş istemeyiz. İşte burada Sticky Bit dediğimiz kavram ortaya çıkıyor. Bunun en güzel örneği ise tmp klasörü. ls -ld /tmp dediğimizde :

![hhhhhhhhhhhhhh](https://user-images.githubusercontent.com/97543719/225691189-f1e36f6e-8d51-4189-90c9-d93c3bdc559c.PNG)

###### burada herhangi bir kullanıcı dosya yaratabilir değişiklik yapabilir ama oturum açan başka bir kullanıcı, diğer kullanıcının yarattığı dosyayı silip değiştirme yetkisine sahip değildir. en sondaki t harfi de bunun Sticky Bit ile tanımlandığını ve biraz önceki özelliklere sahip olduğunu gösteriyor.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### 3) Şifreleme İşlemleri

##### - Ecryptfs Sisteminin Kullanımı :

###### Korumalı bir klasör oluşturalım mı? Oluşturacağımız korumalı klasör içerisinde dosyalarımız otomatik olarak şifrelenecek. Lakin önce bir kütüphane indirmemiz gerekecek. sudo apt install ecryptfs-utils :

![azx](https://user-images.githubusercontent.com/97543719/225696562-00acd341-e789-40fe-8263-6e257cb0f561.PNG)

###### sudo mkdir /troll diye bir dosya oluşturalım. sudo mount -t ecryptfs /troll /troll yazıp klasörü kendi üzerinden mount edelim. Gelen seçenekleri tamamladıktan sonra cd /troll diyelim ve ls yapalım. sudo nano çoktroll.txt diyelim ve bir şeyler yazıp çıkalım. caat çoktroll.txt yazıp içeriği görelim :

![puaha](https://user-images.githubusercontent.com/97543719/225698178-f60840e0-f534-4560-b67f-1e5877c6a65f.PNG)

###### sudo mount -t ecryptfs /secret /secret -o key= passphrase, ecryptfs_cipher= aes, ecryptfs_key_bytes=16, ecryptfs_passthrough=no, ecryptfs_enable_filename_crypto= yes yazalım.
###### Bunları ayarladıktan sonra cd /troll yapıp ls dersek dosyayı görürüz ve cat çoktroll.txt diyerek de içeriğini okuruz.

##### - GPG Uygulamaları :

###### Bu kısımda şifreleme konusunu inceleyeceğiz. Bununla ilgili GPG isimli bir aracı kullanacağız. Hem simetrik hem asimetrik örneklerden bahsedeceğim. Simetrik nedir? Tek bir şifreyi kullanıp onu şifrelemektir. Asimetrik nedir? Ben özel anahtarımla şifrelerken karşı taraf benim açık anahtarımla şifrelemiş olduğum dosyayı açabilecek. Açık anahtar alt yapısında bir özel anahtar vardır bir açık anahtar vardır. Herhangi bir dosyayı şifrelerken özel anahtarımla şifrelerim. Böylelikle karşı taraf benim açık olan anahtarımla bunu açarken bilir ki onu şifreleyen kişi sadece şahsımdır. Dolayısıyla benim imzamla şifrelenmiştir. Kaynağının ben olduğuna yüzde yüz emindir. Aynı şekilde o benim açık anahtarımla herhangi bir şeyi şifrelerse bilir ki onu açabilecek tek kişi benim. Simetrik bir şifreleme yapalım :

![mmmmmmmmmn](https://user-images.githubusercontent.com/97543719/225703078-ba303131-6d9e-4453-9235-49f9dd58e157.PNG)

###### Parola giriyoruz :

![z](https://user-images.githubusercontent.com/97543719/225703300-f931f19c-0594-41f2-a829-f1d8e5c74acb.PNG)

###### ls diyoruz. Ben burada çoktroll.txt görünsün istemiyorum artık :

![x](https://user-images.githubusercontent.com/97543719/225703541-03989e9a-34a7-45e0-9952-fe731e39946a.PNG)

###### shred -u -z çoktroll.txt yapıp ls dediğimizde silindiğini görürüz :

![a](https://user-images.githubusercontent.com/97543719/225704188-965c906c-065d-4c9f-ba48-1e73945fa438.PNG)

###### Özel ve açık anahtar için de yapmak istersek :

![d](https://user-images.githubusercontent.com/97543719/225705664-d8d7a88e-97a1-4e85-99ae-8070071bf0c7.PNG)

##### yes dedikten ve işlem tamamlandıktan sonra anahtarlarımı gördüm. cd .gnups/ yazıp ls diyorum. gpgp --export -a -o defne11_public_key.txt dedikten sonra ls yapalım. cat defne11_public_key.txt yazdığımda açık anahtarım gelmiş olacaktır.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### 4) Erişim Denetimi

##### - IP Tables Kullanımı :

###### Bu bölümde güvenlik duvarı yapılandırmayı inceleyeceğiz. Ubuntu diğer sistemlere göre daha kullanıcı dostu olduğundan ötürü, güvenliği 2. plana atmış durumda. Bu yüzden bizim kendimizin bu güvenliği yapılandırması gerekiyor. Sistemimdeki mevcut güvenlik yapılandırmasını görmemin yolu nedir ?

###### sudo iptables -L ve gördüğümüz gibi 3 tane ana kategori var. Bu ip4 versiyonu için geçerli bu kurallar. Eğer sen ip6 için istiyorsan, sudo ip6tables -L komutunu kullanmalısın. clear yapalım bir. Ip4 versiyonu üzerinden çalışalım biraz. Bir senaryo kuralım, bir sunucu kurdum ve bu sunucuda yavaş yavaş sıkılaştırma yapmak istiyorum. Adım adım ilerleyeyim. Düşünüyorum, kaynağı benim sunucum olan bir trafik karşı tarafla bağlantı kurduysa benim bu trafiği engellememmin bir mantığı yok çünkü zaten trafiğin kaynağı benim. Dolayısıyla bu tarz trafiklere müsade eden bir kuralla başlayalım :

![k](https://user-images.githubusercontent.com/97543719/225710159-86f6eb9e-34f3-487c-83f3-a104f76afbf0.PNG)

###### yazdık ve ilk kuralımızı girmiş olduk. iptables -L dediğimizde gördüğümüz gibi listelemeye başladı. Sonra düşünüyorum, bundan sonra başka ne yapabilirim diye. Ayağa kaldırdığım sunucuya ssh ile ulaşmak istedim değil mi? Öyleyse ssh servisi yani 22 numaralı portu açmamda fayda var :

![v](https://user-images.githubusercontent.com/97543719/225710818-2b001851-c5b0-4145-85de-fa3c2c36bf98.PNG)

###### Böylelikle ssh'ı da kabul eden bir protokol tanımlamış olduk. Ardından düşünüyorum, bu sunucu ister istemez adlandırma sunucusuyla da iletişime geçmek isteyecek. Bir ad almak bir isim almak isteyecek. Dolayısıyla DNS sunucusuyla iletişime geçmek isteyecektir. DNS protokolü neydi? 53 numaralı porttan giden UDP hatta aynı zamanda TCP protokolüydü :

![l](https://user-images.githubusercontent.com/97543719/225711686-1b85930f-1c9d-4627-ad5c-43778972e705.PNG)

###### Ama benim en başta düşünmem gereken trafiklerden ya da kurallardan biri aslında sistemin kendi kendine çelme takmaması yani 127.0.0.1 gibi trafikler. Peki bunu nasıl tanımlayabilirim ve buna nasıl müsade edebilirim :

![ö](https://user-images.githubusercontent.com/97543719/225712345-604c290b-2de5-4c1d-8783-916153ce3694.PNG)

###### Hep duymuşuzdur. Güvenli bir sunucu kurmak istiyorsan pin trafiğini kapatman lazım diye. Kapalı olunca hackerler sizi göremez vs. İnanın hackerlar sizi görür, nmap komutu kadar kolay bir şey. Yeter ki herhangi bir portunuz açık olsun. clear diyelim bir.
###### iptables -A INPUT -m conntrack -p icmp --icmp  -type3 --ctstate NEW, ESTABLISHED, RELATED -j ACCEPT diyoruz. Dikkat ederseniz icmp tür 3 olan trafiğe izin verdim. 3 nedir? Ulaşılamaz mesajlardır. Bir trafiğe gerçekten bir sıkıntı sebebiyle ulaşılamıyorsa benim bunu engellemem iyi bir şey değil, benim bunu fark etmem gerekiyor. Aynı şekilde 3'ün yanında 11 numaralı trafiğe de müsade edelim. Bu da zaman aşımına uğrayan paketleri tanımlıyor. 12'yi de tanımlayalım. 12, bozuk başlık değerine sahip olan icmp türlerini tanımlıyor. Biz bu üçüne müsade edelim. Drop paketi tanımlayalım :

###### iptables -P DROP diyelim. Drop, paketi tamamen kes veya baika bir şey yapma kaale bile alma anlamına gelir. Bu yaptığımız ayarlamalar restart ile uçabilir. Bunların uçmaması için bir paketimiz var . apt install iptables-persistent .
###### Kuralları sıfırlamak istersek :

![ş](https://user-images.githubusercontent.com/97543719/225725365-6da7eea3-cd93-446a-bf9c-6488a9a4a3a1.PNG)

###### Bunları yaptıktan sonra iptables -L dediğimizde en başa döndüğümüzü görmüş oluruz :

![t](https://user-images.githubusercontent.com/97543719/225725669-d67493e1-7e33-499f-a809-445e44ef7262.PNG)

##### - SSH Servis Kurulumu :

###### Bu derste, ssh servisini kullanıcı adı ve şifre girmeden oluşturduğumuz açık ve özel anahtarları kullanarak nasıl doğrudan bağlanabileceğimizi göreceğiz. Senaryomuzda bir kalilinux var diğer tarafta da ubuntumuz var. Aslında bu aynı zamanda hacking yöntemlerinde uzaktan sistemi ele geçirmede birçok zaafiyetli makine çözerken de kullanabileceğimiz bir senaryodan ibaret. Hem güvenlik açısından hem de pentest açısından bu senaryoyu inceleyebiliriz. öncelikle, ssh-keygen diye bir komutumuz var. y diyoruz ve ls diyoruz.

###### cat id_rsa.pub dediğimizde açık anahtarımız ortaya çıkıyor. Bunu kopyalıyoruz. Ubuntuyu açıyoruz. echo "-----" >> authorized_keys adlı bir dosyayı kaydediyoruz. ls dediğimizde dosyayı görüyoruz. Ubuntu artık bu anahtarı karşılayacak özel anahtardan gelen herhangi bir isteği doğrudan kabul edecek, amacımız bu.

###### chmod 600 id_rsa yazıp enter dediğimizde bizden kullanıcı adı ve şifre istemedi. ssh -i id_rsa defne11@ubuntuipsi doğrudan hedef sisteme bağlanmamızı sağladı. Bunun avantajı ne peki? Kullanıcı adı ve şifre girmek bazen risklidir.

##### - SSH Parolasız Erişim Yöntemi :

###### Bu kısımda ssh servisini ayağa kaldıracağız. Bu servis bilgisayarımızda mevcut mu bunu anlamanın yöntemi :

![ç](https://user-images.githubusercontent.com/97543719/225728142-796f12ed-76dd-4910-9ec0-bc9570c5c0a7.PNG)

###### diyorum ve 22 numaralı portta herhangi bir dinleyici bir sınır olmadığını görüyorum. Dolayısıyla servisin çalışmadığı anlamına geliyor. Neler varmış bir bakalım.
###### 53 ve bazı diğer protokoller açıkmış. Anlamanın bir diğer yöntemi de systemctl status ssh ile servislere bakmaktır, aklınızda bulunsun. apt install openssh-server ile ssh ayağa kaldıran paketi indirelim. ssh servisinin sıkılaştırılması anlamında yapılabilecek birçokk şey var. Örneğin, root hesabının uzaktan erişimi kısıtlanabilir, kullanıcı adı şifre yerine özel veya açık anahtarla bağlanmak gibi konfigurasyonlar yapılabilir. Bunun dışında 22 numaralı port yerine başka bir porttan çalışması sağlanarak da sistemim daha kolay erişilmesini engelleyebilir. Basit bir nmap taraması bile bunu ortaya çıkarabilir çünkü. openssh servisinin  sunucumuzda her zaman güncel olması önemlidir. Çünkü openssh çok çok çok eski olursa, ona yönelik zafiyetler de var ve bu bizim pc mizin sunucumuzun güvenliği açısından da riskli bir durum teşkil edebilir. Şimdi bakalım,

###### systemctl enable ssh diyelim, sorun çıkarsa başına sudo koyun her zamanki gibi. Gördüğünüz gibi servisi ayağa kaldırmaya çalışıyoruz. Ekranımızı temizleyip systemctl status shh diyelim ve çalıştığını görelim. ifconfig yazıp ipmizi görelim. Başka bir pc ye geçelim.

###### ssh troll@ipsi yazıp girdiğimizde troll hesabına erişmiş olduk. ssh servisini ayağa kaldrmak ve düzgün yönetmekbizim sistem yöneticiliği alanında hem elimiz ayağımız olacaktır ama aynı zamanda bunun güvenliği ve sıkılaştırmasına önem vermek gereklidir.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

###### | İçeriğimde kullandığım isimler ve takma adlar afetzede çocukların isimleridir. Bu kısım hakkında soru geldiği için ayrıca belirtmek istedim |







































































