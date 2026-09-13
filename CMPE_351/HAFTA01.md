# CMPE 351 Database Systems — 1. Hafta Öğretim Dosyası

**Hafta:** Panorama — tek veri isteği, sekiz modül  
**Dönem yapısı:** 12 hafta, `1 + 3 + 8`  
**Süre:** 155 dakika ders (125 dakika etkin + üç adet 10 dakikalık ara) + 120 dakika laboratuvar (110 dakika etkin çalışma). Resmî ders saati birimi farklıysa dakika planı uyarlanır.  
**Dönem ürünü:** Campus Learning Hub  
**Bu dosyanın sınırı:** Yalnızca 1. haftada anlatılacak, yaptırılacak ve toplanacak işleri içerir.

**İlgili belgeler:** [ERP öğrenci rehberi](ERP_OGRENCI_REHBERI.md) · [Önceki proje planı](PROJE.md) · [Kaynakça](KAYNAKCA.md) · [2. hafta](HAFTA02.md) · [3. hafta](HAFTA03.md) · [4. hafta](HAFTA04.md)  

> **Senaryo uyarısı:** Bu dosya hâlâ önceki Campus Learning Hub örneğini anlatır. Güncel dönem ürünü sandalye fabrikası ERP'sidir; [W2](HAFTA02.md)–[W4](HAFTA04.md) fabrika senaryosunu kullanır. Bu W1 akışı ERP'ye uyarlanana kadar sınıfta kullanılmadan önce [ERP rehberiyle](ERP_OGRENCI_REHBERI.md) karşılaştırılmalıdır.

Bu Türkçe belge eğitmen içindir; öğrenci yönergeleri ve ölçülen içerik için İngilizce eşdeğer hazırlanır. W1 tanılayıcıdır; teslimler geri bildirim sağlar ve yeni bir not bileşeni oluşturmaz. Kurulum aksarsa eşli çalışma ve verilen çıktı üzerinden açıklama kabul edilir; kişisel ortam daha sonra tamamlanır.

## Haftanın ana sorusu

> Bir veri sistemi ders arama ve kayıt iş yükü için neyi garanti etmeli; doğruluk, hız, dayanıklılık, güvenlik ve esneklik arasında hangi bedelleri kabul etmelidir?

İlk haftanın amacı sekiz modülü bitirmek değildir. Öğrenci aynı Campus Learning Hub hikâyesinde gereksinimden modele, SQL’den transaction’a, indeksten dağıtım ve modern veri özelliklerine kadar bütün rotayı görür. Her iddiayı **gözlenen**, **modellenen** veya **gelecekte inşa edilecek** diye sınırlaması beklenir.

## 12 haftalık haritadaki yeri

```text
1. hafta       : sekiz modülün panoraması
2–4. haftalar  : sekiz modül üç haftaya bölünerek ilk sistematik tur (W2 M1–M3, W3 M4–M6, W4 M7–M8)
5–12. haftalar : M1–M8 için sekiz bilinçli derinleşme ve ürün artımı
```

## Ders sonunda öğrencinin göstereceği kanıt

Öğrenci:

- flat file ile DBMS arasındaki temel guarantee ve concurrency farkını açıklar;
- gereksinim, ER/kavramsal model, ilişkisel şema ve constraint zincirini kurar;
- bir arama sorgusunu relational operation ve SQL düzeyinde izler;
- normalization’ın amacını tablo sayısını artırmak değil anomaly azaltmak olarak konumlandırır;
- son kontenjan yarışında transaction/isolation ihtiyacını fark eder;
- index’in sorgu planı ve write/storage maliyeti taşıdığını söyler;
- replication/partitioning/cloud kararlarının trade-off olduğunu görür;
- JSONB, realtime/RLS ve vector search’ü iş yüküne bağlı seçenekler olarak sınıflandırır;
- PostgreSQL üzerinde başarılı okuma ve kontrollü başarısız yazma kanıtı üretir;
- M1–M8 sistem haritasını ve dört cümlelik ilk tasarım kararını teslim eder.

## Eğitmenin ders öncesi hazırlığı

