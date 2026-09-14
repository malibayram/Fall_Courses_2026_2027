# SE 237 Object Oriented Programming — 2. Hafta Öğretim Dosyası

## Haftanın kimliği

| Alan | Plan |
| --- | --- |
| Tema | Fabrikanın nesne yapısı: sorumluluk, invariant ve ilişkiler |
| Ana soru | 100 sandalyelik ihtiyaç hesabını hangi nesneler üretmeli; hangi kural kimin elinde korunmalı? |
| Süre | 155 dakika: 125 dakika etkin öğrenme + üç adet 10 dakikalık ara |
| Başlangıç | W1 `MaterialPlanner` prototipi veya kaynağı belirtilmiş eğitmen tabanı |
| Haftanın ürünü | Ürün–reçete–stok servislerinin bağlandığı çalışan iskelet, bir domain testi, tarayıcı smoke testi, A1–A10 haritası |
| Sonraki kapı | W3'te sipariş planından rezervasyon talebine giden dikey dilim |
| Belge durumu | Öğretim ve görev tasarımı; adı geçen starter, web iskeleti ve fixture'lar ders paketinde ayrıca üretilecektir |

**Bağlantılar:** [Ana ders sistemi](../README.md) · [Güncel kapsam](GUNCEL_KAPSAM.md) · [ERP öğrenci rehberi](ERP_OGRENCI_REHBERI.md) · [W1](HAFTA01.md) · [W3](HAFTA03.md) · [W4](HAFTA04.md) · [Kaynakça](KAYNAKCA.md)

Bu Türkçe dosya eğitmen ve asistanlar içindir. Sondaki İngilizce öğrenci görev özeti doğrudan paketlenebilir; ölçülen kavramların İngilizce notu, hazırlık soruları ve rubrik açıklaması da yayımlanır. Güncel proje sözleşmesi [ERP rehberidir](ERP_OGRENCI_REHBERI.md); [eski Course Registration planı](PROJE.md) yalnız tarihsel referanstır ve öğrenciye yürürlükteki görev olarak verilmez.

## 1. Öğretim amacı ve doğru derinlik

W1'de fabrikanın bütün süreç haritasını ve küçük bir malzeme planı prototipini gördük. Bu hafta aynı bütünü **yapı açısından** kuruyoruz: hangi bilgi hangi nesnede durur, hangi kural nerede korunur, hangi nesne hangi nesneye bağımlıdır?

**Bu hafta A1–A3 ayrıntılı işlenir:** A1 nesne sorumluluğu ve ayrıştırma, A2 encapsulation ve invariant, A3 ilişkiler/sahiplik/UML. A4–A10 bu hafta yeniden anlatılmaz; haritada "bekleyen" olarak işaretlenir ve W3–W4'te ele alınır. Kalıtım hiyerarşisi, interface portları, değer nesnesi eşitliği, generics, kalıcılık ve plugin bu haftanın konusu değildir.

### Ölçülebilir kazanımlar

| Kod | Öğrenci ders sonunda… | Kanıt |
| --- | --- | --- |
| W2-K1 | Plan hesabı, stok durumu ve reçete tanımını ayrı sorumluluklara yerleştirir. | Sınıf başına “ne bilir / ne yapar / ne zaman değişir” kartı |
| W2-K2 | W1 hesabını verilen web/CLI iskeletine bağlar ve davranışını korur. | W1 testlerinin geçmesi; sayfada görünen plan sonucu |
| W2-K3 | `plan(...)` çağrısının stok ve rezervasyonu değiştirmediğini gösterir. | Çağrı öncesi/sonrası aynı `InventorySnapshot` testi |
| W2-K4 | Yayınlanmış BOM revizyonunun dışarıdan değiştirilemediğini gösterir. | Koleksiyon/nesne sızıntısı karşı örneği ve düzeltmesi |
| W2-K5 | Sahiplik (composition) ile kullanım (association) ilişkisini ayırır. | Kodla uyumlu sınıf/nesne diyagramı |
| W2-K6 | `tenant_id`'nin istekten domain'e kadar izlediği yolu gösterir. | F-A/F-B ayrımının tek bir çağrıdaki izi |
| W2-K7 | Henüz uygulanmamış modülleri sınır ve etiketle haritaya koyar. | A1–A10 haritası; satış/finans/servis/İK/connector `F` satırları |

