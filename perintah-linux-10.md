# Bagian 10: Perintah Shell & Scripting

---

## 10.1 Dasar Shell

### Jenis-jenis Shell

```bash
# Cek shell yang tersedia
cat /etc/shells

# Cek shell yang sedang digunakan
echo $SHELL
echo $0

# Ganti shell default
chsh -s /bin/zsh          # Ganti ke zsh
chsh -s /bin/bash         # Ganti ke bash
chsh -s /bin/fish         # Ganti ke fish

# Jalankan shell lain sementara
bash                      # Masuk ke bash
zsh                       # Masuk ke zsh
sh                        # Masuk ke sh
dash                      # Masuk ke dash
ksh                       # Masuk ke ksh
fish                      # Masuk ke fish
```

**Jenis Shell:**
```
bash  → Bourne Again Shell (paling umum di Linux)
sh    → Bourne Shell (kompatibilitas maksimum)
zsh   → Z Shell (fitur lengkap, populer di macOS)
fish  → Friendly Interactive Shell (user-friendly)
ksh   → Korn Shell
dash  → Debian Almquist Shell (ringan, cepat)
tcsh  → TENEX C Shell
```

---

### Konfigurasi Shell (Bash)

```bash
# File konfigurasi bash
~/.bashrc           # Konfigurasi bash untuk interactive shell
~/.bash_profile     # Dijalankan saat login
~/.bash_login       # Dijalankan saat login (jika bash_profile tidak ada)
~/.profile          # Dijalankan saat login (semua shell)
~/.bash_logout      # Dijalankan saat logout
/etc/bash.bashrc    # Konfigurasi bash global
/etc/profile        # Konfigurasi global semua shell
/etc/profile.d/     # Direktori konfigurasi global tambahan

# Reload konfigurasi
source ~/.bashrc
. ~/.bashrc          # Sama dengan source
source ~/.bash_profile

# Contoh isi ~/.bashrc
# Alias
alias ll='ls -la'
alias la='ls -A'
alias l='ls -CF'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
alias df='df -h'
alias du='du -h'
alias free='free -h'
alias update='sudo apt update && sudo apt upgrade'

# Variabel
export PATH="$HOME/.local/bin:$PATH"
export EDITOR=nano
export VISUAL=nano
export PAGER=less
export LANG=en_US.UTF-8

# Prompt (PS1)
export PS1='\u@\h:\w\$ '

# History
export HISTSIZE=10000
export HISTFILESIZE=20000
export HISTCONTROL=ignoredups:erasedups
export HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S "
```

---

## 10.2 Variabel Shell

### Deklarasi & Penggunaan Variabel

```bash
# Deklarasi variabel
nama="John"
umur=25
pi=3.14
aktif=true

# Akses variabel
echo $nama
echo ${nama}           # Kurung kurawal (lebih aman)
echo "Halo, $nama!"
echo "Umur: ${umur} tahun"

# Variabel readonly
readonly KONSTANTA="nilai tetap"
declare -r KONSTANTA="nilai tetap"

# Unset variabel
unset nama

# Cek apakah variabel ada
if [ -z "${nama}" ]; then
    echo "Variabel kosong atau tidak ada"
fi

if [ -n "${nama}" ]; then
    echo "Variabel tidak kosong"
fi
```

---

### Variabel Khusus (Special Variables)

```bash
$0          # Nama script
$1, $2, ... # Argumen ke-1, ke-2, dst.
$#          # Jumlah argumen
$@          # Semua argumen (sebagai array)
$*          # Semua argumen (sebagai string)
$?          # Exit code perintah terakhir
$$          # PID shell saat ini
$!          # PID proses background terakhir
$-          # Opsi shell yang aktif
$_          # Argumen terakhir dari perintah sebelumnya
$RANDOM     # Angka random (0-32767)
$LINENO     # Nomor baris dalam script
$BASH_VERSION  # Versi bash
$BASH_PID   # PID bash
$HOSTNAME   # Hostname
$PWD        # Direktori saat ini
$OLDPWD     # Direktori sebelumnya
$HOME       # Home direktori user
$USER       # Username
$SHELL      # Shell yang digunakan
$PATH       # Path executable
$IFS        # Internal Field Separator (default: spasi, tab, newline)
$PS1        # Primary prompt string
$PS2        # Secondary prompt string
$PS3        # Select prompt
$PS4        # Debug prompt
$SECONDS    # Detik sejak shell dimulai
$REPLY      # Variabel default untuk read
$PIPESTATUS # Array exit code dari pipe
```

---

### Operasi String pada Variabel

```bash
teks="Hello, World!"

# Panjang string
echo ${#teks}                   # 13

# Substring
echo ${teks:7}                  # World!
echo ${teks:7:5}                # World
echo ${teks: -6}                # World! (dari belakang)
echo ${teks: -6:5}              # World

# Hapus prefix
file="path/ke/file.txt"
echo ${file#*/}                 # ke/file.txt (hapus hingga / pertama)
echo ${file##*/}                # file.txt (hapus hingga / terakhir)

# Hapus suffix
echo ${file%.*}                 # path/ke/file (hapus setelah . terakhir)
echo ${file%%.*}                # path/ke/file (hapus setelah . pertama)

# Ganti substring
url="https://example.com/page"
echo ${url/example/test}        # Ganti pertama
echo ${url//example/test}       # Ganti semua
echo ${url/#https/http}         # Ganti di awal
echo ${url/%page/home}          # Ganti di akhir

# Konversi case
nama="hello world"
echo ${nama^}                   # Hello world (huruf besar pertama)
echo ${nama^^}                  # HELLO WORLD (semua huruf besar)
echo ${nama,}                   # hello world (huruf kecil pertama)
echo ${nama,,}                  # hello world (semua huruf kecil)

# Nilai default
echo ${var:-"default"}          # Tampilkan default jika var kosong/tidak ada
echo ${var:="default"}          # Set dan tampilkan default jika kosong
echo ${var:+"alternate"}        # Tampilkan alternate jika var ada
echo ${var:?"Error message"}    # Error jika kosong

# Ekspansi parameter
echo ${!prefix*}                # Variabel yang dimulai dengan prefix
echo ${!arr[@]}                 # Semua indeks array
```

---

### Array

