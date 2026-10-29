# A5. Keputusan Arsitektur dan ADR

<!-- Penanggung jawab: Aidil Luthfi -->
<!-- Yang diminta: Minimal 2 ADR (Title, Status, Context, Decision, Consequences, Compliance). Ringkasan di sini, ADR lengkap di folder adr/. -->

### ADR-001: Implementasi Idempotency Key pada Webhook Pembayaran
- **Title:** Implementasi Idempotency Key pada Webhook Pembayaran
- **Status:** Diterima
- **Context:** Berdasarkan analisis pola trafik dan histori kegagalan komunikasi sistem perbankan/payment gateway di lapangan, latensi jaringan berisiko memicu pengiriman *webhook* ganda yang dapat menyebabkan duplikasi data pembayaran pada SIAKAD FIKOM UMI.
- **Decision:** Kami akan merekomendasikan penerapan *Idempotency Key Pattern* dan *Transactional Outbox Pattern* pada API *callback* konfirmasi pembayaran.
- **Consequences:** Memberikan kepastian akurasi data finansial dan menghilangkan duplikasi transaksi, dengan konsekuensi tambahan pengelolaan log audit transaksi.
- **Compliance:** Dipenuhi melalui tinjauan kode (*code review*), pengujian integrasi (*integration testing*), dan pemantauan log harian.

### ADR-002: Pemilihan Gaya Arsitektur Modular Monolith
- **Title:** Pemilihan Gaya Arsitektur Modular Monolith
- **Status:** Diterima
- **Context:** Menilai kapasitas tim pengembang internal dan kompleksitas pemeliharaan di FIKOM UMI, arsitektur *microservices* penuh pada tahap awal akan memicu kerumitan transaksi terdistribusi yang tidak efisien.
- **Decision:** Kami akan merekomendasikan pendekatan *Modular Monolith* dengan batasan modul yang tegas (*bounded contexts*) di dalam satu basis kode terpusat.
- **Consequences:** Mempercepat waktu pengembangan (*time-to-market*), mempermudah manajemen transaksi basis data, dan menekan biaya infrastruktur awal.
- **Compliance:** Dipenuhi melalui penerapan aturan struktur direktori, *static code analysis*, dan pengujian arsitektur berkala.
