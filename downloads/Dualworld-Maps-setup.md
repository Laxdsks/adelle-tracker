# Peta, bangunan, dan foto jalan Dualworld

Buka **Lapisan peta** (ikon tumpukan di sisi peta). Pilih Jalan, Satelit, atau Medan dan detail yang diperlukan. Panel, pencarian, rute, foto, 3D, dan panorama berada di Dualworld. Tautan sumber/lisensi dapat membuka situs penyedia saat Anda memilih kreditnya.

## Tanpa billing Google

| Fitur | Sumber | Cakupan |
| --- | --- | --- |
| Jalan, alamat, tempat | OpenStreetMap, Nominatim | Nama dan toko yang telah dipetakan |
| Satelit / topografi | Esri World Imagery / World Topo Map | Ketajaman mengikuti citra tersedia; resolusi asli berbeda per wilayah; zoom otomatis dibatasi jika detail tidak tersedia |
| Bangunan, halte/stasiun, sepeda | OpenStreetMap / Overpass | Bentuk, titik, dan jalur tercatat; bukan jadwal transportasi langsung |
| Medan / bangunan 3D | OpenFreeMap, MapLibre, AWS Terrain Tiles | Model, bukan foto 3D; tinggi bisa berupa perkiraan penyedia |
| Foto tempat/sekitar | Wikimedia Commons, Wikidata | Foto tempat tertaut, atau foto sekitar berlabel radius 2 km |
| Foto jalan / panorama | Mapillary | Client Access Token aplikasi diperlukan; hanya lokasi dengan kontribusi foto |
| Kualitas udara | CAMS / Open-Meteo | Perkiraan model area dengan waktu data, bukan sensor di rumah |
| Kebakaran | NASA EONET | Titik laporan terbuka 30 hari terakhir, bukan batas api atau pemantauan langsung |

Perbesar peta untuk detail area, lalu ketuk tempat/bangunan. Tombol **3D** membuka model; gunakan dua jari untuk mengubah sudut. Ketuk kartu profil sendiri/teman lalu **Bangunan & foto** atau **Jalan 360°** untuk menjelajahi dekat posisi GPS. Foto dan citra adalah rekaman; tidak menggambarkan keadaan pasangan saat ini.

## Foto jalan gratis (pilihan pengguna)

APK 1.6.2 menyertakan Client Access Token aplikasi Dualworld yang diberikan pengguna. Di kedua HP cukup buka **Lapisan peta → Tampilan jalan**; tidak perlu akun/proyek baru atau menempel token lagi. Konfigurasi kosong yang tersimpan pada versi lama tidak menutupi konfigurasi bawaan. Langkah di bawah hanya untuk mengganti sumber atau membuat build sendiri.

