# Dualworld 1.5 — profil banyak pengguna

Proyek **dualworld-tracker** dan konfigurasi Web publik yang Anda berikan sudah dibundel dalam APK. Pengguna tidak perlu membuat proyek Firebase atau menyalin konfigurasi/kode ruang. Tidak perlu mengaktifkan billing Google Maps.

## Satu tindakan untuk pemilik proyek

Anonymous Authentication sebelumnya sudah aktif. Versi 1.5 menambahkan pencarian profil, permintaan koneksi, dan heartbeat online, sehingga **aturan Realtime Database perlu diperbarui satu kali**:

1. Buka [Realtime Database → Rules](https://console.firebase.google.com/project/dualworld-tracker/database/dualworld-tracker-default-rtdb/rules).
2. Salin seluruh isi [aturan versi 1.5](https://raw.githubusercontent.com/Laxdsks/adelle-tracker/dualworld-android-download-20261008/downloads/dualworld-database.rules.json), menggantikan isi editor Rules, lalu tekan **Publish**. Berkas sumber yang sama: `database.rules.json`.
3. Pasang APK baru di kedua HP **di atas aplikasi lama tanpa menghapus data**. Pilih nama/avatar saat diminta. Ruang dan izin berbagi yang tersimpan tetap digunakan.

Aturan sudah diuji pada emulator Firebase dengan pemeriksaan isolasi ruang, undangan, direktori terbatas, dan kepemilikan UID. Aturan tersebut belum diterbitkan otomatis ke proyek produksi karena lingkungan ini tidak memiliki sesi admin Firebase. Aturan lama tetap mendukung ruang lama; pencarian profil baru memerlukan Publish di atas. Jangan gunakan aturan `.read: true` atau `.write: true` di root.

## Alur pengguna

1. Buat nama tampilan dan pilih avatar; gender opsional hanya disimpan di perangkat. Izinkan pencarian jika ingin ditemukan teman. Izinkan berbagi lokasi untuk koneksi yang disetujui, lalu berikan izin Lokasi akurat dan notifikasi Android.
2. Buka tombol **♡ Teman**, cari nama teman yang telah membuat profil, pilih profilnya, lalu kirim permintaan.
3. Penerima membuka **♡ Teman → Terima** dan menyetujui koneksi. Berbagi dimulai sesuai izin pengguna. Tombol **Hentikan berbagi** tetap tersedia.

Banyak orang dapat menggunakan proyek yang sama; masing-masing ruang tetap privat untuk dua anggota yang disetujui. Saat ini satu profil memiliki satu koneksi aktif, bukan grup banyak anggota. Profil menggunakan identitas perangkat Firebase Anonymous, bukan login akun Google. Menghapus data aplikasi dapat kehilangan akses profil. Identitas lintas perangkat/akun Google adalah pekerjaan terpisah sebelum rilis publik.

## Lokasi dan status

Bacaan GPS diminta tiap 1 detik dan publikasi bacaan baru dibatasi tiap 2 detik; sensor dan internet dapat lebih lambat. Hanya bacaan baru (maksimum umur 30 detik) dengan radius akurasi ≤35 m yang dibagikan. Filter menahan perubahan kecil dalam ketidakpastian GPS; dua bacaan yang konsisten atau kecepatan sensor dapat melepas pergerakan. Waktu yang ditampilkan berasal dari sensor, bukan heartbeat. Akurasi di rumah tetap bergantung sinyal GPS.

Status online dikirim terpisah sekitar tiap 20 detik. “Online · GPS … terakhir” berarti perangkat tersambung tetapi belum memperoleh pengukuran baru. Posisi tetap dapat dibuka atau dipakai sebagai tujuan, dengan waktu aslinya. HP mati tidak dapat mengirim posisi baru, tetapi posisi terakhir tetap tersimpan. Android memakai foreground service/notifikasi; izin latar dan batas baterai produsen dapat memengaruhi pemulihan setelah reboot. Web membutuhkan halaman aktif untuk pembaruan.

## Penghapusan

**Hentikan berbagi** menghapus lokasi Anda ketika server dapat dihubungi. **Putuskan ruang bersama** mengakhiri koneksi. **Hapus profil dan data** menghapus profil publik, permintaan masuk, data ruang Anda dan identitas Firebase; internet diperlukan. `privacy.html` menjelaskan retensi dan penyedia layanan. Jangan memasukkan lokasi/token pribadi dalam laporan publik.
