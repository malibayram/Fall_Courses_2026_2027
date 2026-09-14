# BİL 536 01 Makine Öğrenmesi — 4. Hafta Öğretim Dosyası

**Hafta:** Öğrenilmiş temsilden yaşam döngüsüne — Ç7, Ç8, Ç9 ve Ç10  
**Dönem yapısı:** 14 hafta, `1 + 3 + 10`  
**Süre:** 155 dakika ders; 125 dakika etkin çalışma + üç adet 10 dakikalık ara  
**Dönem ürünü:** Yeniden Üretilebilir Makine Öğrenmesi Çalışması (`ML Evidence Lab`)  
**İkinci turdaki yeri:** Son bölüm — modern öğrenme, ardışık karar, sorumlu/üretimde ML ve `v0.1` entegrasyonu  
**İlgili belgeler:** [Ders modeli](README.md) · [Matematik ve algoritma kapsam matrisi](KAPSAM_MATRISI.md) · [1. hafta](HAFTA01.md) · [2. hafta](HAFTA02.md) · [3. hafta](HAFTA03.md)

Bu hafta ikinci tur tamamlanır. Ç7–Ç10'un amacı dört geniş alanı bir derste bitirmek değildir; her alanın temel mekanizmasını çalışan küçük bir örnekle kurmak ve dönem ürünündeki karar noktasına bağlamaktır. Öğrenci her modern yöntemi projesine eklemek zorunda değildir. `v0.1`, en karmaşık model değil, en iyi belgelenmiş ve yeniden üretilebilir ilk uçtan uca araştırma sürümüdür.

## Haftanın ana sorusu

> Bir model geliştirme deneyini, öğrenilmiş temsil ve eylem etkilerini de hesaba katan; güvenli, yeniden üretilebilir ve gerektiğinde durdurulabilir bir ML sistemi başlangıcına nasıl dönüştürürüz?

## Bu hafta ayrıntılı işlenecek çapalar

| Çapa | Bu haftanın odağı | Haftanın ürün karşılığı |
| --- | --- | --- |
| **Ç7** | Sinir ağları ve optimizasyon | Aynı protokolde küçük MLP, learning curve ve hata ayıklama kaydı |
| **Ç8** | Modern temsiller, denetimsiz ve üretici öğrenme | Küçük temsil deneyi ve probleme uygunluk kararı |
| **Ç9** | Ardışık kararlar, pekiştirmeli öğrenme ve nedensellik sınırı | Feedback-loop/eylem haritası ve küçük bandit simülasyonu |
| **Ç10** | Sorumlu, sağlam, yeniden üretilebilir ve üretimde ML | Risk kaydı, model/data card, reproducibility manifest, monitoring/rollback taslağı |

## Ders sonunda öğrencinin göstereceği kanıt

Öğrenci:

- katman, ağırlık, bias, activation, loss ve output zincirini küçük bir MLP üzerinde izler;
- forward pass, gradient, backpropagation ve optimizer rollerini ayırır;
- learning rate, initialization, batch ve regularization'ın eğitim davranışını etkilediğini gösterir;
- train/development learning curve üzerinden underfit, overfit veya optimizasyon sorunu için ilk tanı koyar;
- küçük tablo verisinde MLP'nin klasik baseline'a otomatik üstün olmadığını kabul eder;
- PCA, k-means, embedding, autoencoder, CNN ve attention'ı ortak “temsil öğrenme” sorusu içinde konumlandırır;
- k-means atama–merkez güncelleme döngüsünü ve PCA'nın variance yönlerini küçük örnekte izler;
- clustering/anomaly/üretici çıktının ground-truth sınıf veya doğru karar anlamına gelmediğini açıklar; hiyerarşik kümeleme, DBSCAN ve GMM/EM'in hangi veri geometrilerinde k-means'ten ayrıldığını söyler;
- durum, eylem, ödül, politika ve exploration kavramlarını küçük bandit/ardışık karar örneğinde kullanır;
- bir model kararının gelecekte toplanan veriyi nasıl değiştirebileceğini feedback loop ile çizer;
- gözlemsel tahmin başarısından nedensel müdahale etkisi çıkarmaz;
- adalet, gizlilik, güvenlik, sağlamlık ve insan gözetimi için en az bir risk/kontrol çifti üretir;
- kod, veri, split, ortam, yapılandırma ve çıktıyı tek bir run kimliğiyle ilişkilendirir;
- deployment öncesi kabul, monitoring, alarm, rollback ve modelin kullanılmama koşullarını tanımlar;
- W4 checkpoint holdout'u yalnız kabul koşulları sağlandıktan sonra kayıtlı komutla bir kez değerlendirir; final blind holdout'u W14'e kadar kapalı tutar;
- Ç1–Ç10'un tamamını çalışan `v0.1` üzerinde savunur.

