# CEN 302 Operating Systems — Seçilmiş Güncel Kaynakça

**Son kontrol:** 10 Eylül 2026  
**Kullanım ilkesi:** Aşağıdaki ilk beş kaynak dersin çekirdeğidir. Diğerleri ihtiyaca göre başvurulacak destek kaynaklarıdır; hiçbir ücretli kaynak zorunlu değildir.

[Güncel kapsam](GUNCEL_KAPSAM.md) · [İlk hafta](HAFTA01.md) · [Proje planı](PROJE.md)

Güncel resmî belgeler teknik davranışı doğrulamak, eski ama geçerli dersler temel kavramları çalışmak içindir. Devam eden 2026 derslerinin sonraki materyalleri henüz yayımlanmamış veya önceki dönemden aktarılmış olabilir. Bağlantı erişimi, ücretli kurs içeriğinin bütünüyle incelendiği anlamına gelmez.

## Önce bunları kullanın

1. **[Operating Systems: Three Easy Pieces (OSTEP)](https://pages.cs.wisc.edu/~remzi/OSTEP/)** — Ücretsiz ders kitabı; işletim sistemlerini virtualization, concurrency ve persistence eksenlerinde açıklar. Dersin kavramsal omurgası ve haftalık okuma için ilk başvuru kaynağıdır.
2. **[OSTEP Homework / Simulators](https://pages.cs.wisc.edu/~remzi/OSTEP/Homework/homework.html)** · **[GitHub deposu](https://github.com/remzi-arpacidusseau/ostep-homework)** — Scheduling, MLFQ, paging, TLB, RAID ve dosya sistemleri gibi mekanizmaları küçük parametrelerle deneyip sonucu tahmin etmeyi sağlar. Derste “tahmin et → çalıştır → açıkla” döngüsü için en yararlı koleksiyondur.
3. **[MIT 6.1810 Operating System Engineering — Fall 2026](https://pdos.csail.mit.edu/6.828/2026/overview.html)** · **[ders takvimi](https://pdos.csail.mit.edu/6.S081/2026/schedule.html)** — xv6 üzerinden süreç, sanal bellek, trap, kilit, dosya sistemi ve ağ konularını gerçek çekirdek koduyla birleştirir. CEN 302 için ileri laboratuvar ve proje örneği olarak kullanılacaktır.
4. **[xv6: a simple, Unix-like teaching operating system](https://mit-pdos.github.io/xv6-riscv-book/)** · **[xv6-riscv kodu](https://github.com/mit-pdos/xv6-riscv)** — Küçük ama gerçek bir işletim sisteminin kitap ve kaynak kodunu yan yana sunar. Özellikle syscall, process, page table, locking ve file-system bölümlerinde seçilmiş parçalar okunmalıdır.
5. **[Linux man-pages](https://man7.org/linux/man-pages/)** — Linux sistem çağrıları ve C kütüphanesi arayüzleri için birincil başvuru kaynağıdır. Kod yazarken bloglardan önce ilgili `man 2`/`man 3` sayfasına bakma alışkanlığı kazandırır.

## Ders kitabı ve sistematik okuma

- **[Operating System Concepts, 10th Edition — companion site](https://www.os-book.com/OS10/)** — Dersin geniş kapsam referansıdır; sitede bölüm yapısı, alıştırmalar, çalışma rehberi, slaytlar ve örnek kodlar bulunur. Ayrıntılı kapsama veya alternatif açıklamaya ihtiyaç duyulduğunda kullanılır.
- **[The Linux Programming Interface](https://www.man7.org/tlpi/index.html)** · **[çevrimiçi örnek kod](https://www.man7.org/tlpi/code/online/index.html)** — Michael Kerrisk’in Linux/UNIX sistem programlama referansı; süreçler, sinyaller, dosya I/O, thread ve IPC için güçlü bir uygulama kaynağıdır. Kitap ücretlidir, örnek kodlar ücretsizdir.
- **[Linux System Programming Essentials — ücretsiz ders notu](https://man7.org/training/download/Linux_System_Programming_Essentials-mkerrisk_man7.org.pdf)** — Sistem çağrısı, dosya, süreç ve sinyal konularında kısa ve güncel bir uygulama notudur. Özellikle ilk haftalarda hızlı başvuru için uygundur.
- **[The Little Book of Semaphores](https://greenteapress.com/wp/semaphores/)** — Semaphore ve klasik concurrency problemlerini problem çözme yoluyla öğretir. A5 senkronizasyon/deadlock modülünde seçilmiş bulmacalar kullanılabilir.

## Uygulama, laboratuvar ve simülasyon

- **[OSTEP Projects](https://github.com/remzi-arpacidusseau/ostep-projects)** — C/Linux tabanlı başlangıç, process, concurrency ve file-system projeleri içerir. Tam projeyi kopyalamak yerine dersin öğrenme hedefine uygun küçük alt görevler seçilmelidir.
- **[QEMU Documentation](https://www.qemu.org/docs/master/)** · **[RISC-V system emulator](https://www.qemu.org/docs/master/system/target-riscv.html)** — xv6 gibi konuk sistemleri güvenli ve tekrar üretilebilir biçimde çalıştırmak için resmi kaynaktır. Sanal makine ile container arasındaki farkı da somutlaştırır.
- **[Nand2Tetris](https://www.nand2tetris.org/)** · **[araçlar ve çevrimiçi IDE](https://www.nand2tetris.org/software)** — Donanımdan derleyici ve işletim sistemine kadar bilgisayar sistemini katmanlı biçimde kurdurur. CEN 302 için özellikle machine/VM/OS sınırlarını anlamada tamamlayıcıdır.
- **[MIT PDOS GitHub](https://github.com/mit-pdos)** — xv6 yanında araştırma amaçlı sistem projelerini barındırır. Dönemin ilerleyen haftalarında “öğretim sistemi ile üretim/araştırma sistemi nasıl farklılaşır?” sorusu için örnek havuzudur.
- **[UC Berkeley CS 162 — Fall 2026](https://cs162.org/)** — Threads, synchronization, scheduling, virtual memory, file systems ve distributed systems içeren güçlü bir ders rotasıdır. Açık not ve ödevler kullanılabilir; bazı ders video bağlantıları kurum oturumu isteyebilir.

## Gerçek işletim sistemi örnekleri

- **[xv6-riscv](https://github.com/mit-pdos/xv6-riscv)** — Ana çekirdek örneğimizdir; kaynak koduna bağlantılı [kitap](https://mit-pdos.github.io/xv6-riscv-book/) syscall, process, page table, scheduler, kilit ve dosya sistemini izletir. POSIX threads ve tam swap sistemi hazır varsayılmaz.
- **[Zephyr](https://github.com/zephyrproject-rtos/zephyr)** ve **[Apache NuttX](https://github.com/apache/nuttx)** — Gerçek RTOS alternatifleridir; bellek/süreç özellikleri mimari ve yapılandırmaya bağlıdır. xv6'ya göre daha geniş sürücü ve yapılandırma yüzeyi vardır.
- **[InfiniTime](https://github.com/InfiniTimeOrg/InfiniTime)** · **[InfiniSim](https://github.com/InfiniTimeOrg/InfiniSim)** — FreeRTOS tabanlı saat firmware'i ve masaüstü arayüz simülatörüdür. InfiniSim gerçek donanım zamanlamasının kanıtı değildir.
- **[FreeRTOS-Kernel](https://github.com/FreeRTOS/FreeRTOS-Kernel)** — Task, queue, semaphore ve timer kodunu incelemek için küçük bir RTOS çekirdeğidir; dosya sistemi ve Unix süreç modelini tek başına sağlamaz.

Ayrıntılı seçim ve haftalık kaynak kodu haritası [proje planındadır](PROJE.md).

## Performans ve gerçek Linux gözlemi

- **[Brendan Gregg — Linux Performance](https://www.brendangregg.com/linuxperf.html)** — CPU, memory, disk ve network gözlemi için yöntem, araç ve diyagramları bir araya getirir. Tek ölçümden iddia üretmek yerine kapsam, darboğaz ve ölçüm yanlılığını tartışmak için değerlidir.
- **[Linux kernel documentation](https://www.kernel.org/doc/html/latest/)** · **[scheduler bölümü](https://www.kernel.org/doc/html/latest/scheduler/)** — Üretim çekirdeğinin güncel resmi belgeleridir. Başlangıç kaynağı değildir; OSTEP/xv6 modelinin gerçek Linux’taki karşılığını kontrol etmek için seçerek okunmalıdır.
- **[cgroup v2](https://www.kernel.org/doc/html/latest/admin-guide/cgroup-v2.html)** · **[namespaces](https://man7.org/linux/man-pages/man7/namespaces.7.html)** — Container kaynak kontrolü ve görünürlük/izolasyon mekanizmalarını doğrudan resmî sözleşmelerinden okumak için kullanılır. Namespace ile kaynak limiti aynı şey sayılmaz.
- **[eBPF userspace API](https://www.kernel.org/doc/html/latest/userspace-api/ebpf/index.html)** · **[Landlock](https://www.kernel.org/doc/html/latest/userspace-api/landlock.html)** — Gözlemlenebilirlik/runtime extension ile yetkisiz süreç sandbox'ının farklı rollerini gösteren resmî çekirdek belgeleridir.
- **[liburing](https://github.com/axboe/liburing)** — `io_uring` için referans kullanıcı alanı kütüphanesi ve örnekleridir. Core laboratuvarda API ezberi yerine blocking/readiness/submission-completion modeli karşılaştırılır.

## Video ve açık ders

- **[NPTEL — Introduction to Operating Systems](https://nptel.ac.in/courses/106106144)** — IIT Madras'tan Chester Rebeiro'nun xv6 kullanan ücretsiz video dersidir. Düzenli ikinci anlatım arayan öğrenciler için uygundur.
- **[Gate Smashers — Operating System giriş videosu](https://www.youtube.com/watch?v=WJ-UaAaumNA)** — Kısa tekrar ve sınav öncesi kavram taraması için erişilebilir bir başlangıçtır. Teknik ayrıntılar OSTEP, xv6 kitabı veya resmi belgelerle doğrulanmalıdır.
- **[Nand2Tetris Part II — Coursera](https://www.coursera.org/learn/nand2tetris2)** — VM, compiler ve basit OS katmanlarını proje yoluyla ele alır. CEN 302’ye doğrudan alternatif değil, sistem katmanlarını birlikte görmek isteyen öğrenci için tamamlayıcıdır.

## İsteğe bağlı Udemy kursları

- **[Fundamentals of Operating Systems — Hussein Nasser](https://www.udemy.com/course/fundamentals-of-operating-systems/)** — Sistemlerin neden bu biçimde tasarlandığını pratik bir dille anlatır; görsel/konuşmalı tekrar isteyenler için uygundur.
- **[Operating Systems from Scratch: Scheduling Algorithms](https://www.udemy.com/course/scheduling-algorithms-operting-systems-from-scratch/)** — Scheduling algoritmalarını adım adım uygulatır. A6 için ek kodlama pratiği olarak düşünülebilir.
- **[Writing Your Own Operating System From Scratch](https://www.udemy.com/course/writing-your-own-operating-system-from-scratch/)** — Boot ve düşük seviye OS geliştirmesine meraklı öğrenciler içindir; dersin zorunlu çizgisinden daha düşük seviyeye iner.

> Udemy içeriği, fiyatı ve güncellenme tarihi değişebilir. Satın almadan önce müfredat, önizleme, altyazı ve iade koşulları kontrol edilmelidir.

## Kaynakları haftalık kullanma reçetesi

- **A1–A4:** OSTEP + Linux man-pages + TLPI örnek kodu; seçilmiş namespaces/async-I/O trace'i.
- **A5–A6:** OSTEP simulator + Little Book of Semaphores + Berkeley/MIT problem setleri; atomics/futex ve cgroup v2 köprüsü.
- **A7–A8:** OSTEP paging araçları + xv6 kitabı/page-table kodu + QEMU; capabilities/seccomp/Landlock ve container/VM karşılaştırması.
- **A9–A10:** OSTEP file-system/disk simulator + xv6 file system + Brendan Gregg; `perf`/eBPF ve async/NVMe panoraması.
- Her kaynak kullanımında öğrenci üç satır yazmalıdır: **iddiam**, **kanıtım**, **kaynağın/deneyin sınırı**.
