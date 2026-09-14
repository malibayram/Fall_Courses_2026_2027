# CEN 302 — Çekirdek ve Güncel İşletim Sistemleri Kapsamı

**Dönem:** 2026–2027 Güz  
**Ana ürün:** Mini Systems Workbench  
**İlgili belgeler:** [Proje planı](PROJE.md) · [Kaynakça](KAYNAKCA.md)

Bu belge klasik işletim sistemi mekanizmalarını güncel Linux ve bulut çalışma zamanı konularıyla ilişkilendirir. Amaç öğrenciyi araç isimleriyle boğmak değil; yeni bir mekanizmayı **koruma sınırı, kaynak sahipliği, eşzamanlılık, hata ve ölçüm** açısından çözümleyebilir hâle getirmektir.

## Öğrenme derinliği

| Kod | Beklenen kanıt |
| --- | --- |
| **T — Temel** | Mekanizmayı zaman çizgisi, durum makinesi, adres/izin hesabı veya küçük modelle açıklar ve uygular. |
| **K — Karşılaştırmalı laboratuvar** | Sağlanan güvenli düzenekte iki davranışı çalıştırır, ölçer ve sınırlarını raporlar. |
| **P — Panorama** | Resmî belge veya sabit trace üzerinden mimari yeri, kullanım koşulu ve riski açıklar. |

## Kapsam haritası

| Problem ailesi | Klasik çekirdek | Güncel bağlantı | Düzey |
| --- | --- | --- | ---: |
| Kernel yapısı ve koruma | User/kernel mode, trap, syscall, interrupt, monolithic/microkernel | Linux capabilities; seccomp ve Landlock sandbox sınırı; eBPF'nin sandboxed kernel extension/instrumentation rolü | T/K/P |
| Süreç ve thread | `fork/exec/wait`, context switch, lifecycle, POSIX threads | PID/user/mount/network namespaces; `pidfd` fikri; service/container process tree | T/K/P |
| Eşzamanlılık | Race, mutex, semaphore, condition variable, deadlock | C11 atomics ve memory ordering sezgisi; futex'in kullanıcı/kernel bekleme sınırı; multicore cache etkisi | T/K |
| Zamanlama ve kaynak | FCFS, RR, priority, MLFQ, fairness, real-time | cgroup v2 CPU/memory/I/O/pids denetleyicileri; container limiti ile scheduler policy ayrımı | T/K |
| Bellek | Address translation, page table, TLB, allocation, paging, COW, replacement | Huge page/NUMA panoraması; memory pressure/oom; namespace'in kaynak limiti olmadığı | T/K/P |
| IPC ve olaylı I/O | Pipe, signal, shared memory, socket/RPC, blocking I/O | Event loop ve readiness; `io_uring` submission/completion modeli; backpressure ve cancellation | T/K/P |
| Dosya sistemi ve kalıcılık | Inode, directory, allocation, cache, journaling/log, crash consistency | Copy-on-write filesystem panoraması; container overlay katmanı; durability ile görünürlük ayrımı | T/K/P |
| Depolama ve aygıt | Interrupt, DMA, buffer/cache, HDD/SSD/RAID | NVMe queue ve async I/O panoraması; sanallaştırılmış I/O ölçümünün fiziksel donanım iddiası olmaması | T/K/P |
| Sanallaştırma ve izolasyon | VM, hypervisor, privilege, protection | Container = namespaces + cgroups + güvenlik katmanları; VM/container/Wasm sınırları | T/K |
| Gözlemlenebilirlik | Counter, log, trace, profiling ve ölçüm yanlılığı | `strace`, `/proc`, `perf`; eBPF trace örneği; düşük-overhead iddiasının ölçüm istemesi | K/P |

## Güncel köprülerin haftalara yerleşimi

