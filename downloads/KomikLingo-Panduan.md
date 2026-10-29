# TCD 0.1.8 — pembaca terjemahan Android

Aplikasi terpisah dari Dualworld. Cari judul lewat DuckDuckGo, buka tautan komik/novel HTTPS atau pilih gambar, PDF dan TXT. Mode otomatis aktif saat awal: setelah gulir berhenti ±1 detik, aplikasi mengenali teks yang terlihat dan menampilkan Indonesia di area teks. Terjemahan biasa maupun mode 1 halaman menyimpan hasil di RAM dan menampilkannya kembali pada posisi yang sama saat digulir. `Lihat asli` membandingkan gambar asli; `Hasil` membuka terjemahan penuh. Ukuran font menyesuaikan kotak OCR; kotak yang terlalu kecil mengarahkan ke Hasil. File asli tidak ditulis ulang.

## Memakai APK

Pasang `dist/TCD-Android.apk` pada Android 8+. Unduh model Inggris–Indonesia sekali melalui internet (menu ⋮ → Unduh model). OCR Latin sudah dibundel; file dapat diterjemahkan tanpa internet setelah model tersedia. Situs komik tetap membutuhkan internet. File: JPG/PNG/WebP, PDF tanpa kata sandi dan TXT UTF-8. EPUB/DOCX/CBZ belum didukung.

Tautan contoh Kayn Scans telah merespons HTTP 200 saat diperiksa; itu tidak membuktikan pembaca WebView akan selalu diizinkan oleh situs. Pemeriksaan/iklan/login situs tetap berlaku. Jika gambar belum muncul, tunggu lalu gulir sedikit atau tekan Terjemahkan. Aplikasi tidak melewati pembatasan akses situs.

## Dua pilihan gratis, dengan batas yang berbeda

* **Dasar di HP:** ML Kit English → Indonesian; tidak memerlukan API key atau langganan. Dapat salah membaca huruf komik bergaya, susunan balon atau kalimat yang terpotong. Terjemahannya bisa kaku dan belum memahami seluruh cerita. Membuka menu “AI” tidak membuat model dasar menjadi AI berkonteks.
* **AI lokal di komputer:** Ollama dan model terbuka Qwen2.5 7B. Tidak ada biaya API/langganan; membutuhkan komputer sendiri (disarankan RAM 16 GB), unduhan model sekitar beberapa GB, listrik dan internet untuk unduhan. Kecepatan mengikuti komputer. Sekumpulan dialog dan maksimal 12 pasangan dialog sebelumnya dikirim bersama agar gaya/emosi dan nama konsisten. Model tetap dapat salah dan kualitas bahasa perlu dinilai pembaca. Ini bukan jaminan terjemahan profesional.

Mode AI belum aktif pada APK karena komputer/server pengguna belum disiapkan. Tidak ada layanan gratis milik pembuat aplikasi yang diam-diam memproses gambar atau teks.

## Menyalakan AI lokal tanpa domain atau billing

1. Di komputer, pasang Ollama dari https://ollama.com/ lalu jalankan `ollama pull qwen2.5:7b`. Pastikan layanan Ollama aktif.
2. Jalankan `python3 server/reader_server.py`. Simpan kode akses yang tampil untuk dimasukkan di HP. Server hanya mendengar 127.0.0.1.
3. Pasang Android SDK Platform Tools di komputer, sambungkan HP dengan USB, aktifkan USB debugging dan izinkan komputer. Jalankan `adb reverse tcp:8765 tcp:8765`.
4. Di aplikasi: ⋮ → Pengaturan terjemahan AI. Alamat `http://127.0.0.1:8765/translate`, kode akses dari langkah 2. Ini melewati kabel USB, bukan internet publik. Komputer harus tetap menyala dan USB tersambung.
5. Kosongkan alamat AI untuk kembali ke mode dasar. Jika memakai Wi-Fi/internet, pemilik server harus menyiapkan HTTPS dan kontrol akses; aplikasi hanya mengizinkan HTTP untuk localhost.