```bash
# Deklarasi array
buah=("apel" "pisang" "mangga" "jeruk")
declare -a angka=(1 2 3 4 5)

# Akses elemen
echo ${buah[0]}                 # apel (indeks mulai dari 0)
echo ${buah[1]}                 # pisang
echo ${buah[-1]}                # jeruk (terakhir)

# Semua elemen
echo ${buah[@]}                 # apel pisang mangga jeruk
echo ${buah[*]}                 # apel pisang mangga jeruk

# Jumlah elemen
echo ${#buah[@]}                # 4

# Semua indeks
echo ${!buah[@]}                # 0 1 2 3

# Tambah elemen
buah+=("durian")                # Tambah ke akhir
buah[5]="nanas"                 # Set indeks tertentu

# Hapus elemen
unset buah[1]                   # Hapus elemen ke-1
unset buah                      # Hapus seluruh array

# Slice array
echo ${buah[@]:1:2}             # 2 elemen mulai indeks 1

# Iterasi array
for item in "${buah[@]}"; do
    echo "$item"
done

# Iterasi dengan indeks
for i in "${!buah[@]}"; do
    echo "$i: ${buah[$i]}"
done

# Associative array (bash 4+)
declare -A kota
kota["indonesia"]="Jakarta"
kota["jepang"]="Tokyo"
kota["prancis"]="Paris"

echo ${kota["indonesia"]}       # Jakarta
echo ${!kota[@]}                # Semua keys
echo ${kota[@]}                 # Semua values

# Iterasi associative array
for negara in "${!kota[@]}"; do
    echo "$negara: ${kota[$negara]}"
done
```

---

## 10.3 Kondisional

### `if` - Statement

```bash
# Syntax dasar
if [ kondisi ]; then
    perintah
fi

if [ kondisi ]; then
    perintah
else
    perintah_lain
fi

if [ kondisi1 ]; then
    perintah1
elif [ kondisi2 ]; then
    perintah2
else
    perintah3
fi

# Contoh
if [ $umur -ge 18 ]; then
    echo "Dewasa"
else
    echo "Belum dewasa"
fi
```

---

### Operator Perbandingan

**Perbandingan Numerik:**
```bash
[ $a -eq $b ]    # a == b (equal)
[ $a -ne $b ]    # a != b (not equal)
[ $a -lt $b ]    # a < b  (less than)
[ $a -le $b ]    # a <= b (less or equal)
[ $a -gt $b ]    # a > b  (greater than)
[ $a -ge $b ]    # a >= b (greater or equal)
```

**Perbandingan String:**
```bash
[ "$a" = "$b" ]   # a sama dengan b
[ "$a" == "$b" ]  # a sama dengan b (bash)
[ "$a" != "$b" ]  # a tidak sama dengan b
[ "$a" < "$b" ]   # a secara leksikografis lebih kecil
[ "$a" > "$b" ]   # a secara leksikografis lebih besar
[ -z "$a" ]       # String kosong
[ -n "$a" ]       # String tidak kosong
```

**Operator File:**
```bash
[ -e file ]       # File/direktori ada
[ -f file ]       # Adalah file biasa
[ -d file ]       # Adalah direktori
[ -l file ]       # Adalah symbolic link
[ -r file ]       # File bisa dibaca
[ -w file ]       # File bisa ditulis
[ -x file ]       # File bisa dieksekusi
[ -s file ]       # File tidak kosong (ukuran > 0)
[ -b file ]       # Block device
[ -c file ]       # Character device
[ -p file ]       # Named pipe (FIFO)
[ -S file ]       # Socket
[ -g file ]       # SGID bit diset
[ -u file ]       # SUID bit diset
[ -k file ]       # Sticky bit diset
[ -O file ]       # Dimiliki oleh user saat ini
[ -G file ]       # Dimiliki oleh grup saat ini
[ file1 -nt file2 ]  # file1 lebih baru dari file2
[ file1 -ot file2 ]  # file1 lebih lama dari file2
[ file1 -ef file2 ]  # file1 dan file2 adalah inode yang sama
```

**Operator Logika:**
```bash
[ kondisi1 ] && [ kondisi2 ]   # AND
[ kondisi1 ] || [ kondisi2 ]   # OR
! [ kondisi ]                   # NOT
[ kondisi1 -a kondisi2 ]       # AND (dalam test)
[ kondisi1 -o kondisi2 ]       # OR (dalam test)
```

**Double Bracket `[[ ]]` (Bash Extended):**
```bash
[[ $a == $b ]]         # Perbandingan string
[[ $a =~ regex ]]      # Regex matching
[[ -z $a && -n $b ]]  # AND dalam satu bracket
[[ $a > $b ]]          # Perbandingan leksikografis
[[ $str == *"sub"* ]]  # Substring check
```

**Double Parenthesis `(( ))` untuk Aritmatika:**
```bash
(( a == b ))           # Perbandingan numerik
(( a > b ))
(( a < b ))
(( a >= b ))
(( a <= b ))
(( a != b ))
if (( $a > $b )); then echo "a lebih besar"; fi
```

---

### `case` - Pattern Matching

```bash
# Syntax dasar
case $variabel in
    pola1)
        perintah1
        ;;
    pola2)
        perintah2
        ;;
    pola3|pola4)
        perintah3
        ;;
    *)
        default_perintah
        ;;
esac

# Contoh
case $hari in
    "Senin"|"Selasa"|"Rabu"|"Kamis"|"Jumat")
        echo "Hari kerja"
        ;;
    "Sabtu"|"Minggu")
        echo "Akhir pekan"
        ;;
    *)
        echo "Hari tidak valid"
        ;;
esac

# Dengan wildcard
case $ekstensi in
    *.txt|*.doc)
        echo "Dokumen teks"
        ;;
    *.jpg|*.png|*.gif)
        echo "Gambar"
        ;;
    *.sh)
        echo "Shell script"
        ;;
    [0-9]*)
        echo "Dimulai dengan angka"
        ;;
    *)
        echo "Tipe tidak dikenal"
        ;;
esac
```

---

## 10.4 Perulangan (Loop)

### `for` Loop

