# CEN 302 Operating Systems — 1. Hafta Öğretim Dosyası

**Hafta:** Panorama — bir programın yaşam döngüsü  
**Süre:** 180 dakika; 140 dakika etkin öğrenme + 40 dakika ara  
**Dönem ürünü:** Mini Systems Workbench  
**Bu dosyanın sınırı:** Yalnızca 1. haftada anlatılacak, yaptırılacak ve toplanacak işleri içerir.

**İlgili belgeler:** [Prototip ve dönem planı](PROJE.md) · [Kaynakça](KAYNAKCA.md)

Bu Türkçe belge eğitmen içindir; öğrenci yönergeleri ve ölçülen içerik için İngilizce eşdeğer hazırlanır. W1 tanılayıcıdır; teslimler geri bildirim sağlar ve yeni bir not bileşeni oluşturmaz. Kurulum aksarsa eşli çalışma ve verilen çıktı üzerinden açıklama kabul edilir; kişisel ortam daha sonra tamamlanır.

## Haftanın ana sorusu

> Bir programın çalıştırılma isteği kullanıcı alanından başlayıp süreç, zamanlama, bellek, dosya sistemi ve I/O katmanlarından geçerek nasıl gözlemlenebilir bir sonuca dönüşür?

İlk haftanın amacı on konuyu öğretip bitirmek değildir. Öğrenci, dönem boyunca ayrıntılandıracağı on sabit çapayı tek bir çalışan örnek üzerinde görmeli; **gerçek gözlem**, **öğretim modeli** ve **gelecek hafta inşa edilecek parça** arasındaki farkı söyleyebilmelidir.

## Ders sonunda öğrencinin göstereceği kanıt

Öğrenci:

- user space ile kernel sınırını ve bir system call’ın rolünü açıklar;
- program, process ve thread’i aynı şeymiş gibi kullanmaz;
- file descriptor/pipe ile veri akışını temel düzeyde izler;
- race condition, scheduling, address translation, page fault, file ve I/O kavramlarını doğru çapaya yerleştirir;
- küçük bir C byte-counter programını derler ve normal, boş, eksik dosya ve yanlış kullanım durumlarında çalıştırır;
- exit status ile programın yazdığı metni ayırır;
- her iddiasını gözlem/model/gelecek iş etiketiyle sınırlar;
- bir sayfalık A1–A10 haritası ve 60–90 saniyelik açıklama üretir.

## Eğitmenin ders öncesi hazırlığı

- Linux veya doğrulanmış Linux VM/container ortamında `cc`, `make`, `strace`, `wc`, `ps` ve temel shell araçlarını kontrol et.
- İçeriği bilinen küçük `sample.txt` ve boş `empty.txt` hazırla; kişisel dosya kullanma.
- Referans demoyu temiz kopyadan derle ve beklenen çıktıları kaydet.
- C starter’da byte sayacının eksik davranışını görünür bırak; çözümü önceden dağıtma.
- Worksheet’e A1–A10 satırları ile `O = observed`, `M = modelled`, `F = future` sütunlarını koy.
- Erişim sorunu yaşayan öğrenci için önceden alınmış terminal çıktısı ve eşli çalışma seçeneği hazırla.
- Demo sırasında kontrolsüz process bomb, bellek tüketimi veya gerçek crash testi yapılmayacağını açıkça belirt.

## Ders öncesi öğrenci hazırlığı — 25–35 dakika

1. OSTEP’in giriş bölümünden virtualization, concurrency ve persistence kavramlarını oku.
2. Linux man-pages’den `read(2)`, `write(2)` ve `_exit(2)` sayfalarının synopsis kısmına bak.
3. Aşağıdaki sorulara not/AI kapalı ilk tahminini yaz:
   - Bir executable dosya ile çalışan process arasındaki fark nedir?
   - `printf` çağrısı doğrudan diske yazar mı?
   - Aynı program iki kez çalıştırılırsa aynı process midir?
   - Page fault her zaman programın çöktüğü anlamına gelir mi?
   - Programın ekrana `ERROR` yazması exit status’ın sıfır olmadığını kanıtlar mı?
4. Ortam doğrulama komutlarını çalıştır ve hata varsa ekran görüntüsü/çıktıyla derse getir.

Hazırlık doğruluk puanı almaz; eksik ön bilgiyi görünür kılan düşük riskli başlangıç kanıtıdır.

## Tahta ve slayt omurgası

```text
request → program/process → system-call boundary → scheduler
        → address space/virtual memory → file system → storage/I/O
        → observable output + exit status
```

Her teknik durakta aynı dört soru sorulur:

1. Burada hangi problem çözülüyor?
2. Kararı kim veriyor: program, library, kernel, donanım mı?
3. Bugün neyi gerçekten gözledik?
4. Bu mekanizmayı dönem içinde hangi haftada derinleştireceğiz?

## 180 dakikalık ders akışı

