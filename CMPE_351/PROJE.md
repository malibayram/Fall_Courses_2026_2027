# CMPE 351 — SQL Prototipinden Campus Learning Hub Veri Ürününe

> Önceki proje örneğidir. Öğrencilere verilecek güncel kapsam ve teslim sözleşmesi: [Sandalye Fabrikası ERP — CMPE 351 rehberi](ERP_OGRENCI_REHBERI.md). Bu dosya önceki tasarımın referansı olarak korunmuştur.

**Dönem:** 12 öğretim haftası, `1 + 3 + 8`  
**Ürün:** Campus Learning Hub  
**Teknik taban:** PostgreSQL 18, SQL ve `psql`; web arayüzü zorunlu değil  
**Belgeler:** [Ders sistemi](README.md) · [İlk hafta akışı](HAFTA01.md) · [Kaynakça](KAYNAKCA.md)  
**Durum:** Proje planı ve kabul sözleşmesidir; tarif edilen yeni starter/migration/test paketleri henüz hazırlanmış veya çalıştırılmış değildir.

## Amaç ve dönem sonundaki ürün

Öğrenci ilk hafta küçük bir ders kataloğunu SQL ile arar ve bir öğrenciyi derse kaydeder. Aynı veri tabanı; daha iyi model, gelişmiş sorgular, normalizasyon, eşzamanlı kayıt, performans, kurtarma ve modern veri özelliğiyle büyür. Her hafta değişen şey aynı Campus Learning Hub ürününün veri garantisi veya yeteneğidir.

Ürün dönem sonunda ders arar, öğrenci kayıtlarını listeler, duplicate ve geçersiz ilişkiyi reddeder, eşzamanlı son kontenjan yarışında sınırı korur, farklı kullanıcıların verisini yetkiye göre gösterir ve yedekten geri yüklenebilir. Arama için ölçülmüş bir indeks kararı ile seçilmiş bir modern özellik bulunur. İlişkisel çekirdek ve SQL her aşamada görünür kalır.

SE 237 ile aynı problem alanını kullanmak mümkündür; bu dersin teslimi bağımsız şema, SQL, transaction, plan ve kurtarma kanıtıdır. Java uygulaması veya diğer derse kayıtlı olmak önkoşul değildir.

## 1. hafta küçük proje: Mini Course Catalog

Eğitmen dört tablonun DDL/seed iskeletini ve `psql` komutlarını sağlar. Öğrenci bir filtreyi, bir join'i ve kontrollü yazma denemelerini tamamlar. İlk hafta sıfırdan normalizasyon veya güvenli concurrency algoritması yazması beklenmez.

| Tablo | Başlangıç alanları | Temel kural |
| --- | --- | --- |
| `students` | `student_id`, `name` | PK; ad NOT NULL |
| `courses` | `course_id`, `code`, `title` | PK; code UNIQUE NOT NULL |
| `offerings` | `offering_id`, `course_id`, `term`, `section`, `capacity` | FK; UNIQUE(course_id,term,section); capacity>0 ve NOT NULL |
| `enrollments` | `student_id`, `offering_id`, `enrolled_at` | Bileşik PK(student_id,offering_id); iki FK |

Zorunlu alanlara `NOT NULL` ayrıca verilir: `CHECK(capacity>0)` tek başına NULL'u reddetmez. Başlangıçta yalnız aktif kayıt tutulur; iptal sonraki işlem tasarımında ele alınabilir. `student_id` ve `offering_id` birlikte tekillik sağlar; offering yerine course üzerinden tekillik koymak farklı dönem kayıtlarını yanlış engeller.

**Sabit seed:** S100/Ayşe, S101/Deniz; CMPE351/Database Systems; 2026-FALL, section 1, capacity 1, offering_id 1. Arama örneği:

```sql
SELECT c.code, c.title, o.offering_id, o.capacity
FROM courses AS c
JOIN offerings AS o ON o.course_id = c.course_id
WHERE o.term = '2026-FALL' AND c.code = 'CMPE351'
ORDER BY o.offering_id;
```

