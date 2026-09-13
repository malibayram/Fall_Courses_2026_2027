# SE 237 — Sandalye Fabrikası ERP'si: Öğrenci Proje Rehberi

**Ders:** Nesneye Dayalı Programlama / Object Oriented Programming  
**Dönem:** 2026–2027 Güz · 14 hafta · `1 + 3 + 10`  
**Proje:** Factory ERP — üretim, stok, satış ve finans süreçlerinin nesne modeli  
**Belge sürümü:** 1.0 · 11 Eylül 2026  

**Ders oturumu:** 155 dakika; 125 dakika etkin çalışma ve üç adet 10 dakikalık ara. Hazır altyapı ve sınırlı öğrenci değişikliği bu süreye göre planlanır; süre azalması ek ev ödevine aktarılmaz.

**Hedef kitle:** Bu projeyi geliştirecek öğrenciler

Bu rehber, önceki Course Registration örneğinin yerine geçen fabrika projesini tanımlar. Türkçe açıklamalarda İngilizce teknik terimler korunmuştur. Dersin sınav ve resmî değerlendirme dili ilgili izlenceye tabidir. [Veritabanı dersinin rehberi](../CMPE_351/ERP_OGRENCI_REHBERI.md) aynı fabrikanın veri ve transaction tasarımını açıklar; o derse katılmanız bu projeyi yapmanın önkoşulu değildir.

## 1. Sizden beklenen ürün

Gerçek bir sandalye fabrikasının süreçlerini taşıyan, tarayıcıdan erişilen bir ERP'nin iş mantığını geliştireceksiniz. Ürün kartı açmak, stoğu göstermek ve sipariş listelemek başlangıçtır. Asıl hedefiniz, siparişin malzeme ihtiyacına, satınalmaya, üretime, sevkiyata, maliyete ve finansal sonuca dönüşmesini tutarlı nesneler ve sözleşmelerle gerçekleştirmektir.

İlk haftada küçük bir malzeme ihtiyaç prototipi çalışacak. Dönem sonunda aynı kod, web arayüzünden kullanılan modüler bir Java uygulamasına dönüşecek. Ürün–stok–üretim çekirdeği aynı hareket motorunu kullanacak; satış, finans ve dış kanallar bu çekirdeğe açık arayüzlerden bağlanacak.

**Teknik çerçeve:** Java 25 LTS, Maven Wrapper ve JUnit 6. Başlangıçta bellek içi repository ve CLI kullanılır; ilerleyen haftalarda sağlanan HTTP, oturum/yetki ve responsive ekran iskeleti domain servislerine bağlanır. Son demo tarayıcıdan çalışır; depo/üretim ekranı tablet genişliğinde kullanılabilir. Mobil erişim için ayrıca yerel mobil uygulama yazmanız gerekmez.

Bu rehber bir uygulama sözleşmesidir; adı geçen starter, ekran iskeleti ve test fixture'ları ders paketinde sağlanacaktır. İş mantığı, testler, nesne modeli ve değişiklik gerekçesi sizin tesliminizdir.

## 2. Fabrikanın ürün kapsamı

Aşağıdaki üç derinlik düzeyi aynı ürün içinde kullanılır. **Uygulama:** Çalışan iş akışını ve testini geliştirirsiniz. **Bağlantı:** Sağlanan adaptör/örnek ekran üzerinden entegrasyon sözleşmesi ve en az bir başarı/hata senaryosu gösterirsiniz. **Genişleme:** Üretim ortamına geçiş tasarımını ve bağımlılıklarını belgelersiniz. Genişleme, o modülün fabrika ihtiyacından çıkarılması anlamına gelmez.