`I = implemented`: bu sürümde çalışan ve testi olan davranış. `P = previewed`: eğitmenin gösterdiği veya sağlanan hazır örnek. `F = future`: ileride yazılacak. Her satır ayrıca **supplied / student** katkısını belirtir. Bir sınıfın adını koymak, o çapayı uygulamış olmak değildir.

## 2. Kapsam ve sorumluluk paylaşımı

| Şerit | İçerik |
| --- | --- |
| Core | W1 hesabının iskelete taşınması, ürün/reçete/stok nesnelerinin sorumluluk ayrımı, bir domain testi, bir tarayıcı smoke testi, sınıf diyagramı, A1–A10 haritası |
| Sağlanan altyapı | HTTP/ekran iskeleti, CLI giriş noktası, bellek içi repository, sabit fixture verisi, JUnit yapılandırması, `./mvnw` sarmalayıcısı |
| Instructor demo | Tek “ERPManager” sınıfının değişim maliyeti; sızdırılan koleksiyonun invariant'ı bozması |
| Stretch | İkinci bir ürün varyantı için aynı plan çağrısının bağımsız sonucu; zorunlu değildir |
| Sonraya bırakılan | Rezervasyon ve dikey dilim W3; satınalma/mal kabul/üretim/sevk ve `v0.1` W4; kalıtım W8, interface W9, değer nesneleri W11 |

Öğrenci bu hafta HTTP sunucusu, şablon motoru, oturum yönetimi veya kalıcılık katmanı yazmaz. Bunlar ders başlamadan çalışır durumda sağlanır. Tarayıcıda bir sayfanın açılması iş kuralının doğrulandığı anlamına gelmez; kanıt domain testindedir.

## 3. Eğitmenin ders öncesi hazırlığı

### En az beş gün önce yayımlanacak paket

- 20–30 dakikalık Türkçe yapı videosu; aynı ölçülen içeriğin İngilizce notu.
- W1 `MaterialPlanner` çözümünü kabul eden boş modül düzeni ve çalışır `./mvnw test`.
- Ürün, birim, BOM revizyonu ve başlangıç stok fixture'ı: `CHAIR-A`, `FRAME-A`, `WOOD-A`, `FABRIC-A`, `FOAM-A`, `VARNISH-A` ve `R1`.
- Tek ürün ile plan sonucunu gösteren küçük web/CLI giriş noktası; iş mantığı boş bırakılmış olarak.
- İki tenant (`F-A`, `F-B`) içeren sabit veri; aynı SKU'nun iki tenant'ta ayrı kayıt olduğu örnek.
- Bilerek hatalı iki referans sürüm: (1) her işi yapan `ErpManager`, (2) `getBomLines()` ile iç listeyi döndürüp dışarıdan değiştirilmesine izin veren `BomRevision`.
- `implemented / previewed / future` sütunlu A1–A10 çalışma kâğıdı ve on artımlık backlog şablonu.
- Kurulum sorunları için kontrol listesi, ortak laboratuvar ortamı ve sağlanan terminal kaydı.

### Yayımlama öncesi öğretmen kontrolü

