# Panduan belajar 00 — Menyiapkan lingkungan eksperimen

Panduan ini menemani [notebook 00_environment](../notebook/00_environment.ipynb). Baca bagian yang sesuai sebelum menjalankan setiap sel kode. Anda tidak harus menghafal semua perintah: pahami tujuan, bagian yang perlu diubah, dan bukti keberhasilannya terlebih dahulu.

## 1. Sebenarnya kita sedang membuat apa?

Notebook ini menyiapkan tempat kerja dan mencatat kondisi komputer untuk eksperimen Banking77. **Belum ada pengunduhan dataset, pelatihan model, atau pengukuran akurasi.**

Bayangkan eksperimen seperti memasak. Dataset adalah bahan, kode adalah resep, versi pustaka adalah versi alat, seed adalah pengatur beberapa keputusan acak, dan folder run adalah buku catatan untuk satu percobaan. Agar percobaan bisa ditelusuri, kita perlu menyimpan lebih dari hasil akhirnya.

Dalam skripsi, pencatatan ini membantu menjawab: “Hasil tersebut diperoleh memakai perangkat, versi perangkat lunak, dan pengaturan apa?” Catatan lingkungan mendukung keterulangan; ia tidak menjamin hasil selalu persis sama atau otomatis membuat eksperimen valid.

## 2. Istilah dasar

| Istilah | Penjelasan sederhana |
| --- | --- |
| Notebook | Dokumen yang berisi penjelasan dan sel kode yang dapat dijalankan. |
| Sel kode | Satu blok instruksi Python. Sel berikutnya dapat membutuhkan variabel dari sel sebelumnya. |
| Runtime | Komputer/sesi yang menjalankan kode. Pada notebook ini, runtime berada di Colab. |
| Variabel | Nama untuk menyimpan nilai sementara, misalnya `PROJECT_DIR` menyimpan lokasi folder. |
| Dependency/pustaka | Paket kode siap pakai yang dibutuhkan program, seperti NumPy atau PyTorch. |
| CPU | Prosesor untuk pekerjaan komputasi umum. |
| GPU | Perangkat yang dapat mempercepat jenis komputasi tertentu, termasuk pelatihan BERT. |
| CUDA | Teknologi komputasi GPU NVIDIA yang digunakan PyTorch ketika tersedia dan cocok. |
| Seed | Angka awal untuk penghasil bilangan acak agar sebagian proses acak lebih mudah diulang. |
| Run | Satu pelaksanaan percobaan dengan identitas dan folder hasil sendiri. |
| Artefak | Berkas hasil kegiatan: konfigurasi, catatan lingkungan, model, atau prediksi. |
| Reproducibility | Kemampuan mengulang proses penelitian dengan kondisi dan prosedur yang terdokumentasi. |

## 3. Persiapan dan urutan menjalankan

1. Siapkan folder `Skripsi_Banking77` di bagian My Drive Google Drive. Notebook sengaja tidak membuat folder utama ini.
2. Buka notebook di Google Colab. Kode `google.colab`, lokasi `/content/drive`, dan perintah pemeriksaan sistemnya memang ditujukan untuk Colab; menjalankannya langsung di Python Windows memerlukan penyesuaian.
3. Pilih perangkat sesuai pekerjaan. Untuk BERT, gunakan GPU yang tersedia. Untuk pekerjaan CPU atau pengukuran latency CPU, ikuti protokol eksperimen.
4. Jalankan sel kode dari atas ke bawah. Tunggu setiap sel selesai sebelum melanjutkan.
5. Jika pemasangan paket meminta restart, restart runtime lalu jalankan kembali dari awal. Paket yang sudah memiliki versi yang diminta dapat dilewati pemasangannya sesuai komentar notebook.
6. Periksa hasil akhir di Drive, bukan hanya pesan pada layar.

**Drive dan runtime berbeda.** Variabel Python hidup di memori runtime. Berkas yang sudah berhasil ditulis ke Drive tetap dapat digunakan setelah runtime dimulai ulang. Restart menghapus variabel; membuka notebook lain juga tidak otomatis memindahkan variabel dari notebook ini.

