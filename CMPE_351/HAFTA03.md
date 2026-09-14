# CMPE 351 Database Systems — 3. Hafta Öğretim Dosyası

## Haftanın kimliği

| Alan | Plan |
| --- | --- |
| Tema | Veri ürününün davranışı: bağımlılıklar, transaction sınırı ve sorgu planı |
| Ana soru | Aynı ahşabı iki sipariş birden istediğinde şema tek başına neden yetmez; doğru işlem sınırı nedir? |
| Süre | 155 dakika ders (125 dakika etkin + üç adet 10 dakikalık ara) + 120 dakika laboratuvar (110 dakika etkin) |
| Başlangıç | W2 migration/seed sürümü; rol ayrımı ve doğrulanmış ihtiyaç sorgusu |
| Haftanın ürünü | Idempotent rezervasyon transaction iskeleti, iki oturumlu yarış deneyi, önce/sonra sorgu planı, W4 aday sürümü |
| Sonraki kapı | W4'te satınalma–üretim–sevkiyat zinciri ve `v0.1` veri sürümü |
| Belge durumu | Öğretim ve uygulama sözleşmesi; iki oturum harness'i, staging fixture'ı ve seed büyütücüsü ayrıca üretilecektir |

**Bağlantılar:** [Ana ders sistemi](../README.md) · [Güncel kapsam](GUNCEL_KAPSAM.md) · [ERP öğrenci rehberi](ERP_OGRENCI_REHBERI.md) · [Ders modeli](README.md) · [W1](HAFTA01.md) · [W2](HAFTA02.md) · [W4](HAFTA04.md) · [Kaynakça](KAYNAKCA.md)

Dosya öğretim ekibi içindir. Güncel proje sözleşmesi [ERP rehberidir](ERP_OGRENCI_REHBERI.md); rezervasyon protokolünün tam metni rehberin “Rezervasyonun transaction sözleşmesi” bölümündedir.

## 1. Öğretim amacı ve kazanımlar

W2'de şemayı kurduk ve bir bilgi ihtiyacını doğrulanmış SQL'e çevirdik. Bu hafta aynı veriye **eşzamanlı ve hatalı** istekler gönderiyoruz: aynı malzemeyi iki sipariş istiyor, aynı olay iki kez geliyor, bir kalem eksik kalıyor ve sorgu büyüyen veride yavaşlıyor.

**Bu hafta M4–M6 ayrıntılı işlenir:** M4 fonksiyonel bağımlılıklar ve anomaliler, M5 transaction/eşzamanlılık/kurtarma, M6 depolama/indeks/sorgu işleme. M1–M3 yalnız kısa geri çağırmayla kullanılır; M7–M8 W4'e bırakılır ve haritada `F` kalır.

| Kod | Öğrenci ders sonunda… | Kanıt |
| --- | --- | --- |
| W3-K1 | Staging tablosundaki fonksiyonel bağımlılıkları ve ürettiği anomaliyi gösterir. | FD listesi ve güncelleme/ekleme/silme anomali örneği |
| W3-K2 | Anomaliyi gerekçeli ayrıştırmayla giderir ve satır sayılarını uzlaştırır. | Önce–sonra karşı örneği; kaynak/hedef/reddedilen satır |
| W3-K3 | Check-then-write yarışının neden hatalı olduğunu gösterir. | İki oturumda 8+8 isteğin 16 ayırması |
| W3-K4 | Kilitli yeniden kontrolle hep-ya-hiç rezervasyon uygular. | Bir kabul; `reserved = 8`, `available = 2` |
| W3-K5 | Aynı anahtarın aynı/farklı içerikle tekrarını ayırır. | Tek iş etkisi ve çatışma sonucu |
| W3-K6 | Hata sonrası state'in tutarlı kaldığını gösterir. | Rollback sonrası değişmemiş bakiye |
| W3-K7 | Bir sorgunun önce/sonra planını ve indeksin yazma bedelini yorumlar. | `EXPLAIN` çıktıları ve ölçüm ortamı |
| W3-K8 | Satınalma, üretim, finans ve dış olayların transaction sınırlarını haritalar. | İşlem sınırı haritası |

