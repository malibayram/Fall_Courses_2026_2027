# SE 237 Object Oriented Programming — 1. Hafta Öğretim Dosyası

**Hafta:** Panorama — tek kayıt isteği, on tasarım sorusu  
**Süre:** 155 dakika; 125 dakika etkin öğrenme + üç adet 10 dakikalık ara  
**Dönem ürünü:** Course Registration  
**Bu dosyanın sınırı:** Yalnızca 1. haftada anlatılacak, yaptırılacak ve toplanacak işleri içerir.

**İlgili belgeler:** [Prototip ve dönem planı](PROJE.md) · [Kaynakça](KAYNAKCA.md)

Bu Türkçe belge eğitmen içindir; öğrenci yönergeleri ve ölçülen içerik için İngilizce eşdeğer hazırlanır. W1 tanılayıcıdır; teslimler geri bildirim sağlar ve yeni bir not bileşeni oluşturmaz. Kurulum aksarsa eşli çalışma ve verilen çıktı üzerinden açıklama kabul edilir; kişisel ortam daha sonra tamamlanır.

## Haftanın ana sorusu

> Çalışan bir kayıt uygulamasının anlaşılır, test edilebilir ve değiştirilebilir kalması için nesneleri ve türleri nasıl tasarlarız?

İlk hafta Java sözdizimini baştan sona öğretme veya on konuyu bitirme haftası değildir. Tek bir öğrencinin derse kayıt olma isteği üzerinden on sabit tasarım çapası görünür hâle getirilir; ardından öğrenci küçük ama çalışan bir roster davranışı üretir.

## Ders sonunda öğrencinin göstereceği kanıt

Öğrenci:

- class, object ve reference kavramlarını ayırır;
- sorumluluk ve invariant üzerinden temel encapsulation gerekçesi kurar;
- ownership/uses ilişkisini basit UML veya nesne haritasında gösterir;
- inheritance, interface ve composition’ı yalnız syntax değil sözleşme/değişim açısından konumlandırır;
- identity ile logical equality’yi ayırır;
- generic collection’ın sağladığı type safety ile domain kuralını karıştırmaz;
- exception/resource ve runtime selection başlıklarını doğru sistem noktasına yerleştirir;
- capacity ve duplicate kurallarını koruyan küçük roster’ı testlerle kanıtlar.

## Eğitmenin ders öncesi hazırlığı

- Java 25 LTS, build tool ve JUnit 6 ortamını temiz kopyada doğrula.
- Referans uygulamayı üç senaryo için hazırla: accepted, duplicate rejected, full rejected.
- İleri özellikleri gösteren preview ile öğrencinin tamamlayacağı küçük starter’ı ayrı tut.
- Starter’ı tek derste tamamlanabilecek düzeyde sınırla: bir course, öğrenci ID’leri ve capacity.
- Worksheet’e A1–A10, `implemented / previewed / future` sütunları ekle.
- UML için tahta şablonu veya PlantUML başlangıcı hazırla; çizimi görsellik yarışına dönüştürme.
- Kurulum sorunu için terminal çıktısı, ortak makine ve eşli çalışma seçeneği hazırla.

## Ders öncesi öğrenci hazırlığı — 25–35 dakika

1. dev.java’dan Classes and Objects ile Interfaces sayfalarının seçilmiş girişlerini oku.
2. Şu gereksinimi bir kez oku: “Bir dersin capacity’si aşılmayacak; aynı öğrenci iki kez kaydolamayacak; reddedilen istek state’i değiştirmeyecek.”
3. Not/AI kapalı ilk tahminlerini yaz:
   - Hangi üç şey object olabilir; hangisinin gerçekten davranışı vardır?
   - `private List` tek başına duplicate’i önler mi?
   - `==` ile `equals` her zaman aynı soruyu mu sorar?
   - Bir method derleniyorsa davranış sözleşmesini de koruyor mudur?
   - Kayıt memory’de başarılı ama dosyaya yazma başarısız olabilir mi?
4. Java sürümünü ve starter build’ini doğrula; hata mesajını değiştirmeden kaydet.

## Tahta ve slayt omurgası

```text
request → responsible objects → protected state/invariants
        → relationships/contracts → selected behavior
        → collection/equality → failure/resource → result
```

Her çapada şu sorular tekrarlanır:

1. Bu sorumluluk kime ait?
2. Hangi state geçerli kalmalı?
3. Caller hangi söze güveniyor?
4. Gelecek değişiklik en az hangi parçayı etkilemeli?
5. Bu iddiayı hangi test çürütebilir?

