# Otobüs Firması Bilet Satış Sitesi

## Tasarım Belgesi



## Proje Özeti

Sistem, bir otobüs firmasının sefer, otobüs ve güzergâh yönetimini yapmasını; yolcuların ise bilet almasını sağlar. Yolcu **nereden / nereye / tarih** seçerek arama yapar, **koltuk haritasından** koltuğunu kilitler, yolcu bilgilerini girer ve simüle ödeme ile **PNR kodlu bilet** alır. PNR ile bilet sorgulanabilir, iptal ve iade işlemleri yapılabilir.

### Çekirdek Problemler

Bu kurallar sunucu tarafında denetlenir.

| # | Problem                          | Kural / Beklenen davranış                                                                                                                     |
| ---: | -------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 | Koltuk çakışması ve geçici kilit | Seçilen koltuk 10 dakika boyunca kilitlenir. Süre dolunca kilit kaldırılır.                                                                   |
| 2 | Duraklara göre segment doluluğu  | Ara duraklı bir seferde (ör. İstanbul–Ankara–Sivas), aynı koltuk İstanbul–Ankara arasında doluyken Ankara–Sivas arasında yeniden satılabilir. |
| 3 | Karşı cinsiyet kuralı            | Aynı ikili grupta, kesişen etaplarda yan yana koltuklar için karşı cinsiyetten yolcu seçimi engellenir.                                       |
| 4 | İptal ve iade politikası         | Kalkışa kalan süreye göre iade uygulanır: **24 saatten fazla %100**, **6–24 saat %50**, **6 saatten az %0**.                                  |

---

## 1. Mimari

İstek akışı aşağıdaki gibidir. Her katman yalnızca kendisinin altındaki katmanı çağırır.

```mermaid
flowchart LR
    B[Tarayıcı] --> I[public/index.php]
    I --> R[Router]
    R --> C[Controller]
    C --> S[Service]
    S --> P[Repository arayüzü]
    P --> D[(JSON — Aşama 2 / MySQL — Aşama 3)]
    C --> V[View]
```

### 1.1 Katmanlar ve Sorumluluklar

| Katman         | Görevi                                                                         | Yapmaması gerekenler                       | Sınıf örnekleri                                                                                                                                                                                                                                 |
| -------------- | ------------------------------------------------------------------------------ | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Router**     | Adrese göre ilgili Controller metodunu seçer.                                  | İş kuralı içermez.                         | `POST /koltuk/kilitle` → `KoltukController::kilitle`                                                                                                                                                                                            |
| **Controller** | `$_POST` / `$_GET` verisini alır, servisi çağırır, View seçer veya JSON döner. | SQL yazmaz, hesap yapmaz.                  | `KimlikController`, `SeferAramaController`, `KoltukController`, `SatinAlmaController`, `BiletController`, `YonetimGuzergahController`, `YonetimOtobusController`, `YonetimSeferController`, `YonetimRaporController`                            |
| **Service**    | İş kurallarını ve çekirdek algoritmaları yürütür.                              | `$_POST` okumaz, HTML üretmez, SQL yazmaz. | `KoltukKilitService`, `KoltukHaritasiService`, `SatinAlmaService`, `BiletService`, `IadeHesaplayici`, `SeferAramaService`, `SeferService`, `GuzergahService`, `OtobusService`, `KimlikService`, `RaporService`                                  |
| **Repository** | Veriyi okur ve yazar.                                                          | İş kuralı içermez.                         | `KullaniciRepositoryInterface`, `GuzergahRepositoryInterface`, `OtobusRepositoryInterface`, `SeferRepositoryInterface`, `YolcuRepositoryInterface`, `SiparisRepositoryInterface`, `BiletRepositoryInterface`, `KoltukKilidiRepositoryInterface` |
| **Model**      | Veriyi taşıyan sade nesnelerdir.                                               | Veritabanı sorgusu içermez.                | `Kullanici`, `Guzergah`, `Otobus`, `Sefer`, `Yolcu`, `Siparis`, `Bilet`, `KoltukKilidi`                                                                                                                                                         |
| **View**       | HTML görüntüler ve veriyi escape eder.                                         | İş kuralı ve veri çekme içermez.           | `views/seats/map.php`, `views/seferler/list.php`, `views/biletler/show.php`                                                                                                                                                                     |