- PostgreSQL 18 ve `psql` erişimini temiz ortamda doğrula; öğrenciye gizli anahtar gerektirmeyen yerel kurulum sun.
- Küçük sentetik veri hazırla: students, courses, offerings, enrollments ve sınırlı content tablosu.
- Referans akışları hazırla: search, accepted enrollment, duplicate rejection, capacity rejection.
- İki ayrı `psql` oturumuyla son kontenjan yarışını prova et; sonucu sürüm/isolation ayarlarıyla kaydet.
- `EXPLAIN` ve küçük index karşılaştırması hazırla; küçük veri üzerinde süre farkını genellememek için plan şekline odaklan.
- JSONB, RLS/realtime ve pgvector için yalnız kısa preview hazırla; dış servis çalışmazsa ekran kaydı/çıktı kullan.
- Worksheet’e M1–M8 ve `observed / modelled / future` sütunlarını ekle.
- Gerçek öğrenci verisi kullanma; tüm kayıtlar sentetik olsun.

## Ders öncesi öğrenci hazırlığı — 30–40 dakika

1. PostgreSQL belgelerinden “What is PostgreSQL?” ve tutorial girişini oku.
2. SQLBolt’un ilk SELECT dersini tamamla veya eşdeğer iki sorgu yaz.
3. Aşağıdaki sorular için not/AI kapalı tahmin üret:
   - CSV dosyası aynı anda iki kayıt isteğinde son kontenjanı güvenle korur mu?
   - `NOT NULL` ile iş kuralının tamamı ifade edilebilir mi?
   - Index eklemek her sorguyu hızlandırır mı?
   - Commit edilen veri her dağıtık kopyada anında görünür mü?
   - Vector similarity sonucu “doğru cevap” mıdır?
4. `psql --version` ve verilen bağlantı komutunu doğrula; gerçek parola veya bağlantı dizesini teslim dosyasına koyma.

## Panorama haritası: sekiz modül

| Modül | İlk haftadaki soru | Derinleşme haftası |
| --- | --- | ---: |
| M1 — DBMS architecture, workload, guarantees | DBMS neden var; hangi iş yükü ve garanti hedefleniyor? | 5 |
| M2 — Conceptual model, relational schema, integrity | Gereksinim hangi entity/relation/constraint’e dönüşür? | 6 |
| M3 — Relational algebra ve SQL | İstenen sonuç hangi işlemlerle ve sorguyla üretilir? | 7 |
| M4 — Functional dependencies ve normalization | Hangi redundancy hangi anomaly’yi doğurur? | 8 |
| M5 — Transactions, concurrency, recovery | Aynı anda gelen isteklerde doğruluk ve geri dönüş nasıl korunur? | 9 |
| M6 — Storage, indexes, query processing | Veri nasıl bulunur; planın read/write/storage maliyeti nedir? | 10 |
| M7 — Distributed/cloud/resilient data | Veri kopyalanır/bölünürse consistency, availability ve failure nasıl değişir? | 11 |
| M8 — Document, realtime ve modern PostgreSQL | JSONB, RLS/realtime ve vector search hangi iş yükünde anlamlıdır? | 12 |

## 155 dakikalık ders akışı

| Süre | İçerik ve öğretmen hamlesi | Öğrenci işi / kanıt |
| --- | --- | --- |
| 00–08 | Açılış vakası: iki öğrenci son kontenjana aynı anda basıyor. “Sistem neyi garanti etmeli?” | Bireysel guarantee tahmini |
| 08–15 | Referans Campus Learning Hub: search, accepted, duplicate/full rejection. Arayüzden çok veri kararlarını göster. | Request–data–rule–result izi |
| 15–25 | **M1:** DBMS bileşenleri, OLTP/analytical workload ayrımı, correctness/durability/performance hedefleri. | İş yükü kartı |
| 25–30 | M1 retrieval ve “flat file neden yetmeyebilir?” mini tartışması. | Bir gerekçe + sınır |
| 30–40 | **Ara** | |
| 40–51 | **M2:** Requirement → entity/relationship → table/key/constraint. Enrollment’ın neden ayrı ilişki olduğunu tartış. | Mini ER ve iki constraint |
| 51–61 | **M3:** Selection, projection, join fikrini course search sorgusunda göster; SQL ile eşleştir. | Sonuç tahmini + sorgu okuma |
| 61–70 | **M4:** Tek geniş tablo üzerinden update/insert/delete anomaly; FD ve normalization’a yalnız amaç düzeyinde giriş. | Bir anomaly açıklaması |
| 70–80 | **Ara** | |
| 80–90 | **M5:** Atomicity/isolation/durability, iki oturumlu son koltuk yarışı ve rollback. “Check sonra insert” ayrımının yarışa açık olduğunu göster. | Interleaving çizimi |
| 90–98 | **M6:** Heap/page/index/query plan panoraması. Önce plan tahmini, sonra `EXPLAIN`; küçük veride zaman genellemesi yapma. | Plan gözlemi |
| 98–110 | M5–M6 bağlantısı ve trade-off cümlesi; **M7:** replication/partitioning/failure ve cloud managed service sorumlulukları, somut read/write senaryosu. | Trade-off cümlesi + failure senaryosu |
| 110–120 | **Ara** | |
| 120–132 | **M8:** Aynı content için relational sütun, JSONB metadata, realtime event ve vector embedding preview. RLS’nin neden gerekli olduğunu sor. | Teknoloji–ihtiyaç eşleştirmesi |
| 132–142 | Sekiz modülü tek ürün akışında birleştir; takımlar bir tasarım iddiasını savunur, başka takım bir karşı örnek sorar. | M1–M8 ilk harita + iddia–kanıt–sınır kartı |
| 142–150 | Bireysel panorama tablosu: O/M/F ve her modüle bir kanıt/gelecek soru. | Kişisel harita |
| 150–155 | Exit ticket ve laboratuvar hedefini açıkla. | Çıkış kaydı |

