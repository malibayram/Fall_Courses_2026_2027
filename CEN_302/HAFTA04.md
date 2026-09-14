# CEN 302 Operating Systems — 4. Hafta Öğretim Dosyası

## Haftanın kimliği

| Alan | Plan |
| --- | --- |
| Tema | Bütün sistemin entegrasyonu: gözlenebilir, test edilmiş ve yeniden kurulabilir `v0.1` |
| Ana soru | Bir işi başlattığımızda her sonuç yolunu doğru raporlayıp süreç sorumluluğunu tamamladığımızı nasıl kanıtlarız? |
| Süre | 155 dakika: 125 dakika etkin öğrenme + üç adet 10 dakikalık ara |
| Başlangıç | W3 release candidate; normal/başlatma hatası akışları ve W1–W2 regresyonu |
| Haftanın ürünü | Normal/hatalı/sinyalli sonlanmayı ayıran Linux launcher, cleanup kanıtı, guest smoke ve sürüm belgeleri |
| Sonraki bağlantı | W5'te bu tabana sınırlı xv6 süreç istatistiği syscall'ı ve host syscall gözlemi |
| Belge durumu | Öğretim ve kabul sözleşmesi; test harness'leri, fixture programları ve referans release ayrıca hazırlanıp doğrulanacaktır |

**Bağlantılar:** [Ana ders sistemi](../README.md) · [Güncel kapsam](GUNCEL_KAPSAM.md) · [Proje sözleşmesi](PROJE.md) · [W1](HAFTA01.md) · [W2](HAFTA02.md) · [W3](HAFTA03.md) · [Kaynakça](KAYNAKCA.md)

Bu dosya eğitmen/asistan planıdır; sondaki İngilizce görev özeti, eşdeğer İngilizce kavram notu ve rubrikle öğrenciye yayımlanır. **Bu hafta A8–A10'a (sanal bellek/izolasyon, dosya sistemi/tutarlılık, depolama/I/O) entegrasyon açısından ayrıntılı işlenir.** A1–A7 yalnız kısa geri çağırmayla kullanılır; ikinci tur bu haftayla tamamlanır. `v0.1`, bütün OS mekanizmalarının öğrenci tarafından uygulanması anlamına gelmez; ilk çalışır ürün ve sonraki on artımın açık başlangıç noktasıdır.

## 1. Öğretim amacı ve kazanımlar

W3'te gerçek bir işin akışını kurduk. W4'te bu akışın güvenilir sınırını tarif edeceğiz: hangi durumlar destekleniyor, ne raporlanıyor, hangi kaynak kime ait, hata sonrasında ne kalıyor, başka biri aynı sürümü çalıştırabiliyor mu?

| Kod | Öğrenci ders sonunda… | Kanıt |
| --- | --- | --- |
| W4-K1 | Normal exit, worker hatası, başlatma hatası ve sinyalli sonlanmayı ayırır. | Sonuç türlerine göre test matrisi |
| W4-K2 | Sonlandırılmış doğrudan child'ın sonucunu doğru PID ile toplar. | Her işte başarılı reap kaydı; tekrar deneyi |
| W4-K3 | Kesintiye uğrayan beklemeyi ve rapor hatasını başarı gibi göstermez. | Sağlanan hata enjeksiyonu testleri |
| W4-K4 | Tekrar çalışan parent'ın altında kalan child/zombie durumunu kontrol eder. | Yaşayan parent içinde test; yalnız terminal görüntüsüne dayanmayan kanıt |
| W4-K5 | Host ve guest sonuç semantiklerini ayrı raporlar. | Linux kabulü ve xv6 smoke kaydı |
| W4-K6 | Çalışan ürünü temiz kopyadan yeniden kurar. | Başka bir okuyucunun aynı adımlarla doğrulaması |
| W4-K7 | A8–A10'daki mevcut davranışları ve gelecek artımları savunur; A1–A7'yi kısaca geri çağırır. | A1–A10 entegrasyon haritası, backlog ve kısa sözlü |

## 2. `v0.1` kapsam sınırı

