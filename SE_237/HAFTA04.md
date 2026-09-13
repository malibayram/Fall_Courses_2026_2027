# SE 237 Object Oriented Programming — 4. Hafta Öğretim Dosyası

## Haftanın kimliği

| Alan | Plan |
| --- | --- |
| Tema | Entegrasyon: satınalmadan sevkiyata çalışan, test edilmiş ve savunulabilir `v0.1` |
| Ana soru | Aynı 100 sandalyelik talebin malzemeye, üretime ve sevkiyata dönüştüğünü; geçmişin bozulmadığını nasıl kanıtlarız? |
| Süre | 155 dakika: 125 dakika etkin öğrenme + üç adet 10 dakikalık ara |
| Başlangıç | W3 aday sürümü; çalışan rezervasyon dilimi ve W1–W3 regresyonu |
| Haftanın ürünü | 100 sandalye ana akışı, 60+40 sevkiyat, snapshot/koleksiyon/kaynak kanıtları ve `v0.1` sürüm belgeleri |
| Sonraki bağlantı | W5'te A1 derinleşmesi: varyant ve BOM revizyonu ayrıştırması |
| Belge durumu | Öğretim ve kabul sözleşmesi; süreç iskeleti, kalıcılık adaptörü ve provider paketleri ayrıca hazırlanıp doğrulanacaktır |

**Bağlantılar:** [Ana ders sistemi](../README.md) · [ERP öğrenci rehberi](ERP_OGRENCI_REHBERI.md) · [W1](HAFTA01.md) · [W2](HAFTA02.md) · [W3](HAFTA03.md) · [Kaynakça](KAYNAKCA.md)

Bu dosya eğitmen/asistan planıdır. **Bu hafta A7–A10 ayrıntılı işlenir** ve ikinci tur tamamlanır; A1–A6 yalnız kısa geri çağırmayla kullanılır. `v0.1`, on çapanın tamamının öğrenci tarafından uygulandığı anlamına gelmez; ilk çalışır fabrika akışı ve sonraki on artımın açık başlangıç noktasıdır.

## 1. Öğretim amacı ve kazanımlar

W3'te bir talebi ayırdık. W4'te bu talebi gerçek harekete dönüştürüyoruz: eksik ahşap satın alınıp kabul ediliyor, iş emri malzemeyi tüketiyor, mamul kabul ediliyor ve sipariş iki parçada sevk ediliyor. Aynı anda geçmişin korunduğunu gösteriyoruz: yeni reçete revizyonu eski iş emrini değiştirmiyor, yeni fiyat listesi eski siparişin tutarını bozmuyor.

| Kod | Öğrenci ders sonunda… | Kanıt |
| --- | --- | --- |
| W4-K1 | Eksik malzemeyi satınalma ve mal kabulle kullanılabilir stoğa çevirir. | Ahşap 3 → 4 m³; mal kabul öncesi kullanılamaz |
| W4-K2 | Malzeme çıkışı ve mamul kabulünü çift tüketim yapmadan bağlar. | Tek tüketim; WIP kapanışının açıklanması |
| W4-K3 | Siparişi 60+40 sevk eder ve fazlasını reddeder. | 101'inci adedin reddi |
| W4-K4 | Sipariş fiyatı ve iş emri reçetesini snapshot olarak korur. | R2 sonrası R1 iş emrinin değişmemesi |
| W4-K5 | Tür güvenli koleksiyon ve deterministik sorgu sonucu üretir. | Açık iş emri listesi; tenant'ların karışmaması |
| W4-K6 | Hata ve kaynak ömrünü kontrollü yönetir. | Bozuk içe aktarımda kısmi state yayınlanmaması |
| W4-K7 | Yapılandırmadan onaylı bir provider seçer. | Sağlanan provider paketiyle çalışan seçim ve hata yolu |
| W4-K8 | İkinci turun tamamlandığını harita üzerinde savunur. | A1–A10 haritası, backlog ve kısa sözlü |

## 2. `v0.1` kapsam sınırı