## 2. Kapsamı sınırlama

| Şerit | İçerik |
| --- | --- |
| Core | Küçük staging ayrıştırması, idempotent rezervasyon transaction'ı, iki oturumlu yarış deneyi, rollback kanıtı, bir indeks kararı ve önce/sonra plan |
| Sağlanan altyapı | İki oturum bariyer harness'i, staging fixture'ı, `command_results` iskeleti, seed büyütücüsü, plan toplama betiği |
| Instructor demo | Yanlış check-then-write sürümü; kilit sırasının bozulmasıyla oluşan deadlock; gereksiz indeksin yazma maliyeti |
| Stretch | İkinci bir iş yükü için ek plan karşılaştırması; zorunlu değildir |
| W4'e kalan | Satınalma/mal kabul, üretim tüketimi, kısmi sevkiyat, açılış defteri ve `v0.1` |
| Derinleşmeye kalan | Tam normalizasyon çalışması W8; bağımsız concurrency kabulü W9; ölçülmüş indeks portföyü W10 |

**İlk rehberli deney, W9'daki bağımsız eşzamanlılık kabulünün yerine geçmez.** Bu hafta protokol gösterilir ve uygulanır; kapsamlı isolation analizi ve retry politikası W9'dadır. Bu sınır teslimde yazılır.

## 3. Eğitmenin hazırlayacağı paket

- 25–35 dakikalık davranış videosu ve eşdeğer İngilizce not.
- İki `psql` oturumunu belirli noktalarda bekletebilen bariyer harness'i; zamanlamaya değil olaya dayalı.
- Bilerek hatalı check-then-write rezervasyon fonksiyonu ve doğru protokol iskeleti.
- `command_results(tenant_id, request_id, payload_hash, result)` tablosu ve tekillik kısıtı.
- Denormalize staging fixture'ı: aynı tedarikçi adının ve ürün özelliklerinin satır satır tekrarlandığı içe aktarım tablosu.
- Seed büyütücüsü: stok hareket tablosunu plan farkının görülebileceği boyuta çıkarır.
- Plan toplama betiği: aynı sorgunun indeks öncesi/sonrası `EXPLAIN` çıktısını ve yazma süresini kaydeder.
- İşlem sınırı haritası şablonu ve M1–M8 çalışma kâğıdı.

### Altyapının sorumluluğu

Bariyer harness'i, `command_results` iskeleti ve seed büyütücüsü öğretim ekibi tarafından doğrulanır. Öğrenci bu hafta kendi kilit yöneticisini veya bağlantı havuzunu yazmaz; kilit sırasını, yeniden kontrolü ve idempotency anahtarını kullanır ve sonucu yorumlar.

Deney sonuçları `sleep` süresine değil, gözlenen olaya dayanır. Zamanlamaya dayalı “galiba önce bu çalıştı” açıklaması kanıt sayılmaz.

## 4. Ders öncesi çalışma ve soru anahtarı

