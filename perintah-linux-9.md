# Bagian 9: Perintah Kompresi & Arsip

---

## 9.1 TAR - Tape Archive

### `tar` - Archive Files
```bash
tar [opsi] <file_arsip> [file/direktori]
```
> Tool utama untuk membuat dan mengekstrak arsip di Linux

---

### Membuat Arsip TAR

| Perintah | Penjelasan |
|---|---|
| `tar -cvf arsip.tar file1 file2` | Buat arsip tar dari file |
| `tar -cvf arsip.tar direktori/` | Buat arsip dari direktori |
| `tar -cvf arsip.tar /path/ke/direktori/` | Arsip dari path absolut |
| `tar -cvf arsip.tar file1 file2 dir1/` | Beberapa file dan direktori |
| `tar -cvf arsip.tar *` | Semua file di direktori saat ini |
| `tar -cvf arsip.tar --exclude="*.log" direktori/` | Kecualikan file tertentu |
| `tar -cvf arsip.tar --exclude=direktori/temp direktori/` | Kecualikan direktori |
| `tar -cvf arsip.tar --exclude={".git","node_modules"} direktori/` | Kecualikan beberapa |
| `tar -cvf arsip.tar -C /path/ke/ direktori/` | Ganti direktori sebelum arsip |
| `tar -cvf arsip.tar --newer "2024-01-01" direktori/` | Hanya file lebih baru |
| `tar -cvf - direktori/ \| gzip > arsip.tar.gz` | Arsip + compress via pipe |

---

### Membuat Arsip TAR + Kompresi

| Perintah | Penjelasan |
|---|---|
| `tar -czvf arsip.tar.gz direktori/` | Buat arsip + gzip (.tar.gz) |
| `tar -cjvf arsip.tar.bz2 direktori/` | Buat arsip + bzip2 (.tar.bz2) |
| `tar -cJvf arsip.tar.xz direktori/` | Buat arsip + xz (.tar.xz) |
| `tar -cZvf arsip.tar.Z direktori/` | Buat arsip + compress (.tar.Z) |
| `tar --zstd -cvf arsip.tar.zst direktori/` | Buat arsip + zstd (.tar.zst) |
| `tar -cavf arsip.tar.gz direktori/` | Auto detect kompresi dari ekstensi |
| `tar -czvf arsip.tar.gz -C /path/ direktori/` | Ganti base direktori |
| `tar -czvf arsip.tar.gz --exclude="*.tmp" direktori/` | Kecualikan + compress |

---

### Mengekstrak Arsip TAR

| Perintah | Penjelasan |
|---|---|
| `tar -xvf arsip.tar` | Ekstrak arsip tar |
| `tar -xzvf arsip.tar.gz` | Ekstrak tar.gz |
| `tar -xjvf arsip.tar.bz2` | Ekstrak tar.bz2 |
| `tar -xJvf arsip.tar.xz` | Ekstrak tar.xz |
| `tar -xavf arsip.tar.gz` | Auto detect format kompresi |
| `tar -xvf arsip.tar -C /tujuan/` | Ekstrak ke direktori tertentu |
| `tar -xvf arsip.tar file.txt` | Ekstrak file tertentu saja |
| `tar -xvf arsip.tar "dir/file.txt"` | Ekstrak file tertentu dengan path |
| `tar -xvf arsip.tar --wildcards "*.txt"` | Ekstrak berdasarkan pola |
| `tar -xvf arsip.tar --strip-components=1` | Hapus 1 level direktori |
| `tar -xvf arsip.tar --strip-components=2` | Hapus 2 level direktori |
| `tar -xvf arsip.tar --keep-old-files` | Jangan timpa file yang ada |
| `tar -xvf arsip.tar --overwrite` | Timpa file yang ada |

---

### Melihat Isi Arsip TAR

| Perintah | Penjelasan |
|---|---|
| `tar -tvf arsip.tar` | Tampilkan isi arsip |
| `tar -tzvf arsip.tar.gz` | Isi arsip tar.gz |
| `tar -tjvf arsip.tar.bz2` | Isi arsip tar.bz2 |
| `tar -tavf arsip.tar.xz` | Auto detect + list |
| `tar -tvf arsip.tar \| grep "file.txt"` | Cari file dalam arsip |
| `tar -tvf arsip.tar \| sort -k5 -rn` | Sort berdasarkan ukuran |
| `tar -tvf arsip.tar \| wc -l` | Hitung jumlah file |

---

### Menambah & Memperbarui Arsip TAR

| Perintah | Penjelasan |
|---|---|
| `tar -rvf arsip.tar newfile.txt` | Tambah file ke arsip |
| `tar -uvf arsip.tar direktori/` | Update file yang lebih baru |
| `tar -rvf arsip.tar --exclude="*.log" direktori/` | Tambah dengan kecuali |

> ⚠️ Penambahan/update tidak bisa dilakukan pada arsip yang dikompres!

---

### Opsi TAR Penting