| Şerit | İçerik |
| --- | --- |
| Core | Satınalma/mal kabul, malzeme çıkışı, mamul kabul, 60+40 sevkiyat, snapshot koruması, tür güvenli sorgu, kaynak/hata yolu, provider seçimi, sürüm belgeleri |
| Sağlanan altyapı | Süreç iskeleti, kalıcılık/işlem adaptörü, dosya içe aktarım örneği, iki provider paketi, kabul harness'i, basit maliyet hesabı |
| Instructor demo | Sızdırılan koleksiyonla bozulan geçmiş; `BigDecimal` scale farkı; kapatılmayan dosya kaynağı |
| Stretch | İkinci bir kanal için sevk etiketi veya ek bir rapor sorgusu |
| Kapsam dışı | Gerçek banka/e-belge/pazaryeri bağlantısı, çok kullanıcılı eşzamanlılık garantisi, tam genel muhasebe, bordro, gerçek plugin güvenlik sandbox'ı |

Kabul, sağlanan sonlu fixture üzerinde tek iş parçacıklı çalışma içindir. **Eşzamanlılık ve dayanıklılık garantisi verilmez;** bu sınır release belgesinde görünür.

## 3. Eğitmenin hazırlığı ve kaynaklar

### Paket ve test donanımı

En az beş gün önce yayımlanacak:

- 20–30 dakikalık entegrasyon videosu ve eşdeğer İngilizce içerik.
- Sabit W3 aday tabanı ve W4 TODO listesi; yeni proje iskeleti açılmaz.
- Satınalma → mal kabul, iş emri → malzeme çıkışı → mamul kabul, sipariş → kısmi sevk süreç iskeletleri.
- `R2` revizyonu fixture'ı: yayımlandığında eski `R1` iş emrinin ihtiyacını değiştirmemelidir.
- Fiyat listesi güncellemesi fixture'ı: onaylanmış siparişin tutarını değiştirmemelidir.
- Bozuk satır içeren içe aktarım dosyası ve kaynak kapanışını izleyen test.
- İki provider paketi (`ServiceLoader` ile keşfedilen) ve bir hatalı/çift kimlikli paket.
- Basit hareketli ağırlıklı ortalama maliyet uygulaması; 1.050 TL birim maliyeti üretir.
- Kabul formu, sözlü soru kartları ve 100 puanlık ürün rubriği.

### Ortak sayısal beklenti

| Aşama | Beklenen sonuç |
| --- | --- |
| Sipariş | 100 adet CHAIR-A, R1 |
| Eksik | WOOD-A 1 m³ (3 mevcut, 4 gerekli) |
| Satınalma + mal kabul | WOOD-A 4 m³ kullanılabilir |
| Malzeme çıkışı | WOOD 4; FABRIC 150; FOAM 100; VARNISH 10 |
| Mamul kabul | 100 sağlam CHAIR-A |
| Kalan stok | WOOD 0; FABRIC 0; VARNISH 0; **FOAM 20** |
| Sevkiyat | Önce 60, sonra 40; 101'inci adet reddedilir |
| Maliyet | 105.000 TL toplam; birim 1.050 TL |

Bu sayılar sağlanan fixture'dan üretilir. Öğrenci farklı bir sonuç alıyorsa önce fixture ve birim dönüşümü kontrol edilir; ortak örnek sessizce değiştirilmez.

## 4. Ders öncesi öğrenci hazırlığı

Toplam hedef yaklaşık 45–60 dakika. W3 aday sürümünü değiştirmeden test et; eksikleri `davranış / test / belge / ortam` başlıklarıyla ayır. Kabul formunu okuyup “geçiyor / henüz kanıt yok / başarısız” ön değerlendirmesi yap. Yanıtlar dersten 12 saat önce teslim edilir.

| Soru | Beklenen cevap yönü |
| --- | --- |
| 1. Açık satınalma siparişi kullanılabilir stok mudur? | Hayır; beklenen tedariktir. Mal kabul olmadan fiziksel stoğa eklenmez. |
| 2. `R2` yayımlanınca `R1` iş emrinin ihtiyacı değişir mi? | Hayır; iş emri kullandığı revizyonu sabitler. |
| 3. Fiyat listesi güncellenirse onaylı siparişin tutarı ne olur? | Değişmez; sipariş fiyat snapshot'ı taşır. |
| 4. `Money("10.0")` ile `Money("10.00")` aynı değer mi? | Domain'de aynıysa `equals`/`hashCode` bunu korumalı; scale normalize edilmeli. |
| 5. Mamul kabulünde hammadde ikinci kez düşülmeli mi? | Hayır; tüketim malzeme çıkışında olmuştur. |
| 6. `A9` satırına “kalıcılık tamamlandı” yazabilir miyiz? | Ancak sağlanan adaptör sınırı belirtilerek; gerçek dayanıklılık kanıtı W13'tedir. |

