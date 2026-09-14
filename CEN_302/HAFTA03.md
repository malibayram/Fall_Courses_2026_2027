# CEN 302 Operating Systems — 3. Hafta Öğretim Dosyası

## Haftanın kimliği

| Alan | Plan |
| --- | --- |
| Tema | Bütün sistemin davranışı: süreç, durum değişimi ve hata yolu |
| Ana soru | Bir byte sayma işinin başlatılmasını, çalışmasını ve sonucunun toplanmasını nasıl izleriz? |
| Süre | 155 dakika: 125 dakika etkin öğrenme + üç adet 10 dakikalık ara |
| Başlangıç | W2 Workbench iskeleti; host/guest Byte Counter ve ortak fixture |
| Haftanın ürünü | Linux `fork/execv/waitpid` dikey dilimi, üç temel senaryo ve W4 aday sürümü |
| Sonraki kapı | W4'te sinyalli sonlanma, tekrar/cleanup ve temiz kurulumla `v0.1` |
| Belge durumu | Öğretim ve uygulama sözleşmesi; starter, hata kanalı ve harness aşağıdaki tanıma göre ayrıca üretilecektir |

**Bağlantılar:** [Ana ders sistemi](../README.md) · [Güncel kapsam](GUNCEL_KAPSAM.md) · [Proje sözleşmesi](PROJE.md) · [W1](HAFTA01.md) · [W2](HAFTA02.md) · [W4](HAFTA04.md) · [Kaynakça](KAYNAKCA.md)

Dosya öğretim ekibi içindir. Öğrenci görev özeti sonda İngilizce verilmiştir; öğrenciye yayımlanmadan önce hazırlık soruları ve ölçülen içeriğin eşdeğer İngilizce notu da eklenir. **Bu hafta A5–A7'ye (senkronizasyon/deadlock, CPU zamanlama, adres uzayı/çevirisi) davranış ve hata açısından ayrıntılı işlenir.** A1–A4 yalnız kısa geri çağırmayla kullanılır; A8–A10 W4'e bırakılır ve haritada bekleyen kalır.

## 1. Öğretim amacı ve kazanımlar

W2'de “parçalar nerede?” sorusunu yanıtladık. Bu hafta aynı yapıya gerçek bir iş gönderip “önce ne olur, hangi durum değişir, nerede hata oluşur, kim sonucu toplar?” sorularını yanıtlayacağız.

Öğrenci yalnız `fork`, `exec` ve `wait` adlarını bilmekle yetinmez; her adımın başarılı ve başarısız devam yolunu çizer. İki sürecin log sırasını mutlak bir yürütme sırası sanmaz. Çocuk tamamlanmasını parent'ın kendi başarısı veya Byte Counter'ın ürettiği sayıyla karıştırmaz.

| Kod | Öğrenci ders sonunda… | Kanıt |
| --- | --- | --- |
| W3-K1 | `fork` dönüş yollarını ve parent/child kimliklerini ayırır. | PID etiketli kontrol akışı |
| W3-K2 | Başarılı `execv` sonrasında eski programa dönülmediğini açıklar. | Başarı/hata dalları ve yeni program çıktısı |
| W3-K3 | Hedef çocuğun tamamlanmasını bekler ve normal sonucunu yorumlar. | Dönen PID, sonuç türü ve exit code |
| W3-K4 | Eksik executable ile çalışan worker'ın eksik dosya hatasını ayırır. | Üç senaryolu test/trace matrisi |
| W3-K5 | Bekleme/tamamlanma senkronizasyonunu (A5), parent/child zamanlamasını (A6) ve exec sonrası adres uzayı değişimini (A7) davranış/hata haritasına bağlar. | Davranış ve hata haritası |
| W3-K6 | Sağlanan altyapı ile kendi değişikliğini ayırır. | Küçük kod farkı, kaynak/katkı kaydı |
| W3-K7 | W4'e geçilecek sürümü ve eksik kabul koşullarını sabitler. | Release candidate kimliği ve kalan iş listesi |