### 1.2 Klasör Yapısı

```text
otobus-bilet/
├── bootstrap.php
├── composer.json
├── phpunit.xml
├── public/
│   ├── index.php
│   └── assets/
├── src/
│   ├── Controller/
│   ├── Service/
│   │   └── Iade/
│   ├── Repository/
│   │   ├── Json/
│   │   └── MySql/
│   ├── Model/
│   └── Support/
├── views/
├── database/
│   ├── schema.sql
│   └── seed.sql
├── storage/
├── tests/
└── docs/
    ├── tasarim.md
    └── wireframes/
```

> **Veritabanı değişim kontrolü:** Veritabanı veya depolama türü değiştiğinde yalnızca `src/Repository/` altındaki somut sınıflar ile `bootstrap.php` güncellenir. Controller, Service ve Model katmanlarına dokunulmaz.

---

## 2. Sınıf Diyagramı

Mimari kurallara uygun olarak Controller’lar Repository’lere doğrudan erişmez; Service katmanı üzerinden haberleşir.

```mermaid
classDiagram
    class SeferAramaController {
        +aramaFormu() void
        +sonuclar() void
    }
    class KoltukController {
        +harita() void
        +durum() void
        +kilitle() void
        +birak() void
    }
    class SatinAlmaController {
        +yolcuFormu() void
        +ode() void
    }
    class BiletController {
        +sorguFormu() void
        +goster() void
        +iptalEt() void
        +yazdir() void
    }
    class YonetimRaporController {
        +doluluk() void
    }

    class SeferAramaService {
        +ara(int neredenId, int nereyeId, DateTimeImmutable tarih) array
    }
    class KoltukKilitService {
        +kilitle(int kullaniciId, int seferId, int koltukNo, int binisId, int inisId, Cinsiyet cinsiyet) KoltukKilidi
        +birak(int kullaniciId, int seferId, int koltukNo) void
    }
    class SatinAlmaService {
        +satinAl(int kullaniciId, int seferId, array koltukYolculari, string kartNo) Siparis
    }
    class BiletService {
        +sorgula(string pnr, string tc) Bilet
        +iptalEt(Bilet bilet) IadeTeklifi
    }
    class IadeHesaplayici {
        +hesapla(int ucretKurus, int kalanDakika) IadeTeklifi
    }

    class SeferRepositoryInterface {
        <<interface>>
        +idIleBul(int id) Sefer
        +ara(int neredenId, int nereyeId, DateTimeImmutable tarih) array
    }
    class BiletRepositoryInterface {
        <<interface>>
        +kaydet(Bilet bilet) Bilet
        +pnrIleBul(string pnr) Bilet
        +seferdekiAktifler(int seferId) array
    }
    class KoltukKilidiRepositoryInterface {
        <<interface>>
        +kaydet(KoltukKilidi kilit) KoltukKilidi
        +seferdekiAktifler(int seferId, DateTimeImmutable simdi) array
        +suresiDolanlariSil(int seferId, DateTimeImmutable simdi) void
    }

    class JsonBiletRepository {
        -string dosya
    }
    class MySqlBiletRepository {
        -PDO pdo
    }

    class Sefer {
        +int id
        +int guzergahId
        +int otobusId
        +DateTimeImmutable kalkisZamani
    }
    class Bilet {
        +int id
        +string pnr
        +int seferId
        +int koltukNo
        +int binisDurakId
        +int inisDurakId
        +int ucretKurus
    }
    class KoltukKilidi {
        +int id
        +int seferId
        +int koltukNo
        +DateTimeImmutable bitisZamani
    }

    SeferAramaController ..> SeferAramaService
    KoltukController ..> KoltukKilitService
    SatinAlmaController ..> SatinAlmaService
    BiletController ..> BiletService

    SeferAramaService --> SeferRepositoryInterface
    KoltukKilitService --> SeferRepositoryInterface
    KoltukKilitService --> BiletRepositoryInterface
    KoltukKilitService --> KoltukKilidiRepositoryInterface
    SatinAlmaService --> BiletRepositoryInterface
    SatinAlmaService --> KoltukKilidiRepositoryInterface
    BiletService --> BiletRepositoryInterface
    BiletService --> IadeHesaplayici

    JsonBiletRepository ..|> BiletRepositoryInterface
    MySqlBiletRepository ..|> BiletRepositoryInterface
    SeferRepositoryInterface ..> Sefer
    BiletRepositoryInterface ..> Bilet
    KoltukKilidiRepositoryInterface ..> KoltukKilidi
```