Server Python menggunakan pustaka standar, tanpa kunci layanan berbayar. `OLLAMA_MODEL` mengubah model lokal. `READER_TOKEN` dapat menetapkan kode akses privat yang tetap. Server menolak akses tanpa kode, membatasi ukuran/antrean, memeriksa jumlah hasil dan memisahkan konteks per dokumen. Teks tidak dicatat ke log atau disk. Konteks dibuang setelah 30 menit; batas 32 dokumen di memori. Untuk pemakaian pribadi; perlu penguatan operasional sebelum dijadikan layanan publik.

## Build dan pemeriksaan

Proyek Android Gradle standar: AGP 8.7.3, Gradle 8.9, JDK 17, compile/target SDK35, build tools35.0.0. Buka folder ini di Android Studio atau jalankan `./gradlew assembleDebug` setelah mengatur SDK pada `local.properties`. `scripts/build.sh` khusus lingkungan cloud /workspace, menggunakan proxy dan CA session. Keystore debug privat tidak masuk arsip.

Pengujian server: `cd server && python3 -m unittest -v test_reader`. Tes integrasi memakai server Ollama tiruan untuk memeriksa autentikasi, batas input, kontrak hasil dan isolasi konteks; tidak mengukur kualitas model asli. Build APK dan manifest/tanda tangan diperiksa. Sensor/performa/OCR/unduh model dan overlay belum diuji pada HP fisik. APK ditandatangani untuk pengembangan dan belum diterbitkan di Play Store.

## Batas tampilan

Overlay menutup persegi teks hasil OCR dengan warna latar dominan, bukan mengembalikan lukisan di belakang huruf. Cocok terutama untuk balon warna polos. Efek suara, huruf lengkung, tulisan tangan, teks di tepi layar, font dekoratif dan latar bergambar bisa salah. Baca Hasil atau perbesar gambar bila perlu. Tidak ada kamera. Fitur opsional Balon TCD membaca layar aplikasi lain setelah izin Android dan ketukan pengguna.

Teks kecil: cubit/perbesar situs, gambar atau halaman PDF sebelum diterjemahkan. OCR juga menerima tangkapan viewport dengan pembesaran terbatas 1,5× dan batas memori. Area sulit dapat dibesarkan/dinormalisasi secara terpisah. Pembesaran tidak dapat memulihkan huruf yang sudah pecah atau tidak memiliki detail. Gaya otomatis mempertahankan kapitalisasi dan memakai huruf tebal untuk sumber kapital, serta font yang mudah dibaca. Kemiringan, warna dominan, kapitalisasi dan ukuran diperkirakan dari gambar. Font artistik/bentuk huruf asli tidak disalin persis. Paket APK mendukung HP ARM64 dan ARMv7, bukan emulator x86.


## Perbaikan 0.1.1

Pengambilan gambar OCR memakai PixelCopy pada area pembaca di jendela aplikasi. Ini mengambil piksel WebView yang sudah dirender, termasuk gambar yang tampil lewat GPU, setelah lapisan terjemahan dibersihkan selama dua frame. Menggambar ulang WebView langsung ke Canvas tidak lagi dipakai. Kesalahan mengambil gambar dibedakan dari kegagalan mengenali huruf. Pembacaan otomatis dibersihkan saat aplikasi masuk latar/rotasi; callback gagal yang sudah kedaluwarsa juga melepas antrean agar pembaca tidak macet.

Uji regresi Android tersedia di `app/src/captureTests`: `./gradlew -PcaptureTest connectedDebugAndroidTest` pada emulator x86_64. Fixture gambar dan activity uji hanya disertakan jika properti `captureTest` aktif; tidak masuk APK pengguna. Uji memeriksa OCR Latin asli pada gambar WebView dengan akselerasi grafis, pemotongan area header/footer dan perpindahan viewport setelah scroll. Itu tidak menilai kualitas model terjemahan atau perilaku semua situs.


## Membuka situs lain dan gaya dialog (0.1.2)

