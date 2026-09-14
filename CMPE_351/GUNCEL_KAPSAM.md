# CMPE 351 — Çekirdek ve Güncel Veri Sistemleri Kapsamı

**Dönem:** 2026–2027 Güz · 12 hafta  
**Kararlı laboratuvar tabanı:** PostgreSQL 18  
**Ana ürün:** Factory ERP ilişkisel veri omurgası  
**İlgili belgeler:** [Ders modeli](README.md) · [Öğrenci proje rehberi](ERP_OGRENCI_REHBERI.md) · [Kaynakça](KAYNAKCA.md)

Bu belge ilişkisel veri tabanı çekirdeğini güncel veri platformu başlıklarıyla birleştirir. Amaç çok sayıda ürünü yüzeysel kurmak değil; her yeni yaklaşımı **veri modeli, sorgu, transaction garantisi, erişim, gecikme, kurtarma ve işletim maliyeti** üzerinden karşılaştırmaktır.

PostgreSQL 18 dönem boyunca sabit ve desteklenen tabandır. PostgreSQL 19, dönem başlangıcında geliştirme/beta durumunda olduğundan Core davranış veya sınav sözleşmesi yapılmaz; kararlı yayımlanırsa yalnız sürüm farkı okuması olarak kullanılabilir.

## Öğrenme derinliği

| Kod | Beklenen kanıt |
| --- | --- |
| **T — Temel** | Modeli/cebirsel mekanizmayı açıklar, SQL veya kontrollü iki-oturum deneyiyle kurar. |
| **K — Karşılaştırmalı laboratuvar** | Aynı iş yükünde iki tasarımı doğruluk, plan/ölçüm ve maliyetle karşılaştırır. |
| **P — Panorama** | Dağıtık/modern yaklaşımın garanti, veri yerleşimi ve operasyon sınırını karar matrisiyle açıklar. |

## Kapsam haritası

| Problem ailesi | Çekirdek | Güncel bağlantı | Düzey |
| --- | --- | --- | ---: |
| Modelleme ve bütünlük | ER/EER, ilişkisel şema, key/FK/CHECK, normalizasyon | Schema migration, data contract, temporal/history modeli, tenant sınırı | T/K |
| Sorgulama | Relational algebra, join/subquery/aggregate, NULL | Recursive CTE, window, `MERGE ... RETURNING`, SQL/JSON ve `jsonpath` | T/K |
| Transaction | ACID, serializability, MVCC, lock, deadlock, WAL | Idempotency, bounded retry, outbox/inbox ve transaction boundary | T/K |
| Fiziksel tasarım | Page/heap/buffer, B-tree/hash, optimizer | GIN/GiST/BRIN, partial/covering; skip-scan/AIO gibi sürüm özelliklerini plan üzerinden gözleme | T/K/P |
| Dağıtım ve değişim akışı | Replication, partitioning, sharding, consistency | Logical replication/decoding, CDC, replica identity, lag ve conflict | T/K/P |
| Dayanıklılık | Backup, restore, WAL, checkpoint | PITR, RPO/RTO, restore drill ve disaster-recovery runbook | T/K |
| Güvenlik | Role/privilege, view ve transaction yetkisi | RLS, least privilege, connection-pool context reset, audit ve hassas veri sınırı | T/K |
| Esnek/doküman veri | İlişkisel–doküman model trade-off'u | PostgreSQL JSONB/SQL-JSON; document/KV/realtime karşılaştırması | K/P |
| Arama ve AI verisi | Text search ve ranking temeli | Vector exact/ANN, hybrid search, filter-before/after ve relevance evaluation | K/P |
| Analitik ve yönetişim | OLTP/OLAP, aggregate ve materialized result | Warehouse/lakehouse panoraması; lineage, quality, retention ve data ownership | K/P |

## Güncel köprülerin haftalara yerleşimi