## 120 dakikalık laboratuvar akışı

| Süre | Uygulama | Beklenen kanıt |
| --- | --- | --- |
| 00–10 | Ortam, PostgreSQL sürümü ve sentetik veritabanı bağlantısını doğrula. | Sürüm ve başarılı bağlantı |
| 10–25 | Verilen DDL’i çalıştır; PK, FK, UNIQUE, NOT NULL ve CHECK’i şema üzerinde bul. | Constraint haritası |
| 25–40 | Ders listeleme ve anahtar kelime arama sorgusu yaz/çalıştır. | Sorgu + beklenen satırlar |
| 40–55 | Geçerli enrollment ekle; ilgili course/student ile join ederek doğrula. | Başarılı yazma ve okuma |
| 55–70 | Duplicate veya geçersiz FK yazmasını bilerek dene; hata metnini ve değişmeyen state’i kaydet. | Kontrollü başarısız yazma |
| 70–80 | **Ara** | |
| 80–94 | İki oturumlu kısa capacity yarışını önce tahmin et, sonra eğitmen rehberliğinde çalıştır. | Interleaving + sonuç |
| 94–104 | Bir search sorgusunda `EXPLAIN` planını oku; varsa index öncesi/sonrası plan şeklini karşılaştır. | Plan kanıtı + sınırlama |
| 104–112 | M1–M8 haritasına laboratuvardaki O/M/F kanıtlarını ekle. | Güncel panorama haritası |
| 112–118 | Dört cümlelik karar: “Neden çekirdekte PostgreSQL?” Alternatif ve bedel içermeli. | Tasarım kararı |
| 118–120 | Dosyaları kontrol et ve gizli bilgi bulunmadığını doğrula. | Teslim kontrolü |

## Sekiz modülün ilk hafta için doğru derinliği

| Modül | Bu hafta anlat | Bu hafta ertele/iddia etme |
| --- | --- | --- |
| M1 | DBMS; data, metadata, query/transaction ve guarantee yönetir. | İç bileşenlerin ayrıntılı implementasyonu |
| M2 | Model gereksinimden doğar; keys/constraints geçersiz state’i sınırlar. | Her iş kuralının tek constraint ile çözüldüğü iddiası |
| M3 | SQL sonucu relational operations ile açıklanabilir. | SQL sözdiziminin tamamı |
| M4 | FD/redundancy anomaly üretir; decomposition gerekçeli olmalıdır. | “Daha çok tablo her zaman daha iyi” |
| M5 | Transaction sınırı ve isolation concurrent correctness’i etkiler. | Tek kullanıcı testini concurrency kanıtı sayma |
| M6 | Index erişim yolu sunar; write/storage maliyeti ve plan seçimi vardır. | Küçük tek ölçümden hız genellemesi |
| M7 | Replication/partitioning farklı problemleri çözer ve failure modeli getirir. | “Cloud otomatik olarak güvenli/ölçeklenir” |
| M8 | JSONB, realtime/RLS ve vectors belirli erişim ihtiyaçlarına cevap verir. | Vector similarity’yi doğruluk; realtime’ı yetki olarak görme |

Zorunlu prototip küçük şema, arama, başarılı kayıt ve duplicate/FK reddidir. Kontenjan yarışı ve indeks karşılaştırması eğitmen rehberli panorama deneyidir; ilk hafta concurrency çözümü veya performans iyileştirmesi beklenmez.