1. Masuk/daftar sendiri di [Mapillary](https://www.mapillary.com/app/?login=true). Login tidak dapat diselesaikan atas nama Anda dari lingkungan ini.
2. Buka [dashboard developer](https://www.mapillary.com/dashboard/developers), daftarkan aplikasi **Dualworld**, lalu dapatkan **Client Access Token** aplikasi. Jangan gunakan kata sandi, kunci admin, atau User Access Token.
3. Untuk build sendiri, isi `mapillaryClientToken` pada `maps-config.json`, lalu bangun ulang web/APK. Ini tidak mengubah koneksi Firebase atau ruang pengguna.
4. Dualworld mencari rekaman dalam 500 m. Pilih **Panorama 360°** untuk gambar yang dapat diputar penuh; **Semua foto jalan** menampilkan foto biasa dengan sudut terbatas. Pilih foto dari daftar, lalu ikuti panah jika rangkaian jalan tersedia.
5. Titik biru pada peta menandai foto hasil pencarian sebenarnya. Jika belum ada rekaman, pilih jalan lain atau gunakan satelit/3D.

Token tersimpan pada instalasi tersebut. Untuk memasukkannya ke kedua APK, pembuat aplikasi dapat menyalin `maps-config.example.json` ke **`maps-config.json`** (diabaikan Git), mengisi `mapillaryClientToken`, lalu membangun ulang web/Android. Jangan masukkan lokasi atau token autentikasi Firebase ke berkas ini. Token klien terlihat oleh aplikasi/browser; ikuti ketentuan dan batas pemakaian penyedia.

## Google Maps opsional

Pengguna memilih alternatif tanpa billing Google. Perubahan ini tidak mengaktifkan billing atau API Google. Integrasi tambahan tersedia jika nanti Anda memilih layanan Google:

1. Gunakan proyek Google Cloud yang sesuai, misalnya proyek Firebase yang sudah ada. Aktifkan penagihan sesuai ketentuan Google; uji coba meminta verifikasi kartu. Kuota gratis berbeda per layanan, dan akun berbayar bisa ditagih di atas kuota. Budget alerts hanya pemberitahuan; atur kuota API sesuai kebutuhan.
2. Aktifkan **Maps JavaScript API** dan **Places API (New)**. Panorama memakai SDK JavaScript, bukan embed halaman Google Maps.
3. Buat **API key browser**. Pada application restrictions pilih **Websites / HTTP referrers**; izinkan URL hosting HTTPS Dualworld serta `https://appassets.androidplatform.net/*`. Batasi API ke kedua layanan tadi. Kunci Firebase bukan kunci Maps.
4. Buka **Lapisan peta → Atur sumber Street View & foto Google**, isi kunci browser. Map ID JavaScript opsional. Muat ulang Dualworld setelah mengganti kunci yang SDK-nya sudah dimuat.
5. Google map, lalu lintas, transportasi, sepeda, Places dan panorama berada di panel Dualworld. Foto/detail Places hanya ditampilkan bersama peta Google, dengan kredit. Cakupan dan tanggal rekaman mengikuti penyedia.

Jika referrer APK ditolak, periksa origin/referrer dan dukungan WebView sebenarnya. Untuk distribusi yang memerlukan SDK Android native, gunakan registrasi package/sertifikat yang sesuai. Jangan melepas pembatasan kunci atau memakai kredensial admin untuk melewati error.

## Build dan batas pemeriksaan

```sh
npm run build
npm run android:build
```

Library, CSS, dan lisensi dibundel; data/citra memerlukan internet. Service worker hanya menyimpan shell. `maps-config.json`, foto, ubin, API, lokasi, dan token tidak dimasukkan cache shell. Modul peta tidak mengirim kredensial Firebase ke penyedia peta.

Pada 9 Oktober 2026, token klien publik diterima API Mapillary, metadata dan JPEG panorama asli berhasil diambil, dan panorama lokasi publik Singapura dirender dalam viewer Dualworld. Pemeriksaan browser menggunakan relay HTTPS dengan verifikasi sertifikat tetap aktif; Firebase produksi diblokir. Viewer ditutup dan canvas dibersihkan. Pencarian area Dompu/Woja yang diperiksa tidak menghasilkan foto; hasil kosong tidak dapat diperbaiki hanya dengan token. Cakupan dapat berubah dan pemeriksaan ini tidak membuktikan seluruh daerah tidak memiliki rekaman.

Pengujian dua sesi dengan RTDB emulator dan aturan asli memeriksa gerakan koordinat pada kedua layar dan server, penolakan bacaan GPS kasar/lama, mode ikuti dan pemulihan pilihan berbagi. GPS serta autentikasi memakai fixture, bukan sensor HP fisik. Google Maps tidak diaktifkan atau diuji. Build ini belum diperiksa pada HP fisik; ketepatan jalan/rumah tidak dapat dijamin tanpa uji sensor di lokasi Anda.

## Performa dan pemulihan 1.5

Pada layar sentuh, panel memakai warna solid tanpa blur di atas peta. Bentuk bangunan/jalur memakai renderer canvas; label diberi jarak dan dibatasi (28 tempat, 12 jalan, 100 bangunan) agar tidak memenuhi layar. Detail dimuat setelah geser berhenti, dengan cache area dan jeda permintaan 8 detik. Satelit memakai canvas 512 px pada layar retina bila empat tile anak tersedia; kesalahan pemeriksaan cakupan tidak otomatis menurunkan citra ke zoom 16. Ini tidak menciptakan citra baru atau menjamin setiap wilayah memiliki foto tajam.

Panel Model 3D mengambil peta jalan cadangan dan ekstrusi bangunan OSM jika layanan vektor gagal. Elevasi yang gagal dimatikan agar peta dasar tetap tampil. Tutup panel membebaskan canvas/GPU. Jalan 360° memakai viewer Mapillary tersendiri; lokasi tanpa rekaman ditampilkan sebagai tidak tersedia. Awalan Leaflet disembunyikan; kredit sumber peta/foto tetap dipertahankan.

Verifikasi browser versi ini juga berhasil merender model OpenFreeMap/elevasi asli di koordinat publik Singapura menggunakan worker lokal. Tidak ada pengujian WebGL di HP fisik; kegagalan khusus WebView masih perlu diperiksa di perangkat.
