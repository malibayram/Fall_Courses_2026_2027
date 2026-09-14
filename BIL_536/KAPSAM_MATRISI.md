# BİL 536 — Matematik ve Algoritma Kapsam Matrisi

**Amaç:** Makine öğrenmesinde yaygın kullanılan ana problem türlerini, algoritma ailelerini ve bunların matematiksel dayanaklarını eksiksiz ve öğretilebilir bir yapıda göstermek.  
**İlgili belge:** [Dersin ana README dosyası](README.md)

Bu belge bir “algoritma kataloğu” değildir. Her yöntem şu dört soruyla ele alınır:

1. Hangi problem ve veri yapısı için kullanılır?
2. Hangi amaç fonksiyonunu veya olasılıksal varsayımı kullanır?
3. Hangi koşullarda başarısız olur?
4. Hangi baseline ve değerlendirme protokolüyle karşılaştırılır?

“Tüm algoritmalar” ifadesi, literatürdeki her varyant değil; lisansüstü bir makine öğrenmesi dersinde bilinmesi beklenen **yaygın ana ailelerin tamamı** anlamındadır. Yeni veya alan-özel bir yöntem, bu matristeki en yakın aileye; temsil, amaç fonksiyonu, optimizasyon ve değerlendirme açısından yerleştirilebilmelidir.

## Öğrenme derinliği kodları

| Kod | Beklenen öğrenme kanıtı |
| --- | --- |
| **T — Türet ve kur** | Temel denklemi açıklar/türetir, küçük örneği elle hesaplar ve yöntemi kodla kurar. |
| **K — Kur ve karşılaştır** | Hazır kütüphaneyle sızıntısız pipeline kurar, hiperparametreleri ve başarısızlık kiplerini deneyle yorumlar. |
| **P — Panorama** | Yöntemin çalışma fikrini, varsayımını, maliyetini ve ne zaman seçilebileceğini açıklar; eğitmen demosunu analiz eder. |

Her öğrencinin Core yolu bütün **T** kazanımlarını ve probleme uygun **K** deneylerini içerir. **P** düzeyi yöntemler isim ezberiyle değil, bir girdi–temsil–amaç–çıktı–değerlendirme haritasıyla işlenir.

## I. Matematiksel temel

Matematik tek bir “önkoşul haftası” olarak bırakılmaz; ilgili algoritmayla birlikte yeniden kullanılır.

| Matematik alanı | Zorunlu kavramlar | Bağlandığı yöntemler | Ana kanıt |
| --- | --- | --- | --- |
| **Doğrusal cebir** | Skaler, vektör, matris, tensör; iç çarpım; norm; doğrusal bağımsızlık; rank; özdeğer/özvektör; SVD; pozitif yarı tanımlılık | Doğrusal modeller, PCA/SVD, kernel, sinir ağları, matrix factorization | Boyut/şekil izi, matris biçiminde tahmin, küçük PCA/SVD hesabı |
| **Analiz ve optimizasyon** | Türev, kısmi türev, gradyan, chain rule; konvekslik sezgisi; kapalı form; gradient descent, SGD, momentum ve Adam | Regresyon, lojistik regresyon, SVM, boosting, MLP ve derin ağlar | Bir parametrenin gradyanı ve güncelleme yönü; learning-rate deneyi |
| **Olasılık** | Birleşik/marjinal/koşullu olasılık; bağımsızlık ve koşullu bağımsızlık; Bayes teoremi; beklenti, varyans, kovaryans; temel dağılımlar | Naive Bayes, lojistik modeller, LDA/QDA, GMM, kalibrasyon, Bayesçi çıkarım | Posterior hesabı, varsayım grafiği, olasılık–karar ayrımı |
| **İstatistiksel çıkarım** | Örnekleme; tahmin edici; likelihood, log-likelihood, MLE ve MAP; güven aralığı; bootstrap; hipotez testi ve çoklu karşılaştırma sezgisi | Parametrik modeller, model seçimi, belirsizlik ve deney raporlama | Likelihood–loss eşleştirmesi, bootstrap aralığı, sınırlı iddia |
| **Bilgi kuramı** | Self-information, entropy, conditional entropy, cross-entropy, KL divergence, mutual information; information gain | Karar ağaçları, sınıflandırma kayıpları, Naive Bayes, temsil ve üretici modeller | Entropy/information-gain hesabı; cross-entropy ve KL yorumu |
| **Geometri ve benzerlik** | Öklid/Manhattan/cosine uzaklığı; p-norm; ölçek etkisi; margin; yüksek boyutta uzaklık | k-NN, k-means, hiyerarşik kümeleme, DBSCAN, SVM, embeddings | Ölçeklemeden önce/sonra komşuluk veya küme karşılaştırması |
| **İstatistiksel öğrenme** | Hipotez uzayı; ampirik/beklenen risk; genelleme; bias–variance; kapasite; regularization; underfit/overfit | Bütün denetimli yöntemler | Learning curve, regularization ve veri boyutu kontrollü deneyi |
| **Sayısal hesaplama** | Floating-point sınırı; condition/sezgisel kararlılık; log-sum-exp; initialization; yakınsama ölçütü | Olasılıksal modeller, optimizasyon, derin öğrenme | Kararsız ve kararlı hesaplamanın karşılaştırılması |

