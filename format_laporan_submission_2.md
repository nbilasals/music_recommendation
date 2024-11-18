# Sistem Rekomendasi Musik Spotify

## Project Overview

Di era digital, layanan streaming musik seperti Spotify menjadi andalan banyak orang untuk mendengarkan lagu favorit mereka. Dengan jutaan lagu yang tersedia, tantangan terbesar adalah membantu pengguna menemukan musik yang sesuai dengan selera mereka tanpa harus mencarinya secara manual. Di sinilah pentingnya sistem rekomendasi musik.

Spotify menggunakan berbagai algoritma canggih untuk memberikan rekomendasi lagu. Salah satu pendekatan yang sering digunakan adalah content-based filtering. Pendekatan ini bekerja dengan menganalisis fitur-fitur konten dalam hal ini audio untuk mencocokkan lagu berdasarkan karakteristik musik itu sendiri, bukan hanya popularitas atau genre.
  
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

1. **Content-Based Filtering**  
   Sistem rekomendasi ini memanfaatkan fitur audio yang tersedia dalam dataset Spotify, seperti *danceability*, *valence*, dan *energy*. Pendekatan ini menggunakan *cosine similarity* untuk mengukur kesamaan antar lagu berdasarkan fitur-fitur tersebut. Sistem ini ideal untuk memberikan rekomendasi personal bagi pengguna, bahkan tanpa data historis yang besar.  

2. **Hybrid Approach: Content-Based Filtering dan Collaborative Filtering**  
   Selain menggunakan *content-based filtering*, pendekatan ini mengombinasikan data preferensi pengguna (seperti pola mendengarkan musik) dengan karakteristik lagu untuk menghasilkan rekomendasi yang lebih akurat dan kaya konteks.  

3. **Genre-Based Clustering**  
   Pendekatan ini mengelompokkan lagu berdasarkan genre menggunakan teknik clustering (misalnya *K-Means*). Setelah cluster terbentuk, sistem dapat merekomendasikan lagu dari cluster yang sama dengan lagu yang disukai pengguna. Metode ini berguna untuk memastikan relevansi rekomendasi berbasis genre.  

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

## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.

## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
