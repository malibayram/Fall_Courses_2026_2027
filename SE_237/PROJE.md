# SE 237 — Tek Derslik Prototipten Gelişen Kayıt Uygulamasına

**Üniversite:** Maltepe Üniversitesi  
> Önceki proje örneğidir. Öğrencilere verilecek güncel kapsam ve teslim sözleşmesi: [Sandalye Fabrikası ERP — SE 237 rehberi](ERP_OGRENCI_REHBERI.md). Bu dosya önceki tasarımın referansı olarak korunmuştur.

**Dönem:** 14 hafta, `1 + 3 + 10`  
**Ürün:** Course Registration  
**Belgeler:** [Ders sistemi](../README.md) · [İlk hafta akışı](HAFTA01.md) · [Kaynakça](KAYNAKCA.md)  
**Durum:** Proje tasarımı ve kabul sözleşmesidir; yeni starter ve uygulama henüz üretilmiş değildir.

## Amaç ve dönem sonundaki ürün

Öğrenci ilk hafta bir dersin kayıt listesini yönetir. Aynı kod; öğrenciler, dönemlik ders açılışları, kayıt kuralları, bildirimler, veri saklama ve çalışma zamanında seçilen bileşenlerle büyür. Her yeni özellik, o haftanın nesne yönelimli tasarım kavramını kullanmayı gerektiren somut bir değişiklik talebiyle gelir.

Dönem sonu ürününde öğrenci dersleri listeler, bir açılışa kayıt olur ve kayıtlarını görür; öğretim elemanı ders açılışı ve kapasiteyi yönetir. Uygulama duplicate/full/uygunsuz kayıtları açık sonuçlarla reddeder, birden çok kayıt politikasını destekler, veriyi dosyaya kaydeder ve yapılandırmadan bildirim bileşeni seçer. Kullanıcı arayüzü CLI'dır. Böylece tasarım, test ve davranış açıklamasına zaman ayrılır.

**Teknik taban:** Java 25 LTS, Maven Wrapper ve JUnit 6. Eğitmen patch sürümlerini tek starter'da sabitler. Domain tasarımı belirli bir web framework'üne veya veritabanına bağımlı değildir. CMPE 351 ile aynı hikâye kullanılabilir; SE 237 notu bağımsız Java tasarım/test kanıtından gelir.

## 1. hafta küçük proje: One Course Roster

Tek bir `CourseRoster` nesnesi, pozitif capacity ve bellekte öğrenci ID listesi tutar. İlk hafta öğrenci/domain/repository sınıflarının tamamı kurulmaz. Eğitmen constructor, `enroll(String studentId)`, `size()` ve `EnrollmentResult` iskeletini verir; öğrenci kural kontrolünü ve yalnız kabulde state değişimini tamamlar.

Önerilen arayüz:

```java
enum EnrollmentResult { ACCEPTED, DUPLICATE, FULL, INVALID_ID }
// CourseRoster(int capacity)
// EnrollmentResult enroll(String studentId)
// int size()
```

**Kurallar:** Capacity≤0 constructor hatasıdır. Null/boş/yalnız whitespace ID `INVALID_ID` döndürür. Geçerli ID trim edilerek karşılaştırılır, büyük/küçük harf duyarlıdır. Kontrol sırası: geçersiz ID → duplicate → full → mutation. Reddedilen her istek state'i olduğu gibi bırakır.

| Girdi / başlangıç | Sonuç | Son durum |
| --- | --- | --- |
| capacity=1, boş roster; `S100` | ACCEPTED | size=1 |
| Aynı roster; yeniden `S100` | DUPLICATE | size=1 |
| Aynı roster; `S101` | FULL | size=1 |
| Aynı roster; boş ID | INVALID_ID | size=1 |
| Yeni roster, capacity=0 | IllegalArgumentException | Nesne oluşturulamaz |

**Süre:** 30 dakika rehberli kodlama, 10–15 dakika test/açıklama. İlk üç davranış testi temel kabul kapısıdır; iki sınır testi starter desteğiyle tamamlanır. JUnit kurulumu ve test dosya yapısı önceden verilir.

**Teslim:** Küçük kod farkı, testler, tek nesne/sorumluluk çizimi ve A1–A10 haritası. Öğrenci “List türü doğru olsa da duplicate kuralı neden ayrıca gerekiyor?” sorusunu yanıtlar. Bu nesne W2'de domain katmanına taşınır, W5'te sorumluluklara ayrılır; prototip çöpe atılmaz.

