# BİL 536 — Matematik ve Algoritma Kapsam Matrisi

**Amaç:** Makine öğrenmesinde yaygın kullanılan problem türlerini, algoritma ailelerini ve temel matematiksel dayanaklarını öğretilebilir bir yapıda göstermek.
**İlgili belge:** [Dersin ana README dosyası](README.md)

Bu belge bir algoritma ansiklopedisi değildir. “Tüm algoritmalar”, literatürdeki her varyant yerine lisansüstü bir giriş dersinde bilinmesi beklenen yaygın ana aileleri ifade eder. Her yöntem şu sorularla ele alınır:

1. Hangi problem ve veri türü için uygundur?
2. Temel çalışma fikri ve matematiksel varsayımı nedir?
3. Hangi koşullarda başarısız olabilir?
4. Hangi baseline ve değerlendirme yöntemiyle karşılaştırılmalıdır?

## Öğrenme derinliği kodları

| Kod | Beklenen öğrenme kanıtı |
| --- | --- |
| **T — Türet ve kur** | Temel fikri ve gerekli denklemi açıklar, küçük örneği hesaplar ve yöntemi kodla kurar. |
| **K — Kur ve karşılaştır** | Hazır kütüphaneyle sızıntısız deney kurar; sonucu, hiperparametreyi ve hata biçimini yorumlar. |
| **P — Panorama** | Yöntemin çalışma fikrini, kullanım alanını, maliyetini ve temel sınırını açıklar. |

Core yol bütün **T** kazanımlarını ve probleme uygun **K** deneylerini içerir. **P** yöntemlerinde ayrıntılı türetme veya büyük ölçekli eğitim beklenmez.

## I. Matematiksel temel

Matematik ayrı bir ezber haftası olarak bırakılmaz; ilgili algoritmayla birlikte yeniden kullanılır.

| Alan | Zorunlu kavramlar | Bağlandığı yöntemler | Beklenen kanıt |
| --- | --- | --- | --- |
| **Doğrusal cebir** | Vektör, matris, iç çarpım, norm, matris boyutu; özdeğer/SVD sezgisi | Doğrusal modeller, PCA, embeddings ve sinir ağları | Boyut kontrolü ve küçük matris işlemi |
| **Analiz ve optimizasyon** | Türev, gradyan, chain rule, learning rate; konvekslik sezgisi | Regresyon, lojistik regresyon, SVM, boosting ve sinir ağları | Gradyan yönü ve öğrenme eğrisi yorumu |
| **Olasılık** | Koşullu olasılık, bağımsızlık, Bayes; beklenti, varyans ve kovaryans | Naive Bayes, LDA/QDA, GMM ve belirsizlik | Küçük posterior hesabı ve varsayım açıklaması |
| **İstatistik** | Örnekleme, likelihood, MLE/MAP sezgisi, güven aralığı ve bootstrap | Model tahmini, seçimi ve değerlendirme | Sonuçla birlikte belirsizlik ve sınırlı iddia |
| **Bilgi kuramı** | Entropi, cross-entropy, KL divergence ve information gain ilişkisi | Karar ağaçları, sınıflandırma kaybı ve üretici modeller | Küçük entropi/bölme hesabı |
| **Geometri** | Öklid, Manhattan ve cosine; ölçek, komşuluk ve margin | k-NN, k-means, DBSCAN, SVM ve embeddings | Ölçeklemeden önce/sonra karşılaştırma |
| **Öğrenme kuramı** | Genelleme, bias–variance, kapasite, overfit/underfit ve regularization | Bütün denetimli yöntemler | Learning curve ve regularization deneyi |

### Derste kullanılacak çekirdek denklemler

**Ampirik risk ve düzenlileştirme** çoğu denetimli yöntemi ortak bir çerçevede düşünmeyi sağlar:

$$
\hat{f}=\arg\min_f\left[\frac{1}{n}\sum_{i=1}^{n}L\bigl(y_i,f(x_i)\bigr)+\lambda\Omega(f)\right]
$$

Öğrenci farklı kayıp ve ceza seçimlerinin model davranışını nasıl değiştirdiğini açıklar; ayrıntılı ispat beklenmez.

**Bayes teoremi ve Naive Bayes**:

$$
P(y\mid x)=\frac{P(x\mid y)P(y)}{P(x)},
\qquad
P(y\mid x_1,\ldots,x_d)\propto P(y)\prod_j P(x_j\mid y)
$$

Koşullu bağımsızlığın bir modelleme varsayımı olduğu açıklanır. Gaussian, Multinomial ve Bernoulli seçenekleri veri türüne göre ayrılır; smoothing gösterilir.