| Hafta | Güncel köprü | Öğrenci kanıtı | Kapsam sınırı |
| ---: | --- | --- | --- |
| 5 | Syscall gözlemi, capabilities, seccomp/eBPF yeri | `strace` izi + izin/tehdit tablosu | Öğrenci kernel'e rastgele BPF programı yüklemek zorunda değildir. |
| 6 | Process tree, namespace ve `pidfd` fikri | Host/namespace PID görünümü verilen trace'te karşılaştırılır | Namespace tek başına kaynak veya güvenlik garantisi değildir. |
| 7 | Threads, atomics ve memory order | Mutex'li çözüm + küçük atomik/yanlış paylaşım karşı örneği | Lock-free algoritma geliştirmek Core değildir. |
| 8 | Pipe/socket ile readiness ve `io_uring` modeli | Blocking/event/asenkron akış diyagramı; cancellation/backpressure kararı | Donanıma özel zero-copy deneyi yapılmaz. |
| 9 | Mutex/condition/futex sınırı | Bekleme yolunun kullanıcı ve kernel bölümünü trace üzerinde açıklar | Stress testi race yokluğunun ispatı değildir. |
| 10 | Scheduler ve cgroup v2 | Aynı CPU işinde normal ve sınırlı kaynak trace'i; fairness/limit ayrımı | Container orchestrator kurulmaz. |
| 11 | Page permissions, capabilities, seccomp ve Landlock | “Kim, hangi nesneye, hangi işlem?” erişim matrisi + reddedilen işlem | Sandbox mutlak güvenlik iddiası değildir. |
| 12 | VM–container–Wasm izolasyonu | Aynı iş yükü için sınır, kaynak ve tehdit karşılaştırması | Üç runtime'ın tamamını kurmak gerekmez. |
| 13 | Crash consistency ve katmanlı dosya görünümü | Journal/log ile overlay/COW kavramlarını ayıran hata çizgisi | Üretim filesystem benchmark'ı değildir. |
| 14 | `perf`/eBPF gözlemi, async/NVMe panoraması | Bir trace'in kaynağı, overhead'i ve desteklemediği iddia | Tek QEMU ölçümü gerçek donanım performansı sayılmaz. |

## Verimli dönem ilkeleri

- xv6 mekanizmayı okunabilir kılmak, Linux ise üretim karşılığını gözlemek için kullanılır; biri diğerinin davranışı diye sunulmaz.
- Her hafta yalnız bir Core değişiklik yapılır. Güncel köprü, aynı değişikliğin trace'i veya karar kartıdır; ikinci büyük proje değildir.
- Root yetkisi, özel kernel, bulut hesabı veya fiziksel kart zorunlu değildir. Yetki gerektiren deney için eğitmenin sabit trace'i sağlanır.
- Performans görevi “daha hızlı yap” değildir: iş yükü, ortam, baseline, ham ölçüm, varyasyon ve gözlem overhead'i birlikte kaydedilir.
- Güvenlik başlıkları yalnız özellik adıyla geçmez; tehdit, korunan nesne, yetki, beklenen ret ve kalan risk yazılır.

## Asgari dönem sonu yeterliği

Öğrenci:

- syscall, process/thread, synchronization, scheduling, virtual memory, filesystem ve I/O mekanizmalarını uçtan uca izler;
- race, deadlock, starvation, page fault, partial I/O ve crash gibi hata yollarını küçük karşı örnekle açıklar;
- VM, container ve process izolasyonunu; namespaces, cgroup v2 ve güvenlik politikalarının farklı rollerini ayırır;
- blocking, readiness ve asynchronous I/O modellerini karşılaştırır;
- `strace`, `/proc`, `perf` veya sağlanan eBPF trace'inden sınırlı ve yeniden üretilebilir bir iddia çıkarır;
- görmediği yeni bir OS özelliğini durum, koruma, kaynak, eşzamanlılık, hata ve gözlem eksenlerinde sınıflandırır.

## Birincil güncel teknik dayanak

- [Linux kernel cgroup v2 documentation](https://www.kernel.org/doc/html/latest/admin-guide/cgroup-v2.html)
- [Linux kernel eBPF userspace API](https://www.kernel.org/doc/html/latest/userspace-api/ebpf/index.html)
- [Linux kernel Landlock documentation](https://www.kernel.org/doc/html/latest/userspace-api/landlock.html)
- [Linux man-pages namespaces overview](https://man7.org/linux/man-pages/man7/namespaces.7.html)
- [liburing repository and examples](https://github.com/axboe/liburing)

