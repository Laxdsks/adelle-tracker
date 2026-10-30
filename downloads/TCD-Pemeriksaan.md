# Pemeriksaan TCD 0.3.0

## Browser dan fitur lama

Ditambahkan tab persisten (maksimal 16), bookmark dengan bintang (maksimal 200), pemilihan tab, penutupan tab, tautan ke tab baru, Muat ulang dan Maju. Metadata privat berisi ID/URL/judul/posisi/tab aktif; bookmark menyimpan alamat, bukan halaman offline. Satu WebView dipakai dan tab dimuat saat dipilih agar tidak menahan 16 halaman aktif di RAM. Riwayat mundur/maju tab aktif direset ketika berganti tab untuk mencegah Back membuka halaman milik tab lain; riwayat global tetap tersedia.

Beranda awal, DuckDuckGo, Teks, Gambar & file, PDF/TXT, mode otomatis, pemindaian 1 halaman, cache hasil saat scroll, Reset per bagian, Lihat asli, Hasil, gaya/nama, AI opsional dan terjemahan layar tetap tersedia. Mode 1 halaman dan Otomatis pindah ke submenu Terjemahan. Kembali ke tab aktif yang sudah terbuka dapat mempertahankan tampilan/cache tanpa memuat ulang. Pergantian dokumen/tab, refresh atau next/back tetap membersihkan hasil aktif sesuai batas cache sebelumnya.

## Filter dan keamanan

Daftar domain iklan/pelacak dibundel secara lokal; pencocokan batas hostname/subdomain mencegah domain seperti doubleclick.net.safe.example ikut diblokir. Permintaan resource dikenal dibalas kosong sebelum transfer, sedangkan gambar domain biasa tetap diizinkan. Beberapa slot iklan standar disembunyikan dengan CSS. Cookie pihak ketiga dibatasi secara default, dan pengecualian per situs mengizinkannya kembali bersama resource yang disaring. Popup skrip tetap ditolak; anchor nyata yang diketuk dapat dibuka ke tab baru. Akses kamera/mikrofon dari situs ditolak. Pengaturan data/cache WebView menggunakan provider Android, tanpa proxy atau intersepsi TLS tambahan.

Situs HTTP dinaikkan ke HTTPS; URL berisi kredensial/skema file/content/intent/javascript/data ditolak pada navigasi browser. URL file internal hanya untuk renderer file milik aplikasi. Sertifikat bermasalah dibatalkan. Safe Browsing diaktifkan pada WebView, bergantung dukungan provider Android. Ini tidak menjamin semua situs aman atau semua iklan hilang. Iklan pihak pertama/domain baru/iklan pada gambar tidak seluruhnya dicakup daftar kecil ini. Bukan VPN: blokir ISP/DNS, akun, wilayah, CAPTCHA dan sertifikat tidak dilewati. DuckDuckGo tetap mesin pencari.

## Kecepatan dan perbaikan teks

TranslationFlow dapat mengingat keluaran per unit dari batch yang valid dan melewati unit yang sumber/konteks tetangganya sama. Kunci konteks SHA-256 dipadatkan, dihitung hanya bagi engine yang mendukung cache dan digunakan kembali dalam satu alur. Cache dibagi dengan batas 400 entri RAM yang sudah ada; bukan arsip hasil pada disk. Hasil tidak valid/nama hilang, perubahan konteks atau Reset tidak dianggap cache valid. Mode dasar tetap menerjemahkan di HP setelah unduhan model awal. Filter dapat mengurangi transfer iklan, tetapi tidak ada klaim penghematan persentase atau tolok ukur kecepatan pada HP fisik.

Pola OCR NE PLAN dan READY TO MOE memicu pembacaan ulang/penilaian kandidat visual dengan batas percobaan yang sama. Tidak ada substitusi kata otomatis berdasarkan kamus; nama Moe yang dilindungi tetap dijaga. Nama tempat kapital yang belum dikenal kamus sebelum Kingdom/Empire/City/Village dapat ditambahkan berdasarkan bukti struktur. Nama yang sudah salah terbaca masih bisa salah; tidak ada janji mengenali semua nama.

Konteks medis headache/pain/migraine/rash/symptoms is/are flaring up diperjelas sebagai getting worse. Api/flames dan istilah yang dilindungi tidak diganti. Ini tambahan aturan terbatas bersama perbaikan 0.2.2, bukan model AI yang memahami semua percakapan.

Overflow overlay tidak lagi melukis strip penutup sebagian dengan badge Hasil pada teks asli. Jika terjemahan tidak muat pada ukuran minimum, piksel sumber dibiarkan utuh; chip Terjemahan di luar kotak membuka hasil penuh, pilihan Salin dan Reset. Bila tidak ada ruang bebas untuk chip, Hasil global tetap menyimpan teks lengkap. Mode bersih menyembunyikan kontrol inline sampai alat ditampilkan. Untuk context service/non-Activity, dialog menggunakan TYPE_APPLICATION_OVERLAY dengan pemeriksaan izin yang sudah ada; context Activity memakai jendela biasa. MediaProjection penuh tidak diuji ulang pada tahap ini.

