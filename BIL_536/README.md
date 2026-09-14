# BİL 536 01 Makine Öğrenmesi — 14 Haftalık Ders İşleme Sistemi

**Üniversite:** Maltepe Üniversitesi  
**Program düzeyi:** Lisansüstü  
**Ders:** BİL 536 01 Makine Öğrenmesi  
**Dönem:** 2026–2027 Güz  
**Ders süresi:** 14 öğretim haftası, haftada 3 saat  
**Ders modeli:** `1 + 3 + 10` spiral  
**Ders dili:** Türkçe; İngilizce teknik terimler ve kaynaklar kullanılır  
**Dönem ürünü:** Yeniden Üretilebilir Makine Öğrenmesi Çalışması (`ML Evidence Lab`)  
**Teknik omurga:** Python, NumPy, pandas, scikit-learn, PyTorch ve Git; CPU-öncelikli

## İlgili belgeler

[Matematik ve algoritma kapsam matrisi](KAPSAM_MATRISI.md) · [W1](HAFTA01.md) · [W2](HAFTA02.md) · [W3](HAFTA03.md) · [W4](HAFTA04.md) · [2026–2027 lisansüstü akademik takvimi](archive/original_pdfs/Maltepe_Universitesi_2026-2027_Lisansustu_Akademik_Takvim.pdf) · [yalnız arşiv amaçlı eski izlence](archive/original_pdfs/BIL_536_Makine_Ogrenmesi_2023-2024_Bahar_Eski_Izlence.pdf)

Eski izlence yalnız tarihsel kayıt olarak saklanır; bu ders tasarımının konu, sıra, yöntem veya değerlendirme referansı değildir. Bu README ve haftalık dosyalar sıfırdan oluşturulan yeni ders sistemini tanımlar.

## Bu belgenin amacı

Bu README, BİL 536'nın haftalık konu listesinden fazlası olmasını sağlayan öğrenme sistemini tanımlar. Ders; algoritma adlarını art arda anlatmak yerine problem tanımından veriye, modelden değerlendirmeye, riskten üretim izlemeye uzanan bütün makine öğrenmesi yaşam döngüsünü birkaç kez ve artan derinlikle ele alır.

Temel ilke şöyledir:

> **Öğrenci önce bütün makine öğrenmesi yaşam döngüsünü görür, on çapayı üç haftada rehberli biçimde ilk kez kurar ve ardından her çapayı aynı araştırma ürünü üzerinde kanıt üreterek derinleştirir.**

Bu tasarımda “çalıştı” tek başına yeterli değildir. Her iddia; uygun karşılaştırma, ayrılmış test verisi, deney kaydı, belirsizlik ve açık sınırlamalarla desteklenir.

## Neden spiral bir sistem kullanıyoruz?

Makine öğrenmesinde model seçimi; veri toplama, örnekleme, hedef değişken, kayıp, değerlendirme metriği ve kullanım bağlamından bağımsız değildir. Doğrusal bir derste öğrenci bu bağlantıları dönem sonunda kurmaya çalışır; veri sızıntısı, yanlış test düzeni veya bağlam dışı metrik seçimi yüzünden teknik olarak çalışan fakat bilimsel olarak savunulamayan sonuçlar üretebilir.

Spiral sistemde aynı karar zinciri baştan görünür olur:

```text
Karar problemi ve veri üretim süreci
   ↓
Hedef, örnekleme, bölme ve baseline
   ↓
Önişleme, özellikler ve model
   ↓
Eğitim, optimizasyon ve doğrulama
   ↓
Metrikler, belirsizlik ve hata analizi
   ↓
Sağlamlık, adalet, gizlilik ve güvenlik
   ↓
Yeniden üretilebilir paket, sunum ve izleme
   ↓
Kanıtla sınırlandırılmış karar
```

Her yeni matematiksel veya algoritmik ayrıntı bu zincirdeki yerine yerleştirilir. Amaç yalnızca model eğitebilen değil, **hangi varsayım altında neyi öğrendiğini, neyi öğrenmediğini ve sonucu hangi kanıtla savunduğunu** açıklayabilen araştırmacı ve uygulayıcı yetiştirmektir.

