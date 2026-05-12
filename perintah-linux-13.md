# Bagian 13: Perintah Pencarian (Search)

---

## 13.1 Find - Pencarian File & Direktori

### `find` - Find Files (Lengkap)
```bash
find [path] [ekspresi]
```
> Perintah pencarian file paling powerful di Linux

---

### Pencarian Berdasarkan Nama

```bash
# Nama tepat
find . -name "file.txt"              # Cari file bernama file.txt
find . -name "*.txt"                 # Semua file .txt
find . -name "*.log"                 # Semua file .log
find . -name "data*"                 # Nama diawali "data"
find . -name "*backup*"              # Nama mengandung "backup"
find . -name "*.{txt,log,csv}"       # Beberapa ekstensi

# Case-insensitive
find . -iname "file.txt"             # Tidak peduli huruf besar/kecil
find . -iname "*.TXT"                # .TXT, .txt, .Txt, dll.
find . -iname "*.jpg"                # .jpg, .JPG, .Jpg

# Path matching
find . -path "*/direktori/*.txt"     # Path mengandung pola
find . -path "*/.git*"               # File dalam .git
find . -ipath "*node_modules*"       # Path case-insensitive

# Regex
find . -regex ".*/[0-9]+\.txt"       # Nama berupa angka.txt
find . -iregex ".*\.(jpg|png|gif)"  # Ekstensi gambar
find . -regextype posix-extended -regex ".*\.(jpg|png)"

# Inverse matching
find . ! -name "*.txt"               # Bukan file .txt
find . -not -name "*.log"            # Bukan file .log
find . ! -name "*.txt" ! -name "*.log"  # Bukan .txt dan bukan .log
```

---

### Pencarian Berdasarkan Tipe

```bash
find . -type f                       # File biasa
find . -type d                       # Direktori
find . -type l                       # Symbolic link
find . -type b                       # Block device
find . -type c                       # Character device
find . -type p                       # Named pipe (FIFO)
find . -type s                       # Socket
find . -type f -o -type d            # File ATAU direktori
find . -type l -xtype f              # Symlink yang menunjuk ke file
find . -type l -xtype d              # Symlink yang menunjuk ke direktori
find . -L -type l                    # Broken symlink
find . -xtype l                      # Symlink yang rusak
```

---

### Pencarian Berdasarkan Ukuran

```bash
# Ukuran tepat
find . -size 100c                    # Tepat 100 bytes
find . -size 100k                    # Tepat 100 KB
find . -size 100M                    # Tepat 100 MB
find . -size 1G                      # Tepat 1 GB

# Lebih besar dari
find . -size +100k                   # Lebih besar dari 100 KB
find . -size +10M                    # Lebih besar dari 10 MB
find . -size +1G                     # Lebih besar dari 1 GB

# Lebih kecil dari
find . -size -100k                   # Lebih kecil dari 100 KB
find . -size -1M                     # Lebih kecil dari 1 MB

# Range
find . -size +1M -size -100M        # Antara 1MB dan 100MB
find . -size +10k -size -1M         # Antara 10KB dan 1MB

# File kosong
find . -empty                        # File atau direktori kosong
find . -type f -empty                # File kosong
find . -type d -empty                # Direktori kosong

# Unit ukuran:
# c = bytes
# k = kilobytes (1024 bytes)
# M = megabytes
# G = gigabytes
# b = 512-byte blocks (default)
# w = 2-byte words
```

---

### Pencarian Berdasarkan Waktu

```bash
# Modification time (mtime) - waktu konten diubah
find . -mtime 0                      # Diubah hari ini (dalam 24 jam terakhir)
find . -mtime -7                     # Diubah dalam 7 hari terakhir
find . -mtime +30                    # Diubah lebih dari 30 hari lalu
find . -mtime +7 -mtime -30         # Antara 7-30 hari lalu
find . -mmin -60                     # Diubah dalam 60 menit terakhir
find . -mmin +120                    # Diubah lebih dari 120 menit lalu

# Access time (atime) - waktu terakhir diakses
find . -atime -7                     # Diakses dalam 7 hari terakhir
find . -atime +30                    # Tidak diakses lebih dari 30 hari
find . -amin -60                     # Diakses dalam 60 menit terakhir

# Change time (ctime) - waktu metadata berubah
find . -ctime -7                     # Metadata berubah dalam 7 hari
find . -ctime +30                    # Metadata berubah lebih dari 30 hari

# Newer than file
find . -newer file.txt               # Lebih baru dari file.txt
find . -newer /etc/passwd            # Lebih baru dari passwd
find . -not -newer file.txt          # Lebih lama dari file.txt
find . -mnewer file.txt              # mtime lebih baru
find . -anewer file.txt              # atime lebih baru
find . -cnewer file.txt              # ctime lebih baru

# Tanggal tertentu
find . -newermt "2024-01-15"         # Diubah setelah tanggal
find . -newermt "2024-01-01" ! -newermt "2024-02-01"  # Januari 2024
find . -newermt "2024-01-15 10:00"  # Setelah jam tertentu

# Contoh praktis
find /var/log -mtime +7 -name "*.log"  # Log lama
find /tmp -atime +7                    # File tidak terpakai di /tmp
find /home -mtime -1 -type f          # File yang baru diubah
```

---

### Pencarian Berdasarkan Permission & Ownership

