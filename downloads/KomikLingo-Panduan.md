# TCD 0.5.0

Terjemahan Inggris → Indonesia untuk komik, situs, gambar/file, teks, dan percakapan game. **Google Translate online menjadi pilihan awal**, dengan ML Kit offline sebagai cadangan. Tidak membutuhkan komputer, server sendiri, akun atau kunci API. DuckDuckGo tetap dipakai untuk pencarian situs, bukan mesin terjemahan.

## Pasang dan pilih mesin

Pasang `TCD-Android.apk` sebagai pembaruan aplikasi sebelumnya. Android 8+ ARM64/ARMv7, identitas paket dan sertifikat tetap sama. APK masih bertanda tangan pengembangan dan belum tersedia di Play Store.

Google online membutuhkan internet dan mengirim **teks Inggris serta konteks dialog terdekat** ke Google melalui HTTPS. OCR dan gambar layar tetap di HP. Buka **Pengaturan → Mesin terjemahan · Google / Offline**, **Teks → Pilihan → Mesin terjemahan**, atau tombol Mesin terjemahan pada halaman Terjemahan layar. Pilihan berlaku untuk semua mode tersebut.

Pilih **Offline** agar teks diproses di HP. Model bahasa Inggris–Indonesia perlu diunduh sekali. Pada mode online, model cadangan disiapkan di latar tanpa menghalangi penggunaan Google; cadangan hanya dapat bekerja setelah unduhan selesai. Jika Google gagal atau membatasi permintaan, aplikasi mencoba offline dan menampilkan statusnya. Jika kedua mesin gagal, aplikasi menampilkan kegagalan, tidak mengarang hasil.

Google online memakai endpoint web publik Google Translate, **bukan API Google Cloud Translation berbayar dengan SLA**. Endpoint dapat berubah atau dibatasi. Koneksi memiliki batas waktu 2,5 detik dan pembacaan respons 3,5 detik. HTTP 429/pengalihan pembatasan diberi jeda minimal lima menit; header Retry-After dihormati. Hasil online yang lolos pemeriksaan disimpan sementara di RAM (128 permintaan). Kecepatan dan ketepatan tidak dijamin untuk setiap koneksi/dialog. Paragraf tunggal yang melebihi batas 4.600 karakter permintaan online memakai jalur cadangan.

## Game otomatis: dua kotak terpisah

1. Buka **Terjemahan layar → Mulai**, izinkan tampil di atas aplikasi lain dan tangkap layar Android, lalu buka game.
2. Ketuk balon TCD untuk menampilkan **kotak seleksi wilayah**. Geser tengah kotak untuk memindahkan; tarik sudut untuk mengubah ukuran; seret di luar kotak untuk menggambar wilayah baru. Sisakan ruang di atas atau bawah untuk hasil.
3. Pilih **Warna & latar** untuk mengatur kepekatan dan warna teks. Nilai **0%** menghilangkan latar sehingga hanya teks/bayangan terlihat. Pilihan warna: Putih, Kuning, Hijau, Biru muda, Hitam. Tekan Selesai untuk kembali ke seleksi.
4. Tekan **Mulai otomatis**. Bingkai seleksi hilang; hasil tampil pada **panel terjemahan terpisah**. Tekan Next di game seperti biasa. Panel hasil tidak menerima sentuhan dan tidak perlu ditutup.

Ketuk balon untuk jeda/lanjut; tahan untuk menu, memilih ulang area, tampilan hasil, atau menghentikan sesi. Menu menjeda pemantauan. Panel hasil berada di luar wilayah pindai agar terjemahan tidak ikut terbaca kembali. Opacity jendela dibatasi 0,75 untuk kompatibilitas sentuhan Android; slider 100 adalah latar terpekat yang tersedia, bukan jendela pemblokir game.

Mode **Game layar penuh · otomatis** di menu memindai layar di bawah pita subtitle atas tanpa seleksi. **Terjemahkan layar** mengambil satu cuplikan manual; dari hasilnya, tombol **Otomatis di game** membuka seleksi. Subtitle panjang dapat memakai elipsis; hasil lengkap tersedia lewat Lihat teks terakhir. Mengubah orientasi/ukuran layar mengakhiri mode otomatis, sehingga area perlu dipilih ulang. Sesi berhenti saat layar dikunci, izin dicabut, atau Hentikan pada notifikasi ditekan. Layar terlindungi tidak dilewati. Frame tidak disimpan ke file.

## Ketepatan teks dan percakapan

Pembacaan game menunggu dua hasil OCR yang sama agar animasi huruf tidak diterjemahkan satu per satu. Baris yang meragukan dibaca ulang dari piksel, maksimal tiga baris per dialog stabil. Tesseract lokal membandingkan hasil dengan OCR awal; nama yang sudah dilindungi, angka dan penyangkalan dijaga. Tidak ada penggantian kata berdasarkan tebakan kamus saja. Baris yang pecah menjadi beberapa blok diurutkan atas–bawah dan kiri–kanan.

Terjemahan memakai paragraf/dialog sebelum dan sesudah jika tersedia dan muat dalam batas pemrosesan. Hanya bagian sasaran ditampilkan. Jika mesin merusak pemisah paragraf, aplikasi mencoba penanda batas dan memeriksa urutannya; jika tetap gagal, bagian diterjemahkan terpisah. Nama mendapat percobaan ulang dalam kalimat utuh sebelum cadangan fragmen. Pemeriksaan angka dan pemotongan hasil mendeteksi sebagian kesalahan yang dapat diverifikasi, bukan semua kesalahan makna.