## Dönemin ana kurgusu: `1 + 3 + 10`

| Aşama | Haftalar | Amaç | Ürün sonucu |
| --- | ---: | --- | --- |
| **Panorama** | 1 | On çapanın tamamını tek bir küçük uçtan uca deneyde görmek | Referans deney, yaşam döngüsü haritası ve başlangıç tanılaması |
| **Rehberli inşa** | 2–4 | On çapayı üç gruba bölerek ilk kez sistematik biçimde kurmak | Problem/data card → klasik ML dilimi → yeniden üretilebilir `v0.1` |
| **Bilinçli derinleşme** | 5–14 | Her hafta bir çapayı ayrıntılı işlemek ve aynı çalışmaya kanıtlı artım eklemek | On araştırma artımı ve savunulabilir `v1.0` |

İkinci turda bütün içerik her hafta tekrar edilmez:

- **1. hafta:** Ç1–Ç10 panoraması; sızıntılı ve sızıntısız iki deney üzerinden bütün rota.
- **2. hafta:** Ç1–Ç3 — problem, matematiksel temel, veri ve doğrulama tasarımı.
- **3. hafta:** Ç4–Ç6 — klasik modeller, değerlendirme, belirsizlik ve hata analizi.
- **4. hafta:** Ç7–Ç10 — sinir ağları, modern öğrenme, ardışık kararlar, sorumlu ve üretimsel ML; `v0.1` entegrasyonu.
- **5–14. haftalar:** Her hafta Ç1–Ç10'dan biri ayrıntılı olarak derinleştirilir.

## Dönem ürünü: ML Evidence Lab

Öğrenciler birbirinden kopuk on dört notebook teslim etmez. Her öğrenci veya küçük ekip, açıkça tanımlanmış tek bir karar problemi üzerinde dönem boyunca büyüyen bir **Yeniden Üretilebilir Makine Öğrenmesi Çalışması** geliştirir.

Ürün en az şu parçaları içerir:

```text
README ve problem card
data card + veri doğrulama raporu
src/ içinde yeniden kullanılabilir eğitim/değerlendirme kodu
configs/ altında kayıtlı deney ayarları
baseline + en az iki gerekçeli model ailesi
ayrılmış test protokolü ve belirsizlik tahmini
hata dilimleri, kalibrasyon ve sağlamlık analizi
model card + risk kaydı
tek komutla yeniden çalıştırma yolu
sunum/çıkarım prototipi ve izleme taslağı
```

Notebook keşif için kullanılabilir; nihai deney zincirinin tek kaynağı olamaz. Ham veri, dönüştürülmüş veri, model çıktısı ve raporlanan sonuç arasındaki iz sürülebilir olmalıdır.

Dersin ilk haftasındaki ortak vaka, sentetik bir **öngörücü bakım** problemidir: sensör kayıtlarından bir makinenin sonraki 24 saat içinde arızalanma riski tahmin edilir. Veri setinde bilerek eklenmiş olay-sonrası bir sütun, rastgele bölme ve veri sızıntısının nasıl yanıltıcı başarı üretebildiğini görünür kılar. Bu vaka dönem projesini zorunlu olarak belirlemez; ortak kavram dili kurar.

## On sabit çapa

Çapa kodları `Ç1–Ç10` dönem boyunca değişmez. Hazırlık, sınıf etkinliği, deney paketi, kısa yoklama ve sözlü savunma aynı kodları kullanır.

