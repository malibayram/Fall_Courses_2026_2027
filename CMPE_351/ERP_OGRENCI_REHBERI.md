# CMPE 351 — Sandalye Fabrikası ERP'si: Öğrenci Veri Sistemi Rehberi

**Ders:** Database Systems / Veritabanı Sistemleri  
**Dönem:** 2026–2027 Güz · 12 hafta · `1 + 3 + 8`  
**Proje:** Factory ERP — üretim, ortak stok ve finans zincirinin veri omurgası  
**Belge sürümü:** 1.0 · 11 Eylül 2026  

**Ders oturumu:** 155 dakika; 125 dakika etkin çalışma ve üç adet 10 dakikalık ara. Hazır altyapı ve sınırlı öğrenci değişikliği bu süreye göre planlanır; süre azalması ek ev ödevine aktarılmaz.

**Hedef kitle:** Bu projeyi modelleyecek, sorgulayacak ve sınayacak öğrenciler

Bu rehber, önceki Campus Learning Hub örneğinin yerine geçen fabrika projesini tanımlar. Türkçe açıklamalarda İngilizce teknik terimler korunmuştur. Resmî değerlendirme dili ilgili izlenceye tabidir. [OOP rehberi](../SE_237/ERP_OGRENCI_REHBERI.md) aynı fabrikanın Java iş mantığını ele alır; bu dersi tamamlamak için o derse katılmanız veya Java uygulaması yazmanız gerekmez.

## 1. Sizden beklenen veri ürünü

Gerçek bir sandalye fabrikasının siparişten hammaddeye, üretimden sevkiyata ve kârlılığa uzanan veri zincirini kuracaksınız. Temel mesele tabloları çoğaltmak değildir: modeliniz aynı hammaddenin iki siparişe birden ayrılmasını önlemeli, kullanılan reçetenin geçmişini korumalı, hatalı bir mamulün kaynak lotunu bulmalı ve gerçek kârlılığı yanlış join veya çift maliyet yüzünden çarpıtmamalıdır.

**Teknik omurga:** PostgreSQL 18, SQL, `psql`, sürümlü migration, sabit seed ve tekrarlanabilir deneyler. İlk hafta küçük bir veri prototipi; W4'te temel fabrika akışı; W12'de transaction, performans, yetki ve kurtarma kanıtlarıyla bütünleşmiş veri ürünü teslim edilir.

Tarayıcı, B2B portalı ve üretim tableti aynı veri servislerini kullanacak hedef mimarinin parçalarıdır. Ders paketindeki ince web/API istemcisi sorgu ve işlem girişlerini gösterir; sizden ayrıca bir frontend framework'ü öğrenmeniz istenmez. Ürün buluta taşınabilecek biçimde yapılandırılır; dönem kanıtları yerel veya dersin test ortamında üretilebilir. Ücretli bulut hesabı gerekmez.

Bu belge uygulanacak tasarımı tarif eder. Starter, veri üretici, istemci, iki oturum deneyi ve dış servis fixture'ları ders paketinde sağlanacaktır; tasarım kararlarınız, migration, SQL ve doğrulama kanıtınız sizin çalışmanızdır.

## 2. Fabrika kapsamı ve teslim derinliği

**Çekirdek:** Çalışan şema, işlem, sorgu ve test. **Bağlantı:** Diğer modülle ilişkili veri modeli ve sağlanan olayla doğrulanan örnek akış. **Genişleme:** Üretim ortamı tasarımı, bağımlılıklar ve risk/karar kaydı. Bütün modüller ürün haritasında bulunur.

| Modül | Veritabanı dersindeki karşılığı | Derinlik |
| --- | --- | --- |
| Ürün/BOM | Tür, varyant, barkod, birim, çok seviyeli ve revizyonlu reçete | Çekirdek |
| Stok/depo | Lot/durum bazlı hareket, bakiye, rezervasyon, transfer, sayım | Çekirdek |
| Üretim/fason | İş emri, malzeme/operasyon, iyi/fire miktarı, parti ve emanet izleri | Çekirdek |
| Satış/satınalma | Kanaldan bağımsız sipariş, talep, kısmi kabul/sevk, iade | Çekirdek |
| Maliyet/rapor | Fiilî maliyet, zamanlı fiyat, komisyon/kargo, kârlılık | Çekirdek |
| B2B/finans | Bayi erişimi, cari hareket, risk, ödeme-onay ve eşleştirme | Çekirdek örnek + bağlantı |
| Pazaryeri/kargo/e-belge/banka | Inbox/outbox, mapping, tekrar, durum ve uzlaştırma | Bağlantı; gerçek sağlayıcılar genişleme |
| Servis | Belge/fotoğraf metadata'sı, hata sınıfı, üretim/lot izi | Çekirdek örnek |
| İK/PDKS/bordro | Personel, vardiya, işçilik girdisi, izin | Bağlantı; bordro/PDKS genişleme |
| AI/otomasyon | Kaynak belge, öneri, güven, onay ve audit | Bağlantı; seçilmiş JSONB/RLS davranışı çekirdek |
| SaaS/operasyon | Tenant, rol, migration, import, backup/restore, replication kararı | Çekirdek kanıt + dağıtım tasarımı |

Dönem örnekleri banka ödemesi veya resmî e-belge üretmez; dış servisler test yanıtlarıyla çalışır. Tam genel muhasebe ve bordronun kapsamı gerçek vaka metninde de karar gerektirir. Bu alanlar veri aktarım sözleşmesi ve ilerideki modül sınırı olarak korunur; mali mevzuat ürünü tamamlanmış gibi gösterilmez.

### Fabrikanın tamamını bağlayan yan süreçler

Trendyol, Hepsiburada ve Amazon, vaka metnindeki hedef kanal örnekleridir; ders connector'ları bu şirketlerin gerçek API'lerinin tamamını uyguladığını iddia etmez. Kendi e-ticaret sitesi ve B2B portalı da aynı sipariş/ürün/rezervasyon sözleşmesine bağlanır. Sürümlü REST API, webhook, SMS/e-posta, banka, sanal POS, e-fatura/e-arşiv/e-irsaliye ve PDKS sınırlarında kimlik eşlemesi, yetki, hata, tekrar ve uzlaştırma ortak tasarım konularıdır.

Finansta çek/senet için tür, düzenleyen taraf, tutar/para birimi, vade, portföy, tahsile verme, tahsil/karşılıksız/iade ve durum geçmişi tasarlanır. Belgeyi portföye almak banka tahsilatı sayılmaz; gerçekleşmiş tahsilat ayrı olay ve eşleştirmedir. Bu alt süreç bağlantı düzeyindedir; sağlanan bir vade/tahsil örneğiyle gösterilir. Tam genel muhasebe kapsamı ve bordro hesabı müşteriyle netleştirilecek kararlar olarak kalır.