| Opsi | Penjelasan |
|---|---|
| `-c` | Create (buat arsip baru) |
| `-x` | Extract (ekstrak arsip) |
| `-t` | List (tampilkan isi) |
| `-r` | Append (tambah ke arsip) |
| `-u` | Update (perbarui yang lebih baru) |
| `-v` | Verbose (tampilkan proses) |
| `-f` | File (tentukan nama file arsip) |
| `-z` | gzip compression |
| `-j` | bzip2 compression |
| `-J` | xz compression |
| `-Z` | compress (lzw) |
| `-a` | Auto compression |
| `-C` | Ganti direktori |
| `-p` | Preserve permissions |
| `-P` | Preserve absolute paths |
| `--exclude` | Kecualikan pola |
| `--wildcards` | Gunakan wildcard saat ekstrak |
| `--strip-components` | Hapus komponen path |
| `-k` | Keep (jangan timpa) |
| `--overwrite` | Timpa file |
| `--sparse` | Handle sparse files |
| `--numeric-owner` | Gunakan UID/GID numerik |
| `--owner` | Set pemilik arsip |
| `--group` | Set grup arsip |
| `--newer` | Hanya file lebih baru dari tanggal |
| `--checkpoint` | Tampilkan checkpoint saat proses |
| `-I` | Tentukan program kompresi |

**Contoh Penggunaan Lanjutan:**
```bash
# Backup dengan progress
tar -czvf backup.tar.gz /home/user/ \
  --checkpoint=1000 \
  --checkpoint-action=echo="%T"

# Backup ke remote via SSH
tar -czv /home/user/ | ssh user@server "cat > /backup/backup.tar.gz"

# Restore dari remote
ssh user@server "cat /backup/backup.tar.gz" | tar -xzv -C /home/user/

# Split arsip besar menjadi beberapa bagian
tar -czv direktori/ | split -b 1G - backup.tar.gz.part

# Gabung dan ekstrak
cat backup.tar.gz.part* | tar -xzv

# Arsip dengan enkripsi (via openssl)
tar -czv direktori/ | openssl enc -aes-256-cbc -e > backup.tar.gz.enc

# Dekripsi dan ekstrak
openssl enc -aes-256-cbc -d -in backup.tar.gz.enc | tar -xzv

# Verifikasi integritas arsip
tar -tvf arsip.tar.gz > /dev/null && echo "OK" || echo "RUSAK"

# Bandingkan arsip dengan direktori
tar -dvf arsip.tar.gz
```

---

## 9.2 GZIP - GNU Zip

### `gzip` - Compress Files
```bash
gzip [opsi] <file>
```
> Kompresi file menggunakan algoritma DEFLATE

| Perintah | Penjelasan |
|---|---|
| `gzip file.txt` | Kompres file (menghasilkan file.txt.gz, file asli dihapus) |
| `gzip -k file.txt` | Kompres + pertahankan file asli |
| `gzip -c file.txt > file.txt.gz` | Output ke stdout (pertahankan asli) |
| `gzip file1.txt file2.txt` | Kompres beberapa file |
| `gzip -r direktori/` | Kompres semua file dalam direktori (rekursif) |
| `gzip -1 file.txt` | Kompresi tercepat (level 1) |
| `gzip -9 file.txt` | Kompresi terbaik (level 9, paling lambat) |
| `gzip -6 file.txt` | Level kompresi 6 (default) |
| `gzip -v file.txt` | Verbose (tampilkan rasio kompresi) |
| `gzip -q file.txt` | Quiet mode |
| `gzip -f file.txt` | Force (timpa file .gz yang sudah ada) |
| `gzip -l file.txt.gz` | Tampilkan info file gz |
| `gzip -t file.txt.gz` | Test integritas file gz |
| `gzip -d file.txt.gz` | Dekompresi (sama dengan gunzip) |

---

### `gunzip` - Decompress GZ Files
```bash
gunzip [opsi] <file.gz>
```
> Dekompresi file .gz

| Perintah | Penjelasan |
|---|---|
| `gunzip file.txt.gz` | Dekompres file (file.gz dihapus) |
| `gunzip -k file.txt.gz` | Dekompres + pertahankan .gz |
| `gunzip -c file.txt.gz` | Output ke stdout |
| `gunzip -c file.txt.gz > file.txt` | Simpan ke file baru |
| `gunzip -r direktori/` | Dekompres rekursif |
| `gunzip -t file.txt.gz` | Test integritas |
| `gunzip -v file.txt.gz` | Verbose |
| `gunzip -f file.txt.gz` | Force |
| `gunzip *.gz` | Dekompres semua .gz |

---

### `zcat` - View GZ File Content
```bash
zcat file.txt.gz                # Tampilkan isi file gz
zcat file.txt.gz | grep "kata" # Cari dalam file gz
zcat file.txt.gz | head -20    # 20 baris pertama
zcat file.txt.gz | wc -l       # Hitung baris
zcat file1.gz file2.gz         # Tampilkan beberapa file
```

---

