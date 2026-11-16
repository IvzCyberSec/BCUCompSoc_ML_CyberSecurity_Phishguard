# PhishGuard: Build Your Own Phishing URL Detector

**BCU Computing Society | Machine Learning for Cybersecurity**  
**Platform:** Google Colab | **Language:** Python | **Level:** Beginner

## The challenge

Can we train a computer to recognise URL patterns commonly found in phishing examples?

In this workshop, you will use a labelled dataset to train a **Decision Tree classifier**. You will turn URL text into numerical features, train and test the model, investigate its mistakes, and use it to classify new example URLs.

> **Important:** PhishGuard is an educational prototype. Its prediction is not proof that a URL is safe or malicious. Do not use it to make real security decisions.

## What you will build

By the end of the practical, you will have:

- A function that extracts numerical features from a URL.
- A supervised machine learning model trained to classify URLs.
- Evaluation results, including accuracy, precision, recall and a confusion matrix.
- A function that accepts a new URL string and returns a model prediction.
- A basic understanding of false positives, false negatives and model limitations.

## How the project works

```text
Labelled URL dataset
        |
        v
Extract numerical URL features
        |
        v
Split examples into training and testing sets
        |
        v
Train a Decision Tree classifier
        |
        v
Evaluate predictions on held-back examples
        |
        v
Enter a new URL and get a prediction
```

This is **supervised learning** because the training examples have known labels. It is a **binary classification** task because the model predicts one of two categories:

| Workshop label | Meaning |
|---|---|
| `0` | Legitimate |
| `1` | Phishing |

The model learns patterns from labelled examples. It does not understand the intent of the person who created a URL.

## Project files

```text
PhishGuard_ML_Workshop/
├── README.md
└── PhishGuard_URL_Detector.ipynb
```

The Google Colab notebook contains the explanations, code, visualisations and exercises. You do not need to create separate Python files.

## Getting started

You need a laptop and an internet connection. You do not need to install Python locally.

