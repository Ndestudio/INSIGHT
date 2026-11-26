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

```csv
id,tanggal,jam,vendor,jumlah,pembuat,penyetuju,kategori,keterangan
TRX001,2026-11-03,10:12,PT Sinar Abadi,49000000,BUDI,HADI,Pengadaan,Pembelian material
```

## Struktur file

| File | Isi |
|---|---|
| `index.html` | Seluruh aplikasi (HTML, CSS, JavaScript) |
| `ias-logo-white.png` | Logo IAS versi putih |
| `hero-globe.jpg` | Gambar globe untuk header |

Tampilan mengikuti Brand Guideline IAS: font Montserrat, Navy `#1D2250`, Tosca `#04B0C0`, abu-abu `#D1D3D4`.

> Data bawaan aplikasi adalah data contoh fiktif. Pola abnormal adalah indikasi untuk investigasi, bukan bukti fraud.
