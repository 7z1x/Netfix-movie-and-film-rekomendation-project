# Laporan Proyek Machine Learning - Zulfahmi M. Ardianto

## Domain Proyek

### Latar Belakang
Di era digital saat ini, pengguna disuguhi dengan banyak sekali pilihan hiburan, termasuk film. Platform seperti Netflix menyediakan ribuan judul, dan pengguna dapat merasa kewalahan dalam memilih tontonan yang sesuai dengan preferensi mereka. Penelitian menunjukkan bahwa lebih dari 80% konten yang ditonton di Netflix berasal dari rekomendasi sistem mereka [1]. Hal ini menunjukkan efektivitas sistem rekomendasi dalam meningkatkan pengalaman pengguna dan loyalitas pelanggan. Selain itu, pendekatan personalisasi yang digunakan oleh Netflix memungkinkan mereka untuk menyesuaikan rekomendasi berdasarkan perilaku dan preferensi unik setiap pengguna, yang pada akhirnya meningkatkan retensi pelanggan dan mengurangi tingkat churn [2].

### Mengapa dan Bagaimana Masalah Ini Harus Diselesaikan
- Tanpa sistem rekomendasi, pengguna harus mencari film secara manual, yang tidak hanya memakan waktu tetapi juga bisa mengurangi pengalaman pengguna.
- Dengan membangun sistem rekomendasi berbasis data, kita dapat memberikan saran film secara otomatis berdasarkan pola perilaku pengguna lain, sehingga mempercepat proses pengambilan keputusan dan meningkatkan loyalitas pengguna.

## Business Understanding
### Problem Statement:
- Pengguna kesulitan menemukan film yang sesuai dengan preferensi mereka dari ribuan pilihan yang tersedia di platform seperti Netflix. Hal ini dapat menyebabkan pengalaman pengguna yang buruk dan menurunkan engagement di platform.

### Goals:
- Membangun sistem rekomendasi film yang dapat memprediksi film mana yang kemungkinan besar akan disukai oleh pengguna berdasarkan riwayat rating mereka dan pengguna lain yang serupa.

### Solution Statements:
1. Collaborative Filtering
- Pendekatan ini memanfaatkan pola interaksi antar pengguna dan item (film), khususnya dengan algoritma Singular Value Decomposition (SVD). Model ini mempelajari hubungan laten antara pengguna dan film berdasarkan data rating yang diberikan. Dengan memproyeksikan pengguna dan film ke dalam ruang fitur berdimensi rendah, model SVD dapat memprediksi rating yang mungkin diberikan seorang pengguna terhadap film yang belum ditontonnya, dan merekomendasikan film dengan prediksi rating tertinggi.
Penggunaan SVD dalam sistem rekomendasi telah terbukti efektif dalam meningkatkan akurasi prediksi dan mengatasi masalah kelangkaan data (data sparsity) [3].

2. Content-Based Filtering
- Pendekatan ini merekomendasikan film berdasarkan kesamaan atribut konten film dengan preferensi pengguna. Dalam proyek ini, fitur yang digunakan meliputi judul film. Sistem akan menganalisis film yang disukai oleh pengguna, kemudian mencari film lain yang memiliki kemiripan konten. Misalnya, film dengan kata kunci tertentu dalam judulnya, maka sistem akan merekomendasikan film serupa berdasarkan karakteristik tersebut.
Metode Content-Based Filtering terbukti menghasilkan rekomendasi yang relevan dengan tingkat presisi dan recall yang tinggi, terutama saat menggunakan teknik kemiripan seperti cosine similarity [4].

