# indonesian-fake-news-detection

Eksperimen deteksi berita hoaks berbahasa Indonesia menggunakan berbagai kombinasi metode representasi teks dan classifier SVM, dengan studi ablasi pada pipeline preprocessing.

---

## Deskripsi

Proyek ini membandingkan tiga pendekatan representasi teks — **TF-IDF**, **Word2Vec**, dan **IndoBERT** — dikombinasikan dengan **Support Vector Machine (SVM)** untuk mengklasifikasikan artikel berita sebagai *hoax* atau *real*. Eksperimen dilakukan dengan studi ablasi pada empat konfigurasi preprocessing untuk mengukur dampak *stopword removal* dan *stemming* terhadap performa model.

Dataset berita hoaks bersumber dari **TurnBackHoax**, sedangkan berita real diambil dari **CNN Indonesia**, **Kompas**, dan **Tempo**.

---

## Struktur Repository

```
.
├── experimentrm1.ipynb          # Notebook utama eksperimen
├── dataset/
│   └── **/RAW/                  # File dataset .xlsx/.csv mentah
├── preprocessed_cache_full.pkl  # Cache hasil preprocessing (auto-generated)
├── ablation_results.csv         # Hasil ringkasan semua eksperimen (auto-generated)
└── README.md
```

---

## Eksperimen

### Konfigurasi Preprocessing (Ablasi)

| Kode | Stopword Removal | Stemming |
|------|:-:|:-:|
| `C0_baseline` | No | No |
| `C1_stopword` | Yes | No |
| `C2_stemming` | No | Yes |
| `C3_full_pipeline` | Yes | Yes |

### Model yang Diuji

| # | Representasi Teks | Classifier |
|---|---|---|
| 1 | TF-IDF | SVM (Linear, class_weight=balanced) |
| 2 | Word2Vec (dim=100, window=5) | SVM (Linear, class_weight=balanced) |
| 3 | IndoBERT (`indobenchmark/indobert-base-p1`) CLS token | SVM (Linear, class_weight=balanced) |

### Split Data

- Train / Test: **80% / 20%**, stratified, `random_state=42`

---

## Metrik Evaluasi

- Accuracy
- Precision (weighted)
- Recall (weighted)
- F1-Score (weighted)

Hasil lengkap tersimpan otomatis di `ablation_results.csv`.

---

## Instalasi

```bash
pip install pandas numpy scikit-learn gensim transformers torch PySastrawi tqdm openpyxl
```

IndoBERT membutuhkan koneksi internet saat pertama kali dijalankan untuk mengunduh model dari Hugging Face.

---

## Cara Menjalankan

1. Clone repository ini
2. Tempatkan file dataset di `dataset/**/RAW/` sesuai format berikut:
   - Hoax: `dataset_turnbackhoax*.xlsx` / `.csv`
   - Real: `dataset_cnn*.xlsx`, `dataset_kompas*.xlsx`, `dataset_tempo*.xlsx` (dan versi `.csv`)
3. Semua file harus memiliki kolom `FullText`
4. Jalankan semua cell di `experimentrm1.ipynb` secara berurutan

---

## Teknologi

- **Python 3.10.2**
- [PySastrawi](https://github.com/har07/PySastrawi) — stemmer & stopword Bahasa Indonesia
- [Gensim](https://radimrehurek.com/gensim/) — Word2Vec
- [HuggingFace Transformers](https://huggingface.co/) — IndoBERT
- [scikit-learn](https://scikit-learn.org/) — TF-IDF, SVM, evaluasi
- [PyTorch](https://pytorch.org/) — inference IndoBERT

---

## Lisensi

MIT License
