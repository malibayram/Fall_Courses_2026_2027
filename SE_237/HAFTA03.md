# SE 237 Object Oriented Programming — 3. Hafta Öğretim Dosyası

## Haftanın kimliği

| Alan | Plan |
| --- | --- |
| Tema | Fabrikanın davranışı: sözleşmeli denetimler, portlar ve seçilebilir politika |
| Ana soru | Malzeme ayırma kararını, farklı denetimleri ve dış kanalları aynı iş akışına nasıl bağlarız? |
| Süre | 155 dakika: 125 dakika etkin öğrenme + üç adet 10 dakikalık ara |
| Başlangıç | W2 iskeleti; ürün–reçete–stok servisleri ve geçen W2 testleri |
| Haftanın ürünü | Hep-ya-hiç çalışan rezervasyon dikey dilimi, iki denetim implementasyonu, tekrar anahtarı davranışı, W4 aday sürümü |
| Sonraki kapı | W4'te satınalma–üretim–sevkiyat zinciri ve `v0.1` kabulü |
| Belge durumu | Öğretim ve uygulama sözleşmesi; starter, işlem sınırı ve contract test iskeleti ayrıca üretilecektir |

**Bağlantılar:** [Ana ders sistemi](../README.md) · [Güncel kapsam](GUNCEL_KAPSAM.md) · [ERP öğrenci rehberi](ERP_OGRENCI_REHBERI.md) · [W1](HAFTA01.md) · [W2](HAFTA02.md) · [W4](HAFTA04.md) · [Kaynakça](KAYNAKCA.md)

Dosya öğretim ekibi içindir. Öğrenci görev özeti sonda İngilizce verilmiştir. Güncel proje sözleşmesi [ERP rehberidir](ERP_OGRENCI_REHBERI.md).

## 1. Öğretim amacı ve kazanımlar

W2'de “hangi karar kimin?” sorusunu yanıtladık. Bu hafta aynı yapıya gerçek bir talep gönderiyoruz: malzeme ayrılacak, bazı istekler reddedilecek, aynı istek iki kez gelebilecek ve aynı iş akışı farklı denetim/kanal implementasyonlarıyla çalışacak.

**Bu hafta A4–A6 ayrıntılı işlenir:** A4 alt tür sözleşmesi ve yerine kullanılabilirlik, A5 interface/port ve dış sistem sınırı, A6 composition/Strategy ile seçilebilir politika. A1–A3 yalnız kısa geri çağırmayla kullanılır; A7–A10 W4'e bırakılır ve haritada `F` kalır.

| Kod | Öğrenci ders sonunda… | Kanıt |
| --- | --- | --- |
| W3-K1 | Yan etkisiz `evaluate(context) → Decision` sözleşmesini tanımlar. | Soyut denetim ve iki implementasyon |
| W3-K2 | İki implementasyonun aynı contract testinden geçtiğini gösterir. | Ortak test sınıfı; her iki tür için geçen koşu |
| W3-K3 | Sözleşmeyi bozan alt türü gerekçesiyle reddeder. | State değiştiren veya yeni önkoşul ekleyen karşı örnek |
| W3-K4 | Domain'i dış format ve taşımadan ayıran port tanımlar. | Rezervasyon/kanal portu; domain'de HTTP/JSON yok |
| W3-K5 | Seçilebilir uygunluk politikasını collaborator olarak bağlar. | İki politika; sert kuralların devre dışı kalmaması |
| W3-K6 | Hep-ya-hiç rezervasyonu uygular; kısmi ayırma yapmaz. | Eksik malzeme testinde hiçbir kalemin ayrılmaması |
| W3-K7 | Aynı anahtarın aynı/farklı içerikle tekrarını ayırır. | Tek iş etkisi ve çatışma reddi testleri |

## 2. Kapsamı sınırlama

