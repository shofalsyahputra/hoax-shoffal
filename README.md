# Penerapan Algoritma Multinomial Naive Bayes untuk Klasifikasi Berita Hoaks dan Non-Hoaks Berbahasa Indonesia Berbasis Website

Selamat datang di proyek penelitian dan aplikasi sistem cerdas klasifikasi berita hoaks berbasis web. Sistem ini dirancang untuk mendeteksi disinformasi pada teks berita berbahasa Indonesia secara otomatis menggunakan pemrosesan bahasa alami (Natural Language Processing / NLP) dan algoritma Machine Learning **Multinomial Naive Bayes (MNB)** dengan pembobotan kata **TF-IDF** serta **Laplace Smoothing**.

---

## 🌟 Fitur Utama Sistem

1. **Klasifikasi Teks Berita Real-time**:
   - Menganalisis berita berdasarkan judul dan narasi/isi berita.
   - Menyediakan tombol pemuatan cepat sampel berita hoaks dan fakta untuk kemudahan demonstrasi pengujian atau presentasi sidang.
   - Menghasilkan label status: **HOAKS** atau **NON-HOAKS (FAKTA)** dengan tingkat keyakinan (*confidence level*) dan probabilitas posterior $P(C|D)$.

2. **Transparansi Tahapan Text Preprocessing**:
   - Menampilkan tahapan transformasi teks langkah demi langkah:
     - **Case Folding**: Mengubah semua karakter menjadi huruf kecil.
     - **Data Cleansing**: Membersihkan URL, mention, tagar, simbol, tanda baca, dan angka.
     - **Tokenizing**: Pemotongan teks menjadi token kata.
     - **Stopword Removal**: Menghilangkan kata umum/tugas bahasa Indonesia (*dan, yang, di, ke, dari, dll.*).
     - **Stemming**: Menemukan kata dasar bahasa Indonesia (menghapus prefiks, sufiks, infiks, dan konfiks).

3. **Evaluasi & Metrik Pengujian Model**:
   - **Confusion Matrix** interaktif (True Positive, True Negative, False Positive, False Negative).
   - Perhitungan otomatis metrik performa standar akademik: **Akurasi (Accuracy)**, **Presisi (Precision)**, **Recall (Sensitivitas)**, dan **F1-Score**.
   - **Top Informative Features**: Daftar kata yang paling mencirikan berita hoaks vs berita fakta berdasarkan rasio log-likelihood.

4. **Eksplorasi Dataset & Penambahan Data Baru (Retraining)**:
   - Eksplorasi dataset berita berbahasa Indonesia lengkap dengan fitur pencarian dan filter kategori (Kesehatan, Ekonomi, Bencana, Teknologi, Sosial, Hukum, dll).
   - Fitur tombol *Uji Berita Langsung* dari tabel ke form klasifikasi.
   - Form penambahan data berita baru yang secara otomatis melatih ulang (*retrain*) model Naive Bayes secara instan.

5. **Dokumentasi Teori & Rumus Matematis**:
   - Penjelasan matematis lengkap Teorema Bayes, Prior Probability, Likelihood dengan Laplace Smoothing ($\alpha = 1.0$), Log-Space Transformation, dan pembobotan TF-IDF untuk kebutuhan penyusunan Bab 3 dan Bab 4 Skripsi / Tugas Akhir.

---

## 📁 Struktur Direktori Proyek

```
web berita hoax/
│
├── BUKA_WEBSITE.html           <-- KLIK 2x FILE INI UNTUK LANGSUNG MEMBUKA WEBSITE!
├── README.md                   <-- Dokumentasi lengkap proyek & teori skripsi
│
├── dataset/
│   └── dataset_berita.csv      <-- Dataset 60 berita bahasa Indonesia (Hoaks & Fakta)
│
├── web_app/                    <-- Standalone Web Application (Dapat berjalan langsung di browser)
│   ├── index.html              <-- Halaman antarmuka pengguna utama
│   ├── css/
│   │   └── style.css           <-- Desain antarmuka modern, responsif & bersih
│   └── js/
│       ├── stopwords.js        <-- Korpus stopword bahasa Indonesia (Tala/Sastrawi)
│       ├── stemmer.js          <-- Mesin stemming morfologi bahasa Indonesia
│       ├── dataset.js          <-- Dataset berita bawaan
│       ├── naive_bayes.js      <-- Implementasi Algoritma Multinomial Naive Bayes (JS)
│       └── app.js              <-- Logika antarmuka, event handler & evaluasi model
│
└── python_backend/             <-- Backend Full Stack Python (Untuk Sidang / Server API)
    ├── app.py                  <-- Server Flask & REST API (/api/predict, /api/metrics, dll)
    ├── train_model.py          <-- Script training & pengujian model dengan scikit-learn
    ├── preprocessing.py        <-- Pipeline preprocessing teks bahasa Indonesia
    ├── requirements.txt        <-- Daftar dependensi library Python
    └── run.bat                 <-- File batch untuk menjalankan server Flask dengan 1-klik
```

