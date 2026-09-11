# CEN 302 — Küçük Prototipten Gerçek Çekirdek İncelemesine

**Dönem:** 14 hafta, `1 + 3 + 10`  
**Ürün:** Mini Systems Workbench — xv6 incelemesi ve sistem deneyleri  
**Belgeler:** [Ders sistemi](../README.md) · [İlk hafta akışı](HAFTA01.md) · [Kaynakça](KAYNAKCA.md)  
**Durum:** Uygulanacak proje tasarımıdır; aşağıdaki yeni kod, starter ve deney paketleri henüz hazırlanmış veya çalıştırılmış değildir. Kaynak araştırması: 10 Eylül 2026.

## Projenin amacı ve ana seçim

Öğrenci önce dosyadaki byte sayısını hesaplayan küçük bir program yapar. Sonra bu işi başlatan, yöneten, paralelleştiren, çıktılarını taşıyan, bellek ve dosya erişimini gözleyen bir çalışma tezgâhı geliştirir. Aynı iş yükünün gerçek çekirdekte hangi kod yolundan geçtiğini xv6 üzerinde izler ve küçük çekirdek artımları ekler. Dönem sonunda ortaya tekrar üretilebilir deneyleri, kaynak kodu bağlantıları ve açıklamaları olan bütünlüklü bir sistem ürünü çıkar.