---

## 3. Sıralama Diyagramı — Koltuk Kilitleme Akışı

Çekirdek problemin kilit adımındaki katmanlar arası çağrı sırası ve hata durumları:

```mermaid
sequenceDiagram
    participant T as Tarayıcı
    participant C as KoltukController
    participant S as KoltukKilitService
    participant R as Repository (Kilit ve Bilet)

    T->>C: POST /koltuk/kilitle (seferId, koltukNo, binisId, inisId, cinsiyet)
    C->>S: kilitle(kullaniciId, seferId, koltukNo, binisId, inisId, cinsiyet)
    S->>S: Girdi doğrulaması (koltuk sınırı, durak sırası)

    alt Veri veya istek geçersiz
        S-->>C: DogrulamaHatasi
        C-->>T: 422 Unprocessable Entity (JSON hata)
    else İstek geçerli
        S->>R: suresiDolanlariSil(seferId, simdi)
        S->>R: seferdekiAktifler(seferId)
        S->>S: Etap çakışması veya mevcut kilit kontrolü (EtapCakisma)

        alt Koltuk dolu veya kilitli
            S-->>C: KoltukMusaitDegilHatasi
            C-->>T: 409 Conflict (koltuk müsait değil)
        else Koltuk boş
            S->>S: Yan koltukta karşı cinsiyet kontrolü (KoltukPlani)
            alt Cinsiyet kuralı ihlali
                S-->>C: CinsiyetKuraliHatasi
                C-->>T: 409 Conflict (karşı cinsiyet kuralı)
            else Kural uygun
                S->>R: kaydet(koltukKilidi) (10 dakika geçerli)
                S-->>C: KoltukKilidi nesnesi
                C-->>T: 200 OK (kilit başarılı, kalan süre)
            end
        end
    end
```

---

## 4. ER Diyagramı ve Normalizasyon

### 4.1 ER Diyagramı

