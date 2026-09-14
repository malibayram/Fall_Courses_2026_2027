# SE 237 — Modern Java ile Nesne Tasarımı Kapsamı

**Dönem:** 2026–2027 Güz  
**Kararlı taban:** Java 25 LTS, Maven Wrapper, JUnit 6.1  
**Ana ürün:** Factory ERP nesne ve iş davranışı katmanı  
**İlgili belgeler:** [Öğrenci proje rehberi](ERP_OGRENCI_REHBERI.md) · [Kaynakça](KAYNAKCA.md)

Bu dersin odağı framework ezberi değil, değişen iş kuralını koruyan nesne tasarımıdır. Modern Java özellikleri ancak bir tasarım kararını daha açık, tür güvenli veya sınanabilir kıldığında kullanılır. Preview özellikler Core kapsamına alınmaz.

## Öğrenme derinliği

| Kod | Beklenen kanıt |
| --- | --- |
| **T — Temel** | Kavramı küçük nesne modeli, sözleşme ve testle kurar. |
| **K — Karşılaştırmalı kullanım** | Eski ve modern iki ifadeyi aynı davranışta karşılaştırır; trade-off'u açıklar. |
| **P — Panorama** | Runtime/framework özelliğinin mimarideki yerini, sınırını ve riskini açıklar. |

## Kapsam haritası

| Tasarım problemi | Çekirdek kavram | Modern Java bağlantısı | Düzey |
| --- | --- | --- | ---: |
| Sorumluluk ve sınır | Class/object, cohesion, coupling, package, access | Package/module sınırı; ports-and-adapters; framework'ten bağımsız domain | T/K |
| Geçerli durum | Encapsulation, invariant, pre/postcondition | Compact record constructor; fail-fast doğrulama; null/Optional sözleşmesi | T/K |
| İlişki ve sahiplik | Association, aggregation, composition, cardinality | Immutable snapshot ve defensive copy; domain event referansı | T/K |
| Değişen tür davranışı | Inheritance, overriding, LSP/substitutability | Sealed hierarchy ve exhaustive pattern-matching karşılaştırması | T/K |
| Bağımlılık sınırı | Interface, abstract type, dependency inversion | Functional interface, lambda; constructor injection; fake/contract test | T/K |
| Davranış seçimi | Polymorphism, dynamic dispatch, composition, Strategy | Lambda/method reference; pattern switch'in dispatch yerine ne zaman uygun olduğu | T/K |
| Değer ve kimlik | Identity, equality, `hashCode`, alias, copy, immutability | Record; value object; collection elemanlarının gerçek immutability'si | T/K |
| Tür güvenli veri işleme | Generics, variance, bounds, List/Set/Map | Streams/collectors, immutable collection factories; paralel stream sınırı | T/K |
| Hata ve kaynak ömrü | Exception contract, try-with-resources, cleanup | Structured task/resource ownership fikri; virtual-thread I/O demosu; interruption | T/K/P |
| Runtime genişleme | Class loading, initialization, reflection, metadata | Java module system, `ServiceLoader`, allowlist ve plugin trust boundary | T/K/P |
| Değişiklik güvenliği | Unit/contract/integration test, debugging, refactoring | JUnit 6 parameterized/dynamic tests; mutation/property-based test panoraması; static analysis | T/K/P |

## Güncel köprülerin haftalara yerleşimi

