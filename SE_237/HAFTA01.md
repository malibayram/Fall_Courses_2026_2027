# SE 237 Object Oriented Programming — 1. Hafta Öğretim Dosyası

**Hafta:** Panorama — tek malzeme planı, on tasarım sorusu
**Süre:** 155 dakika; 125 dakika etkin öğrenme + üç adet 10 dakikalık ara
**Dönem ürünü:** Factory ERP — nesne ve iş davranışı katmanı
**Bu dosyanın sınırı:** Yalnızca 1. haftada anlatılacak, yaptırılacak ve toplanacak işleri içerir.

**İlgili belgeler:** [Güncel kapsam](GUNCEL_KAPSAM.md) · [ERP öğrenci rehberi](ERP_OGRENCI_REHBERI.md) · [Kaynakça](KAYNAKCA.md) · [2. hafta](HAFTA02.md) · [3. hafta](HAFTA03.md) · [4. hafta](HAFTA04.md)

Bu Türkçe belge eğitmen içindir; öğrenci yönergeleri ve ölçülen içerik için İngilizce eşdeğer hazırlanır. W1 tanılayıcıdır; teslimler geri bildirim sağlar ve yeni bir not bileşeni oluşturmaz. Kurulum aksarsa eşli çalışma ve verilen çıktı üzerinden açıklama kabul edilir; kişisel ortam daha sonra tamamlanır.

## Haftanın ana sorusu

> Bir malzeme ihtiyacı hesabını, stok durumunu değiştirmeden; anlaşılır, test edilebilir ve değiştirilebilir nesnelerle nasıl tasarlarız?

İlk hafta Java sözdizimini baştan sona öğretme veya on konuyu bitirme haftası değildir. Yüz sandalyelik malzeme ihtiyacını hesaplayan yan etkisiz bir plan üzerinden on sabit tasarım çapası görünür hâle getirilir; ardından öğrenci küçük ama çalışan bir `MaterialPlanner` davranışı üretir.

## Ders sonunda öğrencinin göstereceği kanıt

Öğrenci:

- class, object ve reference kavramlarını ayırır;
- sorumluluk ve invariant üzerinden temel encapsulation gerekçesi kurar;
- ownership/uses ilişkisini basit UML veya nesne haritasında gösterir;
- inheritance, interface ve composition’ı yalnız syntax değil sözleşme/değişim açısından konumlandırır;
- identity ile logical equality’yi ayırır;
- generic collection’ın sağladığı type safety ile domain kuralını karıştırmaz;
- exception/resource ve runtime selection başlıklarını doğru sistem noktasına yerleştirir;
- miktar, fire oranı, çok seviyeli reçete ve “plan stokta değişiklik yapmaz” kurallarını küçük testlerle kanıtlar.

## Eğitmenin ders öncesi hazırlığı

- Java 25 LTS, build tool ve JUnit 6 ortamını temiz kopyada doğrula.
- Referans uygulamayı üç senaryo için hazırla: 100 sandalye ihtiyacı, yalnız ahşap eksiği ve geçersiz miktar/reçete reddi.
- İleri özellikleri gösteren preview ile öğrencinin tamamlayacağı küçük starter’ı ayrı tut.
- Starter’ı tek derste tamamlanabilecek düzeyde sınırla: `Product`, `BomLine`, `InventorySnapshot`, `MaterialPlan` ve dolaşma iskeleti.
- Worksheet’e A1–A10, `implemented / previewed / future` sütunları ekle.
- UML için tahta şablonu veya PlantUML başlangıcı hazırla; çizimi görsellik yarışına dönüştürme.
- Kurulum sorunu için terminal çıktısı, ortak makine ve eşli çalışma seçeneği hazırla.

## Ders öncesi öğrenci hazırlığı — 25–35 dakika

