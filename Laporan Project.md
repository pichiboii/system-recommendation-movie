# Laporan Proyek Machine Learning - Hafizha Aghnia Hasya
**Project System Recommendation : Movie**

## **Project Overview**
Sistem rekomendasi telah menjadi bagian penting dalam pengalaman pengguna di berbagai platform digital, terutama dalam industri hiburan seperti film dan serial. Dengan jumlah konten yang terus meningkat setiap harinya, pengguna sering kali kesulitan memilih tontonan yang sesuai dengan preferensi mereka. Untuk itu, dibutuhkan sistem yang mampu memberikan rekomendasi yang relevan dan personal.

Proyek ini bertujuan untuk membangun sistem rekomendasi film yang dapat menyarankan film kepada pengguna berdasarkan minat dan preferensi mereka. Sistem ini menggunakan data historis dari perilaku pengguna atau fitur film itu sendiri untuk mengidentifikasi pola dan memberikan saran tontonan yang sesuai. Dengan demikian, pengguna dapat menghemat waktu dalam pencarian film dan menikmati konten yang lebih sesuai dengan selera mereka.

Pentingnya proyek ini terlihat dari bagaimana sistem rekomendasi telah terbukti meningkatkan kepuasan pengguna dan waktu keterlibatan dalam platform digital. Sebagai contoh, Netflix melaporkan bahwa sekitar 80% tontonan di platform mereka berasal dari hasil rekomendasi algoritma yang mereka kembangkan (Gomez-Uribe dan Hunt, 2015). Hal ini menunjukkan bahwa sistem rekomendasi tidak hanya meningkatkan pengalaman pengguna, tetapi juga memiliki dampak langsung terhadap keberhasilan bisnis.

## **Business Understanding**

### Problem Statements
1. Berdasarkan data mengenai pengguna, bagaimana membuat sistem rekomendasi yang dipersonalisasi dengan teknik content-based filtering?
2. Dengan data rating yang dimiliki, bagaimana perusahaan dapat merekomendasikan film lain yang mungkin disukai dan belum pernah ditonton oleh pengguna?

### Goals
- Membangun sistem rekomendasi film yang mampu memberikan saran tontonan secara personal berdasarkan preferensi pengguna.
- Mengimplementasikan dan membandingkan dua pendekatan sistem rekomendasi: content-based filtering dan collaborative filtering.

### Solution Statements
Untuk mencapai tujuan tersebut, proyek ini mengusulkan dua pendekatan solusi utama:
1. Content-Based Filtering
2. Collaborative Filtering

## **Data Understanding**
### Sumber Dataset
https://www.kaggle.com/datasets/parasharmanas/movie-recommendation-system/
### Deskripsi Dataset
- Dataset yang digunakan adalah MovieLens dataset, salah satu dataset paling populer dalam pengembangan sistem rekomendasi. Terdiri atas data penilaian (rating) pengguna terhadap film dan metadata seperti judul dan genre.
- Terdapat dua dataset : dataset film yang memuat informasi film dan dataset rating yang berisi rating dari pengguna terhadap film tertentu.
- Dataset film (movies.csv) dan rating (ratings.csv) yang pertama kali dimuat tidak memiliki missing value.
- Selain itu, tidak ditemukan adanya data duplikat pada kedua dataset tersebut.
- Sebelum dilakukan eksplorasi dan pemodelan, dataset telah melalui tahap pembersihan dan seleksi awal (detail dijelaskan pada bagian Data Preparation).
- Dataset film yang digunakan berisi 26.362 baris data dengan 3 fitur: movieId, title, dan genres.
- Dataset rating yang digunakan berisi 5.388.010 baris data dengan 3 fitur: userId, movieId, dan rating.

### Deskripsi Fitur yang digunakan
1. movieId (int64): ID unik yang merepresentasikan setiap film. Digunakan sebagai penghubung antara dataset film dan dataset rating.
2. title (object): Judul lengkap dari film, biasanya disertai dengan tahun rilis.
3. genres (object/list): Daftar genre yang dimiliki oleh film dalam bentuk list, misalnya ['Action', 'Thriller']. Genre sebelumnya berupa string dengan pemisah “|” dan telah diproses menjadi list agar memudahkan analisis berbasis genre.
4. userId (int64): ID unik untuk setiap pengguna yang memberikan rating.
5. movieId (int64): ID film yang menerima rating dari pengguna, yang dapat dihubungkan dengan kolom movieId pada dataset film.
6. rating (float64): Nilai rating yang diberikan oleh pengguna terhadap film tertentu, dalam skala 0.5 sampai 5.0.

