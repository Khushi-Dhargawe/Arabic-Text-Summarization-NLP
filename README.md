# 🔤 Arabic Text Summarization — Multi-Algorithm NLP Pipeline

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)
![NLTK](https://img.shields.io/badge/NLTK-3.8-orange)
![Gensim](https://img.shields.io/badge/Gensim-4.3-green)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.4-orange?logo=scikit-learn)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

> **Skills:** Python · NLTK · Gensim · Scikit-learn · NLP · Word2Vec · K-Means · BeautifulSoup4 · TF-IDF · Web Scraping  
> **Dataset:** Arabic Wikipedia articles — live web scraping via BeautifulSoup  
> **Benchmark:** Essex Arabic Summaries Corpus (EASC)

---

## Why This Matters

Arabic is spoken by over 400 million people yet remains severely under-resourced in NLP compared to English. Automated summarization for Arabic requires custom pipelines from scratch — standard tokenisation and matching patterns fail completely due to morphological complexity, diacritics and letter elongations. This project builds and benchmarks three approaches end-to-end.

---

## 📌 Project Overview

An end-to-end NLP pipeline that scrapes Arabic Wikipedia articles, cleans and normalises the text, and applies three distinct extractive summarisation algorithms — comparing their accuracy against the EASC benchmark corpus.

**Three algorithms benchmarked:**
1. **Bag of Words (BoW)** — Statistical word frequency scoring per sentence
2. **TF-IDF** — Vector space term weighting, penalises common corpus words
3. **Word2Vec Skip-Gram + K-Means** — Semantic dense embeddings clustered via spatial centroids

---

## 🗂️ Repository Structure & How Files Connect

```
📁 Arabic-Text-Summarization-NLP/
│
├── 📄 BagOfWords_Summarize.py                   ← Algorithm 1: BoW frequency scoring
│   │   Scrapes URL → cleans → tokenises → scores sentences → top 3 summary
│
├── 📄 TFIDF_Summarize_withoutStopWordsRemoval.py ← Algorithm 2: TF-IDF weighting
│   │   Splits sentences → calculates TF → IDF → TF×IDF → ranks top sentences
│
├── 📄 Arabic_Word2Vec_SkipGramSummarize.py       ← Algorithm 3: Word2Vec + K-Means
│   │   Loads pretrained model → sentence vectors → K-Means → centroid selection
│
├── 📄 utilities.py                               ← Shared NLP helper functions
│   │   clean_str() · get_vec() · calc_vec() · get_ngrams() · get_existed_tokens()
│
├── 📄 Business_Problem_Statement.pdf             ← Business case and metric goals
├── 📦 requirements.txt                           ← Python dependencies
└── 📜 LICENSE                                    ← MIT License
```

---

## 🔗 Pipeline Flow

```
Arabic Wikipedia URL
        │
        ▼
[Step 1] Web Scraping                  ← urllib.request + BeautifulSoup4
        │  Raw HTML → paragraph extraction
        ▼
[Step 2] Text Cleaning & Normalisation ← utilities.py: clean_str()
        │  Remove diacritics (Tashkeel) · Remove elongations (Tatweel)
        │  Strip HTML tags · Regex normalisation
        ▼
[Step 3] NLP Preprocessing
        │  ISRI Stemmer (Arabic root extraction)
        │  NLTK tokenisation · Arabic stopword removal
        ▼
[Step 4] Algorithm Branch
        │
        ├── BoW: word frequency → sentence scoring → heapq top-3
        ├── TF-IDF: normalised TF × IDF → ranked sentence extraction
        └── Word2Vec + K-Means: sentence vectors → k=2 clusters
                                → centroid-closest + first-appearance summaries
        ▼
[Step 5] Summary Output
           Top-ranked sentences returned as extractive summary
```

---

## 📊 Algorithm Performance — EASC Benchmark

| Algorithm | Vector Space | Accuracy | Notes |
|---|---|---|---|
| TF-IDF | Term weight filtering | **60%** | Best performer — local term concentration wins on single documents |
| Bag of Words | Word frequency scoring | **59%** | Strong baseline — simple but effective |
| Word2Vec + K-Means (First Appearance) | Semantic embeddings | **56%** | Captures meaning but loses local relevance |
| Word2Vec + K-Means (Centroid-Closest) | Semantic embeddings | **48%** | Centroid selection less effective than first-appearance |

> **Key finding:** TF-IDF outperforms Word2Vec on single-document extractive summarisation. Dense semantic embeddings excel on large corpora but lose local term concentration advantages on focused Arabic Wikipedia articles.

---

## 🧠 Arabic NLP Challenges Solved

| Challenge | Solution |
|---|---|
| Diacritics (Tashkeel) | Regex removal: `[\u0617-\u061A\u064B-\u0652]` |
| Letter elongations (Tatweel) | Regex pattern: `(.)\1+` → `\1\1` |
| Morphological complexity | NLTK ISRI Stemmer — reduces to 3-letter roots |
| Out-of-vocabulary words | Filtered against Word2Vec vocab before vectorisation |
| Sentence vectorisation | Mean of word vectors per sentence via NumPy |

---

## 🔬 Word2Vec Architecture

| Component | Details |
|---|---|
| Model | Pretrained Skip-Gram (`full_grams_sg_100_wiki`) |
| Dimensions | 100-D word embeddings |
| Sentence vector | Mean of word vectors (NumPy mean axis=0) |
| Clustering | K-Means (k=2, random_state=0) |
| Selection methods | First-appearance per cluster + centroid-closest (pairwise_distances_argmin_min) |

---

## What I'd Do With More Data

With the full EASC corpus I'd tune the K-Means cluster count dynamically based on article length rather than fixing k=2. With a larger pretrained Arabic model (e.g. AraVec 300-D) I'd expect Word2Vec accuracy to surpass TF-IDF — the current pretrained model is the bottleneck, not the clustering logic.

---

## 🚀 How to Run

```bash
git clone https://github.com/Khushi-Dhargawe/Arabic-Text-Summarization-NLP.git
cd Arabic-Text-Summarization-NLP
pip install -r requirements.txt

# Run Bag of Words summariser
python BagOfWords_Summarize.py

# Run TF-IDF summariser
python TFIDF_Summarize_withoutStopWordsRemoval.py

# Run Word2Vec + K-Means summariser (requires pretrained model)
# Download full_grams_sg_100_wiki from AraVec repository and place in project folder
python Arabic_Word2Vec_SkipGramSummarize.py
```

> **Note:** The Word2Vec script requires the `full_grams_sg_100_wiki` pretrained Arabic model. Download from the [AraVec GitHub repository](https://github.com/bakrianoo/aravec) and place in the project root.

---

## 👩‍💻 Author

**Khushi Dhargawe**
MSc Business Analytics — University College Cork (UCC)
BE Artificial Intelligence & Machine Learning (Hons. Cybersecurity) — Mumbai University

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://www.linkedin.com/in/khushi-dhargawe/)
[![GitHub](https://img.shields.io/badge/GitHub-Portfolio-black?logo=github)](https://github.com/Khushi-Dhargawe)

---

## 📜 License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
