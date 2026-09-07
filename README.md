# Optimized Spam Detection System

Project 03 — TechMaster Academy (ML Fundamentals)

نظام Machine Learning بيفرّق بين الرسائل الـ **spam** والـ **ham** (الشرعية)، بيقارن بين 3
موديلات classification مختلفة، ويثبت إن أي تحسين في الأداء حقيقي مش مجرد صدفة.

---

## Project Structure

```
optimized-spam-detector/
├── data/
│   └── spam.csv              # الداتاست (SMS/Email messages)
├── exploration.ipynb          # Notebook للاستكشاف والتجربة (EDA) — اختياري
├── preprocess.py               # Stage 1 — Load, Inspect, Clean, Split
├── features.py                  # Stage 2 — Feature Extraction (TF-IDF)
├── train.py                      # Stage 3 — Model Training & Tuning
├── evaluate.py                    # Stage 4 — Evaluation & Comparison + نقطة التشغيل الرئيسية
├── app.py                          # واجهة Streamlit تفاعلية لتجربة الموديل
├── requirements.txt
└── .gitignore
```

---

## Setup

```bash
# 1. اعمل virtual environment (اختياري بس مستحسن)
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # macOS/Linux

# 2. ركّب المكتبات المطلوبة
pip install -r requirements.txt
```

---

## Usage

### 1) تدريب الموديلات وتقييمها

```bash
python evaluate.py
```

الأمر ده بيشغّل الـ pipeline كامل بالترتيب (`preprocess.py` → `features.py` → `train.py` →
`evaluate.py`)، وفي الآخر:
- بيطبع جدول مقارنة بين الموديلات الثلاثة (Accuracy, Precision, Recall, F1)
- بيحفظ صورة `model_comparison.png` فيها الجدول + Confusion Matrix + Classification Report
- بيحفظ أفضل موديل والـ vectorizer كـ `best_model.pkl` و `vectorizer.pkl` — دول لازم يكونوا
  موجودين قبل ما تشغّل `app.py`

### 2) تشغيل واجهة Streamlit

بعد ما `evaluate.py` يخلص ويحفظ `best_model.pkl` و `vectorizer.pkl`:

```bash
streamlit run app.py
```

هيفتحلك المتصفح تلقائيًا على `http://localhost:8501` (أو بورت تاني لو مشغول). الواجهة بتديك:

- **Input Message** — تكتب رسالة وتضغط Predict
- **Prediction Result** — SPAM أو HAM مع نسبة الاحتمالية لكل واحدة
- **Confusion Matrix** — عرض بصري لنتائج الموديل النهائي على test set
- **Accuracy** — دقة الموديل على الداتا اللي مشافهاش قبل كده
- **Model Comparison** — مقارنة بصرية بين الموديلات الثلاثة

> **ملاحظة:** لازم تشغّل `python evaluate.py` الأول قبل `streamlit run app.py`، لأن الواجهة
> بتحمّل الموديل المحفوظ (`best_model.pkl`) ومش بتدربه من الصفر.

---

## Models Used

| Model | ملاحظات |
|---|---|
| Multinomial Naive Bayes | متدرب بـ `GridSearchCV` لضبط `alpha` |
| Logistic Regression | |
| SVM (SVC) | |

**معيار اختيار أفضل موديل:** أعلى **Precision** — عشان في spam detection، أخطر غلطة هي إن
رسالة شرعية (ham) تتصنف غلط كـ spam (false positive)، والـ Precision بتقيس بالظبط ده.

---

## Tools

Python · scikit-learn · Pandas · NumPy · Matplotlib · Streamlit · joblib

---

## Out of Scope

Deep Learning, Neural Networks, Transformers, LLMs, Cloud Deployment, Production APIs, MLOps.
