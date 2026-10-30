# TCD 0.2.0

Penerjemah Inggris → Indonesia untuk teks, gambar, dokumen, situs dan layar aplikasi lain. Tampilan memakai komponen Android native, tanpa menambahkan iklan, animasi berat atau pustaka UI baru.

## Mulai

Pasang `TCD-Android.apk` pada Android 8+ ARM64/ARMv7 sebagai pembaruan aplikasi sebelumnya. Model Inggris–Indonesia perlu diunduh sekali; setelah siap, mode dasar memproses teks/file di HP. Membuka situs tetap membutuhkan internet.

Beranda menampilkan **Teks**, **Gambar & file**, kolom alamat/pencarian dan **Terjemahan layar**. **Lanjutkan** membuka sumber web terakhir; daftar lain ada di **Riwayat**.

- **Teks:** ketik/tempel teks, tekan Terjemahkan, lalu Salin. Teks langsung diteruskan ke penerjemah lokal tanpa OCR. Paragraf tetap diproses sebagai satu kesatuan; maksimal 16.000 karakter per proses, dan input yang melebihi batas ditolak tanpa dipotong diam-diam. Gaya dan istilah ada di menu Pilihan. Berbagi teks biasa ke TCD membuka masukan ini; berbagi URL membuka situs.
- **Gambar & file:** pilih JPG, PNG, WebP, PDF tanpa kata sandi atau TXT UTF-8 melalui pemilih Android. File sumber tidak ditulis ulang. EPUB/DOCX/CBZ dan PDF terenkripsi belum didukung.
- **Situs:** masukkan URL atau kata kunci. Pencarian memakai DuckDuckGo. Aturan login, pembatasan WebView dan akses situs tetap berlaku; aplikasi tidak melewatinya.
- **Terjemahan layar:** aktifkan balon dengan izin Android, buka aplikasi lain, lalu ketuk balon. Tutup hasil untuk kembali menggulir. Tahan balon untuk menu atau hentikan lewat notifikasi. Sesi berhenti saat layar dikunci. Layar yang dilindungi tidak bisa dibaca. Pengaturan tambahan tidak perlu diisi untuk mulai.

## Area baca yang lebih luas

Bilah atas dipadatkan menjadi satu baris. Bilah bawah berisi Terjemahkan, Lihat asli dan Hasil. Ketuk ikon sudut layar untuk menyembunyikan alat: bilah atas/bawah, bilah sistem Android dan tombol Reset per bagian ikut tersembunyi. Ikon kecil tetap ada untuk menampilkan alat kembali; ikon dapat digeser jika menutupi isi. Android juga dapat menampilkan bilah sistem sementara lewat sapuan tepi layar.

Status pemrosesan hanya muncul sementara, bukan panel tetap. Menu menyimpan Otomatis, pemindaian 1 halaman dan pengaturan tambahan. Tampilan penuh tidak menghapus terjemahan.

## Hasil tersimpan dan Reset

Mode biasa/otomatis menyimpan hasil dalam RAM pada dokumen aktif. Bagian yang sudah selesai tidak perlu diterjemahkan ulang saat kembali ke viewport yang sama. Bagian baru dibaca bertahap saat terlihat. **Reset** di bawah hasil membaca ulang satu bagian; jika ruang tidak cukup, gunakan Hasil → Reset bagian ini. Bagian lain tetap tersedia dan percobaan gagal mempertahankan hasil sebelumnya. Dalam mode bersih, tampilkan alat dahulu untuk mengakses Reset/Hasil.

Refresh, next/back, sumber baru dan penghentian proses menghapus cache dokumen. Riwayat URL/posisi tetap disimpan terpisah. Mengubah gaya/nama/server membersihkan hasil lama agar konfigurasi baru berlaku. Hasil bukan arsip permanen lintas navigasi.

**Terjemahkan 1 halaman** tetap tersedia lewat Menu dan hanya memindai dokumen aktif dari atas sampai bawah, tanpa membuka tautan berikutnya. Gunakan mode biasa untuk membaca bertahap. Batas pemindaian 400 viewport atau 6.000 bagian; halaman tanpa akhir, scroll-container khusus, canvas/iframe atau pergeseran layout masih dapat membatasi hasil.

## Bahasa dan tampilan hasil

