# Pemeriksaan TCD 0.2.1

## Perubahan dan cakupan

Masalah yang diperbaiki: OCR dapat memisahkan kata terakhir dalam balon menjadi unit terjemahan tersendiri, misalnya `... SUCH A PRICKLY` dan `CAT.`. Pemisahan ini menghilangkan konteks kata sifat/kata benda serta menambah tombol Reset. Versi ini menambahkan bukti area polos tertutup, selain pemeriksaan tata bahasa yang sudah ada, untuk menyatukan fragmen berdekatan sebelum dikirim ke penerjemah. Satu hasil gabungan memakai satu Reset.

Mask area dibuat secara malas hanya jika tata bahasa belum cukup dan fragmen berdekatan/selaras. Setiap piksel dalam sel diuji, sehingga garis tipis tidak dilewati hanya karena titik sampel jatuh di sela garis. Konektivitas empat arah, area yang menyentuh tepi gambar, jarak, kolom, warna, teks perantara dan tanda akhir membatasi penyatuan. Halaman putih terbuka dan balon terpotong tidak diperlakukan sebagai area tertutup. Maksimal dua analisis warna, sekitar 350 ribu sel, buffer piksel berupa strip kecil dan batas gambar 8 megapiksel membatasi biaya tambahan. Ini bukan model pendeteksi balon dan masih dapat salah pada panel/ilustrasi atau batas yang tidak jelas.

Kiasan `glare/stare/act/behave ... like a prickly cat/person/...` dinormalisasi ke grumpy hanya dalam konteks perilaku. Sumber dengan duri, jarum, kaktus, patung atau nama yang dilindungi dikecualikan. Keluaran kucing berduri diperiksa hanya jika sumber mendukung kiasan itu. Kata prickly tidak diganti secara global.

Struktur `I/we/you/they have`, `he/she/it has`, subjek nama yang dilindungi, atau bentuk had dengan `no intention of` dinormalisasi menjadi do/does/did not plan on. Penekanan absolutely/certainly/definitely/really, objek dan bentuk gerund tetap dipertahankan. Frasa punya/memiliki yang salah tersisa sebelum tidak berniat/bermaksud/berencana diperbaiki hanya bila sumber mendukung pola penolakan tersebut. Bentuk afirmatif, modal would have dan kepemilikan biasa tidak diubah. Aturan arti 0.2.0 serta antarmuka minimal tetap disertakan.

## Pemeriksaan build dan aturan

`:app:assembleDebug :app:lintDebug` lulus. Paket `com.komiklingo.app`, nama TCD, versionCode12/versionName0.2.1, minSDK26/targetSDK35; ABI ARM64/ARMv7. `apksigner verify` berhasil dan sertifikat tetap `bb1ef448fb34777d2d03a49e7bfdb2bfb355ba7d57fe7b6a11f410d020e346a1`. ZIP/DEX diperiksa tidak memuat fixture, ABI x86 atau CA sesi pengujian. Tidak ada izin, dependensi, provider jaringan atau akun baru.

Lima suite JVM lulus: ReaderRules, NameRules, TranslationFlow, OcrQuality dan ContextMeaning. Kasus baru mencakup input/hasil prickly figuratif dan literal, istilah yang dilindungi, bentuk negatif/afirmatif intention, tense serta pemanggilan TranslationFlow. Engine pada tes alur bersifat deterministik; tes tersebut memeriksa aturan/input/output, bukan kemampuan model ML Kit asli.

Log build: `validation/build-0.2.1.log`; aturan: `validation/jvm-0.2.1.log`; identitas APK: `validation/apk-check-0.2.1.log`.

## Pengujian Android

Sebelas tes Android lulus: BalloonContextTest4, ContextAssemblyTest4 dan RetainedTranslationTest3. Log `validation/balloon-native-0.2.1.log`.

- BalloonContextTest: fragmen tiga blok serta kata sifat yang tidak terdaftar dapat digabung dalam area tertutup; halaman terbuka, balon terpotong, border tipis, kolom berbeda, tanda akhir dan teks perantara membatasi penyatuan. OCR Latin ML Kit nyata pada gambar sintetis miring/kapital menghasilkan satu unit kalimat prickly cat untuk input terjemahan.
- ContextAssemblyTest: aturan sambungan prefiks, modal terputus, powerhouse, nama serta separator lama tetap diperiksa; OCR nyata pada fixture brand of fate tetap disatukan.
- RetainedTranslationTest: terjemahan biasa bertahan saat scroll, Reset hanya mengulang bagian terpilih, hasil lama tetap tersedia saat gagal, refresh/next/back membersihkan dokumen, hasil tumpang tindih tidak ditumpuk dan drag dari Reset tetap menggulir.

Android memakai emulator Android11/API30 x86_64 dengan rendering perangkat lunak. OCR, bitmap, geometri, cache dan View menggunakan implementasi asli. Penerjemah dalam fixture cache memakai engine simulasi. Gambar uji dibangkitkan dari teks, tanpa membundel screenshot pengguna. `captureTest` hanya untuk APK pengujian.

## Batas kualitas dan verifikasi

Model gratis tetap ML Kit. Tidak ada AI pemahaman seluruh cerita yang baru. Menyatukan teks dan memperjelas arti tertentu membantu contoh yang didukung aturan, tetapi tidak menjamin semua dialog, emosi, idiom atau nama akurat. Kalimat terpotong, balon tidak tertutup/berwarna rumit, font dekoratif dan OCR yang salah masih dapat menghasilkan kesalahan. Lihat asli/Hasil tetap tersedia untuk membandingkan.

Percobaan model Inggris–Indonesia asli pada tahap 0.2.0 belum mencapai inferensi: unduhan SDK melewati 180 detik. Log historis `real-model-attempt-0.1.9.log` tetap disertakan. Percobaan tersebut tidak dianggap tes lulus dan tidak diulang sebagai bukti kualitas bahasa versi ini. Hasil terjemahan model asli di HP fisik pengguna belum diverifikasi.

Service balon dan server AI opsional tidak diubah khusus pada rilis ini; service memakai helper paragraf/aturan yang sama. MediaProjection tidak diuji ulang; batas historisnya tetap dicatat dalam validasi 0.1.8. Belum diuji di HP pengguna atau runtime Android14/15. APK memakai tanda tangan pengembangan, belum dirilis di Play Store.

Publikasi mencakup APK, checksum, panduan, catatan pemeriksaan dan ZIP sumber TCD beserta alias kompatibilitas KomikLingo hanya pada branch unduhan. Sumber tidak memuat keystore, local.properties, cache build atau CA sesi. Tidak mengubah main maupun sumber Dualworld. Pemeriksaan antarmuka 0.2.0 tetap tersedia dalam catatan historis.
