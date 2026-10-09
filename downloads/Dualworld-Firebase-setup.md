# Mengaktifkan proyek Dualworld yang sudah ada

Proyek Anda: **dualworld-tracker**. Realtime Database sudah dibuat di Singapura.

URL database yang digunakan:

```text
https://dualworld-tracker-default-rtdb.asia-southeast1.firebasedatabase.app
```

Ekspor `dualworld-tracker-default-rtdb-export.json` berisi koordinat lama pada `tracker_data`. Ekspor data bukan konfigurasi aplikasi. Versi baru memakai ruang privat pada `pairs`; data lama tidak diubah atau dijadikan lokasi langsung.

1. Buka [pengaturan proyek](https://console.firebase.google.com/project/dualworld-tracker/settings/general). Di **Your apps**, pilih aplikasi **Web** (ikon `</>`). Jika belum ada aplikasi Web, daftarkan dengan nama **Dualworld**. Hosting tidak perlu diaktifkan untuk APK.
2. Pada **SDK setup and configuration**, pilih **Config**, lalu salin objek `firebaseConfig`. Konfigurasi yang diperlukan berisi `apiKey`, `projectId`, dan `appId`; tambahkan `databaseURL` di atas jika tidak muncul. Ini konfigurasi klien publik. Jangan memakai service-account atau private key.
3. Buka **Authentication → Get started** jika belum aktif, lalu **Sign-in method / Sign-in providers → Anonymous → Enable → Save**.
4. Buka [Realtime Database → Rules](https://console.firebase.google.com/project/dualworld-tracker/database/dualworld-tracker-default-rtdb/rules). Salin **seluruh isi `database.rules.json` dari versi baru ini** lalu tekan **Publish**. Aturan baru melindungi ruang dan lokasi, dan tidak memberi akses aplikasi baru ke `tracker_data` lama. Jangan menggunakan aturan yang mengizinkan semua orang membaca/menulis.
5. Tempel konfigurasi pada **Koneksi dua perangkat** di satu HP, atau berikan konfigurasi Web publik itu kepada pengembang untuk dibundel di APK. Versi 1.3.0 menerima JSON maupun objek `const firebaseConfig = {...};` yang disalin dari Firebase Console. Untuk proyek `dualworld-tracker`, URL database di atas diisi otomatis jika tidak ada dalam konfigurasi yang Anda tempel.
6. Pilih **Dill**, buat undangan, lalu tekan **Salin** atau **Bagikan**. HP pasangan memilih **Adelle**, membuka tautan `dualworld://join?...` atau menempel seluruh tautan/pesan WhatsApp di **Gabung ke ruang pasangan**, lalu menekan **Minta akses**. Setujui koneksi proyek jika diminta.
7. Dill menekan **Terima** di daftar permintaan. Masing-masing menyalakan **Mulai berbagi** dan memberikan izin lokasi.

Hanya pembuat ruang perlu menyiapkan konfigurasi secara manual. Tautan membawa konfigurasi publik yang sama ke HP pasangan, dengan persetujuan sebelum menggunakannya. Kode ruang saja masih didukung jika kedua perangkat sudah menggunakan proyek yang sama. Matikan berbagi kapan saja untuk menghapus lokasi sendiri; di Android 1.3.0 berbagi tetap berjalan dengan notifikasi saat layar terkunci atau aplikasi ditutup. Aktifkan Lokasi akurat, notifikasi, dan izin sepanjang waktu jika ingin lanjut setelah reboot. HP yang mati tidak dapat mengirim posisi baru, tetapi posisi terakhir tetap ditampilkan beserta waktu pembaruannya.

Pesan aplikasi menunjukkan prasyarat yang belum aktif: Anonymous perlu diaktifkan jika muncul pesan metode masuk; aturan perlu diterbitkan jika muncul akses database ditolak. Citra satelit memakai tingkat detail yang tersedia di wilayah tersebut dan memperbesarnya jika zoom lebih dekat tidak mempunyai citra.

Konfigurasi Web proyek telah dibundel ke APK 1.3.0 dan tetap disertakan pada 1.4.0. Pada 9 Oktober 2026, pemeriksaan langsung proyek ini lulus 26 pemeriksaan: Anonymous aktif, pembuat dapat membuat ruang, pihak luar ditolak, pasangan meminta akses dan disetujui, koordinat uji dibaca dua arah, data dihapus saat berhenti berbagi, serta akses terputus setelah keluar. Koordinat yang digunakan adalah data uji, dan ruang serta akun uji sudah dihapus. Perilaku GPS dan izin perangkat tetap perlu dicoba pada kedua HP.

## Memperbarui dari 1.2.1

Pasang APK 1.3.0 di atas instalasi yang ada pada kedua HP; jangan hapus data aplikasi agar identitas dan ruang tetap tersimpan. Setelah pembaruan pertama, aktifkan Mulai berbagi sekali pada setiap HP dan setujui izin lokasi/notifikasi. Versi lama belum memiliki pilihan berbagi yang persisten. Tidak perlu membuat ulang proyek atau memasang ulang aturan Firebase. Tutup layar Dualworld atau kunci layar untuk penggunaan sehari-hari; gunakan Hentikan berbagi hanya saat memang ingin menghentikan dan menghapus posisi. Atur baterai Dualworld ke aktivitas latar diizinkan / tidak dibatasi jika HP menerapkan pembatasan tambahan.

## Pembaruan lokasi dan peta 1.4.1

Pasang APK 1.4.1 di atas versi yang ada pada **kedua HP**, tanpa menghapus data. Tanda tangan, profil, ruang, dan pilihan berbagi tetap digunakan. Tidak perlu proyek Firebase atau aturan baru. Konfigurasi Mapillary sudah dibundel; lihat **Dualworld-Maps-setup.md** / `MAPS_SETUP.md` untuk batas cakupan foto.

Nyalakan **Lokasi/GPS**, pilih izin **Lokasi akurat**, dan pastikan internet kedua HP aktif. Android meminta pembacaan GPS setiap 1 detik dan mengirim bacaan baru maksimal setiap 2 detik; sensor/koneksi dapat lebih lambat. Bacaan di atas radius ketidakpastian 35 m tidak dikirim. Coba dekat jendela atau di luar rumah jika menunggu GPS akurat. **Posisi** mengikuti pasangan, tombol bidik mengikuti diri sendiri; geser peta untuk berhenti mengikuti. Lingkaran menunjukkan ketidakpastian GPS, bukan batas bangunan. HP mati mempertahankan posisi terakhir dan waktu asli, tetapi tidak dapat mengirim pergerakan baru.