| Şerit | İçerik |
| --- | --- |
| Core | W3 dikey dilimi; guarded status çözümü; hedef PID'yi toplama; sinyal sonucu; tekrar deneyi; host/guest testler; sürüm belgeleri |
| Sağlanan altyapı | CLI/parser, hata kanalı, rapor yazıcısı, sinyal ve EINTR fixture'ları, uzun yaşayan test parent'ı, QEMU otomasyonu |
| Instructor demo | Zombie oluşma/toplanma modeli; xv6 exit/wait karşılığı; kendi başına yeterli olmayan test örnekleri |
| Stretch | Yeni bir worker ile contract testi veya sonuç raporu için ek tüketici |
| Kapsam dışı | Shell/pipeline, birden çok eşzamanlı child, daemon/torun süreç yönetimi, launcher'a gelen sinyaller için job control, otomatik timeout, kernel scheduler değişikliği |

Kabul, sağlanan sonlu test iş yüklerinde tek-thread parent'ın birer doğrudan child çalıştırması içindir. Keyfî, hiç bitmeyen bir programı her koşulda durdurma veya parent'ın zorla öldürülmesinde temizleme garantisi verilmez. Bu sınır release belgesinde görünürdür.

## 3. Eğitmenin hazırlığı ve kaynaklar

### Paket ve test donanımı

En az beş gün önce yayımlanacak:

- 20–30 dakikalık entegrasyon videosu ve eşdeğer İngilizce içerik.
- Sabit W3 aday tabanı ve W4 TODO listesi; yeni bir proje iskeleti açılmaz.
- Sürüm alanları ve CLI kodları tanımlı sonuç raporu örnekleri.
- Kendi içinde `SIGTERM` varsayılan davranışını kurup sinyali bloklamadan kendine gönderen sınırlı worker fixture'ı; testin doğruladığı sinyal platform sabitinden alınır.
- Aynı parent içinde art arda `run(job)` çağıran, başarılı reap'leri ve kalan çocukları denetleyen harness.
- `waitpid` için EINTR enjeksiyonu ve kontrollü fork/rapor hata yolları. Kaynak tüketerek gerçek sistem sınırı zorlanmaz.
- Normal guest sonucu için sağlanan `wait` tabanlı kontrol programı ve W2 smoke testi.
- Temiz kurulum kontrol listesi, kabul formu, sözlü soru kartları ve 100 puanlık ürün rubriği.

Fixture ve harness'ler öğretmen tarafından derlenip test edilmeden öğrenci paketi “hazır” sayılmaz. Sinyal testi rastgele PID, toplu process adı veya zamanlaması belirsiz `sleep` dizileriyle yapılmaz. Dışarıdan sinyal gönderen alternatif harness kullanılırsa yalnız kendi oluşturduğu ve hazır olduğunu doğruladığı child hedeflenir.

### Teknik doğrulama kaynakları

