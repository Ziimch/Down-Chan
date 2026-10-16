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

| 📥 Unduh | 🔄 Konversi |
|---|---|
| Video **MP4** atau musik **MP3** | Gambar: **PNG ⇄ JPG ⇄ WEBP** |
| Kualitas: 128–320 kbps / 360p–4K | Video/audio → **MP3** atau **M4A** |
| Tempel link atau **Bagikan** dari app lain | Hasil masuk ke folder pilihanmu |
| Progress, tombol batal, auto-perbarui mesin | Rotasi foto & latar transparan ditangani |

🎨 Tema gelap Material 3 dengan latar bergaya akuarium dan aksen kuning pita.

---

## ⚠️ Perlu Diketahui

- **Spotify belum didukung.** Link YouTube dan situs lain yang dikenali yt-dlp bisa dipakai.
- Kalau muncul **error 403**, tekan **Perbarui mesin (yt-dlp)** lalu coba lagi.
- Konversi video/audio masih **eksperimen**, jadi kabari lewat *Issues* kalau ada yang gagal.

<details>
<summary><b>🛠️ Build dari sumber</b></summary>

<br />

**Syarat:** Android Studio (mendukung AGP 8.1), JDK 17, internet (dependensi dari Maven & JitPack).

```bash
gradle assembleDebug      # APK debug
gradle assembleRelease    # APK release (R8, memakai kunci debug)
```

APK ada di `app/build/outputs/apk/`.

**Tanpa PC (GitHub Actions):** repo ini sudah punya workflow di `.github/workflows/build.yml`. Buat *tag* berawalan `v` (misalnya `v1.1`) lewat GitHub, lalu APK otomatis muncul di tab **Releases**. Bisa juga dijalankan manual dari tab **Actions → Build APK → Run workflow**.

Untuk dipublikasikan, ganti `signingConfig` di `app/build.gradle.kts` dengan keystore sendiri.

**Ukuran APK:** sebagian besar berasal dari Python + yt-dlp + ffmpeg yang disalin per ABI, jadi proyek ini hanya menyertakan `arm64-v8a`. Tambahkan `armeabi-v7a` (HP 32-bit) atau `x86_64` (emulator) di `abiFilters` bila perlu.

**Struktur:**

```
app/src/main/java/com/example/unduhku/
├── MainActivity.kt   # UI, tab, alur unduh & konversi
├── Downloader.kt     # yt-dlp: unduh, progress, batal, update
├── Converter.kt      # konversi gambar & media
└── Storage.kt        # simpan ke folder pilihan (SAF)
```

**Teknologi:** Kotlin · Material 3 · ViewBinding · Coroutines · [youtubedl-android](https://github.com/yausername/youtubedl-android) (yt-dlp + ffmpeg)

</details>

<details>
<summary><b>🗺️ Rencana & keterbatasan</b></summary>

<br />

- [ ] Unduhan di latar belakang (foreground service)
- [ ] Konversi gambar dari link
- [ ] Pilihan kualitas di tab Konversi
- [ ] Riwayat unduhan & dukungan playlist

Saat ini unduhan berhenti bila aplikasi ditutup, dan kualitas MP3 hasil konversi tetap 192 kbps.

</details>

---

<div align="center">

<sub>Unduh hanya konten yang kamu berhak pakai, dan patuhi ketentuan situs sumber serta hukum hak cipta setempat.<br />
Dibangun dengan [yt-dlp](https://github.com/yt-dlp/yt-dlp) · [FFmpeg](https://ffmpeg.org/) · [Material Components](https://github.com/material-components/material-components-android)<br />
Karakter Anna Yanami © pemegang hak ciptanya, dipakai hanya untuk keperluan pribadi.</sub>

</div>
