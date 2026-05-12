# Bagian 14: Perintah Variabel Lingkungan & Konfigurasi

---

## 14.1 Variabel Lingkungan (Environment Variables)

### Melihat Variabel Environment

```bash
# Tampilkan semua variabel environment
env                              # Semua environment variables
printenv                         # Sama dengan env
env | sort                       # Diurutkan
env | wc -l                      # Hitung jumlah variabel

# Tampilkan variabel tertentu
printenv PATH                    # Nilai PATH
printenv HOME USER SHELL         # Beberapa variabel
echo $PATH                       # Via ekspansi
echo $HOME                       # Home directory
echo $USER                       # Username saat ini
echo ${VARIABLE:-default}        # Dengan nilai default

# Tampilkan semua variabel shell (termasuk fungsi)
set                              # Semua variabel + fungsi shell
set | grep "^VAR"                # Cari variabel tertentu
declare -p                       # Semua variabel dengan atribut
declare -p PATH                  # Variabel tertentu
declare -x                       # Hanya exported variables
```

---

### Variabel Environment Penting

```bash
# Identitas User
echo $USER                       # Username
echo $LOGNAME                    # Login name
echo $HOME                       # Home directory
echo $SHELL                      # Shell yang digunakan
echo $UID                        # User ID
echo $GROUPS                     # Group IDs

# Sistem
echo $HOSTNAME                   # Hostname
echo $HOSTTYPE                   # Tipe host (x86_64)
echo $OSTYPE                     # Tipe OS (linux-gnu)
echo $MACHTYPE                   # Tipe mesin
echo $PWD                        # Direktori saat ini
echo $OLDPWD                     # Direktori sebelumnya
echo $TMPDIR                     # Direktori temporary
echo $TERM                       # Tipe terminal
echo $COLORTERM                  # Terminal warna
echo $DISPLAY                    # X display
echo $LANG                       # Locale bahasa
echo $LC_ALL                     # Override semua locale
echo $TZ                         # Timezone
echo $PATH                       # Path executable
echo $MANPATH                    # Path manual pages
echo $LD_LIBRARY_PATH            # Path library
echo $PYTHONPATH                 # Path Python modules
echo $GOPATH                     # Go workspace
echo $JAVA_HOME                  # Java installation
echo $EDITOR                     # Default editor
echo $VISUAL                     # Visual editor
echo $PAGER                      # Default pager
echo $BROWSER                    # Default browser
echo $MAIL                       # Mailbox location

# Shell
echo $BASH                       # Path bash executable
echo $BASH_VERSION               # Versi bash
echo $BASH_VERSINFO              # Info versi array
echo $BASHPID                    # PID bash saat ini
echo $$                          # PID shell
echo $PPID                       # PID parent
echo $SHLVL                      # Level nesting shell
echo $RANDOM                     # Angka random
echo $SECONDS                    # Detik sejak shell start
echo $LINENO                     # Nomor baris dalam script
echo $IFS                        # Internal Field Separator
echo $PS1                        # Primary prompt
echo $PS2                        # Secondary prompt
echo $PS3                        # Select prompt
echo $PS4                        # Debug trace prompt
echo $HISTFILE                   # File history
echo $HISTSIZE                   # Ukuran history dalam memori
echo $HISTFILESIZE               # Ukuran file history
echo $HISTCONTROL                # Kontrol history
echo $HISTTIMEFORMAT             # Format timestamp history
```

---

### Mengatur Variabel Environment

```bash
# Set variabel (hanya di shell saat ini)
NAMA="John Doe"
PORT=8080
DEBUG=true

# Export ke environment (tersedia di sub-proses)
export NAMA="John Doe"
export PORT=8080
export DEBUG=true

# Set dan export sekaligus
export NAMA="John Doe" PORT=8080

# Set untuk satu perintah saja
VAR=value perintah               # Hanya untuk perintah ini
HTTP_PROXY=http://proxy:8080 curl https://example.com
DEBUG=1 ./script.sh

# Multiple vars untuk satu perintah
VAR1=val1 VAR2=val2 perintah

# Hapus variabel
unset NAMA                       # Hapus variabel
unset -v NAMA                    # Hapus variabel (eksplisit)
unset -f fungsi                  # Hapus fungsi
export -n NAMA                   # Unexport (tetap ada tapi tidak di-export)

# Readonly variabel
readonly KONSTANTA="tetap"
declare -r KONSTANTA="tetap"
readonly -p                      # Tampilkan semua readonly

# Export dengan tipe
declare -x NAMA="John"           # Export
declare -xi ANGKA=42             # Export integer
declare -xa ARR=("a" "b")       # Export array
declare -xr READONLY="tetap"     # Export readonly
```

---

### Konfigurasi PATH

```bash
# Tampilkan PATH
echo $PATH
echo $PATH | tr ':' '\n'         # Satu per baris

# Tambah ke PATH
export PATH="$PATH:/usr/local/bin"              # Tambah di akhir
export PATH="/usr/local/bin:$PATH"              # Tambah di awal
export PATH="$HOME/.local/bin:$PATH"            # Tambah home/.local/bin
export PATH="$HOME/bin:$PATH"                   # Tambah ~/bin

# Tambah beberapa sekaligus
export PATH="/usr/local/bin:/usr/local/sbin:$PATH"

# Hapus duplikat dari PATH
export PATH=$(echo -n $PATH | awk -v RS=: '!x[$0]++' | tr '\n' ':' | sed 's/:$//')

# Tampilkan PATH yang bersih
echo $PATH | tr ':' '\n' | sort -u

# Verifikasi path
command -v python3               # Cek lokasi python3
which python3                    # Sama
type python3                     # Tipe perintah

# PATH permanen di ~/.bashrc atau ~/.profile
cat >> ~/.bashrc << 'EOF'
export PATH="$HOME/.local/bin:$HOME/bin:$PATH"
EOF

# Script untuk manajemen PATH
path_add() {
    case ":$PATH:" in
        *":$1:"*) ;;                        # Sudah ada, skip
        *) export PATH="$1:$PATH" ;;        # Tambah di awal
    esac
}
path_remove() {
    export PATH=$(echo "$PATH" | tr ':' '\n' | grep -v "^$1$" | tr '\n' ':' | sed 's/:$//')
}
```