### `zgrep` - Search in GZ Files
```bash
zgrep "kata" file.txt.gz       # Cari pola dalam file gz
zgrep -i "kata" file.txt.gz    # Case-insensitive
zgrep -r "kata" direktori/     # Rekursif
zgrep -n "kata" file.txt.gz    # Tampilkan nomor baris
zgrep -c "kata" file.txt.gz    # Hitung kecocokan
```

---

### `zless` & `zmore` - View GZ Files
```bash
zless file.txt.gz              # Lihat file gz dengan less
zmore file.txt.gz              # Lihat file gz dengan more
zdiff file1.gz file2.gz        # Bandingkan dua file gz
```

---

## 9.3 BZIP2 - Block Sorting Compressor

### `bzip2` - Compress Files
```bash
bzip2 [opsi] <file>
```
> Kompresi dengan rasio lebih tinggi dari gzip (lebih lambat)

| Perintah | Penjelasan |
|---|---|
| `bzip2 file.txt` | Kompres file (hasilkan file.txt.bz2) |
| `bzip2 -k file.txt` | Kompres + pertahankan asli |
| `bzip2 -c file.txt > file.txt.bz2` | Output ke stdout |
| `bzip2 -d file.txt.bz2` | Dekompresi |
| `bzip2 -1 file.txt` | Level 1 (tercepat) |
| `bzip2 -9 file.txt` | Level 9 (terbaik, default) |
| `bzip2 -v file.txt` | Verbose |
| `bzip2 -q file.txt` | Quiet |
| `bzip2 -f file.txt` | Force |
| `bzip2 -t file.txt.bz2` | Test integritas |
| `bzip2 -z file.txt` | Compress (eksplisit) |

---

### `bunzip2` - Decompress BZ2 Files
```bash
bunzip2 file.txt.bz2           # Dekompresi
bunzip2 -k file.txt.bz2        # Pertahankan .bz2
bunzip2 -c file.txt.bz2        # Output ke stdout
bunzip2 *.bz2                  # Semua file .bz2
```

---

### `bzcat` & `bzgrep` - BZ2 Utilities
```bash
bzcat file.txt.bz2             # Tampilkan isi file bz2
bzcat file.txt.bz2 | grep "kata"  # Cari dalam bz2
bzgrep "kata" file.txt.bz2    # Search langsung
bzless file.txt.bz2            # View dengan less
bzdiff file1.bz2 file2.bz2    # Bandingkan dua file bz2
bzip2recover damaged.bz2       # Coba recover file rusak
```

---

## 9.4 XZ - High Compression

### `xz` - Compress Files
```bash
xz [opsi] <file>
```
> Kompresi terbaik untuk ukuran kecil (paling lambat)

| Perintah | Penjelasan |
|---|---|
| `xz file.txt` | Kompres file (hasilkan file.txt.xz) |
| `xz -k file.txt` | Kompres + pertahankan asli |
| `xz -c file.txt > file.txt.xz` | Output ke stdout |
| `xz -d file.txt.xz` | Dekompresi |
| `xz -0 file.txt` | Level 0 (tercepat, kompresi rendah) |
| `xz -6 file.txt` | Level 6 (default) |
| `xz -9 file.txt` | Level 9 (terbaik, paling lambat) |
| `xz -e file.txt` | Extreme compression |
| `xz -9e file.txt` | Level 9 + extreme (terkecil) |
| `xz -T 4 file.txt` | Gunakan 4 thread |
| `xz -T 0 file.txt` | Gunakan semua CPU |
| `xz -v file.txt` | Verbose |
| `xz -q file.txt` | Quiet |
| `xz -f file.txt` | Force |
| `xz -t file.txt.xz` | Test integritas |
| `xz -l file.txt.xz` | Info file xz |
| `xz -C crc64 file.txt` | Gunakan checksum CRC64 |

---

### `unxz` & `xzcat` - XZ Utilities
```bash
unxz file.txt.xz               # Dekompresi
unxz -k file.txt.xz            # Pertahankan .xz
xzcat file.txt.xz              # Tampilkan isi
xzcat file.txt.xz | grep "kata"  # Cari dalam xz
xzgrep "kata" file.txt.xz     # Search langsung
xzless file.txt.xz             # View dengan less
xzdiff file1.xz file2.xz      # Bandingkan
xzmore file.txt.xz             # View dengan more
```

---

## 9.5 ZSTD - Zstandard Compression

### `zstd` - Modern Fast Compression
```bash
zstd [opsi] <file>
```
> Kompresi modern yang sangat cepat dengan rasio yang baik

> 📦 Perlu instalasi: `sudo apt install zstd`

| Perintah | Penjelasan |
|---|---|
| `zstd file.txt` | Kompres file (hasilkan file.txt.zst) |
| `zstd -k file.txt` | Kompres + pertahankan asli |
| `zstd -c file.txt > file.txt.zst` | Output ke stdout |
| `zstd -d file.txt.zst` | Dekompresi |
| `zstd -1 file.txt` | Level 1 (tercepat) |
| `zstd -19 file.txt` | Level 19 (terbaik) |
| `zstd -T 4 file.txt` | Gunakan 4 thread |
| `zstd -T 0 file.txt` | Semua CPU thread |
| `zstd --ultra -22 file.txt` | Ultra compression level 22 |
| `zstd -v file.txt` | Verbose |
| `zstd -q file.txt` | Quiet |
| `zstd -f file.txt` | Force |
| `zstd -t file.txt.zst` | Test integritas |
| `zstd -l file.txt.zst` | Info file |
| `zstd --train -r direktori/ -o dict.zstd` | Train dictionary |
| `zstd -D dict.zstd file.txt` | Kompres dengan dictionary |