Raporlarda tarih, ürün, müşteri, kanal, bölge ve satış temsilcisi filtreleri; kaydedilen rapor tanımı ve zamanlanmış e-posta işi ayrı kavramlardır. Zamanlanmış iş, oluşturulduğu kullanıcının güncel erişim kapsamıyla çalışır; bir bayiye başka bayinin raporunu göndermez. İK'da vardiya, fazla mesai ve puantaj üretime işçilik girdisi sağlar; personel ücret detayları herkese açık üretim raporuna taşınmaz. Bu bağlantılar çekirdek iş kurallarını atlayan ayrı ekranlar olarak kurulamaz.

### Her hafta nasıl çalışacaksınız?

Ders öncesinde kısa kavram kaynağını inceleyin ve çalışan bir önceki sürümde ilgili iş akışını izleyin. Derste önce bir fabrika değişiklik talebinin sonucunu tahmin edin; ardından referans davranışı çalıştırıp tahmininizi sınayın. Küçük bir kod/SQL değişikliği yapın, başarı ve hata durumlarını test edin, sonucu nesne veya veri modeliyle açıklayın. Ders sonrasında geri bildirime göre düzeltin ve aynı ürünün yeni sürümünü teslim edin.

Dersi baştan sona üç turda görürsünüz: W1 bütün haritanın panoraması; W2–W4 konuların üç haftaya dağıtıldığı ikinci tur; W5–W12 sekiz haftalık ayrıntılı üçüncü tur. İkinci turda her hafta yalnız kendi konu grubu işlenir; tüm modüller her hafta yeniden anlatılmaz. Her şeyi ilk haftada bağımsız geliştirmeniz beklenmez. Derinleşme haftalarında bir konu öne çıkar, fakat stok–üretim–satış zinciri ve önceki testler çalışmaya devam eder. Referans çözüm veya kurtarma tabanı kullanırsanız kaynağını belirtin; neyi kendiniz değiştirdiğinizi gösterin. Yalnızca çalışan ekran, ezberlenmiş tanım veya açıklayamadığınız üretilmiş kod yeterli kanıt değildir.

## Fabrika senaryosu ve ortak sayısal örnek

Sandalye üreten bir fabrikanın hammadde–yarı mamul–mamul zincirini dijitalleştiriyorsunuz. Siparişler bayi portalı, mağaza, e-ticaret ve pazaryerlerinden geliyor; üretim malzemeyi depodan kullanıyor, gerektiğinde fasoncuya gönderiyor, mamulü kalite kontrolünden geçirip sevk ediyor. Yönetim yalnız satılan adedi değil, gerçekleşmiş maliyeti ve tahsilatı da görmek istiyor.

Aşağıdaki adlar, miktarlar ve fiyatlar ders için oluşturulmuş **sentetik test verisidir**. Verilen gerçek fabrika ihtiyaç metni süreçlerin kaynağıdır; gerçek işletmenin kesin reçetesi, fiyatı veya müşteri verisi olduğu varsayılmaz.

| Kod | Tür | Temel birim | Başlangıç kullanılabilir miktar |
| --- | --- | --- | ---: |
| CHAIR-A | Mamul sandalye | adet | 0 |
| FRAME-A | Yarı mamul iskelet | adet | 0 |
| WOOD-A | Hammadde ahşap | m³ | 3 |
| FABRIC-A | Hammadde kumaş | m | 150 |
| FOAM-A | Hammadde sünger | adet | 120 |
| VARNISH-A | Hammadde vernik | kg | 10 |

**Reçete R1:** Bir CHAIR-A, bir FRAME-A + 1,2 m net kumaş + bir adet sünger kullanır. Bir FRAME-A, 0,04 m³ ahşap + 0,1 kg vernik kullanır. Kumaşın planlanan kayıp oranı girdinin %20'sidir; brüt ihtiyaç `net / (1 − kayıp)` olduğundan sandalye başına 1,5 m'dir. Diğer bileşenlerde bu örnek için kayıp sıfırdır. Paketleme malzemesi ana örnekte ayrı maliyet satırıdır; sonraki reçete genişletmesinde stoklu bileşen olabilir.

100 sandalye talebinde ahşap 4 m³, kumaş 150 m, sünger 100 adet ve vernik 10 kg gerekir. Başlangıçta yalnız ahşap 1 m³ eksiktir; bağımsız malzeme hesabıyla **75 sandalye üretilebilir**. Bu sayı hazır mamul, kesin termin veya makine kapasitesi garantisi değildir. Açık satınalma beklenen tedariktir; mal kabul gerçekleşmeden fiziksel kullanılabilir stoğa eklenmez.

Ana kabul senaryosu: 100 sandalyelik sipariş → eksik 1 m³ ahşap için satınalma → mal kabul → R1'e bağlı iş emri → malzeme çıkışı → kesim/montaj/döşeme/kalite/paketleme → 100 sağlam mamul → önce 60, sonra 40 sevkiyat → kanal kesintileri ve tahsilat eşleştirmesi. Bu senaryoda ahşap/kumaş/vernik sıfır, sünger 20 adet kalır. Fire, kısmi üretim, iade ve fason senaryoları ayrıca sınanır; ana örneğin sonucu bunlarla sessizce değiştirilmez.

## Bütün modüllerin paylaştığı iş kuralları