---

## 14.2 File Konfigurasi Shell

### Urutan Loading File Konfigurasi

```
Login Shell:
  /etc/profile
  /etc/profile.d/*.sh
  ~/.bash_profile  (atau ~/.bash_login, atau ~/.profile)
  ~/.bashrc (jika dipanggil dari bash_profile)

Non-login Interactive Shell:
  /etc/bash.bashrc
  ~/.bashrc

Non-interactive Shell (Script):
  File yang ditunjuk $BASH_ENV

Logout:
  ~/.bash_logout
```

---

### `~/.bashrc` - Bash Configuration

```bash
# Edit konfigurasi
nano ~/.bashrc
vim ~/.bashrc

# Reload konfigurasi
source ~/.bashrc
. ~/.bashrc

# Contoh ~/.bashrc yang lengkap
cat > ~/.bashrc << 'BASHRC'
# ===== SHELL OPTIONS =====
# Jangan simpan perintah duplikat dan spasi di awal
HISTCONTROL=ignoredups:ignorespace
HISTSIZE=10000
HISTFILESIZE=20000
HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S "
HISTIGNORE="ls:ll:cd:pwd:exit:clear:history"

# Append ke history (jangan overwrite)
shopt -s histappend

# Update LINES dan COLUMNS setelah resize window
shopt -s checkwinsize

# Glob lebih canggih
shopt -s globstar    # ** untuk recursive glob
shopt -s dotglob     # Include hidden files dalam glob
shopt -s nocaseglob  # Case-insensitive glob
shopt -s cdspell     # Koreksi typo saat cd
shopt -s autocd      # cd dengan nama direktori saja

# ===== PROMPT (PS1) =====
# Warna
RED='\[\033[0;31m\]'
GREEN='\[\033[0;32m\]'
YELLOW='\[\033[1;33m\]'
BLUE='\[\033[0;34m\]'
PURPLE='\[\033[0;35m\]'
CYAN='\[\033[0;36m\]'
WHITE='\[\033[1;37m\]'
RESET='\[\033[0m\]'

# Tampilkan git branch di prompt
parse_git_branch() {
    git branch 2>/dev/null | grep "^*" | sed 's/* //'
}

# Prompt dengan git branch
PS1="${GREEN}\u${RESET}@${BLUE}\h${RESET}:${YELLOW}\w${RESET}"
PS1+='$(git rev-parse --git-dir > /dev/null 2>&1 && echo " ($(parse_git_branch))")'
PS1+="${RESET}\$ "

# ===== ALIAS =====
# Navigation
alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'
alias ~='cd ~'
alias -- -='cd -'

# Listing
alias ls='ls --color=auto'
alias ll='ls -alFh'
alias la='ls -Ah'
alias l='ls -CFh'
alias lt='ls -ltrh'           # Sort by time
alias lS='ls -lSrh'           # Sort by size
alias lx='ls -lXBh'           # Sort by extension
alias ld='ls -lad */'         # Only directories

# Safety
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
alias ln='ln -i'

# Disk
alias df='df -h'
alias du='du -h'
alias dus='du -sh *'
alias duf='du -sh * | sort -rh'

# Network
alias ports='ss -tulpn'
alias myip='curl -s ifconfig.me'
alias localip='hostname -I | awk "{print \$1}"'
alias ping='ping -c 5'

# System
alias top='htop'
alias ps='ps aux'
alias psg='ps aux | grep -v grep | grep -i'
alias free='free -h'
alias update='sudo apt update && sudo apt upgrade -y'
alias install='sudo apt install'
alias remove='sudo apt remove'
alias search='apt search'

# Text
alias cat='bat'                # Pakai bat jika tersedia
alias grep='grep --color=auto'
alias diff='diff --color=auto'
alias less='less -R'

# Git
alias g='git'
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
alias gl='git pull'
alias gd='git diff'
alias gb='git branch'
alias gco='git checkout'
alias glog='git log --oneline --graph --decorate'

# Docker
alias d='docker'
alias dc='docker compose'
alias dps='docker ps'
alias dpsa='docker ps -a'
alias di='docker images'
alias dex='docker exec -it'
alias dlogs='docker logs -f'

# Misc
alias c='clear'
alias q='exit'
alias h='history'
alias j='jobs -l'
alias path='echo $PATH | tr ":" "\n"'
alias reload='source ~/.bashrc'
alias edit='nano ~/.bashrc'
alias week='date +%V'
alias now='date +"%T"'
alias today='date +"%Y-%m-%d"'
alias weather='curl wttr.in'
alias serve='python3 -m http.server 8080'
alias calc='bc -l'
alias timer='echo "Timer started. Ctrl+D to stop." && date && time cat && date'

# ===== FUNCTIONS =====
# Buat direktori dan masuk ke dalamnya
mkcd() {
    mkdir -p "$1" && cd "$1"
}

# Extract berbagai format arsip
extract() {
    if [ -f "$1" ]; then
        case "$1" in
            *.tar.bz2)  tar xjf "$1"   ;;
            *.tar.gz)   tar xzf "$1"   ;;
            *.tar.xz)   tar xJf "$1"   ;;
            *.tar.zst)  tar --zstd -xf "$1" ;;
            *.tar)      tar xf "$1"    ;;
            *.bz2)      bunzip2 "$1"   ;;
            *.gz)       gunzip "$1"    ;;
            *.xz)       unxz "$1"      ;;
            *.zip)      unzip "$1"     ;;
            *.7z)       7z x "$1"      ;;
            *.rar)      unrar x "$1"   ;;
            *.Z)        uncompress "$1";;
            *.zst)      unzstd "$1"    ;;
            *)          echo "'$1' tidak bisa diekstrak" ;;
        esac
    else
        echo "'$1' bukan file yang valid"
    fi
}

# Buat backup file
bak() {
    cp "$1" "${1}.bak.$(date +%Y%m%d_%H%M%S)"
}

# Tampilkan top 10 proses berdasarkan CPU
topcpu() {
    ps aux --sort=-%cpu | head -${1:-10}
}

# Tampilkan top 10 proses berdasarkan memory
topmem() {
    ps aux --sort=-%mem | head -${1:-10}
}

# Hitung ukuran direktori
sizeof() {
    du -sh "${1:-.}" | cut -f1
}

# Cari proses
psg() {
    ps aux | grep -v grep | grep -i "$1"
}

# Quick HTTP server
serve() {
    local port="${1:-8080}"
    echo "Serving at http://localhost:$port"
    python3 -m http.server "$port"
}

# Encode/decode base64
b64enc() { echo "$1" | base64; }
b64dec() { echo "$1" | base64 -d; }

# Konversi ke uppercase/lowercase
upper() { echo "$1" | tr '[:lower:]' '[:upper:]'; }
lower() { echo "$1" | tr '[:upper:]' '[:lower:]'; }

# Cari dan ganti dalam file
replace() {
    # Usage: replace "old" "new" file
    sed -i "s/$1/$2/g" "$3"
}

# Tampilkan warna terminal
colors() {
    for i in {0..255}; do
        printf "\033[38;5;%dm%3d\033[0m " "$i" "$i"
        (( (i + 1) % 16 == 0 )) && echo
    done
}

# Countdown timer
countdown() {
    local secs=$1
    while [ $secs -gt 0 ]; do
        printf "\r%02d:%02d" $((secs/60)) $((secs%60))
        sleep 1
        ((secs--))
    done
    echo -e "\rWaktu habis!   "
}

# ===== ENVIRONMENT =====
export EDITOR=nano
export VISUAL=nano
export PAGER=less
export LESS='-R'
export GREP_OPTIONS='--color=auto'
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
export TZ="Asia/Jakarta"

# PATH
export PATH="$HOME/.local/bin:$HOME/bin:$PATH"
export PATH="$PATH:/usr/local/go/bin"            # Go
export PATH="$PATH:$HOME/.cargo/bin"             # Rust

# Node.js
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

# Python
export PYTHONDONTWRITEBYTECODE=1
export PYTHONUNBUFFERED=1

# ===== COMPLETION =====
# Bash completion
if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
fi

# ===== FZF =====
[ -f ~/.fzf.bash ] && source ~/.fzf.bash
export FZF_DEFAULT_OPTS="--height 40% --layout=reverse --border"
export FZF_DEFAULT_COMMAND='fd --type f --hidden --exclude .git'

BASHRC

source ~/.bashrc
```

