# Pemeriksaan TCD 0.3.2

## Ruang lingkup

ConversationMeaning menambah aturan pragmatik yang dapat dipakai ulang, bukan mengganti hasil satu panel penuh: dugaan must + be + kondisi tertentu versus kewajiban dengan batas perintah/waktu; permintaan maaf, balasan not at all menurut sumber sebelumnya; emphatic ourselves versus by ourselves; rambling versus berjalan di hutan; tokoh penting saat berkumpul versus data angka/benda; hold a doctrine/belief versus memegang benda; heretics sebagai penganut ajaran; preman jalanan; pertanyaan klarifikasi happen to mean; pertumbuhan kelompok; shielding/melindungi; hegemonic nation; seruan good grief dan mind-boggling; tag akhir that is sebagai penjelasan; grave/solemn matter, discussion regarding, fate in the balance, slacking off dan race pada konteks biologis; vocabulary/sophisticated; perilaku frivolously; sugarcoat it; this instant; had him … subdued; wrap this up.

Klausa relatif objek terbatas diperjelas menjadi being created/made/built/developed/produced by + pelaku yang teridentifikasi. Fallback target yang sebelumnya membalik pelaku/objek dibetulkan hanya dengan jangkar marker nama. Penanda nama tetap divalidasi sebelum dipulihkan. Disambiguasi nama mencakup slot Kingdom of …, nama kelompok dan label kutipan sebagai kata benda klausa relatif. Ini heuristik struktural dan masih dapat salah pada nama/kalimat ambigu; bukan model NER atau AI baru.

TranslationFlow menerima sumber sebelum dan sesudah, termasuk pada batch fallback dan Reset satu bagian. Pembaca mengambil tetangga baru/tersimpan yang paling dekat di kolom sama; aturan jarak 65% viewport/overlap horizontal 30% yang sudah ada dipertahankan. Sumber sebelum kini masuk kunci cache sehingga balasan konteks permintaan maaf tidak dipakai untuk konteks pertanyaan lain. Target tersimpan tidak dipakai sebagai sumber konteks dan hasil lain tidak diterjemahkan ulang.

OcrAppearance memberi toleransi variasi latar gelap hanya untuk sambungan sintaksis hold … radical + doctrine/belief yang kuat, dengan batas posisi/ukuran/warna tinta serta piksel garis pemisah. Toleransi tidak diberlakukan ke semua paragraf. OcrQuality menandai pola salah baca ?/7, …P!, nation op/pate op the continent slacking ope dan wait po the me untuk mencoba kandidat OCR independen di bawah batas percobaan yang sama. Tidak ada substitusi P/7/OP ke tanda tanya/OF tanpa bukti kandidat.

## Pengujian

Sembilan suite JVM lulus pada sumber final: ReaderRules, NameRules, TranslationFlow, OcrQuality, ContextMeaning, PhrasalVerbs, BrowserRules, DialogueMeaning dan ConversationMeaning. Kasus baru memakai sumber Inggris dan contoh keluaran salah, berikut variasi/pengecualian serta nama yang dimask. Diuji pembedaan dugaan/kewajiban, balasan permintaan maaf/terima kasih/negasi, sendiri/penekanan, angka/orang, keyakinan/benda, kesedihan/seruan, ras/perlombaan, arah pelaku-objek, idiom tugas, menundukkan orang dan literal makanan/teknologi/kemasan. Batch yang gagal dibagi, fallback individual dan penggantian konteks cache ikut diperiksa. Log: jvm-0.3.2.log.

Dua belas tes Android lulus pada tahap konteks/geometri: DialogueContextTest4, RetainedTranslationTest3, ContextAssemblyTest4 dan MaskRegressionTest1. Ini menguji sumber sebelumnya yang tersimpan saat satu hasil di-Reset, sumber berikutnya, pemilihan tetangga/kolom, sambungan hold … radical + doctrine berlatar gelap dan batas garisnya; cache saat scroll; Reset yang berhasil/gagal; refresh/next/back; drag; penyatuan OCR ML Kit nyata pada gambar sintetis; dan penutup tetangga. Log: native-context-baseline-0.3.2.log. Setelah aturan makna/istilah terakhir ditambahkan, lima DialogueContextTest lulus pada APK uji final, termasuk pemeriksaan arah klausa relatif dan nama sekte. Log: native-final-0.3.2.log. Emulator Android11/API30 x86_64 dengan rendering software. Engine terjemahan dalam fixture deterministik; tidak ada klaim bahwa tes ini menjalankan model terjemahan ML Kit asli.

Build ARM dan lint final lulus: :app:assembleDebug :app:lintDebug. versionName0.3.2/versionCode16, applicationId com.komiklingo.app, minSDK26/targetSDK35, ARM64/ARMv7. Sertifikat SHA-256 tetap bb1ef448fb34777d2d03a49e7bfdb2bfb355ba7d57fe7b6a11f410d020e346a1. ZIP/DEX memastikan kelas bahasa/browser/floating ada dan kelas fixture/tes serta x86 tidak masuk APK pengguna. DEX juga diperiksa berisi keluarga aturan terakhir. Log build-arm-0.3.2.log, build-test-0.3.2.log dan apk-0.3.2.log.

Catatan lama ada dalam validation/functional-0.3.1.md dan functional-0.3.0.md. Tidak ada gambar lampiran pengguna yang dimasukkan ke sumber atau APK.

## Batas

Mesin dasar masih ML Kit. Ini peningkatan aturan konteks dan alur bahasa, bukan pemahaman seluruh cerita/gambar, sarkasme atau semua idiom. Kalimat tanpa petunjuk cukup, nama salah baca, hubungan tokoh, register, kata benda/kerja ambigu dan tanda baca masih dapat salah. Tidak ada klaim semua terjemahan akurat, seluruh teks terbaca atau perubahan kecepatan/kuota yang diukur.

Model asli belum diuji hingga inferensi di emulator: unduhan model pada percobaan historis melewati batas 180 detik. Tes alur menggunakan engine deterministik; uji OCR/geometri menggunakan komponen Android nyata. Keduanya tidak disamakan dengan evaluasi mutu model asli. Belum diuji di HP pengguna atau runtime Android14/15. MediaProjection penuh, server AI, browser/filter seluruh situs tidak diuji ulang; fungsi itu tidak diubah. APK debug, belum rilis Play Store.

Identitas aplikasi dan sertifikat dipertahankan. Tidak ada izin/dependensi/provider baru atau pemintas blokir ISP. Publikasi terbatas pada 10 artefak TCD/alias KomikLingo di branch unduhan sebelumnya, tanpa mengubah main atau Dualworld. Keystore, cache build, local.properties, CA sesi dan gambar pengguna tidak disertakan.
