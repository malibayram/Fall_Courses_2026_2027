# CMPE 351 Database Systems — 4. Hafta Öğretim Dosyası

## Haftanın kimliği

| Alan | Plan |
| --- | --- |
| Tema | Entegrasyon: dayanıklı, erişimi denetlenen ve yeniden kurulabilir `v0.1` veri sürümü |
| Ana soru | Fabrikanın tam akışını taşıyan veriyi yedekten geri yükledikten sonra aynı toplamları ve aynı yetki sınırlarını nasıl kanıtlarız? |
| Süre | 155 dakika ders (125 dakika etkin + üç adet 10 dakikalık ara) + 120 dakika laboratuvar (110 dakika etkin) |
| Başlangıç | W3 aday sürümü; idempotent rezervasyon protokolü ve W1–W3 regresyonu |
| Haftanın ürünü | 100 sandalye ana akışı, 60+40 sevkiyat, açılış defteri uzlaşması, restore kanıtı, JSONB/RLS örneği ve `v0.1` belgeleri |
| Sonraki bağlantı | W5'te M1 derinleşmesi: iş yükü kataloğu ve oturum/bağlantı bütçesi |
| Belge durumu | Öğretim ve kabul sözleşmesi; süreç iskeletleri, restore hedefi, replica deneyi ve RLS iskeleti ayrıca hazırlanıp doğrulanacaktır |

**Bağlantılar:** [Ana ders sistemi](../README.md) · [ERP öğrenci rehberi](ERP_OGRENCI_REHBERI.md) · [Ders modeli](README.md) · [W1](HAFTA01.md) · [W2](HAFTA02.md) · [W3](HAFTA03.md) · [Kaynakça](KAYNAKCA.md)

Bu dosya eğitmen/asistan planıdır. **Bu hafta M7–M8 ayrıntılı işlenir** ve ikinci tur tamamlanır; M1–M6 yalnız kısa geri çağırmayla kullanılır. `v0.1`, sekiz modülün tamamının uygulandığı anlamına gelmez; ilişkisel çekirdeğin çalıştığı ve sonraki sekiz artımın bağlanacağı sınırların açık olduğu anlamına gelir.

## 1. Öğretim amacı ve kazanımlar

W3'te bir talebi güvenli biçimde ayırdık. W4'te bu talebi gerçek harekete çeviriyoruz ve verinin **hayatta kalmasını** sınıyoruz: satınalma ve mal kabul stoğu artırıyor, iş emri tüketiyor, sevkiyat iki parçada gidiyor; sonra veriyi ayrı bir hedefe geri yükleyip aynı toplamları ve aynı yetki sınırlarını arıyoruz.

| Kod | Öğrenci ders sonunda… | Kanıt |
| --- | --- | --- |
| W4-K1 | Eksik malzemeyi satınalma ve mal kabulle kullanılabilir hâle getirir. | Kabul öncesi kullanılamaz; sonra WOOD 4 m³ |
| W4-K2 | Üretim tüketimi ve mamul kabulünü çift tüketim olmadan yazar. | Tek tüketim; kalan FOAM 20 |
| W4-K3 | Kısmi sevkiyat sınırlarını korur. | 60 + 40 geçer; 101'inci reddedilir |
| W4-K4 | Snapshot'ı kontrollü açılış defterine taşır. | Tek açılış; tekrar işlemede çift kayıt yok |
| W4-K5 | Bakiyeyi hareket defteriyle uzlaştırır. | `inventory_balances` = hareket toplamı; fark yok |
| W4-K6 | Yedekten geri yükleme sonrası iş kurallarını yeniden doğrular. | Ayrı hedefe restore ve tekrarlanan kontroller |
| W4-K7 | Replica gecikmesinin garanti üzerindeki etkisini açıklar. | Sağlanan lag izi; RPO/RTO ayrımı |
| W4-K8 | Aynı belgeyi ilişkisel ve JSONB gösterimiyle karşılaştırır; rol erişimini sınar. | Pozitif/negatif RLS testi normal rollerle |

## 2. `v0.1` kapsam sınırı