```bash
# Permission oktal
find . -perm 644                     # Permission tepat 644
find . -perm 755                     # Permission tepat 755
find . -perm -644                    # Minimal permission 644
find . -perm /644                    # Salah satu bit permission 644 set

# Permission simbolik
find . -perm -u=r                    # User bisa read
find . -perm -u=w                    # User bisa write
find . -perm -u=x                    # User bisa execute
find . -perm -g=w                    # Group bisa write
find . -perm -o=r                    # Others bisa read
find . -perm /u+x                    # User executable
find . -perm /a+x                    # Siapapun bisa execute

# Special permissions
find . -perm -4000                   # SUID bit
find . -perm -2000                   # SGID bit
find . -perm -1000                   # Sticky bit
find . -perm /6000                   # SUID atau SGID
find . -perm 4755                    # SUID + rwxr-xr-x

# World-writable (risiko keamanan)
find / -perm -002 -type f            # File yang bisa ditulis semua
find / -perm -002 -type d            # Direktori yang bisa ditulis semua
find / -perm -002 ! -type l          # Bukan symlink

# Ownership
find . -user username                # Dimiliki user tertentu
find . -user root                    # Dimiliki root
find . -uid 1000                     # UID tertentu
find . -group groupname              # Dimiliki grup tertentu
find . -gid 1000                     # GID tertentu
find . -nouser                       # Tidak punya owner (orphan)
find . -nogroup                      # Tidak punya group

# Kombinasi
find / -user root -perm -4000        # File SUID milik root (potensial risiko)
find /home -user john -type f        # File milik john
find . -user john -not -group john   # File john tapi grup bukan john
```

---

### Pencarian dengan Aksi (Actions)

```bash
# Print (default)
find . -name "*.txt" -print          # Cetak path
find . -name "*.txt" -print0         # Null-terminated (aman untuk spasi)
find . -name "*.txt" -printf "%p\n"  # Format kustom
find . -name "*.txt" -printf "%f\n"  # Hanya nama file
find . -name "*.txt" -printf "%s %p\n"  # Ukuran + path
find . -name "*.txt" -printf "%TY-%Tm-%Td %p\n"  # Tanggal + path

# List detail
find . -name "*.txt" -ls             # Seperti ls -l

# Delete
find . -name "*.tmp" -delete         # Hapus file .tmp
find . -type d -empty -delete        # Hapus direktori kosong
find /tmp -atime +7 -delete          # Hapus file lama di /tmp

# Exec - jalankan perintah untuk setiap hasil
find . -name "*.txt" -exec cat {} \;         # Tampilkan isi
find . -name "*.log" -exec rm {} \;          # Hapus
find . -type f -exec chmod 644 {} \;         # Ubah permission
find . -type d -exec chmod 755 {} \;         # Ubah permission direktori
find . -name "*.txt" -exec cp {} /backup/ \; # Copy ke backup

# Exec dengan konfirmasi
find . -name "*.log" -exec rm -i {} \;       # Konfirmasi sebelum hapus

# Exec dengan + (lebih efisien - batch)
find . -name "*.txt" -exec cat {} +          # Batch (lebih cepat)
find . -name "*.log" -exec rm {} +           # Hapus batch
find . -type f -exec chmod 644 {} +          # Batch chmod

# Exec dengan sh -c (untuk perintah kompleks)
find . -name "*.txt" -exec sh -c 'echo "File: $1"; wc -l "$1"' _ {} \;
find . -name "*.log" -exec sh -c 'gzip "$1" && echo "Compressed: $1"' _ {} \;

# Exec dengan kondisi
find . -name "*.txt" -exec grep -l "error" {} \;  # File .txt yang mengandung "error"

# xargs (lebih cepat untuk banyak file)
find . -name "*.txt" -print0 | xargs -0 cat
find . -name "*.log" -print0 | xargs -0 rm
find . -name "*.txt" -print0 | xargs -0 grep "pattern"
find . -type f -print0 | xargs -0 chmod 644
find . -name "*.txt" -print0 | xargs -0 -I {} cp {} /backup/

# OK/false (untuk kondisi dalam find)
find . -name "*.txt" -ok rm {} \;           # rm dengan konfirmasi Y/N
```

---

### Pencarian dengan Logika

```bash
# AND (implicit - default)
find . -name "*.txt" -size +1k       # .txt DAN lebih dari 1KB
find . -type f -mtime -7             # File biasa DAN < 7 hari

# AND eksplisit
find . -name "*.txt" -a -size +1k
find . -name "*.txt" -and -size +1k

# OR
find . -name "*.txt" -o -name "*.log"     # .txt ATAU .log
find . -name "*.txt" -or -name "*.log"
find . \( -name "*.txt" -o -name "*.log" \) -mtime -7  # Grup dengan OR

# NOT
find . ! -name "*.txt"                    # Bukan .txt
find . -not -name "*.txt"                 # Sama
find . ! -type d                          # Bukan direktori
find . \( -name "*.txt" -o -name "*.log" \) ! -empty  # .txt atau .log yang tidak kosong

# Grup dengan parenthesis
find . \( -name "*.jpg" -o -name "*.png" \) -size +1M  # Gambar besar
find . \( -user john -o -user jane \) -mtime -7        # File john atau jane yang baru
find . ! \( -name "*.txt" -o -name "*.log" \) -type f  # Bukan txt/log
```

---

### Opsi Tambahan find

