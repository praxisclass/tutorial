# Bagian 2: Perintah Manajemen Teks & File Content

---

## 2.1 Menampilkan Isi File

### `cat` - Concatenate and Display File
```bash
cat [opsi] <file>
```
> Menampilkan isi file, menggabungkan file, atau membuat file baru

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `cat file.txt` | Tampilkan isi file |
| `cat file1.txt file2.txt` | Tampilkan beberapa file sekaligus |
| `cat -n file.txt` | Tampilkan dengan nomor baris |
| `cat -b file.txt` | Nomor baris hanya pada baris tidak kosong |
| `cat -s file.txt` | Gabungkan baris kosong berurutan menjadi satu |
| `cat -A file.txt` | Tampilkan karakter tersembunyi (`$` di akhir baris, `^I` untuk tab) |
| `cat -T file.txt` | Tampilkan tab sebagai `^I` |
| `cat -E file.txt` | Tampilkan `$` di akhir setiap baris |
| `cat -v file.txt` | Tampilkan karakter non-printing |
| `cat file1.txt file2.txt > gabungan.txt` | Gabungkan file ke file baru |
| `cat file1.txt >> file2.txt` | Tambahkan isi file1 ke akhir file2 |
| `cat > file.txt` | Buat file baru dengan input keyboard (Ctrl+D untuk selesai) |
| `cat >> file.txt` | Tambahkan teks ke akhir file |
| `cat /dev/null > file.txt` | Kosongkan isi file |

---

### `tac` - Reverse Cat
```bash
tac [opsi] <file>
```
> Menampilkan isi file secara terbalik (baris terakhir ditampilkan pertama)

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `tac file.txt` | Tampilkan file secara terbalik |
| `tac -s ":" file.txt` | Gunakan separator tertentu |
| `tac -r file.txt` | Separator sebagai regex |
| `tac file1.txt file2.txt` | Balikkan beberapa file |

---

### `more` - View File Page by Page (Basic)
```bash
more [opsi] <file>
```
> Menampilkan isi file per halaman (scroll ke bawah saja)

**Opsi & Navigasi:**
| Opsi/Tombol | Penjelasan |
|---|---|
| `more file.txt` | Buka file dengan more |
| `more -d file.txt` | Tampilkan pesan bantuan navigasi |
| `more -p file.txt` | Clear screen sebelum menampilkan tiap halaman |
| `more -s file.txt` | Gabungkan baris kosong berurutan |
| `more -n 20 file.txt` | Tampilkan 20 baris per halaman |
| `more +10 file.txt` | Mulai dari baris ke-10 |
| `more +/kata file.txt` | Mulai dari baris yang mengandung "kata" |
| **Space** | Halaman berikutnya |
| **Enter** | Satu baris berikutnya |
| **b** | Kembali satu halaman |
| **q** | Keluar |
| **/** | Cari teks |
| **h** | Bantuan |

---

### `less` - View File Page by Page (Advanced)
```bash
less [opsi] <file>
```
> Menampilkan isi file per halaman dengan navigasi lebih lengkap (bisa scroll atas/bawah)

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `less file.txt` | Buka file dengan less |
| `less -N file.txt` | Tampilkan nomor baris |
| `less -S file.txt` | Jangan wrap baris panjang (scroll horizontal) |
| `less -i file.txt` | Case-insensitive search |
| `less -F file.txt` | Keluar otomatis jika file muat dalam satu layar |
| `less -R file.txt` | Tampilkan warna ANSI |
| `less -X file.txt` | Jangan clear screen saat keluar |
| `less -p "kata" file.txt` | Buka dan langsung cari "kata" |
| `less +G file.txt` | Langsung ke akhir file |
| `less +F file.txt` | Mode follow (seperti `tail -f`) |
| `less -m file.txt` | Tampilkan persentase posisi |
| `less -e file.txt` | Keluar otomatis saat mencapai akhir file |

**Navigasi di dalam less:**
| Tombol | Penjelasan |
|---|---|
| **Space / PageDown** | Halaman berikutnya |
| **b / PageUp** | Halaman sebelumnya |
| **↑ / k** | Satu baris ke atas |
| **↓ / j** | Satu baris ke bawah |
| **g** | Ke awal file |
| **G** | Ke akhir file |
| **/<kata>** | Cari ke depan |
| **?<kata>** | Cari ke belakang |
| **n** | Hasil pencarian berikutnya |
| **N** | Hasil pencarian sebelumnya |
| **q** | Keluar |
| **h** | Bantuan |
| **F** | Mode follow (monitor file baru) |
| **=** | Tampilkan info file saat ini |
| **!<command>** | Jalankan perintah shell |
| **v** | Buka file di editor default |

---

