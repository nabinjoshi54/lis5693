# Text Mining and Machine Learning on Scientific Abstracts

**Course:** LIS 4/5693 — Text Mining and Machine Learning
**Instructor:** Dr. Manika Lamba
**Students:** Madison Bollinger, Nabin Joshi
**University:** University of Oklahoma
**Semester:** Spring 2026

---

# Contents

1. Project Overview
2. Dataset
3. Methods

   * Text Preprocessing
   * Topic Modeling (LDA)
   * Sentiment Analysis
4. Feature Engineering
5. Machine Learning Models
6. Results and Insights
7. Outputs and Repository Structure
8. Reference

---

# 1. Project Overview

This project applies text mining and machine learning techniques to **1,400 scientific abstracts** collected from Lens.org across three research domains.

Inspired by *Tshitoyan et al. (2019)*, this project explores how scientific knowledge embedded in literature can be extracted using NLP techniques.

**Goals:**

* Discover latent themes in research abstracts
* Analyze sentiment differences across domains
* Build classifiers to predict research category

---

# 2. Dataset

**Source:** Lens.org (open-access scholarly database)

### Categories:

| Category              | Query                                         | Size |
| --------------------- | --------------------------------------------- | ---- |
| Battery               | battery AND energy storage AND lithium        | 608  |
| Space                 | space AND satellite AND spacecraft            | 608  |
| Text Mining Materials | text mining AND materials science AND battery | 184  |

**Columns Used:**

* `Abstract` → text data
* `category` → target label

### Class Imbalance:

The *text_mining_materials* class is smaller and handled using:

```python
class_weight='balanced'
```

---

# 3. Methods

## 3.1 Text Preprocessing

Pipeline:

1. Lowercasing
2. Number removal
3. Punctuation removal
4. Tokenization
5. Stopword removal
6. Lemmatization

---

## 3.2 Topic Modeling (LDA)

**Tools:** Gensim, pyLDAvis

* Models tested: 5, 10, 15 topics
* Evaluation: Coherence score (c_v)

**Best model:**

* 10 topics
* Coherence = 0.4883

**Insight:**
Topics align well with dataset categories without supervision.

---

## 3.3 Sentiment Analysis

**Tool:** NLTK VADER

**Purpose:**
Understand tone differences across domains.

**Results:**

* Battery: 0.751 (most positive)
* Space: 0.351 (more neutral/technical)
* Overall: 83.2% positive

---

# 4. Feature Engineering

**Method:** TF-IDF

* 5,000 features
* Unigrams + bigrams
* Sublinear TF scaling

**Output:** 1400 × 5000 matrix

---

# 5. Machine Learning Models

Three classifiers were trained:

| Model               | CV F1 Macro | Std   |
| ------------------- | ----------- | ----- |
| Logistic Regression | 0.980       | 0.011 |
| SVM (LinearSVC)     | 0.979       | 0.009 |
| Random Forest       | 0.965       | 0.011 |

**Setup:**

* Train/test split: 80/20
* Stratified sampling
* 5-fold cross-validation

---

# 6. Results and Insights

* Overall accuracy: **99%**
* Best model: **Logistic Regression**
* Space category: perfectly classified
* Only **3 errors out of 280 test samples**

### Key Insight:

Misclassifications occur mainly between:

```
battery ↔ text_mining_materials
```

This reflects overlapping terminology in research literature.

---

# 7. Outputs and Repository Structure

### outputs/

Contains model results:

* model1_logistic_predictions.csv
* model2_random_forest_predictions.csv
* model3_svm_predictions.csv
* classification reports

### charts/

Contains visualizations:

* category distribution
* abstract length
* top words
* LDA coherence
* sentiment analysis
* model comparison

### notebooks/

* final_project.ipynb (fully annotated)

### dataset/

* final_dataset.csv

---

# 8. Reference

Tshitoyan, V., et al. (2019).
*Unsupervised word embeddings capture latent knowledge from materials science literature.*
Nature, 571(7763), 95–98.

DOI: https://doi.org/10.1038/s41586-019-1335-8

---