```bash
# Kedalaman pencarian
find . -maxdepth 1                   # Hanya level 1 (tidak rekursif)
find . -maxdepth 2                   # Sampai 2 level
find . -mindepth 2                   # Mulai dari level 2
find . -mindepth 2 -maxdepth 4      # Level 2-4
find . -maxdepth 0                   # Hanya path yang diberikan

# Filesystem
find . -mount                        # Jangan cross mount point
find . -xdev                         # Sama dengan -mount
find /home -xdev -name "*.log"       # Hanya di filesystem /home

# Follow symlinks
find -L . -type f                    # Ikuti symlink
find -P . -type l                    # Jangan ikuti symlink (default)
find -H . -type f                    # Ikuti symlink di command line saja

# Optimasi
find . -name "*.txt" -prune          # Prune (skip) direktori
find . -name ".git" -prune -o -name "*.py" -print  # Skip .git
find . -name "node_modules" -prune -o -type f -print

# Contoh kompleks
# Cari file PHP yang bisa ditulis oleh world (security check)
find /var/www -name "*.php" -perm /o+w -ls

# Cari file besar yang tidak diakses dalam 30 hari
find /home -type f -size +100M -atime +30 -printf "%s %p\n" | \
    sort -rn | head -20

# Backup file yang berubah dalam 24 jam
find /etc -mtime -1 -type f -exec cp {} /backup/etc/ \;

# Hapus file log lama dan kosong
find /var/log -name "*.log" \( -empty -o -mtime +30 \) -delete

# Hitung total ukuran file .jpg
find . -name "*.jpg" -printf "%s\n" | awk '{sum+=$1} END {print sum/1024/1024 " MB"}'
```

---

## 13.2 Grep - Pencarian Teks

### `grep` - Global Regular Expression Print (Lengkap)
```bash
grep [opsi] <pola> [file...]
```
> Mencari pola teks dalam file atau input

---

### Pencarian Dasar

```bash
# Pencarian sederhana
grep "kata" file.txt                 # Cari "kata" dalam file
grep "kata" file1.txt file2.txt      # Beberapa file
grep "kata" *.txt                    # Semua file .txt
grep "kata" .                        # File di direktori saat ini

# Case sensitivity
grep "kata" file.txt                 # Case-sensitive (default)
grep -i "kata" file.txt              # Case-insensitive
grep -i "KATA" file.txt              # Sama dengan di atas

# Tipe matching
grep -w "kata" file.txt              # Kata utuh (whole word)
grep -x "baris ini" file.txt        # Seluruh baris cocok
grep -F "literal.string" file.txt   # Fixed string (bukan regex)
grep -E "pola+" file.txt            # Extended regex (ERE)
grep -P "\d+" file.txt              # Perl regex (PCRE)

# Output control
grep -n "kata" file.txt              # Tampilkan nomor baris
grep -l "kata" *.txt                 # Hanya nama file yang cocok
grep -L "kata" *.txt                 # File yang TIDAK cocok
grep -c "kata" file.txt              # Hitung baris yang cocok
grep -o "kata" file.txt              # Hanya bagian yang cocok
grep -h "kata" *.txt                 # Tanpa nama file prefix
grep -H "kata" file.txt              # Selalu tampilkan nama file
grep -q "kata" file.txt              # Quiet (hanya exit code)
```

---

### Context Output

```bash
grep -A 3 "error" log.txt            # 3 baris SETELAH kecocokan
grep -B 3 "error" log.txt            # 3 baris SEBELUM kecocokan
grep -C 3 "error" log.txt            # 3 baris SEBELUM dan SESUDAH
grep -A 5 -B 2 "CRITICAL" log.txt    # 5 setelah, 2 sebelum

# Separator antar konteks
grep --group-separator="---" -A 2 "error" log.txt
grep --no-group-separator -A 2 "error" log.txt  # Tanpa separator
```

---

### Pencarian Rekursif

```bash
grep -r "kata" /path/                # Rekursif
grep -R "kata" /path/                # Rekursif + ikuti symlink
grep -r "kata" --include="*.py" /path/   # Hanya file .py
grep -r "kata" --include="*.{py,js}" /path/  # .py dan .js
grep -r "kata" --exclude="*.log" /path/  # Kecuali .log
grep -r "kata" --exclude-dir=".git" /path/  # Kecuali direktori .git
grep -r "kata" --exclude-dir={".git","node_modules"} /path/

# Kombinasi
grep -rn "TODO" --include="*.py" ./src/  # Cari TODO dalam Python
grep -ril "password" /var/www/           # File yang mengandung "password" (insensitive)
```

---

### Regular Expression dengan grep

```bash
# Anchors
grep "^baris" file.txt               # Baris yang dimulai dengan "baris"
grep "akhir$" file.txt               # Baris yang diakhiri "akhir"
grep "^$" file.txt                   # Baris kosong
grep "^[^#]" file.txt               # Baris yang bukan komentar

# Character classes
grep "[0-9]" file.txt               # Baris mengandung angka
grep "[a-z]" file.txt               # Baris mengandung huruf kecil
grep "[A-Z]" file.txt               # Baris mengandung huruf besar
grep "[a-zA-Z]" file.txt            # Baris mengandung huruf
grep "[^a-z]" file.txt              # Bukan huruf kecil
grep "[[:digit:]]" file.txt         # Angka (POSIX class)
grep "[[:alpha:]]" file.txt         # Huruf
grep "[[:alnum:]]" file.txt         # Huruf dan angka
grep "[[:space:]]" file.txt         # Spasi/tab
grep "[[:upper:]]" file.txt         # Huruf besar
grep "[[:lower:]]" file.txt         # Huruf kecil
grep "[[:punct:]]" file.txt         # Tanda baca

# Quantifiers (BRE)
grep "ab*c" file.txt                # a, lalu 0+ b, lalu c
grep "ab\+c" file.txt               # a, lalu 1+ b, lalu c (BRE)
grep "ab\?c" file.txt               # a, lalu 0 atau 1 b, lalu c (BRE)
grep "a\{3\}" file.txt              # aaa (BRE)
grep "a\{2,4\}" file.txt            # aa, aaa, atau aaaa (BRE)

# Quantifiers (ERE dengan -E)
grep -E "ab+c" file.txt             # a, lalu 1+ b, lalu c
grep -E "ab?c" file.txt             # a, lalu 0 atau 1 b, lalu c
grep -E "a{3}" file.txt             # aaa
grep -E "a{2,4}" file.txt           # aa, aaa, atau aaaa
grep -E "a{2,}" file.txt            # aa atau lebih

# Alternation (ERE)
grep -E "cat|dog" file.txt          # "cat" atau "dog"
grep -E "(cat|dog)s?" file.txt      # cats, cat, dogs, atau dog
grep -E "^(error|warning):" log.txt # Baris dimulai error: atau warning:

# Dot (any character)
grep "a.c" file.txt                 # abc, aXc, a c, dll.
grep "a.*c" file.txt                # a diikuti apapun lalu c
grep "^.{10}$" file.txt            # Baris tepat 10 karakter (ERE)

# Perl Regex (dengan -P)
grep -P "\d{4}-\d{2}-\d{2}" file.txt   # Format tanggal YYYY-MM-DD
grep -P "\b\d{3}-\d{4}\b" file.txt     # Nomor telepon
grep -P "^(?!.*error)" log.txt          # Baris yang tidak mengandung error
grep -P "(?i)password\s*=" config.txt  # Password assignment (insensitive)
grep -P "\bIP\b.*(?:\d{1,3}\.){3}\d{1,3}" file.txt  # Baris dengan IP
```

