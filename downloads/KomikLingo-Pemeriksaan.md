# Pemeriksaan TCD 0.2.2

## Perubahan arti dan struktur

Masalah sumber: `IT WAS A FAILURE. BY THE TIME I BURST IN–` diikuti `IT HAD ALL ALREADY VANISHED.`. Arti burst in ialah masuk mendadak; keluaran aku meledak tidak sesuai. Koreksi tidak mengganti seluruh kalimat panel dengan terjemahan siap pakai atau menebak benda yang telah lenyap.

PhrasalVerbs memeriksa burst/barge, pelaku manusia atau penanda nama, kata bantu, partikel in/into dan pelengkapnya sebagai satu unit. Masukan dapat diperjelas menjadi enter/entered/entering abruptly atau entered without permission. Into memerlukan tujuan tempat yang dikenal. Ejaan bare burst yang tidak menentukan present/past dibiarkan jika bukti waktu tidak cukup; klausa by the time dengan petunjuk lampau dari kalimat pembuka dapat diperjelas. Future/habitual dan bentuk modal/perfect/progressive dibatasi agar tidak ditebak sembarang.

Post-processing memeriksa kesalahan aku/dia/dll. meledak/meletus hanya jika sumber mendukung masuk mendadak dan pelakunya sesuai. Ungkapan tears, flames, out laughing, in pain, unison, on a meeting, percakapan, benda peledak tanpa pelaku manusia dan kata yang dilindungi sebagai nama tidak dianggap masuk. Adanya tindakan explode/detonate eksplisit mengecualikan penggantian target agar ledakan asli tetap terjaga. Ini subset aturan lokal, bukan analisis semua phrasal verb bahasa Inggris.

Partikel yang terpisah blok OCR dapat disatukan dengan verba/pelakunya jika geometri dan piksel pembatas mendukung. Kalimat pembuka lengkap boleh terhubung dengan klausa waktu masuk berikutnya hanya bila berdekatan dan berada dalam area polos tertutup yang sama. Penggabungan ditinjau kembali ke blok sebelumnya setelah blok lanjutan menjadi utuh; tiap penggabungan mengurangi jumlah blok sehingga peninjauan ulang dibatasi jumlah fragmen. Halaman putih terbuka, border/kolom berbeda, kalimat lengkap tak terkait dan nama tetap membatasi sambungan.

TranslationFlow menggunakan posisi setiap unit untuk melihat klausa berikutnya, termasuk bila sumber yang sama muncul dua kali. Tanda pisah pada klausa waktu masuk yang lengkap dapat diganti koma dalam masukan mesin ketika unit berikutnya diawali subjek/kata kerja finit. Stutter/potongan kalimat yang belum lengkap tidak dianggap klausa lengkap. Kotak hasil pada dua balon berjauhan tetap terpisah; tidak menggambar satu kotak besar melintasi keduanya. Batch yang tak mempertahankan pemisahan masih diulang per unit, dengan pemeriksaan nama dan arti yang sama.

## Build dan identitas

`:app:assembleDebug :app:lintDebug` lulus. TCD versionCode13/versionName0.2.2, paket tetap `com.komiklingo.app`, minSDK26/targetSDK35, ARM64/ARMv7. Pemeriksaan ZIP/DEX/metadata memastikan PhrasalVerbs masuk APK sedangkan fixture, x86 dan CA sesi tidak. `apksigner verify` lulus; sertifikat SHA-256 tetap `bb1ef448fb34777d2d03a49e7bfdb2bfb355ba7d57fe7b6a11f410d020e346a1` untuk pembaruan APK lama.

Log `validation/build-0.2.2.log` dan `validation/apk-check-0.2.2.log`. Tidak ada izin, dependensi, provider, biaya API atau akun baru; pengolahan tambahan tetap di HP. Identitas/ikon dan UI minimal 0.2.1 dipertahankan.

## Pengujian aturan dan Android

Enam suite JVM lulus: ReaderRules, NameRules, TranslationFlow, OcrQuality, ContextMeaning dan PhrasalVerbs. Mencakup makna masuk/literal/emosi, bentuk waktu/negasi, nama yang dilindungi, partikel terpisah, klausa yang belum lengkap, kepemilikan penanda, urutan batch, dua sumber identik dengan tetangga berbeda dan fallback per unit. Log `validation/jvm-0.2.2.log`.

Lima belas tes Android lulus: PhrasalContextTest4, BalloonContextTest4, ContextAssemblyTest4 dan RetainedTranslationTest3. Log `validation/phrasal-native-final-0.2.2.log`. Emulator Android11/API30 x86_64 dengan rendering perangkat lunak; fixture dibangun memakai captureTest. Pemeriksaan meliputi PhrasalContextTest, BalloonContextTest, ContextAssemblyTest dan RetainedTranslationTest.

PhrasalContextTest menjalankan OCR Latin ML Kit nyata pada gambar sintetis berisi kalimat failure/burst in dan vanished. Percobaan awal menghasilkan tiga unit, bukan dua: kalimat pembuka terpisah dari klausa waktu sehingga konteks lampau tidak sampai ke masukan mesin. Ini kegagalan sebenarnya pada `phrasal-native-initial-0.2.2.log`, bukan tes lulus. Sambungan area tertutup dan peninjauan ulang ke blok sebelumnya ditambahkan untuk memperbaikinya; uji regresi manual juga mencakup tiga blok berantai. Nilai harapan unit tes tetap dua dan tes diulang setelah perubahan implementasi.

OCR, bitmap, geometri, cache dan View nyata dipakai. Cache/Reset diuji agar scroll mempertahankan hasil, Reset hanya mengulang bagian terpilih, kesalahan tidak menghapus hasil sebelumnya, refresh/next/back membersihkan dokumen dan drag tetap menggulir. Engine terjemahan pada tes alur/cache adalah engine deterministik; tes ini memeriksa masukan/pemetaan/pemeriksaan hasil, bukan menilai kualitas bahasa model asli.

## Batas verifikasi

Model dasar masih ML Kit yang sama. Percobaan unduhan model Inggris–Indonesia asli pada tahap sebelumnya tidak selesai dalam 180 detik, sehingga belum mencapai inferensi; log historis tetap disertakan. Tidak ada klaim bahwa contoh atau semua percakapan sudah dinilai dengan model asli di HP pengguna. Memperjelas frasa dan menyatukan teks membantu aturan yang tercakup, tetapi idiom lain, kata kerja/benda ambigu, rujukan cerita, nama, font dekoratif dan OCR yang salah masih dapat gagal. Model pemahaman cerita yang lebih kuat tidak otomatis tersedia hanya dengan tambahan aturan ini.

Server AI opsional dan service MediaProjection tidak diuji ulang pada tahap ini. Helper yang sama dipakai pembaca dan balon layar, tetapi batas pengujian MediaProjection historis tetap berlaku. Belum diuji pada HP pengguna atau runtime Android14/15. APK memakai tanda tangan pengembangan, belum rilis Play Store.

ZIP sumber tidak memuat keystore, local.properties, cache build, gambar pengguna atau resource CA sesi. Gambar uji dibangkitkan dari teks. Artefak TCD serta alias KomikLingo hanya diperbarui pada branch unduhan; main dan sumber Dualworld tidak diubah. Validasi 0.2.1 dan sebelumnya disimpan sebagai catatan historis.