1. **Tek stok hareket motoru:** Satınalma, üretim, transfer, sevkiyat, sayım ve iade aynı hareket sözleşmesini kullanır. Ekranlar doğrudan “stok miktarını değiştir” işlemi yapmaz.
2. **Stok boyutları:** Şirket, ürün/varyant, depo/lokasyon, lot ve stok durumu ayrılır. Fiziksel miktar; serbest, karantina, bloke gibi durumların toplamıdır. Kullanılabilir miktar = serbest fiziksel miktar − aktif rezervasyon. Karantina serbest stoğa dâhil olmadığından ikinci kez düşülmez.
3. **Rezervasyon hareket değildir:** Rezervasyon fiziksel stoğu azaltmaz. Sevkiyat veya üretime çıkışta ilgili fiziksel miktar ve rezervasyon birlikte kapatılır. İptal yalnız kalan rezervasyonu serbest bırakır.
4. **Reçete sürümü korunur:** Yayınlanmış BOM revizyonu değiştirilmez. İş emri, kök reçeteyi ve alt reçete revizyonlarını sabitler; R2 yayımlanması eski iş emrinin ihtiyacını veya maliyet geçmişini değiştirmez.
5. **Döngü ve birim kontrolü:** A→B→A reçete döngüsü reddedilir. Aynı hammadde farklı dallarda kullanılıyorsa ihtiyaçlar toplanır. m ile m² veya kg ile adet, tanımlı dönüşüm olmadan toplanmaz. Alternatif bileşenlerin tamamı birden tüketilmez; seçilen alternatif ve oranı iş emrinde kaydedilir.
6. **Üretilebilir miktar:** Basit örnekte her girdinin kullanılabilir miktarı bir mamul için brüt ihtiyacına bölünür, en küçük sonuç aşağı yuvarlanır. Çok seviyeli yapıda hazır yarı mamul önce karşılanabilir; kalanı patlatılır. Ortak kaynak, alternatif, parti büyüklüğü ve mevcut rezervasyon varsa aynı stoğu iki kez kullanan saf minimum hesabı yeterli değildir.
7. **Tek tüketim yöntemi:** Ana akış açık malzeme çıkışı kullanır. Depodan WIP'a transfer edilen malzeme üretimde tüketilir; mamul kabulünde yeniden hammaddeden düşülmez. Otomatik backflush alternatifi ayrıca seçilirse aynı miktara ikinci tüketim üretilemez.
8. **Tekrarlanan istek:** Aynı dış sipariş veya hareket anahtarı aynı içerikle tekrar gelirse önceki sonuç döner; ikinci stok/finans kaydı oluşmaz. Aynı anahtar farklı içerikle gelirse çatışma olarak reddedilir.
9. **Kısmi işlemler:** Sevk edilmiş toplam sipariş miktarını; iade toplamı ilgili sevk miktarını aşamaz. İptal edilmiş veya tamamlanmış belgeye geçersiz geçiş yapılamaz. İade önce karantinaya alınır; kalite kararı olmadan satılabilir stoğa dönmez.
10. **Düzeltme ve iz:** Kesinleşmiş stok/finans kayıtları silinerek düzeltilmez; ters kayıt ve gerekçe ile ilişkilendirilir. Aktör, zaman, belge, kaynak olay ve değişiklik nedeni izlenebilir.
11. **Fason sahipliği:** Fasoncuya emanet gönderilen malzeme şirket mülkiyetinde kalır; şirketin fason lokasyonuna transfer edilir. Dönüş, tüketim, fire ve fason hizmet bedeli aynı iş emrine bağlanır.
12. **Şirket ve rol sınırı:** Bir firmanın kayıtları başka firmaya, bir bayinin özel fiyat/bakiyesi başka bayiye sızmaz. Üretim operatörü ödeme onaylayamaz; ödeme talebini açan kişi kendi talebinin son onayını veremez.
13. **Merkez ve dış kanallar:** Merkezî stok doğruluğu işlem sınırında korunur. İnternet üzerinden pazaryeri stokları aynı anda güncellenmiş varsayılmaz; gecikme, tekrar, sıra dışı olay ve uzlaştırma açıkça yönetilir.
14. **AI çıktısı öneridir:** Düşük güvenli belge okuma, fiyat değişimi, yüksek tutarlı ödeme ve geri dönüşü zor hareketlerde insan onayı gerekir. Onaylanmamış tahmin doğrudan stok hareketi veya banka ödemesi oluşturmaz.

## Ortak maliyet ve kârlılık hesabı

Malzeme örnek fiyatları: ahşap 10.000 TL/m³, kumaş 200 TL/m, sünger 100 TL/adet, vernik 500 TL/kg. 100 sağlam sandalye için brüt malzeme maliyeti **85.000 TL**'dir. Gerçekleşen işçilik 10.000, genel üretim gideri 5.000 ve ambalaj 5.000 TL ise toplam **105.000 TL**, birim üretim maliyeti **1.050 TL** olur. Örnekte ayrıca fason bedeli yoktur; varsa eklenir.

Vergi hariç birim satış 1.500 TL, pazaryeri komisyonu %10, ödeme komisyonu %2 ve birim kargo 50 TL varsayımında sipariş katkısı:
`1500 − 1050 − 150 − 30 − 50 = 220 TL`; katkı marjı yaklaşık **%14,67**. Bu, kapsamı belirtilmiş sipariş kârlılığıdır; şirketin vergi sonrası net kârı değildir.

Hedef katkı marjı %20, satışa oranlı giderler toplamı %12 ise:
`fiyat = (1050 + 50) / (1 − 0,12 − 0,20) ≈ 1617,65 TL`.
Bu formül oranların aynı satış tabanına uygulandığı varsayımıyla geçerlidir; oran+hedef marj ≥1 ise geçerli sonlu fiyat üretmez. Marj ile maliyet üzerine ekleme oranı karıştırılmaz.

Gerçek üretim maliyeti; fiilî malzeme tüketimi, işçilik, fason ve tanımlı dağıtım anahtarlı genel giderle bulunur. Planlanan fire maliyeti gerçek tüketim maliyetine ikinci kez eklenmez. Tahmini ve gerçekleşmiş komisyon/kargo ayrı tutulur; ekstre geldiğinde iki tutar birden gider yazılmaz. İade, ters kayıt ve gerekiyorsa ek lojistik/onarım gideri oluşturur. Dönem projesinde hareketli ağırlıklı ortalama maliyet yöntemi seçilir; lot izlenebilirliği bu değerleme yönteminden ayrı korunur.

## 3. İlk hafta prototipi: Malzeme İhtiyaç Sorgusu

İlk hedefiniz, 100 sandalye talebini ürün/BOM/stok verisine bağlamak ve eksik malzemeyi SQL ile göstermektir.

Verilen başlangıç şeması `products`, `bom_revisions`, `bom_lines`, `warehouses` ve `inventory_snapshot` tablolarını içerir. Snapshot, ilk haftanın başlangıç verisidir; henüz canlı stok hareket defteri değildir. W2–W4'te açılış hareketleri ve birikimli bakiye yapısına taşınır, aynı açılış iki kez yazılmaz. `tenant_id` baştan bulunur; F-A örneği yanında F-B erişim testine hazırlık sağlar.

Reçete patlatmanın iki seviyelik SQL iskeleti ve `v_material_requirement_per_unit` görünümü sağlanır. Siz talep miktarıyla ölçekleme, stokla eşleme ve eksik miktar hesaplarını tamamlarsınız. Genel recursive CTE/döngü çözümü M3'te derinleşir.

Örnek sorgu sözleşmesi:
`plan_materials(tenant, product, bom_revision, requested_qty)`
→ ürün, temel birim, brüt ihtiyaç, kullanılabilir, eksik miktar. Tenant filtresi bütün join'lerde korunur.

