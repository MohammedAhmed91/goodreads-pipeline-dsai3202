# goodreads-pipeline-dsai3202

# Goodreads Pipeline — DSAI3202 Assignment

## 🧩 Overview
This repository contains my work for **DSAI3202 Lecture 5 – Data Cleaning, Feature Engineering, and Model-Ready Splitting on AWS**.  
The project demonstrates how to clean, prepare, and feature-engineer data using **AWS Glue DataBrew** and **Amazon SageMaker**, followed by versioning through **GitHub**.

---

## ⚙️ Project Workflow

### 1. Data Cleaning
- **Project:** `goodreads-advanced-cleaning`
- **Job:** `goodreads-cleaning-job`
- **Dataset:** `goodreads-curated-cleaning`
- **Recipe:** `goodreads-advanced-cleaning-recipe`
- **Output Path:** `s3://<your-bucket>/cleaned_v1/`

**Tasks Performed:**
- Filled missing text fields (`review_text`) with `"Unknown"`
- Replaced missing numeric values (`n_votes`, `rating`) with median
- Filled missing boolean fields with mode
- Removed duplicate rows
- Filtered reviews before **January 1, 2014**
- Normalized text by:
  - Converting to lowercase  
  - Removing URLs and special characters  
  - Trimming extra spaces

**Result:**  
A cleaned dataset with only valid and recent reviews, stored in Parquet format in the S3 bucket.

---

### 2. Tabular Feature Engineering
- **Project:** `goodreads-feature-engineering`
- **Recipe:** `goodreads-feature-engineering-recipe`
- **Output Path:** `s3://<your-bucket>/features_v1/`

**Features Added:**
| Feature | Description |
|----------|--------------|
| `rating_deviation` | Absolute difference between individual rating and book average (`ABS(rating - average_rating)`) |
| `user_avg_rating` | Mean of ratings per user (aggregated by `user_id`) |
| `user_rating_std` | Standard deviation of ratings per user |
| `user_total_reviews` | Total number of reviews per user |
| `ratings_count_log` | Log-transformed popularity metric (`LN(1 + ratings_count)`) |

**Result:**  
Feature-enriched tabular dataset saved to S3 for modeling.

---

### 3. Text Feature Engineering
- **Script:** `goodreads_text_features_trial.py`
- **Service:** Amazon SageMaker Processing
- **Trial Output:** `s3://<your-bucket>/trial/trial_output.parquet`
- **Full Feature Output:** `s3://<your-bucket>/features_v2/`

**Process Summary:**
- Loaded tabular features from `features_v1`
- Created a subset using trial script to verify SageMaker setup
- (Optional full step) Generated text features like word vectors or embeddings for model training

---

## 🧾 Deliverables and GitHub Structure

goodreads-pipeline-dsai3202/
├── databrew/
│ ├── recipes/
│ │ ├── advanced_cleaning_v1.json
│ │ └── feature_engineering_v1.json
│ └── jobs/
├── sagemaker/
│ ├── goodreads_text_features_trial.py
│ └── (optional) goodreads_text_features.py
├── notebooks/
│ └── (optional) eda_sample.ipynb
├── configs/
│ └── (optional) split_config.json
├── doc/
│ └── report.md
└── README.md



---

## 🧠 Questions & Explanations

### 1. What data cleaning operations were performed?
I filled missing values, removed duplicates, filtered older data (before 2014), and normalized text columns.  
This ensures data quality, consistency, and reliability for downstream modeling.

### 2. What features were engineered?
I derived both numeric and aggregated features:
- `rating_deviation`, `ratings_count_log` (numeric)
- `user_avg_rating`, `user_rating_std`, `user_total_reviews` (aggregated)

These help capture user behavior, rating consistency, and book popularity.

### 3. Why version with Git?
Versioning allows tracking each stage of data preparation and ensures reproducibility.  
Each recipe and script is committed and tagged so that any version of the dataset can be re-created later.

---

## 📊 Final Feature List

rating_deviation
user_avg_rating
user_rating_std
user_total_reviews
ratings_count_log


---

## 🧱 Git Structure and Versioning

**Branches:**
- `feature/cleaning-v1`
- `feature/feature-engineering-v1`
- `feature/text-features`

**Tags:**
- `data-cleaned_v1`
- `feature-engineering_v1`
- `text-features_v1`

**Commit Message Examples:**
- `feat(cleaning): add DataBrew cleaning recipe`
- `feat: feature engineering recipe`
- `feat: add text feature processing script`





