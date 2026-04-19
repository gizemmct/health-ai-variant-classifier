# health-ai-variant-classifier

Branch 1: Feature Engineering 
 
Veri setindeki biyolojik ham bilgilerin, makine öğrenmesi modellerinin işleyebileceği 
matematiksel vektörlere dönüştürülmesi sürecini aşağıdaki adımlarla yönetir: 

1. Şartnamede sağlanan +-5 nükleotid komşuluğu verilerini, NLP tabanlı K-mer 
frekans vektörlerine dönüştürür. 
2. +-5 amino asit komşuluğu bilgisini kategorik formattan sayısal formata geçirmek 
için One-Hot Encoding matrislerini kodlar. 
3. Varyant öncesi ve sonrası amino asitlerin "hidrofobiklik (sudan kaçma) indeksi" 
ve "moleküler hacim" farklarının mutlak değerini matematiksel olarak 
hesaplayarak modele "Fizikokimyasal Şok Skoru" adında yeni bir sürekli 
(continuous) değişken ekler. 
4. Nötr amino asitlerin artı/eksi yüklü amino asitlere dönüşümünü tespit eden 
"Polarite Değişim Etiketi" (1/0) algoritmalarını yazar. 
5. Eski ve yeni kodonların vücutta bulunma sıklığı farkını literatürden çekerek 
"Kodon Kullanım Sıklığı (Codon Usage Bias)" özniteliğini veri setine entegre eder. 
6. Takımın genel koordinasyonunu sağlar ve geliştirilen modüllerin teknik 
dokümantasyonunu/akademik raporlamasını (PSR ve PDR) tamamlar.

Branch 2: Data Preprocessing

Gelen ham veri matrisinin algoritmik hatalardan arındırılması ve model değerlendirme 
altyapısının kurulması sürecini aşağıdaki adımlarla yönetir: 

1. Veri setindeki eksik (NaN) in-silico risk skorlarını, Scikit-Learn kütüphanesinin 
KNNImputer algoritmasını kullanarak en yakın komşuluk özniteliklerine göre 
doldurur. 
2. Özellik uzayındaki anormal dağılımları (outliers) tespit etmek için IsolationForest 
algoritmasını kurgular ve tutarsız varyantların veri setindeki ağırlıklarını düşürür. 
3. Özellikle CFTR (70 varyant) ve PAH (200 varyant) gen panellerinde azınlık sınıfı 
ezberlemesini önlemek için Imbalanced-Learn kütüphanesinden SMOTE 
(Sentetik Azınlık Aşırı Örnekleme) algoritmasını uygular. 
4. Sınıf dağılımlarının her eğitim döngüsünde sabit kalması için Scikit-Learn 
StratifiedKFold algoritmasıyla 5-katmanlı veri bölme altyapısını kodlar. 
5. Kurulacak ana mimariye kıyaslama (benchmark) oluşturması için Scikit-Learn 
üzerinden RandomForestClassifier ve LogisticRegression temel modellerini 
kurarak ilk referans metrikleri çıkarır.

Branch 3: Training

Veri boru hattı (pipeline) tamamlanmış matrisler üzerinde, projenin en yüksek başarımlı 
ana tahminleme modellerinin inşasını ve şeffaflaştırılmasını aşağıdaki adımlarla 
yönetir: 

1. Sınıflandırma problemi için ana mimari olarak XGBClassifier (XGBoost) ve 
LGBMClassifier (LightGBM) algoritmalarını kurgular. 
2. Ağaç algoritmalarının hiperparametrelerini (maksimum derinlik, öğrenme oranı, 
L2 katsayısı, özellik alt-örneklemesi) manuel denemek yerine, Optuna 
framework'ü üzerinden Bayes Optimizasyonu ile tarar. 
3. Modelin olasılık (probability) çıktılarının klinik hastalıklara uygunluğunu test 
etmek için Isotonic Regression (CalibratedClassifierCV) kullanarak karar 
kalibrasyonu yapar. 
4. Jürinin şeffaflık kısıtını karşılamak üzere XGBoost modeline shap.TreeExplainer 
algoritmasını entegre eder. 
5. SHAP (SHapley Additive exPlanations) değerlerini biyokimyasal, sekans bağlamı 
ve evrimsel gruplar olarak ayrıştırarak shap.summary_plot ile görselleştirir ve 
klinik yorumlanabilirliği sağlar. 