## Eğitmenin ders öncesi hazırlığı

### Ç7 — Küçük sinir ağı deneyi

- W3 ile aynı veri, feature ve development split'ini kullanan CPU-uyumlu küçük MLP hazırla.
- Tek gizli katman, iki gizli katman ve aşırı büyük ağ için kısa learning curve örnekleri üret.
- Çok yüksek/düşük learning rate, yanlış output/loss eşleşmesi ve overfitting örneklerini hazırla.
- PyTorch seed, deterministic ayar ve ortam kaydı kullanımını göster; tam bit düzeyi garanti iddia etme.
- MLP'yi Core model seçiminin zorunlu kazananı yapma; kontrollü karşılaştırma olarak konumlandır.

### Ç8 — Temsil istasyonu

- Küçük 2B/3B PCA görselleştirmesi, explained-variance özeti ve iki iterasyonluk k-means örneği hazırla.
- Aynı veride k-means, hiyerarşik kümeleme, DBSCAN ve GMM sonuçlarını karşılaştır; bu hafta yalnız k-means/PCA hesabını Core tut, diğerlerinin W12'de türetileceğini işaretle.
- Clustering etiketlerinin gerçek sınıflarla bire bir eşleşmediği ve silhouette yüksekliğinin alan geçerliliğini garanti etmediği örnek üret.
- Görsel/metin için önceden üretilmiş küçük embedding veya attention çıktısı kullan; dış servis/ücretli API gerektirme.
- Autoencoder/generative model için yalnız küçük sabit çıktı veya eğitmen demosu hazırla.
- Her örnekte “temsil faydası hangi downstream görevle ölçüldü?” sorusunu görünür tut.

### Ç9 — Ardışık karar istasyonu

- İki kollu bandit simülatörü: sabit ödül olasılıkları, epsilon-greedy politika ve regret grafiği.
- Öngörücü bakım için şu feedback loop'u hazırla: yüksek risk → erken bakım → gözlenen arıza azalır → etiket dağılımı değişir.
- Prediction, policy ve intervention kavramlarını ayrı renklerle göster.
- Offline tarihsel veriden yeni politikanın etkisini kesin bildiğini iddia eden hatalı örnek hazırla.

### Ç10 — Yaşam döngüsü ve kabul kapısı

- `data_card.md`, `model_card.md`, `risk_register.md` ve `reproducibility_manifest.json` şablonlarını hazırla.
- Checkpoint holdout runner'ını yalnız W2/W3 sözleşme testleri geçtiğinde açılacak biçimde yapılandır; final blind holdout'a W14 öncesi erişimi engelle.
- Checkpoint çalıştırmasının zamanını, commit'i, veri/split/model kimliğini ve çıktıyı otomatik kaydet.
- Basit inference entry point, schema hatası, dağılım kayması uyarısı ve güvenli fallback demosu hazırla.
- `v0.1` rubriğini ders başında yayımla; son dakikada yeni kabul koşulu ekleme.

## Ders öncesi öğrenci hazırlığı — 40–50 dakika

