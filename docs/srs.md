**Hazırlayanlar** 2407231006 - Metehan Bayram 2407231029 - Mustafa Efe Kaya 2407231079 - Osman Nuri Gönültaş 

Tarih 13.10.2026 Ders adı PHP Programlama 

**2** 

# İçindekiler

- [1) GİRİŞ](#1-giriş)
  - [1.1) Amaç](#11-amaç)
  - [1.2) Kapsam](#12-kapsam)
- [2) UML DİYAGRAMLARI](#2-uml-diyagramları)
  - [2.1) Mimari / Katman Şeması](#21-mimari--katman-şeması)
  - [2.2) Use Case Diyagramı](#22-use-case-diyagramı)
  - [2.3) Class Diyagramı ve Detaylar](#23-class-diyagramı-ve-detaylar)
  - [2.4) ER Diyagramı](#24-er-diyagramı)
  - [2.5) Tasarım Desenleri](#25-tasarım-desenleri)
  - [2.6) Sıralama Diyagramı](#26-sıralama-diyagramı)
- [3) SPESİFİK GEREKSİNİMLER](#3-spesifik-gereksinimler)
  - [3.1) Fonksiyonel Gereksinimler](#31-fonksiyonel-gereksinimler)
  - [3.2) Fonksiyonel Olmayan Gereksinimler](#32-fonksiyonel-olmayan-gereksinimler)
- [4) ARAYÜZ TASARIMLARI](#4-arayüz-tasarımları)
  - [4.1) Arayüz Sayfa Haritası](#41-arayüz-sayfa-haritası)
- [5) RİSK ANALİZİ VE PLANI](#5-risk-analizi-ve-planı)
- [6) İZLENEBİLİRLİK MATRİSİ](#6-i̇zlenebilirlik-matrisi)
- [7) GEREKSİNİM ÖNCELİKLENDİRME](#7-gereksinim-önceliklendirme)
- [8) AHP ÖNCELİKLENDİRME](#8-ahp-önceliklendirme)

---



**3** 

## **1.1) AMAÇ** 

OtobüsBilet sisteminin amacı, şehirlerarası yolcu taşımacılığı yapan firmaların ve yolcuların bilet alım-satım süreçlerini dijitalleştirerek, güvenli, hızlı ve tutarlı bir platform üzerinden yönetilmesini sağlamaktır. Bu SRS (Yazılım Gereksinim Belirtimi) dokümanı, sistemin geliştirme (kodlama), test etme ve tasarım aşamalarında görev alacak tüm proje ekibine (yazılım mühendisleri, arayüz tasarımcıları, veritabanı yöneticileri) sistemin gereksinimlerini net bir şekilde belirtmek için hazırlanmıştır. 

## **1.2) Kapsam** 

Bu yazılım, otobüs firmalarının sefer düzenleme, araç filosu yönetme ve bilet satış işlemlerini merkezi bir web uygulaması üzerinden gerçekleştirmesini hedefler. Sistem, yolculara istedikleri güzergâh ve tarihte sefer arama, otobüs içi koltuk planını (2+1 veya 2+2) görsel olarak inceleme, yan yana farklı cinsiyetten kişilerin oturmasını engelleyen kurala uygun olarak koltuk seçme ve simüle edilmiş bir ödeme altyapısıyla bilet satın alma imkânı sunar. Yöneticiler ise sistem üzerinden yeni seferler ekleyebilir, otobüs tanımlayabilir ve sefer doluluk oranlarını raporlayabilir. Sistem; eşzamanlılık (aynı koltuğun iki kişiye satılmasını önleme) ve veri tutarlılığı konularına odaklanarak, bilet iptali ve ücret iadesi gibi süreçleri de kapsayacak şekilde tasarlanmıştır. 

## **2) UML Diyagramları** 

## **2.1) Mimari / Katman Şeması** 

<img width="947" height="364" alt="mimarivekatman_semasi" src="https://github.com/user-attachments/assets/6a6e4f2e-5343-4349-9752-77a65d4ee1d1" />





## **2.2) Use Case Diyagramı** 

<img width="2752" height="1536" alt="usecase" src="https://github.com/user-attachments/assets/53298629-4ac6-4c33-b879-2fbd13864b86" />




**4** 

## **2.3) Class Diyagramı ve ** 



<img width="1376" height="768" alt="classdiyagram" src="https://github.com/user-attachments/assets/484bca28-20d5-427d-be78-31dfd68a2cc0" />


## 2 **.4) ER Diyagramı** 



<img width="927" height="440" alt="er_diyagrami" src="https://github.com/user-attachments/assets/ebd41233-f75b-4f6e-8579-228dc6694fb2" />


**5** 

## **2.5) Tasarım Desenleri** 


<img width="872" height="406" alt="tasarımdesenleri" src="https://github.com/user-attachments/assets/2ca1b1b7-471c-4272-a57e-291f46cc86f0" />


## **2.6) Sıralama Diyagramı** 



<img width="2752" height="1536" alt="siralama_diyagram" src="https://github.com/user-attachments/assets/c410a791-92a5-4251-85cd-fa2d6112d69d" />


**3) SPESİFİK GEREKSİNİMLER** 

