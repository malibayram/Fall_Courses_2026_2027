# CMPE 351 Database Systems — Seçilmiş Güncel Kaynakça

**Son kontrol:** 10 Eylül 2026  
**Teknik omurga:** PostgreSQL 18 ve SQL. Modern veri platformları, çekirdek ilişkisel ilkelerin yerine değil, iş yüküne göre verilen tasarım kararları olarak ele alınır. Hiçbir ücretli kaynak zorunlu değildir.

[İlk hafta](HAFTA01.md) · [Proje planı](PROJE.md)

Güncel resmî belgeler teknik davranışı doğrulamak, eski ama geçerli dersler temel kavramları çalışmak içindir. Devam eden 2026 derslerinin sonraki materyalleri henüz yayımlanmamış veya önceki dönemden aktarılmış olabilir. Bağlantı erişimi, ücretli kurs içeriğinin bütünüyle incelendiği anlamına gelmez.

## Önce bunları kullanın

1. **[PostgreSQL 18 Documentation](https://www.postgresql.org/docs/18/)** · **[tek PDF](https://www.postgresql.org/files/documentation/pdf/18/postgresql-18-US.pdf)** — SQL, veri tipleri, constraint, transaction, index, query plan, JSONB ve yönetim için dersin birincil teknik referansıdır. Blog cevabı ile resmi davranış çelişirse bu belge esas alınır.
2. **[CMU 15-445/645 Database Systems — Fall 2026](https://15445.courses.cs.cmu.edu/fall2026/schedule.html)** · **[CMU Database Group dersleri](https://db.cs.cmu.edu/courses/)** — Storage, index, transaction, concurrency, recovery ve query execution’ı ders notu, video ve proje ile birleştiren güçlü bir açık ders rotasıdır. Özellikle M5–M7 için seçilmiş içerikler kullanılacaktır.
3. **[Database System Concepts, 7th Edition](https://www.db-book.com/)** — İlişkisel modelden transaction, query processing ve distributed databases’e kadar sistematik bir akademik omurga sunar. Ders notuna alternatif açıklama ve bölüm sonu soru kaynağıdır.
4. **[PGExercises](https://pgexercises.com/)** · **[GitHub deposu](https://github.com/AlisdairO/pgexercises)** — Basit `SELECT`’ten join, aggregation, window function ve recursive query’ye kadar tarayıcıda PostgreSQL pratiği sunar. M3 için ana alıştırma bankasıdır.
5. **[SQLBolt](https://sqlbolt.com/)** — Kısa, etkileşimli SQL dersleriyle ilk sorgu pratiğini hızlandırır. PostgreSQL’e özgü davranış gereken noktada sonuç resmi PostgreSQL belgesiyle kontrol edilmelidir.
6. **[DB Fiddle — PostgreSQL](https://dbfiddle.dev/postgres)** — Kurulum gerektirmeden küçük şema ve sorguları paylaşılabilir biçimde çalıştırır. Hızlı karşı örnek, constraint deneyi ve sorgu tartışması için uygundur; kalıcı laboratuvar ortamının yerine geçmez.

## PostgreSQL’i doğru anlamak

- **[Concurrency Control](https://www.postgresql.org/docs/18/mvcc.html)** — MVCC, isolation, explicit locking ve veri tutarlılığının resmi açıklamasıdır. M5’te iki oturumlu deneylerden önce ve sonra seçilmiş bölümler okunmalıdır.
- **[Transaction Processing internals](https://www.postgresql.org/docs/18/transactions.html)** — Transaction kimlikleri, kilitler ve alt transaction'ları açıklar; kurtarma için ayrıca [Reliability and WAL](https://www.postgresql.org/docs/18/wal.html) okunmalıdır. “Commit oldu” iddiasının hangi katmanlarda ne anlama geldiğini tartışmak için kullanılır.
- **[Use The Index, Luke!](https://use-the-index-luke.com/)** — SQL indeksleme ve sorgu performansını erişim yolu mantığıyla açıklar. “Index her sorguyu hızlandırır” yanılgısını plan ve ölçümle sınamak için en faydalı ikincil kaynaklardan biridir.
- **[POSETTE 2026](https://www.postgresql.org/about/event/posette-an-event-for-postgres-2026-2569/)** · **[PGConf.EU 2025 kayıtları](https://www.postgresql.eu/events/pgconfeu2025/news/all-pgconfeu-2025-recordings-are-now-online-195/)** — PostgreSQL topluluğunun güncel teknik konuşmalarını sunar. Proje konusu arayan veya üretim deneyimlerini görmek isteyen öğrenciler seçerek izleyebilir.
- **[PGSimCity](https://github.com/NikolayS/pgsimcity)** — PostgreSQL davranışını şehir benzetmesiyle görünür kılan deneysel bir görselleştirme projesidir. İleri düzey gözlem için ilham vericidir; resmi belge veya ölçümün yerine kanıt sayılmaz.

## Modern veri sistemleri ve 8. modül

- **[pgvector — GitHub](https://github.com/pgvector/pgvector)** — PostgreSQL içinde exact/approximate vector search ve indeks seçeneklerini sağlayan açık kaynak uzantıdır. M8’de anlam benzerliğinin aday ürettiği, doğruluk garantisi vermediği deneylerle gösterilir.
- **[Supabase Documentation](https://supabase.com/docs)** · **[Realtime Authorization](https://supabase.com/docs/guides/realtime/authorization)** — PostgreSQL tabanlı auth, row-level security, realtime ve API katmanlarını belgeler. Kolay istemci erişiminin yetki modelini ortadan kaldırmadığını göstermek için kullanılır.
- **[MongoDB University](https://learn.mongodb.com/)** · **[Data Modeling learning path](https://learn.mongodb.com/learning-paths/data-modeling-for-mongodb)** — Document modelinde embed/reference kararlarını ve erişim desenine göre modellemeyi öğretir. M8’de ilişkisel modele rakip slogan olarak değil, farklı trade-off’ları olan bir tasarım seçeneği olarak incelenir.
- **[Cloud Firestore Security Rules](https://firebase.google.com/docs/firestore/security/get-started)** · **[Local Emulator Suite ile kural testi](https://firebase.google.com/docs/firestore/security/test-rules-emulator)** — İstemciye yakın veri erişiminde güvenlik kurallarını ve yerel test yaklaşımını gösterir. Realtime demoyu güvenlik kanıtıyla birlikte tasarlamak için yararlıdır.
- **[Designing Data-Intensive Applications](https://dataintensive.net/)** — Reliability, scalability, maintainability, replication, partitioning ve consistency trade-off’larını sistemler arasında karşılaştırır. M7–M8 için güçlü bir ileri okuma kaynağıdır; kitap ücretlidir, resmi site kapsamı ve örnek bölümleri tanıtır.

## Modelleme ve SQL pratiği için kullanım biçimi

- ER çizimi tek başına teslim değildir: her ilişki bir gereksinime, her constraint bir geçersiz duruma bağlanmalıdır.
- SQLBolt ilk tekrar; PGExercises kontrollü pratik; yerel PostgreSQL ise constraint, transaction ve `EXPLAIN (ANALYZE, BUFFERS)` kanıtı için kullanılır.
- DB Fiddle’a gerçek kişisel veri veya gizli anahtar konmaz. Paylaşılan örnekler küçük, sentetik ve anonim tutulur.
- Bir performans iddiası sorgu sonucu, plan, tekrar sayısı, veri boyutu ve sınırlamalar olmadan kabul edilmez.

## Video ve topluluk kanalları

- **[CMU Database Group — YouTube](https://www.youtube.com/@CMUDatabaseGroup)** — Andy Pavlo ve ekibinin database internals dersleri, seminerleri ve güncel sistem konuşmalarını içerir. M5–M7’de ders takvimindeki ilgili videolar seçilmelidir.
- **[PostgreSQL Europe konferans kayıtları](https://www.postgresql.eu/events/pgconfeu2025/news/all-pgconfeu-2025-recordings-are-now-online-195/)** — Düzenleyicinin kayıt bağlantıları üzerinden sürümü ve konuşmacısı belirli teknik videolara ulaşılır.

## İsteğe bağlı Udemy kursları

- **[SQL and PostgreSQL: The Complete Developer’s Guide](https://www.udemy.com/course/sql-and-postgresql/)** — SQL’den PostgreSQL uygulamasına uzanan bütünlüklü bir rota sunar. Özellikle sorgu pratiğini video üzerinden sürdürmek isteyenler içindir.
- **[Learn SQL Using PostgreSQL](https://www.udemy.com/course/database-postgresql/)** — Başlangıç ve orta düzey PostgreSQL çalışmaları sağlar; resmi doküman ve ders laboratuvarıyla birlikte kullanılmalıdır.
- **[Hands-On Introduction to SQL with PostgreSQL](https://www.udemy.com/course/hands-on-introduction-to-sql-with-postgresql/)** — Uygulama ağırlıklı başlangıç isteyen öğrenciler için kısa yol sunar.
- **[SQL Database Design A–Z](https://www.udemy.com/course/sqldatabases/)** — Modelleme, normalizasyon ve tasarım pratiğine odaklanır. Tasarım kararları Campus Learning Hub gereksinimleriyle yeniden sınanmalıdır.

> Udemy içeriği, fiyatı ve güncellenme tarihi değişebilir. Satın almadan önce müfredat, kullanılan PostgreSQL sürümü, önizleme, altyazı ve iade koşulları kontrol edilmelidir.

## 8 modüle göre kaynak rotası

| Modül | Öncelikli kaynaklar |
| --- | --- |
| M1 — DBMS ve iş yükü | PostgreSQL giriş bölümleri, CMU ilk dersler |
| M2 — Model ve şema | PostgreSQL DDL/constraint belgeleri, modelleme alıştırmaları |
| M3 — Relational algebra ve SQL | PGExercises, SQLBolt, PostgreSQL query belgeleri |
| M4 — FD ve normalization | Database System Concepts ilişkisel tasarım bölümü, Campus Learning Hub anomali deneyleri |
| M5 — Transaction/concurrency/recovery | PostgreSQL MVCC ve transaction internals, iki oturumlu deney |
| M6 — Storage/index/query processing | CMU, Use The Index Luke, `EXPLAIN` belgeleri |
| M7 — Distributed/cloud/resilience | DDIA, CMU ve güncel konferans konuşmaları |
| M8 — Document/realtime/vector | MongoDB University, Supabase, Firestore emulator, pgvector |