**Entropi ve bilgi kazancı**:

$$
H(Y)=-\sum_y p(y)\log p(y),
\qquad
IG=H(\text{ebeveyn})-\sum_k\frac{n_k}{n}H(\text{çocuk}_k)
$$

Entropy ile Gini ayrılır. Cross-entropy ve KL divergence kavramsal olarak ele alınır; ayrı formülleri ezberletilmez.

**Gradyan tabanlı güncelleme**:

$$
\theta_{t+1}=\theta_t-\eta\nabla_\theta L(\theta_t)
$$

Bu ifade doğrusal modelden sinir ağına kadar ortak optimizasyon dili olarak kullanılır.

## II. Problem türüne göre algoritma haritası

### A. Denetimli öğrenme — regresyon

| Yöntem | Düzey | Matematiksel omurga | Temel sınır |
| --- | ---: | --- | --- |
| Mean/median ve kural baseline | T | Merkez ve kayıp | Baseline olmadan ilerleme yorumlanamaz. |
| Doğrusal ve polynomial regresyon | T | Matris çarpımı, least squares, MSE | Extrapolation ve korelasyon–nedensellik ayrımı |
| Ridge, Lasso ve Elastic Net | T | L2/L1 cezası, regularization | Ölçek ve ilişkili özelliklere duyarlılık |
| Robust, quantile ve Poisson/GLM | K/P | Uygun kayıp veya likelihood seçimi | Her hedef için aynı kayıp kullanılmaz. |
| k-NN regression | K | Uzaklık ve yerel ortalama | Ölçek ve yüksek boyut sorunu |
| Regression tree | T | Greedy split ve hata azalması | Parçalı sabit tahmin ve yüksek variance |
| Random forest ve gradient boosting | K | Bagging veya additive learning | Maliyet, ayar ve overfit riski |
| SVR | K | Margin, kernel ve hata toleransı | Ölçek ve büyük veri maliyeti |

### B. Denetimli öğrenme — sınıflandırma

| Yöntem | Düzey | Matematiksel omurga | Temel sınır |
| --- | ---: | --- | --- |
| Majority/stratified baseline | T | Sınıf öncülü ve hata maliyeti | Accuracy dengesiz sınıfta yanıltabilir. |
| Lojistik regresyon | T | Log-odds, sigmoid/softmax ve cross-entropy | Olasılık, threshold ve karar farklıdır. |
| k-NN | T | Mesafe ve komşu oyu | Ölçek ve boyut arttıkça komşuluk bozulur. |
| Gaussian/Multinomial/Bernoulli Naive Bayes | T | Bayes, likelihood, koşullu bağımsızlık | Calibration ve bağımsızlık varsayımı |
| Decision tree | T | Entropy/Gini, greedy split ve budama | Derin ağaçta overfit ve kararsızlık |
| Random forest, Extra Trees ve AdaBoost | K | Bagging, rastgelelik veya örnek ağırlığı | Yorum ve hesap maliyeti |
| Gradient boosting; XGBoost/LightGBM/CatBoost | K | Ardışık zayıf öğreniciler ve regularization | Validation setine aşırı uyum |
| Linear/kernel SVM | T/K | Maksimum margin, hinge loss ve kernel | Doğal olasılık çıktısı yoktur; ölçek önemlidir. |
| Perceptron ve MLP | T | Aktivasyon, chain rule ve backpropagation | Optimizasyon, overfit ve calibration |

Çok sınıflı ve çok etiketli problemler uygun çıktı yapısı ve metrik seçimi üzerinden ayrıca gösterilir.

### C. Denetimsiz öğrenme — kümeleme

| Yöntem | Düzey | Temel fikir | Temel sınır |
| --- | ---: | --- | --- |
| k-means / k-means++ | T | Noktaları en yakın merkeze atama | Küresel kümeler, ölçek, başlangıç ve `k` seçimi |
| Hiyerarşik kümeleme | T | Uzaklığa göre küme ağacı | Linkage seçimi sonucu değiştirir. |
| DBSCAN | T | Yoğun bölgeler ve gürültü | Değişen yoğunluk ve yüksek boyut |
| Gaussian Mixture Model / EM | T/K | Olasılıksal soft assignment | Yerel optimum ve bileşen sayısı |

HDBSCAN ve spectral clustering panorama düzeyindedir. Küme değerlendirmesinde inertia veya silhouette tek başına yeterli sayılmaz; kararlılık ve alan anlamı da tartışılır.

### D. Boyut indirgeme ve temsil öğrenme

