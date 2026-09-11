# CMPE 351 Database Systems — 12 Haftalık Ders İşleme Sistemi

**Üniversite:** İstanbul Bilgi Üniversitesi  
**Ders:** CMPE 351 Database Systems  
**Dönem:** 2026–2027 Güz  
**Ders süresi:** 12 öğretim haftası  
**Ders modeli:** `1 + 3 + 8` spiral  
**Ders dili:** İngilizce  
**Teknik omurga:** PostgreSQL ve SQL öncelikli Factory ERP veri ürünü

## İlgili belgeler

**Güncel öğrenci belgesi:** [Sandalye Fabrikası ERP — CMPE 351 öğrenci rehberi](ERP_CMPE351_OGRENCI_REHBERI.md). Bu README'deki `1 + 3 + 8` ders modeli korunur. Aşağıdaki metin ve önceki proje/ilk hafta planlarında kalan Campus Learning Hub örnekleri önceki senaryoya aittir; güncel iş akışları, haftalık proje teslimleri ve kabul koşullarında ERP rehberi esas alınır.

[Kaynakça](KAYNAKCA.md) · [İlk hafta öğretmen akışı](HAFTA01.md) · [Prototip ve 12 haftalık proje planı](PROJE.md)

Bunlar planlama belgeleridir; adı geçen yeni starter, referans uygulama ve testler ayrıca hazırlanıp çalıştırılarak doğrulanacaktır.

## Bu belgenin amacı

Bu README, CMPE 351 dersinin 12 haftalık yeni işleniş mantığını tanımlar. Önceki tasarım 14 hafta ve on derinleşme modülü üzerine kurulmuştu. İstanbul Bilgi Üniversitesinde bu dönem öğretimin 12 hafta sürmesi nedeniyle sistem **bir panorama haftası, üç rehberli inşa haftası ve sekiz derinleşme modülü** olacak biçimde yeniden düzenlenmiştir.

Bu değişiklik yalnızca takvimde iki haftayı silmek değildir. Birbiriyle doğal olarak devam eden bazı başlıklar tek bir öğrenme modülünde birleştirilmiş; temel veritabanı kazanımlarının, çalışan ürünün ve kanıt temelli değerlendirme yaklaşımının korunması amaçlanmıştır.

Temel ilke şöyledir:

> **Öğrenci önce veri sisteminin tamamını görür, aynı sistemi üç farklı mercekle yeniden kurar ve ardından sekiz modülde aynı veri ürününü derinleştirir.**

## Neden spiral bir sistem kullanıyoruz?

Veritabanı konuları birbirinden bağımsız değildir. ER modeli, ilişkisel şema, SQL, normalizasyon, transaction, indeks, dağıtık sistem ve modern veri platformları aynı tasarım zincirinin farklı noktalarıdır. Doğrusal anlatımda öğrenci bu bağlantıları çoğu zaman dönem sonunda kurmaya çalışır.

Spiral sistemde ise öğrenci ilk haftadan itibaren şu bütünü görür:

```text
Gereksinim
   ↓
Kavramsal model
   ↓
İlişkisel şema ve kısıtlar
   ↓
Sorgular ve transaction'lar
   ↓
Depolama, indeks ve sorgu planı
   ↓
Dağıtım, dayanıklılık ve güvenlik
   ↓
Doküman, realtime ve vektör arama kararları
   ↓
Çalışan, ölçülen ve savunulan veri ürünü
```

Her yeni ayrıntı bu zincirdeki yerine yerleştirilir. Amaç yalnızca SQL yazabilen değil, belirli bir iş yükü için neden o modeli, kısıtı, transaction sınırını, indeksi veya veri platformunu seçtiğini kanıtla açıklayabilen öğrenci yetiştirmektir.

## Dönemin ana kurgusu: `1 + 3 + 8`

| Aşama | Haftalar | Amaç | Ürün sonucu |
| --- | ---: | --- | --- |
| **Panorama** | 1 | Sekiz modülün tamamını tek bir uçtan uca veri hikâyesinde görmek | Referans ürün, sistem haritası ve başlangıç tanılaması |
| **Rehberli inşa** | 2–4 | Bütün modülleri yapı, davranış/hata ve entegrasyon açısından üç kez yeniden dolaşmak | Walking skeleton → vertical slice → çalışan `v0.1` |
| **Bilinçli derinleşme** | 5–12 | Her hafta bir modülü ayrıntılı işlemek ve aynı ürüne kanıtlı bir artım eklemek | Sekiz modül, sekiz ürün artımı ve bütünleşik `v1.0` |