| Deney | Beklenen sonuç | İlk bağ |
| --- | --- | --- |
| Yukarıdaki sorgu | Bir offering satırı | M2 model, M3 sorgu |
| S100 → offering 1 | Bir kayıt eklenir | M1 kalıcı veri, M2 bütünlük |
| Aynı S100 → offering 1 yeniden | UNIQUE/PK ihlali; satır sayısı artmaz | M2 olumsuz test |
| Var olmayan S999 → offering 1 | FK ihlali; satır sayısı artmaz | M2 ilişki |
| Ayrı sandbox'ta S101 → dolu offering 1, doğrudan INSERT | Basit şema bunu kabul edebilir | M5 için kapasite açığı |

Son deney bilinen bir eksikliği kanıtlar: satırlar arası kontenjan kuralı yalnız `CHECK(capacity>0)` ile korunmaz. Açıkça hatalı örnek ayrı test verisinde çalıştırılır; W1 prototipi “concurrency-safe” diye sunulmaz. Beklenen SQLSTATE duplicate için `23505`, FK için `23503`; yerelleştirilmiş hata metniyle birebir eşleştirme yapılmaz.

**Laboratuvarın ilk 70 dakikası:** Bağlantı → DDL/seed → arama → başarılı kayıt → negative test. Sonraki sürede eğitmen concurrency/plan panoraması ve öğrenci haritası bulunur. Her negatif deneme kendi transaction'ında yapılır veya savepoint'e dönülür; beklenen hatanın bütün kurulum betiğini yarıda bırakmasına izin verilmez.

**Teslim:** Şema kısıt eşlemesi, sorgu, bir başarı ve iki negatif test, M1–M8 haritası, kısa PostgreSQL kararı. Bu veritabanı W2'de migration ve test yapısına taşınır; dönem boyunca yeniden kurulan bağımsız haftalık veritabanları açılmaz.

## Hedef mimari ve değişmeyen garantiler

```text
psql / verilen CLI
     → rol ve bağlantı sınırı
     → sorgular + kayıt transaction'ı
     → PostgreSQL: şema, kısıtlar, view, indeks, RLS
     → yedek/kurtarma + seçilmiş modern veri davranışı
```

Hedef depo yapısı: `migrations/`, `seed/`, `queries/`, `transactions/`, `tests/`, `experiments/`, `docs/`. Dersin CLI katmanı SQL'in görünmesini sağlar; ORM sorguların yerini almaz. SQL dosyaları, bağlantı rolü, seed sürümü ve beklenen çıktı birlikte sürümlenir.

| Garanti | Ne zaman ürün kabulüne girer? | Kanıt |
| --- | --- | --- |
| Kimlik, ilişki ve tekillik | W1, W6'da genişletilir | PK/FK/UNIQUE negatif test |
| Arama sonucunun doğruluğu | W1, W7'de derinleşir | Elle belirlenmiş beklenen satır kümesi |
| Kayıt akışının atomikliği | W3–W4 temel transaction | Başarı/rollback sonrası state |
| Eşzamanlı kapasite sınırı | W9 | Kontrollü iki oturum ve bütün yazma yolları |
| Kullanıcı/veri erişim sınırı | W2 rol tasarımı; W12 RLS test kapısı | Yetkili/başka kullanıcı/anonim test |
| Ölçülmüş performans | W10 | Plan + ham ölçüm + yazma/depolama bedeli |
| Yeniden kurma ve geri yükleme | W4 temiz kurulum; W11 restore | Ayrı hedef DB'de doğrulanmış veri |

## Hafta hafta proje planı

### W1 — Sekiz modülün panoraması ve SQL prototipi

**Artım:** Yukarıdaki dört tablo ve küçük sorgu/kayıt deneyleri. **Kavram:** M1–M8 bütünü; öğrencinin gerçek uygulaması M1/M2/M3. **Kanıt:** Sorgu sonucu, başarılı yazma, duplicate/FK reddi ve kapasite açığının sınırı. **Sonraki bağ:** Migration ile tekrar kurulabilir hâle getirme.

### W2 — Bütün sistemin yapısı ve walking skeleton

