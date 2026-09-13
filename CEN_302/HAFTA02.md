# CEN 302 Operating Systems — 2. Hafta Öğretim Dosyası

## Haftanın kimliği

| Alan | Plan |
| --- | --- |
| Tema | Bütün sistemin yapısı: parçalar, sınırlar ve çalışan iskelet |
| Ana soru | Aynı byte sayma işini Linux ve xv6 üzerinde çalıştırırken hangi parçaları koruyor, hangi sınırları değiştiriyoruz? |
| Süre | 155 dakika: 125 dakika etkin öğrenme + üç adet 10 dakikalık ara |
| Başlangıç | W1 Byte Counter veya kaynak sürümü belirtilmiş eğitmen tabanı |
| Haftanın ürünü | Host/guest ayrımı olan Workbench iskeleti, ortak fixture, smoke test, A1–A10 haritası |
| Sonraki kapı | W3'te `run(job) → result` sözleşmesinin süreç başlatan dikey dilime dönüşmesi |
| Belge durumu | Ayrıntılı öğretim ve görev tasarımı; aşağıda adı geçen starter ve test araçları ayrıca hazırlanıp çalıştırılmalıdır |

**Bağlantılar:** [Ana ders sistemi](../README.md) · [Proje sözleşmesi](PROJE.md) · [W1](HAFTA01.md) · [W3](HAFTA03.md) · [W4](HAFTA04.md) · [Kaynakça](KAYNAKCA.md)

Bu Türkçe dosya eğitmen ve asistanlar içindir. Sondaki İngilizce öğrenci görev özeti doğrudan paketlenebilir; ölçülen kavramların İngilizce notu, hazırlık soruları ve rubrik açıklaması da yayımlanır. Yeni bir ders notu bileşeni oluşturulmaz; haftalık kanıtlar ana README'deki mevcut bileşenlere bağlanır.

## 1. Öğretim amacı ve doğru derinlik

W1'de görülen bütünü bu hafta **yapı açısından** yeniden kuracağız. Öğrenci bir program dosyasının, çalışan sürecin, kütüphanenin, işletim sistemi arayüzünün, çekirdeğin ve sanal makine ortamının aynı katmanda olmadığını gösterecek. Aynı işin iki ortamda aynı sonucu vermesi ile aynı implementasyona sahip olması arasındaki farkı açıklayacak.

**Bu hafta A1–A4'e (OS sınırı/syscall, süreç, thread, IPC/descriptor) ayrıntılı işlenir.** A5–A10, ikinci turun W3 ve W4'ünde ele alınacaktır; bu hafta yalnız haritada "bekleyen" olarak işaretlenir, yeniden anlatılmaz. Launcher'ın süreç oluşturması W3'ün işidir; syscall, page table veya scheduler bu hafta yazdırılmaz.

### Ölçülebilir kazanımlar

| Kod | Öğrenci ders sonunda… | Kanıt |
| --- | --- | --- |
| W2-K1 | Linux ortamı, QEMU, xv6 kernel'i ve guest kullanıcı programını ayırır. | İki yürütme yolunu gösteren katman diyagramı |
| W2-K2 | W1 kodunu host bölümüne taşır ve davranışını korur. | W1'in dört regresyon testi |
| W2-K3 | Sağlanan xv6 tabanını ve guest uyarlamasını çalıştırır. | Boot kaydı ve aynı içerikle aynı byte sayısı |
| W2-K4 | Byte-counting sözleşmesini C kütüphanesi ayrıntısından ayırır. | Host `fgetc` / guest `read` karşılaştırması |
| W2-K5 | Her çapaya bir yapı/sorumluluk ve kanıt sınırı atar. | On satırlık O/M/F haritası |
| W2-K6 | Bir işi başlatma isteğinin girdisini, sonucunu ve kaynak sahibini tanımlar. | `Job` / `RunResult` taslağı; henüz çalışan launcher iddiası yok |
| W2-K7 | Aynı sonucu yeniden üretmek için gerekli sürüm ve veriyi kaydeder. | Baseline, fixture manifesti ve komut kaydı |