| Kontrol | Hazır olma koşulu |
| --- | --- |
| Araç zinciri | Java 25 LTS, `./mvnw` ve JUnit 6 temiz kopyada çalışır; ilk indirme süresi ölçülmüştür. |
| Fixture | Reçete sayıları ortak örnekle birebir aynıdır: 100 CHAIR-A → WOOD 4 m³, FABRIC 150 m, FOAM 100 adet, VARNISH 10 kg. |
| Web iskeleti | Sayfa domain çağrısı olmadan da açılır; iş mantığı eklendiğinde sonucu gösterir. |
| Tenant | `F-A` ve `F-B` verisi ayrıdır; iskelet tenant bilgisini domain çağrısına taşır. |
| Karşı örnekler | İki hatalı sürüm derlenir ve testleri gerçekten kırar; “sözde hata” gösterilmez. |
| Süre | Studio'daki öğrenci değişikliği 120–150 aralığındaki 30 etkin dakikaya sığar. |
| Katkı | Sağlanan iskelet ile öğrenciden beklenen TODO sınırı dosya düzeyinde işaretlidir. |

## 4. Ders öncesi öğrenci hazırlığı

Toplam hedef: video/not, seçilmiş okuma, ortam kontrolü ve altı soru için yaklaşık 50–65 dakika. Yanıtlar dersten 12 saat önce teslim edilir. Öğrenci önce kısa bireysel tahmin yazar; sonraki açık hazırlıkta kullandığı kaynak/AI desteğini belirtir.

