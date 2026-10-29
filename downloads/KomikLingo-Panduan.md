# KomikLingo 0.1.2 — pembaca terjemahan Android

Aplikasi terpisah dari Dualworld. Cari judul lewat DuckDuckGo, buka tautan komik/novel HTTPS atau pilih gambar, PDF dan TXT. Mode otomatis aktif saat awal: setelah gulir berhenti ±1 detik, aplikasi mengenali teks yang terlihat dan menampilkan Indonesia di area teks. Menggulir membersihkan overlay lama. `Lihat asli` membandingkan gambar asli; `Hasil` membuka terjemahan penuh. Ukuran font menyesuaikan kotak OCR; kotak yang terlalu kecil mengarahkan ke Hasil. File asli tidak ditulis ulang.

## Memakai APK

Pasang `dist/KomikLingo-Android.apk` pada Android 8+. Unduh model Inggris–Indonesia sekali melalui internet (menu ⋮ → Unduh model). OCR Latin sudah dibundel; file dapat diterjemahkan tanpa internet setelah model tersedia. Situs komik tetap membutuhkan internet. File: JPG/PNG/WebP, PDF tanpa kata sandi dan TXT UTF-8. EPUB/DOCX/CBZ belum didukung.

Tautan contoh Kayn Scans telah merespons HTTP 200 saat diperiksa; itu tidak membuktikan pembaca WebView akan selalu diizinkan oleh situs. Pemeriksaan/iklan/login situs tetap berlaku. Jika gambar belum muncul, tunggu lalu gulir sedikit atau tekan Terjemahkan. Aplikasi tidak melewati pembatasan akses situs.

## Dua pilihan gratis, dengan batas yang berbeda

* **Dasar di HP:** ML Kit English → Indonesian; tidak memerlukan API key atau langganan. Dapat salah membaca huruf komik bergaya, susunan balon atau kalimat yang terpotong. Terjemahannya bisa kaku dan belum memahami seluruh cerita. Membuka menu “AI” tidak membuat model dasar menjadi AI berkonteks.
* **AI lokal di komputer:** Ollama dan model terbuka Qwen2.5 7B. Tidak ada biaya API/langganan; membutuhkan komputer sendiri (disarankan RAM 16 GB), unduhan model sekitar beberapa GB, listrik dan internet untuk unduhan. Kecepatan mengikuti komputer. Sekumpulan dialog dan maksimal 12 pasangan dialog sebelumnya dikirim bersama agar gaya/emosi dan nama konsisten. Model tetap dapat salah dan kualitas bahasa perlu dinilai pembaca. Ini bukan jaminan terjemahan profesional.

Mode AI belum aktif pada APK karena komputer/server pengguna belum disiapkan. Tidak ada layanan gratis milik pembuat aplikasi yang diam-diam memproses gambar atau teks.

## Menyalakan AI lokal tanpa domain atau billing

1. Di komputer, pasang Ollama dari https://ollama.com/ lalu jalankan `ollama pull qwen2.5:7b`. Pastikan layanan Ollama aktif.
2. Jalankan `python3 server/reader_server.py`. Simpan kode akses yang tampil untuk dimasukkan di HP. Server hanya mendengar 127.0.0.1.
3. Pasang Android SDK Platform Tools di komputer, sambungkan HP dengan USB, aktifkan USB debugging dan izinkan komputer. Jalankan `adb reverse tcp:8765 tcp:8765`.
4. Di aplikasi: ⋮ → Pengaturan terjemahan AI. Alamat `http://127.0.0.1:8765/translate`, kode akses dari langkah 2. Ini melewati kabel USB, bukan internet publik. Komputer harus tetap menyala dan USB tersambung.
5. Kosongkan alamat AI untuk kembali ke mode dasar. Jika memakai Wi-Fi/internet, pemilik server harus menyiapkan HTTPS dan kontrol akses; aplikasi hanya mengizinkan HTTP untuk localhost.