| Modül | Dönem içindeki somut kapsam | Derinlik |
| --- | --- | --- |
| Ürün, varyant, barkod, birim | Üç ürün türü, çok seviyeli/revizyonlu BOM, fire, alternatif ve where-used | Uygulama |
| Stok ve depo | Hareket, transfer, rezervasyon, karantina, lot, sayım farkı, kullanılabilir stok | Uygulama |
| Üretim ve fason | İş emri, aşamalar, malzeme çıkışı, iyi/fire miktarı, mamul kabul, emanet stok | Uygulama; fason faturası bağlantı |
| Satınalma | Eksikten talep, satınalma siparişi, kısmi mal kabul, fatura referansı | Uygulama |
| Satış ve B2B | Ortak sipariş modeli, özel fiyat, risk kontrolü, kısmi sevk, iptal/iade | Uygulama; portal iskeleti sağlanır |
| Pazaryeri/e-ticaret | Ortak connector, dış kimlik eşlemesi, idempotency, stok/fiyat senkronizasyonu | Bağlantı; iki test connector'ı |
| Lojistik | Toplama/paket/etiket sözleşmesi, takip, tahmini-gerçek kargo | Uygulama + test adaptörü |
| Maliyet/kârlılık | Planlanan-gerçekleşen maliyet, dağıtım, kanal kesintileri, marj/fiyat | Uygulama |
| Finans/ön muhasebe | Cari hareket, ödeme talebi, çok adımlı onay, banka/hakediş eşlemesi | Sınırlı çalışan akış + adaptör |
| Satış sonrası | Fotoğraflı servis kaydı, durum geçişi, müşteri/ürün/lot bağlantısı | Uygulama; dosya adaptörü sağlanır |
| İK | Personel/operatör, vardiya, izin ve işçilik girdisi | Bağlantı; bordro/PDKS genişleme |
| Raporlama/AI/operasyon | Rol bazlı rapor, içe aktarım, hata kuyruğu, öneri-onay, audit | Uygulama + sağlanan altyapı |

Gerçek pazaryeri, banka, sanal POS ve e-dönüşüm bağlantıları dönem demosunda aynı sözleşmeyi kullanan sandbox/fake adaptörlerle temsil edilir. Canlı para transferi, resmî belge kesimi, tam genel muhasebe ve bordro ürünü bu dersin kabul şartı değildir. Gerçek fabrikanın bu ihtiyaçları mimaride ve canlıya geçiş planında korunur; sağlayıcı ve müşteri kararlarıyla ayrı doğrulanır.

### Fabrikanın tamamını bağlayan yan süreçler

Trendyol, Hepsiburada ve Amazon, vaka metnindeki hedef kanal örnekleridir; ders connector'ları bu şirketlerin gerçek API'lerinin tamamını uyguladığını iddia etmez. Kendi e-ticaret sitesi ve B2B portalı da aynı sipariş/ürün/rezervasyon sözleşmesine bağlanır. Sürümlü REST API, webhook, SMS/e-posta, banka, sanal POS, e-fatura/e-arşiv/e-irsaliye ve PDKS sınırlarında kimlik eşlemesi, yetki, hata, tekrar ve uzlaştırma ortak tasarım konularıdır.

Finansta çek/senet için tür, düzenleyen taraf, tutar/para birimi, vade, portföy, tahsile verme, tahsil/karşılıksız/iade ve durum geçmişi tasarlanır. Belgeyi portföye almak banka tahsilatı sayılmaz; gerçekleşmiş tahsilat ayrı olay ve eşleştirmedir. Bu alt süreç bağlantı düzeyindedir; sağlanan bir vade/tahsil örneğiyle gösterilir. Tam genel muhasebe kapsamı ve bordro hesabı müşteriyle netleştirilecek kararlar olarak kalır.

Raporlarda tarih, ürün, müşteri, kanal, bölge ve satış temsilcisi filtreleri; kaydedilen rapor tanımı ve zamanlanmış e-posta işi ayrı kavramlardır. Zamanlanmış iş, oluşturulduğu kullanıcının güncel erişim kapsamıyla çalışır; bir bayiye başka bayinin raporunu göndermez. İK'da vardiya, fazla mesai ve puantaj üretime işçilik girdisi sağlar; personel ücret detayları herkese açık üretim raporuna taşınmaz. Bu bağlantılar çekirdek iş kurallarını atlayan ayrı ekranlar olarak kurulamaz.

### Her hafta nasıl çalışacaksınız?

Ders öncesinde kısa kavram kaynağını inceleyin ve çalışan bir önceki sürümde ilgili iş akışını izleyin. Derste önce bir fabrika değişiklik talebinin sonucunu tahmin edin; ardından referans davranışı çalıştırıp tahmininizi sınayın. Küçük bir kod/SQL değişikliği yapın, başarı ve hata durumlarını test edin, sonucu nesne veya veri modeliyle açıklayın. Ders sonrasında geri bildirime göre düzeltin ve aynı ürünün yeni sürümünü teslim edin.