### `head` - Display Beginning of File
```bash
head [opsi] <file>
```
> Menampilkan bagian awal file (default: 10 baris pertama)

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `head file.txt` | Tampilkan 10 baris pertama |
| `head -n 20 file.txt` | Tampilkan 20 baris pertama |
| `head -n -5 file.txt` | Tampilkan semua kecuali 5 baris terakhir |
| `head -c 100 file.txt` | Tampilkan 100 byte pertama |
| `head -c -100 file.txt` | Tampilkan semua kecuali 100 byte terakhir |
| `head -q file1.txt file2.txt` | Tampilkan tanpa header nama file |
| `head -v file.txt` | Selalu tampilkan header nama file |
| `head -5 file.txt` | Shorthand untuk `-n 5` |
| `head file1.txt file2.txt` | Tampilkan awal beberapa file |

---

### `tail` - Display End of File
```bash
tail [opsi] <file>
```
> Menampilkan bagian akhir file (default: 10 baris terakhir)

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `tail file.txt` | Tampilkan 10 baris terakhir |
| `tail -n 20 file.txt` | Tampilkan 20 baris terakhir |
| `tail -n +5 file.txt` | Tampilkan mulai dari baris ke-5 hingga akhir |
| `tail -c 100 file.txt` | Tampilkan 100 byte terakhir |
| `tail -f file.txt` | Follow mode: monitor file secara real-time |
| `tail -F file.txt` | Follow mode + retry jika file tidak ada |
| `tail -f --pid=1234 file.txt` | Follow hingga proses dengan PID tertentu selesai |
| `tail -q file1.txt file2.txt` | Tampilkan tanpa header nama file |
| `tail -v file.txt` | Selalu tampilkan header nama file |
| `tail -s 1 -f file.txt` | Follow dengan interval refresh 1 detik |

**Contoh Penggunaan `tail -f`:**
```bash
# Monitor log server secara real-time
tail -f /var/log/nginx/access.log

# Monitor beberapa log sekaligus
tail -f /var/log/syslog /var/log/auth.log

# Monitor 50 baris terakhir + follow
tail -n 50 -f /var/log/apache2/error.log
```

---

## 2.2 Mengedit Teks

### `nano` - Simple Text Editor
```bash
nano [opsi] <file>
```
> Editor teks sederhana untuk terminal

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `nano file.txt` | Buka/buat file |
| `nano -l file.txt` | Tampilkan nomor baris |
| `nano -m file.txt` | Aktifkan dukungan mouse |
| `nano -i file.txt` | Auto-indent |
| `nano -w file.txt` | Nonaktifkan word wrap |
| `nano -c file.txt` | Tampilkan posisi cursor |
| `nano -B file.txt` | Buat backup sebelum menyimpan |
| `nano +10 file.txt` | Buka di baris ke-10 |
| `nano -R file.txt` | Buka hanya untuk membaca |
| `nano -z file.txt` | Aktifkan suspend (Ctrl+Z) |

**Shortcut di dalam nano:**
| Shortcut | Penjelasan |
|---|---|
| **Ctrl+S** | Simpan file |
| **Ctrl+O** | Simpan dengan nama tertentu |
| **Ctrl+X** | Keluar |
| **Ctrl+K** | Potong (cut) baris |
| **Ctrl+U** | Tempel (paste) |
| **Ctrl+W** | Cari teks |
| **Ctrl+\\** | Cari dan ganti |
| **Ctrl+G** | Bantuan |
| **Ctrl+A** | Ke awal baris |
| **Ctrl+E** | Ke akhir baris |
| **Ctrl+Y** | Halaman sebelumnya |
| **Ctrl+V** | Halaman berikutnya |
| **Ctrl+C** | Tampilkan posisi cursor |
| **Alt+U** | Undo |
| **Alt+E** | Redo |
| **Alt+A** | Mulai seleksi |
| **Ctrl+6** | Salin seleksi |

---

### `vim` / `vi` - Advanced Text Editor
```bash
vim [opsi] <file>
```
> Editor teks powerful dengan mode perintah dan mode input

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `vim file.txt` | Buka/buat file |
| `vim +10 file.txt` | Buka di baris ke-10 |
| `vim +/kata file.txt` | Buka dan cari "kata" |
| `vim -R file.txt` | Read-only mode |
| `vim -d file1.txt file2.txt` | Diff mode (bandingkan dua file) |
| `vim -u NONE file.txt` | Buka tanpa konfigurasi |
| `vim -p file1.txt file2.txt` | Buka beberapa file dalam tab |
| `vim -o file1.txt file2.txt` | Buka beberapa file split horizontal |
| `vim -O file1.txt file2.txt` | Buka beberapa file split vertikal |