| Hafta | Güncel köprü | Öğrenci kanıtı | Kapsam sınırı |
| ---: | --- | --- | --- |
| 2 | Package ve mimari sınır | Domain'in HTTP/SQL sınıflarını bilmediğini dependency grafında gösterir | Framework kurulumu öğrenci işi değildir. |
| 3 | Functional interface ve contract test | Aynı policy için iki implementation/lambda ortak testten geçer | Lambda her interface'in yerine kullanılmaz. |
| 4 | Record/immutable snapshot ve module/plugin panoraması | Mutable sınıf–record seçimi ve provider trust notu | Record otomatik deep immutability sağlamaz. |
| 5 | Package-by-feature ve ports/adapters | Değişiklikte etkilenen sınıf/test sayısı | “Katman sayısı arttı” kalite kanıtı değildir. |
| 6 | Null ve Optional sözleşmesi | Geçersiz/yok/boş sonuçların ayrı API kararı | `Optional` alan/parametre olarak otomatik tercih değildir. |
| 7 | Immutable ilişki snapshot'ı | Kaynak nesne değişince tarihsel görünümün korunması | Kopyalama gerçek sahipliğin yerine geçmez. |
| 8 | Sealed types ve pattern matching | Polymorphic dispatch ile exhaustive pattern switch'i aynı vaka üzerinde karşılaştırır | Preview pattern özellikleri kullanılmaz. |
| 9 | Explicit dependency injection | Fake connector ile timeout/duplicate contract testi | DI framework'ü Core değildir. |
| 10 | Lambda, method reference ve Strategy | Stateful class ile saf policy lambda'sının uygunluk farkı | Büyük iş akışı tek lambda zincirine çevrilmez. |
| 11 | Record ve value object | `Money`/`Quantity` için equality, validation ve defensive-copy testi | Her domain entity record yapılmaz. |
| 12 | Streams/collectors ve generics | Loop ve stream çözümünde okunabilirlik, sıra ve duplicate davranışı | Parallel stream performansı varsayılmaz. |
| 13 | Virtual threads ve interruption panoraması | Sağlanan blocking-I/O demosunda task/resource yaşam çizgisi | Virtual thread paylaşılan mutable state'i güvenli yapmaz. |
| 14 | Module, `ServiceLoader` ve reflection güveni | Provider yok/çift/izinli olmayan sınıf durumları | Plugin discovery güvenlik sandbox'ı değildir. |

## Öğrenci verimini artıran kapsam kararları

- Öğrenci aynı hafta domain kuralı, web framework'ü, ORM ve dağıtım altyapısını birlikte kurmaz. Öğretim hedefi olmayan adaptörler sağlanır.
- Pattern adı kullanmak puan getirmez. Değişiklik baskısı, korunan invariant, daha basit alternatif ve test etkisi açıklanır.
- Kalıtım zorunlu bir tasarım hedefi değildir; composition ve sealed hierarchy alternatifleri aynı sözleşmede karşılaştırılır.
- Unit test sayısı yerine sınır sınıfları ölçülür: normal, boundary, invalid, duplicate/retry ve state-preservation.
- Modern syntax okunabilirliği artırmıyorsa klasik açık çözüm kabul edilir. Preview feature ve vendor framework bağımlılığı zorunlu tutulmaz.

## Asgari dönem sonu yeterliği

Öğrenci:

- sorumluluk, invariant, sahiplik, kimlik/değer ve bağımlılık sınırlarını çalışan modelde gösterir;
- inheritance/subtyping ile composition/delegation arasından sözleşmeye göre seçim yapar;
- class ile record, açık hierarchy ile sealed hierarchy, loop ile stream, class Strategy ile lambda arasındaki farkı gerekçelendirir;
- generic API, exception ve resource lifecycle sözleşmesini type-safe testlerle korur;
- JUnit unit, parameterized, contract ve integration testlerinin farklı kanıt rollerini ayırır;
- module/class loading/reflection/plugin keşfinin davranış ve güvenlik sınırlarını açıklar;
- yeni bir Java özelliğini syntax olarak değil, invariant, mutability, type safety, runtime ve test etkisiyle değerlendirir.

## Birincil güncel teknik dayanak

- [Oracle Java SE support roadmap](https://www.oracle.com/java/technologies/java-se-support-roadmap.html)
- [Java SE 25 Language Specification](https://docs.oracle.com/javase/specs/jls/se25/html/)
- [dev.java records](https://dev.java/learn/records/)
- [dev.java pattern matching](https://dev.java/learn/pattern-matching/)
- [dev.java virtual threads](https://dev.java/learn/new-features/virtual-threads/)
- [JUnit 6.1 user guide](https://docs.junit.org/6.1.0/)