| Kontrol | Beklenen sonuç |
| --- | --- |
| 100 CHAIR-A/R1 ihtiyaçları | Ahşap 4; kumaş 150; sünger 100; vernik 10 |
| Eksik listesi | Yalnız ahşap, 1 m³ |
| Üretilebilir miktar | Basit örnekte 75 |
| Aynı sorguyu tekrar çalıştırma | Aynı sonuç; stok değişmez |
| Var olmayan ürünle BOM satırı | FK reddi |
| Miktar≤0 veya kayıp≥1 | NOT NULL/CHECK ve işlem doğrulamasıyla ret |
| F-A'daki BOM'a F-B bileşeni bağlama | Tenant kapsamlı FK reddi |

`CHECK(quantity > 0)` NULL'u tek başına engellemez; `NOT NULL` ayrıca gerekir. İlk hafta ürettiğiniz planın rezervasyon veya eşzamanlı stok garantisi olmadığını rapora yazın.

**Teslim:** DDL/seed incelemesi, sorgu ve beklenen sonuç, iki negatif test, M1–M8 haritası ve bir veri tasarım gerekçesi. İlk hafta tanılayıcıdır. SQL hatalarını ayrı transaction veya savepoint ile çalıştırın; aborted transaction üzerinden yanlış sonuç üretmeyin.

## 4. Mantıksal model ve veri sözlüğü

Aşağıdaki model bir başlangıç sözleşmesidir. Tablo adlarını gerekçeyle değiştirebilirsiniz; iş ilişkilerini ve tenant bütünlüğünü korumalısınız. Tam model dönem boyunca gelişir; bütün tablolar ilk haftada oluşturulmaz.

### 4.1 Ana veri ve reçete

| Tablo | Kritik alanlar | Anahtar / bütünlük |
| --- | --- | --- |
| tenants | id, code, name | Tenant kimliği |
| parties | tenant_id, id, code, name | Müşteri/tedarikçi/bayi rollerini ilişkiyle destekler |
| products / product_variants | tenant_id, id, sku, type, base_unit_id | Tenant içinde SKU tekil; tür RM/WIP/FG |
| barcodes | tenant_id, code, variant_id | Tenant içinde barkod tekil; varyanta FK |
| units / unit_conversions | id, dimension; from/to, factor | Dönüşüm aynı boyutta; ürüne özel dönüşüm gerekebilir |
| bom_revisions | tenant_id, id, product_id, revision, status | Ürün+revision tekil; kullanım sonrası değişmez |
| bom_lines | tenant_id, revision_id, line_no, component_id, child_revision_id, qty, loss_rate, operation_id | Pozitif qty; 0≤loss<1; tutarlı alt reçete |
| bom_alternatives | tenant_id, bom_line_id, component_id, ratio, priority | Seçenek grubu; seçili alternatif iş emrinde sabitlenir |
| routings / routing_operations | tenant_id, id, revision; sequence, work_center | Üretim sırası ve iş merkezi |

Alt reçete FK'sının doğru tenant yanında doğru bileşen ürününe de ait olması gerekir. `child_revision_id` herhangi bir reçeteye işaret edemez. Yayınlanmış revizyonun salt okunurluğu, cycle kontrolü ve iş emri snapshot'ı normal FK'nin ötesinde işlem/yetki kuralıdır.

### 4.2 Stok, üretim ve izlenebilirlik

| Tablo | Kritik alanlar | Rolü |
| --- | --- | --- |
| warehouses / locations | tenant_id, id, type, custodian_party_id | Hammadde, WIP, mamul, karantina, fason lokasyon |
| lots | tenant_id, id, product_id, supplier_lot, received_at | Aynı ürünün kaynak partileri |
| stock_documents / stock_entries | tenant_id, document_id, line_no; product, lot, location, status, signed_qty, unit_cost | Kesinleşmiş stok hareket defteri |
| inventory_balances | tenant, product, lot, location, status, on_hand, reserved | Transaction içinde güncellenen, defterden uzlaştırılan bakiye |
| reservations | tenant_id, id, demand_type, demand_id, position_id, qty, status | Aktif ayrılan miktar; fiziksel hareket değil |
| work_orders | tenant_id, id, product_id, bom_revision_id, requested_qty, status | Sabitlenmiş üretim emri |
| work_order_materials | tenant_id, work_order_id, line_no, chosen_component, chosen_revision, planned_qty | İhtiyaç/alternatif snapshot'ı |
| operation_reports | tenant_id, id, work_order_id, operation_id, good_qty, scrap_qty, labor_minutes | Gerçekleşen üretim ve işçilik |
| material_issues / production_receipts | tenant_id, id, work_order_id, stock_document_id | Tüketim ve üretilen lot ilişkisi |
| lot_genealogy | tenant_id, input_lot_id, output_lot_id, work_order_id, used_qty | Çoktan çoğa geriye/ileriye iz |
| subcontract_jobs | tenant_id, work_order_id, vendor_id, sent/returned_ref | Fason emanet, dönüş ve hizmet maliyeti |

`inventory_balances` bağımsız gerçek kaynağı değildir; hareket ve rezervasyon defteriyle eşleşmelidir. Bakiye anahtarındaki lot gibi opsiyonel boyutlarda NULL tekilliği bilinçli tasarlanır: özel “lotsuz” kayıt veya `UNIQUE NULLS NOT DISTINCT` kullanılabilir. Normal UNIQUE ile iki NULL'lu aynı pozisyon oluşmasına izin verilmez.

### 4.3 Ticaret, lojistik ve finans

| Tablo grubu | Kritik ilişkiler |
| --- | --- |
| sales_orders / sales_order_lines | Tenant, müşteri, kanal, dış sipariş kimliği, varyant, qty, para birimi, fiyat/iskonto snapshot'ı |
| purchase_requests / purchase_orders / purchase_lines | Eksik malzeme veya plan talebi → tedarikçi siparişi |
| goods_receipts / receipt_lines | Satınalma satırına kısmi kabul; lot ve stok belgesine bağlantı |
| shipments / shipment_lines / packages | Sipariş satırına kısmi sevk, lot, paket, barkod ve takip |
| returns / return_lines | Özgün sevk satırı, gerekçe, karantina ve kalite kararı |
| price_lists / price_rules | Kanal, bayi grubu/müşteri, geçerlilik, öncelik ve currency |
| cost_entries / cost_allocations | İş emri, malzeme/fason/işçilik/overhead/ambalaj, dağıtım anahtarı |
| channel_charges / freight_charges | Tahmini/gerçek komisyon, kargo, hizmet ve ödeme kesintileri |
| account_entries / settlements | Cari borç-alacak, tahsilat/ödeme ve dış hakediş bağlantısı |
| payment_requests / approval_steps | Tutar/IBAN/belge sürümü, sıra, karar, aktör ve onay zamanı |
| bank_transactions / reconciliation_links | Banka satırı ile bir veya çok iç belgenin kısmi eşlemesi |
| financial_instruments / instrument_events | Çek/senet türü, vade, taraf, tutar ve portföy/tahsil durum geçmişi |
| invoices / invoice_lines / electronic_documents | Satış/alış belgesi, sipariş/sevk satırı bağlantısı; dış e-belge kimliği ve durum geçmişi |

