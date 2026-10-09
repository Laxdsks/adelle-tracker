# Peta, bangunan, dan foto jalan Dualworld 1.4.0

Buka **Lapisan peta** (ikon tumpukan di sisi peta). Pilih Jalan, Satelit, atau Medan dan detail yang diperlukan. Panel, pencarian, rute, foto, 3D, dan panorama berada di Dualworld. Tautan sumber/lisensi dapat membuka situs penyedia saat Anda memilih kreditnya.

## Tanpa billing Google

| Fitur | Sumber | Cakupan |
| --- | --- | --- |
| Jalan, alamat, tempat | OpenStreetMap, Nominatim | Nama dan toko yang telah dipetakan |
| Satelit / topografi | Esri World Imagery / World Topo Map | Ketajaman mengikuti citra tersedia; zoom dekat dapat memperbesar citra lama |
| Bangunan, halte/stasiun, sepeda | OpenStreetMap / Overpass | Bentuk, titik, dan jalur tercatat; bukan jadwal transportasi langsung |
| Medan / bangunan 3D | OpenFreeMap, MapLibre, AWS Terrain Tiles | Model, bukan foto 3D; tinggi bisa berupa perkiraan penyedia |
| Foto tempat/sekitar | Wikimedia Commons, Wikidata | Foto tempat tertaut, atau foto sekitar berlabel radius 2 km |
| Foto jalan / panorama | Mapillary | Client Access Token aplikasi diperlukan; hanya lokasi dengan kontribusi foto |
| Kualitas udara | CAMS / Open-Meteo | Perkiraan model area dengan waktu data, bukan sensor di rumah |
| Kebakaran | NASA EONET | Titik laporan terbuka 30 hari terakhir, bukan batas api atau pemantauan langsung |

Perbesar peta untuk detail area, lalu ketuk tempat/bangunan. Tombol **3D** membuka model; gunakan dua jari untuk mengubah sudut. Ketuk kartu Dill/Adelle lalu **Bangunan & foto** atau **Jalan 360°** untuk menjelajahi dekat posisi GPS. Foto dan citra adalah rekaman; tidak menggambarkan keadaan pasangan saat ini.

## Foto jalan gratis (pilihan pengguna)

1. Masuk/daftar sendiri di [Mapillary](https://www.mapillary.com/app/?login=true). Login tidak dapat diselesaikan atas nama Anda dari lingkungan ini.
2. Buka [dashboard developer](https://www.mapillary.com/dashboard/developers), daftarkan aplikasi **Dualworld**, lalu dapatkan **Client Access Token** aplikasi. Jangan gunakan kata sandi, kunci admin, atau User Access Token.
3. Di Dualworld terbaru, buka **Lapisan peta → Atur foto jalan gratis**, tempel token, tekan **Simpan & buka jalan**. Ini tidak mengubah koneksi Firebase atau ruang Dill–Adelle.
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

Token Mapillary/kunci Maps belum diberikan, sehingga foto jalan dan layanan Google dengan akun nyata belum dijalankan. Build ini belum diperiksa pada HP fisik; pengujian browser/pairing/rules tidak dijalankan untuk perubahan peta ini. Fitur tidak berarti semua data Google Maps disalin atau tersedia gratis.
