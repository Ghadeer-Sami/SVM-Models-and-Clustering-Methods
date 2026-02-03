# Machine Learning – SVM Models and Clustering Methods

This assignment explores Support Vector Machine (SVM) models and unsupervised clustering methods (K-Means and Hierarchical Clustering) on two datasets:  
**Breast Cancer Diagnostic Dataset** and **Adult Income Dataset**.

---

# Breast Cancer Dataset

## Loading and Preprocessing
- Loaded the dataset from the UCI repository using `ucimlrepo`.
- No missing values.
- Applied `StandardScaler` because SVM is sensitive to feature ranges.
- Split the dataset into 80% training and 20% testing.

---

## Training SVM Models
Three SVM models were trained using different kernels:
- Linear SVM  
- RBF SVM  
- Polynomial SVM  

Each model was trained on the scaled training data and evaluated on the test set.

---

## Evaluation Results

### Performance Metrics

| Model        | Accuracy | Recall | Precision | F1 Score |
|--------------|----------|--------|-----------|----------|
| Linear SVM   | 95.6%    | 88.4%  | 100%      | 0.938    |
| RBF SVM      | 95.6%    | 88.4%  | 100%      | 0.938    |
| Poly SVM     | 88.6%    | 69.8%  | 100%      | 0.822    |

**Observation:**  
Linear and RBF kernels performed identically, indicating the data is linearly separable.  
Polynomial SVM performed worse, especially in recall.

---

## Confusion Matrix (Linear & RBF)

- TN = 71  
- FP = 0  
- FN = 5  
- TP = 38  

The models made **no false positives**, which is ideal for medical datasets, but missed 5 malignant cases (FN).

---

## ROC Curve & AUC
- Linear SVM: **AUC = 0.982**  
- RBF SVM: **AUC = 0.979**  
- Polynomial SVM: **AUC = 0.971**

Linear kernel achieved the highest AUC.  
The dataset is small, clean, and linearly separable, so simple models outperform complex ones.

---

## Hyperparameter Tuning
- **Linear SVM:** Tuned `C = [0.1, 1, 10]`  
- **RBF SVM:** Tuned combinations of `C = [0.1, 1, 10]` and `gamma = ['scale', 'auto']`  
- **Polynomial SVM:** Tuned `C = [0.1, 1, 10]` and degrees `[2, 3, 4]`

Findings:
- Linear SVM was stable with lower C.
- RBF performed best with `C = 1` and `gamma = 'scale'`.
- Polynomial kernel was the most sensitive and prone to overfitting.

---

# Unsupervised Learning – Breast Cancer Dataset

## K-Means Clustering
- Used `k = 2` (two classes: malignant, benign).
- Accuracy: **~91%**
- Confusion matrix showed:
  - 244 benign correctly clustered  
  - 175 malignant correctly clustered  
  - 13 benign misclassified  
  - 37 malignant misclassified  

K-Means performed surprisingly well for an unsupervised method.

---

## Hierarchical Clustering
- Applied both **single linkage** and **complete linkage**.
- Accuracy: **63.1%**
- Much weaker than K-Means.
- Complete linkage produced more balanced clusters; single linkage produced a chain-like structure.

---

## PCA Visualization (Breast Cancer)
- True labels showed natural separation.
- K-Means clusters aligned well with true labels.
- Hierarchical clustering showed significant overlap.

---

# Adult Income Dataset

## Loading and Preprocessing
- Loaded from UCI repository.
- Replaced `?` with NaN and removed missing rows.
- Encoded categorical features using one-hot encoding.
- Scaled all features using `StandardScaler`.
- Split into 80% training and 20% testing.

---

## Training SVM Models
Trained:
- Linear SVM  
- RBF SVM  
- Polynomial SVM  

Reason for testing multiple kernels:
- Linear: good for linearly separable data  
- RBF: flexible for nonlinear boundaries  
- Polynomial: captures complex patterns but may overfit  

---

## Evaluation Results

| Model        | Accuracy | Recall | Precision | F1 Score |
|--------------|----------|--------|-----------|----------|
| Linear SVM   | 84.73%   | 58.1%  | 76.2%     | 65.9%    |
| RBF SVM      | 84.81%   | 57.1%  | 77.2%     | 65.7%    |
| Poly SVM     | 82.67%   | 49.0%  | 74.6%     | 59.2%    |

Linear and RBF performed similarly.  
Polynomial kernel performed the weakest.

---

## Confusion Matrix (Adult Dataset)

**Linear SVM**
- TN = 6328  
- FP = 417  
- FN = 964  
- TP = 1336  

**RBF SVM**
- TN = 6358  
- FP = 387  
- FN = 987  
- TP = 1313  

Both models struggled with the >50K class (lower recall).

---

## ROC Curve & AUC
- Linear SVM: **0.907**  
- RBF SVM: **0.897**  
- Polynomial SVM: **0.874**

Linear kernel performed best overall.

---

## Hyperparameter Tuning (Adult Dataset)
- Linear: best with `C = 0.1`
- RBF: best with `C = 1`, `gamma = 'scale'`
- Polynomial: best with `C = 10`, degree = 2 (still weaker)

---

# Unsupervised Learning – Adult Dataset

## K-Means
- Accuracy: **75.78%**
- Strong bias toward majority class.
- Correctly identified most <=50K cases.
- Misclassified most >50K cases.
- Shows why accuracy alone is misleading in imbalanced datasets.

---

## Hierarchical Clustering
- Full dataset caused crashes due to size.
- Used sampling (10%) to run clustering.
- Accuracy was low and unstable.
- Highly sensitive to sampling ratio.
- Complete linkage produced slightly better structure but still weak.

---

## PCA Visualization (Adult Dataset)
- True labels showed heavy overlap.
- K-Means captured some structure but misclassified many samples.
- Hierarchical clustering collapsed into one dominant cluster.
- PCA confirmed why clustering methods struggled.

---

# Comparative Analysis

## 1. SVM vs K-Means vs Hierarchical (Both Datasets)
- **SVM** consistently performed best due to supervised learning.
- **K-Means** performed well on Cancer dataset but poorly on Adult dataset.
- **Hierarchical** performed the weakest overall.

## 2. Which Method Separates Classes Better?
- **Cancer dataset:** SVM (Linear) separated classes almost perfectly.  
- **Adult dataset:** SVM again performed best; clustering methods struggled.

## 3. Effect of Preprocessing
- Scaling significantly improved SVM and K-Means.
- Encoding was essential for Adult dataset.
- Preprocessing directly shaped model performance.

## 4. Recommended Method
- **Breast Cancer:** Linear SVM  
- **Adult Income:** Linear or RBF SVM  
- Clustering methods are not recommended for final classification.

---