Sipariş → sevk → fatura/finans ilişkilerini tek bir “status” alanına indirgemeyin. Bir sipariş kısmen sevk edilmiş, kısmen tahsil edilmiş ve bazı kalemleri iade edilmiş olabilir. Bir banka satırı birden fazla belgeyi kapatabilir; eşleşme tutarlarının toplamı kaynak tutarlarını aşamaz.

Maliyet raporunda sipariş satırlarıyla sevk, komisyon ve maliyet satırlarını aynı anda doğrudan join etmek tutarları çoğaltabilir. Her çoklu tarafı önce uygun iş anahtarında aggregate edin; sonra birleştirin. Bu, teslimde gösterilecek zorunlu SQL karşı örneğidir.

### 4.4 Servis, İK, platform ve otomasyon

| Tablo grubu | Kritik alan / amaç |
| --- | --- |
| service_cases / service_events | Müşteri, mamul/seri/lot, sevk, hata türü, departman, durum geçmişi |
| attachments | Tenant, owner_type/id, object_key, mime_type, size, checksum, erişim |
| employees / shifts / time_entries / leave_requests | Operatör, vardiya, puantaj/işçilik kaynağı, izin |
| channel_mappings | Dış SKU/kanal kimliği → iç varyant |
| inbox_events / command_results | Dış olay veya komut anahtarı, payload_hash, sonuç, işlem zamanı |
| outbox_events / delivery_attempts | Commit edilmiş olay, hedef, tekrar, hata, sürüm ve durum |
| import_batches / import_errors | Kaynak dosya, dry-run sonucu, mapping, hata ve onay |
| ai_proposals / approvals | Kaynak belge/model sürümü, önerilen alanlar, güven, onay |
| audit_events | Aktör, tenant, işlem, zaman, correlation_id ve gerekçe |
| saved_reports / report_schedules / report_runs | Filtreler, sahip/erişim kapsamı, zamanlama ve gönderim sonucu |

Fotoğraf/video nesne depolamada; metadata ve erişim ilişkisi veritabanındadır. Object store ile PostgreSQL tek transaction paylaşmış varsayılmaz: staged upload, kesinleştirme ve sahipsiz dosya temizliği için durum tutulur. Hassas personel ve banka alanları genel raporlara açılmaz.

## 5. Veri tipi, anahtar ve migration kuralları

- Para ve miktar için `numeric`; olay zamanı için `timestamptz`; adetlerde tam sayı koşulu; oranlarda açık aralık kullanın. Örneğin para `numeric(18,2)`, ara birim maliyet `numeric(18,6)`, miktar `numeric(18,6)` olabilir. Gereken sınır ve yuvarlamayı veri sözlüğünde gerekçelendirin.
- Her iş tablosunun anahtarı tenant kapsamını korur. `PRIMARY KEY(tenant_id,id)` ve aynı tenant'ı taşıyan composite FK, F-A siparişinin F-B ürününe bağlanmasını engeller.
- `UNIQUE(tenant_id,channel_id,external_order_id)` ve command/event tekilliği ayrı ihtiyaçları korur. Boş dış kimliklerin anlamı ayrıca tanımlanır.
- FK silme davranışı açık olsun. Kesinleşmiş sipariş/üretim/hareket geçmişini geniş CASCADE ile yok etmeyin; master kaydı pasifleştirin.
- Stok toplamı, BOM döngüsü veya çok satırlı onay dengesi tek satırlık CHECK ile çözülemez. Constraint, transaction, kilit ve işlem yetkisinin görevlerini ayırın.
- Yayınlanmış migration değiştirilmez. Hem boş DB'den kurulum hem önceki sürümden yükseltme denenir. Normalizasyon eski kimliklere verilen referansları sessizce koparmaz.
- İlk stoklar onaylı opening document olarak aktarılır; aynı import batch tekrar işlendiğinde çift açılış oluşmaz.

## 6. 12 haftalık geliştirme planınız

Üç tam tur uygulanır: W1 M1–M8 panoraması; W2–W4 toplamında ikinci tur; W5–W12 haftada bir modülle ayrıntılı üçüncü tur. İkinci tur W2 M1–M3, W3 M4–M6, W4 M7–M8 olarak bölünür. Her hafta ilgili modüller örnek ve karşı örnekle işlenir. İskelet/dikey dilim/entegrasyon aynı ürünün gelişim adımlarıdır.

### W1 — Panorama ve ihtiyaç sorgusu

**Artım:** Ortak 100 sandalye ihtiyacı, başlangıç snapshot'ı ve eksik ahşap sorgusu. **Kavram:** M1–M8'e ilk bakış; gerçek uygulama model/bütünlük/SQL'de. **Kanıt:** Ortak sayısal sonuç, FK/CHECK reddi, stokta yan etki olmaması. **Sorunuz:** “Üretilebilir” ile “rezervasyonu yapılmış” neden farklı?

### W2 — İkinci tur, M1–M3: DBMS, model ve SQL

**Artım:** W1 verisini migration/seed'e taşıyın; sağlanan sorgu/API girişini bağlayın. Tenant, normal uygulama rolü, migration sahibi, stok/üretim/ticaret sınırlarını ayırın. Sekiz modülün gelecek genişlemeleri tablo/işlem haritasında olsun.  
**Kanıt:** Boş DB kurulumu, health query, ER taslağı, bağlantı rolü, import batch kimliği, web istemcisinden ürün/plan görüntüsü. **Konu kanıtı:** M1 istemci/oturum ve iş yükü haritası; M2 anahtar/FK/CHECK karşı örneği; M3 ihtiyaç sorgusunda selection/join/aggregate ve beklenen sonuç. Sonraki modüller bu hafta yeniden anlatılmaz.

### W3 — İkinci tur, M4–M6: normalizasyon, transaction ve indeks

**Artım:** Siparişten plan ve rezervasyona giden işlem iskeletini, duplicate event ve geçersiz FK yollarını bağlayın. Başarı/rollback durumunu inceleyin; satınalma, üretim, finans ve dış olayların transaction sınırlarını aynı haritada gösterin.  
**Konu kanıtı:** M4 küçük staging tablosunda bağımlılık ve anomali/düzeltme; M5 sağlanan iki oturumda rezervasyon yarışı ve rollback; M6 aynı sorgunun önce/sonra planı ve indeksin yazma bedeli. Ürün kanıtı mutlu yol, hata sonrası state, tenant FK reddi ve tekrar anahtarıdır. İlk rehberli deney, W9'daki bağımsız concurrency kabulünün yerine geçmez.