| Hafta | Güncel köprü | Öğrenci kanıtı | Kapsam sınırı |
| ---: | --- | --- | --- |
| 2 | Migration ve data contract | Boş kurulum + önceki sürümden yükseltme + geçersiz satır reddi | ORM migration aracı zorunlu değildir. |
| 3 | Idempotency/outbox ve plan okuma | Duplicate request ve rollback izi; indeks öncesi/sonrası plan | Dağıtık “exactly once” vaat edilmez. |
| 4 | JSONB/RLS ile replica/restore panoraması | Normal rol negatif testi + sabit lag/restore izi | Bulut hesabı gerekmez. |
| 5 | Connection pooling ve tenant context | Yeniden kullanılan oturumda rol/tenant temizleme testi | Ölçülmemiş SLA yazılmaz. |
| 6 | Tarihsel/temporal model ve veri sözlüğü | Değişen fiyat/BOM'un eski işlemi değiştirmediği test | Tek `updated_at` tam history değildir. |
| 7 | Recursive/window SQL ve `MERGE ... RETURNING` | Beklenen sonuç kümesi + duplicate/source-row sınırı | Syntax kullanımı transaction tasarımının yerine geçmez. |
| 8 | Veri kalitesi, lineage ve import reconciliation | Kaynak–staging–çekirdek satır/toplam uzlaştırması | Normalizasyon tek başına veri kalitesi sağlamaz. |
| 9 | Idempotent transaction, retry ve outbox | Bariyerli concurrency testi + tek etkili komut/olay | Retry yalnız hata alan statement'a uygulanmaz. |
| 10 | PostgreSQL 18 planner/engine yenilikleri | `EXPLAIN (ANALYZE, BUFFERS)` ile seçilmiş indeks; AIO/skip-scan trace yorumu | Sürüme özgü özellik temel indeks teorisinin yerine geçmez. |
| 11 | Logical replication/decoding ve CDC | Primary key/replica identity, lag, order ve conflict karar kartı | Mesaj broker platformu kurmak Core değildir. |
| 12 | SQL/JSON, RLS, vector/hybrid search ve governance | Pozitif/negatif erişim; exact/ANN aday ve relevance ölçümü; retention/owner kaydı | Embedding benzerliği doğruluk veya yetki garantisi değildir. |

## Verimli dönem ilkeleri

- İlişkisel model, SQL, normalizasyon, transaction ve indeksleme daraltılmaz; modern başlıklar bu mekanizmaların gerçek iş yükündeki uzantısıdır.
- Aynı hafta yalnız bir çalışan Core artım vardır. Ürün karşılaştırmaları sağlanan fixture, trace veya emülatörle yapılır.
- `EXPLAIN ANALYZE` bir sorguyu gerçekten çalıştırır; yazma sorguları güvenli test transaction'ında ele alınır.
- RLS testi superuser/tablo sahibiyle yapılmaz. Security, filtre eklemekten ibaret değildir; role, privilege, policy ve connection context birlikte sınanır.
- Vector/JSON/realtime çözümleri özellik gösterisi değildir. Baseline, veri boyutu, filtre, doğruluk/relevance, latency ve maliyet birlikte kaydedilir.
- Backup'ın varlığı restore başarısı değildir; dönem içinde en az bir yeniden üretilebilir restore drill yapılır.

## Asgari dönem sonu yeterliği

Öğrenci:

- gereksinimi kavramsal modele, ilişkisel şemaya, bütünlük kısıtına ve migration'a dönüştürür;
- karmaşık sorgunun ilişkisel anlamını ve `NULL`/duplicate/cardinality sınırlarını açıklar;
- MVCC, lock, isolation, retry, idempotency ve WAL rollerini kontrollü concurrency deneyinde ayırır;
- indeks ve sorgu planı kararını veri/iş yükü ve yazma bedeliyle savunur;
- physical/logical replication, CDC, backup/restore ve PITR'ın farklı amaçlarını açıklar;
- RLS/privilege ile tenant sınırını normal rollerle test eder;
- ilişkisel, JSON/doküman, realtime ve vector yaklaşımını aynı ihtiyaçta garanti ve maliyet üzerinden karşılaştırır;
- yeni bir veri teknolojisini model, query, consistency, security, recovery, scale ve operations eksenlerinde sınıflandırır.

## Birincil güncel teknik dayanak

- [PostgreSQL 18 documentation](https://www.postgresql.org/docs/18/)
- [PostgreSQL 18 release notes](https://www.postgresql.org/docs/release/18.0/)
- [PostgreSQL MERGE](https://www.postgresql.org/docs/18/sql-merge.html)
- [PostgreSQL SQL/JSON and JSON types](https://www.postgresql.org/docs/18/datatype-json.html)
- [PostgreSQL row security policies](https://www.postgresql.org/docs/18/ddl-rowsecurity.html)
- [PostgreSQL logical replication](https://www.postgresql.org/docs/18/logical-replication.html)
- [PostgreSQL logical decoding](https://www.postgresql.org/docs/18/logicaldecoding.html)
- [pgvector](https://github.com/pgvector/pgvector)