## 4. Sel 1 — Hubungkan Drive dan tentukan folder proyek

### Tujuan dan analogi

Kita menghubungkan lemari penyimpanan ke meja kerja, lalu menunjukkan laci proyek yang akan dipakai.

### Membaca kode

- `from google.colab import drive`: mengambil fitur penghubung Drive dari Colab.
- `drive.mount('/content/drive')`: membuat Drive dapat diakses melalui lokasi tersebut. Ikuti proses akses yang muncul.
- `from pathlib import Path`: mengambil alat untuk menangani lokasi berkas/folder.
- `PROJECT_DIR = Path(...)`: menyimpan alamat folder utama proyek.
- `PROJECT_DIR.exists()`: memeriksa apakah lokasi tersebut sudah ada.
- `raise FileNotFoundError(...)`: menghentikan proses jika lokasi tidak ditemukan agar hasil tidak tersimpan di tempat yang salah.
- `print(...)`: menampilkan informasi; perintah ini sendiri tidak menyimpan berkas.

**Yang boleh diubah:** isi lokasi `PROJECT_DIR` jika folder Anda berbeda. Lokasi Drive pada Colab menggunakan `/`, bukan alamat Windows seperti `D:\...`.

### Contoh keluaran dan arti

```text
PROJECT_DIR: /content/drive/MyDrive/Skripsi_Banking77
Folder proyek ditemukan dan siap dipakai.
```

Artinya lokasi berhasil ditemukan. Jika gagal, periksa koneksi Drive, akun yang dipakai, nama folder, dan apakah folder berada pada lokasi yang sesuai.

## 5. Sel 2 — Buat struktur folder

### Tujuan

Pisahkan bahan mentah, hasil kerja, dan keluaran eksperimen agar mudah ditelusuri.

| Folder | Kegunaan |
| --- | --- |
| `notebooks` | Menyimpan notebook di struktur proyek Drive. |
| `data/raw` | Dataset asli yang dipertahankan tanpa perubahan. |
| `data/processed` | Hasil pengolahan dan metadata sesuai tahap eksperimen. |
| `data/splits` | Indeks pembagian data. |
| `configs` | Konfigurasi eksperimen. |
| `runs` | Folder hasil setiap percobaan. |
| `reports` | Laporan dan visualisasi hasil. |
| `app` | Berkas aplikasi pada tahap implementasi. |
| `requirements` | Catatan versi paket. |

`PROJECT_SUBDIRS` adalah daftar nama folder. `for` mengulangi pekerjaan untuk setiap nama. Pada objek `Path`, tanda `/` menggabungkan lokasi folder. `mkdir(parents=True, exist_ok=True)` membuat folder beserta induk yang diperlukan dan mengizinkan folder yang sudah ada.

**Hasil berhasil:** muncul daftar `Subfolder siap`. Folder yang baru dibuat boleh masih kosong. Sel ini tidak mengunduh data atau otomatis memindahkan notebook Anda ke Drive.

Catatan penamaan: folder lokal pada workspace ini bernama `notebook`, sedangkan struktur Drive yang dibuat kode bernama `notebooks`. Keduanya adalah lokasi berbeda, bukan bukti bahwa berkas otomatis tersalin.

## 6. Sel 3 — Pasang paket dengan versi tertentu

### Mengapa nomor versi dicantumkan?

Pustaka dapat berubah antarversi. Penulisan `numpy==1.26.4`, misalnya, meminta versi tersebut secara spesifik. Ini membantu menjaga kondisi eksperimen lebih konsisten.