Dersi baştan sona üç turda görürsünüz: W1 bütün haritanın panoraması; W2–W4 konuların üç haftaya dağıtıldığı ikinci tur; W5–W14 on haftalık ayrıntılı üçüncü tur. İkinci turda her hafta yalnız kendi konu grubu işlenir; tüm modüller her hafta yeniden anlatılmaz. Her şeyi ilk haftada bağımsız geliştirmeniz beklenmez. Derinleşme haftalarında bir konu öne çıkar, fakat stok–üretim–satış zinciri ve önceki testler çalışmaya devam eder. Referans çözüm veya kurtarma tabanı kullanırsanız kaynağını belirtin; neyi kendiniz değiştirdiğinizi gösterin. Yalnızca çalışan ekran, ezberlenmiş tanım veya açıklayamadığınız üretilmiş kod yeterli kanıt değildir.

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

## 3. İlk hafta prototipi: 100 Sandalye İçin Neler Eksik?

### Geliştireceğiniz davranış

`MaterialPlanner.plan(product, revision, quantity, inventorySnapshot)` benzeri bir işlem yazın. İşlem brüt hammadde ihtiyaçlarını, eksikleri ve üretilebilir adedi döndürsün; stok veya rezervasyonu değiştirmesin. Geçerli olmayan miktar, tanımsız birim veya reçete döngüsü açık hata sonucu üretsin.

İlk hafta size ürün/reçete verisi, çok seviyeli dolaşmanın iskeleti ve test dosyaları verilir. Siz miktar çarpımını, fire dönüşümünü, aynı bileşeni toplama ve stokla karşılaştırmayı tamamlarsınız. Başlangıçta dört küçük yapı yeterlidir: `Product`, `BomLine`, `InventorySnapshot`, `MaterialPlan`. Bütün dönem mimarisini ilk günde kurmaya çalışmayın.

| Test | Beklenen sonuç |
| --- | --- |
| CHAIR-A/R1, miktar 100 | WOOD=4; FABRIC=150; FOAM=100; VARNISH=10 |
| Başlangıç stoğuyla karşılaştırma | Yalnız WOOD eksik=1; üretilebilir=75 |
| Aynı planı iki kez çalıştırma | Aynı sonuç; stokta değişiklik yok |
| Miktar 0 veya negatif | İş kuralı hatası |
| Kayıp oranı 1 veya üzeri | Geçersiz reçete; bölme yapılmaz |
| A→B→A | Döngü hatası; sonsuz özyineleme yok |

İlk iki test derste tamamlanır; diğerleri sağlanan testler üzerinden açıklanır ve kısa çalışmada tamamlanır. Teslim: küçük kod farkı, geçen testler, tek sayfalık nesne/akış haritası ve 60–90 saniyelik açıklama. Bu başlangıç tanılayıcıdır; on kavramın tamamını uygulamış sayılmazsınız.

## 4. Nesne tasarımı: Hangi karar kime ait?

| Alan | Örnek nesneler | Koruyacağı sözleşme |
| --- | --- | --- |
| Katalog ve reçete | Product, ProductVariant, Unit, BomRevision, BomLine | Yayınlanmış sürüm ve birim tutarlılığı |
| Planlama | MaterialPlanner, MaterialPlan, Shortage | Planlama sonucu; stokta yan etki yok |
| Stok | InventoryPosition, Reservation, StockMovement | Negatif kullanılabilir stok yok; izlenebilir değişim |
| Üretim | WorkOrder, Operation, MaterialIssue, ProductionReceipt | Doğru revizyon, miktar ve aşama geçişi |
| Ticaret | SalesOrder, OrderLine, PurchaseOrder, GoodsReceipt | Miktar, fiyat anlık görüntüsü ve kısmi işlem |
| Lojistik/servis | Shipment, Package, ReturnCase, ServiceCase | Sevk/iade sınırı ve geçmiş bağlantısı |
| Finans | Money, CostBreakdown, PaymentRequest, Approval, Settlement | Aynı para birimi, doğru dağıtım ve onay |
| Entegrasyon | ChannelConnector, CarrierGateway, DocumentGateway | Kaynak bağımsız iş akışı; tekrar/güvenli hata |

Bunlar başlangıç yönlendirmesidir; isimleri gerekçeyle değiştirebilirsiniz. Her isim için sınıf üretmek yeterli değildir. Sınıfın sorumluluğu, değişiklik nedeni, koruduğu invariant ve bağımlılığı açıklanmalıdır.