Buradaki kritik ayrım şudur: **2., 3. ve 4. haftalarda sekiz modül üç haftaya bölünmez.** Her haftada sekiz modülün tamamı aynı Campus Learning Hub hikâyesi içinde yeniden görülür. Değişen şey ele alınan soru ve ayrıntı düzeyidir:

- **1. hafta:** Bir veri sistemi hangi problemleri çözer; hangi parçalar ve seçenekler vardır?
- **2. hafta:** Verinin, bileşenlerin, sorumlulukların ve güven sınırlarının yapısı nedir?
- **3. hafta:** Sorgular, kısıtlar ve transaction'lar nasıl davranır; sistem nasıl hata verir?
- **4. hafta:** Model, şema, sorgular, testler ve belgeler nasıl çalışan bir veri ürününde birleşir?
- **5–12. haftalar:** Her modül hangi mekanizmaları içerir ve ürünü nasıl daha doğru, hızlı, güvenli veya uygun hâle getirir?

## Dönem ürünü: Campus Learning Hub

Ders boyunca birbirinden kopuk haftalık veritabanları hazırlanmaz. Her öğrenci aynı temel problem alanını kullanan tek bir veri ürününü aşamalı olarak geliştirir.

**Campus Learning Hub** şu temel hikâyeyi taşır:

- Öğrenci dersleri kod veya anahtar kelimeyle arar.
- Uygun dönemde bir derse kayıt olur ve kayıtlarını görüntüler.
- Öğretim elemanı ders kataloğunu ve kontenjanı yönetir.
- Sistem aynı anda gelen kayıt isteklerinde kontenjanı aşmaz.
- Erişim kullanıcı rolü ve veri sahipliğine göre sınırlandırılır.
- Ders içeriği anahtar kelime veya anlam benzerliğiyle aranabilir.

İlk dört haftada ürün PostgreSQL, `psql` ve sade bir komut satırı giriş noktasıyla çalışır. Web arayüzü zorunlu değildir; öğrencinin enerjisi veri modeli ve veri sistemi davranışına ayrılır.

Dönem boyunca ürün şu sorularla büyür:

- Model, gerçek dünyadaki ders ve kayıt kurallarını doğru temsil ediyor mu?
- Veritabanı geçersiz durumu uygulama kodundan bağımsız olarak engelliyor mu?
- Bilgi ihtiyacı doğru ve anlaşılır SQL'e dönüşüyor mu?
- Şema güncelleme, ekleme ve silme anomalileri üretiyor mu?
- İki kayıt aynı anda geldiğinde kontenjan bütünlüğü korunuyor mu?
- Sorgu neden yavaş ve seçilen indeks gerçekten işe yarıyor mu?
- Veri tek düğümün dışına çıktığında hangi garantiler değişiyor?
- Doküman, realtime, JSONB, RLS veya vektör arama hangi somut iş yükünü çözüyor?

## Sekiz sabit modül

Önceki on çapa, içerik kaybını en aza indirecek biçimde sekiz modülde yeniden düzenlenmiştir. Modül kodları `M1–M8` dönem boyunca değişmez; hazırlık, sınıf çalışması, proje, quiz, sözlü ve mimari harita aynı kodları kullanır.

| Modül | Başlık | Ana soru | Derinleşme haftası |
| --- | --- | --- | ---: |
| **M1** | Veri sistemleri, DBMS mimarisi, iş yükü ve garantiler | Hangi iş yükü hangi veri sistemi garantilerine ihtiyaç duyar? | 5 |
| **M2** | Kavramsal model, ilişkisel şema ve bütünlük | Gerçek dünya anlamı nasıl doğru ve çalışabilir bir şemaya dönüştürülür? | 6 |
| **M3** | İlişkisel cebir ve SQL | Bir bilgi ihtiyacı cebirsel olarak nasıl ifade edilir ve doğrulanmış SQL'e çevrilir? | 7 |
| **M4** | Fonksiyonel bağımlılıklar ve normalizasyon | Şema hangi bağımlılıklar yüzünden anomali üretir ve nasıl düzeltilir? | 8 |
| **M5** | Transaction, eşzamanlılık ve kurtarma | Aynı veriye eşzamanlı erişimde doğruluk ve geri kazanılabilirlik nasıl korunur? | 9 |
| **M6** | Depolama, indeksler ve sorgu işleme | Sorgular neden yavaşlar ve hangi fiziksel yapı ölçülebilir iyileştirme sağlar? | 10 |
| **M7** | Dağıtık, bulut ve dayanıklı veri sistemleri | Veri tek düğüm dışına çıktığında hangi garanti, gecikme ve işletim bedelleri ortaya çıkar? | 11 |
| **M8** | Doküman/realtime modeller ve modern PostgreSQL platformu | İlişkisel çekirdek ne zaman JSONB, RLS, realtime veya vektör aramayla genişletilmeli; ne zaman farklı model seçilmeli? | 12 |