### *Exploratory Data Analysis*
Exploratory Data Analysis (EDA) adalah tahap eksplorasi data yang telah melalui proses pembersihan untuk memahami karakteristik dataset. Pada tahap ini, dilakukan descriptive statistics, univariate dan multivariate analysis.

#### a. *Descriptive Statistics*
Statistik deskriptif bertujuan untuk memberikan gambaran umum mengenai karakteristik data secara numerik. Ukuran statistik seperti mean, median, standar deviasi, nilai minimum, maksimum, serta kuartil digunakan untuk menganalisis fitur numerik dalam dataset. Selain itu, karakteristik mengenai fitur kategorik juga dapat dilihat dari descriptive statistics-nya.

Dalam project ini, statistik deskriptif digunakan untuk memahami distribusi nilai dari setiap fitur numerik dan kategorik. Salah satu fitur numerik yang dianalisis adalah rating. Hasil analisis menunjukkan bahwa rating yang diberikan pengguna mulai dari 0.5 hingga 5.0 dengan rata-rata 3.522. Sementara itu, untuk fitur kategorik seperti genres, hasil analisis menunjukkan genre yang paling banyak adalah drama dengan frekuensi 4139 film.

#### b. *Univariate & Multivariate Analysis*
Univariate analysis bertujuan untuk memahami distribusi data dari satu fitur secara individual, sedangkan multivariate analysis digunakan untuk mengidentifikasi pola dan hubungan antar dua atau lebih fitur dalam dataset.