### Derste tekrar kullanılacak çekirdek denklemler

**Ampirik risk ve düzenlileştirme**

$$
\hat{R}(f)=\frac{1}{n}\sum_{i=1}^{n}L\bigl(y_i,f(x_i)\bigr),
\qquad
\hat{f}=\arg\min_f\left[\hat{R}(f)+\lambda\Omega(f)\right]
$$

**Bayes teoremi ve Naive Bayes faktörizasyonu**

$$
P(y\mid x)=\frac{P(x\mid y)P(y)}{P(x)},
\qquad
P(y\mid x_1,\ldots,x_d)\propto P(y)\prod_{j=1}^{d}P(x_j\mid y)
$$

Koşullu bağımsızlık bir gerçeklik iddiası değil, modeli hesaplanabilir ve veri-verimli yapan bir varsayımdır. Gaussian, Multinomial ve Bernoulli Naive Bayes veri türü ve likelihood seçimi üzerinden karşılaştırılır; sıfır olasılık için Laplace/additive smoothing gösterilir.

**Entropy, cross-entropy ve KL divergence**

$$
H(Y)=-\sum_y p(y)\log p(y)
$$

$$
H(p,q)=-\sum_y p(y)\log q(y),
\qquad
D_{KL}(p\Vert q)=\sum_y p(y)\log\frac{p(y)}{q(y)}
$$

Karar ağacında aday bölme, örneğin

$$
IG=H(\text{ebeveyn})-\sum_k\frac{n_k}{n}H(\text{çocuk}_k)
$$

ile değerlendirilir. Entropy ile Gini impurity aynı şey değildir; ikisinin bölme tercihleri ve hesap maliyetleri küçük bir veri üzerinde karşılaştırılır. Sınıflandırmadaki cross-entropy ise ağacın information gain hesabıyla ilişkili fakat aynı kullanım değildir.

**Gradient tabanlı güncelleme**

$$
\theta_{t+1}=\theta_t-\eta\nabla_\theta L(\theta_t)
$$

Bu eşitlik, doğrusal modelden backpropagation'a kadar ortak optimizasyon dili olarak kullanılır.

### Algoritma ailelerini birleştiren amaç fonksiyonları

Algoritmalar ayrı tarifler olarak değil, aşağıdaki ortak matematiksel kalıpların farklı veri/varsayım seçimleri olarak okunur:

