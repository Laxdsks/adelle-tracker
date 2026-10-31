# Pemeriksaan TCD 0.5.0

## Perubahan yang diperiksa

Google Translate online dipilih secara default pada pembaca komik, teks dan balon. OCR/gambar tetap lokal; teks dan konteks dikirim melalui HTTPS. Mesin Offline dapat dipilih di pengaturan bersama. Model cadangan disiapkan tanpa menghalangi mode Google. Endpoint web publik tidak memerlukan kunci API dan bukan Google Cloud Translation dengan SLA.

Router menyimpan maksimal 128 hasil Google valid di RAM, memisahkan offline dari cache Google, membatalkan respons terlambat, serta beralih ke ML Kit saat jaringan/respons gagal. Batas koneksi 2,5 detik, baca 3,5 detik, ukuran respons 128 KiB dan input 4.600 karakter. Redirect tidak diikuti. HTTP 429/302 diberi jeda minimal lima menit; Retry-After angka/tanggal dihormati. Cache pembaca tidak menyimpan hasil cadangan offline sebagai hasil Google.

Game menunggu teks stabil lalu memeriksa maksimal tiga baris OCR meragukan dengan Tesseract. Urutan baris pecah diperiksa. Pemisah konteks yang hilang dicoba ulang dengan penanda berurutan. Nama dicoba ulang dalam kalimat utuh sebelum fragmen. Pemeriksaan hasil mendeteksi sebagian kuantitas yang hilang dan hasil yang terpotong jauh; tidak menilai seluruh makna kalimat. Seleksi wilayah, opacity, warna dan penerusan sentuhan dari versi sebelumnya dipertahankan.

## Hasil

- `validation/jvm-0.5.0.log`: 12 suite lulus, termasuk konteks sebelum/sesudah, urutan respons, perlindungan nama, cache, fallback, cooldown/recovery, pembatalan, kuantitas dan OCR.
- `validation/google-transport-0.5.0.log`: pengodean URL, penggabungan semua segmen JSON, timeout, respons kosong/rusak/terlalu besar, HTTP error/redirect, Retry-After dan nilai batas lulus. Ini fixture HTTP, bukan pengukuran mutu bahasa.
- `validation/android-live-0.5.0.log`: uji game nyata pada aplikasi fixture terpisah lulus (140,577 detik). MediaProjection/OCR nyata; penerjemah deterministik. Menyeleksi wilayah, mengatur warna/latar, meneruskan sentuhan Next melalui overlay, mengganti hasil otomatis, deduplikasi, jeda, menolak respons terlambat, lanjut dan stop diperiksa. Tidak ada snapshot/modal hasil dalam mode live.
- `validation/android-google-provider-0.5.0.log`: lulus (5,769 detik). Mode Google siap tanpa menunggu unduhan offline, pilihan Offline memakai jalur lokal, dan callback dibatalkan setelah cancel. Tidak menguji inferensi model offline.
- `validation/build-arm-0.5.0.log`: assembleDebug dan lintDebug lulus; APK ARM64/ARMv7, minSDK26, targetSDK35, versionCode29. `validation/apk-0.5.0.log` mencatat metadata dan sertifikat.

## Batas bukti bahasa dan jaringan

Probe HTTPS awal ke Google menerima HTTP 200 untuk satu contoh tiga kalimat. “Her scent lingers in the air” menghasilkan “Aromanya tetap melekat di udara”; durasi 1,32 detik di lingkungan uji, bukan tolok ukur HP. Bukti dicatat pada `validation/google-initial-response-0.5.0.txt`.

Percobaan pipeline Google dengan sepuluh contoh belum berhasil diselesaikan: layanan memberi redirect pembatasan (302), dan pemeriksaan HTTPS Python menerima 429. Respons redirect sengaja ditolak, tidak dilewati. Percobaan lanjutan setelah jeda juga dicatat pada `validation/google-live-0.5.0.log`; batas layanan tetap menjadi risiko operasional. Uji ini tidak boleh dianggap lulus atau dijadikan klaim persentase peningkatan akurasi. Cadangan offline dan penanganan kegagalan diuji dengan engine simulasi; inferensi ML Kit nyata sebelumnya belum selesai karena unduhan model emulator melewati batas 180 detik.

Pengujian Android memakai emulator Android 11, bukan HP/game pengguna. Belum diuji pada perangkat fisik atau Android14/15. Tulisan sulit, kalimat ambigu, model offline, koneksi dan pembatasan Google masih dapat menghasilkan kesalahan/jeda. Subtitle panjang dapat memakai elipsis. Tidak ada jaminan terjemahan 100% akurat atau selalu lebih cepat. Sumber berisi log historis; hanya log versi ini adalah pemeriksaan perubahan ini.