**Artım:** W1 şemasını sürümlü migration ve seed'e taşı; verilen `setup/check` girişlerini bağla. Migration sahibi ile normal uygulama rolünü ayır. Bu hafta M1–M3'e (DBMS mimarisi/iş yükü, kavramsal model/şema/bütünlük, ilişkisel cebir/SQL) odaklanılır; M4–M8 haritada bekleyen kalır.

**Kanıt:** Boş test veritabanında kurulum, health query, ER taslağı, sahiplik/güven sınırı, sekiz modüllük backlog. Yapılmış migration değiştirilmez; sonraki değişiklik yeni dosyadır. **Sonraki bağ:** Arama ve kayıt akışının davranışı.

### W3 — Bütün sistemin davranışı ve hata

**Artım:** Gerçek girdiden parametreli SQL'e ve sonuç çıktısına arama dikey dilimi; başarılı kayıt için açık transaction ve iki hata yolu. Bu hafta M4–M6'ya (fonksiyonel bağımlılıklar/normalizasyon, transaction/eşzamanlılık/kurtarma, depolama/indeks/sorgu işleme) odaklanılır; M1–M3 kısaca geri çağrılır, M7–M8 bekleyen kalır.

**Kanıt:** Arama, duplicate, geçersiz FK ve rollback izleri; SQL string birleştirmesi yerine verilen güvenli bağlama yolu; ilk plan gözlemi. **Sınır:** Transaction yazmak tek başına concurrent capacity garantisi değildir. **Sonraki bağ:** `v0.1` sürüm sözleşmesi.

### W4 — Entegrasyon ve `v0.1`

**Artım:** Kurulum, katalog arama, kayıt, listeleme, kontrollü ret ve testleri birleştir. Tek istemcili capacity kontrolü verilebilir; genel eşzamanlılık garantisi W9'a kadar bilinen eksiktir. Bu hafta M7–M8'e (dağıtık/bulut/dayanıklı veri sistemleri, doküman/realtime modeller ve modern PostgreSQL) odaklanılır; M1–M6 kısaca geri çağrılır ve ikinci tur bu haftayla tamamlanır. M1–M8 haritasında uygulanmış ve gelecekteki parçalar ayrı görünür.

**Kanıt:** Temiz kurulum; normal kayıt; duplicate/FK reddi; en az bir rollback; regression suite; README, değişiklik kaydı, sınırlamalar. **Sonraki bağ:** Aynı çalışan taban sekiz derinleşme artımını taşır.

### W5 — M1: İş yükü, DBMS mimarisi ve garantiler

**Talep:** Kayıt ekranı ile toplu rapor aynı veritabanını kullanıyor; bağlantı ve sorgu yükü görünür olsun. **Artım:** Verilen ölçüm kancalarıyla oturum/iş yükü raporu; `pg_stat_activity`, transaction ömrü ve bağlantı bütçesinin incelenmesi. **Geri çağır:** M3 sorgu, M5 transaction.

**Kanıt:** Client → session → parser/planner/executor → storage yolunun haritası; kısa/uzun transaction örneği, OLTP/analitik ayrımı, gözlenen ortam için gecikme/throughput kaydı. Keyfî üretim SLA'sı uydurulmaz. **Sonraki bağ:** Şema büyümesinin maliyeti ve gereksinimi.

### W6 — M2: Kavramsal model, şema ve bütünlük

**Talep:** Bir ders farklı öğretim elemanları ve birden çok section ile açılsın. **Artım:** ER/EER modeli ve güvenli migration; Instructor/TeachingAssignment gibi ilişkiyi gereksinimle ekle. **Geri çağır:** M1 garanti, M3 join.

**Kanıt:** Cardinality/participation varsayımları; doğal/surrogate/candidate key; domain/entity/referential integrity; NULL ve deletion policy testleri. Zayıf varlık ve specialization için kısa alternatif model çizilir; her kavram zorla tabloya çevrilmez. **Regresyon:** W1 sorgusu ve eski kayıtlar. **Sonraki bağ:** Yeni raporlama ihtiyaçları.

### W7 — M3: İlişkisel cebir ve sorgu kütüphanesi

