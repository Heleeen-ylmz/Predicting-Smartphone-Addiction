# 📱 Predict Digital Addiction | End-to-End Machine Learning & R&D Pipeline

![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/scikit_learn-F79A3E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![LightGBM](https://img.shields.io/badge/LightGBM-2E8B57?style=for-the-badge)
![XGBoost](https://img.shields.io/badge/XGBoost-111111?style=for-the-badge)
![CatBoost](https://img.shields.io/badge/CatBoost-FF0000?style=for-the-badge)

Bu proje, **Kaggle Playground Series (Season 6, Episode 8)** kapsamında kullanıcıların demografik verileri, günlük ekran süreleri ve dijital alışkanlıklarına dayanarak **dijital bağımlılık düzeylerini (`addicted_label`)** tahmin etmek amacıyla geliştirilmiştir. Projede klasik modellemeden ziyade endüstriyel **Ar-Ge süreçleri (Domain Validation, Leakage-Free Stacking, Feature Engineering)** ön planda tutulmuştur.

---

## 📌 Proje Mimarisi ve Ar-Ge Yaklaşımı

Bu çalışmada modellerin uçtan uca güvenilirliğini sağlamak adına uygulanan kritik veri mühendisliği adımları:

1. **Domain Validation & Veri Kalitesi Kontrolü:** 
   * Sosyal medya, oyun ve iş/ders saatleri toplamının genel günlük ekran süresini aşıp aşmadığı mantıksal kontrolle denetlenmiştir.
2. **Segment Bazlı Medyan Doldurma (Demographic Imputation):**
   * Veri setindeki eksik değerler jenerik ortalamalar yerine, `age_group` (Genç, Orta Yaş, Yetişkin) ve `gender` kırılımlarında hesaplanan medyanlar ile doldurulmuş; böylece veri profili korunmuştur.
3. **Missing Indicator & Domain Feature Engineering:**
   * Eksik verilerin varlığı model için bir sinyal olarak `_is_missing` bayrakları ile modele sunulmuştur.
   * `social_media_ratio`, `gaming_ratio`, `work_study_ratio`, `notification_per_open` ve `weekend_surge_ratio` gibi oran bazlı etkileşim öznitelikleri türetilmiştir.
4. **Aykırı Değer Baskılama (Winsorization):**
   * Aşırı uç değerler IQR (Çeyrekler Arası Aralık) yöntemiyle üst ve alt sınırlara baskılanarak modelin uç değerlere karşı direnci artırılmıştır.
5. **Veri Sızıntısı (Data Leakage) & OOF Stacking:**
   * Ana modellerin eğitildikleri veri üzerindeki tahminlerini doğrudan meta-modele besleme kurgusunun **Overfitting** (Skorun 0.90'lara gerilemesi) ürettiği tespit edilmiştir.
   * Bu problemi çözmek için **5-Fold Out-of-Fold (OOF)** Cross-Validation mimarisi kurularak sızıntısız, genelleştirme yeteneği yüksek bir **Logistic Regression Stacking Meta-Modeli** inşa edilmiştir.

---

## 📊 Model Performansları ve Karşılaştırma

Eğitim sürecinde dünyada tabüler verilerde kabul görmüş SOTA (State-of-the-Art) algoritmalar ve bunların harmanlanmış kurguları karşılaştırılmıştır:

| Model / Metot | ROC-AUC Skoru | F1-Skoru | Mimari / Açıklama |
| :--- | :---: | :---: | :--- |
| **LightGBM** | **0.9535** | **0.9226** | En hızlı ve en yüksek tekil başarım |
| **XGBoost** | 0.9519 | 0.9212 | Kararlı ve dengeli yapraksal büyüme |
| **CatBoost** | 0.9450 | 0.9158 | Kategorik yapılarda yüksek genelleştirme |
| **Weighted Ensemble** | - | - | 0.50\*LGBM + 0.35\*XGB + 0.15\*CatBlend |
| **5-Fold OOF Stacking** | **0.9487** | - | Sızıntısız (Leak-Free) Logistic Regression Meta-Learner |

---

## 💡 Keşifler ve Dersler (Lessons Learned)

* **Veri Sızıntısı Teşhisi:** Model geliştirme esnasında eğitim verisi üzerinden alınan birinci seviye tahminler meta-modele verildiğinde skor belirgin şekilde düşmüş ve model ezberleme yapmıştır.
* **OOF Çözümü:** 5-Fold Stratified K-Fold mimarisinde her katmanın "daha önce görmediği" doğrulama kümesinden üretilen OOF tahmin matrisi, meta-modelin gerçek dünya test verisindeki davranışını en doğru şekilde simüle etmesini sağlamıştır.

---

## 📁 Proje Yapısı

```bash
.
├── notebooks/
│   └── digital_addiction_eda_modeling.ipynb  # End-to-end Kaggle Notebook
├── data/
│   ├── train.csv                             # Eğitim veri seti (Giriş)
│   └── test.csv                              # Test veri seti (Giriş)
├── submission.csv                            # Final Stacking tahminleri
└── README.md                                 # Proje dokümantasyonu

