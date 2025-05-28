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

## **Modeling**
Pada project ini, dikembangkan sistem rekomendasi film dengan dua pendekatan berbeda, yaitu content-based filtering dan collaborative filtering. Kedua metode ini bertujuan untuk menghasilkan rekomendasi film yang dipersonalisasi berdasarkan preferensi pengguna.

### 1. *Content-Based Filtering*
Content-based filtering merekomendasikan film berdasarkan kemiripan konten (dalam hal ini, genre). Metode ini mengasumsikan bahwa pengguna akan menyukai film yang memiliki genre serupa dengan film yang disukai sebelumnya.

Berikut langkah-langkah yang dilakukan dalam pembuatan model Content-Based Filtering :
- Menggunakan TF-IDF Vectorizer untuk mengekstraksi fitur penting dari kolom ```genres```, menghasilkan representasi vektor untuk setiap film.
- Menghitung ```cosine similarity``` antar film menggunakan fungsi cosine_similarity dari pustaka ```sklearn```.
- Membuat fungsi ```resto_recommendations()``` untuk menghasilkan daftar film serupa berdasarkan input judul film dan jumlah rekomendasi yang diinginkan.

### 2. *Collaborative Filtering*
Collaborative filtering memanfaatkan pola interaksi pengguna dan film untuk memberikan rekomendasi. Sistem ini mengidentifikasi preferensi pengguna berdasarkan kesamaan perilaku antar pengguna.

Berikut langkah-langkah yang dilakukan dalam pembuatan model Collaborative Filtering :
- Membagi data rating menjadi data latih (80%) dan validasi (20%).
- Membangun model dengan pendekatan neural collaborative filtering menggunakan arsitektur ```RecommenderNet``` berbasis embedding untuk user dan item.
- Menghitung dot product antara embedding user dan film untuk memperoleh skor prediksi, lalu menerapkan fungsi aktivasi sigmoid agar skor berada di antara 0 dan 1.
- Menyaring daftar film yang belum ditonton oleh pengguna untuk dijadikan kandidat rekomendasi.

## **Evaluation**
Untuk mengukur performa sistem rekomendasi, dilakukan evaluasi terhadap model collaborative filtering menggunakan Root Mean Squared Error (RMSE).

### Metrik Evaluasi: RMSE
Root Mean Squared Error (RMSE) merupakan metrik yang sering digunakan untuk mengevaluasi kualitas prediksi nilai numerik, seperti rating film. Metrik ini mengukur rata-rata selisih kuadrat antara nilai aktual dan nilai prediksi. Berikut adalah formula untuk mendapatkan nilai RMSE :
![formula RMSE](https://raw.githubusercontent.com/pichiboii/system-recommendation-movie/main/archive/Screenshot%202025-05-28%20130956.png)
Di mana n adalah banyak data, yi adalah rating aktual, dan y^i adalah rating hasil prediksi. RMSE memberikan penalti lebih besar untuk prediksi yang jauh dari nilai sebenarnya. Semakin kecil nilai RMSE, maka semakin baik kualitas prediksi dari model.

### Hasil Evaluasi
Selama proses training, metrik evaluasi (RMSE loss) divisualisasikan pada setiap epoch untuk memantau performa model sebagai berikut :
![visualisasi evaluasi](https://raw.githubusercontent.com/pichiboii/system-recommendation-movie/main/archive/visualisasi%20evaluasi.png)
Grafik menunjukkan bahwa pada epoch pertama, terjadi penurunan drastis pada RMSE data latih, dari sekitar 0.25 ke sekitar 0.222, yang menunjukkan proses pelatihan awal berjalan efektif. Setelah itu, RMSE pada data latih dan data validasi cenderung berfluktuasi namun tetap stabil, dengan nilai RMSE berkisar di angka 0.22–0.23. 

Meskipun terdapat sedikit perbedaan antara error training dan testing, grafik tidak menunjukkan tanda overfitting atau underfitting yang signifikan. Hal ini menandakan bahwa model dapat belajar dengan baik dari data latih dan tetap mempertahankan performa yang konsisten pada data validasi. Dengan nilai RMSE yang cukup rendah, model ini dinilai berhasil dalam memprediksi preferensi pengguna terhadap film.

### *Top 5 Recommendation (Content-Based Filtering)*
Untuk pendekatan Content-Based Filtering, hasil rekomendasi dievaluasi dengan cara mengamati kualitas Top-N rekomendasi berdasarkan film yang pernah ditonton atau disukai pengguna.

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

### *Top 10 Recommendation (Collaborative Filtering)*
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

## **Referensi**
Gomez-Uribe, Carlos A., and Neil Hunt. "The Netflix Recommender System: Algorithms, Business Value, and Innovation." ACM Transactions on Management Information Systems 6.4 (2015): 1–19.