### W4 — İkinci tur, M7–M8 ve ilk veri sürümü: v0.1

**Artım:** Sağlanan işlem iskeletlerinde eksik malzemeden satınalma/mal kabule, iş emrinden tüketim/mamul kabulüne ve kısmi sevkiyata ilerleyin. Snapshot başlangıcı kontrollü açılış defterine dönüşsün; bakiye hareketlerle uzlaştırılsın.  
**Konu kanıtı:** M7 sağlanan replica gecikmesi/restore izi üzerinden garanti ve kurtarma; M8 aynı servis belgesinin ilişkisel/JSONB gösterimi ve rol erişimi karşı örneği. Tam bulut/modern platform kurulumu istenmez. **Ürün kanıtı:** 100 sandalye ana akışı; 60+40 sevkiyat; hareket/rezervasyon toplamı; duplicate belge reddi; sekiz modül haritası ve bilinen concurrency/yetki sınırları. İleri finans/kanal/İK verileri bu aşamada bağlı örneklerdir.

### W5 — M1: DBMS mimarisi, iş yükü ve garantiler

**Talep:** Üretim kiosk'u, B2B araması ve gece kârlılık raporu aynı sisteme yük getiriyor. **Artım:** İş yükü kataloğu ve session/transaction gözlemi; bağlantı havuzu ve süre bütçesi tasarımı. **Geri bağ:** M3 sorgu, M5 kilit.  
**Kanıt:** pg_stat_activity, uzun transaction örneği, OLTP/analitik ayrımı, gecikme/throughput ölçümünün ortamı. Rol ve tenant bağlamının bağlantı yeniden kullanımında sıfırlanması gereğini açıklayın; testte ölçülmemiş SLA yazmayın.

### W6 — M2: Kavramsal model, şema ve bütünlük

**Talep:** Lot, varyant, fason ve kısmi işlemler geldiğinde model geçerli kalsın. **Artım:** ER/EER ve migration; composite tenant FK; stok pozisyonu tekilliği; sipariş/üretim/servis bağlantıları. **Geri bağ:** M1 garanti, M3 join.  
**Kanıt:** Cardinality/participation, candidate key, weak entity, specialization alternatifleri; NULL ve deletion policy; yanlış tenant/ürüne ait alt reçete reddi. Kayıtların çoğu tek ana tabloda mı, alt tür tablolarda mı tutulacak gerekçelendirin.

### W7 — M3: İlişkisel cebir, recursive SQL ve raporlar

**Talep:** Çok seviyeli BOM, where-used, kaynak lot ve net kanal sonucu sorgulansın. **Artım:** Aşağıdaki on sorguluk portföy; recursive CTE, cycle path, JOIN/EXISTS, window, aggregate ve view kullanımı. **Geri bağ:** M2 anahtar, M1 workload.  
**Kanıt:** Sabit sonuç kümeleri, aynı hammaddenin dallar arasında toplanması, maliyet join çoğalması karşı örneği, NULL ve ORDER BY sınırları. Eğitmen sorgu/veri iskeletleri sağlar; kendi tamamladığınız bölümleri belirtin.

### W8 — M4: Fonksiyonel bağımlılık ve normalizasyon

**Talep:** Fabrikanın eski Excel ürün/BOM/fiyat/açık sipariş verisi aktarılacak. **Artım:** Staging modelindeki anomalileri gösterin; 3NF/BCNF yönünde dönüşüm ve import reconciliation ekleyin. Sağlam canlı çekirdeği ödev için bozmayın. **Geri bağ:** M2 key, M3 sonuç eşdeğerliği.  
**Kanıt:** FD, closure, candidate key; 1NF–BCNF; kayıpsızlık ve dependency preservation. Örneğin (tenant,SKU)→ürün özellikleri; (tenant,BOM revision,line_no)→bileşen/miktar. Tarihsel fiyatı yalnız SKU'ya bağımlı varsaymayın. Kaynak satır sayısı, reddedilen satır ve açılış toplamlarını uzlaştırın.

### W9 — M5: Transaction, concurrency ve recovery

**Talep:** İki satış kanalı son malzemeyi aynı anda ayıramasın; yinelenen üretim tamamlama ikinci mamul yaratmasın. **Artım:** Aşağıdaki idempotent rezervasyon ve üretim transaction'ı; ortak kilit sırası ve bounded retry. **Geri bağ:** M2 bütünlük, M4 doğru anahtarlar.  
**Kanıt:** Bariyerli iki oturum deneyi, tek etkili duplicate, atomik çok kalem, rollback, deadlock/retry, MVCC ve WAL açıklaması. PostgreSQL'de Read Uncommitted'in Read Committed gibi davranması ile genel isolation anomalilerini ayırın.

### W10 — M6: Storage, indeks ve query processing

**Talep:** Milyonlarca stok hareketinde kullanılabilir stok, geciken emir ve kanal raporu zamanında gelsin. **Artım:** Seed büyütücüsü, üç iş yükü, gerekçeli indeks ve önce/sonra plan. **Geri bağ:** M3 sorgu, M5 write maliyeti.  
**Kanıt:** Page/heap/buffer, selectivity/statistics/cost; scan/join/sort/aggregate düğümleri. B-tree/hash/GIN/GiST/BRIN, composite/partial/covering seçeneklerini ihtiyaçla karşılaştırın. Her indeksi kurmayın; gerekli bir veya iki seçimi ham ölçüm ve yazma/depolama bedeliyle savunun.

### W11 — M7: Bulut, dağıtık sistem ve kurtarma

**Talep:** Merkezî ERP arızadan geri dönebilsin; pazaryeri senkronizasyonunun gecikmesi yönetilsin. **Artım:** Ayrı DB'ye backup/restore; sağlanan replica/lag deneyi; outbox teslim/uzlaştırma raporu. **Geri bağ:** M5 durability, M6 workload.  
**Kanıt:** Restore sonrası stok ve maliyet toplamları; ölçülen recovery süresi; RPO/RTO hedefi ayrımı; partition/replication/sharding/quorum kararı. CAP/PACELC'i somut ağ bölünmesi ve gecikme tercihiyle açıklayın. pg_dump restore'u PITR saymayın; PITR için base backup ve WAL arşivi ayrıca gerekir.

### W12 — M8: Modern veri modelleri, RLS ve v1.0

