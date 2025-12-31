# Steam Sentiment Analysis

Steam Sentiment Analysis merupakan proyek analisis sentimen berbasis Natural Language Processing (NLP) yang bertujuan untuk mengklasifikasikan ulasan pengguna pada platform Steam ke dalam sentimen positif dan negatif. Proyek ini memanfaatkan representasi fitur teks menggunakan Term Frequency–Inverse Document Frequency (TF-IDF) serta beberapa algoritma machine learning untuk menganalisis persepsi pengguna terhadap gim digital secara otomatis.

---

## Latar Belakang

Pada era digital, data menjadi aset penting dalam mendukung pengambilan keputusan berbasis informasi. Dalam konteks industri gim digital, ulasan pengguna merefleksikan tingkat kepuasan dan pengalaman penggunaan, yang umumnya dinyatakan melalui sentimen positif atau negatif. Platform distribusi gim seperti Steam menyediakan volume ulasan yang sangat besar dan kaya informasi, namun sulit dianalisis secara manual.

Oleh karena itu, diperlukan pendekatan otomatis berbasis NLP, khususnya analisis sentimen, untuk mengklasifikasikan ulasan pengguna secara efisien. Proyek ini menerapkan metode TF-IDF sebagai representasi fitur teks dan membandingkan beberapa algoritma klasifikasi untuk memperoleh model dengan kinerja terbaik dalam tugas analisis sentimen.

---

## Analisis Data

### Sumber dan Karakteristik Data

Data yang digunakan berasal dari dataset publik Steam Reviews yang diperoleh melalui platform Kaggle, dengan total sekitar 6,4 juta data ulasan. Untuk mengurangi beban komputasi selama proses analisis dan pemodelan, digunakan sebanyak 350.000 data yang dipilih dengan mempertimbangkan keseimbangan distribusi kelas sentimen.

Dataset disimpan dalam basis data PostgreSQL dan dideploy menggunakan layanan Supabase, sehingga memungkinkan pengelolaan data berskala besar serta koneksi langsung dengan Python melalui library psycopg2.

Atribut utama yang digunakan dalam penelitian ini meliputi:
- user_id: identitas unik pengguna Steam
- app_id: identitas unik aplikasi atau gim
- app_name: nama aplikasi atau gim
- review_text: teks ulasan pengguna
- review_score: kelas sentimen ulasan (-1 untuk negatif, 1 untuk positif)

### Eksplorasi Data Awal

Tahap eksplorasi data dilakukan untuk memahami karakteristik dataset sebelum proses pemodelan. Analisis mencakup pemeriksaan ukuran data, tipe data, panjang teks ulasan, keberadaan nilai hilang, serta distribusi kelas sentimen.

Hasil eksplorasi menunjukkan bahwa panjang teks ulasan memiliki variasi yang cukup besar dan terdapat sejumlah data kosong pada kolom app_name dan review_text. Data ulasan kosong ditangani pada tahap prapemrosesan. Distribusi kelas sentimen pada data terpilih berada dalam kondisi seimbang, sehingga mendukung proses pelatihan model yang lebih objektif.

---

## Metodologi

Alur pengolahan data dan pengembangan model disusun secara bertahap, dimulai dari integrasi data hingga evaluasi kinerja model.

### Preprocessing Data

Preprocessing dilakukan menggunakan pendekatan batch processing dengan ukuran batch sebesar 5.000 data untuk menjaga efisiensi penggunaan memori dan waktu komputasi. Tahapan preprocessing meliputi:
- Case folding
- Penghapusan karakter tidak perlu seperti newline, URL, dan spasi berlebih
- Penghapusan karakter non-alfanumerik
- Normalisasi kata slang menggunakan kamus slang bahasa Inggris
- Penghapusan stopwords bahasa Inggris
- Stemming menggunakan Porter Stemmer

Setelah seluruh tahapan preprocessing diterapkan, diperoleh sebanyak 337.609 data ulasan bersih yang digunakan pada tahap selanjutnya.

### Ekstraksi Fitur

Teks ulasan hasil preprocessing direpresentasikan ke dalam bentuk numerik menggunakan metode TF-IDF. Ekstraksi fitur dilakukan dengan TfidfVectorizer menggunakan parameter utama:
- max_features: 5.000
- ngram_range: (1,2)
- min_df: 5
- max_df: 0,9

Hasil proses ini berupa matriks fitur TF-IDF berdimensi n_dokumen × 5.000.

### Algoritma Klasifikasi

Penelitian ini membandingkan tiga algoritma klasifikasi, yaitu:
- Logistic Regression sebagai model baseline
- Random Forest sebagai pendekatan ensemble non-linear
- Support Vector Machine (SVM) dengan kernel linear

Pemilihan beberapa algoritma bertujuan untuk membandingkan performa model linear dan non-linear dalam menangani representasi TF-IDF berdimensi tinggi.

### Evaluasi Model

Evaluasi performa model dilakukan menggunakan confusion matrix dan classification report. Metrik evaluasi yang digunakan meliputi accuracy, precision, recall, dan F1-score. Evaluasi dilakukan pada data uji untuk mengukur kemampuan generalisasi model terhadap data yang belum pernah dilihat sebelumnya.

---

## Hasil dan Evaluasi

Hasil evaluasi menunjukkan bahwa Logistic Regression dan SVM memiliki performa yang relatif sebanding dengan accuracy sebesar 0,80. Logistic Regression menunjukkan kinerja paling stabil dan seimbang dalam mengklasifikasikan sentimen positif dan negatif, sedangkan Random Forest memiliki performa yang lebih rendah dalam menangani fitur TF-IDF yang bersifat sparse dan berdimensi tinggi.

Analisis kualitatif terhadap sampel prediksi menunjukkan bahwa seluruh model mampu mengklasifikasikan ulasan dengan sentimen eksplisit secara konsisten, namun masih mengalami kesalahan pada ulasan yang bersifat ambigu atau kontekstual.

---

## Insight dan Rekomendasi

Analisis sentimen yang dilakukan mampu memberikan insight mengenai persepsi pengguna terhadap gim di platform Steam. Hasil visualisasi menunjukkan bahwa beberapa gim memiliki dominasi ulasan positif yang mencerminkan tingkat kepuasan pengguna yang tinggi, sementara gim lainnya memiliki jumlah ulasan negatif yang signifikan. Insight ini dapat dimanfaatkan sebagai dasar dalam pengambilan keputusan dan penyusunan rekomendasi gim berbasis data.

---

## Teknologi yang Digunakan

- Bahasa Pemrograman: Python
- Basis Data: PostgreSQL (Supabase)
- Library:
  - pandas
  - scikit-learn
  - psycopg2
  - matplotlib
  - python-dotenv