---

### Multiple Patterns

```bash
# Multiple -e patterns (OR)
grep -e "error" -e "warning" -e "critical" log.txt
grep -E "error|warning|critical" log.txt    # Sama dengan ERE

# Dari file pattern
cat patterns.txt
# error
# warning
# critical
grep -f patterns.txt log.txt         # Gunakan pattern dari file
grep -if patterns.txt log.txt        # Case-insensitive dari file

# AND (gunakan pipe)
grep "error" log.txt | grep "database"  # Baris dengan error DAN database
grep "error" log.txt | grep -v "minor"  # Error tapi bukan minor error
```

---

### Pencarian Biner

```bash
grep -a "teks" file.bin              # Perlakukan binary sebagai teks
grep -I "teks" file.bin              # Abaikan binary files
grep -U "teks" file.txt              # Mode binary (tidak strip CR)
grep --binary-files=text "teks" file # Sama dengan -a
grep --binary-files=without-match "teks" *.  # Skip binary
```

---

### Variasi grep

```bash
# egrep = grep -E (Extended Regex)
egrep "pola+" file.txt
egrep "(cat|dog)s?" file.txt
egrep "\b[0-9]{4}\b" file.txt

# fgrep = grep -F (Fixed string, tanpa regex)
fgrep "literal.string" file.txt      # Titik bukan wildcard
fgrep "price: $100" file.txt        # Dolar bukan spesial

# zgrep (untuk file gz)
zgrep "error" file.txt.gz
zgrep -i "warning" /var/log/syslog.1.gz

# bzgrep (untuk file bz2)
bzgrep "error" file.txt.bz2

# xzgrep (untuk file xz)
xzgrep "error" file.txt.xz

# rgrep (rekursif)
rgrep "pattern" /path/               # Sama dengan grep -r
```

---

### Contoh Penggunaan Praktis grep

```bash
# Cari IP address
grep -E "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b" access.log

# Cari email address
grep -E "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" file.txt

# Cari URL
grep -E "https?://[^\s]+" file.txt

# Cari nomor kartu kredit
grep -P "\b\d{4}[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}\b" file.txt

# Cari baris yang mengandung angka
grep -E "[0-9]+" file.txt

# Cari konfigurasi yang tidak dikomentari
grep -v "^#" /etc/mysql/mysql.conf | grep -v "^$"

# Cari TODO/FIXME dalam code
grep -rn "TODO\|FIXME\|HACK\|XXX" --include="*.py" ./

# Statistik error dari log
grep -c "ERROR" /var/log/app.log

# Top 10 IP dari access log
grep -E "^[0-9.]+" access.log | grep -oE "^[0-9.]+" | \
    sort | uniq -c | sort -rn | head -10

# Cari file konfigurasi yang mengandung password
grep -r "password\s*=" /etc/ 2>/dev/null | grep -v "^Binary"

# Cari proses yang berjalan
ps aux | grep -v grep | grep nginx

# Monitor log real-time
tail -f /var/log/syslog | grep --line-buffered "error"
```

---

## 13.3 Locate - Pencarian Cepat

### `locate` - Fast File Search
```bash
locate [opsi] <pola>
```
> Mencari file menggunakan database index (jauh lebih cepat dari find)

```bash
# Pencarian dasar
locate file.txt                      # Cari file.txt
locate "*.txt"                       # Cari semua .txt (gunakan quotes)
locate nginx.conf                    # Cari konfigurasi nginx
locate -b "file.txt"                 # Hanya nama file (basename)
locate -i "file.txt"                 # Case-insensitive
locate -c "*.log"                    # Hitung hasil
locate -l 10 "*.conf"               # Batasi 10 hasil
locate -n 10 "*.conf"               # Sama dengan -l
locate -q "*.txt"                    # Quiet (tidak tampilkan error)
locate -S                            # Statistik database
locate -r "\.txt$"                   # Gunakan regex
locate -r "^/home.*\.py$"            # File .py di /home
locate -e "*.txt"                    # Verifikasi file masih ada
locate -0 "*.txt"                    # Null-terminated output
locate --existing "*.log"            # Hanya yang masih ada

# Update database
sudo updatedb                        # Update database locate
sudo updatedb --verbose              # Verbose
sudo updatedb --prunepaths="/proc /sys /dev /tmp"  # Kecuali path ini
cat /etc/updatedb.conf               # Konfigurasi updatedb

# Konfigurasi /etc/updatedb.conf
cat /etc/updatedb.conf
# PRUNEPATHS="/tmp /var/spool /media /home/.ecryptfs"
# PRUNEFS="NFS nfs nfs4 rpc_pipefs afs binfmt_misc proc smbfs autofs iso9660 ncpfs coda devpts ftpfs devfs mfs shfs sysfs cifs lustre tmpfs usbfs udf fuse.glusterfs fuse.sshfs ecryptfs fusesmb"
# PRUNE_BIND_MOUNTS="yes"
```

