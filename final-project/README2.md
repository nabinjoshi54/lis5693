# Discovering Patterns in Scientific Literature Abstracts using Text Mining and Machine Learning

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

# 2. Project Motivation

Scientific literature is growing rapidly across interdisciplinary domains such as battery technology, aerospace systems, and materials informatics. Manual analysis of thousands of research papers is time-consuming and inefficient.

This project demonstrates how Natural Language Processing (NLP) and Machine Learning (ML) techniques can automatically extract meaningful patterns, discover hidden research themes, and classify scientific abstracts into research domains. The project also highlights how unsupervised learning methods such as Latent Dirichlet Allocation (LDA) can uncover latent semantic structures without requiring labeled training data.

---

# 3. Dataset

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

# 4. Methods

## 4.1 Text Preprocessing

Pipeline:

1. Lowercasing
2. Number removal
3. Punctuation removal
4. Tokenization
5. Stopword removal
6. Lemmatization

---

## 4.2 Topic Modeling (LDA)

**Tools:** Gensim, pyLDAvis

* Models tested: 5, 10, 15 topics
* Evaluation: Coherence score (c_v)

**Best model:**

* 10 topics
* Coherence = 0.4883

**Insight:**
The LDA model successfully identified interpretable research themes related to battery systems, spacecraft technologies, and materials informatics without using labeled supervision. This demonstrates the effectiveness of probabilistic topic modeling for discovering latent semantic structures in scientific literature.

Why LDA?
LDA was selected over network analysis because the primary goal of this project was to discover hidden thematic structures within scientific abstracts rather than only analyzing word co-occurrence relationships. Scientific abstracts contain rich contextual information, making LDA more suitable for uncovering latent research topics across interdisciplinary domains.

---

## 4.3 Sentiment Analysis

**Tool:** NLTK VADER

**Purpose:**
Understand tone differences across domains.

**Results:**

* Battery: 0.751 (most positive)
* Space: 0.351 (more neutral/technical)
* Overall: 83.2% positive

---

# 5. Feature Engineering

**Method:** TF-IDF

* 5,000 features
* Unigrams + bigrams
* Sublinear TF scaling

**Output:** 1400 × 5000 matrix

---

# 6. Machine Learning Models

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

# 7. Results and Insights

* Overall accuracy: **99%**
* Best model: **Logistic Regression**
* Space category: perfectly classified
* Only **3 errors out of 280 test samples**

### Key Insight:
Why Was the Accuracy So High?
The dataset categories contain domain-specific terminology that is highly distinguishable across research areas. For example, space-related abstracts frequently contain words such as “satellite,” “orbit,” and “spacecraft,” while battery-related abstracts contain terms such as “lithium,” “electrolyte,” and “energy storage.” TF-IDF vectorization effectively captures these discriminative keywords, enabling the classifiers to achieve very high predictive performance.

Misclassifications occur mainly between:

```
battery = text_mining_materials
```

This reflects overlapping terminology in research literature.

---

# 8. Outputs

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

# 9. Limitations

Although the project achieved strong classification performance, several limitations remain:

* The dataset is very small comparable to other ML models training. The dataset is moderately imbalanced, particularly for the text_mining_materials category.
* Scientific domains often share overlapping terminology, which may introduce ambiguity during classification.
* Sentiment analysis using VADER may not fully capture nuanced scientific writing because research abstracts are generally technical and neutral in tone.
* Topic interpretation in LDA still requires human judgment and domain expertise.

---

# 10. Future Work

Potential future improvements include:

* Applying BERTopic or transformer-based topic modeling methods
* Using word embeddings such as Word2Vec or BERT
* Expanding the dataset using larger scholarly databases
* Developing interactive dashboards for topic exploration
* Applying the workflow to real-time scientific literature monitoring

---

# 11. Reference

Tshitoyan, V., et al. (2019).
*Unsupervised word embeddings capture latent knowledge from materials science literature.*
Nature, 571(7763), 95–98.

DOI: https://doi.org/10.1038/s41586-019-1335-8

---