---

### `unzstd` & `zstdcat` - ZSTD Utilities
```bash
unzstd file.txt.zst            # Dekompresi
unzstd -k file.txt.zst         # Pertahankan .zst
zstdcat file.txt.zst           # Tampilkan isi
zstdgrep "kata" file.txt.zst  # Search dalam zst
zstdless file.txt.zst          # View dengan less
```

---

## 9.6 ZIP - Universal Archive Format

### `zip` - Create ZIP Archive
```bash
zip [opsi] <arsip.zip> <file/direktori>
```
> Membuat arsip ZIP (format universal, kompatibel dengan Windows)

| Perintah | Penjelasan |
|---|---|
| `zip arsip.zip file1.txt file2.txt` | Buat zip dari beberapa file |
| `zip arsip.zip *.txt` | Zip semua file .txt |
| `zip -r arsip.zip direktori/` | Zip direktori (rekursif) |
| `zip -r arsip.zip direktori/ -x "*.log"` | Kecualikan file |
| `zip -r arsip.zip direktori/ -x "*.log" -x "*.tmp"` | Kecualikan beberapa |
| `zip -r arsip.zip direktori/ -x "*/node_modules/*"` | Kecualikan direktori |
| `zip -0 arsip.zip file.txt` | Store only (tanpa kompres) |
| `zip -1 arsip.zip file.txt` | Kompresi tercepat |
| `zip -9 arsip.zip file.txt` | Kompresi terbaik |
| `zip -v arsip.zip file.txt` | Verbose |
| `zip -q arsip.zip file.txt` | Quiet |
| `zip -u arsip.zip newfile.txt` | Update (tambah/perbarui file) |
| `zip -d arsip.zip file.txt` | Hapus file dari zip |
| `zip -m arsip.zip file.txt` | Move (hapus asli setelah zip) |
| `zip -j arsip.zip direktori/` | Junk paths (tanpa struktur direktori) |
| `zip -e arsip.zip file.txt` | Enkripsi dengan password (ZipCrypto) |
| `zip --password "pass" arsip.zip file.txt` | Password langsung |
| `zip -s 100m besar.zip file.iso` | Split zip tiap 100MB |
| `zip -T arsip.zip` | Test integritas |
| `zip -F arsip.zip --out fixed.zip` | Fix zip yang rusak |
| `zip -l arsip.zip file.txt` | Konversi newline (DOS) |
| `zip -ll arsip.zip file.txt` | Konversi newline (Unix) |
| `zipinfo arsip.zip` | Info detail zip |

---

### `unzip` - Extract ZIP Archive
```bash
unzip [opsi] <arsip.zip> [file]
```
> Mengekstrak arsip ZIP

| Perintah | Penjelasan |
|---|---|
| `unzip arsip.zip` | Ekstrak ke direktori saat ini |
| `unzip arsip.zip -d /tujuan/` | Ekstrak ke direktori tertentu |
| `unzip arsip.zip file.txt` | Ekstrak file tertentu |
| `unzip arsip.zip "*.txt"` | Ekstrak berdasarkan pola |
| `unzip -l arsip.zip` | Tampilkan isi zip |
| `unzip -v arsip.zip` | Verbose listing |
| `unzip -t arsip.zip` | Test integritas |
| `unzip -o arsip.zip` | Overwrite tanpa konfirmasi |
| `unzip -n arsip.zip` | Jangan timpa file yang ada |
| `unzip -q arsip.zip` | Quiet mode |
| `unzip -P "password" arsip.zip` | Ekstrak dengan password |
| `unzip -x arsip.zip file.txt` | Kecualikan file saat ekstrak |
| `unzip -j arsip.zip` | Junk paths (ekstrak tanpa struktur) |
| `unzip -a arsip.zip` | Konversi teks otomatis |
| `unzip -f arsip.zip` | Update file yang lebih baru |
| `unzip -u arsip.zip` | Update file yang ada |
| `unzip -C arsip.zip` | Case-insensitive matching |
| `unzip -Z arsip.zip` | Tampilkan info (zipinfo) |

---

### `zipinfo` - ZIP File Information
```bash
zipinfo arsip.zip              # Info detail semua file
zipinfo -1 arsip.zip           # Hanya nama file
zipinfo -2 arsip.zip           # Format pendek
zipinfo -v arsip.zip           # Verbose
zipinfo -h arsip.zip           # Header info
zipinfo -m arsid.zip           # Tampilkan persentase kompresi
zipinfo arsip.zip file.txt     # Info file tertentu
```

---

## 9.7 7-ZIP - High Compression Archive