1. W3 `model_selection.md` dosyasını dondur; seçilen aday ve baseline yapılandırmalarını değiştirme.
2. Şu kavramları notasız eşleştir: neuron, activation, loss, gradient, optimizer, epoch, batch.
3. Aynı tablo verisinde küçük MLP'nin logistic regression'dan neden daha kötü olabileceğine iki gerekçe yaz.
4. Dönem problemin için bir “model kararı → dünyadaki eylem → yeni veri” döngüsü çiz.
5. En ciddi olası zararı ve bunu azaltacak insan/teknik kontrolü birer cümleyle yaz.
6. Temiz ortamdan W3'e kadar olan development deneylerini yeniden çalıştır; checkpoint ve final blind holdout'ları açma.

## Tahta/slayt omurgası

```text
Ç7  Girdi → öğrenilmiş temsil → çıktı → loss → gradient → güncelleme
                      │
Ç8                    └── temsil etiketsiz / transfer / üretici yolla da öğrenilebilir
                      │
Ç9  Tahmin → eylem → dünya değişir → yeni veri → sonraki model
                      │
Ç10 risk + belge + sürüm + yeniden üretim + serving + monitoring + rollback
                      │
                 Ç1–Ç10 bütünleşik v0.1
```

## 155 dakikalık ders akışı

| Süre | İçerik ve öğretmen hamlesi | Öğrenci işi / toplanan kanıt |
| --- | --- | --- |
| 00–08 | W1–W3 retrieval: problem → split → baseline → seçim zincirinde bir hata buldur. | Bireysel zincir kontrolü |
| 08–18 | **Ç7:** Neuron, layer, activation, output ve loss; küçük MLP forward pass. | Şekil üzerinde tensor/şekil izi |
| 18–25 | Backpropagation, gradient ve optimizer; chain-rule sezgisi. | Bir parametrenin güncelleme yönü tahmini |
| 25–30 | Learning rate/regularization/learning curve hata ayıklama. | Üç eğriye ilk tanı |
| 30–40 | **Ara** | |
| 40–49 | MLP'yi W3 sabit protokolünde çalıştır; klasik modelle skor/maliyet karşılaştır. | Tahmin–gözlem farkı |
| 49–58 | **Ç8:** PCA, k-means, hiyerarşik/DBSCAN/GMM, embedding, autoencoder, CNN ve attention'ı temsil sorusuna bağla. | Yöntem–veri geometrisi–downstream görev eşleştirmesi |
| 58–65 | Küçük PCA + iki iterasyonlu k-means istasyonu; temsil/küme kalitesinin nasıl sınanacağını sor. | G/M/F kaydı + bir iç ve bir dış/alan ölçütü |
| 65–70 | Generative/foundation model kullanımında kaynak, lisans, privacy ve evaluation ön izlemesi. | Bir risk + kontrol |
| 70–80 | **Ara** | |
| 80–90 | **Ç9:** State, action, reward, policy, value; supervised prediction'dan farkı. | Kavramları bakım kararına yerleştirme |
| 90–99 | İki kollu bandit ve exploration–exploitation simülasyonu. | Politika tahmini + regret gözlemi |
| 99–105 | Feedback loop ve offline evaluation riski. | Kendi problemi için döngü |
| 105–110 | Correlation, prediction ve intervention/causality sınırı. | Geçersiz nedensel iddiayı düzeltme |
| 110–120 | **Ara** | |
| 120–130 | **Ç10:** Fairness, privacy, security, robustness ve human oversight; bağlama bağlı risk. | Risk–etkilenen–kontrol–kanıt satırı |
| 130–137 | Reproducibility manifest; data/code/env/config/output kimliği ve seed sınırı. | Tek run için provenance zinciri |
| 137–145 | Serving, schema validation, drift, performance, alarm, fallback ve rollback. | Minimum monitoring sözleşmesi |
| 145–151 | `v0.1` kabul kapısı ve kayıtlı checkpoint holdout çalıştırması; sonucu yeni seçim için kullanmama, final blind holdout'u koruma. | Checkpoint run kaydı + geçer/kalır kararı |
| 151–155 | Bireysel exit ticket ve W5 derinleşme başlangıcı. | Çıkış kaydı |