### On başlıktan sekiz modüle geçiş

İki birleşim özellikle bilinçli yapılmıştır:

1. **Kavramsal modelleme ile ilişkisel şema/bütünlük M2 içinde birleşir.** Çünkü bir ER/EER kararının ilişkisel tabloya, anahtara ve kısıta nasıl dönüştüğü aynı tasarım zinciridir. Model çizimi, uygulanabilir şemadan koparılmaz.
2. **Doküman/realtime sistemler ile modern PostgreSQL M8 içinde birleşir.** Bu teknolojiler ayrı ürün tanıtımları olarak değil, aynı erişim deseni ve iş yükü için alternatif tasarım kararları olarak karşılaştırılır.

Depolama/indeks/sorgu işleme ile dağıtık sistemler ayrı tutulmuştur. Çünkü yerel fiziksel performans ile çok düğümlü garanti ve dayanıklılık kararları, her biri bağımsız mekanizma ve kanıt gerektiren temel konulardır.

## Modüllerin ayrıntılı kapsamı

### M1 — Veri sistemleri, mimari, iş yükü ve garantiler

Veri, bilgi, veritabanı, DBMS ve veri ürünü ayrımı; dosya yaklaşımının tekrar, tutarsızlık ve eşzamanlılık sorunları; istemci/sunucu mimarisi; bağlantı, oturum ve sorgu yaşam döngüsü; OLTP, analitik, arama ve realtime iş yükleri; doğruluk, dayanıklılık, gecikme, throughput, erişilebilirlik, maliyet ve işletim yükü ele alınır.

Öğrenci “hangi ürün daha modern?” sorusundan önce “bu iş yükünün hangi garantiye ihtiyacı var?” sorusunu sorar.

### M2 — Kavramsal model, ilişkisel şema ve bütünlük

Varlık, öznitelik, tanımlayıcı, ilişki, kardinalite, katılım, zayıf varlık ve uzmanlaşma/genelleme; primary/candidate/foreign key; entity, referential ve domain integrity; doğal/surrogate anahtar; associative relation; `NOT NULL`, `UNIQUE`, `CHECK` ve `FOREIGN KEY`; güvenli schema migration ele alınır.

Öğrenci bir iş kuralını önce kavramsal modelde ifade eder, sonra ilişkisel şemaya çevirir ve son olarak negatif testlerle veritabanının bu kuralı gerçekten koruduğunu gösterir.

### M3 — İlişkisel cebir ve SQL

Selection, projection, Cartesian product, join, küme işlemleri, rename, grouping ve division fikri; bunların `SELECT`, `WHERE`, `JOIN`, `GROUP BY`, `HAVING`, alt sorgu, CTE, window function ve view ile ifadesi; `NULL` ve üç değerli mantık; sonuç doğrulama işlenir.

SQL sözdizimi ezberlenecek komutlar bütünü olarak değil, bilgi ihtiyacının doğrulanabilir dönüşümü olarak ele alınır.

### M4 — Fonksiyonel bağımlılıklar ve normalizasyon

Güncelleme, ekleme ve silme anomalileri; determinant, closure ve candidate key; tam, kısmi ve geçişli bağımlılık; 1NF, 2NF, 3NF ve BCNF; kayıpsız ayrıştırma ve bağımlılık koruma ele alınır.

Öğrenci yalnızca normal form adını söylemez; önce anomalinin oluştuğunu karşı örnekle gösterir, sonra ayrıştırmanın anlamı ve gerekli bağımlılıkları koruduğunu savunur.

### M5 — Transaction, eşzamanlılık ve kurtarma

ACID; transaction sınırı; schedule ve serializability sezgisi; lost update, dirty read, non-repeatable read ve phantom; PostgreSQL MVCC ve izolasyon düzeyleri; explicit locking, deadlock, retry; write-ahead logging ve recovery temelleri ele alınır.