- [Linux waitpid](https://man7.org/linux/man-pages/man2/waitpid.2.html): normal/sinyalli durum makroları, kesinti ve bekleme sonucu.
- [Linux process API okuması](https://pages.cs.wisc.edu/~remzi/OSTEP/cpu-api.pdf): süreç oluşturma ve sonuç bekleme ilişkisi.
- [Linux execve](https://man7.org/linux/man-pages/man2/execve.2.html): süreç görüntüsü ve descriptor davranışının sınırları.
- [xv6 kullanıcı API'si](https://github.com/mit-pdos/xv6-riscv/blob/9e3161a9abf5f51ea402562d1874caf6c4926597/user/user.h) ve [process kaynağı](https://github.com/mit-pdos/xv6-riscv/blob/9e3161a9abf5f51ea402562d1874caf6c4926597/kernel/proc.c): guest exit/wait sözleşmesi.

Kaynaklar 11 Eylül 2026 tarihinde bu plan hazırlanırken incelenmiştir. Bu, yeni Workbench uygulamasının veya testlerinin çalıştırıldığı anlamına gelmez.

## 4. Ders öncesi öğrenci hazırlığı

Toplam hedef yaklaşık 45–60 dakika. W3 aday sürümünü değiştirmeden test et; eksikleri `davranış / test / belge / ortam` başlıklarıyla ayır. Sürüm kabul formunu okuyup “geçiyor / henüz kanıt yok / başarısız” ön değerlendirmesi yap. Hazırlık yanıtları dersten 12 saat önce teslim edilir.

| Soru | Beklenen cevap yönü |
| --- | --- |
| 1. Bir child'ın normal biçimde çıkması ile başarılı iş yapması aynı mı? | Normal sonlanma kodu sıfır olmayabilir; iş başarısı ayrı yorumdur. |
| 2. `waitpid` hata döndürmüşse status alanını başarı sonucu olarak okuyabilir miyiz? | Hayır; geçerli bir bekleme sonucu yoktur. |
| 3. Worker 2 ile çıkarsa launcher'ın 2 döndürmesi şart mı? | Hayır; bu projenin CLI politikası worker kodunu ayrı raporlar. |
| 4. Test sonunda terminalde süreç görmemek cleanup için yeterli mi? | Parent yaşamı, sahiplik ve wait kaydı da gerekir. |
| 5. QEMU exit code'u guest programın exit code'u mudur? | Farklı süreç/katman sonuçlarıdır. |
| 6. W4'te A8 satırına “virtual memory tamamlandı” yazabilir miyiz? | Ancak gösterilen dar kaynak/model kanıtı belirtilir; W12 artımı gelecektedir. |

İlk tahminler bireysel yazılır. Açık hazırlıkta kaynak/AI kullanılabilir ve katkı açıklanır. Dersin ilk denemesi, puanlanan quiz ve sözlüler bireysel ve AI'sızdır.

## 5. Sonuç raporunun kesin sözleşmesi

### Rapor alanları

| Alan | Kural |
| --- | --- |
| `schema_version` | `1`; W5 ve sonraki kanıt araçlarının okuyacağı rapor biçimi |
| `job_id` | Harness/çağıranın verdiği kimlik; PID ile aynı şey değildir. |
| `parent_pid`, `child_pid` | Gerçek çalışma kimlikleri; child hiç oluşmadıysa `child_pid=null` |
| `kind` | `EXITED`, `LAUNCH_ERROR`, `SIGNALED` veya `SUPERVISOR_ERROR` |
| `exit_code` | Yalnız `EXITED` sonucunda worker'ın kodu; diğerlerinde null |
| `signal` | Yalnız `SIGNALED` sonucunda sonlandıran sinyal; sayı platformdan alınır, sembolik ad gösterilebilir. |
| `stage`, `error` | Başlatma/denetim hatasında aşama ve hata bilgisi; geçersiz alanlar null |
| `child_reaped` | Child gerçekten oluşturulmuş ve başarılı biçimde toplanmışsa true; aksi durumda false |

`child_reaped=false`, child hiç oluşmadığında hata değildir. Child oluştuğu hâlde toplanamadıysa bu kayıt tamamlanmış başarılı iş diye sunulamaz. Rapor oluşturulamıyorsa launcher stderr'e açıklama verir ve supervisor hata koduyla biter; geçerli JSON yazılmış gibi davranılmaz.

Örnek bir `EXITED(0)` kaydı, **şematik ve ölçülmemiş** veri:

```json
{
  "schema_version": 1,
  "job_id": "normal-01",
  "parent_pid": 4100,
  "child_pid": 4101,
  "kind": "EXITED",
  "exit_code": 0,
  "signal": null,
  "stage": null,
  "error": null,
  "child_reaped": true
}
```

Raporu parent yazar. Worker stdout/stderr çıktısı rapor dosyasına karıştırılmaz. Harness job kimliğini verir; komut satırı kullanımında sağlanan parser bir varsayılan kimlik atayabilir. PID/zaman gibi değişken alanlar birebir golden-output karşılaştırmasına sokulmaz.

### Launcher CLI kodları

| Kod | Anlam | Örnek |
| --- | --- | --- |
| 0 | Worker normal kod 0 ile tamamlandı. | Sample byte sayımı |
| 1 | Worker normal biçimde sıfır olmayan kodla tamamlandı. | Worker exit 1, 2 veya 127; gerçek kod raporda |
| 2 | Launcher'ın kendi argüman/sözdizimi hatası | Executable argümanı verilmemiş |
| 3 | Başlatma hatası | Kanal hazırlığı/fork/exec başarısızlığı; aşama raporda |
| 4 | Child sinyalle sonlandı. | Kontrollü SIGTERM fixture'ı |
| 5 | Supervisor/raporlama hatası | Geçerli wait sonucu elde edilemiyor veya sonuç dosyası yazılamıyor |

Bu, Workbench'e ait tasarım kararıdır; Linux'un genel exit kodu standardı olarak öğretilmez. Shell'in sinyal sonucunu sayısal kodla göstermesi yerine raporda sonuç türü ve sinyal alanı kullanılır. Birden fazla hata varsa güvenilir rapor/sonuç üretimini engelleyen supervisor hatası CLI 5 ile öncelik alır; bilinen child sonucu mümkünse ek tanıda korunur.

### Status sınıflandırma

Geçerli child sonucu toplandıktan sonra normal çıkış ve sinyal türü ayrı dallarda çözülür. `WEXITSTATUS` yalnız `WIFEXITED`; `WTERMSIG` yalnız `WIFSIGNALED` doğrulandığında okunur. EINTR yeniden bekleme gerektirir; diğer wait hataları geçerli child sonucu sayılmaz. [Linux wait durum sözleşmesi](https://man7.org/linux/man-pages/man2/waitpid.2.html).

W3'ün sağlanan exec hata kanalı korunur. Güvenilir exec hata kaydı ile normal helper çıkışı `LAUNCH_ERROR` olarak raporlanır. Child sinyalle sonlandıysa `SIGNALED` bilgisi korunur; eldeki launch tanısı varsa ayrıca açıklanır. Kanal EOF'u worker'ın başarıyla tamamlandığının kanıtı değildir. Geçerli worker'ın 127 ile çıkması `EXITED(127)` olarak kalır.

## 6. Kaynak sahipliği ve cleanup

| Kaynak/durum | Sorumlu | Tamamlanma koşulu |
| --- | --- | --- |
| Hazırlıkta açılan rapor ve iç kanal kaynakları | Launcher | İlgili hata/başarı dalında kapanır; child'a gereksiz aktarılmaz. |
| Başarıyla oluşturulan doğrudan child | Parent launcher | Geçerli sonuç toplanır; başarılı reap bir kez kaydedilir. |
| Child'ın exec başarısızlığı | Child hata yardımı + parent | Hata raporlanır; parent child sonucunu yine toplar. |
| Worker'ın girdi dosyası | Byte Counter | W1 kaynak/hata sözleşmesi korunur. |
| EINTR sonrası bekleme | Parent | Aynı hedef için bekleme sürer; ikinci child oluşturulmaz. |
| Sonuç yazma hatası | Parent | Başlamış child için cleanup tamamlanır; yazma hatası başarıya çevrilmez. |

Normal kabul ortamında SIGCHLD'yi otomatik toplama için yok sayma veya başka bir reaper kullanma ayarlanmaz. Hedef child'ı başka kodun toplaması bu projenin sahiplik sözleşmesini bozar. Beklenmedik `ECHILD` hata olarak incelenir; “nasıl olsa temizlenmiş” denilerek başarılı sonuç uydurulmaz.

### Zombie kanıtını doğru kurma

Bitmiş fakat parent tarafından henüz sonucu toplanmamış child, Linux'ta zombie olarak kalabilir. Başarılı wait bu toplanma işini yapar. [Linux wait açıklaması](https://man7.org/linux/man-pages/man2/waitpid.2.html).

Şu deney **öğretmen tarafından sağlanan uzun yaşayan parent harness'inde** yapılır:

1. Parent test boyunca canlı kalır ve aynı `run(job)` fonksiyonuyla 20 sonlu iş çalıştırır.
2. Paket 10 başarılı sayım, 5 worker veri hatası ve 5 eksik executable içerir; sıra sabittir.
3. Her oluşturulmuş child için PID, tek başarılı reap ve sonuç türü kaydedilir.
4. Her çağrı tamamlandıktan sonra harness, o child için kalan toplanabilir sonuç olmadığını kendi parent bağlamında denetler. Tanımlı test probunda hedef için `ECHILD` beklenir; bu normal run akışının başarı ölçütüyle karıştırılmaz.
5. Parent hâlâ yaşarken dış gözlemci ilgili parent altındaki `Z` durumlarını kontrol eder. Bu gözlem reap kaydını destekler.
6. Hata hâlinde harness testi başarısız işaretler ve yalnız kendi test süreçlerini temizler.

“Launcher'ı 20 kez ayrı başlattık, sonunda `ps` boştu” aynı kanıt değildir: parent sonlandıktan sonraki yeniden sahiplenme, hatalı reaping'i gizleyebilir. Tek bir ekran görüntüsü yerine sahiplik kaydı ve kontrollü tekrar birlikte kullanılır. Kapsam, gözlenen test yollarıdır; bütün olası programlar için ispat iddiası kurulmaz.

## 7. A8–A10 entegrasyon turu — bu haftanın odağı

| Çapa | `v0.1` içinde korunacak ilişki | Bu haftaki kabul kanıtı | Sonraki artım |
| --- | --- | --- | --- |
| A8 — Sanal bellek/izolasyon | Sinyalli sonlanma, koruma/izolasyon sınırının gözlenebilir bir örneğidir; SIGTERM child'ın kendi alanında kalır, parent'ı etkilemez. | Kontrollü SIGTERM fixture'ı; model/kaynak açıklaması, bellek fault'u iddiası yok | W12 fault gözlemi |
| A9 — Dosya sistemi/tutarlılık | Sonuç raporu ve fixture dosyası; yazma hatasında tamamlanmamış rapor başarı sayılmaz. | W1 regresyonu, temiz kurulum, rapor yazma hatası testi; crash durability F | W13 dosya/recovery |
| A10 — Depolama/I/O | Girdi/çıktı yolu, host/guest çıktı kanıtı; ölçüm kapsamı açık tutulur. | Host/guest çıktı kanıtı; QEMU gerçek disk hızı iddiası yok | W14 I/O ölçümü |

### A1–A7 kısa geri çağırma

| Çapa | Bu haftaki kısa bağlantı |
| --- | --- |
| A1 | Başlatma hatası aşaması yine syscall sınırına değinir; yeniden anlatılmaz. |
| A2 | Child kimliği ve parent sahipliği; tek reap kaydı W2–W3'ten devam eder. |
| A3 | Eşzamanlı süreç akışı; thread artımı hâlâ F. |
| A4 | Veri/rapor kanalı ayrımı W3'ten korunur. |
| A5 | Sonuç bekleme protokolü; sleep tahmini yerine tamamlanma olayı W3'ten devam eder. |
| A6 | Bekleyen parent/ilerleyen child modeli; scheduler politikası ölçülmüş sayılmaz. |
| A7 | Host/guest ve parent/child adres bağlamı; sayfa tablosu artımı hâlâ F. |

Öğrenci her satıra O/M/F, host/guest/model, upstream/supplied/student ve kanıt konumu ekler. W4'te on konunun her birine kod eklenmesi beklenmez; A8–A10 bu haftanın gerçek öğretim derinliğidir. Entegrasyon haritası ürünün gerçek kapsamıyla birebir uyumlu olmalıdır.

## 8. 155 dakikalık ders akışı

| Süre | Öğretmen hamlesi | Öğrenci işi ve kontrol noktası |
| --- | --- | --- |
| 00–08 | W3'ün iki hata türünü kısaca geri çağır (A1–A7 geri çağırma); normal 0, normal 2 ve sinyal fixture'larını göster. | Üç sonuç türü/kodu ayrımı. |
| 08–20 | **A8:** Status sınıflandırma, rapor alanları ve sinyalli sonlanma; SIGTERM'in izolasyon/koruma sınırını nasıl gösterdiği. | Geçerli/uygulanamaz alanları işaretle. |
| 20–30 | **A8 devam:** Parent/child sahipliği ve sinyal fixture'ının kaynak/izolasyon açıklaması. | Kaynak tablosunun ilgili satırı. |
| 30–40 | Ara | |
| 40–55 | **A9:** Rapor dosyası, yazma hatası ve tamamlanmamış raporun başarı sayılmaması; W1 fixture dosyası regresyonu. | Rapor/dosya tutarlılık örneği. |
| 55–70 | **A10:** Girdi/çıktı yolu, host/guest çıktı kanıtı, ölçüm kapsamının açık tutulması. | Kanıtın ortamını ve tekrar yolunu yaz. |
| 70–80 | Ara | |
| 80–90 | Yaşayan parent içinde cleanup deneyi (A8 izolasyon örneği devamı). | Reap kaydı ile dış gözlemi eşle. |
| 90–100 | A8–A10 entegrasyon haritası ve W5–W14 backlog; A1–A7 satırları kısaca kontrol edilir. | Mevcut davranış/F ayrımı. |
| 100–110 | Kabul formunu örnek eksik teslim üzerinde uygula. | Geçti/başarısız/kanıt yok kararları. |
| 110–120 | Ara | |
| 120–126 | AI'sız bireysel status sınıflandırma denemesi. | İlk çözüm ve hata tahmini. |
| 126–138 | Sinyal sonucu ve kaynak dallarını tamamlat. | T1–T6 kabul akışı. |
| 138–150 | Tekrar/EINTR/regresyon paketini çalıştır; temiz paket üzerinden eşli yeniden üretim. | İlk kabul raporu; roller değişir; eksik dosya/örtük adım kaydı. |
| 150–155 | Exit ticket, release durumu ve W5 köprüsü. | Son eksik/sonraki davranış kaydı. |

Temiz kopya ve araçlar önceden hazırdır; ders içinde internetten toolchain indirilmez. Studio'da eğitmen ve iki asistan aynı kabul formunu kullanır. Seçilmiş bireysel savunmalar bölüm 12'deki ders dışı Core hedefine ve dönem kapsama planına göre ayrı zamanlarda yapılır; her öğrenci yazılı kısa savunma verir.

## 9. `v0.1` test kataloğu

| Test | Senaryo | Beklenen sonuç ve kanıt |
| --- | --- | --- |
| W4-T1 | Normal sample | stdout 4; `EXITED(0)`; CLI 0; child toplanmış |
| W4-T2 | Worker için dosya yok | stdout boş; worker stderr; `EXITED(1)`; CLI 1; child toplanmış |
| W4-T3 | Executable yok | `LAUNCH_ERROR`, stage exec; CLI 3; child toplanmış |
| W4-T4 | Worker'ın yanlış kullanımı | `EXITED(2)`; CLI 1; launcher kullanım hatasıyla karışmaz |
| W4-T5 | Geçerli worker 127 döndürüyor | `EXITED(127)`; CLI 1; exec hata tanısı yok |
| W4-T6 | Sağlanan SIGTERM worker'ı | `SIGNALED`; signal alanı SIGTERM; exit_code null; CLI 4; child toplanmış |
| W4-T7 | Aynı parent içinde 20 iş | Her child için tek başarılı reap; parent canlıyken kalan sonuç/zombie kontrolü |
| W4-T8 | Kontrollü wait EINTR | Aynı hedefe yeniden wait; doğru nihai sonuç; ikinci child yok |
| W4-T9 | Fork başarısızlığı enjeksiyonu | `LAUNCH_ERROR`, stage fork; child_pid null; child_reaped false; kaynak kapanışı |
| W4-T10 | Launcher argüman hatası | CLI 2; child oluşturulmaz |
| W4-T11 | Rapor oluşturma/yazma hatası | CLI 5; varsa başlamış child toplanır; eksik rapor başarı sayılmaz |
| W4-T12 | W1/W2/W3 regresyonu | Önceki sayı, hata ve host/guest smoke davranışları korunur |
| W4-T13 | Temiz kurulum | Belgelenmiş sürüm ve komutlarla host ve guest doğrulanır; yerel kalıntıya ihtiyaç yok |

T8–T11'in düzenekleri hazır verilir; öğrenci gerekli kısa hata dallarını ve sonucu yorumlar. T8 için önce deterministik syscall-wrapper enjeksiyonu kullanılabilir; bu test **injected** diye etiketlenir. Gerçek kesinti gösterimi yapılacaksa sağlanan handler, `SA_RESTART` ayarı ve hazır-olma eşgüdümü eğitmen tarafından doğrulanır. Enjeksiyon sonucu gerçek kernel olayıymış gibi raporlanmaz.

T11 iki alt durum içerir: rapor hazırlığı child oluşmadan başarısızsa child yoktur; yazma sonradan başarısızsa başlatılmış child'ın cleanup sorumluluğu sürer. Parser ve rapor yazıcısının kendi iç testleri altyapı sorumluluğudur.

### Guest kanıtı

Guest boot, sample 4, empty 0 ve sağlanan test programıyla normal guest çıkışı doğrulanır. xv6 sonucu guest `wait` sözleşmesiyle kaydedilir; `WIFSIGNALED` gibi Linux çözümü ve SIGTERM fixture'ı guest'e taşınmaz. `make guest-test` test runner'ın başarılı/başarısız özetidir; QEMU host exit code'u guest uygulama sonucu yerine kullanılmaz.

## 10. Temiz kurulum ve release paketi

Öğrencinin ürünü teslim alındığında şu kayıtlar bulunur:

```text
README.md       Ortam, kurulum, host/guest komutları, CLI ve sonuç sözleşmesi
BASELINE.md     W3 başlangıcı, xv6 commit'i, supplied/student ayrımı
CHANGELOG.md    W4 değişiklikleri ve giderilen hatalar
KNOWN_LIMITS.md Desteklenen iş yükü, kalan F alanları, doğrulanmamış iddialar
AI_USAGE.md     Araç/kaynak katkısı ve doğrulama
host/           Öğrenci launcher/worker kaynakları
xv6/            Yeniden elde edilebilir sabit kaynak ve uygulanan patch'ler
fixtures/       Ortak veri ve manifest
tests/         Sağlanan/öğrenci testlerinin açık ayrımı
evidence/      Kabul tablosu, ham sonuç, sürüm ve harita
```

Dosya adları önerilen starter düzenidir; eşdeğer bölümler aynı bilgiyi taşıyabilir. `xv6/` tam kaynak yerine commit+edinme adımı+patch ile yeniden kurulabiliyorsa bu açıkça yazılır. Sadece yerel bir klasöre referans vermek yeterli değildir.

### Yeniden üretim kontrolü

1. Teslimdeki kesin commit veya paket kimliğini aç; eski `build/` çıktısını kullanma.
2. Belgelenmiş Linux/toolchain ortamını ve xv6 sürümünü doğrula.
3. Fixture'ları manifestten hazırla; guest imajına doğru veri girdiğini kontrol et.
4. Sağlanacak `make host-test`, `make guest-test`, `make evidence` hedeflerini veya belgelenmiş eşdeğerlerini çalıştır.
5. Sonuçları ders test kimlikleriyle eşleştir; başarısız test saklanmaz.
6. Değişken PID/zaman alanları dışındaki iş sonuçlarını kontrol et.
7. Haritadaki her O iddiasının kanıtını, her F alanının sonraki artımını bul.
8. Release durumunu `kabul edildi / düzeltme gerekli` olarak kaydet ve gerekçeyi yaz.

Bu komut hedefleri yeni starter'da hazırlanacaktır; mevcut öğretim belgeleri deposunda çalıştırılabilir oldukları ileri sürülmez. Çalışan bir komutun başarı kodu ancak testlerin gerçekten koştuğunu ve assertion'ların değerlendirildiğini gösteren logla birlikte kullanılır.

## 11. Ürün kabulü ve değerlendirme rubriği

Kabul kapısı ile puan ayrı kaydedilir. Ana akış, child sahipliği veya sonuç türü yanlışsa ürün `v0.1` kapısını geçmez; öğrenci doğru kalan açıklama ve test tasarımı ölçütlerinden geri bildirim/puan alabilir. Düzeltilecek davranış açıkça belirtilir.

| Ölçüt | Beklenen kanıt | Puan |
| --- | --- | ---: |
| Uçtan uca çekirdek davranış | Normal/hatalı/sinyalli sonuç, doğru PID ve CLI/worker ayrımı | 30 |
| Sistem tasarımı ve kavramsal doğruluk | A8–A10, host/guest sınırı, sahiplik ve gelecekteki genişleme | 20 |
| Hata yönetimi ve cleanup | Tek reap, hata dalları, EINTR, kaynak/rapor hatası | 20 |
| Test ve yeniden üretilebilirlik | Kabul matrisi, regresyon, temiz kurulum, gerçek/enjekte kanıt ayrımı | 20 |
| Teknik gerekçe ve sürüm belgeleri | Kararlar, sınırlamalar, baseline ve katkı açıklaması | 10 |
| **Toplam** | | **100** |

Bu rubrik ana README'deki CEN dönem ürünü bileşeninin W4 kapısını ayrıntılandırır; ders toplamına yeni yüzde eklemez. W2/W3'te notlandırılan yerel ölçütler tekrar puanlanmaz; buradaki kanıtlar bütünleşik sonuç ve sürüm kalitesine uygulanır. Video anlatım kalitesi ve puanlanan sözlü kendi bileşenlerinde değerlendirilir.

### 90 saniyelik ürün açıklaması

Öğrenci yaklaşık 90 saniyede bir işi, sonucunu, cleanup kanıtını ve iki çapa bağlantısını gösterebilir. Bu, 2–4 dakikalık haftalık açıklama videosuna ek video gerektirmez; videodaki bir bölüm veya canlı kısa açıklama olabilir.

Bireysel soru kartları:

- Worker 2 döndürdü; CLI neden 1, rapor neden 2 gösteriyor?
- Exec hatasında child gerçekten oluştu mu; onu kim topladı?
- Sinyal testinin çıktısını yanlışlıkla `EXITED(0)` yapan koşulu bul.
- Parent'ı her testte kapatmak cleanup kanıtını neden zayıflatır?
- Aynı `job_id` ve farklı PID'ler hangi durumda anlamlıdır?
- Bu sürümde A8 veya A9 hakkında henüz kanıtlamadığın bir şeyi söyle.
- W5'te syscall sayacı eklemek için hangi mevcut arayüzü korumalısın?

Puanlanan sözlü bireysel ve AI'sızdır. Açıklama ile teslim uyuşmuyorsa yalnız tartışmalı ölçütler için odaklı yeniden savunma uygulanır; tek zayıf yanıt bütün ürünü otomatik sıfırlamaz.

## 12. Teslim, süre ve kurtarma tabanı

Son teslim zamanı LMS paketinde önceden yayımlanır. Teslim, sürüm kimliği, kaynak farkı, W4-T1–T13 kabul tablosu, harita, release belgeleri ve 2–4 dakikalık açıklamayı içerir. Ana teknik metin 1–2 sayfayı hedefler; log/diyagramlar eklerde tutulur.

Ders dışı Core hedefi: durum/cleanup düzeltme 55, sağlanan testleri çalıştırma/yorumlama 40, temiz kurulum 30, harita/release/katkı 40 dakika; toplam yaklaşık 165 dakika. Hazırlık ve en çok 30 dakikalık video ayrıdır. W3 süre yoklaması hedefin aşılacağını gösteriyorsa yeni altyapı işi çıkarılır, daha fazla hazır destek verilir.

W4 sonrasında öğretim ekibi test edilmiş `v0.1` kurtarma tabanı yayımlar. Bu tabanı alan öğrenci:

1. Aldığı kesin sürümü ve kendi başlangıcını belirtir.
2. Kendi sürümünde çalışmayan davranışı, kanıtını ve öğrendiğini açıklar.
3. Sonraki W5 artımını bu taban üzerinden normal biçimde yapar.
4. Hazır sürümü kopyalayarak geçmiş eksik puanı kazanmış sayılmaz.

Kurtarma tabanı da aynı kabul matrisiyle doğrulanır; eğitmen ürünü olması test gereksinimini kaldırmaz.

## 13. Exit ticket ve W5'e geçiş

Notlar/AI kapalı:

1. Kendi `v0.1` sürümünden bir kabul kanıtı ve bir sınırlama yaz.
2. Çocuğun sonlanması ile parent'ın onu toplaması arasındaki farkı söyle.
3. W5'te bir okuma sayacı eklendiğinde hangi iki regresyonu koruyacağını belirt.

**W5 başlangıç cümlesi:** “Elimizde artık bir işi çalıştırıp sonucunu güvenilir biçimde kaydeden taban var. Bu işin kernel sınırından geçerken yaptığı seçilmiş okumaları gözleyip, sınırlı bir xv6 istatistik çağrısıyla rapora yeni kanıt ekleyeceğiz.”

## 14. English student handout — Week 4

**Focus:** Integrate the same Workbench into a reproducible `v0.1` release. Build A8–A10 (virtual memory/isolation, file system/consistency, storage/I/O) in depth through the release's actual behavior and its declared limits; A1–A7 get only a brief callback, completing the second pass across Weeks 2–4.

**Your task:** Complete normal/nonzero/signal result classification, preserve launch-error reporting, finish direct-child ownership and run the supplied acceptance harnesses. The scope remains one direct child per call from a single-threaded parent. Job control, arbitrary nonterminating workloads and descendant-process supervision are outside this release.

**Acceptance evidence:** Provide the W4-T1–T13 matrix. It covers successful counting, worker errors, exec failure, a real exit 127, controlled SIGTERM, repeated jobs in a live parent, interrupted waiting, injected fork failure, launcher usage, report errors, regression and clean setup. Label injected observations as injected. Record actual test results rather than copying expected examples.

**Cleanup evidence:** Use the supplied persistent-parent harness. Every created child in the accepted scenarios must have a collected result. A process-list screenshot after the parent has exited is insufficient. Keep guest application status separate from Linux wait-status interpretation and QEMU status.

**Preparation questions:** Is normal termination always successful work? Can a failed wait provide a valid result? Must launcher and worker codes match? What evidence establishes cleanup? Is QEMU status the guest program status? Which virtual-memory claims remain future work?

**Submission:** A fixed release identity, code and baseline record, commands, fixtures, acceptance results, an A1–A10 integration map, changelog, known limits and contribution disclosure. Give a 2–4 minute explanation or an accessible equivalent, including a short walkthrough of one result and its cleanup evidence.

**Rubric:** 30 points for integrated behavior, 20 for system design, 20 for errors and cleanup, 20 for tests and reproducibility, and 10 for reasoning and release documentation. This is the Week 4 product rubric, not an added course-grade percentage. A failed acceptance gate still permits criterion-level feedback and correction.

**Support:** Target about 165 minutes of out-of-class Core work, excluding preparation and recording. After the gate, a tested instructor baseline supports Week 5 continuation; declare the version and explain your gap if you adopt it.