**Mode vim:**
```
Normal Mode  → Mode default (navigasi & perintah)
Insert Mode  → Mode mengetik teks (tekan i, a, o, dll.)
Visual Mode  → Mode seleksi teks (tekan v, V, Ctrl+V)
Command Mode → Mode perintah (tekan :)
```

**Perintah Dasar vim (Normal Mode):**
| Perintah | Penjelasan |
|---|---|
| `i` | Insert sebelum cursor |
| `a` | Insert setelah cursor |
| `I` | Insert di awal baris |
| `A` | Insert di akhir baris |
| `o` | Baris baru di bawah |
| `O` | Baris baru di atas |
| `Esc` | Kembali ke Normal Mode |
| `h j k l` | Navigasi (kiri, bawah, atas, kanan) |
| `gg` | Ke baris pertama |
| `G` | Ke baris terakhir |
| `nG` | Ke baris ke-n |
| `0` | Ke awal baris |
| `$` | Ke akhir baris |
| `w` | Maju satu kata |
| `b` | Mundur satu kata |
| `dd` | Hapus satu baris (cut) |
| `ndd` | Hapus n baris |
| `yy` | Copy satu baris (yank) |
| `nyy` | Copy n baris |
| `p` | Paste setelah cursor |
| `P` | Paste sebelum cursor |
| `u` | Undo |
| `Ctrl+R` | Redo |
| `x` | Hapus karakter di cursor |
| `r` | Ganti satu karakter |
| `cw` | Ganti satu kata |
| `/kata` | Cari ke depan |
| `?kata` | Cari ke belakang |
| `n` | Hasil pencarian berikutnya |
| `N` | Hasil pencarian sebelumnya |
| `%` | Loncat ke bracket pasangan |
| `>>` | Indent baris |
| `<<` | Unindent baris |

**Perintah Command Mode (diawali `:`):**
| Perintah | Penjelasan |
|---|---|
| `:w` | Simpan file |
| `:q` | Keluar |
| `:wq` atau `:x` | Simpan dan keluar |
| `:q!` | Keluar tanpa menyimpan |
| `:wq!` | Simpan paksa dan keluar |
| `:w namafile.txt` | Simpan dengan nama lain |
| `:e file.txt` | Buka file lain |
| `:set nu` | Tampilkan nomor baris |
| `:set nonu` | Sembunyikan nomor baris |
| `:set syntax=on` | Aktifkan syntax highlighting |
| `:%s/lama/baru/g` | Ganti semua "lama" dengan "baru" |
| `:%s/lama/baru/gc` | Ganti dengan konfirmasi |
| `:n,ms/lama/baru/g` | Ganti di baris n hingga m |
| `:10` | Loncat ke baris 10 |
| `:split file.txt` | Split horizontal |
| `:vsplit file.txt` | Split vertikal |
| `:tabnew file.txt` | Buka tab baru |
| `:tabn` | Tab berikutnya |
| `:tabp` | Tab sebelumnya |
| `:!perintah` | Jalankan perintah shell |
| `:r file.txt` | Sisipkan isi file |
| `:noh` | Matikan highlight pencarian |

---

### `gedit` - GUI Text Editor (GNOME)
```bash
gedit [opsi] <file>
```
> Editor teks grafis untuk lingkungan GNOME

| Perintah | Penjelasan |
|---|---|
| `gedit file.txt` | Buka file di gedit |
| `gedit file1.txt file2.txt` | Buka beberapa file |
| `gedit &` | Buka gedit di background |

---

## 2.3 Memproses Teks

### `grep` - Global Regular Expression Print
```bash
grep [opsi] <pola> <file>
```
> Mencari pola teks dalam file menggunakan regular expression

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `grep "kata" file.txt` | Cari "kata" dalam file |
| `grep -i "kata" file.txt` | Case-insensitive |
| `grep -v "kata" file.txt` | Tampilkan baris yang TIDAK mengandung "kata" |
| `grep -n "kata" file.txt` | Tampilkan nomor baris |
| `grep -c "kata" file.txt` | Hitung jumlah baris yang cocok |
| `grep -l "kata" *.txt` | Tampilkan hanya nama file yang cocok |
| `grep -L "kata" *.txt` | Tampilkan file yang TIDAK mengandung "kata" |
| `grep -r "kata" /path/` | Rekursif dalam direktori |
| `grep -r "kata" /path/ --include="*.txt"` | Rekursif hanya file .txt |
| `grep -r "kata" /path/ --exclude="*.log"` | Rekursif kecuali file .log |
| `grep -w "kata" file.txt` | Cocokkan kata utuh saja |
| `grep -x "baris" file.txt` | Cocokkan seluruh baris |
| `grep -A 3 "kata" file.txt` | Tampilkan 3 baris setelah kecocokan |
| `grep -B 3 "kata" file.txt` | Tampilkan 3 baris sebelum kecocokan |
| `grep -C 3 "kata" file.txt` | Tampilkan 3 baris sebelum dan sesudah |
| `grep -o "kata" file.txt` | Tampilkan hanya bagian yang cocok |
| `grep -m 5 "kata" file.txt` | Hentikan setelah 5 kecocokan |
| `grep -q "kata" file.txt` | Quiet mode (tidak ada output, hanya exit code) |
| `grep -e "pola1" -e "pola2" file.txt` | Beberapa pola (OR) |
| `grep -f pola.txt file.txt` | Baca pola dari file |
| `grep -P "\d+" file.txt` | Gunakan Perl regex |
| `grep -E "pola+" file.txt` | Extended regex (sama dengan egrep) |
| `grep -F "kata.literal" file.txt` | Fixed string (bukan regex) |
| `grep --color=auto "kata" file.txt` | Highlight kecocokan |