| Aile | Temel amaç veya karar kuralı | Derste kurulacak bağlantı |
| --- | --- | --- |
| Least squares / Ridge / Lasso | $\min_\beta \lVert y-X\beta\rVert_2^2+\lambda\lVert\beta\rVert_p$ | Kayıp, geometri, regularization ve kapalı form/iteratif çözüm |
| Logistic / softmax | $\min_\theta -\sum_i \log P_\theta(y_i\mid x_i)$ | Bernoulli/categorical likelihood = negative log-likelihood = cross-entropy |
| k-NN | $\hat y(x)$ komşuların oyu veya yerel ortalamasıdır | Açık eğitim amacı yerine uzaklık, yerellik ve hafıza tabanlı tahmin |
| Naive Bayes / LDA / QDA | $\arg\max_y P(y)P(x\mid y)$ | Aynı Bayes karar kuralı, farklı sınıf-koşullu dağılım ve kovaryans varsayımları |
| Decision tree | En büyük impurity azalmasını veren greedy split | Entropy/Gini/MSE, yerel arama ve budama |
| SVM/SVR | Kayıp + margin'i büyüten norm cezası | Hinge/epsilon-insensitive loss, maksimum marjin ve kernel |
| Boosting | $F_m(x)=F_{m-1}(x)+\eta h_m(x)$ | Zayıf öğrenicilerin additive toplamı ve loss'un negatif gradyanı |
| k-means | $\min_{\mu,z}\sum_i\lVert x_i-\mu_{z_i}\rVert_2^2$ | Atama–merkez güncelleme, yerel optimum ve ölçek |
| PCA | $\max_{\lVert w\rVert=1}\operatorname{Var}(Xw)$ | Maksimum varyans ile minimum doğrusal reconstruction error eşdeğerliği |
| GMM/EM | $\max_\theta\sum_i\log\sum_k\pi_k p(x_i\mid z_i=k,\theta_k)$ | Gizli üyelik, soft assignment ve alternating optimization |
| Sinir ağı | Bileşik fonksiyonun loss'unu backpropagation ile küçültme | Chain rule, representation learning ve stochastic optimization |
| RL | $\max_\pi \mathbb E_\pi[\sum_t\gamma^t r_t]$ | Tahminden politikaya; exploration, delayed reward ve Bellman ilişkisi |

## II. Problem türüne göre algoritma haritası

### A. Denetimli öğrenme — regresyon

| Yöntem | Düzey | Matematiksel omurga | Özellikle öğretilecek sınır |
| --- | ---: | --- | --- |
| Mean/median ve basit kural baseline | T | Beklenti, robust merkez, kayıp | Baseline olmadan model artımı yorumlanamaz. |
| Basit/çoklu doğrusal regresyon | T | Matris çarpımı, least squares, MSE, normal equation sezgisi | Doğrusallık parametrelerdedir; korelasyon nedensellik değildir. |
| Polynomial ve etkileşimli regresyon | K | Basis expansion, kapasite | Özellik dönüşümü overfit ve extrapolation riski getirir. |
| Ridge, Lasso ve Elastic Net | T | L2/L1 cezası, konveks optimizasyon, sparsity | Ölçek ve korelasyon katsayı yorumunu değiştirir. |
| Robust ve quantile regression | K | Mutlak/Huber/pinball loss | Hedeflenen koşullu özet seçilen kayba bağlıdır. |
| GLM/Poisson regresyonu | P | Exponential family, link function, likelihood | Sayım hedefi ve variance varsayımı kontrol edilmelidir. |
| Bayesian linear regression | P | Prior, likelihood ve posterior predictive | Prior duyarlılığı ve belirsizlik yorumunun varsayıma bağlılığı. |
| Gaussian Process regression | P | Kernel covariance ve posterior predictive | Küçük/orta veri, kernel seçimi ve kübik ölçekleme sezgisi. |
| k-NN regression | K | Uzaklık, yerel ortalama, bias–variance | Ölçek, boyut ve komşu sayısına duyarlıdır. |
| Regression tree | T | Greedy split, MSE/variance reduction | Parçalı sabit tahmin ve kararsızlık. |
| Random forest regression | K | Bagging, bootstrap, feature subsampling | Daha düşük variance, daha yüksek maliyet ve sınırlı extrapolation. |
| Gradient boosting regression | K | Additive model, residual/negative gradient | Learning rate–ağaç sayısı etkileşimi ve overfit. |
| Support Vector Regression | K | Margin, epsilon-insensitive loss, kernel | Ölçek ve örnek sayısıyla hesap maliyeti. |

### B. Denetimli öğrenme — sınıflandırma

