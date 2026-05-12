# Bagian 1: Perintah Navigasi & Manajemen File/Direktori

---

## 1.1 Navigasi Direktori

### `pwd` - Print Working Directory
```bash
pwd
```
> Menampilkan path direktori saat ini (lokasi Anda berada)

**Contoh Output:**
```
/home/user/Documents
```

**Opsi:**

| Opsi | Penjelasan |
|---|---|
| `pwd -L` | Menampilkan path logis (mengikuti symlink) |
| `pwd -P` | Menampilkan path fisik (menyelesaikan symlink) |

---

### `cd` - Change Directory
```bash
cd <direktori>
```
> Berpindah ke direktori yang ditentukan

**Contoh Penggunaan:**
| Perintah | Penjelasan |
|---|---|
| `cd /home/user` | Pindah ke direktori absolut |
| `cd Documents` | Pindah ke subdirektori relatif |
| `cd ..` | Pindah ke direktori induk (satu level ke atas) |
| `cd ../..` | Pindah dua level ke atas |
| `cd ~` | Pindah ke direktori home user saat ini |
| `cd ~username` | Pindah ke direktori home user tertentu |
| `cd -` | Kembali ke direktori sebelumnya |
| `cd /` | Pindah ke direktori root |

---

### `ls` - List Directory Contents
```bash
ls [opsi] [direktori]
```
> Menampilkan isi direktori

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `ls -l` | Tampilan detail (long format) |
| `ls -a` | Tampilkan semua file termasuk file tersembunyi (diawali `.`) |
| `ls -la` | Gabungan detail + file tersembunyi |
| `ls -lh` | Tampilan detail dengan ukuran file yang mudah dibaca (KB, MB, GB) |
| `ls -lS` | Urutkan berdasarkan ukuran (terbesar ke terkecil) |
| `ls -lt` | Urutkan berdasarkan waktu modifikasi (terbaru ke terlama) |
| `ls -ltr` | Urutkan berdasarkan waktu modifikasi terbalik (terlama ke terbaru) |
| `ls -R` | Tampilkan isi direktori secara rekursif |
| `ls -d */` | Tampilkan hanya direktori |
| `ls -i` | Tampilkan nomor inode setiap file |
| `ls -n` | Tampilkan UID dan GID numerik |
| `ls -1` | Tampilkan satu file per baris |
| `ls --color=auto` | Tampilkan dengan warna |
| `ls -F` | Tambahkan indikator tipe (`/` dir, `*` executable, `@` symlink) |
| `ls -lA` | Tampilkan semua kecuali `.` dan `..` |
| `ls *.txt` | Tampilkan file dengan ekstensi tertentu |
| `ls -lhS` | Detail + ukuran human-readable + sort by size |

**Contoh Output `ls -la`:**
```
total 48
drwxr-xr-x  5 user user 4096 Jan 15 10:30 .
drwxr-xr-x 20 user user 4096 Jan 15 09:00 ..
-rw-r--r--  1 user user  220 Jan 15 09:00 .bash_logout
-rw-r--r--  1 user user 3526 Jan 15 09:00 .bashrc
drwxr-xr-x  2 user user 4096 Jan 15 10:00 Documents
-rw-r--r--  1 user user 1234 Jan 15 10:30 file.txt
```

---

## 1.2 Membuat File & Direktori

### `mkdir` - Make Directory
```bash
mkdir [opsi] <nama_direktori>
```
> Membuat direktori baru

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `mkdir folder1` | Membuat satu direktori |
| `mkdir folder1 folder2 folder3` | Membuat beberapa direktori sekaligus |
| `mkdir -p /path/ke/direktori/baru` | Membuat direktori beserta direktori induknya jika belum ada |
| `mkdir -p folder/{sub1,sub2,sub3}` | Membuat struktur direktori dengan brace expansion |
| `mkdir -m 755 folder` | Membuat direktori dengan permission tertentu |
| `mkdir -v folder` | Verbose (tampilkan proses pembuatan) |

**Contoh Struktur:**
```bash
mkdir -p project/{src,docs,tests,assets/{images,css,js}}
# Menghasilkan:
# project/
# ├── src/
# ├── docs/
# ├── tests/
# └── assets/
#     ├── images/
#     ├── css/
#     └── js/
```

---