**Contoh Regex dengan grep:**
```bash
grep "^awal" file.txt          # Baris yang dimulai dengan "awal"
grep "akhir$" file.txt         # Baris yang diakhiri "akhir"
grep "^$" file.txt             # Baris kosong
grep "[0-9]" file.txt          # Baris mengandung angka
grep "[a-zA-Z]" file.txt       # Baris mengandung huruf
grep "a.*b" file.txt           # "a" diikuti karakter apapun lalu "b"
grep -E "cat|dog" file.txt     # Mengandung "cat" atau "dog"
grep -E "go{2,4}gle" file.txt  # "go" diulang 2-4 kali
grep -P "\b\d{4}\b" file.txt   # Angka 4 digit
```

---

### `egrep` dan `fgrep`
```bash
egrep "pola+" file.txt     # Sama dengan grep -E (extended regex)
fgrep "kata.literal" file.txt  # Sama dengan grep -F (fixed string)
```

---

### `sed` - Stream Editor
```bash
sed [opsi] 'perintah' <file>
```
> Mengedit teks secara non-interaktif (stream editing)

**Substitusi:**
| Perintah | Penjelasan |
|---|---|
| `sed 's/lama/baru/' file.txt` | Ganti kemunculan pertama di setiap baris |
| `sed 's/lama/baru/g' file.txt` | Ganti semua kemunculan |
| `sed 's/lama/baru/2' file.txt` | Ganti kemunculan ke-2 |
| `sed 's/lama/baru/gi' file.txt` | Ganti semua, case-insensitive |
| `sed -i 's/lama/baru/g' file.txt` | Edit file langsung (in-place) |
| `sed -i.bak 's/lama/baru/g' file.txt` | Edit in-place dengan backup .bak |
| `sed 's/lama/baru/g' file1.txt file2.txt` | Edit beberapa file |

**Menghapus Baris:**
| Perintah | Penjelasan |
|---|---|
| `sed '5d' file.txt` | Hapus baris ke-5 |
| `sed '5,10d' file.txt` | Hapus baris 5 sampai 10 |
| `sed '/kata/d' file.txt` | Hapus baris yang mengandung "kata" |
| `sed '/^$/d' file.txt` | Hapus baris kosong |
| `sed '/^#/d' file.txt` | Hapus baris komentar |
| `sed '1d' file.txt` | Hapus baris pertama |
| `sed '$d' file.txt` | Hapus baris terakhir |

**Mencetak Baris:**
| Perintah | Penjelasan |
|---|---|
| `sed -n '5p' file.txt` | Cetak hanya baris ke-5 |
| `sed -n '5,10p' file.txt` | Cetak baris 5 sampai 10 |
| `sed -n '/kata/p' file.txt` | Cetak baris yang mengandung "kata" |
| `sed -n '1p' file.txt` | Cetak baris pertama |
| `sed -n '$p' file.txt` | Cetak baris terakhir |

**Menyisipkan & Menambahkan:**
| Perintah | Penjelasan |
|---|---|
| `sed '3i\teks baru' file.txt` | Sisipkan teks sebelum baris ke-3 |
| `sed '3a\teks baru' file.txt` | Tambahkan teks setelah baris ke-3 |
| `sed '1i\header' file.txt` | Tambahkan header di awal file |
| `sed '$a\footer' file.txt` | Tambahkan footer di akhir file |

**Lainnya:**
```bash
sed 's/^/  /' file.txt         # Tambah 2 spasi di awal setiap baris
sed 's/ *$//' file.txt         # Hapus spasi di akhir baris
sed '=' file.txt               # Tampilkan nomor baris
sed -n '1~2p' file.txt         # Tampilkan baris ganjil (1,3,5,...)
sed -n '0~2p' file.txt         # Tampilkan baris genap (2,4,6,...)
sed 'y/abc/ABC/' file.txt      # Transliterasi karakter
```

---

