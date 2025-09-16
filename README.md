#Phishing
Project overview :-

Phishing Website Predictor is a machine learning pipeline that classifies websites/URLs as phishing or legitimate. It extracts URL-level and HTML-level features (when available), trains multiple classifiers, evaluates performance, and exposes a prediction interface (script + optional Flask API).
phishing website detector link:-http://127.0.0.1:8000/docs#/default/predict_predict__feature__get
Key features :-

URL-based feature extraction (length, tokens, suspicious tokens, presence of IP address, hyphens, subdomain count, etc.)

HTML-based features (forms, suspicious scripts, external resource ratio) — optional

Multiple models supported: Logistic Regression, Random Forest, XGBoost, LightGBM, simple Neural Network

Training pipeline with reproducible experiment config

Evaluation with confusion matrix, ROC-AUC, precision, recall, F1

CLI for single-URL prediction and batch predictions (CSV)

Optional Flask API to serve predictions

Data :-

This project uses labeled URLs/website examples containing phishing and legitimate samples. Common public sources (you can replace with your own) include:

PhishTank dataset (phishing URLs)

Open legitimate URL lists (e.g., Alexa top sites snapshot — if used, ensure licensing)

Custom labeled datasets

Feature extraction :-

Features should be computed from the URL string and, if available, from the HTML of the page:

URL-based:

length of URL, length of domain, number of dots, number of subdomains

presence of '@', '-', 'https', 'ip address instead of domain'

count of suspicious tokens: login, account, secure, verify, update, bank etc.

ratio of digits to total chars

entropy / randomness estimate of URL

Host-based (optional):

WHOIS age (domain age), TTL, registrar

HTML-based (optional, when HTML available):

number of forms, presence of password input, number of external scripts/stylesheets

iframe usage, meta refresh tags, anchors pointing to external domains

NLP/text features:

Bag-of-words or TF-IDF on page visible text (if HTML provided)

The src/features.py should implement a extract_features(url, html=None) function that returns a dictionary or numpy vector.

Models & experiments :-

Try a few baseline models and choose best by validation metrics:

Baselines: Logistic Regression, DecisionTree

Tree-based: RandomForest, XGBoost, LightGBM

Ensemble stacking of best models

Neural network for combined dense features or TF-IDF + dense features

Use stratified train/test split (or cross-validation) and keep holdout test set for final evaluation.

Results & expected metrics :-

Your mileage will vary by dataset. Typical baseline expectations on a balanced, well-curated dataset:

Accuracy: 90%+ (depends heavily on class balance & data quality)

Precision/Recall/F1 for phishing class: aim for Precision ≥ 0.85 and Recall ≥ 0.80

ROC-AUC: 0.90+

Always report metrics on a held-out test set and provide confusion matrix to show false positives vs false negatives. In phishing detection, minimizing false negatives (missed phishing) is often more critical, but excessive false positives are bad too — choose thresholds accordingly.
