# ADR-002: Pemilihan Gaya Arsitektur Modular Monolith

- **Status:** Diterima
- **Tanggal:** 29-10-2026
- **Pengambil keputusan:** Aidil Luthfi

## Context
Kebutuhan riil di lapangan pada layanan pembayaran FIKOM UMI menuntut sistem yang stabil dan mudah dikelola oleh tim internal universitas tanpa kompleksitas infrastruktur kontainer terdistribusi yang berlebihan pada tahap awal implementasi.

## Decision
Kami akan merekomendasikan penggunaan arsitektur *Modular Monolith*, di mana seluruh modul layanan (Informasi Tagihan, Pembayaran, Konfirmasi, Riwayat) berada dalam satu repositori dengan batasan domain logika (*bounded contexts*) yang tegas.

## Consequences
- **Positif:** Mempermudah pengelolaan transaksi basis data secara ACID, mempercepat *deployment*, dan menekan biaya infrastruktur server awal.
- **Negatif / Trade-off:** Memerlukan kedisiplinan tim untuk menjaga batasan antar modul agar tidak terjadi kopling ketat (*tight coupling*).

## Compliance
Cara memastikan keputusan ini dipatuhi: penerapan aturan *linting* struktur direktori, tinjauan kode (*code review*) yang ketat, serta pengujian arsitektur berbasis modul secara otomatis.