| Şerit | İçerik |
| --- | --- |
| Core | Satınalma/mal kabul, üretim tüketimi/mamul kabul, 60+40 sevkiyat, açılış defteri, bakiye uzlaşması, restore kanıtı, bir JSONB sorgusu, tenant+bayi RLS politikası |
| Sağlanan altyapı | Süreç iskeletleri, restore hedefi, replica/lag deneyi, RLS iskeleti, auth rol eşlemesi, kabul harness'i |
| Instructor demo | `pg_dump` restore'un PITR olmaması; superuser ile alınan sahte izolasyon kanıtı; JSONB'nin her alanı esnetme yanılgısı |
| Stretch | İkinci bir servis belgesi için JSONB sorgusu veya ek bir rapor filtresi |
| Kapsam dışı | Gerçek bulut kurulumu, ücretli hizmet, tam PITR altyapısı, üretim ölçeğinde replication, gerçek e-belge/banka bağlantısı |

Kabul, sağlanan fixture ve yerel/ders ortamı içindir. **Ölçülmemiş SLA, kesintisizlik veya mevzuat uygunluğu iddiası yazılmaz.** Bilinen eşzamanlılık ve yetki sınırları release belgesinde görünür.

## 3. Eğitmenin hazırlığı ve kaynaklar

- 20–30 dakikalık entegrasyon videosu ve eşdeğer İngilizce içerik.
- Sabit W3 aday tabanı ve W4 TODO listesi; yeni şema açılmaz.
- Satınalma → mal kabul, iş emri → malzeme çıkışı → mamul kabul, sipariş → kısmi sevk işlem iskeletleri.
- Açılış defteri dönüşümü: `inventory_snapshot` → onaylı opening document; aynı `import_batch` iki kez işlendiğinde çift açılış oluşmamalıdır.
- Bakiye–defter uzlaştırma sorgusu: `inventory_balances` ile `stock_entries` toplamını karşılaştırır.
- Ayrı restore hedefi ve `pg_dump`/`pg_restore` betikleri; PITR'ın ayrıca base backup + WAL arşivi gerektirdiğini gösteren not.
- Sağlanan replica gecikme izi: commit edilmiş bir kaydın replica'da hemen görünmemesi.
- Servis belgesi için iki gösterim: ilişkisel sütunlar ve `jsonb` alanı; aynı iş sorusu ikisinde de sorgulanır.
- RLS iskeleti ve **normal roller**: `erp_admin_fa`, `erp_dealer_b1`, `erp_dealer_b2`, `erp_user_fb`, `erp_operator`. Superuser/BYPASSRLS ile kanıt üretilmez.
- Kabul formu, sözlü soru kartları ve 100 puanlık ürün rubriği.

### Ortak sayısal beklenti

| Aşama | Beklenen sonuç |
| --- | --- |
| Sipariş | 100 adet CHAIR-A, R1 |
| Eksik | WOOD-A 1 m³ (3 mevcut, 4 gerekli) |
| Satınalma + mal kabul | WOOD-A 4 m³ kullanılabilir |
| Üretim tüketimi | WOOD 4; FABRIC 150; FOAM 100; VARNISH 10 |
| Kalan stok | WOOD 0; FABRIC 0; VARNISH 0; **FOAM 20** |
| Sevkiyat | 60 + 40; 101'inci adet reddedilir |
| Maliyet | 105.000 TL toplam; birim 1.050 TL |
| Restore sonrası | Aynı toplamlar; aynı tenant/bayi görünürlüğü |

## 4. Ders öncesi öğrenci hazırlığı

Toplam hedef yaklaşık 45–60 dakika. W3 aday sürümünü değiştirmeden test et; eksikleri `şema / işlem / kanıt / ortam` başlıklarıyla ayır. Yanıtlar dersten 12 saat önce teslim edilir.

| Soru | Beklenen cevap yönü |
| --- | --- |
| 1. “Restore komutu başarılı” yeterli bir kurtarma kanıtı mı? | Hayır; iş kuralları ve toplamlar yeniden doğrulanmalıdır. |
| 2. `pg_dump` restore'u PITR midir? | Hayır; PITR için base backup ve WAL arşivi ayrıca gerekir. |
| 3. Commit edilen veri replica'da anında görünür mü? | Hayır; gecikme vardır ve okuma garantisi buna göre tanımlanır. |
| 4. RPO ile RTO farkı nedir? | RPO kabul edilen veri kaybı penceresi; RTO hizmete dönüş süresi. |
| 5. JSONB her alanı esnek yapmak için mi kullanılır? | Hayır; sabit ve sorgulanan iş alanları sütun kalır; JSONB değişken metadata içindir. |
| 6. RLS'yi superuser ile test edebilir miyiz? | Hayır; politika atlanır. Normal rollerle pozitif ve negatif test gerekir. |

## 5. Dayanıklılık ve erişim sözleşmesi