Sabit veri: `S100`, `S101`; `CMPE351` dersi; `2026-FALL` döneminde capacity=`1` olan offering. İlk `S100` başarılı, ikinci aynı kayıt UNIQUE ihlali, olmayan `S999` FK ihlalidir. Farklı öğrenci yazmasının basit şemada kontenjanı aşabildiğini ayrıca göster; çözümü M5'e bağla. Hataları ayrı transaction veya savepoint ile çalıştır; hata sonrasında aborted transaction için rollback gerekir. Ayrıntı [prototip sözleşmesindedir](PROJE.md).

## Sorulacak kritik sorular ve beklenen yön

- **DBMS kullanınca bütün iş kuralları otomatik korunur mu?** Hayır; doğru model, constraint, transaction ve uygulama sözleşmesi gerekir.
- **Enrollment neden `student.course_id` alanından ibaret olmayabilir?** Çoktan çoğa ilişki, dönem/section, status ve kayıt zamanı gibi ilişkiye ait özellikler vardır.
- **SQL declarative ise execution maliyetini düşünmeye gerek yok mu?** Sorgu niyeti declarative’dir; optimizer plan seçer fakat şema, istatistik ve index tasarımı maliyeti etkiler.
- **Transaction içindeki iki ayrı statement otomatik olarak son koltuğu korur mu?** Isolation ve kullanılan locking/constraint stratejisine bağlıdır; interleaving ile kanıtlanmalıdır.
- **Index her şeyi hızlandırır mı?** Hayır; seçicilik, sorgu şekli, maintenance ve storage bedeli vardır.
- **Replica varsa veri kaybı imkânsız mıdır?** Hayır; replication mode, lag, failure ve recovery hedefleri belirtilmelidir.
- **RLS yalnız UI’daki rol kontrolünün tekrarı mıdır?** Hayır; veri erişim sınırını veritabanına yaklaştırır, fakat politikaların ayrıca test edilmesi gerekir.
- **Vector search cevabı bilir mi?** Benzer adaylar üretir; relevance değerlendirmesi, filtre, yetki ve uygulama doğrulaması gerekir.

## Yaygın yanılgılar ve müdahale

- “Database = Excel/tablolar.” → Concurrency, constraint, query, transaction ve recovery farklarını aynı vaka üzerinde göster.
- “ER diagram doğruysa şema da doğrudur.” → Key, nullability ve geçersiz durum örnekleri sor.
- “Normalization performansı bozar; gereksizdir.” → Önce anomaly/correctness sorununu, sonra ölçülmüş trade-off’u ayır.
- “Commit = her yerde anında görünür ve sonsuza dek güvende.” → Local durability, replica visibility ve backup/recovery hedeflerini ayır.
- “NoSQL şemasızdır.” → Verinin fiilî şekli ve validation/erişim deseninin yine var olduğunu göster.
- “Modern özellik kullanmak sistemi modern yapar.” → Her teknoloji için çözdüğü somut ihtiyacı ve getirdiği maliyeti sordur.

## Hafta sonu teslim paketi

1. M1–M8 için `observed / modelled / future + kanıt` haritası.
2. Şema/constraint işaretlemesi ve course-search SQL’i.
3. Bir başarılı enrollment çıktısı.
4. Bir kontrollü constraint failure ve değişmeyen state kanıtı.
5. Dört cümlelik PostgreSQL çekirdek kararı: ihtiyaç, tercih, alternatif, bedel.
6. En fazla 90 saniyelik bireysel ürün/veri akışı açıklaması veya canlı sözlü kontrol.
7. AI kullanıldıysa araç, amaç, kabul/reddedilen öneri ve doğrulama yöntemi.

## Exit ticket ve 2. haftaya köprü

Öğrenci notsuz yanıtlar:

1. Bugün doğrudan gözlediğin bir database guarantee nedir?
2. Yalnızca model/preview olarak gördüğün bir özellik nedir?
3. Son kontenjan probleminde hangi iki operation tek doğruluk sınırına alınmalıdır?
4. Sekiz modülden hangisinde ilk sezgin değişti?
5. 2. haftada M1–M3'ü işlerken ilk hangi veri veya güven sınırını görünür yaparsın?

2. haftaya başlangıç cümlesi: **“Panoramada gördüğümüz sekiz modülün ilk üçünü; açık şema, bağlantı ve sorgu sınırları olan küçük bir data-product iskeletine dönüştüreceğiz.”**
