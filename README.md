
# Roof Classification from Sentinel Satellite Imagery

## Mô tả dự án

Dự án sử dụng các kỹ thuật học máy để phân biệt **mái nhà** và **không phải mái nhà** dựa trên dữ liệu ảnh vệ tinh từ Sentinel-1 (SAR) và Sentinel-2 (quang học).  
Dữ liệu được biểu diễn theo từng **pixel** với các thông tin đặc trưng bao gồm:

- Các bands: `VV`, `VH` (Sentinel-1), `B2`, `B3`, `B4` (Sentinel-2)
- Tọa độ địa lý: `lat`, `long`
- Nhãn (`label`): mái nhà (`1`) hoặc không phải mái nhà (`0`)

## Cấu trúc dữ liệu

| Lat  | Long | VV   | VH   | B2   | B3   | B4   | Label |
|------|------|------|------|------|------|------|-------|
| ...  | ...  | ...  | ...  | ...  | ...  | ...  | 0/1   |

Tổng cộng có **24,000 dòng**, được chia thành **80% train** và **20% test** theo tỷ lệ phân bố nhãn.

---

## Các mô hình sử dụng

### 1. **Random Forest**
- `n_estimators = 100`
- `random_state = 42`

> Ưu điểm: ổn định, xử lý tốt dữ liệu có nhiều đặc trưng  
> Nhược điểm: khó giải thích, chạy chậm hơn nếu quá nhiều cây  

### 2. **Logistic Regression**
- `solver='liblinear'`, `penalty='l2'`, `C=1.0`

> Ưu điểm: đơn giản, dễ giải thích, chạy nhanh  
> Nhược điểm: giới hạn trong mô hình tuyến tính  

### 3. **Support Vector Machine (SVM)**
- `kernel='rbf'`, `C=1.0`, `gamma='scale'`

> Ưu điểm: mô hình mạnh trong không gian phức tạp  
> Nhược điểm: khó giải thích, tốn tài nguyên với dataset lớn  

---

## Các bước xử lý

1. **Tiền xử lý dữ liệu**
   - Gộp dữ liệu Sentinel-1 và Sentinel-2
   - Loại outlier bằng boxplot
   - Gán nhãn nhị phân cho `label`
   - Tách `X` (features) và `y` (labels)

2. **Chia tập train-test (80:20)**  
   ```python
   from sklearn.model_selection import train_test_split
   X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, stratify=y)
   ```

3. **Huấn luyện mô hình**  
   - Với mỗi mô hình: fit → predict → đánh giá (confusion matrix, accuracy, recall, precision, F1-score)
   - Có vẽ biểu đồ **confusion matrix** và **ROC curve**

---

## 📈 Kết quả (ví dụ với Random Forest)

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

## Công nghệ sử dụng

- Python (pandas, scikit-learn, matplotlib, seaborn)
- Jupyter Notebook
- Dữ liệu Sentinel xử lý trước

---

## Hướng mở rộng

- Thử nghiệm thêm các thuật toán như XGBoost, LightGBM
- Dùng mô hình deep learning (CNN) trực tiếp từ ảnh
- Tối ưu hyperparameter với GridSearchCV
- Tích hợp trên bản đồ với toạ độ GPS

---

## Người thực hiện

- Tên: Phạm Tuấn Đạt
- Mã sinh viên: 22028218
- Liên hệ: ptdat46work@gmail.com
