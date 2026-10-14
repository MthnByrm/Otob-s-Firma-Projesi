# Yapay Zekâ Günlüğü

Bu dosya, Otobüs Bilet Sistemi projesinin geliştirme ve tasarım sürecinde yapay zekâdan alınan destek kayıtlarını içerir. Kayıt tarihi, amaç, kullanılan istem (özet), sonuç, kabul edilen/değiştirilen/reddedilen kısım, nedeni ve yapay zekânın yanıldığı durumlar şeffaf bir şekilde kayıt altına alınmıştır.

---

### Kayıt 1 — FR-07 ve FR-08: Çekirdek problem ve etap bazlı doluluk kurallarının analizi
- **Tarih:** [11.10.2026]
- **Amaç:** Ara duraklı seferlerde (ör. İstanbul–Ankara–Sivas) koltuk doluluk algoritmasının ve 10 dakikalık geçici kilit kuralının sınır durumlarını kontrol etmek.
- **Kullanılan istem (özet):** Yazılan FR-07 ve FR-08 gereksinimlerine bakılarak ara durak çakışmalarında matematiksel kuralın nasıl kurulması gerektiği ve eksik bir senaryo olup olmadığı soruldu.
- **Sonuç:** Yapay zekâ, iki etabın çakışması için `a1 < b2` ve `b1 < a2` (biniş ve iniş durak sıraları) eşitsizliğinin kullanılması gerektiğini ve 10 dakikalık kilit süresinin ödeme adımında sunucu tarafında tekrar kontrol edilmesi gerektiğini önerdi.
- **Kabul edilen/reddedilen kısım:** Kabul edildi.
- **Nedeni:** İstanbul–Ankara arası dolu olan bir koltuğun Ankara–Sivas arasında tekrar satılabilmesi için durak sırası bazlı kontrol şarttı.
- **Yapay zekânın yanıldığı / atladığı durum:** Yok.

---

### Kayıt 2 — FR-09: Yan koltukta karşı cinsiyet kuralının netleştirilmesi
- **Tarih:** [11.10.2026]
- **Amaç:** FR-09'da yer alan yan yana koltuklarda karşı cinsiyet kısıtlamasının 2+1 ve 2+2 otobüs tiplerine göre nasıl işleyeceğini netleştirmek.
- **Kullanılan istem (özet):** Mevcut FR-09 açıklamasının hem 2+1 (tekli koltuklar hariç) hem de 2+2 otobüs düzenlerini ve kesişen etapları kapsayacak şekilde daha net ifade edilmesi istendi.
- **Sonuç:** Yapay zekâ, tekli koltukların bu kuraldan muaf tutulması ve yalnızca "aynı ikili koltuk grubunda, kesişen etaplarda aktif bilet veya kilit varsa" karşı cinsiyetin engellenmesi şeklinde açıklamayı revize etti.
- **Değiştirilen kısım:** FR-09 kabul kriterleri ve açıklaması önerilen ifadeye göre güncellendi.
- **Nedeni:** İlk ifadede 2+1 otobüslerdeki tekli koltukların durumu ve farklı etaplarda inen yolcuların durumu belirsizdi.
- **Yapay zekânın yanıldığı / atladığı durum:** Yok.

---

### Kayıt 3 — FR-14: İptal ve iade politikası için tasarım deseni seçimi
- **Tarih:** [12.10.2026]
- **Amaç:** Kalkışa kalan süreye göre (%100: >24 saat, %50: 6–24 saat, %0: <6 saat) değişen iade kurallarını `if-else` karmaşasından kurtarmak.
- **Kullanılan istem (özet):** FR-14 gereksinimindeki zaman dilimli iade hesaplaması için hangi tasarım deseninin (Design Pattern) en uygun olduğu soruldu.
- **Sonuç:** Yapay zekâ, `IadePolitikasiInterface` arayüzünü kullanan ve `TamIadePolitikasi`, `YarimIadePolitikasi`, `IadeYokPolitikasi` sınıflarından oluşan **Strategy Pattern** yapısını önerdi.
- **Kabul edilen/reddedilen kısım:** Kabul edildi.
- **Nedeni:** Open/Closed prensibine uyum sağlamak ve ileride yeni bir kampanya/kesinti kuralı eklendiğinde mevcut servis kodunu değiştirmemek için kabul edildi.
- **Yapay zekânın yanıldığı / atladığı durum:** Yapay zekâ ilk örnekte ücretleri `float` (ondalıklı sayı) olarak hesapladı. Yuvarlama hatası riski nedeniyle bu kısım reddedilip tüm tutarlar kuruş cinsinden `int` (tam sayı) olacak şekilde değiştirildi.