```bash
# For loop dengan list
for item in item1 item2 item3; do
    echo "$item"
done

# For loop dengan range
for i in {1..10}; do
    echo "$i"
done

# For loop dengan step
for i in {0..20..5}; do    # 0, 5, 10, 15, 20
    echo "$i"
done

# For loop C-style
for ((i=0; i<10; i++)); do
    echo "$i"
done

# For loop dengan array
buah=("apel" "pisang" "mangga")
for item in "${buah[@]}"; do
    echo "$item"
done

# For loop file
for file in *.txt; do
    echo "Memproses: $file"
    cat "$file"
done

# For loop direktori
for dir in */; do
    echo "Direktori: $dir"
done

# For loop dengan command substitution
for user in $(cut -d: -f1 /etc/passwd); do
    echo "$user"
done

# For loop dengan glob
for file in /var/log/*.log; do
    echo "Log: $file"
    wc -l "$file"
done

# Nested for loop
for i in {1..3}; do
    for j in {1..3}; do
        echo "$i x $j = $((i*j))"
    done
done
```

---

### `while` Loop

```bash
# Syntax dasar
while [ kondisi ]; do
    perintah
done

# Contoh counter
i=0
while [ $i -lt 10 ]; do
    echo "$i"
    ((i++))
done

# While dengan double bracket
i=0
while [[ $i -lt 10 ]]; do
    echo "$i"
    ((i++))
done

# While dengan aritmatika
i=0
while (( i < 10 )); do
    echo "$i"
    ((i++))
done

# While true (infinite loop)
while true; do
    echo "Tekan Ctrl+C untuk berhenti"
    sleep 1
done

# While baca file baris per baris
while IFS= read -r baris; do
    echo "$baris"
done < file.txt

# While baca dengan custom IFS
while IFS=: read -r user pass uid gid gecos home shell; do
    echo "User: $user, Home: $home"
done < /etc/passwd

# While pipe
cat file.txt | while read baris; do
    echo ">> $baris"
done

# While dengan multiple conditions
while [ $a -lt 10 ] && [ $b -gt 0 ]; do
    echo "a=$a, b=$b"
    ((a++))
    ((b--))
done
```

---

### `until` Loop

```bash
# Kebalikan while (loop sampai kondisi TRUE)
i=0
until [ $i -ge 10 ]; do
    echo "$i"
    ((i++))
done

# Until dengan proses
until ping -c 1 google.com &>/dev/null; do
    echo "Menunggu koneksi internet..."
    sleep 5
done
echo "Koneksi tersedia!"
```

---

### `select` Loop

```bash
# Menu interaktif
PS3="Pilih buah: "
select buah in "Apel" "Pisang" "Mangga" "Keluar"; do
    case $buah in
        "Apel")
            echo "Anda memilih Apel"
            ;;
        "Pisang")
            echo "Anda memilih Pisang"
            ;;
        "Mangga")
            echo "Anda memilih Mangga"
            ;;
        "Keluar")
            echo "Sampai jumpa!"
            break
            ;;
        *)
            echo "Pilihan tidak valid"
            ;;
    esac
done
```

---

### Loop Control

```bash
# break - keluar dari loop
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        break        # Keluar dari loop
    fi
    echo "$i"
done

# break dengan level
for i in {1..3}; do
    for j in {1..3}; do
        if [ $j -eq 2 ]; then
            break 2   # Keluar dari 2 level loop
        fi
        echo "$i $j"
    done
done

# continue - lewati iterasi saat ini
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        continue     # Lewati iterasi ke-5
    fi
    echo "$i"
done

# continue dengan level
for i in {1..3}; do
    for j in {1..3}; do
        if [ $j -eq 2 ]; then
            continue 2  # Continue ke loop luar
        fi
        echo "$i $j"
    done
done
```

---

## 10.5 Fungsi

### Deklarasi & Pemanggilan Fungsi

```bash
# Cara 1: function keyword
function salam() {
    echo "Halo, $1!"
}

# Cara 2: tanpa keyword
salam() {
    echo "Halo, $1!"
}

# Panggil fungsi
salam "World"
salam "Linux"

# Fungsi dengan return value
tambah() {
    local hasil=$(( $1 + $2 ))
    echo $hasil    # "Return" via echo
}

# Tangkap hasil fungsi
hasil=$(tambah 5 3)
echo "5 + 3 = $hasil"

# Fungsi dengan return status
cek_file() {
    if [ -f "$1" ]; then
        return 0   # Sukses
    else
        return 1   # Gagal
    fi
}

if cek_file "/etc/passwd"; then
    echo "File ada"
else
    echo "File tidak ada"
fi

# Variabel lokal dalam fungsi
fungsi_lokal() {
    local var_lokal="saya lokal"
    var_global="saya global"
    echo "Dalam fungsi: $var_lokal"
}

fungsi_lokal
echo "Setelah fungsi: $var_global"
# var_lokal tidak bisa diakses di sini

# Fungsi rekursif
factorial() {
    local n=$1
    if [ $n -le 1 ]; then
        echo 1
    else
        local prev=$(factorial $((n-1)))
        echo $((n * prev))
    fi
}
echo "5! = $(factorial 5)"

# Fungsi dengan array parameter
proses_array() {
    local arr=("$@")   # Terima semua argumen sebagai array
    for item in "${arr[@]}"; do
        echo "Item: $item"
    done
}

buah=("apel" "pisang" "mangga")
proses_array "${buah[@]}"
```

---

### Library Fungsi

```bash
# Simpan fungsi ke file library
cat > ~/lib/functions.sh << 'EOF'
#!/bin/bash

# Logging
log_info() {
    echo "[INFO] $(date '+%Y-%m-%d %H:%M:%S') - $*"
}

log_error() {
    echo "[ERROR] $(date '+%Y-%m-%d %H:%M:%S') - $*" >&2
}

log_warning() {
    echo "[WARN] $(date '+%Y-%m-%d %H:%M:%S') - $*"
}

# Cek root
require_root() {
    if [[ $EUID -ne 0 ]]; then
        log_error "Script ini harus dijalankan sebagai root"
        exit 1
    fi
}

# Konfirmasi
confirm() {
    read -p "$1 [y/N] " response
    case $response in
        [yY][eE][sS]|[yY]) return 0 ;;
        *) return 1 ;;
    esac
}
EOF

# Gunakan library dalam script
source ~/lib/functions.sh
log_info "Script dimulai"
```

---