## 5. Snapshot, koleksiyon ve kaynak sözleşmesi

### A7 — Kimlik, eşitlik ve snapshot

```text
Değer nesneleri:   Money(amount, currency), Quantity(value, unit), ProductId(tenant, sku)
Snapshot noktaları:
  SalesOrderLine.unitPrice   → onay anında sabitlenir
  WorkOrder.bomSnapshot      → serbest bırakma anında sabitlenir

Kurallar:
  - equals/hashCode birlikte tanımlanır ve domain eşitliğini korur
  - TRY ile EUR kur bilgisi olmadan toplanmaz
  - dışarı verilen koleksiyon iç state'i değiştiremez
```

`BigDecimal.equals` scale'i de karşılaştırır: `10.0` ile `10.00` eşit değildir. Domain aynı değeri kastediyorsa `Money` bunu normalize ederek yönetir; yoksa aynı tutar iki farklı anahtar üretir ve rapor toplamları bozulur.

### A8 — Tür güvenli koleksiyon ve sorgu

```text
Repository<T, ID>          tür güvenli erişim
Map<ProductId, Position>   kimlikle erişim
Set<ReservationId>         tekillik
List<WorkOrder>            deterministik sıralı rapor sonucu
```

Yanlış tür derleme sınırında yakalanır. Rapor sırası deterministiktir; aynı fixture aynı sırayı verir. Farklı tenant kayıtları aynı sonuç kümesinde karışmaz.

### A9 — Hata ve kaynak ömrü

Dosya içe aktarımı `try-with-resources` ile kapatılır. Bozuk bir satır bulunduğunda **kısmi state yayınlanmaz**: ya tamamı kabul edilir ya da hiçbiri. Dış gönderim başarısız olursa domain sonucu ile dış gönderim sonucu ayrı raporlanır; “gönderildi” varsayılmaz.

### A10 — Runtime provider seçimi

Sağlanan iki provider paketi `ServiceLoader` ile keşfedilir; yapılandırma yalnız **onaylı** kimliği seçer. Provider yoksa, kimlik çiftse veya yanıt hatalıysa iş akışı kontrollü hata verir. Bu mekanizma güvenlik sandbox'ı sağlamaz; keyfi kod yükleme bu dersin kapsamı değildir.

### A7–A10 entegrasyon turu — bu haftanın odağı

| Çapa | `v0.1` içinde korunacak ilişki | Bu haftaki kabul kanıtı | Sonraki artım |
| --- | --- | --- | --- |
| A7 | Sipariş fiyatı ve iş emri reçetesi snapshot'ı | R2 sonrası R1 değişmez; fiyat güncellemesi eski siparişi bozmaz | W11 değer nesneleri ve immutability |
| A8 | Tür güvenli repository ve deterministik rapor | Açık iş emri listesi; tenant ayrımı | W12 generics ve modüller arası sorgu |
| A9 | Kaynak kapanışı ve kısmi state yayınlamama | Bozuk içe aktarım testi; ayrı raporlanan dış gönderim | W13 kalıcılık ve outbox/retry |
| A10 | Yapılandırmadan onaylı provider seçimi | Provider yok / çift kimlik / hatalı yanıt yolları | W14 plugin ve AI öneri onayı |

### A1–A6 kısa geri çağırma

| Çapa | Bu haftaki kısa bağlantı |
| --- | --- |
| A1 | Satınalma, üretim ve sevkiyat ayrı servislerde kalır; tek sınıfta toplanmaz. |
| A2 | Miktar ve durum geçişi invariant'ları yeni akışlarda da korunur. |
| A3 | İş emri → malzeme çıkışı → mamul → sevk zinciri ilişki diyagramında görünür. |
| A4 | W3'ün denetim sözleşmesi sevkiyat öncesinde de kullanılır. |
| A5 | Kargo/kanal portları aynı sınırdan çağrılır. |
| A6 | Maliyet ve fiyat politikası collaborator olarak kalır; W10'da derinleşir. |