**Talep:** Boş kontenjan, hiç kaydı olmayan öğrenci ve bölüm bazlı yoğunluk raporları üret. **Artım:** Gereksinim → cebir → SQL açıklamasıyla sorgu kütüphanesi ve bir view. **Geri çağır:** M2 anahtar/kardinalite, M1 iş yükü.

**Kanıt:** JOIN, outer join, aggregate/HAVING, subquery/EXISTS, CTE, window ve küme işlemleri için küçük sabit fixture'lar. Selection/projection/product/rename/division fikri ve SQL'in bag/NULL semantiği karşılaştırılır. `NOT IN` + NULL tuzağı; `ORDER BY` yokken sıra garantisi verilmemesi. Eğitmen veri seti sağlar; öğrenci seçilmiş sorguları tamamlar ve kalan örnekleri izler. **Sonraki bağ:** Tekrarlı verinin model hatası olup olmadığı.

### W8 — M4: FD ve normalizasyon

**Talep:** Eski bir spreadsheet ithal edilecek; öğretim elemanı/bölüm adları çelişiyor. **Artım:** Ayrı `staging` tablosundaki tekrarlı modeli analiz et; 3NF/BCNF yönünde dönüşüm ve migration ekle. Sağlam çekirdek sırf ödev için bozulmaz. **Geri çağır:** M2 key/constraint, M3 sonuç eşdeğerliği.

**Kanıt:** Gerçek update/insert/delete anomaly; FD kümesi, closure ve candidate key hesabı; 1NF–BCNF gerekçesi; kayıpsız ayrıştırma ve bağımlılık koruma kontrolü. Örnek veri üstünde join'in tutması tek başına genel kayıpsızlık ispatı sayılmaz. BCNF dönüşümü bağımlılık korumuyorsa bu açıkça yazılır. **Regresyon:** Migration öncesi/sonrası iş sorgularının aynı anlamlı sonuçları. **Sonraki bağ:** Yeni şema üstünde concurrency.

### W9 — M5: Transaction, concurrency ve recovery

**Talep:** İki öğrenci son kontenjana aynı anda kayıt olunca yalnız biri kabul edilsin. **Artım:** Offering satırını kilitleyen ortak kayıt transaction'ı; aynı offering'e yazan tüm akışların bu yolu kullanması. **Geri çağır:** M2 bütünlük, M4 doğru anahtarlar.

**Kanıt:** Aşağıdaki kontrollü interleaving; rollback; bounded retry gereken hata; deadlock ve WAL/recovery izi. ACID, schedule/serializability, MVCC, lost update/non-repeatable read/phantom örnekleri SQL standardı ile PostgreSQL davranışı ayrılarak açıklanır. PostgreSQL'de dirty read üretildiği iddia edilmez. **Regresyon:** Duplicate ve FK kuralları. **Sonraki bağ:** Doğru transaction'ın maliyeti.

### W10 — M6: Storage, index ve query processing

**Talep:** Katalog büyüdüğünde belirli dönem/code araması yavaşlıyor. **Artım:** Verilen seed büyütücüsüyle ölçüm; bir hedef sorguya gerekçeli indeks/migration ve önce–sonra plan. **Geri çağır:** M3 sorgu, M5 yazma maliyeti.

**Kanıt:** Page/record/heap/buffer yolu; selectivity/statistics/cost; scan/join/sort/aggregate düğümleri. B-tree/hash/GIN/GiST/BRIN ve composite/partial/covering seçenekleri karar matrisiyle karşılaştırılır; hepsi kurulmaz. `EXPLAIN (ANALYZE, BUFFERS)` sorguyu gerçekten çalıştırır; yazma sorgularında etkisi hesaba katılır. Ham tekrarlar, veri boyutu, indeks boyutu ve write maliyeti teslim edilir. **Sonraki bağ:** Yerel performanstan dayanıklılığa geçiş.

### W11 — M7: Dağıtım, bulut ve dayanıklılık

**Talep:** Veritabanı kaybedilirse sistem yeniden hizmete alınabilsin; okuma kopyası seçeneğinin bedeli anlaşılsın. **Artım:** Yedeği ayrı boş test DB'ye restore et; kritik sorgu/kayıt kurallarıyla doğrula. Eğitmenin sağladığı iki düğümlü replication deneyinde lag/failure izi gözle. **Geri çağır:** M5 durability, M6 workload.