1. Download or clone this repository.
2. Open [Google Colab](https://colab.research.google.com/).
3. Select **File → Upload notebook**.
4. Upload `PhishGuard_URL_Detector.ipynb`.
5. Select **Runtime → Run all**, or run the cells in order using the play buttons.
6. Read the short instructions in each section and look at the output before moving on.

The notebook installs its required Python packages and downloads the dataset automatically. The dataset download is approximately 54 MB, so it may take a little while.

### If the dataset download fails

Use the official [UCI PhiUSIIL dataset page](https://archive.ics.uci.edu/dataset/967/phiusil+phishing+url+dataset) to download the dataset. Then return to the notebook and follow the **If the download fails** instructions to upload the CSV.

The fallback expects a CSV with columns named `URL` and `Label` (capitalisation does not matter). Only download the dataset file. **Do not open any URLs stored inside it.**

## Dataset

This workshop uses the **PhiUSIIL Phishing URL (Website) Dataset** from the UCI Machine Learning Repository.

- 235,795 labelled examples in the original dataset.
- Original UCI labels: `1 = legitimate`, `0 = phishing`.
- The notebook converts these to `0 = legitimate`, `1 = phishing` so that phishing is the positive class in the evaluation.
- The notebook removes empty or invalid rows and duplicate URLs.
- To keep the practical manageable in Colab, it selects up to **30,000 examples**, preserving the class proportions.

The original dataset contains URL information and webpage-derived features. For this workshop, the notebook uses the URL string and extracts its own features. It does not use webpage-derived fields.

## Features used by the model

The notebook extracts 12 numerical features from each URL:

| Feature | What it measures |
|---|---|
| `url_length` | Total number of characters in the URL |
| `hostname_length` | Number of characters in the hostname |
| `dots_in_hostname` | Number of dots in the hostname |
| `hyphens_in_hostname` | Number of hyphens in the hostname |
| `digits_in_url` | Number of digits in the URL |
| `special_characters` | Count of selected symbols such as `@`, `?`, `=`, `&`, `%` and `_` |
| `path_length` | Length of the URL path |
| `query_length` | Length of the query string |
| `has_ip_address` | Whether the hostname is an IP address (`1` or `0`) |
| `uses_https` | Whether the URL uses HTTPS (`1` or `0`) |
| `has_at_symbol` | Whether the URL contains `@` (`1` or `0`) |
| `keyword_count` | Number of selected words found, such as `login`, `verify`, `account` and `password` |

These are simple clues, not rules that prove a URL is malicious. Legitimate websites can have long URLs, hyphens, numbers, login pages and other features that may also appear in phishing URLs.

## Model and training

The practical uses scikit-learn's `DecisionTreeClassifier` with the following settings:

```python
DecisionTreeClassifier(
    max_depth=6,
    min_samples_leaf=25,
    class_weight="balanced",
    random_state=42
)
```

The examples are split into:

- **80% training data**: examples used to fit the model.
- **20% testing data**: examples held back to evaluate its predictions.

The split is stratified so the proportion of legitimate and phishing examples remains similar in both groups. The fixed random seed makes the split repeatable.

## What you will investigate

The notebook guides you through these stages:

1. Set up the Python environment.
2. Download and inspect the labelled dataset.
3. Understand how a URL is split into its parts.
4. Extract numerical URL features.
5. Prepare the feature table and labels.
6. Split the data into training and testing sets.
7. Train the Decision Tree.
8. Evaluate the model using accuracy, precision, recall and a confusion matrix.
9. Visualise the tree and feature importance.
10. Use `check_url()` to test new example URLs.
11. Experiment with tree depth and compare training and testing accuracy.

## Final challenge

Use the examples in the notebook to create two **fictional** URLs:

1. One that looks suspicious to you but the model predicts as legitimate-like.
2. One that looks ordinary to you but the model predicts as potentially phishing.

Use reserved `.example` domains only. Do not open the URLs.

Discuss:

- Which extracted features might have influenced the prediction?
- Why might the model have made a mistake?
- What additional information would help an analyst assess the URL?
- Why should a prediction not automatically block a website?

## Understanding the evaluation

- **Accuracy:** the proportion of test examples classified correctly.
- **Precision:** of the URLs predicted as phishing, how many were labelled phishing?
- **Recall:** of the URLs labelled phishing, how many did the model identify?
- **False positive:** a legitimate URL is incorrectly predicted as phishing.
- **False negative:** a phishing-labelled URL is incorrectly predicted as legitimate.

In cybersecurity, both types of mistake matter. False positives can interrupt legitimate users and create unnecessary alerts. False negatives can allow suspicious links to go unnoticed. Accuracy alone does not tell the whole story.

## Limitations and safe use

- PhishGuard analyses URL text only. It does not visit the website, follow redirects, inspect webpage content, validate certificates or check domain reputation.
- URL characteristics are indicators, not proof. Legitimate and phishing URLs can share the same characteristics.
- The model learns from the dataset labels. Incorrect, incomplete or outdated labels can affect what it learns.
- A random test split does not guarantee that the model will work on future phishing campaigns or previously unseen domains.
- Do not open URLs from the dataset or enter live suspicious links into the practical.
- Use fictional `.example` addresses for the exercises.
- Do not connect the model to a real browser, account or network service, and do not automatically block real traffic based on its predictions.

## Libraries used

The notebook installs `ucimlrepo` and uses:

- **pandas** and **NumPy** for data handling.
- **scikit-learn** for splitting data, training the Decision Tree and evaluating predictions.
- **Matplotlib** for charts and model visualisations.
- Python standard-library modules for URL parsing and feature extraction.

## Further learning

- [UCI PhiUSIIL Phishing URL Dataset](https://archive.ics.uci.edu/dataset/967/phiusil+phishing+url+dataset)
- [Google Teachable Machine](https://teachablemachine.withgoogle.com/)
- [scikit-learn: Decision Trees](https://scikit-learn.org/stable/modules/tree.html)
- [scikit-learn: Model evaluation](https://scikit-learn.org/stable/modules/model_evaluation.html)
- [UCI Machine Learning Repository](https://archive.ics.uci.edu/)

## Dataset acknowledgement

Prasad, A. & Chandra, S. (2024). *PhiUSIIL Phishing URL (Website) Dataset*. UCI Machine Learning Repository. [https://doi.org/10.24432/C56W3W](https://doi.org/10.24432/C56W3W). Licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

Keep this acknowledgement if you reuse the dataset or adapt this workshop.

---

**BCU Computing Society | PhishGuard: Build Your Own Phishing URL Detector**
