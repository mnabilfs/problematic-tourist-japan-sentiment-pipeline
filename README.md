# Pipeline Preprocessing: Isu Problematic Tourist di Jepang

Dokumentasi ini berisi penjelasan teknis mengenai proses pembangunan *pipeline preprocessing* otomatis untuk menormalisasi dan melabeli data keluhan atau komentar media sosial terkait fenomena turis bermasalah (*problematic tourist*) di Jepang. Pipeline ini disusun menggunakan Python di dalam Jupyter Notebook (`pipeline_preprocessing.ipynb`).

---

## 🛠 Persiapan Lingkungan & Pustaka (Library)
Pipeline ini menggunakan beberapa pustaka utama Python, antara lain:
- **`pandas` & `numpy`**: Untuk manipulasi struktur data (Dataframe).
- **`re` (Regular Expression)**: Untuk pencarian pola (menghapus URL, mention, hashtag).
- **`string`**: Digunakan untuk mengidentifikasi dan menghapus tanda baca standar.
- **`emoji`**: Untuk mendeteksi, menghitung, dan menghapus emoji di dalam teks.
- **`collections.Counter`**: Untuk menghitung frekuensi kemunculan kata pada tahap *data profiling*.

---

## 📊 Tahap 1: Data Profiling (Minggu 3-4)
Tahap ini bertujuan untuk memahami karakteristik data mentah (seberapa "kotor" data tersebut) dengan tiga metrik analisis utama:

1. **Pengecekan Duplikat**: 
   Menghitung persentase duplikasi data teks menggunakan metode bawaan Pandas `df.duplicated().sum()`.
   
2. **Identifikasi Noise (Fungsi `count_noise(text)`)**:
   Fungsi ini membaca setiap baris teks dan mengembalikan tiga nilai metrik:
   - *Emoji*: Menggunakan `emoji.emoji_list(text)` untuk menghitung total emoji.
   - *Link/URL*: Menggunakan regex `r'http\S+|www\.\S+'`.
   - *Mention*: Menggunakan regex `r'@\w+'`.
   
3. **Analisis Frekuensi Kata (Word Count)**:
   Membersihkan seluruh teks dari tanda baca untuk sementara dan mencari 20 kata terbanyak (*most common words*) menggunakan `Counter`. Ini membantu kita menentukan *keyword* untuk pelabelan nanti.

---

## 🧹 Tahap 2: Basic Cleaning (Minggu 5-6)
Tahap pembersihan utama berada pada fungsi **`basic_cleaning(text)`**. Alur yang dilakukan pada fungsi ini meliputi:

1. **Case Folding**: Mengubah semua huruf menjadi huruf kecil (`text.lower()`).
2. **Regex Cleaning**: 
   - Menghapus URL (`http\S+|www\.\S+`).
   - Menghapus Hashtag (`#\w+`).
   - Menghapus Mention (`@\w+`).
   - Menghapus Angka (`\d+`).
3. **Emoji Removal**: Dihapus total menggunakan pustaka `emoji.replace_emoji(text, replace='')`.
4. **Whitespace & Karakter Khusus**: Menghapus baris baru (`\n`, `\r`, `\t`) dan seluruh tanda baca menggunakan `str.maketrans` terhadap `string.punctuation`.
5. **Strip & Normalisasi Spasi**: Menghapus spasi ganda menggunakan `re.sub(r'\s+', ' ', text)`.

Setelah fungsi diaplikasikan, pipeline akan melakukan **Filtering** untuk membuang teks yang sangat pendek (di bawah 3 kata) dan membuang kembali baris yang menjadi duplikat setelah teksnya dibersihkan.

---

## 🗣 Tahap 3: Advanced Normalization (Minggu 7-8)
Tahap ini berfokus pada normalisasi bahasa gaul/singkatan menjadi bahasa baku (Formal).

- **Fungsi `slang_to_formal(text)`**:
  Fungsi ini memecah teks (*tokenize*) berdasarkan spasi (`split()`), kemudian melakukan pencarian pada struktur data *dictionary* Python (`slang_dict`). Jika kata ditemukan di *dictionary*, maka kata tersebut diganti (*mapping*); jika tidak, kata tersebut dibiarkan apa adanya. Teks kemudian disatukan kembali menggunakan `' '.join()`.

---

## 🏷 Tahap 4: Domain-Specific Labeling (Minggu 9-10)
Untuk memberikan konteks spesifik terhadap masalah turis, pipeline menggunakan pendekatan *Keyword-based Labeling* melalui fungsi **`tourist_issue_labeling(text)`**. 

Fungsi ini memiliki *dictionary* bersarang berisi kategori dan *keywords*:
1. **Overtourism & Keramaian**: `['ramai', 'penuh', 'overtourism', 'padat', 'antre', 'banyak orang', 'sesak', 'membludak']`
2. **Pelanggaran Etika & Perilaku**: `['adab', 'sopan', 'maling', 'sampah', 'berisik', 'bising', 'nakal', 'mengganggu', 'kriminal', 'etika', 'kasar']`
3. **Regulasi & Kebijakan**: `['aturan', 'larangan', 'denda', 'sistem', 'dilarang', 'pemerintah', 'pajak', 'visa', 'hukum']`
4. **Isu Ekonomi & Harga**: `['harga', 'murah', 'mahal', 'yen', 'belanja', 'dual pricing', 'bayar', 'ekonomi']`

**Cara Kerja**: 
Fungsi akan mengecek keberadaan setiap *keyword* (menggunakan perulangan `any(word in text)`). Jika sebuah kalimat mengandung kata kunci dari beberapa kategori, label yang dihasilkan akan digabungkan (menggunakan koma). Jika teks tidak memiliki *keyword* tersebut, fungsi mengembalikan *string* `'Lain-lain'`.

---

## ✅ Tahap 5: Validation & Documentation (Minggu 11-12)
Bagian akhir pada pipeline bertujuan untuk mevalidasi dan menyimpan hasil (*export*).
- **Sampling Data**: Pipeline mengambil 100 sampel acak (`random_state=42`) dari dataset dan menyimpannya menjadi `sample_validasi_tourist.csv`. Hal ini digunakan peneliti untuk melakukan pengecekan manual akurasi pelabelan dan pembersihan yang telah dibuat oleh sistem (Teks Mentah vs Teks Bersih vs Label).
- **Dataset Final**: Menyimpan dataset lengkap yang telah selesai dibersihkan dan dilabeli ke dalam format CSV dengan nama `data_final_tourist_berlabel.csv`.