**Kanıt:** Restore sonrası satır ve iş kuralı kontrolleri, ölçülen recovery süresi; RPO/RTO hedefi ve ölçüm ayrımı. Replication/sharding/partitioning, leader/follower/multi-leader, quorum, consistency, CAP/PACELC ve failover seçenekleri somut hata senaryosuyla karşılaştırılır. PITR ayrıca WAL arşivi ister; `pg_dump` restore'u PITR diye sunulmaz. **Sonraki bağ:** Yetki ve modern veri seçiminin final ürüne eklenmesi.

### W12 — M8: Modern veri davranışı ve `v1.0`

**Talep:** İçerik metadata'sı esnek olsun ve kullanıcı yalnız izinli veriyi görebilsin. **Core artım:** Verilen content/RLS iskeletinde bir JSONB filtre sorgusu ve role göre erişim kuralı tamamla. RLS güvenlik kapısı çekirdektir; ayrıca tüm bulut ürünlerini kurma görevi verilmez. **Geri çağır:** M2 model, M3 sorgu, M5 tutarlılık, M6 indeks.

**Kanıt:** Rol bazlı pozitif/negatif test; JSONB şekli ve indeks gerekçesi; ilişkisel/document/realtime karşılaştırma matrisi. MongoDB embedding/reference, Firestore kuralları, Realtime Database JSON ağacı/listener ve Supabase Auth/API/Storage/Realtime özellikleri sağlanan kısa örneklerde aynı ihtiyaca göre değerlendirilir. pgvector exact/approximate ve hybrid search için eğitmen sabit veri/embedding demosu verir; benzerlik “doğru cevap” sayılmaz. **Stretch:** Bir realtime veya vector davranışını bağımsız uygula. **Final:** Önceki tüm kapılarla `v1.0`.

## Son kontenjan deneyinin açık doğruluk sözleşmesi

Başlangıç: offering capacity=1 ve sıfır kayıt. Ayrı oturumlar T1/S100 ve T2/S101 kullanılır. `sleep` ile şansa bağlı yarış yerine eğitmenin iki oturum bariyerleri kullanılır.

1. **Hatalı sürüm:** T1 count=0 okur; T2 count=0 okur; ikisi farklı student ID ile INSERT ve COMMIT yapar. UNIQUE bunu engellemez; iki satır kapasiteyi aşar.
2. **Düzeltilmiş sürüm:** `READ COMMITTED` transaction'ında önce offering satırını `SELECT ... FOR UPDATE` ile kilitle; sonra ayrı statement ile duplicate/count kontrolü yap; uygunsa INSERT ve COMMIT, değilse ret ve rollback/commit.
3. T1 kilidi tutarken T2 beklemelidir. T1 commit sonrası T2'nin yeni statement'ı güncel count=1 görür ve FULL döndürür. Her kabul yolu aynı kilit protokolünü kullanır.
4. Uygulama rolünün doğrudan enrollment yazması engellenir; eğitmenin sağladığı kontrollü veritabanı işlevi üzerinden kayıt yaptırılır. Capacity değişimi/iptal gibi yollar aynı offering kilidini alır; aksi hâlde garanti eksiktir.
5. Duplicate exception'ı ve retry edilebilir `40001`/`40P01` gibi sonuçlar ayrılır. Retry varsa yalnız son INSERT değil bütün transaction sınırlı sayıda yeniden denenir.

