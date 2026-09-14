# 2026–2027 Güz Dönemi Ders Tasarım Deposu

> **Bütünden ayrıntıya; çalışan ürün, sınanabilir iddia ve bağımsız teknik savunma.**

Bu depo, 2026–2027 Güz döneminde yürütülecek dört bilgisayar bilimi dersinin öğretim tasarımını, haftalık akışlarını, proje sözleşmelerini ve kaynaklarını bir arada tutar.

| Ders | Üniversite ve düzey | Dönem ürünü | Öğretim modeli | Ana başlangıç belgesi |
| --- | --- | --- | --- | --- |
| **CEN 302 — Operating Systems** | İstanbul Bilgi Üniversitesi · Lisans | Mini Systems Workbench | 14 hafta · `1 + 3 + 10` | [Proje ve ders planı](CEN_302/PROJE.md) |
| **SE 237 — Object Oriented Programming** | İstanbul Bilgi Üniversitesi · Lisans | Factory ERP — nesne modeli ve iş davranışları | 14 hafta · `1 + 3 + 10` | [ERP öğrenci rehberi](SE_237/ERP_OGRENCI_REHBERI.md) |
| **CMPE 351 — Database Systems** | İstanbul Bilgi Üniversitesi · Lisans | Factory ERP — ilişkisel veri omurgası | 12 hafta · `1 + 3 + 8` | [Ders modeli](CMPE_351/README.md) |
| **BİL 536 01 — Makine Öğrenmesi** | Maltepe Üniversitesi · Lisansüstü | ML Evidence Lab — yeniden üretilebilir ML çalışması | 14 hafta · `1 + 3 + 10` | [Ders modeli](BIL_536/README.md) |

**Öğretim elemanı:** M. Ali Bayram  
**Dönem:** 2026–2027 Güz  
**Belge dili:** Türkçe; teknik terimler, kod ve seçilmiş kaynaklar dersin gereğine göre İngilizce olabilir  
**Depo durumu:** Öğretim tasarımı ve materyal geliştirme aşaması

Bu depo resmî ders bilgi paketi veya kurum onayı yerine geçmez. Takvim, not ağırlıkları, sınav biçimi ve idarî koşullarda onaylanmış izlence ile üniversitenin güncel düzenlemeleri esas alınır.

## Hızlı erişim

### CEN 302 — Operating Systems

[Güncel kapsam](CEN_302/GUNCEL_KAPSAM.md) · [Proje planı](CEN_302/PROJE.md) · [W1](CEN_302/HAFTA01.md) · [W2](CEN_302/HAFTA02.md) · [W3](CEN_302/HAFTA03.md) · [W4](CEN_302/HAFTA04.md) · [Kaynakça](CEN_302/KAYNAKCA.md)

### SE 237 — Object Oriented Programming

[Güncel kapsam](SE_237/GUNCEL_KAPSAM.md) · [ERP öğrenci rehberi](SE_237/ERP_OGRENCI_REHBERI.md) · [W1](SE_237/HAFTA01.md) · [W2](SE_237/HAFTA02.md) · [W3](SE_237/HAFTA03.md) · [W4](SE_237/HAFTA04.md) · [Kaynakça](SE_237/KAYNAKCA.md) · [önceki proje planı](SE_237/PROJE.md)

### CMPE 351 — Database Systems

[Güncel kapsam](CMPE_351/GUNCEL_KAPSAM.md) · [Ders modeli](CMPE_351/README.md) · [ERP öğrenci rehberi](CMPE_351/ERP_OGRENCI_REHBERI.md) · [W1](CMPE_351/HAFTA01.md) · [W2](CMPE_351/HAFTA02.md) · [W3](CMPE_351/HAFTA03.md) · [W4](CMPE_351/HAFTA04.md) · [Kaynakça](CMPE_351/KAYNAKCA.md) · [önceki proje planı](CMPE_351/PROJE.md)

### BİL 536 01 — Makine Öğrenmesi

[Matematik ve algoritma kapsamı](BIL_536/KAPSAM_MATRISI.md) · [Ders modeli](BIL_536/README.md) · [W1](BIL_536/HAFTA01.md) · [W2](BIL_536/HAFTA02.md) · [W3](BIL_536/HAFTA03.md) · [W4](BIL_536/HAFTA04.md) · [2026–2027 lisansüstü akademik takvimi](BIL_536/archive/original_pdfs/Maltepe_Universitesi_2026-2027_Lisansustu_Akademik_Takvim.pdf) · [yalnız arşiv amaçlı eski izlence](BIL_536/archive/original_pdfs/BIL_536_Makine_Ogrenmesi_2023-2024_Bahar_Eski_Izlence.pdf)