`%pip install` menjalankan pemasangan paket dari notebook. `--quiet` mengurangi keluaran rutin, bukan menghilangkan kemungkinan kesalahan. Tanda `\` di akhir baris menyambungkan perintah ke baris berikutnya.

| Paket | Peran dalam rangkaian eksperimen |
| --- | --- |
| `transformers` | Komponen model dan tokenizer seperti BERT. |
| `datasets` | Membaca dan mengelola dataset. |
| `accelerate` | Membantu eksekusi model pada perangkat yang tersedia. |
| `scikit-learn` | TF-IDF, SVM, dan alat evaluasi. |
| `pandas` | Mengolah data berbentuk tabel. |
| `numpy` | Operasi array dan komputasi numerik. |
| `evaluate` | Alat pengukuran/evaluasi. |
| `sentencepiece` | Pustaka tokenisasi untuk model yang membutuhkannya; dipasang sebagai bagian baseline, bukan berarti setiap model memakainya. |

PyTorch tidak dipaksa berganti versi oleh sel pemasangan ini. Notebook mencatat versi PyTorch dan CUDA yang tersedia pada runtime.

**Cara menilai hasil:** baca keluaran pemasangan. Pesan `Pemasangan selesai` saja belum membuktikan semua paket kompatibel. Periksa kesalahan dan versi aktual pada sel 6. Versi yang ditetapkan adalah baseline notebook, bukan jaminan kompatibel dengan setiap runtime Colab di masa depan.

Jika pemasangan gagal, jangan melanjutkan seolah lingkungan sudah benar. Simpan pesan kesalahan, identifikasi paket/versi yang bermasalah, dan catat setiap perubahan baseline yang memang diperlukan.

## 7. Sel 4 — Buat identitas percobaan

`EXPERIMENT_SEED = 42` menetapkan seed percobaan. `PURPOSE = 'environment'` menandai tujuan run ini: menyiapkan dan mencatat lingkungan.

`RUN_ID` menggabungkan waktu, tujuan, dan seed. Contoh ilustratif:

```text
2026-10-31_090000_environment_seed42
```

Angka waktu mengikuti zona waktu runtime, sehingga jangan langsung menganggapnya WIB. Catatan waktu UTC juga disimpan pada sel 6.

`RUN_DIR` adalah lokasi hasil run. `exist_ok=False` membuat kode berhenti jika nama folder sudah ada, sehingga tidak diam-diam menggunakan folder run lama. Nama berbasis waktu sampai detik mengurangi benturan; jika dijalankan dua kali dalam detik yang sama, benturan masih mungkin terjadi.

Di dalamnya disiapkan `metrics`, `predictions`, `models`, dan `logs`. Pada tahap lingkungan, folder ini wajar masih kosong.

**Yang boleh diubah:** `PURPOSE` dan seed sesuai rancangan eksperimen. Jangan mengubah seed sembarangan demi memperoleh skor lebih tinggi. Catatan notebook menyebut seed BERT final 13, 42, dan 2026; masing-masing memerlukan run terpisah.

**Penting saat mengulang:** menjalankan sel ini lagi pada waktu berbeda membuat identitas dan folder run baru. Semua sel berikutnya memakai nilai `RUN_DIR` yang terakhir aktif. Jangan mengulang sel ini di tengah eksperimen tanpa sengaja.

## 8. Sel 5 — Atur sumber keacakan

### Analogi

Seed seperti titik awal pada daftar angka acak. Memakai titik awal yang sama membantu menghasilkan urutan yang sama dalam kondisi yang sesuai.

Fungsi `set_experiment_seed(seed)` mengelompokkan beberapa instruksi agar dapat dipanggil bersama:

- `random.seed`: mengatur generator acak bawaan Python.
- `np.random.seed`: mengatur generator acak NumPy yang digunakan melalui API tersebut.
- `torch.manual_seed`: mengatur seed PyTorch.
- Pemeriksaan `torch.cuda.is_available()`: memastikan pengaturan khusus CUDA dijalankan ketika tersedia.
- Pengaturan cuDNN dan deterministic: meminta perilaku operasi yang lebih mudah diulang, dengan kemungkinan konsekuensi pada kecepatan.

`warn_only=True` berarti operasi tertentu yang tidak deterministik dapat menghasilkan peringatan alih-alih menghentikan proses. Karena itu, **seed yang sama bukan jaminan semua hasil identik**, terutama jika perangkat, versi, urutan operasi, atau pengaturan data berubah.

Catatan teknis: mengisi `PYTHONHASHSEED` di dalam proses Python yang sudah berjalan tidak mengatur ulang pengacakan hash proses tersebut. Jangan menafsirkan baris itu sebagai jaminan penuh determinisme Python.

**Hasil berhasil:** muncul `Seed aktif: 42` untuk konfigurasi awal. Ini menunjukkan fungsi sudah dipanggil, bukan pengujian bahwa seluruh eksperimen pasti deterministik.

## 9. Sel 6 — Catat kondisi lingkungan

Sel ini membuat variabel `environment`, yaitu kumpulan pasangan nama dan nilai. Analogi sederhananya adalah formulir identitas komputer dan perangkat lunak.

| Informasi | Alasan dicatat |
| --- | --- |
| `run_id`, `purpose`, `seed` | Mengaitkan lingkungan dengan percobaan. |
| Waktu lokal dan UTC | Mengetahui kapan pencatatan dilakukan. |
| Python dan sistem operasi | Mengenali lingkungan eksekusi. |
| CPU dan RAM | Memberi konteks sumber daya komputasi. |
| PyTorch, CUDA, cuDNN | Menelusuri perangkat lunak komputasi model. |
| `packages` | Menyimpan versi paket yang benar-benar terpasang. |
| `gpu` | Mencatat ketersediaan GPU dan rincian perangkat ketika tersedia. |
| `nvidia_smi` | Menyimpan keluaran pemeriksaan perangkat NVIDIA. |

`package_version(...)` membaca versi paket; jika paket tidak ditemukan, hasilnya `None` yang tampil sebagai `null` pada JSON. `command_output(...)` menjalankan perintah pemeriksaan dan mengembalikan keterangan `not available` jika perintah gagal atau tidak ada.

`json.dumps(..., indent=2)` mengubah catatan menjadi teks JSON yang mudah dibaca. Pada tahap ini catatan baru ditampilkan; penyimpanan berkas dilakukan pada sel berikutnya.

Contoh bagian keluaran pada runtime CPU:

```json
{
  "gpu": {
    "available": false
  }
}
```

Ini wajar untuk pekerjaan CPU. Jika Anda bermaksud melatih BERT dengan GPU, periksa pengaturan runtime sebelum melanjutkan. `torch_cuda_build` menjelaskan versi CUDA yang terkait dengan build PyTorch; nilai itu sendiri tidak membuktikan GPU dapat dipakai. Gunakan juga `gpu.available`.

## 10. Sel 7 — Simpan bukti ke Drive

### Empat keluaran utama

| Berkas | Isi dan kegunaan |
| --- | --- |
| `runs/<RUN_ID>/environment.json` | Catatan perangkat, seed, waktu, dan versi paket. |
| `runs/<RUN_ID>/requirements.lock.txt` | Daftar paket yang dilaporkan oleh `pip freeze` pada runtime. |
| `requirements/<RUN_ID>_core.lock.txt` | Versi aktual delapan paket inti yang terdeteksi. |
| `runs/<RUN_ID>/config.json` | Identitas run dan lokasi catatan/lock terkait. Pada notebook ini belum berisi seluruh hyperparameter model. |

`write_text(..., encoding='utf-8')` menulis teks ke berkas. `relative_to(PROJECT_DIR)` menyimpan lokasi relatif terhadap folder utama, misalnya `runs/.../environment.json`.

Catatan versi lengkap dan catatan paket inti berbeda: yang pertama merekam lebih banyak paket runtime, sedangkan yang kedua merangkum paket inti yang dipilih notebook. Nama “lock” di sini adalah nama berkas catatan versi; berkas ini tidak menyimpan driver, perangkat keras, atau seluruh kondisi sistem dan bukan jaminan lingkungan dapat dipulihkan secara identik.

**Perhatian saat mengulang:** run berbeda memiliki lokasi terpisah. Namun menjalankan sel 7 kembali tanpa membuat `RUN_ID` baru akan menulis ulang berkas dengan nama yang sama pada run aktif. Perlindungan antarrun tidak berarti setiap penulisan berkas bersifat tidak dapat ditimpa.

Jika `pip freeze` gagal, fungsi pembantu dapat menghasilkan teks `not available` dan teks itu tetap ditulis ke berkas. Karena itu, buka berkas lock dan pastikan isinya benar-benar daftar paket, bukan hanya mengandalkan pesan `Tersimpan`.

## 11. Contoh pemeriksaan akhir

Setelah selesai, struktur hasil kurang lebih seperti ini. Nama run berikut hanya ilustrasi.

```text
Skripsi_Banking77/
  runs/
    2026-10-31_090000_environment_seed42/
      environment.json
      requirements.lock.txt
      config.json
      metrics/
      predictions/
      models/
      logs/
  requirements/
    2026-10-31_090000_environment_seed42_core.lock.txt
