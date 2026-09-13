# 2026–2027 Güz Dönemi — Bütünden Ayrıntıya, Çalışan Sistemlerle Öğrenme

Bu depo, **CEN 302 Operating Systems**, **SE 237 Object Oriented Programming** ve **CMPE 351 Database Systems** derslerini ortak bir öğretim yaklaşımıyla yürütmek için hazırlanır. Amacımız, öğrencinin dönem başında bütününü gördüğü bir sistemi her hafta biraz daha iyi anlaması, geliştirmesi, sınaması ve kendi kararlarını bağımsız biçimde savunabilmesidir.

CEN 302'de dosya okuyan küçük bir C programından **Mini Systems Workbench** adlı sistem araçları ve deneyleri bütününe ilerleyeceğiz. SE 237 ve CMPE 351'de ise **gerçek bir sandalye fabrikasının ihtiyaçlarından hareketle tasarlanan Factory ERP** üzerinde çalışacağız: biri nesne modeli ve iş davranışlarını, diğeri veri modeli, sorgular ve işlem doğruluğunu ele alacak.

| Bilgi | Açıklama |
| --- | --- |
| Öğretim elemanı | M. Ali Bayram |
| Ders oturumu | 155 dakika: 125 dakika etkin çalışma + 30 dakika ara |
| Dönem | 2026–2027 Güz |
| Belgenin amacı | Ortak hedefleri, derslerin kapsamını, haftalık çalışma biçimini, teslimleri ve hazırlık sorumluluklarını açıklamak |
| Belgenin dili | Türkçe; derslerin teknik materyalleri ve değerlendirme dili ilgili İngilizce izlenceyle uyumlu hazırlanır |
| Mevcut durum | Öğretim ve proje tasarımı. Güncel starter, referans uygulama ve test paketleri henüz bu depoda üretilmiş değildir. |

> **Dönem boyunca yapmak istediğimiz iş:** Öğrenciye önce anlamlı bir bütün göstermek; ikinci turda tüm konuları üç haftaya yayarak işlemek; üçüncü turda her konuyu daha ayrıntılı ele alıp çalışan ürüne yapılan küçük ve kanıtlanabilir değişikliklerle derinleştirmek.

## İçindekiler