### `awk` - Pattern Scanning and Processing
```bash
awk [opsi] 'program' <file>
```
> Bahasa pemrosesan teks yang powerful

**Dasar awk:**
| Perintah | Penjelasan |
|---|---|
| `awk '{print}' file.txt` | Cetak semua baris |
| `awk '{print $1}' file.txt` | Cetak kolom pertama |
| `awk '{print $2}' file.txt` | Cetak kolom kedua |
| `awk '{print $NF}' file.txt` | Cetak kolom terakhir |
| `awk '{print $1, $3}' file.txt` | Cetak kolom 1 dan 3 |
| `awk '{print NR, $0}' file.txt` | Cetak nomor baris + isi baris |
| `awk 'NR==5' file.txt` | Cetak baris ke-5 |
| `awk 'NR>=5 && NR<=10' file.txt` | Cetak baris 5-10 |
| `awk '/kata/' file.txt` | Cetak baris yang mengandung "kata" |
| `awk '!/kata/' file.txt` | Cetak baris yang TIDAK mengandung "kata" |
| `awk -F: '{print $1}' /etc/passwd` | Gunakan ":" sebagai delimiter |
| `awk -F"," '{print $1}' data.csv` | Baca file CSV |

**Variabel Bawaan awk:**
| Variabel | Penjelasan |
|---|---|
| `$0` | Seluruh baris saat ini |
| `$1, $2, ...` | Kolom ke-1, ke-2, dst. |
| `NF` | Jumlah kolom (field) di baris saat ini |
| `NR` | Nomor baris saat ini |
| `FS` | Field separator (default: spasi) |
| `RS` | Record separator (default: newline) |
| `OFS` | Output field separator |
| `ORS` | Output record separator |
| `FILENAME` | Nama file yang sedang diproses |

**Contoh Lanjutan:**
```bash
# Hitung total kolom ke-3
awk '{sum += $3} END {print "Total:", sum}' file.txt

# Filter dan format output
awk -F: '$3 >= 1000 {print $1, $3}' /etc/passwd

# Cetak baris dengan kondisi
awk '$2 > 100 {print $1, "->", $2}' data.txt

# Ubah separator output
awk 'BEGIN{OFS=","} {print $1, $2, $3}' file.txt

# Block BEGIN dan END
awk 'BEGIN{print "=== MULAI ==="} {print} END{print "=== SELESAI ==="}' file.txt

# Hitung jumlah baris
awk 'END{print NR}' file.txt

# Cetak baris duplikat
awk 'seen[$0]++' file.txt

# Cetak baris unik
awk '!seen[$0]++' file.txt
```

---

### `cut` - Remove Sections from Lines
```bash
cut [opsi] <file>
```
> Memotong bagian tertentu dari setiap baris

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `cut -c1-5 file.txt` | Ambil karakter 1 sampai 5 |
| `cut -c1 file.txt` | Ambil karakter pertama |
| `cut -c1,5,10 file.txt` | Ambil karakter ke-1, 5, dan 10 |
| `cut -c-5 file.txt` | Ambil karakter dari awal sampai ke-5 |
| `cut -c5- file.txt` | Ambil karakter dari ke-5 sampai akhir |
| `cut -d: -f1 /etc/passwd` | Gunakan ":" sebagai delimiter, ambil field ke-1 |
| `cut -d: -f1,3 /etc/passwd` | Ambil field 1 dan 3 |
| `cut -d: -f1-3 /etc/passwd` | Ambil field 1 sampai 3 |
| `cut -d"," -f2 data.csv` | Baca CSV, ambil kolom ke-2 |
| `cut -d" " -f1 file.txt` | Gunakan spasi sebagai delimiter |
| `cut -b1-5 file.txt` | Ambil byte 1 sampai 5 |
| `cut --complement -d: -f1` | Ambil semua field KECUALI field ke-1 |

---

### `paste` - Merge Lines of Files
```bash
paste [opsi] <file1> <file2>
```
> Menggabungkan baris dari beberapa file secara horizontal

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `paste file1.txt file2.txt` | Gabungkan baris dua file dengan tab |
| `paste -d"," file1.txt file2.txt` | Gunakan "," sebagai pemisah |
| `paste -d"\|" file1.txt file2.txt` | Gunakan "\|" sebagai pemisah |
| `paste -s file.txt` | Ubah baris menjadi kolom (serial) |
| `paste -s -d"," file.txt` | Serial dengan delimiter koma |

---

