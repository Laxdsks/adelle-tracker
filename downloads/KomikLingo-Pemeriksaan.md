# Pemeriksaan TCD 0.1.7

- `:app:assembleDebug :app:lintDebug` berhasil. APK pengguna: `com.komiklingo.app`, TCD, versionCode 8 / versionName 0.1.7, minSDK26 / targetSDK35, ARM64 dan ARMv7. APK pengguna tidak berisi activity/service fixture atau ABI x86.
- `apksigner verify --verbose --print-certs` berhasil. SHA256 sertifikat tetap `bb1ef448fb34777d2d03a49e7bfdb2bfb355ba7d57fe7b6a11f410d020e346a1`, sama dengan APK sebelumnya. Dapat dipasang sebagai pembaruan tanpa sengaja mengganti paket/penyimpanan.
- JVM: aturan teks, nama/istilah, TranslationFlow dan kualitas OCR lulus. Kasus tambahan memeriksa pemicu baca ulang 60/PO, konflik angka-kata GO/60, serta larangan koreksi global. Server Python: dua pengujian kontrak/autentikasi/isolasi konteks lulus, memakai Ollama tiruan.

## Android: halaman, navigasi dan penangkapan

`validation/page-navigation-0.1.7.log`: **6 tes lulus** pada emulator Android 11 / API30 x86_64 dengan rendering perangkat lunak.

- Pemindaian tiga bagian dalam satu dokumen melalui WebView/PixelCopy/OCR nyata mencapai teks pertama dan terakhir, tidak mengikuti tautan chapter berikutnya, kembali ke posisi awal dan menyimpan hasil di RAM.
- Scroll dan zoom mempertahankan koordinat/gaya/hasil; kotak tumpang tindih tidak digandakan. Fragment URL tidak membersihkan dokumen yang sama. Refresh, next dan back membersihkan hasil; pembacaan halaman sebelumnya perlu diulang.
- Beranda, Lanjutkan bacaan, riwayat bab, posisi gulir dan callback URL lama tetap berjalan.
- Pengambilan piksel WebView berakselerasi grafis diuji pada viewport pertama dan sesudah scroll; area header/footer tidak ikut dibaca, view yang tidak terlihat ditolak.

Fixture halaman mengganti terjemahan ML Kit dengan keluaran deterministik; yang diuji nyata adalah browser, perpindahan viewport, OCR, koordinat dan siklus cache. Ini bukan pengukuran kualitas bahasa model terjemahan. Deteksi akhir halaman memakai ukuran/posisi DOM dokumen aktif. Situs dengan pembaca di scroll-container khusus, canvas/iframe, konten tanpa akhir atau pergeseran layout masih dapat membatasi pemindaian.

## Android: huruf dan tata letak

Enam tes TypographyTest lulus pada siklus pembaruan ini, termasuk **OCR nyata GO! dan angka 60 pada gambar huruf miring tebal besar**: GO terbaca sebagai GO dan 60 tetap 60. Pembacaan normalisasi tidak boleh mengubah angka menjadi kata yang bentuknya mirip tanpa pengenal independen. Tes lain mencakup teks berwarna merah, penyambungan kata terpenggal, paragraf, susunan teks besar, batas keterbacaan dan pemilihan blok OCR yang lebih baik. Log `typography-and-initial-page-0.1.7.log` juga memuat kegagalan awal pemindaian halaman karena waktu emulator; setelah pembesaran viewport dibatasi 1,5×, pengujian halaman lulus pada `page-native-0.1.7.log` dan diulang bersama navigasi pada log enam tes final di atas.

Estimasi warna/kemiringan berlangsung pada worker, bukan loop UI; tabel luminansi dan cache layout mengurangi pekerjaan ulang. Pembacaan ulang tetap dibatasi pada area meragukan. Hasil ini tidak menjamin semua font dekoratif, nama atau kalimat sudah benar; model dasar tetap ML Kit Inggris–Indonesia.

## Android: balon TCD

`validation/floating-0.1.7.log`: tes sesi MediaProjection **lulus**, meliputi persetujuan sistem Android, pembacaan layar activity dari paket/UID lain, OCR nyata, tampilan hasil, pelepasan bitmap saat Tutup, tangkap ulang setelah rotasi dan penghentian dari aksi notifikasi. Terjemahan pada fixture diganti keluaran deterministik; jalur produksi memakai ML Kit lokal dan tidak menggunakan server AI pembaca internal.

Tes pembatalan persetujuan dan dekoder ImageReader RGBA juga lulus pada siklus ini; log awal lengkap tersedia di `floating-initial-0.1.7.log` (tes sesi penuh pada run awal gagal karena fixture peluncuran/keadaan SystemUI, lalu diperbaiki dan lulus terpisah). Dekoder memeriksa warna merah/biru dan dimensi agar padding baris tidak menjadi bagian gambar. Emulator perangkat lunak sempat memiliki dialog SystemUI ANR yang menutupi aplikasi uji. Untuk mengisolasi handler balon, gesture diberikan langsung ke View overlay nyata, sedangkan dialog persetujuan, ImageReader, OCR dan aplikasi sumber tetap nyata. Ini bukan pengujian end-to-end sentuhan fisik di HP pengguna.

Tidak ada penyimpanan cuplikan/teks layar aplikasi lain ke disk atau pengiriman ke AI. Permukaan tangkap dilepas saat tidak diminta; sesi hanya dimulai melalui izin pengguna, tidak dipulihkan otomatis, dan berhenti saat layar dikunci. Manifest memakai overlay/foreground-media-projection/notifikasi, tanpa aksesibilitas, mikrofon atau kamera. Komponen izin/service tidak diekspor.

## Batas dan artefak

Belum diuji pada HP fisik pengguna atau runtime Android 14/15. Cabang API34/35 dibangun dan lint diperiksa, tetapi izin/layanan di versi tersebut masih perlu pengujian perangkat. Kecepatan pada emulator perangkat lunak bukan ukuran kecepatan HP; tidak ada klaim waktu terjemahan atau akurasi 100%. Mode 1 halaman lebih lama daripada satu layar dan tidak mengunduh chapter lain. Cache hanya untuk dokumen aktif, bukan penyimpanan terjemahan lintas navigasi. Foto dilindungi Android tidak dapat dibaca.

APK utama, checksum, sumber dan panduan TCD disiapkan bersama alias KomikLingo yang byte-identik (sidecar checksum menyesuaikan nama file). Sumber tidak menyertakan keystore, local.properties, build, model terjemahan unduhan atau gambar komik pengguna. APK bertanda tangan pengembangan, bukan rilis Play Store.