## Build

`:app:assembleDebug :app:lintDebug` lulus. TCD versionName0.3.0/versionCode14, paket `com.komiklingo.app`, minSDK26/targetSDK35, ARM64/ARMv7. Sertifikat SHA-256 tetap `bb1ef448fb34777d2d03a49e7bfdb2bfb355ba7d57fe7b6a11f410d020e346a1`, diperiksa dengan apksigner. ZIP/DEX memastikan kelas browser masuk dan fixture, kelas tes, x86 serta CA sesi tidak masuk APK pengguna. Tidak ada izin atau pustaka UI/provider baru. Log build dan pemeriksaan APK disertakan dalam validation.

## Tes yang dijalankan

Tujuh suite JVM lulus: ReaderRules, NameRules, TranslationFlow, OcrQuality, ContextMeaning, PhrasalVerbs dan BrowserRules. Kasus baru mencakup batas domain/HTTPS, resource gambar, cache unit dan perubahan tetangga, hasil cache rusak, perlindungan nama, reset, pemicu baca ulang tanpa substitusi, nama kerajaan dan kiasan medis. `jvm-0.3.0.log`.

Dua puluh tes Android lulus pada emulator Android11/API30 x86_64, rendering perangkat lunak: BrowserTest2, ReaderRepairTest4, MinimalUiTest2, HomeHistoryTest2, RetainedTranslationTest3, MaskRegressionTest1 dan TypographyTest6. `browser-native-final-0.3.0.log`.

BrowserTest menjalankan WebView nyata dengan resource HTTPS sintetis yang diintersep oleh fixture: domain iklan benar-benar mendapat balasan kosong dari filter produksi, gambar biasa tetap diminta, slot iklan disembunyikan, popup tidak menambah tab, bookmark disimpan lewat pilihan menu sebenarnya, tombol Tab baru dan pemilihan judul dipakai, posisi dipulihkan, Back tidak melintasi tab, dan metadata bertahan setelah Activity dibuat ulang. Percobaan awal tes menilai jumlah tab dalam giliran UI yang sama sebelum callback tombol Dialog diproses; tes diperbaiki untuk menunggu callback, bukan mengubah produksi agar lolos. Log awal dipisahkan.

ReaderRepairTest membandingkan kandidat OCR, menjaga nama, memeriksa piksel sumber pada overflow tetap identik, membuka/menyalin teks penuh melalui chip, dan menjalankan OCR ML Kit nyata pada tulisan miring WE/MOVE/UHWANA. Uji lama memeriksa area baca yang membesar, history, Reset, cache saat scroll, angka 60/GO, warna/ukuran dan penutup tetangga. Terjemahan dalam fixture UI/cache memakai engine deterministik; ini bukan verifikasi kemampuan bahasa model ML Kit asli.

Tiga pemeriksaan UI lanjutan lulus (dua BrowserTest diulang untuk screenshot setelah kompositor siap, serta satu tes dialog non-Activity dengan izin overlay pada emulator). Window dialog benar-benar TYPE_APPLICATION_OVERLAY dan dapat ditampilkan. Cold start memakai dua proses instrumentation terpisah: seed satu tes lulus, force-stop paket aplikasi, kemudian verify satu tes lulus. Dua tab, bookmark, ID aktif dan posisi bertahan, sementara startup tidak memuat halaman tersimpan. Log browser-ui-final, browser-cold-seed dan browser-cold-verify disertakan. Screenshot browser memakai halaman sintetis dan menunggu kompositor sebelum pengambilan, bukan screenshot pengguna.

## Batas

Model gratis tetap ML Kit yang sama. Unduhan model asli dalam percobaan historis melewati 180 detik sebelum inferensi; tes itu tidak dianggap lulus dan tidak dipakai sebagai bukti kualitas seluruh terjemahan. Pengujian ini belum memastikan arti setiap dialog di HP pengguna. OCR/font dekoratif, ejaan nama, rujukan cerita, kata kerja/benda ambigu dan idiom masih dapat salah. AI lokal opsional tetap memerlukan server milik pengguna dan tidak otomatis aktif.

Belum diuji pada HP fisik pengguna, runtime Android14/15, seluruh situs/perantara jaringan atau alur login popup skrip. MediaProjection/service penuh dan backend server tidak diuji ulang; catatan historis tetap berlaku. APK ditandatangani untuk pengembangan, belum rilis Play Store.

Sumber dan artefak tidak menyertakan keystore, local.properties, cache build, CA sesi atau gambar pengguna. Publikasi memperbarui APK/checksum/panduan/catatan/sumber TCD serta alias KomikLingo hanya pada branch unduhan. Main dan sumber Dualworld tidak diubah.