```mermaid
flowchart TB
    %% Katmanlı yerleşim: tablolar ayrı tutulur, ilişkiler 1:N olarak etiketlenir.
    subgraph A["Kullanıcı ve satış"]
      direction LR
      K["KULLANICILAR<br/>PK id<br/>UK eposta<br/>parola_ozeti, rol"]
      S["SIPARISLER<br/>PK id<br/>FK kullanici_id<br/>odeme_zamani"]
      B["BILETLER<br/>PK id · UK pnr<br/>FK siparis_id, sefer_id<br/>FK yolcu_id<br/>koltuk_no, ucret, durum"]
      Y["YOLCULAR<br/>PK id · UK tc_no<br/>ad_soyad, cinsiyet"]
      K -->|"1 : N"| S
      S -->|"1 : N"| B
      Y -->|"1 : N"| B
    end

    subgraph C["Sefer ve araç"]
      direction LR
      OT["OTOBUS_TIPLERI<br/>PK id · UK kod<br/>sira_sayisi"]
      O["OTOBUSLER<br/>PK id · UK plaka<br/>FK otobus_tipi_id"]
      SF["SEFERLER<br/>PK id<br/>FK guzergah_id, otobus_id<br/>kalkis_zamani"]
      G["GUZERGAHLAR<br/>PK id<br/>ad"]
      OT -->|"1 : N"| O
      O -->|"1 : N"| SF
      G -->|"1 : N"| SF
    end

    subgraph D["Güzergâh durakları"]
      direction LR
      SE["SEHIRLER<br/>PK id · UK ad"]
      GD["GUZERGAH_DURAKLARI<br/>PK id<br/>FK guzergah_id, sehir_id<br/>durak_sirasi, baslangictan_ucret"]
      G -->|"1 : N"| GD
      SE -->|"1 : N"| GD
    end

    subgraph E["Geçici koltuk kilitleri"]
      direction LR
      KL["KOLTUK_KILITLERI<br/>PK id<br/>FK sefer_id, kullanici_id<br/>FK binis_durak_id, inis_durak_id<br/>koltuk_no, bitis_zamani"]
      SF -->|"1 : N"| KL
      K -->|"1 : N"| KL
      GD -->|"1 : N · biniş"| KL
      GD -->|"1 : N · iniş"| KL
    end

    GD -->|"1 : N · biniş"| B
    GD -->|"1 : N · iniş"| B

    classDef user fill:#eaf2ff,stroke:#4776b8,color:#162b49,stroke-width:1.5px;
    classDef sale fill:#e8f7ef,stroke:#3c9270,color:#173d31,stroke-width:1.5px;
    classDef route fill:#fff3df,stroke:#c58b36,color:#573b12,stroke-width:1.5px;
    classDef lock fill:#f5eaff,stroke:#8c62b0,color:#38214e,stroke-width:1.5px;
    class K,S,B,Y user;
    class OT,O,SF sale;
    class G,SE,GD route;
    class KL lock;
```

> **İlişki okuma anahtarı:** `||` tek kaydı, `o{` sıfır veya daha çok kaydı, `|{` bir veya daha çok kaydı belirtir. Örneğin `KULLANICILAR ||--o{ SIPARISLER`, **bir kullanıcının sıfır veya çok sayıda siparişi** olabileceği anlamına gelir (1:N).

### 4.2 3NF Normalizasyon Gerekçesi

| Normalizasyon | Gerekçe                                                                                                                                                                |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1NF**       | Tüm hücreler atomiktir. Biletler tekil satırlarda tutulur; dizi veya virgülle ayrılmış metin saklanmaz.                                                                |
| **2NF**       | Her sütun birincil anahtara (PK) tam bağımlıdır. Birleşik doğal anahtarlar yerine yapay `id` birincil anahtar olarak seçilmiştir.                                      |
| **3NF**       | Anahtar olmayan sütunlar arasında geçişli bağımlılık yoktur. Yolcu adı ve cinsiyeti `YOLCULAR` tablosundadır; otobüs kapasitesi `OTOBUS_TIPLERI` üzerinden belirlenir. |

> **Satış anı kaydı istisnası:** `BILETLER.ucret` sütunu 3NF ihlali değildir; biletin satıldığı andaki maliyeti kaydeder. Güzergâh fiyatı ileride değişse bile eski bilet tutarı korunur.

---

## 5. Tasarım Desenleri

### 5.1 Repository Pattern 

| Başlık         | Açıklama                                                                                                                  |
| -------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Problem**    | Veri erişiminin depolama türünden (JSON veya MySQL) bağımsız olması gerekir.                                              |
| **Uygulama**   | `BiletRepositoryInterface`, `SeferRepositoryInterface` vb. arayüzler ve bunların `Json*` / `MySql*` uygulamaları yazılır. |
| **Alternatif** | Servisler içinde doğrudan SQL veya `file_get_contents` kullanmak.                                                         |
| **Neden?**     | Veritabanı değiştiğinde yalnızca Repository sınıfları değişir; iş kuralları etkilenmez.                                   |

### 5.2 Strategy Pattern (İade Politikası)

