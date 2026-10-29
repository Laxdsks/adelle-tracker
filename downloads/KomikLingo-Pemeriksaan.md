# Pemeriksaan KomikLingo 0.1.2

- assembleDebug dan lintDebug: berhasil. Paket pengguna com.komiklingo.app, versionCode3/versionName0.1.2, minSDK26/targetSDK35, ARM64/ARMv7.
- Sertifikat debug sama dengan versi sebelumnya: SHA256 bb1ef448fb34777d2d03a49e7bfdb2bfb355ba7d57fe7b6a11f410d020e346a1. Perbarui tanpa menghapus aplikasi/data untuk mempertahankan model dan pengaturan.
- Tes JVM ReaderRulesTest lulus: sapaan aku/kamu/saya/Anda, batas kata dan kapitalisasi, pengecualian sapaan hormat, koreksi OCR terikat pola, nama/seruan yang tidak boleh diubah, validasi jumlah paragraf, tautan tanpa skema/tautan berbagi/dua domain dan pemisahan kunci cerita. Ditambah pengkodean kata kunci DuckDuckGo, pengenalan URL versus judul, tampilan kembali kata kunci dan penolakan skema berbahaya.
- Tes server Python lulus dengan model tiruan: autentikasi, batas input/catatan, gaya yang valid, pengiriman style/story_notes, pemisahan konteks bila gaya/catatan/dokumen berubah, dan penolakan jumlah hasil yang salah. Tidak menilai kualitas bahasa model AI sebenarnya.
- Uji instrumentasi Android 30 x86_64 ReadingFlowTest: OK (1 test). Membuka dua halaman fixture HTTPS dengan host berbeda melalui tombol Go kolom alamat aplikasi, memeriksa isi halaman dan gaya formal pada cerita pertama tidak terbawa ke cerita kedua. Uji terbaru juga mengetik judul lewat IME Search, memeriksa alamat DuckDuckGo dan kata kunci di kolom, lalu mengklik hasil menuju domain komik lain di WebView yang sama. Halaman disajikan oleh interceptor uji lokal; ini bukan bukti seluruh situs internet mengizinkan WebView.
- Jalur PixelCopy/OCR tidak berubah dari 0.1.1. Uji 0.1.1 sebelumnya OK (2 tests): OCR ML Kit asli membaca gambar WebView berakselerasi grafis sebelum/sesudah scroll, memotong header/footer, dan menangani view terlepas. Hasil lama disimpan terpisah untuk menjelaskan ruang lingkupnya.
- Activity/fixture uji tidak masuk APK pengguna; hanya disertakan dengan -PcaptureTest.

Batas: belum diuji pada HP pengguna, semua situs atau huruf komik. Belum ada tolok ukur kualitas terjemahan ML Kit berkelompok atau model Ollama asli pada cerita ini. Mode dasar memakai preferensi sapaan dan perbaikan OCR terbatas; tidak otomatis mengenali hubungan suami/istri atau memahami seluruh emosi/alur. Model AI lokal tetap memerlukan komputer/server yang disiapkan. Kesesuaian hasil bahasa perlu diperiksa pembaca.

Pemeriksaan DuckDuckGo dari jaringan cloud mendapat HTTP202 dengan halaman berjudul pencarian dan elemen verifikasi; bukan pengujian hasil pencarian bebas verifikasi pada HP. Tidak ada bypass proteksi situs.