| Çapa | Başlık | Ana soru | Derinleşme haftası |
| --- | --- | --- | ---: |
| **Ç1** | Problem çerçeveleme, veri üretim süreci ve baseline | Tahmin hangi kararı destekler; hedef, birim, kapsam ve başarısızlık bedeli nedir? | 5 |
| **Ç2** | Matematiksel/istatistiksel temel ve genelleme | Model örnekten popülasyona hangi varsayımlarla geneller? | 6 |
| **Ç3** | Veri, önişleme, özellikler ve doğrulama tasarımı | Sızıntısız ve gerçek kullanım koşullarını temsil eden bir deney nasıl kurulur? | 7 |
| **Ç4** | Doğrusal ve olasılıksal modeller | Basit, düzenlileştirilmiş ve kalibre edilebilir modeller ne zaman yeterlidir? | 8 |
| **Ç5** | Ağaçlar, çekirdek yöntemleri ve ensemble'lar | Doğrusal olmayan yapı hangi model ailesi ve hangi bedelle öğrenilir? | 9 |
| **Ç6** | Değerlendirme, belirsizlik, açıklama ve hata analizi | Sonucun güvenilirliği nasıl ölçülür, sınırlandırılır ve yanlışlanmaya açılır? | 10 |
| **Ç7** | Sinir ağları ve optimizasyon | Temsil ve karar fonksiyonu birlikte nasıl öğrenilir ve nasıl hata ayıklanır? | 11 |
| **Ç8** | Modern temsiller, denetimsiz ve üretici öğrenme | Etiketin sınırlı olduğu veya yüksek boyutlu veride yararlı temsil nasıl öğrenilir? | 12 |
| **Ç9** | Ardışık kararlar, pekiştirmeli öğrenme ve nedensellik sınırı | Tahmin eylemi ve gelecekteki veriyi etkilediğinde hangi varsayımlar değişir? | 13 |
| **Ç10** | Sorumlu, sağlam, yeniden üretilebilir ve üretimde ML | Bir modelin yaşam döngüsü nasıl belgelenir, korunur, izlenir ve gerektiğinde durdurulur? | 14 |

### Ç1 — Problem çerçeveleme, veri üretim süreci ve baseline

Tahmin ile karar ayrımı; gözlem birimi, hedef ve tahmin ufku; veri üretim süreci; örnekleme ve seçim yanlılığı; yanlış pozitif/negatif maliyeti; basit kural ve dummy baseline; teknik metrik ile iş/araştırma sonucu ayrımı ele alınır. Öğrenci model eğitiminden önce “bu tahmin kim için, hangi anda ve hangi eylem için?” sorusunu yanıtlar.

### Ç2 — Matematiksel/istatistiksel temel ve genelleme

Vektör/matris işlemleri, normlar, özdeğer/özvektör ve SVD; olasılık, koşullu bağımsızlık, beklenti, varyans ve kovaryans; olabilirlik, MLE/MAP ve Bayesçi bakış; entropy, cross-entropy, KL divergence ve mutual information; türev, chain rule, gradyan ve optimizasyon; ampirik risk, aşırı/eksik öğrenme, bias–variance, regularization ve kapasite ele alınır. İspat ve hesap, model davranışını açıklamak için kullanılır; matematik ilgili algoritmadan kopuk öğretilmez.

### Ç3 — Veri, önişleme, özellikler ve doğrulama tasarımı

Eksik değer, kategorik değişken, ölçekleme, aykırı değer ve özellik üretimi; sınıf dengesizliği; train/validation/test ayrımı; cross-validation; group/time-aware split; preprocessing pipeline; target/proxy/temporal leakage; veri sürümleme ve doğrulama ele alınır. Bütün öğrenilen dönüşümler yalnız eğitim katında fit edilir.

### Ç4 — Doğrusal ve olasılıksal modeller

Doğrusal/polynomial regresyon, Ridge, Lasso ve Elastic Net; lojistik regresyon; k-NN; Gaussian/Multinomial/Bernoulli Naive Bayes; LDA/QDA; kayıp ve olabilirlik ilişkisi; generative/discriminative ayrımı; olasılıksal çıktı, karar eşiği ve kalibrasyon ele alınır. Katsayı veya komşuluğun nedensel etki olmadığı özellikle sınırlandırılır.

### Ç5 — Ağaçlar, çekirdek yöntemleri ve ensemble'lar

Karar ağaçlarında entropy, information gain, Gini impurity ve budama; bagging, random forest/Extra Trees, AdaBoost ve gradient boosting; SVM/SVR, margin, hinge loss ve kernel sezgisi ele alınır. Yaygın XGBoost/LightGBM/CatBoost uygulamaları aynı gradient-boosting ailesinin mühendislik varyantları olarak konumlandırılır. Hiperparametre arama maliyeti ve nested validation ihtiyacı gösterilir; model karmaşıklığı yalnız skorla değil gecikme, bellek, kararlılık ve açıklanabilirlikle karşılaştırılır.

