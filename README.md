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







































