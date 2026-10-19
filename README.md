# WA Cloning — Android, tampilan HP (versi 1.1.0)

Proyek Android ini membuka situs **resmi** `https://web.whatsapp.com/` di dalam WebView. Ikon launcher menggunakan gambar RK yang diberikan pemilik proyek.

> Ini pembungkus pihak ketiga, bukan aplikasi resmi atau afiliasi WhatsApp. WhatsApp dapat membatasi atau memutus sesi pada browser/aplikasi yang tidak didukung. Jangan anggap ini jaminan anti-logout. Baca kebijakan WhatsApp: https://faq.whatsapp.com/378279804439436/

## Perbaikan versi 1.1.0

- WebView memakai lebar viewport HP, tidak lagi mengecilkan seluruh halaman desktop agar muat di layar.
- Zoom bawaan WebView dimatikan; input/kolom ketik diberi ukuran dasar 16px untuk mengurangi pembesaran saat mengetik.
- Toolbar bawah didesain ulang agar tombol Muat ulang, Desktop, Unduh media, dan Browser terlihat jelas di layar portrait. Toolbar bawah otomatis disembunyikan ketika keyboard terbuka agar area chat lebih lega.
- Tombol Back tidak lagi memanggil `webView.goBack()` yang bisa membawa pengguna ke layar login. Tombol Back menampilkan konfirmasi; data cookie tidak dihapus ketika aplikasi ditutup.
- Tombol Unduh media mencari foto/video yang sedang terlihat di halaman dan mencoba menyimpan tautan langsung atau sumber Blob yang bisa dibaca WebView.
- Ekspor Blob dibatasi maksimal 40 MB agar pemakaian memori lebih terkendali di perangkat lama. Media yang tidak bisa dibaca, sumber sementara yang ditolak, atau berkas lebih besar bisa gagal diunduh.
- Ikon aplikasi memakai PNG RK yang sama untuk semua kepadatan layar; tidak memakai ikon adaptif berbeda yang membuat logo tampak mengecil.

## Fitur

- Membuka WhatsApp Web resmi dengan user-agent desktop yang kompatibel tetapi tata letak viewport mengikuti lebar HP.
- Cookie, DOM storage, dan cache WebView tidak dihapus oleh aplikasi; `CookieManager.flush()` dipanggil ketika aplikasi dijeda atau dihentikan.
- Pemilih file Android untuk mengirim foto, video, atau dokumen.
- Unduh URL HTTP/HTTPS biasa melalui DownloadManager, dan percobaan ekspor foto/video yang sedang terlihat di halaman melalui tombol Unduh media.
- Meminta izin kamera/mikrofon hanya saat halaman meminta fitur tersebut.
- Dapat dipasang pada Android API 24 atau lebih baru, termasuk Android Oreo (API 26).

## Batasan unduhan

Tekan tombol **Unduh media** setelah foto, video, atau Status yang ingin disimpan terlihat di layar. Pilih item yang ditemukan. Sebagian media menggunakan URL internal atau Blob dan dapat diekspor, tetapi WhatsApp bisa mengubah format, menolak akses, atau tidak menyediakan datanya. Tidak semua media/status dijamin bisa diunduh. Aplikasi ini tidak mencoba menyimpan pesan/media sekali lihat. Video Blob di atas 40 MB tidak diproses agar risiko kehabisan memori lebih rendah.

## Sesi dan keluar aplikasi

Aplikasi tidak menghapus cookie atau penyimpanan web. Android biasanya mempertahankan sesi WebView jika proses aplikasi ditutup biasa. Namun, WhatsApp sendiri tetap dapat meminta tautan ulang atau mencabut sesi; aplikasi pihak ketiga tidak dapat mencegah keputusan tersebut. Saat Android benar-benar mematikan proses, layar chat tertentu juga mungkin perlu dimuat ulang, meskipun cookie masih tersimpan.

## Cara membuat APK lewat GitHub Actions

1. Unduh ZIP source dan ekstrak di komputer.
2. Buat repository baru di GitHub.
3. Unggah **isi folder `wa-cloning-android`** langsung ke root repository, termasuk folder tersembunyi `.github/workflows`. Jangan mengunggah ZIP sebagai satu file saja.
4. Buka **Actions → Build WA Cloning APK → Run workflow**.
5. Setelah proses sukses (hijau), buka run tersebut dan unduh **Artifacts → wa-cloning-apk**. Ekstrak artifact untuk mendapatkan `app-debug.apk`.
6. Pindahkan APK ke HP Android dan instal. Izinkan pemasangan dari sumber tersebut hanya jika kamu mempercayai APK hasil build milikmu sendiri.

Workflow memasang JDK 17 dan Android SDK 36 sebelum menjalankan Gradle. Proyek ini belum menghasilkan APK yang diuji pada perangkat fisik dari lingkungan pembuatan ini; hasil kompilasi GitHub Actions harus berhasil terlebih dahulu, lalu uji login, keyboard, pemulihan sesi, unggah file, dan unduhan pada HP-mu.

## Apakah perlu Netlify?

Tidak. Aplikasi langsung membuka `web.whatsapp.com`; tidak membutuhkan situs backend atau hosting Netlify. Meng-host halaman WhatsApp Web sendiri bukan cara yang tepat karena sesi autentikasi berasal dari situs WhatsApp resmi.
