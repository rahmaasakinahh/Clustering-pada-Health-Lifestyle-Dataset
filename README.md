# **Clustering on Health & Lifestyle Dataset**
---

## Dataset
---

[Health and Lifestyle Dataset](https://www.kaggle.com/datasets/chik0di/health-and-lifestyle-dataset/data) — 100.000 sampel, 16 fitur (usia, BMI, langkah kaki per hari, jam tidur, kadar kolesterol, dll).

---

## Metode

- K-Means Clustering
- Hierarchical Clustering (Agglomerative)
- DBSCAN (Density-Based Clustering)

---

## Alur Pengerjaan

### 1. EDA
Dicek distribusi fitur, shape dataset, dan correlation heatmap. Tidak ada missing values. Semua fitur terdistribusi merata dengan korelasi antar kolom yang hampir nol — menandakan tidak ada cluster alami yang terlalu jelas dari awal.

### 2. Preprocessing
- Hapus outlier menggunakan metode IQR
- Normalisasi fitur dengan `StandardScaler` agar kolom berskala besar tidak mendominasi perhitungan jarak

### 3. K-Means
- Nilai K optimal dicari pakai Elbow Method dan Silhouette Score
- K optimal = 2, menghasilkan dua cluster seimbang (~50k data per cluster)

### 4. Hierarchical Clustering
- Sampling 10.000 data karena keterbatasan komputasi
- Dicoba 4 linkage: Single, Complete, Ward, Average
- Jumlah cluster ditentukan dari cut-off dendrogram yang dihitung otomatis
- Linkage terbaik: **Single** (Silhouette Score tertinggi, meski rentan chaining effect)

### 5. DBSCAN
- Epsilon optimal dicari lewat K-Distance Graph
- Tuning epsilon di berbagai nilai (0.5 – 1.46)
- Hasil terbaik: `eps=1.4` → 2 cluster, 1.149 data terdeteksi sebagai noise (1.15%)

---

## Evaluasi

| Metode | Silhouette ↑ | Davies-Bouldin ↓ | Calinski-Harabasz ↑ |
|---|---|---|---|
| K-Means | 0.0747 | — | 8119.83 |
| Hierarchical | — | 0.8624 | — |
| DBSCAN | 0.0993 | — | — |

---

## Kesimpulan

Ketiga metode menghasilkan pemisahan cluster yang tidak terlalu kontras — konsisten dengan hasil EDA yang menunjukkan distribusi merata dan korelasi antar fitur sangat rendah.

- **DBSCAN** unggul di Silhouette Score dan bisa deteksi noise secara otomatis
- **Hierarchical** unggul di Davies-Bouldin, tapi kurang representatif karena chaining effect pada single linkage
- **K-Means** jadi pilihan paling cocok untuk dataset ini — cepat di 100k data, cluster seimbang, dan tidak butuh tuning yang rumit

---

## Tech Stack

- Python
- pandas, NumPy
- scikit-learn
- Matplotlib, Seaborn
- Jupyter Notebook
- Matplotlib, Seaborn
- Jupyter Notebook
