# Pemeriksaan TCD 0.3.11

Kelompok terjemahan kini memakai dialog sebelum/sesudah kelompok jika tersedia, masing-masing maksimal 1200 karakter, total maksimal 4000. Bagian konteks dikeluarkan dari hasil tampilan; nama dan jumlah balon tetap divalidasi. Pengujian mesin tiruan memastikan konteks sampai ke mesin dan tidak muncul sebagai balon tambahan.

Kandidat OCR diperiksa terhadap kata negasi dan angka pada bacaan awal. Kandidat yang menghapus/menambah negasi atau mengubah jumlah ditolak, walaupun skor pengenalan lebih tinggi. Kontraksi don't dan do not diperlakukan setara. Pengujian meliputi penghapusan NOT, perubahan 70 menjadi 10, penambahan CANNOT, serta ordinal 7TH. Pemeriksaan ini konservatif: bila bacaan angka atau negasi awal sudah salah, koreksi otomatis dapat tertahan.

Sembilan suite JVM lulus: validation/jvm-0.3.11.log. Build ARM dan lint: validation/build-arm-0.3.11.log. Metadata dan sertifikat: validation/apk-0.3.11.log. Uji emulator versi 0.3.10 tetap tersedia sebagai bukti historis, tidak dijalankan ulang untuk versi ini. Belum ada pengujian menyeluruh pada HP pengguna atau penilaian manusia atas seluruh hasil mesin penerjemah.

Model offline tetap ML Kit, tanpa server/model AI besar baru. Konteks membantu menyediakan masukan yang lebih lengkap tetapi tidak menjamin mesin memahami cerita atau menghasilkan kalimat tepat 100%. Tidak ada penggantian nama atau tebakan ejaan berbasis kamus baru. Sertifikat sama dengan pembaruan sebelumnya.