---

### `mlocate` & `plocate`

```bash
# mlocate (biasanya menggantikan locate)
sudo apt install mlocate
sudo updatedb                        # Update database

# plocate (jauh lebih cepat dari mlocate)
sudo apt install plocate
plocate "*.txt"                      # Pencarian sangat cepat
plocate -i "readme"                  # Case-insensitive
plocate -c "*.log"                   # Hitung hasil
plocate -l 5 "nginx"                # Batasi 5 hasil
plocate -A "config" "nginx"         # AND: mengandung keduanya
plocate -r "\.py$"                  # Regex
sudo updatedb                        # Update database plocate juga
```

---

## 13.4 Which, Whereis, Type

### `which` - Locate Command
```bash
which <perintah>
```
> Menemukan lokasi executable perintah dalam PATH

```bash
which ls                             # Lokasi ls
which python3                        # Lokasi python3
which -a python                      # Semua lokasi python dalam PATH
which bash zsh fish                  # Lokasi beberapa shell
which nginx                          # Lokasi nginx
which -a java                        # Semua java dalam PATH

# Cek apakah perintah ada
if which nginx &>/dev/null; then
    echo "nginx tersedia"
fi

# Atau lebih baik:
if command -v nginx &>/dev/null; then
    echo "nginx tersedia"
fi
```

---

### `whereis` - Locate Binary, Source, Manual
```bash
whereis <perintah>
```
> Mencari binary, source code, dan manual page

```bash
whereis ls                           # Semua lokasi ls
whereis python3                      # Semua lokasi python3
whereis -b ls                        # Hanya binary
whereis -m ls                        # Hanya manual page
whereis -s ls                        # Hanya source code
whereis -u ls                        # Unusual (tidak punya semua tiga)
whereis -b -u *                      # Binary tidak biasa
whereis -l                           # Daftar direktori yang dicari
whereis -B /usr/bin -f ls            # Cari binary di direktori tertentu

# Output contoh:
# ls: /bin/ls /usr/share/man/man1/ls.1.gz
```

---

### `type` - Command Type
```bash
type <perintah>
```
> Menampilkan tipe perintah (alias, builtin, external, function)

```bash
type ls                              # ls is aliased to 'ls --color=auto'
type cd                              # cd is a shell builtin
type python3                         # python3 is /usr/bin/python3
type ll                              # ll is aliased to 'ls -la'
type -a ls                           # Semua definisi ls
type -t ls                           # Hanya tipe (alias, builtin, file, keyword, function)
type -P ls                           # Hanya path (seperti which)
type -f ls                           # Abaikan fungsi
type -a python                       # Semua definisi python

# Tipe yang mungkin:
# alias    → Perintah adalah alias
# builtin  → Shell builtin
# file     → Program eksternal
# keyword  → Shell keyword
# function → Shell function
```

---

## 13.5 Pencarian Teks Lanjutan

### `ripgrep` (`rg`) - Modern Fast Grep
```bash
sudo apt install ripgrep
# atau
cargo install ripgrep
```
> Grep yang sangat cepat, modern, dan user-friendly

```bash
# Pencarian dasar
rg "kata" file.txt                   # Cari dalam file
rg "kata" /path/                     # Rekursif (default)
rg "kata"                            # Rekursif dari direktori saat ini

# Opsi umum
rg -i "kata"                         # Case-insensitive
rg -w "kata"                         # Whole word
rg -n "kata"                         # Tampilkan nomor baris
rg -l "kata"                         # Hanya nama file
rg -c "kata"                         # Hitung per file
rg -v "kata"                         # Inverse match
rg -o "kata"                         # Hanya bagian yang cocok
rg -e "pola1" -e "pola2"            # Multiple patterns (OR)
rg -f patterns.txt                   # Pattern dari file

# Context
rg -A 3 "error"                      # 3 baris setelah
rg -B 3 "error"                      # 3 baris sebelum
rg -C 3 "error"                      # 3 baris sekitar

# Filter file
rg "kata" --type py                  # Hanya file Python
rg "kata" --type-add "conf:*.conf" --type conf  # Tipe kustom
rg "kata" -g "*.txt"                 # Glob pattern
rg "kata" -g "!*.log"               # Kecuali .log
rg "kata" --include "*.py"           # Include glob
rg "kata" --exclude "*.log"          # Exclude glob
rg "kata" -g "!{.git,node_modules}"  # Kecuali direktori

# Regex
rg -e "^\d{4}-\d{2}-\d{2}" log.txt # Format tanggal
rg "(?i)error" log.txt              # Case insensitive regex
rg -P "\d{4}" file.txt              # PCRE2 regex
rg --fixed-strings "literal.string" # Fixed string

# Output
rg "kata" --json                     # JSON output
rg "kata" --vimgrep                  # Format vimgrep
rg "kata" --no-heading               # Tanpa header file
rg "kata" --heading                  # Dengan header file (default)
rg "kata" --line-number              # Dengan nomor baris
rg "kata" --column                   # Dengan nomor kolom
rg "kata" --stats                    # Tampilkan statistik
rg "kata" -q                         # Quiet

# Advanced
rg "kata" --follow                   # Ikuti symlink
rg "kata" -L                         # Sama dengan --follow
rg "kata" --hidden                   # Cari file hidden juga
rg "kata" --no-ignore                # Abaikan .gitignore
rg "kata" --no-ignore-vcs            # Abaikan VCS ignore files
rg "kata" -u                         # Unrestricted (abaikan semua ignore)
rg "kata" -uu                        # Abaikan ignore + search hidden
rg "kata" -uuu                       # Cari semua termasuk binary
rg "kata" --max-depth 3              # Maksimum kedalaman 3
rg "kata" --max-count 5              # Maksimum 5 kecocokan per file
rg "kata" -m 5                       # Sama dengan --max-count
rg "kata" --max-filesize 1M          # Skip file > 1MB
rg "kata" --multiline                # Multiline mode
rg "kata" -U                         # Sama
rg "kata" --pre <script>             # Preprocessing per file
rg "kata" --sort path                # Sort by path
rg "kata" --sortr modified           # Sort by modified time (reverse)

# Tipe file yang tersedia
rg --type-list                       # Daftar semua tipe

# Contoh praktis
rg "TODO|FIXME|HACK" --type py      # Cari TODO dalam Python
rg -l "import requests"              # File Python yang import requests
rg "password" --type yaml -i        # Password dalam YAML
rg "console\.log" --type js         # console.log dalam JS
rg "def.*auth" --type py            # Fungsi auth dalam Python
```

