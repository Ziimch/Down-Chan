# 🌊 Down-Chan

> Aplikasi Android untuk **mengunduh** video & musik dari link, sekaligus **mengonversi** file ke format lain — dengan tampilan bertema Anna Yanami.

![Platform](https://img.shields.io/badge/platform-Android%207.0%2B-3DDC84?logo=android&logoColor=white)
![Kotlin](https://img.shields.io/badge/Kotlin-1.9.0-7F52FF?logo=kotlin&logoColor=white)
![Material 3](https://img.shields.io/badge/UI-Material%203-6750A4)
![ABI](https://img.shields.io/badge/ABI-arm64--v8a-blue)

---

## ✨ Fitur

### 📥 Unduh
- Tempel link (YouTube, YouTube Music, dan situs lain yang didukung **yt-dlp**), atau bagikan link langsung dari aplikasi lain lewat tombol **Bagikan**.
- Pilih format **Video (MP4)** atau **Musik (MP3)**.
- Pilih kualitas:
  - Musik: 128 / 192 / 256 / 320 kbps
  - Video: Terbaik tersedia, 4K, 1440p, 1080p, 720p, 480p, 360p
- Progress bar dengan persentase dan tombol **Batalkan**.
- Folder simpan dipilih sekali, lalu diingat.
- Kalau unduhan gagal karena **HTTP 403** atau diminta login, aplikasi otomatis memperbarui yt-dlp sekali lalu mengulang. Ada juga tombol **Perbarui mesin (yt-dlp)** untuk memperbarui manual.

### 🔄 Konversi
| Jenis file | Dikonversi ke | Cara kerja |
|---|---|---|
| Gambar (PNG, JPG, WEBP, dll) | JPG, PNG, WEBP | `Bitmap.compress` bawaan Android |
| Video / audio | MP3, M4A | yt-dlp + ffmpeg (`--enable-file-urls`) |

- PNG transparan yang diubah ke JPG diberi latar putih (bukan hitam).
- Rotasi EXIF foto kamera dibaca supaya hasil tidak miring.
- Hasil konversi disimpan ke folder yang sama dengan unduhan.

### 🎨 Tampilan
- Material 3, tema gelap dengan palet dari ikon (biru indigo + kuning pita).
- Latar gambar penuh dengan kartu semi-transparan ("kaca").
- Edge-to-edge: latar sampai ke balik status bar dan navigasi.

---

## 🧱 Teknologi

| Bagian | Yang dipakai |
|---|---|
| Bahasa | Kotlin 1.9.0 |
| Build | Android Gradle Plugin 8.1.1, `compileSdk` / `targetSdk` 34, `minSdk` 24 |
| UI | Material Components 1.11 (Material 3), ViewBinding |
| Async | Kotlin Coroutines + `lifecycleScope` |
| Mesin unduh | [youtubedl-android](https://github.com/yausername/youtubedl-android) (fork junkfood02, v0.18.1) — yt-dlp + Python + ffmpeg |
| Penyimpanan | Storage Access Framework (`DocumentFile`) |
| Gambar | `BitmapFactory`, `ExifInterface` |

---

## 📁 Struktur Proyek

```
Unduhku Chan/
├── build.gradle.kts
├── settings.gradle.kts
├── gradle.properties
└── app/
    ├── build.gradle.kts
    ├── proguard-rules.pro
    └── src/main/
        ├── AndroidManifest.xml
        ├── java/com/example/unduhku/
        │   ├── MainActivity.kt     # UI, tab, alur unduh & konversi
        │   ├── Downloader.kt       # panggilan yt-dlp, progress, pembatalan, update mesin
        │   ├── Converter.kt        # konversi gambar & media
        │   ├── Storage.kt          # salin hasil ke folder pilihan (SAF)
        │   └── ArchiveSearch.kt    # kerangka kosong (belum dipakai)
        └── res/
            ├── layout/activity_main.xml
            ├── drawable-nodpi/bg_anna.webp   # gambar latar
            ├── values/                       # warna, tema, string (Indonesia)
            └── mipmap-*/ic_launcher.webp
```

---

## 🚀 Cara Build

**Syarat:** Android Studio (versi yang mendukung AGP 8.1), JDK 17, dan koneksi internet untuk mengunduh dependensi (termasuk dari JitPack).

1. Buka folder proyek di Android Studio, lalu tunggu Gradle sync selesai.
2. Sambungkan HP (USB debugging aktif) atau pakai emulator.
3. Jalankan **Run ▶**, atau lewat terminal:

```bash
# APK debug
./gradlew assembleDebug

# APK release (R8 + penyusutan resource, memakai kunci debug)
./gradlew assembleRelease
```

APK ada di `app/build/outputs/apk/`.

> **Catatan release:** build release memakai kunci *debug* supaya langsung bisa dipasang. Untuk dipublikasikan, ganti `signingConfig` di `app/build.gradle.kts` dengan keystore milikmu sendiri.

---

## 📦 Ukuran APK

Sebagian besar ukuran APK berasal dari Python + yt-dlp + ffmpeg, yang disalin **untuk setiap ABI**. Karena itu:

- Proyek ini hanya menyertakan **`arm64-v8a`** (hampir semua HP modern).
- HP 32-bit lama → tambahkan `"armeabi-v7a"` di `abiFilters`.
- Emulator PC → tambahkan `"x86_64"`.
- Build release memakai R8 + `shrinkResources`, dan hanya menyimpan terjemahan `en` dan `id`.

```kotlin
// app/build.gradle.kts
ndk {
    abiFilters.add("arm64-v8a")
    // abiFilters.add("armeabi-v7a")
    // abiFilters.add("x86_64")
}
```

---

## ⚠️ Keterbatasan yang Perlu Diketahui

- **Belum diuji kompilasi.** Kode ditulis tanpa akses Android SDK. Kalau Gradle error pada panggilan `execute(...)`, `destroyProcessById`, atau `updateYoutubeDL`, cek signature-nya di versi library yang dipakai.
- **HTTP 403 dari YouTube** bisa terjadi bila yt-dlp kedaluwarsa. Gunakan tombol **Perbarui mesin**. Kalau tetap gagal, YouTube mungkin mewajibkan token atau login untuk video tersebut.
- **Spotify tidak didukung.** yt-dlp tidak bisa mengunduh dari Spotify (DRM), dan `spotdl` tidak bisa dipasang di Android seperti di Colab.
- **Konversi video/audio** memakai pendekatan `--enable-file-urls` yang **bersifat eksperimen**. Bagian ini perlu diuji di perangkat.
- File video besar akan lama diproses karena disalin dulu ke cache sebelum dikonversi.
- Unduhan berhenti bila aplikasi ditutup (belum memakai foreground service).
- Kualitas MP3 hasil konversi tetap 192 kbps.

---

## 🗺️ Rencana Berikutnya

- [ ] Foreground service agar unduhan jalan di latar belakang
- [ ] Konversi gambar dari link
- [ ] Pilihan kualitas di tab Konversi
- [ ] Pencarian dalam aplikasi (`ArchiveSearch.kt`)
- [ ] Riwayat unduhan
- [ ] Dukungan playlist

---

## ⚖️ Penggunaan yang Bertanggung Jawab

Aplikasi ini hanyalah alat. Unduh dan konversi hanya konten yang kamu punya hak untuk menggunakannya, dan patuhi ketentuan layanan situs sumber serta hukum hak cipta yang berlaku di tempatmu.

---

## 🙏 Kredit

- [yt-dlp](https://github.com/yt-dlp/yt-dlp) — mesin pengunduh
- [youtubedl-android](https://github.com/yausername/youtubedl-android) dan fork [junkfood02](https://github.com/JunkFood02/youtubedl-android) — pembungkus Android
- [FFmpeg](https://ffmpeg.org/) — konversi audio/video
- [Material Components for Android](https://github.com/material-components/material-components-android)
- Ikon dan gambar latar: karakter Anna Yanami dari *Make Heroine ga Oosugiru!* — hak cipta milik pemegang aslinya, dipakai di sini hanya untuk keperluan pribadi.