| Yöntem | Düzey | Temel fikir | Temel sınır |
| --- | ---: | --- | --- |
| PCA | T | En yüksek varyansı taşıyan doğrusal yönler | Ölçek ve doğrusal yapı varsayımı |
| Truncated SVD / LSA | K | Sparse veride düşük rank temsil | Bileşenlerin yorumlanması |
| t-SNE ve UMAP | P | Yerel komşulukları görselleştirme | Görsel küme, gerçek sınıf kanıtı değildir. |
| Embedding ve transfer learning | K | Önceden öğrenilmiş temsil ve benzerlik | Domain shift, lisans ve gizlilik |

NMF ve autoencoder, düşük boyutlu temsilin alternatifleri olarak panorama düzeyinde tanıtılır.

### E. Anomali ve yenilik tespiti

| Yöntem | Düzey | Temel fikir | Temel sınır |
| --- | ---: | --- | --- |
| İstatistiksel eşik / robust z-score | T | Merkezden sapma | Dağılım ve tek değişken varsayımı |
| Isolation Forest | K | Rastgele bölmelerde kısa yol | Eşik ve contamination kararı |
| Local Outlier Factor | K | Yerel yoğunluk farkı | Novelty ve outlier kullanım farkı |

One-Class SVM ve reconstruction/density skorları panorama düzeyindedir. Etiket yoksa sentetik enjeksiyon, uzman incelemesi ve backtest birlikte kullanılır.

### F. Sinir ağları ve derin öğrenme

| Yöntem | Düzey | Temel fikir | Temel sınır |
| --- | ---: | --- | --- |
| Perceptron ve MLP | T | Katmanlı dönüşüm ve backpropagation | Learning rate, initialization ve overfit |
| CNN | K | Yerel filtre ve paylaşılan ağırlık | Görüntü/ızgara varsayımı ve veri ihtiyacı |
| Attention ve Transformer | K/P | Query–key–value ile bağlam | Veri, hesap ve bağlam uzunluğu maliyeti |

RNN/LSTM/GRU ve transfer learning, uygun veri örnekleri üzerinden panorama düzeyinde karşılaştırılır.

### G. Zaman serisi ve sıralı veri

| Yöntem | Düzey | Temel fikir | Temel sınır |
| --- | ---: | --- | --- |
| Naive/seasonal naive ve moving average | T | Geçmiş değer ve mevsimsel baseline | Rastgele split kullanılamaz. |
| Lag/rolling özellikli linear ve tree modelleri | K | Zaman penceresini denetimli veriye çevirme | Gelecek bilgisi sızıntısı |
| Exponential smoothing ve ARIMA | P | Trend, mevsimsellik ve otokorelasyon | Stationarity ve doğru horizon |

HMM ve neural sequence modelleri panorama düzeyindedir. Değerlendirme rolling veya expanding-window backtest ile yapılır.

### H. Öneri sistemleri ve örüntü madenciliği

| Yöntem | Düzey | Temel fikir | Temel sınır |
| --- | ---: | --- | --- |
| Popularity/recency baseline | T | Sıklık ve güncellik | Popülerlik yanlılığı |
| Content-based ve collaborative filtering | K | Özellik veya kullanıcı–ürün benzerliği | Cold start ve exposure bias |
| Matrix factorization | K | Düşük boyutlu kullanıcı–ürün temsili | Eksik etkileşimlerin rastgele olmaması |

Apriori/FP-Growth örüntü madenciliği panoramasında yer alır. Birliktelik nedensellik değildir; offline ranking metriği de gerçek kullanıcı değeriyle aynı değildir.

### I. Yarı denetimli, öz-denetimli ve üretici öğrenme

| Yöntem | Düzey | Temel fikir | Temel sınır |
| --- | ---: | --- | --- |
| Pseudo-labeling / self-training | P | Güvenli tahminleri yeni etiket gibi kullanma | Hatalar pekişebilir. |
| Contrastive/self-supervised öğrenme | P | Benzer ve farklı örneklerden temsil öğrenme | Augmentation seçimi semantiği etkiler. |
| Topic model, autoregressive model, VAE, GAN ve diffusion | P | Gizli yapı veya veri üretim dağılımı | Kalite, çeşitlilik ve güvenlik ayrı ölçülür. |

Label propagation, graf tabanlı yarı denetimli öğrenmeye kısa bir örnek olarak verilir.

### J. Pekiştirmeli öğrenme ve ardışık karar

