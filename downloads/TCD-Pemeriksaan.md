# Pemeriksaan TCD 0.2.0

## Build dan identitas APK

`:app:assembleDebug :app:lintDebug` lulus. APK pengguna: TCD, `com.komiklingo.app`, versionCode11/versionName0.2.0, minSDK26/targetSDK35, ARM64/ARMv7. `apksigner verify` berhasil; SHA-256 sertifikat tetap `bb1ef448fb34777d2d03a49e7bfdb2bfb355ba7d57fe7b6a11f410d020e346a1` sehingga APK dapat memperbarui aplikasi sebelumnya. Pemeriksaan ZIP/DEX/metadata memastikan fixture, ABI x86 dan resource CA uji tidak masuk APK pengguna. Log: `build-0.2.0.log`, `apk-check-0.2.0.log`.

## Perubahan tampilan

Beranda berisi Teks, Gambar & file, pencarian/alamat situs, Terjemahan layar dan satu sumber terbaru. Riwayat lengkap dibuka dalam dialog. Tidak ada istilah genre pada label produk; nama dokumen asli tidak disensor. Pembaca memiliki satu bilah atas dan tiga tombol bawah. Ikon yang bisa digeser menyembunyikan/menampilkan alat, Reset per bagian dan bilah sistem tanpa menghapus terjemahan. Status hanya chip sementara, serta kontrol aplikasi disembunyikan saat PixelCopy agar tidak ikut OCR. Warna dan kontras bilah sistem dipulihkan ketika alat ditampilkan.

Masukan teks langsung diteruskan ke model lokal tanpa OCR; tombol Salin menulis hasil ke clipboard. Perubahan input membatalkan hasil yang terlambat. Teks berbagi biasa membuka masukan teks, sedangkan URL utuh membuka pembaca. Tidak ada framework UI, provider, akun, izin atau biaya API baru.

## Bahasa: aturan terbatas, bukan model cerita baru

Lima suite JVM lulus: ReaderRules, NameRules, TranslationFlow, OcrQuality, ContextMeaning. Log: `jvm-0.2.0.log`.

- Sambungan kalimat diperiksa melalui tata bahasa dan geometri: prefiks artikel/preposisi dan kelanjutan yang jelas. Border, kolom berbeda, akhir kalimat dan nama dilindungi membatasi penyatuan.
- Modal terputus seperti `I COULDN’T...` dapat disatukan dengan kata kerja seperti `SENSE HER PRESENCE`; jeda di tengah struktur kata bantu dihapus dari input penerjemah. Pertanyaan mandiri seperti WHY NOT dan nama Sense tidak dipaksa bergabung.
- `brand of fate/destiny` diperjelas sebagai tanda takdir; konteks produk/perusahaan serta nama yang dilindungi tetap dijaga. Artikel A yang tertinggal setelah adalah/merupakan/ada hanya dibersihkan bila sumber mendukung makna artikel.
- Kesalahan target “sedihnya kehadirannya” diperbaiki menjadi “merasakan kehadirannya” hanya bila sumber mengandung sense/sensed/sensing presence. Sumber yang menyebut kesedihan sungguhan dikecualikan. Ini tidak mengubah kata English sadly menjadi sense secara global.
- Predikat orang seperti `she’s a powerhouse` diperjelas menjadi very powerful person sebelum mesin menerjemahkan. Target pembangkit listrik diperiksa hanya dalam konteks sumber itu. Istilah yang dilindungi, kalimat tanpa subjek manusia dan konteks eksplisit pembangkitan listrik dikecualikan. Kelanjutan on the level of dapat disatukan bila dekat dalam balon yang sama. Perlindungan nama Acratos dan pengecualian listrik diuji.
- Jalur TranslationFlow diuji menggunakan engine deterministik, termasuk input yang dinormalisasi dan keluaran yang diperiksa berdasarkan sumber. Tidak ada penggantian seluruh kalimat panel atau klaim memahami semua percakapan.

## Pengujian Android

Emulator Android11/API30 x86_64 dengan rendering perangkat lunak. Log awal `ui-initial-0.2.0.log`: 10 tes lulus (MinimalUi2, HomeHistory2, RetainedTranslation3, ContextAssembly3). Log `ui-reset-hidden-0.2.0.log`: dua tes UI lulus setelah Reset ikut disembunyikan pada mode bersih. Log `context-sense-0.2.0.log`: empat tes assembly lulus termasuk modal terputus. Pemeriksaan terakhir UI/assembly: enam tes lulus, termasuk geometri powerhouse dan kontras bilah sistem ketika alat ditampilkan kembali. Log `ui-context-final-0.2.0.log`.

Uji beranda mencakup label umum, pemilihan item Riwayat sebenarnya dan pemulihan posisi. Uji pembaca menjalankan WebView/PixelCopy/OCR nyata pada fixture sumber HELLO WORLD, memastikan kontrol/status tidak terbaca, area membesar ketika bilah disembunyikan dan cache dipertahankan. Uji masukan teks menjalankan translate/copy/edit dan membersihkan hasil basi. Engine terjemahan UI/cache dibuat deterministik agar pengujian tidak bergantung unduhan model.

Uji assembly menjalankan OCR ML Kit nyata pada gambar balon sintetis serta pemeriksaan geometri modal/powerhouse, border dan kolom. Cache/Reset memastikan hasil bertahan saat scroll, hanya bagian yang dipilih dibaca ulang, percobaan gagal menjaga hasil sebelumnya, hasil tumpang tindih tidak ditumpuk, serta refresh/next/back membersihkan dokumen. Screenshot `minimal-*.png` adalah fixture sintetis, bukan bukti keluaran bahasa mesin asli atau gambar pengguna.

## Model terjemahan asli: belum berhasil diuji

RealTranslationTest mencoba mengunduh model Inggris–Indonesia melalui SDK ML Kit. Task tidak selesai dalam batas 180 detik, sehingga gagal TimeoutException sebelum inferensi. Log `real-model-attempt-0.1.9.log` merekam percobaan; ini bukan tes lulus dan tidak membuktikan kualitas terjemahan model asli. Resource trust CA percobaan hanya berada di luar proyek dan tidak masuk APK pengguna/sumber. TLS produksi tidak dilonggarkan.

Model gratis tetap ML Kit yang sama. Aturan lokal membantu frasa tertentu dan sambungan teks, tetapi tidak menjamin semua idiom, emosi, nama atau makna cerita benar. Teks OCR salah, font dekoratif dan kalimat terpotong masih dapat gagal. Pengguna tetap dapat membandingkan sumber melalui Lihat asli/Hasil. AI lokal opsional pada pembaca membutuhkan komputer/server sendiri; masukan teks dan balon tetap lokal.

Tidak diuji di HP fisik pengguna atau runtime Android14/15. Service balon tidak diuji ulang pada tahap ini; batas MediaProjection emulator pada 0.1.8 tetap tercatat dalam validasi historis. Backend server tidak berubah dan tes server tidak diulang. APK bertanda tangan pengembangan, belum rilis Play Store.

Sumber tidak menyertakan keystore, local.properties, cache build, resource CA sesi atau gambar pengguna. Artefak TCD dan alias kompatibilitas KomikLingo dipublikasikan hanya pada branch unduhan; main dan sumber Dualworld tidak diubah.