İşlevin ownership/izin/search_path iskeleti eğitmen tarafından hazırlanır; öğrenci yarışın mekanizmasına odaklanır. Doğrudan yetkili yönetici yazmasının bu uygulama protokolünü aşabileceği sınır olarak belirtilir. [PostgreSQL isolation belgesi](https://www.postgresql.org/docs/18/transaction-iso.html).

## RLS ve arama için yanlış güvenceyi önleyen testler

- RLS testleri tablo sahibi/superuser/BYPASSRLS rolüyle yapılmaz. `student_a`, `student_b` ve anonim erişim için farklı oturumlar kullanılır; hem okuma hem yazma kontrol edilir. Başka kimlikle kayıt denemesi reddedilmelidir. [Row Security Policies](https://www.postgresql.org/docs/18/ddl-rowsecurity.html).
- İstemcinin serbestçe değiştirebildiği kullanıcı kimliği güvenilir auth yerine geçmez. Yerel demo için PostgreSQL rolünden eğitmenin sağladığı kimlik eşlemesi kullanılır; Supabase seçeneğinde doğrulanmış auth bağlamı ayrıca incelenir.
- pgvector demosunda sabit embedding'ler küçük bir fixture'dır; ücretli API gerekmez. ANN, exact aramayla recall/top-k açısından karşılaştırılır; relevance ayrıca etiketli örneklerle ölçülür. RLS/erişim filtresini aşan sonuç kabul edilmez. [pgvector](https://github.com/pgvector/pgvector).
- Realtime bildirimi gelmesi transaction doğruluğunun veya erişim yetkisinin kanıtı değildir. Yeniden bağlanma, yinelenen bildirim ve yetkisiz abone örnekleri karşılaştırma dosyasında bulunur. [Supabase realtime authorization](https://supabase.com/docs/guides/realtime/authorization).

## Tek teslim ve sürüm düzeni

Her hafta başlangıç sürümü, yeni migration/sorgu/deney, regresyon çıktısı, M1–M8 harita farkı ve kısa gerekçe birlikte teslim edilir. “Çalıştırıldı” kanıtı komut, rol, sürüm, fixture ve beklenen/gerçek sonucu içerir. Migration testi hem sıfırdan kurulum hem bir önceki sürümden yükseltme yapar; eski veri sessizce atılmaz.

**W1:** Tanılayıcı prototip. **W4:** İlişkisel `v0.1`. **W8:** Model/migration ara kontrolü ve gerekirse kurtarma tabanı. **W12:** M8 artımıyla birlikte `v1.0`. Not ağırlıkları [CMPE 351 ders modeli](README.md) ile aynıdır; W12 ayrı bir ikinci büyük proje değildir.

Final demosu: temiz kurulum → search → geçerli/duplicate kayıt → iki oturumda son kontenjan → ölçülmüş sorgu planı → restore doğrulaması → başka kullanıcı erişiminin reddi → seçilmiş modern davranış. Öğrenci her adımda hangi garantiye hangi kanıtın yettiğini ve neyi kanıtlamadığını açıklar.

## İş yükü ve eğitmen desteği

Eğitmen bağlantı/build ortamını, migration runner'ı, sabit seed'leri, iki oturum bariyerini, veri büyütücüsünü, replication düzeneğini ve RLS başlangıcını sağlar. Öğrenci her hafta sınırlı bir tasarım/SQL/deney kararını tamamlar. Hazırlık, lab ve proje aynı işi yeniden yazdırmaz; lab çıktısı haftalık paketin başlangıcıdır.

Özellikle W12'de bütün modern teknolojiler derin kodlama ödevi olmaz; bir çekirdek artım ve sağlanan karşılaştırma kanıtlarıyla sekiz modül tamamlanır. W4/W8 kurtarma tabanları ve yerel çalışma yolu bulunur; ücretli hizmet zorunlu değildir. Ek özellik istenirse aynı haftanın başka uygulama yükünün yerine geçer.

## Başvuru kaynakları

[PostgreSQL 18](https://www.postgresql.org/docs/18/) şema/SQL için; [transaction isolation](https://www.postgresql.org/docs/18/transaction-iso.html) ve [WAL](https://www.postgresql.org/docs/18/wal.html) M5 için; [Database System Concepts](https://www.db-book.com/) model/cebir/FD için; [CMU Database Systems](https://15445.courses.cs.cmu.edu/fall2026/schedule.html) storage ve query processing için; [MongoDB veri modelleme](https://learn.mongodb.com/learning-paths/data-modeling-for-mongodb) alternatif model için kullanılır. Ayrıntılı seçki dersin [kaynakçasındadır](KAYNAKCA.md).