### `touch` - Create Empty File / Update Timestamp
```bash
touch [opsi] <nama_file>
```
> Membuat file kosong baru atau memperbarui timestamp file yang sudah ada

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `touch file.txt` | Membuat file kosong baru |
| `touch file1.txt file2.txt file3.txt` | Membuat beberapa file sekaligus |
| `touch -a file.txt` | Hanya update access time |
| `touch -m file.txt` | Hanya update modification time |
| `touch -t 202301151030 file.txt` | Set timestamp tertentu (format: YYYYMMDDhhmm) |
| `touch -r file1.txt file2.txt` | Gunakan timestamp file1 untuk file2 |
| `touch -d "2023-01-15 10:30:00" file.txt` | Set timestamp dengan format string |

---

## 1.3 Menyalin, Memindahkan, dan Menghapus

### `cp` - Copy
```bash
cp [opsi] <sumber> <tujuan>
```
> Menyalin file atau direktori

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `cp file.txt /tujuan/` | Salin file ke direktori lain |
| `cp file.txt salinan.txt` | Salin file dengan nama baru |
| `cp -r folder/ /tujuan/` | Salin direktori secara rekursif |
| `cp -i file.txt /tujuan/` | Konfirmasi jika file tujuan sudah ada |
| `cp -u file.txt /tujuan/` | Salin hanya jika sumber lebih baru dari tujuan |
| `cp -p file.txt /tujuan/` | Pertahankan permission, timestamp, dan ownership |
| `cp -a folder/ /tujuan/` | Archive mode (rekursif + pertahankan semua atribut) |
| `cp -v file.txt /tujuan/` | Verbose (tampilkan proses penyalinan) |
| `cp -n file.txt /tujuan/` | Jangan timpa file yang sudah ada |
| `cp -l file.txt /tujuan/` | Buat hard link alih-alih menyalin |
| `cp -s file.txt /tujuan/` | Buat symbolic link alih-alih menyalin |
| `cp -f file.txt /tujuan/` | Force (hapus tujuan jika tidak bisa ditimpa) |
| `cp *.txt /tujuan/` | Salin semua file dengan ekstensi tertentu |
| `cp -rp folder/ /backup/` | Rekursif + pertahankan atribut (untuk backup) |

---

### `mv` - Move / Rename
```bash
mv [opsi] <sumber> <tujuan>
```
> Memindahkan atau mengganti nama file/direktori

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `mv file.txt /tujuan/` | Pindahkan file ke direktori lain |
| `mv file.txt namabar.txt` | Ganti nama file |
| `mv folder/ /tujuan/` | Pindahkan direktori |
| `mv -i file.txt /tujuan/` | Konfirmasi jika file tujuan sudah ada |
| `mv -u file.txt /tujuan/` | Pindah hanya jika sumber lebih baru |
| `mv -v file.txt /tujuan/` | Verbose (tampilkan proses pemindahan) |
| `mv -n file.txt /tujuan/` | Jangan timpa file yang sudah ada |
| `mv -f file.txt /tujuan/` | Force (timpa tanpa konfirmasi) |
| `mv *.txt /tujuan/` | Pindahkan semua file dengan ekstensi tertentu |

---

### `rm` - Remove
```bash
rm [opsi] <file/direktori>
```
> Menghapus file atau direktori

> ⚠️ **PERINGATAN:** Perintah `rm` bersifat permanen! Tidak ada Recycle Bin di Linux.

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `rm file.txt` | Hapus file |
| `rm file1.txt file2.txt` | Hapus beberapa file |
| `rm *.txt` | Hapus semua file .txt |
| `rm -i file.txt` | Konfirmasi sebelum menghapus |
| `rm -f file.txt` | Force (hapus tanpa konfirmasi, abaikan error) |
| `rm -r folder/` | Hapus direktori secara rekursif |
| `rm -rf folder/` | Force hapus direktori dan isinya (BERBAHAYA!) |
| `rm -ri folder/` | Rekursif dengan konfirmasi tiap file |
| `rm -v file.txt` | Verbose (tampilkan proses penghapusan) |
| `rm -d folder/` | Hapus direktori kosong |
| `rm --no-preserve-root /` | ⛔ SANGAT BERBAHAYA - jangan gunakan! |

---

### `rmdir` - Remove Empty Directory
```bash
rmdir [opsi] <direktori>
```
> Menghapus direktori yang kosong

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `rmdir folder/` | Hapus direktori kosong |
| `rmdir -p a/b/c/` | Hapus direktori beserta induknya jika kosong |
| `rmdir -v folder/` | Verbose |
| `rmdir folder1/ folder2/` | Hapus beberapa direktori kosong |

---

## 1.4 Link File