## 10.6 Input & Output dalam Script

### `read` - Read User Input

```bash
# Baca input dasar
read nama
echo "Halo, $nama!"

# Dengan prompt
read -p "Masukkan nama: " nama
echo "Halo, $nama!"

# Baca password (tersembunyi)
read -s -p "Masukkan password: " password
echo ""
echo "Password dimasukkan"

# Baca dengan timeout
read -t 10 -p "Masukkan dalam 10 detik: " input
if [ $? -ne 0 ]; then
    echo "Timeout!"
fi

# Baca beberapa variabel
read -p "Nama depan dan belakang: " depan belakang
echo "Nama depan: $depan"
echo "Nama belakang: $belakang"

# Baca karakter tunggal
read -n 1 -p "Tekan sembarang tombol..."
echo ""

# Baca dengan delimeter
read -d ":" bagian pertama
echo "Bagian: $bagian"

# Baca array
read -a buah -p "Masukkan buah-buahan: "
for item in "${buah[@]}"; do
    echo "- $item"
done

# Baca file baris per baris
while IFS= read -r baris; do
    echo ">> $baris"
done < file.txt

# Baca dari pipe
echo "teks" | while read baris; do
    echo "Input: $baris"
done
```

---

### Output Warna & Format

```bash
# Kode warna ANSI
MERAH='\033[0;31m'
HIJAU='\033[0;32m'
KUNING='\033[1;33m'
BIRU='\033[0;34m'
UNGU='\033[0;35m'
CYAN='\033[0;36m'
PUTIH='\033[1;37m'
RESET='\033[0m'    # Reset warna

# Penggunaan
echo -e "${MERAH}Ini teks merah${RESET}"
echo -e "${HIJAU}Sukses!${RESET}"
echo -e "${KUNING}Peringatan!${RESET}"
echo -e "${BIRU}Info${RESET}"

# Fungsi print berwarna
print_info()    { echo -e "${BIRU}[INFO]${RESET} $*"; }
print_success() { echo -e "${HIJAU}[OK]${RESET} $*"; }
print_warning() { echo -e "${KUNING}[WARN]${RESET} $*"; }
print_error()   { echo -e "${MERAH}[ERROR]${RESET} $*" >&2; }

# Bold, underline, blink
BOLD='\033[1m'
DIM='\033[2m'
UNDERLINE='\033[4m'
BLINK='\033[5m'
REVERSE='\033[7m'

echo -e "${BOLD}Teks tebal${RESET}"
echo -e "${UNDERLINE}Teks bergaris bawah${RESET}"

# Background color
BG_MERAH='\033[41m'
BG_HIJAU='\033[42m'
BG_KUNING='\033[43m'
BG_BIRU='\033[44m'

echo -e "${BG_HIJAU}${PUTIH} Sukses! ${RESET}"

# Cek apakah terminal mendukung warna
if [ -t 1 ] && [ "$(tput colors)" -ge 8 ]; then
    USE_COLOR=true
fi

# printf dengan format
printf "%-15s %5s %10s\n" "Nama" "Usia" "Kota"
printf "%-15s %5d %10s\n" "John Doe" 25 "Jakarta"
printf "%-15s %5d %10s\n" "Jane Doe" 30 "Bandung"
```

---

### `printf` - Formatted Output

```bash
# Syntax
printf FORMAT [ARGUMEN...]

# Format specifiers
printf "%s\n" "string"           # String
printf "%d\n" 42                 # Integer
printf "%f\n" 3.14               # Float
printf "%.2f\n" 3.14159          # Float 2 desimal
printf "%e\n" 12345.678          # Scientific notation
printf "%o\n" 8                  # Octal
printf "%x\n" 255                # Hexadecimal lowercase
printf "%X\n" 255                # Hexadecimal uppercase
printf "%b\n" "teks\n"           # Interpret escape

# Lebar field
printf "%10s\n" "kanan"          # Right-align dalam 10 karakter
printf "%-10s\n" "kiri"          # Left-align
printf "%010d\n" 42              # Zero-padding: 0000000042
printf "%+d\n" 42                # Tampilkan tanda: +42

# Tabel
printf "%-20s %-10s %-10s\n" "Nama" "Umur" "Kota"
printf "%-20s %-10d %-10s\n" "John Doe" 25 "Jakarta"
printf "%-20s %-10d %-10s\n" "Jane Smith" 30 "Bandung"

# Multiple arguments (printf mengulang format)
printf "%s\n" apel pisang mangga    # Cetak satu per baris

# Warna dengan printf
printf '\033[0;32m%s\033[0m\n' "Teks Hijau"
```

---

## 10.7 Penanganan Error

### Exit Code

```bash
# Exit code
# 0 = sukses
# 1-255 = error

# Cek exit code
ls /path/ada
echo $?      # 0 (sukses)

ls /path/tidak/ada
echo $?      # 2 (error: tidak ada)

# Keluar dari script
exit 0       # Sukses
exit 1       # Error umum
exit 2       # Misuse of shell
exit 126     # Command tidak bisa dieksekusi
exit 127     # Command tidak ditemukan
exit 130     # Script dihentikan oleh Ctrl+C
exit 137     # Kill -9

# Gunakan exit code dalam script
if ! command; then
    echo "Perintah gagal"
    exit 1
fi

# Tangkap exit code
perintah
status=$?
if [ $status -ne 0 ]; then
    echo "Error dengan status: $status"
fi
```

---

### `trap` - Signal Handling

```bash
# Syntax
trap 'perintah' SINYAL

# Trap Ctrl+C (SIGINT)
trap 'echo "Dihentikan!"; exit 1' INT

# Trap SIGTERM
trap 'echo "Terminated!"; cleanup; exit 0' TERM

# Trap EXIT (dijalankan saat script selesai)
trap 'echo "Script selesai"' EXIT

# Cleanup saat keluar
trap cleanup EXIT

cleanup() {
    echo "Membersihkan..."
    rm -f /tmp/tempfile_$$
    # Operasi cleanup lainnya
}

# Trap multiple signals
trap 'cleanup; exit 1' INT TERM EXIT

# Ignore sinyal
trap '' SIGINT    # Abaikan Ctrl+C

# Reset trap ke default
trap - SIGINT     # Reset ke default

# Trap dengan ERR (setiap error)
trap 'echo "Error pada baris $LINENO"' ERR

# Contoh lengkap
#!/bin/bash
set -euo pipefail

TEMPDIR=$(mktemp -d)
trap "rm -rf $TEMPDIR" EXIT

echo "Bekerja di $TEMPDIR"
# ... operasi ...
echo "Selesai"
# TEMPDIR akan dihapus otomatis
```