Okuma sınırı: [dev.java — Classes and Objects](https://dev.java/learn/classes-objects/) sayfalarından sınıf, nesne, alan ve metot girişleri; [ERP rehberinin](ERP_OGRENCI_REHBERI.md) “Nesne tasarımı: Hangi karar kime ait?” ve “Bütün modüllerin paylaştığı iş kuralları” bölümleri. Java dilinin tamamı bu haftanın ödevi değildir.

| Soru | Eğitmenin beklediği yön |
| --- | --- |
| 1. `plan(...)` çağrısı stoğu neden değiştirmemeli? | İhtiyaç hesabı bir okuma işlemidir; ayırma ayrı bir karar ve ayrı bir işlemdir. |
| 2. Reçete satırının miktarını kim değiştirebilmeli? | Yayınlanmış revizyon hiç kimse; değişiklik yeni revizyondur. |
| 3. `Product` sınıfı stok miktarını tutmalı mı? | Hayır; miktar depo/lot/durum boyutlu ayrı bir konumdur, katalog tanımı değildir. |
| 4. Bütün işleri yapan tek bir `ErpManager` sınıfının maliyeti ne? | Her değişiklik nedeni aynı dosyada toplanır; test, paralel çalışma ve etki alanı büyür. |
| 5. `getBomLines()` iç listeyi döndürürse ne olabilir? | Çağıran satır ekleyip yayınlanmış revizyonu bozar; invariant sınıfın dışına kaçar. |
| 6. Aynı SKU iki fabrikada varsa kimlik nasıl kurulur? | Kimlik tenant kapsamını taşır; `F-A` ve `F-B` kayıtları karışmaz. |

Öğrenci W1 testlerini tekrar çalıştırır; başarısızsa hatayı ortam, derleme, fixture veya davranış olarak sınıflandırır. Kısa hata kaydı derste destek sırasını belirler.

## 5. Tahta ve slayt omurgası

```text
HTTP isteği / CLI komutu        [sağlanan iskelet]
  └─ application service        tenant + istek doğrulama
       └─ MaterialPlanner       ihtiyaç hesabı; yan etkisiz
            ├─ BomRevision      yayınlanmış reçete; salt okunur
            │    └─ BomLine     bileşen + miktar + kayıp oranı
            └─ InventorySnapshot kullanılabilir miktar görünümü

Yan etki yok: plan çağrısı hiçbir stok/rezervasyon kaydı yazmaz.
Sahiplik: BomRevision kendi satırlarına sahiptir (composition).
Kullanım: BomLine, Product'a referans verir (association).
```

### Hedef modül düzeni

Aşağıdaki ağaç **hazırlanacak starter'ın sözleşmesidir**; bu öğretim belgeleri deposunda kod varmış gibi okunmaz.

```text
factory-erp/
├── src/main/java/.../domain/catalog/      Product, Unit, BomRevision, BomLine
├── src/main/java/.../domain/inventory/    InventoryPosition, InventorySnapshot
├── src/main/java/.../domain/planning/     MaterialPlanner, MaterialPlan, Shortage
├── src/main/java/.../application/         PlanningService (tenant + istek sınırı)
├── src/main/java/.../ports/               Repository arayüzleri (W9'da derinleşir)
├── src/main/java/.../adapters/memory/     Bellek içi repository (sağlanır)
├── src/main/java/.../web/                 Sağlanan HTTP/ekran iskeleti
├── src/test/java/...                      Domain ve smoke testleri
├── fixtures/                              Sabit ürün/BOM/stok verisi
└── docs/                                  Harita, diyagram ve karar kaydı
```

### A1–A3 yapı turu — bu haftanın odağı

| Çapa | Yapı sorusu | Bu hafta gösterilecek şey | Derinleşme bağlantısı |
| --- | --- | --- | --- |
| A1 — Sorumluluk ve ayrıştırma | Hangi karar hangi nesnenin işi? | Katalog/stok/planlama ayrımı; `ErpManager` karşı örneğinin bölünmesi | W5 varyant ve BOM revizyonu ayrıştırması |
| A2 — Encapsulation ve invariant | Geçersiz durum nasıl engellenir? | Yan etkisiz plan; `qty > 0`, `0 ≤ loss < 1`; yayınlanmış revizyonun salt okunurluğu | W6 kontrollü command metotları |
| A3 — İlişkiler, sahiplik ve UML | Hangi nesne kime sahip, kim kimi kullanır? | Revision→Line composition; Line→Product association; where-used yönü | W7 lot ve izlenebilirlik zinciri |

Her öğrenci satırın yanına I/P/F ve supplied/student etiketini ekler. Diyagramdaki ok yönü kodda gerçekten var olan bağımlılıkla uyuşmalıdır.

### A4–A10 — bu hafta yalnız haritada bekleyen

| Çapa | Nerede ele alınacak |
| --- | --- |
| A4 — Kalıtım ve yerine kullanılabilirlik | W3 (ikinci tur), W8 (derinleşme) |
| A5 — Interface ve dış sistem sınırları | W3 (ikinci tur), W9 (derinleşme) |
| A6 — Composition, Strategy ve maliyet | W3 (ikinci tur), W10 (derinleşme) |
| A7 — Kimlik, eşitlik ve immutability | W4 (ikinci tur), W11 (derinleşme) |
| A8 — Generics ve collections | W4 (ikinci tur), W12 (derinleşme) |
| A9 — Kaynaklar, kalıcılık ve hata | W4 (ikinci tur), W13 (derinleşme) |
| A10 — Runtime metadata ve plugin | W4 (ikinci tur), W14 (derinleşme) |

Bu yedi çapa bu hafta yeniden anlatılmaz; harita satırlarında `F` kalır. A1–A3 örneği bunlardan birine değerse yalnız tek cümlelik bağlantı kurulur.

## 6. 155 dakikalık ders akışı

| Süre | Öğretmen hamlesi | Öğrenci işi ve kontrol noktası |
| --- | --- | --- |
| 00–08 | W1'in plan sonucunu notsuz geri çağır; aynı hesabı tarayıcıda göster. | 4 / 150 / 100 / 10 ve eksik 1 m³ sayılarını yaz. |
| 08–20 | **A1:** Katalog, stok ve planlama sorumluluklarını ayır; “ne bilir / ne yapar / ne zaman değişir”. | Üç sorumluluk kartı. |
| 20–30 | **A1 karşı örneği:** Sağlanan `ErpManager` sürümünde bir fiyat değişikliğinin etkisini izlet. | Aynı dosyada kaç değişiklik nedeni var? |
| 30–40 | Ara | |
| 40–55 | **A2:** Yan etkisiz plan; `qty > 0` ve `0 ≤ loss < 1` invariant'ları; constructor doğrulaması. | Çağrı öncesi/sonrası stok karşılaştırması. |
| 55–70 | **A2 karşı örneği:** `getBomLines()` iç listeyi döndürür; yayınlanmış revizyon dışarıdan bozulur. | Sızıntıyı bul ve düzeltme önerisi yaz. |
| 70–80 | Ara | |
| 80–95 | **A3:** Composition/association ayrımı; Revision→Line sahipliği, Line→Product referansı, where-used yönü. | Kodla uyumlu küçük sınıf diyagramı. |
| 95–110 | Tenant kimliğinin istekten domain'e yolu; modül haritasına satış/finans/servis/İK/connector sınırlarını ekle. | `F-A`/`F-B` izi ve beş `F` satırı. |
| 110–120 | Ara | |
| 120–125 | Studio başlangıcı: bireysel AI'sız yapı tahmini. | Hangi sınıf hangi pakete girecek? |
| 125–136 | W1 hesabını modül düzenine taşıt ve testleri çalıştır. | Taşıma davranışı bozdu mu? |
| 136–145 | Plan servisini sağlanan web/CLI girişine bağlat. | Sayfada bir ürün ve plan sonucu. |
| 145–150 | Diyagram ile kodu karşılaştır; uyuşmayan bir oku düzelt. | Ara kabul; driver/predictor rolleri değişir. |
| 150–155 | Exit ticket ve W3 köprüsü. | Üç kısa yanıt ve eksik iş kaydı. |

Eğitmen ve iki asistan studio'da ortam, test ve kavramsal açıklama kontrollerini paylaşır. Puanlanan sözlüler dönem kapsama kaydına göre ayrı zamanlarda seçilir; tüm sınıf aynı gün sözlüye alınmaz.

## 7. Studio: aynı hesabı yapılı hâle getirme

### Adım 1 — Baseline

Başlangıç sürümünü kaydet. Kendi W1 kodun kullanılıyorsa bunu; eğitmen tabanı alındıysa sürümü ve eksikliği belirt. Fixture değerleri ortak örnekten alınır ve değiştirilmez.

### Adım 2 — Sorumluluk ayrımı

`MaterialPlanner` yalnız hesaplar; `InventorySnapshot` yalnız kullanılabilir miktarı okur; `BomRevision` yalnız yayınlanmış reçeteyi taşır. Hesabın girdisi ve çıktısı açıktır: talep miktarı ve snapshot girer; brüt ihtiyaç, eksik liste ve üretilebilir adet çıkar.

### Adım 3 — Invariant'ları sınıfa taşıma

Geçersiz miktar ve geçersiz kayıp oranı nesne oluşturulurken veya çağrı sınırında reddedilir. Yayınlanmış `BomRevision` dışarıya değiştirilebilir referans vermez. Reddedilen her istek state'i olduğu gibi bırakır.

### Adım 4 — Test matrisi

| Test | Beklenen sonuç | Kontrol edilen sınır |
| --- | --- | --- |
| W2-T1: 100 CHAIR-A/R1 planı | WOOD 4; FABRIC 150; FOAM 100; VARNISH 10 | W1 davranışı korunur (rehber O01). |
| W2-T2: başlangıç stoğuyla karşılaştırma | Yalnız WOOD eksik 1 m³; üretilebilir 75 | Eksik hesabı ve aşağı yuvarlama. |
| W2-T3: aynı planı iki kez çalıştırma | Aynı sonuç; snapshot değişmemiş | Yan etkisizlik (A2). |
| W2-T4: miktar 0 veya negatif | İş kuralı hatası; plan üretilmez | Girdi invariant'ı. |
| W2-T5: kayıp oranı ≥ 1 | Geçersiz reçete; bölme yapılmaz | `net / (1 − loss)` sınırı (rehber O02). |
| W2-T6: dönen BOM satır koleksiyonuna ekleme | Yayınlanmış revizyon değişmez | Koleksiyon sızıntısı (A2/A3). |
| W2-T7: `F-B` bileşeni `F-A` reçetesine bağlama | Reddedilir | Tenant kimliği sınırı. |
| W2-T8: tarayıcı smoke testi | Sayfa bir ürün ve plan sonucu gösterir | İskelet–domain bağlantısı. |

T1–T3 ders içinde tamamlanır. T4–T8 sağlanan testlerle çalıştırılır ve kısa çalışmada açıklanır. Bir testin ekranda yazı göstermesi yeterli değildir; ilgili nesne durumu da doğrulanır.

### Adım 5 — W3 için sınır

```text
Bu hafta:  plan(talep, snapshot) → MaterialPlan        [yan etkisiz]
W3'te:     reserve(talep) → ReservationResult          [state değiştirir]

W3'e taşınacak sorular:
  - Kısmi ayırma yapılabilir mi?  (hayır: hep ya da hiç)
  - Aynı request ID iki kez gelirse ne olur?
  - Rezervasyon fiziksel stoğu azaltır mı?  (hayır)
```

Bu hafta yalnız sınır ve sorumluluk gerekçesi yazılır. Çalışan rezervasyon davranışı W3'ün işidir; boş bir stub başarı döndürerek tamamlanmış gibi gösterilemez.

## 8. Öğretmen soruları, karşı örnekler ve müdahale

| Yanılgı/soru | Müdahale ve beklenen açıklama |
| --- | --- |
| “Her isim için bir sınıf açalım.” | Sorumluluk, davranış ve değişiklik nedeni sor; veri taşıyan kayıt ile karar veren nesneyi ayırt ettir. |
| “Plan çalıştı, demek ki malzeme ayrıldı.” | İhtiyaç hesabı ile rezervasyonun ayrı işlemler olduğunu ortak örnek üzerinde göster. |
| “Alanlar `private` olduğu için invariant güvende.” | `getBomLines()` sızıntısıyla yayınlanmış revizyonu bozdur. |
| “Stok miktarını `Product` içinde tutalım.” | Depo, lot ve durum boyutlarını sor; aynı ürünün iki lokasyondaki miktarını çizdir. |
| “Üretilebilir 75 ise 75 sandalye hazır demektir.” | Hesabın kapasite, termin ve rezervasyon garantisi olmadığını söylettir. |
| “Tarayıcıda göründü, iş kuralı doğrulandı.” | Kanıtın domain testinde olduğunu; ekranın yalnız gösterim olduğunu ayırt ettir. |
| “Tenant'ı sonra ekleriz.” | Kimliğe sonradan tenant eklemenin maliyetini ve sızıntı riskini göster. |

**Kısa sözlü kartları:** Bir sınıfın değişiklik nedenini söyle; plan çağrısının hangi state'i değiştirmediğini kanıtla; composition ile association farkını kendi diyagramından göster; aynı SKU iki tenant'ta varsa hangi kimlik kullanılır?

## 9. Teslim, geri bildirim ve süre

Teslim zamanı haftalık LMS paketinde açıkça yazılır. Paket şunları içerir:

1. W2 baseline, kendi kod farkı ve yeniden üretim komutları (`./mvnw test` dahil).
2. W2-T1–T8 için beklenen/gözlenen sonuç.
3. Kodla uyumlu sınıf/nesne diyagramı; composition ve association ayrı gösterilir.
4. Bir sayfalık A1–A10 haritası; A1–A3 için I/P kanıtı, A4–A10 için `F` ve hedef hafta.
5. Bir sorumluluk kararı ve gerekçesi; en fazla 150 kelime.
6. Bilinen sınırlama ve AI/dış katkı açıklaması.
7. 2–4 dakikalık video veya eşdeğer erişilebilir açıklama: plan sonucunu göster, bir invariant'ı ve onu koruyan kodu açıkla.

### Haftalık geri bildirim rubriği

Aşağıdaki 100 puan ölçeği bu paketin kalite geri bildirimidir; dersin toplam notuna yeni yüzde eklemez. W2 ürün doğruluğu W4'te ayrı bir özellik puanı olarak tekrar sayılmaz.

| Ölçüt | Puan |
| --- | ---: |
| W1 hesabının korunması ve iskelete doğru bağlanması | 25 |
| Sorumluluk ayrımı ve `ErpManager` yığılmasının çözülmesi (A1) | 25 |
| Invariant'ların sınıf içinde korunması ve sızıntının kapatılması (A2) | 20 |
| İlişki/sahiplik diyagramının kodla uyumu (A3) | 15 |
| Harita, tenant izi ve teknik gerekçe | 15 |
| **Toplam** | **100** |

Kısmi kanıt ilgili ölçütte değerlendirilir. Ortam hatası ile kavramsal hata ayrı geri bildirim alır.

Ders dışı Core hedefi: sorumluluk ayrımı 55, test/kanıt 40, diyagram/harita 40, belge/katkı 20 dakika; toplam yaklaşık 155 dakika. Hazırlık ve en çok 30 dakikalık video bu toplamdan ayrıdır. Kurulum yükü büyürse ortak laboratuvar ortamı sağlanır.

## 10. Exit ticket ve W3 köprüsü

Notlar/AI kapalı:

1. Plan çağrısının değiştirmediği bir state ve bunu kanıtlayan testi yaz.
2. Diyagramındaki bir composition ve bir association ilişkisini gerekçesiyle belirt.
3. W3'te rezervasyon eklenirken hangi invariant'ın ilk kez tehlikeye gireceğini tahmin et.

**W3 başlangıç cümlesi:** “Şimdi bu yapının içinden gerçek bir talep geçireceğiz: malzemeyi ayıracağız, aynı isteğin iki kez gelmesini yöneteceğiz ve kontroller bitmeden hiçbir state'in değişmediğini göstereceğiz.”

## 11. English student handout — Week 2

**Focus:** Build A1–A3 (responsibility and decomposition, encapsulation and invariants, relationships and ownership) in depth on the chair-factory ERP. The remaining seven anchors stay on the map as future work for Weeks 3–4 and the deepening weeks.

**Your task:** Move your Week 1 material-planning calculation into the supplied module layout and wire it to the supplied web/CLI entry point. Keep the calculation side-effect free: planning must not change stock or reservations. Separate catalogue, inventory and planning responsibilities instead of collecting them in one manager class.

**Required evidence:** 100 CHAIR-A against revision R1 requires 4 m³ wood, 150 m fabric, 100 foam units and 10 kg varnish; with the starting stock only wood is short by 1 m³ and 75 chairs are producible. Running the plan twice must return the same result and leave the snapshot unchanged. A released BOM revision must not be modifiable through a returned collection.

**Design output:** A class/object diagram that matches the code, distinguishing composition from association, plus a one-page A1–A10 map marking implemented, previewed and future work and separating supplied from student code. Show how the tenant identity travels from the request into the domain, and add the sales, finance, service, HR and connector boundaries as future entries.

**Preparation questions:** Why must planning leave stock untouched? Who may change a published recipe line? Should `Product` hold a stock quantity? What does a single all-purpose manager class cost you? What can go wrong if a getter returns the internal list? How is identity formed when the same SKU exists in two factories?

**Submission:** Code difference, test evidence, diagram, map, one responsibility decision, known limitations, contribution disclosure and one 2–4 minute explanation. An accessible equivalent is available. The feedback rubric allocates 25 points to preserving the Week 1 calculation, 25 to responsibility separation, 20 to invariants, 15 to the relationship diagram and 15 to the map and reasoning. These are not additional course-grade percentages.

**Tools and workload:** Begin studio with an individual AI-free attempt and disclose assistance in permitted open work. Target about 155 minutes of out-of-class Core work, excluding preparation and up to 30 minutes for the explanation recording. Use the supplied environment if setup blocks progress and label supplied output honestly.