Eski BİL 536 izlencesi yalnız tarihsel kayıt olarak saklanır. Yeni dersin kapsamı, sırası, öğretim yöntemi ve değerlendirme yaklaşımı bu belgeden kopyalanmamış; sıfırdan ve daha kapsayıcı biçimde tasarlanmıştır.

## Depoyu kullanma sırası

Bir ders için çalışma yaparken şu sırayı izleyin:

1. Bu ana README ile ortak öğretim yaklaşımını ve belge önceliğini okuyun.
2. Dersin **güncel kapsam/matris** belgesinden çekirdek konuları, modern köprüleri ve beklenen derinliği görün.
3. Yukarıdaki **ana başlangıç belgesini**, ardından ilgili `HAFTAxx.md` dosyasını açın; hazırlık, sınıf akışı, Core görev ve kanıtları alın.
4. Proje/öğrenci rehberinden veri, davranış ve kabul sözleşmesini doğrulayın.
5. Teknik kaynakları dersin `KAYNAKCA.md` dosyasından veya haftalık belgedeki sabit bağlantılardan seçin.
6. Tarihsel belgeleri yalnız güncel tasarımla karşılaştırarak kullanın.

## Ortak öğretim yaklaşımı

Dört ders aynı pedagojik omurgayı paylaşır:

> **Öğrenci önce sistemin bütününü görür, ikinci turda bütün ana konuları rehberli biçimde ilk kez işler, üçüncü turda aynı ürün üzerinde kanıtlı artımlar yaparak derinleşir.**

Bu yaklaşım konu listesini çalışan bir öğrenme sistemine dönüştürür. Öğrenci yalnızca bir yöntem veya komut öğrenmez; onun sistemdeki yerini, varsayımlarını, hata biçimlerini, alternatiflerini ve bedellerini açıklar.

### Üç öğretim turu

| Tur | 14 haftalık dersler | 12 haftalık CMPE 351 | Amaç | Ürün sonucu |
| --- | --- | --- | --- | --- |
| **1 — Panorama** | W1 | W1 | Bütün kavram haritasını tek uçtan uca vaka içinde görmek | Referans gösterim, küçük prototip ve başlangıç tanılaması |
| **2 — Rehberli ilk işleyiş** | W2–W4 | W2–W4 | Ana konuları üç gruba bölerek ilk kez sistematik işlemek | İskelet → dikey dilim → bütünleşik `v0.1` |
| **3 — Bilinçli derinleşme** | W5–W14 | W5–W12 | Her hafta bir çapa/modülü aynı ürün üzerinde ayrıntılandırmak | Kanıtlı artımlar → savunulabilir `v1.0` |

İkinci tur, dersin her hafta baştan anlatıldığı üç tekrar değildir. Konular W2, W3 ve W4 arasında paylaştırılır; üç haftanın toplamında ilk sistematik tur tamamlanır. Üçüncü turda öğrenci sorumluluğu ve kanıt derinliği kademeli olarak artar.

### Ortak öğrenme döngüsü

```text
Haritala → Geri çağır → Tahmin et → Modelle → Kur → Sına → Açıkla → Sınırlandır → Yansıt
```

| Adım | Öğrencinin yaptığı iş | Tipik çıktı |
| --- | --- | --- |
| **Haritala** | Konuyu sistemin bütünü ve komşu kavramlarla ilişkilendirir. | Çapa/modül haritası |
| **Geri çağır** | Önceki bilgiyi not ve AI desteği olmadan hatırlar. | Kısa cevap veya mekanizma özeti |
| **Tahmin et** | Sistemi çalıştırmadan önce sonucu ve gerekçeyi yazar. | Çıktı, trace, hata veya performans tahmini |
| **Modelle** | Akışı, durumu, bağımlılığı veya varsayımı görünür kılar. | UML, ER, denklem, tablo veya zaman çizgisi |
| **Kur** | Mevcut ürüne sınırlandırılmış bir artım ekler. | Kod, SQL, migration, deney veya yapılandırma |
| **Sına** | Mutlu yolun yanında karşı örnek ve hata koşulu kullanır. | Test, sorgu sonucu, ölçüm veya deney izi |
| **Açıkla** | Kararı ve mekanizmayı elde edilen kanıtla savunur. | Teknik gerekçe veya kısa sözlü açıklama |
| **Sınırlandır** | Kanıtın desteklemediği genellemeleri belirtir. | Varsayım, risk ve bilinen sınırlama |
| **Yansıt** | Hatasını ve bir sonraki değişikliği kaydeder. | Changelog, fark analizi veya backlog |

