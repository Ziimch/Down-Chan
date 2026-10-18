<div align="center">

<img src="app/src/main/res/mipmap-xxxhdpi/ic_launcher.webp" width="120" alt="Ikon Down-Chan" />

# 🌊 Down-Chan

**Unduh video & musik dari link. Konversi file. Semua di satu aplikasi.**

![Android](https://img.shields.io/badge/Android-7.0%2B-3DDC84?logo=android&logoColor=white&style=flat-square)
![Kotlin](https://img.shields.io/badge/Kotlin-1.9-7F52FF?logo=kotlin&logoColor=white&style=flat-square)
![Material 3](https://img.shields.io/badge/Material-3-6750A4?style=flat-square)
![ARM64](https://img.shields.io/badge/ABI-arm64--v8a-4285F4?style=flat-square)

<br />

[![Unduh APK](https://img.shields.io/badge/⬇%20%20UNDUH%20APK%20TERBARU-FFD54A?style=for-the-badge&logo=android&logoColor=2B1F00&labelColor=2A4478)](https://github.com/Ziimch/Down-Chan/releases/latest)

<sub>Buka tab **Releases** → pilih file `.apk` → pasang. Gratis, tanpa iklan.</sub>

</div>

---

## ⬇️ Cara Pasang

1. Klik tombol **Unduh APK Terbaru** di atas, lalu unduh file `.apk`.
2. Buka file tersebut. Kalau diminta, izinkan *pasang dari sumber tidak dikenal*.
3. Buka **Down-Chan**, pilih folder simpan, tempel link, selesai.

> Butuh **Android 7.0+** dan HP **64-bit (arm64)**, yang berarti hampir semua HP keluaran beberapa tahun terakhir.

---

## ✨ Fitur

| 📥 Unduh | 🔄 Konversi | 🎧 Putar & Kelola |
|---|---|---|
| **Pratinjau** link: thumbnail, durasi, perkiraan ukuran | Gambar: **PNG ⇄ JPG ⇄ WEBP** | Pemutar mini dengan tombol **🎲 Acak** dan timer tidur |
| **Antrean** & pilih lagu dari playlist | Perkecil gambar, **gambar → PDF** | **Riwayat** + pencarian, buka / bagikan / hapus |
| Unduh di **latar belakang** + notifikasi, jeda & lanjut | Video/audio → **MP3 / M4A** | Cover & metadata otomatis di MP3 |
| Kualitas 128–320 kbps / 360p–4K | **Kompres video**, video → **GIF** | Buang intro/outro/sponsor |
| **Potong** bagian tertentu, efek *Slowed + reverb* / *Nightcore* / samakan volume | Pilih **banyak file** sekaligus | Subtitle, folder per channel, hanya Wi-Fi, batas kecepatan |

🔔 **Pengingat pembaruan:** aplikasi mengecek GitHub Releases saat dibuka (maks. 1x per 12 jam, hemat data) dan memberi tahu bila ada versi baru.

🎨 Tema gelap Material 3 dengan latar akuarium (bisa diganti dari galeri), aksen kuning pita, dan Anna yang ikut bereaksi.

---

## ⚠️ Perlu Diketahui

- **Spotify belum didukung.** Link YouTube dan situs lain yang dikenali yt-dlp bisa dipakai.
- Kalau muncul **error 403**, aplikasi otomatis memperbarui yt-dlp sekali. Bisa juga manual lewat **Atur → Perbarui mesin**.
- Konversi video/audio, kompres, dan GIF memakai ffmpeg bawaan lewat yt-dlp, jadi masih **eksperimen**. Kalau gagal, salin log di **Atur → Salin log** dan kirim lewat *Issues*.
- Konversi berjalan selama aplikasi terbuka (layar dijaga tetap menyala). Hanya **unduhan** yang berjalan di latar belakang.

<details>
<summary><b>🛠️ Build dari sumber</b></summary>

<br />

**Syarat:** Android Studio (mendukung AGP 8.1), JDK 17, internet (dependensi dari Maven & JitPack).

```bash
gradle assembleDebug      # APK debug
gradle assembleRelease    # APK release (R8, memakai kunci debug)
```

APK ada di `app/build/outputs/apk/`.

**Tanpa PC (GitHub Actions):** repo ini sudah punya workflow di `.github/workflows/build.yml`. Buat *tag* berawalan `v` (misalnya `v2.4`) lewat GitHub, lalu APK otomatis muncul di tab **Releases**. Bisa juga dijalankan manual dari tab **Actions → Build APK → Run workflow**.

Workflow mengerti dua susunan repo: proyek langsung di root, atau repo yang hanya berisi satu file `.zip` proyek (mudah diunggah dari HP). `versionName` aplikasi otomatis diambil dari nama tag, jadi pengingat pembaruan selalu cocok dengan rilis. Lampirkan `.apk` ke rilis supaya tombol "Unduh APK" langsung berfungsi (workflow melakukannya otomatis).

**Kunci tanda tangan (penting agar update bisa menimpa versi lama).** Tanpa kunci tetap, tiap build memakai kunci debug acak, jadi Android menolak memasang versi baru di atas yang lama dan pengguna harus uninstall dulu. Buat satu keystore sekali saja, simpan baik-baik (jangan diunggah ke repo), lalu isi empat *Secrets* di **Settings → Secrets and variables → Actions**:

| Secret | Isi |
|---|---|
| `KEYSTORE_BASE64` | isi file `.jks` dalam base64 |
| `KEYSTORE_PASSWORD` | kata sandi keystore |
| `KEY_ALIAS` | nama alias kunci |
| `KEY_PASSWORD` | kata sandi kunci |

```bash
keytool -genkeypair -v -keystore rilis.jks -alias downchan -keyalg RSA -keysize 2048 -validity 10000
base64 -w0 rilis.jks   # hasilnya ditempel ke KEYSTORE_BASE64
```

Tidak punya PC? `keytool` bisa dijalankan dari HP lewat **Termux** (`pkg install openjdk-17`, lalu perintah di atas) atau lewat **GitHub Codespaces** di peramban.

Kalau kunci ini hilang, versi berikutnya tidak bisa menimpa versi lama. Jangan pernah membagikannya.

**Ukuran APK:** sebagian besar berasal dari Python + yt-dlp + ffmpeg yang disalin per ABI, jadi proyek ini hanya menyertakan `arm64-v8a`. Tambahkan `armeabi-v7a` (HP 32-bit) atau `x86_64` (emulator) di `abiFilters` bila perlu.

**Struktur:**

```
app/src/main/java/com/example/unduhku/
├── MainActivity.kt    # UI & lima tab
├── DownloadService.kt # antrean unduhan di latar belakang
├── Downloader.kt      # yt-dlp: unduh, pratinjau, update
├── Converter.kt       # konversi gambar & media
├── Pemutar.kt         # mesin pemutar musik (satu instance untuk seluruh proses)
├── MusikService.kt    # notifikasi & kontrol media (layar kunci, headset)
├── Pembaruan.kt       # pengingat versi baru dari GitHub Releases
└── Prefs.kt           # pengaturan & riwayat
```

**Teknologi:** Kotlin · Material 3 · ViewBinding · Coroutines (tanpa Room/Coil/Media3, supaya ringan) · [youtubedl-android](https://github.com/yausername/youtubedl-android) (yt-dlp + ffmpeg)

</details>

<details>
<summary><b>🗺️ Rencana & keterbatasan</b></summary>

<br />

- [ ] Cari & putar langsung dari YouTube Music (mode "Jelajah")
- [ ] Konversi gambar dari link
- [ ] Gabung beberapa audio
- [ ] Jadwal unduh

Kualitas MP3 hasil konversi tetap 192 kbps.

</details>

---

<div align="center">

<sub>Unduh hanya konten yang kamu berhak pakai, dan patuhi ketentuan situs sumber serta hukum hak cipta setempat.<br />
Dibangun dengan [yt-dlp](https://github.com/yt-dlp/yt-dlp) · [FFmpeg](https://ffmpeg.org/) · [Material Components](https://github.com/material-components/material-components-android)<br />
Karakter Anna Yanami © pemegang hak ciptanya, dipakai hanya untuk keperluan pribadi.</sub>

</div>
