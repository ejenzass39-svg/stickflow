# StickFlow - APK via GitHub Actions + Termux

Aplikasi kosong (tanpa data bawaan). API key Gemini diisi langsung di dalam aplikasi, tab Profil.

## A. Persiapan sekali saja (Termux)
1. Pasang Termux dari F-Droid (bukan Play Store).
2. Buka Termux, jalankan:
```
pkg update -y && pkg upgrade -y
pkg install -y git gh unzip
termux-setup-storage
```
Izinkan akses penyimpanan saat diminta.
3. Atur identitas git:
```
git config --global user.name "NamaAnda"
git config --global user.email "emailanda@gmail.com"
git config --global init.defaultBranch main
```
4. Login GitHub (buat akun dulu di github.com bila belum punya):
```
gh auth login
```
Pilih: GitHub.com > HTTPS > Yes (authenticate git) > Login with a web browser. Salin kode 8 karakter, buka tautan yang muncul, tempel kode, lalu Authorize.

## B. Unggah proyek
```
cp ~/storage/downloads/stickflow.zip ~
cd ~ && unzip -o stickflow.zip && cd stickflow
git init
git add .
git commit -m "StickFlow awal"
gh repo create stickflow --public --source=. --push
```
Repo dibuat PUBLIC supaya build gratis tanpa batas dan APK mudah diunduh. Aman, karena API key tidak ada di kode.

## C. Pantau build
```
gh run watch
```
Tunggu sekitar 5-10 menit sampai muncul tanda centang hijau. Jika gagal: `gh run view --log-failed`.

## D. Unduh dan pasang APK
- Browser HP: buka `https://github.com/NAMAGITHUB/stickflow/releases`, unduh `StickFlow.apk`.
- Atau lewat Termux: `gh release download --pattern StickFlow.apk -D ~/storage/downloads --clobber`
- Buka file APK, izinkan "Pasang dari sumber tidak dikenal", lalu Install.

## E. Isi API key
Buka aplikasi > tab Profil > tempel API key Gemini (gratis di https://aistudio.google.com/apikey) > Simpan.

## E2. Cadangan Groq (opsional)
Jika Gemini penuh atau error, aplikasi mencoba ulang 3 kali lalu otomatis beralih ke Groq. Ambil key gratis di https://console.groq.com/keys, tempel di Profil > Cadangan: Groq. Kolom ini kosong secara bawaan.

## E3. Fitur terbaru
- Prompt gambar dan animasi lebih detail (timeline gerak, kamera, efek).
- Tombol "Animasi eror? Buat ulang" per scene: tempel pesan error dari Flow, aplikasi membuat versi baru yang lebih sederhana dan aman filter.
- Pilihan format video panjang 16:9 atau Shorts 9:16.
- Tab Jadwal berisi tabel jam upload.

## F. Mengubah aplikasi
Edit `www/index.html`, lalu:
```
git add . && git commit -m "update" && git push
```
APK baru otomatis dibuat. Catatan: tiap build memakai tanda tangan debug baru, jadi hapus (uninstall) aplikasi lama sebelum memasang yang baru. Data di dalam aplikasi ikut terhapus.

## Masalah umum
- "Error: ... API key not valid": key salah atau ada spasi.
- "model not found": ganti nama model di Profil.
- Build gagal: jalankan `gh run view --log-failed` dan kirim hasilnya.