| Süre | İçerik ve öğretmen hamlesi | Öğrenci işi / kanıt |
| --- | --- | --- |
| 00–05 | Dönem ürünü ve ana soruyu tanıt. “Bir komut verdiğimizde gerçekte ne başlar?” | İlk bireysel tahmin |
| 05–12 | Referans programı normal, eksik dosya ve yanlış kullanım ile çalıştır. Kodu henüz açma. | Girdi–çıktı–exit tablosu |
| 12–18 | **A1:** OS sınırı, privilege, syscall, interrupt/trap ayrımı. `strace`ten yalnız seçilmiş satırlar göster. | User/kernel sınırını işaretle |
| 18–24 | **A2:** Program ve process; PID, address space, açık kaynaklar, process state. | Program/process karşılaştırması |
| 24–30 | **A4:** File descriptor, standard streams, redirection ve pipe ile gözlenebilir çıktı. | `stdin/stdout/stderr` eşleştirmesi |
| 30–40 | **Ara** | |
| 40–47 | Önceki üç çapayı notsuz geri çağır; bir yanlış sezgiyi tahtaya taşı. | 60 saniyelik retrieval |
| 47–54 | **A3:** Thread, paylaşılan adres uzayı ve nondeterminism. “Aynı sayacı iki thread artırırsa?” | Sonuç tahmini |
| 54–61 | **A5:** Atomicity, race, lock ve deadlock’a panorama. Kilidin amaç değil invariant koruma aracı olduğunu vurgula. | Güvenlik koşulu yaz |
| 61–67 | **A6:** Ready/running/blocked, scheduler ve policy/mechanism ayrımı. Küçük timeline çiz. | Sıralama/tamamlama tahmini |
| 67–70 | A3–A6 bağlantısını tek cümlede kurdur. | Çift paylaşımı |
| 70–80 | **Ara** | |
| 80–86 | **A7:** Virtual address, page, frame ve address translation için küçük sayısal model. | Adres parçalama denemesi |
| 86–92 | **A8:** Page fault, demand paging, protection ve isolation. “Her fault disk erişimi midir?” | İki karşı örnek |
| 92–98 | **A9:** Path, file, data/metadata ve crash consistency. Normal kapanmanın durability kanıtı olmadığını göster. | İddia sınırı yaz |
| 98–104 | **A10:** Library buffer, kernel cache, device; latency/throughput ve ölçüm kapsamı. | Alternatif açıklama üret |
| 104–110 | On çapayı aynı komutun yaşam döngüsüne yerleştir. | A1–A10 O/M/F haritası |
| 110–120 | **Ara** | |
| 120–128 | Studio starter’ını derlet; `bytes=0` sonucunun neden eksik olduğunu tahmin ettir. | Build çıktısı + tahmin |
| 128–140 | `fgetc`/EOF döngüsüyle byte sayacını birlikte tamamla; sonra `wc -c` ile oracle karşılaştırması yap. | Küçük kod değişikliği |
| 140–150 | Dosya açma, stream error, kullanım hatası ve `stdout/stderr` ayrımını eklet. Roller: driver/predictor; 140’ta değiştir. | Normal + hata çıktısı |
| 150–160 | **Ara** | |
| 160–170 | Test matrisi: normal, empty, missing, wrong usage. Önce beklenen exit/output, sonra çalıştırma. | Dört satırlık test tablosu |
| 170–176 | İkili sözlü savunma: bir öğrenci yaşam döngüsünü anlatır, diğeri O/M/F sınırını sorgular. | 60–90 saniyelik açıklama |
| 176–180 | Exit ticket ve 2. hafta köprüsü. | Bireysel çıkış kaydı |

## On çapanın ilk hafta için doğru derinliği

| Çapa | Bu hafta anlat | Bu hafta anlatma/iddia etme |
| --- | --- | --- |
| A1 | Syscall kontrollü kernel girişidir; library call ile aynı değildir. | Tüm trap/interrupt implementasyonu |
| A2 | Process çalışan programın state ve kaynak taşıyan örneğidir. | Tam process-control uygulaması |
| A3 | Thread’ler state paylaşabilir; sıra deterministik olmayabilir. | Thread API ayrıntıları |
| A4 | Descriptor bir kernel kaynağına erişim tanıtıcısıdır; pipe byte akışıdır. | Karmaşık IPC protokolü |
| A5 | Race, sonucun interleaving’e bağlı olmasıdır; lock doğru kapsam ister. | “Lock varsa kod doğrudur” genellemesi |
| A6 | Scheduler seçim yapar; policy hedefe göre trade-off taşır. | Tek “en iyi” algoritma iddiası |
| A7 | Virtual address page/offset olarak modellenip frame’e çevrilebilir. | Tahta modelini gerçek host ölçümü sayma |
| A8 | Fault recoverable veya fatal olabilir; isolation erişimi sınırlar. | Her fault’u disk okuması sayma |
| A9 | Path, file, data ve metadata farklı kavramlardır. | Normal exit’i crash durability kanıtı sayma |
| A10 | Birden çok buffering/cache katmanı ölçümü etkiler. | Tek koşudan performans sonucu çıkarma |