---

### `~/.bash_profile` & `~/.profile`

```bash
# ~/.bash_profile untuk login shell
cat > ~/.bash_profile << 'EOF'
# Jalankan .bashrc jika ada
if [ -f ~/.bashrc ]; then
    source ~/.bashrc
fi

# Tambahan untuk login shell
export PATH="$HOME/bin:$PATH"
umask 022
EOF

# ~/.profile untuk semua POSIX shell
cat > ~/.profile << 'EOF'
# Tambahkan ~/bin ke PATH
if [ -d "$HOME/bin" ]; then
    PATH="$HOME/bin:$PATH"
fi

if [ -d "$HOME/.local/bin" ]; then
    PATH="$HOME/.local/bin:$PATH"
fi
EOF
```

---

### `~/.bash_logout`

```bash
cat > ~/.bash_logout << 'EOF'
# Jalankan saat logout dari login shell
clear
echo "Sampai jumpa, $USER!"
history -a    # Simpan history ke file
EOF
```

---

## 14.3 Konfigurasi System-wide

### `/etc/environment` - System-wide Environment

```bash
# Variabel environment untuk semua user (tidak shell-specific)
cat /etc/environment

# Edit
sudo nano /etc/environment

# Contoh isi /etc/environment:
cat /etc/environment
# PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
# JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64"
# EDITOR="nano"
# LANG="en_US.UTF-8"
# TZ="Asia/Jakarta"

# Load manual (biasanya sudah otomatis)
. /etc/environment
set -a; . /etc/environment; set +a
```

---

### `/etc/profile` & `/etc/profile.d/`

