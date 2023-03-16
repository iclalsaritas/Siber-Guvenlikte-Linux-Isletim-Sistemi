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

###### Sudo komutu, komutları yönetici yetkisiyle çalıştırmamıza olanak sağlayan komuttur. Peki neden böyle bir komuta ihtiyaç duyarız? Bütün komutları root olarak yönetmek aslında tehlikelidir. Çünkü farkında olmadan çok güçlü bir komutu çalıştırıp sisteme zarar verebiliriz ya da daha da önemlisi farkında olmadan zararlı bir dosyayı çalıştırıp sistemin saldırgan tarafından ele geçirilmesine sebep olabiliriz. whoami komutunu yazdığımızda iclal olduğumu söyledi : 

###### id komutunu yazdığımda :


###### groups komutunu yazdığımda :

###### yerlerinin üyesi olduğumu söyledi. Sudo üyesi olmak ne anlama geliyor mesela? Bunu öğrenmek için sudo -l komutunu yazalım. Şifreyi girelim. Bu komut hem belli bir süreliğine tekrardan şifre istemeyecek hem de tüm yetkilere sahip olduğumuzu gösterecek. Sudo -k dersem sudo -l özelliğini iptal etmiş olurum. Tekrar sudo -l yazmaya kalkışsam bu sefer şifre girmemi isteyecektir. Sudo -i komutu root olmamızı sağlar : 

###### Peki kullanıcı eklemek istersem ? Bunu yapmak için sudo useradd komutunu kullanırız ve eklemek istediğimiz kişinin adını yazarız :

###### Bu kişiye parola oluşturmak istersek sudo passwd komutunu kullanırız : 

[![Staj](https://user-images.githubusercontent.com/97543719/225608457-cc26636d-31c3-4e57-aed8-a3ce2d80e1a2.png)
](http://url/to/img.png)