## Studio görevi: kanıtlı byte counter

Program bir dosya yolu alacak, dosyadaki byte sayısını `stdout`’a yazacak ve durumları ayırt eden exit code döndürecektir. Beklenen asgari davranış:

- doğru kullanım + okunabilir dosya → doğru byte sayısı, exit `0`;
- boş dosya → `0`, exit `0`;
- dosya yok/okunamıyor → tanısal mesaj `stderr`, sıfır olmayan exit;
- argüman sayısı yanlış → usage mesajı `stderr`, sıfır olmayan exit.

Öğrenci her testten önce sonucu yazar. `wc -c` bir karşılaştırma oracle’ıdır; tek başına öğrencinin mekanizmayı anladığını kanıtlamaz.

Sabit örnekler: LF ile biten `abc` dört byte, boş dosya sıfır byte; eksik dosya exit `1`, yanlış kullanım exit `2`. `fgetc` sonucu `int` tutulur; `EOF` sonrası `ferror` kontrol edilir. UTF-8 karakter ve byte sayısı ayrılır; başarılı çıktıdan sonra `fflush(stdout)` hatası da ele alınır.

Üç tahta örneği: (1) İki artırmanın okuma–artırma–yazma adımlarını iç içe geçir; başlangıç 0 iken kayıp güncelleme ile 1 sonucunu çıkar. Gerçek C data race'inin tanımsız davranış olduğunu, çizimin öğretim modeli olduğunu söyle. (2) Tek CPU'da aynı anda gelen, burst süreleri 3 ve 1 olan işleri FCFS ve quantum=1 RR ile çiz. (3) Yalnız öğretim modelinde 100 byte sayfa, page 2 → frame 7 için adres 234'ü 734'e çevir; gerçek xv6 sayfalarıyla karıştırma.

## Sorulacak kritik sorular ve beklenen yön

- **`printf` system call mıdır?** Genellikle library işlevidir; buffering sonrası `write` gibi bir syscall’a yol açabilir.
- **Program iki kez çalışınca ne paylaşılır?** Executable aynı olabilir; process kimliği, address space ve çalışma state’i ayrıdır. Paylaşım ayrıca tasarlanır.
- **Race yalnız “çok hızlı çalışma” problemi midir?** Hayır; kritik işlemlerin interleaving’ine ve korunması gereken invariant’a bağlı correctness problemidir.
- **Page fault hata mıdır?** Bir olaydır; talep üzerine sayfa getirme ile çözülebilir veya geçersiz erişimde süreç sonlandırılabilir.
- **İkinci okuma daha hızlıysa disk hızlanmış mıdır?** Kanıtlanamaz; cache, buffering, sistem yükü ve ölçüm yöntemi alternatif açıklamalardır.
- **Ekranda hata metni görmek program başarısızlığını kanıtlar mı?** Hayır; shell’in gördüğü exit status ayrıca kontrol edilir.

## Yaygın yanılgılar ve müdahale

- “OS sadece arayüzdür.” → Aynı istekte protection, allocation ve multiplexing kararlarını göster.
- “Process program dosyasıdır.” → Aynı executable’dan iki PID çalıştır.
- “Thread’ler paralel olmak zorundadır.” → Concurrency ve parallelism’i ayrı eksen olarak çiz.
- “Lock eklemek race’i otomatik çözer.” → Yanlış kapsamlı lock karşı örneği sor.
- “Virtual memory RAM’den daha büyük bellek demektir.” → Translation + isolation işlevlerini öne al.
- “Dosyayı kapattık, veri kesin kalıcıdır.” → Normal program sözleşmesi ile power-loss durability’yi ayır.

## Hafta sonu teslim paketi

1. A1–A10 için `O/M/F + bir cümle kanıt` haritası.
2. Byte counter kaynak kodu ve dört durumluk test çıktısı.
3. En fazla 150 kelimelik “bir komutun yaşam döngüsü” açıklaması.
4. 60–90 saniyelik bireysel açıklama videosu veya eşdeğer canlı sözlü kontrol.
5. “En çok emin olmadığım çapa ve neden?” yansıması.
6. AI kullanıldıysa araç, amaç, kabul/reddedilen öneri ve doğrulama yöntemi.

## Exit ticket ve 2. haftaya köprü

Öğrenci notsuz yanıtlar:

1. Bugün doğrudan gözlediğin bir olay nedir?
2. Yalnızca modellediğin bir mekanizma nedir?
3. `stdout`, `stderr` ve exit status hangi üç ayrı kanıtı taşır?
4. 1. haftada walking skeleton kurarken hangi sınırı görünür yapmak istersin?

5. haftaya başlangıç cümlesi: **“Panoramada gördüğümüz yaşam döngüsünü, açık bileşen ve sınırlarla derlenebilir küçük bir iskelete dönüştüreceğiz.”**