Tekan × pada kolom alamat lalu tempel URL, tekan Cari/tombol pencarian keyboard, atau pilih ⋮ → Cari komik / buka situs. Alamat tanpa skema otomatis memakai HTTPS. Bagikan tautan dari browser ke TCD juga didukung; tidak ada daftar domain komik yang dikunci ke Kayn Scans. Situs dengan pembatasan WebView, login atau perlindungan akses tetap mengikuti aturannya sendiri.

Gaya bawaan **Dialog akrab (aku–kamu)** menyesuaikan sapaan mode dasar. Pilihan **Netral (saya–kamu)** dan **Formal (saya–Anda)** tersedia di ⋮ → Gaya bahasa cerita. Pilihan disimpan per situs/cerita (pola path chapter/episode dikelompokkan), sehingga dua situs/cerita tidak otomatis berbagi pilihan. Sapaan hormat yang terlihat jelas seperti Sir/Your Majesty mempertahankan register formal di mode dasar. Ini preferensi gaya dengan aturan terbatas, bukan pendeteksian suami/istri atau pemahaman hubungan tokoh dari gambar.

OCR membersihkan beberapa kesalahan terikat pola: kata penghubung menyatu dengan I sebelum verba, huruf akhir ganda pada kosakata umum tertentu, dan awalan F salah pada everyone/everything. Tidak ada penggantian kalimat komik secara hafalan. Nama seperti Andi dan seruan seperti nooo dipertahankan oleh uji regresi. Aturan tetap bisa salah pada nama/teks yang tidak lazim.

Pada versi 0.1.2, untuk 2–6 blok sampai 5.000 karakter, mode dasar mencoba menerjemahkan paragraf bersama. Hasil hanya dipakai bila jumlah paragraf cocok; bila tidak, tiap blok diterjemahkan ulang agar posisi/urutan tidak tercampur. Hasil menampilkan pasangan Inggris yang sudah dibersihkan dan Indonesia. Mengelompokkan paragraf serta menyesuaikan sapaan tidak menjadikan model dasar memahami seluruh cerita atau menjamin semua idiom benar.

AI lokal opsional menerima gaya bahasa serta Catatan cerita (maksimum 800 karakter pada aplikasi), misalnya hubungan pasangan atau pilihan sapaan tokoh. Server membawa konteks beberapa dialog sebelumnya dan memisahkannya bila dokumen, gaya atau catatan berubah. AI belum otomatis aktif tanpa komputer/server yang disiapkan. Kualitas keluaran model asli tetap perlu dinilai pembaca.

Pemeriksaan aturan: `bash tests/run.sh` dengan JDK17+. Tes server: `cd server && python3 -m unittest -v test_reader`. Uji UI Android dengan `-PcaptureTest` memakai halaman fixture dua domain HTTPS, bukan bukti akses seluruh situs nyata.


## Pencarian DuckDuckGo (termasuk dalam 0.1.2)

Kolom atas menerima **nama komik/kata kunci atau tautan**. Nama seperti “Chronicle of Runes komik” membuka pencarian DuckDuckGo; domain seperti `nama-situs.com/bab/1` langsung dibuka melalui HTTPS. Tekan Cari atau tombol pencarian keyboard. DuckDuckGo menjadi halaman awal untuk instalasi baru; instalasi lama tetap melanjutkan tautan terakhir. Pilihan menu Cari komik / buka situs menyediakan kolom kosong agar lebih mudah mengganti pencarian.

Hasil pencarian ditampilkan sebagai situs DuckDuckGo di dalam WebView, dengan tautan hasil tetap di pembaca. Aplikasi tidak menggunakan API pencarian berbayar, tidak membuat indeks sendiri dan tidak menyediakan saran alamat secara native. Terjemahan otomatis ditahan pada halaman pencarian supaya tidak menutup tombol/hasil; setelah masuk situs komik, pembacaan otomatis melanjutkan pilihan sebelumnya. Tombol Terjemahkan manual tetap dapat digunakan pada hasil pencarian.