Ürün türlerini `RawMaterial extends Product` biçiminde ayırmak zorunda değilsiniz. Davranış farkı yoksa tür alanı ve bileşim yeterli olabilir. Kalıtım, gerçekten ortak bir davranış sözleşmesi bulunan kural/hesaplama nesnelerinde gösterilir. Domain nesneleri HTTP isteğini, SQL bağlantısını veya pazaryeri JSON'unu doğrudan bilmez.

Hedef yapı:
`web → application services → domain`; repository, dosya, bildirim, kanal ve maliyet verisi dış sınırlara aittir. Modüller tek uygulamada çalışır; bağımsız servis dağıtımı zorunlu değildir. Demo şirketleri F-A ve F-B ile tenant sınırı baştan kimliklere eklenir; bu, ileride eklenmesi zor bir tasarım kararıdır.

### Durum geçişleri

- BOM: DRAFT → RELEASED → RETIRED. Kullanılmış RELEASED revizyon yeniden düzenlenmez.
- İş emri: DRAFT → RELEASED → IN_PROGRESS → COMPLETED. Eksik malzeme/kalite için blokaj ayrıca izlenir; gerçekleşmiş hareketler iptal durumunda ters işlem ister.
- Sipariş: DRAFT → CONFIRMED → PARTIALLY_SHIPPED → SHIPPED → CLOSED. Ödeme, sevk ve belge durumları tek bir dev enum'a sıkıştırılmaz.
- Ödeme talebi: DRAFT → SUBMITTED → APPROVED → PAID veya REJECTED. Tutar değişirse eski onay geçerliliği yeniden değerlendirilir.
- Servis: OPEN → TRIAGED → IN_PROGRESS → RESOLVED → CLOSED. Yeniden açma gerekiyorsa ayrı geçiş ve neden tutulur.

## 5. 14 haftalık geliştirme planınız

Üç tam tur: W1'de A1–A10 panorama; W2–W4 toplamında A1–A10'un ikinci işlenişi; W5–W14'te haftada bir çapayla ayrıntılı üçüncü tur. İkinci turun dağılımı W2 A1–A3, W3 A4–A6, W4 A7–A10'dur. İskelet/dikey dilim/entegrasyon ürün adımlarıdır; konu dağılımının yerine geçmez. Her hafta önceki sürümün testleri korunur.

İkinci turun ayrıntılı öğretmen dosyaları: [W2 — Nesne yapısı](HAFTA02.md) · [W3 — Rezervasyon dilimi](HAFTA03.md) · [W4 — Entegrasyon ve v0.1](HAFTA04.md). Bu dosyalar hazırlık, ders akışı, studio, test matrisi ve kabul ölçütlerini ayrıntılandırır; adı geçen starter ve fixture'lar ayrıca üretilecektir.

### W1 — Panorama ve malzeme planı

**Artım:** Yukarıdaki prototip ve fabrika süreç haritası. **Kavram:** A1–A10'a ilk bakış; nesne, invariant, referans ve koleksiyon örnekleri. **Teslim:** 100 sandalye hesabı, testler, değiştirmediğiniz stok durumu. **Açıklama sorusu:** İhtiyaç hesaplamak ile malzeme ayırmak neden ayrı işlemler?

### W2 — İkinci tur, A1–A3: nesne modeli, invariant ve ilişkiler

**Artım:** Ürün–reçete–stok–üretim servislerini verilen web/CLI iskeletine bağlayın. Bellekte repository kullanın; sayfada bir ürün ve plan sonucu görülsün. Satış, finans, servis, İK ve connector sınırlarını haritaya ekleyin; henüz uygulanmamış olanları etiketleyin.  
**Konu:** A1 nesne sorumluluğu, A2 encapsulation/invariant, A3 UML/ilişki/sahiplik. Her biri ürün–reçete–stok örneği ve bir karşı örnekle işlenir; diğer çapalar yalnız haritada bekleyen konulardır. **Kanıt:** Tarayıcı smoke testi, bir domain testi, sınıf/nesne diyagramı, tenant kimliğinin izlediği yol.

### W3 — İkinci tur, A4–A6: kalıtım, interface ve polimorfizm