```bash
# File utama (semua user, login shell)
cat /etc/profile

# Direktori untuk script tambahan
ls /etc/profile.d/

# Tambah konfigurasi custom
sudo nano /etc/profile.d/custom.sh
cat > /etc/profile.d/custom.sh << 'EOF'
#!/bin/bash
# Konfigurasi environment untuk semua user

# Java
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH

# Python
export PYTHONDONTWRITEBYTECODE=1

# Custom prompt
export PS1='\u@\h:\w\$ '

# Ulimit
ulimit -n 65535          # Maksimum open files
ulimit -u 4096           # Maksimum user processes

echo "Sistem diinisialisasi: $(date)"
EOF

sudo chmod +x /etc/profile.d/custom.sh
```

---

### `/etc/bash.bashrc` - Global Bashrc

```bash
# Konfigurasi bash untuk semua user (non-login interactive)
cat /etc/bash.bashrc

sudo nano /etc/bash.bashrc
```

---

## 14.4 Locale & Timezone

### Konfigurasi Locale

```bash
# Lihat locale saat ini
locale
locale -a                        # Semua locale yang tersedia
locale -k LC_TIME                # Detail kategori tertentu

# Variabel locale
echo $LANG                       # Bahasa utama
echo $LC_ALL                     # Override semua
echo $LC_MESSAGES                # Bahasa pesan
echo $LC_TIME                    # Format waktu
echo $LC_NUMERIC                 # Format angka
echo $LC_MONETARY                # Format mata uang
echo $LC_COLLATE                 # Urutan pengurutan
echo $LC_CTYPE                   # Klasifikasi karakter

# Set locale
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
export LC_TIME=id_ID.UTF-8       # Format waktu Indonesia

# Generate locale
sudo locale-gen en_US.UTF-8
sudo locale-gen id_ID.UTF-8
sudo update-locale LANG=en_US.UTF-8

# Konfigurasi locale permanen
sudo dpkg-reconfigure locales    # Interaktif (Debian/Ubuntu)
sudo localectl set-locale LANG=en_US.UTF-8  # Systemd

# Lihat konfigurasi locale systemd
localectl
localectl status
localectl list-locales           # Semua locale tersedia
localectl set-locale LANG=en_US.UTF-8
localectl set-locale LC_TIME=id_ID.UTF-8
```

---

### Konfigurasi Timezone

```bash
# Lihat timezone saat ini
date
timedatectl
timedatectl status
echo $TZ

# Daftar timezone
timedatectl list-timezones
timedatectl list-timezones | grep Asia
timedatectl list-timezones | grep Indonesia
ls /usr/share/zoneinfo/
ls /usr/share/zoneinfo/Asia/

# Set timezone
sudo timedatectl set-timezone Asia/Jakarta
sudo timedatectl set-timezone UTC
sudo timedatectl set-timezone America/New_York

# Cara manual
sudo ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
sudo dpkg-reconfigure tzdata    # Interaktif (Debian/Ubuntu)

# Set per-sesi
export TZ="Asia/Jakarta"
TZ="Asia/Tokyo" date            # Timezone satu perintah saja

# Verifikasi
date
date -u                          # UTC
date +"%Z %z"                   # Nama dan offset timezone
```

---

## 14.5 Konfigurasi Ulimit

### `ulimit` - User Limits

```bash
# Tampilkan semua limit
ulimit -a                        # Semua limit (soft)
ulimit -aH                       # Semua limit (hard)
ulimit -Sa                       # Soft limits
ulimit -Ha                       # Hard limits

# Limit tertentu
ulimit -n                        # Maksimum open files (soft)
ulimit -Hn                       # Maksimum open files (hard)
ulimit -u                        # Maksimum user processes
ulimit -m                        # Maksimum memory size (KB)
ulimit -v                        # Maksimum virtual memory (KB)
ulimit -s                        # Stack size (KB)
ulimit -t                        # CPU time (detik)
ulimit -f                        # Maksimum file size (blok 512 byte)
ulimit -c                        # Core file size
ulimit -l                        # Locked memory (KB)
ulimit -q                        # POSIX message queues (bytes)
ulimit -r                        # Real-time priority
ulimit -x                        # File locks
ulimit -i                        # Pending signals
ulimit -e                        # Scheduling priority (nice)

# Set limit
ulimit -n 65535                  # Set open files ke 65535
ulimit -u 4096                   # Set max processes ke 4096
ulimit -s unlimited              # Unlimited stack size
ulimit -c unlimited              # Unlimited core dumps
ulimit -n unlimited              # Unlimited open files

# Limit hanya berlaku untuk sesi saat ini
# Untuk permanen, edit /etc/security/limits.conf

# Konfigurasi /etc/security/limits.conf
cat /etc/security/limits.conf
sudo nano /etc/security/limits.conf

# Format:
# <domain> <type> <item> <value>
# domain: username, @group, *, atau @syslog
# type: soft, hard, atau -
# item: nofile, nproc, stack, dll.
# value: nilai atau unlimited

cat >> /etc/security/limits.conf << 'EOF'
# Konfigurasi untuk semua user
*               soft    nofile          65535
*               hard    nofile          65535
*               soft    nproc           4096
*               hard    nproc           8192

# Konfigurasi untuk user tertentu
webserver       soft    nofile          100000
webserver       hard    nofile          100000

# Konfigurasi untuk grup
@developers     soft    nproc           10000
@developers     hard    nproc           20000

# Database user
mysql           soft    nofile          65535
mysql           hard    nofile          65535
mysql           soft    memlock         unlimited
mysql           hard    memlock         unlimited
EOF

# Konfigurasi systemd service limits
sudo mkdir -p /etc/systemd/system/nginx.service.d/
cat > /etc/systemd/system/nginx.service.d/limits.conf << 'EOF'
[Service]
LimitNOFILE=65535
LimitNPROC=4096
LimitMEMLOCK=infinity
EOF
sudo systemctl daemon-reload
sudo systemctl restart nginx

# Cek limit proses yang berjalan
cat /proc/$(pgrep nginx | head -1)/limits
```