Kata kunci dikirim ke DuckDuckGo sesuai tindakan pencarian pengguna. DuckDuckGo/situs tujuan dapat menampilkan verifikasi atau menolak WebView sesuai ketentuan mereka; aplikasi tidak melewati pembatasan itu. Pemeriksaan dari jaringan cloud menerima HTTP202 berjudul pencarian DuckDuckGo dengan elemen verifikasi, sehingga bukan bukti hasil pencarian langsung selalu tersedia di semua jaringan. Tes UI memakai halaman fixture untuk memeriksa alamat mesin, tampilan kata kunci dan pembukaan tautan di pembaca.


## Nama tokoh, tempat dan barang khusus (0.1.3)

Nama yang sudah dikenali/didaftarkan dikunci **sebelum** OCR dibersihkan dan teks dikirim ke mesin terjemahan. Hasil diperiksa sebelum ditampilkan dan nama dipasang kembali sesudah penyesuaian aku–kamu. Bila mode dasar mengubah penanda, aplikasi mencoba menerjemahkan hanya bagian di luar nama. Bila terjemahan tetap gagal, teks asli dibiarkan terlihat; aplikasi tidak menebak penggantian dari kata Indonesia seperti “larangan”.

Nama **Ban** dan judul **Chronicle of Runes** sudah disiapkan khusus untuk cerita contoh pada Kayn Scans. Ban tidak dimasukkan sebagai nama untuk semua komik. Saat ganti judul/situs, daftar terpisah dipakai dan pengenalan baru berjalan otomatis. Bab dengan path chapter/episode dan awalan cerita yang sama berbagi daftar. Situs dengan identitas cerita tidak jelas memakai path/query lengkap agar daftar tidak bercampur; dua mirror/domain tidak otomatis digabung. File lokal memakai identitas masing-masing file, sedangkan halaman satu PDF berbagi daftar.

Pengenalan gratis di HP memakai petunjuk terbatas: sapaan seperti “Luna, are you okay?”, “Hey Akira!”, perkenalan “My name is Luna”, serta nama tempat/barang yang disebut jelas sebagai named/called. Huruf kapital semua saja **bukan** bukti sebuah kata adalah nama. Ini bukan NER/AI yang memahami seluruh cerita, jadi nama baru/ambigu bisa luput dan deteksi bisa keliru. Mode AI opsional tetap memerlukan komputer sendiri; hasilnya juga diperiksa untuk menjaga nama yang sudah masuk daftar.

Koreksi lewat **⋮ → Nama & istilah tetap**. Isi satu nama per baris, misalnya nama tokoh, kota, atau pedang khusus. Ejaan disimpan per cerita, mendukung nama beberapa kata, apostrof/tanda hubung dan huruf Unicode; maksimum 80 istilah, masing-masing 64 karakter. Pencocokan memakai batas kata dan tidak peka huruf besar/kecil; istilah terpanjang didahulukan. Jika suatu kata sekaligus nama dan kata umum (misalnya Ban), semua kecocokan kata itu dalam cerita akan mengikuti daftar. Bila itu tidak cocok, hapus/koreksi entri. Nama yang dihapus tidak ditambahkan otomatis lagi pada cerita itu; bisa ditambahkan manual kembali. Pengenalan otomatis bisa dimatikan. Pengaturan tetap tersimpan saat aplikasi ditutup/diperbarui.

Server AI sumber diperbarui untuk menerima pemetaan nama, menjaga penanda persis sekali per blok dan menolak hasil dengan nama hilang/berulang. Riwayat konteks memakai nama asli yang dipulihkan, bukan penanda yang bisa dipakai ulang untuk nama lain. Jika memakai AI, perbarui juga `server/reader_server.py` di komputer. Nama/daftar yang cocok ikut dikirim hanya ke server AI pilihan pengguna; mode dasar tetap di HP.


## Pembacaan dialog dan tata huruf (0.1.4)

