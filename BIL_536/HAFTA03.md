# BİL 536 01 Makine Öğrenmesi — 3. Hafta Öğretim Dosyası

**Hafta:** Modelden güvenilir karşılaştırmaya — Ç4, Ç5 ve Ç6  
**Dönem yapısı:** 14 hafta, `1 + 3 + 10`  
**Süre:** 155 dakika ders; 125 dakika etkin çalışma + üç adet 10 dakikalık ara  
**Dönem ürünü:** Yeniden Üretilebilir Makine Öğrenmesi Çalışması (`ML Evidence Lab`)  
**İkinci turdaki yeri:** Orta bölüm — klasik modeller, değerlendirme, belirsizlik ve hata analizi  
**İlgili belgeler:** [Ders modeli](README.md) · [1. hafta](HAFTA01.md) · [2. hafta](HAFTA02.md) · [4. hafta](HAFTA04.md)

W2'de belirlenen problem, veri ve split sözleşmesi bu hafta dondurulur. Amaç mümkün olan en fazla modeli denemek değil; farklı varsayımlara sahip az sayıda modeli aynı protokolde karşılaştırmak ve sonucun karar bağlamındaki anlamını açıklamaktır. W4 checkpoint ve W14 final blind holdout W3 boyunca kapalı kalır.

## Haftanın ana sorusu

> İki modelden hangisinin “daha iyi” olduğuna, test verisini kirletmeden ve tek bir skora aldanmadan nasıl karar veririz?

## Bu hafta ayrıntılı işlenecek çapalar

| Çapa | Bu haftanın odağı | Haftanın ürün karşılığı |
| --- | --- | --- |
| **Ç4** | Doğrusal ve olasılıksal modeller | Düzenlileştirilmiş logistic/linear model, olasılık ve calibration başlangıcı |
| **Ç5** | Ağaçlar, çekirdek yöntemleri ve ensemble'lar | Sınırlı doğrusal olmayan alternatif ve sabit protokolde karşılaştırma |
| **Ç6** | Değerlendirme, belirsizlik, açıklama ve hata analizi | Birincil metrik, eşik, CV özeti, hata dilimleri ve model seçim kaydı |

Ç7–Ç10'un mekanizmaları bu hafta ayrıntılı işlenmez. Sinir ağı, modern temsil, feedback loop ve üretim yaşam döngüsü W4'te ilk sistematik turunu tamamlar.

## Ders sonunda öğrencinin göstereceği kanıt

Öğrenci:

- doğrusal regresyonda tahmin, residual ve squared loss ilişkisini açıklar;
- logistic fonksiyon, log-odds, olasılık skoru ve karar eşiğini ayırır;
- likelihood ile loss arasındaki bağlantıyı sezgisel olarak kurar;
- L1 ve L2 regularization'ın kapasite, katsayılar ve genelleme üzerindeki etkisini yorumlar;
- katsayı veya feature importance değerini nedensel etki diye sunmaz;
- karar ağacında split, impurity, derinlik ve overfitting ilişkisini gösterir;
- bagging/random forest ile boosting'in temel hata azaltma fikirlerini ayırır;
- SVM'de margin ve kernel fikrini çalışma düzeyinde konumlandırır;
- model ailelerini aynı feature, split ve değerlendirme protokolünde karşılaştırır;
- hiperparametre seçimini development verisi içinde tutar ve iki holdout sonucuna bakmaz;
- sınıf dengesine ve karar maliyetine uygun birincil metrik seçer;
- threshold değişiminin precision, recall ve beklenen maliyete etkisini gösterir;
- en az iki hata dilimi üretir ve küçük örnekli dilimlerde kesin hüküm vermez;
- model seçimini skor, belirsizlik, maliyet, kararlılık ve açıklama sınırıyla kaydeder.

## Eğitmenin ders öncesi hazırlığı

### Sabit deney protokolü

- W2'de onaylanan veri ve development split kimliğini dondur.
- W4 checkpoint ve W14 final blind holdout dosyalarına öğrenci runner'ının W3 yapılandırmasından erişemediğini doğrula.
- Ortak veri için şu Core modelleri hazırla:
  1. W2 dummy/kural baseline,
  2. scaling/encoding içeren regularized logistic regression,
  3. sınırlı derinlikte decision tree veya random forest.
- SVM ve gradient boosting'i küçük instructor-demo yapılandırması olarak hazırla; her öğrenciden bütün aileleri tune etmesini bekleme.
- Bütün modellerin aynı fold/split kimliğini kullandığını otomatik kontrol eden test ekle.

### Görsel ve karşı örnekler

