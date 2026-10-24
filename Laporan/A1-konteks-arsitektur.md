# A1. Analisis Empat Konteks Arsitektur

<!-- Penanggung jawab: [Aidil Luthfi] -->

| Konteks | Kondisi subsistem | Pengaruh terhadap arsitektur |
|---|---|---|
| Teknis | Layanan pembayaran biaya pendidikan mencakup informasi tagihan, pembayaran, konfirmasi, dan riwayat pembayaran. Keberadaan teknologi, basis data, dan integrasi dengan layanan pembayaran perlu dipastikan melalui pengamatan. | Arsitektur perlu memperhatikan integritas data transaksi, keamanan informasi pembayaran, dan integrasi antarkomponen agar informasi tetap konsisten. |
| Siklus hidup proyek | Layanan pembayaran dapat mengalami perubahan kebutuhan, pemeliharaan, pengujian, dan pembaruan sistem. Proses pengembangan dan pemeliharaan aktual perlu dikonfirmasi kepada pihak terkait. | Arsitektur perlu mudah dipelihara, diuji, dan dikembangkan agar perubahan kebutuhan tidak mengganggu fungsi utama layanan. |
| Bisnis | Layanan mendukung mahasiswa dalam memperoleh informasi tagihan dan status pembayaran, serta mendukung administrasi dalam memeriksa transaksi. | Arsitektur perlu mendukung proses pembayaran yang efisien, informasi yang akurat, dan pemantauan status transaksi secara jelas. |
| Profesional | Layanan melibatkan mahasiswa, petugas administrasi, dan pihak lain yang berwenang. Setiap pihak memiliki tanggung jawab dan hak akses yang berbeda terhadap informasi pembayaran. | Arsitektur perlu menerapkan pembatasan akses berdasarkan peran, menjaga kerahasiaan data, serta memastikan tanggung jawab pengelolaan transaksi jelas. |