PostgreSQL'de Read Uncommitted, Read Committed gibi davranır; dirty read gerçek PostgreSQL deneyi olarak vaat edilmez. Genel anomaly örnekleri ile ürünün desteklediği izolasyon davranışı ayrılır.

Ana laboratuvar senaryosu, son kontenjana iki öğrencinin aynı anda kayıt olmaya çalışmasıdır. Doğruluk tek bir başarılı çalıştırmayla değil, kontrollü eşzamanlı deney ve tekrar üretilebilir izlerle gösterilir.

### M6 — Depolama, indeksler ve sorgu işleme

Page, record, heap ve buffer kavramları; B-tree, hash, GIN, GiST, BRIN ve bileşik/partial/covering indeks seçimi; parser, planner, optimizer ve executor; selectivity, statistics ve cost; `EXPLAIN` ile `EXPLAIN ANALYZE` ele alınır.

Her performans iddiası ölçüm ister. Öğrenci yalnızca indeks eklemez; sorgu planını, veri büyüklüğünü, seçiciliği, okuma kazancını ve yazma/depolama bedelini birlikte yorumlar.

### M7 — Dağıtık, bulut ve dayanıklı veri sistemleri

Replication, partitioning ve sharding; leader/follower ve multi-leader fikirleri; quorum sezgisi; consistency modelleri; CAP ve PACELC'in tasarım bağlamı; failover, backup, point-in-time recovery ve disaster recovery; veri yerleşimi, gecikme, maliyet ve operasyon yükü ele alınır.

Öğrenci “dağıtık” kelimesini otomatik olarak “daha iyi” veya “daha ölçeklenebilir” ile eşitlemez. Hangi hata modeline karşı hangi garantiden, gecikmeden veya işletim sadeliğinden vazgeçildiğini açıklar.

### M8 — Doküman/realtime modeller ve modern PostgreSQL

MongoDB'nin document/collection modeli, embedding/referencing tercihleri; Firestore yapısı ve güvenlik kuralları; Firebase Realtime Database JSON ağacı ve listener davranışı; PostgreSQL `jsonb` ve GIN; Supabase Auth, API, Realtime, Storage ve Row Level Security; pgvector, exact/approximate nearest neighbor, semantic ve hybrid search ele alınır.

Bu geniş modül bir ürün turuna dönüşmez. Campus Learning Hub içindeki sınırlı bir içerik arama veya canlı durum iş yükü seçilir; aynı gereksinim ilişkisel, doküman ve realtime modellerde veri şekli, sorgulanabilirlik, güvenlik, tutarlılık, maliyet ve işletim yükü açısından karşılaştırılır. Uygulama artımı yalnızca seçilen bir çekirdek davranışı gerçekleştirir; diğer teknolojiler kontrollü örnek ve karar matrisiyle değerlendirilir.

## İlk dört hafta: sekiz modülün dört tam geçişi

### 1. hafta — Panorama: veri kararlarının tamamı

Eğitmen tamamlanmış referans Campus Learning Hub sürümünü uçtan uca çalıştırır. Bir öğrencinin ders araması, kayıt olması, kısıtla karşılaşması, verinin saklanması ve daha sonra aranması üzerinden `M1–M8` görünür hâle getirilir.

Öğrenci:

- PostgreSQL/Docker/`psql` ortamını doğrular,
- referans ürünü çalıştırır,
- bir başarılı sorgu ve bir reddedilen işlem gözlemler,
- sekiz modüllük dönem haritası çıkarır,
- “bildiğim / emin olmadığım / yeni” envanteri hazırlar,
- değişen bir gereksinimin hangi modülleri etkileyebileceğini tahmin eder.

İlk hafta ayrıntılı uygulama sınavı değildir. Amaç öğrencinin gideceği yeri ve veri sistemindeki temel karar zincirini görmesidir.

### 2. hafta — Parçalar, veri sınırları ve walking skeleton

Aynı ürün; istemci, veritabanı, şema, migration, veri sahipliği, güven sınırı, sorgu giriş noktası ve gelecekteki teknoloji sınırları üzerinden yeniden incelenir.

Öğrenci:

- PostgreSQL'i ayağa kaldıran yürüyen bir iskelet kurar,
- sağlık kontrolünü ve temiz kurulum yolunu doğrular,
- ilk bağlam diyagramı ve ER taslağını hazırlar,
- temel tablo/kısıt yerlerini belirler,
- `M1–M8` için gelecek artımları backlog'a yazar,
- bir veri sahipliği veya güven sınırı kararını gerekçelendirir.