Mode gratis di HP tetap memakai model ML Kit yang sama; pembaruan ini memperbaiki masukan OCR, pengelompokan dan tampilan hasil, bukan mengganti model dengan layanan AI tersembunyi. Dialog diterjemahkan dalam kelompok berurutan sampai 8 paragraf/sekitar 4.000 karakter, termasuk layar dengan lebih dari 6 blok yang sebelumnya langsung diproses sendiri-sendiri. Jika jumlah paragraf keluaran berubah, aplikasi mencoba tiap bagian kembali. Penanda nama kini berbeda antarbalon dalam satu pembacaan; keluaran yang memindahkan/menghilangkan nama tidak langsung dipakai. Pembacaan lama dibatalkan saat pengguna menggulir agar hasil tidak menempel di posisi lama.

Kontraksi Inggris yang tidak ambigu seperti “don't”, “can't”, “you're” dinormalisasi pada masukan mesin; “he's” atau “I'd” tidak ditebak karena punya beberapa arti. Perapian Indonesia mengatur spasi dan tanda baca, bukan mengganti maksud kalimat dengan contoh hafalan. Tidak ada jaminan pemahaman alur, semua idiom atau hubungan tokoh pada mode dasar. AI lokal opsional memakai konteks dialog sebelumnya dan instruksi menjaga rujukan, negasi, maksud serta emosi; tetap membutuhkan komputer/server sendiri dan dapat salah. Tidak otomatis aktif pada APK.

**Huruf bergaya/terpotong:** OCR membaca piksel yang terlihat di aplikasi. Jika hasil kosong atau ada baris dengan confidence rendah, aplikasi melakukan satu percobaan tambahan pada gambar berkontras abu-abu di HP, lalu mempertahankan blok yang skornya lebih baik. Gambar asli dipakai untuk menilai warna/ukuran; file asli tidak diubah. Percobaan tambahan dibatasi satu kali dan memori; confidence OCR bukan bukti kebenaran. Huruf dekoratif ekstrem atau gambar pecah tetap bisa salah.

Pemenggalan baris yang jelas pada kosakata terbatas, misalnya “SUD-” lalu “DENLY”, digabung sebelum diterjemahkan. Gagap, ucapan terputus, gabungan kata asing dan nama tidak ditebak sembarangan. Nama bertanda hubung yang sudah ada dalam daftar diperiksa sebelum penggabungan. Jeda vertikal besar antarbaris OCR menjadi paragraf terpisah. Urutan masih berdasarkan posisi teks, bukan pengenalan panel/pembicara penuh.

Teks yang menyentuh tepi atas/bawah layar tidak dipaksakan menjadi kalimat lengkap: sumber tetap terlihat dan aplikasi meminta gulir sedikit supaya balon utuh. Ini menangani potongan di batas viewport, bukan memulihkan huruf yang memang hilang dari gambar. Untuk huruf sangat kecil, perbesar halaman. Tidak ada rekonstruksi ucapan atau isi gambar yang tidak terlihat.

**Warna dan ukuran:** aplikasi mengambil perkiraan warna tinta dominan dari area kata OCR untuk tiap paragraf. Teks merah/biru/warna terang dapat tetap berwarna; warna yang kurang kontras diberi bayangan penegas. Paragraf dengan beberapa warna/gradien tidak memiliki pemetaan arti per kata, sehingga memakai warna dominan. Hiasan, garis tepi huruf, font artistik dan gambar di belakang teks belum ditiru persis. Warna latar juga merupakan perkiraan dan dapat keliru pada gambar ramai.

Ukuran huruf mengikuti tinggi tulisan sumber dan ruang hasil OCR, dengan batas 38 piksel lama dihapus. Teks besar menjaga penekanannya, teks panjang mengecil sebatas ukuran yang masih terbaca. Jika tidak muat pada batas itu, sumber dibiarkan terlihat dengan penanda “Hasil ↗”; baca terjemahan lengkap lewat tombol Hasil. Teks tidak dipaksa menjadi ukuran 5 piksel. Penyelarasan tengah/kiri diperkirakan dari posisi baris sumber. Jarak antarbaris 1,12 dan jarak paragraf tambahan disesuaikan dengan ukuran huruf. Panel Hasil menyediakan teks yang bisa dipilih, ukuran lebih nyaman dan jarak antardialog.