## **3.1) Fonksiyonel Gereksinimler** 

- **FR-01:** Sistem, yolcu ve yönetici rollerini barındıracak şekilde kullanıcı kayıt, giriş ve çıkış işlemlerini desteklemelidir; bu süreçler KimlikController ve KimlikService bileşenleri tarafından yönetilecektir. 

- **FR-02:** Sistem, ara durakları da içerecek şekilde detaylı güzergâh ve durak yönetimine imkan tanımalıdır; bu işlemler YonetimGuzergahController ve GuzergahService ile sağlanacaktır. 

- **FR-03:** Otobüslerin 2+1 ve 2+2 koltuk planları gibi tasarımlarına göre sisteme eklenip yönetilmesi işlemleri YonetimOtobusController ve OtobusService üzerinden gerçekleştirilecektir. 

**6** 

- **FR-04:** Sistem üzerinde seferlerin oluşturulması ve düzenlenmesi YonetimSeferController ve SeferService kullanılarak yapılacaktır. 

- **FR-05:** Kullanıcılar nereden, nereye ve tarih parametrelerini girerek ara duraklar dâhil olmak üzere sefer araması yapabilmelidir; arama işlemleri SeferAramaController ve SeferAramaService tarafından işlenecektir. 

- **FR-06:** Kullanıcılara koltukların boş, dolu, kilitli veya seçili durumlarını anlık olarak güncelleyen görsel bir koltuk haritası sunulmalı ve bu yapı KoltukController ile KoltukHaritasiService tarafından kontrol edilmelidir. 

- **FR-07:** Kullanıcı bir koltuk seçtiğinde çakışmaları önlemek adına 10 dakikalık geçici bir kilit uygulanmalı, süre dolduğunda kilit otomatik kalkmalıdır; bu mekanizma KoltukController ve KoltukKilitService ile yönetilecektir. 

- **FR-08:** Koltuk doluluk durumları durağa göre (etap bazlı) hesaplanmalı ve KoltukKilitService ile EtapCakisma bileşenleri sayesinde yönetilmelidir. 

- **FR-09:** Sistem, yan yana olan koltuklarda karşı cinsiyetten kişilerin oturmasını engelleyen kuralı KoltukKilitService ve KoltukPlani üzerinden denetlemelidir. 

- **FR-10:** Satın alma aşamasında yolcu bilgileri alınmalı, TC kimlik doğrulaması yapılmalı ve işlem başına en fazla 5 koltuk sınırı konulmalıdır; bu aşama SatinAlmaController, SatinAlmaService ve TcKimlikDogrulayici tarafından yürütülecektir. 

- **FR-11:** Sistem, simüle edilmiş bir ödeme süreci sonrasında PNR koduna sahip bilet üretimini SatinAlmaService, OdemeService ve PnrUretici kullanarak tamamlamalıdır. 

- **FR-12:** Kullanıcılara yazdırılabilir formatta bilet sunulabilmesi için BiletController ve views/biletler/show.php kullanılacaktır. 

- **FR-13:** Kullanıcıların sahip oldukları PNR kodunu kullanarak bilet sorgulaması yapabilmesi BiletController ve BiletService ile sağlanacaktır. 

- **FR-14:** Bilet iptal işlemleri ve zamana/orana göre iade tutarının hesaplanması BiletService ve IadeHesaplayici tarafından gerçekleştirilecektir. 

- **FR-15:** Yöneticiler için seferlerin doluluk oranlarını ve gelir durumlarını gösteren raporlar YonetimRaporController ve RaporService ile oluşturulacaktır. 

