Text Mining and Machine Learning on Scientific Abstracts

Course: LIS 4/5693 — Text Mining and Machine Learning

Professor: Dr. Manika Lamba, PhD

Students: Madison Bollinger and Nabin Joshi

Institution: University of Oklahoma

Semester: Spring 2026

Project Overview:
This project applies text mining and machine learning techniques to 1,400 scholarly abstracts collected from Lens.org across three research domains. Inspired by Tshitoyan et al. (2019), who demonstrated that scientific knowledge embedded in published abstracts can be extracted using NLP methods, this project performs a smaller-scale version of that work — discovering latent themes, comparing rhetorical tone, and building classifiers to predict research category from abstract text alone.

The full APA to the article is: 
Tshitoyan, V., Dagdelen, J., Weston, L., Dunn, A., Rong, Z., Kononova, O., ... & Jain, A. (2019). Unsupervised word embeddings capture latent knowledge from materials science literature. Nature, 571(7763), 95-98.

Dataset Source
All data was collected from Lens.org, an open-access scholarly literature platform. Three separate searches were conducted and exported as CSV files.
Search Queries and Labels:
These are three search queries we used: 
1. battery AND energy storage AND lithium ; The catergory 'battery' is labelled for this search query. It has 608 rows. 
2. space AND satellite AND spacecraft ; The catergory 'space' is labelled for this search query. It also has 608 rows.
3. text mining AND materials science AND battery ; The catergory 'text_mining_materials' is labelled for this search query. It only has 184 rows.

Dataset Details

Text column 'Abstract'  is the primary text used for all analysis. Similarly, label column 'category' is used as the target variable for ML classification.

Note on class imbalance: text_mining_materials has fewer documents (184) than the other two categories (608 each), reflecting its niche nature. This was handled using class_weight='balanced' in all ML models.

Methods:

Text Preprocessing
Raw abstracts were cleaned through a 7-step pipeline:

Lowercasing
Number removal
Punctuation removal
Tokenization
Stopword removal (standard English + custom academic stopwords)
Lemmatization (WordNetLemmatizer)

Text Analysis Method 1: Topic Modeling (LDA)

Tool: Gensim LDA + pyLDAvis
Since Tshitoyan et al. (2019) also used LDA approach — extracting latent thematic structure from scientific abstracts without supervision. It answers what themes exist across three research domains and is ideal for structured, content-rich scientific text.
Models trained: 5, 10, and 15 topics compared using coherence score (c_v)
Best model: 10 topics (coherence = 0.4883)
Result: 10 topics mapped cleanly onto the three dataset categories without any labels provided.

Text Analysis Method 2: Sentiment Analysis (VADER)

Tool: NLTK VADER (SentimentIntensityAnalyzer)
Scientific abstracts frequently make confident, achievement-oriented claims. VADER reveals how researchers write across domains — comparing emotional tone as a complementary perspective to LDA's structural analysis. Network Analysis was not selected because it requires explicit entity relationships not present at the abstract level.
Result: Battery papers (mean = 0.751) significantly more positive than space papers (mean = 0.351); 83.2% of all abstracts classified as positive

Feature Engineering

Method: TF-IDF (Term Frequency-Inverse Document Frequency)
Settings: 5,000 features, unigrams + bigrams, sublinear TF normalization
Output: 1,400 × 5,000 numerical matrix

Machine Learning (Task 2)
Three supervised classifiers trained and compared:
Model    CV F1 Macro    Std    Rank
Logistic Regression    0.9800    .011    1
SVM (LinearSVC)    0.979    0.009    2
Random Forest    0.965    0.011    3

Train/test split: 80/20 with stratification
Evaluation: 5-fold cross-validation, classification report, confusion matrix
Best classifier: Logistic Regression (highest CV F1 macro = 0.980)