Layout dihitung saat hasil berubah, disimpan dan dipakai ulang saat menggambar, sehingga gulir tidak menghitung ulang seluruh susunan teks pada tiap frame. Pemindaian kontras, sampling warna dan model terjemahan tetap memakai sumber daya HP; performa belum diukur di perangkat pengguna.


## Beranda, bab terakhir dan huruf kapital miring (0.1.5)

Saat peluncuran baru, aplikasi menampilkan **Beranda**: kolom pencarian/tautan, **Lanjutkan bacaan**, dan **Riwayat bab**. Ketuk judul aplikasi bertanda ⌂ atau pilih ⋮ → Beranda untuk kembali dari pembaca. Tautan baru yang dibagikan ke aplikasi langsung dibuka dan diprioritaskan. Tautan berbagi yang telah dipakai tidak disimpan sebagai perintah untuk diulang pada peluncuran berikutnya. Kembali melalui daftar aplikasi Android dapat melanjutkan activity yang memang masih terbuka.

Riwayat menyimpan maksimal 30 halaman web: judul, URL, waktu kunjungan dan posisi gulir. Pencarian DuckDuckGo dan file pribadi tidak masuk daftar bab. Bab lama yang menyimpan posisi belakangan tidak mengalahkan waktu kunjungan bab yang lebih baru. Callback halaman lama juga tidak boleh menimpa URL halaman aktif. Pilih Lanjutkan atau bab di riwayat untuk kembali ke tautan dan posisi yang tersimpan. Situs dengan gambar yang dimuat terlambat atau tata letak yang berubah dapat membuat posisi gulir belum tepat; ini bukan pengunduhan bab offline. Pilihan nama/gaya per cerita tetap tersimpan terpisah.

Huruf kapital miring/besar dibaca ulang pada **area teks yang sulit** dengan ukuran huruf dinormalisasi, lalu dengan pelurusan kemiringan dan pembesaran terkontrol. Maksimal dua area dan dua percobaan per area, berurutan. Jika maksimal dua baris masih meragukan, pembaca Tesseract LSTM English yang terpisah mencoba baris itu di HP. Model cadangan dibundel dan disalin ke ruang privat aplikasi saat dipakai; tidak memerlukan akun/API atau unduhan model OCR tambahan. Jika tidak ada area dikenali, percobaan kontras tetap tersedia. Koordinat hasil dikembalikan ke gambar asli supaya posisi, warna dan ukuran overlay tidak ikut bergeser. Semua percobaan tambahan sementara di memori HP.

Tesseract memakai model `tessdata_fast` English (~4 MB), bukan layanan terjemahan. Tambahan ukuran paket dan ruang model lokal diperlukan. Kedua pengenal tetap bisa salah; hasil cadangan hanya dipakai jika lolos pemeriksaan kualitas, geometri dan nama. Pembanding kosakata CMUdict lokal membantu menilai hasil OCR bersama confidence pengenal. Campuran huruf/angka yang mencurigakan juga memicu pembacaan ulang. **Tidak ada penggantian global** “50ML” menjadi “SOUL” atau perubahan semua kata asing menjadi kata kamus. Angka, ukuran, nama fantasi dan font tidak lazim tetap dapat ambigu. Nama yang sudah dikenal diperiksa agar tidak hilang saat hasil area diganti. Kualitas skor dan kamus bukan jaminan kata yang dipilih selalu benar.

Perbaikan diuji pada potongan panel contoh pengguna dari sumber publik: LAUGH, DIRECTLY dan SOUL berhasil dikenali melalui jalur pembacaan baru. Panel berhak cipta itu tidak dimasukkan ke APK atau arsip sumber. Tes opsional CapsOcrTest memakai file uji privat `files/ocr-panel.png` di data aplikasi debug; tanpa file itu tes dilewati. Uji ini bukan bukti semua teks kapital pada semua komik sudah benar.

Awalan gagap yang jelas diterjemahkan sebagai bagian dari kalimat utuh lalu dikembalikan mengikuti huruf awal kata Indonesia, sehingga tidak menyalin awalan salah seperti P-JANGAN. Nama yang dipertahankan tidak diubah ejaannya. Mode dasar tetap memakai ML Kit gratis, belum memahami seluruh cerita, dan dapat salah menerjemahkan meski OCR lebih baik.