| Yöntem | Düzey | Matematiksel omurga | Özellikle öğretilecek sınır |
| --- | ---: | --- | --- |
| Majority/stratified ve kural baseline | T | Sınıf öncülü, beklenen hata/maliyet | Accuracy dengesiz sınıfta yanıltabilir. |
| Lojistik regresyon | T | Log-odds, sigmoid/softmax, Bernoulli/categorical likelihood, cross-entropy | Olasılık, threshold ve karar birbirinden ayrıdır. |
| k-NN classification | T | Mesafe, komşuluk, majority/weighted vote | Ölçek ve curse of dimensionality. |
| Gaussian/Multinomial/Bernoulli Naive Bayes | T | Bayes, koşullu bağımsızlık, likelihood, smoothing | Yanlış bağımsızlık varsayımına rağmen kararın neden çalışabileceği; calibration sınırı. |
| LDA ve QDA | K | Sınıf-koşullu Gaussian, kovaryans, Bayes karar sınırı | Ortak/ayrı kovaryans varsayımı ve küçük örnek sorunu. |
| Decision tree | T | Entropy, information gain, Gini, greedy bölme ve budama | Derinlik ile train/development farkı; yüksek variance. |
| Random forest / Extra Trees | K | Bagging, bootstrap, decorrelation | Importance yanlılığı ve hesap/yorum maliyeti. |
| AdaBoost | K | Ağırlıklı örnekler, additive learning, exponential loss sezgisi | Gürültü ve aykırı değerlere duyarlılık. |
| Gradient boosting; XGBoost/LightGBM/CatBoost ilkeleri | K | Functional gradient, shrinkage, regularization | Kütüphane adı algoritmik anlayışın yerine geçmez; validation overfit riski. |
| Linear ve kernel SVM | T/K | Maximum margin, hinge loss, dual/kernel sezgisi | Olasılık çıktısı doğal değildir; ölçek ve büyük-n maliyeti. |
| Perceptron ve MLP | T | Doğrusal eşik, aktivasyon, chain rule, backpropagation | Doğrusal ayrılabilirlik, optimizasyon ve calibration. |

Çok sınıflı problemler one-vs-rest, one-vs-one ve softmax üzerinden; çok etiketli problemler ise problem dönüşümü ve uygun metrikler üzerinden ayrıca ele alınır.

### C. Denetimsiz öğrenme — kümeleme

| Yöntem | Düzey | Matematiksel omurga | Özellikle öğretilecek sınır |
| --- | ---: | --- | --- |
| k-means / k-means++ | T | Öklid uzaklığı, within-cluster sum of squares, alternating optimization | Küresel kümeler, ölçek, başlangıç ve `k` seçimi. |
| Hiyerarşik kümeleme | T | Pairwise distance, single/complete/average/Ward linkage, dendrogram | Linkage seçimi farklı hiyerarşiler üretir. |
| DBSCAN | T | Yoğunluk komşuluğu, `eps`, `min_samples`, core/border/noise | Değişen yoğunluk ve yüksek boyut sorunu; küme sayısını önceden istemez. |
| HDBSCAN | P | Yoğunluk hiyerarşisi ve stability | Ek bağımlılık ve parametre yorumunun dikkatli yapılması. |
| Gaussian Mixture Model | T | Olasılıksal soft assignment, Gaussian likelihood, latent variable | Küme bileşeni gerçek sınıf değildir; kovaryans ve bileşen sayısı duyarlılığı. |
| Expectation–Maximization | T | Jensen/lower-bound sezgisi, E ve M adımları | Yerel optimum, initialization ve yakınsama. |
| Spectral clustering | K | Benzerlik grafı, graph Laplacian, eigenvectors | Benzerlik/ölçek seçimi ve büyük veri maliyeti. |

Küme değerlendirmesinde inertia tek başına kullanılmaz. Silhouette ve benzeri iç ölçüler; varsa dış etiket ölçüleri; stability; alan anlamı ve downstream fayda ayrı kanıtlar olarak raporlanır.

### D. Boyut indirgeme ve temsil öğrenme