## Dört dersin ilişkisi ve sınırları

```text
CEN 302 ── Mini Systems Workbench
             süreç · thread · bellek · dosya · I/O

SE 237  ─┐
          ├── Factory ERP
CMPE 351 ─┘   nesne davranışları ↔ veri modeli ve transaction doğruluğu

BİL 536 ── ML Evidence Lab
             problem · veri · model · değerlendirme · risk · izleme
```

### Ortak Factory ERP vakası

SE 237 ve CMPE 351 aynı sentetik sandalye fabrikası bağlamını kullanır. Böylece aynı iş kuralının uygulama ve veri katmanlarındaki karşılıkları birlikte düşünülebilir:

- **SE 237**, iş davranışını, nesne sorumluluğunu, invariant'ı, sözleşmeyi ve değiştirilebilirliği inceler.
- **CMPE 351**, veri modelini, anahtarı, kısıtı, sorguyu, transaction'ı, eşzamanlılığı ve kurtarmayı inceler.

Bu iki ders birbirinin önkoşulu değildir. Bir öğrencinin diğer dersin katmanını geliştirmesi beklenmez; gerekli istemci, adaptör, veri veya iskele öğretim ekibince sağlanır. Ortak senaryo kullanılsa da öğrenme kanıtı ders ve öğrenci düzeyinde ayrı tutulur.

### Bağımsız ürünler

- **CEN 302**, Linux ve xv6-riscv üzerinde Mini Systems Workbench ile ilerler. ERP'ye bağlanması zorunlu değildir.
- **BİL 536**, ML Evidence Lab ile bağımsız bir araştırma/deney zinciri kurar. İlk ortak vaka sentetik öngörücü bakımdır; dönem problemi bu vakayla sınırlı değildir.

Kavramsal bağlantılar kurulabilir; ancak ortak depo, derslerden birine diğer dersin teknik kapsamını zorunlu kılmaz.

## Derslerin kısa profili

### CEN 302 — Operating Systems

**Ana soru:** Bir program işletim sistemi içinde nasıl çalışır, kaynak kullanır, hata verir ve gözlemlenir?

Mini Systems Workbench; küçük bir C programından süreç, thread, IPC, zamanlama, adres uzayı, sanal bellek, dosya sistemi ve I/O deneylerine doğru büyür. Linux gözlemi, xv6 davranışı ve öğretim modeli birbirine karıştırılmadan raporlanır.

**Başlıca kanıtlar:** syscall/process trace, eşzamanlılık deneyi, scheduler/bellek modeli, dosya ve I/O ölçümü, regresyon testi, ortamı belirtilmiş yeniden üretim.

### SE 237 — Object Oriented Programming

**Ana soru:** İş kurallarını koruyan ve değişen gereksinimlere uyarlanabilen nesneler nasıl tasarlanır?

Factory ERP'nin nesne katmanı; ürün/reçete, stok, üretim, sipariş, sevkiyat, maliyet ve dış sistem sınırları üzerinden gelişir. Encapsulation, ilişki, kalıtım, interface, polimorfizm, composition, kimlik, generics, exception ve runtime genişletilebilirlik gerçek davranışlar içinde sınanır.

**Başlıca kanıtlar:** invariant ve sözleşme testleri, nesne haritası/UML, hata sonrası tutarlı durum, değişiklik etkisi, fake adaptör, bireysel tasarım savunması.

### CMPE 351 — Database Systems

**Ana soru:** Veriler nasıl modellenir, sorgulanır ve eşzamanlı işlemlerde doğru, hızlı ve geri kazanılabilir tutulur?

Factory ERP'nin veri omurgası; kavramsal model, ilişkisel şema, bütünlük, SQL, normalizasyon, transaction, indeks, sorgu planı, dağıtık sistem ve modern PostgreSQL kararlarıyla kurulur. Ders 12 haftalık `1 + 3 + 8` modeline sahiptir.