## Nama dan ikon (0.1.6)

Aplikasi kini bernama **TCD** (sebelumnya KomikLingo), dengan ikon balon percakapan komik berisi TCD. Ikon adaptif memakai latar navy, balon putih/tulisan gelap dan aksen hijau, dengan ruang untuk mask launcher Android. Nama pada launcher, tombol Beranda, menu, panduan berbagi dan APK telah disesuaikan. Identitas paket `com.komiklingo.app` dan sertifikat sengaja dipertahankan untuk pembaruan di atas APK lama; riwayat, pengaturan dan model tetap berada pada aplikasi yang sama.

APK utama: `dist/TCD-Android.apk`; sumber: `dist/TCD-Source.zip`. Tautan artefak lama KomikLingo pada branch unduhan diperbarui sebagai alias. Gambar ikon asli hasil generasi disertakan sebagai resource `app/src/main/res/drawable-nodpi/tcd_balloon.png`.


## Halaman aktif dan balon aplikasi lain (0.1.7)

**1 halaman:** buka satu halaman komik di pembaca TCD, tekan **1 halaman**, lalu tunggu pemindaian otomatis dari atas sampai bawah selesai. Ini hanya halaman/chapter yang sedang dibuka, tidak membuka chapter berikutnya atau seluruh seri. Aplikasi menggulir dengan area tumpang tindih, menunggu gambar terlihat dimuat (maksimal 7 detik per layar), lalu kembali ke posisi awal. Bilah kemajuan menyediakan Berhenti; menyentuh halaman juga menghentikan pemindaian. Jangan menavigasi/menggulir selama pemindaian jika ingin menyelesaikannya.

Hasil dan koordinatnya berada **di RAM untuk dokumen aktif saja**: menggulir atau memperbesar menampilkan kembali terjemahan yang telah dibaca; refresh, pindah halaman, next/back chapter membersihkannya dan halaman perlu diterjemahkan ulang. Hasil tidak menjadi arsip terjemahan offline. Riwayat judul/URL/posisi bacaan tetap fitur terpisah. Hasil sebagian tetap tersedia jika pemindaian dihentikan. Batas pengaman 400 viewport atau 6.000 bagian; halaman tanpa akhir, gambar gagal dimuat, canvas/iframe khusus atau perubahan tata letak setelah scan dapat menyebabkan bagian terlewat/tidak tepat. Satu blok sangat panjang diproses per baris yang terlihat. Kotak yang tidak muat tetap menyediakan Hasil, bukan teks terlalu kecil.

**Balon TCD:** Beranda → Balon TCD → Mulai. Izinkan tampil di atas aplikasi lain dan tangkap layar Android. Notifikasi ditawarkan pada Android 13+. Buka browser/aplikasi lain lalu ketuk balon yang dapat digeser. Hasil adalah cuplikan statis: Tutup untuk kembali menggulir, lalu ketuk lagi. Tahan balon untuk menu atau Hentikan dari notifikasi. Balon berhenti ketika layar dikunci; untuk sesi baru, buka TCD dan berikan izin layar lagi. Tidak memerlukan aksesibilitas, kamera, mikrofon atau login.

Cuplikan dari aplikasi lain hanya diproses oleh OCR dan terjemahan **di HP**, meski server AI pembaca internal pernah diatur. Tidak direkam terus-menerus, tidak disimpan ke disk, tidak dikirim ke server AI, dan dilepas saat hasil ditutup/sesi dihentikan. Teks hasil terakhir hanya di RAM sampai sesi dihentikan. Isi pribadi yang terlihat bisa ikut terbaca; gunakan pada layar yang ingin diterjemahkan. Layar yang dilindungi Android (misalnya sebagian aplikasi bank/DRM) tidak dapat diterjemahkan. Fitur 1 halaman penuh tersedia di pembaca internal, sedangkan balon hanya membaca layar yang terlihat setiap kali diketuk.