### 3. hafta — Davranış, transaction ve hata

Ürün bu kez sorgu akışı, veri değişimi, transaction sınırı ve arıza yollarıyla yeniden dolaşılır. Ders arama senaryosu girdiden SQL'e ve sonuç çıktısına kadar çalışan en küçük dikey dilim olur.

Öğrenci:

- çalışan ders arama dikey dilimini kurar,
- geçersiz veri ve yinelenen kayıt gibi en az iki hata üretir,
- bir transaction sınırını gösterir,
- bir kısıtın hangi hatayı engellediğini açıklar,
- ilk sorgu planını okur,
- en az üç deterministik test veya tekrar üretilebilir iz sunar.

### 4. hafta — Entegrasyon ve `v0.1`

Model, şema, sorgular, kayıt akışı, hata yönetimi, testler ve belgeler tek sürümde birleştirilir.

`v0.1` kabul kapısı:

- temiz kurulumdan sonra PostgreSQL ve ürün giriş noktası çalışır,
- ders listeleme/arama senaryosu çalışır,
- öğrenci kayıt senaryosu çalışır,
- tekrarlı kayıt veya dolu kontenjan gibi en az bir hata kontrollü reddedilir,
- görünür regresyon testleri yeniden çalıştırılabilir,
- `M1–M8` haritası mevcut kodu ve gelecekteki genişleme noktalarını gösterir,
- `README`, `CHANGELOG`, bilinen sınırlamalar ve AI kullanım notu bulunur,
- öğrenci temel veri akışını ve üç tasarım kararını yaklaşık 90 saniyede kanıtla savunabilir.

`v0.1`, bütün veri teknolojilerinin uygulanmış olduğu anlamına gelmez. İlişkisel çekirdeğin çalıştığı ve sonraki sekiz modülün bağlanacağı sınırların açık olduğu anlamına gelir.

## 5–12. haftaların ürün ve kanıt haritası

| Hafta | Modül | Campus Learning Hub artımı | Asgari kanıt |
| ---: | --- | --- | --- |
| 5 | M1 — DBMS mimarisi ve garantiler | İstemci/sunucu, bağlantı ve oturum gözlemi; iş yükü/garanti kararı | Oturum-sorgu yaşam döngüsü, bağlantı bütçesi, sorumluluk sınırı kaydı |
| 6 | M2 — Model, şema ve bütünlük | Genişletilmiş ER/EER modeli ve ilişkisel migration | Kardinalite/katılım varsayımları, anahtarlar, kısıtlar ve negatif testler |
| 7 | M3 — İlişkisel cebir ve SQL | Doğrulanmış sorgu kütüphanesi ve view'lar | Cebir–SQL eşlemesi, join/alt sorgu/küme işlemi, `NULL` sınır durumu |
| 8 | M4 — Normalizasyon | Anomali üreten şemanın 3NF/BCNF yönünde güvenli dönüşümü | FD/closure, kayıpsızlık, bağımlılık koruma ve önce–sonra karşı örneği |
| 9 | M5 — Transaction ve eşzamanlılık | Son kontenjan yarışı ve güvenli kayıt çözümü | İki oturumlu deney, izolasyon/kilit izi, hata veya retry davranışı |
| 10 | M6 — Depolama ve performans | Hedefli indeks ve sorgu iyileştirmesi | `EXPLAIN ANALYZE`, önce–sonra ölçümü, yazma/depolama bedeli |
| 11 | M7 — Dağıtık ve dayanıklı veri | Dağıtım/replication kararı ve kurtarma tatbikatı | Hata senaryosu, garanti matrisi, backup–restore kanıtı |
| 12 | M8 — Modern veri platformu | Seçilmiş JSONB/RLS/realtime/vector davranışı ve `v1.0` | Karşılaştırma matrisi, güvenlik/arama kanıtı, regresyon ve final mimari savunma |

## Her haftanın öğrenme döngüsü

Bütün haftalarda aynı döngü kullanılır:

> **Haritala → Hatırla → Modelle → Kur → Sına → Açıkla → Yansıt**