### `7z` - 7-Zip Archive Manager
```bash
7z [perintah] [opsi] <arsip> [file]
```
> Archive manager dengan kompresi 7z yang sangat tinggi

> 📦 Perlu instalasi: `sudo apt install 7zip` atau `p7zip-full`

**Membuat Arsip:**
| Perintah | Penjelasan |
|---|---|
| `7z a arsip.7z file.txt` | Buat arsip 7z |
| `7z a arsip.7z direktori/` | Arsip direktori |
| `7z a arsip.7z *.txt` | Arsip semua .txt |
| `7z a arsip.zip file.txt` | Buat arsip ZIP |
| `7z a arsip.tar file.txt` | Buat arsip TAR |
| `7z a -t7z arsip.7z direktori/` | Tentukan format |
| `7z a -tzip arsip.zip direktori/` | Format ZIP |
| `7z a -ttar arsip.tar direktori/` | Format TAR |
| `7z a -mx=0 arsip.7z file.txt` | Store only |
| `7z a -mx=1 arsip.7z file.txt` | Kompresi tercepat |
| `7z a -mx=9 arsip.7z file.txt` | Kompresi terbaik |
| `7z a -p"password" arsip.7z file.txt` | Dengan password |
| `7z a -mhe=on -p"pass" arsip.7z file.txt` | Enkripsi header |
| `7z a -v100m arsip.7z file.iso` | Split tiap 100MB |
| `7z a -mmt=4 arsip.7z direktori/` | Gunakan 4 thread |
| `7z a -xr!*.log arsip.7z direktori/` | Kecualikan file |
| `7z a -xr!temp/ arsip.7z direktori/` | Kecualikan direktori |
| `7z a -r arsip.7z direktori/` | Rekursif |

**Mengekstrak Arsip:**
| Perintah | Penjelasan |
|---|---|
| `7z x arsip.7z` | Ekstrak dengan path penuh |
| `7z e arsip.7z` | Ekstrak tanpa struktur direktori |
| `7z x arsip.7z -o/tujuan/` | Ekstrak ke direktori |
| `7z x arsip.7z file.txt` | Ekstrak file tertentu |
| `7z x arsip.7z -p"password"` | Ekstrak dengan password |
| `7z x arsip.7z -y` | Overwrite tanpa konfirmasi |
| `7z x arsip.7z.001` | Ekstrak arsip yang di-split |

**Informasi & Manajemen:**
| Perintah | Penjelasan |
|---|---|
| `7z l arsip.7z` | Tampilkan isi arsip |
| `7z l -slt arsip.7z` | List dengan info teknis |
| `7z t arsip.7z` | Test integritas |
| `7z d arsip.7z file.txt` | Hapus file dari arsip |
| `7z u arsip.7z newfile.txt` | Update arsip |
| `7z rn arsip.7z old.txt new.txt` | Rename file dalam arsip |
| `7z i` | Info versi dan kemampuan |

---

## 9.8 RAR - Roshal Archive

### `rar` & `unrar` - RAR Archive
```bash
# Install
sudo apt install rar unrar
# atau
sudo apt install unrar-free  # Hanya ekstrak

# Membuat RAR
rar a arsip.rar file.txt         # Buat arsip rar
rar a arsip.rar direktori/       # Arsip direktori
rar a -r arsip.rar direktori/   # Rekursif
rar a -p arsip.rar file.txt      # Dengan password (prompt)
rar a -hp arsip.rar file.txt     # Enkripsi header + password
rar a -v100m arsip.rar file.iso  # Split tiap 100MB
rar a -m0 arsip.rar file.txt     # Store only
rar a -m5 arsip.rar file.txt     # Kompresi terbaik
rar a -x*.log arsip.rar dir/     # Kecualikan file
rar a -rr5 arsip.rar dir/        # Recovery record 5%

# Mengekstrak RAR
unrar x arsip.rar                # Ekstrak dengan path
unrar e arsip.rar                # Ekstrak tanpa path
unrar x arsip.rar -o/tujuan/    # Ke direktori tertentu
unrar p arsip.rar file.txt       # Tampilkan isi file ke stdout
unrar x -p"password" arsip.rar  # Dengan password

# Informasi RAR
unrar l arsip.rar                # List isi arsip
unrar v arsip.rar                # Verbose list
unrar t arsip.rar                # Test integritas
rar l arsip.rar                  # List dengan rar
rar t arsip.rar                  # Test dengan rar
```

---

## 9.9 Utilitas Kompresi Lainnya

### `lz4` - Fast Compression
```bash
sudo apt install lz4

lz4 file.txt                   # Kompres ke file.txt.lz4
lz4 -k file.txt                # Pertahankan asli
lz4 -d file.txt.lz4            # Dekompresi
lz4 -c file.txt > file.lz4     # Ke stdout
lz4 -1 file.txt                # Level 1 (sangat cepat)
lz4 -12 file.txt               # Level 12 (lebih baik)
lz4 -9 file.txt                # Level 9
lz4 --fast file.txt            # Mode tercepat
lz4 -v file.txt                # Verbose
lz4 -t file.txt.lz4            # Test
lz4cat file.txt.lz4            # Tampilkan isi
```