---

### `ag` - The Silver Searcher
```bash
sudo apt install silversearcher-ag
```
> Alternatif grep yang cepat (lebih lama dari rg tapi masih populer)

```bash
ag "kata"                            # Cari rekursif
ag "kata" /path/                     # Di path tertentu
ag -i "kata"                         # Case-insensitive
ag -w "kata"                         # Whole word
ag -l "kata"                         # Hanya nama file
ag -c "kata"                         # Hitung
ag -n "kata"                         # Nomor baris
ag -v "kata"                         # Inverse
ag -A 3 "kata"                       # Context after
ag -B 3 "kata"                       # Context before
ag -C 3 "kata"                       # Context both
ag -G "\.py$" "kata"                # Hanya file .py
ag --python "kata"                   # Hanya file Python
ag --js "kata"                       # Hanya file JavaScript
ag --ignore "*.log" "kata"           # Kecuali .log
ag --ignore-dir ".git" "kata"        # Kecuali direktori .git
ag -Q "literal.string"               # Fixed string
ag --hidden "kata"                   # Termasuk hidden files
ag -u "kata"                         # Unrestricted
ag --stats "kata"                    # Statistik
ag -0 "kata" | xargs -0 grep -l ""  # Null-terminated

# Contoh
ag "TODO" --python                   # TODO dalam Python files
ag -l "import numpy"                 # File yang import numpy
```

---

### `fd` - Modern Find Alternative
```bash
sudo apt install fd-find
# Binary mungkin bernama 'fdfind' atau 'fd'
ln -s $(which fdfind) ~/.local/bin/fd
```
> Alternatif find yang lebih cepat dan user-friendly

```bash
# Pencarian dasar
fd "pattern"                         # Cari file dengan pola
fd "pattern" /path/                  # Di path tertentu
fd -e txt                            # File dengan ekstensi .txt
fd -e txt -e log                    # .txt atau .log

# Tipe
fd -t f "pattern"                    # Hanya file
fd -t d "pattern"                    # Hanya direktori
fd -t l "pattern"                    # Hanya symlink
fd -t x "pattern"                    # Executable files
fd -t e "pattern"                    # Empty files/dirs

# Case
fd -i "pattern"                      # Case-insensitive
fd -s "pattern"                      # Case-sensitive (default)

# Ukuran
fd --size +1M                        # Lebih dari 1MB
fd --size -10k                       # Lebih kecil 10KB
fd --size +1k --size -10M           # Range

# Waktu
fd --changed-within 1d               # Berubah dalam 1 hari
fd --changed-before 30d              # Berubah sebelum 30 hari

# Filter
fd -H "pattern"                      # Termasuk hidden files
fd -I "pattern"                      # Abaikan .gitignore
fd --no-ignore "pattern"             # Sama
fd -E ".git" "pattern"               # Kecuali .git
fd --exclude "node_modules" "pattern"  # Kecuali node_modules
fd -d 3 "pattern"                    # Maksimum 3 level

# Aksi
fd "pattern" -x cat                  # Exec cat untuk setiap hasil
fd -e log -x rm                      # Hapus semua .log
fd -t f -e jpg -x convert {} {.}.png  # Konversi jpg ke png
fd -t f --size +100M -x du -sh       # Ukuran file besar

# Output
fd "pattern" -0                      # Null-terminated
fd "pattern" -l                      # Long format (seperti ls -l)
fd "pattern" --color always          # Selalu berwarna
fd "pattern" -q                      # Quiet

# Regex
fd -r "^\d{4}_" photos/              # File dimulai dengan 4 angka
fd -r "\.(jpg|png|gif)$"             # Ekstensi gambar

# Contoh praktis
fd -e py -x wc -l                    # Hitung baris semua Python
fd -t f -e log --changed-before 7d -x rm  # Hapus log lama
fd -t d -e empty -x rmdir            # Hapus direktori kosong
fd "CMakeLists" -x sed -i 's/old/new/g'  # Edit banyak file
```

---

### `fzf` - Fuzzy Finder
```bash
sudo apt install fzf
```
> Pencarian interaktif fuzzy yang sangat berguna