## Dört çapanın bu hafta için doğru derinliği

| Çapa | Bu hafta ayrıntılı işle | Sonraki derinleşmeye bırak |
| --- | --- | --- |
| Ç7 | MLP bileşenleri, forward/backprop sezgisi, optimizer, learning curve ve temel debug | Ayrıntılı mimari tasarım, ileri optimizasyon ve büyük ölçekli eğitim |
| Ç8 | Temsil öğrenme ortak sorusu; PCA ve k-means mekanizması; hierarchical/DBSCAN/GMM, anomaly, embedding ve modern mimarilerin yeri | W12'de denetimsiz yöntemlerin ayrıntılı karşılaştırması; her modalite için tam model eğitimi, ileri generative yöntemler ve fine-tuning |
| Ç9 | State/action/reward/policy, bandit, exploration ve feedback/causality sınırı | Bellman türetimleri, ileri RL algoritmaları ve kapsamlı causal inference |
| Ç10 | Risk kaydı, card'lar, reproducibility, serving/monitoring/rollback sözleşmesi | Üretim platformu kurulumu, ileri güvenlik, privacy-preserving learning ve kurumsal yönetişim |

## Dört istasyonlu sınıf çalışması

Ekipler 20 dakikalık bütünleşik stüdyo süresinde istasyon çıktılarının verilen örneklerini inceler; ders içi akıştaki mini etkinliklerle birlikte her çapanın ilk sistematik uygulaması tamamlanır.

### İstasyon A — Sinir ağı debug kartı

Verilen üç learning curve ve yapılandırmadan birini seç:

- çok yüksek learning rate;
- training artarken development kötüleşiyor;
- loss/output eşleşmesi yanlış.

Öğrenci tanıyı, tek değişiklik önerisini ve değişiklikten sonra beklediği eğriyi yazar. Birden fazla şeyi aynı anda değiştirmez.

### İstasyon B — Temsil kararı

Veri türü ve amaç kartlarından biri seçilir:

- sensör feature'larında PCA;
- bakım notlarında önceden üretilmiş embedding;
- etiketsiz kayıtları clustering;
- anomali için reconstruction error.

Öğrenci yöntemin çözdüğü soruyu, başarı ölçütünü ve temsilin neyi garanti etmediğini belirtir.

### İstasyon C — Politika ve feedback

Bandit simülasyonunda iki epsilon ayarı karşılaştırılır. Ardından model önerisinin bakım eylemi yoluyla yeni etiketleri nasıl değiştirdiği çizilir. Öğrenci “yüksek riskli makineye bakım yapmak arızayı azaltır” ifadesinin tahmin modeliyle tek başına neden kanıtlanamayacağını açıklar.

### İstasyon D — Risk ve yaşam döngüsü

Ekip bir risk satırını tamamlar:

```text
Risk:
Etkilenen kişi/sistem:
Oluşma yolu:
Önleme veya azaltma kontrolü:
Kontrolün kanıtı:
İzlenecek sinyal ve eşik:
Fallback / rollback:
Sorumlu insan:
```

## `v0.1` ürün sözleşmesi

### Beklenen depo yapısı

```text
ml-evidence-lab/
├── README.md
├── problem_card.md
├── data_card.md
├── model_card.md
├── risk_register.md
├── AI_ASSISTANCE.md
├── pyproject.toml veya requirements.lock
├── configs/
│   ├── baseline.yaml
│   ├── selected_model.yaml
│   └── mlp_preview.yaml
├── src/
│   ├── data.py
│   ├── models.py
│   ├── train.py
│   ├── evaluate.py
│   └── predict.py
├── tests/
│   ├── test_data_contract.py
│   ├── test_experiment_contract.py
│   └── test_prediction_schema.py
└── artifacts/
    ├── model_comparison.csv
    ├── test_metrics.json
    ├── error_slices.csv
    ├── reproducibility_manifest.json
    └── monitoring_plan.md
```