Perbaikan penyatuan kalimat dan pemilihan arti termasuk dalam versi ini. Potongan OCR dengan sambungan tata bahasa yang jelas dicoba disatukan berdasarkan posisi, warna dan piksel pembatas. Potongan yang belum utuh ditunda, bukan ditebak. Penyesuaian arti lokal yang terbatas membedakan frasa seperti brand of fate (tanda takdir) dari konteks produk (merek). Nama yang telah dilindungi dipertahankan; daftar manual tersedia di pengaturan sumber. Jeda sesudah kata bantu seperti “I couldn’t…” dicoba disambungkan dengan kata kerja berikutnya. Frasa “sense her presence” mendapat pemeriksaan arti terbatas agar keluaran salah “sedihnya kehadirannya” dapat diperbaiki; sumber yang menyebut kesedihan sungguhan tidak ditimpa. “She’s a powerhouse” diperjelas sebagai orang yang sangat kuat, dengan pengecualian konteks listrik dan istilah yang dilindungi. Ini aturan lokal yang terikat sumber, bukan pemahaman otomatis semua cerita. Model dasar tetap ML Kit, bukan AI pemahaman seluruh konteks.

Penutup sumber digambar sebelum semua teks hasil agar penutup tetangga tidak memotong baris. Warna, ukuran dan kemiringan diperkirakan; font asli tidak disalin persis dan gambar di balik huruf tidak dipulihkan. Jika teks tidak muat pada ukuran terbaca, Hasil menyediakan teks lengkap. Tulisan dekoratif, gambar pecah, latar ramai, nama ambigu dan kalimat yang terpotong masih bisa salah. Bandingkan Lihat asli/Hasil.

## AI lokal opsional

Pembaca situs/file dapat memakai server pengguna sendiri untuk konteks yang lebih luas. Masukan teks langsung dan balon layar tetap memakai model di HP. Tidak ada server AI publik yang otomatis aktif atau API berbayar tersembunyi. Model lokal membutuhkan komputer, RAM, unduhan model dan listrik; hasil tetap dapat keliru.

1. Di komputer, pasang Ollama dan unduh model: `ollama pull qwen2.5:7b`.
2. Jalankan `python3 server/reader_server.py`. Simpan kode akses yang ditampilkan.
3. Hubungkan HP dengan USB debugging, izinkan komputer, lalu jalankan `adb reverse tcp:8765 tcp:8765`.
4. Pada pembaca TCD: Menu → Pengaturan → AI opsional. Alamat `http://127.0.0.1:8765/translate` dan kode akses dari server.
5. Kosongkan alamat untuk kembali ke mode dasar. Server jaringan lain wajib HTTPS serta kontrol akses yang disiapkan pemiliknya.

Server contoh mendengar 127.0.0.1, memeriksa kode akses/ukuran/antrean dan memisahkan konteks dokumen di RAM. Konteks kedaluwarsa 30 menit dengan batas 32 dokumen. Ini untuk penggunaan pribadi dan membutuhkan penguatan sebelum dijadikan layanan publik.

## Build dan batas pengujian

AGP8.7.3, Gradle8.9, JDK17, compile/targetSDK35. Buka di Android Studio atau jalankan Gradle setelah menentukan SDK. `scripts/build.sh` khusus workspace cloud dan menjaga proxy/CA sesi. Keystore privat tidak masuk sumber. Identitas paket/sertifikat dipertahankan agar pembaruan tetap memakai data aplikasi yang sama; APK masih bertanda tangan pengembangan dan belum dirilis di Play Store.

Pengujian Android memakai `-PcaptureTest` sehingga fixture/x86 hanya masuk APK uji. Property `modelTestResources` bersifat khusus pengujian; resource CA sesi tidak masuk APK pengguna/sumber. Percobaan model ML Kit asli belum mencapai inferensi karena unduhan model emulator melewati batas 180 detik. Uji UI/OCR/aturan serta engine simulasi dicatat terpisah dalam Pemeriksaan dan tidak dianggap jaminan kualitas seluruh bahasa. Belum diperiksa pada HP fisik pengguna atau runtime Android14/15.

APK utama dan sumber: `TCD-Android.apk` / `TCD-Source.zip`. Artefak lama tetap dipertahankan sebagai alias untuk kompatibilitas tautan.