- [1. Amaç ve öğrenme hedefleri](#amac)
- [2. Üç dersin ortaklığı ve sınırları](#dersler)
- [3. Depo yapısı ve hangi belgenin esas alınacağı](#belgeler)
- [4. Dönemin spiral yapısı](#spiral)
- [5. Ortak fabrika vakası ve ürün kapsamı](#fabrika)
- [6. CEN 302: Mini Systems Workbench](#cen302)
- [7. SE 237: Factory ERP'nin nesne modeli](#se237)
- [8. CMPE 351: Factory ERP'nin veri sistemi](#cmpe351)
- [9. Haftalık öğrenme ve teslim düzeni](#haftalik-duzen)
- [10. Sürüm kapıları ve kabul ölçütleri](#kabul)
- [11. Ölçme ve değerlendirme](#degerlendirme)
- [12. Yapay zekâ, kaynak kullanımı ve bireysel sorumluluk](#ai)
- [13. Destek, erişilebilirlik ve iş yükü](#destek)
- [14. Öğretim ekibinin hazırlık ve yürütme planı](#hazirlik)
- [15. Dönem sonunda başarıyı nasıl anlayacağız?](#basari)

<a id="amac"></a>

## 1. Amaç ve öğrenme hedefleri

Öğrencinin bir konuyu ilk kez öğrenirken onun sistemdeki yerini de görmesini istiyoruz. Süreç, arayüz, kalıtım, transaction veya indeks gibi kavramlar; gerçek bir işin hangi aşamasında gerekli oldukları ve hangi hatayı önledikleri üzerinden öğrenilecek. Her yeni konu, öğrencinin zaten çalıştırdığı ve tanıdığı uygulamayla ilişkilendirilecek.

Bu yaklaşımın üç somut sonucu olmalı:

1. **Sistem anlayışı:** Öğrenci girdiden sonuca giden yolu gösterebilmeli; parçaları, sınırları, sorumlulukları ve birbirlerine bağımlılıklarını açıklayabilmeli.
2. **Kanıta dayalı geliştirme:** Bir değişikliğin doğru olduğunu test, sorgu sonucu, çalışma izi, karşı örnek veya ölçümle gösterebilmeli; önceki davranışların korunduğunu doğrulayabilmeli.
3. **Bağımsız teknik karar:** Verilen çözümü inceleyebilmeli, hatasını bulabilmeli, değişen bir gereksinime uyarlayabilmeli ve kendi tercihinin nedenini savunabilmeli.

Dönem sonunda öğrenciden beklediğimiz beceriler şunlardır:

| Beceri | Gözlenebilir karşılığı |
| --- | --- |
| Kavramı konumlandırma | Kavramın sistem haritasındaki yerini ve en az iki bağlantısını gösterir. |
| Mekanizmayı açıklama | Kodun, nesnelerin veya verinin hangi sırayla değiştiğini izler. |
| Sonucu tahmin etme | Çalıştırmadan önce çıktı, hata, durum değişimi veya performans etkisi hakkında gerekçeli tahminde bulunur. |
| Doğruluğu sınama | Mutlu yolun yanında sınır durumunu, geçersiz girdiyi ve yanlış çözümü açığa çıkaran karşı örneği kullanır. |
| Güvenli değişiklik yapma | Yeni davranışı ekler; ilgili eski testlerin geçmeye devam ettiğini gösterir. |
| Kararı savunma | Alternatifleri, seçtiği çözümün bedelini ve geçerli olduğu koşulları açıklar. |
| Çalışmayı yeniden üretme | Başka birinin aynı sürümü kurup aynı kanıtı elde etmesini sağlar. |
| Katkıyı ayırt etme | Sağlanan altyapıyı, dış kaynakları, AI katkısını ve kendi çalışmasını açıkça belirtir. |

Bu hedefler hazırlık sorularını, sınıf içi uygulamayı, proje teslimlerini ve sınavları birlikte yönlendirir. Öğrenci her değerlendirmede farklı bir çalışma kültürüne geçmek zorunda kalmaz.

<a id="dersler"></a>

## 2. Üç dersin ortaklığı ve sınırları

| Ders | Temel soru | Dönem ürünü | Süre ve model | Ana teknik ortam |
| --- | --- | --- | --- | --- |
| **CEN 302 — Operating Systems** | Bir program işletim sisteminde nasıl çalışır, kaynak kullanır ve gözlemlenir? | Mini Systems Workbench | 14 hafta · `1 + 3 + 10` | C, Linux, xv6-riscv, QEMU ve mekanizma modelleri |
| **SE 237 — Object Oriented Programming** | İş kurallarını koruyan ve değiştirilebilir kalan nesneler nasıl tasarlanır? | Factory ERP'nin Java iş mantığı ve web üzerinden kullanılan uygulaması | 14 hafta · `1 + 3 + 10` | Rehberde seçilen Java 25 LTS, Maven Wrapper, JUnit 6; sağlanan web ve kalıcılık iskeleleri |
| **CMPE 351 — Database Systems** | Veriler nasıl modellenir, sorgulanır ve eşzamanlı işlemlerde doğru tutulur? | Factory ERP'nin ilişkisel veri omurgası | 12 hafta · `1 + 3 + 8` | Rehberde seçilen PostgreSQL 18, SQL, `psql`, migration ve deney paketleri |

CMPE 351, İstanbul Bilgi Üniversitesi için hazırlanan 12 öğretim haftalık plana sahiptir. CEN 302 ve SE 237'nin 14 haftalık takvimi bu derse aynen uygulanmaz.

### Aynı vaka üzerinde iki farklı öğrenme sorumluluğu

SE 237 ve CMPE 351 aynı ürün adlarını, sentetik verileri ve fabrika kurallarını kullanır. Böylece bir iş kuralının uygulama ve veritabanı tarafındaki karşılıkları birlikte düşünülebilir.

Örneğin “aynı malzeme iki siparişe birden ayrılamaz” kuralı:

- **SE 237'de** rezervasyon davranışının hangi nesneye ait olduğu, hangi durumların geçerli olduğu, hata sonrasında nesnelerin tutarlılığı ve arayüz sözleşmeleri üzerinden incelenir.
- **CMPE 351'de** aynı kuralın anahtarlar, transaction sınırı, kilitleme, eşzamanlı oturumlar ve tekrar deneme davranışıyla nasıl korunduğu kanıtlanır.

**Dersler birbirinin önkoşulu değildir.** SE 237 öğrencisi diğer dersin veritabanını tamamlamakla, CMPE 351 öğrencisi Java uygulaması veya frontend geliştirmekle yükümlü değildir. Ortak senaryonun diğer katmanına ihtiyaç duyulduğunda öğretim ekibi gerekli adaptörü, istemciyi veya test verisini sağlar. Birlikte kullanım mümkün olduğunda her dersin öğrenme kanıtı ayrı ve bireysel olarak görünür kalır.

CEN 302 kendi iş yükü ve projesi üzerinden ilerler. Dosya, bellek, eşzamanlılık ve kalıcılık gibi kavramsal bağlantılar kurulabilir; CEN projesinin ERP'ye bağlanması bir teslim şartı değildir.

<a id="belgeler"></a>

## 3. Depo yapısı ve hangi belgenin esas alınacağı

### Okumaya nereden başlamalı?

| İhtiyaç | Okunacak belge | Kullanım amacı |
| --- | --- | --- |
| Genel hedef ve çalışma düzeni | Bu README | Üç dersin ortak öğretim sözleşmesi ve hazırlık planı |
| CEN 302'nin güncel ürünü | [CEN 302 proje planı](CEN_302/PROJE.md) | Byte Counter, Workbench, xv6 kapsamı, haftalık artımlar ve kabul koşulları |
| CEN 302'nin ilk dört haftası | [W1](CEN_302/HAFTA01.md) · [W2](CEN_302/HAFTA02.md) · [W3](CEN_302/HAFTA03.md) · [W4](CEN_302/HAFTA04.md) | Ayrıntılı öğretmen akışı, hazırlık, studio, test ve kabul planları |
| SE 237'nin güncel ürünü | [SE 237 ERP öğrenci rehberi](SE_237/ERP_OGRENCI_REHBERI.md) | Fabrika iş kuralları, nesne modeli, 14 haftalık proje ve test kataloğu |
| SE 237'nin ikinci turu | [W2](SE_237/HAFTA02.md) · [W3](SE_237/HAFTA03.md) · [W4](SE_237/HAFTA04.md) | A1–A3, A4–A6 ve A7–A10 için öğretmen akışı, studio, test ve kabul planları |
| CMPE 351'in güncel ürünü | [CMPE 351 ERP öğrenci rehberi](CMPE_351/ERP_OGRENCI_REHBERI.md) | Veri modeli, SQL, transaction sözleşmesi, 12 haftalık proje ve test kataloğu |
| CMPE 351'in ikinci turu | [W2](CMPE_351/HAFTA02.md) · [W3](CMPE_351/HAFTA03.md) · [W4](CMPE_351/HAFTA04.md) | M1–M3, M4–M6 ve M7–M8 için ders akışı, laboratuvar, deney ve kabul planları |
| CMPE 351'in pedagojik ayrıntıları | [CMPE 351 ders modeli](CMPE_351/README.md) | `1 + 3 + 8`, sekiz modül ve değerlendirme taslağı; proje senaryosunda ERP rehberi esas alınır |
| Teknik okuma ve başvuru | [CEN kaynakları](CEN_302/KAYNAKCA.md) · [SE kaynakları](SE_237/KAYNAKCA.md) · [CMPE kaynakları](CMPE_351/KAYNAKCA.md) | Haftanın öğrenme hedefi için seçilecek kaynak havuzu |
| İlk haftanın öğretmen akışı | [CEN W1](CEN_302/HAFTA01.md) · [SE W1](SE_237/HAFTA01.md) · [CMPE W1](CMPE_351/HAFTA01.md) | Sınıf uygulaması hazırlığı; SE/CMPE'deki eski senaryolar ERP'ye uyarlanmalıdır |
| Önceki tasarımlar ve üretimler | [Arşiv haritası](archive/README.md) | Tarihsel inceleme ve doğrulanarak yeniden kullanım |

### Belge önceliği

Belgeler arasında farklılık olduğunda şu ayrım uygulanır:

1. **Resmî ders koşulları:** Onaylanmış izlence, not ağırlıkları, sınav düzeni ve kurumsal takvim için esas kaynaktır. Bu depo idari onay yerine geçmez.
2. **Ortak öğretim yaklaşımı:** Amaç, spiral çalışma, kanıt üretme ve ortak işleyiş için ana README kullanılır.
3. **Güncel proje kapsamı:** CEN 302'de `PROJE.md`; SE 237 ve CMPE 351'de `ERP_OGRENCI_REHBERI.md` uygulama ve kabul sözleşmesidir. CMPE 351'in 12 haftalık modül sırası korunur.
4. **Haftalık görev paketi:** O haftanın başlangıç sürümünü, sınırlı Core işini, teslimini ve rubriğini somutlaştırır; yeni bir zorunluluk sessizce eklenmez.
5. **Eski örnekler ve arşiv:** Güncel sözleşmeyle karşılaştırılmadan öğrenciye yürürlükteki görev olarak verilmez.

**Senaryo geçişi:** Course Registration ve Campus Learning Hub, önceki SE 237 ve CMPE 351 tasarımlarının örnekleridir. İlgili eski [SE proje planı](SE_237/PROJE.md), [CMPE proje planı](CMPE_351/PROJE.md), ilk hafta belgeleri ve CMPE ders README'sinde bu örnekler hâlâ bulunabilir. Güncel proje, **sandalye fabrikası ERP'sidir**. Eski örneklerdeki kayıt/kontenjan kabul koşulları yeni projeye ek yük olarak taşınmaz.

### Şu anda depoda ne var?

```text
.
├── README.md                       Ortak hedefler ve ders işleme sistemi
├── CEN_302/
│   ├── PROJE.md                    Güncel Workbench proje sözleşmesi
│   ├── HAFTA01.md                  İlk hafta öğretmen akışı
│   ├── HAFTA02.md                  İkinci tur: A1–A4; çalışan iskelet
│   ├── HAFTA03.md                  İkinci tur: A5–A7; davranış artımı
│   ├── HAFTA04.md                  İkinci tur: A8–A10; v0.1 kabulü
│   └── KAYNAKCA.md                 Teknik kaynaklar
├── SE_237/
│   ├── ERP_OGRENCI_REHBERI.md       Güncel fabrika/OOP proje sözleşmesi
│   ├── PROJE.md                    Önceki senaryonun proje tasarımı
│   ├── HAFTA01.md                  ERP'ye uyarlanacak ilk hafta akışı
│   ├── HAFTA02.md                  İkinci tur: A1–A3; fabrika iskeleti
│   ├── HAFTA03.md                  İkinci tur: A4–A6; rezervasyon dilimi
│   ├── HAFTA04.md                  İkinci tur: A7–A10; v0.1 kabulü
│   └── KAYNAKCA.md                 Teknik kaynaklar
├── CMPE_351/
│   ├── README.md                   12 haftalık model; eski senaryo izleri içerir
│   ├── ERP_OGRENCI_REHBERI.md       Güncel fabrika/veritabanı sözleşmesi
│   ├── PROJE.md                    Önceki senaryonun proje tasarımı
│   ├── HAFTA01.md                  ERP'ye uyarlanacak ilk hafta akışı
│   ├── HAFTA02.md                  İkinci tur: M1–M3; migration ve sorgu
│   ├── HAFTA03.md                  İkinci tur: M4–M6; transaction ve plan
│   ├── HAFTA04.md                  İkinci tur: M7–M8; v0.1 veri sürümü
│   └── KAYNAKCA.md                 Teknik kaynaklar
└── archive/                        Önceki belgeler ve üretimler
```

Bu depoyu bugün klonlamak, tarif edilen yeni uygulamaların çalıştırılabilir sürümlerini sağlamaz. Starter, referans çözüm, otomatik testler, veri üreticileri, web iskeletleri ve ders videoları hazırlanacak materyallerdir. Arşivde bir dosyanın veya kodun bulunması, güncel plana uygunluğunun ve çalıştığının doğrulandığı anlamına gelmez.

<a id="spiral"></a>

## 4. Dönemin spiral yapısı: dersi baştan sona üç kez görmek

**Dönem boyunca dersin tamamı üç turda işlenir; her tur bir öncekinden daha ayrıntılıdır.** CEN 302 ve SE 237 için bu turlar sırasıyla **1 hafta + 3 hafta + 10 hafta** sürer. CMPE 351'in mevcut 12 haftalık takviminde üçüncü tur sekiz modüle ayrılır: **1 + 3 + 8**. Takvim farkı, üç tur ilkesini değiştirmez.

| Tur | CEN 302 / SE 237 | CMPE 351 | Ayrıntı düzeyi | Dönem ürünündeki karşılığı |
| --- | --- | --- | --- | --- |
| **1. tur — Panorama** | W1: on çapanın tamamı | W1: sekiz modülün tamamı | Büyük resim, temel sorular, kavramlar arası ilişkiler | Referans gösterimi, küçük prototip ve ilk harita |
| **2. tur — Sistematik ilk işleyiş** | W2–W4: on çapa üç haftaya dağıtılır | W2–W4: sekiz modül üç haftaya dağıtılır | Her konu için mekanizma, küçük örnek, hata/karşı örnek ve rehberli uygulama | İskelet → dikey dilim → `v0.1` |
| **3. tur — Derinleşme** | W5–W14: haftada bir çapa | W5–W12: haftada bir modül | Ayrıntılı tasarım, uygulama, deney, ölçüm ve bağımsız savunma | Birikimli artımlar ve `v1.0` |

**İkinci turda tüm modüller üç haftanın toplamında bir kez işlenir.** W2, W3 ve W4 ayrı tam tekrarlar değildir. Bir haftanın konusunu açıklarken önceki bir kavrama kısa bağlantı kurmak, dersi baştan sona yeniden anlatmak sayılmaz. İkinci tur W4 sonunda; üçüncü tur CEN/SE'de W14, CMPE'de W12 sonunda tamamlanır.

### İkinci turun derslere göre konu dağılımı

| Ders | W2 — İkinci turun ilk bölümü | W3 — İkinci turun orta bölümü | W4 — İkinci turun son bölümü |
| --- | --- | --- | --- |
| CEN 302 | **A1–A4:** OS sınırı ve syscall, süreç, thread, IPC/descriptor | **A5–A7:** senkronizasyon/deadlock, CPU zamanlama, adres uzayı/çeviri | **A8–A10:** sanal bellek/izolasyon, dosya sistemi/tutarlılık, depolama/I/O |
| SE 237 | **A1–A3:** nesne modeli, encapsulation/invariant, ilişkiler/UML | **A4–A6:** kalıtım/yerine kullanılabilirlik, interface/sözleşme, polimorfizm/composition/Strategy | **A7–A10:** kimlik/eşitlik/kopyalama, generics/collections, exception/kaynak, runtime metadata/plugin |
| CMPE 351 | **M1–M3:** DBMS/iş yükü, model/şema/bütünlük, ilişkisel cebir/SQL | **M4–M6:** bağımlılıklar/normalizasyon, transaction/eşzamanlılık, depolama/indeks/sorgu işleme | **M7–M8:** dağıtık/bulut/dayanıklılık, modern veri modelleri ve platform kararları |

Bu dağılım ikinci turdaki ana öğretim odağını belirler. Her konunun W1'deki kısa tanıtımından daha ileri gidilir; ayrıntılı bağımsız uygulama üçüncü turda tamamlanır. İkinci turda küçük bir model, hazır örnekte değişiklik veya kontrollü test yeterli olabilir. Bir konunun ilk işlenişini yalnız adını söyleyip sonraya bırakmak yeterli değildir.

### Öğretim turu ile ürün geliştirme adımını ayırma

İskelet, dikey dilim ve entegrasyon ürünün gelişme adımlarıdır; haftanın bütün ders içeriğinin adı değildir. Örneğin CEN W3'te launcher üzerindeki bekleme değişikliği yapılırken esas ders konuları A5–A7'dir. W4'te ürün kabulü yapılması A8–A10 öğretimini ortadan kaldırmaz.

- **W1:** Küçük prototip çalışır, bütün kavramların ilk haritası çıkarılır; ileri özellikler referans/model olarak etiketlenir.
- **W2:** İlk konu grubu örnek ve karşı örneklerle işlenir; sağlanan parçalarla çalışan iskelet kurulur. Haritada yalnız bu haftanın ilgili satırları güncellenir.
- **W3:** İkinci konu grubu işlenir; mevcut ürüne sınırlı dikey dilim veya davranış düzeltmesi eklenir. Önceki konular gerektiği kadar geri çağrılır.
- **W4:** Son konu grubu işlenir; üç haftanın birikimiyle `v0.1` kapısı değerlendirilir. Son harita tüm konuları kapsar; bu bir belge/kapsam kontrolüdür, yeni tam anlatım turu değildir.
- **W5 ve sonrası:** Her hafta bir çapa/modül ayrıntılı işlenir ve en az iki önceki/komşu kavramla ilişkilendirilir. Aynı ürün testlerle büyür.

W4'te temel akışın çalışması için henüz öğrencinin bağımsız yazmadığı altyapı öğretim ekibi tarafından sağlanır. Her teslim mevcut davranış, sağlanan kod, model ve gelecekteki öğrenci artımını ayırır. Üçüncü turda aynı hazır örneğin nasıl ve neden çalıştığı daha ayrıntılı incelenir; öğrenci katkısı kademeli artar.

CEN ve SE'deki `A1–A10` etiketleri ders içinde sabittir; aynı numara iki derste aynı konuyu ifade etmez. CMPE `M1–M8` kullanır. Ortak kayıtlarda `CEN/A5`, `SE/A5`, `CMPE/M5` biçimi kullanılır.

<a id="fabrika"></a>

## 5. Ortak fabrika vakası ve ürün kapsamı

### Ürünün amacı

Factory ERP, sandalye üreten bir işletmenin **sipariş → malzeme ihtiyacı → tedarik → üretim → sevkiyat → maliyet ve tahsilat** zincirini izlenebilir hâle getirecek. Kullanıcı bir ekranda yaptığı işlemin stokta, üretimde ve finansal kayıtta hangi sonucu doğurduğunu görebilecek. Ürün kartı, stok listesi ve raporlar bu zincirin parçalarıdır.

Dönem boyunca şu rolleri ve ihtiyaçlarını ele alacağız:

| Rol | Üründen beklediği sonuç |
| --- | --- |
| Üretim planlayıcısı | Siparişin hangi reçeteyle, hangi malzemelerle karşılanabileceğini ve eksiğini görmek |
| Satınalmacı | Eksikten satınalma talebi/siparişi açmak ve kısmi mal kabulünü izlemek |
| Üretim operatörü | İş emri aşamalarını, malzeme kullanımını, sağlam ve fire miktarlarını kaydetmek |
| Depo ve lojistik kullanıcısı | Rezervasyon, lot, transfer, toplama, kısmi sevkiyat ve iadeyi doğru yürütmek |
| Satış ve bayi kullanıcısı | Yetkisi kapsamındaki ürün, fiyat, sipariş ve sevkiyat durumunu görmek |
| Finans ve yönetim kullanıcısı | Maliyet, kanal gideri, cari hareket, onay ve tahsilat eşleşmesini izlemek |
| Servis kullanıcısı | Bir sorunu satılan ürüne, üretim partisine ve kaynak malzemeye bağlamak |

Tarayıcı ve tablet genişliğindeki üretim/depo ekranları hedef kullanım biçimidir. SE 237'de sağlanan ekranlar domain servislerine bağlanır; CMPE 351'de sağlanan ince istemci veri işlemlerini görünür kılar. Öğrencinin esas sorumluluğu, dersinin kavramlarını bu iş akışında doğru gerçekleştirmektir.

### Ana kabul senaryosu: 100 sandalye

Her iki ERP rehberi aynı **sentetik** ürün ve miktarları kullanır. Gerçek fabrika ihtiyacı süreçleri yönlendirir; aşağıdaki veriler gerçek işletmenin kesin reçetesi veya müşteri verisi olarak sunulmaz.

`CHAIR-A` mamulü bir `FRAME-A` yarı mamulü, kumaş ve sünger kullanır. `FRAME-A` ahşap ve vernikten üretilir. R1 reçetesinde sandalye başına net kumaş 1,2 m, girdiye göre planlanan kayıp %20'dir; brüt ihtiyaç `1,2 / (1 − 0,20) = 1,5 m` olur.

| Hammadde | Bir sandalye için brüt ihtiyaç | Başlangıç kullanılabilir stok | 100 sandalye ihtiyacı | Eksik |
| --- | ---: | ---: | ---: | ---: |
| Ahşap | 0,04 m³ | 3 m³ | 4 m³ | **1 m³** |
| Kumaş | 1,5 m | 150 m | 150 m | 0 |
| Sünger | 1 adet | 120 adet | 100 adet | 0 |
| Vernik | 0,1 kg | 10 kg | 10 kg | 0 |

Bu başlangıçta malzeme açısından **75 sandalye üretilebilir**. Bu hesap hazır mamul, makine kapasitesi veya kesin teslim tarihi garantisi değildir. Planı tekrar çalıştırmak stokta ya da rezervasyonda değişiklik oluşturmaz.

Bütünleşik senaryo şu sırayla çalışır:

1. 100 sandalyelik sipariş alınır ve R1 üzerinden ihtiyaç hesaplanır.
2. Eksik 1 m³ ahşap için satınalma süreci başlatılır.
3. Mal kabulü yapılır; gelen miktar ilgili stok hareketiyle kaydedilir.
4. İş emri reçete revizyonlarına bağlanır; malzeme çıkışı ve üretim aşamaları izlenir.
5. Ana senaryoda 100 sağlam sandalye mamul stoğuna alınır.
6. Sipariş önce 60, sonra 40 adet sevk edilir; fazladan sevkiyat reddedilir.
7. Maliyet, kanal giderleri ve tahsilat eşleşmesi gösterilir.

Ana senaryo sonunda ahşap, kumaş ve vernik sıfır; sünger 20 adet kalır. Fire, kısmi üretim, fason, iade ve hata senaryoları ayrı testlerdir.

Rehberlerdeki maliyet fixture'ında 100 sandalye için malzeme 85.000 TL; işçilik 10.000 TL, genel üretim gideri 5.000 TL ve ambalaj 5.000 TL'dir. Toplam üretim maliyeti 105.000 TL, birim maliyet **1.050 TL** olur. Vergi hariç 1.500 TL satış, %10 kanal komisyonu, %2 ödeme komisyonu ve 50 TL kargoyla birim sipariş katkısı **220 TL**'dir. Bu değerler hesaplama testinin beklenen sonuçlarıdır; ayrıntılı varsayımlar ve maliyet yöntemi ERP rehberlerinde bulunur.

### Modüller ve geliştirme derinliği

Fabrikanın bütün ihtiyaçları ürün haritasında görünür olacak. Her modülün dönem içindeki uygulama derinliği ayrıca sınırlandırılacak.

| Alan | Ortak ürün kapsamı | Dönem içindeki yaklaşım |
| --- | --- | --- |
| Ürün ve reçete | Hammadde, yarı mamul, mamul; varyant, birim, çok seviyeli ve revizyonlu BOM | Çalışan çekirdek ve testler |
| Stok ve depo | Hareket, lot, lokasyon, rezervasyon, karantina, transfer, sayım | Çalışan çekirdek ve testler |
| Üretim ve fason | İş emri, aşama, malzeme tüketimi, sağlam/fire miktarı, emanet stok | Çalışan çekirdek; hizmet ve dış belge bağlantıları |
| Satınalma ve satış | Eksikten talep, satınalma, kısmi kabul, sipariş, kısmi sevk, iptal/iade | Çalışan çekirdek ve testler |
| B2B, pazaryeri ve e-ticaret | Ortak sipariş, özel fiyat, dış kimlik eşlemesi, stok/fiyat olayları | Sağlanan portal ve test connector'larıyla sınırlı entegrasyon |
| Lojistik ve servis | Paket, etiket, takip; fotoğraf metadata'sı, servis ve lot izi | Çekirdek örnekler ve sağlanan adaptörler |
| Maliyet ve raporlama | Planlanan/gerçekleşen maliyet, dağıtım, kanal gideri, kârlılık | Sayısal olarak doğrulanan hesap ve sorgular |
| Finans ve ön muhasebe | Cari hareket, ödeme talebi, onay, banka/hakediş eşleşmesi | Sınırlı çalışan akış ve bağlantı sözleşmeleri |
| İK | Personel, vardiya, izin ve üretime işçilik girdisi | Bağlantı düzeyi; bordro ve PDKS için genişleme tasarımı |
| Operasyon ve otomasyon | Tenant/rol, içe aktarım, audit, hata kuyruğu, AI öneri/onay | Dersin kapsamına göre çalışan kanıt ve sağlanan altyapı |

Rehberlerdeki **Uygulama/Çekirdek**, **Bağlantı** ve **Genişleme** düzeyleri hangi işin gerçekten gerçekleştirileceğini belirler. Bağlantı düzeyinde veri veya arayüz sözleşmesi ile başarı/hata örneği gösterilir. Genişleme düzeyinde üretim ortamı bağımlılıkları ve kararlar belgelenir. Haftanın zorunlu işi ayrıca Core olarak belirtilir.

Gerçek pazaryeri, banka, kargo, sanal POS ve e-dönüşüm servisleri dönem demosunda sandbox veya fake adaptörlerle temsil edilir. Tam genel muhasebe, bordro, gerçek ödeme ve resmî belge üretimi dersin kabul şartı değildir. Canlıya geçiş tasarımı; veri aktarımı, kullanıcı eğitimi, yedekleme, geri dönüş ve sağlayıcı kararlarını kapsayan ayrı bir hazırlık çıktısıdır.

### Bütün modüllerin koruyacağı ortak kurallar

- **Tek stok hareket sözleşmesi:** Satınalma, üretim, sevk, transfer ve iade aynı kuralları kullanır. Ekranlar stok bakiyesini doğrudan değiştirerek bu sözleşmeyi atlayamaz.
- **Fiziksel stok ve rezervasyon ayrımı:** Kullanılabilir miktar, serbest fiziksel miktardan aktif rezervasyon çıkarılarak hesaplanır. Karantina serbest stoğa dâhil edilmez; rezervasyon fiziksel tüketim sayılmaz.
- **Geçmişin korunması:** İş emrinde kullanılan BOM revizyonları ve siparişteki fiyat bilgisi sabitlenir. Yeni reçete veya fiyat, geçmiş işlemi sessizce değiştiremez.
- **Miktar ve birim doğruluğu:** Döngülü BOM, tanımsız birim dönüşümü, geçersiz miktar ve aynı malzemeyi iki kez kullanma hataları görünür biçimde reddedilir.
- **Tek iş etkisi:** Aynı anahtar ve içerikle tekrar gelen işlem ikinci stok veya finans kaydı üretmez; aynı anahtarla farklı içerik çatışma olarak ele alınır.
- **Kısmi işlemlerin sınırı:** Sevk edilen toplam siparişi, iade toplamı ilgili sevkiyatı aşamaz. İade, kalite kararı verilmeden serbest stoğa dönmez.
- **Atomiklik ve düzeltme:** Çok kalemli işlem kısmen uygulanmış durumda bırakılmaz. Kesinleşmiş hareket düzeltmeleri ters kayıt ve gerekçeyle izlenir.
- **Erişim sınırı:** Şirketler ve bayiler birbirlerinin yetki dışı kayıtlarını göremez; ödeme talebi ve son onay sorumlulukları ayrılır.
- **Dış sistemlerin gecikmesi:** Merkezî doğruluk ile dış kanal senkronizasyonu ayrı ele alınır; timeout, tekrar, sıra dışı olay ve uzlaştırma test edilir.
- **AI önerisinin durumu:** Belge okuma veya fiyat önerisi, kaynak ve onay bilgisiyle tutulur. Onaylanmamış çıktı doğrudan stok hareketi veya ödeme oluşturmaz.

Bu kuralların ayrıntıları ve test kodları, [SE rehberindeki](SE_237/ERP_OGRENCI_REHBERI.md) ve [CMPE rehberindeki](CMPE_351/ERP_OGRENCI_REHBERI.md) kabul kataloglarında tanımlanır.

<a id="cen302"></a>

## 6. CEN 302: Mini Systems Workbench

### Ne geliştireceğiz?

İlk hafta tek bir dosyanın byte sayısını hesaplayan C programı yazılacak. Bu program sonraki haftalarda başlatılan, beklenen, paralelleştirilen, çıktısı taşınan ve kaynak kullanımı gözlenen ortak iş yükü olacak.

Workbench üç bileşeni aynı proje içinde birleştirir:

- **Linux kullanıcı programları:** Byte Counter, launcher, süreç ve `pthread` deneyleri.
- **xv6-riscv incelemesi ve küçük artımlar:** Gerçek çekirdekte sistem çağrısı, süreç, pipe, sayfa tablosu, kilit ve dosya sistemi yolları.
- **Mekanizma modelleri ve kanıtlar:** Zamanlama, bellek politikaları, depolama ve sistem karşılaştırmaları için kontrollü deneyler.

Her kanıt hangi ortamdan elde edildiğini belirtir. Linux davranışı, xv6 davranışı ve öğretim modelinin sonucu ayrı yorumlanır. xv6 tabanının mevcut özellikleri starter hazırlanırken sabitlenir ve doğrulanır; haftalık görev, tabanda zaten bulunan bir özelliği eksik varsayarak yazılmaz.

### İlk prototip ve W4 hedefi

W1 Byte Counter, geçerli dosyada doğru byte sayısını; boş dosyada sıfırı; bulunamayan dosyada hata kanalını; yanlış argümanda kullanım hatasını gösterir. Byte ile karakter ayrımı test edilir. Eğitmen iskeleyi sağlar, öğrenci sayım ve hata kontrolü üzerinde çalışır.

W4 `v0.1`, Linux'ta program başlatma, bekleme, normal/hatalı/sinyalli sonlanmayı ayırt etme ve çocuk süreçleri toplama davranışlarını birleştirir. Tekrar çalıştırma sonrasında zombie süreç bırakmama ve ayrı xv6 boot/bytecount smoke testi kabul kanıtıdır.

### On çapa, on derinleşme artımı

| Hafta | Çapa ve konu | Workbench'e eklenecek çalışma | Asgari kanıt |
| --- | --- | --- | --- |
| W5 | A1 — OS sınırı, syscall ve kesmeler | Sınırlı xv6 süreç istatistiği çağrısı; host syscall gözlemi | Sayaç değişimi, geçersiz kullanıcı girdisi ve user/kernel sınırı |
| W6 | A2 — Süreçler | En fazla N çocuk çalıştıran launcher | Eşzamanlılık sınırı, her çocuğun bir kez toplanması ve yaşam döngüsü izi |
| W7 | A3 — Thread ve eşzamanlılık | Çok dosyalı `pthread` worker yolu | Seri/thread toplamlarının eşitliği; paylaşım ve ölçüm sınırı |
| W8 | A4 — IPC ve descriptor | Producer → pipe → consumer akışı | Doğru sonuç, gereksiz uçların kapanması ve EOF davranışı |
| W9 | A5 — Senkronizasyon ve deadlock | Sağlanan kuyrukta mutex/condition-variable protokolü | Kayıp/yinelenen iş olmaması, bekleme koşulu ve kilit sırası açıklaması |
| W10 | A6 — CPU zamanlama | FCFS/RR model karşılaştırması ve scheduler trace'i | Elle hesapla eşleşen waiting/turnaround/response sonuçları |
| W11 | A7 — Adres uzayı ve çeviri | Sayfa tablosu diagnostic çıktısı | Sayfa/offset, erişim izinleri ve süreçler arası eşleme karşılaştırması |
| W12 | A8 — Sanal bellek ve izolasyon | Sabit tabandaki fault/allocation davranışını gözleme | İlk dokunma, ayrılan sayfa ve geçersiz erişim izi |
| W13 | A9 — Dosya sistemi ve çökme tutarlılığı | Guest rapor dosyası ve sağlanan recovery deneyi | Dosyayı yeniden açma; kopya diskte kesinti öncesi/sonrası tutarlılık |
| W14 | A10 — Depolama ve I/O | I/O ölçümü ve bütünleşik `v1.0` | Ortamı belirtilmiş ham ölçümler, birikimli regresyon ve uçtan uca savunma |

CEN kapsamı bu on uygulama satırından daha geniştir. *Operating System Concepts* 10. baskının 1–21. bölümleri ve A–D ekleri, proje planındaki kapsam matrisiyle hazırlık, kaynak kodu okuma, deney, model, vaka, sözlü ve sınav kanıtlarına bağlanır. Her konu ayrı bir kernel özelliği olarak yazdırılmaz.

Ayrıntılı sınırlar, hazırlanacak ortam ve kaynak kodu haritası: [CEN 302 proje planı](CEN_302/PROJE.md).

<a id="se237"></a>

## 7. SE 237: Factory ERP'nin nesne modeli

### Ne geliştireceğiz?

Öğrenci, fabrikanın iş davranışlarını sorumluluğu belirli nesneler ve açık sözleşmelerle gerçekleştirecek. İlk ihtiyaç hesaplayıcısı dönem boyunca stok, üretim, sipariş, maliyet ve entegrasyon davranışlarıyla büyüyecek.

Hedef bağımlılık yönü şöyledir:

```text
Web / CLI
    ↓
Uygulama servisleri
    ↓
Domain nesneleri ve iş kuralları

Dış sınırlar: repository, kalıcılık/transaction, dosya, kanal, kargo, belge
```

Domain davranışı ekrandan bağımsız sınanabilir olacak. Aynı komut web, CLI veya connector üzerinden geldiğinde aynı kuralları koruyacak. Modüller tek uygulama içinde çalışabilir; öğrencinin ayrıca mikroservis dağıtımı veya yerel mobil uygulama geliştirmesi beklenmez.

Öğretim ekibi web/oturum iskeleti, adaptörler, test fixture'ları ve gerekli işlem sınırlarını sağlar. Öğrenci nesnelerin sorumluluklarını, invariant'larını, ilişkilerini, testlerini ve değişiklik gerekçelerini üretir. `Product`, `BomRevision`, `InventoryPosition`, `WorkOrder`, `SalesOrder`, `Money` gibi isimler başlangıç yönlendirmesidir; tasarım, koruduğu davranışla değerlendirilir.

### On çapa, on fabrika değişikliği

| Hafta | Çapa ve konu | Fabrikadaki değişiklik | Asgari kanıt |
| --- | --- | --- | --- |
| W5 | A1 — Ayrıştırma ve nesne modeli | Ürün/varyant/BOM/iş emri sorumluluklarını ayırma | Bağımsız varyant planları, ortak bileşen toplamı, güncel nesne haritası |
| W6 | A2 — Encapsulation ve invariant | Miktar, reçete ve durum değişimlerini kontrollü davranışlara taşıma | Geçersiz isteğin reddi ve ret sonrasında tutarlı durum |
| W7 | A3 — İlişkiler, UML ve sahiplik | Lot → üretim → sevk → servis izini ve fason ilişkisini kurma | İleri/geri izlenebilirlik ve doğru cardinality/sahiplik diyagramı |
| W8 | A4 — Kalıtım ve substitutability | Ortak uygunluk denetimi altında farklı davranışlar | Alt türlerin aynı sözleşmeyi koruduğunu gösteren contract testleri |
| W9 | A5 — Interface ve soyut sözleşmeler | Kanal, kargo ve belge portları; iki fake kanal | Format eşleme, bilinmeyen SKU, timeout, tekrar; ara entegrasyon |
| W10 | A6 — Polimorfizm, composition ve Strategy | Kanal/fiyat/maliyet/risk politikalarının seçimi | 1.050 TL maliyet ve 220 TL katkı; yeni politikanın eski akışı koruması |
| W11 | A7 — Kimlik, eşitlik, kopyalama ve immutability | Değer nesneleri, fiyat ve BOM snapshot'ları | R1 geçmişinin korunması, equality/hashCode uyumu, referans sızıntısı testi |
| W12 | A8 — Generics ve collections | Tür güvenli repository, sorgu ve rapor sonuçları | Tür/tekillik/sıralama davranışı ve tenant ayrımı |
| W13 | A9 — Yaşam döngüsü, exception ve kaynaklar | Kalıcılık, dosya içe aktarımı, outbox/retry bağlantısı | Save/load, bozuk girdide tutarlılık, kaynak kapatma ve tekrar anahtarı |
| W14 | A10 — Runtime metadata, class loading ve plugin | Sağlanan provider'ları keşfetme; AI belge önerisini onaya bağlama | Eksik/hatalı provider, initialization izi, onay akışı ve `v1.0` |

Kalıtım, interface veya pattern kullanımı gerçek bir sorumluluk ve davranış üzerinden gerekçelendirilir. Para, miktar, tarihsel fiyat, nesne kimliği ve hata durumları tasarımın doğruluğunu sınayan somut örneklerdir.

**Son ürün:** Sağlanan web arayüzünden kullanılan, ana fabrika akışını tamamlayan; geçmişi, iş kurallarını ve testlerini koruyan modüler Java uygulaması. Ayrıntılı görevler ve O01–O16 test kataloğu: [SE 237 ERP öğrenci rehberi](SE_237/ERP_OGRENCI_REHBERI.md).

<a id="cmpe351"></a>

## 8. CMPE 351: Factory ERP'nin veri sistemi

### Ne geliştireceğiz?

Öğrenci, aynı fabrikanın veri omurgasını kuracak. Ürün; kavramsal model, ilişkisel şema, anahtarlar, kısıtlar, migration, sabit test verisi, SQL sorguları, transaction'lar ve yeniden üretilebilir deneylerden oluşacak.

Veritabanı şu sorulara kanıtla cevap vermeli:

- 100 sandalye için hangi malzeme ne kadar gerekiyor ve hangi malzeme eksik?
- Bir sipariş veya iş emrinin hangi reçete sürümünü kullandığını koruyabiliyor muyuz?
- İki kanal aynı anda son malzemeyi ayırmaya çalışınca ne oluyor?
- Tekrar gelen üretim tamamlama olayı ikinci kez mamul kaydı oluşturuyor mu?
- Bir servis kaydı hangi üretim ve hammadde lotlarına bağlanıyor?
- Kârlılık raporundaki join'ler miktarı veya maliyeti yanlışlıkla çoğaltıyor mu?
- Başka şirketin veya bayinin verisi okunabiliyor mu?
- Yedekten döndüğümüzde aynı stok ve maliyet sonuçlarını doğrulayabiliyor muyuz?

İlk hafta küçük ihtiyaç sorgusu çalışır. W4'te temel fabrika akışı birleştirilir; sonraki sekiz haftada veri sisteminin model, doğruluk, performans ve dayanıklılık boyutları derinleşir. İnce web/API istemcisi öğretim ekibince sağlanır; öğrenme kanıtları SQL ve veritabanı davranışından elde edilir.

### Sekiz modül, sekiz veri sistemi artımı

| Hafta | Modül ve konu | Fabrikadaki çalışma | Asgari kanıt |
| --- | --- | --- | --- |
| W5 | M1 — DBMS mimarisi, iş yükü ve garantiler | Üretim, B2B ve rapor iş yükleri; bağlantı/oturum gözlemi | Session/transaction izi, sorumluluk sınırı ve ortamı belirtilmiş ölçüm |
| W6 | M2 — Kavramsal model, ilişkisel şema ve bütünlük | Lot, varyant, fason, kısmi işlem ve tenant ilişkileri | ER/EER, migration, anahtar/kısıt kararları ve negatif testler |
| W7 | M3 — İlişkisel cebir ve SQL | BOM, where-used, lot izi ve rapor sorgu portföyü | Beklenen sonuç kümeleri; recursive CTE, join/aggregate ve NULL karşı örnekleri |
| W8 | M4 — Fonksiyonel bağımlılıklar ve normalizasyon | Eski fabrika verisinin staging'den kontrollü aktarımı | FD/closure, kayıpsızlık, bağımlılık koruma ve import uzlaştırması |
| W9 | M5 — Transaction, eşzamanlılık ve recovery | Son stok yarışı; idempotent rezervasyon ve üretim | Kontrollü iki oturum deneyi, atomiklik, rollback, kilit sırası ve retry |
| W10 | M6 — Depolama, indeks ve sorgu işleme | Büyütülmüş veride stok, geciken emir ve kanal raporu | Önce/sonra sorgu planı, ham ölçüm, yazma ve depolama bedeli |
| W11 | M7 — Dağıtık/bulut sistemleri ve dayanıklılık | Backup/restore, sağlanan replica/lag deneyi, olay uzlaştırma | Restore sonrası iş toplamları, kurtarma ölçümü ve garanti karşılaştırması |
| W12 | M8 — Modern veri modelleri ve PostgreSQL genişlemeleri | Tenant/bayi RLS politikası ve seçilmiş JSONB sorgusu | Pozitif/negatif erişim testleri, model karşılaştırması ve `v1.0` |

M8 kapsamında MongoDB, Firestore/Realtime Database, Supabase ve pgvector aynı iş ihtiyacına göre karşılaştırılır. Öğrenci her platformu sıfırdan kurmaz; sağlanan örnekler ve karar matrisi kullanılır. Zorunlu artım ERP rehberindeki sınırlı RLS/JSONB davranışıdır. M7–M8'in genişliği, temel model/SQL/transaction konularını yüzeysel bırakmaya gerekçe olamaz.

**Son ürün:** Boş test veritabanından kurulabilen; ana fabrika akışını, eşzamanlı rezervasyonu, tarihsel veriyi, raporları, erişim sınırlarını ve kurtarmayı kanıtlayan veri sistemi. Ayrıntılar: [CMPE 351 ERP öğrenci rehberi](CMPE_351/ERP_OGRENCI_REHBERI.md).

<a id="haftalik-duzen"></a>

## 9. Haftalık öğrenme ve teslim düzeni

### Ortak öğrenme döngüsü

> **Haritala → Geri çağır → Modelle → İnşa et → Test et → Açıkla → Yansıt**

| Adım | Öğrencinin yaptığı iş | Örnek çıktı |
| --- | --- | --- |
| Haritala | Güncel konuyu sistemin bütünü ve diğer kavramlarla ilişkilendirir. | Çapa/modül etiketli harita |
| Geri çağır | Önceki bir mekanizmayı kısa süreyle notlar ve AI kapalıyken hatırlar. | Kısa tahmin veya açıklama |
| Modelle | Akışı, durumu veya kuralı görünür hâle getirir. | UML, ER, trace, durum tablosu, sorgu tahmini |
| İnşa et | Mevcut sürüme sınırlı değişikliği uygular. | Kod, migration, sorgu veya deney farkı |
| Test et | Yeni iddiayı ve korunacak eski davranışları sınar. | Yeni test, negatif test, regresyon veya ölçüm |
| Açıkla | Mekanizmayı ve kararı kanıt üzerinden savunur. | Teknik gerekçe, video veya sözlü yanıt |
| Yansıt | Hatasını, öğrendiğini ve sonraki bağlantıyı kaydeder. | Harita farkı, changelog ve güncel backlog |

Yeni bir mekanizmada önce tahmin yapılır, sonra sistem çalıştırılır. Tahminle gerçek gözlem karşılaştırılır; öğrenci farkın nedenini açıklar ve benzer bir yeni duruma uygular. Öğretim ekibi bu döngüyü soru ve deney tasarımında da kullanır.

### Ders öncesi

CEN 302 ve SE 237'de hazırlık paketi en az beş gün önce yayımlanır; yanıtlar ders başlamadan 12 saat önce teslim edilir. Normal paket şunları içerir:

- 20–35 dakikalık Türkçe kavram videosu ve ölçülen içeriğin eşdeğer İngilizce notu/transcript'i;
- seçilmiş kısa okuma ve çalıştırılabilir küçük örnek;
- toplam 4–6 soru: sistem haritası, eski kavramı geri çağırma, güncel mekanizma ve değişen duruma transfer.

CMPE 351'de de kısa video/okuma, ortam kontrolü ve aynı soru türleri kullanılır; teslim zamanı kendi haftalık paketinde açıkça yayımlanır. Hazırlık çıktısı öğrencinin bir tahmin veya çözüm denemesiyle derse gelmesini sağlar.

### Ders içi

**Ders oturumu 155 dakikadır: 125 dakika etkin çalışma + üç adet 10 dakikalık ara.** İlk iki blok 30'ar, üçüncü blok 30, son blok 35 dakikadır. Konu anlatımı, rehberli örnek ve uygulama aşağıdaki süreyi paylaşır.

| Dakika | Çalışma |
| --- | --- |
| 00–30 | Geri çağırma, haftanın konu grubunun ilk bölümü ve örnek |
| 30–40 | Ara |
| 40–70 | Konu grubunun devamı, tahmin ve karşı örnek |
| 70–80 | Ara |
| 80–95 | Konu grubunun kalan kısmı veya rehberli çözüm |
| 95–110 | Studio: bireysel ilk deneme ve küçük uygulama |
| 110–120 | Ara |
| 120–150 | Studio: test, düzeltme, kısa bireysel kontrol |
| 150–155 | Çıkış kaydı ve sonraki haftaya bağlantı |

Bu akış 75 dakika konu/rehberli örnek, 45 dakika studio ve 5 dakika çıkış kaydı sağlar. Eski akıştaki süre farkı ek ev ödevine aktarılmaz: kurulum ve yardımcı altyapı önceden hazır verilir, tekrarlayan gösterimler ve öğrenciye bırakılan kod miktarı azaltılır. Haftalık dosyalar aynı toplamı koruyarak blok içi dağılımı değiştirebilir.

CMPE 351'in mevcut ayrı 120 dakikalık laboratuvarı bu 155 dakikanın içine gizlenmez; ayrı tahsis olarak belirtilir ve dersle aynı işi yeniden teslim ettirmez. CMPE'nin hafta/laboratuvar düzeni ayrıca kesinleştirilirse iş yükü tablosu ona göre güncellenir.

Studio kısa bir **bireysel ve AI'sız ilk deneme** ile başlar. Ardından görevin izin verdiği rehberlik, eşli tartışma ve araç kullanımı devreye girer. Katılımın kanıtı öğrencinin kendi trace'i, sorgusu, kod değişikliği, test sonucu veya açıklamasıdır.

### Ders sonrası: tek haftalık paket

Öğrenci aynı ürün üzerinde yaptığı çalışmayı tek paket hâlinde teslim eder. Raporun ana metni yaklaşık 1–2 sayfayı hedefler; diyagram, test çıktısı ve ham ölçümler eklerde tutulabilir.

```text
Ders / hafta:
Ana çapa veya modül:
Yeniden ziyaret edilen en az iki çapa veya modül:
Başlangıç sürümü (baseline / commit / migration):
Bu haftanın artımı — tek cümle:
Çalıştırma ve doğrulama komutları:
Beklenen sonuç / gözlenen sonuç:
Sınır veya hata durumu:
Korunan önceki davranışlar ve regresyon kanıtı:
Harita / UML / ER / işlem sınırındaki değişiklik:
Tasarım kararı ve bilinen sınırlama:
CHANGELOG ve bir sonraki haftaya bağlantı:
Sağlanan altyapı, dış kaynak ve AI katkısı:
```

CEN'de kod ve deney izleri; SE'de kod, nesne sözleşmeleri ve testler; CMPE'de DDL/SQL, migration, seed ve veritabanı deneyleri bu paketin teknik çekirdeğidir. Belgedeki bir iddia ilgili dosya, komut veya sonuçla ilişkilendirilir.

### Açıklama videoları

CEN 302 ve SE 237'de her hafta 2–4 dakikalık tek çekim açıklama videosu planlanır. W1 panoraması için 60–90 saniye yeterlidir; ikinci video istenmez. Video şu dört noktayı gösterir:

1. Problem ve elde edilen sonuç.
2. Üründeki bir teknik karar.
3. Hata, sınırlama veya karşı örnek.
4. Küçük bir değişikliğin beklenen etkisi.

Planlama, kayıt ve yükleme dâhil haftalık hedef 30 dakikadır. Kamera ve kurgu zorunlu değildir; teknik açıklama değerlendirilir. Video bileşeninde en iyi 12 geçerli teslimin tamamlanması ve dönemin iki yarısından rastgele seçilen iki videonun içerik kalitesi kullanılır; alt puan dağılımı rubrikte yayımlanır.

CMPE 351'de kısa açıklama ve bireysel savunma bulunur; CEN/SE'nin 14 haftalık video puanlama kuralı bu derse otomatik aktarılmaz. Erişilebilir anlatımlı slayt, canlı açıklama veya LMS üzerinden özel teslim gibi eşdeğer yollar sağlanır.

<a id="kabul"></a>

## 10. Sürüm kapıları ve kabul ölçütleri

### Ortak tamamlanma tanımı

Bir artım veya sürüm, şu kanıtlar birlikte bulunduğunda tamamlanmış kabul edilir:

- Kesin başlangıç sürümü, ortam ve gerekli komutlar belirtilmiştir.
- Yeni davranış anlamlı bir senaryoda çalışır; beklenen sonuç açıkça tanımlıdır.
- En az bir ilgili sınır/hata durumu sınanmıştır.
- Gerekli önceki davranışlar için regresyon kanıtı vardır.
- Kod/SQL/deney ile sistem haritası birbiriyle uyumludur.
- Teknik karar, sınırlamalar ve dış katkılar açıklanmıştır.
- Öğrenci kendi çalışmasını izleyebilir ve küçük bir değişikliğin etkisini savunabilir.

Testin veya senaryonun gerçekten çalıştırılıp çalıştırılmadığı açıkça belirtilir. Gelecek özellikler backlog'da, sağlanan altyapı kendi katkı kaydında tutulur.

### Derslere göre kapılar

| Kapı | CEN 302 | SE 237 | CMPE 351 |
| --- | --- | --- | --- |
| **W1 prototip** | Byte Counter ve temel hata/çıktı testleri | Yan etkisiz 100 sandalye malzeme planı | Aynı veriyle ihtiyaç/eksik sorgusu ve temel bütünlük reddi |
| **W4 `v0.1`** | Host launcher, sonlanma/cleanup, guest smoke testi | Sağlanan iskelette satınalma–üretim–60+40 sevk akışı | Migration/seed ile aynı fabrika akışı, hareket/rezervasyon uzlaştırması |
| **Ara kontrol** | Haftalık deney ve kapsam haritası | W9 bütünleşik web/domain/connector sürümü | W8 model, normalizasyon ve import kontrolü |
| **Son sürüm** | W14 `v1.0` | W14 `v1.0` | W12 `v1.0` |

W4'te tüm ileri özellikler tamamlanmış sayılmaz. Özellikle ERP'de çok kullanıcılı doğruluk, gelişmiş yetki, maliyet ve dış servis davranışlarının hangi kısmının hazır olduğu açıkça kaydedilir. Sonraki artımlar bu sınırları genişletir.

### Son demo ne göstermeli?

**CEN 302:** Bir işin başlatılmasından süreç/thread, IPC, bellek, dosya ve I/O gözlemlerine uzanan yol; Linux/xv6/model ayrımı; kaynak ve öğrenci katkısı; birikimli test ve kapsam kataloğu.

**SE 237:** Tarayıcı üzerinden 100 sandalye planı, eksik malzemenin kabulü, üretim, 60+40 sevk ve maliyet; bayi sınırı, servis/lot ilişkisi, tekrar gelen olay ve onaya bağlı AI önerisi. Uygulamanın yeniden başlatma ve hata davranışı da gösterilir.

**CMPE 351:** Boş test DB'den kurulum, aynı ana fabrika akışı, iki oturumlu rezervasyon, R1 geçmişi, lot izi, kârlılık sorgusu, başka bayiye erişimin reddi ve restore sonrası testler. Seçilmiş modern veri davranışının gerekçesi açıklanır.

Son sürüm, dönem sonunda sıfırdan başlanan ek bir proje değildir. Haftalık artımların birlikte çalışması, regresyon, mimari tutarlılık ve bireysel anlayış değerlendirilir.

<a id="degerlendirme"></a>

## 11. Ölçme ve değerlendirme

Notlandırma, ürünün niteliği ile öğrencinin bağımsız anlayışını birlikte görünür kılmalıdır. Açık kaynak veya AI desteğiyle hazırlanan ürün; bireysel geri çağırma, sözlü açıklama, quiz ve sınav kanıtlarıyla tamamlanır.

**Aşağıdaki ders ağırlıkları planlama taslağıdır.** Uygulanacak dağılım, resmî izlence ve ilk değerlendirme öncesinde yayımlanacak koşullarla kesinleşir. Proje içi rubrikler, dersin toplam notuna katkı yüzdesinden ayrıdır.

### CEN 302 ve SE 237 için planlanan ders ağırlıkları

| Bileşen | CEN 302 | SE 237 |
| --- | ---: | ---: |
| Gelişen dönem ürünü | %25 | %35 |
| Kısa analitik ödevler | %15 | — |
| Hazırlık | %5 | %5 |
| İki bireysel sözlü savunma | %5 | %5 |
| Sınıf içi studio | %10 | %10 |
| Haftalık açıklama videoları | %5 | %5 |
| Kısa bireysel quiz/checkpoint | — | %5 |
| Vize | %15 | %15 |
| Final | %20 | %20 |
| **Toplam** | **%100** | **%100** |

Her iki derste dönem içi bileşenler %65, bireysel sınavlar %35'tir. CEN ürün bileşeni W4 ve on artımı; SE ürün bileşeni W4, on artım ve W9/W14 entegrasyonunu kapsar. Bu bileşenlerin kendi içindeki puan dağılımı ayrıca yayımlanır.

### CMPE 351 için 12 haftalık sürekli değerlendirme taslağı

| Bileşen | Sayı ve uygulama | Katkı |
| --- | --- | ---: |
| Yazılı öğrenme kontrolleri | W1 puansız tanılama; W2–W12 arasındaki 11 kaydın en düşük ikisi çıkarılır, 9 kayıt kullanılır | %30 |
| Bireysel sözlü açıklamalar | Döneme dengeli yayılmış en az 3 kontrol | %10 |
| Ürün geliştirme paketleri | W2–W11 arasındaki 10 paketin en düşük biri çıkarılır, 9 paket kullanılır | %50 |
| Son sürüm ve mimari savunma | W12 `v1.0` ve bireysel savunma | %10 |
| **Toplam** | | **%100** |

Bu taslak ayrıca vize/final yapılmaması varsayımına dayanır. Kurum ayrı sınav öngörürse ağırlıklar ilk değerlendirmeden önce yeniden yayımlanır. On iki haftalık derse eski on dört haftalık teslim sayıları taşınmaz.

### ERP projelerinin kendi içindeki kalite rubriği

| SE 237 ölçütü | Proje içi pay | CMPE 351 ölçütü | Proje içi pay |
| --- | ---: | --- | ---: |
| İş kuralları ve fabrika zincirinin doğruluğu | %30 | Model, anahtar, migration ve bütünlük | %25 |
| Nesne modeli, sözleşmeler ve değiştirilebilirlik | %25 | SQL/cebir, BOM ve rapor doğruluğu | %20 |
| Test, hata yolu ve regresyon | %20 | Transaction, idempotency ve eşzamanlılık | %25 |
| Entegrasyon, yeniden üretim ve kalıcılık | %15 | Performans, erişim kontrolü ve kurtarma | %20 |
| Bireysel açıklama ve teknik belge | %10 | Bireysel açıklama ve yeniden üretilebilirlik | %10 |
| **Toplam** | **%100** | **Toplam** | **%100** |

CEN'de ürün kapıları çekirdek davranış, mekanizma/tasarım, hata yönetimi, test/yeniden üretim ve açıklama kanıtlarıyla değerlendirilir; sayısal alt rubrik görev paketinde belirlenir.

### Aynı ölçütü iki kez puanlamama

Bir teslim farklı beceriler için kanıt sağlayabilir. Ürün puanı doğruluk ve tasarımı; studio bireysel ilk deneme ve düzeltmeyi; video teknik anlatımı; sözlü bağımsız anlayışı ölçer. Entegrasyonda tek tek eski özelliklerin yerel doğruluğu yeniden puanlanmaz; birlikte çalışma ve değişiklik etkisi değerlendirilir.

Build hatası, okunabilir bir doğru akıl yürütme veya test tasarımının puanını otomatik silmez. Değerlendirici kanıtı ilgili ölçüte göre inceler. Otomatik testler insan değerlendirmesini yönlendirir; notu tek başına belirlemez.

### Sözlü savunma ve sınavlar

CEN ve SE'de her öğrenci W1–W7 arasında bir, W8–W14 arasında bir kısa puanlanan sözlü savunmaya girer. CMPE'de en az üç bireysel kontrol 12 haftaya dengeli dağıtılır. Öğrenci kendi teslimindeki bir kararı veya izi açıklar; ardından küçük bir değişikliğin sonucunu tahmin eder ya da uygular.

Teslimle açıklama arasında belirgin uyumsuzluk varsa tartışmalı ölçütler için odaklı yeniden savunma yapılır. Tek bir zayıf yanıt bütün ürünü otomatik olarak sıfırlamaz. CEN/SE planında her öğrenci geri bildirimden sonra bir eşdeğer telafi savunması isteyebilir. Teknik anlayış değerlendirilir; aksan, konuşma hızı veya sunum özgüveni puan ölçütü değildir.

CEN/SE vize ve finalinin önerilen soru omurgası yaklaşık %25 kavram/kavram yanılgısı, %35 trace ve analiz, %30 onarım/tasarım, %10 gerekçeli tercih açıklamasıdır. Final birikimlidir. Sınavdaki görev türleri dönem içinde çalışılmış olur; öğrenciden yeni durumda aynı düşünme becerisini kullanması beklenir.

<a id="ai"></a>

## 12. Yapay zekâ, kaynak kullanımı ve bireysel sorumluluk

> **Açık öğrenme görevlerinde AI kullanılabilir. Öğrenci teslim ettiği önemli her kısmı açıklayabilmeli, izleyebilmeli, sınayabilmeli, değiştirebilmeli ve savunabilmelidir.**

AI kullanımı isteğe bağlıdır; ücretli araç zorunlu değildir. Hazırlık ve izin verilen ev/lab çalışmalarında açıklama, alternatif tasarım, kod/SQL taslağı, hata ayıklama veya test fikri için kullanılabilir. Görev paketi izin verilen modu açıkça belirtir.

Vize, final, puanlanan quiz ve sözlü savunmalar bireysel ve AI'sızdır. Studio da kısa bireysel AI'sız denemeyle başlar; sonraki bölümün araç kullanımı yönergede açıklanır.

Her pakette kısa bir katkı kaydı bulunur:

```text
AI Assistance
Araç/model veya “AI kullanılmadı”:
Kullanım amacı ve etkilenen dosya/bölümler:
Önemli prompt ya da kısa etkileşim özeti:
Kabul ettiğim, değiştirdiğim veya reddettiğim öneri ve nedeni:
Sonucu doğruladığım test, sorgu, trace veya ölçüm:
Diğer insan katkıları, sağlanan altyapı ve dış kaynaklar:
```

Öğrenci üretilen kodu/SQL'i ilgili ortamda çalıştırır ve sonucu doğrular. Kaynak, log, ölçüm, deney sonucu veya yazarlık bilgisi uydurulamaz. Başka öğrencilerin çalışmaları, kişisel veriler, erişim anahtarları ve yayımlanmamış sınav soruları dış araçlara yüklenmez. Fabrika örneklerinde dersin sentetik verileri kullanılır.

ERP içindeki AI özelliği ile öğrencinin geliştirme sırasında AI kullanımı ayrı konulardır. ERP'deki belge okuyucu veya öneri servisi kaynak, güven ve insan onayıyla modellenen bir ürün davranışıdır; öğrencinin teslimindeki AI katkısı ise yukarıdaki kayıtla açıklanır.

Ekip çalışması verilirse her öğrenci kendi katkısını ve temel kararlarını savunur. İş bölümü, bir öğrencinin bütün bir kavram grubuyla hiç çalışmamasına yol açacak biçimde yapılmaz.

<a id="destek"></a>

## 13. Destek, erişilebilirlik ve iş yükü

### Destek kademeli azalır

| Aşama | Öğretim ekibinin desteği | Öğrencinin artan sorumluluğu |
| --- | --- | --- |
| W1 | Referans gösterimi, küçük prototip iskeleti ve test verisi | Temel davranışı tamamlama, çalıştırma ve açıklama |
| W2 | Yapı, arayüzler, adaptörler ve smoke test | Sınırları bağlama, haritalama ve karar gerekçesi |
| W3 | Örnek akış, hata fixture'ları ve işlem/deney iskeleti | Dikey dilim, durum analizi ve test |
| W4 | Açık kabul sözleşmesi ve geri bildirim | Çalışan sürüm, entegrasyon ve bireysel savunma |
| Derinleşme | Sınırlı görev, gerekli altyapı ve hedefli geri bildirim | Tasarım, uygulama, karşı örnek ve kanıt üretme |

### Kurtarma tabanı

İlk teknik hata sonraki haftalara katılımı engellememelidir. W4 sonrasında öğretim ekibi test edilmiş `v0.1` tabanı sağlar. Gerekirse CEN/SE'de W9, CMPE'de W8 sonrasında ikinci kontrollü taban yayımlanabilir.

Bu tabanı kullanan öğrenci sürümü `BASELINE.md` içinde belirtir, kendi çözümündeki eksikliği ve öğrendiğini kısa bir fark analiziyle açıklar. Sonraki artımlarda normal değerlendirilir. Hazır tabanı almak, önceki eksik özelliklerin puanını kendiliğinden kazandırmaz.

### Haftalık kapsam ve süre

Her görev üç şerit taşır:

- **Core:** Herkesin tamamlaması beklenen, sınırları ve kabul testi belli zorunlu iş.
- **Stretch:** İsteğe bağlı genişletme; eksik Core çalışmasının yerine geçmez.
- **Instructor demo:** Eğitmenin gösterdiği ileri mekanizma; öğrenci uygulaması zorunlu değildir.

CEN/SE'de normal haftalık Core artımının kod, test, kısa gerekçe ve katkı açıklamasıyla yaklaşık üç saat ders dışı çalışmaya sığması hedeflenir. Bu süre tüm haftalık ders yükü değildir; hazırlık, video ve sınava çalışma ayrıca toplam iş yükünde yer alır. W3, W6 ve W10'da anonim süre yoklamalarıyla yük izlenir; aşım görüldüğünde sonraki hafta da kontrol edilir. Tipik Core süresi iki hafta üst üste hedefi aşıyorsa görev küçültülür veya destek artırılır.

CEN/SE için 14 oturumun tahsis toplamı 2.170 dakika (36 saat 10 dakika), aralar dışındaki etkin süre 1.750 dakika (29 saat 10 dakika) olur. 5 AKTS başına yaklaşık 140 saatlik dönem toplamı ayrı bir planlama hedefidir; resmî izlenceyle doğrulanır. CMPE 351'de 12 ders oturumu 1.860 dakika tahsis, 1.500 dakika etkin çalışma içerir. Ayrı laboratuvar korunursa 12 × 120 dakika tahsis ve mevcut 110 etkin dakika varsayımıyla 1.320 dakika laboratuvar çalışması ayrıca sayılır. 6 AKTS toplamı, gerçek temas saati ve sınav düzeniyle doğrulanır. On dört haftalık eski tablo bu derse doğrudan kopyalanmaz. Uygulama, rapor, video ve proje süreleri mükerrer sayılmaz; son haftaya gizli büyük proje yükü bırakılmaz.

### Dil ve erişim

Teknik materyaller, değerlendirme yönergeleri ve sınavlar ilgili İngilizce izlenceyle uyumlu hazırlanır. Türkçe videolar kavramsal erişimi destekler; ölçülen içerik için eşdeğer İngilizce metin bulunur. Bu Türkçe planlama belgesi, resmî değerlendirme dilini değiştirmez.

Ücretli kurs, bulut hesabı, AI aboneliği veya özel donanım zorunlu tutulmaz. Yerel/test ortamları, emülatörler, sağlanan trace'ler ve adaptörler kullanılır. Video ve sözlü açıklama için eşdeğer erişilebilir teslim yolları sunulur.

<a id="hazirlik"></a>

## 14. Öğretim ekibinin hazırlık ve yürütme planı

### Üretilecek materyaller

Bu tasarımın uygulanabilmesi için aşağıdaki paketlerin hazırlanması gerekir. Liste tamamlanmış işler olarak okunmamalıdır.

| İş paketi | Ortaya çıkacak çıktı | Hazır sayılması için kanıt |
| --- | --- | --- |
| Güncel belgelerin uyumu | SE/CMPE eski örneklerinin ERP'ye uyarlanması; doğru bağlantı ve hafta etiketleri | Ana README, rehber, ilk hafta ve görev paketlerinin aynı kapsamı anlatması |
| Ortam ve sürüm sabitleme | CEN için Linux/QEMU/RISC-V; SE için Java/Maven/JUnit; CMPE için PostgreSQL ortamı | Temiz kurulumdan çalıştırma, sürüm kaydı ve desteklenen makine yönergeleri |
| W1 starter'ları | Byte Counter; Java malzeme planlayıcı; SQL ihtiyaç prototipi | Başarı/sınır/hata testleri ve ders süresine uygun öğrenci değişikliği |
| Panorama referansları | Hedef sistemin uçtan uca gösterimi ve kavram haritası | Her çapa/modülün gösterimdeki yerinin ve hazır olmayan kısmın belirtilmesi |
| W2–W4 iskeleleri | Çalışan yapı, dikey dilim ve entegrasyon tabanı | Temiz kopyadan `v0.1` kabul senaryosu |
| Ortak ERP fixture'ları | R1, stok, fiyat, sipariş ve olay örnekleri | Java ve SQL örneklerinin aynı sayısal kabul sonuçlarını üretmesi |
| Ders altyapıları | Web/rol/adaptör; xv6 deney kancaları; veri üretici ve iki oturum düzeneği | Ders kavramının dışında kalan altyapının öğretim ekibince doğrulanması |
| Haftalık paketler | Core/Stretch/demo, soru, başlangıç sürümü, test ve rubrik | Görev süresi denemesi ve önceki sürümle regresyon kontrolü |
| Ölçme araçları | Quiz, sözlü soru havuzu, video rubriği ve örnek yanıtlar | Değerlendiricilerin örnek çalışmalar üzerinde kalibrasyonu |
| Kurtarma ve son sürüm | `v0.1` kurtarma tabanı, gerekirse ara taban, final demo planı | Temiz kurulum, sürüm/katkı kaydı ve birikimli kabul kanıtı |

Önerilen üretim sırası: **belgeleri uyumlu hâle getirme → ortamı sabitleme → W1 starter ve testleri → panorama referansı → W2–W4 tabanı → derinleşme paketleri → ölçme kalibrasyonu**. Öğrenciye yayımlanan her paketin çalıştırıldığı sürüm ve bilinen sınırları kayıtlı olmalıdır.

### Yaklaşık 80 kişilik sınıfta yürütme

CEN/SE uygulama planında 95–110 ve 120–150 aralıklarındaki toplam 45 etkin studio dakikasında eğitmenle birlikte iki asistan görev alır. Üç değerlendirici ortak kısa soru havuzu, rubrik ve kapsama kaydı kullanır. Sözlü seçim o dönem yarısında en az kontrol edilmiş öğrencilerden yapılır; değerlendirmenin sınıfa dengeli yayılması izlenir.

Eğitmen haftalık asistan kayıtlarından örneklem kontrol eder ve nihai not kararını verir. Otomatik testler sorunlu teslimleri görünür kılar; kavramsal açıklama ve tasarım kararları insan incelemesiyle değerlendirilir. CMPE'de aynı izlenebilirlik ilkesi, kendi öğrenci sayısı ve uygulama saatine göre planlanır.

### Dönem boyunca tutulacak kayıtlar

- Her öğrenci için hazırlık, ürün sürümü, geri bildirim, sözlü kapsam ve sonraki kontrol ihtiyacı.
- Her hafta için sık görülen kavram yanılgıları, başarısız test türleri ve görev süresi.
- Her paket için başlangıç sürümü, değişiklik kaydı, kullanılan kaynaklar ve bilinen sınırlamalar.
- Her çapa/modül için nerede anlatıldığı, uygulandığı ve bireysel olarak doğrulandığı.

Bu kayıtlar, sonraki haftanın kapsamını ve desteğini ayarlamak için kullanılır. Kaynakçalar geniş başvuru havuzudur; haftalık pakette öğrencinin gerçekten kullanacağı bölüm ve bunun hangi görevle ilişkili olduğu ayrıca seçilir.

<a id="basari"></a>

## 15. Dönem sonunda başarıyı nasıl anlayacağız?

Başarıyı üç ayrı aşamada gözleyeceğiz:

| Zaman | Öğrencide görmek istediğimiz durum |
| --- | --- |
| **W1 sonunda** | Küçük prototipi çalıştırır, temel sonucu ve hata yolunu açıklar; dersin genel haritasını tanır. |
| **W4 sonunda** | Küçük bütünleşik ürünü temiz kopyadan kurar, uçtan uca izler, test eder ve ilk mimari kararlarını savunur. |
| **CEN/SE W14; CMPE W12 sonunda** | Aynı ürüne yeni bir değişiklik yapar; mekanizmayı, etkilediği parçaları, hata koşullarını ve doğruluk kanıtını bağımsız açıklar. |

CEN öğrencisi bir programın işletim sistemi içindeki yolunu gerçek kaynak ve deneylerle ilişkilendirebiliyorsa; SE öğrencisi fabrika davranışını tutarlı ve değiştirilebilir nesne sözleşmeleriyle kurabiliyorsa; CMPE öğrencisi aynı işin verisini modelleyip eşzamanlılık, performans, yetki ve kurtarma kararlarını kanıtlayabiliyorsa derslerin ayrı hedeflerine ulaşılmış olur.

Ortak başarı, öğrencinin dönem başındaki haritasıyla son sürümünü karşılaştırıp **hangi kavramı nasıl daha iyi anladığını, hangi hatayı hangi kanıtla düzelttiğini ve sistemin neden bu şekilde evrildiğini** anlatabilmesidir. Depodaki bütün materyaller bu ilerlemeyi mümkün kılmak ve görünür hâle getirmek için hazırlanacaktır.