**Talep:** Bayi yalnız kendi verisini görsün; servis/AI belgesi esnek metadata taşısın. **Artım:** Sağlanan auth/RLS iskeletinde tenant+bayi politikası ve bir JSONB belge/servis sorgusu. **Geri bağ:** M2 model, M3 filtre, M5 tutarlılık, M6 indeks.  
**Kanıt:** Rol bazlı pozitif/negatif test, dosya metadata ilişkisi, düşük güvenli AI önerisi için onay kaydı. MongoDB embedding/reference; Firestore/Realtime Database; Supabase Auth/API/Storage/Realtime ve pgvector exact/ANN/hybrid yaklaşımları aynı iş ihtiyacıyla karşılaştırılır. Sabit embedding/olay demoları sağlanır; her ürünün sıfırdan kurulması istenmez. **v1.0:** Bütün önceki kanıtlar ve son fabrika demosu.

## 7. Zorunlu sorgu portföyü

| Kod | İş sorusu | Beklenen SQL/kanıt |
| --- | --- | --- |
| Q01 | 100 sandalye için brüt ihtiyaç ve eksik nedir? | Recursive CTE, yol miktarı, gruplanmış hammadde |
| Q02 | Bu ahşap hangi mamul/revizyonlarda kullanılıyor? | Ters BOM/where-used; döngü koruması |
| Q03 | Depo/lot/durum bazında kullanılabilir stok ne? | Hareket ve rezervasyon ayrı aggregate; negatif kontrol |
| Q04 | Açık tedarik dikkate alındığında hangi ihtiyaç ne zaman karşılanır? | Kısmi kabul, beklenen tarih; fiziksel stoktan ayrım |
| Q05 | Hangi operasyonlarda fire veya gecikme arttı? | Group/window; sağlam ve fire ayrımı |
| Q06 | Bir servis mamulünün kaynak lotları ve etkilenen diğer sevkler hangileri? | Genealogy üzerinde geri/ileri iz |
| Q07 | Planlanan ve gerçekleşen iş emri maliyeti farkı ne? | Malzeme/işçilik/fason/overhead ayrı toplamlar |
| Q08 | Sipariş/kanal bazında gerçek katkı ve marj ne? | Çoklu join çoğalması önlenmiş finans raporu |
| Q09 | Hangi ödeme/hakedişler kısmen eşleşmiş veya onay bekliyor? | Many-to-many tahsis toplamı, remaining amount |
| Q10 | Hangi import/connector/AI işleri insan müdahalesi bekliyor? | Durum, retry, güven ve yaşlandırma görünümü |

Her sorguda parametreler, tenant/rol, kullanılan revizyon/tarih, beklenen satır kümesi ve NULL/boş sonuç davranışı bulunur. Performans ve doğruluk farklı kanıtlardır. Boş sonuç bazen doğru cevaptır; test bunu kapsamalıdır.

## 8. Rezervasyonun transaction sözleşmesi

Aynı pozisyonda serbest fiziksel 10, reserved 0 olsun. T1 ve T2 farklı siparişler için 8'er birim istiyor. Hatalı check-then-write sürümünde ikisi de 10 okuyup 16 ayırabilir. Düzeltmeniz şu akışı izlemelidir:

1. BEGIN; actor/tenant doğrulanır. `command_results` içine tenant+request_id tekilliği ve payload hash ile giriş yapılır. Eşzamanlı aynı key önceki transaction sonucunu bekler; aynı içerikte kayıtlı sonuç, farklı içerikte çatışma döner.
2. Tüm stok pozisyonları deterministik aynı sırada `SELECT ... FOR UPDATE` ile kilitlenir. Eksik bakiye satırlarının oluşturulması tekil anahtar ve ortak protokolle çözülür.
3. Kilit altındaki kullanılabilir miktar her kalem için yeniden kontrol edilir. Herhangi biri yetersizse stokta hiç değişiklik yapılmaz; ret sonucu kaydedilip transaction tamamlanır. Böylece tekrarlanan reddedilmiş isteğin sonucu da belirli olur.
4. Hepsi uygunsa rezervasyon satırları eklenir; bakiye reserved güncellenir; command sonucu ve outbox olayı aynı transaction'da yazılır; COMMIT.
5. Ağ hatasıyla yanıt kaybolursa istemci aynı key ile sonucu sorar. Deadlock/serialization gibi retry edilebilir transaction hatalarında bütün transaction sınırlı sayıda yeniden denenir; yalnız son statement tekrarlanmaz.

Son durumda yalnız bir talep kabul edilir, reserved=8 ve kullanılabilir=2 olur. Bütün rezervasyon/sevkiyat/iptal/üretim yolları aynı protokolü kullanır; doğrudan bakiye yazma izni normal uygulama rolüne verilmez. RLS tek başına bu yarış sorununu çözmez.

Üretim tamamlama da iş emri/ilgili stok pozisyonlarını kilitler; tamamlanan miktar, malzeme tüketimi, mamul lotu, maliyet kayıtları ve outbox etkisini bir transaction'da doğrular. Aynı command ikinci mamul girişi yaratamaz. Temel yöntemde önceden çıkılmış malzeme ikinci kez düşülmez.

## 9. Tenant ve bayi güvenliği

RLS'yi ayrı normal roller/oturumlarla test edin; superuser, tablo sahibi veya BYPASSRLS rolüyle “başarılı izolasyon” kanıtı üretmeyin. `USING` ile görünür satırı, `WITH CHECK` ile yazılabilecek satırı değerlendirin. Tablo izinleri, tenant FK'leri, işlem yetkileri ve RLS birbirini tamamlar.

F-A ERP yöneticisi, F-A bayi B1, F-A bayi B2, F-B kullanıcısı ve üretim operatörü için izin matrisi verin. B1; B2'nin özel fiyatı, bakiyesi, siparişi veya eklerini görememeli. Operatör kendi iş ekranında gereken miktarı görmeli; personel ücretleri/banka bilgileri görünmemeli.

İstemcinin istediği `tenant_id` veya serbestçe değiştirdiği session değişkeni güvenilir kimlik değildir. Yerel deneyde sağlanan PostgreSQL rol–kimlik eşlemesini kullanın; bulut seçeneğinde doğrulanmış auth bağlamının havuzlanan bağlantıya nasıl taşınıp temizlendiğini belgeleyin. SECURITY DEFINER işlevleri varsa sabit güvenli search_path, doğru owner ve sınırlı EXECUTE izni gerekir; bunların iskeleti sağlanır.

## 10. Performans ve dayanıklılık deneyiniz

Üç ölçüm iş yükü seçin: yüksek frekanslı stok sorgusu, geciken iş emri listesi ve dönemlik kanal kârlılığı. Aynı fixture/parametre ve yetkiyle önce/sonra plan alın. Veri boyutu, tekrar sayısı, cache durumu, median ve yayılımı bildirin; tek hızlı çalıştırma yeterli değildir.

