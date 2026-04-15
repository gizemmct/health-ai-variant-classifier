# health-ai-variant-classifier

Branch: veri-on-isleme

Görev: Gelen ham veri matrisinin algoritmik hatalardan arındırılması, model değerlendirme altyapısının kurulması ve referans metriklerin oluşturulması.

Kullanılacak Kütüphaneler: scikit-learn (KNNImputer, RobustScaler, MinMaxScaler, IsolationForest, StratifiedKFold, RandomForestClassifier, LogisticRegression), imbalanced-learn (SMOTE).

Veri Setine Uygulanışı:
Eksik Veri (NaN) ve Normalizasyon: Kaggle veri setindeki sayısal özniteliklerin (popülasyon frekansları vb.) normalizasyonunu (Robust/Min-Max Scaler) yapıp, eksik değerleri KNNImputer algoritması ile mesafe temelli olarak doldurur.

Aykırı Değer Filtresi: Özellik uzayındaki anormal dağılımları (outliers) tespit etmek için IsolationForest algoritmasını kurgular ve tutarsız varyantların ağırlıklarını düşürür.

Dengesizlik Çözümü (SMOTE): Filtreleme sonrası azınlıkta kalacak sınıfların model tarafından ezberlenmesini önlemek için Imbalanced-Learn kütüphanesinden SMOTE (Sentetik Azınlık Aşırı Örnekleme) algoritmasını uygular.

Çapraz Doğrulama ve Benchmark: Sınıf dağılımlarının sabit kalması için StratifiedKFold ile 5-katmanlı veri bölme altyapısını kodlar. Ana mimariye kıyaslama oluşturması için RandomForestClassifier ve LogisticRegression temel modellerini kurarak ilk referans metrikleri çıkarır.

Branch: biyoinformatik-temsilleme

Görev: Veri setindeki biyolojik ham bilgilerin (string/metin), makine öğrenmesi modellerinin işleyebileceği matematiksel vektörlere dönüştürülmesi ve takımın genel koordinasyonu.

Kullanılacak Kütüphaneler: pandas, numpy, scikit-learn (OneHotEncoder).

Veri Setine Uygulanışı:
Sekansların Vektörizasyonu: Şartnamede belirtilen formattaki ±5 nükleotid komşuluğu verilerini NLP tabanlı K-mer frekans vektörlerine; ±5 amino asit komşuluğu bilgisini ise kategorik formattan sayısal formata geçirmek için One-Hot Encoding matrislerine dönüştürür.

Fizikokimyasal Şok Skoru: Veri setindeki varyant öncesi ve sonrası amino asitlerin "hidrofobiklik (sudan kaçma) indeksi" ve "moleküler hacim" farklarının mutlak değerini matematiksel olarak hesaplayarak modele sürekli (continuous) bir değişken olarak ekler.

Polarite Değişim Etiketi: Nötr amino asitlerin artı/eksi yüklü amino asitlere dönüşümünü tespit eden ikili (1/0) algoritmaları yazar.

Kodon Kullanım Sıklığı (Codon Usage Bias): Eski ve yeni kodonların vücutta bulunma sıklığı farkını literatürden çekerek (veya veri setindeki kodon tablosundan hesaplayarak) yeni bir öznitelik olarak entegre eder.

Branch: model-optimizasyon

Görev: Veri boru hattı (pipeline) tamamlanmış matrisler üzerinde, projenin en yüksek başarımlı ana tahminleme modellerinin inşası ve şeffaflaştırılması.

Kullanılacak Kütüphaneler: xgboost, lightgbm, optuna, scikit-learn (CalibratedClassifierCV), shap.

Veri Setine Uygulanışı:
Hiperparametre Optimizasyonu: Sınıflandırma problemi için ana mimari olarak XGBClassifier (XGBoost) ve LGBMClassifier (LightGBM) algoritmalarını kurgular. Ağaç algoritmalarının parametrelerini (maksimum derinlik, öğrenme oranı, L2 katsayısı, özellik alt-örneklemesi) Optuna framework'ü üzerinden Bayes Optimizasyonu ile tarar.

Olasılık Kalibrasyonu (Label Shift): Test setinde oluşabilecek dağılım kaymasını (Label Shift) yönetmek ve Makro F1 skorunu maksimize etmek amacıyla, Isotonic Regression (CalibratedClassifierCV) kullanarak modelin olasılık (probability) çıktılarını kalibre eder.

SHAP Entegrasyonu (Şeffaflık): Jürinin şeffaflık kısıtını karşılamak üzere XGBoost modeline shap.TreeExplainer algoritmasını entegre eder. SHAP değerlerini biyokimyasal, sekans bağlamı ve evrimsel gruplar olarak ayrıştırarak shap.summary_plot ile görselleştirir.