| Yöntem | Düzey | Matematiksel omurga | Özellikle öğretilecek sınır |
| --- | ---: | --- | --- |
| PCA | T | Merkezleme, kovaryans, eigen-decomposition/SVD, explained variance | Ölçek, doğrusal yapı ve yüksek varyansın görev için önem anlamına gelmemesi. |
| Truncated SVD / LSA | K | Düşük rank approximation, sparse matrix | Merkezlenmemiş/sparse veri ve bileşen yorumu. |
| ICA | P | İstatistiksel bağımsızlık ve non-Gaussianity | Bileşen sırası/işareti ve kaynak ayrıştırma varsayımı. |
| t-SNE | P | Komşuluk olasılıkları ve KL divergence | İki boyutlu görselde küme görünmesi sınıf kanıtı değildir; global uzaklık korunmaz. |
| UMAP | P | Komşuluk grafı/manifold sezgisi | Parametre ve seed'e duyarlı keşif görselidir. |
| NMF | K | Non-negative low-rank factorization | Parça tabanlı temsil, initialization ve rank seçimi. |
| Autoencoder | K | Encoder–latent–decoder, reconstruction loss | İyi reconstruction, iyi downstream temsil veya anomali kanıtı değildir. |
| Embedding ve transfer learning | K | Benzerlik geometrisi, pretrained representation | Domain shift, lisans, privacy ve downstream evaluation. |

### E. Anomali/yenilik tespiti

| Yöntem | Düzey | Matematiksel omurga | Özellikle öğretilecek sınır |
| --- | ---: | --- | --- |
| Basit istatistiksel eşik / robust z-score | T | Ortalama/standart sapma, median/MAD, quantile | Çok değişkenli yapı ve dağılım varsayımı. |
| Isolation Forest | K | Rastgele bölmeler ve beklenen yol uzunluğu | Skor eşiği/contamination bir karar varsayımıdır. |
| Local Outlier Factor | K | Yerel yoğunluk oranı | Novelty ile outlier kullanım kiplerinin farkı. |
| One-Class SVM | K | Kernel sınırı ve support estimation | Ölçek, `nu`/kernel ve büyük veri maliyeti. |
| GMM density / reconstruction error | K | Likelihood veya reconstruction loss | Düşük likelihood/error ile gerçek operasyonel anomali aynı değildir. |

Etiket yokluğu değerlendirme yokluğu anlamına gelmez: sentetik enjeksiyon, uzman incelemesi, zaman içi backtest ve alarm bütçesi birlikte tasarlanır.

### F. Sinir ağları ve derin öğrenme

| Yöntem | Düzey | Matematiksel omurga | Özellikle öğretilecek sınır |
| --- | ---: | --- | --- |
| Perceptron/tek nöron ve MLP | T | Affine dönüşüm, aktivasyon, chain rule, backprop, SGD/Adam | Initialization, learning rate, overfit ve küçük veride güçlü baseline. |
| CNN | K | Convolution, weight sharing, receptive field | Görüntü/ızgara varsayımı, augmentation ve veri maliyeti. |
| RNN, LSTM ve GRU | P | Recurrent state, backpropagation through time, gating | Vanishing/exploding gradient ve uzun bağımlılıklar. |
| Attention ve Transformer | K/P | Query–key–value, scaled dot-product, positional information | Quadratic attention maliyeti, veri/hesap ve bağlam sınırı. |
| Transfer learning/fine-tuning | K | Representation reuse, optimization ve regularization | Domain shift, catastrophic forgetting ve evaluation leakage. |

### G. Zaman serisi ve sıralı veri

| Yöntem | Düzey | Matematiksel omurga | Özellikle öğretilecek sınır |
| --- | ---: | --- | --- |
| Naive/seasonal naive ve moving-average baseline | T | Zaman indeksi, trend/mevsimsellik | Rastgele split kullanılmaz. |
| Lag/rolling özellikli linear ve tree/boosting modelleri | K | Otokorelasyon, feature window, supervised dönüşüm | Gelecek bilgisi ve rolling-feature leakage. |
| Exponential smoothing ve ARIMA fikri | P | Stationarity, differencing, autocorrelation | İstatistiksel tahmin baseline'ı olarak konumlandırılır. |
| Hidden Markov Model | P | Gizli durum, transition/emission olasılıkları ve forward–backward/Viterbi fikri | Durum sayısı ve stationarity/Markov varsayımları. |
| RNN/LSTM/Transformer tahmini | P | Sequence representation ve çok-adımlı loss | Karmaşıklık basit zaman-serisi baseline'ına karşı kanıtlanır. |