`O = observed`: gerçekten çalıştırılan veya kaynakta incelenen şey; kaynak gözlemi ayrıca `source` diye belirtilir. `M = modelled`: tahta/öğretim modeli. `F = future`: ileride uygulanacak davranış. Her satır ayrıca **upstream / supplied / student** katkısını yazar. Kaynağı okumak, mekanizmayı öğrenci olarak uygulamış olmak anlamına gelmez.

## 2. Kapsam ve sorumluluk paylaşımı

| Şerit | İçerik |
| --- | --- |
| Core | Host düzeni, W1 regresyonu, sağlanan guest programı ve fixture aktarımı, bir host/guest smoke testi, sistem haritası ve launcher sözleşmesi |
| Stretch | Aynı sayımın farklı tampon boylarıyla doğrulanması veya bir ek binary fixture; performans iddiası zorunlu değil |
| Instructor demo | xv6 kaynak ağacında sınırlar, syscall yolu, process state, page table ve dosya/aygıt bağlantısı |
| Sonraya bırakılan | `fork/exec/waitpid` uygulaması W3; sonlanma/cleanup kabulü W4; kernel artımları W5 ve sonrası |

Öğrenci compiler/toolchain, QEMU otomasyonu veya xv6 dosya sistemi imajı üreticisini sıfırdan yazmaz. Bu altyapılar ders başlamadan çalışır durumda sağlanır. Kurulumu aksayan öğrenci doğrulanmış laboratuvar ortamında kendi komutlarını çalıştırabilir; yalnız hazır çıktı kullanırsa bunu `supplied evidence` olarak işaretler ve daha sonra canlı doğrulamayı tamamlar.

## 3. Eğitmenin ders öncesi hazırlığı

### En az beş gün önce yayımlanacak paket

- 20–30 dakikalık Türkçe yapı videosu; aynı ölçülen içeriğin İngilizce notu.
- W1 kaynaklarını kabul eden boş Workbench yapısı ve çalışır build dosyaları.
- Sabit xv6 commit'i, Linux ortamı, QEMU ve RISC-V araç sürümlerini gösteren baseline kaydı.
- Guest Byte Counter'ın `open/read/close` uyarlaması; öğrencinin gözden geçireceği küçük ve işaretli bölüm.
- Byte düzeyinde tanımlı fixture'lar, expected-output dosyaları ve guest imaja ekleme düzeneği.
- Başlangıç smoke testi, O/M/F çalışma kâğıdı, on artımlık backlog şablonu.
- Kurulum sorunları için kısa kontrol listesi ve sağlanan terminal kaydı.

### Yayımlama öncesi öğretmen kontrolü

| Kontrol | Hazır olma koşulu |
| --- | --- |
| Linux host | `cc`, `make`, `wc` ve gerekli araçlar tanımlı ortamda çalışır. macOS/Windows ana makine varsa Linux VM/container yolu ayrıca belgelenir. |
| Guest ortamı | QEMU xv6'yı açar; guest programı dosya sistemi imajında bulunur. |
| Sürüm | [Proje planındaki](PROJE.md) xv6 commit'i başlangıç adayıdır; gerçekten kullanılan commit ve varsa patch'ler baseline'a yazılır. |
| Fixture | Host ve guest girdilerinin içeriği byte düzeyinde aynıdır; guest dosyası host dosyasına otomatik erişiyormuş gibi varsayılmaz. |
| Derleme | Linux executable'ı ile RISC-V/xv6 executable'ı karışmaz. |
| Çıkış kontrolü | QEMU'dan çıkış ve test timeout'u öğretim ekibinin harness'inde denenmiştir. |
| Katkı | Kernel, uyarlama ve test altyapısının sağlanan bölümleri açıktır. |
| Süre | Studio'daki öğrenci değişikliği 120–150 aralığındaki 30 etkin dakikaya sığar. |

