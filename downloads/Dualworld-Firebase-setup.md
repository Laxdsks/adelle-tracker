# Dualworld 1.6 — pertemanan dan chat

Proyek **dualworld-tracker** dan konfigurasi Web publik yang Anda berikan sudah dibundel dalam APK. Pengguna tidak perlu membuat proyek Firebase atau menyalin konfigurasi/kode ruang. Tidak perlu mengaktifkan billing Google Maps.

## Satu tindakan untuk pemilik proyek

Anonymous Authentication sebelumnya sudah aktif. Versi 1.6 menambahkan teman permanen, izin lokasi per teman, chat pribadi/global dan lampiran, sehingga **aturan Realtime Database perlu diperbarui satu kali**:

1. Buka [Realtime Database → Rules](https://console.firebase.google.com/project/dualworld-tracker/database/dualworld-tracker-default-rtdb/rules).
2. Salin seluruh isi [aturan versi 1.6](https://raw.githubusercontent.com/Laxdsks/adelle-tracker/dualworld-android-download-20261008/downloads/dualworld-database.rules.json), menggantikan isi editor Rules, lalu tekan **Publish**. Berkas sumber yang sama: `database.rules.json`.
3. Pasang APK baru di kedua HP **di atas aplikasi lama tanpa menghapus data**. Pilih nama/avatar saat diminta. Koneksi lama yang disetujui dimigrasikan menjadi pertemanan; pilihan berbagi dipertahankan.

Aturan sudah diuji pada emulator Firebase dengan pemeriksaan isolasi ruang, undangan, direktori terbatas, dan kepemilikan UID. Aturan tersebut belum diterbitkan otomatis ke proyek produksi karena lingkungan ini tidak memiliki sesi admin Firebase. Aturan lama tetap mendukung ruang lama; pencarian profil baru memerlukan Publish di atas. Jangan gunakan aturan `.read: true` atau `.write: true` di root.

## Alur pengguna

1. Buat nama dan avatar (gender opsional), kemudian izinkan GPS/notifikasi jika diinginkan.
2. Buka **♡ Teman**, cari **nama atau ID `dw-…`**, lalu kirim permintaan. Nama/avatar bukan bukti identitas; pastikan penerima adalah orang yang Anda kenal.
3. Penerima menekan **Terima**. Pertemanan tersimpan, tanpa otomatis memberikan lokasi kepada semua teman.
4. Tekan **Bagikan lokasi** hanya pada teman yang boleh melihat posisi Anda. Teman lain tetap tidak dapat membacanya. Berhenti berbagi tidak menghapus teman; **Hapus teman** mencabut akses lokasi/chat. Daftar teman bertahan setelah aplikasi ditutup.
5. Tekan **CHAT** pada teman untuk teks/emoji, foto/file (2 MiB) atau VN (60 detik). **Chat global** hanya teks/emoji. Mikrofon diminta saat VN digunakan, tidak ada telepon/video call.

Profil memakai Firebase Anonymous yang disimpan pada instalasi, bukan login Google atau akun lintas perangkat. Jangan menghapus data APK ketika memperbarui. Pemulihan akun sebelum rilis publik masih diperlukan. Pengguna tidak perlu membuat proyek, token, TURN, atau memasukkan kode konfigurasi.

## Penyimpanan, moderasi dan keamanan

Aturan memisahkan teman, permintaan, per penerima lokasi, chat/media privat, serta laporan. Pengguna luar tidak memiliki akses chat/lokasi pribadi. Global dapat dibaca pengguna aplikasi yang masuk; hanya teks/emoji diterima. File disimpan terpisah di RTDB dan dimuat saat diketuk. Media lama tidak dibuka kembali oleh pertemanan baru setelah hubungan dihapus. Pemilik Firebase tetap memiliki akses administratif; chat bukan end-to-end encrypted.

Laporan tersimpan di `reports/<uid>` untuk pemilik meninjau melalui Console. Tidak ada moderasi otomatis atau pemindaian file. Sebelum publikasi luas, siapkan moderasi operasional, perlindungan penyalahgunaan/App Check yang sesuai web dan Android, retensi/pembersihan pesan serta kuota media. Batas per pesan/identitas saja tidak mencegah orang membuat akun Anonymous baru. Pantau Usage pada Realtime Database; batas Spark tetap berlaku.

## Lokasi dan status

Bacaan GPS diminta tiap 1 detik dan publikasi bacaan baru dibatasi tiap 2 detik; sensor dan internet dapat lebih lambat. Hanya bacaan baru (maksimum umur 30 detik) dengan radius akurasi ≤35 m yang dibagikan. Filter menahan perubahan kecil dalam ketidakpastian GPS; dua bacaan yang konsisten atau kecepatan sensor dapat melepas pergerakan. Waktu yang ditampilkan berasal dari sensor, bukan heartbeat. Akurasi di rumah tetap bergantung sinyal GPS.

Status online dikirim terpisah sekitar tiap 20 detik. “Online · GPS … terakhir” berarti perangkat tersambung tetapi belum memperoleh pengukuran baru. Posisi tetap dapat dibuka atau dipakai sebagai tujuan, dengan waktu aslinya. HP mati tidak dapat mengirim posisi baru, tetapi posisi terakhir tetap tersimpan. Android memakai foreground service/notifikasi; izin latar dan batas baterai produsen dapat memengaruhi pemulihan setelah reboot. Web membutuhkan halaman aktif untuk pembaruan.

## Penghapusan

**Hentikan berbagi** menghapus lokasi Anda ketika server dapat dihubungi. **Hapus teman** mengakhiri akses lokasi/chat pertemanan itu. **Hapus profil dan data** menghapus profil publik, permintaan masuk/keluar, pertemanan/chat terkait, media dan pesan global sendiri, lokasi pribadi serta identitas Firebase; internet diperlukan. `privacy.html` menjelaskan retensi dan penyedia layanan. Jangan memasukkan lokasi/token pribadi dalam laporan publik.
