# BİL 536 01 Makine Öğrenmesi — 2. Hafta Öğretim Dosyası

**Hafta:** Problemden geçerli deneye — Ç1, Ç2 ve Ç3  
**Dönem yapısı:** 14 hafta, `1 + 3 + 10`  
**Süre:** 155 dakika ders; 125 dakika etkin çalışma + üç adet 10 dakikalık ara  
**Dönem ürünü:** Yeniden Üretilebilir Makine Öğrenmesi Çalışması (`ML Evidence Lab`)  
**İkinci turdaki yeri:** İlk bölüm — problem, genelleme ve veri/doğrulama sözleşmesi  
**İlgili belgeler:** [Ders modeli](README.md) · [1. hafta](HAFTA01.md) · [3. hafta](HAFTA03.md) · [4. hafta](HAFTA04.md)

Bu hafta yeni ve karmaşık bir algoritma seçme haftası değildir. Öğrenci önce hangi kararı desteklediğini, verinin nasıl oluştuğunu ve gelecekteki kullanımı hangi değerlendirme düzeninin temsil ettiğini kesinleştirir. Model ailesi seçimi, geçerli bir deney sözleşmesinden sonra gelir.

## Haftanın ana sorusu

> Modeli eğitmeden önce hangi kararlar verilmezse, elde edilen başarı skoru bilimsel olarak savunulamaz hâle gelir?

## Bu hafta ayrıntılı işlenecek çapalar

| Çapa | Bu haftanın odağı | Haftanın ürün karşılığı |
| --- | --- | --- |
| **Ç1** | Problem çerçeveleme, veri üretim süreci ve baseline | `problem_card.md`, karar ve maliyet tablosu, dummy/kural baseline |
| **Ç2** | Matematiksel/istatistiksel temel ve genelleme | Kayıp–metrik ayrımı, varsayım kaydı, küçük kapasite/regularization deneyi |
| **Ç3** | Veri, önişleme, özellikler ve doğrulama tasarımı | `data_card.md`, split sözleşmesi, pipeline ve leakage kontrolleri |

Ç4–Ç10 bu hafta yeniden anlatılmaz. Model aileleri, ileri değerlendirme, sinir ağları ve üretim yaşam döngüsü yalnızca alınan kararların daha sonra nereye bağlanacağını göstermek için kısa olarak haritada tutulur.

## Ders sonunda öğrencinin göstereceği kanıt

Öğrenci:

- tahmin problemi ile bu tahmini kullanan karar problemini ayırır;
- karar sahibi, gözlem birimi, tahmin anı, hedef, tahmin ufku ve eylemi tanımlar;
- yanlış pozitif ve yanlış negatif maliyetlerinin neden simetrik olmayabileceğini açıklar;
- hedefin tahmin anında gerçekten bilinemez ve daha sonra güvenilir biçimde ölçülebilir olduğunu kontrol eder;
- veri üretim sürecini kaynak → örnekleme → ölçüm → etiket → kullanım zinciriyle çizer;
- selection, measurement, survivorship ve historical bias için en az bir olası kaynak belirtir;
- loss, evaluation metric ve karar maliyetinin aynı kavram olmadığını örnekle gösterir;
- empirical risk, overfitting, underfitting, bias–variance ve regularization ilişkisini küçük bir deneyde yorumlar;
- random, stratified, group ve time split seçeneklerini kullanım senaryosuna göre karşılaştırır;
- preprocessing'i split sonrasında ve yalnız training katında fit eden bir pipeline kurar;
- target, temporal, group ve preprocessing leakage için kontrol üretir;
- basit bir baseline'ı aynı protokolde çalıştırır ve daha karmaşık modelden önce kaydeder.

## Eğitmenin ders öncesi hazırlığı

### Ortak materyaller

- W1 sentetik öngörücü bakım verisinin sürümünü ve bütünlük özetini sabitle.
- Aşağıdaki üç problem cümlesini hazırla:
  1. belirsiz ve model-merkezli kötü tanım,
  2. hedefi tahmin anından sonra oluşan sızıntılı tanım,
  3. karar sahibi, zaman ve eylemi açık iyi tanım.
- Aynı veri için random, group ve time-aware split kimliklerini önceden üret.
- Bilerek hatalı iki preprocessing örneği hazırla: bütün veride scaling ve split öncesi missing-value imputation.
- Majority baseline ile basit kural baseline'ını aynı metrik dosyasında çalıştır.
- Düşük ve yüksek kapasiteli iki küçük modelin train/validation eğrilerini hazırla; ayrıntılı algoritma karşılaştırmasını W3'e bırak.

### Öğrenci proje yolu

Öğrenci iki yoldan birini seçebilir:

- **Ortak veri yolu:** W1 öngörücü bakım verisiyle devam eder.
- **Kendi problem yolu:** Açık lisanslı, sentetik veya izinli bir veriyle öneri sunar; eğitmen onayına kadar ortak veri üzerinde çalışır.

Kendi problem yolunda kişisel/hassas veri, belirsiz lisans, erişilemeyen veri veya dönem süresini aşan veri toplama planı kabul edilmez. Yüksek riskli bir bağlam seçilirse otomatik karar sistemi yapılmaz; karar desteği, risk ve insan gözetimi açıkça tanımlanır.

### Starter ve şablonlar

- `problem_card.md` ve `data_card.md` şablonları.
- `split_contract.yaml`: birim, zaman sınırı, grup alanı, oranlar ve seed.
- `src/data.py`: schema doğrulama, split üretme ve pipeline girişleri.
- `src/baseline.py`: dummy ve verilen kural baseline'ı.
- `tests/test_data_contract.py`: hedef, zaman, duplicate, leakage ve split ayrıklığı kontrolleri.
- `configs/week02.yaml`: veri ve deney kimlikleri.
- Kurulum aksaması için sabit örnek çıktı ve tamamlanmış tek bir referans çalışma.

## Ders öncesi öğrenci hazırlığı — 35–45 dakika

1. W1 exit ticket yanıtını tekrar oku; en yüksek skorun neden geçerli olmadığını tek cümlede yeniden yaz.
2. Dönem çalışması için iki aday problem getir. Her biri için yalnız şu beş alanı doldur:
   - karar sahibi,
   - gözlem birimi,
   - tahmin anı,
   - hedef ve tahmin ufku,
   - yanlış kararın olası bedeli.
3. Aşağıdaki kavramları notasız tanımlamayı dene: feature, target, sample, population, loss, metric, baseline, train/validation/test.
4. Şu tahmini yaz: Aynı makinenin kayıtları train ve testte bulunursa skor hangi yönde değişebilir ve neden?
5. Starter ortamını doğrula; verilen `validate-data` ve `make-splits --dry-run` komutlarını çalıştır.

## Tahta/slayt omurgası

```text
Karar sahibi ve eylem
        ↓
Gözlem birimi + tahmin anı + ufuk
        ↓
Hedefin ölçülmesi
        ↓
Veri üretim süreci ve örnekleme
        ↓
Train / validation / W4 checkpoint / W14 final blind holdout sözleşmesi
        ↓
Training-only preprocessing pipeline
        ↓
Baseline + kayıp + metrik + karar maliyeti
        ↓
Sınırlı ve yanlışlanabilir deney iddiası
```

Tahtanın yanında üç ayrı soru görünür tutulur:

```text
Ne optimize ediyoruz?  → loss
Neyi raporluyoruz?     → evaluation metric
Hangi sonucu önemsiyoruz? → decision/impact cost
```

## 155 dakikalık ders akışı

| Süre | İçerik ve öğretmen hamlesi | Öğrenci işi / toplanan kanıt |
| --- | --- | --- |
| 00–08 | W1 retrieval: sızıntılı yüksek skor vakasını yeniden kur. | Bireysel üç cümle: iddia–kanıt–sınır |
| 08–18 | **Ç1:** Tahmin ve karar ayrımı; karar sahibi, eylem, gözlem birimi, tahmin anı ve ufuk. | Kötü problem cümlesini yeniden yazma |
| 18–25 | Hedef tanımı, label availability ve yanlış pozitif/negatif maliyeti. | Hedef zaman çizgisi + maliyet tablosu |
| 25–30 | Baseline türleri: çoğunluk, ortalama, son değer ve basit alan kuralı. | Probleme uygun en ucuz baseline seçimi |
| 30–40 | **Ara** | |
| 40–50 | Veri üretim süreci: kaynak, örnekleme, ölçüm, etiket ve kullanım dağılımı. | DGP diyagramı |
| 50–59 | Bias kaynakları: selection, measurement, historical ve survivorship. | Bir bias kaynağı + beklenen yön |
| 59–66 | **Ç2:** Random variable, conditional probability, expectation ve variance sezgisi. | Küçük olasılık/ortalama sorusu |
| 66–70 | Loss, metric ve karar maliyetinin ayrılması. | Üçlü eşleştirme kartı |
| 70–80 | **Ara** | |
| 80–90 | Empirical risk, train/validation farkı, underfit/overfit ve genelleme. | İki learning curve yorumu |
| 90–98 | Model kapasitesi, bias–variance ve regularization için kontrollü deney. | Sonucu çalıştırmadan tahmin + gözlem |
| 98–104 | **Ç3:** Random, stratified, group ve time split; hangi iddiayı sınadıkları. | Dört vaka–split eşleştirmesi |
| 104–110 | Development, W4 checkpoint ve W14 final blind holdout rolleri. | Hangi veriye ne zaman dokunulacağı sözleşmesi |
| 110–120 | **Ara** | |
| 120–130 | Preprocessing pipeline; imputation/encoding/scaling yalnız training fold'da fit edilir. | Hatalı akışta leakage yerini işaretleme |
| 130–143 | Stüdyo: problem card, DGP, split contract ve baseline'ın ilk çalışan sürümü. | Dört dosyalık Core paket |
| 143–150 | Akran kırmızı takım: başka ekip tek bir leakage/bias/karar belirsizliği arar. | Bulgu + kabul/red gerekçesi |
| 150–155 | Bireysel exit ticket; iki holdout'un hâlâ kapalı olduğunu doğrula. | Çıkış kaydı |