---

### Error Handling Best Practices

```bash
#!/bin/bash

# ===== HEADER SCRIPT =====
set -euo pipefail
# -e: exit jika ada error
# -u: error jika variabel tidak terdefinisi
# -o pipefail: gagal jika ada dalam pipe yang gagal

# IFS yang aman
IFS=$'\n\t'

# ===== FUNGSI ERROR =====
error_handler() {
    local exit_code=$?
    local line_number=$1
    echo "ERROR: Script gagal di baris $line_number dengan exit code $exit_code"
    exit $exit_code
}

trap 'error_handler $LINENO' ERR

# ===== CONTOH PENGGUNAAN =====

# Validasi argumen
if [ $# -lt 2 ]; then
    echo "Usage: $0 <input> <output>" >&2
    exit 1
fi

# Validasi file
INPUT_FILE=$1
OUTPUT_FILE=$2

if [ ! -f "$INPUT_FILE" ]; then
    echo "Error: File input '$INPUT_FILE' tidak ditemukan" >&2
    exit 1
fi

if [ ! -r "$INPUT_FILE" ]; then
    echo "Error: File '$INPUT_FILE' tidak bisa dibaca" >&2
    exit 1
fi

# Periksa perintah yang dibutuhkan
command -v awk >/dev/null 2>&1 || {
    echo "Error: 'awk' tidak ditemukan" >&2
    exit 1
}

# Jalankan dengan error handling
if ! proses "$INPUT_FILE" > "$OUTPUT_FILE"; then
    echo "Error: Gagal memproses file"
    rm -f "$OUTPUT_FILE"
    exit 1
fi

echo "Berhasil!"
```

---

## 10.8 Operasi Aritmatika

### Cara Menghitung dalam Bash

```bash
# Cara 1: expr (lama)
hasil=$(expr 5 + 3)
echo $hasil

# Cara 2: $((  )) - arithmetic expansion
hasil=$((5 + 3))
echo $((10 - 4))
echo $((6 * 7))
echo $((15 / 4))    # Integer division: 3
echo $((15 % 4))    # Modulo: 3
echo $((2 ** 10))   # Power: 1024

# Cara 3: let
let "hasil = 5 + 3"
let "a++"
let "b += 5"

# Cara 4: (( )) - arithmetic command
((hasil = 5 + 3))
((a++))
((b += 5))
if (( a > b )); then echo "a lebih besar"; fi

# Cara 5: bc untuk float
echo "scale=2; 10 / 3" | bc         # 3.33
echo "scale=4; sqrt(2)" | bc         # 1.4142
echo "3.14 * 2^2" | bc               # 12.56
hasil=$(echo "scale=2; 10 / 3" | bc)
echo $hasil

# Cara 6: awk untuk float
awk "BEGIN {printf \"%.2f\n\", 10/3}"
awk "BEGIN {print 2^10}"

# Operasi dalam loop
for i in {1..5}; do
    echo "$i kuadrat = $((i * i))"
done

# Increment/decrement
a=5
((a++))     # a = 6
((a--))     # a = 5
((++a))     # a = 6 (pre-increment)
((--a))     # a = 5 (pre-decrement)

# Compound assignment
((a += 10))   # a = a + 10
((a -= 5))    # a = a - 5
((a *= 2))    # a = a * 2
((a /= 3))    # a = a / 3
((a %= 7))    # a = a % 7
((a **= 2))   # a = a ^ 2

# Bitwise operations
echo $(( 5 & 3 ))    # AND: 1
echo $(( 5 | 3 ))    # OR: 7
echo $(( 5 ^ 3 ))    # XOR: 6
echo $(( ~5 ))        # NOT: -6
echo $(( 5 << 1 ))   # Left shift: 10
echo $(( 5 >> 1 ))   # Right shift: 2
```

---

## 10.9 String Operations

```bash
# Panjang string
str="Hello, World!"
echo ${#str}                    # 13

# Konversi case
echo ${str,,}                   # hello, world! (lowercase)
echo ${str^^}                   # HELLO, WORLD! (uppercase)
echo ${str^}                    # Hello, World! (capitalize first)

# Substring
echo ${str:0:5}                 # Hello
echo ${str:7}                   # World!
echo ${str:7:5}                 # World
echo ${str: -6}                 # World!

# Penggantian
echo ${str/World/Linux}         # Hello, Linux!
echo ${str//l/L}                # HeLLo, WorLd!

# Hapus pattern
path="/home/user/file.txt"
echo ${path##*/}                # file.txt (ambil nama file)
echo ${path%/*}                 # /home/user (ambil direktori)
echo ${path##*.}                # txt (ambil ekstensi)
echo ${path%.*}                 # /home/user/file (hapus ekstensi)

# Trim whitespace
str="  hello world  "
echo "${str}" | xargs           # hello world (trim kedua sisi)
echo "${str#"${str%%[![:space:]]*}"}"  # ltrim
echo "${str%"${str##*[![:space:]]}"}"  # rtrim

# Split string
str="a:b:c:d"
IFS=':' read -ra arr <<< "$str"
for item in "${arr[@]}"; do
    echo "$item"
done

# Join array
arr=("a" "b" "c" "d")
IFS=':'; joined="${arr[*]}"; unset IFS
echo "$joined"                  # a:b:c:d

# Cek substring
str="Hello, World!"
if [[ $str == *"World"* ]]; then
    echo "Mengandung 'World'"
fi

# Regex matching
if [[ $str =~ ^Hello ]]; then
    echo "Dimulai dengan 'Hello'"
fi

if [[ "123" =~ ^[0-9]+$ ]]; then
    echo "String adalah angka"
fi

# Repeat string
printf '%0.s-' {1..20}; echo   # Cetak 20 tanda -

# Reverse string
echo "Hello" | rev              # olleH
```

---

## 10.10 File Operations dalam Script