- 2B sentetik veride doğrusal decision boundary ve ağaç bölmeleri.
- Aşırı derin ağacın train/development farkı.
- Aynı ROC-AUC fakat farklı calibration/eşik davranışı üreten iki model.
- Aynı accuracy fakat farklı pozitif sınıf recall'ına sahip iki confusion matrix.
- Küçük bir dilimde yüksek hata oranı fakat geniş belirsizlik örneği.
- Feature importance'ın korelasyonlu özellikler arasında kararsızlaşabildiği karşı örnek.

### Starter ve şablonlar

- `src/train.py`, `src/evaluate.py` ve `src/models.py` iskeleti.
- `configs/logistic.yaml` ve `configs/tree.yaml`.
- `artifacts/model_comparison.csv` şeması.
- `model_selection.md`: seçim, reddedilen alternatif, kanıt ve sınır.
- `tests/test_experiment_contract.py`: aynı split, iki holdout kilidi, pipeline ve metrik şeması kontrolleri.
- Öğrencilerin hiperparametre aramasını süre ve deneme sayısıyla sınırlayan bütçe.

## Ders öncesi öğrenci hazırlığı — 35–45 dakika

1. W2 problem card, split contract ve baseline geri bildirimlerini kapat.
2. Şu kavramları notasız birer cümleyle açıkla: linear score, probability, threshold, regularization, tree depth, cross-validation, calibration.
3. Baseline çıktından confusion matrix'i elle yeniden hesapla.
4. Şu iki tahmini yaz:
   - Regularization çok güçlenirse train ve validation performansı hangi yönde değişebilir?
   - Karar eşiği düşerse pozitif sınıf recall ve false positive sayısı nasıl değişebilir?
5. Kendi problemin için birincil metriği ve karar maliyetiyle bağlantısını en fazla 80 kelimeyle öner.
6. W3 starter'ında `check-contract` komutunu çalıştır; checkpoint veya final blind holdout'a erişmeye çalışma.

## Tahta/slayt omurgası

```text
Sabit problem + sabit veri + sabit split
                   ↓
Baseline
                   ↓
Doğrusal/olasılıksal model  ↔  ağaç/ensemble/kernel
                   ↓
Development içi model ve hiperparametre seçimi
                   ↓
Olasılık → calibration → karar eşiği
                   ↓
Birincil metrik + ikincil metrikler + maliyet
                   ↓
Belirsizlik + hata dilimleri + açıklama sınırı
                   ↓
Belgelenmiş model seçim kararı
```

## 155 dakikalık ders akışı

| Süre | İçerik ve öğretmen hamlesi | Öğrenci işi / toplanan kanıt |
| --- | --- | --- |
| 00–08 | W2 retrieval: problem, split ve baseline sözleşmesini kısa vaka üzerinden yokla. | Bireysel sözleşme kontrolü |
| 08–18 | **Ç4:** Doğrusal tahmin, residual, squared loss ve regularization. | Üç nokta için tahmin/residual hesabı |
| 18–26 | Logistic score, sigmoid, log-odds, likelihood/loss sezgisi. | Skor–olasılık–etiket ayrımı |
| 26–30 | L1/L2 ve katsayı yorumunun sınırı. | Regularization tahmini |
| 30–40 | **Ara** | |
| 40–50 | Logistic pipeline'ı çalıştır; coefficient, validation metriği ve calibration başlangıcı. | Çalıştırma kaydı + bir sınırlı yorum |
| 50–60 | **Ç5:** Decision tree; split, impurity, depth ve aşırı öğrenme. | Train/development eğrisi yorumu |
| 60–66 | Bagging/random forest ve boosting farkı; variance/bias bağlantısı. | İki mekanizma karşılaştırması |
| 66–70 | SVM margin ve kernel panoraması; hangi ölçekte neden düşünülebileceği. | Bir kullanım koşulu + maliyet |
| 70–80 | **Ara** | |
| 80–90 | **Ç6:** Confusion matrix; accuracy, precision, recall, specificity ve F1. | Verilen matristen metrik hesabı |
| 90–98 | ROC/PR, sınıf oranı ve karar bağlamı; birincil metriğin önceden seçimi. | Problem için metrik gerekçesi |
| 98–104 | Calibration ve threshold; tahmin olasılığı ile eylem kararını ayır. | İki eşikte beklenen sonuç |
| 104–110 | Cross-validation, hiperparametre seçimi ve test kilidi; nested yaklaşımın yeri. | Doğru/yanlış değerlendirme akışı |
| 110–120 | **Ara** | |
| 120–130 | Belirsizlik, fold değişkenliği ve bootstrap sezgisi; tek sayı sınırı. | Ortalama + dağılım/sınır yorumu |
| 130–143 | Stüdyo: logistic ve ağaç tabanlı modeli sabit protokolde çalıştır, threshold ve iki hata dilimi üret. | Karşılaştırma tablosu + hata analizi |
| 143–150 | Kırmızı takım: seçilen modele karşı en güçlü alternatif açıklamayı veya protokol riskini sor. | Seçim kaydında revizyon |
| 150–155 | Bireysel exit ticket; checkpoint ve final blind holdout'un hâlâ kapalı olduğunu doğrula. | Çıkış kaydı |