| Şerit | İçerik |
| --- | --- |
| Core | Hep-ya-hiç rezervasyon; soyut denetim ve iki implementasyon; bir port; bir seçilebilir politika; tekrar anahtarı sonucu |
| Sağlanan altyapı | İşlem/unit-of-work sınırı, request kaydı iskeleti, iki küçük denetim implementasyonu, sahte kanal girdisi, contract test taslağı |
| Instructor demo | Sözleşmeyi bozan alt tür; domain'e sızmış JSON; politikanın sert kuralı devre dışı bırakma denemesi |
| Stretch | Üçüncü bir denetim implementasyonu veya ikinci sahte kanal; zorunlu değildir |
| W4'e kalan | Satınalma/mal kabul, malzeme çıkışı, mamul kabul, kısmi sevkiyat, `v0.1` kabulü |
| Derinleşmeye kalan | Gerçek kalıtım hiyerarşisi W8; port ailesi ve entegrasyon W9; fiyat/maliyet politikaları W10 |

Rezervasyon bu hafta bellek içi ve tek iş parçacıklıdır. **Tek-thread doğruluğu çok kullanıcılı güvence olarak sunulmaz;** eşzamanlılık ve kalıcılık ayrı kanıt ister ve sonraki haftalara aittir. Bu sınır teslimde açıkça yazılır.

## 3. Eğitmenin hazırlayacağı paket

Paket en az beş gün önce yayımlanır; hazırlık yanıtı dersten 12 saat önce alınır.

- 25–35 dakikalık davranış videosu ve eşdeğer İngilizce not.
- W2 tabanı üzerinde derlenebilir rezervasyon iskeleti; öğrenci TODO'ları kontrol sırası, hep-ya-hiç uygulama ve sonuç eşlemesiyle sınırlıdır.
- Sağlanan `AvailabilityCheck` soyut sözleşmesi ve iki implementasyon: **stok uygunluğu** ve **kalite serbest bırakma**.
- Ortak contract testi taslağı: her implementasyon için yan etkisizlik, aynı girdiye aynı karar, `Decision` alanlarının doldurulması.
- Bilerek bozuk üç referans sürüm: (1) `evaluate` içinde stok yazan alt tür, (2) üst türde olmayan yeni önkoşul isteyen alt tür, (3) `SalesOrder` içinde pazaryeri JSON alanı okuyan domain sınıfı.
- Tekrar anahtarı fixture'ı: aynı `requestId` + aynı payload; aynı `requestId` + farklı payload.
- Sequence diagram şablonu ve I/P/F harita kâğıdı.

### Altyapının sorumluluğu

İşlem sınırı (unit-of-work) ve request kaydı öğretim ekibi tarafından sağlanır ve kendi testleriyle doğrulanır. Öğrenci bu hafta kalıcılık, kilitleme veya gerçek transaction yöneticisi yazmaz; birden çok nesneyi güncelleyen komutun **tek sınırda** başarılı olması veya bütünüyle geri alınması gerektiğini gösterir ve bu sınırı kullanır.

## 4. Ders öncesi çalışma ve soru anahtarı

