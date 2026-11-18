# E-Commerce Analysis

Pembersihan dan analisis data transaksi toko online (UCI Online Retail) dengan Pandas untuk menghitung pendapatan per negara. Proyek ini merupakan bagian dari latihan Data Analyst di [roadmap.sh](https://roadmap.sh/projects/ecommerce-data-analysis).

## Ringkasan

Data transaksi nyata memuat baris yang tidak masuk akal: retur dengan jumlah negatif, barang gratis, penyesuaian stok, dan data kosong. Proyek ini membersihkannya dengan keputusan yang disengaja untuk setiap masalah, membuat kolom `Revenue` (`Quantity × UnitPrice`), lalu mencari 10 negara dengan pendapatan terbesar.

## Dataset

| Aspek | Keterangan |
|---|---|
| Sumber | [UCI Online Retail](https://archive.ics.uci.edu/dataset/352/online+retail) |
| Isi | Transaksi toko online berbasis di Inggris, 1 Desember 2010 sampai 9 Desember 2011 |
| Ukuran penuh | 541.909 baris, 8 kolom |
| Kolom | InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, Country |
| Sampel yang dipakai | 10% acak (54.191 baris, `random_state=42`) |
| Sisa setelah pembersihan | 52.995 baris |

## Teknologi

| Alat | Kegunaan |
|---|---|
| Python | Bahasa pemrograman |
| Pandas | Pembersihan dan perhitungan |
| Matplotlib dan Seaborn | Visualisasi |
| Google Colab | Lingkungan kerja |

## Alur Pembersihan

| Tahap | Yang dilakukan |
|---|---|
| 1 | Memuat data dan mengambil sampel 10% |
| 2 | Inspeksi dengan `head`, `info`, dan jumlah data kosong |
| 3 | Mengukur baris retur, barang gratis, dan penyesuaian stok |
| 4 | Memeriksa isi 129 baris berjumlah negatif tanpa awalan `C` |
| 5 | Menyaring baris bermasalah |
| 6 | Memastikan `InvoiceDate` bertipe datetime dan mengubah `CustomerID` ke `Int64` |
| 7 | Membuat kolom `Revenue` |
| 8 | Menghitung dan memvisualisasikan 10 negara teratas |
| 9 | Menguji apakah keputusan tentang `CustomerID` memengaruhi hasil |

## Keputusan Pembersihan

| Masalah | Jumlah (baris) | Keputusan | Alasan |
|---|---|---|---|
| Retur (faktur berawalan C) | 945 | Dibuang | Retur menghasilkan Revenue negatif dan bukan penjualan |
| Quantity negatif tanpa awalan C | 129 | Dibuang | Seluruhnya berharga 0 dan tanpa CustomerID, dengan keterangan seperti "Damaged" dan "check", sehingga merupakan penyesuaian stok |
| UnitPrice 0 dengan Quantity positif | 122 | Dibuang | Barang gratis tidak menghasilkan pendapatan |
| Description kosong | 149 | Terbuang bersama barang gratis | Seluruhnya berharga 0 |
| CustomerID kosong | 13.640 (sisa 13.360) | Dipertahankan | Pertanyaan hanya tentang pendapatan per negara, dan membuang baris ini menghilangkan sekitar seperempat data |
| InvoiceDate | - | Dipastikan datetime | Sudah datetime sejak dibaca dari Excel |
| CustomerID bertipe desimal | - | Diubah ke Int64 | Nilai kosong membuat kolom terbaca sebagai desimal |

Total 1.196 baris (sekitar 2,2% sampel) dibuang.

## Hasil: 10 Negara dengan Pendapatan Terbesar

Total pendapatan pada sampel adalah 1.096.775,81.

| Peringkat | Negara | Pendapatan | Porsi dari total | Baris transaksi |
|---|---|---|---|---|
| 1 | United Kingdom | 941.619,99 | 85,85% | 48.465 |
| 2 | Netherlands | 27.435,83 | 2,50% | 233 |
| 3 | EIRE (Irlandia) | 26.066,00 | 2,38% | 804 |
| 4 | France | 23.645,33 | 2,16% | 835 |
| 5 | Germany | 22.389,51 | 2,04% | 935 |
| 6 | Australia | 12.429,99 | 1,13% | 117 |
| 7 | Spain | 5.600,90 | 0,51% | 256 |
| 8 | Switzerland | 5.483,89 | 0,50% | 189 |
| 9 | Hong Kong | 3.597,85 | 0,33% | - |
| 10 | Belgium | 3.593,51 | 0,33% | 190 |

Tanda "-" berarti negara tersebut tidak termasuk 10 negara dengan baris terbanyak. Angka pendapatan hanya sekitar sepersepuluh dari data penuh karena memakai sampel 10%.

## Temuan Penting

- United Kingdom mendominasi: sekitar 85,85% dari total pendapatan dan sekitar 91,5% dari seluruh baris transaksi.
- Netherlands dan Australia memperoleh pendapatan tinggi dari transaksi yang sangat sedikit (233 dan 117 baris), dengan pendapatan per baris sekitar 118 dan 106, dibanding sekitar 19 untuk United Kingdom. Peringkat keduanya bergantung pada sedikit transaksi bernilai besar.
- **Uji keputusan `CustomerID`:** jika baris tanpa `CustomerID` dibuang, peringkat delapan negara teratas tidak berubah, tetapi pendapatan United Kingdom turun 18,4%, EIRE turun 6,6%, dan Hong Kong hilang karena seluruh transaksinya tidak memiliki `CustomerID`. Karena itu baris tersebut dipertahankan.
- Sembilan negara selain United Kingdom sulit dibandingkan pada satu grafik karena skalanya jauh berbeda, sehingga dibuat grafik terpisah tanpa United Kingdom.

## Keterbatasan

- Analisis memakai sampel acak 10%, sehingga jumlah nominal hanya perkiraan dan peringkat di bagian bawah daftar (Spain sampai Belgium) serta selisih Netherlands dengan EIRE dapat berubah pada sampel lain.
- Pendapatan dihitung dari harga dan jumlah pada data, tanpa memperhitungkan retur yang nilainya sudah dibuang dari analisis ini.
- Negara adalah negara pelanggan pada kolom `Country`, dan baris tanpa `CustomerID` tetap dihitung.

## Struktur Repositori

```
E-Commerce-Analysis/
├── E-Commerce-Analysis.ipynb
└── README.md
```

## Cara Menjalankan

| Langkah | Yang dilakukan |
|---|---|
| 1 | Instal library: `pip install pandas matplotlib seaborn openpyxl jupyter` |
| 2 | Buka notebook dengan `jupyter notebook` |
| 3 | Jalankan semua sel dari atas ke bawah |

Sel pertama mengunduh `Online Retail.xlsx` (sekitar 22,6 MB) langsung dari UCI, jadi koneksi internet diperlukan dan prosesnya bisa memakan waktu satu sampai dua menit. Jika tautan unduhan berubah, unduh filenya secara manual dari halaman dataset UCI, lalu ganti `url` pada sel pertama dengan lokasi file di komputermu.

## Tautan

| Item | Tautan |
|---|---|
| Notebook di Colab | [isi tautan notebook Colab] |
| Halaman proyek | https://roadmap.sh/projects/ecommerce-data-analysis |

## Penulis

[Nama kamu] · [tautan GitHub atau LinkedIn]
