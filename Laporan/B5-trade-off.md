# B5. Analisis Trade-off

Penanggung jawab: Aidil Luthfi

Berikut adalah analisis *trade-off* dari sistem pembayaran biaya pendidikan FIKOM UMI yang mencakup minimal dua pasang karakteristik yang saling memengaruhi beserta penjelasan dan keputusan yang diambil:

---

### 1. Eksklusivitas Mitra Tunggal (BNI Virtual Account) vs. Keterbatasan Pilihan Kanal bagi Mahasiswa
* **Karakteristik yang Berinteraksi:** 
  * Penyediaan kanal pembayaran eksklusif melalui satu mitra perbankan saja, yaitu *Virtual Account* BNI.
  * Keterbatasan fleksibilitas bagi mahasiswa yang tidak memiliki rekening BNI dalam melakukan transaksi pembayaran langsung tanpa biaya tambahan (*interbank fee*).
* **Penjelasan Trade-off:** 
  Menggunakan satu mitra eksklusif (BNI) menyederhanakan pengelolaan arsitektur integrasi, mempercepat proses rekonsiliasi keuangan fakultas, dan memperkuat kepastian perjanjian kerja sama institusi. Namun, *trade-off*-nya adalah mahasiswa yang menggunakan bank lain harus melakukan transfer antarbank yang berpotensi menimbulkan biaya tambahan atau jeda waktu kliring jika tidak menggunakan jaringan ATM Bersama/Prima yang terhubung ke VA BNI.
* **Keputusan yang Diambil:** 
  Memilih integrasi *Host-to-Host* (H2H) secara mendalam dengan BNI untuk menjamin stabilitas dan keandalan transaksi utama, serta menyediakan dokumentasi panduan transfer antarbank yang jelas bagi mahasiswa pengguna bank selain BNI.

---

### 2. Sinkronisasi Data Harian (*Batch Synchronization*) vs. Kecepatan Akses "Belanja Mata Kuliah" Mahasiswa
* **Karakteristik yang Berinteraksi:** 
  * Kebijakan sistem yang mensyaratkan mahasiswa menunggu waktu satu hari (*1-day waiting period*) setelah status pembayaran lunas agar data tervalidasi penuh sebelum dapat mulai "belanja mata kuliah" (pengisian KRS).
  * Tuntutan sivitas akademika terhadap pembaruan status dan akses akademik secara instan (*real-time*) pascapembayaran.
* **Penjelasan Trade-off:** 
  Menerapkan sinkronisasi berbasis kelompok harian (*batch processing*) mengurangi beban konkurensi pada basis data SIAKAD secara drastis dan memastikan konsistensi rekonsiliasi finansial harian tertib sebelum hak akses akademik dibuka. Namun, *trade-off*-nya adalah adanya jeda waktu tunggu bagi mahasiswa (1 hari setelah pembayaran lunas) sebelum mereka bisa mengakses menu pemilihan mata kuliah, yang berpotensi menimbulkan keluhan jika mahasiswa melakukan pembayaran mendadak di hari pendaftaran.
* **Keputusan yang Diambil:** 
  Mempertahankan mekanisme sinkronisasi harian demi menjaga stabilitas infrastruktur dan keamanan basis data akademik, serta mengimbanginya dengan sosialisasi jadwal pembayaran dan tenggat waktu (*deadline*) yang jelas kepada mahasiswa FIKOM UMI agar mereka melakukan pelunasan jauh hari sebelum jadwal pengisian KRS dibuka.
