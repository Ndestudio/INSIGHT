# INSIGHT — Pattern Recognition in Internal Audit

Aplikasi web satu halaman untuk Internal Audit InJourney Aviation Services. Aplikasi ini menguji seluruh populasi transaksi terhadap aturan pola, memberi skor risiko 0–100, dan menyajikan temuannya untuk ditindaklanjuti.

## Fitur

- **Ringkasan**: angka utama, sebaran tingkat risiko, timeline harian, ilustrasi kasus teratas, dan vendor dengan skor tertinggi.
- **Temuan**: daftar transaksi berpola dengan filter, alasan per aturan, status tindak lanjut, dan salin CSV.
- **Laporan pimpinan (PDF)**: dari menu Ringkasan atau Temuan, buka pratinjau laporan eksekutif A4 bergaya dashboard dengan identitas IAS, lalu klik **Unduh PDF** atau **Cetak**. Isinya:
  - *Halaman 1 – Ikhtisar*: sampul dengan periode dan sumber data, 5 indikator utama, pesan utama untuk pimpinan (disusun otomatis dari hasil analisis), sebaran tingkat risiko, temuan per jenis pola, vendor berisiko, status tindak lanjut, dan timeline harian.
  - *Halaman 2 – Prioritas & rekomendasi*: 10 temuan berisiko tertinggi, temuan per kelompok pola, uji Benford's Law, metodologi singkat, rekomendasi tindak lanjut sesuai pola yang ditemukan, catatan, dan kolom tanda tangan.
  - *Lampiran*: daftar temuan lainnya (34 baris per halaman).
- **Export laporan (Excel)**: dari menu Ringkasan atau Temuan, unduh rekap temuan dalam satu file `.xlsx` berisi 4 sheet: *Ringkasan* (indikator utama, temuan per pola, status tindak lanjut, vendor berisiko), *Daftar Temuan* (seluruh transaksi berpola beserta skor, pola, alasan, dan status), *Benford*, dan *Parameter* mesin deteksi yang dipakai.
- **Benford's Law**: uji digit pertama dengan MAD (ambang Nigrini) dan z-stat per digit.
- **Relasi & Perilaku**: matriks pembuat × vendor, profil penyetuju (SoD), vendor favorit, aktivitas di luar jam kerja.
- **Sumber Data**: unggah file Excel (`.xlsx`/`.xls`) atau CSV, atau tempel teks CSV. Data diproses di browser dan tidak dikirim ke server.
- **Mesin Deteksi**: parameter dan bobot 9 aturan dapat diubah.

| Kelompok | Aturan |
|---|---|
| Transaction Pattern | Duplicate payment, split transaction, mendekati batas otorisasi, round amount, weekend transaction |
| Behavioral Pattern | Di luar jam kerja, pembuat = penyetuju, vendor favorit |
| Anomaly Pattern | Outlier nominal (robust z-score per kategori) |

## Cara membuka

- **Di komputer:** klik dua kali `index.html`, atau seret ke jendela browser (Chrome, Edge, Firefox). Tidak perlu instalasi atau server. Koneksi internet dipakai untuk memuat font Montserrat, pembaca Excel (SheetJS), dan pembuat PDF (html2canvas + jsPDF). Tanpa internet, aplikasi tetap jalan dengan file CSV. Saat dibuka sebagai file lokal, tombol **Unduh PDF** bisa diblokir browser; gunakan **Cetak → Save as PDF**, atau buka lewat situs web.
- **Lewat web:** aktifkan GitHub Pages (Settings → Pages → Deploy from a branch → `main` / root). Aplikasi lalu bisa dibuka di `https://ndestudio.github.io/INSIGHT/`.

## Format data (Excel atau CSV)

Kolom wajib: `tanggal`, `vendor`, `jumlah`. Opsional: `id`, `jam`, `pembuat`, `penyetuju`, `kategori`, `keterangan`.

- **Excel:** data dibaca dari sheet bernama *Transaksi* (atau sheet pertama). Kolom tanggal dan jam boleh berformat tanggal/jam Excel biasa.
- **CSV:** pemisah kolom boleh koma, titik koma, atau tab. Tanggal `YYYY-MM-DD` atau `DD/MM/YYYY`. Nominal boleh `49000000` atau `49.000.000`.

### Template siap pakai

- [`template-transaksi.xlsx`](template-transaksi.xlsx) **(disarankan)**: sheet *Transaksi* berisi 31 transaksi contoh (fiktif) dengan format tanggal, jam, dan Rupiah, plus sheet *Petunjuk* yang menjelaskan setiap kolom. Tombol **Unduh template Excel** di menu Sumber Data menghasilkan file yang sama.
- [`template-transaksi.csv`](template-transaksi.csv): isi yang sama dalam CSV (pemisah titik koma, UTF-8).

Kedua template memicu kesembilan aturan deteksi:

| Baris contoh | Pola yang diperagakan |
|---|---|
| TRX-003, TRX-004 | Duplicate payment + split transaction + mendekati batas (2 × Rp49 jt, hari sama) |
| TRX-008 s.d. TRX-010 | Split transaction (3 transaksi perbaikan AC di hari sama, total Rp66,6 jt) |
| TRX-012/013, TRX-022/023 | Duplicate payment (vendor & nominal sama dalam 1–2 hari) |
| TRX-011 | Weekend + di luar jam kerja + pembuat = penyetuju |
| TRX-017 | Pembuat = penyetuju |
| TRX-014, TRX-027 | Di luar jam kerja (02:15, 23:05) |
| TRX-021 | Weekend + mendekati batas otorisasi |
| TRX-006, TRX-016 | Round amount (Rp15 jt, Rp150 jt) |
| TRX-019, TRX-029 | Outlier nominal dibanding kategorinya |
| TRX-002, 020, 021, 024, 025 | Vendor favorit (semua transaksi DEWI ke PT Data Prima) |
| Baris lainnya | Transaksi normal sebagai pembanding |

Cara pakai: buka `template-transaksi.xlsx`, hapus baris contoh di sheet *Transaksi*, isi data Anda dengan kolom yang sama, simpan, lalu klik **Unggah Excel / CSV** di menu Sumber Data.

## Struktur file

| File | Isi |
|---|---|
| `index.html` | Seluruh aplikasi (HTML, CSS, JavaScript) |
| `template-transaksi.xlsx` | Template unggah Excel (sheet Transaksi + Petunjuk) |
| `template-transaksi.csv` | Template unggah CSV berisi 31 transaksi contoh |
| `ias-logo-white.png` | Logo IAS versi putih |
| `hero-globe.jpg` | Gambar globe untuk header |

Tampilan mengikuti Brand Guideline IAS: font Montserrat, Navy `#1D2250`, Tosca `#04B0C0`, abu-abu `#D1D3D4`.

> Data bawaan aplikasi adalah data contoh fiktif. Pola abnormal adalah indikasi untuk investigasi, bukan bukti fraud.
