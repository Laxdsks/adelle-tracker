# Pemeriksaan TCD 0.3.5

## Perubahan

Aturan sumber Inggris menangani ungkapan dan kesalahan harfiah pada cuplikan Chronicle of Runes bab 54–55: `am I to take it that`, `quick on the uptake`, `keep one another in check`, `strip away everything he holds dear`, `pull stunt this a whole year later`, `Grand Duke`, `felled by a mere spirit-race wench`, `in your favor`, `preemptive strike`, `nor any justification`, `convene the council`, `living in peace ... grand delusion`, dan `justification wouldn't matter in the first place`. Teks yang terbaca sebagai dua blok dapat disatukan hanya jika tata bahasa, jarak, warna, dan piksel pembatas cocok. Pola `then ami to take it` hanya memicu baca ulang OCR; huruf tidak diganti tanpa kandidat dari piksel. Contoh harfiah seperti mengambil benda, penyerapan air, menebang pohon, dan pencegahan medis menjadi pembanding.

## Pengujian

Sembilan suite JVM lulus pada sumber final, termasuk contoh keluaran yang dilaporkan dan pembanding. Log: `validation/jvm-0.3.5.log`. Build APK ARM dan lint lulus: `:app:assembleDebug :app:lintDebug`. Log: `validation/build-arm-0.3.5.log`.

Uji Android `ContextAssemblyTest` lulus 5/5 pada emulator, termasuk penyatuan pertanyaan lintas blok, batas jarak, dan garis pemisah balon. Log: `validation/android-context-0.3.5.log`. Ini memeriksa alur OCR dan geometri, bukan hasil model terjemahan asli.

APK pengguna diperiksa langsung: `com.komiklingo.app`, versionName `0.3.5`/versionCode `19`, minSDK26/targetSDK35, ARM64 dan ARMv7. Sertifikat SHA-256 tetap `bb1ef448fb34777d2d03a49e7bfdb2bfb355ba7d57fe7b6a11f410d020e346a1`, sehingga dapat dipasang sebagai pembaruan. DEX memuat aturan baru; kelas fixture dan x86 tidak masuk APK pengguna. Log: `validation/apk-0.3.5.log`.

Penyisiran seluruh tes Android tidak menjadi bukti kelulusan: `BrowserColdStartTest.verify` memang dirancang berjalan di proses terpisah setelah `seed`; dalam penyisiran tunggal ia gagal. Tes capture layar mengambang melaporkan `NO_TEXT`, lalu proses emulator mengalami crash pada percobaan terpisah. Log mentah ada di `validation/android-full-0.3.5.log` dan `validation/android-floating-0.3.5.log`. Tidak ada perubahan pada browser, MediaProjection, izin, atau layanan layar mengambang dalam versi ini.

## Batas

Mesin dasar masih ML Kit. Uji bahasa memakai keluaran deterministik; inferensi model terjemahan asli belum terverifikasi karena unduhan model pada emulator sebelumnya melewati 180 detik. Aturan tidak menjamin semua dialog, OCR dekoratif, nama, atau kalimat terpotong benar. Belum diuji pada HP pengguna maupun Android14/15. APK masih bertanda tangan pengembangan, bukan rilis Play Store.

Identitas aplikasi, sertifikat, izin, dan dependensi tidak berubah. Sumber/artefak tidak menyertakan gambar pengguna, keystore, CA sesi, atau kredensial.
