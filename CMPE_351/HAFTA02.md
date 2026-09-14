# CMPE 351 Database Systems — 2. Hafta Öğretim Dosyası

## Haftanın kimliği

| Alan | Plan |
| --- | --- |
| Tema | Veri ürününün yapısı: mimari sınır, şema bütünlüğü ve doğrulanmış SQL |
| Ana soru | 100 sandalyelik ihtiyaç sorgusunu, sürümlenmiş ve bütünlüğü korunan bir şemanın üzerine nasıl oturturuz? |
| Süre | 155 dakika ders (125 dakika etkin + üç adet 10 dakikalık ara) + 120 dakika laboratuvar (110 dakika etkin) |
| Başlangıç | W1 prototip sorgusu ve `inventory_snapshot` verisi; kaynağı belirtilmiş eğitmen tabanı da olabilir |
| Haftanın ürünü | Boş veritabanından kurulan sürümlü migration/seed, rol ayrımı, ER taslağı, doğrulanmış ihtiyaç sorgusu |
| Sonraki kapı | W3'te siparişten rezervasyona giden transaction iskeleti |
| Belge durumu | Öğretim ve görev tasarımı; starter, migration aracı, veri üretici ve web istemcisi ders paketinde ayrıca üretilecektir |

**Bağlantılar:** [Ana ders sistemi](../README.md) · [Güncel kapsam](GUNCEL_KAPSAM.md) · [ERP öğrenci rehberi](ERP_OGRENCI_REHBERI.md) · [Ders modeli](README.md) · [W1](HAFTA01.md) · [W3](HAFTA03.md) · [W4](HAFTA04.md) · [Kaynakça](KAYNAKCA.md)

Bu Türkçe dosya eğitmen ve asistanlar içindir. Güncel proje sözleşmesi [ERP rehberidir](ERP_OGRENCI_REHBERI.md); [eski Campus Learning Hub planı](PROJE.md) yalnız tarihsel referanstır ve öğrenciye yürürlükteki görev olarak verilmez.

## 1. Öğretim amacı ve doğru derinlik

W1'de fabrikanın sekiz modüllük panoramasını ve küçük bir ihtiyaç sorgusunu gördük. Bu hafta aynı bütünü **yapı açısından** kuruyoruz: hangi bileşen hangi sınırda çalışır, hangi kural şemada zorunlu kılınır, bir bilgi ihtiyacı hangi cebirsel işlemlerle doğrulanmış SQL'e dönüşür?

**Bu hafta M1–M3 ayrıntılı işlenir:** M1 DBMS mimarisi/iş yükü/garantiler, M2 kavramsal model/ilişkisel şema/bütünlük, M3 ilişkisel cebir ve SQL. M4–M8 bu hafta yeniden anlatılmaz; tablo/işlem haritasında "bekleyen" olarak işaretlenir ve W3–W4'te ele alınır. Normalizasyon tartışması, transaction protokolü, indeks kararı, replication ve JSONB/RLS bu haftanın konusu değildir.

### Ölçülebilir kazanımlar

| Kod | Öğrenci ders sonunda… | Kanıt |
| --- | --- | --- |
| W2-K1 | İstemci, oturum, bağlantı ve sorgu yaşam döngüsünü ayırır. | `pg_stat_activity` gözlemi ve iş yükü kartı |
| W2-K2 | Migration sahibi ile normal uygulama rolünü ayırır. | Uygulama rolüyle reddedilen DDL denemesi |
| W2-K3 | W1 verisini sürümlü migration ve seed'e taşır. | Boş veritabanından kurulum ve sürüm tablosu |
| W2-K4 | Tenant kapsamını anahtar ve FK ile zorunlu kılar. | `F-B` bileşenini `F-A` reçetesine bağlama reddi |
| W2-K5 | `NOT NULL`, `CHECK` ve `UNIQUE`'in farklı işleri olduğunu gösterir. | NULL ve `qty ≤ 0` için ayrı karşı örnekler |
| W2-K6 | İhtiyaç sorgusunu selection/join/aggregate olarak açıklar. | Cebir–SQL eşlemesi ve beklenen satır kümesi |
| W2-K7 | Açılış verisinin tekrar yüklenmesinde çift kayıt oluşmadığını gösterir. | Import batch kimliği ve ikinci koşunun sonucu |
| W2-K8 | M4–M8 genişlemelerini sınır ve etiketle haritaya koyar. | Tablo/işlem haritasındaki `F` satırları |

