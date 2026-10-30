# B3. Tiga Struktur Arsitektur

Dokumen ini mendeskripsikan tiga struktur arsitektur utama perangkat lunak (berdasarkan pandangan *Bass, Clements, & Kazman*) untuk subsistem **Academic Management**, dengan fokus pada layanan inti **Pembayaran Biaya Pendidikan** (informasi tagihan, pembayaran, konfirmasi, dan riwayat pembayaran). Ketiga struktur tersebut meliputi **Struktur Modul** (*Module Structure*), **Struktur Komponen dan Konektor** (*C&C Structure*), dan **Struktur Alokasi** (*Allocation Structure*).

---

## 1. Struktur Modul (*Module Structure*)
Struktur modul menunjukkan bagaimana sistem didekomposisi ke dalam unit-unit kode terenkapsulasi (modul, kelas, atau *package*) berdasarkan prinsip tanggung jawab tunggal (*Single Responsibility Principle*).

Pada layanan pembayaran biaya pendidikan, dekomposisi modul dibagi menjadi:
* **Modul Informasi Tagihan (`BillingModule`):** 
  * Menangani logika bisnis penarikan data tagihan mahasiswa per semester dari basis data akademik.
  * Menghitung rincian komponen biaya (SPP/UKT, SKS, denda, atau potongan beasiswa).
* **Modul Eksekusi Pembayaran (`PaymentExecutionModule`):** 
  * Mengelola pembuatan kode bayar atau nomor *Virtual Account* berkoordinasi dengan layanan perbankan/eksternal.
  * Menerapkan fungsi *payment lock* untuk mencegah transaksi ganda.
* **Modul Konfirmasi & Verifikasi (`ConfirmationModule`):** 
  * Menangani *webhook listener* otomatis dari *Payment Gateway* serta alur unggah bukti transfer manual beserta modul validasi bagi staf keuangan (BAUK).
* **Modul Riwayat & Laporan (`HistoryReportingModule`):** 
  * Bertanggung jawab mencatat log transaksi ke buku besar (*immutable ledger*) dan menghasilkan dokumen bukti lunas (*e-Receipt* PDF).

---

## 2. Struktur Komponen dan Konektor / C&C (*Component-and-Connector Structure*)
Struktur C&C mendeskripsikan elemen-elemen saat sistem berjalan (*runtime elements*) seperti komponen, layanan (*services*), antrean, basis data, serta interaksi antar-elemen melalui konektor komunikasi.

* **Alur Komponen & Konektor Sistem Pembayaran:**
  1. **Client Layer (Portal Mahasiswa / Admin BAUK):** Mengirimkan permintaan HTTP/REST melalui protokol HTTPS ke *API Gateway*.
  2. **API Gateway / Routing Layer:** Berperan sebagai pintu masuk tunggal yang melakukan verifikasi token autentikasi (JWT) dan meneruskan permintaan ke layanan finansial internal.
  3. **Application Services (Backend Core):** 
     * Kumpulan layanan modular yang memproses informasi tagihan, eksekusi bayar, dan pencatatan riwayat.
     * Menggunakan **Message Queue** (seperti RabbitMQ) secara asinkron untuk mengirimkan notifikasi (email/WhatsApp) dan mencetak kwitansi agar tidak membebani proses transaksi utama.
  4. **Eksternal Payment Gateway / Bank API:** Pihak ketiga tempat mahasiswa melakukan pembayaran secara langsung (*Virtual Account*, QRIS, e-Wallet). Mengirimkan notifikasi balik (*Webhook Callback*) ke server kampus setelah pembayaran sukses.
  5. **Persistence Layer (Database & Storage):** 
     * Relational Database (PostgreSQL/MySQL) yang dikelola dengan transaksi **ACID** untuk tabel tagihan dan transaksi pembayaran.
     * Object Storage (AWS S3 / MinIO) untuk menyimpan berkas bukti transfer manual dan file PDF kwitansi.

---

## 3. Struktur Alokasi (*Allocation Structure*)
Struktur alokasi memetakan elemen perangkat lunak (modul/komponen) ke lingkungan fisik atau infrastruktur tempat sistem tersebut dijalankan dan dikelola.

* **Pemetaan Perangkat Lunak ke Infrastruktur:**
  * **Application Containerization (Docker):** Seluruh modul layanan *backend* (Informasi Tagihan, Pembayaran, Konfirmasi, Riwayat) dikemas ke dalam kontainer Docker terisolasi agar mudah diskala (*scaling*) saat terjadi lonjakan trafik masa pembayaran awal semester.
  * **Database Deployment:** Basis data relasional ditempatkan pada server basis data terdedikasi dengan konfigurasi *Primary-Replica* untuk memisahkan beban baca (*read-heavy* pada riwayat/tagihan) dan tulis (*write-heavy* saat transaksi pembayaran).
  * **Storage Allocation:** Berkas bukti transfer dan *e-Receipt* dialokasikan ke layanan *Cloud Object Storage* atau direktori *Network Attached Storage* (NAS) kampus yang aman.
  * **Web Server & Load Balancer:** Menggunakan Nginx sebagai *Reverse Proxy* dan *Load Balancer* di depan kontainer aplikasi untuk mendistribusikan ribuan akses konkuren dari mahasiswa secara merata ke beberapa instance server *backend*.
