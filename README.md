# Pipeline Preprocessing: Problematic Tourist Sentiment di Jepang

Repositori ini memuat *pipeline preprocessing* otomatis untuk membersihkan, menormalisasi, dan melabeli (*keyword-based labeling*) data keluhan masyarakat/netizen dari media sosial mengenai fenomena "Problematic Tourist" atau masalah-masalah pariwisata yang terjadi di Jepang (seperti Overtourism, Pelanggaran Etika, dan Isu Ekonomi).

---

## 📂 Struktur Folder
Repositori ini telah diorganisir ke dalam struktur direktori standar sebagai berikut:
- **`data/raw/`**: Berisi dataset mentah asli (`data_komentar_jepang.csv`) yang belum diproses.
- **`data/processed/`**: Berisi seluruh *output* dataset yang sudah dibersihkan dan dilabeli.
- **`scripts/`**: Memuat *Jupyter Notebook* (`pipeline_preprocessing.ipynb`) yang berisi alur kode *preprocessing* langkah demi langkah.
- **`dictionary/`**: Direktori yang memuat berkas pengetahuan (berupa file JSON) yang berisi kamus normalisasi kata gaul (`slang_dict.json`).

---

## 📊 Deskripsi Dataset
Dataset yang digunakan dalam proyek ini adalah kumpulan data teks (komentar) sosial media (Twitter/X) berbahasa Indonesia yang membahas tentang turis asing di Jepang.
- **Kondisi Awal (`data/raw/`)**: Memiliki format tabular dengan teks yang masih memuat duplikasi, *noise* (URL, emoji, mention, hashtag), dan bahasa tidak baku (gaul).
- **Kondisi Akhir (`data/processed/`)**: Telah dinormalisasi (bahasa baku), bebas dari *noise*, dan dilengkapi dengan kolom **Label** kategori permasalahan (Overtourism, Etika, Regulasi, atau Ekonomi).

---

## 🚀 Langkah-Langkah Menjalankan Kode
Untuk mereplikasi atau menjalankan ulang *pipeline* ini, ikuti petunjuk berikut:

1. **Clone Repositori**:
   ```bash
   git clone https://github.com/mnabilfs/problematic-tourist-japan-sentiment-pipeline.git
   cd problematic-tourist-japan-sentiment-pipeline
   ```
2. **Instalasi Pustaka (Dependencies)**:
   Pastikan Python sudah terpasang. Instal seluruh *library* yang diperlukan menggunakan file `requirements.txt` melalui terminal:
   ```bash
   pip install -r requirements.txt
   ```
3. **Menjalankan Pipeline**:
   - Buka Jupyter Notebook atau ekstensi Jupyter di VSCode.
   - Buka file `scripts/pipeline_preprocessing.ipynb`.
   - Jalankan (*Run All*) mulai dari *cell* pertama hingga terakhir.
   - Proses ini akan secara otomatis membaca dataset mentah dari `data/raw/` dan menyalin semua hasil pemrosesan (termasuk *sampling* validasi manual) ke dalam folder `data/processed/`.

---

## ☁️ Visualisasi Word Cloud
Sebagai bentuk validasi performa pembersihan (Data Profiling vs Data Bersih), pipeline ini akan menghasilkan visualisasi kata-kata dominan (**Word Cloud**) yang disisipkan di akhir file Notebook:
- **Word Cloud - Sebelum Pembersihan**: Menggambarkan kondisi teks yang masih penuh dengan URL terpotong, hashtag, *slang*, atau emoji yang terlewat.
- **Word Cloud - Sesudah Pembersihan**: Menunjukkan daftar kata-kata yang bersih, berfokus murni pada substansi atau inti pesan (misal: "turis", "jepang", "ramai", "sopan") yang siap dimasukkan ke dalam algoritma *Machine Learning*.
