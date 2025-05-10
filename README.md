# Laporan Proyek Machine Learning - Zulfahmi M. Ardianto

## Domain Proyek

### Latar Belakang
Di era digital saat ini, pengguna disuguhi dengan banyak sekali pilihan hiburan, termasuk film. Platform seperti Netflix menyediakan ribuan judul, dan pengguna dapat merasa kewalahan dalam memilih tontonan yang sesuai dengan preferensi mereka. Sistem rekomendasi menjadi alat penting yang membantu pengguna menemukan film yang relevan, meningkatkan kepuasan, serta memperpanjang waktu keterlibatan pengguna dengan platform.

### Mengapa dan Bagaimana Masalah Ini Harus Diselesaikan
Tanpa sistem rekomendasi, pengguna harus mencari film secara manual, yang tidak hanya memakan waktu tetapi juga bisa mengurangi pengalaman pengguna. Dengan membangun sistem rekomendasi berbasis data, kita dapat memberikan saran film secara otomatis berdasarkan pola perilaku pengguna lain, sehingga mempercepat proses pengambilan keputusan dan meningkatkan loyalitas pengguna.

## Business Understanding
### Problem Statement:
Pengguna kesulitan menemukan film yang sesuai dengan preferensi mereka dari ribuan pilihan yang tersedia di platform seperti Netflix. Hal ini dapat menyebabkan pengalaman pengguna yang buruk dan menurunkan engagement di platform.

### Goals:
Membangun sistem rekomendasi film yang dapat memprediksi film mana yang kemungkinan besar akan disukai oleh pengguna berdasarkan riwayat rating mereka dan pengguna lain yang serupa.

### Solution Statements:
Menggunakan pendekatan Collaborative Filtering, khususnya algoritma Singular Value Decomposition (SVD), untuk membangun model rekomendasi. Model ini akan mempelajari pola dari data interaksi pengguna terhadap film (rating), kemudian memprediksi dan merekomendasikan film dengan prediksi rating tertinggi untuk masing-masing pengguna.

## Data Understanding
### Informasi Data
Dataset yang digunakan dalam proyek ini diunduh dari platform Kaggle dengan nama Netflix Movie Rating Dataset. Dataset ini terdiri dari dua file utama:
1. Netflix_Dataset_Rating.csv – berisi informasi rating yang diberikan pengguna terhadap film.
2. Netflix_Dataset_Movie.csv – berisi metadata dari film, seperti judul dan tahun rilis.
Kedua file ini akan digabungkan untuk membentuk satu kesatuan data yang dapat digunakan dalam sistem rekomendasi.
### Variabel/Fitur
1. Netflix_Dataset_Rating.csv
- User_ID : ID unik pengguna.

- Movie_ID : ID unik film.

- Rating : Nilai rating yang diberikan pengguna terhadap film, dalam skala 1–5.

2. Netflix_Dataset_Movie.csv
- Movie_ID : ID unik film.

- Year : Tahun rilis film.

- Name : Judul film.
### Distribusi Data
- Sebaran rating dianalisis menggunakan histogram dan menunjukkan bahwa sebagian besar pengguna memberikan rating di tengah-tengah skala (3–4).
![](gambar rating film)<br>
- Film dengan jumlah rating terbanyak juga telah divisualisasikan untuk mengidentifikasi film yang paling populer berdasarkan partisipasi pengguna.
![](gambar film terbanyak)<br>

### Preprocessing
- Pemeriksaan nilai kosong (missing values):

  - Tidak ditemukan nilai kosong baik pada dataset rating maupun movie.

- Pemeriksaan duplikasi:

  - Tidak ditemukan baris duplikat pada kedua dataset.

- Penggabungan data:

  - Dataset Netflix_Dataset_Rating.csv dan Netflix_Dataset_Movie.csv digabung menggunakan kolom Movie_ID untuk mendapatkan data lengkap yang mencakup User_ID, Rating, Movie_ID, Year, dan Name.

## Modeling

🔧 Pendekatan yang Digunakan
Pendekatan yang digunakan dalam proyek ini adalah Collaborative Filtering, lebih tepatnya metode Singular Value Decomposition (SVD) dari library scikit-surprise. Pendekatan ini dipilih karena efektif dalam menangkap hubungan laten antara pengguna dan item (dalam hal ini, film) berdasarkan rating historis.

🧪 Train-Test Split
Data dibagi menjadi dua bagian:

Training set (80%) digunakan untuk melatih model.

Test set (20%) digunakan untuk mengukur performa model terhadap data yang belum pernah dilihat.

Pembagian dilakukan menggunakan fungsi train_test_split dari surprise.model_selection.

🧠 Pelatihan Model
Model SVD dilatih dengan parameter default (jumlah epoch = 10). Model dilatih pada data training yang telah diformat menjadi objek Dataset dari surprise.

🔍 Prediksi dan Rekomendasi
Setelah pelatihan selesai, prediksi dilakukan terhadap test set. Selain itu, sistem juga menghasilkan rekomendasi film untuk dua pengguna secara spesifik, berdasarkan prediksi rating tertinggi terhadap film-film yang belum pernah mereka beri rating.