```bash
# Baca file ke variabel
isi=$(cat file.txt)
baris_pertama=$(head -1 file.txt)
baris_terakhir=$(tail -1 file.txt)

# Tulis ke file
echo "teks" > file.txt          # Timpa
echo "teks" >> file.txt         # Append

# Tulis blok teks (heredoc)
cat > file.txt << 'EOF'
Baris pertama
Baris kedua
$variabel tidak diexpand (single quote)
EOF

cat > file.txt << EOF
Baris pertama
$variabel akan diexpand
EOF

# Tulis dengan tee
echo "teks" | tee file.txt      # Tampilkan + simpan
echo "teks" | tee -a file.txt   # Tampilkan + append

# Proses file baris per baris
while IFS= read -r baris; do
    # Proses setiap baris
    echo ">> $baris"
done < file.txt

# Proses file dengan nomor baris
baris_num=0
while IFS= read -r baris; do
    ((baris_num++))
    echo "$baris_num: $baris"
done < file.txt

# Proses CSV
while IFS=',' read -r kolom1 kolom2 kolom3; do
    echo "K1=$kolom1 K2=$kolom2 K3=$kolom3"
done < data.csv

# Skip baris pertama (header)
{
    read header
    while IFS=',' read -r k1 k2 k3; do
        echo "$k1 | $k2 | $k3"
    done
} < data.csv

# Tulis CSV
{
    echo "Nama,Umur,Kota"
    echo "John,25,Jakarta"
    echo "Jane,30,Bandung"
} > output.csv

# Cek dan buat direktori
[ ! -d "/path/dir" ] && mkdir -p "/path/dir"

# Buat file temporary
tmpfile=$(mktemp)
tmpdir=$(mktemp -d)
trap "rm -f $tmpfile; rm -rf $tmpdir" EXIT
```

---

## 10.11 Perintah Shell Berguna

### `echo` - Print Text

```bash
echo "Hello, World!"           # Print dengan newline
echo -n "Tanpa newline"        # Tanpa newline di akhir
echo -e "Dengan\tescape"       # Interpret escape characters
echo -e "Baris1\nBaris2"       # Newline
echo -e "\tTab"                # Tab
echo -e "\a"                   # Bell sound
echo -e "\033[31mMerah\033[0m" # Warna
echo ""                        # Baris kosong
echo $'\n'                     # Newline via $'...'
```

---

### `printf` - Formatted Print

```bash
printf "Hello, %s!\n" "World"
printf "Angka: %d\n" 42
printf "Float: %.2f\n" 3.14159
printf "Hex: %x\n" 255
printf "%-10s %5d\n" "Item" 100
```

---

### `eval` - Evaluate Expression

```bash
# Eval menjalankan string sebagai perintah
cmd="ls -la"
eval "$cmd"

# Dinamis membuat perintah
for i in 1 2 3; do
    eval "var_$i=$i"
done
echo $var_1 $var_2 $var_3

# Gunakan dengan hati-hati (risiko keamanan)
```

---

### `exec` - Replace Shell

```bash
exec ls -la              # Ganti shell dengan ls (shell ditutup setelah)
exec > output.txt        # Redirect semua stdout ke file
exec 2> error.txt        # Redirect semua stderr ke file
exec 3> logfile.txt      # Buka file descriptor 3
echo "log" >&3           # Tulis ke fd 3
exec 3>&-               # Tutup file descriptor 3
```

---

### `source` & `.` - Execute in Current Shell

```bash
source ~/.bashrc         # Reload bashrc
. ~/.bashrc              # Sama dengan source
source functions.sh      # Load fungsi dari file
. /etc/os-release        # Load variabel OS info
```

---

### `alias` - Command Aliases

```bash
# Lihat semua alias
alias

# Buat alias
alias ll='ls -la'
alias la='ls -A'
alias ..='cd ..'
alias ...='cd ../..'
alias update='sudo apt update && sudo apt upgrade -y'
alias ports='ss -tulpn'
alias myip='curl -s ifconfig.me'
alias weather='curl wttr.in'
alias serve='python3 -m http.server 8080'
alias grep='grep --color=auto'
alias diff='diff --color=auto'
alias ip='ip --color=auto'

# Alias dengan parameter (gunakan fungsi, bukan alias)
# SALAH: alias mkcd='mkdir $1 && cd $1'
# BENAR:
mkcd() { mkdir -p "$1" && cd "$1"; }

# Hapus alias
unalias ll
unalias -a     # Hapus semua alias

# Alias permanen (tambahkan ke ~/.bashrc)
echo "alias ll='ls -la'" >> ~/.bashrc
source ~/.bashrc

# Bypass alias (jalankan perintah asli)
\ls             # Jalankan ls tanpa alias
command ls      # Sama
builtin cd      # Jalankan builtin cd
```

---

### `history` - Command History

```bash
history                    # Tampilkan riwayat perintah
history 20                 # 20 perintah terakhir
history | grep "perintah"  # Cari dalam history
history -c                 # Hapus history
history -d 100             # Hapus entri ke-100
history -w                 # Tulis history ke file
history -r                 # Baca history dari file
history -n                 # Baca history baru dari file

# Penggunaan history
!!                         # Jalankan perintah terakhir
!100                       # Jalankan perintah ke-100
!-2                        # Jalankan 2 perintah sebelumnya
!ls                        # Jalankan perintah ls terakhir
!ls:p                      # Print (tidak dijalankan)
!$                         # Argumen terakhir dari perintah sebelumnya
!*                         # Semua argumen dari perintah sebelumnya
!^                         # Argumen pertama dari perintah sebelumnya
^lama^baru                 # Ganti "lama" dengan "baru" di perintah sebelumnya

# Variabel history
HISTFILE=~/.bash_history   # File history
HISTSIZE=10000             # Jumlah perintah dalam memori
HISTFILESIZE=20000         # Jumlah perintah dalam file
HISTCONTROL=ignoredups     # Abaikan duplikat
HISTCONTROL=ignorespace    # Abaikan yang dimulai dengan spasi
HISTCONTROL=ignoreboth     # Keduanya
HISTTIMEFORMAT="%F %T "   # Timestamp dalam history
HISTIGNORE="ls:ll:cd:pwd:clear:exit"  # Perintah yang tidak disimpan

# Ctrl+R: Reverse search history
# Ctrl+G: Keluar dari reverse search
```