## Kalıcı sözleşmeler ve hedef mimari

```text
CLI → RegistrationService → CourseOffering + EnrollmentRule
                          → Student/Course/Enrollment repository sınırları
                          → Notification
                  dosya adaptörü / bellekte adaptör / seçili provider
```

| Bileşen | Sorumluluk | Gelişim zamanı |
| --- | --- | --- |
| Student / StudentId | Öğrenci kimliği ve eşitlik | Basit W2; değer nesnesi W11 |
| Course / CourseOffering | Katalog tanımı ile dönemlik açılışı ayırma | Basit W2–W4; W5–W7 derinleşme |
| Enrollment | Student–offering ilişkisinin kimliği ve durumu | W5–W7 |
| RegistrationService | İşlem akışını koordine etme | W2 iskelet, W3–W4 çalışan akış |
| EnrollmentRule | Ek uygunluk kararını sağlama | W8 alt tür sözleşmesi, W10 composition |
| Repository | Kimlikle erişim ve saklama sınırı | W9 sözleşme, W12 generics |
| Notification | Kabul edilen kaydı raporlama | W9 interface, W14 provider seçimi |
| File adapter | Serileştirme, kaynak kapama ve hata raporu | W13 |

Hedef ağaç starter'da giderek oluşur: `src/main/java/.../{cli,domain,application,ports,adapters}`, `src/test/java/...`, `fixtures/`, `docs/`. W1'de bu klasörlerin tamamını doldurmak gerekmez.

Ürün boyunca dört güvence korunur: `size ≤ capacity`; aynı student/offering için en fazla bir aktif kayıt; ret durumunda state değişmemesi; dışarı verilen koleksiyon üzerinden iç state'in değiştirilememesi. Politika değişimi kapasite ve tekillik gibi sert kuralları devre dışı bırakamaz. Bu Java ürünü başlangıçta tek iş parçacıklıdır; thread-safe olduğu iddia edilmez.

## Hafta hafta ürün gelişimi

### W1 — Panorama ve roster prototipi

**Artım:** Yukarıdaki `enroll/size` davranışı. **Kavram:** A1–A10 panorama; küçük uygulamada A1/A2/A7/A8 görünür. **Kanıt:** ACCEPTED/DUPLICATE/FULL ve değişmeyen state; ileri kavramlar preview etiketi taşır. **Sonraki bağ:** Roster W2 iskeletine alınır.

### W2 — Bütün sistemin yapısı ve iskelet

**Artım:** Verilen CLI, service ve bellekte saklama parçalarını bağla; bir sabit kayıt senaryosu uçtan uca çalışsın. Bu hafta A1–A3'e (nesne modeli, encapsulation/invariant, ilişkiler/UML) odaklanılır; A4–A10 haritada bekleyen olarak işaretlenir, yeniden anlatılmaz.

**Kanıt:** `./mvnw test`, CLI smoke testi, class/object diagram farkı, sahiplik ve değişiklik noktaları, on artımlık backlog. İleri parçalar stub diye işaretlenir. **Regresyon:** W1 testleri. **Sonraki bağ:** Gerçek komut girdisi.

### W3 — Bütün sistemin davranışı ve hata

**Artım:** Verilen komut ayrıştırıcıyla `enroll` isteğini service ve roster üzerinden gerçek state değişimine bağla. Bilinmeyen öğrenci/açılış ve hatalı komut kontrollü reddedilir. Bu hafta A4–A6'ya (kalıtım/yerine kullanılabilirlik, interface/sözleşme, polimorfizm/composition) odaklanılır; A1–A3 kısaca geri çağrılır, A7–A10 bekleyen kalır.

**Kanıt:** Mutlu yol sequence diagram'ı, iki ret izi, en az üç deterministik test; çağrı öncesi/sonrası state. **Regresyon:** Capacity ve duplicate. **Sonraki bağ:** Listeleme ve hata çıktısının birlikte sürümleştirilmesi.

### W4 — Entegrasyon ve `v0.1`

