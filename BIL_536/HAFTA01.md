# BİL 536 01 Makine Öğrenmesi — 1. Hafta Öğretim Dosyası

**Hafta:** Panorama — yüksek skor neden yeterli değildir?  
**Dönem yapısı:** 14 hafta, `1 + 3 + 10`  
**Süre:** 155 dakika ders; 125 dakika etkin çalışma + üç adet 10 dakikalık ara  
**Dönem ürünü:** Yeniden Üretilebilir Makine Öğrenmesi Çalışması (`ML Evidence Lab`)  
**Ortak vaka:** Sentetik öngörücü bakım — sonraki 24 saatte arıza riski  
**Kapsam:** Ç1–Ç10 panoraması; ayrıntılı teori ve algoritma türetimi daha sonraki haftalara bırakılır  
**İlgili belgeler:** [Dersin ana README dosyası](README.md) · [Matematik ve algoritma kapsam matrisi](KAPSAM_MATRISI.md)

Bu Türkçe belge eğitmen içindir. Öğrenci yönergeleri gerektiğinde ayrı ve kısaltılmış olarak yayımlanır. W1 tanılayıcıdır; teslimler geri bildirim sağlar ve yeni bir not bileşeni oluşturmaz. Kurulum aksarsa eşli çalışma veya eğitmenin sağladığı sabit çıktılar üzerinden analiz kabul edilir; kişisel ortam daha sonra tamamlanır.

## Haftanın ana sorusu

> Bir modelin test skoru çok yüksekse, onun doğru problemi öğrendiğine ve gerçek kullanımda güvenilir olduğuna karar verebilir miyiz?

İlk haftanın amacı on çapayı bitirmek değildir. Amaç, öğrencinin bir ML iddiasının problem tanımı, veri üretim süreci, split, baseline, model, metrik, belirsizlik, risk ve yeniden üretilebilirlik zincirinden oluştuğunu görmesidir.

Öğrenci her gözlemi şu üç etiketten biriyle işaretler:

- **G — Gözlendi:** Kod veya çıktı üzerinde doğrudan görüldü.
- **M — Modellendi:** Şema, denklem veya düşünce deneyiyle temsil edildi.
- **F — Gelecekte inşa edilecek:** Bu hafta yalnızca yeri gösterildi.

## 14 haftalık haritadaki yeri

```text
1. hafta       : Ç1–Ç10 panoraması
2–4. haftalar  : ilk sistematik tur
                 W2 Ç1–Ç3 · W3 Ç4–Ç6 · W4 Ç7–Ç10 + v0.1
5–14. haftalar : Ç1–Ç10 için on bilinçli derinleşme ve ürün artımı
```

## Ders sonunda öğrencinin göstereceği kanıt

Öğrenci:

- tahmin problemi ile karar problemini ayırır;
- gözlem birimi, hedef, tahmin anı ve tahmin ufkunu söyler;
- majority baseline ile öğrenen modeli aynı protokolde karşılaştırır;
- olaydan sonra oluşan bir alanın neden target/temporal leakage yarattığını gösterir;
- rastgele split ile zaman/makine grubu duyarlı split'in farklı iddiaları sınadığını açıklar;
- preprocessing adımlarının neden yalnız training fold üzerinde öğrenilmesi gerektiğini belirtir;
- accuracy'nin dengesiz sınıfta neden tek başına yetersiz olabileceğini gösterir;
- olasılık skoru, karar eşiği ve kalibrasyonun aynı şey olmadığını fark eder;
- yüksek performansın adalet, güvenlik, yeniden üretilebilirlik veya üretim uygunluğu anlamına gelmediğini söyler;
- Ç1–Ç10 için G/M/F haritası, temiz bir deney çıktısı ve kısa kanıt–sınır açıklaması teslim eder.

## Eğitmenin ders öncesi hazırlığı

### Ortam ve starter

- Desteklenen Python sürümü ve kilitli bağımlılıklarla temiz CPU ortamını doğrula.
- Starter'ın tek komutla üç koşulu çalıştırdığını kontrol et:
  1. `dummy_majority` — çoğunluk sınıfı baseline'ı,
  2. `leaky_random` — olay-sonrası alanı içeren rastgele split,
  3. `clean_group_time` — olay-sonrası alanı çıkarılmış, zaman ve makine kimliğini gözeten split.
