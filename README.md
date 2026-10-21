# Phishing Website Detection using Random Forest

**IBM SkillsBuild Cyber Security 2026 (BharatCares) — Final Project**
**Author:** Vikash

---

## Project Description

Phishing is one of the most common ways attackers gain initial access: a fake website that looks legitimate tricks the user into giving away credentials. Checking every URL manually is impossible, so this project builds a machine learning model that classifies a website as **phishing** or **legitimate** using only its URL.

The project covers the full ML workflow:

1. Load and inspect the dataset
2. Check data quality (missing values, duplicates, class balance)
3. Extract numeric features from raw URL strings
4. Split data into training and testing sets
5. Train a Random Forest classifier
6. Evaluate using accuracy, precision, recall, F1-score and a confusion matrix
7. Analyse which features matter most

---

## Dataset

**Phishing Website Detection Dataset (Kaggle)**
https://www.kaggle.com/datasets/karthick2803/phishing-website-detection-dataset

| Property | Value |
|---|---|
| Total rows | 20,000 |
| Columns | `url` (text), `label` (0 = legitimate, 1 = phishing) |
| Class balance | 10,000 legitimate / 10,000 phishing |
| Missing values | 0 |
| Duplicate rows | 0 |

The label mapping (0 = legitimate, 1 = phishing) was verified by manually inspecting sample URLs from each class.

---

## Technologies Used

- Python 3
- pandas, numpy (data handling)
- scikit-learn (model, train/test split, metrics)
- matplotlib, seaborn (visualisation)
- Google Colab / Jupyter Notebook

---

## Features Extracted from URLs

The raw URL is converted into 13 numeric features:

| Feature | Description |
|---|---|
| `url_length` | Total length of the URL |
| `host_length` | Length of the domain/host part |
| `path_length` | Length of the path part |
| `num_dots` | Number of `.` characters |
| `num_hyphens` | Number of `-` characters |
| `num_digits` | Number of digits |
| `num_slashes` | Number of `/` characters |
| `num_params` | Number of `?` and `&` characters |
| `has_at` | 1 if the URL contains `@` |
| `has_ip` | 1 if the host is an IP address |
| `num_subdomains` | Number of subdomains |
| `is_https` | 1 if the URL starts with `https` |
| `has_susp_word` | 1 if the URL contains words like login, verify, secure, account, update, bank, confirm |

---

## Model

- **Algorithm:** Random Forest Classifier (`n_estimators=100`, `random_state=42`)
- **Split:** 80% training (16,000 URLs) / 20% testing (4,000 URLs), stratified so both sets keep the same class ratio

---

## Results

Evaluated on the 4,000 unseen test URLs:

| Class | Precision | Recall | F1-score |
|---|---|---|---|
| Legitimate (0) | 0.83 | 0.87 | 0.85 |
| Phishing (1) | 0.86 | 0.82 | 0.84 |

**Overall accuracy: 0.84**

Confusion matrix:

| | Predicted legit | Predicted phishing |
|---|---|---|
| **Actual legit** | 1732 | 268 |
| **Actual phishing** | 354 | 1646 |

In a security setting, the 354 missed phishing URLs (false negatives) are the most costly errors, so phishing **recall (0.82)** is the key metric to improve.

The most important features were the length-based ones (`path_length`, `host_length`, `url_length`), followed by `num_digits` and `num_dots`.

---

## Setup and Run

1. Download `dataset.csv` from the Kaggle link above and place it in the same folder as the notebook (in Colab, upload it to the working folder).
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Open `Vikash_PhishingDetector.ipynb` in Jupyter or Google Colab.
4. Run all cells from top to bottom (**Runtime → Run all** in Colab).

---

## Project Files

| File | Purpose |
|---|---|
| `Vikash_PhishingDetector.ipynb` | Complete project code |
| `requirements.txt` | Python dependencies with versions |
| `Vikash_ProjectReport.docx` | Full project documentation |
| `README.md` | This file |

---

## Key Information and Limitations

- The model uses **only URL-based features**. It does not look at page content, domain age, or reputation, so phishing pages hosted on trusted platforms (for example, shared Google documents) are hard to catch.
- The URLs in the dataset appear without the `http://` / `https://` prefix, so the `is_https` feature carries little information.
- Real datasets can contain labelling noise, so some labels may not be perfect.
- Possible improvements: add features such as suspicious top-level domains (`.tk`, `.xyz`, `.cc`), digit ratio and special-character counts, and compare other models.

---

## Ethical Note

This project is for educational purposes and defensive security research. It detects phishing; it does not create or deliver phishing content.