**Artım:** Öğrenci/katalog/açılış ekleme, kayıt ve listelemeyi aynı CLI oturumunda birleştir. Bu hafta A7–A10'a (kimlik/eşitlik/kopyalama, generics/collections, exception/kaynak, runtime metadata/plugin) odaklanılır; A1–A6 kısaca geri çağrılır ve ikinci tur bu haftayla tamamlanır. On çapanın implementasyon/gelecek ayrımı haritada güncellenir.

**Kanıt:** Temiz build, kabul/duplicate/full/bilinmeyen kimlik, tutarlı çıktı, regression suite, üç tasarım kararı. Persistence ve plugin henüz vaat edilmez. **Sonraki bağ:** Gereksinim büyüdüğünde sınıf sınırları değişecek.

### W5 — A1: Ayrıştırma ve nesne modeli

**Değişiklik talebi:** Aynı katalog dersi birden fazla dönemde açılabilsin. **Artım:** Course ile CourseOffering'i ayır; Enrollment ilişkisinin kimliğini görünür yap. Gereksiz `main` sorumluluklarını service/domain'e taşı. **Geri çağır:** A2 invariant, A3 ilişki.

**Kanıt:** Aynı course'un iki offering'inde bağımsız kontenjan; önceki davranış testlerinin korunması; her sınıf için “ne bilir/ne yapar/ne zaman değişir?”. **Sonraki bağ:** Kapasite güncelleme kuralı.

### W6 — A2: Encapsulation, API ve invariant

**Talep:** Kapasite sonradan değişebilsin ama mevcut kayıt sayısının altına düşmesin. **Artım:** Constructor ve `changeCapacity` sözleşmesi, kontrollü mutation ve iç listenin korunması. **Geri çağır:** A1 sorumluluk, A7 referans sızıntısı.

**Kanıt:** Geçersiz değer ve sınır testleri; rejected update'te state aynı; dışarı alınan görünüm üzerinden kayıt eklenememesi. Private alanın tek başına yeterli olmadığı bir karşı örnek. **Sonraki bağ:** Birden çok ilişkiyi yöneten işlem.

### W7 — A3: İlişkiler, UML ve sahiplik

**Talep:** Kayıt iptal edilsin ve bir öğrenci birden çok offering'e katılabilsin. **Artım:** Enrollment sahipliğini belirle; iptal bir yerde yapılır, iki ayrı tutarsız liste oluşmaz. **Geri çağır:** A2 invariant, A8 koleksiyon.

**Kanıt:** UML cardinality ile gerçek nesne ağı eşleşir; iptal sonrası kontenjan açılır; diğer offering etkilenmez; ikinci iptalin sonucu sözleşmede belirtilir. Association/aggregation/composition farkı nesne yaşam döngüsü üzerinden açıklanır. **Sonraki bağ:** Değişen uygunluk kuralları.

### W8 — A4: Kalıtım, overriding ve substitutability

**Talep:** Önkoşul isteyen ve istemeyen kayıt kuralları aynı istemciden çağrılsın. **Artım:** Sınırlı `EnrollmentRule` soyut sınıfı ve iki alt tür; `evaluate(context)` karar döndürür ve kayıt state'ini değiştirmez. **Geri çağır:** A2 sözleşme, A3 collaborator.

**Kanıt:** Ortak contract testleri her alt türe uygulanır; method overriding ile overloading ayrılır. Mutasyon yapan veya beklenmedik önkoşul ekleyen alt tür testte reddedilir. Constructor zinciri ve `super` küçük örnekle incelenir. **Sonraki bağ:** Benzer sözleşmeler interface ile dış bağımlılıklara uygulanır.

### W9 — A5: Interface, soyut tür ve entegrasyon

**Talep:** Kayıt sonrası bildirim gönderilsin, testte gerçek dış servis gerekmesin. **Artım:** `Notification` interface'i, console ve test double; bellekte repository için açık port. **Geri çağır:** A4 substitutability, A9 hata sınırı.

**Kanıt:** Caller concrete sınıfı bilmeden çalışır; bildirim domain state'ini değiştirmez. Bildirim başarısızsa kayıt kabulü geri alınmaz, “kayıt başarılı/bildirim başarısız” ayrı raporlanır. Ortak contract ve mock yerine basit fake testleri. **Entegrasyon kapısı:** W1–W9 birlikte çalışır, UML kodla uyuşur. **Sonraki bağ:** Politikaları birleştirme.

### W10 — A6: Polimorfizm, composition ve Strategy

