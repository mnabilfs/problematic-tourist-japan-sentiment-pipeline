# Pipeline Preprocessing: Problematic Tourist Sentiment di Jepang

Repositori ini memuat *pipeline preprocessing* otomatis untuk membersihkan, menormalisasi, dan melabeli (*keyword-based labeling*) data keluhan masyarakat/netizen dari media sosial mengenai fenomena "Problematic Tourist" atau masalah-masalah pariwisata yang terjadi di Jepang (seperti *Overtourism*, Pelanggaran Etika, dan Isu Ekonomi). Proyek ini merupakan pemenuhan tugas mata kuliah **Data, Informasi, dan Pengetahuan**.

---

## 📂 Struktur Folder
Repositori ini telah diorganisir ke dalam struktur direktori standar sebagai berikut:
- **`data/raw/`**: Berisi dataset mentah asli (`data_komentar_jepang.csv`) yang dikumpulkan melalui *crawling* Twitter.
- **`data/processed/`**: Berisi seluruh *output* dataset yang sudah dibersihkan dan dilabeli, termasuk file sampel untuk validasi manual.
- **`scripts/`**: Memuat *Jupyter Notebook* utama (`pipeline_preprocessing.ipynb`) yang berisi alur kode *preprocessing* dari tahap inisiasi hingga validasi.
- **`dictionary/`**: Direktori yang memuat berkas pengetahuan eksternal berformat JSON, yaitu kamus normalisasi kata gaul (`slang_dict.json`) dengan >100 entri kata.

---

## ⚙️ Tahapan Pipeline

Pipeline dalam proyek ini dirancang sistematis mencakup 5 tahapan utama (Minggu 1-12):

1. **Inisiasi & Akuisisi Data:** Pengumpulan data mentah >1.000 baris terkait fenomena turis bermasalah di Jepang dari Twitter/X.
2. **Data Profiling:** Analisis kekotoran data (distribusi *noise* seperti emoji, URL, mention), persentase duplikasi, deteksi *missing value*, dan analisis frekuensi kata gaul yang dominan.
3. **Basic Cleaning:** Proses pembersihan dasar seperti *case folding*, menghapus karakter HTML, emoji, tanda baca, serta *filtering* tweet yang terlalu pendek.
4. **Advanced Normalization (Slang-to-Formal):** Konversi bahasa gaul/slang menjadi kosa kata baku (formal) menggunakan kamus JSON eksternal, tanpa mengubah makna *Data Integrity* (Integritas Data).
5. **Domain-Specific Labeling & Validation:** Pemberian multi-label (Overtourism, Etika, Regulasi, Ekonomi) secara otomatis menggunakan *keyword-based labeling*, dilanjutkan dengan simulasi validasi manual beserta kalkulasi tingkat akurasi dan visualisasi *Word Cloud*.

---

## 📊 Deskripsi Dataset
Dataset yang digunakan dalam proyek ini adalah kumpulan data teks (komentar) berbahasa Indonesia (dan sedikit bahasa Inggris) yang membahas tentang turis asing di Jepang.
- **Kondisi Awal (`data/raw/`)**: Format tabular, kotor, penuh *noise* (URL, emoji, hashtag, html tags), terdapat baris kosong, dan mayoritas menggunakan bahasa tidak baku.
- **Kondisi Akhir (`data/processed/`)**: Telah dinormalisasi (bahasa baku), bebas *noise*, dan dilengkapi dengan kolom **Label** kategori permasalahan pariwisata.

---

## 🚀 Langkah-Langkah Menjalankan Kode
Untuk mereplikasi atau menjalankan ulang *pipeline* ini, ikuti petunjuk berikut:

1. **Clone Repositori**:
   ```bash
   git clone https://github.com/mnabilfs/problematic-tourist-japan-sentiment-pipeline.git
   cd problematic-tourist-japan-sentiment-pipeline
   ```
2. **Instalasi Pustaka (Dependencies)**:
   Pastikan Python sudah terpasang. Instal seluruh *library* pendukung melalui terminal:
   ```bash
   pip install -r requirements.txt
   ```
3. **Menjalankan Pipeline**:
   - Buka Jupyter Notebook atau ekstensi Jupyter di VSCode.
   - Buka file `scripts/pipeline_preprocessing.ipynb`.
   - Jalankan (*Run All*) mulai dari *cell* pertama hingga terakhir.
   - Seluruh visualisasi (Grafik Noise dan Word Cloud) akan di-_render_ otomatis, dan hasil pemrosesan akan disalin ke folder `data/processed/`.

---

## ☁️ Visualisasi Data
Sebagai bentuk validasi performa pembersihan (Data Profiling vs Data Bersih), pipeline ini akan menghasilkan beberapa *plot*:
- **Bar Chart Noise**: Menunjukkan jumlah baris data yang mengandung *link*, *mention*, dan *emoji* berlebih pada tahap awal.
- **Word Cloud (Sebelum vs Sesudah)**: Perbandingan visual yang menggambarkan transisi kata-kata dari yang awalnya penuh hashtag/*slang* menjadi sekumpulan kata baku yang bersih dan terfokus pada substansi masalah.