---

### `xargs` - Build and Execute Commands

```bash
# Dasar
echo "file1 file2 file3" | xargs ls
ls *.txt | xargs wc -l
find . -name "*.log" | xargs rm

# Dengan placeholder
find . -name "*.txt" | xargs -I {} cp {} /backup/
find . -name "*.jpg" | xargs -I {} convert {} {}.png

# Parallel execution
find . -name "*.mp4" | xargs -P 4 -I {} ffmpeg -i {} {}.mp3

# Batasi argumen per perintah
echo "a b c d e" | xargs -n 2 echo
# Output:
# a b
# c d
# e

# Dengan delimiter
echo "a:b:c" | xargs -d: echo    # a b c

# Konfirmasi sebelum eksekusi
find . -name "*.tmp" | xargs -p rm

# Null-terminated (aman untuk nama file dengan spasi)
find . -name "*.txt" -print0 | xargs -0 rm
find . -name "*.txt" -print0 | xargs -0 -I {} cp {} /backup/

# Kombinasi
find /var/log -name "*.log" -mtime +30 | \
    xargs -I {} sh -c 'echo "Menghapus: {}"; rm "{}"'
```

---

### `tee` - Split Output

```bash
# Output ke file dan stdout
command | tee file.txt
command | tee -a file.txt         # Append
command | tee file1.txt file2.txt # Beberapa file
command | tee /dev/stderr         # Output ke stderr juga

# Dalam pipeline
cat input.txt | \
    tee step1.txt | \
    grep "pattern" | \
    tee step2.txt | \
    awk '{print $1}' > final.txt
```

---

### `watch` - Execute Periodically

```bash
watch perintah                    # Jalankan setiap 2 detik (default)
watch -n 1 perintah               # Setiap 1 detik
watch -n 0.5 perintah             # Setiap 0.5 detik
watch -d perintah                 # Highlight perubahan
watch -d=cumulative perintah      # Highlight semua perubahan
watch -t perintah                 # Tanpa header
watch -g perintah                 # Keluar saat output berubah
watch -e perintah                 # Keluar jika perintah gagal

# Contoh
watch -n 1 "df -h"               # Monitor disk
watch -n 1 "free -h"             # Monitor memori
watch -n 2 "ss -tulpn"           # Monitor port
watch -n 1 "cat /proc/loadavg"   # Monitor load
watch -d -n 1 "ls -la /tmp"      # Monitor perubahan file
```

---

## 10.12 Scripting Lanjutan

### Argumen Parsing

```bash
#!/bin/bash

# Cara 1: Posisional sederhana
nama=$1
umur=$2
echo "Nama: $nama, Umur: $umur"

# Cara 2: getopts
usage() {
    echo "Usage: $0 [-n nama] [-u umur] [-v] [-h]"
    exit 1
}

verbose=false
nama=""
umur=""

while getopts ":n:u:vh" opt; do
    case $opt in
        n) nama="$OPTARG" ;;
        u) umur="$OPTARG" ;;
        v) verbose=true ;;
        h) usage ;;
        :) echo "Opsi -$OPTARG membutuhkan argumen"; usage ;;
        \?) echo "Opsi tidak valid: -$OPTARG"; usage ;;
    esac
done

shift $((OPTIND-1))   # Hapus opsi yang sudah diproses

$verbose && echo "Mode verbose aktif"
echo "Nama: $nama"
echo "Umur: $umur"
echo "Argumen sisa: $@"

# Cara 3: getopt (long options)
OPSI=$(getopt -o n:u:vh \
    --long nama:,umur:,verbose,help \
    -n "$0" -- "$@")

eval set -- "$OPSI"

while true; do
    case "$1" in
        -n|--nama) nama="$2"; shift 2 ;;
        -u|--umur) umur="$2"; shift 2 ;;
        -v|--verbose) verbose=true; shift ;;
        -h|--help) usage ;;
        --) shift; break ;;
        *) echo "Error"; exit 1 ;;
    esac
done
```

---

### Debugging Script

```bash
# Aktifkan debugging
bash -x script.sh              # Trace eksekusi
bash -n script.sh              # Check syntax tanpa eksekusi
bash -v script.sh              # Verbose (print setiap baris)
bash -xv script.sh             # Trace + verbose

# Dalam script
set -x                         # Aktifkan trace
set +x                         # Nonaktifkan trace

# Debug bagian tertentu
set -x
# kode yang ingin di-debug
set +x

# PS4 untuk prefix debug yang lebih informatif
export PS4='+(${BASH_SOURCE}:${LINENO}): ${FUNCNAME[0]:+${FUNCNAME[0]}(): }'
set -x

# Trap untuk debug
trap 'echo "Baris $LINENO: $BASH_COMMAND"' DEBUG

# Cek exit status
set -e                         # Exit saat ada error
set -u                         # Error jika variabel tidak didefinisikan
set -o pipefail                # Error jika pipe gagal
set -euo pipefail              # Kombinasi ketiganya

# Log debugging ke file
exec 2> debug.log
set -x
```

---

### Subshell & Process Substitution

```bash
# Subshell - jalankan dalam subshell
(cd /tmp && ls)               # Tidak mengubah direktori saat ini
( export VAR=test; echo $VAR) # Variabel tidak keluar
hasil=$(perintah)              # Command substitution (subshell)

# Process substitution
diff <(ls dir1/) <(ls dir2/)   # Bandingkan output dua perintah
cat <(head -5 file1.txt) <(tail -5 file2.txt)  # Gabungkan
while read baris; do
    echo "$baris"
done < <(cat file.txt | grep "pola")

# Pipe (kiri berjalan dalam subshell)
echo "test" | read var         # TIDAK BEKERJA (subshell)
read var < <(echo "test")      # BEKERJA

# Coproc - coprocess
coproc MYPROC { cat; }        # Jalankan sebagai coprocess
echo "test" >&${MYPROC[1]}    # Kirim ke coprocess
read hasil <&${MYPROC[0]}     # Baca dari coprocess
```

---

### Heredoc & Herestring