---

### `lzma` - LZMA Compression
```bash
lzma file.txt                  # Kompres ke file.txt.lzma
lzma -k file.txt               # Pertahankan asli
lzma -d file.txt.lzma          # Dekompresi
unlzma file.txt.lzma           # Alias dekompresi
lzcat file.txt.lzma            # Tampilkan isi
lzma -1 file.txt               # Level 1 (cepat)
lzma -9 file.txt               # Level 9 (terbaik)
```

---

### `compress` & `uncompress` - LZW Compression
```bash
compress file.txt              # Kompres ke file.txt.Z
compress -k file.txt           # Pertahankan asli
compress -v file.txt           # Verbose
uncompress file.txt.Z          # Dekompresi
zcat file.txt.Z                # Tampilkan isi
```

---

### `zpaq` - Journaling Archiver
```bash
sudo apt install zpaq

zpaq a backup.zpaq direktori/  # Buat arsip
zpaq x backup.zpaq             # Ekstrak
zpaq l backup.zpaq             # List isi
zpaq a backup.zpaq direktori/ -m5  # Level kompresi 5
```

---

### `pixz` - Parallel XZ
```bash
sudo apt install pixz

pixz file.txt                  # Kompres paralel
pixz -d file.txt.xz            # Dekompresi
pixz -t file.txt.xz            # Test
tar -Ipixz -cf arsip.tar.xz direktori/  # Tar + pixz
```

---

## 9.10 Perbandingan Format Kompresi

### Benchmark Kompresi

```bash
# Membandingkan rasio dan kecepatan
# Buat file test
dd if=/dev/urandom bs=1M count=100 of=test.bin

# Test gzip
time gzip -k -9 test.bin
ls -lh test.bin.gz

# Test bzip2
time bzip2 -k -9 test.bin
ls -lh test.bin.bz2

# Test xz
time xz -k -9 test.bin
ls -lh test.bin.xz

# Test zstd
time zstd -k -19 test.bin
ls -lh test.bin.zst

# Test lz4
time lz4 -k test.bin
ls -lh test.bin.lz4

# Hapus file test
rm test.bin.gz test.bin.bz2 test.bin.xz test.bin.zst test.bin.lz4
```

### Tabel Perbandingan Format

```
┌─────────┬──────────────┬─────────────┬────────────────────────────────────┐
│ Format  │ Kecepatan    │ Rasio       │ Kegunaan                           │
├─────────┼──────────────┼─────────────┼────────────────────────────────────┤
│ gzip    │ Cepat        │ Sedang      │ Web server, backup umum            │
│ bzip2   │ Sedang       │ Baik        │ Source code distribution           │
│ xz      │ Lambat       │ Sangat Baik │ Package Linux, arsip long-term     │
│ zstd    │ Sangat Cepat │ Baik        │ Database, real-time compression    │
│ lz4     │ Ultra Cepat  │ Rendah      │ Real-time, network compression     │
│ zip     │ Cepat        │ Sedang      │ Cross-platform, Windows compat     │
│ 7z      │ Lambat       │ Terbaik     │ Arsip distribusi, storage          │
│ rar     │ Sedang       │ Baik        │ Windows compat, recovery records   │
└─────────┴──────────────┴─────────────┴────────────────────────────────────┘
```

---

## 9.11 Arsip Khusus

### `ar` - Create Static Libraries / Archives
```bash
ar [opsi] <arsip.a> [file]
```
> Membuat arsip statik (digunakan untuk library)

```bash
ar cr libfoo.a file1.o file2.o  # Buat/replace arsip
ar t libfoo.a                    # Tampilkan isi
ar x libfoo.a                    # Ekstrak semua
ar x libfoo.a file1.o           # Ekstrak file tertentu
ar d libfoo.a file1.o           # Hapus file dari arsip
ar r libfoo.a newfile.o         # Replace/tambah file
ar u libfoo.a file.o            # Update yang lebih baru
ar v libfoo.a                   # Verbose
ar s libfoo.a                   # Buat symbol index
```

---

### `cpio` - Copy Files To/From Archives
```bash
cpio [opsi]
```
> Format arsip lama yang masih digunakan untuk initramfs

```bash
# Buat arsip
find direktori/ | cpio -ov > arsip.cpio

# Ekstrak arsip
cpio -idv < arsip.cpio

# Buat dengan gzip
find direktori/ | cpio -ov | gzip > arsip.cpio.gz

# Ekstrak arsip gz
gunzip -c arsip.cpio.gz | cpio -idv

# Tampilkan isi
cpio -tv < arsip.cpio

# Copy dengan preservasi
find . | cpio -pvd /tujuan/
```

---

### Membuat & Mengelola ISO

