# HAM10000 Skin Lesion Classification — Swin Tiny

This repository contains the training artifacts, evaluation results, and visualizations for a 7-class skin lesion classifier fine-tuned on the **HAM10000** dataset using a **Swin Transformer (Tiny)** backbone.

## Model

- **Base model:** [`timm/swin_tiny_patch4_window7_224.ms_in1k`](https://huggingface.co/timm/swin_tiny_patch4_window7_224.ms_in1k) (ImageNet-1k pretrained)
- **Checkpoint:** `best_swin_tiny_ham10000.pth` (best epoch by validation metric)
- **Split strategy:** lesion-level split (no lesion appears in both train and test, avoiding data leakage from multiple images of the same lesion)
- **Best epoch:** 54 (of 79 trained)

## Classes

The model predicts 7 diagnostic categories:

1. Actinic keratoses
2. Basal cell carcinoma
3. Benign keratosis
4. Dermatofibroma
5. Melanoma
6. Melanocytic nevi
7. Vascular lesions

## Test Set Results

| Metric | Value |
|---|---|
| Accuracy | 0.592 |
| Balanced accuracy | 0.262 |
| Macro F1 | 0.253 |
| Macro AUC | 0.546 |

**Per-class performance** (see `classification_report.txt` for full detail):

| Class | Precision | Recall | F1 | Support |
|---|---|---|---|---|
| Actinic keratoses | 0.00 | 0.00 | 0.00 | 51 |
| Basal cell carcinoma | 0.00 | 0.00 | 0.00 | 77 |
| Benign keratosis | 0.57 | 0.75 | 0.65 | 157 |
| Dermatofibroma | 0.00 | 0.00 | 0.00 | 22 |
| Melanoma | 0.34 | 0.39 | 0.36 | 168 |
| Melanocytic nevi | 0.83 | 0.70 | 0.76 | 1000 |
| Vascular lesions | 0.00 | 0.00 | 0.00 | 22 |

> ⚠️ **Class imbalance issue:** The dataset is heavily dominated by *Melanocytic nevi* (1000 of 1497 test samples). The model performs well on this majority class but fails entirely (0 precision/recall) on the four smallest classes (Actinic keratoses, Basal cell carcinoma, Dermatofibroma, Vascular lesions). This is reflected in the large gap between overall accuracy (0.59) and balanced/macro metrics (~0.25–0.26), and indicates the model needs further work (e.g., stronger class balancing, focal loss, oversampling, or more aggressive augmentation for minority classes) before it could be considered reliable across all diagnostic categories.

## Repository Contents

### Model
- `best_swin_tiny_ham10000.pth` — trained model weights (best checkpoint, ~105 MB)

### Metrics & Reports
- `classification_report.txt` — full per-class precision/recall/F1
- `final_summary.json` — headline test metrics and best epoch
- `training_history.json` — per-epoch train/val loss, accuracy, balanced accuracy, F1, and learning rate (79 epochs)

### Visualizations
- `training_curves.png` — training/validation loss and accuracy over epochs
- `confusion_matrix.png` — confusion matrix on the test set
- `roc_pr_curves.png` — ROC and precision-recall curves per class
- `misclassified_examples.png` — sample of misclassified test images
- `class_distribution_raw.png` — class distribution before balancing
- `class_distribution_balanced.png` — class distribution after balancing/resampling
- `sample_images_per_class.png` — example images from each diagnostic class
- `augmentation_examples.png` — examples of data augmentation applied during training
- `__results___files/` — additional inline plots exported from the training notebook

### Metadata
 `__huggingface_repos__.json` — reference to the pretrained backbone used (`timm/swin_tiny_patch4_window7_224.ms_in1k`)

## Notes

- Training ran for 79 epochs; the checkpoint saved corresponds to the best-performing epoch (54) on the validation set rather than the final epoch.
- Given the strong class imbalance, accuracy alone is a misleading metric for this model — refer to balanced accuracy, macro F1, and the per-class report when assessing real-world performance, especially for rarer but clinically important classes like melanoma and basal cell carcinoma.