## Üç çapanın bu hafta için doğru derinliği

| Çapa | Bu hafta ayrıntılı işle | Sonraki derinleşmeye bırak |
| --- | --- | --- |
| Ç1 | Karar sahibi, birim, zaman, hedef, ufuk, eylem, maliyet, DGP ve baseline | Kapsamlı stakeholder araştırması, nedensel etki tahmini ve saha deneyi |
| Ç2 | Olasılık/istatistik sezgisi, empirical risk, capacity, under/overfit, bias–variance ve regularization | Ayrıntılı ispatlar, ileri Bayesçi çıkarım ve genelleme sınırları |
| Ç3 | Veri sözleşmesi, split seçimi, training-only preprocessing ve temel leakage/bias kontrolleri | Gelişmiş feature selection, missingness mekanizmaları ve nested CV ayrıntıları |

## Stüdyo görevi: modelden önce deney sözleşmesi

Her ekip ortak veya onay bekleyen kendi problemi için en küçük geçerli deney omurgasını kurar.

### A. Problem card

Şu alanlar boş bırakılamaz:

```text
Karar sahibi:
Desteklenen karar ve olası eylem:
Gözlem birimi:
Tahmin anı:
Hedef ve tahmin ufku:
Hedefin ne zaman/nasıl ölçüldüğü:
Yanlış pozitif ve yanlış negatif bedeli:
Kapsam dışı kullanım:
En ucuz anlamlı baseline:
Başarı iddiasını yanlışlayacak sonuç:
```

### B. Data card taslağı

- veri kaynağı, lisans/izin ve sürüm;
- satırın neyi temsil ettiği;
- kapsanan zaman, grup ve popülasyon;
- hedef üretim yolu;
- eksik değer, duplicate ve sınıf dağılımı özeti;
- tahmin anında kullanılamayan alanlar;
- olası bias, gizlilik ve temsil sınırları.

### C. Split sözleşmesi

Öğrenci yalnız oran yazmaz; ayrımın savunduğu kullanım iddiasını belirtir.

```yaml
unit: machine_timestamp
group_key: machine_id
time_key: timestamp
strategy: group_time
development_window: documented_range
checkpoint_holdout_window: documented_future_range
final_blind_holdout_window: documented_later_range
seed: documented_integer
checkpoint_access: week04_once_after_acceptance
final_blind_access: week14_once_after_acceptance
```

Alan adları probleme göre değişebilir. Zaman veya grup yapısı olmayan veri setinde bunun neden olmadığı belgelenir; otomatik olarak sahte alan eklenmez.

### D. Çalışan baseline

- schema doğrulaması geçer;
- split'ler birim ve grup sözleşmesine göre ayrıdır;
- dummy veya basit kural baseline'ı yalnız development protokolünde çalışır;
- preprocessing pipeline içinde ve training katında fit edilir;
- veri özeti, split kimliği, seed ve metrikler kaydedilir;
- W4 checkpoint ve W14 final blind holdout sonuçlarına W2'de bakılmaz.

## Core kabul koşulları

- Problem card'da karar, birim, zaman, hedef ve eylem birbiriyle tutarlıdır.
- Hedef tahmin anından sonra ölçülür; fakat model girdilerinde geleceği açığa çıkaran alan yoktur.
- Veri kaynağı ve kullanım izni/lisansı belgelenmiştir.
- DGP diyagramı en az kaynak, örnekleme, ölçüm, etiket ve kullanım aşamalarını içerir.
- Split stratejisi gerçek kullanım iddiasıyla gerekçelendirilmiştir.
- Train/validation, W4 checkpoint holdout ve W14 final blind holdout rolleri ayrıdır.
- Öğrenilen önişleme bütün veri üzerinde fit edilmez.
- Baseline ve sonuç kaydı tek komutla yeniden üretilebilir.
- En az iki otomatik veri/split kontrolü vardır.
- Öğrenci bir leakage ve bir bias riskini sözlü açıklayabilir.

## Stretch