### `ln` - Link
```bash
ln [opsi] <target> <nama_link>
```
> Membuat link (hard link atau symbolic link)

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `ln file.txt hardlink.txt` | Membuat hard link |
| `ln -s file.txt symlink.txt` | Membuat symbolic link (symlink) |
| `ln -s /path/absolut/ symlink` | Symlink dengan path absolut |
| `ln -sf target symlink` | Force buat/timpa symlink yang sudah ada |
| `ln -v file.txt link.txt` | Verbose |
| `ln -r file.txt ../link.txt` | Buat symlink dengan path relatif |

**Perbedaan Hard Link vs Symbolic Link:**
```
Hard Link  → Menunjuk langsung ke inode (data) yang sama
           → Tetap valid meski file asli dihapus
           → Tidak bisa lintas filesystem

Symlink    → Menunjuk ke path file asli
           → Rusak jika file asli dihapus (broken link)
           → Bisa lintas filesystem & direktori
```

---

## 1.5 Melihat Struktur Direktori

### `tree` - Display Directory Tree
```bash
tree [opsi] [direktori]
```
> Menampilkan struktur direktori dalam format pohon

> 📦 Perlu instalasi: `sudo apt install tree`

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `tree` | Tampilkan pohon direktori dari lokasi saat ini |
| `tree /path/ke/dir` | Tampilkan pohon direktori tertentu |
| `tree -L 2` | Batasi kedalaman tampilan (level 2) |
| `tree -a` | Tampilkan termasuk file tersembunyi |
| `tree -d` | Tampilkan hanya direktori |
| `tree -f` | Tampilkan path lengkap tiap file |
| `tree -h` | Tampilkan ukuran file dalam format human-readable |
| `tree -s` | Tampilkan ukuran file dalam bytes |
| `tree -p` | Tampilkan permission file |
| `tree -u` | Tampilkan nama pemilik file |
| `tree -g` | Tampilkan nama grup file |
| `tree -C` | Tampilkan dengan warna |
| `tree -I "node_modules"` | Abaikan pola tertentu |
| `tree -o output.txt` | Simpan output ke file |
| `tree --dirsfirst` | Tampilkan direktori terlebih dahulu |
| `tree -n` | Tampilkan tanpa warna |

**Contoh Output:**
```
project/
├── docs/
│   ├── README.md
│   └── guide.pdf
├── src/
│   ├── main.py
│   └── utils.py
└── tests/
    └── test_main.py

3 directories, 5 files
```

---

## 1.6 Informasi File

### `file` - Determine File Type
```bash
file [opsi] <nama_file>
```
> Menentukan tipe/jenis file

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `file file.txt` | Tampilkan tipe file |
| `file *` | Cek tipe semua file di direktori saat ini |
| `file -i file.txt` | Tampilkan MIME type |
| `file -b file.txt` | Brief (tanpa nama file) |
| `file -z file.gz` | Cek isi file terkompresi |
| `file -L symlink` | Ikuti symlink |

**Contoh Output:**
```bash
file document.pdf
# document.pdf: PDF document, version 1.4

file script.sh
# script.sh: Bourne-Again shell script, ASCII text executable

file image.jpg
# image.jpg: JPEG image data, JFIF standard 1.01
```

---

### `stat` - Display File Status
```bash
stat [opsi] <file>
```
> Menampilkan informasi lengkap tentang file (ukuran, permission, timestamp, inode, dll.)

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `stat file.txt` | Tampilkan semua informasi file |
| `stat -f file.txt` | Informasi filesystem tempat file berada |
| `stat -c "%s" file.txt` | Tampilkan hanya ukuran file (bytes) |
| `stat -c "%n %s %U" file.txt` | Format kustom (nama, ukuran, pemilik) |
| `stat -c "%a" file.txt` | Tampilkan permission dalam format oktal |
| `stat -c "%y" file.txt` | Tampilkan waktu modifikasi |

**Contoh Output:**
```
  File: file.txt
  Size: 1234       Blocks: 8     IO Block: 4096   regular file
Device: 802h/2050d  Inode: 131073    Links: 1
Access: (0644/-rw-r--r--)  Uid: (1000/  user)   Gid: (1000/  user)
Access: 2024-01-15 10:30:00.000000000 +0700
Modify: 2024-01-15 10:25:00.000000000 +0700
Change: 2024-01-15 10:25:00.000000000 +0700
 Birth: 2024-01-15 09:00:00.000000000 +0700
```

---