---

### Kayıt 4 — Mimari: Katman sorumlulukları ve Repository Pattern kurgusu
- **Tarih:** [13.10.2026]
- **Amaç:** Aşama 2'de JSON dosyalarıyla, Aşama 3'te ise MySQL (PDO) ile çalışacak 6 katmanlı mimari yapıyı (`Router -> Controller -> Service -> Repository -> Model -> View`) oluşturmak.
- **Kullanılan istem (özet):** Veritabanı değiştiğinde Service ve Controller sınıflarının hiç etkilenmemesi için klasör yapısının ve Repository arayüzlerinin nasıl tanımlanması gerektiği soruldu.
- **Sonuç:** `BiletRepositoryInterface`, `SeferRepositoryInterface` ve `KoltukKilidiRepositoryInterface` arayüzleri ile bunların `Json*` ve `MySql*` somut sınıfları tasarlandı.
- **Kabul edilen/reddedilen kısım:** Kabul edildi.
- **Nedeni:** Katmanlı mimari kurallarına tam uyum sağlanması ve Aşama 2'den Aşama 3'e geçiş maliyetinin sıfıra indirilmesi.
- **Yapay zekânın yanıldığı / atladığı durum:** Yok.

---

### Kayıt 5 — Sınıf Diyagramı: İlk taslağın oluşturulması ve eksik sınıfların tespiti
- **Tarih:** [14.10.2026]
- **Amaç:** GitHub değerlendirme kriterlerine uygun olarak en az 12 sınıf içeren Mermaid formatında UML sınıf diyagramı oluşturmak.
- **Kullanılan istem (özet):** Projedeki temel bilet ve koltuk kilitleme akışını gösteren Mermaid `classDiagram` kodu istendi.
- **Sonuç:** Yapay zekâ ilk aşamada yalnızca `KoltukController`, `KoltukKilitService`, `Bilet`, `Sefer` gibi temel sınıfları içeren **7 sınıflık** dar kapsamlı bir diyagram üretti.
- **Kabul edilen/reddedilen kısım:** Reddedildi ve yeniden istendi.
- **Nedeni:** Sunum ve ödev kriterlerinde "en az 12 sınıf" şartı bulunması ve ilk diyagramda Strategy deseni sınıfları ile somut Repository sınıflarının (`JsonBiletRepository`, `MySqlBiletRepository`) eksik olması.
- **Yapay zekânın yanıldığı / atladığı durum:** Yapay zekâ diyagramı basitleştirmek adına yalnızca çekirdek varlıkları çizip diğer katman ve desen sınıflarını atlamıştı.

---

### Kayıt 6 — Sınıf Diyagramı: Katman yönetimi ve tam mimariye genişletilmesi
- **Tarih:** [14.10.2026]
- **Amaç:** İlk sınıf diyagramındaki eksikleri gidererek Controller, Service, Strategy, Repository arayüzleri, somut Repository sınıfları ve yardımcı algoritmaları (`EtapCakisma`, `KoltukPlani`) içeren tam diyagramı elde etmek.
- **Kullanılan istem (özet):** Diyagramda Controller'ların doğrudan Repository'ye erişmemesi gerektiği, Strategy ve Repository desenlerinin somut sınıflarıyla birlikte gösterilmesi ve en az 18-20 sınıflı tam mimarinin çizilmesi istendi.
- **Sonuç:** `SeferAramaController`, `KoltukController`, `SatinAlmaController`, `BiletController`, ilgili tüm Service sınıfları, `IadeHesaplayici`, Repository arayüzleri ve `Json/MySql` gerçeklemelerini içeren kapsamlı sınıf diyagramı üretildi.
- **Değiştirilen kısım:** 7 sınıflı ilk taslak tamamen kaldırılarak tüm mimari katmanları kapsayan yeni diyagram `docs/tasarim.md` belgesine eklendi.
- **Nedeni:** "Controller yalnızca Service'i çağırır" kuralını görsel olarak kanıtlamak ve sunumda katman atlama eleştirisi almamak.
- **Yapay zekânın yanıldığı / atladığı durum:** İlk taslaktaki eksik sınıf sayısı problemi bu yönlendirmeyle giderildi.

---