`O = observed`: bu veritabanında gerçekten çalıştırılan. `P = previewed`: eğitmenin gösterdiği veya sağlanan hazır örnek. `F = future`: ileride kurulacak. Her satır ayrıca **supplied / student** katkısını belirtir.

## 2. Kapsam ve sorumluluk paylaşımı

| Şerit | İçerik |
| --- | --- |
| Core | Sürümlü migration/seed, rol ayrımı, tenant kapsamlı anahtar/FK, bütünlük karşı örnekleri, doğrulanmış ihtiyaç sorgusu, ER taslağı, M1–M8 haritası |
| Sağlanan altyapı | PostgreSQL 18 ortamı, migration aracı, sabit fixture üretici, `v_material_requirement_per_unit` görünümü, ince web/API istemcisi, health query |
| Instructor demo | Bağlantı havuzunda taşınan rol bağlamı; tek satırlık `CHECK` ile çözülemeyen kurallar |
| Stretch | İkinci bir tenant için aynı sorgunun bağımsız sonucu; zorunlu değildir |
| Sonraya bırakılan | Transaction/rezervasyon W3; satınalma–üretim–sevkiyat ve `v0.1` W4; normalizasyon W8; indeks W10 |

Öğrenci bu hafta bağlantı havuzu, migration aracı veya web istemcisi yazmaz. Bunlar ders başlamadan çalışır durumda sağlanır. Sorgunun sonuç döndürmesi iş kuralının korunduğu anlamına gelmez; kanıt reddedilen negatif denemelerdedir.

## 3. Eğitmenin ders öncesi hazırlığı

### En az beş gün önce yayımlanacak paket

- 20–30 dakikalık Türkçe yapı videosu; aynı ölçülen içeriğin İngilizce notu.
- W1 şemasını kabul eden boş migration dizini ve çalışır sürüm tablosu.
- Sabit fabrika fixture'ı: `CHAIR-A`, `FRAME-A`, `WOOD-A`, `FABRIC-A`, `FOAM-A`, `VARNISH-A`, `R1` ve iki tenant (`F-A`, `F-B`).
- İki ayrı rol: `erp_migrator` (DDL sahibi) ve `erp_app` (yalnız DML). Uygulama rolüyle `CREATE TABLE` denemesi reddedilmelidir.
- Health query ve `pg_stat_activity` üzerinden oturum gözlemi için hazır betik.
- Açılış verisi için `import_batches` kimliği ve aynı batch'i iki kez işleme denemesi.
- Bilerek hatalı üç referans sürüm: (1) tenant'sız tekil `PRIMARY KEY(id)`, (2) yalnız `CHECK(quantity > 0)` olup `NOT NULL` olmayan sütun, (3) alt reçeteyi yanlış tenant'a bağlayabilen FK.
- `observed / previewed / future` sütunlu M1–M8 çalışma kâğıdı.

### Yayımlama öncesi öğretmen kontrolü

| Kontrol | Hazır olma koşulu |
| --- | --- |
| Ortam | PostgreSQL 18 ve `psql` temiz kopyada çalışır; ücretli bulut hesabı gerekmez. |
| Migration | Hem boş veritabanından kurulum hem W1 sürümünden yükseltme denenmiştir. |
| Fixture | 100 CHAIR-A/R1 → WOOD 4 m³, FABRIC 150 m, FOAM 100 adet, VARNISH 10 kg; eksik WOOD 1; üretilebilir 75. |
| Roller | `erp_app` ile DDL reddedilir; `erp_migrator` ile uygulanır. Şifreler ders fixture'ındadır, gerçek anahtar kullanılmaz. |
| Tenant | Composite anahtar ve FK, `F-A`/`F-B` karışmasını gerçekten engeller. |
| Karşı örnekler | Üç hatalı sürüm çalıştırılabilir ve beklenen yanlış davranışı üretir. |
| Süre | Laboratuvardaki öğrenci işi 110 etkin dakikaya sığar. |
| Veri | Tüm kayıtlar sentetiktir; gerçek müşteri/personel verisi yoktur. |