| Yöntem | Düzey | Temel fikir | Temel sınır |
| --- | ---: | --- | --- |
| Multi-armed bandit; epsilon-greedy/UCB | T | Exploration–exploitation dengesi | Eylem gelecekteki veriyi değiştirir. |
| Markov Decision Process | T | Durum, eylem, geçiş, ödül ve politika | Markov varsayımı ve ödül tasarımı |
| Value/policy iteration ve Q-learning | K | Bellman ilişkisi ve değer güncelleme | Durum uzayı, exploration ve kararlılık |

Policy gradient ve actor–critic, örnek verimsizliği ve reward hacking riskiyle panorama düzeyinde ele alınır.

## III. Algoritma seçme dili

Öğrenci “en iyi algoritma” yerine aşağıdaki sorularla probleme uygun seçim yapar:

| Eksen | Sorulacak soru |
| --- | --- |
| Hedef | Sürekli değer, sınıf, sıralama, küme, anomali, temsil veya eylem mi? |
| Etiket | Etiket var mı; güvenilir ve temsili mi? |
| Veri yapısı | Doğrusal, yerel, yoğunluk-temelli, sıralı veya yüksek boyutlu mu? |
| Varsayım | Bağımsızlık, mesafe, stationarity veya Markov varsayımı makul mü? |
| Ölçek | Örnek/özellik sayısı, sparsity, bellek ve inference bütçesi nedir? |
| Risk | Açıklanabilirlik, adalet, gizlilik veya insan gözetimi gerekiyor mu? |
| Değerlendirme | Split, metrik, baseline ve belirsizlik gerçek kullanımı temsil ediyor mu? |

## IV. 14 haftalık yerleşim

| Hafta | Matematik ve kavram | Algoritma/problem aileleri | Düzey |
| ---: | --- | --- | --- |
| 1 | Tahmin, kayıp, baseline ve genelleme dili | Uçtan uca sınıflandırma vakası; ailelerin haritası | P |
| 2 | Vektör/matris, olasılık, Bayes ve türev temeli | Baseline ve sızıntısız pipeline | T |
| 3 | MSE, likelihood, entropy/Gini ve margin | Linear/logistic, Naive Bayes, k-NN, tree; ensemble/SVM bağlantısı | T/K |
| 4 | Gradient, temsil geometrisi ve beklenen ödül | MLP, PCA/k-means, bandit ve modern aileler | T/P |
| 5 | Karar ve veri üretim süreci | Problem tipleri, baseline ve hata maliyeti | T |
| 6 | Olasılık, bilgi kuramı, optimizasyon ve genelleme | Küçük hesaplar ve öğrenme eğrisi | T |
| 7 | Uzaklık, dönüşüm ve doğrulama | k-NN, preprocessing, random/group/time split | T/K |
| 8 | Least squares, Bayes, regularization ve calibration | Linear/Ridge/Lasso, logistic, Naive Bayes, LDA/QDA | T/K |
| 9 | Entropy, Gini, margin ve boosting | Tree, random forest, boosting ve SVM/SVR | T/K |
| 10 | Örnekleme, eşik ve belirsizlik | Regresyon/sınıflandırma metrikleri ve hata analizi | T/K |
| 11 | Backpropagation ve stochastic optimization | MLP; CNN ve attention panoraması | T/K/P |
| 12 | PCA/SVD, mesafe, yoğunluk ve gizli üyelik | Kümeleme, temsil, anomali ve öneri | T/K/P |
| 13 | Beklenen ödül, regret ve Markov fikri | Bandit, MDP, Q-learning ve nedensellik sınırı | T/K/P |
| 14 | Dağılım kayması ve operasyonel risk | İzleme, yeniden eğitim/rollback ve seçim savunması | Bütünleştirme |

## V. Asgari dönem sonu yeterliği

Dersi tamamlayan öğrenci:

- regresyon, sınıflandırma, kümeleme, boyut indirgeme ve anomali problemlerini ayırır;
- linear/logistic regression, Naive Bayes, k-NN, decision tree, k-means, DBSCAN, PCA ve temel MLP'nin mekanizmasını açıklar;
- regularized linear model, ensemble, SVM, en az iki kümeleme ve bir anomali yöntemini sabit protokolde karşılaştırır;
- entropy, cross-entropy, likelihood, Bayes, information gain, Gini, margin, gradient, bias–variance ve regularization kavramlarını doğru aileye bağlar;
- zaman serisi, öneri, üretici öğrenme ve pekiştirmeli öğrenmenin kullanım alanını ve temel riskini tanır;
- bir yöntemi yalnız skora göre değil, varsayım, veri yapısı, belirsizlik, maliyet ve kullanım riskiyle seçer;
- görmediği bir algoritmayı problem türü, temsil, amaç, optimizasyon ve değerlendirme açısından sınıflandırır.
