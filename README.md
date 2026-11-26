# INSIGHT — Pattern Recognition in Internal Audit

Aplikasi web satu halaman untuk Internal Audit InJourney Aviation Services. Aplikasi ini menguji seluruh populasi transaksi terhadap aturan pola, memberi skor risiko 0–100, dan menyajikan temuannya untuk ditindaklanjuti.

## Fitur

- **Ringkasan**: angka utama, sebaran tingkat risiko, timeline harian, ilustrasi kasus teratas, dan vendor dengan skor tertinggi.
- **Temuan**: daftar transaksi berpola dengan filter, alasan per aturan, status tindak lanjut, dan salin CSV.
- **Benford's Law**: uji digit pertama dengan MAD (ambang Nigrini) dan z-stat per digit.
- **Relasi & Perilaku**: matriks pembuat × vendor, profil penyetuju (SoD), vendor favorit, aktivitas di luar jam kerja.
- **Sumber Data**: unggah atau tempel CSV. Data diproses di browser dan tidak dikirim ke server.
- **Mesin Deteksi**: parameter dan bobot 9 aturan dapat diubah.

| Kelompok | Aturan |
|---|---|
| Transaction Pattern | Duplicate payment, split transaction, mendekati batas otorisasi, round amount, weekend transaction |
| Behavioral Pattern | Di luar jam kerja, pembuat = penyetuju, vendor favorit |
| Anomaly Pattern | Outlier nominal (robust z-score per kategori) |

## Cara membuka

- **Di komputer:** klik dua kali `index.html`, atau seret ke jendela browser (Chrome, Edge, Firefox). Tidak perlu instalasi atau server. Koneksi internet hanya dipakai untuk memuat font Montserrat.
- **Lewat web:** aktifkan GitHub Pages (Settings → Pages → Deploy from a branch → `main` / root). Aplikasi lalu bisa dibuka di `https://ndestudio.github.io/INSIGHT/`.

## Format CSV

Kolom wajib: `tanggal`, `vendor`, `jumlah`. Opsional: `id`, `jam`, `pembuat`, `penyetuju`, `kategori`, `keterangan`.

Pemisah kolom boleh koma, titik koma, atau tab. Tanggal `YYYY-MM-DD` atau `DD/MM/YYYY`. Nominal boleh `49000000` atau `49.000.000`.

### Template siap pakai

[`template-transaksi.csv`](template-transaksi.csv) berisi 31 transaksi contoh (fiktif) yang memicu kesembilan aturan. File ini memakai pemisah titik koma dan UTF-8 sehingga langsung terbuka rapi di Excel berbahasa Indonesia. Tombol **Unduh template CSV** juga tersedia di menu Sumber Data aplikasi.

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

Cara pakai: buka template di Excel, hapus baris contoh, isi data Anda dengan kolom yang sama, lalu **Save As → CSV (Comma delimited)** atau CSV UTF-8, dan unggah di menu Sumber Data.

## Struktur file

| File | Isi |
|---|---|
| `index.html` | Seluruh aplikasi (HTML, CSS, JavaScript) |
| `template-transaksi.csv` | Template unggah CSV berisi 31 transaksi contoh |
| `ias-logo-white.png` | Logo IAS versi putih |
| `hero-globe.jpg` | Gambar globe untuk header |

Tampilan mengikuti Brand Guideline IAS: font Montserrat, Navy `#1D2250`, Tosca `#04B0C0`, abu-abu `#D1D3D4`.

> Data bawaan aplikasi adalah data contoh fiktif. Pola abnormal adalah indikasi untuk investigasi, bukan bukti fraud.