### Ç6 — Değerlendirme, belirsizlik, açıklama ve hata analizi

Regresyon/sınıflandırma metrikleri; confusion matrix, precision–recall, ROC ve eşik seçimi; calibration; bootstrap ve güven aralığı sezgisi; çoklu karşılaştırma; hata dilimleri ve dağılım kayması; post-hoc açıklamaların sınırları ele alınır. Test seti geliştirme sırasında tekrar tekrar danışılan bir doğrulama setine dönüştürülmez.

### Ç7 — Sinir ağları ve optimizasyon

Algılayıcı, çok katmanlı ağ, aktivasyonlar, ileri/geri yayılım, otomatik türev; SGD ve uyarlamalı optimizer'lar; initialization, normalization, regularization, early stopping; öğrenme eğrileri ve hata ayıklama ele alınır. Küçük veride derin ağın otomatik olarak üstün olmadığı deneyle gösterilir.

### Ç8 — Modern temsiller, denetimsiz ve üretici öğrenme

PCA/SVD/NMF ve boyut indirgeme; k-means, hiyerarşik kümeleme, DBSCAN ve GMM/EM; Isolation Forest, LOF ve One-Class SVM ile anomaly detection; öneri sistemleri ve association rules panoraması; embeddings; CNN, recurrent ağlar, attention ve transformer fikri; transfer learning, autoencoder ve üretici model sezgisi ele alınır. Her tema tam ölçekli ürün eğitimi olarak değil, ortak temsil/amaç fonksiyonu problemi ve kontrollü deneylerle bağlanır.

### Ç9 — Ardışık kararlar, pekiştirmeli öğrenme ve nedensellik sınırı

Durum, eylem, ödül, politika ve değer; exploration–exploitation; bandit ve Markov decision process sezgisi; offline değerlendirme riski; feedback loop; korelasyon, müdahale ve nedensel iddia sınırı ele alınır. Tahmin modelinin etkisiyle veri dağılımının değişebileceği görünür kılınır.

### Ç10 — Sorumlu, sağlam, yeniden üretilebilir ve üretimde ML

Adalet ölçülerinin bağlama bağımlılığı; gizlilik, güvenlik, adversarial/operasyonel sağlamlık; datasheet ve model card; random seed'in sınırları; ortam/veri/kod/deney sürümleme; paketleme, servis, latency, drift, monitoring, rollback ve insan gözetimi ele alınır. Üretime alma teknik bir final değil, yeni bir değerlendirme evresidir.

## Kapsam sözleşmesi

Algoritmalar problem türüne göre **regresyon, sınıflandırma, kümeleme, boyut indirgeme/temsil, anomali tespiti, zaman serisi, öneri/örüntü madenciliği, yarı/öz-denetimli ve üretici öğrenme ile pekiştirmeli öğrenme** olarak gruplandırılır. Her yöntem için problem türü, matematiksel omurga, varsayım, başarısızlık kipi, hesap maliyeti ve uygun değerlendirme protokolü belirtilir.

Ders, literatürdeki her isimlendirilmiş varyantı eşit derinlikte işlemeyi amaçlamaz. [Matematik ve algoritma kapsam matrisi](KAPSAM_MATRISI.md), yaygın ana ailelerin tamamını üç öğrenme derinliğiyle güvence altına alır:

- **T — Türet ve kur:** temel denklemi/hesabı ve çalışan uygulamayı zorunlu kılar;
- **K — Kur ve karşılaştır:** sızıntısız kontrollü deney ve seçim gerekçesi ister;
- **P — Panorama:** mekanizma, varsayım, kullanım alanı ve sınırların açıklanmasını ister.

Bir yöntemin yalnız adının veya kütüphane çağrısının gösterilmesi kapsama alınmış sayılmaz. Öğrenci yeni bir algoritmayı problem türü, temsil, amaç fonksiyonu, optimizasyon ve değerlendirme eksenlerinde konumlandırabilmelidir.

## İlk dört hafta

### 1. hafta — Panorama: yüksek skor neden yeterli değildir?

