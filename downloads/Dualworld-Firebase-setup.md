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
5. Tempel konfigurasi pada **Koneksi dua perangkat** di satu HP, atau berikan konfigurasi Web publik itu kepada pengembang untuk dibundel di APK. Versi 1.2 menerima JSON maupun objek `const firebaseConfig = {...};` yang disalin dari Firebase Console. Untuk proyek `dualworld-tracker`, URL database di atas diisi otomatis jika tidak ada dalam konfigurasi yang Anda tempel.
6. Pilih **Dill**, buat undangan, lalu tekan **Salin** atau **Bagikan**. HP pasangan memilih **Adelle**, membuka tautan `dualworld://join?...` atau menempel seluruh tautan di **Gabung ke ruang pasangan**, lalu menekan **Minta akses**. Setujui koneksi proyek jika diminta.
7. Dill menekan **Terima** di daftar permintaan. Masing-masing menyalakan **Mulai berbagi** dan memberikan izin lokasi.

Hanya pembuat ruang perlu menyiapkan konfigurasi secara manual. Tautan membawa konfigurasi publik yang sama ke HP pasangan, dengan persetujuan sebelum menggunakannya. Kode ruang saja masih didukung jika kedua perangkat sudah menggunakan proyek yang sama. Matikan berbagi kapan saja untuk menghapus lokasi sendiri; lokasi dibagikan ketika aplikasi terbuka.

Pesan aplikasi menunjukkan prasyarat yang belum aktif: Anonymous perlu diaktifkan jika muncul pesan metode masuk; aturan perlu diterbitkan jika muncul akses database ditolak. Citra satelit memakai tingkat detail yang tersedia di wilayah tersebut dan memperbesarnya jika zoom lebih dekat tidak mempunyai citra.