Upstream'de kullanıcı programları ve imaj üretimi [Makefile](https://github.com/mit-pdos/xv6-riscv/blob/9e3161a9abf5f51ea402562d1874caf6c4926597/Makefile) üzerinden izlenir. `_bytecount` ekleme ve fixture paketleme değişiklikleri eğitmen starter'ında hazır olur. Upstream `make qemu` ile bu ders için hazırlanacak `make guest-test` ayrı komutlardır.

## 4. Ders öncesi öğrenci hazırlığı

Toplam hedef: video/not, seçilmiş okuma, ortam kontrolü ve altı soru için yaklaşık 50–65 dakika. Yanıtlar dersten 12 saat önce teslim edilir. Öğrenci önce kısa bireysel tahmin yazar; sonraki açık hazırlıkta kullandığı kaynak/AI desteğini belirtir.

Okuma sınırı: [xv6 kitabının](https://mit-pdos.github.io/xv6-riscv-book/) işletim sistemi arayüzü ve organizasyon başlıklarından seçilmiş sayfalar; [xv6 kullanıcı API'si](https://github.com/mit-pdos/xv6-riscv/blob/9e3161a9abf5f51ea402562d1874caf6c4926597/user/user.h); Linux [`read(2)`](https://man7.org/linux/man-pages/man2/read.2.html) dönüş değerleri. Kitabın tamamı bu haftanın ödevi değildir.

| Soru | Eğitmenin beklediği yön |
| --- | --- |
| 1. W1 programında sayı, hata metni ve exit code nereden görülür? | Veri çıktısı, tanı ve süreç sonucu ayrıdır. |
| 2. QEMU içinde xv6 açıldığında Byte Counter hangi OS arayüzünü kullanır? | Guest program xv6 arayüzünü kullanır; QEMU host'ta çalışan başka bir süreçtir. |
| 3. Aynı C dosyasını farklı ortamlarda derlemek her zaman yeterli midir? | Kütüphane, ABI, mimari ve sistem arayüzü farkları olabilir. |
| 4. `read` tamponu 16 byte, dönüş değeri 4 ise sayaca ne eklenir? | Dönen 4; tampon kapasitesi değil. |
| 5. Host ile guest'te aynı path yazılması aynı dosyayı açtığımızı kanıtlar mı? | Hayır; ayrı dosya sistemi bağlamları ve içerik kontrolü gerekir. |
| 6. İleride üç işi çalıştırırsak PID ve sonlanma bilgisini hangi bileşen tutmalı? | Launcher sonuç/sahiplik sınırı; Byte Counter'ın sayma sorumluluğu korunur. |

Öğrenci W1'in dört testini tekrar çalıştırır; başarısızsa hatayı sınıflandırır: ortam, build, fixture, davranış veya çıktı karşılaştırması. Kısa hata kaydı derste destek sırasını belirler.

## 5. Tahta ve slayt omurgası

```text
Yol 1: Linux ortamı
shell → host bytecount → C library → Linux syscall sınırı → Linux kernel → dosya/I/O

Yol 2: aynı Linux ortamında QEMU
QEMU [host süreci]
  └─ sanal RISC-V makinesi
       └─ xv6 kernel
            └─ xv6 shell → guest bytecount → xv6 kullanıcı API'si → xv6 kernel

Ortak sözleşme: aynı byte dizisi → aynı sayı
Ayrı sorumluluklar: derleme, API, executable biçimi, dosya sistemi, süreç sonucu
```

Okların bir kısmı çağrı, bir kısmı barındırma ilişkisi taşır; eğitmen bunları farklı çizgiyle gösterir. Diyagram syscall'ın donanıma ulaşan bütün ayrıntılarını göstermeyi amaçlamaz.

### Hedef proje yapısı

Aşağıdaki ağaç **hazırlanacak starter'ın sözleşmesidir**; bu öğretim belgeleri deposunda kod varmış gibi okunmaz.

```text
workbench/
├── host/bytecount.c
├── host/launcher.h         W3 için Job / RunResult taslağı
├── xv6/                    Sabit upstream ve sağlanan guest uyarlaması
├── fixtures/               sample.txt, empty.txt, manifest
├── tests/                  Host/guest smoke ve expected-output
├── evidence/               W2 komutları, harita ve sürüm kaydı
├── models/                 Gelecek mekanizma deneylerinin yeri
├── BASELINE.md
├── CHANGELOG.md
└── README.md
```

### A1–A4 yapı turu — bu haftanın odağı

| Çapa | Yapı sorusu | Bu hafta gösterilecek şey | Derinleşme bağlantısı |
| --- | --- | --- | --- |
| A1 — OS sınırı | Kütüphane ve kernel nerede ayrılır? | Host/guest çağrı katmanları; xv6 syscall girişinin kaynak konumu | W5 istatistik syscall'ı |
| A2 — Süreç | Çalışan işi kim temsil eder? | Host süreç / QEMU / guest süreç ayrımı; `Job` ve sonuç sahibi | W6 çoklu child |
| A3 — Thread | Paylaşılan çalışma nereye eklenecek? | Host worker modülünün gelecekteki thread yolu; hangi ortak durumun korunacağı | W7 worker pool |
| A4 — IPC/descriptor | Veri ve hata hangi sınırdan geçer? | Standart kanallar; guest `read`; launcher iç sınır taslağı | W8 pipe akışı |

Her öğrenci satırın yanına O/M/F, host/guest/model ve katkı etiketini ekler. “Kernel kaynak dosyasını buldum” ile “çalışma zamanında o yolu ölçtüm” ayrı kanıtlardır.

### A5–A10 — bu hafta yalnız haritada bekleyen

| Çapa | Nerede ele alınacak |
| --- | --- |
| A5 — Senkronizasyon | W3 (davranış/hata turu) |
| A6 — Zamanlama | W3 (davranış/hata turu) |
| A7 — Adres çevirisi | W3 (davranış/hata turu) |
| A8 — Sanal bellek | W4 (entegrasyon turu) |
| A9 — Dosya sistemi | W4 (entegrasyon turu) |
| A10 — I/O | W4 (entegrasyon turu) |

Bu altı çapa bu hafta yeniden anlatılmaz; harita satırlarında `F` (future) olarak kalır. A1–A4 içindeki bir örnek bu çapalardan birine değinirse yalnız tek cümlelik bağlantı kurulur.

## 6. 155 dakikalık ders akışı

| Süre | Öğretmen hamlesi | Öğrenci işi ve kontrol noktası |
| --- | --- | --- |
| 00–08 | W1'in üç gözlem kanalını notsuz geri çağır; aynı fixture'ı host ve guest'te göster. | Bireysel kısa yanıt; aynı sayıdan hangi sonucu çıkarabileceğini yaz. |
| 08–18 | **A1:** Linux/QEMU/xv6 katmanlarını çiz; host/guest syscall sınırı, xv6 syscall girişinin kaynak konumu. | Her executable ve kernel'i doğru kutuya yerleştir. |
| 18–30 | **A1 devam:** `strace`/kaynak turuyla syscall yolu; host kütüphane çağrısı ile kernel giriş noktasını ayır. | User/kernel sınırını iki örnekte işaretle. |
| 30–40 | Ara | |
| 40–55 | **A2:** Host süreç / QEMU / guest süreç ayrımı; `Job` ve sonuç sahibi taslağı. | Üç süreç türünü ayrı kutuda göster; sahiplik kararı. |
| 55–70 | **A4:** Standart kanallar; `fgetc` ile guest `read` sınırını karşılaştır; launcher iç sınır taslağı. | API farkı ve ortak iş sözleşmesi. |
| 70–80 | Ara | |
| 80–95 | **A3:** Host worker modülünün gelecekteki thread yolu; hangi ortak durumun korunacağı, sahibi kim. | İki gelecek (F) noktası ve sahibi. |
| 95–110 | Proje ağacı, katkı sahipliği; `Job`/sonuç ve veri/hata sözleşmesini birlikte tasarla. | Girdi, sonuç ve sahibi için üç karar. |
| 110–120 | Ara | |
| 120–125 | Studio başlangıcı: bireysel AI'sız yapı tahmini. | Hangi dosya nerede derlenecek? |
| 125–136 | W1 kodunu host yapısına taşıt ve regresyonu çalıştır. | Dört test; taşıma davranışı bozdu mu? |
| 136–145 | Hazır guest adaptörünü incelet ve ortak fixture'ı çalıştır. | Okuma dönüş değeri ve sayı eşleşmesi. |
| 145–150 | Smoke sonucu ve A1–A4 haritasını kontrol et; A5–A10'un bekleyen olarak kaldığını doğrula. | Ara kabul; driver/predictor rolleri değişir. |
| 150–155 | Exit ticket ve W3 köprüsü. | Üç kısa yanıt ve eksik iş kaydı. |

Eğitmen ve iki asistan studio'da ortam, test ve kavramsal açıklama kontrollerini paylaşır. Her öğrencinin uzun sözlüsü aynı gün yapılmaz; puanlanan sözlüler dönem kapsama kaydına göre seçilir. Eski akıştaki bağımsız tekrar ve harita kontrolü, bölüm 9'daki ders dışı Core hedefine zaten dahildir; sınıfta yeniden tekrarlanmaz.

## 7. Studio: aynı işi iki ortamda doğrulama

### Adım 1 — Baseline ve veri

Başlangıç commit/paketini kaydet. W1 kendi kodun kullanılıyorsa bunu; eğitmen tabanı alındıysa sürümü ve eksikliği belirt. `sample.txt` içeriği `61 62 63 0a` hexadecimal byte'larıdır; `empty.txt` sıfır byte'tır. Guest imajındaki dosyalar da aynı manifestten üretilir.

### Adım 2 — Host regresyonu

Program davranışını değiştirmeden dosya yerini/build yolunu güncelle. Sayı, newline, stdout/stderr ve exit code ayrı sınanır. W1'in normal, boş, bulunamayan dosya ve yanlış kullanım testleri geçmelidir.

### Adım 3 — Guest uyarlamasını açıklama

Sağlanan guest döngüsünde her başarılı okumada dönen byte miktarını toplama, EOF'ta bitirme ve hata yolunu ayırma mantığını işaretle. Linux [`read(2)` dönüş sözleşmesi](https://man7.org/linux/man-pages/man2/read.2.html) ve seçili [xv6 API'si](https://github.com/mit-pdos/xv6-riscv/blob/9e3161a9abf5f51ea402562d1874caf6c4926597/user/user.h) karşılaştırılır; Linux `errno` davranışı guest'e otomatik taşınmaz.

Guest çözümü eğitmen tarafından sağlanır; öğrencinin görevi sayım adımını açıklamak, aynı içerikle çalıştırmak ve farkları belgelemektir. Kernel kaynak değişikliği bu adımın şartı değildir.

### Adım 4 — Smoke ve sınırı

| Test | Beklenen sonuç | Kontrol edilen sınır |
| --- | --- | --- |
| W2-T1: host, normal dosya | `4` ve newline; exit 0 | W1 davranışı korunur. |
| W2-T2: host, boş dosya | `0` ve newline; exit 0 | EOF başlangıç durumu. |
| W2-T3: host, eksik dosya | stdout boş; stderr tanı; exit 1 | Veri ve hata ayrımı. |
| W2-T4: host, yanlış argüman | stderr kullanım; exit 2 | CLI sözleşmesi. |
| W2-T5: guest boot + normal dosya | Guest prompt/program erişimi; sayı 4 | Derleme–imaj–çalıştırma zinciri. |
| W2-T6: guest, boş dosya | Sayı 0 | Aynı fixture için aynı iş sonucu. |
| W2-T7: tekrar | Aynı sayılar; fixture değişmemiş | Smoke yeniden üretilebilirliği. |

Guest exit sonucu ölçülecekse öğretmenin sağladığı `wait` tabanlı test programı kullanılır. xv6 shell'de Linux shell'in `$?` özelliği varmış gibi komut verilmez. QEMU host exit code'u da guest Byte Counter'ın sonucu sayılmaz. Bu ayrım [xv6 shell kaynağı](https://github.com/mit-pdos/xv6-riscv/blob/9e3161a9abf5f51ea402562d1874caf6c4926597/user/sh.c) üzerinden gösterilir.

### Adım 5 — W3 için sözleşme

```text
Job:
  executable_path   çalıştırılacak dosyanın açık yolu
  argv              argv[0] dahil argümanlar; son eleman NULL

run(job) → RunResult:
  işin ve parent/child kimliklerinin kaydı
  başlatma hatası veya toplanmış süreç sonucu
  normal exit / sinyal / supervisor hatası için ayrı sonuç türleri
```

Bu hafta yalnız API taslağı ve sorumluluk gerekçesi yazılır. Gerçek çalışan parçalar host/guest Byte Counter'lardır. Sonuç türlerinin W3 ve W4'te hangi sırayla uygulanacağı [W3 sözleşmesinde](HAFTA03.md) açıklanır. Kullanılmayan bir stub başarı sonucu döndürerek tamamlanmış gibi gösterilemez.

Planlanan `make host-test`, `make guest-test`, `make evidence` hedefleri starter üretiminde eklenecektir. Bu dosyalar hazır olmadan ana öğretim deposunda bu komutların çalıştığı iddia edilmez.

## 8. Öğretmen soruları, karşı örnekler ve müdahale

| Yanılgı/soru | Müdahale ve beklenen açıklama |
| --- | --- |
| “İki yerde 4 çıktı; aynı sistem çağrıları kullanılmıştır.” | Aynı gözlenebilir sözleşmeyi farklı implementasyonların karşılayabileceğini göster. |
| “QEMU bir guest process'tir.” | Host süreç listesi ve guest shell'i ayrı çiz; gözlemin katmanını sor. |
| “Host dosyası guest'te zaten vardır.” | İmajda eksik fixture senaryosunu göster; dosya adını içerik kanıtından ayır. |
| “`sizeof(buffer)` kadar veri okundu.” | Kapasitesi 16, okuma sonucu 4 olan örnekte sayacı hesaplat. |
| “Proje ağacında `models/` varsa scheduling tamamdır.” | F etiketi ve gelecekteki kabul testini yazdır. |
| “Bir smoke test bütün OS projesini doğrular.” | Smoke'un yalnız çalıştırılan dar senaryoyu kapsadığını söylettir. |

**Kısa sözlü kartları:** Bir katman oku ve karşı örneğini ver; guest fixture kaybolursa hangi sınırı kontrol edersin; sayma koduna PID toplama eklemek hangi sorumlulukları karıştırır; aynı çıktı hangi bellek/performans iddialarını kanıtlamaz?

## 9. Teslim, geri bildirim ve süre

Teslim zamanı haftalık LMS paketinde açıkça yazılır. Paket şunları içerir:

1. W2 baseline/commit, kendi kod farkı ve yeniden üretim komutları.
2. W2-T1–T7 için beklenen/gözlenen sonuç ve ortam etiketi.
3. Bir sayfalık A1–A10 yapı haritası; her satırda kanıt ve katkı etiketi.
4. `Job`/sonuç taslağı, bir sınır kararı ve W5–W14 için on kısa backlog maddesi.
5. `CHANGELOG`, bilinen eksik ve AI/dış katkı açıklaması.
6. 2–4 dakikalık video veya eşdeğer erişilebilir açıklama: aynı işi iki ortamda göster, bir API farkını ve bir kanıt sınırını açıkla.

### Haftalık geri bildirim rubriği

Aşağıdaki 100 puan ölçeği bu paketin kalite geri bildirimidir; dersin toplam notuna yeni yüzde eklemez. W2 ürün doğruluğu W4'te ayrı bir özellik puanı olarak tekrar sayılmaz.

| Ölçüt | Puan |
| --- | ---: |
| Host davranışının ve W1 regresyonunun korunması | 25 |
| Guest smoke ve aynı fixture'ın doğrulanması | 20 |
| Katmanlar, A1–A4 doğruluğu ve katkı sınırları | 25 |
| Job/sonuç sorumluluğu ve gelecek artımların açıklığı | 15 |
| Yeniden üretim ve teknik gerekçe | 15 |
| **Toplam** | **100** |

Kısmi kanıt ilgili ölçütte değerlendirilir. Ortam hatası ile kavramsal hata ayrı geri bildirim alır. Video anlatımı ve varsa puanlanan sözlü, ana README'deki kendi ölçütleriyle ele alınır.

Ders dışı Core hedefi: host düzeltme 35, guest tekrar 40, test/kanıt 35, harita/sözleşme 35, belge/katkı 20 dakika; toplam yaklaşık 165 dakika. Hazırlık ve en çok 30 dakikalık video bu toplamdan ayrıdır. Ek kurulum yükü büyürse ortak laboratuvar ortamı sağlanır.

## 10. Exit ticket ve W3 köprüsü

Notlar/AI kapalı:

1. Aynı işi çalıştıran host ve guest yollarındaki iki somut farkı yaz.
2. Haritandaki bir O ve bir F satırını kanıtıyla belirt.
3. `run(job)` için henüz gerçekleşmemiş hangi davranışı W3'te ekleyeceksin?

**W3 başlangıç cümlesi:** “Şimdi bu yapının içinden gerçek bir iş geçireceğiz: çocuk süreci oluşturacak, programını yükleyecek, tamamlanmasını bekleyecek ve başarısızlığın hangi aşamada olduğunu göstereceğiz.”

## 11. English student handout — Week 2

**Focus:** Build A1–A4 (OS boundary, process, thread, IPC/descriptor) in depth through the structure of one growing system; the remaining six anchors stay on the map as future work for Weeks 3–4. Start from your Week 1 Byte Counter or a declared instructor baseline.

**Your task:** Move the host program into the supplied Workbench layout. Run the four Week 1 regression cases. Boot the supplied xv6 baseline and run the supplied guest adaptation with the same fixture bytes. Explain the differences between the Linux environment, the QEMU host process, the xv6 kernel and the guest user program.

**Required evidence:** Host sample = 4 bytes, host empty = 0, host missing-file exit = 1, host usage exit = 2; guest sample = 4 and guest empty = 0. Record the baseline and the fixture manifest. Guest application status, QEMU status and host program status are separate observations. Do not claim a launcher implementation this week.

**Design output:** Provide a one-page A1–A10 map and a `Job`/`RunResult` contract for Week 3. Mark observed, modelled and future behavior; distinguish upstream, supplied and student work. Add one future increment per anchor for Weeks 5–14.

**Preparation questions:** What do stdout, stderr and exit status tell you? Which OS interface does the guest program use? Why might the same C source need adaptation? How many bytes should be counted when a 16-byte read request returns 4? Does the same path identify the same file in both environments? Who should own future child-process results?

**Submission:** Code difference, test evidence, map, contract, changelog, limitations, contribution disclosure and one 2–4 minute explanation. An accessible equivalent is available. The feedback rubric allocates 25 points to host regression, 20 to guest evidence, 25 to the map and boundaries, 15 to the future contract and 15 to reproducibility and reasoning. These are not additional course-grade percentages.

**Tools and workload:** Begin studio with an individual AI-free attempt. Disclose assistance in permitted open work. Target about 165 minutes of out-of-class Core work, excluding preparation and up to 30 minutes for the explanation recording. Use the supplied environment if setup blocks progress; label supplied traces honestly.