## 155 dakikalık ders akışı

| Süre | İçerik ve öğretmen hamlesi | Öğrenci işi / kanıt |
| --- | --- | --- |
| 00–05 | Course Registration hikâyesini ve tek isteği tanıt: “S100 son kontenjana kayıt olmak istiyor.” | İlk nesne/sorumluluk tahmini |
| 05–10 | Referans uygulamada accepted, duplicate ve full sonuçlarını göster; kodu henüz açıklama. | Request–rule–state–result izi |
| 10–16 | **A1:** Class, object, instance; noun avı yerine sorumluluk. | “Ne bilir/ne yapar?” kartı |
| 16–22 | **A2:** Encapsulation, private state ve invariant; rejected request’in state’i değiştirmemesi. | Invariant cümlesi |
| 22–30 | **A3:** Association, uses, owns; caller ile roster’ın sınırı. | Küçük object diagram |
| 30–40 | **Ara** | |
| 40–46 | A1–A3 notsuz retrieval; “Her şeyi `main` yapsa hangi değişiklikler yayılır?” | Değişiklik etkisi tahmini |
| 46–52 | **A4:** Subtype ve substitutability; aynı signature’ın doğru davranışa yetmemesi. | Sözleşme ihlali örneği |
| 52–59 | **A5:** Interface type, implementation ve davranış/failure contract. | Caller’ın bilmemesi gerekenler |
| 59–66 | **A6:** Composition ve Strategy; değişen policy’yi kararlı akıştan ayır. | Alternatif policy yeri |
| 66–70 | Inheritance–interface–composition arasındaki ihtiyacı bir cümleyle ayırt ettir. | Hızlı eşleştirme |
| 70–80 | **Ara** | |
| 80–85 | **A7:** Reference aliasing, identity, logical equality, defensive copy/immutability panoraması. | `==`/`equals` tahmini |
| 85–90 | **A8:** `List<String>`, generics, `List`/`Set`; type rule ile duplicate domain kuralını ayır. | Yanlış güvenceyi düzeltme |
| 90–95 | **A9:** Memory başarısı, persistence hatası, exception contract ve try-with-resources. | İki ayrı başarı durumu |
| 95–100 | **A10:** Config ile bilinen implementation seçimi, runtime metadata; keyfi plugin’in ayrı güvenlik problemi olması. | Type/instance eşleştirmesi |
| 100–110 | On çapayı tek enrollment request’e yerleştir; studio starter’ını derle ve önce tek kaydı accept et. | A1–A10 I/P/F haritası + build/ilk test |
| 110–120 | **Ara** | |
| 120–132 | Capacity check’i mutation’dan önce uygula; full rejection sonrası count’un değişmediğini göster. | Sınır testi |
| 132–142 | Duplicate check’i ekle; aynı ID ikinci kez geldiğinde state’in değişmediğini göster. Driver/predictor rollerini değiştir. | Duplicate testi |
| 142–150 | Normal, capacity ve duplicate için JUnit/tekrar üretilebilir testler; kod ile UML/nesne haritasını karşılaştır. | Üç geçen test + map delta |
| 150–155 | Bireysel exit ticket ve 2. hafta köprüsü. | Çıkış kaydı |

## On çapanın ilk hafta için doğru derinliği

| Çapa | Bu hafta anlat | Bu hafta ertele |
| --- | --- | --- |
| A1 | Object = kimlik/state/davranış taşıyan örnek; sorumluluk tasarım ölçütüdür. | Büyük decomposition/refactoring |
| A2 | Encapsulation erişimi sınırlar; operation invariant’ı korur. | Setter kataloğu |
| A3 | Uses/owns farklı ilişkilerdir; model kodla uyuşmalıdır. | Tüm UML gösterimi |
| A4 | Subtype client beklentisini korumalıdır. | Ayrıntılı LSP karşı örnekleri |
| A5 | Interface çağrı sınırıdır; contract davranış ve failure’ı da içerir. | Interface’in doğruluğu otomatik kanıtladığı iddiası |
| A6 | Composition değişen davranışı collaborator’a verebilir. | Her `if` için pattern/class üretme |
| A7 | Aynı object’e birden çok reference ulaşabilir; identity/equality ayrıdır. | Derin/shallow copy ayrıntıları |
| A8 | Generic type element türünü sınırlar; domain invariant sağlamaz. | Collections API’nin tamamı |
| A9 | Memory ve external resource başarısı ayrıdır; cleanup sınırı gerekir. | Persistence katmanının tamamı |
| A10 | Config bilinen bir implementation seçebilir; runtime type gözlenebilir. | Güvenilmeyen arbitrary plugin yükleme |