### `du` - Disk Usage
```bash
du [opsi] [file/direktori]
```
> Menampilkan penggunaan ruang disk oleh file atau direktori

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `du` | Tampilkan ukuran semua direktori secara rekursif |
| `du file.txt` | Tampilkan ukuran file |
| `du -h` | Human-readable (KB, MB, GB) |
| `du -s folder/` | Tampilkan hanya total ukuran direktori |
| `du -sh folder/` | Total ukuran dalam format human-readable |
| `du -sh *` | Ukuran semua file/folder di direktori saat ini |
| `du -sh */ | sort -h` | Urutkan berdasarkan ukuran |
| `du -a folder/` | Tampilkan ukuran setiap file dan direktori |
| `du -c folder/` | Tampilkan total keseluruhan di akhir |
| `du --max-depth=1` | Batasi kedalaman rekursi |
| `du -L folder/` | Ikuti symbolic link |
| `du --exclude=*.log` | Abaikan file tertentu |
| `du -x folder/` | Jangan cross filesystem |
| `du -b file.txt` | Tampilkan ukuran dalam bytes |

---

### `wc` - Word Count
```bash
wc [opsi] <file>
```
> Menghitung baris, kata, dan karakter dalam file

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `wc file.txt` | Tampilkan jumlah baris, kata, dan byte |
| `wc -l file.txt` | Hitung jumlah baris |
| `wc -w file.txt` | Hitung jumlah kata |
| `wc -c file.txt` | Hitung jumlah byte |
| `wc -m file.txt` | Hitung jumlah karakter |
| `wc -L file.txt` | Tampilkan panjang baris terpanjang |
| `wc file1.txt file2.txt` | Hitung untuk beberapa file sekaligus |
| `wc -l *.txt` | Hitung baris semua file .txt |
| `cat file.txt \| wc -l` | Hitung baris dari pipe |
| `ls \| wc -l` | Hitung jumlah file di direktori |

---

## 1.7 Mencari File & Direktori

### `find` - Find Files and Directories
```bash
find [path] [ekspresi]
```
> Mencari file dan direktori berdasarkan berbagai kriteria

**Berdasarkan Nama:**
| Perintah | Penjelasan |
|---|---|
| `find . -name "file.txt"` | Cari file bernama "file.txt" |
| `find . -name "*.txt"` | Cari semua file .txt |
| `find . -iname "*.TXT"` | Cari nama file (case-insensitive) |
| `find /home -name "*.conf"` | Cari di direktori tertentu |

**Berdasarkan Tipe:**
| Perintah | Penjelasan |
|---|---|
| `find . -type f` | Cari hanya file biasa |
| `find . -type d` | Cari hanya direktori |
| `find . -type l` | Cari hanya symbolic link |
| `find . -type b` | Cari block device |
| `find . -type c` | Cari character device |

**Berdasarkan Ukuran:**
| Perintah | Penjelasan |
|---|---|
| `find . -size 100k` | File berukuran tepat 100KB |
| `find . -size +100k` | File lebih besar dari 100KB |
| `find . -size -100k` | File lebih kecil dari 100KB |
| `find . -size +1M -size -10M` | File antara 1MB dan 10MB |
| `find . -empty` | Cari file/direktori kosong |

**Berdasarkan Waktu:**
| Perintah | Penjelasan |
|---|---|
| `find . -mtime -7` | Dimodifikasi dalam 7 hari terakhir |
| `find . -mtime +30` | Dimodifikasi lebih dari 30 hari lalu |
| `find . -atime -1` | Diakses dalam 24 jam terakhir |
| `find . -ctime -7` | Status berubah dalam 7 hari terakhir |
| `find . -newer file.txt` | Lebih baru dari file.txt |
| `find . -mmin -60` | Dimodifikasi dalam 60 menit terakhir |

**Berdasarkan Permission & Kepemilikan:**
| Perintah | Penjelasan |
|---|---|
| `find . -perm 755` | File dengan permission 755 |
| `find . -perm -644` | File dengan minimal permission 644 |
| `find . -user username` | Dimiliki oleh user tertentu |
| `find . -group groupname` | Dimiliki oleh grup tertentu |
| `find . -perm /u+x` | File yang executable oleh pemilik |

**Dengan Aksi:**
| Perintah | Penjelasan |
|---|---|
| `find . -name "*.txt" -delete` | Cari dan hapus file .txt |
| `find . -name "*.log" -exec rm {} \;` | Jalankan perintah untuk setiap hasil |
| `find . -name "*.txt" -exec cp {} /backup/ \;` | Salin semua hasil |
| `find . -type f -exec chmod 644 {} \;` | Ubah permission semua file |
| `find . -name "*.txt" -print` | Tampilkan path (default) |
| `find . -name "*.txt" -ls` | Tampilkan detail seperti `ls -l` |
| `find . -type f \| xargs grep "kata"` | Cari kata dalam file hasil find |

