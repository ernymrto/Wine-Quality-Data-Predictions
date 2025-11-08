# Wine-Quality-Data-Predictions

# 🍷 Proyek UTS Data Mining: Klasifikasi Kualitas Anggur (Wine Quality)

Repositori ini berisi pengerjaan Ujian Tengah Semester (UTS) mata kuliah Data Mining. Tujuan dari proyek ini adalah untuk membangun model klasifikasi yang dapat memprediksi kualitas anggur (`quality`) berdasarkan 11 fitur kimiawinya.

## 🚀 Latar Belakang Masalah

Dataset "Wine Quality" memiliki tantangan utama yaitu **Class Imbalance** (ketidakseimbangan kelas). Sebagian besar anggur memiliki kualitas rata-rata (skor 5 dan 6), sementara anggur berkualitas sangat baik (skor 8) atau sangat buruk (skor 3) jumlahnya sangat sedikit.

Tantangan kedua adalah **Class Overlap**, di mana fitur-fitur kimiawi dari anggur berkualitas 5 dan 6 sangat mirip, sehingga sulit bagi model untuk membedakannya.

## 📊 Dataset
Berikut adalah penjelasan untuk setiap fitur (kolom) dalam dataset:

| Fitur | Deskripsi |
| :--- | :--- |
| **fixed acidity** | Kebanyakan asam yang terlibat dalam anggur (asam non-volatil/tidak mudah menguap). |
| **volatile acidity** | Jumlah asam asetat (cuka) dalam anggur. |
| **citric acid** | Jumlah asam sitrat dalam anggur. |
| **residual sugar** | Jumlah gula yang tersisa setelah proses fermentasi berhenti. |
| **chlorides** | Jumlah garam (klorida) dalam anggur. |
| **free sulfur dioxide** | Jumlah sulfur dioksida bebas (SO2) yang tersedia untuk bereaksi (bersifat anti-mikroba dan anti-oksidan). |
| **total sulfur dioxide** | Jumlah total SO2 (bentuk bebas dan terikat). |
| **density** | Ukuran seberapa padat material anggur (massa per volume). |
| **pH** | Menggambarkan seberapa asam atau basa anggur pada skala 0 (sangat asam) hingga 14 (sangat basa). Kebanyakan anggur ada di antara 3-4. |
| **alcohol** | Persentase kandungan alkohol dalam anggur. |
| **quality** | **(Variabel Target)** Skor kualitas berdasarkan data sensorik (penilaian manusia), dengan skor antara 3 (buruk) dan 8 (sangat baik). |

Proyek ini menggunakan dua file data:
* `data_training.csv`: Berisi 800+ sampel data dengan fitur dan label `quality` yang digunakan untuk melatih model.
* `data_testing.csv`: Berisi 700+ sampel data dengan fitur, yang label `quality`-nya harus diprediksi.

## 🛠️ Alur Kerja (Workflow)

Proses pengerjaan proyek ini dibagi menjadi beberapa tahap utama:

### 1. Pembersihan Data (Data Cleaning)
* **Hapus Kolom `Id`**: Kolom `Id` dihapus dari data training dan testing karena tidak relevan untuk prediksi.
* **Hapus Duplikat**: Data duplikat di `data_training.csv` diidentifikasi dan dihapus untuk mencegah *data leakage* dan model yang *overfitting*.

### 2. Pra-pemrosesan (Preprocessing)
* **Imputasi**: Menggunakan `SimpleImputer` dengan strategi `mean` untuk mengisi data yang hilang (jika ada), meskipun pada dataset ini tidak ditemukan data *null*.
* **Feature Scaling**: Menggunakan `StandardScaler` untuk menstandarisasi semua fitur. Ini sangat penting karena rentang nilai fitur sangat bervariasi (misalnya, `total sulfur dioxide` vs `chlorides`).

### 3. Pemodelan & Optimasi
* **Model Pilihan**: `RandomForestClassifier` dipilih karena kekuatannya dalam menangani data tabular dan kemampuannya untuk mengabaikan fitur yang tidak penting.
* **Penanganan Imbalance**: Daripada menggunakan SMOTE, strategi `class_weight='balanced'` diterapkan langsung pada model. Ini adalah cara yang efisien untuk "menghukum" model jika salah menebak kelas minoritas.
* **Hyperparameter Tuning**: Menggunakan `RandomizedSearchCV` untuk secara otomatis mencari kombinasi parameter terbaik (seperti `n_estimators`, `max_depth`, `min_samples_leaf`) agar didapat model dengan performa paling optimal.

### 4. Evaluasi Model (Evaluasi Jujur)
* Sebelum melatih model final, data training dibagi menjadi `train_set` (80%) dan `validation_set` (20%).
* Model terbaik hasil *tuning* dievaluasi pada `validation_set` (data yang belum pernah dilihat).

### 5. Pembuatan Prediksi Final
* Model terbaik (dengan parameter dari `RandomizedSearchCV`) dilatih ulang menggunakan **seluruh** data `data_training.csv` (termasuk yang 20% tadi) agar model "sepintar" mungkin.
* Model final ini kemudian digunakan untuk memprediksi `data_testing.csv`.
* Hasil prediksi disimpan dalam file 

## 📚 Library yang Digunakan
* Pandas
* Numpy
* Scikit-learn (sklearn)
* Matplotlib & Seaborn
