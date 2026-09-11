# SE 237 Object Oriented Programming — Seçilmiş Güncel Kaynakça

**Son kontrol:** 10 Eylül 2026  
**Teknik taban:** Java 25 LTS. Kaynaklardaki daha yeni Java özellikleri, derste açıkça seçilmedikçe kullanılmayacaktır. Hiçbir ücretli kaynak zorunlu değildir.

[İlk hafta](HAFTA01.md) · [Proje planı](PROJE.md)

Güncel resmî belgeler teknik davranışı doğrulamak, eski ama geçerli dersler temel kavramları çalışmak içindir. Devam eden 2026 derslerinin sonraki materyalleri henüz yayımlanmamış veya önceki dönemden aktarılmış olabilir. Bağlantı erişimi, ücretli kurs içeriğinin bütünüyle incelendiği anlamına gelmez.

## Önce bunları kullanın

1. **[dev.java — Classes and Objects](https://dev.java/learn/classes-objects/)** · **[Inheritance](https://dev.java/learn/inheritance/)** · **[Interfaces](https://dev.java/learn/interfaces/)** — Java’nın resmi öğrenme portalındaki güncel, kısa ve çalıştırılabilir anlatımlardır. A1–A6 için ilk teknik başvuru kaynağıdır.
2. **[MIT 6.102 Software Construction — Spring 2026](https://web.mit.edu/6.102/www/sp26/)** — Specifications, testing, abstract data types, equality, interfaces ve concurrency’yi sağlam yazılım geliştirme bağlamında işler. 2026 sürümü TypeScript kullanır; kavramsal okumalar seçilir, Java örnekleri için dev.java/MITx 6.005 kullanılır.
3. **[Exercism Java Track](https://exercism.org/tracks/java)** — Kavramlara ayrılmış ücretsiz alıştırmalar ve geri bildirim sunar. Kısa, düzenli pratik için; çözümü göndermeden önce test yazma ve karar açıklama koşuluyla kullanılmalıdır.
4. **[JUnit 6 User Guide](https://docs.junit.org/6.1.0/overview.html)** · **[GitHub deposu](https://github.com/junit-team/junit-framework)** — Test altyapısının resmi ve güncel kaynağıdır. Her nesne sözleşmesini normal durum, sınır ve hata örneğiyle kanıtlarken kullanılır.
5. **[Java SE 25 Language Specification](https://docs.oracle.com/javase/specs/jls/se25/html/)** · **[Java 25 API](https://docs.oracle.com/en/java/javase/25/docs/api/)** — Dil davranışı veya API sözleşmesi hakkında kesin cevap gerektiğinde başvurulacak birincil kaynaklardır. Baştan sona okunacak ders kitabı değil, anlaşmazlık çözen referanslardır.

## Resmi Java öğrenme rotası

- **[Generics](https://dev.java/learn/generics/)** — Type parameter, bounded type ve generic API tasarımını açıklar. A8’de ham türlerden kaçınmak ve derleme zamanı güvencesini anlamak için kullanılır.
- **[Collections Framework](https://dev.java/learn/api/collections-framework/)** — Collection seçimini yalnız syntax değil davranış ve maliyet açısından ele alır. `List`, `Set`, `Map` seçimi dönem uygulamasındaki gerçek ihtiyaca bağlanmalıdır.
- **[Exceptions](https://dev.java/learn/exceptions/)** — Exception üretme, yakalama ve try-with-resources yapısını resmi örneklerle anlatır. A9’da hata sözleşmesi ve kaynak yaşam döngüsü için temel kaynaktır.
- **[Java Collection API contract](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/Collection.html)** — Mutability, optional operation, equality ve thread-safety gibi ayrıntıları doğrudan sözleşmeden kontrol etmeyi öğretir.
- **[Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)** — Ortak kod biçimi ve okunabilirlik için somut kurallar sunar. Tasarım doğruluğunun yerine geçmez; ekip içinde gereksiz biçim tartışmasını azaltır.

## Ücretsiz dersler ve alıştırma bankaları

- **[MITx 6.005.1x — Software Construction in Java](https://openlearninglibrary.mit.edu/courses/course-v1%3AMITx%2B6.005.1x%2B3T2016/about)** — Java ile specification, testing, abstraction ve doğru program tasarımı için ücretsiz, yapılandırılmış bir rotadır. Tarihsel sürüm olsa da temel ilkeleri güncelliğini korur.
- **[MIT OCW — Introduction to Programming in Java](https://ocw.mit.edu/courses/6-092-introduction-to-programming-in-java-january-iap-2010/)** — Java temeli zayıf öğrenciler için ücretsiz not, video ve ödev sağlar. SE 237’nin yerine değil, önkoşul açığını kapatmak için kullanılmalıdır.
- **[University of Helsinki Java Programming MOOC](https://java-programming.mooc.fi/)** — Çok sayıda otomatik kontrollü alıştırma içeren güçlü bir pratik bankasıdır. Site artık aktif biçimde güncellenmediğini belirttiği için araç kurulumu veya sürüm ayrıntılarında resmi Java belgeleri esas alınmalıdır.

## Tasarım, UML ve refactoring

- **[Refactoring.Guru — Design Patterns](https://refactoring.guru/design-patterns)** — Pattern’ları amaç, yapı ve trade-off ile görselleştirir. Bir pattern adı ezberlemek için değil, mevcut değişiklik baskısının gerçekten o çözüme ihtiyaç duyup duymadığını tartışmak için kullanılmalıdır.
- **[Java Design Patterns — GitHub](https://github.com/iluwatar/java-design-patterns)** — Çok geniş ve güncel bir Java pattern örnekleri koleksiyonudur. Başlangıçta tamamını okumak yerine Strategy, Factory ve Adapter gibi derste adı geçen örnekler seçilmelidir.
- **[PlantUML Class Diagram](https://plantuml.com/class-diagram)** — Metinden tekrar üretilebilir class diagram üretir. UML çiziminin kodla birlikte sürüm kontrolünde tutulması için uygundur.
- **[Gilded Rose Refactoring Kata](https://github.com/emilybache/GildedRose-Refactoring-Kata)** — Birçok dilde kötü tasarlanmış başlangıç kodu ve karakterizasyon testi pratiği sunar. A6 sonrası “davranışı koruyarak tasarımı değiştir” çalışması için seçilmiş Java sürümü kullanılabilir.

## Video

- **[Java — resmi YouTube kanalı](https://www.youtube.com/@java)** — Yeni Java sürümleri, dil özellikleri ve topluluk konuşmaları için birincil video kanalıdır. İleri özelliklerde sürüm numarasını kontrol ederek izlenmelidir.
- **[Object-Oriented Programming with Java — ForrestKnight](https://www.youtube.com/watch?v=TiccevwEVe8)** — Class, object, encapsulation, inheritance ve polymorphism için hızlı bir görsel tekrar sunar. Dersteki sözleşme ve tasarım tartışmasının yerine geçmez.

## İsteğe bağlı Udemy kursları

- **[Java Design Patterns](https://www.udemy.com/course/java-design-patterns/)** — Pattern’ları Java örnekleriyle sistematik biçimde çalışmak isteyenler içindir. Her örnek “hangi değişiklik problemi bunu gerektiriyor?” sorusuyla değerlendirilmelidir.
- **[Complete Java Design Patterns Masterclass](https://www.udemy.com/course/javadesignpatterns/)** — Daha geniş pattern pratiği sunar; başlangıç öğrencisinin tüm kataloğu aynı anda tüketmesi önerilmez.
- **[Clean Code with Java Examples](https://www.udemy.com/course/clean-code-java/)** — İsimlendirme, method/class boyutu ve okunabilirlik üzerinde uygulama yaptırır. Kurallar mutlak yasa değil, bakım maliyetini tartışmak için araç olarak ele alınmalıdır.
- **[Java OOP, OOAD and Design Patterns](https://www.udemy.com/course/java-object-oriented-programming-analysis-design-oops-ooad/)** — OOP ile analiz/tasarım arasındaki bağı kurmak isteyen öğrenciye bütünlüklü bir rota sağlar.
- **[Java Clean Code, Refactoring and TDD](https://www.udemy.com/course/java-clean-code-with-refactoring-and-tdd/)** — Test güvenlik ağı altında refactoring pratiği arayanlar için uygundur.

> Udemy içeriği, fiyatı ve güncellenme tarihi değişebilir. Satın almadan önce müfredat, önizleme, altyazı ve iade koşulları kontrol edilmelidir.

## Kaynakları haftalık kullanma reçetesi

- **A1–A3:** dev.java + MIT 6.102 + PlantUML.
- **A4–A6:** Interfaces/Inheritance + Refactoring.Guru + seçilmiş Java Design Patterns örneği.
- **A7–A8:** Java 25 API/JLS + Generics/Collections + Exercism.
- **A9–A10:** Exceptions + JUnit + resmi Java kanalı ve API belgeleri.
- Her örnekte öğrenci şunları göstermelidir: **sorumluluk**, **korunan invariant/sözleşme**, **test kanıtı**, **değişiklik gerekçesi**.