- En az bir işletim sisteminde temiz kurulum provası yap; kurulum sorunu için sabit `metrics.json`, confusion matrix ve deney günlüğü çıktıları hazır tut.
- Bütün Core deneyleri CPU üzerinde ders içinde birkaç dakika içinde tamamlanacak büyüklükte tut.

### Sentetik veri sözleşmesi

Her satır, belirli bir makinenin belirli bir ölçüm zamanındaki durumunu temsil eder.

Önerilen alanlar:

| Alan | Anlam | Kullanım notu |
| --- | --- | --- |
| `machine_id` | Makine/grup kimliği | Aynı makinenin iki split'e sızmasını tartışmak için |
| `timestamp` | Ölçüm zamanı | Geleceğin geçmişe karışmasını engellemek için |
| `temperature_c` | Sıcaklık | Model girdisi |
| `vibration_rms` | Titreşim | Model girdisi |
| `pressure_bar` | Basınç | Model girdisi |
| `load_pct` | Yük yüzdesi | Model girdisi |
| `shift` | Vardiya | Kategorik model girdisi |
| `hours_since_service` | Son bakımdan beri süre | Model girdisi |
| `repair_code_after_event` | Olaydan sonra girilen tamir kodu | Bilerek eklenmiş leakage alanı; temiz modelde yasak |
| `failure_next_24h` | Sonraki 24 saatte arıza | İkili hedef |

- Veri tamamen sentetik olmalı; gerçek çalışan, müşteri veya kurum kaydı kullanılmamalı.
- Pozitif sınıfı azınlık yap; ancak kesin sınıf oranını dersten önce öğrenciye söyleme.
- `repair_code_after_event` alanını hedefle güçlü ilişkili üret; sütun adını ilk tahminden sonra görünür kıl.
- Aynı makinenin yakın zaman kayıtlarında benzerlik bulunmasını sağla; random split'in aşırı iyimser olabilmesini mümkün kıl.
- Sabit bir veri sürümü ve SHA-256 özeti yayımla.

### Gösterim ve güvenlik

- Confusion matrix, precision–recall, calibration ve zaman sıralı performans için küçük hazır grafikler bulundur.
- `seed` sabitlenmiş ve sabitlenmemiş iki kısa çıktıyı karşılaştırmaya hazır ol.
- Kaynak/dataset lisansı, veri sözleşmesi ve AI kullanım açıklaması için boş şablon hazırla.
- İlk hafta gerçek deployment veya yüksek riskli karar otomasyonu yapma; yalnız yaşam döngüsündeki yerini göster.

## Ders öncesi öğrenci hazırlığı — 30–40 dakika

1. Verilen kısa “supervised learning, feature, label, train/test” okumasını tamamla.
2. Aşağıdaki soruları nota ve AI'ya bakmadan yanıtla:
   - %95 accuracy her zaman güçlü bir model midir?
   - Test verisinde iyi çalışan sütun mutlaka geçerli bir özellik midir?
   - Rastgele `train_test_split` her veri için uygun mudur?
   - Aynı seed aynı sonucu garanti eder mi?
   - 0,80 tahmin olasılığı olayın gerçekten %80 sıklıkta gerçekleştiğini kanıtlar mı?
3. `python --version` ve starter'ın `--help` komutunu doğrula.
4. Kurulum tamamlanmadıysa hata metnini ve denediğin tek çözümü kaydet; gizli anahtar veya kişisel sistem bilgisi paylaşma.

## Tahta/slayt omurgası

Derste aynı şema görünür kalır:

```text
İddia
 ├─ Hangi karar ve kim için?                  Ç1
 ├─ Hangi varsayım ve genelleme?             Ç2
 ├─ Hangi veri, split ve dönüşüm?             Ç3
 ├─ Hangi basit/olasılıksal model?            Ç4
 ├─ Hangi doğrusal olmayan alternatif?        Ç5
 ├─ Hangi metrik, belirsizlik ve hata?        Ç6
 ├─ Hangi öğrenilmiş temsil/optimizasyon?     Ç7
 ├─ Etiketsiz/yüksek boyutlu veri ne olacak?  Ç8
 ├─ Tahmin eylemi ve geleceği etkiler mi?      Ç9
 └─ Risk, yeniden üretim ve üretim yaşamı?     Ç10
```

Altına ders boyunca şu kanıt zinciri eklenir:

```text
tahmin → çalıştır → gözle → karşılaştır → açıkla → sınırlandır → aktar
```

## 155 dakikalık ders akışı