İkinci tur bu haftayla tamamlanır. Kümülatif harita bunu kaydeder; yeniden tam anlatım yapılmaz.

## 6. 155 dakikalık ders akışı

| Süre | Öğretmen hamlesi | Öğrenci işi ve kontrol noktası |
| --- | --- | --- |
| 00–08 | W3'ün rezervasyon sonuçlarını kısaca geri çağır (A1–A6); tam zinciri referans sürümde göster. | Hangi adımda fiziksel stok ilk kez değişir? |
| 08–20 | **A7:** Sipariş fiyatı ve iş emri reçetesi snapshot'ı; R2 yayımlandığında R1'in korunması. | Snapshot alınma anını işaretle. |
| 20–30 | **A7 karşı örneği:** `BigDecimal` scale farkı ve sızdırılan koleksiyonla bozulan geçmiş. | İki `Money` eşit mi; neden? |
| 30–40 | Ara | |
| 40–55 | **A8:** Tür güvenli repository; `Map` ile kimlik, `Set` ile tekillik; deterministik rapor sırası ve tenant ayrımı. | Açık iş emri sorgusunun imzası. |
| 55–70 | **A9:** `try-with-resources`; bozuk içe aktarımda kısmi state yayınlamama; dış gönderim sonucunun ayrı raporlanması. | Hangi state yayınlanmamalı? |
| 70–80 | Ara | |
| 80–95 | **A10:** `ServiceLoader` ile provider keşfi; yapılandırmadan onaylı seçim; yok/çift/hatalı yolları. | Üç hata yolunun sonucu. |
| 95–110 | Kabul formunu örnek eksik teslim üzerinde uygula; A1–A10 haritasını ikinci turun tamamlanmasıyla kapat. | Geçti / başarısız / kanıt yok kararları. |
| 110–120 | Ara | |
| 120–126 | Studio başlangıcı: bireysel AI'sız zincir tahmini. | Hangi adım hangi stoğu değiştirir? |
| 126–138 | Satınalma/mal kabul ve malzeme çıkışı/mamul kabul adımlarını tamamlat. | Çift tüketim yok; kalan FOAM 20. |
| 138–150 | 60+40 sevkiyatı, 101'inci adet reddini ve tekrarlanan üretim tamamlamayı çalıştır. | İlk kabul raporu; roller değişir. |
| 150–155 | Exit ticket, release durumu ve W5 köprüsü. | Son eksik ve sonraki davranış kaydı. |

Temiz kopya ve araçlar önceden hazırdır; ders içinde bağımlılık indirilmez. Studio'da eğitmen ve iki asistan aynı kabul formunu kullanır. Her öğrenci yazılı kısa savunma verir; puanlanan sözlüler dönem kapsama planına göre ayrı zamanlarda seçilir.

## 7. `v0.1` test kataloğu

| Test | Senaryo | Beklenen sonuç ve kanıt |
| --- | --- | --- |
| W4-T1 | 100 sandalye planı | WOOD 4 / FABRIC 150 / FOAM 100 / VARNISH 10; eksik WOOD 1 (rehber O01) |
| W4-T2 | Satınalma ve mal kabul | Kabul öncesi kullanılamaz; sonra WOOD 4 m³ |
| W4-T3 | İş emri malzeme çıkışı | Tek tüketim; WIP kapanışı açıklanır (rehber O07) |
| W4-T4 | Mamul kabul | 100 sağlam CHAIR-A; hammadde ikinci kez düşülmez |
| W4-T5 | Kalan stok | WOOD 0; FABRIC 0; VARNISH 0; FOAM 20 |
| W4-T6 | 60 + 40 sevkiyat | İkisi de geçer; sipariş `SHIPPED` |
| W4-T7 | 101'inci adet sevk | Reddedilir (rehber O08) |
| W4-T8 | Tekrarlanan üretim tamamlama | İkinci mamul stoğu oluşmaz |
| W4-T9 | `R2` yayımlama | `R1` iş emrinin ihtiyacı ve maliyeti korunur (rehber O06) |
| W4-T10 | Fiyat listesi güncelleme | Onaylı siparişin tutarı değişmez |
| W4-T11 | Koleksiyon/snapshot sızıntısı | Dış mutation iç state'i değiştirmez (rehber O12) |
| W4-T12 | Bozuk içe aktarım dosyası | Kaynak kapalı; kısmi state yayınlanmamış (rehber O14) |
| W4-T13 | Provider yok / çift kimlik / hatalı yanıt | Üç yol da kontrollü hata verir |
| W4-T14 | Maliyet hesabı | 105.000 TL toplam; birim 1.050 TL; çift gider yok (rehber O11) |
| W4-T15 | W1–W3 regresyonu | Plan, invariant, rezervasyon ve tekrar anahtarı davranışları korunur |

