# Roof Classification from Sentinel Satellite Imagery

## Project Description

This project uses machine learning techniques to distinguish **roof** and **non-roof** areas based on satellite imagery data from Sentinel-1 (SAR) and Sentinel-2 (optical).  
The data is represented per **pixel** with the following features:

- Bands: `VV`, `VH` (Sentinel-1), `B2`, `B3`, `B4` (Sentinel-2)
- Geographic coordinates: `lat`, `long`
- Label (`label`): roof (`1`) or non-roof (`0`)

## Data Structure

| Lat  | Long | VV   | VH   | B2   | B3   | B4   | Label |
|------|------|------|------|------|------|------|-------|
| ...  | ...  | ...  | ...  | ...  | ...  | ...  | 0/1   |

A total of **24,000 rows**, split into **80% train** and **20% test** according to label distribution.

---

## Models Used

### 1. **Random Forest**
- `n_estimators = 100`
- `random_state = 42`

> Advantages: stable, handles data with many features well  
> Disadvantages: hard to interpret, slower if too many trees  

### 2. **Logistic Regression**
- `solver='liblinear'`, `penalty='l2'`, `C=1.0`

> Advantages: simple, easy to interpret, fast  
> Disadvantages: limited to linear models  

### 3. **Support Vector Machine (SVM)**
- `kernel='rbf'`, `C=1.0`, `gamma='scale'`

> Advantages: powerful in complex spaces  
> Disadvantages: hard to interpret, resource-intensive with large datasets  

---

## Processing Steps

1. **Data Preprocessing**
   - Merge Sentinel-1 and Sentinel-2 data
   - Remove outliers using boxplot
   - Assign binary labels to `label`
   - Split into `X` (features) and `y` (labels)

2. **Train-test Split (80:20)**  
   ```python
   from sklearn.model_selection import train_test_split
   X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, stratify=y)
   ```

3. **Model Training**  
   - For each model: fit → predict → evaluate (confusion matrix, accuracy, recall, precision, F1-score)
   - Plot **confusion matrix** and **ROC curve**

---

## 📈 Results (example with Random Forest)

- **Accuracy**: 98.88%  
- **Recall**: 99.14%  
- **Precision**: 99.14%  
- **F1-score**: 99.14%  
- Confusion Matrix:

```
[[1656   27]
 [  27 3111]]
```

---

## Technologies Used

- Python (pandas, scikit-learn, matplotlib, seaborn)
- Jupyter Notebook
- Pre-processed Sentinel data

---

## Future Directions

- Experiment with additional algorithms such as XGBoost, LightGBM
- Use deep learning models (CNN) directly on images
- Hyperparameter optimization with GridSearchCV
- Integrate with maps using GPS coordinates

---

## Author

- Name: Pham Tuan Dat
- Student ID: 22028218
- Contact: ptdat46work@gmail.com