#### `mkisofs` / `genisoimage` - Create ISO
```bash
sudo apt install genisoimage

# Buat ISO dari direktori
mkisofs -o output.iso /path/direktori/

# Dengan label
mkisofs -o output.iso -V "Label" /path/direktori/

# Bootable ISO
mkisofs -o boot.iso \
  -b boot/isolinux/isolinux.bin \
  -c boot/isolinux/boot.cat \
  -no-emul-boot \
  -boot-load-size 4 \
  -boot-info-table \
  /path/direktori/

# Rock Ridge + Joliet (Linux + Windows kompatibel)
mkisofs -o output.iso -R -J -V "Label" /path/direktori/

# ISO dari perangkat
dd if=/dev/cdrom of=backup.iso bs=2048

# genisoimage (sama dengan mkisofs)
genisoimage -o output.iso -R -J /path/direktori/
```

#### `isoinfo` - ISO Information
```bash
isoinfo -d -i file.iso         # Info ISO
isoinfo -l -i file.iso         # List isi ISO
isoinfo -x /FILE.TXT -i file.iso  # Ekstrak file
```

#### Mount ISO
```bash
# Mount ISO
sudo mount -o loop image.iso /mnt/iso
sudo mount -t iso9660 -o loop image.iso /mnt/iso

# Unmount
sudo umount /mnt/iso

# Mount ISO read-write (menggunakan fuseiso)
sudo apt install fuseiso
fuseiso image.iso /mnt/iso
fusermount -u /mnt/iso
```

---

### `squashfs` - Compressed Filesystem
```bash
sudo apt install squashfs-tools

# Buat squashfs
mksquashfs direktori/ filesystem.sqsh

# Buat dengan opsi
mksquashfs direktori/ filesystem.sqsh -comp xz  # Kompresi xz
mksquashfs direktori/ filesystem.sqsh -comp zstd # Kompresi zstd
mksquashfs direktori/ filesystem.sqsh -noI -noD -noF -no-progress  # Cepat
mksquashfs direktori/ filesystem.sqsh -e "*.log"  # Kecualikan

# Mount squashfs
sudo mount -t squashfs -o loop filesystem.sqsh /mnt/squash

# Info squashfs
sudo unsquashfs -l filesystem.sqsh    # List isi
sudo unsquashfs -d output/ filesystem.sqsh  # Ekstrak
sudo unsquashfs -s filesystem.sqsh    # Statistik
```

---

## 9.12 Checksum & Verifikasi

### `md5sum` - MD5 Checksum
```bash
md5sum file.txt                # Hitung MD5
md5sum file1.txt file2.txt     # Beberapa file
md5sum *.txt                   # Semua .txt
md5sum -c checksums.md5        # Verifikasi dari file
md5sum file.txt > file.md5     # Simpan checksum
md5sum -b file.bin             # Binary mode
md5sum -t file.txt             # Text mode
md5sum --quiet -c file.md5     # Quiet, hanya tampilkan yang gagal
```

---

### `sha1sum` - SHA-1 Checksum
```bash
sha1sum file.txt               # Hitung SHA-1
sha1sum -c checksums.sha1      # Verifikasi
sha1sum file.txt > file.sha1   # Simpan
```

---

### `sha256sum` - SHA-256 Checksum
```bash
sha256sum file.txt             # Hitung SHA-256
sha256sum -c checksums.sha256  # Verifikasi
sha256sum file.txt > file.sha256  # Simpan
sha256sum *.tar.gz             # Semua file tar.gz
sha256sum --quiet -c checksums  # Quiet mode
```

---

### `sha512sum` - SHA-512 Checksum
```bash
sha512sum file.txt             # Hitung SHA-512
sha512sum -c checksums.sha512  # Verifikasi
```

---

### `b2sum` - BLAKE2 Checksum
```bash
b2sum file.txt                 # Hitung BLAKE2b
b2sum -c checksums.b2          # Verifikasi
b2sum -l 256 file.txt          # Output 256-bit
```

---

### `cksum` - CRC Checksum
```bash
cksum file.txt                 # Hitung CRC32 + ukuran
cksum file1.txt file2.txt      # Beberapa file
```

---

### `sum` - Checksum (BSD)
```bash
sum file.txt                   # Hitung checksum BSD
sum -s file.txt                # Algoritma SysV
```

---

### `openssl` Checksum
```bash
openssl md5 file.txt           # MD5
openssl sha1 file.txt          # SHA-1
openssl sha256 file.txt        # SHA-256
openssl sha512 file.txt        # SHA-512
openssl dgst -sha256 file.txt  # Digest SHA-256
```

---

### Contoh Verifikasi File Download
```bash
# Download file beserta checksumnya
wget https://example.com/software.tar.gz
wget https://example.com/software.tar.gz.sha256

# Verifikasi
sha256sum -c software.tar.gz.sha256

# Atau manual
sha256sum software.tar.gz
# Bandingkan dengan nilai dari website

# Verifikasi GPG signature
gpg --verify software.tar.gz.sig software.tar.gz
gpg --keyserver keyserver.ubuntu.com --recv-keys KEY_ID
```

---

## 9.13 Enkripsi File

### `gpg` - GNU Privacy Guard
```bash
gpg [opsi] <file>
```
> Enkripsi dan signing file menggunakan GPG