**Talep:** Önkoşul ve program uygunluğu birlikte uygulanabilsin. **Artım:** Service'in kullanacağı kural collaborator'ını dışarıdan ver; kuralları küçük bir composite ile birleştir. Her yeni koşul için service `if` zincirini büyütme. **Geri çağır:** A5 bağımlılık sınırı, A2 sert invariant.

**Kanıt:** İki policy seçimi aynı kayıt akışında farklı uygunluk sonucu üretir; hiçbir seçim capacity/duplicate'i aşamaz. İki tasarımın değişen dosya ve test sayısını karşılaştır. Dynamic dispatch izi ve basit lambda/functional interface alternatifi okunur. **Sonraki bağ:** Karar girdisini güvenle taşıyan immutable snapshot.

### W11 — A7: Kimlik, eşitlik, kopyalama ve immutability

**Talep:** Eski kayıt raporu daha sonra yapılan değişikliklerle değişmesin. **Artım:** `StudentId` gibi immutable değer nesnesi ve immutable enrollment snapshot oluştur; eşitlik/hashCode politikasını belirle. **Geri çağır:** A2 dışarı sızıntı, A3 sahiplik.

**Kanıt:** Eş kimlikler eş hash üretir; aynı nesne referansı ile eş değer ayrılır; source mutation sonrası snapshot değişmez. Unmodifiable liste ile immutable elemanların farklı güvenceler olduğu, shallow/deep copy farkı test edilir. **Sonraki bağ:** Güvenilir `Set/Map` anahtarları.

### W12 — A8: Generics ve collections

**Talep:** Farklı domain türleri için repository tekrarını azalt ve kimlikle arama ekle. **Artım:** Var olan portu `Repository<ID,T>` gibi typed yapıya dönüştür; `Map` ile kimlik indeksi, `List/Set` için sıralama/tekillik kararını uygula. **Geri çağır:** A7 equality, A5 interface.

**Kanıt:** Yanlış tür derleme kontrolünde reddedilir; iteration sırası gerektiren test açıkça sıralanır. Raw type/unchecked cast temizlenir; bounded type ve wildcard/PECS küçük kod okuma örneğiyle açıklanır. Generic yapı domain kısıtlarının yerini almaz. **Sonraki bağ:** Aynı portun dosya adaptörü.

### W13 — A9: Exception, yaşam döngüsü ve kaynaklar

**Talep:** Uygulama kapanıp açıldığında kayıtlar geri yüklenebilsin. **Artım:** Verilen format/parser iskeletiyle file adapter; try-with-resources; parse/I/O hatasının uygun katmana çevrilmesi. Yükleme önce geçici modele yapılır, doğrulanınca aktif model değiştirilir. **Geri çağır:** A5 port, A7 snapshot.

**Kanıt:** Save/load round trip aynı mantıksal state'i verir; bozuk veri aktif state'i kısmen değiştirmez; I/O hatası başarı diye raporlanmaz. Kaynağın kapanması test double ile gözlenir; GC dosya kapatma garantisi sayılmaz. Memory başarısı ve kalıcı yazma ayrı durumdur; crash durability ayrıca uygulanıp test edilmedikçe vaat edilmez. **Sonraki bağ:** Adaptör seçimini yapılandırmaya taşıma.

### W14 — A10: Runtime metadata, class loading ve `v1.0`

**Talep:** Kaynak akışı değiştirilmeden onaylı bildirim sağlayıcısı seçilebilsin. **Artım:** Eğitmenin verdiği iki küçük provider paketini `ServiceLoader` ile bul, yapılandırılmış kimlikten birini seç; seçilen sınıfı raporla. Classpath/service metadata iskeleti hazır verilir. **Geri çağır:** A5 sözleşme, A6 dispatch.

**Kanıt:** Provider var/yok/çift kimlik durumları; bilinmeyen seçim açık hata; kayıt testleri her provider ile korunur. Class loading, initialization, reflection ve nesne oluşturma farkı çağrı iziyle açıklanır. ServiceLoader güvenlik sandbox'ı değildir; yalnız dersin sağladığı provider'lar kullanılır. **Final:** W13 persistence dâhil birikimli senaryo ve mimari savunma.

## Kavram kapsamı ve öğrenme kanıtı