Toplam hazırlık hedefi yaklaşık 55–70 dakika. Seçili okuma: [Transaction Isolation](https://www.postgresql.org/docs/18/transaction-iso.html); [ERP rehberinin](ERP_OGRENCI_REHBERI.md) “Rezervasyonun transaction sözleşmesi” bölümü. Belgelerin tamamı bu haftanın ödevi değildir.

| Hazırlık sorusu | Beklenen yön |
| --- | --- |
| 1. `SELECT` ile kontrol edip sonra `UPDATE` yazmak neden yeterli değil? | İki oturum aynı eski değeri okuyup ikisi birden yazabilir; kontrol ile yazma arasında pencere vardır. |
| 2. Kullanılabilir miktar şemadan tek satırda okunabilir mi? | Hayır; serbest fiziksel ve aktif rezervasyon ayrı toplamlardır. |
| 3. Dört kalemin biri eksikse kısmen ayırmalı mıyız? | Hayır; hiçbir kalem ayrılmaz ve ret sonucu kaydedilir. |
| 4. Reddedilen isteğin sonucu neden kaydedilir? | Tekrarlanan aynı isteğin sonucu da belirli olsun diye. |
| 5. Deadlock'tan kaçınmak için ne yaparız? | Tüm işlemlerde deterministik aynı kilit sırası; retry edilebilir hatada bütün transaction'ı sınırlı sayıda yeniden dene. |
| 6. İndeks eklemek bedelsiz mi? | Hayır; yazma ve depolama maliyeti vardır, seçicilik düşükse okuma da hızlanmaz. |

## 5. Teknik sözleşme: bir talebi ayırmak

### Yanlış sürüm

```text
T1: SELECT on_hand - reserved FROM inventory_balances ...   → 10
T2: SELECT on_hand - reserved FROM inventory_balances ...   → 10
T1: UPDATE ... reserved = reserved + 8                      → 8
T2: UPDATE ... reserved = reserved + 8                      → 16   ✗

Sonuç: fiziksel 10 iken 16 birim ayrılmış görünür.
```

### Doğru protokol

```text
1. BEGIN; aktör ve tenant doğrulanır.
2. command_results'a (tenant, request_id, payload_hash) yazılır.
   - aynı key + aynı hash  → kayıtlı sonuç döner
   - aynı key + farklı hash → çatışma
3. İlgili tüm stok pozisyonları DETERMİNİSTİK AYNI SIRADA
   SELECT ... FOR UPDATE ile kilitlenir.
4. Kilit altında kullanılabilir miktar her kalem için YENİDEN kontrol edilir.
   - herhangi biri yetersiz → hiçbir değişiklik yapılmaz; ret sonucu kaydedilir; COMMIT
5. Tümü uygunsa: rezervasyon satırları + bakiye + command sonucu + outbox olayı
   AYNI transaction'da yazılır; COMMIT.
6. Yanıt kaybolursa istemci aynı key ile sonucu sorar.
   Deadlock/serialization hatasında BÜTÜN transaction sınırlı sayıda yeniden denenir.
```

Ortak sayısal sonuç: başlangıçta `on_hand = 10`, `reserved = 0`; T1 ve T2 farklı siparişler için 8'er birim ister. Doğru protokolde **yalnız biri kabul edilir**, `reserved = 8` ve kullanılabilir `2` olur.

Doğrudan bakiye yazma izni normal uygulama rolüne verilmez. **RLS tek başına bu yarışı çözmez;** erişim denetimi ile eşzamanlılık denetimi ayrı problemlerdir.

### M4–M6 davranış turu — bu haftanın odağı

| Modül | İzlenen davranış | Hata/karşı örnek | Bu hafta kanıtı |
| --- | --- | --- | --- |
| M4 — Bağımlılıklar ve normalizasyon | Staging satırında hangi alan hangisini belirler? | Tedarikçi adını tek satırda düzeltmenin diğer satırları bozuk bırakması | FD listesi, anomali ve ayrıştırma |
| M5 — Transaction ve eşzamanlılık | Kilitli yeniden kontrol, hep-ya-hiç ve idempotency | Check-then-write ile 16 birim ayrılması | İki oturum deneyi; `reserved = 8` |
| M6 — Depolama, indeks ve sorgu işleme | Plan şekli seçiciliğe ve fiziksel yapıya bağlıdır. | Kullanılmayan indeksin yazmayı yavaşlatması | Önce/sonra `EXPLAIN` ve yazma bedeli |

### M1–M3 kısa geri çağırma, M7–M8 bekleyen

M1 (mimari/oturum), M2 (anahtar/bütünlük) ve M3 (cebir/SQL) W2'de işlendi; bu hafta yalnız “bu kural hangi kısıtla korunuyordu?” düzeyinde birer cümleyle hatırlatılır. M7 (dağıtık/kurtarma) ve M8 (doküman/realtime/RLS) W4'te ele alınacaktır.

## 6. 155 dakikalık ders akışı

| Süre | İçerik ve öğretmen hamlesi | Öğrenci işi / kanıt |
| --- | --- | --- |
| 00–08 | W2 şemasını ve ihtiyaç sorgusunu kısaca geri çağır; iki oturumlu yarışın hatalı sonucunu göster. | Fiziksel 10 iken 16 nasıl ayrıldı? |
| 08–20 | **M4:** Staging tablosunda fonksiyonel bağımlılıkları belirle; determinant ve candidate key sezgisi. | Üç FD ve bir candidate key |
| 20–30 | **M4:** Güncelleme/ekleme/silme anomalisi; gerekçeli ayrıştırma ve satır sayısı uzlaştırması. | Bir anomali ve düzeltmesi |
| 30–40 | **Ara** | |
| 40–55 | **M5:** Check-then-write penceresi; kilit altında yeniden kontrol; hep-ya-hiç kuralı. | Protokolün beş adımı |
| 55–70 | **M5:** Idempotency anahtarı, reddedilen sonucun kaydı, rollback ve deterministik kilit sırası. | Aynı key/aynı hash ve aynı key/farklı hash sonuçları |
| 70–80 | **Ara** | |
| 80–95 | **M6:** Aynı sorgunun indeks öncesi/sonrası planı; seçicilik, scan/join düğümleri. | Önce/sonra plan şekli |
| 95–110 | **M6:** İndeksin yazma ve depolama bedeli; “her indeksi kur” yanılgısı. | Bir indeks kararı ve gerekçesi |
| 110–120 | **Ara** | |
| 120–135 | Satınalma, üretim, finans ve dış olayların transaction sınırlarını aynı haritaya koy; duplicate event ve geçersiz FK yolları. | İşlem sınırı haritası |
| 135–145 | M1–M8 haritasını güncelle; M7–M8 satırlarını `F` olarak bırak; W4 eksik listesini çıkar. | Güncel harita ve aday sürüm kaydı |
| 145–150 | Kısa bireysel kontrol: kilit sırasını ve bir plan kararını savun. | Bireysel açıklama |
| 150–155 | Exit ticket ve laboratuvar hedefini açıkla. | Çıkış kaydı |

## 7. 120 dakikalık laboratuvar akışı

| Süre | Uygulama | Beklenen kanıt |
| --- | --- | --- |
| 00–10 | Ortam ve W2 regresyonu; migration sürümünü doğrula. | Geçen W2 kontrolleri |
| 10–25 | Staging fixture'ında FD'leri bul; bir güncelleme anomalisi üret. | FD listesi ve bozulan satırlar |
| 25–40 | Küçük ayrıştırmayı uygula; kaynak, hedef ve reddedilen satır sayılarını uzlaştır. | Önce–sonra tablo ve sayı mutabakatı |
| 40–55 | Hatalı check-then-write sürümünü iki oturumda çalıştır. | `reserved = 16`; fiziksel 10 |
| 55–70 | Doğru protokolü uygula; aynı deneyi tekrarla. | Bir kabul; `reserved = 8`, kullanılabilir 2 |
| 70–80 | **Ara** | |
| 80–94 | Idempotency: aynı key/aynı içerik ve aynı key/farklı içerik; ardından bir kalemi eksilterek hep-ya-hiç reddini gör. | Tek iş etkisi, çatışma ve hiç ayrılmamış kalemler |
| 94–104 | Seed'i büyüt; hedef sorgunun planını indeks öncesi/sonrası al; yazma süresini karşılaştır. | İki plan ve yazma bedeli ölçümü |
| 104–112 | İşlem sınırı haritasını ve M1–M8 haritasını güncelle. | Güncel haritalar |
| 112–118 | Baseline, komutlar ve beklenen/gözlenen sonuçları kaydet. | Yeniden üretim kaydı |
| 118–120 | Teslimi kontrol et; negatif denemelerin disposable veride yapıldığını doğrula. | Teslim kontrolü |

Ölçümde veri boyutu, tekrar sayısı, cache durumu ve ortam bildirilir. Tek bir hızlı çalıştırma performans kanıtı değildir.

## 8. Kabul denemeleri

| Test | Senaryo | Beklenen sonuç ve kanıt |
| --- | --- | --- |
| W3-D1 | Hatalı check-then-write | `reserved = 16`; problem görünür kılınır |
| W3-D2 | Doğru protokolle 8+8 | Bir kabul; `reserved = 8`, kullanılabilir 2 (rehber D06) |
| W3-D3 | Çok kalemli istekte son kalem eksik | Hiçbir kalem ayrılmaz (rehber D07) |
| W3-D4 | Aynı key, aynı payload | Kayıtlı sonuç döner; ikinci kayıt yok (rehber D05) |
| W3-D5 | Aynı key, farklı payload | Çatışma; sessiz üzerine yazma yok |
| W3-D6 | Reddedilen isteğin tekrarı | Aynı ret sonucu; belirli davranış |
| W3-D7 | Transaction ortasında hata | Rollback; bakiye ve rezervasyon değişmemiş |
| W3-D8 | Geçersiz tenant FK | Ret; tutarlı state (rehber D02) |
| W3-D9 | Staging ayrıştırması | Kaynak/hedef/reddedilen satır uzlaşır (rehber D16) |
| W3-D10 | İndeks öncesi/sonrası plan | Plan şekli değişir; yazma bedeli raporlanır |
| W3-D11 | W2 regresyonu | İhtiyaç sorgusu ve bütünlük kısıtları korunur |

Hata çıktılarında SQLSTATE ve iş sonucu kullanılır; lokalize mesajın tamamına bağımlı olunmaz. “Reddedildi” kanıtında hangi satırların değişmediği de gösterilir.

## 9. Yanılgılar, kısa sorular ve cevap yönü

| Yanılgı | Öğretmen müdahalesi |
| --- | --- |
| “`BEGIN` yazınca yarış çözülür.” | Transaction sınırının tek başına yeterli olmadığını; kilit ve yeniden kontrol gerektiğini iki oturumda göster. |
| “Isolation seviyesini yükseltmek her şeyi çözer.” | Seviyenin neyi garanti ettiğini sor; retry gereğini ve maliyetini tartıştır. |
| “PostgreSQL'de dirty read gösterelim.” | Read Uncommitted'in Read Committed gibi davrandığını; genel anomali ile ürün davranışını ayırt ettir. |
| “RLS koyduk, eşzamanlılık da güvenli.” | Erişim denetimi ile eşzamanlılık denetiminin farklı problemler olduğunu söylettir. |
| “Kısmen ayıralım, kalanı sonra.” | Kısmen ayrılmış malzemenin kilitlenme ürettiğini göster. |
| “Daha çok indeks daha hızlı sistem.” | Yazma bedelini ölçtür; kullanılmayan indeksin planda görünmediğini göster. |
| “Bir kez hızlı çalıştı, iyileşti.” | Tekrar sayısı, cache durumu ve yayılım isteyip ölçüm ortamını yazdır. |

**Bireysel sözlü kartları:** Check-then-write penceresini bir olay çizgisiyle anlat; kilit sırasının neden deterministik olması gerektiğini açıkla; reddedilen sonucun kaydedilmesinin nedenini söyle; seçtiğin indeksin yazma bedelini nasıl ölçtün?

## 10. Teslim ve geri bildirim

1. W2'ye göre migration/kod farkı ve kesin W3 aday sürümü.
2. W3-D1–D11 için beklenen/gözlenen sonuç ve değişmeyen satır kanıtı.
3. Rezervasyon protokolünün kilit sırası ve hata yolları ile birlikte şeması.
4. Staging FD listesi, anomali örneği ve ayrıştırma mutabakatı.
5. Önce/sonra plan çıktıları, ölçüm ortamı ve indeks kararı gerekçesi.
6. İşlem sınırı haritası ve güncel M1–M8 haritası.
7. Bilinen sınırlama (rehberli deney; W9 kabulünün yerine geçmez) ve AI/dış katkı açıklaması.

| Haftalık kalite ölçütü | Puan |
| --- | ---: |
| Idempotent ve hep-ya-hiç rezervasyon protokolü (M5) | 30 |
| İki oturum deneyi, rollback ve tekrar anahtarı kanıtı | 20 |
| FD/anomali analizi ve ayrıştırma mutabakatı (M4) | 20 |
| Plan yorumu, indeks kararı ve yazma bedeli (M6) | 15 |
| İşlem sınırı haritası ve yeniden üretim | 15 |
| **Toplam** | **100** |

Ders dışı Core hedefi: protokol tamamlama 55, deney/kanıt 40, staging ayrıştırması 30, plan/harita/belge 25 dakika; toplam yaklaşık 150 dakika. Hazırlık, laboratuvar ve en çok 30 dakikalık video ayrıdır.

## 11. Exit ticket ve W4 köprüsü

Notlar/AI kapalı:

1. Check-then-write yarışını bir olay çizgisiyle yaz.
2. Reddedilen bir isteğin sonucunun neden kaydedildiğini açıkla.
3. W4'te açılış defteri geldiğinde hangi toplamın hareketlerle uzlaşması gerekeceğini söyle.

**W4 başlangıç cümlesi:** “Ayırdığımız malzemeyi artık gerçekten hareket ettireceğiz: satınalmadan mal kabule, iş emrinden mamul kabule ve kısmi sevkiyata; sonra bu veriyi yedekten geri yükleyip aynı toplamları arayacağız.”

## 12. English student handout — Week 3

**Focus:** Build M4–M6 (functional dependencies and anomalies, transactions and concurrency, storage/index and query plans) in depth; M1–M3 get only a brief callback and M7–M8 stay on the map for Week 4.

**Your task:** Turn the reservation path into a correct transaction. Start from the supplied faulty check-then-write version and show why it fails: with 10 m³ on hand and no reservations, two sessions each requesting 8 m³ can both succeed and leave 16 reserved. Then implement the protocol: record the request key and payload hash, lock all affected stock positions in one deterministic order with `SELECT ... FOR UPDATE`, re-check availability under the lock, apply all-or-nothing, and write reservations, balances, the command result and the outbox event in the same transaction.

**Required evidence:** After the correct protocol only one request is accepted, leaving reserved = 8 and available = 2. A multi-line request whose last line is short must reserve nothing. The same key with the same payload returns the stored result; the same key with a different payload is a conflict. A rejected request must also have a recorded, repeatable result. Show that after a mid-transaction failure the balances are unchanged.

**Also this week:** Find the functional dependencies in the supplied denormalized staging table, demonstrate one update, insert or delete anomaly, apply a justified decomposition and reconcile source, target and rejected row counts. Take before/after `EXPLAIN` output for one query on the enlarged seed and report the index's write and storage cost together with the measurement environment.

**Preparation questions:** Why is select-then-update insufficient? Can available quantity be read from a single row? Should you reserve partially? Why record a rejected result? How do you avoid deadlock? Is adding an index free?

**Submission:** Migration/code difference, the D1–D11 results with unchanged-row evidence, the protocol diagram with lock order and failure paths, the FD analysis and reconciliation, both plans with the measurement environment, the transaction-boundary map and the updated M1–M8 map. State clearly that this guided experiment does not replace the independent concurrency acceptance in Week 9.

**Feedback:** 30 points for the idempotent all-or-nothing protocol, 20 for the two-session experiment and repeat-key evidence, 20 for the dependency analysis, 15 for plan interpretation and index cost, and 15 for the boundary map and reproducibility. This rubric does not add a course-grade component.