### Açılış defteri ve uzlaşma

```text
W1:  inventory_snapshot          başlangıç verisi
W4:  opening stock_document      onaylı açılış hareketi
     + import_batch kimliği      aynı batch iki kez → tek açılış

Uzlaşma kuralı:
  inventory_balances(on_hand)  ==  SUM(stock_entries.signed_qty)
  inventory_balances(reserved) ==  SUM(aktif reservations.qty)

inventory_balances bağımsız gerçek kaynak değildir; defterden uzlaştırılır.
```

### M7 — Kurtarma ve gecikme

Yedek ayrı bir hedefe geri yüklenir ve **aynı testler yeniden koşturulur**: stok/rezervasyon toplamları, R1 iş emri geçmişi, maliyet ve tenant görünürlüğü. “Restore başarılı” çıktısı tek başına kanıt değildir.

Sağlanan replica izinde commit edilmiş bir kaydın replica'da hemen görünmediği gözlenir. Buradan okuma garantisi, gecikme toleransı ve hangi raporun hangi kaynaktan okunacağı tartışılır. Ölçülmemiş kesintisizlik hedefi yazılmaz.

### M8 — Esnek gösterim ve satır düzeyi erişim

Aynı servis belgesi iki biçimde tutulur: sabit ve sorgulanan alanlar (müşteri, mamul, lot, sevk, durum) ilişkisel sütunlarda; değişken metadata (`jsonb`) alanında. Aynı iş sorusu iki gösterimde de sorgulanır; sorgulanabilirlik, bütünlük ve indeksleme farkı karşılaştırılır.

```text
RLS testi:
  USING       → hangi satır görünür
  WITH CHECK  → hangi satır yazılabilir

Roller: erp_admin_fa | erp_dealer_b1 | erp_dealer_b2 | erp_user_fb | erp_operator
Beklenti: B1, B2'nin fiyat/bakiye/sipariş/eklerini göremez; operatör ücret alanlarını göremez.
```

**RLS eşzamanlılık sorununu çözmez** (W3 geri çağırması) ve superuser/tablo sahibi/BYPASSRLS ile alınan sonuç izolasyon kanıtı değildir.

### M7–M8 entegrasyon turu — bu haftanın odağı

| Modül | `v0.1` içinde korunacak ilişki | Bu haftaki kabul kanıtı | Sonraki artım |
| --- | --- | --- | --- |
| M7 — Dağıtık, bulut ve dayanıklılık | Yedek, geri yükleme ve gecikme toleransı | Ayrı hedefe restore + tekrarlanan kontroller; lag izi; RPO/RTO ayrımı | W11 PITR, replication/sharding kararı |
| M8 — Doküman/realtime ve modern PostgreSQL | Esnek metadata ve satır düzeyi erişim | Bir JSONB sorgusu; normal rollerle pozitif/negatif RLS testi | W12 vektör/realtime karşılaştırması ve `v1.0` |

### M1–M6 kısa geri çağırma

| Modül | Bu haftaki kısa bağlantı |
| --- | --- |
| M1 | Restore hedefi ayrı bir sunucu/oturum bağlamıdır; roller yeniden kurulur. |
| M2 | Açılış defteri ve sevk satırları aynı tenant kapsamlı anahtarları taşır. |
| M3 | Uzlaştırma sorgusu gruplanmış aggregate'tir; çoklu join çoğalmasına dikkat edilir. |
| M4 | Açılış aktarımında kaynak/hedef/reddedilen satır uzlaşması korunur. |
| M5 | Üretim tamamlama ve sevkiyat W3 protokolünü kullanır; duplicate belge reddedilir. |
| M6 | Uzlaştırma ve sevk sorguları büyüyen defterde plan bakımından gözlenir; indeks kararı W10'a bırakılır. |

İkinci tur bu haftayla tamamlanır. Kümülatif harita bunu kaydeder; yeniden tam anlatım yapılmaz.

## 6. 155 dakikalık ders akışı

