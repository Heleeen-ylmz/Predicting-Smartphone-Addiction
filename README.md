# 📱 Predict Digital Addiction | Kaggle Playground Series (s6e8)

![Python](https://img.shields.io/badge/Python-3.12-blue.svg)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.x-orange.svg)
![LightGBM](https://img.shields.io/badge/LightGBM-4.x-green.svg)
![XGBoost](https://img.shields.io/badge/XGBoost-2.x-red.svg)
![CatBoost](https://img.shields.io/badge/CatBoost-1.x-yellow.svg)

Bu proje, **Kaggle Playground Series (s6e8)** yarışması kapsamında kullanıcıların demografik verileri, günlük ekran süreleri ve uygulama kullanım alışkanlıklarına dayanarak **dijital bağımlılık düzeylerini (addicted_label)** tahmin etmek amacıyla geliştirilmiştir[cite: 2].

---

## 📌 Proje Öne Çıkanları ve Ar-Ge Yaklaşımı

* **Domain Validation & Veri Kalitesi Kontrolü:** Ekran sürelerinin alt kategoriler ile uyumu ve mantıksal tutarsızlıkları incelenmiştir[cite: 2].
* **Gruplanmış Medyan Doldurma (Demographic Group Imputation):** Eksik veriler jenerik ortalamalar yerine `age_group` ve `gender` bazlı medyanlar ile doldurulmuştur[cite: 2].
* **Aykırı Değer Yönetimi (Outlier Capping):** IQR (Çeyrekler Arası Aralık) yöntemiyle ekstrem uç değerler bilgi kaybı yaşanmadan üst/alt sınırlara baskılanmıştır (Winsorization)[cite: 2].
* **Veri Sızıntısı (Data Leakage) & Overfitting Analizi:** Doğrudan train verisi tahminleri üzerinden kurulan ilk Stacking mimarisinde modellerin ezber yaptığı (skorun düşmesi) tespit edilmiş; bu durum **5-Fold Out-of-Fold (OOF)** Cross-Validation kurgusuna geçilerek çözülmüştür[cite: 2].

---

## 📊 Model Performansları ve Karşılaştırma

| Model / Metot | ROC-AUC Skoru | F1-Skoru | Açıklama / Not |
| :--- | :---: | :---: | :--- |
| **LightGBM** | 0.9535 | 0.9226 | En hızlı ve en yüksek tekil performans[cite: 2] |
| **XGBoost** | 0.9519 | 0.9212 | Kararlı ve dengeli sınıflandırma[cite: 2] |
| **CatBoost** | 0.9450 | 0.9158 | Kategorik verilerde yüksek genelleme[cite: 2] |
| **5-Fold OOF Stacking** | **0.9487** | - | Meta-Model (Logistic Regression) ile veri sızıntısız ensemble[cite: 2] |

---

## 💡 Keşif ve Dersler (Lessons Learned)

> **Veri Sızıntısı Notu:** Ana modellerin kendi eğitildikleri veri (train set) üzerindeki tahminlerini doğrudan Stacking meta-modeline beslemek aşırı öğrenmeye (overfitting) yol açmaktadır[cite: 2]. Bu problemi aşmak için modelin daha önce hiç görmediği katmanlardan elde edilen **Out-of-Fold (OOF)** tahmin matrisi kurgulanmıştır[cite: 2].
