# BAB IV. Penutup

<!-- Kesimpulan dan saran. Penanggung jawab: Aidil Luthfi -->

## 4.1 Kesimpulan

Berdasarkan perancangan dan analisis arsitektur perangkat lunak pada sistem pembayaran biaya pendidikan Fakultas Ilmu Komputer Universitas Muslim Indonesia (FIKOM UMI), dapat ditarik beberapa kesimpulan utama sebagai berikut:

1. **Penerapan Gaya Arsitektur *Modular Monolith*:**
   Penggunaan gaya arsitektur *Modular Monolith* terbukti sangat efektif untuk mengorganisasi basis kode ke dalam batasan domain (*bounded contexts*) yang terisolasi dengan jelas—meliputi Modul Informasi Tagihan, Modul Pembayaran BNI, Modul Konfirmasi & Webhook, Modul Riwayat & Rekonsiliasi, serta Modul Integrasi SIAKAD—sehingga mempermudah pemeliharaan sistem tanpa kompleksitas infrastruktur terdistribusi.

2. **Integrasi Eksklusif *BNI Virtual Account*:**
   Penyediaan kanal pembayaran tunggal secara *Host-to-Host* (H2H) menggunakan *Virtual Account* BNI memberikan kepastian identifikasi transaksi mahasiswa secara instan, menyederhanakan proses rekonsiliasi keuangan fakultas, serta memperkuat keamanan pencatatan finansial institusi.

3. **Pengelolaan Kebijakan Operasional (*1-Day Batch Synchronization*):**
   Implementasi kebijakan jeda waktu sinkronisasi harian (*1-day batch synchronization*) memastikan stabilitas serta meredam lonjakan beban konkurensi pada basis data SIAKAD, sehingga validasi finansial tuntas sepenuhnya sebelum hak akses akademik untuk "belanja mata kuliah" (pengisian KRS) dibuka bagi mahasiswa.

4. **Kelengkapan Dokumen & Struktur Desain:**
   Perancangan sistem telah didukung oleh dokumentasi Keputusan Arsitektur (*Architectural Decision Records* / ADR), Analisis *Trade-off*, serta tiga struktur desain utama (Struktur Modul, Struktur Komponen & Konektor, dan Struktur Alokasi) yang telah divalidasi secara komprehensif.

## 4.2 Saran

Berdasarkan pengembangan sistem pembayaran biaya pendidikan FIKOM UMI yang telah dilakukan, berikut adalah beberapa saran untuk pengembangan dan optimalisasi di masa mendatang:

1. **Automasi Pemantauan Jaringan H2H:**
   Disarankan untuk menambahkan sistem pemantauan otomatis (*automated health check* dan *alerting*) pada jalur koneksi *Host-to-Host* BNI guna mendeteksi potensi gangguan atau kelambatan respons *callback* secara dini.

2. **Peningkatan Kapasitas Antrean Asinkron:**
   Seiring dengan bertambahnya jumlah mahasiswa dan volume transaksi musiman, implementasi *Async Message Queue* dapat terus dioptimalkan menggunakan klaster antrean yang lebih skalabel guna menjamin keandalan pengiriman *webhook* tanpa risiko *timeout*.

3. **Perluasan Panduan Integrasi Mandiri:**
   Mengingat penggunaan kanal pembayaran eksklusif BNI, penyediaan dokumentasi dan panduan transfer antarbank yang interaktif bagi mahasiswa pengguna bank lain dapat ditingkatkan untuk meminimalkan kendala atau kesalahan transfer.
