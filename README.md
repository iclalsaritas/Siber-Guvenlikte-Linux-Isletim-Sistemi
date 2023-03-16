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


#### -Komut Kullanımı :

###### Sudo komutu, komutları yönetici yetkisiyle çalıştırmamıza olanak sağlayan komuttur. Peki neden böyle bir komuta ihtiyaç duyarız? Bütün komutları root olarak yönetmek aslında tehlikelidir. Çünkü farkında olmadan çok güçlü bir komutu çalıştırıp sisteme zarar verebiliriz ya da daha da önemlisi farkında olmadan zararlı bir dosyayı çalıştırıp sistemin saldırgan tarafından ele geçirilmesine sebep olabiliriz.