```bash
# Pencarian dasar
fzf                                  # Pilih file interaktif
fzf --query "init"                   # Dengan query awal
cat file.txt | fzf                   # Pilih baris dari input

# Preview
fzf --preview "cat {}"               # Preview file yang dipilih
fzf --preview "bat {}"               # Preview dengan syntax highlight (bat)
fzf --preview "head -20 {}"         # Preview 20 baris pertama
fzf --preview "ls -la {}"           # Preview direktori

# Sorting dan matching
fzf --sort                           # Sort hasil
fzf --no-sort                        # Jangan sort
fzf --exact                          # Exact matching
fzf -e                               # Sama
fzf --algo=v1                        # Algoritma matching v1
fzf --algo=v2                        # Algoritma v2 (default)

# Output
fzf --multi                          # Pilih beberapa (Tab untuk mark)
fzf -m                               # Sama
fzf --print0                         # Null-terminated output
fzf -0                               # Sama
fzf --with-nth 1,2                   # Tampilkan field tertentu saja
fzf --nth 1,2                        # Field yang di-match

# Layout
fzf --height 40%                     # Tinggi 40% terminal
fzf --reverse                        # Prompt di atas
fzf --border                         # Border
fzf --border=rounded                 # Border rounded

# Key bindings
fzf --bind "ctrl-a:select-all"       # Ctrl+A pilih semua
fzf --bind "ctrl-d:deselect-all"    # Ctrl+D batal pilih
fzf --bind "ctrl-y:execute(echo {} | xclip)"  # Copy ke clipboard

# Integrasi dengan shell
# Di ~/.bashrc atau ~/.zshrc:

# Ctrl+R untuk history search
eval "$(fzf --bash)"  # Atau:
source /usr/share/doc/fzf/examples/key-bindings.bash

# Fuzzy cd
fcd() { cd "$(find . -type d | fzf)"; }

# Fuzzy open
fopen() { xdg-open "$(fzf)"; }

# Fuzzy edit
fedit() { nano "$(fzf)"; }

# Kill process interaktif
fkill() {
    ps aux | fzf --multi | awk '{print $2}' | xargs kill -9
}

# Git branch checkout dengan fzf
fbranch() {
    git branch | fzf | xargs git checkout
}

# Docker container select
fdocker() {
    docker ps | fzf --header-lines=1 | awk '{print $1}'
}

# Contoh penggunaan
ls | fzf                             # Pilih file
cat /etc/passwd | fzf                # Pilih user
git log --oneline | fzf             # Pilih commit
history | fzf                        # Pilih dari history
```

---

## 13.6 Pencarian Database & Konten

### Mencari dalam File Tertentu

```bash
# Cari dalam file konfigurasi
grep -r "ServerName" /etc/apache2/   # Konfigurasi Apache
grep -r "listen" /etc/nginx/         # Konfigurasi Nginx
grep -r "port" /etc/mysql/           # Konfigurasi MySQL

# Cari dalam code
grep -rn "def " --include="*.py" ./  # Fungsi Python
grep -rn "function " --include="*.js" ./  # Fungsi JavaScript
grep -rn "class " --include="*.java" ./   # Class Java
grep -rn "import " --include="*.py" ./    # Import Python

# Cari dalam log
grep -E "ERROR|FATAL" /var/log/app.log
grep "$(date '+%Y-%m-%d')" /var/log/app.log  # Log hari ini
grep "2024-01-15 10:" /var/log/app.log        # Log jam tertentu
```

---

### `xargs` dengan Pencarian

```bash
# Kombinasi find + xargs
find . -name "*.py" | xargs grep "import os"
find . -name "*.txt" -print0 | xargs -0 grep -l "error"
find . -type f -name "*.log" | xargs wc -l | sort -rn

# Kombinasi grep + xargs
grep -rl "old_function" --include="*.py" | xargs sed -i 's/old_function/new_function/g'
grep -rl "TODO" --include="*.py" | xargs -I {} sh -c 'echo "{}:"; grep -n "TODO" "{}"'

# Proses paralel
find . -name "*.jpg" | xargs -P 4 -I {} convert {} {}.png
```

---

### `ack` - Programmer's Grep
```bash
sudo apt install ack
```
> Grep yang dioptimalkan untuk programmer

```bash
ack "pattern"                        # Cari rekursif (skip binary, .git, dll.)
ack -i "pattern"                     # Case-insensitive
ack -w "pattern"                     # Whole word
ack -l "pattern"                     # Hanya nama file
ack -c "pattern"                     # Hitung
ack -A 3 "pattern"                   # Context after
ack -B 3 "pattern"                   # Context before
ack --python "pattern"               # Hanya Python
ack --js "pattern"                   # Hanya JavaScript
ack --type=python "pattern"          # Sama
ack --type-list                      # Daftar tipe
ack -f                               # Hanya tampilkan nama file yang akan dicari
ack -g "pattern"                     # Hanya tampilkan nama file
ack --ignore-dir=.git "pattern"      # Kecuali direktori
ack --ignore-file=match:.log "pola" # Kecuali file .log
ack -v "pattern"                     # Inverse
ack -Q "literal.string"              # Fixed string
ack -o "pattern"                     # Hanya output yang cocok
```

---

## 13.7 Pencarian Lanjutan & Kombinasi

### Kombinasi Pencarian