Gerçek klasör adı ve paketleme aracı dönem başında sabitlenebilir; kanıt sözleşmesi korunur.

### Tek komutlu akış

Temiz ortamda belgelenmiş komut sırası şunları yapar:

1. veri bütünlüğünü doğrular;
2. sabit split kimliklerini yükler;
3. development aşamasında baseline ve seçilen modeli yeniden üretir;
4. veri/deney sözleşme testlerini çalıştırır;
5. daha önce açılmadıysa W4 checkpoint holdout'u kayıtlı biçimde bir kez değerlendirir; final blind holdout'un kapalı olduğunu doğrular;
6. metrik, run manifest ve card özetlerini üretir;
7. tek örnek prediction için giriş/çıkış schema kontrolünü gösterir.

Checkpoint sonucu görüldükten sonra model, feature, threshold veya preprocessing değiştirilirse bu sonuç yeni sürüm için tarafsız değerlendirme değildir. Değişiklik ve checkpoint'e uyarlanma riski açıkça kaydedilir. Dönem boyunca geliştirilen son sürümün tarafsız değerlendirmesi için final blind holdout W14'e kadar açılmaz.

## `v0.1` kabul kapısı

### Problem ve veri

- Karar sahibi, gözlem birimi, tahmin anı, hedef/ufuk ve kapsam dışı kullanım açıktır.
- Veri kaynağı/lisansı, DGP, temel bias/leakage riskleri ve split sözleşmesi belgelenmiştir.
- Tahmin anında bulunmayan alanlar feature setinde yoktur.

### Deney ve model

- Dummy/kural baseline ile seçilen klasik model aynı protokolde karşılaştırılmıştır.
- Küçük MLP aynı development protokolünde en az bir kontrollü preview olarak çalıştırılmıştır.
- Modern temsil örneği ve bandit/feedback etkinliği ayrı küçük kanıt olarak bulunur; probleme anlamsızsa ana pipeline'a zorla eklenmez.
- Model ve threshold seçimi checkpoint açılmadan önce dondurulmuştur.
- Checkpoint sonucu run kimliğiyle bir kez kaydedilmiş; final blind holdout erişimi engellenmiştir.

### Değerlendirme

- Birincil metrik karar maliyetiyle ilişkilidir.
- Baseline farkı, confusion matrix/eşdeğer sonuç, calibration özeti ve en az iki hata dilimi vardır.
- Tek skor geniş genelleme, nedensellik veya güvenlik kanıtı olarak sunulmaz.

### Yeniden üretim ve yazılım kalitesi

- Temiz CPU ortamı için kurulum ve çalıştırma yolu belgelenmiştir.
- Veri, split, kod/commit, ortam, config, seed ve çıktı kimlikleri manifestte bağlıdır.
- Veri, deney ve prediction schema testleri geçer.
- Gizli anahtar, kişisel yol veya izinsiz veri yoktur.

### Sorumluluk ve yaşam döngüsü

- Problem/data/model card taslakları birbiriyle tutarlıdır.
- En az dört risk satırı vardır: veri/temsil, karar zararı, güvenlik/gizlilik ve operasyon/dağılım.
- Her kritik risk için kontrol, gözlenecek sinyal ve insan sorumlusu belirtilmiştir.
- Monitoring planı giriş schema/dağılımı, model çıktısı, kalite gecikmesi, latency/hata ve alarm eşiği içerir.
- Güvenli fallback, rollback ve “modeli kullanma/durdur” koşulu vardır.

### Bireysel savunma

Her öğrenci rastgele seçilen bir soruyu yanıtlar:

- Bir feature neden tahmin anında geçerli?
- Split hangi gerçek kullanım iddiasını temsil ediyor?
- Seçilen model neden baseline'dan daha anlamlı?
- Threshold değişirse hangi hata maliyeti değişir?
- MLP neden kazanmış veya kaybetmiş olabilir?
- Bir risk kontrolünün gerçekten çalıştığını nasıl sınarsın?
- Checkpoint'i tekrar tekrar kullanmak hangi iddiayı bozar; final blind holdout neden korunur?
- Model hangi koşulda otomatik olarak durmalıdır?

## Core, Stretch ve eğitmen demosu

### Core

- Küçük CPU-uyumlu MLP ve learning curve analizi.
- PCA/embedding/clustering örneğinden bir temsil etkinliği.
- İki kollu bandit ve kendi probleminde feedback-loop haritası.
- Tam `v0.1` kabul paketi, risk/izleme sözleşmesi ve bireysel savunma.

### Stretch

- Erken durdurma, farklı optimizer veya calibration yöntemi için tek faktörlü ablation.
- Probleme uygunsa önceden eğitilmiş küçük bir representation extractor ile baseline karşılaştırması.
- Basit off-policy evaluation simülasyonu; güçlü varsayım ve sınırlarıyla.
- Veri kayması sentetik enjeksiyonu ve alarm eşiği hassasiyet analizi.

### Instructor demo

- CNN/transformer veya generative modelin küçük önceden üretilmiş çıktısı.
- Model serving ve monitoring dashboard örneği.
- Adversarial, privacy veya büyük model güvenlik vakası.
- GPU/dağıtık eğitim ve foundation model fine-tuning maliyet görünümü.

## Kritik sorular ve beklenen yön

- **MLP daha esnekse neden klasik modelden kötü olabilir?** Veri azlığı, optimizasyon, ölçek, regularization, gürültü ve tabular yapı etkileyebilir.
- **Learning curve sorunun nedenini kesin gösterir mi?** Tanı için kanıttır; tek başına nedensel kesinlik sağlamaz, kontrollü değişiklikle sınanır.
- **PCA'da ayrılan kümeler gerçek sınıfları kanıtlar mı?** Hayır; projection yapısı ve varyans, görev etiketi veya iş değeri değildir.
- **K-means neden her küme yapısını bulamaz?** Öklid uzaklığı ve merkez etrafında yaklaşık küresel/eş-ölçekli yapı varsayar; başlangıç, ölçek ve `k` seçimine duyarlıdır.
- **DBSCAN neyi farklı yapar?** Merkez yerine yoğunluk bağlantısı kullanır, gürültü noktası ayırabilir ve küme sayısını baştan istemez; değişen yoğunluk ve yüksek boyutta zorlanır.
- **Embedding benzerliği doğru/ilgili cevap mıdır?** Aday yakınlığıdır; downstream relevance, filtre, yetki ve insan/alan doğrulaması gerekir.
- **Bandit supervised learning'den neden farklıdır?** Eylem hangi ödül verisinin gözleneceğini etkiler; exploration ve counterfactual eksikliği vardır.
- **Prediction doğruysa intervention da doğru mudur?** Hayır; tahmin ilişkisi müdahale etkisini tanımlamaz.
- **Hassas sütunu silmek adaleti sağlar mı?** Proxy'ler, örnekleme, etiket ve farklı hata maliyetleri devam edebilir.
- **Model card güvenliği garanti eder mi?** Belgeleme görünürlük ve sorumluluk sağlar; test, kontrol ve yönetişimin yerine geçmez.
- **Aynı seed neden aynı sonucu garanti etmeyebilir?** Donanım, sürüm, nondeterministic işlem, paralellik ve veri sırası etkileyebilir.
- **Monitoring yalnız accuracy midir?** Gerçek etiket gecikebilir; schema, drift, skor/eşik oranı, latency, hata, override ve iş sonucu birlikte izlenir.