Ana örnek **[MIT xv6-riscv](https://github.com/mit-pdos/xv6-riscv)** olacaktır. ANSI C ve sınırlı assembly ile yazılmış gerçek bir Unix-benzeri öğretim işletim sistemidir; QEMU içinde kendi çekirdeği ve kullanıcı programlarıyla açılır. [Kodla birlikte okunabilen kitabı](https://mit-pdos.github.io/xv6-riscv-book/) öğretim açısından belirleyici avantajdır. Saat donanımına gerek yoktur.

“Bütün OS kavramları hazır bulunan minimal sistem” iddiası kullanılmaz. xv6; sistem çağrıları, süreçler, sayfa tabloları, zamanlama, kilitler, pipe, dosya sistemi ve disk sürücüsü için güçlüdür. POSIX threads, kapsamlı sinyaller, swap/replacement politikaları, ağ yığını, gelişmiş erişim denetimi ve dağıtık sistemlerin tamamı standart tabanda bulunmaz. Bu boşluklar aşağıdaki planda gerçek Linux deneyi, mekanizma modeli, kontrollü xv6 artımı veya karşılaştırma kanıtıyla kapatılır.

## 1. hafta küçük proje: Byte Counter

**Girdi:** Tek bir dosya yolu. **Çıktı:** Dosyanın byte sayısı ve ayrı bir exit status. İlk sürüm Linux'ta çalışan C kullanıcı programıdır; kernel yazma önkoşulu yoktur.

Eğitmen dosya açma/kapatma, argüman kontrolü ve derleme iskeletini verir. Öğrenci yaklaşık 40 dakikalık uygulamada `fgetc`/EOF döngüsünü, sayaç güncellemesini ve hata kontrollerini tamamlar; kalan sürede test ve açıklama yapar. `int ch` EOF'u temsil eder; `ferror`, `fclose` ve çıktı flush sonucu kontrol edilir. Dosya binary modda açılır; UTF-8 karakteriyle byte aynı şey sayılmaz.

| Deney | Beklenen sonuç | Kavram |
| --- | --- | --- |
| LF ile biten `abc` dosyası | stdout `4`, exit 0 | A1 library/syscall; A9 dosya; A10 okuma |
| Boş dosya | stdout `0`, exit 0 | Sınır ve EOF |
| Dosya mevcut değil | stdout boş, stderr açıklama, exit 1 | Hata kanalı ve kaynak |
| Argüman yok veya fazladan argüman | stderr kullanım açıklaması, exit 2 | CLI sözleşmesi |
| UTF-8 `ş`, satır sonu yok | stdout `2`, exit 0 | Byte/karakter ayrımı; ek test |

Çıktı testi sayı ve satır sonunu denetler; stderr metninin tamamını işletim sistemine bağımlı hâle getirmez. `wc -c` karşılaştırma aracı olur. Çalıştırılan komutun exit status'ı hemen kaydedilir; sonraki komutun status'ıyla karıştırılmaz.

**Teslim:** Kod farkı, ilk dört test, A1–A10 haritası, 60–90 saniyelik açıklama. A3/A5/A7/A8 gibi bu prototipte uygulanmamış mekanizmalar panorama modeli olarak işaretlenir. Bu kod dönem projesinde korunur; ileride worker olarak kullanılır.

## Büyüyen ürünün mimarisi

```text
Mini Systems Workbench
├── host/          Linux C: byte counter, launcher, pthread deneyleri
├── xv6/           Sabit upstream commit + öğrencinin kernel/user değişiklikleri
├── models/        Scheduling, replacement ve disk erişimi öğretim modelleri
├── fixtures/      Ortak küçük dosyalar ve belirli iş yükleri
├── tests/         Host testleri, QEMU oturum denetimi, regresyon
└── evidence/      A1–A10 ve OSC kapsamı; trace, ölçüm, kaynak ve kararlar
```

Bu ağaç hedef yapıdır; üç ayrı dönem projesi değildir. Aynı `bytecount` iş yükü ve test niyeti host ile xv6 üzerinde sürer. xv6 küçük C kütüphanesi kullandığından host'un `fgetc` kodu doğrudan kopyalanmaz; W2'de verilen `open/read/close` uyarlama katmanı kullanılır. Linux sinyal/`waitpid` sözleşmesi ile xv6 `wait/kill` sözleşmesi ayrı kaydedilir.

Planlanan ortak komutlar `make host-test`, `make guest-test` ve `make evidence` olur; starter üretiminde eklenecek hedeflerdir. Upstream xv6'nın gerçek başlangıç komutu, gerekli RISC-V toolchain ve QEMU kurulduktan sonra `make qemu`dur. [Upstream kurulum açıklaması](https://github.com/mit-pdos/xv6-riscv).

## Eğitmenin sağlayacağı taban ve kapsam

- Dönem başlamadan Linux ortamı, QEMU ve RISC-V toolchain doğrulanır; x86-64/ARM64 ana makineler için çalıştırma yolu belgelenir. Gerçek kart zorunlu değildir.
- xv6 commit'i, araç sürümleri ve bilinen özellikleri `BASELINE` kaydında sabitlenir. `riscv` dalı zamanla değişir; eski MIT ödevinin “eksik” kabul ettiği özellik yeni tabanda zaten bulunabilir.
- W1 için C iskeleti; W2 için QEMU açılış paketi ve guest bytecount uyarlaması; W5/W11 için boş syscall/diagnostic arayüzleri; W13 için disk-kopyası ve hata-enjeksiyon düzeneği sağlanır.
- Standart kullanıcı programları ve çekirdek öğrencinin eseri sayılmaz. Her artımda upstream/supplied/student ayrımı ve değişiklik gerekçesi bulunur.
- Normal haftalık ders dışı Core işi yaklaşık üç saate sığar. Kernel altyapısı eğitmence hazırlanır; her hafta hem büyük Linux ödevi hem büyük kernel ödevi verilmez. Stretch işleri Core'un yerine geçmez.

## Hafta hafta gelişim

### W1 — Panorama ve Byte Counter

**Amaç/artım:** Yukarıdaki dört durumluk prototip. **Kavram:** A1–A10 panoraması; gerçek uygulama A1/A4/A9/A10. **Kanıt:** Sayı, stderr ve exit ayrımı; her çapaya gözlem/model/gelecek etiketi. **Sonraki bağ:** Dosya okuyan iş, W2–W4 launcher'ının worker'ı olur.

### W2 — Bütün sistemin yapısı ve çalışan iskelet

**Amaç/artım:** W1 kodunu `host/` içine yerleştir; verilen xv6 tabanını aç, guest bytecount'u derle ve ortak fixture ile çalıştır. CLI → worker → OS sınırını çiz. A1–A10'un tamamına ilgili kod veya gelecekteki genişleme noktasını ekle; yapı turunda on konu bölünmez.

**Kanıt:** Host ve guest'te aynı byte sayısı; bir smoke test; monolithic kernel, user/kernel modu, koruma alanı ve veri/kontrol akışı haritası. **Sınır:** Bu hafta syscall veya page table tasarımı yazılmaz. **Sonraki bağ:** Launcher API'si `run(job) → result` olarak tanımlanır.

### W3 — Bütün sistemin davranışı ve hata yolu

**Amaç/artım:** Linux launcher'ı `fork/exec/waitpid` ile byte counter'ı çalıştırır; `exec` başarısızlığı ve normal tamamlanma görünür olur. xv6'daki karşılıklar izlenir; adres uzayı, descriptor mirası, zamanlama, bellek ve dosya yolu birlikte A1–A10 turuna bağlanır.

**Kanıt:** Normal, missing executable, worker failure olmak üzere üç iz; parent/child kimlikleri ve doğru wait. **Regresyon:** W1 dört testi. **Sonraki bağ:** Sonucun tek tip raporlanması ve cleanup sözleşmesi.

### W4 — Entegrasyon: `v0.1`

**Amaç/artım:** Launcher Linux'ta exit ile sinyal sonlanmasını ayırır, çocuklarını toplar ve raporlar. xv6 guest testi ayrı exit sözleşmesiyle çalışır. On çapada gerçek özellikler ve backlog tekrar savunulur.

**Kanıt:** Temiz kurulum, başarılı/hatalı/sinyalli host çalıştırma, tekrar sonrası zombie kalmaması, guest boot/bytecount testi, A1–A10 haritası. Bu ilk ürün kapısıdır. **Sonraki bağ:** W5 ölçümü için sürümlü sonuç kaydı.

### W5 — A1: Syscall ve koruma sınırı

**Artım:** Verilen arayüzle xv6'ya sınırlı bir süreç istatistiği syscall'ı ekle; örneğin çağıranın tamamlanan `read` byte toplamı. Host tarafında seçilmiş `strace` satırlarını rapora bağla. **Geri çağır:** A2 süreç sahipliği, A4 descriptor.

**Kanıt:** Kullanıcı programının istediği sayaç, kontrollü okuma ile değişir; geçersiz kullanıcı işaretçisi varsa kernel'i düşürmeden reddedilir. Library call/syscall/trap/hardware interrupt ayrımı ve küçük tehdit modeli yazılır. **Regresyon:** Launcher ve guest boot. **Stretch:** Syscall filtreleme.

### W6 — A2: Süreç yaşam döngüsü

**Artım:** Launcher'a en fazla N eşzamanlı child ve her PID için sonuç toplama ekle. xv6 `proc.c` üzerinden oluşturma, bekleme ve temizleme yolunu kaynak konumlarıyla göster. **Geri çağır:** A1 sınır, A6 CPU paylaşımı.

**Kanıt:** N sınırı aşılmaz, biten her çocuk bir kez toplanır; kısa ömürlü çocuk testinde sonuç kaybolmaz. Linux/BSD süreç modeli ve timer/preemption karşılaştırması aynı kanıt dosyasına eklenir. **Sonraki bağ:** Süreç içinde daha hafif worker gereksinimi.

### W7 — A3: Thread ve paylaşılan durum

**Artım:** Host byte counter'a verilen worker-pool iskeletiyle çok dosyalı `pthread` yolu ekle; thread-local sonuçları `join` sonrası topla. **Geri çağır:** A2 process, A5 paylaşım doğruluğu.

**Kanıt:** Seri ve threaded toplam aynı; deterministik fixture ile test, multicore kazanımı hakkında ölçüm sınırı. Standart xv6'daki her process'in kernel yürütüm bağlamı ile POSIX kullanıcı thread'i eşitlenmez. **Sonraki bağ:** Paylaşılan kuyruk/race düzeneği W9'a hazırlanır; tam pthread katmanı xv6'ya eklenmez.

### W8 — A4: IPC ve descriptor ömrü

**Artım:** Guest'te producer → pipe → consumer akışı kur; host fixture'ıyla aynı sayı sonucunu al. Parent ve child gereksiz pipe uçlarını kapatır. **Geri çağır:** A2 yaşam döngüsü, A10 I/O.

**Kanıt:** Veri kaybı yok, son writer kapanınca EOF gelir, açık kalan uç örneği neden bekletir açıklanır. Descriptor/VFS arayüzü, ağdaki message/RPC ve Mach message-passing sınırı karşılaştırılır. **Regresyon:** W1 sonuçları. **Stretch:** Loopback socket uyarlaması.

### W9 — A5: Senkronizasyon ve deadlock

**Artım:** W7'deki hazır bounded-buffer kuyruğunun mutex/condition-variable TODO'larını tamamla; kilit sırası belirle. xv6 spinlock/sleep-lock ayrımı kaynak kodunda incelenir. **Geri çağır:** A3 paylaşım, A4 backpressure.

**Kanıt:** Üretilen=tüketilen, yinelenen/kayıp iş yok; bekleme koşulu `while` ile yeniden kontrol edilir. Stress testi tek başına race yokluğu ispatı sayılmaz. Readers–writers, philosophers ve dört deadlock koşulu kısa izlerle; prevention/avoidance/detection/recovery kararları tabloyla kapsanır. **Sonraki bağ:** Bekleme/CPU süreleri W10 raporuna girer.

### W10 — A6: Zamanlama ve politika

**Artım:** Aynı işlerin burst/arrival kayıtlarını alan verilen modelde FCFS ve RR karşılaştırmasını tamamla; waiting/turnaround/response raporu ekle. Eğitmenin sağladığı xv6 scheduler trace'ini bu rapora bağla. **Geri çağır:** A2 process state, A5 blocked/runnable ayrımı.

**Kanıt:** Küçük örneğin elle hesabı modelle eşleşir; model zamanı ile host duvar saati karıştırılmaz. Starvation, priority inversion, real-time deadline ve multicore perspektifi; Windows 10 tarihsel politika vakası. **Stretch:** xv6 scheduling policy değişikliği; zorunlu kapsam ölçüm ve karşılaştırmadır.

### W11 — A7: Adres çevirisi ve protection

**Artım:** xv6'ya verilen diagnostic sınır içinde page-table yazdırma ekle; `walk` yolunu ve R/W/X/U bayraklarını izle. **Geri çağır:** A1 ayrıcalık, A2 adres uzayı.

**Kanıt:** Sayfa/offset hesabı, geçersiz erişim reddi, parent/child eş adreslerin farklı fiziksel eşlemeleri. Paging/segmentation, allocation ve access matrix/ACL/capability modelleri uygun karşılaştırmayla eklenir. **Regresyon:** Boot ve kullanıcı programları. **Sonraki bağ:** W12 fault sayacı.

### W12 — A8: Sanal bellek, fault ve izolasyon

**Artım:** Sabit xv6 tabanının mevcut lazy allocation yoluna fault/ayrılan sayfa gözlemi ekle; ayrılmış fakat dokunulmamış alan ile dokunulan sayfaları karşılaştır. Erişim sınırı testi ekle. **Geri çağır:** A7 translation, A6 baskı altında ilerleme.

**Kanıt:** Tabanın hangi fault'u çözdüğü; ilk dokunmanın etkisi; geçersiz erişimin diğer süreci bozmaması. COW ile eager-copy karşılaştırması eğitmen düzeneğinde, replacement/thrashing OSTEP modelinde; container/VM/hypervisor farkı kanıt haritasında. **Stretch:** Hazır laboratuvar tabanında COW uygulaması. Swap bulunduğu varsayılmaz.

### W13 — A9: Dosya sistemi ve crash consistency

**Artım:** Job raporunun guest dosyasına yazılmasını ve yeniden açılmasını ekle. Verilen hata-enjeksiyon düzeneğinde log commit öncesi/sonrası konuk sistem kesintisini seçip kopya disk üzerinde recovery gözlemi üret. **Geri çağır:** A4 descriptor, A8 cache/bellek.

**Kanıt:** Path → inode → block → log sırası; yalnız ilgili transaction için eski/yeni tutarlı durum. Çoklu syscall'dan oluşan uygulama güncellemesinin otomatik atomik olmadığı açıklanır. Allocation/free-space, mounting/sharing, Linux VFS ve BSD farkları haritada bulunur. **Regresyon:** Bytecount dosya testleri.

### W14 — A10: Depolama, I/O ve `v1.0`

**Artım:** Verilen kernel ölçüm kancalarıyla aynı işin block I/O sayılarını raporla; buffer boyutu ve cache etkisini ayır. Önceki artımları tek demo/kanıt kataloğunda birleştir. **Geri çağır:** A6 zamanlama, A9 kalıcılık.

**Kanıt:** Tekrarlı örneklerin ham verisi, ortamı ve varyasyonu; UART/virtio interrupt yolu, DMA kavramsal izi, HDD/SSD/RAID karşılaştırması. QEMU sonucu gerçek SSD throughput'u olarak sunulmaz. Ağ/dağıtık hata senaryosu ve tarihsel vaka kapsamı portföyle tamamlanır. **Final:** Tek işin launch → IPC/thread → bellek → dosya → I/O yolunu savun; birikimli regresyonu çalıştır.

## Gerçek xv6 kaynak kodu okuma haritası

Araştırmada `riscv` dalı `9e3161a9abf5f51ea402562d1874caf6c4926597` commit'ini gösteriyordu. [Bu sabit kernel ağacı](https://github.com/mit-pdos/xv6-riscv/tree/9e3161a9abf5f51ea402562d1874caf6c4926597/kernel) inceleme başlangıcıdır; ders tabanı olarak kabul edilmeden önce boot ve regresyon kontrollerinden geçirilir. Bu belge hazırlanırken çekirdek derlenmedi.

| Çapalar | Kaynak dosyaları | İzlenecek ilişki |
| --- | --- | --- |
| A1 | `syscall.c`, `sysproc.c`, `trap.c`, `trampoline.S` | Kullanıcı çağrısından kernel girişine ve dönüşe |
| A2, A6 | `proc.c`, `proc.h`, `swtch.S` | Process state, context switch, scheduler, wait/wakeup |
| A3, A5 | `spinlock.c`, `sleeplock.c`, `proc.c` | Paylaşılan kernel state ve kilit/bekleme protokolü |
| A4 | `pipe.c`, `file.c`, `sysfile.c` | Descriptor, pipe ve referans ömrü |
| A7, A8 | `vm.c`, `kalloc.c`, `riscv.h`, `trap.c` | Page table, allocation, fault ve izin |
| A9 | `fs.c`, `log.c`, `bio.c` | Inode/block, log commit, buffer cache |
| A10 | `virtio_disk.c`, `uart.c`, `plic.c` | Aygıt isteği, interrupt ve tamamlanma |

10 Eylül 2026'da incelenen [vm.c](https://github.com/mit-pdos/xv6-riscv/blob/riscv/kernel/vm.c) zaten `vmfault` yolu içeriyordu. Dolayısıyla W12 “lazy allocation sıfırdan eklenecek” diye sabitlenmez; tabanın mevcut davranışı genişletilir. [proc.c](https://github.com/mit-pdos/xv6-riscv/blob/riscv/kernel/proc.c) içindeki kernel işlev adları ile kullanıcı API adları da sürüme göre ayrı doğrulanır.

## OSC kapsamının tamamı projeye nasıl bağlanır?

On çapa, kitabın her bölümünün ayrı kernel özelliği olarak yazılacağı anlamına gelmez. Aşağıdaki konular **zorunlu kanıt kapsamındadır**; kısa okumalar ve izler mevcut haftalık paketin içine girer. [OSC 10. baskı bölüm listesi](https://www.os-book.com/OS10/slide-dir/index.html).

| Bölüm / ek | Hafta | Ürün veya portföy kanıtı |
| --- | --- | --- |
| 1 Introduction; A Influential OS | W1, W14 | OS amacı ve Unix→xv6 tasarım çizgisi |
| 2 Structures | W2, W5 | Syscall, kernel yapısı ve sınır haritası |
| 3 Processes | W3, W6 | Launcher ve process trace |
| 4 Threads & Concurrency | W7 | Thread/process karşılaştırması ve worker sonuçları |
| 5 CPU Scheduling | W6, W10 | Timer/preemption ve politika raporu |
| 6–8 Synchronization / Examples / Deadlocks | W5, W7, W9 | Kilit, klasik problemler ve deadlock karşı örnekleri |
| 9 Main Memory | W2, W11 | Translation, paging/segmentation ve allocation |
| 10 Virtual Memory | W12 | Fault, COW, replacement ve thrashing kanıtları |
| 11 Mass Storage; 12 I/O | W8, W14 | Driver/buffer/DMA ve storage ölçüm sınırı |
| 13–15 File System Interface / Implementation / Internals | W8, W13 | Path, inode, allocation, sharing, recovery/VFS |
| 16 Security; 17 Protection | W2, W5, W11, W12 | Tehdit modeli, auth/izin, erişim matrisi ve izolasyon |
| 18 Virtual Machines | W12, W14 | Guest/host, hypervisor/container farkı |
| 19 Networks & Distributed Systems | W8, W14 | Pipe→message/RPC; timeout, yeniden deneme ve kısmi hata |
| 20 Linux; C BSD UNIX | W6, W13 | Süreç ve filesystem karşılaştırması |
| 21 Windows 10; B Windows 7 | W10, W14 | Tarihsel mimari, scheduling, memory/I/O karşılaştırması |
| D Mach | W8, W12 | Message-passing ve microkernel sınırları |

Windows 7/10 kitabın tarihsel vaka adlarıdır; güncel destek veya ürün önerisi sayılmaz. Final kapsam matrisi 1–21 ve A–D'yi tek tek açar; her satırda hafta, kaynak, öğrencinin kanıtı ve sınırı bulunur.

## Alternatif açık kaynak sistemler

| Aday | Güçlü yanı | Ders açısından sınırı ve rolü |
| --- | --- | --- |
| **xv6-riscv — ana seçim** | Küçük gerçek Unix çekirdeği, kodla eşleşen kitap, QEMU | Üretim OS özelliklerinin tamamı hazır değil; en dengeli inceleme tabanı |
| **[FreeRTOS-Kernel](https://github.com/FreeRTOS/FreeRTOS-Kernel)** | Task, queue, semaphore, timer; MIT lisanslı küçük RTOS çekirdeği | Tam Unix süreç, VM ve filesystem öğretimini tek başına karşılamaz; A3/A5/A6 karşılaştırması |
| **[Apache NuttX](https://github.com/apache/nuttx)** | POSIX odaklı gömülü RTOS, daha geniş sistem API yüzeyi | Flat/protected/kernel build ve bellek özellikleri hedefe bağlı; xv6'dan daha çok yapılandırma |
| **[Zephyr](https://github.com/zephyrproject-rtos/zephyr)** | RTOS, sürücüler, ağ ve yapılandırılabilir user mode | VM için MMU/build desteği gerekir; demand paging ayrıca etkinleştirilir; kapsamlı ama başlangıçta daha büyük |
| **[InfiniTime](https://github.com/InfiniTimeOrg/InfiniTime)** | Gerçek PineTime firmware'i, FreeRTOS görevleri, UI/BLE/sensör entegrasyonu | Saat zorunlu değil; [InfiniSim](https://github.com/InfiniTimeOrg/InfiniSim) UI demosu sağlar, donanım zamanlamasını doğrulamaz |
| **[wasp-os](https://github.com/wasp-os/wasp-os)** | MicroPython ile anlaşılır saat uygulamaları | README bakımcı arandığını ve eski binary'lerin geri çekildiğini bildiriyor; zorunlu dönem tabanı için seçilmedi |

NuttX'in [bellek yapılandırmaları](https://nuttx.apache.org/docs/latest/implementation/memory_configurations.html) ve Zephyr'in [VM belgesi](https://docs.zephyrproject.org/latest/kernel/memory_management/virtual_memory.html), “bu RTOS'ta VM var/yok” genellemesinin hedef belirtilmeden yapılmaması gerektiğini gösterir. Zepp OS'un [açık geliştirici portalı](https://docs.zepp.com/docs/intro/) mini program ve watchface geliştirmesini belgeler; bu, değiştirilebilir açık kaynak kernel bulunduğunun kanıtı değildir.

## Kabul kapıları ve bireysel başarı

- **W1:** Dört prototip testi; kendi kodu ile model/preview ayrımı.
- **W4 `v0.1`:** Host launcher, guest smoke test, hata/cleanup ve on çapa haritası.
- **W5–W14:** Haftanın artımı + ilgili regresyon + bir karşı örnek + kaynak kodu konumu + harita farkı. Desteklenmeyen mekanizmalar “gerçekleştirildi” yazılmaz.
- **W14 `v1.0`:** A1–A10 ve OSC tam kapsamı, yeniden üretilebilir komutlar, kaynak/öğrenci katkısı ayrımı, birikimli testler ve bireysel mimari savunma.

Eğitmen W4 ve gerektiğinde W9 kurtarma tabanı sunar. Öğrenci kullandığı tabanı ve kendi eksiklerini açıklar; sonraki haftalara katılım sürer. Büyük başarı göstergesi, boot ekranı göstermekten öte, bir programın neden beklediğini, hata verdiğini veya farklı maliyetle çalıştığını kaynak ve deney üzerinden açıklayabilmektir.