Daftar nama/istilah manual tersedia per sumber. Nama yang dikenali dilindungi sebelum diterjemahkan dan dipulihkan setelah pemeriksaan. Gaya Natural memakai aku–kamu, Netral saya–kamu, Formal saya–Anda. Aturan frasa terbatas membantu konteks seperti perintah tempur, ungkapan percakapan dan istilah militer, tetapi tidak memberikan pemahaman seluruh cerita. Tulisan dekoratif, sumber terpotong, nama ambigu dan OCR salah masih dapat menghasilkan kesalahan. **Tidak ada jaminan akurasi 100%.** Gunakan Lihat asli/Hasil untuk membandingkan.

## Teks, file dan pembaca situs

- **Teks:** ketik/tempel hingga 16.000 karakter per proses. Hasil dapat disalin. Gaya, nama dan mesin ada di Pilihan. Berbagi teks biasa ke TCD membuka halaman ini, sedangkan berbagi URL membuka situs.
- **Gambar & file:** pilih JPG, PNG, WebP, PDF tanpa kata sandi atau TXT UTF-8 melalui pemilih Android. File asli tidak diubah. EPUB/DOCX/CBZ dan PDF terenkripsi belum didukung.
- **Situs:** masukkan URL atau kata kunci. DuckDuckGo digunakan untuk pencarian. Login dan pembatasan situs/WebView tetap berlaku.

Ikon sudut layar menyembunyikan/menampilkan alat pembaca dan bisa digeser. Hasil tetap tersimpan dalam RAM saat menggulir dokumen aktif. Reset membaca ulang satu bagian tanpa menghapus hasil lain. Refresh, next/back, sumber baru dan penghentian proses menghapus cache hasil; riwayat URL/posisi disimpan terpisah. Mengubah gaya/nama/server/mesin membersihkan hasil terkait. Mode **Terjemahkan 1 halaman** memindai dokumen aktif tanpa membuka bab berikutnya, dengan batas 400 viewport atau 6.000 bagian. Halaman tanpa akhir, canvas, iframe atau scroll-container khusus dapat membatasi hasil.

Angka di bilah alamat membuka Tab (maksimal 16). Bookmark (maksimal 200) dan Riwayat (maksimal 30 halaman) disimpan lokal. Bookmark menyimpan alamat, bukan arsip halaman offline. Satu WebView memuat tab aktif untuk menghemat RAM. Tekan lama tautan untuk membuka tab baru atau menyimpan bookmark.

Penyaring domain iklan/pelacak dibundel, cookie pihak ketiga dibatasi dan popup skrip ditolak. Menu Perlindungan situs menyediakan pengecualian domain. Tidak semua iklan dapat dihilangkan. HTTPS, pemeriksaan sertifikat dan Safe Browsing provider Android tetap berlaku; situs tidak diberi kamera/mikrofon. TCD/DuckDuckGo bukan VPN dan tidak melewati CAPTCHA, blokir ISP, pembatasan akun/wilayah, atau layar terlindungi.

## Server AI opsional

Pembaca situs/file masih dapat memakai server milik pengguna jika alamatnya diisi. Server ini terpisah dari pilihan Google/Offline pada teks dan balon. Tidak diperlukan untuk pemakaian biasa.

1. Di komputer, pasang Ollama dan jalankan `ollama pull qwen2.5:7b`.
2. Jalankan `python3 server/reader_server.py` dan simpan kode akses yang ditampilkan.
3. Dengan USB debugging yang diizinkan, jalankan `adb reverse tcp:8765 tcp:8765`.
4. Di TCD → Pengaturan → AI opsional, masukkan `http://127.0.0.1:8765/translate` dan kode akses.
5. Kosongkan alamat untuk kembali ke mesin Google/Offline. Server jaringan lain wajib HTTPS dan kontrol akses yang disiapkan pemiliknya.

Server contoh mendengar localhost, memeriksa kode akses/ukuran/antrean, serta memisahkan konteks dokumen di RAM (kedaluwarsa 30 menit, maksimal 32 dokumen). Ini server pribadi, bukan layanan publik siap produksi.

## Build dan pemeriksaan

AGP 8.7.3, Gradle 8.9, JDK 17, compile/target SDK 35. Tentukan Android SDK saat membuka di Android Studio. `scripts/build.sh` khusus workspace cloud dan mempertahankan proxy/CA sesi. Kunci privat tidak masuk ZIP sumber.

`bash tests/run.sh` menjalankan uji aturan, konteks, nama, router, cache, pembatalan dan penjagaan hasil. `bash tests/google-transport.sh` menguji transport dengan respons HTTP simulasi pada JVM host, menggunakan dependensi org.json yang hash-nya diperiksa; APK memakai org.json Android. Tambahkan `--live` untuk mencoba endpoint Google sebenarnya, yang dapat gagal karena jaringan/pembatasan layanan. Pengujian Android memakai `-PcaptureTest`, sehingga fixture/x86 hanya masuk APK uji. Resource CA pengujian tidak masuk APK pengguna atau sumber.

Lihat `VALIDATION.md` untuk hasil dan keterbatasan pengujian terbaru, serta `PRIVACY.md` untuk aliran data. Artefak utama: `TCD-Android.apk` dan `TCD-Source.zip`; alias KomikLingo tetap disediakan untuk tautan lama.
