# Görev Dağılımı ve İş Planı

Bu doküman, Otobüs Bilet Satış Sistemi projesinde görev alan ekip üyelerinin sorumluluk alanlarını, Aşama 1 (Tasarım) kapsamında tamamlanan çalışmaları ve Aşama 2 / Aşama 3 (Kodlama ve Test) süreçlerinde yapılacak işleri içerir.

---

## 1. Mustafa Efe Kaya
**Rol:** Veritabanı, Çekirdek İş Kuralları ve Test

### Tamamlanan Çalışmalar (Aşama 1)
- Sistemdeki 11 tablonun 3NF kurallarına uygun olarak tasarlanması ve ER diyagramının çizilmesi.
- 10 dakikalık geçici koltuk kilitleme ve doğrulama akışı için Sıralama (Sequence) diyagramının oluşturulması.
- Bilet iptal ve iade kuralları için `IadePolitikasiInterface` tabanlı Strategy tasarım deseninin kurgulanması.
- Ara duraklı seferlerde biniş ve iniş sıralarına göre etap çakışma mantığının (`a1 < b2` ve `b1 < a2`) belirlenmesi.

### Planlanan Çalışmalar (Aşama 2 ve 3)
- `database/schema.sql` ve örnek veri (`seed.sql`) dosyalarının hazırlanarak MySQL tablolarının oluşturulması.
- `KoltukKilitService`, `EtapCakisma`, `KoltukPlani` ve `IadeHesaplayici` sınıflarının kodlanması (`FR-07`, `FR-08`, `FR-09`, `FR-14`).
- Etap çakışması, yan koltuk cinsiyet kontrolü ve saat bazlı iade kesintileri (%100, %50, %0) için PHPUnit birim testlerinin yazılması.
- Aşama 2'de JSON, Aşama 3'te PDO tabanlı `BiletRepository` ve `KoltukKilidiRepository` sınıflarının yazılması.
- Eşzamanlı koltuk seçimlerinde çift satışı önlemek için dosya kilidi (`flock`) ve veritabanı transaction (`FOR UPDATE`) yapısının kurulması.

---

## 2. Metehan Bayram
**Rol:** Proje Yönetimi, Mimari ve Yönetim Paneli (Backend)

### Tamamlanan Çalışmalar (Aşama 1)
- Katmanlı mimari yapısının (`Router -> Controller -> Service -> Repository -> Model -> View`) ve klasör ağacının oluşturulması.
- Sistemin genel UML Sınıf Diyagramının hazırlanması ve Repository tasarım deseninin kurgulanması.
- GitHub deposunun açılması, dal (branch) kurallarının ve proje panosunun oluşturulması.
- Yönetici ve yolcu rolleri ile sefer, otobüs ve güzergâh tablolarının ilişkilerinin tanımlanması.

### Planlanan Çalışmalar (Aşama 2 ve 3)
- Gelen HTTP isteklerini ilgili Controller sınıflarına yönlendiren `Router` yapısının ve `bootstrap.php` dosyasının kodlanması.
- `KimlikService` ile kullanıcı kayıt, giriş, çıkış ve rol bazlı yetkilendirme (`password_hash` ve session) işlemlerinin yapılması (`FR-01`).
- Yönetici paneli üzerinden güzergâh/durak (`FR-02`), otobüs tipi 2+1 / 2+2 (`FR-03`) ve sefer (`FR-04`) ekleme-güncelleme işlemlerinin kodlanması.
- Sefer bazlı doluluk oranı ve toplam gelir verilerini hesaplayan `RaporService` ve ilgili SQL sorgularının yazılması (`FR-15`).
- Açılan Pull Request'lerin (PR) incelenmesi ve ana dala (`main`) birleştirilmesi.

---

## 3. Osman Nuri Gönültaş
**Rol:** Arayüz (Frontend), Kullanıcı Akışı ve Bilet İşlemleri

### Tamamlanan Çalışmalar (Aşama 1)
- Yolcu ve yönetici ekran geçişlerini gösteren Sayfa Haritasının çıkarılması.
- Sefer arama, koltuk seçimi, ödeme ve bilet sorgulama ekranları için taslakların (wireframe) hazırlanması.
- TC Kimlik No maskeleme ve PNR sorgulama adımlarının arayüz kurgusunun yapılması.

### Planlanan Çalışmalar (Aşama 2 ve 3)
- Sefer arama ekranının ve ara durakları kapsayan listeleme sayfasının (`SeferAramaController` / View) kodlanması (`FR-05`).
- 2+1 ve 2+2 otobüs düzenine göre boş, dolu ve kilitli koltukları renklendirerek gösteren görsel koltuk haritasının ve 10 dakikalık geri sayım sayacının yapılması (`FR-06`).
- Yolcu bilgisi formunun, 11 haneli TC Kimlik doğrulama sınıfının (`TcKimlikDogrulayici`) ve tek işlemde en fazla 5 koltuk sınırının kodlanması (`FR-10`).
- Simüle kredi kartı ödeme ekranının ve benzersiz PNR kodu üretiminin (`PnrUretici`) tamamlanması (`FR-11`).
- PNR ve TC Kimlik No ile bilet sorgulama (`FR-13`) ve yazdırılabilir bilet detayı (`FR-12`) sayfalarının hazırlanması.

---

## 4. Ortak Görevler (Tüm Ekip)

### Tamamlanan Çalışmalar (Aşama 1)
- Fonksiyonel gereksinimlerin (FR-01 – FR-16) GitHub Issues üzerinde tanımlanması, etiketlenmesi ve proje panosuna (Backlog) aktarılması.
- Mimari, UML diyagramları ve risk analizini içeren `docs/tasarim.md` belgesinin hazırlanması.
- Tasarım sürecinde alınan yapay zekâ desteklerinin `docs/yapay_zeka_gunlugu.md` dosyasına kaydedilmesi.

### Planlanan Çalışmalar (Aşama 2 ve 3)
- Form işlemlerinde CSRF token kontrolünün, View katmanında XSS kaçışlarının (`htmlspecialchars`) ve PDO'da parametreli sorguların uygulanması (`FR-16`).
- Aşama 2'de tüm sistemin JSON veri depolama katmanıyla uçtan uca test edilmesi.
- Aşama 3'te MySQL veritabanına geçişin tamamlanması ve sınır durum testlerinin yapılması.
- Sunum 2 ve Sunum 3 öncesi canlı demo senaryolarının test edilmesi.