Eğitmen sentetik öngörücü bakım verisinde üç koşulu çalıştırır: çoğunluk baseline'ı, olay-sonrası sütun içeren rastgele bölünmüş sızıntılı model ve zaman/makine grubu gözeten temiz pipeline. Öğrenci skor sırasını önce tahmin eder, sonra “en yüksek skor” ile “en güvenilir deney” arasındaki farkı açıklar.

Öğrenci Ç1–Ç10'un tamamını `gözlendi / modellendi / gelecekte inşa edilecek` biçiminde haritalar; basit bir sızıntıyı giderir ve deney iddiasını kanıt–sınır diliyle yeniden yazar. Ayrıntılı akış [HAFTA01.md](HAFTA01.md) dosyasındadır.

### 2. hafta — Ç1–Ç3: araştırma sorusu ve sızıntısız veri omurgası

Öğrenci dönem problemini gözlem birimi, tahmin anı, hedef, eylem ve hata maliyetleriyle sınırlar; baseline ve başarı ölçütünü tanımlar. Veri üretim sürecini ve olası selection/measurement bias kaynaklarını çizer. Train/validation/test sözleşmesini kullanım koşuluna göre seçer ve tüm önişlemeyi pipeline içine alır.

**Ürün artımı:** problem card, data card taslağı, bölme protokolü, dummy baseline ve veri doğrulama testleri.

### 3. hafta — Ç4–Ç6: klasik ML dikey dilimi

Doğrusal/lojistik bir model ile en az bir ağaç tabanlı model aynı sabit protokolde karşılaştırılır. Ortak küçük veride Naive Bayes posterior/smoothing, k-NN uzaklık/ölçek ve decision-tree entropy/information-gain hesapları yapılır; ensemble ve SVM aileleri bu mekanizmalarla ilişkilendirilir. Hiperparametre seçimi yalnız eğitim/validation katında yapılır. Öğrenci birincil metrik, karar eşiği, kalibrasyon ve en az iki hata dilimini raporlar; tek skor yerine belirsizlik ve sınırlama sunar.

**Ürün artımı:** yeniden çalıştırılabilir eğitim komutu, model karşılaştırması, metrik dosyası, hata analizi ve ilk model card taslağı.

### 4. hafta — Ç7–Ç10 ve entegrasyon: `v0.1`

Küçük bir sinir ağı ile PCA/k-means mekanizma örneği, “daha büyük veya daha karmaşık her zaman daha iyi değildir” sorusuyla klasik baseline'a bağlanır; hiyerarşik kümeleme, DBSCAN ve GMM/EM'in farklı veri geometrileri panoraması kurulur. Ardışık karar/feedback loop, adalet, gizlilik, sağlamlık, yeniden üretilebilirlik ve izleme gereksinimleri ürün yaşam döngüsüne yerleştirilir.

`v0.1` kabul kapısı:

- temiz ortamdan tek belgeli komutla çalışır;
- veri, kod, ortam ve deney kimlikleri kaydedilir;
- baseline ve en az iki model aynı ayrılmış protokolde karşılaştırılır;
- test verisi model/hiperparametre seçiminde kullanılmaz;
- metrikler makinece okunabilir biçimde kaydedilir;
- en az bir hata yolu ve bir veri sızıntısı testi vardır;
- problem card, data card, model card ve risk kaydı taslaktır;
- bir ekip üyesi seçilen herhangi bir kararı bireysel olarak savunabilir.

## 5–14. hafta yol haritası

