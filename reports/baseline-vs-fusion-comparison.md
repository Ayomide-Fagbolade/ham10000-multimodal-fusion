# Baseline vs. Multimodal Fusion — Results Comparison

> **Baseline**: EfficientNetV2-S (image-only)  
> **Fusion**: EfficientNetV2-S + Metadata (multimodal)  
> **Test set size**: 996 samples  
> **Optimization metric**: Macro F1 (averaged across all 7 classes, treating each equally)

---

## 1. Overall Metrics

| Metric | Baseline (Image-Only) | Fusion (Image+Meta) | Δ (Fusion − Baseline) |
|---|---|---|---|
| **Accuracy** | 86.65% | 86.95% | **+0.30 pp** |
| **Macro F1** ⭐ | 80.42% | 80.41% | −0.01 pp |
| **Weighted F1** | 86.79% | 87.26% | **+0.47 pp** |
| **Balanced Accuracy** | 83.42% | 84.04% | **+0.62 pp** |

> ⭐ = **Primary optimization metric** — both models were trained to maximize macro F1.

### Key Takeaways

- **Macro F1 (the optimization target) is essentially unchanged** at −0.01 pp, meaning the fusion model preserves the primary objective while redistributing performance across classes.
- The fusion model improves **accuracy** by +0.30 pp and **weighted F1** by +0.47 pp, indicating better performance on the majority class (`nv`).
- **Balanced accuracy** improves by +0.62 pp, showing the fusion model is slightly fairer across all classes despite class imbalance.
- Although macro F1 is flat, the **composition of per-class F1 shifts**: gains on `mel` (+3.3 pp) and `vasc` (+2.5 pp) are offset by losses on `df` (−3.8 pp) and `bcc` (−1.5 pp).

---

## 2. Per-Class Detailed Comparison

### 2.1 Per-Class F1-Score Summary

| Class | Full Name | Support | Baseline F1 | Fusion F1 | Δ F1 | Winner |
|---|---|---|---|---|---|---|
| akiec | Actinic keratoses | 33 | 81.16% | 80.60% | −0.56 pp | Baseline |
| bcc | Basal cell carcinoma | 52 | 84.21% | 82.76% | −1.45 pp | Baseline |
| bkl | Benign keratosis-like | 109 | 76.19% | 75.83% | −0.36 pp | Baseline |
| df | Dermatofibroma | 11 | 80.00% | 76.19% | −3.81 pp | Baseline |
| mel | Melanoma | 111 | 66.38% | 69.64% | **+3.26 pp** | **Fusion** |
| nv | Melanocytic nevi | 666 | 92.61% | 92.98% | **+0.37 pp** | **Fusion** |
| vasc | Vascular lesions | 14 | 82.35% | 84.85% | +2.50 pp | **Fusion** |

> **Fusion wins on 3/7 classes** — most notably `mel` (+3.3 pp) and `vasc` (+2.5 pp). Baseline wins on 4/7, but by smaller margins (< 1.5 pp each, except `df` at −3.8 pp on only 11 samples).

### 2.2 Per-Class Precision Summary

| Class | Full Name | Baseline Precision | Fusion Precision | Δ Precision | Winner |
|---|---|---|---|---|---|
| akiec | Actinic keratoses | 77.78% | 79.41% | +1.63 pp | **Fusion** |
| bcc | Basal cell carcinoma | 77.42% | 75.00% | −2.42 pp | Baseline |
| bkl | Benign keratosis-like | 79.21% | 78.43% | −0.78 pp | Baseline |
| df | Dermatofibroma | 88.89% | 80.00% | −8.89 pp | Baseline |
| mel | Melanoma | 63.64% | 63.24% | −0.40 pp | Baseline |
| nv | Melanocytic nevi | 93.97% | 95.56% | **+1.59 pp** | **Fusion** |
| vasc | Vascular lesions | 70.00% | 73.68% | +3.68 pp | **Fusion** |

### 2.3 Per-Class Recall Summary

| Class | Full Name | Baseline Recall | Fusion Recall | Δ Recall | Winner |
|---|---|---|---|---|---|
| akiec | Actinic keratoses | 84.85% | 81.82% | −3.03 pp | Baseline |
| bcc | Basal cell carcinoma | 92.31% | 92.31% | 0.00 pp | Tie |
| bkl | Benign keratosis-like | 73.39% | 73.39% | 0.00 pp | Tie |
| df | Dermatofibroma | 72.73% | 72.73% | 0.00 pp | Tie |
| mel | Melanoma | 69.37% | 77.48% | **+8.11 pp** | **Fusion** |
| nv | Melanocytic nevi | 91.29% | 90.54% | −0.75 pp | Baseline |
| vasc | Vascular lesions | 100.00% | 100.00% | 0.00 pp | Tie |

