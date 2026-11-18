# Kasus: Sistem Pemesanan Transportasi Online

![](files/latihan-1-1.png)

Sebuah perusahaan transportasi online menyediakan aplikasi yang mempertemukan pelanggan dengan pengemudi. Pelanggan dapat mendaftar atau masuk ke aplikasi, menentukan lokasi penjemputan dan tujuan, memilih jenis layanan, lalu mengajukan pesanan perjalanan.

Sistem akan mencari pengemudi yang tersedia di sekitar lokasi penjemputan. Pengemudi dapat menerima atau menolak pesanan. Jika pesanan diterima, pelanggan dapat melihat informasi pengemudi dan melacak perjalanan. Setelah pengemudi tiba di lokasi tujuan, pelanggan melakukan pembayaran melalui uang tunai atau dompet digital dan dapat memberikan penilaian kepada pengemudi.

Pesanan dapat berstatus: menunggu pengemudi, diterima, pengemudi menuju lokasi penjemputan, perjalanan berlangsung, selesai, atau dibatalkan. Pelanggan dapat membatalkan pesanan sebelum perjalanan dimulai. Pengemudi juga dapat membatalkan pesanan apabila terjadi kendala sebelum menjemput pelanggan.

## Instruksi Soal
1. Buat _use case diagram_ untuk sistem pemesanan transportasi online tersebut.
    - Tentukan aktor yang terlibat.
    - Identifikasi use case utama dari sisi pelanggan, pengemudi, dan sistem pembayaran.
2. Buat _activity diagram_ untuk proses pemesanan perjalanan, mulai dari pelanggan menentukan lokasi hingga pembayaran dan perjalanan selesai.
3. Buat _state machine diagram_ untuk menggambarkan perubahan status sebuah pesanan perjalanan, dari pesanan dibuat sampai selesai atau dibatalkan.