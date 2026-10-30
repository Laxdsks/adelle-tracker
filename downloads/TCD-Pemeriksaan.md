# Pemeriksaan TCD 0.3.8

Perubahan umum untuk mode offline: perlindungan otomatis nama sesudah Mr./Mrs./Ms./Dr. pada semua jalur yang menggunakan ProtectedTerms; penyaringan pemberitahuan penerbit dan domain tunggal dalam pemindaian browser/layar mengambang; percobaan ulang terjemahan dengan jendela dialog tetangga yang dibatasi panjangnya. Hasil dipilih hanya untuk balon sasaran setelah jumlah paragraf dan kepemilikan penanda nama diperiksa. Jendela yang sudah gagal tidak diulang identik. Jika model tetap menyatukan hasil, terjemahan tunggal menjadi cadangan.

Sembilan suite JVM lulus, termasuk nama Falling/White/Rose, pengecualian Mr. President, pemberitahuan penerbit versus kalimat percakapan, pemulihan kelompok empat balon melalui jendela konteks, Reset satu balon dengan dialog tetangga, cache konteks, dan penanda nama. Log: validation/jvm-0.3.8.log.

Build ARM dan lint lulus: validation/build-arm-0.3.8.log. Metadata, ABI dan sertifikat diperiksa dalam validation/apk-0.3.8.log: com.komiklingo.app, versi 0.3.8 kode 22, Android 8+, ARM64/ARMv7. Sertifikat sama dengan versi sebelumnya.

Uji memakai mesin terjemahan deterministik untuk memeriksa alur. Model ML Kit asli dan hasil pada HP pengguna belum diuji pada versi ini. Tidak ada bukti bahwa seluruh dialog menjadi benar atau bahwa akurasi mencapai 100%. Model offline masih bisa salah memahami idiom, pelaku, nada, dan konteks yang tidak terlihat. Jendela konteks menambah pekerjaan saat percobaan ulang; ukuran dibatasi 4.000 karakter. Penyaringan watermark dibatasi pola teks, bukan pengenal semua watermark visual. Tidak ada model AI besar baru atau server yang diaktifkan.

Uji Android geometri pada versi 0.3.5 pernah lulus 5/5; ini bukan bukti untuk penyaringan baru versi 0.3.8. Uji capture versi sebelumnya mengalami NO_TEXT dan crash emulator. Log lama disertakan sebagai riwayat, bukan kelulusan versi ini. APK menggunakan sertifikat pengembangan.