| Adım | Veritabanı dersindeki karşılığı |
| --- | --- |
| **Haritala** | Güncel modülü veri ürünündeki karar zincirinde konumlandırmak |
| **Hatırla** | Önceki model, kısıt, sorgu veya transaction fikrini kaynaksız geri çağırmak |
| **Modelle** | ER/UML, ilişkisel cebir, transaction schedule, sorgu planı veya karar matrisi kurmak |
| **Kur** | Şema, migration, SQL, test, deney veya mimari karar eklemek |
| **Sına** | Başarılı yolun yanında negatif durum, karşı örnek veya performans ölçümü üretmek |
| **Açıkla** | Sonucu veri, sorgu, plan, test ya da trace üzerinden savunmak |
| **Yansıt** | Haritayı, karar günlüğünü, changelog'u ve bir sonraki modül bağlantısını güncellemek |

Her yeni mekanizmada mümkün olduğunca **tahmin → çalıştırma → gözlem → açıklama → yeni duruma transfer** sırası izlenir. Öğrenci sorgu sonucunu, kısıt davranışını, eşzamanlılık sonucunu veya sorgu planını çalıştırmadan önce tahmin eder; tahmin ile gerçek kanıt arasındaki farkı açıklar.

## Ders öncesi, ders içi ve ders sonrası

### Ders öncesi

Öğrenciye kısa video veya okuma, bir ortam/örnek kontrolü ve hazırlık soruları verilir. Hazırlık soruları şu dört türü dengeler:

- sistem haritasında “neredeyiz?” sorusu,
- önceki modülden geri çağırma,
- güncel mekanizmayı tahmin etme,
- değişen iş yüküne veya gereksinime transfer.

### Ders içi

Dersin 3 saatlik kuramsal ve 2 saatlik uygulamalı yapısı, uzun bir kesintisiz anlatım olarak kullanılmaz. Kuramsal açıklama; canlı SQL/veritabanı gösterimi, tahmin, küçük trace, karşı örnek ve kısa bireysel kontrollerle bölünür. Uygulama kısmında öğrenci kendi ürününde somut kanıt üretir.

Önerilen akış:

1. Kısa bireysel giriş kontrolü ve eski modülün geri çağrılması
2. Güncel modülün sistem haritasındaki yerinin kurulması
3. Mekanizma anlatımı ve canlı örnek
4. Yanlış sezgi veya hata senaryosu
5. Rehberli SQL/model/deney çalışması
6. Bireysel ürün artımı ve kanıt üretimi
7. Kısa çıkış kontrolü, sözlü açıklama ve yansıtma

Fiziksel katılım tek başına puan değildir. Öğrencinin kendisine ait sorgu, model, migration, test, ölçüm, trace veya karar kaydı bulunmalıdır.

### Ders sonrası

Her haftalık paket dört temel kanıt içerir:

1. **Çalışan kanıt:** Yeniden üretilebilir komut, sorgu veya senaryo çıktısı
2. **Doğruluk kanıtı:** Kısıt, negatif test, transaction izi, ölçüm veya karşı örnek
3. **Tasarım kanıtı:** Model, migration, karar kaydı ya da karşılaştırma tablosu
4. **Açıklama kanıtı:** Kısa teknik gerekçe, sınırlama ve gerektiğinde sözlü savunma

Bunlara ürün haritasındaki değişiklik, changelog ve AI kullanım açıklaması eklenir. Kod veya SQL'in yalnızca çalışması yeterli değildir; iddia uygun veri ve testle doğrulanmalıdır.

## Ölçme-değerlendirme iskeleti

Arşivdeki CMPE 351 tasarımı sürekli değerlendirme modelini kullanır. Resmî ağırlıkların bölüm ve üniversite onayına bağlı olduğu unutulmamalıdır. Bu model korunacaksa 12 haftaya uyarlanmış sayı ve sınırlar şöyledir:

| Bileşen | Önerilen uygulama | Katkı |
| --- | --- | ---: |
| Haftalık yazılı öğrenme kontrolleri | W1 tanılayıcıdır ve puan hesabına girmez; W2–W12'nin en düşük iki kaydı çıkarılır, en iyi 9 kayıt kullanılır | %30 |
| Sözlü kavram açıklamaları | Döneme dengeli yayılmış en az 3 bireysel kontrol | %10 |
| Ürün geliştirme paketleri | W2–W11 arasındaki 10 paket; en düşük bir paket çıkarılır | %50 |
| Son sürüm ve mimari savunma | W12 `v1.0`, yeniden üretilebilirlik ve bireysel savunma | %10 |
| **Toplam** | | **%100** |