### `sort` - Sort Lines
```bash
sort [opsi] <file>
```
> Mengurutkan baris dalam file

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `sort file.txt` | Urutkan secara alfabet (ascending) |
| `sort -r file.txt` | Urutkan terbalik (descending) |
| `sort -n file.txt` | Urutkan secara numerik |
| `sort -rn file.txt` | Urutkan numerik terbalik |
| `sort -h file.txt` | Urutkan human-readable (1K, 10M, 1G) |
| `sort -u file.txt` | Urutkan dan hapus duplikat |
| `sort -k2 file.txt` | Urutkan berdasarkan kolom ke-2 |
| `sort -k2,2 file.txt` | Urutkan hanya berdasarkan kolom ke-2 |
| `sort -k2n file.txt` | Urutkan kolom ke-2 secara numerik |
| `sort -t: -k3n /etc/passwd` | Pakai delimiter ":" dan urutkan kolom 3 numerik |
| `sort -f file.txt` | Case-insensitive |
| `sort -b file.txt` | Abaikan spasi di awal baris |
| `sort -M file.txt` | Urutkan berdasarkan nama bulan |
| `sort -R file.txt` | Acak (random) urutan |
| `sort -c file.txt` | Cek apakah file sudah terurut |
| `sort -m file1.txt file2.txt` | Gabungkan file yang sudah terurut |
| `sort -o output.txt file.txt` | Simpan hasil ke file |
| `sort --stable file.txt` | Pertahankan urutan asli untuk yang sama |

---

### `uniq` - Report or Omit Repeated Lines
```bash
uniq [opsi] <file>
```
> Menghapus atau melaporkan baris duplikat yang berurutan

> ⚠️ **Catatan:** `uniq` hanya mendeteksi duplikat yang berurutan. Gunakan `sort | uniq` untuk duplikat tidak berurutan.

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `uniq file.txt` | Hapus baris duplikat berurutan |
| `uniq -c file.txt` | Tampilkan jumlah kemunculan setiap baris |
| `uniq -d file.txt` | Tampilkan hanya baris yang duplikat |
| `uniq -D file.txt` | Tampilkan semua baris duplikat |
| `uniq -u file.txt` | Tampilkan hanya baris yang unik |
| `uniq -i file.txt` | Case-insensitive |
| `uniq -f 2 file.txt` | Abaikan 2 field pertama saat membandingkan |
| `uniq -s 5 file.txt` | Abaikan 5 karakter pertama |
| `uniq -w 10 file.txt` | Bandingkan hanya 10 karakter pertama |
| `sort file.txt \| uniq` | Hapus semua duplikat (tidak harus berurutan) |
| `sort file.txt \| uniq -c \| sort -rn` | Hitung & urutkan frekuensi |

---

### `tr` - Translate Characters
```bash
tr [opsi] <set1> [set2]
```
> Menerjemahkan, menghapus, atau memampatkan karakter

**Opsi:**
| Perintah | Penjelasan |
|---|---|
| `echo "hello" \| tr 'a-z' 'A-Z'` | Ubah huruf kecil ke besar |
| `echo "HELLO" \| tr 'A-Z' 'a-z'` | Ubah huruf besar ke kecil |
| `echo "hello world" \| tr ' ' '_'` | Ganti spasi dengan underscore |
| `cat file.txt \| tr -d '\n'` | Hapus newline |
| `cat file.txt \| tr -d ' '` | Hapus semua spasi |
| `cat file.txt \| tr -d '[:punct:]'` | Hapus tanda baca |
| `cat file.txt \| tr -s ' '` | Kompresi spasi berurutan |
| `cat file.txt \| tr -s '\n'` | Kompresi newline berurutan |
| `echo "abc123" \| tr -d '[:digit:]'` | Hapus semua angka |
| `echo "abc123" \| tr -d '[:alpha:]'` | Hapus semua huruf |
| `echo "hello" \| tr -c 'a-z' '*'` | Ganti karakter di luar a-z dengan '*' |

**Kelas Karakter tr:**
| Kelas | Penjelasan |
|---|---|
| `[:alpha:]` | Semua huruf |
| `[:digit:]` | Semua angka |
| `[:alnum:]` | Huruf dan angka |
| `[:space:]` | Spasi, tab, newline |
| `[:upper:]` | Huruf besar |
| `[:lower:]` | Huruf kecil |
| `[:punct:]` | Tanda baca |
| `[:print:]` | Karakter yang bisa dicetak |

---

### `tee` - Read from stdin and Write to stdout and Files
```bash
tee [opsi] <file>
```
> Menyalin stdin ke stdout DAN ke file secara bersamaan

**Opsi:**
| Perintah | Penjelasan |
|---|---|
| `command \| tee file.txt` | Tampilkan output dan simpan ke file |
| `command \| tee -a file.txt` | Tambahkan ke file (append) |
| `command \| tee file1.txt file2.txt` | Simpan ke beberapa file |
| `command \| tee /dev/null` | Buang output (gunakan /dev/null) |
| `sudo command \| tee file.txt` | Simpan output perintah sudo |
| `command \| tee >(wc -l) \| grep "kata"` | Kirim ke beberapa proses |

