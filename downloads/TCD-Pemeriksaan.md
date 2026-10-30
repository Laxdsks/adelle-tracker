# Pemeriksaan TCD 0.3.3

## Perubahan

Versi ini menambah aturan bahasa yang hanya aktif ketika frasa Inggrisnya terbaca: `show some restraint` (menahan diri), `away from my post` (pos tugas), `stay here any longer` (lebih lama di sini), `dally around with that sly fox of an elf` (berlama-lama bersama Elf licik), perintah `hurry up and get lost` (cepat pergi), salam `see you around` (sampai jumpa lagi), serta susunan `crush them beneath my feet`. Perbaikan keluaran harfiah dibatasi oleh sumber, sehingga `restraints` fisik, `blog post`, rubah sungguhan, tersesat secara harfiah, dan pemakaian `see you around` dalam kalimat biasa tidak ikut diganti. `Are you leaving?` sudah benar secara makna dan dibiarkan.

## Pengujian

Sembilan suite JVM lulus pada sumber final, termasuk ConversationMeaningTest dengan contoh hasil yang dilaporkan serta pembanding literal di atas. Log: `validation/jvm-0.3.3.log`. Build ARM dan lint lulus: `:app:assembleDebug :app:lintDebug`. Log: `validation/build-arm-0.3.3.log`.

Paket yang dibagikan diperiksa langsung: `com.komiklingo.app`, versionName `0.3.3`/versionCode `17`, minSDK26/targetSDK35, ARM64 dan ARMv7. Sertifikat SHA-256 tetap `bb1ef448fb34777d2d03a49e7bfdb2bfb355ba7d57fe7b6a11f410d020e346a1`, sehingga APK bisa dipasang sebagai pembaruan aplikasi sebelumnya. DEX mengandung aturan baru, sedangkan kelas fixture dan x86 tidak masuk APK pengguna. Log: `validation/apk-0.3.3.log`.

Tes Android pada versi 0.3.2 yang menjadi dasar perubahan bahasa ini lulus 12 tes konteks/geometri dan 5 tes integrasi final. Tidak ada perubahan pada UI, OCR, perizinan, atau alur Android dalam 0.3.3; tes Android tidak dijalankan ulang untuk versi ini. Catatan rinci dan log dasar tersedia di `validation/functional-0.3.2.md` dan `validation/native-*-0.3.2.log`.

## Batas

Mesin dasar masih ML Kit. Uji bahasa dan fixture Android memakai mesin keluaran deterministik; inferensi model terjemahan asli belum terverifikasi karena unduhan model pada emulator pernah melewati 180 detik. Aturan frasa tidak menjamin memahami semua percakapan, gaya bahasa, OCR dekoratif atau nama yang salah terbaca. Belum diuji pada HP pengguna maupun Android14/15. APK masih bertanda tangan pengembangan, bukan rilis Play Store. Fungsi browser, filter, MediaProjection dan server AI tidak diubah dan tidak diuji ulang pada 0.3.3.

Identitas aplikasi, sertifikat, izin dan dependensi tidak berubah. Sumber/artefak tidak menyertakan gambar pengguna, keystore, CA sesi atau kredensial. Catatan pemeriksaan versi lama disimpan dalam `validation/functional-0.3.2.md`.