Değerlendirme expanding/rolling window backtest ile yapılır; horizon bazında hata ve değişen rejimler raporlanır.

### H. Öneri sistemleri ve örüntü madenciliği

| Yöntem | Düzey | Matematiksel omurga | Özellikle öğretilecek sınır |
| --- | ---: | --- | --- |
| Popularity ve recency baseline | T | Frekans, zaman ve exposure | Popülerlik yanlılığı. |
| Content-based ve user/item k-NN collaborative filtering | K | Cosine similarity, sparse interaction matrix | Cold start ve exposure bias. |
| Matrix factorization | K | Düşük rank, dot product, regularized loss | Missing-not-at-random ve implicit feedback. |
| Apriori / FP-Growth | P | Support, confidence ve lift | Birliktelik nedensellik değildir; çoklu arama sahte örüntü üretir. |

Offline ranking metrikleri (`precision@k`, `recall@k`, MAP/NDCG) kullanıcı değeriyle ve çevrimiçi deneyle aynı şey değildir.

### I. Yarı denetimli, öz-denetimli ve üretici öğrenme

| Yöntem | Düzey | Matematiksel omurga | Özellikle öğretilecek sınır |
| --- | ---: | --- | --- |
| Pseudo-labeling / self-training | P | Confidence threshold ve iterative fitting | Hatalar pekişebilir; test verisi etiket kaynağı değildir. |
| Label propagation | P | Benzerlik grafı ve smoothness varsayımı | Graf yapısı sınıf yapısını temsil etmeyebilir. |
| Contrastive/self-supervised temsil | P | Positive/negative pairs ve contrastive loss | Augmentation seçimi semantiği belirler. |
| Latent Dirichlet Allocation (topic model) | P | Dirichlet prior, latent topic mixture ve approximate inference | Buradaki LDA'nın Linear Discriminant Analysis olmadığını; topic sayısı ve yorumun varsayıma bağlılığını ayırır. |
| Autoregressive model, VAE, GAN ve diffusion | P | Likelihood/ELBO, adversarial objective, denoising | Kalite, çeşitlilik ve güvenlik ayrı değerlendirmelerdir. |

### J. Pekiştirmeli öğrenme ve ardışık karar

| Yöntem | Düzey | Matematiksel omurga | Özellikle öğretilecek sınır |
| --- | ---: | --- | --- |
| Multi-armed bandit; epsilon-greedy/UCB fikri | T | Beklenen ödül, exploration–exploitation, regret | Eylem gözlenecek veriyi değiştirir. |
| Markov Decision Process | T | State, action, transition, reward, return, discount | Markov varsayımı ve ödül tasarımı. |
| Bellman denklemi; value/policy iteration | T/P | Dynamic programming ve fixed point | Model bilgisi ve state-space maliyeti. |
| Q-learning | K | Temporal-difference target ve off-policy update | Exploration, instability ve offline veri riski. |
| Policy gradient / actor–critic | P | Beklenen getiri gradyanı ve variance | Örnek verimsizliği, güvenlik ve reward hacking. |

RL başarısı yalnız toplam ödülle raporlanmaz; regret, güvenlik kısıtları, varyans ve politika değişiminin veri üretimine etkisi değerlendirilir.

## III. Algoritmalar arasında seçim dili

Öğrenci “en iyi algoritma” demez; aşağıdaki eksenlerde probleme uygun seçim yapar:

| Eksen | Sorulacak soru |
| --- | --- |
| Hedef | Sürekli değer, sınıf, sıralama, küme, yoğunluk, temsil veya eylem mi? |
| Etiket | Etiket var mı; güvenilir mi; seçim/ölçüm yanlılığı taşıyor mu? |
| Veri geometrisi | Doğrusal, yerel, ağaçla bölünebilir, yoğunluk-temelli, sıralı veya yüksek boyutlu mu? |
| Varsayım | Bağımsızlık, Gaussianlık, ortak kovaryans, mesafe, stationarity veya Markov varsayımı makul mü? |
| Kayıp ve çıktı | Nokta tahmini, olasılık, quantile, ranking, density veya politika mı gerekiyor? |
| Ölçek | `n`, özellik sayısı, sparsity, bellek, eğitim ve inference bütçesi nedir? |
| Risk | Calibration, açıklanabilirlik, adalet, gizlilik, güvenlik ve insan gözetimi gereksinimi nedir? |
| Değerlendirme | Random/group/time split, metric, belirsizlik ve baseline gerçek kullanımı temsil ediyor mu? |

