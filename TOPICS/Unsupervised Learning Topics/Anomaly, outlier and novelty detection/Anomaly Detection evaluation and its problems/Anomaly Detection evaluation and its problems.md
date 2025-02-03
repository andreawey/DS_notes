[[Anomaly, outlier and novelty detection]]

# Evaluating Anomaly Detection (AD)

Evaluating AD is inherently difficult because it is challenging to obtain datasets with high-quality anomalies. Even if such a dataset is collected, it is unclear whether all possible anomalies have been covered. An easy way to create an AD dataset is to synthetically create or corrupt data. However, such data may not be representative of true anomalous data. The only other known way to create AD data is to manually label anomalies by human experts.

In many applications, the cost of false alarms and missed anomalies are different. Thus, the anomaly score threshold has to be tuned appropriately, giving priority to **false positives (FP)**, **false negatives (FN)**, **true positives (TP)**, and **true negatives (TN)** accordingly.

## Common Measures for AD

- **AUROC (or AUC)**: The most common measure for AD.
- [[F-score]]: 
  $$
  \text{F-Score} = \frac{TP}{TP + 0.5(FP + FN)}
  $$  
  The F-Score is frequently used to evaluate binary classifiers when the two classes have semantically different meanings.

[[Precision, Recall, Accuracy]]
- **Precision**: 
  $$
  \text{Precision} = \frac{TP}{TP + FP}
  $$

- **Recall**: 
  $$
  \text{Recall} = \frac{TP}{TP + FN}
  $$

- **Accuracy**: 
  $$
  \text{Accuracy} = \frac{TP + TN}{TP + FP + TN + FN}
  $$

## Probabilistic and Reconstruction-based Methods

Probabilistic and reconstruction-based methods for AD can directly provide a score for quantifying the degree of anomality.
