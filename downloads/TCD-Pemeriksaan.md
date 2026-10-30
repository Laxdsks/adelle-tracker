# Pemeriksaan TCD 0.3.4

## Perubahan

Balon `My apologies for the delay` kini dapat membetulkan keluaran harfiah “permintaan maaf aku atas ...” menjadi “Maaf atas ...”. `I had some urgent matters to attend to` diperjelas sebagai urusan mendesak yang harus ditangani. Pada sapaan kerajaan, `your majesty's ocean-like grace` berarti kemurahan hati Yang Mulia yang seluas lautan, dan `I was able to preserve my life` berarti nyawa pembicara selamat. Aturan hanya aktif jika petunjuk Inggris yang sesuai terbaca. Perawatan pasien (`attend to the patient`) dan pelestarian kehidupan laut tetap diperlakukan sesuai konteks masing-masing.

## Pengujian

Sembilan suite JVM lulus pada sumber final, termasuk contoh hasil yang dilaporkan dan pembanding literal. Log: `validation/jvm-0.3.4.log`. Build ARM dan lint lulus: `:app:assembleDebug :app:lintDebug`. Log: `validation/build-arm-0.3.4.log`.

APK pengguna diperiksa langsung: `com.komiklingo.app`, versionName `0.3.4`/versionCode `18`, minSDK26/targetSDK35, ARM64 dan ARMv7. Sertifikat SHA-256 tetap `bb1ef448fb34777d2d03a49e7bfdb2bfb355ba7d57fe7b6a11f410d020e346a1`, sehingga dapat dipasang sebagai pembaruan. DEX berisi aturan baru, sedangkan kelas fixture dan x86 tidak masuk APK pengguna. Log: `validation/apk-0.3.4.log`.

Perubahan versi ini hanya pada aturan bahasa. UI, OCR, izin dan alur Android tidak diubah. Tes Android tidak dijalankan ulang; basis sebelumnya lulus 12 tes konteks/geometri dan 5 tes integrasi versi 0.3.2. Rinciannya ada dalam `validation/functional-0.3.2.md`; pemeriksaan 0.3.3 tersimpan di `validation/functional-0.3.3.md`.

## Batas

Mesin dasar masih ML Kit. Uji bahasa memakai contoh keluaran deterministik; inferensi model terjemahan asli belum terverifikasi karena unduhan model pada emulator pernah melewati 180 detik. Aturan ini tidak menjamin memahami semua dialog, OCR dekoratif, nama atau kalimat terpotong. Belum diuji pada HP pengguna maupun Android14/15. APK masih bertanda tangan pengembangan, bukan rilis Play Store. Fungsi browser, filter, MediaProjection dan server AI tidak diubah atau diuji ulang pada 0.3.4.

Identitas aplikasi, sertifikat, izin dan dependensi tidak berubah. Sumber/artefak tidak menyertakan gambar pengguna, keystore, CA sesi atau kredensial.