---

## 14.6 Konfigurasi Kernel (sysctl)

### `/etc/sysctl.conf` & `sysctl.d/`

```bash
# Lihat semua parameter kernel
sysctl -a                        # Semua parameter
sysctl -a | wc -l               # Hitung jumlah
sysctl -a | grep net.ipv4        # Filter jaringan IPv4
sysctl net.ipv4.ip_forward       # Parameter tertentu
sysctl -n net.ipv4.ip_forward    # Hanya nilai
sysctl -q net.ipv4.ip_forward    # Quiet

# Set parameter sementara
sudo sysctl net.ipv4.ip_forward=1
sudo sysctl -w net.ipv4.ip_forward=1
sudo sysctl net.core.somaxconn=65535

# Load dari file
sudo sysctl -p                   # Load /etc/sysctl.conf
sudo sysctl -p /etc/sysctl.d/custom.conf  # File tertentu
sudo sysctl --system             # Load semua file konfigurasi

# Konfigurasi permanen
cat /etc/sysctl.conf
ls /etc/sysctl.d/

# Buat file konfigurasi
sudo cat > /etc/sysctl.d/99-custom.conf << 'EOF'
# ===== KEAMANAN JARINGAN =====
# Nonaktifkan IP forwarding (kecuali untuk router)
net.ipv4.ip_forward = 0

# Aktifkan SYN cookies (proteksi SYN flood)
net.ipv4.tcp_syncookies = 1

# Tolak ICMP redirects
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv6.conf.all.accept_redirects = 0

# Tolak source routing
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0

# Log martian packets
net.ipv4.conf.all.log_martians = 1

# Aktifkan reverse path filtering
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1

# Tolak ICMP broadcasts
net.ipv4.icmp_echo_ignore_broadcasts = 1

# Ignore bogus ICMP errors
net.ipv4.icmp_ignore_bogus_error_responses = 1

# ===== PERFORMA JARINGAN =====
# TCP buffer sizes
net.core.rmem_default = 1048576
net.core.wmem_default = 1048576
net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.ipv4.tcp_rmem = 4096 1048576 67108864
net.ipv4.tcp_wmem = 4096 1048576 67108864

# Maksimum koneksi
net.core.somaxconn = 65535
net.core.netdev_max_backlog = 65535
net.ipv4.tcp_max_syn_backlog = 65535

# TIME_WAIT
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_fin_timeout = 30

# Keepalive
net.ipv4.tcp_keepalive_time = 1200
net.ipv4.tcp_keepalive_intvl = 30
net.ipv4.tcp_keepalive_probes = 5

# ===== MEMORI =====
# Swappiness (0-100, semakin rendah semakin jarang swap)
vm.swappiness = 10

# Dirty page ratio
vm.dirty_ratio = 15
vm.dirty_background_ratio = 5

# Overcommit memory
vm.overcommit_memory = 0
vm.overcommit_ratio = 80

# ===== FILESYSTEM =====
# Maksimum file descriptors
fs.file-max = 2097152

# Inotify watches
fs.inotify.max_user_watches = 524288
fs.inotify.max_user_instances = 512
fs.inotify.max_queued_events = 16384

# ===== KERNEL =====
# Maksimum PID
kernel.pid_max = 4194304

# Panic timeout (reboot setelah kernel panic)
kernel.panic = 10

# Nonaktifkan core dumps SUID programs
fs.suid_dumpable = 0

# Randomize virtual address space (ASLR)
kernel.randomize_va_space = 2

# ===== UNTUK SERVER DATABASE =====
# vm.nr_hugepages = 1024
# kernel.shmmax = 68719476736
# kernel.shmall = 4294967296
EOF

# Terapkan perubahan
sudo sysctl --system
sudo sysctl -p /etc/sysctl.d/99-custom.conf
```

---

## 14.7 Konfigurasi Aplikasi

### `.env` Files - Environment per Aplikasi

```bash
# File .env untuk aplikasi
cat > .env << 'EOF'
# Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=myapp
DB_USER=admin
DB_PASSWORD=secret123

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# API Keys
API_KEY=your-api-key-here
SECRET_KEY=your-secret-key-here

# App Config
APP_ENV=production
APP_PORT=8080
APP_DEBUG=false
LOG_LEVEL=info

# External Services
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your@email.com
SMTP_PASS=email-password
EOF

# Load .env dalam bash
export $(grep -v '^#' .env | xargs)

# Atau lebih aman:
set -a
source .env
set +a

# Fungsi untuk load .env
load_env() {
    local env_file="${1:-.env}"
    if [ -f "$env_file" ]; then
        while IFS='=' read -r key value; do
            [[ $key =~ ^#.*$ ]] && continue  # Skip komentar
            [[ -z $key ]] && continue         # Skip baris kosong
            export "$key=$value"
        done < "$env_file"
    fi
}

# .env.example untuk dokumentasi
cat > .env.example << 'EOF'
# Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=appname
DB_USER=
DB_PASSWORD=

# API Keys (isi nilai sebenarnya)
API_KEY=
SECRET_KEY=
EOF

# Jangan commit .env ke git
echo ".env" >> .gitignore
echo ".env.local" >> .gitignore
```