Pembacaan ulang terbatas pada area meragukan, termasuk PO/DO dan GO/60; tidak ada penggantian global angka 60 menjadi GO. Pergantian angka menjadi kata yang bentuknya mirip memerlukan pembacaan OCR independen sebelum diterima, dengan pemeriksaan kualitas dan nama. Angka asli tetap perlu dipertahankan. Salah OCR masih mungkin; gunakan Lihat asli/Hasil untuk membandingkan. Sampling warna/kemiringan berjalan di worker, tabel luminansi dipakai ulang dan layout hasil dicache agar scrolling tidak melakukan OCR/penataan ulang berulang. Waktu pemindaian seluruh halaman bergantung panjang halaman, gambar, HP dan model; bukan proses seketika. Belum ada angka kecepatan pada HP pengguna.


## Terjemahan biasa tersimpan dan Reset per bagian (0.1.8)

Tidak perlu memindai satu halaman penuh untuk menyimpan hasil. Tekan **Terjemahkan**, atau biarkan mode otomatis membaca setelah gulir berhenti. Bagian yang sudah berhasil diterjemahkan tetap tampil saat menggulir ke atas/bawah dan memperbesar halaman. Saat kembali ke viewport yang sudah dibaca lengkap, aplikasi memakai hasil tersimpan dan tidak memanggil penerjemah lagi. Bagian baru dibaca bertahap saat terlihat. Mode 1 halaman tetap tersedia dan memakai hasil yang sudah ada.

Di bawah tiap bagian terjemahan yang memiliki ruang tersedia ada tombol kecil **Reset**. Ketuk untuk membaca ulang gambar pada bagian tersebut dan menerjemahkannya lagi. OCR Reset memakai crop area terpilih, bukan memproses seluruh halaman. Hasil bagian lain tetap disimpan. Tombol berubah menjadi **Reset ✓** setelah hasil diganti; panel Hasil juga memberi keterangan Diperbarui. Jika pembacaan/penerjemahan ulang gagal, hasil sebelumnya tetap tersedia dan status menjelaskan kegagalannya. Menggulir saat proses berjalan membatalkan pekerjaan yang posisinya sudah berubah. Menyeret dari tombol Reset tetap dapat menggulir bacaan, bukan memicu Reset tak sengaja.

Jika tidak ada ruang di bawah teks (misalnya dua balon sangat rapat), tombol tidak ditaruh menutupi balon lain. Buka **Hasil** dan tekan **Reset bagian ini** di bawah terjemahan yang dipilih; pembaca akan mengarahkan viewport ke bagian tersebut. Satu bagian mengikuti paragraf/blok OCR dalam balon, bukan jaminan satu tombol untuk tiap tanda titik.

Hasil hanya berlaku untuk **dokumen yang sedang dibuka**. Refresh, next, back, membuka tautan/file lain atau menutup proses aplikasi membersihkannya. Riwayat URL/posisi bacaan tetap disimpan terpisah; teks terjemahan bukan arsip permanen. Mengubah gaya/nama/server menerapkan pengaturan baru dengan membersihkan hasil dokumen lama. Batas hasil halaman tetap 6.000 bagian; wilayah yang sudah dipindai disimpan terbatas di memori.

Kotak penutup sumber dibuat dengan margin tambahan, termasuk huruf miring, dan memakai latar penuh sampai tepi kotak. Semua penutup digambar sebelum huruf Indonesia, sehingga penutup bagian berikutnya tidak memotong baris hasil sebelumnya. Kotak OCR yang tumpang tindih pada bagian yang sama digabung/diganti, bukan ditumpuk. Ini mengurangi sisa huruf dan garis akibat penimpaan; latar ramai, OCR yang salah batas, gambar yang bergeser setelah dimuat dan font ekstrem masih bisa keliru. Model terjemahan tetap ML Kit gratis: Reset bisa menghasilkan kalimat yang sama dan tidak menjamin maknanya selalu tepat. Bandingkan lewat Lihat asli/Hasil bila ragu.
