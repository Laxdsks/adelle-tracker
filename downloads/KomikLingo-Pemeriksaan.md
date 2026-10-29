# Pemeriksaan KomikLingo 0.1.1

- assembleDebug dan lintDebug: berhasil. APK pengguna com.komiklingo.app, versionCode2/versionName0.1.1, minSDK26/targetSDK35, ARM64/ARMv7.
- Tanda tangan debug sama dengan 0.1.0: SHA256 sertifikat bb1ef448fb34777d2d03a49e7bfdb2bfb355ba7d57fe7b6a11f410d020e346a1. Data/model perangkat tidak perlu dihapus untuk memperbarui.
- Uji instrumentasi Android 30 x86_64, WebView dengan akselerasi grafis dan OCR ML Kit asli: OK (2 tests). Sumber berupa gambar fixture lokal dengan huruf bergaya casual; tidak memakai layanan OCR tiruan.
- Uji pertama memeriksa view terlepas menghasilkan kesalahan pengambilan gambar, bukan hasil OCR kosong palsu.
- Uji kedua memeriksa teks HAPPENS terbaca dari gambar WebView, header/footer aplikasi tidak masuk area OCR, kemudian teks REASON terbaca setelah scroll dan teks viewport sebelumnya tidak ikut terbaca. Pengambilan gambar menunggu WebView visual-state callback dan dua frame sebelum PixelCopy. Bitmap sebelum/sesudah diperiksa secara visual.
- APK pengguna tidak menyertakan CaptureFixtureActivity. Activity/fixture uji hanya dibangun dengan -PcaptureTest.
- Callback OCR gagal/kedaluwarsa kini melepas antrean otomatis. Pengambilan gambar memiliki batas waktu 8 detik. Membuka latar/rotasi membatalkan hasil yang sudah kedaluwarsa.
- Tes server 0.1.0 sebelumnya lulus: autentikasi, batas input, kontrak hasil dan isolasi konteks. Server tidak diubah pada perbaikan ini.

Batas: pengujian ini memverifikasi input gambar GPU ke OCR serta scroll di emulator. Belum diuji pada HP pengguna, seluruh situs Kayn Scans di Android, unduhan/keluaran model terjemahan asli dalam emulator, kualitas/emotional fidelity AI Ollama, semua jenis huruf atau kinerja baterai. Perubahan ini memperbaiki jalur pembacaan gambar; tidak membuktikan kualitas terjemahan sempurna.
