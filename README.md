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

<br>

## Data Understanding
### Informasi Data
Dataset yang digunakan dalam proyek ini diunduh dari platform [Dataset Kaggle](https://www.kaggle.com/datasets/rishitjavia/netflix-movie-rating-dataset) dengan nama akun @rishitjavia Netflix Movie Rating Dataset. Dataset ini terdiri dari dua file utama:
## 📄 Dataset

| Nama File                  | Deskripsi                                                                 |
|---------------------------|---------------------------------------------------------------------------|
| Netflix_Dataset_Rating.csv| Berisi informasi rating yang diberikan pengguna terhadap film.            |
| Netflix_Dataset_Movie.csv | Berisi metadata dari film, seperti judul dan tahun rilis. |


### Variabel/Fitur
### 📁 Netflix_Dataset_Rating.csv
Berisi **17.337.458 juta data 3 fitur** dibawah ini:

| Kolom     | Tipe Data | Deskripsi                                                              | Data Unik    |
|-----------|-----------|-------------------------------------------------------------------------|------------|
| User_ID   | Integer   | ID unik pengguna                                                       | 143458     |
| Movie_ID  | Integer   | ID unik film                                                           | 1350       |
| Rating    | Integer   | Nilai rating yang diberikan pengguna terhadap film (skala 1–5)         | 5          |




### 📁 Netflix_Dataset_Movie.csv

Berisi **17770 data informasi dan 3 fitur** detail tentang film dalam dataset:

| Kolom     | Tipe Data | Deskripsi           | Data Unik  |
|-----------|-----------|----------------------|---------|
| Movie_ID  | Integer   | ID unik film         | 17770   |
| Year      | Integer   | Tahun rilis film     | 91      |
| Name      | Object    | Judul film           | 17297   |

### Pemeriksaan nilai kosong (missing values) & Duplikasi:

| Kolom     | Jumlah Missing Values | Jumlah Duplikasi |
|-----------|------------------------|------------------|
| User_ID   | 0                      | 0                |
| Movie_ID  | 0                      | 0                |
| Rating    | 0                      | 0                |

| Kolom     | Jumlah Missing Values | Jumlah Duplikasi |
|-----------|------------------------|------------------|
| Movie_ID   | 0                      | 0                |
| Year       | 0                      | 0                |
| Name   | 0                      | 0                |

### Distribusi Data
- Sebaran rating dianalisis menggunakan histogram dan menunjukkan bahwa sebagian besar pengguna memberikan rating di tengah-tengah skala (3–4).
  
![](https://github.com/7z1x/Netfix-movie-and-film-rekomendation-project/blob/6d464a4597c6441dd30cfe8107b12d2f515f7f6f/image/rating.jpg)

- Film dengan jumlah rating terbanyak juga telah divisualisasikan untuk mengidentifikasi film yang paling populer berdasarkan partisipasi pengguna.
  
![](https://github.com/7z1x/Netfix-movie-and-film-rekomendation-project/blob/6d464a4597c6441dd30cfe8107b12d2f515f7f6f/image/jumlah%20film.jpg)

### Cek Outlier
- Dataset Rating : Semua kolom terlihat bersih, tidak ada outlier signifikan secara statistik, untuk **rating**  hanya sedikit outlier yang menjadi titik pada boxplotnya namun nilai rendah seperti 1 sebagai outlier secara statistik, bukan karena nilai itu salah, tapi karena distribusi data yang tidak seimbang (kebanyakan nilai 3–5)
  
![](https://github.com/7z1x/Netfix-movie-and-film-rekomendation-project/blob/04277974630fd6c8f0e428b476e11c7b461676aa/image/boxplot_Movie_ID.png)

![](https://github.com/7z1x/Netfix-movie-and-film-rekomendation-project/blob/04277974630fd6c8f0e428b476e11c7b461676aa/image/boxplot_Rating.png)

![](https://github.com/7z1x/Netfix-movie-and-film-rekomendation-project/blob/04277974630fd6c8f0e428b476e11c7b461676aa/image/boxplot_User_ID.png)

<br>

- Dataset Movie : Dataset memiliki outlier pada kolom **Year** namun tidak perlu ditangani karena kolom ini tidak digunakan sebagai fitur dalam content-based filtering dan tidak memengaruhi hasil rekomendasi.

![](https://github.com/7z1x/Netfix-movie-and-film-rekomendation-project/blob/04277974630fd6c8f0e428b476e11c7b461676aa/image/boxplot_movie_df_Movie_ID.png)

![](https://github.com/7z1x/Netfix-movie-and-film-rekomendation-project/blob/04277974630fd6c8f0e428b476e11c7b461676aa/image/boxplot_Year.png)

<br>

## Data Preparation
- Handling Missing Value
  - Karena tidak ada yang null pada kedua dataset, maka tidak diperlukan proses imputasi atau penghapusan data akibat missing value. Hal ini mempercepat proses data preparation dan memastikan bahwa seluruh data dapat digunakan secara maksimal dalam proses training dan evaluasi model.

- Handling Duplicates
  - Karena tidak ada duplicate maka tidak diperlukan proses penghapusan data ganda. Ini menunjukkan bahwa data yang tersedia telah bersih dari redundansi dan siap digunakan untuk proses analisis dan pemodelan tanpa risiko perhitungan ganda yang dapat memengaruhi hasil.

- Handling Outliers
  -  Karena outlier pada rating bukan karena nilai itu salah, tapi karena distribusi data yang tidak seimbang (kebanyakan nilai 3–5), maka tidak diperlukan proses tindakan handling outlier karena data itu juga penting dalam model nantinya. dan pada movie_df dataset movie terdapat outliers pada year namun tidak digunakan sebagai fitur, outlier tidak akan berdampak signifikan, jika dibuang berpotensi kehilangan film klasik atau penting secara historis.

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

### Collaborative Filltering Preparation
  - Pemilihan Kolom Relevan: Hanya kolom User_ID, Movie_ID, dan Rating yang dipilih, karena ini merupakan struktur dasar dari user-item interaction matrix dalam sistem rekomendasi.

  - Penyesuaian Skala Rating: Objek Reader dari library Surprise digunakan untuk mendefinisikan rentang rating (1–5), yang penting agar model memahami batas nilai saat mempelajari preferensi pengguna.

  - Konversi ke Format Surprise: Dataset diformat menggunakan Dataset.load_from_df() agar bisa digunakan oleh algoritma Surprise SVD.
      
```python
data_df = df[['User_ID', 'Movie_ID', 'Rating']]
reader = Reader(rating_scale=(1, 5))
data = Dataset.load_from_df(data_df, reader)
```

  - Train-Test Split, data dibagi menjadi dua bagian, training set (80%) digunakan untuk melatih model. Test set (20%) digunakan untuk mengukur performa model terhadap data yang belum pernah dilihat. Pembagian dilakukan menggunakan fungsi train_test_split dari surprise.model_selection.
```python
trainset, testset = train_test_split(data, test_size=0.2, random_state=42)
```

  - Surprise adalah sebuah pustaka Python yang dirancang khusus untuk membangun dan menganalisis sistem rekomendasi yang berfokus pada data rating eksplisit[5]. Library ini menyediakan berbagai algoritma prediksi, termasuk metode collaborative filtering berbasis matrix factorization seperti SVD, serta alat bantu untuk evaluasi model dan pemilihan parameter.


### Collaborative Filltering Preparation
  - Memastikan bahwa hanya film unik yang digunakan dalam proses pemodelan dengan menghapus duplikat pada kolom Movie_ID, Name, dan Year. Selain itu, nilai kosong pada kolom Name diisi dengan string kosong ('') agar tidak mengganggu proses ekstraksi fitur berbasis teks pada tahap content-based filtering.
```python
movie_features = movie_df[['Movie_ID', 'Name', 'Year']].drop_duplicates()
movie_features['Name'] = movie_features['Name'].fillna('')
movie_features.sample(5)
```
  - Melakukan text preprocessing pada judul film untuk meningkatkan kualitas representasi teks sebelum dihitung kemiripannya. Prosesnya mencakup normalisasi huruf kecil, penghapusan angka dan tanda baca, serta pembersihan spasi.
  - Setelah teks dibersihkan, dilakukan ekstraksi fitur menggunakan teknik yang digunakan TF-IDF (Term Frequency-Inverse Document Frequency) digunakan untuk mengekstraksi fitur dari teks judul film menjadi numerik. TF-IDF adalah metode yang digunakan untuk merepresentasikan dokumen dalam ruang vektor berdimensi tinggi, menangkap konten unik mereka sambil mengurangi pengaruh istilah umum[7].
```python
def preprocess_text(text):
    text = text.lower()  
    text = re.sub(r'\d+', '', text)  
    text = text.translate(str.maketrans('', '', string.punctuation))  
    text = text.strip()  
    return text

movie_features = movie_features.reset_index(drop=True)
movie_features['Clean_Name'] = movie_features['Name'].apply(preprocess_text)
tfidf = TfidfVectorizer(stop_words='english')
tfidf_matrix = tfidf.fit_transform(movie_features['Clean_Name'])
```

<br>

## Modeling

### Model Collaborative Filtering
- Model menggunakan algoritma SVD yang dilatih selama 10 epoch, artinya model akan menyempurnakan representasi laten pengguna dan item melalui 10 iterasi agar semakin akurat dalam memprediksi rating.
- Evaluasi menggunakan RMSE (Root Mean Squared Error) memberikan gambaran sejauh mana prediksi rating model mendekati rating sebenarnya; semakin rendah nilai RMSE, semakin akurat model.
- Model SVD bekerja dengan mendekomposisi matriks rating pengguna-item menjadi representasi laten berdimensi rendah, yang menangkap pola tersembunyi antara preferensi pengguna dan karakteristik item; melalui proses ini, sistem mampu memprediksi rating yang belum diberikan secara akurat meskipun data sangat jarang (sparse), SVD terbukti memiliki keunggulan dalam menangani dataset besar, mengekstraksi fitur, mengurangi noise dan dimensi, sehingga mempercepat komputasi[10].
- Proses sistem rekomendasi berbasis model prediktif SVD yang digunakan untuk menghasilkan rekomendasi personal bagi pengguna berdasarkan estimasi rating tertinggi terhadap film yang belum pernah mereka tonton. Prosesnya meliputi:
  - Identifikasi film yang belum ditonton oleh pengguna (unrated_movies), agar sistem hanya memberikan rekomendasi yang benar-benar baru dan relevan.
  - Prediksi rating untuk semua film tersebut menggunakan model SVD yang telah dilatih sebelumnya, yang memanfaatkan pola tersembunyi antara pengguna dan item.
  - Seleksi Top-N rekomendasi dengan memilih 10 film dengan estimasi rating tertinggi, menunjukkan sistem tidak hanya memprediksi tetapi juga memprioritaskan konten terbaik untuk pengguna.
  - Penggabungan data film seperti judul dan tahun rilis agar hasil rekomendasi dapat disajikan secara informatif dan mudah dipahami.
  - Secara keseluruhan, proses ini mencerminkan praktik standar dalam sistem rekomendasi modern, di mana pendekatan filtering prediktif digunakan untuk meningkatkan pengalaman pengguna melalui saran yang bersifat personal, relevan, dan data-driven.

```python
# Langkah 1: Ambil semua Movie_ID yang belum di-rating oleh user 
user_id = (user_id)

# Movie yang sudah dirating oleh user ini
rated_movies = data_df[data_df['User_ID'] == user_id]['Movie_ID'].tolist()

# Movie yang belum dirating
all_movies = data_df['Movie_ID'].unique()
unrated_movies = [movie for movie in all_movies if movie not in rated_movies]

# Langkah 2: Prediksi rating untuk semua movie yang belum dirating
user_predictions = [model.predict(user_id, movie_id) for movie_id in unrated_movies]

# Langkah 3: Ambil top-N (misal 10) dengan rating tertinggi
top_n_preds = sorted(user_predictions, key=lambda x: x.est, reverse=True)[:10]

# Langkah 4: Ambil informasi judul dan tahun dari movie_df
top_n_result = []
for pred in top_n_preds:
    movie_info = movie_df[movie_df['Movie_ID'] == int(pred.iid)].iloc[0]
    top_n_result.append({
        'Year': movie_info['Year'],
        'Name': movie_info['Name'],
        'Estimated_Rating': round(pred.est, 4)
    })

# Langkah 5: Tampilkan hasil rekomendasi
top_n_df = pd.DataFrame(top_n_result)
print(top_n_df)
```

- Ambil 10 film dengan prediksi rating tertinggi, tampilkan judul dan tahun film sebagai rekomendasi.
  - Rekomendasi Film untuk user_id: 2643069

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

   - Rekomendasi untuk user_id : 43441

| No | Tahun | Judul Film                     | Estimasi Rating |
|----|-------|--------------------------------|-----------------|
| 1  | 2004  | Lost: Season 1                 | 4.6197          |
| 2  | 2001  | The West Wing: Season 3        | 4.4577          |
| 3  | 1994  | The Simpsons: Season 6         | 4.4395          |
| 4  | 2004  | Stargate SG-1: Season 8        | 4.4234          |
| 5  | 1995  | Pride and Prejudice            | 4.3858          |
| 6  | 2002  | Gilmore Girls: Season 3        | 4.3693          |
| 7  | 2004  | Six Feet Under: Season 4       | 4.3690          |
| 8  | 2001  | Alias: Season 1                | 4.3671          |
| 9  | 2003  | Foyle's War: Set 2             | 4.3659          |
| 10 | 2002  | Firefly                        | 4.3624          |

<br>

### Modeling Content-Based Filtering

- Cosine Similarity digunakan untuk mengukur kemiripan antar vektor judul film. Cosine similarity digunakan untuk mengukur kesamaan antara dua dokumen dengan menghitung cosinus dari sudut antara vektor TF-IDF mereka[7]. Menggunakan linear_kernel untuk menghitung cosine similarity antar semua kombinasi film berdasarkan vektor TF-IDF.
```python
cosine_sim = linear_kernel(tfidf_matrix, tfidf_matrix)
```
  
- Fungsi Rekomendasi
  - Fungsi get_recommendations(movie_id) mengambil Movie_ID dari film referensi dan mengembalikan daftar film paling mirip berdasarkan kemiripan judul, misal **Judul yang dicari : Lost: Season 1**
# Fungsi untuk memberikan rekomendasi berdasarkan movie_id
```python
def get_recommendations(movie_id, cosine_sim, movie_id_to_index, movie_df, top_n=10):
    # Cek apakah movie_id ada dalam mapping
    if movie_id not in movie_id_to_index:
        return f"Movie_ID {movie_id} tidak ditemukan dalam data."
    
    idx = movie_id_to_index[movie_id]
    
    # Ambil skor similarity untuk film tersebut
    sim_scores = list(enumerate(cosine_sim[idx]))
    
    # Urutkan berdasarkan skor similarity dan ambil top_n
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)[1:top_n+1]
    
    # Ambil indeks film yang relevan berdasarkan skor similarity
    movie_indices = [i[0] for i in sim_scores]
    similarity_scores = [i[1] for i in sim_scores]
    
    # Ambil informasi film yang relevan berdasarkan indeks
    recommended_movies = movie_df.iloc[movie_indices][['Movie_ID', 'Name', 'Year']]
    
    # Tambahkan kolom Similarity ke dalam hasil rekomendasi
    recommended_movies['Similarity'] = similarity_scores
    
    # Urutkan berdasarkan Similarity
    recommended_movies = recommended_movies.sort_values(by='Similarity', ascending=False)
    
    return recommended_movies[['Movie_ID', 'Name', 'Year', 'Similarity']]

recommendations = get_recommendations(3456, cosine_sim, movie_id_to_index, movie_features)
print(recommendations)
```

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

## Evaluation
### Evaluation Collaborative Filtering
- RMSE (Root Mean Square Error): Mengukur seberapa besar perbedaan antara rating sebenarnya dan prediksi. Semakin kecil nilai RMSE, semakin baik prediksi model.
- MAE (Mean Absolute Error): Rata-rata kesalahan absolut antara rating aktual dan prediksi.
  
| Metrik | Nilai   |
|--------|---------|
| RMSE   | 0.8684  |
| MAE    | 0.6776  |

- Hasil Evaluasi
  - RMSE: Nilai RMSE dari hasil pengujian menunjukkan bahwa model memiliki error prediksi yang dapat diterima untuk skala 1–5.
  - MAE: Nilai MAE juga menunjukkan rata-rata kesalahan yang relatif rendah, memperkuat bukti bahwa model memberikan prediksi yang cukup akurat.
  - Model ini mampu memberikan hasil rekomendasi yang relevan berdasarkan pola interaksi pengguna lain, sesuai tujuan dari sistem rekomendasi berbasis Collaborative Filtering.

### Evaluation Content-Based Filtering
Untuk mengukur seberapa baik rekomendasi dari model Content-Based Filtering, dilakukan evaluasi berbasis similarity judul menggunakan library fuzzywuzzy. FuzzyWuzzy adalah library Python yang menggunakan algoritma Levenshtein distance untuk mengukur kesamaan antara dua string[9]. Evaluasi dilakukan dengan pendekatan berikut:

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
| Precision@10  | 0.3000|
| Recall@10     | 0.0323|
| Hit Rate      | 1.0000|

Angka-angka ini menunjukkan bahwa model mampu merekomendasikan film dengan judul yang relevan dalam jumlah yang signifikan, dan setidaknya ada satu film relevan di hasil rekomendasi.

## Kesimpulan
Evaluasi model Collaborative Filtering (SVD) menghasilkan nilai RMSE sebesar 0.8684 dan MAE sebesar 	0.6776, yang menunjukkan bahwa model ini cukup baik dalam memprediksi rating pengguna terhadap film. Sementara itu, Content-Based Filtering yang dievaluasi menggunakan Precision@10 sebesar 0.3000, Recall@10 sebesar 0.0323, dan Hit Rate sebesar 1.0000, menunjukkan bahwa sistem mampu merekomendasikan film yang cukup relevan berdasarkan kemiripan judul, meskipun cakupannya terhadap semua film relevan masih terbatas. Dari kesimpulan ini model Collaborative Filtering (SVD) lebih cocok ketimbang Content-Based Filtering karena dataset yang saya gunakan cenderung lebih cenderung lebih besar dan kurang beragam beragam. Collaborative Filtering (SVD) sangat efektif dalam menangkap pola tersembunyi dalam preferensi pengguna yang tidak bergantung pada fitur eksplisit dari item (seperti genre, aktor, atau deskripsi film). Sebaliknya, Content-Based Filtering membutuhkan fitur eksplisit dari item untuk membuat rekomendasi, yang mungkin tidak tersedia atau tidak lengkap dalam dataset yang saya gunakan.

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

[10] Attalariq, M., & Baizal, Z. K. A. (2023). Chatbot-Based Book Recommender System Using Singular Value Decomposition. Journal of Information System Research (JOSH), 4(4). https://doi.org/10.47065/josh.v4i4.3817