W1'in puansız tanılama olması nedeniyle iki düşük kontrol çıkarıldıktan sonra puanlanan kayıt sayısı dokuzdur. Bu sayı düzeltmesi bileşen ağırlıklarını değiştirmez.

Bu dağılım, ayrıca vize/final yapılmayacağı varsayımına dayanır. Üniversite veya bölüm ayrı sınav zorunluluğu getirirse ağırlıklar ilk değerlendirmeden önce resmî olarak yeniden yayımlanmalıdır. Bu README tek başına idari onay yerine geçmez.

### Kanıtların sınırı

Aynı çalışma farklı becerileri gösterebilir; fakat aynı ölçüt iki kez puanlanmaz:

- Ürün artımı; çalışan çözüm, doğruluk, tasarım ve yeniden üretilebilirliği ölçer.
- Quiz; öğrencinin bireysel kavram, SQL okuma ve sonuç tahmini becerisini ölçer.
- Sözlü; öğrencinin kendi kararını kanıt üzerinden bağımsız açıklayabilmesini ölçer.
- Final sürümü; tek tek özellikleri yeniden puanlamak yerine bütünleşme, regresyon, mimari tutarlılık ve değişiklik etkisini ölçer.

## `v0.1`, haftalık artım ve `v1.0` kapıları

### W4 `v0.1`

İlişkisel çekirdek temiz kurulmalı, ana senaryo çalışmalı, hata yolu görünür olmalı, testler tekrar çalıştırılabilmeli ve öğrenci tasarım kararlarını açıklayabilmelidir.

### W5–W11 modül artımları

Her paket şu alanları açıkça belirtir:

- **Primary module:** Güncel `M1–M7`
- **Revisited modules:** En az iki eski veya komşu modül
- **Baseline:** Başlangıç sürümü
- **Increment:** Yeni davranış veya karar
- **Regression:** Korunacak eski senaryo
- **Evidence:** Test, trace, sorgu planı veya ölçüm
- **Map delta:** Veri ürünü haritasındaki değişiklik
- **Next hook:** Sonraki modülün kullanacağı sınır

### W12 `v1.0`

Son hafta hem M8 derinleşmesini hem de bütünleşik sürüm kapısını taşır. Kapsamın aşırı büyümemesi için M8 uygulaması sınırlı tutulur. Öğrenci her modern teknolojiyi kurmak zorunda değildir; bir davranışı uygular, alternatifleri kontrollü kanıtla karşılaştırır.

`v1.0` için:

- temiz kurulum ve çalıştırma belgelenir,
- ana arama ve kayıt senaryoları çalışır,
- bütün migration'lar doğru sırada uygulanır,
- birikimli regresyon testleri geçer,
- transaction ve bütünlük davranışı korunur,
- performans iddiaları ölçümle desteklenir,
- modern platform kararının iş yükü ve garanti gerekçesi açıklanır,
- bilinen sınırlamalar dürüstçe yazılır,
- öğrenci mimari evrimi bireysel olarak savunur.

## Yapay zekâ kullanımı: kullan, doğrula ve savun

Açık öğrenme görevlerinde yapay zekâ; açıklama, örnek üretme, SQL taslağı, alternatif model, hata ayıklama ve test fikri için kullanılabilir. Kullanım isteğe bağlıdır; ücretli hesap zorunlu değildir.

Öğrenci:

- AI katkısını açıkça belirtir,
- üretilen SQL'i gerçek PostgreSQL üzerinde çalıştırır,
- sonuç ve sorgu planını kontrol eder,
- uydurma kaynak, log, ölçüm veya deney sonucu sunmaz,
- başka öğrencilerin verisini, kişisel bilgiyi, erişim anahtarını veya yayımlanmamış soruları dış araçlara yüklemez,
- teslim ettiği her önemli kararı bağımsız olarak açıklayabilir.

Quizler ve puanlanan sözlü kontroller AI'sız ve bireyseldir. Laboratuvar çalışması da önce kısa bireysel tahminle başlar; daha sonra izin verilen araç kullanımı devreye girebilir.

## Kurtarma tabanı

Bir öğrencinin ilk ürün mimarisindeki hata, kalan sekiz modüle katılmasını engellememelidir. W4 sonrasında eğitmen test edilmiş bir `v0.1` tabanı yayımlar.

Bu tabanı kullanan öğrenci:

