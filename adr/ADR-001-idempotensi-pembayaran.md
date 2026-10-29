# ADR-001: Implementasi Idempotency Key pada Webhook Pembayaran

- **Status:** Diterima
- **Tanggal:** 29-10-2026
- **Pengambil keputusan:** Aidil Luthfi

## Context
Dalam operasional nyata sistem pembayaran di Fakultas Ilmu Komputer UMI, kegagalan jaringan temporer sering kali memicu *payment gateway* atau bank mitra mengirim ulang (*retry*) data konfirmasi pembayaran ke server kampus. Tanpa pengaman, hal ini memicu duplikasi data pembayaran pada database SIAKAD.

## Decision
Kami akan merekomendasikan penerapan *Idempotency Key Pattern* dengan memanfaatkan nomor referensi transaksi unik sebagai kunci verifikasi, dipadukan dengan *Transactional Outbox Pattern* untuk sinkronisasi data akademik.

## Consequences
- **Positif:** Menghilangkan risiko duplikasi data pembayaran secara permanen dan mempermudah audit keuangan fakultas.
- **Negatif / Trade-off:** Memerlukan mekanisme pembersihan tabel log transaksi secara berkala.

## Compliance
Cara memastikan keputusan ini dipatuhi: peninjauan kode (*code review*) pada modul *callback handler*, pengujian integrasi dengan skenario pengiriman *payload* ganda, serta pemantauan log transaksi harian.
