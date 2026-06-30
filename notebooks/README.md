# Notebooks Overview

This directory contains the Jupyter notebooks used for the **ham10000‑multimodal‑fusion** project. Each notebook focuses on a specific stage of the workflow, from data exploration to model training and evaluation.

## 1️⃣ `01-ham10000-data-exploration-and-splits.ipynb`

* **Purpose** – Explore the HAM10000 skin‑lesion dataset, clean the metadata, and create reproducible train/validation/test splits.
* **Key steps**
	- Load the raw CSV metadata and inspect basic statistics (missing values, class distribution).
	- Visualise the distribution of diagnoses and compute class‑wise percentages.
	- Map image IDs to their file paths across the two image folders.
	- Remove records with missing age (57 rows) and encode the diagnosis labels.
	- Perform stratified splits (80 % train, 10 % validation, 10 % test) ensuring class balance.
	- Save the cleaned metadata and split CSV files (`metadata_clean.csv`, `train_split.csv`, `val_split.csv`, `test_split.csv`) along with a `label_mapping.csv` for later use.

## 2️⃣ `ham10000-image-baseline.ipynb`

* **Purpose** – Train a baseline image‑only classifier using an EfficientNet‑V2‑S backbone.
* **Key steps**
	- Define a custom `HAM10000ImageDataset` that loads images and labels.
	- Set up data augmentations (random flips, rotations, normalization) and create `DataLoader`s for train/val/test.
	- Replace the final classifier layer of EfficientNet‑V2‑S with a linear layer matching the number of classes.
	- Use a weighted cross‑entropy loss to mitigate class imbalance, and train with AdamW optimizer.
	- Implement early stopping based on validation macro‑F1 and save the best checkpoint (`best_efficientnetv2s_image_only_ham10000.pt`).
	- After training, evaluate on the test set, compute accuracy, macro‑F1, weighted‑F1, balanced accuracy, generate a classification report and confusion matrix, and persist these results as CSV files.

---

Both notebooks are designed to be run sequentially: first run the data‑exploration notebook to generate the split files, then run the image‑baseline notebook to train and evaluate the model.