Örnek adaylar: tenant+ürün+lokasyon bakiye anahtarı için B-tree; aktif rezervasyon için partial index; tarih aralıklı büyük hareket tablosunda uygun veri düzeni varsa BRIN; JSONB sorgu biçimine uygun GIN. Bunlar karar adaylarıdır; plan ve ölçüm olmadan doğru indeks ilan edilmez. `EXPLAIN ANALYZE` sorguyu çalıştırır; yan etkili işlemde test transaction'ı ve rollback gereğini değerlendirin.

Kurtarma deneyinde ayrı bir hedefe restore yapın; yalnız “restore komutu başarılı” sonucuyla durmayın. Stok/rezervasyon toplamları, R1 iş emri, maliyet, tenant görünürlüğü ve command tekilliği testlerini yeniden çalıştırın. Outbox'ta gönderilmiş fakat işaretlenmemiş olaylar yeniden gidebilir; alıcı tekilleştirmesini doğrulayın.

## 11. Kabul testleri ve hata kataloğu

| Kod | Deney | Beklenen kanıt |
| --- | --- | --- |
| D01 | Ana BOM ve eksik sorgusu | 4/150/100/10; eksik ahşap 1; üretilebilir 75 |
| D02 | Geçersiz miktar, NULL, yanlış tenant FK | Ret; tutarlı state |
| D03 | Döngülü ve yanlış ürüne ait alt reçete | Yayınlama/işlem reddi |
| D04 | R2 ve eski R1 iş emri | Eski ihtiyaç/maliyet değişmez |
| D05 | Aynı açılış/import/event tekrar işleme | Tek iş etkisi |
| D06 | İki oturumda 8+8 rezervasyon | Bir kabul; reserved=8, available=2 |
| D07 | Çok kalemli rezervasyonda son kalem eksik | Hiçbir kalem ayrılmaz |
| D08 | Üretim tamamlamayı tekrar gönderme | Tek mamul/maliyet olayı |
| D09 | 60+40 sevkiyat ve fazla iade | Miktar sınırları korunur |
| D10 | Fason ve kalite/karantina | Mülkiyet, lot ve kullanılabilir miktar doğru |
| D11 | Çoklu join kârlılık raporu | 1.050 maliyet / 220 katkı; şişmiş toplam yok |
| D12 | İki bayi ve iki tenant | Okuma/yazma/ek erişimi ayrışır |
| D13 | Yedekten geri yükleme | İş kuralları ve toplamlar tekrar geçer |
| D14 | Gecikmiş/tekrarlı dış olay | Uzlaştırılabilir durum; merkez stok bozulmaz |
| D15 | Düşük güvenli AI belgesi | Onay bekler; stok/ödeme oluşmaz |
| D16 | Staging'den migration | Kaynak/hedef/reddedilen satır uzlaşır |

Hata çıktıları için SQLSTATE ve iş sonucu kullanın; lokalize mesajın tamamına bağımlı olmayın. Negatif deneyler disposable test verisinde yapılır. “Reddedildi” kanıtında hangi satırların değişmediği de gösterilir.

## 12. Teslimler, değerlendirme ve final

Haftalık paket; baseline/migration sürümü, DDL/SQL, seed, tekrar üretim komutları, beklenen/gerçek sonuç, ER veya işlem haritası farkı, tasarım kararı, sınırlama ve AI/dış katkı açıklamasını içerir. Metin raporu 1–2 sayfa olabilir; ER, plan ve ham çıktılar ekte tutulur. Canlı erişim anahtarı veya gerçek müşteri/personel verisi teslim etmeyin; ders fixture'larını kullanın.

**Sürüm kapıları:** W1 prototip; W4 v0.1; W8 model/import kontrolü; W12 v1.0. Kurtarma tabanı gerekiyorsa kaynak sürümü ve kendi eksiğiniz açıklanır; sonraki modüllere katılım sürer. Her öğrenci sorgusunu, kilit sırasını ve veri kararını bağımsız savunur.

| Ölçüt | Proje içi pay |
| --- | ---: |
| Model, anahtarlar, migration ve bütünlük | 25 |
| SQL/cebir, BOM ve rapor doğruluğu | 20 |
| Transaction, idempotency ve eşzamanlılık | 25 |
| Performans, erişim kontrolü ve kurtarma | 20 |
| Bireysel açıklama ve yeniden üretilebilirlik | 10 |
| **Toplam** | **100** |

Bu rubrik proje içindir; ders toplam not ağırlığı resmî izlenceden alınır.

Final demosunda boş test DB'den kurulum yapın; 100 sandalye akışını gösterin; iki oturum rezervasyonunu çalıştırın; R1 geçmişini ve bir servis lot izini sorgulayın; kârlılık hesabını doğrulayın; başka bayi erişimini reddedin; restore sonrası aynı testlerin geçtiğini gösterin. Modern veri örneğinin hangi iş ihtiyacını çözdüğünü ayrıca açıklayın.

Canlıya geçiş tasarımınız ürün/BOM/cari/fiyat/açık sipariş/stok aktarımı, sayım mutabakatı, kullanıcı eğitimi ve geri dönüş planını içersin. Faz 1 çekirdek/üretim; faz 2 satış kanalları/finans; faz 3 servis/İK/ileri otomasyondur. Stok ve üretim modülleri ayrı doğruluk kurallarıyla devreye alınamaz; ortak hareket motoru faz 1'in parçasıdır.

## 13. Teknik kaynaklar

- [PostgreSQL Constraints](https://www.postgresql.org/docs/18/ddl-constraints.html): Anahtar, NULL, CHECK ve FK sınırları.
- [Recursive WITH ve CYCLE](https://www.postgresql.org/docs/18/queries-with.html): BOM ve genealogy dolaşımı; path/cycle kontrolü.
- [Transaction Isolation](https://www.postgresql.org/docs/18/transaction-iso.html): MVCC, görünürlük ve retry gerekçesi.
- [Row Security Policies](https://www.postgresql.org/docs/18/ddl-rowsecurity.html): Rol/tenant/bayi testleri.
- [PostgreSQL 18 belgeleri](https://www.postgresql.org/docs/18/): EXPLAIN, indeks, WAL ve backup için sürümlü başvuru.
- [Ders kaynakçası](KAYNAKCA.md): Cebir, FD/normalizasyon, dağıtık sistem ve modern veri modeli destekleri.

İş ihtiyaçlarının kaynağı paylaşılan sandalye fabrikası ERP vaka metnidir. Sayısal fixture, tablo önerileri, transaction protokolü ve ders kapsamı bu ihtiyaçları doğrulanabilir öğrenci çalışmalarına dönüştürmek üzere burada tasarlanmıştır.