Server Python menggunakan pustaka standar, tanpa kunci layanan berbayar. `OLLAMA_MODEL` mengubah model lokal. `READER_TOKEN` dapat menetapkan kode akses privat yang tetap. Server menolak akses tanpa kode, membatasi ukuran/antrean, memeriksa jumlah hasil dan memisahkan konteks per dokumen. Teks tidak dicatat ke log atau disk. Konteks dibuang setelah 30 menit; batas 32 dokumen di memori. Untuk pemakaian pribadi; perlu penguatan operasional sebelum dijadikan layanan publik.

## Build dan pemeriksaan

Proyek Android Gradle standar: AGP 8.7.3, Gradle 8.9, JDK 17, compile/target SDK35, build tools35.0.0. Buka folder ini di Android Studio atau jalankan `./gradlew assembleDebug` setelah mengatur SDK pada `local.properties`. `scripts/build.sh` khusus lingkungan cloud /workspace, menggunakan proxy dan CA session. Keystore debug privat tidak masuk arsip.

Pengujian server: `cd server && python3 -m unittest -v test_reader`. Tes integrasi memakai server Ollama tiruan untuk memeriksa autentikasi, batas input, kontrak hasil dan isolasi konteks; tidak mengukur kualitas model asli. Build APK dan manifest/tanda tangan diperiksa. Sensor/performa/OCR/unduh model dan overlay belum diuji pada HP fisik. APK ditandatangani untuk pengembangan dan belum diterbitkan di Play Store.

## Batas tampilan

Overlay menutup persegi teks hasil OCR dengan warna latar dominan, bukan mengembalikan lukisan di belakang huruf. Cocok terutama untuk balon warna polos. Efek suara, huruf lengkung, tulisan tangan, teks di tepi layar, font dekoratif dan latar bergambar bisa salah. Baca Hasil atau perbesar gambar bila perlu. Tidak ada pengenalan kamera/layar aplikasi lain; aplikasi membaca isi pembacanya sendiri.

Teks kecil: cubit/perbesar situs, gambar atau halaman PDF sebelum diterjemahkan. OCR juga menerima tangkapan viewport yang diperbesar dengan batas memori. Pembesaran tidak dapat memulihkan huruf yang sudah pecah atau tidak memiliki detail. Gaya otomatis mempertahankan kapitalisasi dan memakai huruf tebal untuk sumber kapital, serta font yang mudah dibaca. Font artistik/kemiringan/bentuk huruf asli tidak disalin persis. Paket APK mendukung HP ARM64 dan ARMv7, bukan emulator x86.


## Perbaikan 0.1.1

Pengambilan gambar OCR memakai PixelCopy pada area pembaca di jendela aplikasi. Ini mengambil piksel WebView yang sudah dirender, termasuk gambar yang tampil lewat GPU, setelah lapisan terjemahan dibersihkan selama dua frame. Menggambar ulang WebView langsung ke Canvas tidak lagi dipakai. Kesalahan mengambil gambar dibedakan dari kegagalan mengenali huruf. Pembacaan otomatis dibersihkan saat aplikasi masuk latar/rotasi; callback gagal yang sudah kedaluwarsa juga melepas antrean agar pembaca tidak macet.

Uji regresi Android tersedia di `app/src/captureTests`: `./gradlew -PcaptureTest connectedDebugAndroidTest` pada emulator x86_64. Fixture gambar dan activity uji hanya disertakan jika properti `captureTest` aktif; tidak masuk APK pengguna. Uji memeriksa OCR Latin asli pada gambar WebView dengan akselerasi grafis, pemotongan area header/footer dan perpindahan viewport setelah scroll. Itu tidak menilai kualitas model terjemahan atau perilaku semua situs.


## Membuka situs lain dan gaya dialog (0.1.2)

Tekan × pada kolom alamat lalu tempel URL, tekan Cari/tombol pencarian keyboard, atau pilih ⋮ → Cari komik / buka situs. Alamat tanpa skema otomatis memakai HTTPS. Bagikan tautan dari browser ke KomikLingo juga didukung; tidak ada daftar domain komik yang dikunci ke Kayn Scans. Situs dengan pembatasan WebView, login atau perlindungan akses tetap mengikuti aturannya sendiri.