## IV. 14 haftalık yerleşim

| Hafta | Ana matematik | Algoritma/problem aileleri | Beklenen düzey |
| ---: | --- | --- | --- |
| 1 | Tahmin, kayıp, baseline ve genelleme dili | Uçtan uca sınıflandırma vakası; bütün ailelerin haritası | Panorama |
| 2 | Vektör/matris, olasılık, Bayes, türev ve ampirik risk temeli | Dummy/kural baseline ve sızıntısız pipeline | T |
| 3 | MSE, likelihood, cross-entropy, entropy/Gini, margin | Linear/logistic, Naive Bayes, k-NN, tree; RF/boosting/SVM karşılaştırması | İlk sistematik tur |
| 4 | Chain rule, gradient, temsil geometrisi ve beklenen ödül | MLP, PCA/k-means, bandit; modern aileler panoraması | İlk sistematik tur |
| 5 | Karar teorisi ve veri üretim süreci | Problem tipleri, baseline ailesi, hata maliyeti | T |
| 6 | Linear algebra, probability/statistics, information theory, optimization ve öğrenme kuramı | Küçük elle hesaplar; matematik–algoritma bağlantıları | T |
| 7 | Uzaklık, dönüşüm, örnekleme ve doğrulama | k-NN; preprocessing; random/group/time split | T/K |
| 8 | Least squares, likelihood/MAP, Bayes, covariance ve calibration | Linear/Ridge/Lasso/Elastic Net; logistic; Naive Bayes; LDA/QDA; GLM panoraması | T/K |
| 9 | Entropy, information gain, Gini, margin ve additive optimization | Trees; RF/Extra Trees; AdaBoost/gradient boosting; SVM/SVR | T/K |
| 10 | Sampling distributions, bootstrap, karar eşiği ve ranking | Regresyon/sınıflandırma/ranking metrikleri; belirsizlik ve açıklama | T/K |
| 11 | Chain rule, backpropagation ve stochastic optimization | Perceptron/MLP; CNN; sequence/attention panoraması | T/K/P |
| 12 | Eigen/SVD, mesafe, density, latent variables ve EM | PCA/SVD/NMF; k-means, hierarchical, DBSCAN, GMM; anomaly; recommender/association ve generative panorama | T/K/P |
| 13 | Beklenen getiri, regret, Markov ve Bellman | Bandit, MDP, Q-learning; policy gradient panoraması; causality sınırı | T/K/P |
| 14 | Dağılım kayması, risk ve operasyonel istatistik | Model seçimi, izleme, yeniden eğitim/rollback; bütün ailelerin seçim savunması | Bütünleştirme |

## V. Asgari mezuniyet kanıtı

Dersi tamamlayan öğrenci:

- bir regresyon, bir sınıflandırma ve bir denetimsiz öğrenme problemini doğru biçimde ayırır;
- linear/logistic regression, Naive Bayes, k-NN, decision tree, k-means, DBSCAN, PCA, temel MLP ve bandit için çekirdek matematiksel mekanizmayı açıklar;
- regularized linear model, ağaç/ensemble, SVM, en az iki kümeleme ve en az bir anomali yöntemini sabit protokolde karşılaştırır;
- entropy, cross-entropy, KL divergence, likelihood, Bayes, information gain, Gini, margin, gradient, bias–variance ve regularization kavramlarını doğru algoritmaya bağlar;
- bir yöntemi yalnız skoru nedeniyle değil, varsayımı, veri geometrisi, belirsizliği, maliyeti ve kullanım riskiyle seçer;
- görmediği yeni bir algoritmayı problem türü, temsil, amaç fonksiyonu, optimizasyon ve değerlendirme eksenlerinde sınıflandırır.