**Artım:** Sipariş planından rezervasyon talebine ilerleyin; eksik malzeme ve yinelenen request ID sonucunu yönetin. Tüm kontroller bitmeden state değişmesin; geçersiz istekte rezervasyon oluşmasın. Verilen işlem sınırı birden çok nesnenin güncellenmesini koordine eder.  
**Konu:** A4 ortak uygunluk denetiminde alt tür sözleşmesi; A5 rezervasyon/kanal portu; A6 seçilebilir uygunluk politikası ve composition. Sağlanan iki küçük implementation aynı contract testinden geçirilir. A2 invariant kısa geri çağrılır; ileri hata/alias ayrıntıları W4 ve üçüncü tura bırakılır. **Kanıt:** Mutlu yol sequence diagram'ı, eksik stok, aynı anahtar/aynı içerik ve aynı anahtar/farklı içerik testleri. Bellek içi tek-thread doğruluğunu çok kullanıcılı güvence olarak sunmayın.

### W4 — İkinci tur, A7–A10 ve ilk fabrika sürümü: v0.1

**Artım:** Sağlanan süreç iskeletinde eksik malzemeyi satınalma ve mal kabule, iş emrini malzeme çıkışı/mamul kabulüne ve siparişi kısmi sevkiyata bağlayın. Her alt modül bu aşamada az sayıda ama gerçek davranış içerir.  
**Konu:** A7 snapshot/equality/kopya, A8 tür güvenli koleksiyon, A9 hata/kaynak ömrü, A10 sağlanan provider üzerinden runtime seçim. Her konu küçük bir davranış ve karşı örnekle görülür; tam kalıcılık/plugin altyapısı hazırdır. Kümülatif A1–A10 haritası ikinci turun tamamlandığını kaydeder; yeniden tam anlatım yapılmaz. **Kanıt:** 100 sandalye ana senaryosu; 60+40 sevk; tekrarlanan üretim tamamlama isteğinin ikinci stok yaratmaması; güncel model ve sınırlar. Maliyet yöntemi basit sağlanan uygulamadır, W10'da derinleşir.

### W5 — A1: Ayrıştırma ve domain modeli

**Değişiklik talebi:** Aynı sandalye farklı kumaş/renk varyantlarıyla üretilebilsin. **Artım:** Ürün tanımı, varyant, BOM revizyonu ve iş emri sorumluluklarını ayrıştırın; tek “ERPManager” sınıfında toplanan işleri taşıyın. **Geri bağ:** A2 kurallar, A3 ilişkiler.  
**Kanıt:** İki varyantın birbirini etkilemeyen planı; aynı bileşenin farklı BOM dallarındaki toplamı; where-used sonucu; eski testlerin korunması. **Sorunuz:** Varyant değişince hangi nesne kimliği değişmeli?

### W6 — A2: Encapsulation ve invariant

**Talep:** Kullanıcı stok, sipariş veya iş emri miktarını geçersiz biçimde değiştiremesin. **Artım:** Kontrollü command metotları; miktar/birim doğrulaması; yayımlanmış BOM'un değişmezliği; sipariş ve iş emri geçiş kuralları. **Geri bağ:** A1 sorumluluk, A7 referans sızıntısı.  
**Kanıt:** Negatif miktar, geçersiz durum, yetersiz stok, tüketilmiş malzemeyi tekrar tüketme reddi; her ret sonrasında aynı state. Public setter ile güvenceyi bozan karşı örneği düzeltin.

### W7 — A3: İlişkiler, sahiplik ve izlenebilirlik

**Talep:** Servise gelen sandalyenin üretim partisi ve hammaddesi bulunabilsin. **Artım:** Lot → malzeme çıkışı → iş emri → üretim partisi → sevk → servis bağlantısı; fason emanet stok ilişkisi. **Geri bağ:** A2 yaşam döngüsü, A8 koleksiyon.  
**Kanıt:** Çoklu kaynak lotu olan bir üretim partisi; hem geriye hem ileriye iz; kompozisyon/association ve cardinality diyagramı. Bir lotu kopyalamakla ona referans vermenin farkını açıklayın.

### W8 — A4: Kalıtım, overriding ve yerine kullanılabilirlik