## Data Understanding
### Informasi Data
Dataset yang digunakan dalam proyek ini diunduh dari platform [Kaggle](https://www.kaggle.com/datasets/rishitjavia/netflix-movie-rating-dataset) dengan nama Netflix Movie Rating Dataset. Dataset ini terdiri dari dua file utama:
## 📄 Dataset

| Nama File                  | Deskripsi                                                                 |
|---------------------------|---------------------------------------------------------------------------|
| Netflix_Dataset_Rating.csv| Berisi informasi rating yang diberikan pengguna terhadap film.            |
| Netflix_Dataset_Movie.csv | Berisi metadata dari film, seperti judul dan tahun rilis. |

### Variabel/Fitur
### 📁 Netflix_Dataset_Rating.csv
Berisi data interaksi pengguna dengan film berupa rating:

| Kolom     | Tipe Data | Deskripsi                                                              |
|-----------|-----------|-------------------------------------------------------------------------|
| User_ID   | Integer   | ID unik pengguna                                                       |
| Movie_ID  | Integer   | ID unik film                                                           |
| Rating    | Integer   | Nilai rating yang diberikan pengguna terhadap film (skala 1–5)         |

### 📁 Netflix_Dataset_Movie.csv

Berisi informasi detail tentang film dalam dataset:

| Kolom     | Tipe Data | Deskripsi           |
|-----------|-----------|----------------------|
| Movie_ID  | Integer   | ID unik film         |
| Year      | Integer   | Tahun rilis film     |
| Name      | Object    | Judul film           |

### Distribusi Data
- Sebaran rating dianalisis menggunakan histogram dan menunjukkan bahwa sebagian besar pengguna memberikan rating di tengah-tengah skala (3–4).
![](https://github.com/7z1x/Netfix-movie-and-film-rekomendation-project/blob/6d464a4597c6441dd30cfe8107b12d2f515f7f6f/image/rating.jpg)<br>

- Film dengan jumlah rating terbanyak juga telah divisualisasikan untuk mengidentifikasi film yang paling populer berdasarkan partisipasi pengguna.
![](https://github.com/7z1x/Netfix-movie-and-film-rekomendation-project/blob/6d464a4597c6441dd30cfe8107b12d2f515f7f6f/image/jumlah%20film.jpg)<br>

### Preprocessing
- Pemeriksaan nilai kosong (missing values):

| Kolom     | Jumlah Missing Values |
|-----------|------------------------|
| User_ID   | 0                      |
| Movie_ID  | 0                      |
| Rating    | 0                      |

| Kolom     | Jumlah Missing Values |
|-----------|------------------------|
| Movie_ID  | 0                      |
| Year      | 0                      |
| Name      | 0                      |

- Pemeriksaan duplikasi:

  - Tidak ditemukan baris duplikat pada kedua dataset.

- Penggabungan data:

  - Dataset Netflix_Dataset_Rating.csv dan Netflix_Dataset_Movie.csv digabung menggunakan kolom Movie_ID untuk mendapatkan data lengkap yang mencakup User_ID, Rating, Movie_ID, Year, dan Name.<br>
  
| User_ID  | Rating | Movie_ID | Year | Name      |
|----------|--------|----------|------|-----------|
| 712664   | 5      | 3        | 1997 | Character |
| 1331154  | 4      | 3        | 1997 | Character |
| 2632461  | 3      | 3        | 1997 | Character |
| 44937    | 5      | 3        | 1997 | Character |
| 656399   | 4      | 3        | 1997 | Character |
| 439011   | 1      | 3        | 1997 | Character |
| ...      | ...    | ...      | ...  | ...       |

- Prepare Data
  - Pemilihan Kolom Relevan: Hanya kolom User_ID, Movie_ID, dan Rating yang dipilih, karena ini merupakan struktur dasar dari user-item interaction matrix dalam sistem rekomendasi.
  - Penyesuaian Skala Rating: Objek Reader dari library Surprise digunakan untuk mendefinisikan rentang rating (1–5), yang penting agar model memahami batas nilai saat mempelajari preferensi pengguna.
  - Konversi ke Format Surprise: Dataset diformat menggunakan Dataset.load_from_df() agar bisa digunakan oleh algoritma Surprise seperti SVD, KNN, dll

- Train-Test Split
Data dibagi menjadi dua bagian:
  - Training set (80%) digunakan untuk melatih model.
  - Test set (20%) digunakan untuk mengukur performa model terhadap data yang belum pernah dilihat.
  - Pembagian dilakukan menggunakan fungsi train_test_split dari surprise.model_selection.
Surprise adalah sebuah pustaka Python yang dirancang khusus untuk membangun dan menganalisis sistem rekomendasi yang berfokus pada data rating eksplisit[5]. Library ini menyediakan berbagai algoritma prediksi, termasuk metode collaborative filtering berbasis matrix factorization seperti SVD, serta alat bantu untuk evaluasi model dan pemilihan parameter.


## Modeling

### Pelatihan Model Collaborative Filtering
- Model SVD dilatih dengan parameter default (jumlah epoch = 10). Model dilatih pada data training yang telah diformat menjadi objek Dataset dari surprise.

### Prediksi dan Rekomendasi
- Kesalahan kuadrat rata-rata akar (Root Mean Squared Error - RMSE) dan kesalahan absolut rata-rata (Mean Absolute Error - MAE) adalah dua metrik standar yang digunakan dalam evaluasi model. Untuk sampel sebanyak n observasi y (yi, i = 1,2,..., n) dan n prediksi model yang bersesuaian ŷ[6]
- Setelah pelatihan selesai, prediksi dilakukan terhadap test set. Selain itu, sistem juga menghasilkan rekomendasi film untuk dua pengguna secara spesifik, berdasarkan prediksi rating tertinggi terhadap film-film yang belum pernah mereka beri rating.

| Metrik | Nilai   |
|--------|---------|
| RMSE   | 0.8689  |
<br>

- Ambil 10 film dengan prediksi rating tertinggi, tampilkan judul dan tahun film sebagai rekomendasi.<br>
  
- Rekomendasi Film untuk User 2643069

| No | Tahun | Judul Film                                        | Estimasi Rating |
|----|-------|---------------------------------------------------|-----------------|
| 1  | 1973  | Upstairs                                          | 4.7530          |
| 2  | 2001  | The West Wing: Season 3                           | 4.7417          |
| 3  | 2004  | Lost: Season 1                                    | 4.7322          |
| 4  | 2003  | Foyle's War: Set 2                                | 4.7148          |
| 5  | 1965  | The Battle of Algiers                             | 4.6892          |
| 6  | 2004  | Family Guy: Freakin' Sweet Collection             | 4.6730          |
| 7  | 1999  | Futurama: Monster Robot Maniac Fun Collection     | 4.6453          |
| 8  | 1994  | The Simpsons: Season 6                            | 4.6383          |
| 9  | 2002  | Curb Your Enthusiasm: Season 3                    | 4.6367          |
| 10 | 1995  | Pride and Prejudice                               | 4.6352          |


### Evaluation
- RMSE (Root Mean Square Error): Mengukur seberapa besar perbedaan antara rating sebenarnya dan prediksi. Semakin kecil nilai RMSE, semakin baik prediksi model.
- MAE (Mean Absolute Error): Rata-rata kesalahan absolut antara rating aktual dan prediksi.
  
| Metrik | Nilai   |
|--------|---------|
| RMSE   | 0.8689  |
| MAE    | 0.6781  |

- Hasil Evaluasi
  - RMSE: Nilai RMSE dari hasil pengujian menunjukkan bahwa model memiliki error prediksi yang dapat diterima untuk skala 1–5.
  - MAE: Nilai MAE juga menunjukkan rata-rata kesalahan yang relatif rendah, memperkuat bukti bahwa model memberikan prediksi yang cukup akurat.
  - Model ini mampu memberikan hasil rekomendasi yang relevan berdasarkan pola interaksi pengguna lain, sesuai tujuan dari sistem rekomendasi berbasis Collaborative Filtering.

### Modeling Content-Based Filtering

- Teknik yang digunakan TF-IDF (Term Frequency-Inverse Document Frequency) digunakan untuk mengekstraksi fitur dari teks judul film menjadi numerik. TF-IDF adalah metode yang digunakan untuk merepresentasikan dokumen dalam ruang vektor berdimensi tinggi, menangkap konten unik mereka sambil mengurangi pengaruh istilah umum[7]
- Cosine Similarity digunakan untuk mengukur kemiripan antar vektor judul film. Cosine similarity digunakan untuk mengukur kesamaan antara dua dokumen dengan menghitung cosinus dari sudut antara vektor TF-IDF mereka[7]. Menggunakan linear_kernel untuk menghitung cosine similarity antar semua kombinasi film berdasarkan vektor TF-IDF.
- Fungsi Rekomendasi

  - Fungsi get_recommendations(movie_id) mengambil Movie_ID dari film referensi dan mengembalikan daftar film paling mirip berdasarkan kemiripan judul, misal **Judul yang dicari : Lost: Season 1**
<br>

### Daftar Film dengan Similarity Tertinggi

| Movie_ID | Name                                      | Year | Similarity |
|----------|-------------------------------------------|------|------------|
| 4984     | Lost & Found                              | 1999 | 0.826104   |
| 11693    | The Lost World: Season 2                  | 2000 | 0.794335   |
| 17018    | The Lost World: Season 1                  | 1999 | 0.794335   |
| 9127     | Lost in Space: Season 1                   | 1965 | 0.740536   |
| 8414     | Land of the Lost: Season 3               | 1976 | 0.717135   |
| 16063    | Land of the Lost: Season 1               | 1974 | 0.717135   |
| 16582    | Land of the Lost: Season 2               | 1975 | 0.717135   |
| 956      | Lost in Space: Season 2: Vol. 1          | 1966 | 0.659653   |
| 5931     | Lost in Space: Season 2: Vol. 2          | 1966 | 0.659653   |
| 8136     | Lost in Space: Season 3: Vol. 2          | 1968 | 0.659653   |


### Evaluation
Untuk mengukur seberapa baik rekomendasi dari model Content-Based Filtering, dilakukan evaluasi berbasis similarity judul menggunakan library fuzzywuzzy. FuzzyWuzzy adalah library Python yang menggunakan algoritma Levenshtein distance untuk mengukur kesamaan antara dua string[7]. Evaluasi dilakukan dengan pendekatan berikut:

- Ground Truth: Film dianggap relevan jika memiliki kemiripan judul (berdasarkan fuzz.partial_ratio) di atas 80 terhadap film referensi.

- Metrik Evaluasi

| Metrik         | Deskripsi                                                                 |
|----------------|---------------------------------------------------------------------------|
| Precision@K    | Persentase dari K rekomendasi teratas yang relevan.[8]                       |
| Recall@K       | Persentase film relevan yang berhasil direkomendasikan dalam top-K.[8]       |
| Hit Rate       | Nilai 1 jika setidaknya satu item relevan muncul di hasil rekomendasi.[8]     |

- **Untuk Movie_ID = 3456, hasil evaluasi bisa seperti berikut:**

| Metrik        | Nilai |
|---------------|-------|
| Precision@10  | 0.4   |
| Recall@10     | 0.25  |
| Hit Rate      | 1.0   |

Angka-angka ini menunjukkan bahwa model mampu merekomendasikan film dengan judul yang relevan dalam jumlah yang signifikan, dan setidaknya ada satu film relevan di hasil rekomendasi.

## Kesimpulan
### Hasil Evaluasi
Evaluasi model Collaborative Filtering (SVD) menghasilkan nilai RMSE sebesar 0.86 dan MAE sebesar 0.66, yang menunjukkan bahwa model ini cukup baik dalam memprediksi rating pengguna terhadap film. Sementara itu, Content-Based Filtering yang dievaluasi menggunakan Precision@10 sebesar 0.4, Recall@10 sebesar 0.25, dan Hit Rate sebesar 1.0, menunjukkan bahwa sistem mampu merekomendasikan film yang cukup relevan berdasarkan kemiripan judul, meskipun cakupannya terhadap semua film relevan masih terbatas.

### Kelebihan dan Kekurangan
Collaborative Filtering memiliki keunggulan dalam memberikan rekomendasi yang bersifat personal karena mempertimbangkan preferensi pengguna lain yang serupa, namun lemah dalam menangani data pengguna atau film baru (cold start). Sebaliknya, Content-Based Filtering lebih kuat untuk merekomendasikan film baru karena hanya mengandalkan informasi konten seperti judul, namun cenderung memberikan hasil yang kurang variatif karena terbatas pada kemiripan fitur permukaan.

## Referensi
[1] Wired. (2018, January 25). This is how Netflix’s top-secret recommendation system works. https://www.wired.com/story/how-do-netflixs-algorithms-work-machine-learning-helps-to-predict-what-viewers-will-like

[2] Qin, X. (2024). Netflix’s billion-dollar secret: How recommendation systems fuel growth. LinkedIn Pulse. https://www.linkedin.com/pulse/netflixs-billion-dollar-secret-how-recommendation-systems-qin-phd-7zece

[3] Suriya, S., & Virumeshwaran, M. (2023). Music Recommendation System using Collaborative Filtering with SVD. Journal of Information Technology and Digital World, 5(2), 93–114. https://irojournals.com/itdw/article/view/5/2/2

[4] Fiarni, C., & Maharani, H. (2019). Product Recommendation System Design Using Cosine Similarity and Content-based Filtering Methods. International Journal of Information Technology and Electrical Engineering, 3(1), 1–6. https://jurnal.ugm.ac.id/ijitee/article/view/45538

[5] Hug, N. (2020). Surprise: A Python library for recommender systems. Journal of Open Source Software, 5(52), 2174. https://doi.org/10.21105/joss.02174

[6] Willmott, C. J., & Matsuura, K. (2005). Advantages of the mean absolute error (MAE) over the root mean square error (RMSE) in assessing average model performance. Climate Research, 30(1), 79–82. https://doi.org/10.3354/cr030079

[7] Widianto, A., Pebriyanto, E., Fitriyanti, & Marna. (2024). Document Similarity Using Term Frequency-Inverse Document Frequency Representation and Cosine Similarity. Journal of DINDA, 4(2), 149–153. Retrieved from https://www.researchgate.net/publication/383453541_Document_Similarity_Using_Term_Frequency-Inverse_Document_Frequency_Representation_and_Cosine_Similarity

[8] Evidently AI. (2025). Precision and recall at K in ranking and recommendations. Retrieved from https://www.evidentlyai.com/ranking-metrics/precision-recall-at-k

[9] HKUST Library. (2023). Fuzzy String Matching Using FuzzyWuzzy. Retrieved from https://library.hkust.edu.hk/sc/fuzzywuzzy/