| Süre | İçerik ve öğretmen hamlesi | Öğrenci işi / kanıt |
| --- | --- | --- |
| 00–08 | W3 protokolünü kısaca geri çağır (M1–M6); tam zinciri referans sürümde göster. | Hangi adımda fiziksel miktar ilk kez değişir? |
| 08–20 | **M7:** Yedek/geri yükleme; “restore başarılı” çıktısının neyi kanıtlamadığı; ayrı hedefe restore ve tekrarlanan kontroller. | Restore sonrası hangi beş kontrol koşulmalı? |
| 20–30 | **M7:** Replica gecikme izi; RPO/RTO ayrımı; `pg_dump` restore'un PITR olmaması. | Gecikme toleransı cümlesi |
| 30–40 | **Ara** | |
| 40–55 | **M8:** Aynı servis belgesinin ilişkisel ve JSONB gösterimi; hangi alan sütun, hangisi metadata kalmalı. | İki gösterimin karşılaştırma tablosu |
| 55–70 | **M8:** `USING` ve `WITH CHECK`; normal rollerle pozitif/negatif test; superuser ile alınan sahte kanıt. | Beş rol için izin matrisi taslağı |
| 70–80 | **Ara** | |
| 80–95 | Açılış defteri dönüşümü ve bakiye–defter uzlaşması; aynı batch'in ikinci kez işlenmesi. | Uzlaştırma sorgusu ve tek açılış |
| 95–110 | Kabul formunu örnek eksik teslim üzerinde uygula; duplicate belge reddi ve 60+40 sınırları. | Geçti / başarısız / kanıt yok kararları |
| 110–120 | **Ara** | |
| 120–135 | M1–M8 haritasını ikinci turun tamamlanmasıyla kapat; bilinen eşzamanlılık ve yetki sınırlarını yaz. | Güncel harita ve sınır listesi |
| 135–145 | Release paketi: baseline, migration sürümü, komutlar, beklenen/gerçek sonuç. | Yeniden üretim planı |
| 145–150 | Kısa bireysel kontrol: bir restore kanıtını ve bir RLS kararını savun. | Bireysel açıklama |
| 150–155 | Exit ticket, release durumu ve W5 köprüsü. | Son eksik ve sonraki davranış kaydı |

## 7. 120 dakikalık laboratuvar akışı

| Süre | Uygulama | Beklenen kanıt |
| --- | --- | --- |
| 00–10 | Ortam ve W3 regresyonu; rezervasyon protokolünün geçtiğini doğrula. | Geçen W3 kontrolleri |
| 10–25 | Eksik ahşap için satınalma ve mal kabul; kabul öncesi/sonrası kullanılabilirlik. | WOOD 3 → 4 m³; kabul öncesi kullanılamaz |
| 25–40 | İş emri malzeme çıkışı ve mamul kabul; çift tüketim kontrolü. | Tek tüketim; kalan FOAM 20 |
| 40–55 | 60 + 40 sevkiyat; 101'inci adet ve duplicate belge denemesi. | İki sevk geçer; iki deneme reddedilir |
| 55–70 | Açılış defteri dönüşümü ve bakiye–defter uzlaştırma sorgusu. | Fark yok; aynı batch tekrarında tek açılış |
| 70–80 | **Ara** | |
| 80–92 | Ayrı hedefe `pg_dump`/`pg_restore`; ardından stok, maliyet ve R1 kontrollerini yeniden koş. | Restore sonrası aynı toplamlar |
| 92–102 | Sağlanan replica gecikme izini incele; hangi raporun hangi kaynaktan okunacağını kaydet. | Gecikme gözlemi ve RPO/RTO notu |
| 102–110 | Servis belgesini JSONB ile sorgula; beş rolle pozitif/negatif RLS testi yap. | JSONB sonucu; B1'in B2 verisini görememesi |
| 110–116 | M1–M8 haritasını ve release belgelerini güncelle. | Güncel harita ve `KNOWN_LIMITS` |
| 116–120 | Teslimi kontrol et; gerçek anahtar/müşteri verisi bulunmadığını doğrula. | Teslim kontrolü |

RLS testleri normal rollerle yapılır. Superuser veya tablo sahibiyle alınan “başarılı izolasyon” sonucu kabul edilmez.

## 8. `v0.1` kabul kataloğu