**Talep:** Üretim ve sevkiyat öncesinde farklı uygunluk denetimleri aynı mekanizmadan çalışsın. **Artım:** Yan etkisiz `evaluate(context) → Decision` sözleşmeli soyut kontrol ve en az iki alt tür; stok uygunluğu ve kalite serbest bırakma örnekleri. **Geri bağ:** A2 sözleşme, A3 collaborator.  
**Kanıt:** Ortak contract testleri; state değiştiren veya beklenmedik önkoşul ekleyen alt türün reddi; overriding/overloading ve constructor zinciri açıklaması. Gerçek iş akışının zaten korunan sert kuralları devre dışı bırakılamaz.

### W9 — A5: Interface ve dış sistem sınırları

**Talep:** B2B ve pazaryeri siparişleri aynı domain modeline gelsin. **Artım:** `ChannelConnector`, `CarrierGateway`, `DocumentGateway` portları; iki sahte kanal ve bir kargo/e-belge yanıtı. Dış SKU ile iç varyant eşleştirmesi açık olsun. **Geri bağ:** A4 contract, A9 failure.  
**Kanıt:** Farklı dış formatlardan eş sipariş; bilinmeyen SKU; timeout; tekrarlı sipariş; kısmi sevk etiketi. **Ara entegrasyon:** W1–W9 senaryoları ve web ekranları birlikte çalışır.

### W10 — A6: Composition, Strategy ve maliyet

**Talep:** Bayi, pazaryeri ve mağaza için farklı fiyat/iskonto/kargo uygulanabilsin. **Artım:** Fiyatlandırma, gider dağıtımı ve risk/onay politikalarını collaborator olarak seçin. Planlanan maliyet ile fiilî malzeme/işçilik/fason giderini ayırın. **Geri bağ:** A5 port, A2 invariant.  
**Kanıt:** Ortak 1.050 TL maliyet ve 220 TL katkı hesabı; yeni kanalın ana sipariş akışını bozmaması; hatalı marj paydasının reddi; komisyon/fason/ambalajın iki kez sayılmaması. Müşteri/tarih/kanal bazlı fiyat önceliğini test edin.

### W11 — A7: Kimlik, eşitlik, kopyalama ve immutability

**Talep:** Eski siparişin fiyatı, yeni liste ve BOM revizyonuyla değişmesin. **Artım:** `Money`, `Quantity`, `ProductId` gibi değer nesneleri; sipariş fiyatı ve iş emri BOM snapshot'ı; güvenli dış koleksiyon görünümleri. **Geri bağ:** A2 değişmezlik, A3 sahiplik.  
**Kanıt:** R2 yayımlandıktan sonra R1 iş emri aynı; fiyat güncellense de eski sipariş toplamı aynı; equals/hashCode uyumu; shallow/deep copy ayrımı. BigDecimal'da sayısal eşitlik ile scale içeren equals farkını normalize ederek yönetin.

### W12 — A8: Generics, collections ve modüller arası sorgu

**Talep:** Açık iş emirleri, ödeme onayı bekleyen kayıtlar ve tekrarlayan servis sorunları filtrelenebilsin. **Artım:** Tür güvenli repository ve sorgu sonuçları; Map ile kimlik erişimi, Set ile tekillik; personel/vardiya kaydını işçilik girdisine bağlayın. **Geri bağ:** A7 equality, A5 interface.  
**Kanıt:** Yanlış türün derleme sınırında yakalanması, deterministik rapor sırası, farklı tenant kayıtlarının karışmaması; bounded type/wildcard kullanım gerekçesi. İnsan kaynakları verisinin satış raporuna gereksiz taşınmadığını gösterin.

### W13 — A9: Kaynaklar, kalıcılık ve hata yönetimi

**Talep:** Süreç yeniden başlasa da kayıtlar ve bekleyen entegrasyon işi kaybolmasın. **Artım:** Sağlanan kalıcılık/işlem adaptörünü portlara bağlayın; dosya içe aktarımı ve servis eki kaynaklarını kapatın; başarısız dış gönderim için outbox/retry sonucu gösterin. **Geri bağ:** A5 bağımlılık, A7 snapshot.  
**Kanıt:** Save/load; bozuk içe aktarımda kısmi state yayınlanmaması; gönderim sonrası yanıt kaybında aynı idempotency key; dosya kaynağının kapanması. Domain commit ile dış servis başarı durumunu ayrı raporlayın. Gerçek atomik outbox için kayıt ve olayın aynı kalıcı transaction'da yazılması gerekir.

### W14 — A10: Runtime metadata, plugin ve son sürüm