Toplam hazırlık hedefi yaklaşık 55–70 dakika. Seçili okuma: [dev.java — Interfaces](https://dev.java/learn/interfaces/); [ERP rehberinin](ERP_OGRENCI_REHBERI.md) 2, 3, 6 ve 8 numaralı ortak iş kuralları. Tüm Java tür sistemi bu haftanın ödevi değildir.

| Hazırlık sorusu | Beklenen yön |
| --- | --- |
| 1. Rezervasyon fiziksel stoğu azaltır mı? | Hayır; yalnız kullanılabilir miktarı düşürür. Fiziksel azalma çıkış/sevk anındadır. |
| 2. Dört kalemin üçü yeterliyse kısmen ayırabilir miyiz? | Hayır; hiçbir kalem ayrılmaz, istek reddedilir. |
| 3. Aynı `requestId` aynı içerikle tekrar gelirse? | Önceki sonuç döner; ikinci rezervasyon oluşmaz. |
| 4. Aynı `requestId` farklı içerikle gelirse? | Çatışma olarak reddedilir; sessizce üzerine yazılmaz. |
| 5. Alt tür derleniyorsa yerine kullanılabilir mi? | Hayır; çağıranın davranış beklentisini ve sözleşmeyi de korumalıdır. |
| 6. Uygunluk politikası kapasite kuralını gevşetebilir mi? | Hayır; politika seçilebilir kısmı değiştirir, sert invariant'ı devre dışı bırakamaz. |

İlk kısa tahmin bireysel yazılır. Studio ilk denemesi, puanlanan quiz ve sözlü kontroller AI'sızdır.

## 5. Teknik sözleşme: bir talebi ayırmak

### Rezervasyon sözleşmesi

```text
ReservationRequest = tenant + requestId + demand(product, revision, qty) + policyId
reserve(request) → ReservationResult

Önkoşul: geçerli tenant; qty > 0; yayınlanmış revizyon
Kural:   kullanılabilir = serbest fiziksel − aktif rezervasyon
Kural:   hep ya da hiç; kısmi ayırma yok
Kural:   fiziksel miktar değişmez
Sonuç:   ACCEPTED | INSUFFICIENT | CONFLICT | REJECTED_BY_CHECK
```

### Ortak sayısal örnek

Başlangıç: WOOD-A 3 m³, FABRIC-A 150 m, FOAM-A 120 adet, VARNISH-A 10 kg; hiçbir aktif rezervasyon yok.

**75 sandalyelik talep** (R1 üzerinden): WOOD 3 m³, FABRIC 112,5 m, FOAM 75 adet, VARNISH 7,5 kg. Hepsi karşılanır.

| Ürün | Fiziksel (değişmez) | Aktif rezervasyon | Kullanılabilir |
| --- | ---: | ---: | ---: |
| WOOD-A | 3 | 3 | 0 |
| FABRIC-A | 150 | 112,5 | 37,5 |
| FOAM-A | 120 | 75 | 45 |
| VARNISH-A | 10 | 7,5 | 2,5 |

Bu kabulden sonra **tek bir sandalyelik ikinci talep** bile reddedilir: WOOD kullanılabilir 0'dır. Fiziksel miktar hâlâ 3 m³ olduğu hâlde reddedilmesi, rezervasyonun hareket olmadığını gösteren en iyi sınıf örneğidir.

**100 sandalyelik talep** doğrudan gelirse WOOD 4 m³ gerekir; 3 m³ vardır. Eksik tek kalem olsa da **hiçbir kalem ayrılmaz**; kumaş, sünger ve vernik de serbest kalır.

### Yan etkisiz denetim sözleşmesi

```text
abstract Decision evaluate(CheckContext context)

Sözleşme:
  - yan etki yok: hiçbir state değiştirilmez
  - aynı context → aynı Decision
  - Decision: izin | ret + gerekçe kodu
  - alt tür yeni önkoşul ekleyemez, üst türün reddettiğini kabule çeviremez
```

Sağlanan iki implementasyon: **StockAvailabilityCheck** (kullanılabilir miktar yeterli mi?) ve **QualityReleaseCheck** (lot karantinadan serbest mi?). İkisi de aynı contract testinden geçer. Aynı mekanizma W8'de gerçek kalıtım tartışmasıyla derinleşir.

### Port sınırı

```text
domain  ──uses──▶  ReservationPort      (ayırma isteği)
domain  ──uses──▶  ChannelPort          (dış sipariş girdisi)
adapters ──implements──▶ her iki port

Domain HTTP, JSON, SQL veya pazaryeri alan adı bilmez.
Dış SKU → iç varyant eşlemesi adaptörün işidir.
```

### A4–A6 davranış turu — bu haftanın odağı

| Çapa | İzlenen davranış | Hata/karşı örnek | Bu hafta kanıtı |
| --- | --- | --- | --- |
| A4 — Alt tür sözleşmesi | İki denetim aynı `evaluate` sözleşmesiyle çağrılır. | `evaluate` içinde stok yazan veya yeni önkoşul isteyen alt tür. | Ortak contract testi; reddedilen karşı örnek |
| A5 — Interface ve dış sınır | Talep bir port üzerinden gelir; domain formatı bilmez. | Domain sınıfında pazaryeri JSON alanı okumak. | Port arayüzü; sahte kanaldan gelen eş talep |
| A6 — Composition ve Strategy | Uygunluk politikası collaborator olarak seçilir. | Politikanın kapasite/tekillik gibi sert kuralı gevşetmesi. | İki politika; sert kuralların korunduğu test |

### A1–A3 kısa geri çağırma, A7–A10 bekleyen

A1 (sorumluluk), A2 (invariant) ve A3 (ilişkiler) W2'de işlendi; bu hafta yalnız “bu kural hangi nesnede korunuyordu?” düzeyinde birer cümleyle hatırlatılır. A7 (kimlik/eşitlik/snapshot), A8 (generics/collections), A9 (kaynak/hata) ve A10 (runtime seçim) W4'te ele alınacaktır; haritada `F` kalır.

## 6. 155 dakikalık ders akışı

| Süre | Öğretmen hamlesi | Öğrenci işi ve kontrol noktası |
| --- | --- | --- |
| 00–08 | W2 sorumluluk haritasını kısaca geri çağır; 75'lik talebin kabulünü, ardından 1 adetlik talebin reddini göster. | Fiziksel 3 iken neden reddedildi? |
| 08–20 | **A4:** Yan etkisiz `evaluate(context) → Decision` sözleşmesi; iki implementasyonun aynı mekanizmadan çalışması. | Sözleşmenin dört maddesini yaz. |
| 20–30 | **A4 karşı örneği:** Stok yazan ve yeni önkoşul ekleyen alt türleri incelet. | Hangi sözleşme maddesi ihlal edildi? |
| 30–40 | Ara | |
| 40–55 | **A5:** Rezervasyon ve kanal portları; çağıranın bilmemesi gerekenler; dış SKU eşlemesinin yeri. | Port arayüzünün imzasını taslakla. |
| 55–70 | **A5 karşı örneği:** Domain sınıfına sızmış pazaryeri JSON alanı; sözleşmenin hata yolunu da kapsaması. | Sızıntıyı adaptöre taşı. |
| 70–80 | Ara | |
| 80–95 | **A6:** Seçilebilir uygunluk politikası; composition ile kalıtım arasındaki tercih; sert kuralların dokunulmazlığı. | İki politika, bir değişmez kural. |
| 95–110 | Hep-ya-hiç akışı ve tekrar anahtarı: kontrol sırası, sonuç türleri, sequence diagram. | Mutlu yol sequence diagram'ı. |
| 110–120 | Ara | |
| 120–125 | Studio başlangıcı: bireysel AI'sız kontrol sırası tahmini. | Hangi kontrol önce çalışmalı? |
| 125–138 | `reserve(request)` akışını tamamlat; kontroller bitmeden state değiştirme. | Mutlu yol ve eksik stok yolu. |
| 138–150 | Aynı anahtar/aynı içerik ve aynı anahtar/farklı içerik testlerini çalıştır. | Dört sonucun tablosu. |
| 150–155 | Exit ticket ve anonim iş yükü yoklaması. | Sonuç türü, sınır ve süre kaydı. |

Eşli çalışmada kodu yazan ve sonucu tahmin eden roller 138. dakikada değişir. Her öğrenci kendi testini ve çıkış yanıtını verir. Kısa sözlüler dönem kapsama kaydına göre ayrı zamanlarda yapılır.

## 7. Çalışılmış örnek: dört sonuç

| Aşama | Kabul | Eksik malzeme | Aynı anahtar/aynı içerik | Aynı anahtar/farklı içerik |
| --- | --- | --- | --- | --- |
| İstek | 75 adet, `req-01` | 100 adet, `req-02` | 75 adet, `req-01` (tekrar) | 80 adet, `req-01` |
| Denetimler | Tümü izin verir | Stok denetimi reddeder | Yeniden çalıştırılmaz | Yeniden çalıştırılmaz |
| State etkisi | Rezervasyon satırları eklenir | Hiçbir kalem ayrılmaz | Yeni kayıt yok | Yeni kayıt yok |
| Sonuç | `ACCEPTED` | `INSUFFICIENT` | Önceki `ACCEPTED` döner | `CONFLICT` |
| Fiziksel stok | Değişmez | Değişmez | Değişmez | Değişmez |

**Çıkarım çalışması:** `INSUFFICIENT` ile `CONFLICT` farklı nedenlerdir ve farklı düzeltme gerektirir. İlki stok durumuyla, ikincisi istemcinin gönderdiği içerikle ilgilidir. İkisini tek bir “hata” sonucunda birleştiren tasarım, çağıranın ne yapacağını belirsiz bırakır.

## 8. Studio ve test sözleşmesi

### Öğrencinin tamamlama sırası

1. W2 baseline'ını kaydet; W2 testlerinin geçtiğini doğrula.
2. Sağlanan iki denetim implementasyonunu incele; ortak contract testini çalıştır.
3. Kontrol sırasını belirle: tenant/girdi doğrulama → tekrar anahtarı → denetimler → ayırma.
4. Bütün kalemleri önce değerlendir; tümü uygunsa tek noktada ayır.
5. Tekrar anahtarı sonucunu kaydet; aynı içerikte önceki sonucu, farklı içerikte çatışmayı döndür.
6. Seçilebilir politikayı collaborator olarak bağla; sert kuralların dışında kalmasını sağla.
7. Dört senaryoyu ve W1–W2 regresyonunu çalıştır.
8. Sequence diagram ile haritayı güncelle; W4 eksiklerini ve aday sürümü kaydet.

### Test matrisi

| Test | Beklenen sonuç | Yanlış uygulamayı açığa çıkaran nokta |
| --- | --- | --- |
| W3-T1: 75 adet rezervasyon | `ACCEPTED`; kullanılabilir WOOD 0; fiziksel 3 | Rezervasyon fiziksel stoğu azaltmaz. |
| W3-T2: ardından 1 adet | `INSUFFICIENT` | Kullanılabilir hesabı rezervasyonu içerir. |
| W3-T3: 100 adet doğrudan | `INSUFFICIENT`; hiçbir kalem ayrılmamış | Kısmi ayırma yapılmaz (rehber O04). |
| W3-T4: `req-01` aynı içerikle tekrar | Önceki sonuç; ikinci kayıt yok | Tek iş etkisi (rehber O05). |
| W3-T5: `req-01` farklı içerikle | `CONFLICT` | Sessiz üzerine yazma yok. |
| W3-T6: iki denetim contract testi | Her ikisi de geçer; yan etki yok | Sözleşme gerçekten ortak (A4). |
| W3-T7: state yazan alt tür | Contract testi başarısız | Yan etkisizlik sınanabilir (A4). |
| W3-T8: sahte kanaldan eş talep | Aynı domain sonucu | Port sınırı çalışıyor (A5). |
| W3-T9: ikinci politika | Sert kurallar hâlâ geçerli | Politika invariant'ı gevşetemez (A6). |
| W3-T10: W2 regresyonu | Plan sonuçları ve invariant testleri korunur | Yeni akış eskiyi bozmamıştır. |

T1–T5 ders içindeki asgari izlerdir. Testlerde gerçek saat ve rastgele kimlik kullanılmaz; `Clock` ve sağlanan ID üretici kullanılır.

## 9. Yanılgılar, kısa sorular ve cevap yönü

| Yanılgı | Öğretmen müdahalesi |
| --- | --- |
| “Rezervasyon stoğu düşürür.” | Fiziksel 3 iken kullanılabilir 0 olan tabloyu çizdir. |
| “Üç kalem yeterliyse onları ayıralım.” | Kısmen ayrılmış malzemenin kimseye yaramadığını ve kilitlenme ürettiğini göster. |
| “Aynı istek iki kez gelirse iki rezervasyon olmalı.” | Ağ tekrarını ve idempotency anahtarını ayırt ettir. |
| “Alt tür derleniyorsa sözleşmeyi koruyor.” | State yazan alt türü contract testinde kırdır. |
| “Interface eklersek bağımlılık çözülür.” | Domain'e sızmış JSON alanını göster; tür sınırı ile davranış sözleşmesini ayır. |
| “Politika varsa her kural değiştirilebilir.” | Kapasite ve tekillik gibi sert kuralların politika dışı olduğunu söylettir. |
| “Tek-thread çalıştı, eşzamanlılık da güvenli.” | Çok kullanıcılı güvencenin ayrı kanıt istediğini ve bu haftanın kapsamı dışında olduğunu yazdır. |

**Bireysel sözlü kartları:** `INSUFFICIENT` ile `CONFLICT` farkını birer olayla açıkla; iki denetimin ortak sözleşmesinin hangi maddesi yan etkiyi yasaklar; portun hangi bilgisi adaptöre ait; politikanın değiştiremeyeceği bir kuralı söyle.

## 10. Teslim ve geri bildirim

1. W2'ye göre kod farkı ve kesin W3 aday sürümü.
2. T1–T5 için istek/denetim/state/sonuç tablosu; T6–T10 sonuçları.
3. Mutlu yol sequence diagram'ı ve port sınırının göründüğü şema.
4. A1–A10 haritası; A4–A6 için I/P kanıtı, A7–A10 için `F` ve hedef hafta.
5. Bir tasarım kararı: “bu denetimi neden alt tür, şu politikayı neden collaborator yaptım?”
6. Bilinen sınırlama (tek-thread, bellek içi), AI/dış katkı açıklaması.
7. 2–4 dakikalık video veya eşdeğer açıklama: bir kabul, bir ret ve bir tekrar anahtarı davranışı.

| Haftalık kalite ölçütü | Puan |
| --- | ---: |
| Hep-ya-hiç rezervasyon akışının doğruluğu | 30 |
| Alt tür sözleşmesi ve contract testi (A4) | 20 |
| Port sınırı ve dış format ayrımı (A5) | 15 |
| Seçilebilir politika ve sert kuralların korunması (A6) | 15 |
| Tekrar anahtarı davranışı ve regresyon kanıtı | 20 |
| **Toplam** | **100** |

Bu ölçek haftalık geri bildirimi yapılandırır; dersin toplam notuna ek yüzde getirmez. W4'te eski testlerin geçmesi entegrasyon koşuludur; W3'ün aynı yerel ölçütü ikinci kez puanlanmaz.

Ders dışı Core hedefi: rezervasyon akışı 60, contract test/port 40, test/kanıt 35, diyagram/karar/belge 20 dakika; toplam yaklaşık 155 dakika. Hazırlık ve en çok 30 dakika video ayrıdır.

## 11. Exit ticket ve W4 köprüsü

Notlar/AI kapalı:

1. Fiziksel stok 3 m³ iken bir talebin neden reddedilebileceğini açıkla.
2. İki denetim implementasyonunun paylaştığı sözleşmenin bir maddesini ve onu sınayan testi yaz.
3. W4'te satınalma ve mal kabul eklenince hangi miktarın ilk kez gerçekten değişeceğini söyle.

**W4 başlangıç cümlesi:** “Ayırdığımız malzemeyi artık gerçekten hareket ettireceğiz: eksik ahşabı satın alıp kabul edecek, iş emriyle tüketecek, mamulü kabul edecek ve 60+40 sevkiyatla ilk sürümü kapatacağız.”

## 12. English student handout — Week 3

**Focus:** Build A4–A6 (subtype contracts and substitutability, interfaces and external boundaries, composition and selectable policy) in depth; A1–A3 get only a brief callback and A7–A10 stay on the map for Week 4.

**Your task:** Extend the Week 2 structure with a working reservation slice. A reservation must be all-or-nothing: if any component is short, nothing is reserved. Reservations never reduce physical stock; they reduce available quantity, where available = free physical − active reservations.

**Required scenarios:** Reserving for 75 chairs succeeds and drives available wood to 0 while physical wood stays at 3 m³; a following one-chair request is therefore rejected. A direct 100-chair request is rejected with no component reserved. The same request id with the same payload returns the earlier result; the same id with a different payload is a conflict.

**Contract work:** Use the supplied side-effect-free `evaluate(context) → Decision` abstraction with its two implementations (stock availability and quality release) and run both through one shared contract test. Keep the domain free of transport and external formats by using a port; external SKU mapping belongs to the adapter. Attach the selectable availability policy as a collaborator — a policy may not switch off hard rules such as capacity or uniqueness.

**Preparation questions:** Does a reservation reduce physical stock? May you reserve partially? What happens on a repeated request id with identical content, and with different content? Does compiling prove substitutability? Can a policy relax a hard invariant?

**Submission:** Code difference, the four main result traces, regression evidence, a happy-path sequence diagram, the anchor map, one design decision, known limitations (single-threaded, in-memory), contribution disclosure and one 2–4 minute explanation. State explicitly that single-threaded correctness is not a multi-user guarantee.

**Feedback:** 30 points for the all-or-nothing flow, 20 for the subtype contract, 15 for the port boundary, 15 for the selectable policy and 20 for repeat-key behavior and regression. This rubric does not add a course-grade component. Target about 155 minutes of out-of-class Core work, with preparation and recording budgeted separately.