## Studio görevi: tek course roster

Asgari davranış:

- capacity pozitif olmalıdır;
- ilk benzersiz öğrenci uygun kontenjana kabul edilir;
- aynı ID’nin ikinci isteği reddedilir ve count değişmez;
- capacity dolduktan sonraki farklı öğrenci reddedilir ve count değişmez;
- sonuç caller’a açık bir değer/sonuç olarak döner; yalnız console metnine bağımlı kalınmaz.

Bu hafta amaç “en çok class” üretmek değildir. Öğrenci küçük bir tasarım kararını açıklayabilmeli ve kuralları testle kanıtlayabilmelidir.

Sabit demo: capacity=`1`; sırasıyla `S100`, `S100`, `S101` → `ACCEPTED`, `DUPLICATE`, `FULL`; count her adım sonunda `1`. Duplicate kontrolü capacity kontrolünden önce yapılır. Constructor capacity≤0 değerini reddeder; boş/null ID ayrı sınır testidir.

A7: `b=a` için tek nesne çiz; `b` üzerinden değişikliğin `a` ile görüldüğünü göster. İki ayrı `new String("S100")` için `==` false, `equals` true beklenir. A4–A6 preview'ında `Notification` kayıt state'ini değiştirmemelidir; sessiz implementation ancak sözleşme sessiz bildirime izin veriyorsa geçerli sayılır.

## Sorulacak kritik sorular ve beklenen yön

- **Bir isim gördüğümüz her yerde class açmalı mıyız?** Hayır; sorumluluk, davranış, yaşam döngüsü ve değişiklik ihtiyacı değerlendirilir.
- **State `private` ise invariant güvende midir?** Hayır; public operations yanlış mutation yapabilir veya mutable referansı dışarı sızdırabilir.
- **Subtype derleniyorsa yerine kullanılabilir midir?** Hayır; client’ın davranış beklentisini ve sözleşmeyi koruması gerekir.
- **Interface implementation’ı doğru yapar mı?** Hayır; type boundary sağlar, davranış test/kanıt ister.
- **`List<String>` duplicate’i engeller mi?** Yalnız eleman türünü sınırlar; duplicate ayrı domain kuralıdır. `Set` seçimi de equality politikasına bağlıdır.
- **Memory’de accepted sonucu persistence başarısını kanıtlar mı?** Hayır; dış kaynak ayrı failure ve cleanup sınırı getirir.

## Yaygın yanılgılar ve müdahale

- “OOP = class açmak.” → Her class için sorumluluk ve değişiklik nedeni sordur.
- “Encapsulation = bütün field’lar private.” → Geçersiz state üreten public method karşı örneği ver.
- “Inheritance code reuse içindir.” → Client sözleşmesini bozan subtype göster.
- “Interface varsa loose coupling tamamdır.” → Caller’ın concrete type/config bilgisine bağımlılığını incelet.
- “`==` metin içeriğini karşılaştırır.” → İki ayrı `new String` ile tahmin yaptır.
- “Exception’ı yakalamak hatayı çözmektir.” → Recovery, propagation ve sessiz yutmayı ayır.

## Hafta sonu teslim paketi

1. A1–A10 için `implemented / previewed / future + kanıt` haritası.
2. Course roster kaynak kodu ve en az üç davranış testi.
3. Kodla uyumlu küçük UML/object diagram.
4. En fazla 150 kelimelik bir tasarım kararı: “duplicate kuralı neden burada?”
5. 60–90 saniyelik bireysel açıklama veya eşdeğer canlı sözlü kontrol.
6. AI kullanıldıysa araç, amaç, kabul/reddedilen öneri ve doğrulama yöntemi.

## Exit ticket ve 2. haftaya köprü

Öğrenci notsuz yanıtlar:

1. Bugün koruduğun invariant nedir?
2. Type safety ile domain rule arasındaki fark nedir?
3. Bir reddetme testinin önce/sonra state kanıtı ne olmalıdır?
4. 2. haftada sistemi paket ve sorumluluk sınırlarına ayırırken ilk hangi bağımlılığı görünür kılarsın?

2. haftaya başlangıç cümlesi: **“Bugün gördüğümüz on tasarım sorusunu, derlenen ve smoke test geçen bir walking skeleton’ın açık sınırlarına dönüştüreceğiz.”**
