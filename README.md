# METADIGGER V0.1

Tool CLI berbasis Python untuk mendeteksi, mengekstrak, dan mendemonstrasikan data tersembunyi di dalam file (teknik *append after EOF*), cocok untuk belajar forensik file dan tantangan CTF.

Instagram & GitHub: **@atesyra**

---

## Fitur

| Perintah | Fungsi |
|----------|--------|
| `scan`    | Cari data tambahan setelah marker akhir resmi (EOF) dan signature format lain di dalam file |
| `list`    | Daftar ringkas semua signature format yang ditemukan (termasuk offset 0) |
| `extract` | Ambil (carve) data mulai dari offset tertentu ke file baru |
| `append`  | Tempel `secret_file` ke akhir `cover_file` (demo teknik menyembunyikan data) |
| `view`    | Lihat tipe file, hex dump, percobaan decode teks UTF-8, dan printable strings |

Format yang dikenali: JPEG, PNG, ZIP, ZIP (kosong), RAR, GZIP, PDF, 7Z, BMP, GIF.

---

## Kebutuhan

- **Python 3.8+**
- **Tidak ada library tambahan.** Hanya memakai modul bawaan Python (`sys`, `os`, `re`, `time`, `shutil`, `itertools`, `threading`), jadi **tidak perlu `pip install` apa pun** dan tidak ada `requirements.txt`.
- Terminal dengan dukungan warna ANSI (Linux, macOS, Windows Terminal, atau CMD/PowerShell modern).

---

## Cara Pakai

```bash
python3 run.py -h          # menu ringkas
python3 run.py -hh         # penjelasan lengkap + contoh
```

### scan
```bash
python3 run.py scan foto.jpg
```
Menampilkan ringkasan file, data tersembunyi setelah EOF (jika ada), dan signature format lain beserta offsetnya.

### list
```bash
python3 run.py list foto.jpg
```

### extract
```bash
python3 run.py extract foto.jpg 59554 hasil.bin
```
Offset didapat dari hasil `scan` atau `list`. Mengambil semua byte dari offset sampai akhir file.

### append
```bash
python3 run.py append foto.jpg rahasia.txt gabungan.jpg
python3 run.py scan gabungan.jpg
```
File hasil tetap terbuka normal di image viewer biasa.

### view
```bash
python3 run.py view hasil.bin          # cuplikan
python3 run.py view hasil.bin --all    # tampil penuh
```

---

## Contoh Alur Kerja (CTF)

```bash
python3 run.py scan soal.jpg                    # temukan offset data tersembunyi
python3 run.py extract soal.jpg 59554 out.bin   # ekstrak
python3 run.py view out.bin                     # periksa isinya
```

Jika signature ZIP/RAR ditemukan tetapi file tidak bisa dibuka, kemungkinan beberapa byte signature sengaja dikorupsi. Bandingkan dengan signature asli (ZIP = `50 4b 03 04`) lewat `view` atau `xxd`, lalu perbaiki dengan hex editor.

---

## Jika Script Di-encode / Di-obfuscate

Script ini bisa dibungkus (packed) menjadi satu baris seperti:

```python
_=lambda __,k:__import__('marshal').loads(__import__('zlib').decompress(...));exec((_)(b'...'))
```

Bungkus semacam ini **tidak butuh instalasi tambahan**, karena hanya memakai modul bawaan Python: `marshal`, `zlib`, `base64`, `codecs`. Jadi `requirements.txt` tetap tidak diperlukan.

Catatan penting:

1. **Versi Python harus sama.** Format `marshal` bergantung pada versi minor Python. File yang di-encode dengan Python 3.11 umumnya tidak bisa dijalankan di 3.10 atau 3.12 (muncul error `bad marshal data` atau `unmarshallable object`). Jalankan dengan versi Python yang sama dengan saat meng-encode, atau distribusikan versi source asli.
2. Menjalankan `exec` pada kode yang tidak dikenal berisiko. Jalankan hanya file dari sumber yang kamu percaya.
3. Obfuscation bukan enkripsi. Siapa pun tetap bisa membongkarnya, jadi jangan menaruh data sensitif di dalamnya.

---

## Struktur Proyek

```
.
├── run.py        # script utama
└── README.md
```

---

## Catatan Etika

Gunakan tool ini untuk belajar, riset, dan CTF, atau pada file milik sendiri. Jangan dipakai untuk menyembunyikan atau menyebarkan konten berbahaya, atau memeriksa file milik orang lain tanpa izin.

---

## Lisensi

Bebas dipakai dan dimodifikasi untuk keperluan belajar. Mohon cantumkan kredit **@atesyra**.