| Süre | İçerik ve öğretmen hamlesi | Öğrenci işi / toplanan kanıt |
| --- | --- | --- |
| 00–08 | Vaka: “Yarın arızalanacak makineyi durdurmalı mıyız?” Teknik tahmin ile bakım kararını ayır. | Bireysel karar ve hata maliyeti tahmini |
| 08–15 | Sınıf oranını göster; `dummy_majority` sonucunu çalıştır. “Accuracy iyi görünüyor mu?” | Baseline yorumu ve şüphe notu |
| 15–23 | `leaky_random` sonucu: olağanüstü yüksek skor. Önce alkışlat, sonra hangi bilgiyle öğrenildiğini sordur. | En yüksek skorun geçerliliğine ilişkin tahmin |
| 23–30 | **Ç1–Ç2:** hedef/ufuk/baseline; ampirik risk, genelleme ve varsayım panoraması. | Problem cümlesi + bir genelleme varsayımı |
| 30–40 | **Ara** | |
| 40–50 | **Ç3:** sütun zaman çizgisi, target/temporal leakage ve random/group/time split. Olay-sonrası sütunu açığa çıkar. | Leakage kaynağını işaretleme |
| 50–58 | Temiz pipeline'ı çalıştır; preprocessing'in training fold içinde fit edildiğini göster. | Sızıntılı–temiz sonuç karşılaştırması |
| 58–65 | **Ç4:** logistic modelin skor/olasılık üretmesi; regularization ve karar eşiğine kısa giriş. | Eşik değişince beklenen FP/FN yönü |
| 65–70 | **Ç5:** tree/ensemble/kernel ailelerini aynı protokolde seçenek olarak yerleştir; “daha karmaşık = daha geçerli” iddiasını sorgula. | Bir model ailesi + beklenen bedel |
| 70–80 | **Ara** | |
| 80–90 | **Ç6:** confusion matrix, precision/recall, PR eğrisi, calibration ve hata dilimleri. | Karar bağlamına uygun birincil metrik |
| 90–98 | **Ç7:** küçük MLP preview; loss, gradient ve backprop zincirini tek şekille göster. Aynı veride otomatik üstünlük iddia etme. | Klasik baseline'ın korunma gerekçesi |
| 98–104 | **Ç8:** PCA/embedding/attention/generative learning'i “temsil öğrenme” ortak sorusuna yerleştir. | G/M/F etiketi ve ertelenen soru |
| 104–110 | **Ç9:** bakım kararının sonraki veriyi değiştirdiği feedback loop; prediction ile intervention ayrımı. | Bir geri besleme oku |
| 110–120 | **Ara** | |
| 120–129 | **Ç10:** data/model card, seed sınırı, veri/kod/ortam kaydı, drift, monitoring, rollback ve insan gözetimi. | Minimum yeniden üretim kaydı |
| 129–143 | Stüdyo: ekipler sızıntılı yapılandırmayı temizler, uygun split'i seçer, baseline ve logistic modeli yeniden çalıştırır. | `metrics.json` + düzeltilen gerekçe |
| 143–150 | Ekipler bir iddiayı `iddia–kanıt–sınır` formatında savunur; başka ekip karşı örnek sorar. | Bir savunma ve bir karşı örnek |
| 150–155 | Bireysel Ç1–Ç10 G/M/F haritası ve exit ticket. | Çıkış kaydı |

## On çapanın ilk hafta için doğru derinliği

| Çapa | Bu hafta göster | Bu hafta ertele / iddia etme |
| --- | --- | --- |
| Ç1 | Tahminin bir kullanım anı, eylemi, baseline'ı ve hata maliyeti vardır. | Problem tanımının model seçildikten sonra yapılması |
| Ç2 | Eğitim başarısı genelleme garantisi değildir; kapasite ve regularization etkiler. | Bütün genelleme kuramı ve ayrıntılı ispatlar |
| Ç3 | Split veri üretim sürecini temsil etmeli; öğrenilen dönüşümler training fold'da fit edilmelidir. | Random split'i evrensel varsayılan sayma |
| Ç4 | Logistic model skor/olasılık üretir; eşik ayrı karardır. | Katsayıyı nedensel etki veya ham skoru kalibre olasılık sayma |
| Ç5 | Ağaç/kernel/ensemble farklı varsayım ve maliyet taşır. | En karmaşık veya en yüksek skorlu modeli otomatik seçme |
| Ç6 | Metrik karar bağlamına bağlıdır; belirsizlik, calibration ve hata dilimi gerekir. | Tek test skoru üzerinden geniş genelleme |
| Ç7 | Sinir ağı temsil ve karar fonksiyonunu birlikte öğrenebilir; optimizasyon izlenmelidir. | Derin modelin küçük tablo verisinde otomatik üstünlüğü |
| Ç8 | PCA, embedding, attention ve generative yöntemler temsil sorusunun farklı yanlarıdır. | Bir haftada bu ailelerin uygulama yeterliği |
| Ç9 | Eylem gelecekteki veriyi değiştirebilir; tahmin nedensel müdahale değildir. | Gözlemsel başarıdan müdahale etkisi çıkarma |
| Ç10 | Risk, yeniden üretim, deployment ve monitoring model yaşam döngüsünün parçasıdır. | Seed'in tam yeniden üretim veya deployment'ın son aşama olduğu iddiası |