---

### Konfigurasi Aplikasi Umum

```bash
# Nginx
cat /etc/nginx/nginx.conf
cat /etc/nginx/sites-available/default
sudo nginx -t                    # Test konfigurasi
sudo nginx -T                    # Dump konfigurasi
sudo systemctl reload nginx      # Reload tanpa restart

# Apache
cat /etc/apache2/apache2.conf
sudo apache2ctl configtest       # Test konfigurasi
sudo apache2ctl -S               # Virtual host info
sudo a2ensite mysite.conf        # Enable site
sudo a2dissite default           # Disable site
sudo a2enmod rewrite             # Enable modul
sudo a2dismod mpm_prefork        # Disable modul
sudo systemctl reload apache2    # Reload

# MySQL/MariaDB
cat /etc/mysql/mysql.conf.d/mysqld.cnf
mysql -u root -p -e "SHOW VARIABLES;"
mysql -u root -p -e "SHOW STATUS;"
mysqladmin -u root -p status     # Status server
mysqladmin -u root -p variables  # Semua variabel
sudo mysqladmin -u root -p reload  # Reload grant tables
sudo systemctl restart mysql     # Restart

# PostgreSQL
cat /etc/postgresql/*/main/postgresql.conf
cat /etc/postgresql/*/main/pg_hba.conf
sudo -u postgres psql -c "SHOW ALL;"
sudo pg_ctlcluster 14 main reload  # Reload
sudo systemctl restart postgresql  # Restart

# Redis
cat /etc/redis/redis.conf
redis-cli ping                   # Cek koneksi
redis-cli info                   # Info server
redis-cli config get maxmemory   # Baca konfigurasi
redis-cli config set maxmemory 1gb  # Set konfigurasi
redis-cli config rewrite         # Simpan ke file

# PHP
php --ini                        # File konfigurasi
php -i | grep "php.ini"         # Path php.ini
php -i | grep "memory_limit"    # Memory limit
php -r "phpinfo();" | grep -i "loaded"  # Extensions
```

---

## 14.8 `direnv` - Directory-specific Environment

```bash
# Install
sudo apt install direnv

# Tambahkan ke ~/.bashrc
eval "$(direnv hook bash)"

# Buat .envrc dalam direktori proyek
cat > project/.envrc << 'EOF'
export DB_HOST=localhost
export DB_PORT=5432
export API_KEY=dev-api-key
export PATH="$PWD/bin:$PATH"
export PYTHONPATH="$PWD/src:$PYTHONPATH"
EOF

# Izinkan .envrc
cd project/
direnv allow .                   # Izinkan direktori saat ini
direnv allow /path/to/project    # Izinkan path tertentu
direnv deny .                    # Tolak
direnv reload                    # Reload .envrc
direnv status                    # Status

# .envrc diload otomatis saat masuk direktori
# dan di-unload saat keluar direktori
```

---

## 14.9 Konfigurasi Git

### `~/.gitconfig` & Konfigurasi Git

```bash
# Lihat konfigurasi
git config --list                # Semua konfigurasi
git config --global --list       # Global saja
git config --system --list       # System saja
git config --local --list        # Lokal saja
git config user.name             # Nilai tertentu

# Set konfigurasi global
git config --global user.name "Nama Anda"
git config --global user.email "email@example.com"
git config --global core.editor nano
git config --global core.editor "code --wait"     # VSCode
git config --global core.editor "vim"             # Vim
git config --global init.defaultBranch main
git config --global pull.rebase false
git config --global push.default current
git config --global merge.tool vimdiff
git config --global diff.tool vimdiff
git config --global color.ui auto
git config --global core.autocrlf input          # Linux
git config --global core.autocrlf true           # Windows

# Alias git
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.lg "log --oneline --graph --decorate --all"
git config --global alias.aliases "config --get-regexp alias"

# Credential storage
git config --global credential.helper store        # Simpan plaintext
git config --global credential.helper cache        # Cache sementara
git config --global credential.helper "cache --timeout=3600"  # 1 jam

# Contoh ~/.gitconfig lengkap
cat > ~/.gitconfig << 'EOF'
[user]
    name = Nama Anda
    email = email@example.com
    signingkey = GPG_KEY_ID

[core]
    editor = nano
    autocrlf = input
    whitespace = fix,-indent-with-non-tab,trailing-space,cr-at-eol
    excludesfile = ~/.gitignore_global
    pager = less -FRX

[init]
    defaultBranch = main

[pull]
    rebase = false

[push]
    default = current
    autoSetupRemote = true

[merge]
    tool = vimdiff
    ff = false

[diff]
    tool = vimdiff
    colorMoved = zebra

[color]
    ui = auto
    branch = auto
    diff = auto
    status = auto

[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    lg = log --oneline --graph --decorate --all
    last = log -1 HEAD
    unstage = reset HEAD --
    aliases = config --get-regexp alias
    undo = reset HEAD~1 --mixed
    amend = commit --amend --no-edit
    stash-all = stash save --include-untracked
    pub = push -u origin HEAD
    cleanup = "!git branch --merged | grep -v '\\*\\|main\\|master\\|develop' | xargs git branch -d"

[credential]
    helper = cache --timeout=3600

[commit]
    gpgsign = false
    template = ~/.gitmessage

[tag]
    forceSignAnnotated = false

[rerere]
    enabled = true

[help]
    autocorrect = 1

[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge -- %f
    process = git-lfs filter-process
    required = true

[url "git@github.com:"]
    insteadOf = https://github.com/
EOF

# Global gitignore
cat > ~/.gitignore_global << 'EOF'
# OS
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db
desktop.ini

# Editor
.vscode/
.idea/
*.swp
*.swo
*~
.project
.classpath
.settings/

# Logs
*.log
logs/

# Temporary
*.tmp
*.temp
*.bak
*.backup
*.orig

# Build
build/
dist/
*.egg-info/
__pycache__/
*.pyc
*.pyo
.pytest_cache/
node_modules/
.npm/
EOF

git config --global core.excludesfile ~/.gitignore_global
```