```bash
# Heredoc - multi-line string
cat << EOF
Baris pertama
Baris kedua
Variabel: $HOME
EOF

# Heredoc tanpa expand variabel
cat << 'EOF'
Variabel: $HOME (tidak di-expand)
EOF

# Heredoc dengan indentasi (menggunakan -)
cat <<- EOF
    Baris pertama (tab di depan dihapus)
    Baris kedua
    EOF

# Heredoc ke file
cat > file.txt << EOF
Isi file
$variabel di-expand
EOF

# Heredoc ke perintah
ssh user@server << EOF
echo "Ini dijalankan di server"
ls -la
whoami
EOF

# Heredoc ke variabel
teks=$(cat << EOF
Baris 1
Baris 2
EOF
)

# Herestring - single string
cat <<< "Ini herestring"
read var <<< "nilai"
grep "pola" <<< "$teks"
```

---

### Parallel Processing dalam Script

```bash
# Background jobs
for file in *.txt; do
    proses "$file" &
done
wait    # Tunggu semua selesai

# Dengan kontrol jumlah paralel
max_jobs=4
count=0

for file in *.txt; do
    proses "$file" &
    ((count++))
    if (( count >= max_jobs )); then
        wait -n    # Tunggu salah satu selesai (bash 4.3+)
        ((count--))
    fi
done
wait

# GNU Parallel
sudo apt install parallel

# Jalankan 4 proses paralel
ls *.txt | parallel -j4 proses {}
find . -name "*.jpg" | parallel -j8 convert {} {.}.png
parallel -j4 wget ::: url1 url2 url3 url4
cat daftar.txt | parallel -j4 "curl -s {} > {#}.html"

# xargs paralel
find . -name "*.txt" | xargs -P 4 -I {} proses {}
```

---

### Scripting Best Practices

```bash
#!/bin/bash
# ============================================
# Nama Script  : contoh.sh
# Deskripsi    : Contoh script terbaik
# Author       : Nama Anda
# Tanggal      : 2024-01-15
# Versi        : 1.0.0
# ============================================

# === PENGATURAN AMAN ===
set -euo pipefail
IFS=$'\n\t'

# === KONSTANTA ===
readonly SCRIPT_NAME=$(basename "$0")
readonly SCRIPT_DIR=$(cd "$(dirname "$0")" && pwd)
readonly VERSION="1.0.0"
readonly LOG_FILE="/var/log/${SCRIPT_NAME}.log"

# === WARNA ===
readonly RED='\033[0;31m'
readonly GREEN='\033[0;32m'
readonly YELLOW='\033[1;33m'
readonly NC='\033[0m'

# === LOGGING ===
log() {
    local level=$1
    shift
    echo "$(date '+%Y-%m-%d %H:%M:%S') [$level] $*" | tee -a "$LOG_FILE"
}

log_info()    { log "INFO"  "$*"; }
log_success() { log "OK"    "${GREEN}$*${NC}"; }
log_warning() { log "WARN"  "${YELLOW}$*${NC}"; }
log_error()   { log "ERROR" "${RED}$*${NC}" >&2; }

# === CLEANUP ===
cleanup() {
    local exit_code=$?
    log_info "Cleanup dimulai..."
    # Tambahkan cleanup di sini
    exit $exit_code
}
trap cleanup EXIT INT TERM

# === VALIDASI ===
require_root() {
    [[ $EUID -eq 0 ]] || {
        log_error "Script harus dijalankan sebagai root"
        exit 1
    }
}

require_command() {
    command -v "$1" >/dev/null 2>&1 || {
        log_error "Perintah tidak ditemukan: $1"
        exit 1
    }
}

# === USAGE ===
usage() {
    cat << EOF
Penggunaan: $SCRIPT_NAME [OPSI] <argumen>

Opsi:
    -h, --help      Tampilkan bantuan ini
    -v, --verbose   Mode verbose
    -n, --dry-run   Simulasi tanpa perubahan

Contoh:
    $SCRIPT_NAME -v input.txt
    $SCRIPT_NAME --dry-run direktori/

EOF
    exit 0
}

# === MAIN ===
main() {
    log_info "Script dimulai v$VERSION"
    # Kode utama di sini
    log_success "Script selesai"
}

# === PARSE ARGUMENTS ===
VERBOSE=false
DRY_RUN=false

while [[ $# -gt 0 ]]; do
    case "$1" in
        -h|--help)    usage ;;
        -v|--verbose) VERBOSE=true; shift ;;
        -n|--dry-run) DRY_RUN=true; shift ;;
        --)           shift; break ;;
        -*)           log_error "Opsi tidak dikenal: $1"; usage ;;
        *)            break ;;
    esac
done

# Jalankan main
main "$@"
```

---

## Ringkasan Perintah Bagian 10

```
Shell Basics:
  bash/zsh/sh    → Jenis shell
  source/.       → Load file dalam shell saat ini
  exec           → Ganti proses shell
  eval           → Evaluasi string sebagai perintah

Variabel:
  $VAR / ${VAR}  → Akses variabel
  declare        → Deklarasi variabel/array
  readonly       → Variabel konstan
  unset          → Hapus variabel
  export         → Export ke environment

Kondisional:
  if/elif/else   → Percabangan
  case           → Pattern matching
  [ ] / [[ ]]   → Test kondisi
  (( ))          → Arithmetic test

Perulangan:
  for            → Loop dengan list/range
  while          → Loop dengan kondisi
  until          → Loop sampai kondisi true
  select         → Menu interaktif
  break/continue → Kontrol loop

Fungsi:
  function()     → Deklarasi fungsi
  local          → Variabel lokal
  return         → Return status
  $1..$n         → Argumen fungsi

Input/Output:
  read           → Baca input user
  echo           → Tampilkan output
  printf         → Formatted output
  tee            → Split output

Error Handling:
  set -euo       → Safe scripting mode
  trap           → Signal handler
  exit           → Keluar dengan kode
  $?             → Exit code

Aritmatika:
  $(( ))         → Arithmetic expansion
  bc             → Calculator (float)
  let            → Arithmetic command

Utilitas:
  alias          → Command alias
  history        → Riwayat perintah
  xargs          → Build & execute commands
  watch          → Jalankan berkala
  parallel       → Eksekusi paralel
  getopts        → Parse argumen script
```

---

## ✅ Bagian 10 Selesai!

**Lanjut ke Bagian 11: Perintah SSH & Remote?**