| Başlık         | Açıklama                                                                                                                     |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **Problem**    | Kalkışa kalan süreye göre %100, %50 veya %0 iade hesaplanır; kurallar değişebilir.                                           |
| **Uygulama**   | `IadePolitikasiInterface` arayüzünden `TamIadePolitikasi`, `YarimIadePolitikasi` ve `IadeYokPolitikasi` sınıfları türetilir. |
| **Alternatif** | `if / elseif / else` blokları kullanmak.                                                                                     |
| **Neden?**     | Yeni bir iade kuralı, mevcut koda dokunmadan yeni bir politika sınıfı eklenerek uygulanabilir (Open/Closed Principle).       |

#### Strategy sınıf diyagramı

```mermaid
flowchart TB
    G["IadeHesaplayici<br/>hesapla(ucret, kalanDakika)"]
    I["IadePolitikasiInterface"]
    T["TamIadePolitikasi<br/>24 saatten fazla<br/>%100 iade"]
    Y["YarimIadePolitikasi<br/>6–24 saat<br/>%50 iade"]
    N["IadeYokPolitikasi<br/>6 saatten az<br/>%0 iade"]
    R["IadeTeklifi<br/>iade tutarı + oran"]

    G -->|"uygun politikayı seçer"| I
    I -. "uygulanır" .-> T
    I -. "uygulanır" .-> Y
    I -. "uygulanır" .-> N
    T --> R
    Y --> R
    N --> R

    classDef context fill:#eaf2ff,stroke:#4776b8,color:#162b49,stroke-width:1.5px;
    classDef strategy fill:#e8f7ef,stroke:#3c9270,color:#173d31,stroke-width:1.5px;
    classDef result fill:#fff3df,stroke:#c58b36,color:#573b12,stroke-width:1.5px;
    class G,I context;
    class T,Y,N strategy;
    class R result;
```

---

## 6. Arayüz Tasarımı

### 6.1 Sayfa Haritası

```mermaid
flowchart LR
    A[Ana Sayfa: Sefer Ara] --> B[Sefer Listesi]
    B --> C[Koltuk Haritası — çekirdek ekran]
    C --> D[Yolcu Bilgisi ve Ödeme]
    D --> E[Bilet Detayı / Yazdır]
    A --> F[Bilet Sorgula / İptal]

    subgraph Admin[Yönetici Paneli]
        P[Admin Dashboard] --> R1[Güzergâh Yönetimi]
        P --> R2[Otobüs Yönetimi]
        P --> R3[Sefer Yönetimi]
        P --> R4[Raporlar]
    end
```


## 7. Risk Yönetimi

| # | Risk                                                      | Olasılık | Etki   | Önlem                                                                                                              |
| ---: | --------------------------------------------------------- | -------- | ------ | ------------------------------------------------------------------------------------------------------------------ |
| 1 | Eşzamanlılık nedeniyle aynı koltuğun iki kişiye satılması | Orta     | Yüksek | Satır kilitleme (`FOR UPDATE` / `flock`) ve kilit adımlarının sunucu tarafında yeniden doğrulanması                |
| 2 | Ara duraklı satışlarda etap çakışma hatası                | Orta     | Yüksek | `EtapCakisma` sınıfı için sınır durumlarını kapsayan PHPUnit birim testleri yazılması                              |
| 3 | Ödeme sırasında 10 dakikalık kilit süresinin dolması      | Orta     | Orta   | Satın alma adımında sürenin sunucuda yeniden kontrol edilmesi; süre dolduysa kullanıcının haritaya yönlendirilmesi |
| 4 | Yan yana koltuklarda cinsiyet kuralının hatalı çalışması  | Orta     | Orta   | `KoltukPlani` yardımcı sınıfının 2+1 ve 2+2 düzenleri için ayrı birim testlerle doğrulanması                       |
| 5 | TC Kimlik No doğrulama algoritması hatası                 | Düşük    | Orta   | Resmî kuralları ve mod10 / mod11 kontrollerini kapsayan PHPUnit testleri yazılması                                 |
| 6 | Kapsamın büyümesi ve sürenin aşılması                     | Yüksek   | Orta   | SRS dışındaki özelliklerin (gerçek ödeme, SMS gönderimi) kesin olarak kapsam dışında tutulması                     |

---