## Üç çapanın bu hafta için doğru derinliği

| Çapa | Bu hafta ayrıntılı işle | Sonraki derinleşmeye bırak |
| --- | --- | --- |
| Ç4 | Linear/logistic model, loss/likelihood sezgisi, L1/L2, probability ve threshold | Ayrıntılı türetimler, Bayesian modelleme ve ileri kalibrasyon yöntemleri |
| Ç5 | Tree split/depth, bagging/random forest, boosting ve SVM/kernel ana fikri | Her algoritmanın tam implementasyonu ve geniş hiperparametre araması |
| Ç6 | Metrik seçimi, CV protokolü, calibration, threshold, belirsizlik başlangıcı ve hata dilimleri | Nested CV ayrıntıları, ileri uncertainty, açıklama yöntemleri ve çoklu karşılaştırma düzeltmeleri |

## Stüdyo görevi: aynı protokolde üç seviye

Öğrenci W2 baseline'ını korur ve iki model ailesini ekler:

1. **Seviye 0 — baseline:** Dummy veya onaylı basit kural.
2. **Seviye 1 — basit model:** Regularized logistic regression; regresyon probleminde regularized linear model.
3. **Seviye 2 — doğrusal olmayan model:** Sınırlı karar ağacı veya random forest.

### Deney sözleşmesi

- Üç seviye aynı development split/fold kimliğini kullanır.
- Feature listesi ve preprocessing sözleşmesi model bazında sessizce değişmez.
- Hiperparametre bütçesi önceden kaydedilir; Core için en fazla 12 aday yapılandırma.
- Birincil metrik model çalıştırılmadan önce `evaluation_plan.md` içine yazılır.
- W4 checkpoint ve W14 final blind holdout hiçbir arama, seçim, grafik veya hata analizi için kullanılmaz.
- Random seed tek başarı koşusunu seçmek için kullanılmaz; kullanılan seed'ler kaydedilir.

### Üretilecek karşılaştırma

| Alan | Zorunlu içerik |
| --- | --- |
| Model ve yapılandırma kimliği | Tam yeniden çalıştırılabilir ad |
| Baseline'a göre fark | Aynı birincil metrikte mutlak fark |
| Fold/tekrar özeti | Ortalama ve değişkenliği gösteren değerler |
| Eğitim ve tahmin maliyeti | Aynı ortamda kaba süre/boyut; geniş genelleme yapılmaz |
| Calibration/eşik | En az bir calibration özeti ve iki eşik |
| Hata dilimleri | En az iki anlamlı, önceden gerekçeli dilim |
| Sınırlama | Modelin desteklemediği en az bir iddia |

### Model seçimi

Öğrenci en yüksek skoru otomatik olarak seçmez. `model_selection.md` şu biçimdedir:

```text
Seçilen aday:
Birincil kanıt:
Baseline'a göre artım:
Belirsizlik/kararlılık:
Eşik ve karar maliyeti:
Hata dilimleri:
Hesaplama/yorumlama bedeli:
Reddedilen en güçlü alternatif ve nedeni:
Checkpoint holdout açılmadan önce kalan risk:
```

## Core kabul koşulları

- W2 problem, veri ve split sözleşmesi değiştirilmemiş veya değişiklik açıkça sürümlenmiştir.
- Baseline, doğrusal/olasılıksal model ve ağaç tabanlı model aynı protokolde çalışır.
- Preprocessing her fold'un yalnız training bölümünde fit edilir.
- Hiperparametre seçimi development verisi içinde kalır.
- W4 checkpoint ve W14 final blind holdout'a erişilmez.
- Birincil metrik karar bağlamıyla gerekçelendirilmiştir.
- En az iki eşik için confusion matrix veya eşdeğer maliyet sonucu vardır.
- En az iki hata dilimi ve örnek sayıları raporlanır.
- Model karşılaştırma tablosu ve seçim kaydı yeniden üretilebilir.
- Katsayı/importance açıklaması nedensel iddia olarak sunulmaz.
- Öğrenci seçilen ve reddedilen modeli bireysel olarak savunabilir.

## Stretch

- SVM veya gradient boosting'i aynı bütçe/protokolde ekle; Core modelleri kaldırma.
- Calibration curve ve Brier score ekle; calibration yöntemini yalnız development içinde fit et.
- Development verisi içinde nested CV örneği çalıştır ve sıradan CV ile rol farkını açıkla.
- Bootstrap ile metrik değişkenliği üret; bağımsız gözlem varsayımını ihlal eden grup/zaman yapısını tartış.

## Kritik sorular ve beklenen yön