```bash
# Find + Grep
find /var/log -name "*.log" -exec grep -l "error" {} \;
find . -name "*.py" -exec grep -n "TODO" {} /dev/null \;
find . -name "*.txt" | xargs grep -l "keyword"

# Beberapa grep dalam pipe
grep "error" app.log | grep -v "debug" | grep -v "test"
grep "^2024-01-15" app.log | grep "ERROR" | awk '{print $3}'

# Find + Sort + Filter
find . -name "*.log" -printf "%T@ %p\n" | sort -rn | head -10 | cut -d' ' -f2-

# Cari file duplikat berdasarkan nama
find . -type f | sort | uniq -d

# Cari file duplikat berdasarkan konten (md5)
find . -type f -exec md5sum {} \; | sort | awk '$1 seen[$1]++ {print "DUPLIKAT:", $2}'

# Cari file dengan inode yang sama (hard links)
find . -type f ! -links 1 -printf "%i %n %p\n" | sort -n

# Cari symlink yang rusak
find . -type l ! -exec test -e {} \; -print

# Cari file executable yang SUID (security audit)
find / -perm -4000 -type f -ls 2>/dev/null

# Cari file konfigurasi yang berubah baru-baru ini
find /etc -mtime -7 -type f -ls

# Cari file besar
find / -type f -size +100M -printf "%s %p\n" 2>/dev/null | \
    sort -rn | head -20 | awk '{printf "%.2f GB\t%s\n", $1/1073741824, $2}'

# Cari semua file yang dimiliki user tertentu
find / -user username -ls 2>/dev/null

# Cari kata dalam semua file konfigurasi
grep -r "keyword" /etc/ 2>/dev/null | grep -v "^Binary"

# Analisis penggunaan disk per ekstensi
find . -type f | sed 's/.*\.//' | sort | uniq -c | sort -rn | head -20
```

---

### Script Pencarian Komprehensif

```bash
#!/bin/bash
# search.sh - Skrip pencarian komprehensif

USAGE="
Usage: $0 [OPTIONS]
  -n PATTERN    Cari berdasarkan nama file
  -t TEXT       Cari teks dalam file
  -s SIZE       Cari berdasarkan ukuran (contoh: +10M, -1k)
  -d DAYS       Diubah dalam N hari terakhir
  -p PATH       Path pencarian (default: .)
  -e EXT        Ekstensi file (contoh: py,js,txt)
  -h            Bantuan
"

PATH_SEARCH="."
NAME_PATTERN=""
TEXT_SEARCH=""
SIZE_FILTER=""
DAYS_FILTER=""
EXTENSIONS=""

while getopts "n:t:s:d:p:e:h" opt; do
    case $opt in
        n) NAME_PATTERN="$OPTARG" ;;
        t) TEXT_SEARCH="$OPTARG" ;;
        s) SIZE_FILTER="$OPTARG" ;;
        d) DAYS_FILTER="$OPTARG" ;;
        p) PATH_SEARCH="$OPTARG" ;;
        e) EXTENSIONS="$OPTARG" ;;
        h) echo "$USAGE"; exit 0 ;;
        *) echo "$USAGE"; exit 1 ;;
    esac
done

CMD="find $PATH_SEARCH -type f"

# Tambah filter nama
if [ -n "$NAME_PATTERN" ]; then
    CMD="$CMD -name '*${NAME_PATTERN}*'"
fi

# Tambah filter ekstensi
if [ -n "$EXTENSIONS" ]; then
    EXT_FILTER=""
    IFS=',' read -ra EXTS <<< "$EXTENSIONS"
    for ext in "${EXTS[@]}"; do
        [ -n "$EXT_FILTER" ] && EXT_FILTER="$EXT_FILTER -o"
        EXT_FILTER="$EXT_FILTER -name '*.${ext}'"
    done
    CMD="$CMD \( $EXT_FILTER \)"
fi

# Tambah filter ukuran
if [ -n "$SIZE_FILTER" ]; then
    CMD="$CMD -size $SIZE_FILTER"
fi

# Tambah filter waktu
if [ -n "$DAYS_FILTER" ]; then
    CMD="$CMD -mtime -$DAYS_FILTER"
fi

echo "Menjalankan: $CMD"
echo "================================"

# Cari file
FILES=$(eval "$CMD" 2>/dev/null)

if [ -z "$FILES" ]; then
    echo "Tidak ada file ditemukan"
    exit 0
fi

# Jika ada pencarian teks, filter dengan grep
if [ -n "$TEXT_SEARCH" ]; then
    echo "$FILES" | xargs grep -l "$TEXT_SEARCH" 2>/dev/null
else
    echo "$FILES"
fi

echo "================================"
echo "Total: $(echo "$FILES" | wc -l) file"
```

---

## Ringkasan Perintah Bagian 13

```
Pencarian File:
  find             → Pencarian file paling lengkap
  locate/plocate   → Pencarian cepat via database
  fd               → Alternatif find yang modern
  which            → Lokasi executable
  whereis          → Binary, source, manual
  type             → Tipe perintah

Pencarian Teks:
  grep             → Pencarian teks standar
  egrep            → grep dengan Extended Regex
  fgrep            → grep Fixed String
  zgrep            → grep untuk file gz
  rg (ripgrep)     → Pencarian teks paling cepat
  ag               → The Silver Searcher
  ack              → Grep untuk programmer
  fzf              → Fuzzy finder interaktif

Opsi grep Penting:
  -i               → Case-insensitive
  -r               → Rekursif
  -n               → Nomor baris
  -l               → Hanya nama file
  -c               → Hitung kecocokan
  -v               → Inverse match
  -w               → Whole word
  -A/B/C N         → Context lines
  -E               → Extended regex
  -P               → Perl regex
  -F               → Fixed string

Opsi find Penting:
  -name            → Berdasarkan nama
  -type            → Berdasarkan tipe (f,d,l)
  -size            → Berdasarkan ukuran
  -mtime/-atime    → Berdasarkan waktu
  -perm            → Berdasarkan permission
  -user/-group     → Berdasarkan kepemilikan
  -exec            → Jalankan perintah
  -print0          → Null-terminated output
  -delete          → Hapus hasil
  -maxdepth        → Batasi kedalaman
  ! / -not         → Negasi
  -o / -or         → OR kondisi
  -a / -and        → AND kondisi (default)
```

---

## ✅ Bagian 13 Selesai!

**Lanjut ke Bagian 14: Perintah Variabel Lingkungan & Konfigurasi?**
