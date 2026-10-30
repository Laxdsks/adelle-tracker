# Pemeriksaan TCD 0.3.1

## Perubahan bahasa

Ditambahkan DialogueMeaning dengan petunjuk makna yang terikat sumber. Collection/gathering/retrieval/recovery of bodies diperjelas sebagai gathering of dead bodies hanya bila ada petunjuk pemakaman pada sumber atau klausa tujuan sesudahnya. Keluaran koleksi/pengoleksian tubuh/badan dibetulkan menjadi pengumpulan jenazah; bentuk tubuh/badan yang terikat konteks itu menjadi jenazah. Arti tubuh hidup, korban luka, badan mobil/robot/binatang, benda langit, body of evidence/water/work dan nama terlindungi tidak diubah. Sumber dengan beberapa penyebutan body berbeda dibiarkan agar tidak menukar semua arti sekaligus.

Urutan nama marker + Kingdom/Empire/City/Village pada sumber menjadi kerajaan/kekaisaran/kota/desa + nama pada hasil jika mesin membaliknya. Nama tetap dipulihkan dari marker aslinya; tidak hardcode Bahain atau satu terjemahan panel. Nama baru masih bergantung kualitas OCR dan penemuan nama lama yang sudah ada.

TranslationFlow memakai klarifikasi yang sama pada batch, pengulangan per unit, fallback fragmen nama dan pembacaan ulang satu bagian. Kunci cache mencakup petunjuk sesudahnya. Pembaca situs/file mengambil petunjuk dari paragraf baru atau sumber tersimpan yang terdekat di kolom yang sama; jarak maksimum 65% tinggi viewport, overlap horizontal minimal 30%, tidak melintasi halaman lain. Keluaran tersimpan lain tidak diterjemahkan ulang. Mode teks dan layar memakai unit sesudahnya pada proses yang sama. Teks sumber tetap terpisah dari hasil dan tidak ditulis ulang di situs.

## Pemeriksaan

Delapan suite JVM lulus: ReaderRules, NameRules, TranslationFlow, OcrQuality, ContextMeaning, PhrasalVerbs, BrowserRules dan DialogueMeaning. Kasus baru meliputi contoh dua bagian pengguna, sumber yang menyatu, fallback batch, fallback fragmen nama, Reset satu bagian, penggantian konteks pada cache, berbagai jenis nama tempat dan pengecualian arti body. Log: validation/jvm-0.3.1.log.

Lima tes Android lulus pada emulator Android11/API30 x86_64: DialogueContextTest2 dan RetainedTranslationTest3. Tes baru memastikan kolom/jarak membatasi petunjuk, koordinat CSS tetap benar setelah perubahan skala/posisi, clear dokumen menghapus petunjuk, serta jalur MainActivity.translateGrouped mengambil sumber tersimpan saat satu bagian diterjemahkan ulang dan mempertahankan hasil lain. Mesin terjemahan fixture deterministik; model bahasa nyata tidak diuji oleh ini. Uji yang dipertahankan menjalankan WebView/OCR sebenarnya, scroll/cache, Reset terpilih, kegagalan Reset, refresh/next/back, penanganan drag dan penutup yang bertumpuk. Log: validation/native-0.3.1.log.

Build ARM dan lint lulus (:app:assembleDebug :app:lintDebug). versionName0.3.1/versionCode15, paket com.komiklingo.app, ARM64/ARMv7, minSDK26/targetSDK35. Sertifikat SHA-256 tetap bb1ef448fb34777d2d03a49e7bfdb2bfb355ba7d57fe7b6a11f410d020e346a1. Pemeriksaan ZIP/DEX memastikan kelas semantik dan browser masuk, sedangkan fixture, tes dan x86 tidak masuk APK pengguna. Log build-arm-0.3.1.log, build-test-0.3.1.log dan apk-0.3.1.log disertakan.

Catatan 0.3.0 disimpan terpisah dalam validation/functional-0.3.0.md. Tes browser/overlay menyeluruh dari rilis itu tidak diulang karena perubahan ini terbatas pada arti dan jalur konteks/cache. MediaProjection penuh, server AI serta model ML Kit asli tidak diuji ulang. Sumber/artefak tidak memuat keystore, local.properties, cache build atau gambar pengguna. Publikasi hanya memperbarui 10 artefak TCD/alias KomikLingo pada branch unduhan yang sudah digunakan, tanpa mengubah main atau sumber Dualworld.

## Batas kualitas

Mesin gratis masih ML Kit yang sama. Ini perbaikan aturan bahasa dan penyaluran petunjuk, bukan pengganti dengan AI pemahaman seluruh percakapan. Model asli belum berhasil diuji hingga inferensi pada emulator: percobaan historis unduhan model melewati batas 180 detik. Uji alur dengan mesin deterministik tidak dianggap sebagai bukti ketepatan semua hasil ML Kit. Penafsiran kata lain, rujukan tokoh, font sulit dan hasil OCR masih bisa keliru. Tidak ada janji semua kalimat benar, persentase akurasi atau peningkatan kecepatan yang diukur.

Browser, tab/bookmark, penyaring lokal, mode teks/file/layar, cache saat scroll, Reset, gaya, nama, overflow dan opsi AI sebelumnya dipertahankan. Tidak ada dependensi, izin, API berbayar atau model unduhan baru. Tidak ada perubahan keamanan jaringan atau pemintas blokir ISP. Belum diuji pada HP pengguna atau runtime Android14/15. APK bertanda tangan pengembangan, bukan rilis Play Store.