### Kayıt 7 — Sıralama Diyagramı: Koltuk kilitleme ve hata dallarının (`alt/else`) kurgulanması
- **Tarih:** [14.10.2026]
- **Amaç:** Çekirdek problem olan "Koltuk seçimi, etap çakışması, karşı cinsiyet kontrolü ve 10 dk geçici kilit" akışını hata durumlarıyla birlikte sıralama diyagramına (Sequence Diagram) aktarmak.
- **Kullanılan istem (özet):** Soldan sağa `Tarayıcı -> KoltukController -> KoltukKilitService -> Repository` katmanlarını içeren, SQL barındırmayan ve çakışma/kural ihlali durumlarını `alt / else` bloklarıyla gösteren Mermaid `sequenceDiagram` kodu istendi.
- **Sonuç:** `POST /koltuk/kilitle` isteğiyle başlayan; girdi doğrulaması (422), süresi dolan kilitlerin silinmesi, etap çakışması (409 Conflict), yan koltuk cinsiyet kuralı ihlali (409 Conflict) ve başarılı 10 dakikalık kilit kaydını (200 OK) gösteren diyagram oluşturuldu.
- **Kabul edilen/reddedilen kısım:** Kısmen kabul edildi; süresi dolan kilitlerin temizlenmesi adımı eklendi.
- **Nedeni:** Aktif kilitler sorgulanmadan hemen önce `suresiDolanlariSil()` metodunun çağrılması, 10 dakikası dolmuş koltukların boş yere kilitli kalmasını önlemek için diyagrama dahil edildi.
- **Yapay zekânın yanıldığı / atladığı durum:** Yapay zekâ ilk taslakta süresi dolmuş kilitleri temizleme adımını atlayıp doğrudan aktif kilitleri sorgulamıştı; müdahale edilerek `suresiDolanlariSil` çağrısı eklendi.

---

### Kayıt 8 — ER Diyagramı ve 3NF Normalizasyon Gerekçesi
- **Tarih:** [14.10.2026]
- **Amaç:** Projedeki 11 veritabanı tablosunu 1:N ilişkileriyle göstermek ve 3NF kurallarına uygunluğunu kanıtlamak.
- **Kullanılan istem (özet):** Kullanıcı, sipariş, bilet, yolcu, sefer, otobüs, güzergâh durakları ve geçici koltuk kilitlerini içeren 11 tablonun ilişkisel şeması ve 1NF, 2NF, 3NF açıklamaları istendi.
- **Sonuç:** Tablolar arası PK/FK/UK yapılarını gösteren diyagram ve normalizasyon tablosu üretildi. `BILETLER` tablosundaki `ucret` alanının 3NF ihlali değil, "satış anı tarihsel kaydı" olduğu açıklandı.
- **Kabul edilen/reddedilen kısım:** Kabul edildi; görsel okunabilirlik için gruplandırılmış yerleşim tercih edildi.
- **Nedeni:** İleride güzergâh fiyatı zamlandığında geçmişte satılmış biletin fiyatının değişmemesi gerektiği için tarihsel kayıt istisnası özellikle belgelendi.
- **Yapay zekânın yanıldığı / atladığı durum:** Yok.

---

### Kayıt 9 — İş Planı ve Görev Dağılımının Yapılandırılması
- **Tarih:** [15.10.2026]
- **Amaç:** 3 kişilik proje ekibinin (Metehan Bayram, Mustafa Efe Kaya, Osman Nuri Gönültaş) şimdiye kadar yaptığı tasarım işlerini ve ilerleyen haftalarda yapacağı kodlama/test görevlerini belgelemek.
- **Kullanılan istem (özet):** FR bazlı tablo yerine kişi bazlı olarak "Bugüne Kadar Yapılanlar" ve "İlerleyen Haftalarda Yapılacaklar" başlıklarıyla detaylı iş planı hazırlanması istendi.
- **Sonuç:** İlk denemede yapay zekâ FR bazlı tek bir tablo üretti. İkinci yönlendirme ile kişi bazlı ve tüm geliştirme/test/güvenlik süreçlerini kapsayan detaylı iş planı oluşturuldu.
- **Değiştirilen kısım:** FR tablosu formatı reddedilip kişi ve zaman odaklı (`Bugüne Kadar Yapılanlar` / `İlerleyen Haftalarda Yapılacaklar`) formatına geçildi.
- **Nedeni:** Sunumda her grup üyesinin kendi sorumluluk alanını ve gelecek haftalardaki iş yükünü net bir şekilde gösterebilmesi.
- **Yapay zekânın yanıldığı / atladığı durum:** Yapay zekâ ilk istemde kullanıcının "kişi bazlı geçmiş ve gelecek işler" talebini yanlış anlayıp yalnızca FR tablosu üretmişti; geri bildirimle düzeltildi.