**Başlıca kanıtlar:** migration ve seed, negatif constraint testi, beklenen sonuçlu SQL, iki oturumlu eşzamanlılık deneyi, sorgu planı, erişim kontrolü ve restore sonrası doğrulama.

### BİL 536 01 — Makine Öğrenmesi

**Ana soru:** Bir modelin ne öğrendiği, ne kadar genellediği ve gerçek kullanımda ne zaman güvenilir olduğu hangi kanıtlarla savunulur?

ML Evidence Lab; problem çerçeveleme ve veri üretim sürecinden klasik modellere, sinir ağlarına, modern temsillere, değerlendirme ve belirsizlikten sorumlu/üretimde ML yaşam döngüsüne uzanır. Öğrenci tek bir yeniden üretilebilir çalışmayı dönem boyunca derinleştirir.

**Başlıca kanıtlar:** baseline, sızıntısız split/pipeline, sabit deney protokolü, belirsizlik ve hata dilimleri, problem/data/model card, risk kaydı, yeniden üretim ve monitoring taslağı.

## Haftalık çalışma ve kanıt düzeni

Her haftalık dosya dersin gereğine göre uyarlansa da şu dört evre korunur:

### 1. Ders öncesi

- kısa ve hedefli okuma/video;
- AI ve not desteği olmadan geri çağırma;
- çalıştırmadan önce gerekçeli tahmin;
- ortam, starter ve veri kontrolü.

### 2. Ders içinde

- önceki haftayla bağlantı ve ana vaka;
- kısa kavram/mekanizma blokları;
- canlı gösterim veya kontrollü deney;
- bireysel ilk deneme ve küçük grup stüdyosu;
- test, açıklama ve exit ticket.

Ana planlarda 155 dakikalık oturum, **125 dakika etkin çalışma ve üç adet 10 dakikalık ara** olarak düzenlenir. Resmî ders/laboratuvar saati farklı olan derslerde ilgili haftalık belge esas alınır. BİL 536 `3+0` olduğundan ilk hafta için ayrı laboratuvar eklenmemiştir.

### 3. Ders sonrası

Öğrenci yeni ve kopuk bir ürün başlatmak yerine aynı dönem ürününe sınırlı bir artım ekler. Paket en az şunları içerir:

```text
Ders ve hafta:
Ana çapa/modül:
Başlangıç sürümü ve ortam:
Bu haftanın tek cümlelik artımı:
Çalıştırma/doğrulama komutları:
Beklenen ve gözlenen sonuç:
Hata, karşı örnek veya sınır durumu:
Korunan eski davranış ve regresyon kanıtı:
Tasarım kararı, alternatif ve bedel:
Bilinen sınırlama:
Dış kaynak, sağlanan altyapı ve AI katkısı:
```

### 4. Bireysel doğrulama

Grup ürünü veya araç desteği kullanılsa bile her öğrenci seçilen bir dosyayı, sorguyu, deneyi, trace'i ya da tasarım kararını kendi sözleriyle açıklayabilmelidir. Teslim ile açıklama arasında uyumsuzluk varsa ilgili ölçüt için odaklı yeniden kontrol yapılır.

## Ortak tamamlanma tanımı

Bir haftalık artım veya sürüm şu kanıtlar birlikte bulunduğunda tamamlanmış sayılır:

- başlangıç sürümü, ortam ve gerekli komutlar bellidir;
- Core davranış anlamlı bir senaryoda çalışır;
- en az bir sınır, hata veya karşı örnek sınanmıştır;
- ilgili eski davranışlar için regresyon kanıtı vardır;
- teknik çıktı ile kavram/sistem haritası uyumludur;
- tercih, alternatif, bedel ve sınırlama açıklanmıştır;
- kullanılan kaynaklar, sağlanan altyapı ve dış katkılar belirtilmiştir;
- öğrenci küçük bir değişikliğin etkisini bağımsız olarak savunabilir.

Ekran görüntüsü, tek başarı çıktısı, yüksek skor veya test aracının yeşil işareti tek başına yeterli kanıt değildir. Sonucun hangi sürümde, hangi koşulda ve hangi sınırlar içinde elde edildiği görünür olmalıdır.

## Kapsam ve iş yükü yönetimi

Her görev üç şeritten biriyle işaretlenir:

- **Core:** Her öğrencinin tamamlaması gereken, süresi ve kabul koşulu belirli çekirdek iş.
- **Stretch:** Core tamamlandıktan sonra yapılabilecek isteğe bağlı genişletme.
- **Instructor demo:** Özel donanım, yüksek hesaplama veya kapsam dışı altyapı gerektiren eğitmen gösterimi.

Stretch çalışması eksik Core'un yerine geçmez. Starter, adaptör, veri üretici, web iskeleti veya yüksek maliyetli altyapı dersin ana öğrenme hedefi değilse öğretim ekibince sağlanır.

### Geri kazanım tabanı

W4 sonunda doğrulanmış `v0.1`, daha sonraki haftalar için ortak geri kazanım noktasıdır. Geride kalan öğrenci:

1. dersin yayımlanmış taban sürümünden başlar;
2. yalnız eksik Core kabul testlerini tamamlar;
3. kendi çözümü ile taban arasındaki farkı açıklar;
4. kısa bireysel kontrolle ana akışa yeniden katılır.

Hazır taban kullanmak önceki eksik çalışmanın puanını otomatik kazandırmaz; sonraki öğrenmenin tek bir erken teknik hata yüzünden durmasını engeller.

## Ölçme ve değerlendirme ilkeleri

Derslerin not ağırlıkları kendi ana belgelerinde ve resmî izlencelerinde tanımlanır. Ortak tasarım şu ilkeleri korur:

- ürün kalitesi ile bireysel anlayış birlikte ölçülür;
- hazırlık, stüdyo, haftalık artım, ara/final çalışma ve sözlü savunma farklı kanıt türleridir;
- aynı davranış farklı bileşenlerde tekrar tekrar puanlanmaz;
- otomatik testler değerlendirmeyi destekler, notu tek başına belirlemez;
- doğru akıl yürütme ile çalışan ürün ayrı rubrik ölçütleri olarak görülebilir;
- öğrenciye değerlendirilmediği bir görev türü ilk kez sınavda verilmez;
- grup çalışmasında bireysel katkı ve anlayış görünür tutulur.

## Üretken yapay zekâ ve kaynak kullanımı

> **Açık öğrenme çalışmalarında AI kullanılabilir; öğrenci teslim ettiği her önemli kısmı açıklayabilmeli, sınayabilmeli, değiştirebilmeli ve savunabilmelidir.**

AI kullanımı isteğe bağlıdır ve ücretli araç zorunlu değildir. Dersin açıkça izin verdiği hazırlık, proje ve laboratuvar çalışmalarında açıklama, alternatif tasarım, kod/SQL taslağı, deney planı, hata ayıklama veya test fikri için kullanılabilir.

Kısa bireysel yoklamalar, sınavlar ve sözlü savunmalar AI'sızdır. Stüdyo çalışmalarının ilk bireysel denemesi de aksi belirtilmedikçe AI'sız yapılır.

AI kullanılan her pakette şu kayıt bulunur:

```text
Araç/model veya “AI kullanılmadı”:
Kullanım amacı ve etkilenen bölüm:
Önemli etkileşimin kısa özeti:
Kabul edilen, değiştirilen veya reddedilen öneri ve gerekçesi:
Sonucu doğrulayan test, sorgu, trace veya deney:
Diğer insan katkıları, sağlanan altyapı ve dış kaynaklar:
```

AI çıktısı kaynak, ölçüm veya deney sonucu yerine geçmez. Kaynak, log, performans değeri, veri veya yazarlık bilgisi uydurulamaz. Başka öğrencilere ait çalışmalar, kişisel/kurumsal veriler, erişim anahtarları ve yayımlanmamış değerlendirme materyalleri dış araçlara yüklenmez.

## Belge önceliği ve sürüm disiplini

Belgeler arasında farklılık olduğunda şu sıra uygulanır:

1. **Resmî koşullar:** Onaylanmış izlence, akademik takvim ve üniversite düzenlemeleri.
2. **Ders mimarisi:** Dersin `README.md`, `PROJE.md` veya güncel öğrenci rehberi.
3. **Haftalık sözleşme:** İlgili `HAFTAxx.md`; başlangıç sürümü, Core görev, teslim ve kabul ölçütü.
4. **Teknik kaynak:** `KAYNAKCA.md` veya haftalık belgede sabitlenen kaynak/sürüm.
5. **Arşiv:** Yalnız tarihsel inceleme; güncel görev olarak kendiliğinden yürürlüğe girmez.