- **Logistic regression neden “regression” adını taşır?** Sınıf için doğrusal skor/log-odds modelleyip olasılığa dönüştürür; sürekli sınıf etiketi üretmez.
- **Katsayı büyükse feature önemli midir?** Ölçek, korelasyon, regularization ve model varsayımı etkiler; önem ve nedensellik ayrı iddialardır.
- **Ağaç scaling istemiyorsa pipeline gereksiz midir?** Hayır; imputation, encoding, feature sözleşmesi ve sızıntı kontrolü yine pipeline gerektirir.
- **Random forest her zaman tek ağaçtan iyi midir?** Genellikle variance azaltabilir; fakat maliyet, veri, ayar ve karar bağlamına bağlıdır.
- **ROC-AUC yüksekse uygun threshold hazır mıdır?** Hayır; ranking metriği karar eşiğini ve yanlış karar maliyetini belirlemez.
- **Cross-validation sonucu gerçek üretim performansı mıdır?** Yalnız split ve veri üretim varsayımları geleceği temsil ettiği ölçüde bir tahmindir.
- **Holdout'a yalnız bir kez bakmak sihirli midir?** Süreç önceden belirlenmişse uyarlama riskini azaltır; holdout veri kaynağı kullanımı temsil etmiyorsa yine geçersiz olabilir.
- **Bir dilimde düşük skor ayrımcılığı kanıtlar mı?** Hata farkı araştırılması gereken kanıttır; örnek sayısı, etiket kalitesi, bağlam ve belirsizlik incelenmelidir.

## Yaygın yanılgılar ve müdahale

- “Bütün modelleri deneyip en iyisini seçmek bilimseldir.” → Deneme bütçesi, multiple comparison ve validation overfitting riskini görünür kıl.
- “Accuracy yükseldiyse threshold iyidir.” → Aynı modelin iki eşikte FP/FN maliyetini hesaplat.
- “Feature importance modelin gerçeği keşfettiğini gösterir.” → Korelasyonlu feature ve permutation karşı örneği kullan.
- “CV kullanınca leakage olmaz.” → Pipeline dışındaki preprocessing'in her fold'a bilgi taşıdığını göster.
- “Düşük variance her zaman daha iyidir.” → Sürekli yanlış fakat kararlı baseline karşı örneğini ver.
- “Calibration yalnız sağlık/finans için gerekir.” → Bakım kapasitesi planlamasında risk skorunun kaynak tahsisini nasıl etkilediğini göster.
- “En yorumlanabilir model otomatik olarak en güvenlidir.” → Veri/karar/protokol riskinin model sadeliğinden ayrı olduğunu hatırlat.

## Hafta sonu teslim paketi

```text
week03/
├── evaluation_plan.md
├── configs/logistic.yaml
├── configs/tree.yaml
├── src/models.py
├── src/train.py
├── src/evaluate.py
├── tests/test_experiment_contract.py
├── artifacts/model_comparison.csv
├── artifacts/threshold_report.csv
├── artifacts/calibration_summary.json
├── artifacts/error_slices.csv
├── model_selection.md
├── decision_log.md
└── AI_ASSISTANCE.md
```

Teknik açıklama en fazla 500 kelimedir ve şu soruları yanıtlar:

1. Hangi model hangi varsayım ve mekanizma nedeniyle farklı davrandı?
2. Birincil metriğin karar problemiyle ilişkisi nedir?
3. Eşik değişimi kimin açısından hangi bedeli değiştirdi?
4. Hangi hata dilimi en önemli, fakat kanıt neden hâlâ sınırlı?
5. Holdout'lar açılmadan önce neden seçilen modelin gerçek performansını bildiğimizi söyleyemeyiz?

## Exit ticket

1. Probability score ile karar eşiği arasındaki fark nedir?
2. Bu hafta model seçerken iki holdout'u neden kullanmadın?
3. Aynı accuracy'ye sahip iki model hangi nedenle farklı karar kalitesi üretebilir?
4. Seçtiğin modelin baseline'a göre sağladığı en güçlü kanıt nedir?
5. Ç7–Ç10'dan hangisi model seçim kararını W4'te en çok değiştirebilir?

## 4. haftaya köprü

> **Klasik ML dikey dilimi artık sabit protokolde çalışıyor. 4. haftada küçük bir sinir ağı ve modern temsil örneğiyle model uzayını genişletecek; tahminin eylemi değiştirdiği durumda feedback loop'u inceleyecek ve bütün zinciri risk, yeniden üretim, test ve izleme sözleşmesiyle `v0.1` hâline getireceğiz.**

W4 öncesinde ekipler model seçim kaydını dondurur. Checkpoint holdout yalnız W4 kabul kapısında, komut ve sonuç otomatik kayda alınarak bir kez açılacaktır. Final blind holdout W14'e kadar kapalı kalır.