---

## 14.10 Konfigurasi Editor

### Nano Configuration

```bash
# File konfigurasi nano
cat > ~/.nanorc << 'EOF'
# Aktifkan syntax highlighting
include "/usr/share/nano/*.nanorc"
include "/usr/share/nano/extra/*.nanorc"

# Tampilkan nomor baris
set linenumbers

# Tampilkan posisi cursor
set constantshow

# Auto-indent
set autoindent

# Konversi tab ke spasi
set tabspaces 4

# Tampilkan whitespace
set whitespace "»·"

# Mouse support
set mouse

# Backup file
set backup
set backupdir "~/.nano_backups"

# Soft wrap
set softwrap

# Case-sensitive search (default)
# set casesensitive

# History pencarian
set historylog

# Spell checker
set speller "aspell -x -c"

# Kata kunci
set wordchars "_"

# Warna
set titlecolor bold,white,blue
set statuscolor bold,white,green
set errorcolor bold,white,red
set selectedcolor bold,white,cyan
set numbercolor cyan
set keycolor cyan
set functioncolor green
EOF
```

---

### Vim Configuration

```bash
# File konfigurasi vim
cat > ~/.vimrc << 'EOF'
" ===== DASAR =====
set nocompatible              " Gunakan mode vim, bukan vi
syntax enable                 " Syntax highlighting
set encoding=utf-8            " Encoding UTF-8
set fileencoding=utf-8
set number                    " Tampilkan nomor baris
set relativenumber            " Nomor baris relatif
set ruler                     " Tampilkan posisi cursor
set showcmd                   " Tampilkan perintah yang sedang diketik
set showmode                  " Tampilkan mode saat ini
set laststatus=2              " Selalu tampilkan status bar
set wildmenu                  " Enhanced command completion
set wildmode=list:longest     " Completion mode

" ===== INDENTASI =====
set autoindent                " Auto indent
set smartindent               " Smart indent
set tabstop=4                 " Tab = 4 spasi
set shiftwidth=4              " Indent = 4 spasi
set expandtab                 " Tab ke spasi
set softtabstop=4

" ===== PENCARIAN =====
set incsearch                 " Incremental search
set hlsearch                  " Highlight hasil pencarian
set ignorecase                " Case insensitive
set smartcase                 " Case sensitive jika ada uppercase

" ===== TAMPILAN =====
set cursorline                " Highlight baris cursor
set showmatch                 " Highlight bracket yang cocok
set scrolloff=8               " Keep 8 baris di atas/bawah cursor
set sidescrolloff=8
set wrap                      " Wrap panjang baris
set linebreak                 " Wrap di kata, bukan karakter
set colorcolumn=80            " Garis kolom 80
set background=dark           " Background gelap
colorscheme desert            " Color scheme (ganti sesuai keinginan)

" ===== PERFORMA =====
set hidden                    " Buffer hidden saat ditinggalkan
set history=1000              " Simpan 1000 history
set undofile                  " Persistent undo
set undodir=~/.vim/undodir    " Direktori undo
set noswapfile                " Tidak buat swap file
set nobackup                  " Tidak buat backup
set updatetime=250            " Faster update

" ===== KEY MAPPING =====
" Leader key
let mapleader = ","

" Simpan dengan Ctrl+S
nnoremap <C-s> :w<CR>
inoremap <C-s> <Esc>:w<CR>a

" Quit dengan Q
nnoremap Q :q<CR>

" Clear search highlight
nnoremap <leader>/ :noh<CR>

" Pindah antar window
nnoremap <C-h> <C-w>h
nnoremap <C-j> <C-w>j
nnoremap <C-k> <C-w>k
nnoremap <C-l> <C-w>l

" Resize window
nnoremap <C-Up> :resize +2<CR>
nnoremap <C-Down> :resize -2<CR>
nnoremap <C-Left> :vertical resize -2<CR>
nnoremap <C-Right> :vertical resize +2<CR>

" Tab management
nnoremap <leader>tn :tabnew<CR>
nnoremap <leader>tc :tabclose<CR>
nnoremap <leader>th :tabprev<CR>
nnoremap <leader>tl :tabnext<CR>

" ===== PLUGIN (jika menggunakan vim-plug) =====
" call plug#begin('~/.vim/plugged')
" Plug 'preservim/nerdtree'
" Plug 'vim-airline/vim-airline'
" Plug 'junegunn/fzf.vim'
" call plug#end()

" ===== AUTOCMD =====
" Hapus trailing whitespace saat save
autocmd BufWritePre * %s/\s\+$//e

" Return ke posisi terakhir
autocmd BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif

" Buat direktori jika belum ada saat save
autocmd BufWritePre * silent! call mkdir(expand('<afile>:p:h'), 'p')
EOF

# Buat direktori undo
mkdir -p ~/.vim/undodir
```

---

## 14.11 Konfigurasi Tmux