T12–T13 düzenekleri hazır verilir; öğrenci hata dallarını ve sonucu yorumlar. Testlerde gerçek saat, rastgele kimlik veya internet zorunlu değildir.

## 8. Release paketi ve yeniden üretim

Öğrencinin ürünü teslim alındığında şu kayıtlar bulunur:

```text
README.md        Kurulum, çalıştırma komutları, ana senaryo ve sonuç sözleşmesi
BASELINE.md      W3 başlangıcı, supplied/student ayrımı
CHANGELOG.md     W4 değişiklikleri ve giderilen hatalar
KNOWN_LIMITS.md  Tek-thread sınırı, bellek içi kalıcılık, kapsam dışı modüller
AI_USAGE.md      Araç/kaynak katkısı ve doğrulama
src/             Domain, application, ports, adapters
src/test/        Sağlanan ve öğrenci testlerinin açık ayrımı
fixtures/        Ürün, BOM, stok, fiyat ve içe aktarım verisi
docs/            A1–A10 haritası, diyagramlar, kabul tablosu ve karar kaydı
```

### Yeniden üretim kontrolü

1. Teslimdeki kesin sürümü aç; eski `target/` çıktısını kullanma.
2. Belgelenmiş Java sürümünü doğrula; `./mvnw test` çalıştır.
3. Fixture'ları değiştirmeden ana senaryoyu koştur.
4. Sonuçları ders test kimlikleriyle eşleştir; başarısız test saklanmaz.
5. Haritadaki her `I` iddiasının kanıtını, her `F` alanının hedef haftasını bul.
6. Release durumunu `kabul edildi / düzeltme gerekli` olarak kaydet ve gerekçeyi yaz.

Bir komutun başarı kodu, ancak testlerin gerçekten koştuğunu ve assertion'ların değerlendirildiğini gösteren çıktıyla birlikte kullanılır.

## 9. Ürün kabulü ve değerlendirme rubriği

Kabul kapısı ile puan ayrı kaydedilir. Ana zincir, çift tüketim veya sevk sınırı yanlışsa ürün `v0.1` kapısını geçmez; öğrenci doğru kalan açıklama ve test tasarımı ölçütlerinden geri bildirim/puan alabilir.

| Ölçüt | Beklenen kanıt | Puan |
| --- | --- | ---: |
| Uçtan uca fabrika zinciri | Satınalma → üretim → 60+40 sevkiyat; kalan FOAM 20 | 30 |
| Snapshot ve geçmişin korunması (A7) | R2 ve fiyat güncellemesi sonrası değişmeyen kayıtlar | 20 |
| Tür güvenli koleksiyon ve sorgu (A8) | Deterministik sonuç; tenant ayrımı | 15 |
| Hata, kaynak ve provider yolları (A9/A10) | Kısmi state yok; üç provider hata yolu | 20 |
| Sürüm belgeleri, sınırlar ve gerekçe | Bilinen sınırlamalar ve karar kaydı | 15 |
| **Toplam** | | **100** |

Bu rubrik ERP rehberindeki W4 kapısını ayrıntılandırır; ders toplamına yeni yüzde eklemez. W2/W3'te notlandırılan yerel ölçütler tekrar puanlanmaz.

### 90 saniyelik ürün açıklaması

Öğrenci yaklaşık 90 saniyede bir siparişi, malzeme hareketini, sevkiyatı ve bir snapshot kanıtını gösterebilir. Bu, 2–4 dakikalık haftalık videoya ek kayıt gerektirmez.

Bireysel soru kartları:

