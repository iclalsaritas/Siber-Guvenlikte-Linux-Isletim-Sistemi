###### Herkese merhaba. Siber Güvenlikte Linux İşletim Sistemi başlığı altında Linux güvenliği ve sıkılaştırması ile ilgileneceğiz. Linux güvenliğine temel atmaya karar verdiysen doğru yerdesin. Ubuntu üzerinden anlatımımı gerçekleştireceğim. İlerlememiz şu başlıklar altında olacaktır : 

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
#### -Komut Kullanımı :

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






