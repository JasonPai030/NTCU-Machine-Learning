# 挑戰一：監督式與非監督式學習比較

##  實驗目標
使用信用卡詐騙資料集，透過監督式與非監督式學習方法進行詐騙偵測，並比較其預測能力與分類效果。

---

##  資料集介紹
- 來源：Kaggle - Credit Card Fraud Detection
- 時間：2013年9月，歐洲持卡人兩日內的交易資料
- 筆數：284,807 筆，其中詐欺交易僅有 492 筆（約 0.172%），屬高度不平衡資料
- 欄位：經 PCA 處理的 V1~V28 及 'Amount' 欄位（'Time' 已移除）

---

##  模型實驗與結果

### 🔹 1. Random Forest（監督式學習）

- 使用 `train_test_split` 將資料分成 70% 訓練、30% 測試
- Amount 欄位進行標準化處理
- 使用 100 棵樹 (`n_estimators=100`)

**模型評估結果：**
```
              precision    recall  f1-score   support

           0       1.00      1.00      1.00     85307
           1       0.94      0.82      0.88       136

    accuracy                           1.00     85443
   macro avg       0.97      0.91      0.94     85443
weighted avg       1.00      1.00      1.00     85443
```

🔸 詐欺樣本的 precision 為 **0.94**，recall 為 **0.82**，效果相當不錯。

---

### 🔹 2. KMeans（非監督式學習）

- 使用 1000 筆正常樣本進行 KMeans 訓練
- 用 silhouette score 找到最佳 K 值為 2
- 對測試資料做預測並嘗試對齊 label

**模型評估結果：**
```
              precision    recall  f1-score   support

           0       1.00      1.00      1.00     85307
           1       0.00      0.00      0.00       136

    accuracy                           1.00     85443
   macro avg       0.50      0.50      0.50     85443
weighted avg       1.00      1.00      1.00     85443
```

🔸 無法正確偵測詐欺樣本，precision/recall 皆為 0，說明非監督學習對於此類高度不平衡資料較難有效分類。

---

## 心得與觀察

- Random Forest 作為監督式模型，在偵測詐騙交易時表現優異
- KMeans 在無標籤情況下難以發現詐騙交易，主要受限於詐騙樣本數過少與分布複雜
- 日後可嘗試使用 **非監督式做異常初篩 + 監督式精準分類**，以達到更好的偵測效能（對應挑戰二）

---


```