> **The standout result**: melanoma recall jumps from 69.4% → 77.5% (+8.1 pp). This is clinically significant — 9 additional melanomas are correctly identified by the fusion model.

### 2.4 Full Per-Class Breakdown

| Class | Label | Metric | Baseline | Fusion | Δ |
|---|---|---|---|---|---|
| akiec | 0 | Precision | 0.7778 | 0.7941 | +0.0163 |
| akiec | 0 | Recall | 0.8485 | 0.8182 | −0.0303 |
| akiec | 0 | F1 | 0.8116 | 0.8060 | −0.0056 |
| bcc | 1 | Precision | 0.7742 | 0.7500 | −0.0242 |
| bcc | 1 | Recall | 0.9231 | 0.9231 | 0.0000 |
| bcc | 1 | F1 | 0.8421 | 0.8276 | −0.0145 |
| bkl | 2 | Precision | 0.7921 | 0.7843 | −0.0078 |
| bkl | 2 | Recall | 0.7339 | 0.7339 | 0.0000 |
| bkl | 2 | F1 | 0.7619 | 0.7583 | −0.0036 |
| df | 3 | Precision | 0.8889 | 0.8000 | −0.0889 |
| df | 3 | Recall | 0.7273 | 0.7273 | 0.0000 |
| df | 3 | F1 | 0.8000 | 0.7619 | −0.0381 |
| mel | 4 | Precision | 0.6364 | 0.6324 | −0.0040 |
| mel | 4 | Recall | 0.6937 | 0.7748 | **+0.0811** |
| mel | 4 | F1 | 0.6638 | 0.6964 | **+0.0326** |
| nv | 5 | Precision | 0.9397 | 0.9556 | **+0.0159** |
| nv | 5 | Recall | 0.9129 | 0.9054 | −0.0075 |
| nv | 5 | F1 | 0.9261 | 0.9298 | **+0.0037** |
| vasc | 6 | Precision | 0.7000 | 0.7368 | +0.0368 |
| vasc | 6 | Recall | 1.0000 | 1.0000 | 0.0000 |
| vasc | 6 | F1 | 0.8235 | 0.8485 | +0.0250 |

### 2.5 Per-Class Key Takeaways

- **`mel` (melanoma)** — The biggest gain: recall jumps from 69.4% → 77.5% (+8.1 pp) and F1 from 66.4% → 69.6% (+3.3 pp). This is clinically significant — more melanomas are correctly identified.
- **`nv` (melanocytic nevi)** — Precision improves from 93.97% → 95.56%, contributing to the overall weighted F1 gain. F1 also edges up +0.37 pp.
- **`vasc` (vascular lesions)** — F1 improves from 82.4% → 84.8% with a precision boost of +3.7 pp. Recall stays perfect at 100%.
- **`akiec` (actinic keratoses)** — Precision improves slightly (+1.6 pp) but recall drops (−3.0 pp), resulting in a small net F1 loss (−0.56 pp).
- **`bcc` (basal cell carcinoma)** — Recall unchanged at 92.3%, but precision drops (−2.4 pp), leading to a −1.45 pp F1 decrease.
- **`bkl` (benign keratosis-like)** — Nearly identical performance; F1 drops by only −0.36 pp.
- **`df` (dermatofibroma)** — Largest F1 drop (−3.81 pp) driven by a precision decline (−8.9 pp), though this class has only 11 test samples so the result is noisy.

---

## 3. Confusion Matrix Comparison

### Baseline (Image-Only)

| | akiec | bcc | bkl | df | mel | nv | vasc |
|---|---|---|---|---|---|---|---|
| **akiec** | 28 | 3 | 0 | 1 | 0 | 1 | 0 |
| **bcc** | 0 | 48 | 1 | 0 | 2 | 1 | 0 |
| **bkl** | 5 | 3 | 80 | 0 | 10 | 10 | 1 |
| **df** | 0 | 1 | 0 | 8 | 0 | 2 | 0 |
| **mel** | 1 | 0 | 6 | 0 | 77 | 25 | 2 |
| **nv** | 2 | 7 | 14 | 0 | 32 | 608 | 3 |
| **vasc** | 0 | 0 | 0 | 0 | 0 | 0 | 14 |