- **FR-16:** Sistem güvenliğini sağlamak amacıyla CSRF koruması, çıktı kaçışlama, rol tabanlı yetkilendirme ve sunucu tarafı doğrulamaları Router ve tüm Controller'lar seviyesinde uygulanacaktır. 

## **3.2) Fonksiyonel olmayan Gereksinimler** 

- **NFR-01 Performans:** Sistemdeki sefer arama ve koltuk haritası görüntüleme işlemleri, sunucu tarafında maksimum 2 saniyenin altında yanıtlanmalıdır. 

- **NFR-02 Doğruluk:** Veri tutarlılığını sağlamak amacıyla; aynı koltuk, aynı etapta (durak aralığında) kesinlikle iki farklı kişiye satılamaz. 

- **NFR-03 Güvenlik:** Sistem güvenliği; kullanıcı şifrelerinin password_hash ile şifrelenmesi, form işlemlerinde CSRF belirteci (token) kullanılması, XSS saldırılarına karşı çıktı kaçışlama (escaping) yapılması, SQL enjeksiyonlarına karşı hazırlanmış sorgu (prepared statements) kullanılması, sayfalarda rol yetkisi denetimi yapılması ve arayüzde TC kimlik numaralarının maskelenmesi yöntemleriyle sağlanmalıdır. 

- **NFR-04 Kullanılabilirlik:** Kullanıcı deneyimini artırmak için koltuk haritası sayfa yenilenmeden (asenkron olarak) güncellenmeli, kullanıcılara sistem tarafından anlaşılır Türkçe hata mesajları gösterilmeli ve tüm arayüz mobil cihazlarla tam uyumlu (responsive) olmalıdır. 

- **NFR-05 Güvenilirlik:** Bilet satın alma işlemleri veritabanı seviyesinde "ya hep ya hiç" (transaction) prensibiyle çalışmalı; herhangi bir hata anında işlemler geri alınarak (rollback) sistemde yarım veya tutarsız kayıt kalması engellenmelidir. 

**7** 

   - **NFR-06 Bakım:** Proje, belirlenen katmanlı mimari standartlarına sıkı sıkıya bağlı kalılarak geliştirilmeli; gelecekte yapılabilecek olası bir veritabanı veya ORM değişimi yalnızca Repository katmanını etkilemelidir. 

   - **NFR-07 Test Edilebilirlik:** Sistemin çekirdek servisleri (özellikle kilit ve biletleme işlemleri) dışa bağımlılıkları izole edilerek birim testli (unit test) olarak geliştirilmeli; zamana bağlı iş kurallarının testi, sisteme sahte saat (mock clock) enjekte edilerek güvenilir şekilde yapılabilmelidir. 

   - **NFR-08 Uyumluluk:** Geliştirilen uygulama PHP 8.2 ve üzeri sürümlerde, MySQL 8 veritabanı altyapısıyla ve tüm güncel web tarayıcılarında sorunsuz çalışmalı; sunucu ve veritabanı zaman dilimi standart olarak Europe/Istanbul saatine ayarlanmalıdır. 

- **4) Arayüz Tasarımları** 

## **4.1) Arayüz Sayfa Haritası** 


<img width="902" height="398" alt="arayuz_sayfa_haritasi" src="https://github.com/user-attachments/assets/1c44e658-072d-4367-804d-2a5159357dd4" />

## **5) Risk Analizi ve Planı** 



<img width="878" height="395" alt="risk_analizi" src="https://github.com/user-attachments/assets/8363f9eb-2646-4b44-aa5d-774a6eeeed48" />



## **8** 
## **6) İzlenebilirlik Matrisi ** 
<img width="2752" height="1536" alt="izlenebilirlik_matirisi" src="https://github.com/user-attachments/assets/18f610e9-6ef0-454c-b810-9d99b8db9b0d" />


## **7) Gereksinim Önceliklendirme** 



<img width="2752" height="1536" alt="gereksinim_onceliklendirme" src="https://github.com/user-attachments/assets/98312eea-9134-40b1-9aca-d5ee4f8cf92a" />

## **8)AHP Önceliklendirme** 


<img width="2752" height="1536" alt="ahp" src="https://github.com/user-attachments/assets/98160269-80f2-405e-946f-ab9b642de18a" />

**9** 

