# Sistem Rekomendasi Musik Spotify

## Project Overview

Di era digital, layanan streaming musik seperti Spotify menjadi andalan banyak orang untuk mendengarkan lagu favorit mereka. Dengan jutaan lagu yang tersedia, tantangan terbesar adalah membantu pengguna menemukan musik yang sesuai dengan selera mereka tanpa harus mencarinya secara manual. Di sinilah pentingnya sistem rekomendasi musik.

Salah satu pendekatan yang sering digunakan adalah **content-based filtering**. Pendekatan ini bekerja dengan menganalisis fitur-fitur konten dalam hal ini audio untuk mencocokkan lagu berdasarkan karakteristik musik itu sendiri, bukan hanya popularitas atau genre. Berbeda dengan *collaborative filtering* yang mengandalkan data perilaku pengguna lain, content-based filtering berfokus pada karakteristik musik itu sendiri. Keuntungannya adalah rekomendasi yang lebih personal dan relevan.
  
Referensi: [MUSIC RECOMMENDATION SYSTEM BASED ON COSINE SIMILARITY AND SUPERVISED GENRE CLASSIFICATION](https://ejournal.nusamandiri.ac.id/index.php/jitk/article/view/4324) 

## Business Understanding

### Problem Statements  

1.  Pengguna layanan streaming musik kesulitan menemukan lagu yang sesuai dengan selera mereka tanpa harus mencari secara manual di tengah jutaan pilihan lagu. Hal ini mengurangi pengalaman pengguna dan menyebabkan frustrasi.  

2. Sistem rekomendasi berbasis popularitas atau artis terkadang kurang personal dan tidak mencerminkan preferensi musik pengguna secara spesifik, terutama untuk pengguna baru yang belum memiliki banyak data historis.  

### Goals  

1. Mengembangkan sistem rekomendasi musik yang dapat menyarankan lagu-lagu berdasarkan karakteristik audio seperti *danceability*, *valence*, *tempo*, dan *energy*, sehingga lebih relevan dengan preferensi pengguna.  

2. Menggunakan algoritma *content-based filtering* untuk memberikan rekomendasi personal yang tidak bergantung pada popularitas atau data historis pengguna, melainkan karakteristik dari lagu itu sendiri.  

### Solution Approach  

#### Solution Statements  

1. **Content-Based Filtering Berdasarkan Fitur Audio**  
   Pendekatan ini menggunakan algoritma K-Nearest Neighbors (KNN) untuk mengidentifikasi lagu-lagu yang memiliki kemiripan berdasarkan fitur audio, seperti danceability, energy, loudness, tempo, dan valence. Pendekatan ini memberikan rekomendasi lagu yang memiliki karakteristik serupa secara akustik dengan lagu yang dipilih pengguna.

2. **Content-Based Filtering Berdasarkan Genre Lagu**  
   Sistem ini memanfaatkan algoritma cosine similarity untuk mengukur tingkat kemiripan antar lagu berdasarkan kategori track genre. Pendekatan ini dirancang untuk memberikan rekomendasi lagu dari genre yang sama dengan lagu yang disukai pengguna.

---

## Data Understanding

Dataset ini diambil dari [Kaggle - Spotify Tracks Dataset](https://www.kaggle.com/datasets/maharshipandya/-spotify-tracks-dataset), berisi informasi tentang lagu-lagu yang tersedia di Spotify dengan berbagai fitur yang relevan untuk keperluan sistem rekomendasi musik. Dataset mencakup 89.740 baris dan 21 kolom, yang merepresentasikan lagu-lagu dari 125 genre yang berbeda.  

### Deskripsi Variabel
Berikut adalah deskripsi variabel pada dataset:  
- **track_id**: ID unik lagu di Spotify.  
- **artists**: Nama artis yang tampil di lagu tersebut (jika lebih dari satu, dipisahkan dengan ";").  
- **album_name**: Nama album tempat lagu tersebut berada.  
- **track_name**: Judul lagu.  
- **popularity**: Popularitas lagu, dengan nilai antara 0-100. Popularitas dihitung berdasarkan jumlah dan waktu terakhir lagu diputar.  
- **duration_ms**: Durasi lagu dalam milidetik.  
- **explicit**: Indikator apakah lagu memiliki lirik eksplisit (true/false).  
- **danceability**: Tingkat kesesuaian lagu untuk menari, berdasarkan elemen seperti tempo dan stabilitas ritme. (Skala 0.0–1.0).  
- **energy**: Tingkat intensitas dan aktivitas lagu, dengan nilai tinggi menunjukkan lagu yang cepat dan keras (Skala 0.0–1.0).  
- **key**: Nada dasar dari lagu, direpresentasikan dengan nilai integer (0–11).  
- **loudness**: Tingkat kekerasan suara lagu dalam desibel (dB).  
- **mode**: Modus skala lagu, 1 untuk mayor dan 0 untuk minor.  
- **speechiness**: Proporsi konten lisan dalam lagu. Nilai mendekati 1 menunjukkan dominasi kata-kata.  
- **acousticness**: Kemungkinan bahwa lagu bersifat akustik (Skala 0.0–1.0).  
- **instrumentalness**: Kemungkinan bahwa lagu tidak memiliki vokal (Skala 0.0–1.0).  
- **liveness**: Kemungkinan bahwa lagu dilakukan secara langsung (Skala 0.0–1.0).  
- **valence**: Tingkat emosi positif yang dipancarkan lagu, nilai tinggi menggambarkan suasana ceria.  
- **tempo**: Tempo lagu dalam beats per minute (BPM).  
- **time_signature**: Pola irama lagu, direpresentasikan dengan integer (3–7).  
- **track_genre**: Genre lagu.  

### Kondisi Data
- **Jumlah Data**: 89.740 baris dan 21 kolom.
- **Duplikasi Data**: Terdapat 24.259 track ID yang terduplikasi.
- **Ketersediaan Fitur**: Dataset mencakup fitur-fitur audio, metadata lagu, dan informasi popularitas.  
### Kondisi Missing Values

Dataset ini memiliki beberapa nilai yang hilang pada kolom tertentu, berikut rincian jumlah missing values per kolom:  

| Kolom              | Jumlah Missing Values |
|---------------------|-----------------------|
| Unnamed: 0         | 0                     |
| track_id           | 0                     |
| artists            | 1                     |
| album_name         | 1                     |
| track_name         | 1                     |
| popularity         | 0                     |
| duration_ms        | 0                     |
| explicit           | 0                     |
| danceability       | 0                     |
| energy             | 0                     |
| key                | 0                     |
| loudness           | 0                     |
| mode               | 0                     |
| speechiness        | 0                     |
| acousticness       | 0                     |
| instrumentalness   | 0                     |
| liveness           | 0                     |
| valence            | 0                     |
| tempo              | 0                     |
| time_signature     | 0                     |
| track_genre        | 0                     |

Dapat dilihat bahwa kolom **artists**, **album_name**, dan **track_name** masing-masing memiliki satu nilai yang hilang.

## Data Preparation

Pada tahap ini, dilakukan beberapa langkah penting untuk mempersiapkan data sebelum melakukan analisis lebih lanjut. Proses data preparation yang diterapkan di dalam notebook dan laporan adalah sebagai berikut:

### 1. Drop Duplicate Values
Pada tahap ini, data duplikat dihapus berdasarkan kolom 'track_id'. Duplikasi ini bisa terjadi akibat adanya data yang sama dalam dataset yang mungkin tidak relevan untuk analisis lebih lanjut. Untuk itu, dilakukan langkah berikut:

```python
data_no_duplicates = data.drop_duplicates(subset='track_id')
```
**Alasan**:
**Proses**: Menghapus baris yang memiliki nilai duplikat berdasarkan track_id.
**Alasan**: Setiap track_id mewakili lagu yang unik. Jika terdapat duplikasi data dengan track_id yang sama, maka hal ini dapat menyebabkan hasil analisis atau model menjadi bias, karena satu lagu akan dihitung lebih dari satu kali. Oleh karena itu, langkah ini bertujuan untuk memastikan setiap lagu hanya muncul sekali.

### 2. Menghapus Nilai yang Hilang

```python
data = data_no_duplicates.dropna()
```
**Proses**:Menghapus baris yang memiliki nilai kosong atau missing values pada kolom mana pun.
**Alasan**: Nilai yang hilang, terutama pada kolom-kolom penting seperti artists, album_name, atau track_name, dapat mengganggu kelancaran analisis atau pengembangan model. Misalnya, jika ada lagu tanpa nama artis atau album, maka sistem rekomendasi tidak dapat bekerja dengan baik. Oleh karena itu, langkah ini memastikan hanya data yang lengkap yang digunakan dalam tahap analisis selanjutnya.

### 3. Menghapus Kolom yang Tidak Relevan

```python
data = data.drop('Unnamed: 0', axis=1)

```
**Proses**: Menghapus kolom Unnamed: 0.
**Alasan**: Kolom ini biasanya merupakan hasil dari proses pengindeksan atau pengimporan data dan tidak mengandung informasi yang relevan untuk analisis. Menghapus kolom yang tidak diperlukan ini bertujuan untuk meminimalkan noise pada dataset dan menjaga agar hanya kolom yang relevan yang digunakan dalam analisis.


## **Modeling**
Tahapan ini membahas model sistem rekomendasi yang dibangun untuk menyelesaikan permasalahan. Pada bagian ini, disajikan dua pendekatan berbeda yang menghasilkan *Top-N Recommendation* sebagai output.

### **1. K-Nearest Neighbors (KNN) Berdasarkan Fitur Audio**
Pendekatan ini memanfaatkan data numerik pada kolom:
- `danceability`
- `energy`
- `loudness`
- `tempo`
- `valence`

Model ini menggunakan algoritma *K-Nearest Neighbors (KNN)* untuk mencari lagu dengan karakteristik audio yang mirip berdasarkan jarak Euclidean.

#### **Langkah-langkah Implementasi:**
1. **Inisialisasi Model KNN:**
   Model dilatih menggunakan data dari kolom fitur numerik.
2. **Proses Rekomendasi:**
   Untuk setiap lagu input, model mencari lagu-lagu lain yang memiliki nilai fitur audio terdekat.

#### **Kode Implementasi:**
```python
from sklearn.neighbors import NearestNeighbors

# Inisialisasi dan pelatihan model KNN
X = data[['danceability', 'energy', 'loudness', 'tempo', 'valence']]
knn = NearestNeighbors(n_neighbors=10, algorithm='ball_tree')
knn.fit(X)

# Fungsi untuk rekomendasi berdasarkan nama lagu
def recommend_tracks_by_name(track_name, n_recommendations=5):
    if track_name in data['track_name'].values:
        idx_data = data[data['track_name'] == track_name].index[0]
        idx_X = X.index.get_loc(idx_data)
        distances, indices = knn.kneighbors([X.iloc[idx_X]], n_neighbors=n_recommendations + 1)
        recommended_tracks = data.iloc[indices[0][1:]]
        return recommended_tracks[['track_name', 'artists', 'track_genre']]
    else:
        print(f"Track name '{track_name}' not found in the dataset.")
        return None
```

#### **Hasil (Top-5 Recommendation):**
Input lagu: *Call It Fate, Call It Karma*

| **Track Name**         | **Artists**            | **Track Genre** |
|-------------------------|------------------------|------------------|
| Студенточка            | Pyotr Leshchenko      | romance          |
| Same Ole Me            | George Jones          | honky-tonk       |
| Last Date              | Floyd Cramer          | honky-tonk       |
| Нет, не любил он       | Valentina Ponomaryova | romance          |
| Mi Bandoneon Y Yo      | Rubén Juárez;Carlos Garcia | tango         |

---

### **2. Cosine Similarity Berdasarkan Genre Lagu**
Pendekatan ini menggunakan data teks pada kolom `track_genre`. Genre lagu diubah menjadi representasi numerik menggunakan *TF-IDF Vectorizer* dan dihitung tingkat kemiripannya dengan *cosine similarity*.

#### **Langkah-langkah Implementasi:**
1. **TF-IDF Vectorization:**
   Data genre diubah menjadi representasi numerik menggunakan *Term Frequency-Inverse Document Frequency (TF-IDF)*.
2. **Perhitungan Cosine Similarity:**
   Digunakan untuk menghitung kesamaan antara genre lagu input dan lagu lainnya.

#### **Kode Implementasi:**
```python
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity

# TF-IDF Vectorization
tf = TfidfVectorizer()
tfidf_matrix = tf.fit_transform(data['track_genre'])

# Menghitung Cosine Similarity
cosine_sim = cosine_similarity(tfidf_matrix)

# Fungsi rekomendasi berdasarkan genre
def music_recommendations(nama_track, similarity_data, items, k=5):
    index = similarity_data.loc[:, nama_track].to_numpy().argpartition(range(-1, -k, -1))
    closest = similarity_data.columns[index[-1:-(k+2):-1].ravel()]
    closest = closest.drop(nama_track, errors='ignore')
    return pd.DataFrame(closest).merge(items).head(k)
```

#### **Hasil (Top-5 Recommendation):**
Input lagu: *Call It Fate, Call It Karma*

| **Track Name**            | **Artists**         | **Track Genre** |
|----------------------------|---------------------|------------------|
| Razón - En Vivo           | Los Caligaris       | alt-rock         |
| Razón - En Vivo           | Los Caligaris       | alt-rock         |
| Baby Came Home 2 / Valentines | The Neighbourhood | alt-rock         |
| Love It If We Made It      | The 1975           | alt-rock         |
| Butterfly                 | Crazy Town          | alt-rock         |

---

### **Kelebihan dan Kekurangan Pendekatan**

| **Pendekatan**                    | **Kelebihan**                                                                                                                                             | **Kekurangan**                                                                                                                |
|-----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| **KNN Berdasarkan Fitur Audio**  | - Memberikan rekomendasi personal berdasarkan nuansa dan karakteristik audio. <br> - Cocok untuk pengguna yang mencari lagu dengan suasana spesifik.     | - Membutuhkan data numerik yang terstandarisasi. <br> - Kurang efektif jika fitur audio tidak merepresentasikan preferensi pengguna secara keseluruhan. |
| **Cosine Similarity Berdasarkan Genre** | - Mudah diterapkan pada data berbasis teks. <br> - Relevan untuk eksplorasi lagu dalam genre tertentu.                                                   | - Tidak memperhitungkan fitur audio lain, sehingga kurang fleksibel untuk pengguna yang ingin variasi dalam genre.             |


## Evaluation
Sistem rekomendasi dievaluasi dengan menggunakaan precision. 
**Precision** mengukur seberapa banyak item yang relevan (similar) dari semua item yang direkomendasikan. Artinya, semakin banyak rekomendasi yang relevan (mempunyai genre yang sama dengan input pengguna), semakin tinggi precision-nya. 
Precision dihitung dengan rumus:

$$
\text{Precision} = \frac{\text{Jumlah rekomendasi yang relevan}}{\text{Jumlah item yang direkomendasikan}}
$$

### Langkah-langkah evaluasi menggunakan Precision:
1. **Rekomendasi yang Diberikan:**  
   Sistem memberikan 5 item rekomendasi untuk lagu *Call It Fate, Call It Karma*. Dalam hal ini, semua rekomendasi yang diberikan adalah lagu dengan genre **alt-rock**.

2. **Relevansi Rekomendasi:**  
   Dari 5 item yang direkomendasikan, semuanya memiliki genre yang sama, yaitu **alt-rock**, yang relevan dengan genre dari lagu input (*Call It Fate, Call It Karma*).

3. **Menghitung Precision:**  
   Karena semua rekomendasi memiliki genre yang relevan (alt-rock), maka precision-nya adalah:
   
$$
\text{Precision} = \frac{5}{5} = 1 \text{ atau } 100\%
$$

Ini berarti 100% dari rekomendasi yang diberikan oleh sistem adalah relevan (mempunyai genre yang sesuai).