## Stüdyo görevi: sızıntısız en küçük deney

Öğrencilere çalışan fakat bilerek hatalı bir yapılandırma verilir. Görev yeni bir algoritma yazmak değil, deney iddiasını geçerli hâle getirmektir.

### Yapılacaklar

1. `repair_code_after_event` alanını model girdilerinden çıkar.
2. Tahmin anından sonra oluşan başka alan olup olmadığını veri sözlüğünde kontrol et.
3. Aynı makinenin yakın kayıtlarını iki tarafa dağıtmayan, zamanı gözeten verilen split seçeneğini kullan.
4. İmputation, encoding ve scaling adımlarını modelle aynı pipeline içine al.
5. `DummyClassifier` ile logistic modeli aynı split ve metriklerde çalıştır.
6. Accuracy yanında pozitif sınıf için precision, recall, F1 ve confusion matrix kaydet.
7. Deney yapılandırması, veri özeti, split kimliği, seed ve paket sürümlerini `run.json` içine yaz.
8. Sonucu aşağıdaki üç satırla açıkla:

```text
İddia   : Bu deney hangi sınırlı iddiayı destekliyor?
Kanıt   : Hangi karşılaştırma ve çıktı bunu destekliyor?
Sınır   : Bu deney neyi göstermiyor; hangi risk açık kalıyor?
```

### Core kabul koşulları

- Leakage alanı temiz model girdilerinde yoktur.
- Preprocessing split'ten önce bütün veri üzerinde fit edilmez.
- Baseline ve logistic model aynı test protokolünü kullanır.
- `metrics.json` ve `run.json` üretilebilir.
- İddia test skorundan daha geniş değildir.
- Öğrenci yapılan iki düzeltmeyi sözlü açıklayabilir.

### Stretch

- Karar eşiğini yanlış negatif/yanlış pozitif maliyetine göre değiştir ve iki confusion matrix karşılaştır.
- Makine yaşı veya vardiya için hata dilimi oluştur; az örnekli dilimlerde kesin hüküm verme.
- Üç farklı seed çalıştırıp değişkenliği raporla; bunu güven aralığıyla eşitleme.

## Sorulacak kritik sorular ve beklenen yön

- **%95 accuracy neden kötü olabilir?** Pozitif sınıf %5 ise hiç arıza tahmin etmeyen baseline aynı değere ulaşabilir; karar maliyeti ve sınıf-odaklı metrik gerekir.
- **Sızıntılı model geleceği gerçekten mi öğrendi?** Hayır; tahmin anında mevcut olmayan olay-sonrası bilgiyi kullanarak değerlendirme protokolünü ihlal etti.
- **Rastgele split neden sorun olabilir?** Aynı makinenin veya yakın zamanların benzer kayıtları iki tarafa geçebilir; gerçek gelecek/makine genellemesini temsil etmeyebilir.
- **Ölçekleyiciyi bütün veride fit etmek küçük bir ayrıntı mı?** Hayır; test dağılımından öğrenilmiş parametreler değerlendirmeye bilgi taşır.
- **En yüksek validation skoruna sahip model finalde en iyisi midir?** Ancak protokol önceden belirlenmiş, seçim validation katında kalmış ve test yalnız son değerlendirmede kullanılmışsa sınırlı biçimde savunulabilir.
- **0,80 skoru kalibre olasılık mıdır?** Kendiliğinden değildir; benzer skorlu örneklerin gerçekleşme sıklığı calibration analiziyle sınanır.
- **SHAP veya feature importance neden nedensellik göstermez?** Modelin veri üzerindeki tahmin davranışını açıklar; müdahale etkisini kanıtlamaz.
- **Seed sabitse deney tamamen yeniden üretilebilir mi?** Paket, donanım, algoritma ve nondeterministic işlemler de sonucu etkileyebilir.
- **Model yayına alındığında iş biter mi?** Hayır; giriş/çıktı dağılımı, kalite, gecikme, hata ve insan etkisi izlenmeli; durdurma/rollback yolu olmalıdır.
- **Hassas özellik yoksa adalet sorunu yok mudur?** Proxy değişkenler, örnekleme, etiket kalitesi ve farklı hata oranları yine sorun yaratabilir.