**Talep:** Yeni connector/AI belge okuyucu iş akışı değiştirilmeden takılabilsin. **Artım:** Sağlanan provider paketlerini ServiceLoader ile keşfedin; yapılandırmadan onaylı provider seçin. Belge okuma önerisini güven skoru ve insan onayına bağlayın. **Geri bağ:** A5 sözleşme, A6 dispatch, A9 hata.  
**Kanıt:** Provider yok/çift kimlik/hatalı yanıt; düşük güvenli belgenin doğrudan mal kabul/ödeme oluşturmaması; yüklenen sınıf bilgisi ve initialization izi. **v1.0:** Bütün fabrika akışı, tarayıcı demosu, regresyon, kurtarma ve bireysel savunma.

## 6. İş mantığının teknik uygulama kuralları

- Para için `double` kullanmayın; BigDecimal ve para birimi birlikte taşınsın. Miktarlar temel birimde tutulsun; adet gerektiren girdide kesir reddedilsin. Ara hesap ve son parasal yuvarlama politikası belgelenmelidir.
- `Money("10.0","TRY")` ile `Money("10.00","TRY")` domain'de aynı değeri temsil ediyorsa equals/hashCode da bunu korusun. TRY ve EUR kur bilgisi olmadan toplanamaz.
- Domain state'i repository arkasında saklamak tek başına atomiklik sağlamaz. Birden çok nesneyi güncelleyen komut, sağlanan transaction/unit-of-work sınırında başarılı olur veya bütünüyle geri alınır.
- Entegrasyon olayı commit öncesinde dış servise gönderilmez. Atomik kalıcı kayıt/outbox, tekrar deneme ve alıcı tekilleştirmesi birlikte ele alınır; “exactly once ağ teslimi” varsayılmaz.
- Onaylı fiyata etki eden teklif değişikliğinde eski onay/snapshot yeniden değerlendirilir. Ödeme talebi IBAN, tutar ve belge sürümünü onaya bağlar; otomatik gerçek ödeme yapılmaz.
- GUI validation domain doğrulamasını kaldırmaz. Aynı komut CLI, HTTP veya connector'dan gelse de aynı iş kurallarına tabidir.
- Dosya depolama adaptörü metadata ve erişim hakkı döndürür; servis fotoğrafı herkesçe okunabilir genel URL'ye dönüştürülmez.
- AI çıktısı kaynak belge, önerilen alanlar, güven ve onay geçmişini içerir; “AI söyledi” maliyet veya stok kanıtı değildir.

## 7. Asgari test kataloğu

| Kod | Senaryo | Başarı ölçütü |
| --- | --- | --- |
| O01 | 100 sandalye planı | Ortak sayısal sonuçlar; state değişmez |
| O02 | BOM döngüsü / birim uyuşmazlığı | Açık hata; kısmi plan kullanılmaz |
| O03 | Aynı malzeme iki BOM dalında | Toplam ihtiyaç doğru; stok iki kez kullanılamaz |
| O04 | Yetersiz malzeme rezervasyonu | Hiçbir kalem kısmen ayrılmaz |
| O05 | Aynı talebin tekrarı | Tek iş etkisi; farklı payload çatışır |
| O06 | R2 yayımlama | R1 iş emri ve maliyeti korunur |
| O07 | Malzeme çıkışı + mamul kabul | Çift tüketim yok; WIP kapanışı açıklanır |
| O08 | 60+40 sevk; 101'inci adet | İlk ikisi geçer; fazlası reddedilir |
| O09 | İade ve kalite | İade karantinada; izinle serbest stok |
| O10 | Fason gönderim/dönüş | Emanet mülkiyet ve miktar izi korunur |
| O11 | Maliyet/kanal gideri | 1.050 TL üretim, 220 TL katkı; çift gider yok |
| O12 | Snapshot ve koleksiyon sızıntısı | Dış mutation iç state'i değiştirmez |
| O13 | Connector timeout ve yanıt kaybı | Doğru durum, tekrar anahtarı, görünür hata |
| O14 | Bozuk dosya ve restart | Kaynak kapalı; geçerli önceki state korunur |
| O15 | Yetkisiz bayi/tenant/ödeme onayı | Erişim veya işlem reddedilir |
| O16 | Düşük güvenli AI önerisi | Onay kuyruğu; gerçek hareket yok |