---

## 2.4 Membandingkan File

### `diff` - Compare Files Line by Line
```bash
diff [opsi] <file1> <file2>
```
> Membandingkan dua file dan menampilkan perbedaannya

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `diff file1.txt file2.txt` | Bandingkan dua file |
| `diff -u file1.txt file2.txt` | Format unified (lebih mudah dibaca) |
| `diff -c file1.txt file2.txt` | Format context |
| `diff -y file1.txt file2.txt` | Format side-by-side |
| `diff -i file1.txt file2.txt` | Case-insensitive |
| `diff -w file1.txt file2.txt` | Abaikan semua spasi |
| `diff -b file1.txt file2.txt` | Abaikan perubahan jumlah spasi |
| `diff -B file1.txt file2.txt` | Abaikan baris kosong |
| `diff -r dir1/ dir2/` | Rekursif (bandingkan direktori) |
| `diff -q file1.txt file2.txt` | Hanya tampilkan apakah berbeda |
| `diff -s file1.txt file2.txt` | Tampilkan jika file identik |
| `diff --color file1.txt file2.txt` | Tampilkan dengan warna |
| `diff -I "^#" file1.txt file2.txt` | Abaikan baris yang cocok pola |

**Membaca Output diff:**
```
< baris di file1 (dihapus)
> baris di file2 (ditambahkan)
--- pemisah
a = tambah (add)
d = hapus (delete)
c = ubah (change)
```

---

### `diff3` - Compare Three Files
```bash
diff3 file1.txt file2.txt file3.txt
```
> Membandingkan tiga file sekaligus

---

### `cmp` - Compare Files Byte by Byte
```bash
cmp [opsi] <file1> <file2>
```
> Membandingkan dua file byte per byte

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `cmp file1.txt file2.txt` | Bandingkan dua file |
| `cmp -s file1.txt file2.txt` | Silent (hanya exit code) |
| `cmp -l file1.txt file2.txt` | Tampilkan semua perbedaan byte |
| `cmp -b file1.txt file2.txt` | Tampilkan byte berbeda |
| `cmp -i 10 file1.txt file2.txt` | Lewati 10 byte pertama |

---

### `comm` - Compare Sorted Files Line by Line
```bash
comm [opsi] <file1> <file2>
```
> Membandingkan dua file yang sudah diurutkan

> ⚠️ Kedua file harus sudah diurutkan dengan `sort`

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `comm file1.txt file2.txt` | Tampilkan 3 kolom: hanya di file1, hanya di file2, ada di keduanya |
| `comm -1 file1.txt file2.txt` | Sembunyikan kolom 1 (hanya di file1) |
| `comm -2 file1.txt file2.txt` | Sembunyikan kolom 2 (hanya di file2) |
| `comm -3 file1.txt file2.txt` | Sembunyikan kolom 3 (ada di keduanya) |
| `comm -12 file1.txt file2.txt` | Tampilkan hanya yang ada di keduanya (irisan) |
| `comm -23 file1.txt file2.txt` | Tampilkan hanya yang ada di file1 saja |
| `comm -13 file1.txt file2.txt` | Tampilkan hanya yang ada di file2 saja |

---

## 2.5 Konversi & Format Teks

### `column` - Format Text into Columns
```bash
column [opsi] <file>
```
> Memformat teks menjadi kolom yang rapi

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `column file.txt` | Format teks menjadi kolom |
| `column -t file.txt` | Format sebagai tabel dengan kolom rata |
| `column -s"," -t data.csv` | Format CSV sebagai tabel |
| `column -s":" -t /etc/passwd` | Format /etc/passwd sebagai tabel |
| `column -c 80 file.txt` | Batasi lebar output 80 karakter |

---

### `fold` - Wrap Lines
```bash
fold [opsi] <file>
```
> Membungkus baris panjang ke lebar yang ditentukan

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `fold file.txt` | Bungkus di 80 karakter (default) |
| `fold -w 60 file.txt` | Bungkus di 60 karakter |
| `fold -s file.txt` | Bungkus di spasi (tidak memotong kata) |
| `fold -w 60 -s file.txt` | 60 karakter + tidak memotong kata |

---

### `fmt` - Format Text
```bash
fmt [opsi] <file>
```
> Memformat teks paragraf agar rapi

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `fmt file.txt` | Format dengan lebar 75 karakter (default) |
| `fmt -w 60 file.txt` | Format dengan lebar 60 karakter |
| `fmt -u file.txt` | Seragamkan spasi antar kata |
| `fmt -s file.txt` | Jangan gabungkan baris pendek |

---

