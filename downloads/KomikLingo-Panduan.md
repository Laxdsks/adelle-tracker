# KomikLingo 0.1.0 — pembaca terjemahan Android

Aplikasi terpisah dari Dualworld. Buka tautan komik/novel HTTPS atau pilih gambar, PDF dan TXT. Mode otomatis aktif saat awal: setelah gulir berhenti ±1 detik, aplikasi mengenali teks yang terlihat dan menampilkan Indonesia di area teks. Menggulir membersihkan overlay lama. `Lihat asli` membandingkan gambar asli; `Hasil` membuka terjemahan penuh. Ukuran font menyesuaikan kotak OCR; kotak yang terlalu kecil mengarahkan ke Hasil. File asli tidak ditulis ulang.

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