- Mal kabulden önce ahşap neden kullanılabilir değildi?
- Mamul kabulünde hammadde neden yeniden düşülmüyor?
- R2 yayımlandı; R1 iş emri neden aynı kaldı?
- 101'inci adedin reddini hangi kural sağlıyor?
- Bozuk içe aktarımda hangi state yayınlanmadı?
- Provider kimliği çiftse iş akışı ne yapar?
- Bu sürümde A9 hakkında henüz kanıtlamadığın bir şeyi söyle.

## 10. Teslim, süre ve kurtarma tabanı

Teslim; sürüm kimliği, kod farkı, W4-T1–T15 kabul tablosu, A1–A10 haritası, release belgeleri ve 2–4 dakikalık açıklamayı içerir. Ana teknik metin 1–2 sayfayı hedefler; log ve diyagramlar eklerde tutulur.

Ders dışı Core hedefi: zincir tamamlama 55, sağlanan testleri çalıştırma/yorumlama 40, snapshot/koleksiyon düzeltmeleri 30, harita/release/katkı 30 dakika; toplam yaklaşık 155 dakika. Hazırlık ve en çok 30 dakikalık video ayrıdır.

W4 sonrasında öğretim ekibi test edilmiş bir `v0.1` kurtarma tabanı yayımlar. Bu tabanı alan öğrenci aldığı sürümü belirtir, kendi sürümündeki sorunu ve öğrendiğini açıklar, W5 artımını bu taban üzerinden yapar. Hazır sürümü kopyalayarak geçmiş eksik puan kazanılmış sayılmaz.

## 11. Exit ticket ve W5'e geçiş

Notlar/AI kapalı:

1. Kendi `v0.1` sürümünden bir kabul kanıtı ve bir sınırlama yaz.
2. Rezervasyon ile fiziksel hareket arasındaki farkı bir olayla açıkla.
3. W5'te varyant eklenince hangi iki regresyonu koruyacağını belirt.

**W5 başlangıç cümlesi:** “Elimizde artık siparişten sevkiyata çalışan bir taban var. Şimdi aynı sandalyenin farklı kumaş ve renk varyantlarını ekleyecek; sorumlulukları ve reçete revizyonlarını bu talebe göre yeniden ayıracağız.”

## 12. English student handout — Week 4

**Focus:** Integrate the factory chain into a reproducible `v0.1` release. Build A7–A10 (identity/equality/snapshots, generics and collections, errors and resource lifetime, runtime provider selection) in depth; A1–A6 get only a brief callback, completing the second pass across Weeks 2–4.

**Your task:** Extend the Week 3 candidate so the shortage of 1 m³ of wood becomes a purchase order and a goods receipt, the work order issues materials and receives finished goods, and the order ships as 60 then 40. Open purchase orders are expected supply, not available stock. Finished-goods receipt must not deduct raw materials a second time.

**Acceptance evidence:** Provide the W4-T1–T15 matrix. Wood ends at 0, fabric at 0, varnish at 0 and foam at 20. A 101st unit must be rejected. Publishing revision R2 must leave the R1 work order's requirement and cost unchanged, and a price-list update must leave a confirmed order's total unchanged. A repeated production-completion request must not create a second finished-goods entry.

**Contract work:** Keep value objects consistent — `BigDecimal` compares scale, so `10.0` and `10.00` are not equal unless your `Money` normalizes them. Return collections that cannot mutate internal state. Close file resources with try-with-resources and publish no partial state from a broken import. Discover the supplied providers with `ServiceLoader` and select only an approved id from configuration; handle missing, duplicate and failing providers.

**Preparation questions:** Is an open purchase order available stock? Does R2 change an existing R1 work order? What happens to a confirmed order's total when prices change? Are `Money("10.0")` and `Money("10.00")` the same value? Should finished-goods receipt deduct raw materials again? What may you honestly claim about persistence this week?

**Submission:** A fixed release identity, code and baseline record, commands, acceptance results, the A1–A10 map showing the completed second pass, changelog, known limits (single-threaded, in-memory) and contribution disclosure, plus a 2–4 minute explanation covering one order and one snapshot proof.

**Rubric:** 30 points for the end-to-end chain, 20 for snapshots and preserved history, 15 for type-safe collections and queries, 20 for error/resource/provider paths and 15 for release documentation and reasoning. This is the Week 4 product rubric, not an added course-grade percentage. A failed acceptance gate still permits criterion-level feedback and correction.