### `expand` dan `unexpand`
```bash
expand file.txt       # Konversi tab menjadi spasi
unexpand file.txt     # Konversi spasi menjadi tab
expand -t 4 file.txt  # Gunakan 4 spasi per tab
```

---

### `nl` - Number Lines
```bash
nl [opsi] <file>
```
> Menambahkan nomor baris pada file

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `nl file.txt` | Tambahkan nomor baris |
| `nl -b a file.txt` | Nomori semua baris termasuk kosong |
| `nl -b t file.txt` | Nomori hanya baris tidak kosong (default) |
| `nl -n rz file.txt` | Nomor dengan leading zeros (001, 002) |
| `nl -v 10 file.txt` | Mulai nomor dari 10 |
| `nl -i 2 file.txt` | Increment nomor sebesar 2 |
| `nl -w 3 file.txt` | Lebar nomor 3 karakter |

---

### `rev` - Reverse Lines
```bash
rev file.txt
```
> Membalikkan karakter di setiap baris

```bash
echo "hello" | rev
# olleh

echo "abcde" | rev
# edcba
```

---

### `strings` - Print Printable Strings
```bash
strings [opsi] <file>
```
> Menampilkan string yang bisa dibaca dari file binary

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `strings file.bin` | Tampilkan string dari file binary |
| `strings -n 5 file.bin` | Tampilkan string minimal 5 karakter |
| `strings -o file.bin` | Tampilkan dengan offset |
| `strings /usr/bin/ls` | Lihat string dalam executable |

---

### `hexdump` - Display File in Hexadecimal
```bash
hexdump [opsi] <file>
```
> Menampilkan isi file dalam format hexadecimal

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `hexdump file.txt` | Tampilkan dalam hex |
| `hexdump -C file.txt` | Tampilkan hex + ASCII (paling umum) |
| `hexdump -n 16 file.txt` | Tampilkan hanya 16 byte pertama |
| `hexdump -s 10 file.txt` | Lewati 10 byte pertama |
| `xxd file.txt` | Alternatif hexdump yang lebih mudah dibaca |

---

## 2.6 Redirect & Pipe

### Redirect Output
```bash
command > file.txt       # Redirect stdout ke file (timpa)
command >> file.txt      # Redirect stdout ke file (tambahkan)
command 2> error.txt     # Redirect stderr ke file
command 2>> error.txt    # Redirect stderr ke file (tambahkan)
command &> all.txt       # Redirect stdout DAN stderr ke file
command > file.txt 2>&1  # Redirect stdout ke file, stderr ke stdout
command > /dev/null      # Buang stdout
command 2> /dev/null     # Buang stderr
command &> /dev/null     # Buang semua output
```

### Redirect Input
```bash
command < file.txt       # Ambil input dari file
command << EOF           # Here document (input multi-baris)
teks
EOF
command <<< "string"     # Here string (input satu baris)
```

### Pipe
```bash
command1 | command2           # Kirim output command1 ke command2
command1 | command2 | command3  # Chain beberapa perintah
command1 |& command2          # Pipe stdout DAN stderr
```

**Contoh Kombinasi:**
```bash
# Cari kata, urutkan, hapus duplikat, simpan
grep "error" /var/log/syslog | sort | uniq -c | sort -rn > hasil.txt

# Lihat proses, filter, format
ps aux | grep nginx | awk '{print $1, $2, $11}' | column -t

# Hitung file per ekstensi
find . -type f | sed 's/.*\.//' | sort | uniq -c | sort -rn

# Top 10 IP dari access log
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -10
```

---

## Ringkasan Perintah Bagian 2

```
cat          → Tampilkan/gabungkan isi file
tac          → Tampilkan file terbalik
more         → Lihat file per halaman (basic)
less         → Lihat file per halaman (advanced)
head         → Tampilkan awal file
tail         → Tampilkan akhir file / monitor real-time
nano         → Editor teks sederhana
vim/vi       → Editor teks powerful
grep         → Cari pola teks
sed          → Edit teks secara stream
awk          → Pemrosesan teks & data
cut          → Potong bagian baris
paste        → Gabungkan baris dari beberapa file
sort         → Urutkan baris
uniq         → Hapus/laporan duplikat
tr           → Transliterasi karakter
tee          → Duplikasi output ke file
diff         → Bandingkan dua file
cmp          → Bandingkan byte per byte
comm         → Bandingkan file terurut
column       → Format teks ke kolom
fold         → Bungkus baris panjang
nl           → Tambahkan nomor baris
rev          → Balikkan karakter
strings      → Tampilkan string dari file binary
hexdump      → Tampilkan file dalam hex
Redirect     → >, >>, <, 2>, &>
Pipe         → |
```

---

## ✅ Bagian 2 Selesai!

**Lanjut ke Bagian 3: Perintah Manajemen User & Permission?**
