# Pemeriksaan TCD 0.1.8

## Build dan identitas APK

- `:app:assembleDebug :app:lintDebug` lulus. Paket `com.komiklingo.app`, label TCD, versionCode 9 / versionName 0.1.8, minSDK26 / targetSDK35, ABI ARM64 dan ARMv7.
- Tanda tangan diverifikasi dengan apksigner; sertifikat SHA256 tetap `bb1ef448fb34777d2d03a49e7bfdb2bfb355ba7d57fe7b6a11f410d020e346a1`, sama dengan versi sebelumnya. Bisa dipasang sebagai pembaruan. Manifest APK pengguna tidak memuat activity/service fixture. Keystore tidak disertakan dalam sumber.
- JVM: ReaderRulesTest, NameRulesTest, TranslationFlowTest dan OcrQualityTest lulus. Model terjemahan, backend Python dan aturan bahasa tidak diganti; uji server tidak diulang karena tidak ada perubahan server.

## Pengujian Android

`validation/retained-native-0.1.8.log`: **9 tes lulus**, emulator Android 11/API30 x86_64, rendering perangkat lunak. APK uji terpisah dari APK ARM pengguna.

1. Terjemahkan biasa pada dua paragraf WebView: hasil masuk cache dokumen tanpa menyalakan pemindaian 1 halaman. Hasil tetap ada sesudah scroll ke bawah/atas; kembali dan menekan Terjemahkan lagi tidak memanggil engine terjemahan ulang untuk viewport yang sudah selesai dibaca.
2. Tombol Reset kecil benar-benar ada di bawah hasil. Reset membaca crop terpilih dan hanya mengirim paragraf itu ke engine, menampilkan hasil baru/Reset ✓; objek hasil paragraf lain tetap tidak berubah. Percobaan gagal mempertahankan hasil lama. Refresh, next dan back membersihkan cache dokumen.
3. Cache memetakan posisi/ukuran pada zoom, memakai ulang ID saat kotak yang sama diperbarui dan tidak menumpuk kotak koreksi. Membersihkan hasil juga membersihkan coverage viewport.
4. Gesture drag yang dimulai pada Reset diteruskan untuk scroll tanpa memicu klik Reset; area overlay yang bukan tombol tidak menangkap gesture bacaan.
5. Uji piksel: kotak penutup tetangga yang tumpang tindih tidak menghapus ujung baris huruf yang sudah digambar. Sudut mask menutup latar sumber secara penuh. Penutup digambar dahulu, lalu semua teks, sehingga tidak ada penutup berikutnya yang menghapus teks hasil sebelumnya.
6. Pemindaian satu dokumen masih mencapai bagian pertama dan terakhir, kembali ke posisi semula dan tidak membuka chapter berikutnya. Cache/coverage/gaya bertahan saat gulir/zoom; refresh/next/back membersihkannya.
7. PixelCopy tetap membaca gambar WebView yang sudah dirender, sebelum/sesudah scroll, mengecualikan header/footer dan menolak view yang tidak terlihat.
8. Tata huruf tetap mempertahankan warna, ukuran besar, jarak paragraf dan fallback Hasil untuk kotak yang tidak muat pada ukuran terbaca.

Satu tes melakukan beberapa pemeriksaan pada daftar di atas. OCR, PixelCopy, WebView, cache, tombol dan gesture memakai komponen Android nyata. Engine terjemahan fixture sengaja diganti keluaran deterministik untuk menghitung pemanggilan dan membandingkan hasil; ini **bukan** bukti akurasi bahasa model ML Kit. Screenshot `retained-preview-0.1.8.png` adalah halaman buatan dengan engine tiruan (kata FIRST/SECOND/UPDATED merupakan label pengujian), bukan contoh kualitas terjemahan komik. Uji awal 3 tes juga disimpan sebagai log historis siklus ini.

## Perilaku hasil dan Reset

- Mode biasa/otomatis menyimpan hasil dan koordinat CSS di RAM sampai refresh/navigasi/dokumen baru/proses dihentikan. Pengaturan gaya/nama/server yang diubah membersihkan hasil lama agar konfigurasi baru dipakai. Riwayat URL/posisi tetap terpisah; terjemahan tidak diarsipkan ke disk.
- Wilayah viewport yang sudah dibaca selesai dicatat untuk menghindari OCR/terjemahan berulang saat kembali. Bagian baru dibaca bertahap. Batas cache 6.000 bagian; pencatatan viewport terbatas 800 wilayah.
- Reset membaca crop layar pada blok/paragraf OCR yang dipilih dan menghapus cache engine sementara agar permintaan dikirim lagi. Penimpaan hasil berlaku jika seluruh bagian yang diproses ulang berhasil; jika gagal, hasil sebelumnya dipertahankan. Menggulir/navigasi membatalkan callback yang sudah tidak cocok dengan posisi/dokumennya.
- Margin penutup sumber ditambah untuk huruf miring dan tepi glyph. Kotak yang hampir sama diganti, bukan ditumpuk. Semua mask digambar dahulu sebelum huruf. Footer Reset diletakkan di ruang yang tidak menutupi hasil lain; jika tidak muat, Reset bagian ini tersedia di panel Hasil. Tombol dapat diakses sebagai View Android, bukan hanya gambar pada Canvas.
- Fitur balon aplikasi lain tetap lokal dengan izin MediaProjection; komponen layout hasil dipakai bersama. Percobaan regresi sesi balon tambahan tidak dapat diselesaikan: runner melaporkan `INSTRUMENTATION_ABORTED: System has crashed`, dan host emulator melaporkan `bad color buffer handle 223`. Buffer crash juga menunjukkan `audioserver`/audio Ranchu emulator mengalami `TimeCheck timeout for IAudioFlinger command 1`, lalu runner gagal melepas UiTestAutomationService dan proses telepon melaporkan DeadSystemException. Ini bukan hasil lulus untuk uji balon. Log disimpan terpisah dalam `floating-overlay-0.1.8.log`; perilaku balon 0.1.8 masih memerlukan verifikasi di perangkat. Sembilan tes fitur pembaca di atas selesai dan lulus sebelum kejadian ini.

## Batas praktis

Belum diuji pada HP fisik pengguna atau runtime Android14/15. Kecepatan emulator perangkat lunak bukan ukuran kecepatan HP. Model dasar tetap ML Kit Inggris–Indonesia; tidak ada janji terjemahan selalu benar, pemahaman cerita penuh atau koreksi sempurna semua font. Reset dapat menghasilkan kalimat yang sama. OCR/posisi masih bisa keliru pada sumber pecah, tulisan dekoratif, latar gambar atau layout yang bergeser setelah dimuat. Mask memakai warna latar dominan, bukan inpainting ilustrasi di balik teks. Bagian yang terlalu panjang tetap memakai Hasil agar tidak dipaksa terlalu kecil.

APK/panduan/sumber TCD dan alias KomikLingo diperbarui pada branch unduhan yang sama, tanpa perubahan main atau aplikasi Dualworld. APK tetap bertanda tangan pengembangan, bukan rilis Play Store.