Gaya bawaan **Dialog akrab (aku–kamu)** menyesuaikan sapaan mode dasar. Pilihan **Netral (saya–kamu)** dan **Formal (saya–Anda)** tersedia di ⋮ → Gaya bahasa cerita. Pilihan disimpan per situs/cerita (pola path chapter/episode dikelompokkan), sehingga dua situs/cerita tidak otomatis berbagi pilihan. Sapaan hormat yang terlihat jelas seperti Sir/Your Majesty mempertahankan register formal di mode dasar. Ini preferensi gaya dengan aturan terbatas, bukan pendeteksian suami/istri atau pemahaman hubungan tokoh dari gambar.

OCR membersihkan beberapa kesalahan terikat pola: kata penghubung menyatu dengan I sebelum verba, huruf akhir ganda pada kosakata umum tertentu, dan awalan F salah pada everyone/everything. Tidak ada penggantian kalimat komik secara hafalan. Nama seperti Andi dan seruan seperti nooo dipertahankan oleh uji regresi. Aturan tetap bisa salah pada nama/teks yang tidak lazim.

Untuk 2–6 blok sampai 5.000 karakter, mode dasar mencoba menerjemahkan paragraf bersama. Hasil hanya dipakai bila jumlah paragraf cocok; bila tidak, tiap blok diterjemahkan ulang agar posisi/urutan tidak tercampur. Hasil menampilkan pasangan Inggris yang sudah dibersihkan dan Indonesia. Mengelompokkan paragraf serta menyesuaikan sapaan tidak menjadikan model dasar memahami seluruh cerita atau menjamin semua idiom benar.

AI lokal opsional menerima gaya bahasa serta Catatan cerita (maksimum 800 karakter pada aplikasi), misalnya hubungan pasangan atau pilihan sapaan tokoh. Server membawa konteks beberapa dialog sebelumnya dan memisahkannya bila dokumen, gaya atau catatan berubah. AI belum otomatis aktif tanpa komputer/server yang disiapkan. Kualitas keluaran model asli tetap perlu dinilai pembaca.

Pemeriksaan aturan: `bash tests/run.sh` dengan JDK17+. Tes server: `cd server && python3 -m unittest -v test_reader`. Uji UI Android dengan `-PcaptureTest` memakai halaman fixture dua domain HTTPS, bukan bukti akses seluruh situs nyata.


## Pencarian DuckDuckGo (termasuk dalam 0.1.2)

Kolom atas menerima **nama komik/kata kunci atau tautan**. Nama seperti “Chronicle of Runes komik” membuka pencarian DuckDuckGo; domain seperti `nama-situs.com/bab/1` langsung dibuka melalui HTTPS. Tekan Cari atau tombol pencarian keyboard. DuckDuckGo menjadi halaman awal untuk instalasi baru; instalasi lama tetap melanjutkan tautan terakhir. Pilihan menu Cari komik / buka situs menyediakan kolom kosong agar lebih mudah mengganti pencarian.

Hasil pencarian ditampilkan sebagai situs DuckDuckGo di dalam WebView, dengan tautan hasil tetap di pembaca. Aplikasi tidak menggunakan API pencarian berbayar, tidak membuat indeks sendiri dan tidak menyediakan saran alamat secara native. Terjemahan otomatis ditahan pada halaman pencarian supaya tidak menutup tombol/hasil; setelah masuk situs komik, pembacaan otomatis melanjutkan pilihan sebelumnya. Tombol Terjemahkan manual tetap dapat digunakan pada hasil pencarian.

Kata kunci dikirim ke DuckDuckGo sesuai tindakan pencarian pengguna. DuckDuckGo/situs tujuan dapat menampilkan verifikasi atau menolak WebView sesuai ketentuan mereka; aplikasi tidak melewati pembatasan itu. Pemeriksaan dari jaringan cloud menerima HTTP202 berjudul pencarian DuckDuckGo dengan elemen verifikasi, sehingga bukan bukti hasil pencarian langsung selalu tersedia di semua jaringan. Tes UI memakai halaman fixture untuk memeriksa alamat mesin, tampilan kata kunci dan pembukaan tautan di pembaca.
