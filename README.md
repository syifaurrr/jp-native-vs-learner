# jp-native-vs-learner

**Pattern Analysis and Recognition — Japanese Native vs. Learner Speech**

Analisis akustik dan pemodelan machine learning untuk membedakan pola tutur penutur asli bahasa Jepang (*native*) dengan pembelajar (*learner*), menggunakan fitur-fitur prosodi dan spektral dari sinyal audio.

---

## 📌 Ringkasan Proyek

Repositori ini berisi pipeline analisis sinyal wav untuk mengekstraksi fitur akustik dari sampel tutur bahasa Jepang, kemudian membangun model klasifikasi (SVM, Random Forest, kNN) untuk memprediksi apakah sebuah sampel berasal dari penutur native atau learner.

Fokus utamanya adalah **perbandingan pola prosodi** (F0, durasi, laju suku kata) dan **kualitas spektral** (MFCC, formant) yang membedakan kedua kelompok.

---

## 🗂️ Struktur Repositori

```
.
├── data/
│   ├── data_raw/              # Audio mentah (WAV, dilacak via Git LFS)
│   ├── data_preprocessed/     # Audio setelah preprocessing (WAV, LFS)
│   └── metadata.csv           # Metadata sampel (speaker, kelas, dll.)
├── notebooks/
│   ├── 01-exploration.ipynb   # Eksplorasi awal & modelling dasar
│   ├── eda.ipynb              # Analisis data eksploratif
│   ├── all.ipynb              # Pipeline lengkap (fitur + model)
│   ├── all_rapi.ipynb         # Versi rapi dari pipeline lengkap
│   ├── features.csv           # Tabel fitur hasil ekstraksi
│   └── *.png                  # Visualisasi (boxplot F0, distribusi prosodi, feature importance)
├── .gitattributes             # Konfigurasi Git LFS untuk *.wav
└── .gitignore
```

---

## 🎛️ Fitur Akustik yang Diekstraksi

Berdasarkan `notebooks/features.csv`, fitur yang digunakan mencakup:

| Kategori | Fitur |
|---|---|
| **Spektral (MFCC)** | `mfcc1_mean` – `mfcc13_mean`, `mfcc1_std` – `mfcc13_std` |
| **Delta MFCC** | `delta1_mean` – `delta13_mean`, `delta1_std` – `delta13_std` (termasuk orde 2) |
| **Prosodi (F0)** | `f0_mean`, `f0_std`, `f0_range`, `f0_delta_mean_abs`, `f0_delta_std` |
| **Durasi & Laju** | `voiced_duration`, `syllable_rate` |
| **Kualitas Suara** | `jitter_local`, `shimmer_local` |
| **Formant** | `f1_mean`, `f2_mean`, `f1f2_ratio` |

Fitur-fitur ini sejalan dengan literatur analisis aksen L2 bahasa Jepang, di mana **F0 peak alignment** dan **formant /u/** dilaporkan sebagai prediktor kuat persepsi aksen. Selain itu, perbedaan **durasi relatif** dan **F0** antara penutur native dan non-native juga konsisten ditemukan dalam studi akustik lintas bahasa.

---

## 🧪 Model yang Digunakan

Beberapa model klasifikasi telah diimplementasikan di `01-exploration.ipynb` dan `all.ipynb`:

- **Support Vector Machine (SVM)**
- **Random Forest**
- **k-Nearest Neighbors (kNN)**

Visualisasi *feature importance* dari seluruh model tersedia di `notebooks/feature_importances_all_models.png`.

---

## ⚙️ Setup & Instalasi

### Prasyarat

- Python 3.10+
- Git LFS (untuk mengunduh file WAV)

### Langkah-langkah

```bash
# 1. Clone repo (pastikan Git LFS terpasang)
git lfs install
git clone https://github.com/syifaurrr/jp-native-vs-learner.git
cd jp-native-vs-learner

# 2. Buat virtual environment
python -m venv .venv
source .venv/bin/activate    # Linux/macOS
# .venv\Scripts\activate     # Windows

# 3. Install dependensi
pip install -r requirements.txt
```

> **Catatan:** File audio `.wav` disimpan menggunakan **Git LFS**. Jika kamu meng-clone tanpa Git LFS, file akan berupa pointer teks, bukan audio.

---

## 🚀 Cara Menjalankan

1. Buka notebook secara berurutan:
   - `notebooks/all_rapi.ipynb` — pipeline lengkap (fitur + model) yang sudah dirapikan.
   - `notebooks/modelling_rapi.ipynb` — modelling berdasarkan fitur yang telah diekstrak.

3. Fitur yang sudah diekstraksi tersedia di `notebooks/features.csv` jika ingin langsung melompat ke pemodelan.

4. Metadata sampel dapat dilihat di `data/metadata.csv`.

---

## 🔐 Catatan Privasi Data

- File `rename_mapping_PRIVATE.csv` dan `data/bulk_rename_anonymize.py` **tidak disertakan** dalam repositori ini untuk melindungi identitas subjek.
- Data audio telah dianonimkan sebelum dipublikasikan.
- Jika kamu menemukan data sensitif yang tidak sengaja terpublikasi, silakan buka issue atau hubungi pemilik repo.

---

## 📊 Visualisasi

Beberapa visualisasi yang dihasilkan:

- `boxplot_f0_gender.png` — distribusi F0 berdasarkan gender.
- `distribusi_prosodi.png` — distribusi fitur prosodi per kelas.
- `feature_importances_all_models.png` — perbandingan feature importance antar model.

---

## 📚 Referensi Terkait

- Studi tentang akustik aksen L2 Jepang oleh Mandarin speakers menemukan bahwa **F2 /u/**, **F0 peak alignment**, dan **kontur F0** menjadi prediktor utama persepsi aksen.
- Perbedaan **durasi relatif** dan **F0** antara penutur native dan non-native juga dilaporkan dalam analisis lintas bahasa.
- Pendekatan *nativeness score* berbasis deviasi F0 dan durasi dari penutur native dapat dilihat pada model prediksi naturalitas.

---

## 📄 Lisensi

Belum ditentukan. Silakan tambahkan lisensi yang sesuai (mis. MIT, CC BY-NC 4.0) jika repo ini akan dipublikasikan lebih luas.

---

## 🙋 Kontribusi

Repo ini merupakan bagian dari proyek akademik *Pattern Analysis and Recognition*. Untuk pertanyaan atau kolaborasi, silakan buka issue di repositori ini.
