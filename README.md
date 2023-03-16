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

###### Sudo komutu, komutları yönetici yetkisiyle çalıştırmamıza olanak sağlayan komuttur. Peki neden böyle bir komuta ihtiyaç duyarız? Bütün komutları root olarak yönetmek aslında tehlikelidir. Çünkü farkında olmadan çok güçlü bir komutu çalıştırıp sisteme zarar verebiliriz ya da daha da önemlisi farkında olmadan zararlı bir dosyayı çalıştırıp sistemin saldırgan tarafından ele geçirilmesine sebep olabiliriz. whoami komutunu yazdığımızda defne11 olduğumu söyledi : 

![whoami](https://user-images.githubusercontent.com/97543719/225611237-8d27d6dd-6747-4e33-8264-abe52fcedb1a.png)


###### id komutunu yazdığımda :

![id](https://user-images.githubusercontent.com/97543719/225611319-684b5618-4cf5-428f-82ac-f7194336e818.png)


###### groups komutunu yazdığımda :

![groups](https://user-images.githubusercontent.com/97543719/225611386-77369ae2-f038-4167-b482-e96a158ccbe3.PNG)


###### yerlerinin üyesi olduğumu söyledi. Sudo üyesi olmak ne anlama geliyor mesela? Bunu öğrenmek için sudo -l komutunu yazalım. Şifreyi girelim. Bu komut hem belli bir süreliğine tekrardan şifre istemeyecek hem de tüm yetkilere sahip olduğumuzu gösterecek. Sudo -k dersem sudo -l özelliğini iptal etmiş olurum. Tekrar sudo -l yazmaya kalkışsam bu sefer şifre girmemi isteyecektir : 

![sudo l ve k](https://user-images.githubusercontent.com/97543719/225612160-6d1966c3-0997-4e14-abc4-c9d6dd4fa97d.PNG)

###### sudo -i beni root yapacak :

![sudo -i](https://user-images.githubusercontent.com/97543719/225613071-d01c5986-dda2-443d-95e7-f025a88ed772.png)


###### Peki kullanıcı eklemek istersem ? Bunu yapmak için sudo useradd komutunu kullanırız ve eklemek istediğimiz kişinin adını yazarız :

###### Bu kişiye parola oluşturmak istersek sudo passwd komutunu kullanırız : 