- kullandığı sürümü belirtir,
- kendi çözümündeki sorunu ve öğrendiği noktayı kısa bir gap analysis ile açıklar,
- sonraki modül artımlarında normal değerlendirilir,
- yalnızca tabanı kopyalayarak önceki eksik puanları geri kazanmaz.

Gerekirse W8 sonrasında, normalizasyon ve migration zincirini güvenli biçimde tamamlayan ikinci kontrollü taban yayımlanabilir. Böylece öğrenci transaction, performans ve dağıtık sistem modüllerinden kopmaz.

## Kapsam yönetimi

Her haftanın içeriği üç şeritte sunulur:

- **Core:** Her öğrencinin tamamlayacağı ve notlandırılacak sınırlı iş
- **Stretch:** İsteğe bağlı ileri çalışma; eksik Core'un yerine geçmez
- **Instructor demo:** Sistemin daha ileri olanaklarını gösterir; öğrenci uygulaması zorunlu değildir

Özellikle M7 ve M8'de amaç çok sayıda bulut ürününü yüzeysel biçimde kurmak değildir. Ücretli hesap zorunlu tutulmaz. Yerel örnekler, sağlanmış trace'ler, emülatörler ve karar matrisleri kullanılabilir. Her teknoloji, Campus Learning Hub içindeki belirli bir gereksinim veya mevcut PostgreSQL çözümünün belirli bir sınırı üzerinden ele alınır.

## 12 haftalık modele geçişte korunacak ilkeler

Takvim kısaldığı için:

- ilk dört haftadaki dört tam sistem geçişinden vazgeçilmez,
- temel ilişkisel model, SQL, normalizasyon, transaction ve indeks konuları daraltılmaz,
- iki haftalık kayıp daha büyük ev ödevleriyle öğrenciye yüklenmez,
- modern teknolojiler ürün turuna dönüştürülmez,
- uygulama kapsamı azaltılırken açıklama ve kanıt kalitesi korunur,
- W12'de M8 artımı ile final entegrasyonu aynı ürün üzerinde yapılır,
- 14 haftalık eski teslim sayıları ve iş yükü tablosu aynen kullanılmaz.

CMPE 351, 6 AKTS'lik bir ders olarak toplam iş yükünü koruyabilir; ancak 12 haftalık gerçek ders takvimi, resmî temas saati ve sınav düzeni kesinleştikten sonra iş yükü hesabı yeniden yapılmalıdır.

## Başarı tanımı

1. haftanın sonunda öğrenci:

- PostgreSQL tabanlı küçük ama çalışan bir veri ürününü kurabiliyor,
- gereksinimden şemaya ve sorguya giden yolu gösterebiliyor,
- en az bir bütünlük veya transaction hatasını kontrollü biçimde ele alabiliyor,
- ürünün sekiz modülünü harita üzerinde açıklayabiliyorsa

ilk spiral tamamlanmıştır.

 1. haftanın sonunda öğrenci:

- gerçek dünya gereksinimini kavramsal ve ilişkisel modele dönüştürebiliyor,
- bütünlüğü veritabanı kısıtlarıyla koruyabiliyor,
- ilişkisel cebir ve SQL ile doğru sorgular kurabiliyor,
- şema anomalilerini tanıyıp normalizasyon kararını savunabiliyor,
- eşzamanlı işlemlerde doğruluğu kanıtlayabiliyor,
- sorgu planını okuyup performans kararını ölçebiliyor,
- dağıtım ve dayanıklılık ödünleşimlerini açıklayabiliyor,
- ilişkisel, doküman, realtime ve vektör yaklaşımlarını iş yükü üzerinden karşılaştırabiliyor,
- bütün bu kararların Campus Learning Hub mimarisini nasıl değiştirdiğini gösterebiliyorsa

sistem amacına ulaşmıştır.

Başarı, sekiz modülü sırayla anlatıp bitirmek değildir. Başarı, **aynı veri ürününü on iki hafta boyunca yeniden modellemek, sınamak, ölçmek ve gerekçeli kararlarla daha güçlü hâle getirmektir.**

## Belge durumu

Bu README, 12 haftalık yeni `1 + 3 + 8` ders işleme sisteminin ana açıklamasıdır. Arşivdeki CMPE 351 belgeleri 14 haftalık `1 + 3 + 10` önceki tasarımı temsil eder ve tarihsel kaynak olarak korunur. Yeni üretimde hafta ve modül planı için bu belge esas alınmalı; eski `W13`, `W14`, `A9` ve `A10` yapıları doğrudan kopyalanmamalıdır.