| Hafta | Derinleşme | Temel deney/ürün artımı | Ana kanıt |
| ---: | --- | --- | --- |
| 5 | Ç1 — Problem ve veri üretim süreci | Hedef/ufuk/eylem revizyonu, baseline ve hata maliyeti | Problem card + yanlışlanabilir başarı ölçütü |
| 6 | Ç2 — Matematik ve genelleme temeli | Linear algebra, olasılık/Bayes, likelihood, entropy/KL, gradient, kapasite ve regularization | Elle hesap + öğrenme eğrisi + varsayım açıklaması |
| 7 | Ç3 — Veri ve doğrulama | Random/group/time split ve leakage karşılaştırması | Bölme gerekçesi + sızıntı testi |
| 8 | Ç4 — Regresyon ve sınıflandırmanın doğrusal/olasılıksal ailesi | Linear/Ridge/Lasso/Elastic Net, logistic, k-NN, Naive Bayes ve LDA/QDA | Kayıp/likelihood + Bayes hesabı + calibration grafiği |
| 9 | Ç5 — Ağaç/kernel/ensemble | Entropy/Gini ile tree; RF/Extra Trees, AdaBoost/gradient boosting ve SVM/SVR | Split hesabı + performans–maliyet–kararlılık tablosu |
| 10 | Ç6 — Değerlendirme ve hata analizi | Bootstrap, eşik ve slice analizi | Belirsizlik + hata taksonomisi |
| 11 | Ç7 — Sinir ağları | MLP, optimizasyon ve hata ayıklama | Learning curves + ablation/debug günlüğü |
| 12 | Ç8 — Denetimsiz öğrenme ve modern temsiller | PCA/SVD; k-means, hierarchical, DBSCAN, GMM/EM; anomaly ve probleme uygun transfer/öneri uzantısı | En az iki kümeleme + temsil/anomali kontrollü kanıtı |
| 13 | Ç9 — Ardışık karar ve nedensellik sınırı | Bandit/feedback-loop simülasyonu veya etki haritası | Politika riski + nedensel olmayan iddia sınırı |
| 14 | Ç10 — Sorumlu ve üretimde ML | Risk denetimi, paketleme, monitoring/rollback ve `v1.0` | Yeniden üretim kaydı + model/data card + savunma |

## Değişmeyen öğrenme döngüsü

Her hafta aynı bilişsel ve teknik döngü kullanılır:

```text
Haritala → Tahmin et → Modelle → Kur → Sına → Açıkla → Sınırlandır → Yansıt
```

1. **Haritala:** Yeni ayrıntının Ç1–Ç10 yaşam döngüsündeki yerini bul.
2. **Tahmin et:** Kodu çalıştırmadan önce beklenen sonucu ve gerekçeyi yaz.
3. **Modelle:** Varsayımı, veri üretim sürecini, kaybı ve başarı ölçütünü belirt.
4. **Kur:** En küçük yeniden üretilebilir deneyi gerçekleştir.
5. **Sına:** Baseline, karşı örnek, hata yolu ve değişen tek faktörle iddiayı zorla.
6. **Açıkla:** Sonucu denklem, grafik, tablo ve sade teknik dille savun.
7. **Sınırlandır:** Kanıtın desteklemediği genellemeleri ve riskleri açıkça yaz.
8. **Yansıt:** Bir sonraki deneyde neyi değiştireceğini kaydet.

## Her haftanın çalışma düzeni

### Ders öncesi — 30–45 dakika

- kısa ana kaynak okuması veya video;
- AI/nota kapalı üç geri çağırma sorusu;
- deney sonucu için gerekçeli tahmin;
- ortam ve veri doğrulaması.

### Derste — 155 dakika

- 10–15 dakika retrieval ve vaka;
- kısa kavram/denklem blokları;
- eğitmen modellemesi ve canlı deney;
- ikili/küçük grup stüdyo çalışması;
- üç adet 10 dakikalık ara;
- bireysel exit ticket.

### Ders sonrası — 3–5 saat

- aynı dönem ürününe tek kanıtlı artım;
- deney kaydı ve otomatik kontrol;
- grafik/tablo ile teknik açıklama;
- bireysel kısa savunma veya yansıma.

## Her artımda zorunlu dört kanıt

1. **Çalışan ürün:** Tekrarlanabilir komut, kod, yapılandırma ve çıktı.
2. **Bilimsel doğruluk:** Baseline, sabit protokol, ayrılmış değerlendirme ve uygun karşılaştırma.
3. **Tasarım kararı:** Tercih, reddedilen alternatif, varsayım ve bedel.
4. **Bireysel açıklama:** Öğrencinin sonucu ve sınırlamasını kendi sözleriyle savunması.

Model dosyası veya ekran görüntüsü tek başına kanıt değildir. Veri sürümü, split kimliği, ortam, seed, deney ayarı ve metrik kaydı birlikte tutulur. Seed kullanımı yararlıdır fakat bütün donanım ve yazılım ortamlarında bit düzeyinde yeniden üretilebilirlik garantisi sayılmaz.