**Kombinasi Logika:**
| Perintah | Penjelasan |
|---|---|
| `find . -name "*.txt" -o -name "*.pdf"` | OR: cari .txt atau .pdf |
| `find . -name "*.txt" -a -size +1k` | AND: .txt DAN lebih dari 1KB |
| `find . ! -name "*.txt"` | NOT: bukan .txt |
| `find . -maxdepth 2 -name "*.txt"` | Batasi kedalaman pencarian |
| `find . -mindepth 2 -name "*.txt"` | Mulai dari kedalaman tertentu |

---

### `locate` - Locate Files Quickly
```bash
locate [opsi] <pola>
```
> Mencari file menggunakan database index (lebih cepat dari find)

> 📦 Perlu instalasi: `sudo apt install mlocate`

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `locate file.txt` | Cari file bernama file.txt |
| `locate "*.txt"` | Cari semua file .txt |
| `locate -i file.txt` | Case-insensitive search |
| `locate -c file.txt` | Hitung jumlah hasil |
| `locate -l 10 file` | Batasi 10 hasil |
| `locate -r "\.txt$"` | Gunakan regex |
| `locate -e file.txt` | Verifikasi file masih ada (tidak hanya di database) |
| `updatedb` | Perbarui database locate (perlu sudo) |

---

### `which` - Locate a Command
```bash
which <perintah>
```
> Menemukan lokasi file executable sebuah perintah

**Contoh:**
```bash
which python3
# /usr/bin/python3

which -a python
# /usr/bin/python
# /usr/local/bin/python

which ls
# /bin/ls
```

---

### `whereis` - Locate Binary, Source, and Manual
```bash
whereis [opsi] <perintah>
```
> Menemukan lokasi binary, source code, dan manual page

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `whereis ls` | Tampilkan semua lokasi (binary, source, manual) |
| `whereis -b ls` | Hanya binary |
| `whereis -m ls` | Hanya manual page |
| `whereis -s ls` | Hanya source code |

**Contoh Output:**
```
ls: /bin/ls /usr/share/man/man1/ls.1.gz
```

---

### `type` - Display Command Type
```bash
type <perintah>
```
> Menampilkan tipe suatu perintah (alias, built-in, external, dll.)

**Contoh:**
```bash
type ls
# ls is aliased to `ls --color=auto'

type cd
# cd is a shell builtin

type python3
# python3 is /usr/bin/python3

type -a ls
# Tampilkan semua lokasi perintah ls
```

---

## 1.8 Manajemen Path

### `basename` - Strip Directory from Filename
```bash
basename <path> [suffix]
```
> Mengambil nama file dari path lengkap

**Contoh:**
```bash
basename /home/user/Documents/file.txt
# file.txt

basename /home/user/Documents/file.txt .txt
# file (menghapus suffix .txt)

basename /home/user/Documents/
# Documents
```

---

### `dirname` - Strip Last Component from Filename
```bash
dirname <path>
```
> Mengambil direktori dari path lengkap

**Contoh:**
```bash
dirname /home/user/Documents/file.txt
# /home/user/Documents

dirname /home/user/
# /home
```

---

### `realpath` - Resolve Symlinks and Relative Paths
```bash
realpath <path>
```
> Menampilkan path absolut dan nyata (menyelesaikan symlink)

**Contoh:**
```bash
realpath ./Documents/../file.txt
# /home/user/file.txt

realpath -s symlink.txt
# Tampilkan path tanpa menyelesaikan symlink
```

---

## Ringkasan Perintah Bagian 1

```
pwd          → Lihat direktori saat ini
cd           → Pindah direktori
ls           → Lihat isi direktori
mkdir        → Buat direktori
touch        → Buat file kosong
cp           → Salin file/direktori
mv           → Pindah/rename file
rm           → Hapus file/direktori
rmdir        → Hapus direktori kosong
ln           → Buat link (hard/symlink)
tree         → Tampilkan struktur direktori
file         → Cek tipe file
stat         → Info detail file
du           → Ukuran penggunaan disk
wc           → Hitung baris/kata/karakter
find         → Cari file (lengkap & powerful)
locate       → Cari file (cepat, pakai index)
which        → Lokasi executable
whereis      → Lokasi binary, source, manual
type         → Tipe perintah
basename     → Ambil nama file dari path
dirname      → Ambil direktori dari path
realpath     → Path absolut nyata
```

---

## ✅ Bagian 1 Selesai!

**Lanjut ke Bagian 2: Perintah Manajemen Teks & File Content?**