## Yaygın yanılgılar ve müdahale

- “Skor yükseldi, model iyileşti.” → Aynı split, aynı metrik, baseline, belirsizlik ve veri kullanılabilirlik zamanını sordur.
- “Train ve test ayrı dosyaysa leakage yoktur.” → Dönüşümlerin nerede fit edildiğini, duplicate/grup/zaman ilişkilerini çizdir.
- “Accuracy nesnel metriktir.” → Farklı yanlış karar maliyetleriyle aynı confusion matrix'i yeniden değerlendir.
- “Cross-validation test setini gereksiz yapar.” → Model seçimi ile nihai tarafsız değerlendirme rollerini ayır.
- “Daha fazla feature her zaman iyidir.” → Tahmin anında bulunmayan, pahalı, kararsız veya proxy alanları işaretlet.
- “Deep learning modern olduğu için üstündür.” → Aynı protokolde dummy, logistic, tree ve küçük MLP'yi maliyetle birlikte karşılaştır.
- “Açıklanabilir model adildir.” → Yorumlanabilirlik ile adalet, gizlilik ve nedenselliğin ayrı sorular olduğunu göster.
- “Notebook bende çalışıyor; yeniden üretilebilir.” → Temiz ortam, tek komut, veri/ortam kimliği ve kayıtlı çıktı iste.
- “Sentetik veri risksizdir.” → Sentetik üretim varsayımlarının gerçeği temsil etmeme ve sahte güven üretme riskini eklet.

## Hafta sonu tanılayıcı teslim paketi

1. Ç1–Ç10 için `G / M / F + kanıt veya gelecek soru` haritası.
2. Temiz Core deneyinden `metrics.json` ve `run.json`.
3. Baseline ile logistic modeli karşılaştıran küçük bir tablo.
4. İşaretlenmiş veri zaman çizgisi: hangi sütun tahmin anında kullanılabilir, hangisi değildir?
5. En fazla 180 kelimelik `iddia–kanıt–sınır` açıklaması.
6. En fazla 90 saniyelik bireysel sözlü/video açıklama veya derste canlı kontrol.
7. AI kullanıldıysa araç, amaç, kabul/reddedilen öneri ve doğrulama yöntemi.

Teslimin amacı kod miktarını ölçmek değildir. Kurulum sorunu yaşayan öğrenci 2. ve 3. maddelerde eğitmenin sabit çıktısını kullanabilir; hangi adımı kendisinin çalıştıramadığını açıkça belirtir.

## Exit ticket

Öğrenci notsuz ve bireysel yanıtlar:

1. Bugünkü en yüksek skor neden en güvenilir kanıt değildi?
2. Bu vakada gözlem birimi, hedef ve tahmin ufku nedir?
3. Random split hangi gerçek kullanım sorusunu yanlış temsil edebilir?
4. Birincil metriği seçmek için hangi karar maliyetini bilmek gerekir?
5. Ç1–Ç10'dan hangisi şu anda en belirsiz ve 2–4. haftalarda hangi kanıtı görmek istersin?

## 2. haftaya köprü

2. haftaya başlangıç cümlesi:

> **Panoramada gördüğümüz yaşam döngüsünün ilk üç çapasıyla kendi araştırma problemimizi, veri üretim sürecimizi ve sızıntısız değerlendirme sözleşmemizi kuracağız; henüz karmaşık model seçmeyeceğiz.**

Öğrenci 2. haftaya iki aday problemle gelir. Her aday için yalnız şu beş satırı yazar: karar sahibi, gözlem birimi, tahmin anı, hedef/ufuk ve yanlış kararın olası bedeli. Veri veya model seçimi bu beş satır netleşmeden kesinleştirilmez.