**Enkripsi:**
```bash
# Enkripsi dengan password (symmetric)
gpg -c file.txt                        # Enkripsi simetris
gpg --symmetric file.txt               # Sama
gpg -c --cipher-algo AES256 file.txt  # Algoritma tertentu
gpg -c --armor file.txt               # Output ASCII armor

# Enkripsi dengan public key (asymmetric)
gpg -e -r "nama@email.com" file.txt   # Enkripsi untuk penerima
gpg --encrypt -r "KEY_ID" file.txt    # Menggunakan Key ID

# Enkripsi + sign
gpg -es -r "nama@email.com" file.txt
```

**Dekripsi:**
```bash
gpg -d file.txt.gpg                    # Dekripsi
gpg --decrypt file.txt.gpg             # Sama
gpg -d file.txt.gpg > output.txt      # Simpan ke file
gpg -o output.txt -d file.txt.gpg     # Dengan output
```

**Manajemen Kunci:**
```bash
# Generate key pair
gpg --gen-key                          # Interaktif (simple)
gpg --full-generate-key                # Interaktif (full)
gpg --batch --gen-key params.txt      # Non-interaktif

# List keys
gpg --list-keys                        # Public keys
gpg --list-secret-keys                 # Secret keys
gpg -k                                  # Shorthand

# Export keys
gpg --export "nama@email.com" > pubkey.gpg
gpg --export --armor "nama@email.com" > pubkey.asc
gpg --export-secret-keys > privkey.gpg
gpg --export-secret-keys --armor > privkey.asc

# Import keys
gpg --import pubkey.gpg
gpg --import privkey.gpg

# Delete keys
gpg --delete-key "nama@email.com"
gpg --delete-secret-key "nama@email.com"

# Key server
gpg --keyserver keyserver.ubuntu.com --search-keys "nama"
gpg --keyserver keyserver.ubuntu.com --recv-keys KEY_ID
gpg --keyserver keyserver.ubuntu.com --send-keys KEY_ID

# Signing
gpg --sign file.txt                    # Buat signed file
gpg --clearsign file.txt               # Signed dalam ASCII
gpg --detach-sign file.txt             # Detached signature

# Verify
gpg --verify file.txt.sig file.txt     # Verifikasi signature
gpg --verify file.txt.gpg              # Verifikasi + dekripsi

# Fingerprint
gpg --fingerprint "nama@email.com"

# Edit key
gpg --edit-key "nama@email.com"
```

---

### `openssl` - Enkripsi File
```bash
# Enkripsi file
openssl enc -aes-256-cbc -in file.txt -out file.enc
openssl enc -aes-256-cbc -in file.txt -out file.enc -k "password"
openssl enc -aes-256-cbc -in file.txt -out file.enc -pass pass:"password"

# Dengan salt (lebih aman)
openssl enc -aes-256-cbc -salt -in file.txt -out file.enc

# Dekripsi
openssl enc -aes-256-cbc -d -in file.enc -out file.txt
openssl enc -aes-256-cbc -d -in file.enc -out file.txt -pass pass:"password"

# Base64 encoding
openssl base64 -in file.bin -out file.b64      # Encode
openssl base64 -d -in file.b64 -out file.bin   # Decode

# Generate random data
openssl rand 32 -hex                            # 32 byte hex
openssl rand -base64 32                         # 32 byte base64
```

---

## Ringkasan Perintah Bagian 9

```
tar          → Buat/ekstrak arsip tar (format utama Linux)
gzip/gunzip  → Kompresi/dekompresi .gz
bzip2/bunzip2→ Kompresi/dekompresi .bz2 (rasio lebih baik)
xz/unxz      → Kompresi/dekompresi .xz (rasio terbaik)
zstd/unzstd  → Kompresi/dekompresi .zst (kecepatan terbaik)
lz4          → Kompresi ultra cepat .lz4
zip/unzip    → Buat/ekstrak .zip (universal)
7z           → Arsip 7z dengan kompresi tinggi
rar/unrar    → Buat/ekstrak arsip .rar
ar           → Arsip statik (library)
cpio         → Format arsip klasik
mkisofs      → Buat file ISO
squashfs     → Filesystem terkompresi
md5sum       → Checksum MD5
sha256sum    → Checksum SHA-256
sha512sum    → Checksum SHA-512
gpg          → Enkripsi & signing GPG
openssl      → Enkripsi & checksum via OpenSSL

Ekstensi file:
.tar         → Arsip tanpa kompresi
.tar.gz/.tgz → Tar + gzip
.tar.bz2     → Tar + bzip2
.tar.xz      → Tar + xz
.tar.zst     → Tar + zstd
.gz          → Hanya gzip
.bz2         → Hanya bzip2
.xz          → Hanya xz
.zst         → Hanya zstd
.zip         → ZIP archive
.7z          → 7-Zip archive
.rar         → RAR archive
.iso         → Disk image
```

---

## ✅ Bagian 9 Selesai!

**Lanjut ke Bagian 10: Perintah Shell & Scripting?**
