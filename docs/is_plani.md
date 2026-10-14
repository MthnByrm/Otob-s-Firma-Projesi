# Otobüs Bilet Sistemi - Görev Dağılımı ve İş Planı

## 1. Mustafa Efe Kaya (Veritabanı, Çekirdek İş Kuralları ve Test Sorumlusu)
**Bugüne Kadar Yapılanlar:**
* Veritabanı yapısı (11 tablo) 3NF standartlarına göre tasarlandı ve ER diyagramı çizildi.
* Projenin çekirdek problemi olan "10 Dakikalık Koltuk Kilitleme" süreci için Sıralama (Sequence) diyagramı hazırlandı.
* İade oranları (%100, %50, %0) için Strategy tasarım deseni kurgulandı.
* Biniş ve iniş duraklarına göre etap çakışma mantığı (`a1 < b2` ve `b1 < a2`) teorik olarak planlandı.

**İlerleyen Haftalarda Yapılacaklar:**
* **Tabloların Kurulumu:** ER diyagramındaki 11 tablo için MySQL `schema.sql` dosyası yazılacak ve veritabanı ayağa kaldırılacak.
* **Çekirdek Algoritmalar:** `KoltukKilitService`, `EtapCakisma` ve `IadeHesaplayici` sınıfları kodlanacak.
* **Birim Testleri (Unit Tests):** Yazılan algoritmalar için PHPUnit kullanılarak sınır değer testleri yazılacak (Ör: 6 saat sınırında iade testi, tam çakışan durak testleri).
* **Veritabanı İşlemleri (Aşama 3):** `MySqlBiletRepository` ve `MySqlKoltukKilidiRepository` sınıfları PDO kullanılarak yazılacak.
* **Eşzamanlılık (Concurrency) Çözümleri:** Aynı koltuğa aynı milisaniyede tıklayan iki kişiyi engellemek için veritabanında `FOR UPDATE` (Transaction kilitleri) veya dosya kilidi algoritmaları kodlanacak.
* **Test Verisi (Seeder):** Sistemin test edilebilmesi için sahte şehirler, güzergahlar ve otobüs verileri üreten bir SQL/PHP betiği yazılacak.

## 2. Metehan Bayram (Proje Yöneticisi, Mimar ve Backend Sorumlusu)
**Bugüne Kadar Yapılanlar:**
* Projenin genel yazılım mimarisi (MVC ve Repository deseni) belirlendi ve Sınıf diyagramı oluşturuldu.
* GitHub ana deposu açıldı, README, klasör altyapıları ve Issue panosu kuruldu.
* Kullanıcı rolleri (yönetici/yolcu), güzergâh, otobüs ve sefer tablolarının sistemdeki entegrasyonu planlandı.

**İlerleyen Haftalarda Yapılacaklar:**
* **Altyapı (Routing):** Sistemin ana omurgası olan `Router.php` yazılarak gelen HTTP isteklerinin doğru Controller'lara yönlendirilmesi sağlanacak.
* **Kimlik Doğrulama (Auth):** `KimlikService` ile yönetici girişi yapılacak, şifreler hash'lenecek ve oturum (Session) güvenliği (Middleware) sağlanacak.
* **Yönetici Paneli (Backend):** Güzergâh, durak sırası, otobüs (2+1, 2+2) ve sefer ekleme/silme işlemleri (CRUD) için Controller ve Service katmanları kodlanacak.
* **Raporlama Algoritmaları:** Yönetici paneli için sefer bazlı doluluk oranlarını (%) ve toplam geliri (SUM) hesaplayan karmaşık SQL `JOIN` sorguları yazılacak.
* **Proje Yönetimi:** GitHub üzerinde açılan Pull Request'ler (PR) incelenecek, kod çakışmaları (Merge Conflict) çözülecek ve dal (Branch) yönetimi yapılacak.

## 3. Osman Nuri Gönültaş (Arayüz - Frontend, UX ve Müşteri Akışı Sorumlusu)
**Bugüne Kadar Yapılanlar:**
* Sistemdeki tüm yolcu ve yönetici akışlarını gösteren Sayfa Haritası çıkarıldı.
* Sefer arama, bilet alma ve PNR sorgulama ekranlarının taslakları (wireframe) oluşturuldu.
* TC Kimlik no maskeleme ve ödeme simülasyonunun kullanıcı deneyimi (UX) adımları belirlendi.

**İlerleyen Haftalarda Yapılacaklar:**
* **Arayüz Kodlama (HTML/CSS):** Wireframe taslakları, Bootstrap/Tailwind gibi araçlarla veya saf CSS ile mobil uyumlu (responsive) sayfalara dönüştürülecek.
* **Görsel Koltuk Haritası:** PHP ve JS kullanılarak 2+1/2+2 koltuk dizilimi ekrana basılacak; boş (beyaz), dolu (kırmızı) ve kilitli (sarı) koltuklar görselleştirilecek. Dinamik geri sayım sayacı eklenecek.
* **Form Validasyonları (JS):** Yolcu bilgileri girilirken boş alan kontrolü, TC Kimlik No format kontrolü ve sahte kredi kartı doğrulama işlemleri JavaScript ile istemci tarafında (Client-side) yapılacak.
* **Bildirimler:** "Koltuk 10 dk kilitlendi", "Karşı cinsiyet kuralı ihlali" gibi durumlarda kullanıcıya gösterilecek şık uyarı mesajları (Alert/Toast) tasarlanacak.
* **Bilet Dökümü:** Satın alma sonrası oluşturulan PNR numarası ve yolculuk detaylarının yer aldığı, yazıcıya uygun (Printable) bilet sonuç sayfası tasarlanacak.

## 4. Tüm Ekip (Ortak Görevler)
**Bugüne Kadar Yapılanlar:**
* GitHub Projects (İş Panosu) kuruldu, Etiketler (Labels) ve Dönüm Noktaları (Milestones) belirlendi.
* Tüm Fonksiyonel Gereksinimler (FR) Issue olarak açılıp panoya taşındı.
* Proje tasarım belgesi (`tasarim.md`) tamamlanarak Sunum 1 için eksiksiz hale getirildi.

**İlerleyen Haftalarda Yapılacaklar:**
* **Güvenlik Sıkılaştırması:** Tüm formlardan gelen veriler filtrelenecek, XSS saldırılarına karşı HTML kaçışları (`htmlspecialchars`) eklenecek ve POST işlemlerinde CSRF koruması sağlanacak.
* **Aşama 2 Teslimi:** Veritabanı olmadan verilerin JSON dosyalarına yazılıp okunduğu (Mock) ara sürüm tamamlanıp test edilecek.
* **Aşama 3 Teslimi:** JSON altyapısından tamamen MySQL veritabanına geçiş yapılacak.
* **Kalite Güvence (QA):** Sistemin baştan sona (End-to-End) testi yapılacak, bulunan açıklar ve hatalar (Bug) GitHub panosuna eklenip çözülecek.
* **Sunum Hazırlığı:** Her aşama sonunda hocalara yapılacak canlı demolar için sunum slaytları hazırlanacak ve sistemdeki örnek verilerle prova yapılacak.