1. dev.java’dan Classes and Objects ile Interfaces sayfalarının seçilmiş girişlerini oku.
2. Şu gereksinimi bir kez oku: “100 sandalye için brüt ihtiyaçlar bulunacak; eksik malzeme raporlanacak; planlama stok veya rezervasyonu değiştirmeyecek.”
3. Not/AI kapalı ilk tahminlerini yaz:
   - Hangi üç şey object olabilir; hangisinin gerçekten davranışı vardır?
   - `private List` tek başına reçete döngüsünü veya geçersiz miktarı önler mi?
   - `==` ile `equals` her zaman aynı soruyu mu sorar?
   - Bir method derleniyorsa davranış sözleşmesini de koruyor mudur?
   - Plan memory’de doğruyken dış stok kaynağını okumak başarısız olabilir mi?
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
| 00–05 | Factory ERP hikâyesini ve tek isteği tanıt: “100 CHAIR-A için hangi malzemeler gerekir?” | İlk nesne/sorumluluk tahmini |
| 05–10 | Referans uygulamada ihtiyaç, eksik ahşap ve geçersiz reçete sonuçlarını göster; kodu henüz açıklama. | Request–rule–state–result izi |
| 10–16 | **A1:** Class, object, instance; noun avı yerine sorumluluk. | “Ne bilir/ne yapar?” kartı |
| 16–22 | **A2:** Encapsulation, private state ve invariant; rejected request’in state’i değiştirmemesi. | Invariant cümlesi |
| 22–30 | **A3:** Association, uses, owns; planner, BOM ve inventory snapshot sınırı. | Küçük object diagram |
| 30–40 | **Ara** | |
| 40–46 | A1–A3 notsuz retrieval; “Her şeyi `main` yapsa hangi değişiklikler yayılır?” | Değişiklik etkisi tahmini |
| 46–52 | **A4:** Subtype ve substitutability; aynı signature’ın doğru davranışa yetmemesi. | Sözleşme ihlali örneği |
| 52–59 | **A5:** Interface type, implementation ve davranış/failure contract. | Caller’ın bilmemesi gerekenler |
| 59–66 | **A6:** Composition ve Strategy; değişen policy’yi kararlı akıştan ayır. | Alternatif policy yeri |
| 66–70 | Inheritance–interface–composition arasındaki ihtiyacı bir cümleyle ayırt ettir. | Hızlı eşleştirme |
| 70–80 | **Ara** | |
| 80–85 | **A7:** Reference aliasing, identity, logical equality, defensive copy/immutability panoraması. | `==`/`equals` tahmini |
| 85–90 | **A8:** `List<BomLine>`, generics, `List`/`Map`; eleman türü ile reçete domain kuralını ayır. | Yanlış güvenceyi düzeltme |
| 90–95 | **A9:** Memory hesabı, veri kaynağı hatası, exception contract ve try-with-resources. | İki ayrı başarı durumu |
| 95–100 | **A10:** Config ile bilinen implementation seçimi, runtime metadata; keyfi plugin’in ayrı güvenlik problemi olması. | Type/instance eşleştirmesi |
| 100–110 | On çapayı tek planlama isteğine yerleştir; studio starter’ını derle ve ilk brüt ihtiyacı hesapla. | A1–A10 I/P/F haritası + build/ilk test |
| 110–120 | **Ara** | |
| 120–132 | Net miktarı fire oranıyla brüt miktara çevir; aynı bileşeni dallar arasında topla. | 100 sandalye ihtiyaç testi |
| 132–142 | Snapshot ile karşılaştırıp eksik ve üretilebilir miktarı hesapla; çağrı öncesi/sonrası stoğun aynı olduğunu göster. | Eksik + yan etkisizlik testi |
| 142–150 | Normal, geçersiz miktar ve reçete döngüsü için JUnit testleri; kod ile UML/nesne haritasını karşılaştır. | Üç geçen test + map delta |
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

## Studio görevi: yan etkisiz malzeme planı

Asgari davranış:

- istenen üretim miktarı pozitif olmalıdır;
- BOM satırının miktarı pozitif, fire oranı `0 ≤ loss < 1` olmalıdır;
- aynı hammadde farklı dallarda kullanılıyorsa brüt ihtiyaçlar toplanmalıdır;
- stok snapshot'ıyla karşılaştırma yalnız eksiği ve üretilebilir miktarı hesaplamalı, stoğu değiştirmemelidir;
- reçete döngüsü açık sonuçla reddedilmeli, sonsuz özyineleme olmamalıdır;
- sonuç caller'a açık bir değer nesnesi olarak dönmeli, yalnız console metnine bağımlı kalınmamalıdır.