## 4. Ders öncesi öğrenci hazırlığı

Toplam hedef: video/not, seçilmiş okuma, ortam kontrolü ve altı soru için yaklaşık 50–65 dakika. Yanıtlar dersten 12 saat önce teslim edilir.

Okuma sınırı: [PostgreSQL Constraints](https://www.postgresql.org/docs/18/ddl-constraints.html); [ERP rehberinin](ERP_OGRENCI_REHBERI.md) “Mantıksal model ve veri sözlüğü” 4.1 bölümü ile “Veri tipi, anahtar ve migration kuralları”. Belgelerin tamamı bu haftanın ödevi değildir.

| Soru | Eğitmenin beklediği yön |
| --- | --- |
| 1. Bağlantı, oturum ve transaction aynı şey mi? | Hayır; bir bağlantı birçok oturum davranışı ve çok sayıda transaction taşır. |
| 2. Uygulama rolü tablo oluşturabilmeli mi? | Hayır; DDL migration sahibinindir. Yetki ayrımı kazayla şema değişimini engeller. |
| 3. `CHECK(quantity > 0)` NULL'u engeller mi? | Hayır; NULL karşılaştırması bilinmeyen döner. `NOT NULL` ayrıca gerekir. |
| 4. `PRIMARY KEY(id)` tenant izolasyonu için yeter mi? | Hayır; `PRIMARY KEY(tenant_id, id)` ve aynı tenant'ı taşıyan composite FK gerekir. |
| 5. İhtiyaç sorgusu hangi cebirsel işlemleri kullanır? | Selection, join, projection ve gruplanmış aggregate. |
| 6. Aynı açılış dosyası iki kez yüklenirse ne olmalı? | Batch kimliği tekilliği sayesinde ikinci açılış oluşmaz. |

Öğrenci W1 sorgusunu tekrar çalıştırır; başarısızsa hatayı ortam, şema, veri veya sorgu olarak sınıflandırır.

## 5. Tahta ve slayt omurgası

```text
Web/API istemcisi   [sağlanan]
  └─ bağlantı havuzu ── oturum ── transaction ── sorgu
       └─ PostgreSQL
            ├─ erp_migrator : DDL, sürümlü migration
            └─ erp_app      : yalnız DML, tenant kapsamlı erişim

İş yükleri:  üretim kiosk'u (OLTP)  |  B2B arama (OLTP)  |  gece kârlılık raporu (analitik)
```

### Bu haftanın tablo çekirdeği

Aşağıdaki küme **hazırlanacak starter'ın sözleşmesidir**; ERP rehberindeki tam modelin yalnız ilk dilimidir.

```text
tenants(id, code, name)
units(id, dimension)                       unit_conversions(from, to, factor)
products(tenant_id, id, sku, type, base_unit_id)
bom_revisions(tenant_id, id, product_id, revision, status)
bom_lines(tenant_id, revision_id, line_no, component_id, child_revision_id, qty, loss_rate)
warehouses(tenant_id, id, type)
inventory_snapshot(tenant_id, product_id, warehouse_id, on_hand)   → W4'te açılış defterine taşınır
import_batches(tenant_id, id, source, processed_at)

PRIMARY KEY(tenant_id, id)  ve aynı tenant'ı taşıyan composite FK her iş tablosunda korunur.
```

### M1–M3 yapı turu — bu haftanın odağı

| Modül | Yapı sorusu | Bu hafta gösterilecek şey | Derinleşme bağlantısı |
| --- | --- | --- | --- |
| M1 — DBMS mimarisi ve iş yükü | Bir sorgu hangi sınırlardan geçer? | İstemci/havuz/oturum/transaction zinciri; üç iş yükü; rol ayrımı | W5 iş yükü kataloğu ve süre bütçesi |
| M2 — Model, şema ve bütünlük | Geçersiz durum nasıl engellenir? | Composite tenant anahtarı; FK/`NOT NULL`/`CHECK`/`UNIQUE` ayrı işleri | W6 ER/EER ve deletion policy |
| M3 — İlişkisel cebir ve SQL | Bilgi ihtiyacı nasıl doğrulanır? | İhtiyaç sorgusunda selection/join/aggregate; beklenen satır kümesi | W7 recursive CTE ve rapor portföyü |

Her öğrenci satırın yanına O/P/F ve supplied/student etiketini ekler. “Sorgu çalıştı” ile “sonuç doğru” ayrı kanıtlardır.

### M4–M8 — bu hafta yalnız haritada bekleyen

| Modül | Nerede ele alınacak |
| --- | --- |
| M4 — Bağımlılıklar ve normalizasyon | W3 (ikinci tur), W8 (derinleşme) |
| M5 — Transaction ve eşzamanlılık | W3 (ikinci tur), W9 (derinleşme) |
| M6 — Depolama, indeks ve sorgu işleme | W3 (ikinci tur), W10 (derinleşme) |
| M7 — Dağıtık, bulut ve dayanıklılık | W4 (ikinci tur), W11 (derinleşme) |
| M8 — Doküman/realtime ve modern PostgreSQL | W4 (ikinci tur), W12 (derinleşme) |

Bu beş modül bu hafta yeniden anlatılmaz; harita satırlarında `F` kalır. Gelecek genişleme noktaları tablo/işlem haritasında yer alır fakat bu hafta uygulanmaz.

## 6. 155 dakikalık ders akışı

| Süre | İçerik ve öğretmen hamlesi | Öğrenci işi / kanıt |
| --- | --- | --- |
| 00–08 | W1 ihtiyaç sorgusunun sonucunu notsuz geri çağır; aynı sonucu web istemcisinde göster. | 4 / 150 / 100 / 10 ve eksik 1 m³ |
| 08–20 | **M1:** İstemci → havuz → oturum → transaction → sorgu zinciri; `pg_stat_activity` gözlemi. | Zincirin beş kutusu |
| 20–30 | **M1:** Üç iş yükü (kiosk, B2B arama, gece raporu) ve rol ayrımı; `erp_app` ile DDL reddi. | İş yükü kartı ve rol tablosu |
| 30–40 | **Ara** | |
| 40–55 | **M2:** Gereksinimden şemaya; `PRIMARY KEY(tenant_id, id)` ve composite FK ile tenant kapsamı. | Composite anahtar gerekçesi |
| 55–70 | **M2 karşı örnekleri:** NULL'u geçiren `CHECK`; tenant'sız FK'nin `F-B` bileşenini kabul etmesi. | İki reddedilmesi gereken satır |
| 70–80 | **Ara** | |
| 80–95 | **M3:** İhtiyaç sorgusunu cebirle eşle: selection → join → projection → gruplanmış aggregate. | Cebir–SQL eşleme tablosu |
| 95–110 | **M3:** Beklenen satır kümesi, birim tutarlılığı ve `NULL` davranışı; boş sonucun doğru cevap olabilmesi. | Beklenen/gözlenen satır karşılaştırması |
| 110–120 | **Ara** | |
| 120–135 | Sürümlü migration ve seed; boş DB'den kurulum ile önceki sürümden yükseltme; yayımlanmış migration değiştirilmez. | İki kurulum yolunun adımları |
| 135–145 | Import batch kimliği ve çift açılış koruması; M1–M8 haritasına M4–M8 sınırlarını `F` olarak ekle. | Batch tekilliği + beş `F` satırı |
| 145–150 | Kısa bireysel kontrol: bir constraint kararını ve bir sorgu adımını savun. | Bireysel açıklama |
| 150–155 | Exit ticket ve laboratuvar hedefini açıkla. | Çıkış kaydı |

## 7. 120 dakikalık laboratuvar akışı

| Süre | Uygulama | Beklenen kanıt |
| --- | --- | --- |
| 00–10 | Ortam, PostgreSQL sürümü ve iki rolün bağlantısını doğrula. | Sürüm, `erp_migrator` ve `erp_app` bağlantıları |
| 10–25 | Boş veritabanında migration'ları uygula; sürüm tablosunu ve health query'yi çalıştır. | Temiz kurulum kaydı ve sürüm numarası |
| 25–40 | W1 verisini seed'e taşı; `erp_app` rolüyle `CREATE TABLE` dene. | Başarılı seed ve reddedilen DDL |
| 40–55 | Negatif denemeler: NULL `qty`, `qty ≤ 0`, `loss_rate ≥ 1`, `F-B` bileşenli `F-A` reçetesi. | Dört ret; her birinde SQLSTATE ve değişmeyen satırlar |
| 55–70 | İhtiyaç sorgusunu çalıştır; brüt ihtiyaç, kullanılabilir ve eksik miktarı üret. | 4/150/100/10; eksik WOOD 1; üretilebilir 75 |
| 70–80 | **Ara** | |
| 80–94 | Önceki sürümden yükseltme yolunu dene; yayımlanmış migration'ı değiştirmeden yeni dosya ekle. | İki yolun da aynı şemayı vermesi |
| 94–104 | Aynı import batch'ini iki kez işlet; açılış toplamlarını karşılaştır. | Tek açılış; toplamlar değişmedi |
| 104–112 | ER taslağını çıkar ve M1–M8 haritasına O/P/F ile işle. | Kodla uyumlu ER ve güncel harita |
| 112–118 | Baseline, komutlar ve beklenen/gözlenen sonuçları kaydet. | Yeniden üretim kaydı |
| 118–120 | Teslimi kontrol et; gerçek bağlantı dizesi veya parola bulunmadığını doğrula. | Teslim kontrolü |

Laboratuvar 155 dakikalık dersin içine gizlenmez; ayrı tahsistir ve dersle aynı işi ikinci kez teslim ettirmez.

## 8. Öğretmen soruları, karşı örnekler ve müdahale

| Yanılgı/soru | Müdahale ve beklenen açıklama |
| --- | --- |
| “`CHECK(quantity > 0)` her geçersiz değeri engeller.” | NULL satırı ekletip `CHECK`'in bilinmeyen sonucunu göster; `NOT NULL` ekletir. |
| “Tek `PRIMARY KEY(id)` yeterli.” | `F-B` bileşenini `F-A` reçetesine bağlatıp sızıntıyı gösterir. |
| “Uygulama rolü her şeyi yapabilmeli.” | Kazayla düşen tablo senaryosunu ve yetki ayrımını tartıştır. |
| “Sorgu sonuç döndürdü, model doğru.” | Negatif denemeleri sor; reddedilenin de kanıt olduğunu söylettir. |
| “Boş sonuç hatadır.” | Boş sonucun bazen doğru cevap olduğunu ve testin bunu kapsaması gerektiğini göster. |
| “Migration'ı düzeltip yeniden çalıştırırım.” | Yayımlanmış migration'ın değişmezliğini ve yükseltme yolunu ayırt ettir. |
| “Snapshot canlı stok defteridir.” | Snapshot'ın W4'te açılış defterine taşınacağını ve çift açılış riskini açıkla. |

**Kısa sözlü kartları:** Bir constraint'in hangi hatayı engellediğini söyle; ihtiyaç sorgusundaki aggregate adımının neden gerekli olduğunu açıkla; iki rolün farkını bir kaza senaryosuyla anlat; aynı batch iki kez işlenirse ne olur?

## 9. Teslim, geri bildirim ve süre

Paket şunları içerir:

1. Migration sürümü, seed ve yeniden üretim komutları.
2. Boş DB kurulumu ve önceki sürümden yükseltme kanıtı.
3. Dört negatif denemenin SQLSTATE ve değişmeyen satır kanıtı.
4. İhtiyaç sorgusu, cebir eşlemesi ve beklenen/gözlenen satır kümesi.
5. ER taslağı ve M1–M8 haritası; M1–M3 için O/P kanıtı, M4–M8 için `F` ve hedef hafta.
6. Bir veri tasarım kararı (en fazla 150 kelime) ve bilinen sınırlama.
7. AI/dış katkı açıklaması.

| Ölçüt | Puan |
| --- | ---: |
| Sürümlü migration, seed ve temiz kurulum | 25 |
| Tenant kapsamlı anahtar/FK ve bütünlük karşı örnekleri (M2) | 25 |
| İhtiyaç sorgusunun cebirsel açıklaması ve doğru sonucu (M3) | 20 |
| Mimari sınır, rol ayrımı ve iş yükü haritası (M1) | 15 |
| Harita, yeniden üretim ve teknik gerekçe | 15 |
| **Toplam** | **100** |

Bu ölçek haftalık geri bildirimi yapılandırır; dersin toplam notuna yeni yüzde eklemez.

Ders dışı Core hedefi: migration/seed 45, negatif denemeler ve sorgu 40, ER/harita 35, belge/katkı 20 dakika; toplam yaklaşık 140 dakika. Hazırlık, laboratuvar ve en çok 30 dakikalık video bu toplamdan ayrıdır.

## 10. Exit ticket ve W3 köprüsü

Notlar/AI kapalı:

1. Bir constraint'in engellediği somut bir geçersiz satır yaz.
2. İhtiyaç sorgusundaki bir join ve bir aggregate adımının işini açıkla.
3. W3'te rezervasyon eklenince hangi sayının şemadan tek başına okunamayacağını tahmin et.

**W3 başlangıç cümlesi:** “Şimdi bu şemanın üzerinden gerçek bir işlem geçireceğiz: aynı malzemeyi iki sipariş isteyecek, transaction sınırını çizecek ve şemanın tek başına çözemediği yarışı göreceğiz.”

## 11. English student handout — Week 2

**Focus:** Build M1–M3 (DBMS architecture and workloads, conceptual model/schema/integrity, relational algebra and SQL) in depth on the chair-factory data product. M4–M8 stay on the map as future work for Weeks 3–4.

**Your task:** Move the Week 1 data into versioned migrations and a seed. Install from an empty database and also upgrade from the previous version without editing a published migration. Separate the migration owner role from the normal application role, and keep tenant scope in keys and foreign keys.

**Required evidence:** 100 CHAIR-A against revision R1 needs 4 m³ wood, 150 m fabric, 100 foam units and 10 kg varnish; only wood is short, by 1 m³, and 75 chairs are producible. Provide four rejected attempts: a NULL quantity, a non-positive quantity, a loss rate of 1 or more, and a component from tenant F-B attached to an F-A recipe. `CHECK(quantity > 0)` does not reject NULL on its own — `NOT NULL` is a separate requirement.

**Design output:** An ER draft matching the shipped schema, the algebra-to-SQL mapping for the requirement query with its expected row set, and an M1–M8 map marking observed, previewed and future work while separating supplied from student work. Show that reprocessing the same import batch does not create a second opening.

**Preparation questions:** Are a connection, a session and a transaction the same thing? Should the application role create tables? Does a CHECK constraint reject NULL? Is a single-column primary key enough for tenant isolation? Which algebraic operations does the requirement query use? What should happen when the same opening file is loaded twice?

**Submission:** Migration version, seed, reproduction commands, both install paths, the four negative results with SQLSTATE, the query with expected and observed rows, the ER draft and map, one data-design decision, known limitations and contribution disclosure. Never submit a real connection string or password; use the course fixtures.

**Feedback:** 25 points for versioned migration and clean install, 25 for tenant-scoped integrity, 20 for the query and its algebraic explanation, 15 for the architectural boundary and roles, and 15 for the map and reasoning. This rubric does not add a course-grade component.