Her çalıştırılabilir paket; sürüm, ortam, bağımlılık, veri/fixture kimliği ve bilinen sınırlamalarla yayımlanır. Bir özelliğin belgede planlanmış olması, kodunun veya testinin hazır olduğu anlamına gelmez.

## Güncel klasör yapısı

```text
.
├── README.md
├── CEN_302/
│   ├── PROJE.md
│   ├── HAFTA01.md ... HAFTA04.md
│   └── KAYNAKCA.md
├── SE_237/
│   ├── ERP_OGRENCI_REHBERI.md
│   ├── PROJE.md
│   ├── HAFTA01.md ... HAFTA04.md
│   └── KAYNAKCA.md
├── CMPE_351/
│   ├── README.md
│   ├── ERP_OGRENCI_REHBERI.md
│   ├── PROJE.md
│   ├── HAFTA01.md ... HAFTA04.md
│   └── KAYNAKCA.md
├── BIL_536/
│   ├── README.md
│   ├── HAFTA01.md ... HAFTA04.md
│   └── archive/original_pdfs/
│       ├── BIL_536_Makine_Ogrenmesi_2023-2024_Bahar_Eski_Izlence.pdf
│       └── Maltepe_Universitesi_2026-2027_Lisansustu_Akademik_Takvim.pdf
└── archive/
    ├── README.md
    └── önceki ortak tasarım ve planlama belgeleri
```

Dosya adlarında ders kodu ve hafta numarası korunur. Yeni haftalar iki basamaklı olarak `HAFTA05.md` biçiminde eklenir. Üretilmiş geçici dosyalar, editör artıkları, gizli bilgiler ve yerel ortam klasörleri depoya eklenmez.

## Mevcut durum ve sonraki üretimler

| Ders | Hazır tasarım belgeleri | Sıradaki doğal işler |
| --- | --- | --- |
| **CEN 302** | Proje planı, W1–W4, kaynakça | Starter/test paketleri ve W5–W14 ayrıntıları |
| **SE 237** | ERP rehberi, W1–W4, kaynakça | W1 senaryo uyumu, starter/test paketleri ve W5–W14 ayrıntıları |
| **CMPE 351** | Ders modeli, ERP rehberi, W1–W4, kaynakça | W1 senaryo uyumu, veri ortamı/test paketleri ve W5–W12 ayrıntıları |
| **BİL 536** | Yeni ders modeli, W1–W4, eski izlence ve takvim arşivi | Starter/sentetik veri, W5–W14 ve haftalık kaynak sabitleme |

Önerilen materyal üretim sırası:

```text
belge uyumu
  → ortam ve sürüm sabitleme
  → W1 starter + test/deney verisi
  → panorama referans gösterimi
  → W2–W4 iskeleti ve v0.1 kapısı
  → derinleşme haftaları
  → ölçme araçları ve değerlendirici kalibrasyonu
```

## Dönem sonunda başarı

Başarı yalnızca ürünün çalışması değildir. Öğrenci dönem sonunda:

- sistemin veya araştırma iddiasının bütününü haritalayabilmeli;
- bir mekanizmanın girdiden çıktıya nasıl ilerlediğini gösterebilmeli;
- çalıştırmadan önce gerekçeli tahmin yapabilmeli;
- mutlu yolun yanında hata, sınır durum ve karşı örnek üretebilmeli;
- yeni davranış eklerken eski davranışları koruyabilmeli;
- seçtiği çözümü alternatif ve bedelleriyle savunabilmeli;
- başka birinin aynı sürümü ve kanıtı yeniden üretmesini sağlayabilmeli;
- kendi katkısını, sağlanan altyapıyı, dış kaynakları ve AI yardımını ayırabilmelidir.

| Aşama | Beklenen durum |
| --- | --- |
| **W1 sonunda** | Küçük prototipi/deneyi çalıştırır; dersin bütünü içindeki yerini ve en az bir yanıltıcı sezgiyi açıklar. |
| **W4 sonunda** | `v0.1` sürümünü temiz başlangıçtan kurar; uçtan uca akışı, hata yolunu ve ilk tasarım kararlarını savunur. |
| **Dönem sonunda** | Aynı ürüne bağımsız bir değişiklik yapar; mekanizmayı, etkilenen parçaları, sınırları ve doğruluk kanıtını açıklar. |

Depodaki bütün belgeler ve materyaller bu ilerlemeyi planlamak, desteklemek ve görünür kılmak için hazırlanır.