### Fusion (Image + Metadata)

| | akiec | bcc | bkl | df | mel | nv | vasc |
|---|---|---|---|---|---|---|---|
| **akiec** | 27 | 3 | 1 | 1 | 0 | 1 | 0 |
| **bcc** | 0 | 48 | 1 | 0 | 2 | 1 | 0 |
| **bkl** | 5 | 4 | 80 | 1 | 12 | 7 | 0 |
| **df** | 0 | 1 | 0 | 8 | 0 | 2 | 0 |
| **mel** | 1 | 0 | 5 | 0 | 86 | 17 | 2 |
| **nv** | 1 | 8 | 15 | 0 | 36 | 603 | 3 |
| **vasc** | 0 | 0 | 0 | 0 | 0 | 0 | 14 |

### Notable Differences

| True Class | Change | Detail |
|---|---|---|
| **mel → mel** | 77 → **86** (+9) | Fusion correctly classifies 9 more melanoma cases |
| **mel → nv** | 25 → **17** (−8) | 8 fewer melanomas are misclassified as nevi |
| **nv → mel** | 32 → **36** (+4) | Slight increase in nevi misclassified as melanoma (false positives) |
| **nv → nv** | 608 → **603** (−5) | 5 fewer nevi correctly classified |
| **bkl → mel** | 10 → **12** (+2) | 2 more bkl misclassified as mel |
| **bkl → nv** | 10 → **7** (−3) | 3 fewer bkl misclassified as nv |

---

## 4. Summary

| Aspect | Winner | Notes |
|---|---|---|
| **Macro F1** (optimization target) | ≈ Tie | −0.01 pp (negligible) |
| Overall Accuracy | **Fusion** | +0.30 pp |
| Weighted F1 | **Fusion** | +0.47 pp |
| Balanced Accuracy | **Fusion** | +0.62 pp |
| Melanoma Recall | **Fusion** | +8.1 pp — clinically meaningful |
| Melanoma F1 | **Fusion** | +3.3 pp |
| NV Precision | **Fusion** | +1.6 pp |
| Minority classes (df, bcc, akiec) | **Baseline** | Slightly better F1 on rare classes |

### Conclusion

Both models were optimized for **macro F1**, which is essentially tied (80.42% vs 80.41%). This static macro F1 score effectively shows that the significant improvements in **melanoma (`mel`) recall** (jumping from 69.37% to 77.48%, an 8.1 percentage point increase) were "paid for" by minor performance losses elsewhere, such as small drop-offs in F1 for rarer classes like dermatofibroma (`df`) and basal cell carcinoma (`bcc`).

From a clinical screening perspective, this is a highly beneficial redistribution. Missed melanomas are life-threatening, whereas false alarms on benign lesions or minor drops on extremely rare conditions have minor consequences. Thus, while the headline optimization metric shows no net change, the fusion model represents a major clinical advancement.

### Limitation & Suggestions

#### 1. Limitations of Tuning on Macro F1
- **Unweighted Penalization**: Macro F1 treats all classes equally regardless of clinical urgency or support size. This means a performance swing on a tiny, benign class (such as `df` with only 11 test samples) is penalized just as heavily as a drop in detecting highly malignant melanoma (`mel`).
- **Zero-Sum Trade-off**: Standard multi-class training and tuning can result in a zero-sum game under a symmetric metric like Macro F1, where model capacity shifts and positive gains in critical categories are mechanically balanced out by losses in noisy categories.

#### 2. Actionable Suggestions for Future Work
- **Asymmetric Optimization ($F_{\beta}$ Tuning)**: Instead of symmetric F1, future training and hyperparameter sweeps should optimize for an $F_{\beta}$ metric (e.g., $F_2$ or $F_3$) specifically for malignant classes, which explicitly weighs recall multiple times higher than precision.
- **Cost-Sensitive Loss Functions**: Integrate a clinical utility or cost matrix directly into the cross-entropy loss function. This would apply a larger penalty for misclassifying malignant lesions as benign compared to misclassifying benign lesions as malignant.
- **Constrained Optimization**: Tune ensemble weights and hyperparameters under a hard threshold or constraint (e.g., maximize Macro F1 *subject to* a minimum Melanoma Recall of 80%).

---

## 5. Ensemble Results

The ensemble evaluation was performed by combining the predictions of the baseline (image-only) and fusion (image + metadata) models using learned weights. The best performing configuration, which maximized macro‑F1 on the validation set, achieved the following test-set performance:

### 5.1 Overall Metrics

| Metric | Ensemble (Image + Meta Fusion) |
|---|---|
| **Accuracy** | 87.05% |
| **Macro F1** ⭐ | 80.22% |
| **Weighted F1** | 87.24% |
| **Balanced Accuracy** | 83.52% |

### 5.2 Per-Class Detailed Comparison

#### 5.2.1 Per-Class F1-Score Summary

| Class | Full Name | Support | Ensemble F1 |
|---|---|---|---|
| akiec | Actinic keratoses | 33 | 79.41% |
| bcc | Basal cell carcinoma | 52 | 83.48% |
| bkl | Benign keratosis-like | 109 | 75.60% |
| df | Dermatofibroma | 11 | 76.19% |
| mel | Melanoma | 111 | 68.91% |
| nv | Melanocytic nevi | 666 | 93.12% |
| vasc | Vascular lesions | 14 | 84.85% |

#### 5.2.2 Per-Class Precision Summary

| Class | Full Name | Ensemble Precision |
|---|---|---|
| akiec | Actinic keratoses | 77.14% |
| bcc | Basal cell carcinoma | 76.19% |
| bkl | Benign keratosis-like | 79.00% |
| df | Dermatofibroma | 80.00% |
| mel | Melanoma | 64.57% |
| nv | Melanocytic nevi | 94.86% |
| vasc | Vascular lesions | 73.68% |

#### 5.2.3 Per-Class Recall Summary

| Class | Full Name | Ensemble Recall |
|---|---|---|
| akiec | Actinic keratoses | 81.82% |
| bcc | Basal cell carcinoma | 92.31% |
| bkl | Benign keratosis-like | 72.48% |
| df | Dermatofibroma | 72.73% |
| mel | Melanoma | 73.87% |
| nv | Melanocytic nevi | 91.44% |
| vasc | Vascular lesions | 100.00% |

### 5.3 Confusion Matrix

| | akiec | bcc | bkl | df | mel | nv | vasc |
|---|---|---|---|---|---|---|---|
| **akiec** | 27 | 3 | 1 | 1 | 0 | 1 | 0 |
| **bcc** | 0 | 48 | 1 | 0 | 2 | 1 | 0 |
| **bkl** | 5 | 4 | 79 | 1 | 12 | 8 | 0 |
| **df** | 0 | 1 | 0 | 8 | 0 | 2 | 0 |
| **mel** | 1 | 0 | 5 | 0 | 82 | 21 | 2 |
| **nv** | 2 | 7 | 14 | 0 | 31 | 609 | 3 |
| **vasc** | 0 | 0 | 0 | 0 | 0 | 0 | 14 |

## 6. Final Model Selection

Using macro-F1 as the primary optimization metric, the image-only baseline is marginally the best model, with macro-F1 = 80.42% compared with 80.41% for the multimodal fusion model and 80.22% for the validation-selected ensemble. However, the difference between baseline and fusion is only 0.01 percentage points and is therefore practically negligible.

The fusion model provides the strongest clinical trade-off. It preserves macro-F1 while improving accuracy, weighted F1, balanced accuracy, and most importantly melanoma recall. Melanoma recall increases from 69.37% to 77.48%, meaning 9 additional melanoma cases are correctly identified on the held-out test set. This improvement comes at the cost of a small increase in benign lesions, especially nevi, being classified as melanoma.

The validation-selected ensemble achieved the highest accuracy at 87.05%, but its macro-F1 of 80.22% is lower than both individual models. Therefore, the ensemble is not selected as the final model under the macro-F1 criterion. It is best interpreted as an accuracy-improving variant rather than a macro-F1-improving model.

Overall, if strict metric selection is required, the baseline is the best model by macro-F1. However in clinical screening utility is prioritized, the multimodal fusion model is preferable because it substantially improves melanoma sensitivity while maintaining nearly identical macro-F1.

| Metric | Baseline | Fusion | Ensemble | Best |
|---|---:|---:|---:|---|
| Accuracy | 86.65% | 86.95% | **87.05%** | Ensemble |
| Macro F1 | **80.42%** | 80.41% | 80.22% | Baseline |
| Weighted F1 | 86.79% | **87.26%** | 87.24% | Fusion |
| Balanced Accuracy | 83.42% | **84.04%** | 83.52% | Fusion |
| Melanoma Recall | 69.37% | **77.48%** | 73.87% | Fusion |
| Melanoma F1 | 66.38% | **69.64%** | 68.91% | Fusion |