Testlerde gerçek saat, rastgele kimlik veya internet zorunlu olmasın; `Clock`, ID üretici ve fake adaptör kullanın. Değişiklik sonrası eski davranış testleri çalışır. Testin başarısı, yalnız ekranda görülen yazıdan değil, ilgili stok/rezervasyon/belge durumundan doğrulanır.

## 8. Teslimler ve değerlendirme

Her haftalık teslim: çalışan kod ve komutlar, yeni test/sonuç, korunacak regresyon, UML veya akış farkı, kısa tasarım kararı, bilinen sınırlama ve dış katkı/AI açıklaması. Haftalık rapor 1–2 sayfayı hedeflesin; gerekli test çıktısı ve diyagram ek olabilir. Bir kavramın adını yazmak yerine kodda nerede bulunduğunu ve hangi problemi çözdüğünü gösterin.

**Kapılar:** W1 prototip; W4 v0.1; W9 entegrasyon; W14 v1.0. Son ürün yeni başlanan ayrı bir dönem sonu ödevi değildir. Ortak altyapı kullanıldıysa sürümü ve kendi katkınız belirtilir. Ekip çalışması verilirse her öğrenci kendi kararını/testini savunur; görev dağılımı bütün bir kavram kümesini hiç öğrenmemeye gerekçe olamaz.

Aşağıdaki rubrik proje içindeki kalite puanını tanımlar; dersin toplam not ağırlığı resmî izlenceden alınır.

| Ölçüt | Proje içi pay |
| --- | ---: |
| İş kuralları ve fabrika zincirinin doğruluğu | 30 |
| Nesne modeli, sözleşmeler ve değiştirilebilirlik | 25 |
| Test, hata yolu ve regresyon kanıtı | 20 |
| Entegrasyon, yeniden üretim ve kalıcılık | 15 |
| Bireysel açıklama ve teknik belge | 10 |
| **Toplam** | **100** |

## 9. Son demo ve canlıya geçiş düşüncesi

Son demoda üretim planlayıcısı 100 sandalyelik ihtiyacı açar; satınalmacı eksik malzemeyi kabul eder; operatör kiosk görünümünden aşama/malzeme bilgisi girer; depocu 60+40 sevk eder; satış/finans kullanıcısı maliyet ve hakedişi görür. Bayi yalnız kendi siparişini görür. Bir servis kaydı üretim ve hammadde lotuna bağlanır. Tekrarlanan webhook ve düşük güvenli belge önerisi gösterilir.

Fabrikanın canlıya geçişi için ürün/BOM/cari/fiyat/stok/açık sipariş aktarımlarını dry-run → doğrulama → onaylı açılış hareketleri sırasına koyun. Birinci faz çekirdek ve üretim; ikinci faz kanallar/lojistik/finans; üçüncü faz servis, İK ve gelişmiş otomasyondur. Kullanıcı eğitimi, yedek/geri dönüş, destek ve SLA tasarımı release belgesinde bulunur. Test ortamında ölçmediğiniz kapasite, kesintisizlik veya mevzuata uygunluk garantisini vermeyin.

## 10. Teknik kaynaklar ve kullanma yolu

- [dev.java — Classes and Objects](https://dev.java/learn/classes-objects/), [Interfaces](https://dev.java/learn/interfaces/) ve [Generics](https://dev.java/learn/generics/): İlgili haftanın Java mekanizmasını çalışın, sonra fabrikanın nesnesine uygulayın.
- [Java 25 BigDecimal](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/math/BigDecimal.html): Para, yuvarlama ve equality/scale ayrıntılarını buradan doğrulayın.
- [Java 25 ServiceLoader](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/ServiceLoader.html): W14 provider keşfinin sözleşmesi; plugin güvenlik sandbox'ı sağlamaz.
- [JUnit kılavuzu](https://docs.junit.org/6.1.0/overview.html): Domain ve contract testleri için.
- [Ders kaynakçası](KAYNAKCA.md): Ek alıştırma ve tasarım kaynakları; eski kayıt sistemi örneklerini bu rehberin fabrika senaryosuyla eşleştirin.

İş ihtiyaçlarının kaynağı ders için paylaşılan sandalye fabrikası ERP vaka metnidir. Reçete sayıları, sınıf isimleri, algoritma tercihleri ve ders kapsamı bu vakayı uygulanabilir/test edilebilir hâle getirmek için bu rehberde tanımlanmıştır.