Pada project ini, univariate analysis menghasilkan grafik visualisasi mengenai banyak film per tahunnya seperti berikut :
![banyak film per tahun](https://raw.githubusercontent.com/pichiboii/system-recommendation-movie/main/archive/jml%20film%20per%20tahun.png)
Dari grafik tersebut, dapat disimpulkan bahwa banyak film yang dirilis secara bertahap bertambah mulai dari tahun 2005 hingga mencapai puncaknya pada tahun 2015 yang kemudian menurun pada tahun 2018.

Selain itu, dilakukan juga analisis banyak film per genre nya dengan grafik seperti berikut :
![banyak film per genre](https://raw.githubusercontent.com/pichiboii/system-recommendation-movie/main/archive/banyak%20film%20per%20genre.png)
Dari grafik tersebut, dapat dilihat bahwa film paling banyak bergenre Drama, diikuti genre Comedy. Sedangkan paling sedikit film bergenre Film-Noir dan IMAX.

Kemudian, dilakukan juga analisis distribusi rating dan dihasilkan grafik sebagai berikut :
![distribusi rating](https://raw.githubusercontent.com/pichiboii/system-recommendation-movie/main/archive/distribusi%20rating.png)
Dari grafik tersebut, dapat disimpulkan bahwa distribusi rating menunjukkan pola left-skewd, di mana pengguna paling banyak memberikan rating pada rentang 4.0 hingga 5.0

## **Data Preparation**
Sebelum membangun model sistem rekomendasi, dilakukan beberapa tahap pembersihan dan transformasi data untuk memastikan kualitas serta relevansi data yang digunakan. Adapun langkah-langkah data cleaning yang dilakukan adalah sebagai berikut:
1. Menghapus kolom ```timestamp``` pada dataset rating karena tidak memberikan kontribusi langsung terhadap proses rekomendasi, serta tidak digunakan dalam perhitungan atau analisis lebih lanjut.
2. Menyaring data film berdasarkan tahun rilis, di mana hanya film yang dirilis pada tahun 2005 atau setelahnya yang dipertahankan. Hal ini bertujuan agar sistem rekomendasi lebih fokus pada konten yang relatif lebih baru dan relevan dengan preferensi pengguna saat ini.
3. Melakukan transformasi pada kolom ```genres```, dari string yang sebelumnya dipisahkan oleh tanda pipe (```|```) menjadi format list/array. Contohnya, string seperti ```"Drama|Comedy"``` diubah menjadi ```["Drama", "Comedy"]```. Transformasi ini memudahkan dalam analisis dan manipulasi berbasis genre.
4. Menghapus entri film yang tidak memiliki informasi genre, karena genre merupakan fitur penting dalam pendekatan content-based filtering. Film tanpa genre tidak dapat dianalisis lebih lanjut untuk menemukan kesamaan konten.

### *Data Preparation - Content-Based Filtering*
Setelah data dibersihkan, data disiapkan agar dapat digunakan pada pendekatan content-based filtering. Fokus dari pendekatan ini adalah pada informasi konten film, khususnya genre. Berikut adalah tahapan persiapan datanya:
1. Preprocessing Genre
Karakter khusus seperti tanda hubung (-) dalam nama genre (misalnya Sci-Fi) diubah menjadi tanpa tanda hubung agar lebih konsisten saat diolah oleh algoritma NLP.
2. Ekstraksi Fitur dengan TF-IDF
Untuk merepresentasikan setiap film berdasarkan genre-nya, digunakan teknik TF-IDF (Term Frequency-Inverse Document Frequency). Teknik ini akan mengubah data string genre menjadi vektor numerik yang mencerminkan pentingnya genre tertentu dalam keseluruhan dataset.
Matriks hasil transformasi ini nantinya digunakan untuk menghitung kemiripan antar film berdasarkan genre, yang menjadi dasar dalam memberikan rekomendasi.

### *Data Preparation - Collaborative Filtering*
Untuk pendekatan collaborative filtering, fokusnya adalah pada interaksi antara pengguna dan film (dalam hal ini berupa rating). Persiapan data bertujuan untuk memastikan proses pelatihan model dapat berjalan optimal.
1. Pengacakan Data
Dataset diacak terlebih dahulu untuk menghindari bias urutan data ketika dilakukan pembagian data latih dan validasi.
2. Normalisasi Rating
Rating yang diberikan pengguna dinormalisasi ke skala 0 hingga 1 menggunakan rumus min-max normalization. Hal ini membantu model lebih cepat konvergen dan menghindari ketimpangan skala input.
```y = df2['rating'].apply(lambda x: (x - min_rating) / (max_rating - min_rating)).values```
3. Pemisahan Data untuk Pelatihan dan Validasi
Data disiapkan untuk pelatihan model dengan membentuk pasangan fitur dan target, di mana fitur terdiri dari kombinasi userId dan movieId, sedangkan targetnya adalah rating yang telah dinormalisasi. Selanjutnya, data dibagi menjadi dua bagian, yaitu 80% untuk data pelatihan (training set) dan 20% untuk data validasi (validation set), guna mengevaluasi performa model terhadap data yang belum pernah dilihat sebelumnya.

## **Modeling**
Pada project ini, dikembangkan sistem rekomendasi film dengan dua pendekatan berbeda, yaitu content-based filtering dan collaborative filtering. Kedua metode ini bertujuan untuk menghasilkan rekomendasi film yang dipersonalisasi berdasarkan preferensi pengguna.

### 1. *Content-Based Filtering*
Content-based filtering merekomendasikan film berdasarkan kemiripan konten (dalam hal ini, genre). Metode ini mengasumsikan bahwa pengguna akan menyukai film yang memiliki genre serupa dengan film yang disukai sebelumnya.

#### Kelebihan dan Kekurangan Cosine Similarity
Kelebihan :
- Tidak bergantung pada data pengguna lain: Sistem dapat memberikan rekomendasi meskipun pengguna baru belum memberikan rating, selama ada informasi konten (genre) dari item yang tersedia.
- Interpretasi lebih jelas: Karena berbasis konten (seperti genre), alasan di balik rekomendasi dapat lebih mudah dijelaskan.
- Cepat dan efisien: Perhitungan cosine similarity relatif ringan dan cepat dilakukan setelah representasi vektor terbentuk.

Kekurangan :
- Rentan terhadap keterbatasan informasi konten: Jika informasi genre tidak lengkap atau terlalu umum, maka akurasi rekomendasi bisa rendah.
- Kurang mampu menangkap selera pengguna secara personal: Sistem hanya fokus pada kemiripan antar item, tanpa mempertimbangkan preferensi aktual pengguna.

#### Langkah Pembuatan
Berikut langkah-langkah yang dilakukan dalam pembuatan model Content-Based Filtering dengan Cosine Similarity :
1. Menghitung ```cosine similarity``` antar film menggunakan fungsi ```cosine_similarity``` dari pustaka ```sklearn``` pada matriks TF-IDF yang sudah disiapkan sebelumnya.
2. Membuat fungsi ```movie_recommendations()``` untuk menghasilkan daftar film serupa berdasarkan input judul film dan jumlah rekomendasi yang diinginkan.
Fungsi tersebut menghasilkan daftar film yang paling mirip dengan film input berdasarkan skor kemiripan (misalnya dari cosine similarity). Fungsi ini bekerja dengan mencari k film teratas yang paling mirip, lalu mengembalikan informasi judul dan genre-nya, dengan mengecualikan film input dari daftar hasil rekomendasi.

#### *Top 5 Recommendation (Content-Based Filtering)*
Untuk pendekatan Content-Based Filtering, hasil rekomendasi didapatkan dengan cara mengamati kualitas Top-N rekomendasi berdasarkan film yang pernah ditonton atau disukai pengguna.

Ketika pengguna sebelumnya menyukai film "Death Kiss (2018)" dengan genre sebagai berikut :
|title|genre|
|-------|---------|
| Death Kiss (2018)   | [Action, Thriller] |

Maka sistem merekomendasikan 5 film :

|title|genre|
|-------|---------|
| Hotel California (2008) | [Action, Thriller] |
| HHhH (2017) | [Action, Thriller] |
|Renegades (2017) | [Action, Thriller] |
| Alleycats (2016) | [Action, Thriller] |
| Daylight (Daglicht) (2013) | [Action, Thriller] |

Rekomendasi tersebut relevan karena memiliki genre yang sama yaitu Action dan Thriller.

### 2. *Collaborative Filtering*
Collaborative filtering memanfaatkan pola interaksi pengguna dan film untuk memberikan rekomendasi. Sistem ini mengidentifikasi preferensi pengguna berdasarkan kesamaan perilaku antar pengguna.

#### Kelebihan dan Kekurangan RecommenderNet
Kelebihan :
- Mampu mempelajari pola kompleks: Model berbasis neural network dapat menangkap hubungan non-linier antara pengguna dan item, sehingga hasil rekomendasi lebih personal dan adaptif.
- Menghasilkan prediksi yang lebih akurat: Model dapat mempelajari representasi laten dari interaksi pengguna-item, yang sering kali lebih efektif dibanding pendekatan tradisional.
- Skalabilitas baik: Cocok diterapkan pada dataset besar karena arsitektur embedding mendukung efisiensi komputasi.

Kekurangan :
- Membutuhkan banyak data interaksi: Performa model menurun jika jumlah rating dari pengguna masih sedikit (masalah cold start).
- Proses pelatihan lebih kompleks dan memakan waktu: Dibandingkan metode sederhana seperti cosine similarity, model ini memerlukan sumber daya lebih besar dan proses pelatihan yang lebih lama.
- Kurang interpretatif: Sulit untuk menjelaskan alasan spesifik di balik suatu rekomendasi karena model bersifat “black-box”.

#### Langkah Pembuatan
Berikut langkah-langkah yang dilakukan dalam pembuatan model Collaborative Filtering dengan RecommenderNet :
1. Membangun model dengan arsitektur ```RecommenderNet```, yang memanfaatkan embedding untuk merepresentasikan pengguna dan film dalam bentuk vektor berdimensi 50.
2. Menghitung skor prediksi dengan melakukan dot product antara vektor embedding pengguna dan film, lalu menerapkan fungsi aktivasi sigmoid untuk mengubah skor menjadi nilai antara 0 dan 1.
3. Melatih model dengan Binary Crossentropy sebagai fungsi loss, Adam sebagai optimizer, dan Root Mean Squared Error (RMSE) sebagai metrik evaluasi.
4. Model dilatih selama 5 epoch dengan batch size 128, dan menghasilkan performa akhir berupa:
    - Loss training: 0.6299
    - RMSE training: 0.2281
    - Loss validasi: 0.6328
    - RMSE validasi: 0.2305
5. Setelah pelatihan, sistem akan menyaring daftar film yang belum pernah ditonton oleh pengguna untuk dijadikan kandidat rekomendasi personal.

#### *Top 10 Recommendation (Collaborative Filtering)*
Untuk pendekatan Collaborative Filtering, sistem memberikan Top-N rekomendasi film untuk pengguna tertentu berdasarkan skor kecocokan. Misalnya user 3959 yang memberikan rating tinggi pada film berikut :
| Title                                                | Genre                      |
| ---------------------------------------------------- | -------------------------- |
| Hunt, The (Jagten) (2012)                            | \[Drama]                   |
| Winter Sleep (Kis Uykusu) (2014)                     | \[Drama]                   |
| Ex Machina (2015)                                    | \[Drama, Sci-Fi, Thriller] |
| The Jinx: The Life and Deaths of Robert Durst (2015) | \[Documentary]             |

Maka sistem merekomendasikan 10 film berikut :

| Title                                          | Genre                             |
| ---------------------------------------------- | --------------------------------- |
| Girl from Monday, The (2005)                   | \[Action, Comedy, Sci-Fi]         |
| Miss Congeniality 2: Armed and Fabulous (2005) | \[Adventure, Comedy, Crime]       |
| Guess Who (2005)                               | \[Comedy, Romance]                |
| Ballad of Jack and Rose, The (2005)            | \[Drama]                          |
| Beauty Shop (2005)                             | \[Comedy]                         |
| Dust to Glory (2005)                           | \[Action, Adventure, Documentary] |
| Sahara (2005)                                  | \[Action, Adventure, Comedy]      |
| Fever Pitch (2005)                             | \[Comedy, Romance]                |
| We (2018)                                      | \[Drama]                          |
| Bad Poems (2018)                               | \[Comedy, Drama]                  |

Rekomendasi tersebut relevan dengan preferensi pengguna, terutama karena banyaknya film yang memiliki genre mirip dengan film-film yang sebelumnya disukai oleh pengguna. Hal ini menunjukkan bahwa model collaborative filtering berhasil mempelajari pola ketertarikan pengguna berdasarkan perilaku pengguna lain yang mirip.

## **Evaluation**
Untuk mengukur performa sistem rekomendasi, dilakukan evaluasi terhadap model collaborative filtering menggunakan Root Mean Squared Error (RMSE).

### Metrik Evaluasi: RMSE
Root Mean Squared Error (RMSE) merupakan metrik yang sering digunakan untuk mengevaluasi kualitas prediksi nilai numerik, seperti rating film. Metrik ini mengukur rata-rata selisih kuadrat antara nilai aktual dan nilai prediksi. Berikut adalah formula untuk mendapatkan nilai RMSE :
![formula RMSE](https://raw.githubusercontent.com/pichiboii/system-recommendation-movie/main/archive/Screenshot%202025-05-28%20130956.png)
Di mana n adalah banyak data, yi adalah rating aktual, dan y^i adalah rating hasil prediksi. RMSE memberikan penalti lebih besar untuk prediksi yang jauh dari nilai sebenarnya. Semakin kecil nilai RMSE, maka semakin baik kualitas prediksi dari model.

### Hasil Evaluasi
Selama proses training pada collaborative filtering, metrik evaluasi (RMSE loss) divisualisasikan pada setiap epoch untuk memantau performa model sebagai berikut :
![visualisasi evaluasi](https://raw.githubusercontent.com/pichiboii/system-recommendation-movie/main/archive/visualisasi%20evaluasi.png)
Grafik menunjukkan bahwa pada epoch pertama, terjadi penurunan drastis pada RMSE data latih, dari sekitar 0.25 ke sekitar 0.222, yang menunjukkan proses pelatihan awal berjalan efektif. Setelah itu, RMSE pada data latih dan data validasi cenderung berfluktuasi namun tetap stabil, dengan nilai RMSE berkisar di angka 0.22–0.23. 

Meskipun terdapat sedikit perbedaan antara error training dan testing, grafik tidak menunjukkan tanda overfitting atau underfitting yang signifikan. Hal ini menandakan bahwa model dapat belajar dengan baik dari data latih dan tetap mempertahankan performa yang konsisten pada data validasi. Dengan nilai RMSE yang cukup rendah, model ini dinilai berhasil dalam memprediksi preferensi pengguna terhadap film.

## **Referensi**
Gomez-Uribe, Carlos A., and Neil Hunt. "The Netflix Recommender System: Algorithms, Business Value, and Innovation." ACM Transactions on Management Information Systems 6.4 (2015): 1–19.