| Test | Senaryo | Beklenen sonuç ve kanıt |
| --- | --- | --- |
| W4-D1 | Ana BOM ve eksik sorgusu | 4/150/100/10; eksik WOOD 1; üretilebilir 75 (rehber D01) |
| W4-D2 | Satınalma ve mal kabul | Kabul öncesi kullanılamaz; sonra WOOD 4 m³ |
| W4-D3 | Malzeme çıkışı ve mamul kabul | Tek tüketim; 100 sağlam mamul |
| W4-D4 | Kalan stok | WOOD 0; FABRIC 0; VARNISH 0; FOAM 20 |
| W4-D5 | 60 + 40 sevkiyat, sonra fazla | İlk ikisi geçer; fazlası reddedilir (rehber D09) |
| W4-D6 | Duplicate belge/olay | Tek iş etkisi (rehber D05) |
| W4-D7 | Açılış defteri tekrarı | Çift açılış yok; toplamlar aynı |
| W4-D8 | Bakiye–defter uzlaşması | `on_hand` = hareket toplamı; `reserved` = aktif rezervasyon toplamı |
| W4-D9 | Üretim tamamlamayı tekrar gönderme | İkinci mamul/maliyet olayı yok (rehber D08) |
| W4-D10 | Maliyet raporu | 1.050 TL birim; çoklu join çoğalması yok (rehber D11) |
| W4-D11 | Yedekten geri yükleme | İş kuralları ve toplamlar tekrar geçer (rehber D13) |
| W4-D12 | Replica gecikmesi | Gecikme gözlenir; okuma garantisi yazılır (rehber D14) |
| W4-D13 | JSONB servis belgesi | Aynı iş sorusu iki gösterimde de yanıtlanır |
| W4-D14 | İki bayi ve iki tenant | Okuma/yazma/ek erişimi ayrışır; normal rollerle (rehber D12) |
| W4-D15 | W1–W3 regresyonu | İhtiyaç sorgusu, bütünlük ve rezervasyon protokolü korunur |

Negatif denemeler disposable test verisinde yapılır. “Reddedildi” kanıtında hangi satırların değişmediği de gösterilir.

## 9. Release paketi ve yeniden üretim

```text
README.md        Kurulum, roller, komutlar, ana senaryo ve beklenen sonuçlar
BASELINE.md      W3 başlangıcı, migration sürümü, supplied/student ayrımı
CHANGELOG.md     W4 değişiklikleri ve giderilen hatalar
KNOWN_LIMITS.md  Eşzamanlılık sınırı, yetki sınırı, PITR yokluğu, ölçülmemiş SLA
AI_USAGE.md      Araç/kaynak katkısı ve doğrulama
migrations/      Sürümlü ve değiştirilmemiş dosyalar
seed/            Sabit fixture ve açılış batch'i
queries/         İhtiyaç, uzlaştırma, maliyet ve servis sorguları
evidence/        Kabul tablosu, restore kaydı, planlar ve haritalar
```

### Yeniden üretim kontrolü

1. Boş test veritabanında migration'ları uygula; sürümü doğrula.
2. Seed'i ve açılış batch'ini yükle; toplamları kaydet.
3. Ana senaryoyu koştur; D1–D15 sonuçlarını topla.
4. Ayrı hedefe restore et; aynı kontrolleri yeniden koştur.
5. RLS testlerini normal rollerle tekrarla.
6. Haritadaki her `O` iddiasının kanıtını, her `F` alanının hedef haftasını bul.
7. Release durumunu `kabul edildi / düzeltme gerekli` olarak kaydet ve gerekçeyi yaz.

Canlı erişim anahtarı, gerçek müşteri veya personel verisi teslim edilmez.

## 10. Ürün kabulü ve değerlendirme rubriği

Kabul kapısı ile puan ayrı kaydedilir. Ana zincir, uzlaşma veya sevk sınırı yanlışsa ürün `v0.1` kapısını geçmez; öğrenci doğru kalan açıklama ve deney tasarımı ölçütlerinden geri bildirim/puan alabilir.

| Ölçüt | Beklenen kanıt | Puan |
| --- | --- | ---: |
| Uçtan uca fabrika veri akışı | Satınalma → üretim → 60+40 sevkiyat; kalan FOAM 20 | 25 |
| Açılış defteri ve bakiye–defter uzlaşması | Tek açılış; fark üretmeyen uzlaştırma sorgusu | 20 |
| Kurtarma kanıtı ve gecikme yorumu (M7) | Ayrı hedefe restore; tekrarlanan kontroller; RPO/RTO ayrımı | 20 |
| Esnek gösterim ve erişim denetimi (M8) | JSONB sorgusu; normal rollerle pozitif/negatif RLS | 20 |
| Sürüm belgeleri, sınırlar ve yeniden üretim | Bilinen sınırlamalar ve karar kaydı | 15 |
| **Toplam** | | **100** |

Bu rubrik ERP rehberindeki W4 kapısını ayrıntılandırır; ders toplamına yeni yüzde eklemez. W2/W3'te notlandırılan yerel ölçütler tekrar puanlanmaz.

Bireysel soru kartları:

- Mal kabulden önce ahşap neden kullanılabilir değildi?
- `inventory_balances` neden bağımsız gerçek kaynak değil?
- “Restore komutu başarılı” neden yeterli kanıt değil?
- Commit edilmiş bir kayıt replica'da neden hemen görünmeyebilir?
- Hangi alanı JSONB'ye koymazdın ve neden?
- RLS testini superuser ile yapmak neden geçersiz?
- Bu sürümde eşzamanlılık hakkında henüz kanıtlamadığın şey nedir?

## 11. Teslim, süre ve kurtarma tabanı

Teslim; migration sürümü, kod/SQL farkı, W4-D1–D15 kabul tablosu, restore kaydı, M1–M8 haritası, release belgeleri ve 2–4 dakikalık açıklamayı içerir. Metin raporu 1–2 sayfayı hedefler; planlar ve ham çıktılar ekte tutulur.

Ders dışı Core hedefi: zincir tamamlama 50, uzlaştırma ve kabul testleri 40, restore/RLS kanıtı 30, harita/release/katkı 25 dakika; toplam yaklaşık 145 dakika. Hazırlık, laboratuvar ve en çok 30 dakikalık video ayrıdır.

W4 sonrasında öğretim ekibi test edilmiş bir `v0.1` kurtarma tabanı yayımlar. Bu tabanı alan öğrenci kaynak sürümü ve kendi eksiğini açıklar; W5 artımını bu taban üzerinden yapar. Hazır sürümü kopyalamak geçmiş eksik puanı kazandırmaz.

## 12. Exit ticket ve W5'e geçiş

Notlar/AI kapalı:

1. Restore sonrası koştuğun bir kontrolü ve sonucunu yaz.
2. Bakiye ile hareket defteri arasındaki uzlaşmanın neden gerekli olduğunu açıkla.
3. W5'te iş yükü kataloğu çıkarılırken hangi iki sorgunun farklı garanti isteyeceğini tahmin et.

**W5 başlangıç cümlesi:** “Elimizde artık kurulabilen, geri yüklenebilen ve erişimi denetlenen bir veri ürünü var. Şimdi bu ürüne gelen farklı iş yüklerini ayıracak; üretim kiosk'u, B2B araması ve gece raporunun aynı sistemden ne istediğini ölçeceğiz.”

## 13. English student handout — Week 4

**Focus:** Integrate the factory data chain into a reproducible `v0.1` release. Build M7–M8 (distributed/cloud/durability, document models and modern PostgreSQL including RLS) in depth; M1–M6 get only a brief callback, completing the second pass across Weeks 2–4.

**Your task:** Turn the shortage of 1 m³ of wood into a purchase order and a goods receipt, issue materials against the work order, receive finished goods without a second deduction, and ship 60 then 40. Convert the Week 1 snapshot into a controlled opening document and reconcile `inventory_balances` against the movement ledger — balances are not an independent source of truth.

**Durability evidence:** Restore a backup into a separate target and rerun the checks: stock and reservation totals, the R1 work-order history, cost, and tenant visibility. A successful restore command is not evidence on its own. Examine the supplied replica-lag trace and state the read guarantee you rely on. A `pg_dump` restore is not point-in-time recovery; PITR additionally needs a base backup and a WAL archive. Do not write SLA or availability targets you did not measure.

**Access and flexible storage:** Keep stable, queried service-case fields in relational columns and variable metadata in `jsonb`, then answer the same business question from both representations. Test row-level security with the ordinary roles provided — dealer B1 must not see B2's prices, balances, orders or attachments, and the operator must not see wage fields. Use `USING` for visibility and `WITH CHECK` for writability. A result obtained as superuser, table owner or with BYPASSRLS is not isolation evidence, and RLS does not solve the Week 3 race.

**Acceptance evidence:** Provide the W4-D1–D15 matrix. Wood, fabric and varnish end at 0 and foam at 20; a 101st unit and a duplicate document are rejected; a repeated production completion creates no second finished-goods event; the cost report gives 1.050 TL per unit without join inflation.

**Submission:** Migration version, SQL difference, acceptance table, restore record, the M1–M8 map showing the completed second pass, release documents with known limits and contribution disclosure, plus a 2–4 minute explanation. Never submit live credentials or real customer or personnel data.

**Rubric:** 25 points for the end-to-end chain, 20 for the opening ledger and reconciliation, 20 for recovery evidence and lag interpretation, 20 for flexible storage and access control, and 15 for release documentation and reproducibility. This is the Week 4 product rubric, not an added course-grade percentage.