## Değerlendirme sistemi

| Bileşen | Ağırlık | Ne ölçer? |
| --- | ---: | --- |
| Hazırlık ve kısa bireysel yoklamalar | %15 | Geri çağırma, temel matematik ve kavram ayrımları |
| Haftalık deney/ürün artımları | %40 | Çalışan ürün, bilimsel kanıt, tasarım kararı ve açıklama |
| Ara dönem deney denetimi | %15 | W1–W7 zincirinin yeniden üretimi, hata bulma ve savunma |
| Final `v1.0` araştırma paketi | %20 | Uçtan uca doğruluk, risk analizi, belge ve yeniden üretilebilirlik |
| Bireysel final teknik savunması | %10 | Yazılan kodu, sonucu, sınırlamayı ve alternatifleri açıklama |

Kısa yoklamalar ve bireysel savunmalar kişiseldir. Grup ürünü teslim edilse bile her öğrenci seçilen bir dosyayı, deneyi veya kararı açıklayabilmelidir. Ara ve final değerlendirmeleri üniversitenin ilan ettiği takvim ve ilgili lisansüstü yönetmelik çerçevesinde uygulanır.

## Kapsam yönetimi

Her teknik başlık üç seviyeden biriyle etiketlenir:

- **Core:** Her öğrencinin kurup açıklaması gereken çekirdek.
- **Stretch:** Çekirdeği tamamlayan öğrencinin kontrollü uzantısı.
- **Instructor demo:** Yüksek hesaplama, özel donanım veya veri erişimi gerektiren panorama gösterimi.

GPU zorunlu değildir. Büyük dil modeli, büyük görsel model, dağıtık eğitim veya ücretli bulut servisi Core kapsamına alınmaz. Bu başlıklar küçük önceden eğitilmiş model, sentetik veri, indirgenmiş örnek ya da eğitmen demosuyla kavramsal ve deneysel olarak incelenebilir.

Kapsam genişliği, “her öğrenci her algoritmayı projesine ekler” anlamına gelmez. Bütün öğrenciler matristeki T düzeyi mekanizmaları öğrenir; K düzeyi yöntemleri ortak kontrollü laboratuvarlarda karşılaştırır; dönem ürününde ise problemine uygun, gerekçeli az sayıda aileyi kullanır. Bu ayrım model alışverişini ve validation set üzerinde kontrolsüz aramayı önler.

## Araç ve ortam ilkeleri

- Dönem başında desteklenen Python ve paket sürümleri kilit dosyasında sabitlenir.
- Temiz kurulum ve CPU üzerinde makul sürede çalışan Core yol sağlanır.
- Rastgelelik kaynakları açıkça yönetilir; deterministik olmayan işlemler belgelenir.
- Gerçek kişisel, kurumsal veya hassas veri izinsiz kullanılmaz.
- Büyük dosyalar Git geçmişine kontrolsüz eklenmez; veri edinme yolu ve bütünlük özeti belgelenir.
- Gizli anahtar, erişim belirteci ve kişisel yol bilgisi teslim edilmez.

## Üretken yapay zekâ kullanımı

Üretken yapay zekâ açık ürün çalışmalarında yardımcı olabilir; kısa yoklama ve bireysel savunmada kullanılamaz.

AI kullanılan her teslimde öğrenci şunları açıklar:

- kullanılan araç ve amaç;
- kabul edilen ve reddedilen önemli öneriler;
- üretilen kod/iddianın nasıl doğrulandığı;
- öğrencinin değiştirdiği karar ve gerekçesi.

AI çıktısı kaynak veya deney kanıtı yerine geçmez. Öğrenci teslim ettiği bütün kodu, denklemi, grafiği ve yorumu savunmakla yükümlüdür. Açıklanamayan veya yeniden üretilemeyen bölüm tamamlanmış kabul edilmez.

## Geri kazanım tabanı

4. haftada çalışan `v0.1`, sonraki haftalar için ortak geri kazanım noktasıdır. Geride kalan öğrenci:

1. doğrulanmış starter/veri örneğinden başlar;
2. yalnız Core kabul testlerini tamamlar;
3. Ç1–Ç10 haritasında eksik kanıtı işaretler;
4. kısa bireysel kontrolle yeniden ana akışa katılır.