---

## 🚀 Cara Menjalankan Website

Proyek ini dirancang secara **Dual-Mode**, sehingga Anda memiliki fleksibilitas penuh:

### Cara 1: Buka Langsung di Browser (Paling Cepat & Praktis)
*Cocok jika Anda ingin langsung melihat, mendemokan, atau menguji aplikasinya tanpa perlu instalasi apapun.*

1. Buka folder `c:\Users\D_C MDR\OneDrive\Documents\web berita hoax`.
2. Klik ganda (double-click) file **`BUKA_WEBSITE.html`** (atau buka file `web_app/index.html` dengan Google Chrome / Microsoft Edge).
3. Website akan langsung terbuka dan seluruh fitur (klasifikasi, evaluasi, preprocessing, dan dataset) siap digunakan secara penuh!

### Cara 2: Menjalankan Menggunakan Python Flask Backend
*Cocok untuk pengujian skripsi berbasis backend server / REST API.*

1. Pastikan Python 3.9+ telah terpasang di komputer Anda.
2. Buka folder `python_backend/`.
3. Buka Terminal atau Command Prompt di folder tersebut, lalu install dependensi:
   ```bash
   pip install -r requirements.txt
   ```
4. Jalankan script pelatihan model:
   ```bash
   python train_model.py
   ```
5. Jalankan server Flask:
   ```bash
   python app.py
   ```
6. Buka browser dan akses alamat: `http://localhost:5000`

---

## 📐 Landasan Teori & Rumus Matematis

### 1. Teorema Bayes Dasar
Teorema Bayes menghitung probabilitas posterior dari suatu kelas dokumen $C$ berdasarkan term kata pada dokumen $D$:
$$P(C | D) = \frac{P(D | C) \cdot P(C)}{P(D)}$$

Keputusan klasifikasi diambil dengan mencari nilai maksimum dari perkalian probabilitas awal (*Prior*) dan kemungkinan bersyarat (*Likelihood*) (**Maximum A Posteriori / MAP**):
$$c_{MAP} = \arg\max_{c \in C} \left[ P(c) \prod_{i=1}^{n} P(w_i | c) \right]$$

### 2. Prior Probability
$$P(c) = \frac{N_c}{N}$$
- $N_c$: Jumlah dokumen latih pada kelas $c$ (Hoaks atau Non-Hoaks).
- $N$: Total seluruh dokumen latih.

### 3. Likelihood dengan Laplace Smoothing ($\alpha = 1$)
Untuk menghindari probabilitas bernilai nol (*zero-probability problem*) pada kata yang belum pernah muncul pada data latih:
$$P(w_k | c) = \frac{\sum_{d \in c} f(w_k, d) + \alpha}{\sum_{w'} \sum_{d \in c} f(w', d) + \alpha \cdot |V|}$$
- $f(w_k, d)$: Frekuensi kemunculan kata / bobot TF-IDF kata $w_k$ pada kelas $c$.
- $|V|$: Ukuran vocabulary (total kata unik di seluruh korpus).
- $\alpha$: Parameter penghalusan Laplace (umumnya bernilai 1).

### 4. Transformasi Log-Space
Untuk mencegah *arithmetic underflow* akibat perkalian angka pecahan yang sangat kecil:
$$\text{Score}(c) = \ln P(c) + \sum_{i=1}^{n} \ln P(w_i | c)$$

---

## 📊 Metrik Evaluasi (Confusion Matrix)

Performa sistem diuji menggunakan 4 metrik standar:
1. **Akurasi (Accuracy)**:
   $$\text{Akurasi} = \frac{TP + TN}{TP + TN + FP + FN} \times 100\%$$
2. **Presisi (Precision)**:
   $$\text{Presisi} = \frac{TP}{TP + FP} \times 100\%$$
3. **Recall (Sensitivitas)**:
   $$\text{Recall} = \frac{TP}{TP + FN} \times 100\%$$
4. **F1-Score**:
   $$\text{F1-Score} = 2 \times \frac{\text{Presisi} \times \text{Recall}}{\text{Presisi} + \text{Recall}} \times 100\%$$