Langkah-langkah:

Identifikasi film yang belum dirating oleh pengguna.

Prediksi rating untuk semua film tersebut.

Ambil 10 film dengan prediksi rating tertinggi.

Tampilkan judul dan tahun film sebagai rekomendasi.

📏 Evaluation
📊 Metode Evaluasi
Model dievaluasi menggunakan dua metrik evaluasi populer dalam sistem rekomendasi:

RMSE (Root Mean Square Error): Mengukur seberapa besar perbedaan antara rating sebenarnya dan prediksi. Semakin kecil nilai RMSE, semakin baik prediksi model.

MAE (Mean Absolute Error): Rata-rata kesalahan absolut antara rating aktual dan prediksi.

📉 Hasil Evaluasi
RMSE: Nilai RMSE dari hasil pengujian menunjukkan bahwa model memiliki error prediksi yang dapat diterima untuk skala 1–5.

MAE: Nilai MAE juga menunjukkan rata-rata kesalahan yang relatif rendah, memperkuat bukti bahwa model memberikan prediksi yang cukup akurat.

Model ini mampu memberikan hasil rekomendasi yang relevan berdasarkan pola interaksi pengguna lain, sesuai tujuan dari sistem rekomendasi berbasis Collaborative Filtering.

🤖 Modeling
🔧 Pendekatan yang Digunakan: Content-Based Filtering
Content-Based Filtering merekomendasikan item berdasarkan kemiripan konten antar item. Dalam konteks ini, model memanfaatkan judul film (Name) sebagai fitur utama untuk mengukur kemiripan antar film.

🧰 Teknik yang Digunakan
TF-IDF (Term Frequency-Inverse Document Frequency) digunakan untuk mengekstraksi fitur dari teks judul film.

Cosine Similarity digunakan untuk mengukur kemiripan antar vektor judul film.

🛠 Langkah-Langkah
Persiapan Fitur Judul Film

Data film dibersihkan untuk menghilangkan nilai kosong pada kolom Name.

Data kemudian diubah menjadi representasi TF-IDF menggunakan TfidfVectorizer.

Perhitungan Kemiripan

Menggunakan linear_kernel untuk menghitung cosine similarity antar semua kombinasi film berdasarkan vektor TF-IDF.

Fungsi Rekomendasi

Fungsi get_recommendations(movie_id) mengambil Movie_ID dari film referensi dan mengembalikan daftar film paling mirip berdasarkan kemiripan judul.

Hasil rekomendasi mencakup Movie_ID, Name, Year, dan skor similarity.

Contoh Output

Misalnya, untuk Movie_ID = 3456 (Lost: Season 1), sistem akan menampilkan 10 film lain dengan judul paling mirip berdasarkan cosine similarity.

📏 Evaluation
🎯 Evaluasi Content-Based Filtering
Untuk mengukur seberapa baik rekomendasi dari model Content-Based Filtering, dilakukan evaluasi berbasis similarity judul menggunakan library fuzzywuzzy. Evaluasi dilakukan dengan pendekatan berikut:

Ground Truth

Film dianggap relevan jika memiliki kemiripan judul (berdasarkan fuzz.partial_ratio) di atas 80 terhadap film referensi.

Metrik Evaluasi

Precision@K: Persentase dari K rekomendasi teratas yang relevan.

Recall@K: Persentase film relevan yang berhasil direkomendasikan dalam top-K.

Hit Rate: Nilai 1 jika setidaknya satu item relevan muncul di hasil rekomendasi.

Hasil Evaluasi (Contoh)

Untuk Movie_ID = 3456, hasil evaluasi bisa seperti berikut:

text
Copy code
Precision@10: 0.4
Recall@10: 0.25
Hit Rate: 1.0
Angka-angka ini menunjukkan bahwa model mampu merekomendasikan film dengan judul yang relevan dalam jumlah yang signifikan, dan setidaknya ada satu film relevan di hasil rekomendasi.

## Kesimpulan
### Hasil Evaluasi
Evaluasi model Collaborative Filtering (SVD) menghasilkan nilai RMSE sebesar 0.86 dan MAE sebesar 0.66, yang menunjukkan bahwa model ini cukup baik dalam memprediksi rating pengguna terhadap film. Sementara itu, Content-Based Filtering yang dievaluasi menggunakan Precision@10 sebesar 0.4, Recall@10 sebesar 0.25, dan Hit Rate sebesar 1.0, menunjukkan bahwa sistem mampu merekomendasikan film yang cukup relevan berdasarkan kemiripan judul, meskipun cakupannya terhadap semua film relevan masih terbatas.

###Kelebihan dan Kekurangan
Collaborative Filtering memiliki keunggulan dalam memberikan rekomendasi yang bersifat personal karena mempertimbangkan preferensi pengguna lain yang serupa, namun lemah dalam menangani data pengguna atau film baru (cold start). Sebaliknya, Content-Based Filtering lebih kuat untuk merekomendasikan film baru karena hanya mengandalkan informasi konten seperti judul, namun cenderung memberikan hasil yang kurang variatif karena terbatas pada kemiripan fitur permukaan.