```bash
# Konfigurasi tmux lengkap
cat > ~/.tmux.conf << 'EOF'
# ===== PREFIX KEY =====
set -g prefix C-a                    # Ganti prefix ke Ctrl+A
unbind C-b
bind C-a send-prefix

# ===== DASAR =====
set -g default-terminal "screen-256color"  # 256 warna
set -g base-index 1                  # Mulai dari index 1
setw -g pane-base-index 1           # Pane mulai dari index 1
set -g renumber-windows on          # Renumber saat window ditutup
set -g history-limit 50000          # History 50000 baris
set -g display-time 4000            # Pesan ditampilkan 4 detik
set -sg escape-time 0               # Tanpa delay ESC
set -g focus-events on

# ===== MOUSE =====
set -g mouse on                      # Aktifkan mouse

# ===== RELOAD CONFIG =====
bind r source-file ~/.tmux.conf \; display "Config reloaded!"

# ===== SPLIT PANE =====
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"
bind c new-window -c "#{pane_current_path}"
unbind '"'
unbind %

# ===== NAVIGASI PANE (vim-like) =====
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# Pindah dengan shift+arrow
bind -n S-Left select-pane -L
bind -n S-Right select-pane -R
bind -n S-Up select-pane -U
bind -n S-Down select-pane -D

# ===== RESIZE PANE =====
bind -r H resize-pane -L 5
bind -r J resize-pane -D 5
bind -r K resize-pane -U 5
bind -r L resize-pane -R 5

# ===== COPY MODE =====
setw -g mode-keys vi                 # Vi keys dalam copy mode
bind Enter copy-mode                 # Enter untuk copy mode
bind -T copy-mode-vi v send -X begin-selection  # v untuk seleksi
bind -T copy-mode-vi y send -X copy-selection-and-cancel  # y untuk copy
bind -T copy-mode-vi C-v send -X rectangle-toggle  # Ctrl+V untuk block select
bind p paste-buffer                  # p untuk paste

# Copy ke clipboard (Linux)
bind -T copy-mode-vi y send -X copy-pipe-and-cancel "xclip -in -selection clipboard"

# ===== STATUS BAR =====
set -g status on
set -g status-interval 5            # Update setiap 5 detik
set -g status-position bottom
set -g status-justify left

# Warna status bar
set -g status-style fg=white,bg=colour235

# Kiri status bar
set -g status-left-length 50
set -g status-left "#[fg=colour39,bold] [#S] #[fg=colour245]| "

# Kanan status bar
set -g status-right-length 100
set -g status-right "#[fg=colour245]| #[fg=colour39]CPU: #{cpu_percentage} #[fg=colour245]| #[fg=colour39]RAM: #{ram_percentage} #[fg=colour245]| #[fg=colour39]%Y-%m-%d %H:%M "

# Window status
setw -g window-status-format " #I:#W "
setw -g window-status-current-format "#[fg=colour39,bold,bg=colour236] #I:#W "
setw -g window-status-style fg=colour245,bg=colour235
setw -g window-status-current-style fg=colour39,bold,bg=colour236

# ===== PANE BORDER =====
set -g pane-border-style fg=colour238
set -g pane-active-border-style fg=colour39

# ===== NOTIFIKASI =====
setw -g monitor-activity on
set -g visual-activity off
set -g visual-bell off

# ===== PLUGIN (tmux plugin manager) =====
# set -g @plugin 'tmux-plugins/tpm'
# set -g @plugin 'tmux-plugins/tmux-sensible'
# set -g @plugin 'tmux-plugins/tmux-resurrect'
# set -g @plugin 'tmux-plugins/tmux-continuum'
# set -g @plugin 'tmux-plugins/tmux-cpu'
# run '~/.tmux/plugins/tpm/tpm'
EOF
```

---

## Ringkasan Perintah Bagian 14

```
Environment Variables:
  env/printenv     → Tampilkan variabel environment
  export           → Set dan export variabel
  unset            → Hapus variabel
  declare          → Deklarasi variabel dengan atribut
  readonly         → Variabel konstan

PATH Management:
  echo $PATH       → Tampilkan PATH
  export PATH=...  → Tambah ke PATH
  which/command -v → Cek lokasi executable

Shell Config Files:
  ~/.bashrc        → Konfigurasi bash (interactive)
  ~/.bash_profile  → Konfigurasi bash (login)
  ~/.profile       → Konfigurasi semua POSIX shell
  /etc/environment → Environment system-wide
  /etc/profile     → Profile system-wide
  /etc/profile.d/  → Script konfigurasi tambahan

Locale & Timezone:
  locale           → Lihat/set locale
  localectl        → Manajemen locale (systemd)
  timedatectl      → Manajemen timezone (systemd)
  date             → Tanggal dan waktu

Limits:
  ulimit           → User resource limits
  /etc/security/limits.conf  → Konfigurasi permanen

Kernel Parameters:
  sysctl -a        → Semua parameter kernel
  sysctl key=val   → Set parameter
  /etc/sysctl.conf → Konfigurasi permanen
  /etc/sysctl.d/   → Konfigurasi tambahan

Aplikasi:
  .env             → Environment per aplikasi
  direnv           → Directory-specific environment
  git config       → Konfigurasi git
  ~/.nanorc        → Konfigurasi nano
  ~/.vimrc         → Konfigurasi vim
  ~/.tmux.conf     → Konfigurasi tmux
```

---

## ✅ Bagian 14 Selesai!

**Lanjut ke Bagian 15: Perintah Tambahan & Utilitas Lanjutan (Bagian Terakhir)?**