| Kavram kümesi | Hafta / ürün içindeki yeri |
| --- | --- |
| Class/object, constructor, alan/method, erişim, static/instance | W1–W6; roster ve domain ayrıştırması |
| Encapsulation, invariant, pre/postcondition, exception sözleşmesi | W1, W6, W9, W13; ret ve değişmeyen state |
| UML class/object/sequence, cardinality, sahiplik | W2–W7; offering/enrollment |
| Inheritance, abstract class, overriding/overloading, substitutability | W8; kural alt türleri ve ortak test |
| Interface, dependency boundary, test double | W9; bildirim ve repository |
| Polymorphism, dynamic dispatch, composition, Strategy | W10; değişen kurallar |
| Reference/alias, equals/hashCode, identity/value, copy, immutability | W11; ID ve snapshot |
| Generics, bounds/wildcards, List/Set/Map, iteration | W12; typed repository ve query |
| Exception propagation, resource lifecycle, try-with-resources, GC sınırı | W13; dosya adaptörü |
| Metadata/reflection, loading/initialization, plugin discovery | W14; provider seçimi |
| Test, debugging, refactoring, regresyon ve tasarım gerekçesi | Her hafta aynı davranış testlerinin büyümesi |

Her konsept ilgili artımda uygulanır veya açıkça etiketli küçük karşı örnek/kod okuma kanıtıyla gösterilir. Salt kavram adını rapora yazmak kapsama sayılmaz.

## Kabul, test ve teslim düzeni

**Sürüm kapıları:** W1 prototip; W4 `v0.1`; W9 ara entegrasyon; W14 `v1.0`. Haftalık paket önceki sürümden başlar ve yeni davranış, eski regresyon, UML farkı, kısa karar, bilinen sınırlama ve AI katkısını içerir. Ağırlıklar [ana README](../README.md) ile aynıdır; yeni not kalemi eklenmez.

**Test katmanları:** Domain unit testleri kural/state'i; contract testleri alternatif implementation'ları; CLI integration testleri kullanıcı sonucunu; file testleri geçici dizinde save/load'u kontrol eder. Duvar saatine veya rastgele ID'ye bağımlı testler yerine verilen saat/ID üretici kullanılır. Negatif test yalnız hata mesajını değil, state'in korunmasını da denetler.

**Son kabul senaryosu:** İki dönemlik offering oluştur → bir öğrenciyi kaydet → duplicate/full reddini göster → politika değiştir → snapshot al → dosyaya kaydet/yükle → başka notification provider ile yeniden çalıştır. Bu akışta eski kuralların hâlâ korunduğu kanıtlanır. Öğrenci kendisine verilen küçük bir değişiklik talebinin hangi sınıf ve testleri etkileyeceğini savunur.

## İskele ve iş yükü

Eğitmen W1 build/test iskeletini; W2 CLI ayrıştırıcısını; W8 ortak contract-test örneğini; W13 dosya formatını; W14 provider paketlemesini verir. Böylece her hafta asıl tasarım kararı öğrenciye kalır. W4/W9 kurtarma tabanı, önceki eksiğin sonraki konuları bloke etmesini önler.

Haftalık Core yaklaşık üç saat ders dışı çalışmaya sığar; rapor ve test bu sürenin içindedir. İsteğe bağlı GUI, HTTP API, gerçek e-posta, veritabanı adaptörü veya concurrency bu hedefe ek zorunluluk değildir. Dönem sonu proje, birikmiş haftalık artımların bütünleşik sürümüdür.

## Teknik dayanaklar

- [dev.java: classes/objects](https://dev.java/learn/classes-objects/), [inheritance](https://dev.java/learn/inheritance/), [interfaces](https://dev.java/learn/interfaces/): Dil mekanizması için birincil öğrenme kaynakları.
- [Java 25 API](https://docs.oracle.com/en/java/javase/25/docs/api/) ve [JLS](https://docs.oracle.com/javase/specs/jls/se25/html/): Equality, generics ve loading/initialization ayrıntıları için sürümlü referans.
- [ServiceLoader](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/ServiceLoader.html): W14 sağlayıcı keşfi ve yapılandırması.
- [JUnit 6 kılavuzu](https://docs.junit.org/6.1.0/overview.html): Test altyapısı; örnekler starter'ın sabit sürümüyle doğrulanır.
- [MIT 6.102, Spring 2026](https://web.mit.edu/6.102/www/sp26/): Specification, invariant ve soyutlama için kavramsal destek; örnek dili TypeScript'tir.
