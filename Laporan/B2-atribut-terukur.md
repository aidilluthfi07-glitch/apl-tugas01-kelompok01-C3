# B2. Atribut Kualitas yang Terukur

<!-- Penanggung jawab: [Andi Muhammad Fadel] -->
|  Karakteristik  |  Angka target  |	Kondisi pengukuran  |  Cara mengukur  |
|---|---|---|
|  Security  |	100% skenario pengujian akses tanpa izin ditolak  |	Pengujian menggunakan akun tanpa login, akun mahasiswa lain, dan akun dengan hak akses terbatas  |  Melakukan pengujian autentikasi dan otorisasi, kemudian menghitung persentase akses ilegal yang berhasil ditolak  |  Integrity  |  100% transaksi uji konsisten dan 0 transaksi pembayaran ganda  |  Pengujian transaksi berhasil, gagal, dan pengiriman permintaan pembayaran berulang  |	  Membandingkan data tagihan, status pembayaran, dan riwayat transaksi serta memeriksa duplikasi ID transaksi  |  Availability	  |  Minimal 99,5% per bulan  |Pemantauan layanan selama periode operasional yang telah disepakati  |  Mencatat waktu layanan tersedia dan waktu gangguan, kemudian menghitung persentase ketersediaan  |