```

- [ ] Semua sel selesai tanpa kesalahan yang belum ditangani.
- [ ] `PROJECT_DIR` menunjuk proyek yang benar.
- [ ] Ketersediaan GPU sesuai jenis pekerjaan.
- [ ] Versi aktual paket sudah diperiksa.
- [ ] Keempat berkas keluaran ada dan dapat dibuka di Drive.
- [ ] Berkas lock berisi daftar paket yang masuk akal.
- [ ] Anda dapat menjelaskan tujuan run dan seed yang dipakai.

Tidak adanya model atau skor akurasi pada tahap ini adalah normal.

## 12. Masalah umum

| Gejala | Yang perlu diperiksa/dilakukan |
| --- | --- |
| `Folder proyek tidak ditemukan` | Buat folder utama atau benarkan `PROJECT_DIR`; pastikan Drive terhubung. |
| `No module named google.colab` saat dijalankan lokal | Buka notebook di Colab untuk mengikuti alur ini. |
| `NameError`, misalnya `RUN_DIR` belum ada | Jalankan sel prasyarat; sesudah restart, mulai lagi dari atas. |
| Instalasi gagal atau muncul konflik versi | Baca pesan paket yang bermasalah; jangan menganggap pesan akhir sebagai bukti berhasil. |
| GPU tidak tersedia | Pastikan jenis runtime sesuai; untuk tugas CPU hal ini dapat normal. |
| `FileExistsError` saat membuat run | Nama run sudah ada. Jangan hapus hasil lama; jalankan kembali sel pembuat run setelah waktunya berbeda. |
| Banyak folder run kosong | Sel pembuat run mungkin dijalankan berulang sebelum penyimpanan; identifikasi run aktif dari keluaran `RUN_DIR`. |
| Berkas tidak ditemukan di Drive | Periksa lokasi yang dicetak sel 7 dan apakah sel penulisan selesai. |

## 13. Setelah notebook ini selesai

Lanjutkan ke [01_dataset_acquisition.ipynb](../notebook/01_dataset_acquisition.ipynb) sesuai urutan roadmap proyek. Baca prasyarat notebook berikutnya terlebih dahulu.

Menjalankan notebook 00 tidak otomatis menerapkan seed dan variabelnya ke notebook lain. Setiap runtime eksperimen harus menyiapkan atau memuat pengaturan yang diperlukan dan merekam lingkungan yang benar-benar dipakai pada percobaan tersebut.

Saat eksperimen model dijalankan, simpan pula konfigurasi model, pembagian data, prediksi, metrik, dan artefak model yang sesuai. `environment.json` hanya menjelaskan lingkungan, bukan pengganti semua bukti eksperimen.

## 14. Latihan pemahaman singkat

1. **Mengapa menyimpan hasil ke Drive?** Agar berkas hasil yang sudah ditulis tidak bergantung pada keberlangsungan memori runtime.
2. **Apakah seed 42 berarti akurasi 42%?** Tidak. Seed mengatur titik awal proses acak, bukan target skor.
3. **Apakah `PURPOSE = 'bert_seed42'` otomatis melatih BERT?** Tidak. Itu hanya nama tujuan run.
4. **Mengapa satu folder untuk satu run?** Agar konfigurasi dan hasil percobaan berbeda tidak tercampur.
5. **Apakah notebook ini sudah menguji model?** Belum. Ia menyiapkan dan mendokumentasikan lingkungan.

Jika Anda bisa menjelaskan lima jawaban ini dengan kata-kata sendiri serta menemukan berkas keluaran di Drive, dasar penggunaan notebook ini sudah dipahami.
