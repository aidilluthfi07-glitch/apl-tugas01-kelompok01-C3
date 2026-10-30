# B3. Tiga Struktur Desain

<!-- Penanggung jawab: Ahmad Zaky Fayyad -->

Bagian ini mendokumentasikan tiga struktur utama arsitektur perangkat lunak untuk subsistem **Academic Management**, khususnya pada layanan inti **Pembayaran Biaya Pendidikan** (informasi tagihan, pembayaran, konfirmasi, dan riwayat pembayaran).

---

## 1. Struktur Modul (*Module Structure*)
Struktur modul mendokumentasikan dekomposisi unit kode perangkat lunak dan hierarki relasi antar komponen (*uses*, *is-part-of*):
* **Billing Module (`/modules/billing`):** Menarik data SKS/status akademik dan menghitung besaran tagihan (UKT, SKS, denda).
* **Payment Module (`/modules/payment`):** Mengelola eksekusi pembayaran otomatis via *Payment Gateway*/*Virtual Account* atau transaksi manual.
* **Confirmation Module (`/modules/confirmation`):** Menangani jalur verifikasi *webhook callback* atau validasi manual bukti transfer oleh staf keuangan.
* **History & Ledger Module (`/modules/history`):** Mencatat log audit transaksi (*immutable log*) dan pembuatan kwitansi PDF (*e-Receipt*).

---

## 2. Struktur Komponen dan Konduktor (*Component-and-Connector / C&C Structure*)
Struktur C&C menggambarkan aspek *runtime* dan komunikasi antar layanan saat sistem berjalan (referensi visual dapat disertakan dari folder `diagram/`):
* **Client Layer:** Portal Web/Mobile bagi Mahasiswa dan Admin BAUK.
* **API Gateway & Auth Service:** Pintu gerbang autentikasi (JWT) dan perutean permintaan ke layanan terkait.
* **Financial Services:** Kumpulan layanan bisnis inti (Billing, Payment, Confirmation, History).
* **External Systems:** Integrasi dengan *Payment Gateway* / Bank (VA / QRIS).
* **Persistence Layer:** Basis data relasional (PostgreSQL) berstandar ACID dan *Object Storage* untuk berkas bukti bayar/kwitansi.

---

## 3. Struktur Alokasi (*Allocation Structure*)
Struktur alokasi memetakan elemen perangkat lunak ke dalam infrastruktur fisik, lingkungan server, dan alokasi tim:
* **Deployment Mapping:** 
  * Aplikasi backend dan API Gateway dijalankan dalam kontainer Docker pada *Cloud/VPS Server* kampus.
  * Basis data ditempatkan pada kluster terisolasi dengan sistem *automated backup*.
  * Berkas bukti transfer disimpan pada *Cloud Object Storage* yang aman.
* **Team Allocation:** Pembagian tugas pengembangan modul didasarkan pada tanggung jawab fungsional (backend, frontend, database, dan integrasi sistem).