- İki olası split stratejisinde baseline değişkenliğini karşılaştır.
- Basit kural baseline'ı oluştur ve dummy baseline'a karşı hangi ek bilgiyi kullandığını açıkla.
- Eksik değer desenini hedef ve zamanla çaprazla; nedensiz imputasyon önerme.
- Küçük bir model kapasitesi/regularization deneyi ekle; final model seçimi yapma.

## Kritik sorular ve beklenen yön

- **Tahmin hedefi neden iş hedefiyle aynı değildir?** Model bir sonucu tahmin eder; değer ancak bu tahmin belirli bir kararı daha iyi kılarsa oluşur.
- **Hedefi ölçebiliyor olmak onun doğru hedef olduğu anlamına gelir mi?** Hayır; kolay ölçülen proxy, asıl sonucu yanlış temsil edebilir ve zararlı teşvik üretebilir.
- **Daha fazla veri her zaman daha iyi midir?** Temsil etmeyen, yanlış etiketli, sızıntılı veya kullanım dışı veri örnek sayısını artırırken geçerliliği düşürebilir.
- **Stratified split neyi çözmez?** Sınıf oranını korur; zaman, kişi/makine grubu ve duplicate sızıntısını kendiliğinden çözmez.
- **Holdout neden erkenden açılmaz?** Tekrarlanan kararlar holdout'a uyum sağlayarak onu tarafsız değerlendirme olmaktan çıkarır. W4 checkpoint süreç provası, W14 final blind holdout ise son sürüm içindir.
- **Loss ve metric neden farklı olabilir?** Optimizasyon için türevlenebilir bir surrogate gerekirken raporlanan karar metriği farklı olabilir.
- **Regularization yalnız küçük katsayı demek midir?** Daha genel olarak modelin etkili kapasitesini veya çözüm tercihlerini sınırlar.
- **Baseline'ın güçlü olması neden yararlıdır?** Karmaşıklığın gerçekten ek değer üretip üretmediğini ve veri/protokol hatalarını görünür kılar.

## Yaygın yanılgılar ve müdahale

- “Önce iyi model bulalım, problemi sonra yazarız.” → Aynı skorun iki farklı karar bağlamında zıt değer taşıdığı örnek ver.
- “Hedef sütunu varsa problem tanımlıdır.” → Hedefin oluşma zamanı, ölçüm hatası ve proxy ilişkisini sordur.
- “Veri halka açıksa sınırsız kullanılabilir.” → Lisans, amaç, kişisel veri ve yeniden dağıtım koşullarını kontrol ettir.
- “Stratification bütün sızıntıları engeller.” → Aynı makinenin yakın kayıtlarını iki tarafa yerleştirip karşı örnek göster.
- “Preprocessing model değildir.” → İmputer ve scaler parametrelerinin veriden öğrenildiğini göster.
- “Validation en iyi modeli seçer; test gereksizdir.” → Model seçiminin validation'a uyum sağladığını ve bağımsız test rolünü çiz.
- “Düşük training hatası öğrenmenin kanıtıdır.” → Train/validation eğrisi ve kapasite farkıyla genelleme sorusunu geri getir.

## Hafta sonu teslim paketi

```text
week02/
├── problem_card.md
├── data_card.md
├── dgp_diagram.*
├── split_contract.yaml
├── configs/week02.yaml
├── src/data.py
├── src/baseline.py
├── tests/test_data_contract.py
├── artifacts/data_summary.json
├── artifacts/baseline_metrics.json
├── decision_log.md
└── AI_ASSISTANCE.md
```

Teslimde en fazla 300 kelimelik teknik not bulunur:

1. Bu split hangi gerçek kullanım iddiasını sınar?
2. En ciddi leakage veya bias riski nedir?
3. Baseline hangi bilgiyi kullanır ve neyi göstermez?
4. W3'te model karşılaştırmadan önce hangi sözleşme değişmemelidir?

## Exit ticket

1. Tahmin anı ile hedefin ölçülme zamanı arasındaki fark nedir?
2. Senin problemin için random split hangi nedenle geçerli veya geçersiz?
3. Loss, metric ve karar maliyetine birer örnek ver.
4. Baseline'ı yenmek neden tek başına yeterli değildir?
5. W4 checkpoint holdout ve W14 final blind holdout hangi farklı amaçlarla korunacak?

## 3. haftaya köprü

> **Artık neyi, kimin için ve hangi veri sözleşmesiyle sınadığımız belli. 3. haftada aynı sabit protokol içinde basit olasılıksal modelleri, doğrusal olmayan alternatifleri ve güvenilir değerlendirmeyi kuracağız; protokolü modele göre değiştirmeyeceğiz.**

W3'e gelmeden önce her ekip problem ve split sözleşmesindeki eğitmen geri bildirimini kapatır. Onaylanmamış kendi veri yolu yerine ortak veri yolunu kullanır. W4 checkpoint ve W14 final blind holdout sonuçları açılmaz.
