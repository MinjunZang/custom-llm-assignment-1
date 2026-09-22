# Custom LLM with nanoGPT — Assignment Report

This repository contains the setup, training, and evaluation for building a custom small language model using Karpathy's **nanoGPT** architecture. The task compares two experimental runs: the **Baseline Classroom Corpus** and an **Expanded Corpus** with custom teaching data.

---

## 1. Executive Summary & Results Overview

| Experiment Stage | Scorable Accuracy | All-Case Success | Vocabulary Coverage | Validation Loss (3000 steps) |
| :--- | :---: | :---: | :---: | :---: |
| **Baseline Untrained** | 37.5% (9/24) | 18.8% (9/48) | 50.0% (24/48) | 4.9275 |
| **Baseline Trained** | 83.3% (20/24) | 41.7% (20/48) | 50.0% (24/48) | 0.7061 |
| **Expanded Untrained** | 29.6% (8/27) | 16.7% (8/48) | 56.3% (27/48) | 5.5728 |
| **Expanded Trained** | **92.6% (25/27)** | **52.1% (25/48)** | **56.3% (27/48)** | 0.7638 |

---

## 2. Training Hyperparameters & Loss Curves

Both experiments were executed using identical architectural settings to ensure a fair comparison:
* **Steps:** 3,000 steps
* **Learning Rate:** 0.001 (with warmup & cosine decay)
* **Model Config:** `n_layer=2`, `n_head=4`, `n_embd=64`, `block_size=48`

### Loss Tracking Comparison
* **Baseline Run:**
  * Step 0: Training Loss = 4.9263 | Validation Loss = 4.9275
  * Step 1500: Training Loss = 0.6821 | Validation Loss = 0.7182
  * Step 3000: Training Loss = 0.6783 | Validation Loss = 0.7061
* **Expanded Corpus Run:**
  * Step 0: Training Loss = 5.5560 | Validation Loss = 5.5728
  * Step 1500: Training Loss = 0.7226 | Validation Loss = 0.7698
  * Step 3000: Training Loss = 0.7153 | Validation Loss = 0.7638

*Note: Validation losses between the baseline and expanded runs are not directly comparable because adding new teaching materials changed the corpus and expanded the vocabulary set.*

---

## 3. Evaluation Analysis & Extension Impact

### Key Improvements in Expanded Corpus:
1. **Vocabulary Coverage:** Adding targeted text expanded the scorable evaluation cases from 24 to 27, raising vocabulary coverage from 50.0% to 56.25%.
2. **Category Breakthrough (`opposites`):** 
   * **Baseline:** Scored **0/3 (0%)** due to lack of opposition pattern data in the core classroom corpus.
   * **Expanded:** Rose to **2/3 (66.7%)** accuracy after introducing specific teaching text (e.g., `opposites_teaching.txt`).
3. **Transfer Generalization (`starter_transfer` & `new_wording`):**
   * Baseline `starter_transfer`: 50.0% (4/8) $\rightarrow$ Expanded: **87.5% (7/8)**.
   * Baseline `new_wording`: 50.0% (4/8) $\rightarrow$ Expanded: **87.5% (7/8)**.

### Failure Analysis & Limitations:
* **Unscorable Categories:** Categories like `sequence`, `grammar`, and `categories_and_analogies` scored 0/3 because key words in those evaluation prompts remained outside the retained 509-token vocabulary.
* **Overfitting vs. Underfitting:** The validation loss flattened out around step 1500–3000 without sharply diverging from training loss, indicating stable convergence without severe overfitting.

---

## 4. Interactive Interface & Demonstration

The model can be interacted with using the command-line chat helper:

```bash
python chat.py