Bu hafta amaç “en çok class” üretmek değildir. Öğrenci küçük bir tasarım kararını açıklayabilmeli ve kuralları testle kanıtlayabilmelidir.

Sabit demo: CHAIR-A/R1, miktar `100` → `WOOD=4 m³`, `FABRIC=150 m`, `FOAM=100`, `VARNISH=10 kg`; başlangıç snapshot'ında yalnız `WOOD=1 m³` eksik ve üretilebilir miktar `75`tir. Aynı plan iki kez çağrıldığında aynı sonuç döner, stok değişmez. Miktar `0`, loss≥`1` ve A→B→A döngüsü ayrı ret testidir.

A7: `b=a` için tek snapshot referansı çiz; paylaşılan mutable koleksiyonun sonuçları nasıl bozabileceğini göster. İki eş `ProductId` için identity ile logical equality ayrılır. A4–A6 preview'ında alternatif bileşen politikası planlama state'ini gizlice değiştirmemelidir.

## Sorulacak kritik sorular ve beklenen yön

- **Bir isim gördüğümüz her yerde class açmalı mıyız?** Hayır; sorumluluk, davranış, yaşam döngüsü ve değişiklik ihtiyacı değerlendirilir.
- **State `private` ise invariant güvende midir?** Hayır; public operations yanlış mutation yapabilir veya mutable referansı dışarı sızdırabilir.
- **Subtype derleniyorsa yerine kullanılabilir midir?** Hayır; client’ın davranış beklentisini ve sözleşmeyi koruması gerekir.
- **Interface implementation’ı doğru yapar mı?** Hayır; type boundary sağlar, davranış test/kanıt ister.
- **`List<BomLine>` reçete doğruluğunu sağlar mı?** Yalnız eleman türünü sınırlar; pozitif miktar, birim ve döngü ayrı domain kurallarıdır.
- **Memory’de doğru plan dış veri erişiminin başarısını kanıtlar mı?** Hayır; dış kaynak ayrı failure ve cleanup sınırı getirir.

## Yaygın yanılgılar ve müdahale

- “OOP = class açmak.” → Her class için sorumluluk ve değişiklik nedeni sordur.
- “Encapsulation = bütün field’lar private.” → Geçersiz state üreten public method karşı örneği ver.
- “Inheritance code reuse içindir.” → Client sözleşmesini bozan subtype göster.
- “Interface varsa loose coupling tamamdır.” → Caller’ın concrete type/config bilgisine bağımlılığını incelet.
- “`==` metin içeriğini karşılaştırır.” → İki ayrı `new String` ile tahmin yaptır.
- “Exception’ı yakalamak hatayı çözmektir.” → Recovery, propagation ve sessiz yutmayı ayır.

## Hafta sonu teslim paketi

1. A1–A10 için `implemented / previewed / future + kanıt` haritası.
2. `MaterialPlanner` kaynak kodu ve en az üç davranış testi.
3. Kodla uyumlu küçük UML/object diagram.
4. En fazla 150 kelimelik bir tasarım kararı: “planlama neden stoğu değiştirmiyor?”
5. 60–90 saniyelik bireysel açıklama veya eşdeğer canlı sözlü kontrol.
6. AI kullanıldıysa araç, amaç, kabul/reddedilen öneri ve doğrulama yöntemi.

## Exit ticket ve 2. haftaya köprü

Öğrenci notsuz yanıtlar:

1. Bugün koruduğun invariant nedir?
2. Type safety ile domain rule arasındaki fark nedir?
3. Bir reddetme testinin önce/sonra state kanıtı ne olmalıdır?
4. 2. haftada sistemi paket ve sorumluluk sınırlarına ayırırken ilk hangi bağımlılığı görünür kılarsın?

2. haftaya başlangıç cümlesi: **“Bugün gördüğümüz on tasarım sorusunu, derlenen ve smoke test geçen bir walking skeleton’ın açık sınırlarına dönüştüreceğiz.”**
