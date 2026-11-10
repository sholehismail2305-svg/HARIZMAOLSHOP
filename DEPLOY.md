# HARIZMA ONLINE: Panduan Supabase dan Penerapan

## 1. Menyiapkan proyek Supabase

1. Buat proyek Supabase. Siapkan URL proyek dan kunci publik (publishable key atau anon key lama).
2. Pada pengaturan Authentication, aktifkan masuk dengan email dan kata sandi. Isi Site URL dengan alamat HTTPS aplikasi setelah diterapkan. Tambahkan alamat itu dan `http://localhost:8000` ke daftar alamat pengalihan yang diizinkan.
3. Buka SQL Editor di Supabase, lalu jalankan isi file `supabase/schema.sql`.
4. Pastikan tabel `public.user_app_data` mengaktifkan RLS (aturan pembatasan akses per baris). Jangan menonaktifkannya dan jangan menambahkan aturan akses publik.
5. Isi URL proyek dan kunci publik Supabase pada `supabase-config.js`. Jangan pernah memasukkan `service_role` atau kunci rahasia ke kode aplikasi.

Kunci publishable/anon memang dapat dilihat pengunjung situs. Data dilindungi oleh Supabase Auth, aturan RLS yang membatasi data ke pemilik akun, dan fungsi `save_app_data` yang menolak perubahan versi lama.

## 2. Menerapkan aplikasi ke Netlify

1. Unggah folder ini sebagai folder utama situs, atau hubungkan repositorinya ke Netlify. `netlify.toml` menentukan folder publik dan `_headers` memasang header keamanan.
2. Aktifkan HTTPS. Gunakan alamat situs yang sama persis pada Site URL dan daftar alamat pengalihan Supabase.
3. Buka situs, buat akun, konfirmasikan email jika diminta, lalu masuk.
4. Jika peramban ini berisi data POS lama dan akun Supabase masih kosong, aplikasi akan bertanya apakah data lama ingin dipindahkan. Pemindahan hanya dilakukan setelah Anda menyetujuinya.
5. Pastikan status menunjukkan `Data tersimpan di server`. Tambahkan produk uji, muat ulang halaman, dan pastikan produk tetap ada. Buat akun kedua untuk memastikan akun itu tidak dapat melihat data akun pertama.

Pemasangan PWA (aplikasi web yang dapat dipasang) dan penyimpanan tampilan agar bisa dibuka tanpa jaringan memerlukan HTTPS. `localhost` boleh digunakan saat pengembangan. Pemindaian kamera memerlukan peramban yang mendukungnya dan izin kamera. Perubahan data memerlukan jaringan; perubahan yang belum terkirim disimpan sementara di perangkat dan dicoba lagi saat jaringan kembali. Jangan gunakan data usaha sebenarnya sebelum URL/kunci proyek diisi, skema SQL diterapkan, dan pemisahan data antar akun berhasil diuji.

## 3. Konflik perubahan dan cadangan

Setiap kumpulan data memiliki nomor versi di server. Jika perangkat lain mengubah kumpulan yang sama, server menolak kiriman versi lama agar perubahan terbaru tidak tertimpa diam-diam. Unduh cadangan sebelum memilih `Muat data server`; pilihan itu mengganti antrean lokal yang berkonflik dengan data versi server. Cadangan JSON berisi data usaha dan foto produk, jadi simpan di tempat aman.

Peramban menyimpan salinan sementara per akun agar data dapat dibuka dan perubahan dapat menunggu saat tidak ada jaringan. Keluar dari akun akan menghapus salinan akun tersebut dari perangkat. Salinan itu tidak dienkripsi untuk mencegah orang yang sudah bisa mengakses profil sistem operasi atau peramban yang sama.

## 4. Pemeriksaan sebelum digunakan

- Gunakan akun terpisah untuk setiap operator dan jangan berbagi kata sandi.
- Data dipisahkan berdasarkan akun Supabase. Fitur berbagi data toko dengan peran staf berbeda belum tersedia.
- Uji pendaftaran, konfirmasi email, masuk, keluar, muat ulang, dan pemisahan data pada alamat situs yang sudah diterapkan.
- Uji konflik perubahan dari dua perangkat dan alur cadangan/pemulihan sebelum memasukkan data sebenarnya.
- Atur serta uji cadangan Supabase. Ekspor cadangan JSON aplikasi secara berkala sebagai jalur pemulihan tambahan.
- Simpan rahasia proyek Supabase dan kata sandi basis data di luar `supabase-config.js` dan kode peramban.
