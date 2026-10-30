# B.3 Tiga Struktur Arsitektur (FIKOM UMI)

Dokumen ini mendeskripsikan arsitektur subsistem *Academic Management* untuk layanan pembayaran biaya pendidikan (UKT/SPP) FIKOM UMI berdasarkan tiga struktur utama: Struktur Modul, Struktur Komponen-dan-Konektor (C&C), dan Struktur Alokasi.

---

## 1. Struktur Modul (Module Structure)

Struktur modul merepresentasikan pembagian unit kode sumber secara statis berdasarkan pemisahan tanggung jawab (*separation of concerns*).

![Struktur Modul](../diagram/b3-1-struktur-modul.png)

### Komponen Modul:
1. **Modul Informasi Tagihan (Billing & Komponen Biaya UKT/SPP):** Mengelola rincian komponen biaya pendidikan mahasiswa dan pembuatan draft tagihan.
2. **Modul Pembayaran BNI (Integrasi BNI Virtual Account Host-to-Host):** Menangani pembuatan nomor Virtual Account (VA) BNI dan komunikasi awal transaksi.
3. **Modul Konfirmasi & Webhook (Listener Callback & Validasi Signature):** Menerima notifikasi *real-time* (callback) dari BNI, melakukan validasi keamanan *signature*, serta memastikan keabsahan transaksi.
4. **Modul Riwayat & Rekonsiliasi (Audit Trail & Kuitansi Digital):** Mencatat jejak audit seluruh transaksi, pencocokan data keuangan, serta penerbitan kuitansi digital.
5. **Modul Integrasi SIAKAD (Sinkronisasi Status & Belanja Mata Kuliah):** Memperbarui status pembayaran mahasiswa di SIAKAD secara otomatis agar mahasiswa dapat melakukan pengisian KRS/belanja mata kuliah.

### Garis Ketergantungan (Dependency Flow):
* `Modul Informasi Tagihan` $\rightarrow$ `Modul Pembayaran BNI` $\rightarrow$ `Modul Konfirmasi & Webhook` $\rightarrow$ `Modul Riwayat & Rekonsiliasi`.
* `Modul Konfirmasi & Webhook` juga memiliki ketergantungan langsung ke `Modul Integrasi SIAKAD` untuk *trigger* pembukaan akses KRS sesaat setelah pembayaran terkonfirmasi.

---

## 2. Struktur Komponen & Konektor (Runtime C&C Structure)

Struktur C&C mendeskripsikan elemen-elemen sistem yang berjalan pada saat *runtime* beserta pola interaksi dan konektor antarelemen.

![Struktur C&C](../diagram/b3-2-struktur-cnc.png)

### Elemen Runtime:
* **Klien Mahasiswa (Portal Web / Mobile App):** Antarmuka pengguna untuk melihat tagihan dan status pembayaran.
* **API Gateway / Router:** Gerbang tunggal penanganan lalu lintas permintaan (request routing, rate limiting, dan autentikasi).
* **Backend Modular Monolith (Logika Bisnis & Layanan VA BNI):** Layanan pemprosesan utama yang mengeksekusi logika bisnis pembayaran secara terpusat.
* **API BNI Payment Gateway (Sistem Eksternal Perbankan - Host-to-Host):** Layanan eksternal milik BNI untuk pemprosesan Virtual Account.
* **Antrean Pesan Asinkron / Outbox (Webhook Listener & Event Dispatcher):** Mekanisme pemrosesan *event* secara asinkron untuk menjamin keandalan penerimaan notifikasi *webhook* dari BNI tanpa mengganggu performa utama.
* **Database SIAKAD & Keuangan (PostgreSQL Terpusat):** Penyimpan data relasional utama untuk data akademis dan riwayat transaksi.

---

## 3. Struktur Alokasi (Allocation Structure)

Struktur alokasi menggambarkan pemetaan elemen perangkat lunak ke dalam lingkungan fisik/infrastruktur (*deployment*) serta alokasi tanggung jawab tim pengembang.

![Struktur Alokasi](../diagram/b3-3-struktur-alokasi.png)

### Pemetaan Lingkungan Deployment & Tim:
* **Server Aplikasi Cloud (Docker Container / Nginx + App):** Wadah eksekusi *Backend Modular Monolith* yang dikemas dalam Docker container dan dilayani oleh Nginx *reverse proxy*.
* **Klaster Basis Data Relasional (PostgreSQL Master-Replica):** Infrastruktur basis data terkonfigurasi *High Availability* (Master-Replica) untuk menjamin konsistensi data transaksi dan ketahanan sistem.
* **Server Host-to-Host BNI (Terowongan Aman VPN / TLS 1.3):** Infrastruktur komunikasi khusus menuju BNI yang diproteksi menggunakan enkripsi TLS 1.3 / jalur VPN terisolasi.
* **Tim Pengembang Internal FIKOM UMI (Backend, SIAKAD, & Tim QA):** Alokasi SDM internal yang bertanggung jawab atas pemeliharaan, *deployment*, dan pengujian pada *Server Aplikasi Cloud*.