## Yaygın yanılgılar ve müdahale

- “Neural network kullanmadıysak proje modern değildir.” → Problem/protokol geçerliliğini ve basit modelin güçlü baseline rolünü geri getir.
- “Loss düşüyorsa model iyileşiyor.” → Development eğrisi, calibration, karar metriği ve hata dilimlerini birlikte göster.
- “Clustering gerçeğin doğal sınıflarını bulur.” → Ölçek, mesafe ve algoritma değişince kümelerin değiştiği örneği çalıştır.
- “RL ödülü maksimize ederse doğru davranır.” → Eksik/yanlış reward ve güvenlik constraint'i karşı örneği ver.
- “Fairness tek bir metrikle çözülür.” → Çatışan ölçüler ve bağlama bağlı zarar/uygunluk sorusunu sordur.
- “Reproducible = seed sabit.” → Veri, kod, config, ortam ve çalışma kaydını eksik bırakıp yeniden üretim denetimi yaptır.
- “Deployment yalnız modeli API'ye koymaktır.” → Schema, latency, fallback, human override, drift ve rollback'i kabul kapısına ekle.
- “Checkpoint kötü geldiyse birkaç ayar daha yapıp aynı sonucu yeniden kontrol ederiz.” → Checkpoint'in validation'a dönüşeceğini açıkla; değişikliği kaydet ve final blind holdout'u W14'e kadar koru.

## Hafta sonu teslim paketi

`v0.1` depo yapısına ek olarak şu kısa belgeler teslim edilir:

1. Ç1–Ç10 güncel sistem/kanıt haritası.
2. MLP için learning curve ve en fazla 200 kelimelik debug yorumu.
3. PCA/k-means küçük hesabı ve temsil istasyonu için yöntem–veri geometrisi–ölçüt–sınır kartı.
4. Bandit çıktısı ve kendi probleme ait feedback-loop diyagramı.
5. En az dört satırlık risk register ve monitoring planı.
6. Checkpoint holdout'un ilk/tek çalıştırılma kaydı veya kabul önkoşulu geçmediyse neden açılmadığı; final blind holdout'un kapalı olduğuna dair kontrol.
7. En fazla 750 kelimelik `v0.1` teknik özeti: iddia, kanıt, sınır, risk ve W5'te düzeltilecek en önemli nokta.
8. AI kullanımı ve dış katkı kaydı.

Checkpoint holdout'un açılmaması otomatik başarısızlık değildir; sözleşme testi geçmiyorsa holdout'u korumak, geçersiz bir skor üretmekten daha doğru karardır. Eksik kabul koşulu ve geri kazanım planı açıkça yazılır. Final blind holdout hiçbir durumda W4'te açılmaz.

## Exit ticket

1. MLP'nin klasik modele göre en önemli ek varsayımı veya bedeli nedir?
2. Temsil yönteminin başarısını hangi downstream kanıtla ölçersin?
3. Model kararının gelecekteki veriyi değiştirdiği bir yol nedir?
4. `v0.1` için en ciddi açık risk ve kontrolü nedir?
5. Modeli hangi gözlem veya eşikte durdurursun?
6. W5'te problem çerçevesinde ilk yeniden incelemek istediğin karar hangisi?

## 5. haftaya köprü

> **İkinci tur tamamlandı: Ç1–Ç10 artık çalışan tek bir `v0.1` yaşam döngüsünde yer alıyor. 5. haftada Ç1'e derinlemesine dönerek problem tanımını, veri üretim sürecini, baseline'ı ve karar maliyetini daha sert karşı örneklerle yeniden sınayacağız.**

W5'e geçişte ürün dondurulmaz; `v0.1` etiketlenir. Sonraki on hafta bu tabana kanıtlı artımlar ekler. Her artım, mevcut protokolü değiştiriyorsa nedenini ve önceki sonuçlarla karşılaştırılabilirliğin nasıl etkilendiğini kaydeder.