## 2. Kapsamı sınırlama

| Şerit | İçerik |
| --- | --- |
| Core | Tek-thread parent, her çağrıda tek doğrudan child; açık executable yolu; argüman dizisi; normal sonuç ve başlatma hatası; hedef PID'yi toplama |
| Sağlanan altyapı | CLI parser, rapor yazıcısı, `exec` hata kanalı, fixture programları, test/trace otomasyonu |
| Instructor demo | xv6 shell ve kernel süreç yolu, olası çıktı sıraları, descriptor mirası, adres uzayı modeli |
| Stretch | İzin verilmeyen executable için ek hata senaryosu veya boşluk içeren tek argümanın aktarımı |
| W4'e kalan | Tam sinyal sonuç sınıflandırması, cleanup tekrar paketi, sürüm kabulü |
| W5 ve sonrasına kalan | Öğrenci syscall'ı, birden çok eşzamanlı child, pthread, gerçek iş verisi pipe hattı, scheduler/VM artımları |

Launcher bu aşamada shell değildir: komut metni ayrıştırma, `PATH` arama, pipeline, job control, timeout politikası ve torun süreç yönetimi zorunlu değildir. `execv` için dosyanın açık yolu ve NULL ile sonlanan `argv` verilir; `execvp` ile yapılan PATH araması ayrı bir tasarım seçimidir. [Linux exec ailesi](https://man7.org/linux/man-pages/man3/exec.3.html).

## 3. Eğitmenin hazırlayacağı paket

Paket en az beş gün önce yayımlanır; hazırlık yanıtı dersten 12 saat önce alınır.

- 25–35 dakikalık davranış videosu ve eşdeğer İngilizce not.
- W2 tabanı üzerinde derlenebilir launcher iskeleti: öğrenci TODO'ları `fork` dalları, `execv`, hedefi bekleme ve normal sonuç eşleme ile sınırlıdır.
- `Job`/`RunResult` başlıkları, CLI parser ve parent'ın yazdığı ayrı rapor dosyası desteği.
- Önceden test edilmiş, close-on-exec özellikli `exec` hata kanalı yardımcıları.
- Başarılı Byte Counter, eksik girdi, bulunamayan executable ve exit 127 fixture'ı.
- `waitpid` kesintisini yeniden deneyen sağlanmış yardımcı veya açık TODO ve testi; sinyal handler yazımı öğrenciye verilmez.
- Parent/child/PID, olay, sonuç türü ve errno adı alanlı trace çalışma kâğıdı.
- Guest shell/process kaynak turu ve gerekli öğretmen çıktıları.
- Hatalı sürüm örnekleri: child dalından geri dönme, yanlış PID'yi bekleme, her exit'i başarı sayma.

### Altyapının sorumluluğu

Öğretim ekibi hata kanalındaki kısmi okuma/yazma, `EINTR`, FD kapanışı ve protokol hatalarını kendi testleriyle doğrular. Öğrenci henüz tam IPC kütüphanesi yazmaz; iki durumun neden yalnız exit code üzerinden ayırt edilemediğini ve yardımcıyı hangi dalda kullanacağını açıklar.

Host testi yalnız doğrulanmış Linux ortamında yürütülür. Öğretmen, xv6 `user/user.h` ve `proc.c` dosyalarını baseline commit'iyle eşleştirir. Belgedeki örnek kayıtlar **tasarlanmış beklenen çıktıdır**; bu hazırlık sırasında elde edilmiş çalışma logu değildir.

## 4. Ders öncesi çalışma ve soru anahtarı

Toplam hazırlık hedefi yaklaşık 55–70 dakika. Seçili okuma: [OSTEP — Process API](https://pages.cs.wisc.edu/~remzi/OSTEP/cpu-api.pdf) içindeki fork/exec/wait örnekleri ve Linux [`fork`](https://man7.org/linux/man-pages/man2/fork.2.html), [`execv`](https://man7.org/linux/man-pages/man3/exec.3.html), [`waitpid`](https://man7.org/linux/man-pages/man2/waitpid.2.html) dönüş/hata bölümleri. Tüm man-page seçenekleri ezberletilmez.

| Hazırlık sorusu | Beklenen yön |
| --- | --- |
| 1. Launcher ile worker'ın sorumluluğu nasıl ayrılıyor? | Launcher çalıştırma/sonuç; worker sayma ve girdi hatası. |
| 2. Başarılı `fork` sonrasında hangi dal hangi kimlikle devam eder? | Parent child PID'sini, child 0 dönüşünü görür; iki farklı süreç. |
| 3. `execv` çağrısından sonraki satır hangi durumda çalışır? | Çağrı hata döndürdüğünde; başarılı yükleme eski akışa dönmez. |
| 4. Çocuk parent `waitpid` çağırmadan önce biterse sonuç kaybolur mu? | Bu dersin varsayılan child-reaping düzeninde sonuç daha sonra toplanabilir. |
| 5. Eksik executable ile eksik veri dosyası arasındaki fark nedir? | İlkinde worker yüklenemez; ikincisinde worker çalışıp girdi hatası verir. |
| 6. İki PID aynı `stdout` hedefine yazıyorsa log sırası neyi kanıtlar? | Yalnız gözlenen yazı sırası; bütün iç yürütmenin tek zorunlu sırası değildir. |

Sorular için ilk kısa tahmin bireysel yazılır. Açık hazırlıkta kullanılan kaynak ve AI katkısı belirtilir. Studio ilk denemesi, puanlanan quiz ve sözlü kontroller AI'sızdır.

## 5. Teknik sözleşme: bir işi başlatmak ve sonucunu toplamak

### `Job` ve yürütme sınırı

```text
Job = executable_path + argv
Önkoşul: geçerli yol metni, argv[0], sonda NULL; çağrı boyunca geçerli bellek
Çalışma dizini/ortam: parent'tan devralınır ve testte sabitlenir
Eşzamanlılık: bir run çağrısında bir child; parent tek-thread
Çıktı: worker stdout/stderr devralır; sonuç raporu ayrı parent dosyasına gider
```

`system()` çağrısı bu öğrenme hedefinin yerine geçmez. Öğrenci process-control dallarını kendi kodunda görünür kılar. Parent, worker çıktısını byte-byte yakalayan yeni bir pipe altyapısı yazmaz; stdout/stderr yakalama test harness'inin dış yönlendirmesidir.

### W3 ve W4'ün paylaştığı sonuç modeli

| Sonuç türü | Anlam | W3 durumu |
| --- | --- | --- |
| `EXITED` | Child toplandı; normal sonlanma kodu var. 0 başarı, diğer kodlar worker sonucu. | Öğrenci uygular. |
| `LAUNCH_ERROR` | Başlatma aşaması başarısız; `stage` ve hata bilgisi var. Fork sonrası child varsa ayrıca toplanır. | `fork`/`exec` yolları uygulanır; kanal iskeleti sağlanır. |
| `SIGNALED` | Child toplandı; sonlandıran sinyal ayrı alanda. | Tür ayrılmıştır; sınıflandırma ve kabul W4'tedir. |
| `SUPERVISOR_ERROR` | Bekleme, iç protokol veya rapor gibi denetim hatası; güvenilir sonuç üretilememiştir. | Sağlanan hata sınırı; başarı gibi raporlanmaz. |

W3'te henüz sınıflandırılmamış bir non-normal sonuç başarıya çevrilmez; `SUPERVISOR_ERROR`, `stage=classify` ve `not-yet-classified` açıklamasıyla CLI 5 döner. Toplanmış child bilgisi korunur ve bu geçici sınırlama aday sürüme yazılır. W4 kabulünde sinyalli sonlanma kendi türüne taşınır.

Parent'ın CLI dönüş politikası W4'te kesinleştirilen aynı tabloyu kullanır: 0 başarılı worker, 1 worker'ın sıfır olmayan normal çıkışı, 2 launcher kullanım hatası, 3 başlatma hatası, 4 sinyalli sonlanma, 5 supervisor hatası. **Worker'ın gerçek kodu raporda saklanır.** Örneğin worker 2 ile çıkarsa launcher 1 döner; launcher'ın kendi yanlış kullanımı 2'dir.

### `fork → exec → wait` karar ağacı

```text
job doğrula; rapor ve hata kanalı hazırlığını yap
  ├─ hazırlık hatası → child yok; ilgili hata sonucunu bildir
  └─ fork
       ├─ hata → child yok; açılan kaynakları kapat
       ├─ child
       │    ├─ gereksiz uçları kapat
       │    └─ execv(path, argv)
       │         ├─ başarı → worker programı
       │         └─ hata → errno bilgisini sağlanan kanala yaz; _exit
       └─ parent
            ├─ kendi gereksiz ucunu kapat
            ├─ hata kanalı gözlemini al
            ├─ hedef child için waitpid; EINTR ise yeniden dene
            └─ PID + toplanma durumu + hata aşaması/exit sonucunu raporla
```

`fork` belleği ayrı süreç bağlamlarına taşır; descriptor kopyaları ise aynı açık dosya tanımına işaret edebilir. Bu nedenle “her şey ortak” ve “hiçbir şey ortak değil” genellemeleri yerine kaynağın türü sorulur. [Linux fork sözleşmesi](https://man7.org/linux/man-pages/man2/fork.2.html).

Başarılı `exec` süreç görüntüsünü değiştirir; yeni PID yaratma işi değildir. Hata dalında `_exit` kullanılması, parent'tan kalan stdio tamponlarını yeniden flush etmeme niyetini taşır; hata bilgisi sağlanan düşük seviyeli kanaldan gönderilir. [execve](https://man7.org/linux/man-pages/man2/execve.2.html), [_exit](https://man7.org/linux/man-pages/man2/_exit.2.html).

### Neden yalnız exit 127 yeterli değil?

Ders uygulaması child'ın `exec` hatasında `_exit(127)` kullanabilir. Ancak geçerli bir worker da 127 ile çıkabilir. Dolayısıyla 127 tek başına “executable bulunamadı” kanıtı değildir.

Sağlanan yardımcı, fork öncesi oluşturulan kanalı kullanır. Child ucundaki close-on-exec sayesinde başarılı yükleme bu ucu kapatır; `exec` başarısızlığı hata kaydı üretir. Parent bu bilgiyi **wait sonucu ile birlikte** yorumlar. Kanalda EOF tek başına başarılı iş kanıtı değildir; child yükleme öncesi de sonlanmış olabilir. [pipe/close-on-exec](https://man7.org/linux/man-pages/man2/pipe.2.html).

Bu W3/W4 ders tasarımına ait raporlama tercihidir. Öğrenci yardımcı protokolün tüm implementasyonunu yazmaz; `LAUNCH_ERROR` ve `EXITED(127)` örneklerini ayırt ederek nedenini açıklar.

## 6. A5–A7 davranış ve hata turu — bu haftanın odağı

| Çapa | İzlenen davranış | Hata/karşı örnek | Bu hafta kanıtı |
| --- | --- | --- | --- |
| A5 — Senkronizasyon/deadlock | `waitpid` ile hedef child'ın tamamlanma koşulu beklenir; bu launcher'ın parent/child arası tek senkronizasyon noktasıdır. | “Bir saniye uyumak” tamamlanma kanıtı değildir. | Bekleme protokolü; erken/geç child örneği |
| A6 — Zamanlama | Parent beklerken child bağımsız ilerleyebilir; ikisinin CPU'ya ne zaman seçileceği scheduler'ın işidir. | Log sırası tek scheduling garantisi sayılır. | Zorunlu/olası sıralama tablosu |
| A7 — Adres çevirisi | Fork ile adres uzayı kopyalanır; exec ile aynı süreç kimliğinde tamamen değişir. | Aynı sayısal adres aynı fiziksel nesne sanılır. | İşaretli bellek modeli, ölçüm iddiası yok |

Haritada her satır O/M/F, ortam, kaynak/öğrenci katkısı ve bir kanıt konumu taşır.

### A1–A4 kısa geri çağırma, A8–A10 bekleyen

A1 (OS sınırı), A2 (süreç), A3 (thread) ve A4 (IPC/descriptor) W2'de işlendi; bu hafta yalnız fork/exec/wait akışının hangi anchor'a dokunduğunu birer cümlede hatırlatmak için kullanılır, yeniden anlatılmaz. A8 (sanal bellek/izolasyon), A9 (dosya sistemi) ve A10 (I/O) W4'te ele alınacaktır; haritada `F` olarak kalır.

## 7. 155 dakikalık ders akışı

| Süre | Öğretmen hamlesi | Öğrenci işi ve kontrol noktası |
| --- | --- | --- |
| 00–08 | W2 yapı haritasından launcher/worker sınırını geri çağır (A1–A4 kısa geri çağırma). Başarı, eksik executable, eksik veri dosyasını kod açmadan göster. | Üç sonucun aşamasını tahmin et. |
| 08–18 | **A5:** Fork dönüş dalları ve PID; hedef child'ın tamamlanma koşulunun neden beklenmesi gerektiği. | Parent/child yollarını farklı renkle çiz. |
| 18–30 | **A5 devam:** “Bir saniye uyumak” karşı örneği; erken/geç biten child senaryoları. | Bekleme protokolü tahmini. |
| 30–40 | Ara | |
| 40–55 | **A6:** İki olası log sırası, parent/child scheduling; zorunlu ile gözlenen sırayı ayırma. | Zorunlu/olası sıralama tablosu. |
| 55–70 | **A7:** Exec yükleme ve adres uzayı değişimi; hangi satırın başarıda çalışmayacağı. | Adres uzayı öncesi/sonrası çizimi. |
| 70–80 | Ara | |
| 80–90 | Bir mutlu yolu tam trace tablosunda çöz. | Olay, PID, durum, kanıt satırları. |
| 90–100 | 127 çakışması ve sağlanan hata kanalı. | İki 127'nin neden farklı olduğunu açıkla. |
| 100–110 | xv6 karşılığı (kısa, A1 geri çağırma) ve A5–A7 davranış haritası. | Kaynak yolu, ortam farkı ve F noktaları. |
| 110–120 | Ara | |
| 120–126 | AI'sız bireysel fork/exec/wait iskelet tahmini. | İlk kontrol akışı. |
| 126–138 | Parent/child dallarını tamamlat. | Mutlu yol; `exec` sonrası yanlış dönüş yok. |
| 138–150 | Hata kanalı çağrısı, hedefi bekleme ve üç temel testi birlikte çalıştırma. | Eksik executable ve eksik dosya testleri; beklenen/gözlenen tablo. |
| 150–155 | Exit ticket ve anonim iş yükü yoklaması. | Hata aşaması, sıralama ve süre kaydı. |

Eşli çalışmada kodu yazan ve sonucu tahmin eden roller 138. dakikada değişir. Her öğrenci kendi trace'ini ve çıkış yanıtını verir. 127 karşı örneği ve W4 eksik listesi bölüm 11'deki ders dışı Core hedefine dahildir; kısa sözlüler dönem kapsama kaydına göre ayrı zamanlarda yapılır, tüm sınıfı tek derste puanlanan sözlüye almak hedeflenmez.

## 8. Çalışılmış örnek: üç olay çizgisi

Aşağıdaki PID'ler yalnız şematik örnektir. Gerçek testler sabit PID veya tam log sırası beklemez.

| Aşama | Başarılı sayım | Executable yok | Worker girdisi yok |
| --- | --- | --- | --- |
| İstek | `bytecount sample.txt` | `missing-worker sample.txt` | `bytecount missing.txt` |
| Parent | Child 4101'i oluşturur. | Child 4102'yi oluşturur. | Child 4103'ü oluşturur. |
| Child yükleme | Worker görüntüsüne geçer. | Exec hata kanalına bilgi verir. | Worker görüntüsüne geçer. |
| İş davranışı | Dosyayı sayar; stdout `4`. | Worker çalışmaz. | Worker açma hatası verir; stdout boş. |
| Child sonucu | Normal kod 0 | Ders helper'ının hata çıkışı | Normal kod 1 |
| Parent raporu | `EXITED`, exit 0, reaped | `LAUNCH_ERROR`, stage exec, reaped | `EXITED`, exit 1, reaped |
| Launcher CLI kodu | 0 | 3 | 1 |

**Çıkarım çalışması:** Parent “child oluşturuldu” logunu yazmadan child'ın bir log yazması mümkün mü? Öğrenci iki olası çizgi çizer. Ancak parent'ın child sonucunu başarılı biçimde toplamasından sonraki sonuç raporu, toplanma olayından önceye yerleştirilemez. Bu, tüm terminal çıktısının zorunlu tek sıra taşıdığı anlamına gelmez.

### xv6 karşılaştırma turu — yaklaşık 10 dakika

Eğitmen [sabit `user/sh.c`](https://github.com/mit-pdos/xv6-riscv/blob/9e3161a9abf5f51ea402562d1874caf6c4926597/user/sh.c) içindeki fork/exec/wait yolunu, [kullanıcı API'sini](https://github.com/mit-pdos/xv6-riscv/blob/9e3161a9abf5f51ea402562d1874caf6c4926597/user/user.h) ve [`kernel/proc.c`](https://github.com/mit-pdos/xv6-riscv/blob/9e3161a9abf5f51ea402562d1874caf6c4926597/kernel/proc.c) içindeki oluşturma/bekleme/çıkış ilişkisini işaretler.

| Linux Core | xv6 kaynak turu |
| --- | --- |
| `execv(path, argv)` | `exec(path, argv)` kullanıcı arayüzü |
| `waitpid(child_pid, &status, 0)` | `wait(&status)`; aynı seçilebilir-PID API'si varsayılmaz |
| Linux wait status yorumlama | Guest status'u kendi sözleşmesiyle okunur; Linux makroları taşınmaz |
| Linux errno ve sinyaller | Guest'te aynı hata kodu/sinyal modeli varsayılmaz |

Öğrenci bu hafta guest launcher yazmaz. W2 guest smoke korunur; kaynak turu, Linux implementasyonunun bütün OS'lerde aynen çalışacağı yanılgısını önler.

## 9. Studio ve test sözleşmesi

### Öğrencinin tamamlama sırası

1. W2 baseline'ını kaydet; host/guest smoke sonuçlarını kontrol et.
2. `Job` doğrulamasını ve açık yolu incele; supplied CLI parser'ı değiştirme.
3. Fork hata, child ve parent dallarını tamamla.
4. Child exec hata yolunda sağlanan raporlama yardımcısını çağır ve child akışını bitir.
5. Parent'ta hedef PID'yi bekle; geçerli sonuç olmadan status çözme.
6. Normal sonlanmayı ve hata kanalından gelen başlatma hatasını sonuç modeline eşle.
7. Üç temel senaryoyu, W1 regresyonunu ve 127 karşı örneğini çalıştır.
8. Trace/harita farkını yaz; W4 eksiklerini ve aday commit/paketini kaydet.

### Test matrisi

| Test | Beklenen sonuç | Yanlış uygulamayı açığa çıkaran nokta |
| --- | --- | --- |
| W3-T1: normal sample | Sayı 4; `EXITED(0)`; child PID toplanmış | Launcher yalnız başlatıp erken dönemez. |
| W3-T2: executable bulunamıyor | `LAUNCH_ERROR`, exec aşaması; child toplanmış; CLI 3 | Worker veri hatasıyla karışmaz. |
| W3-T3: worker için veri dosyası yok | stdout boş; worker tanısı; `EXITED(1)`; CLI 1 | Parent'ın kendisi çalıştı diye başarı sayılmaz. |
| W3-T4: geçerli worker 127 döndürüyor | `EXITED(127)`; CLI 1 | Exit 127 tek başına exec hatası değildir. |
| W3-T5: W1 dört test | Önceki sayı/exit sözleşmeleri korunur. | Launcher eklenirken worker bozulmamıştır. |
| W3-T6: çok kısa ömürlü child | Hızlı bitse de doğru PID/sonuç toplanır. | Sleep süresine dayalı test yoktur. |
| W3-T7: W2 guest smoke | Guest sample 4, empty 0 | Host değişikliği guest paketini bozmamıştır. |

T1–T3 ders içindeki asgari üç izdir. Diğerleri sağlanan fixture/harness ile çalıştırılır. Fork kaynağını tüketerek hata üretme yapılmaz; W4'te kontrol edilen hata enjeksiyonu kullanılır.

### Örnek çağrı düzeni

Aşağıdaki komutlar **hazırlanacak starter'ın arayüz sözleşmesidir**. `build/`, executable ve rapor yazıcısı bu öğretim belgeleriyle birlikte üretilmiş değildir. Komutlar Workbench kökünde, `build/` ve `evidence/` hazırlanmışken çalışacaktır.

```sh
./build/workbench --report evidence/w3-normal.json -- ./build/bytecount fixtures/sample.txt
launcher_rc=$?
printf 'launcher_rc=%s\n' "$launcher_rc"
```

Bu başarılı örnekte worker çıktısı 4, launcher kodu 0'dır. Hata örnekleri test harness'inde ayrı çalıştırılır; worker kodu JSON rapordan okunur. PID, zaman ve işletim sistemine bağlı tanı metninin tamamı golden-output olarak sabitlenmez. `strace` satırları yorumlanırken C API adının alttaki syscall adıyla birebir aynı olmak zorunda olmadığı belirtilir.

## 10. Yanılgılar, kısa sorular ve cevap yönü

| Yanılgı | Öğretmen müdahalesi |
| --- | --- |
| “`exec` ikinci bir child oluşturur.” | Fork öncesi/sonrası ve exec öncesi/sonrası kimlik tablosu çizdir. |
| “Child dalındaki hata sonrası `return` sorun olmaz.” | Child'ın parent için yazılmış akışa ulaşabildiği karşı örneği izlet. |
| “`waitpid` dönüşü exit code'dur.” | Dönüş PID'si ile status alanını ayrı kutularda göster. |
| “Her status'a `WEXITSTATUS` uygularım.” | Normal sonlanma koşulu olmayan girdinin yorumlanamayacağını W4'e bağla. |
| “Exec hata metni varsa child toplamak gerekmez.” | Başlatılmış child'ın kaynak/sonuç sahipliğini takip ettir. |
| “Parent'ın önce log yazması garantidir.” | Alternatif geçerli olay sırası çizdir. |
| “Bir saniye bekleme child'ın bittiğini kanıtlar.” | Beklenen olayın ne olduğunu sor; hedefli wait ile karşılaştır. |

**Bireysel sözlü kartları:** Verilen iki hatadan hangisinde worker çalıştı; exec başarılıysa hangi kod artık yürütülmez; 127 sonucunu nasıl ayırırsın; child çok erken biterse rapor nasıl oluşur; trace'in bir adres uzayı ve bir descriptor bağlantısını açıkla.

## 11. Teslim ve geri bildirim

1. W2'ye göre kod farkı ve kesin W3 aday sürümü.
2. T1–T3 için olay/PID/durum/kanıt tablosu; T4–T7 sonuçları.
3. A1–A10 davranış haritası; her çapa için O/M/F, ortam ve katkı kaydı.
4. Bir hata yolu kararı ve bir karşı örnek; ana metin 1–2 sayfayı hedefler.
5. W4 için eksik liste: sinyal sınıflandırması, tekrar/cleanup, release belgeleri.
6. CHANGELOG, bilinen sınırlama ve AI/dış katkı açıklaması.
7. 2–4 dakikalık video veya eşdeğer açıklama: bir mutlu yol, bir başarısızlık aşaması, bir değişiklik tahmini.

| Haftalık kalite ölçütü | Puan |
| --- | ---: |
| Fork/exec/normal wait akışının doğruluğu | 30 |
| Başlatma hatası ve worker hatasının ayrılması | 20 |
| Üç temel iz ve ilgili regresyon kanıtı | 20 |
| A5–A7 davranış bağlantısı ve kaynak sınırı | 15 |
| Yeniden üretim, karar ve aday sürüm kaydı | 15 |
| **Toplam** | **100** |

Bu ölçek haftalık geri bildirimi yapılandırır; dersin toplam notuna ek yüzde getirmez. W4'te eski testlerin geçmesi entegrasyon koşuludur; W3'ün aynı yerel ölçütü ikinci kez puanlanmaz. Video ve varsa puanlanan sözlü kendi bileşenlerinde değerlendirilir.

Ders dışı Core hedefi: launcher düzeltme 65, test/trace 45, harita/karar 35, sürüm/katkı 20 dakika; toplam yaklaşık 165 dakika. Hazırlık ve en çok 30 dakika video ayrıdır. W3 anonim süre yoklamasında kurulum, kod, test, belge ve video ayrı sorulur; kapsam sonraki paketlerde gerçek süreye göre ayarlanır.

## 12. Exit ticket ve W4 köprüsü

Notlar/AI kapalı:

1. `LAUNCH_ERROR` ile `EXITED(1)` arasındaki farkı birer olayla açıkla.
2. Olay çizginde zorunlu bir sıralama ve değişebilen bir sıralama yaz.
3. W4'te hangi kanıt olmadan “child cleanup tamam” demeyeceksin?

**W4 başlangıç cümlesi:** “Çalışan bu dilimi bir sürüm sözleşmesine dönüştüreceğiz: sonucu doğru sınıflandıracağız, başlattığımız çocuğu toplayacağız ve aynı ürünü temiz ortamda yeniden kuracağız.”

## 13. English student handout — Week 3

**Focus:** Build A5–A7 (synchronization/deadlock, CPU scheduling, address translation) in depth through behavior, state changes and failure; A1–A4 get only a brief callback and A8–A10 stay on the map for Week 4. Extend your Week 2 Workbench; keep its tests and fixtures.

**Your task:** Complete a single-threaded Linux launcher that creates one direct child per job, executes an explicit executable path with its argument vector, waits for the target child and reports the outcome. The instructor supplies the parser, report writer, exec-error channel and test infrastructure.

**Required scenarios:** A successful Byte Counter run; an executable that cannot be loaded; a valid Byte Counter run whose input file is missing. Record the event sequence, process identities, failure stage and collected result. A real worker returning 127 must remain distinguishable from an exec failure. Preserve the Week 1 tests and Week 2 guest smoke tests.

**Result contract:** Keep worker output, worker exit status and launcher exit status separate. The project CLI uses 0 for successful work, 1 for a nonzero normal worker exit, 2 for launcher usage errors, 3 for launch errors, 4 for signal termination and 5 for supervisor errors. Full signal classification is a Week 4 task; an unclassified result must not become success.

**Preparation questions:** Who owns counting versus execution? What are the fork branches? When can the line after exec run? Can an early child result still be collected? How do a missing executable and a missing input differ? What does the observed log order prove?

**Submission:** Code difference, the three main traces, supporting regression evidence, an A1–A10 behavior map, one design decision, contribution disclosure, and a fixed release-candidate identity with remaining Week 4 work. Include one 2–4 minute explanation or an accessible equivalent.

**Feedback:** 30 points for the control flow, 20 for failure-stage separation, 20 for traces and regression, 15 for the anchor map and 15 for reproducibility and the candidate record. This rubric does not add a course-grade component. Start studio individually without AI; disclose permitted assistance. Target about 165 minutes of out-of-class Core work, with preparation and recording budgeted separately.