Stretch işleri Core eksiklerini telafi etmez. Bir artım geciktiğinde bütün projeyi baştan yazmak yerine son doğrulanmış sürüme dönülür.

## 7,5 AKTS için planlanan iş yükü

| Etkinlik | Saat |
| --- | ---: |
| Ders | 42 |
| Ders öncesi hazırlık | 28 |
| Haftalık deney paketleri | 48 |
| Dönem projesi/veri çalışması | 45 |
| Okuma ve küçük replikasyonlar | 15 |
| Problem/data/model card ve teknik belge | 15 |
| Ara/final hazırlığı | 12 |
| Değerlendirme ve savunma | 3,5 |
| **Toplam** | **208,5** |

Bu dağılım planlama tahminidir; resmi ders bilgi paketine girilirken programın güncel AKTS ve ölçme kurallarıyla yeniden doğrulanır.

## Temel ve güncel kaynaklar

- Ethem Alpaydın, [*Introduction to Machine Learning*, 4. baskı](https://mitpress.mit.edu/9780262043793/introduction-to-machine-learning/).
- Kevin P. Murphy, [*Probabilistic Machine Learning: An Introduction*](https://probml.github.io/pml-book/book1.html).
- Simon J. D. Prince, [*Understanding Deep Learning*](https://udlbook.github.io/udlbook/).
- [scikit-learn kullanıcı kılavuzu](https://scikit-learn.org/stable/user_guide.html); özellikle model seçimi, değerlendirme, pipeline ve [common pitfalls](https://scikit-learn.org/stable/common_pitfalls.html) bölümleri.
- PyTorch belgeleri; autograd, model geliştirme ve [reproducibility notları](https://docs.pytorch.org/docs/stable/notes/randomness.html).
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework).
- [*Datasheets for Datasets*](https://arxiv.org/abs/1803.09010) ve [*Model Cards for Model Reporting*](https://arxiv.org/abs/1810.03993).

Kaynaklar konu sırasını belirleyen tek bir ders kitabı gibi izlenmez. Her hafta bir ana okuma, bir uygulama belgesi ve gerektiğinde birincil araştırma makalesi seçilir; sürüm ve erişim bağlantıları haftalık dosyada sabitlenir.

## Başarı tanımı

Dersi başarıyla tamamlayan öğrenci:

- bir gerçek dünya kararını yanlışlanabilir ML problemine dönüştürür;
- veri üretim sürecini, örnekleme yanlılığını ve sızıntı riskini analiz eder;
- uygun baseline, split, pipeline, model ailesi ve metriği seçer;
- regresyon, sınıflandırma, kümeleme, boyut indirgeme, anomali tespiti ve ardışık karar problemlerini ayırır;
- doğrusal/olasılıksal, komşuluk, ağaç/ensemble, kernel, kümeleme ve sinir ağı yöntemlerinin matematiksel varsayımlarını karşılaştırır;
- entropy, cross-entropy, KL divergence, Bayes, likelihood, information gain, Gini, margin, gradient ve regularization kavramlarını doğru algoritmaya bağlar;
- model seçimini test verisini kirletmeden gerçekleştirir;
- performansı belirsizlik, kalibrasyon ve hata dilimleriyle raporlar;
- açıklama ile nedensel iddia arasındaki sınırı korur;
- ardışık karar ve feedback loop riskini fark eder;
- adalet, gizlilik, güvenlik, sağlamlık ve insan gözetimini tasarıma ekler;
- veri, kod, ortam ve deney kaydını yeniden üretilebilir bir pakette birleştirir;
- modelin ne zaman kullanılmaması veya durdurulması gerektiğini savunur.

## Belge durumu

Bu dosya 2026–2027 Güz dönemi için yeni ders mimarisidir. Ayrıntılı W1–W4 dosyaları ve matematik/algoritma kapsam matrisi hazırdır; `HAFTA05.md`–`HAFTA14.md`, starter repository, sentetik veri ve kabul testleri bu omurgaya göre ayrıca hazırlanacaktır. Arşivdeki eski izlence bu geliştirme sürecinde içerik referansı olarak kullanılmayacaktır.
