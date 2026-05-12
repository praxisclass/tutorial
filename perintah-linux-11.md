# Bagian 11: Perintah SSH & Remote

---

## 11.1 SSH - Secure Shell

### `ssh` - Connect to Remote Server
```bash
ssh [opsi] [user@]host [perintah]
```
> Protokol jaringan untuk koneksi remote yang aman

---

### Koneksi Dasar

| Perintah | Penjelasan |
|---|---|
| `ssh user@hostname` | Koneksi ke server |
| `ssh user@192.168.1.100` | Koneksi via IP |
| `ssh hostname` | Koneksi dengan user saat ini |
| `ssh -p 2222 user@hostname` | Port SSH tertentu |
| `ssh -i ~/.ssh/id_rsa user@hostname` | Gunakan private key tertentu |
| `ssh -v user@hostname` | Verbose (debug) |
| `ssh -vv user@hostname` | Lebih verbose |
| `ssh -vvv user@hostname` | Maksimum verbose |
| `ssh -q user@hostname` | Quiet mode |
| `ssh -l user hostname` | Tentukan username dengan -l |
| `ssh user@hostname "perintah"` | Jalankan perintah di remote |
| `ssh user@hostname "ls -la /home"` | Jalankan ls di remote |
| `ssh user@hostname "sudo systemctl restart nginx"` | Restart service |
| `ssh -t user@hostname "sudo bash"` | Force pseudo-terminal |
| `ssh -T user@hostname` | Tanpa pseudo-terminal |
| `ssh -4 user@hostname` | Gunakan IPv4 |
| `ssh -6 user@hostname` | Gunakan IPv6 |
| `ssh -a user@hostname` | Nonaktifkan agent forwarding |
| `ssh -A user@hostname` | Aktifkan agent forwarding |
| `ssh -X user@hostname` | Aktifkan X11 forwarding |
| `ssh -Y user@hostname` | Trusted X11 forwarding |
| `ssh -C user@hostname` | Aktifkan kompresi |
| `ssh -o "StrictHostKeyChecking=no" user@host` | Skip host key check |
| `ssh -o "ConnectTimeout=10" user@host` | Timeout koneksi |
| `ssh -o "ServerAliveInterval=60" user@host` | Keep-alive tiap 60 detik |

---

### SSH dengan Jump Host (Bastion)

```bash
# Koneksi melalui jump host
ssh -J user@jumphost user@target

# Multiple jump host
ssh -J user@jump1,user@jump2 user@target

# Dengan port berbeda
ssh -J user@jumphost:2222 user@target

# Dengan key berbeda
ssh -J user@jumphost -i ~/.ssh/target_key user@target

# ProxyJump dalam config
Host target
    HostName 10.0.0.100
    User admin
    ProxyJump user@jumphost.example.com

# ProxyCommand (cara lama)
ssh -o ProxyCommand="ssh -W %h:%p user@jumphost" user@target
```

---

### SSH Tunneling

```bash
# Local Port Forwarding
# Akses remote_host:remote_port via localhost:local_port
ssh -L local_port:remote_host:remote_port user@ssh_server

# Contoh: akses database di server via localhost:3306
ssh -L 3306:localhost:3306 user@db-server
ssh -L 3306:db-internal:3306 user@jump-server

# Remote Port Forwarding
# Buka port di remote server yang forward ke lokal
ssh -R remote_port:localhost:local_port user@ssh_server

# Contoh: expose localhost:8080 ke port 80 di server
ssh -R 80:localhost:8080 user@server

# Dynamic Port Forwarding (SOCKS Proxy)
ssh -D local_port user@ssh_server
ssh -D 1080 user@server   # Buat SOCKS5 proxy di port 1080

# Tunnel tanpa shell (background)
ssh -N -f -L 3306:localhost:3306 user@server
ssh -N -f -D 1080 user@server
# -N = tidak menjalankan remote command
# -f = background sebelum eksekusi

# Reverse tunnel (server ke client)
ssh -N -f -R 2222:localhost:22 user@public-server
# Kemudian dari server:
# ssh -p 2222 localhost

# Contoh tunnel MySQL
ssh -L 3307:127.0.0.1:3306 user@server &
mysql -h 127.0.0.1 -P 3307 -u dbuser -p

# Contoh tunnel Redis
ssh -L 6380:127.0.0.1:6379 user@server &
redis-cli -h 127.0.0.1 -p 6380

# Tunnel dengan autossh (reconnect otomatis)
sudo apt install autossh
autossh -M 20000 -N -f -L 3306:localhost:3306 user@server
autossh -M 0 -N -f -o "ServerAliveInterval=30" \
    -L 3306:localhost:3306 user@server
```

---

### Konfigurasi SSH Client

```bash
# File konfigurasi: ~/.ssh/config
# Format:
# Host <alias>
#     HostName <hostname/IP>
#     User <username>
#     Port <port>
#     IdentityFile <path_to_key>
#     <opsi_lain> <nilai>

# Contoh ~/.ssh/config
cat > ~/.ssh/config << 'EOF'
# Default untuk semua host
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    ConnectTimeout 10
    Compression yes
    AddKeysToAgent yes
    IdentityFile ~/.ssh/id_ed25519

# Server production
Host prod
    HostName prod.example.com
    User admin
    Port 22
    IdentityFile ~/.ssh/prod_key

# Server development
Host dev
    HostName dev.example.com
    User developer
    Port 2222
    IdentityFile ~/.ssh/dev_key

# Database server (via jump host)
Host db
    HostName 10.0.0.50
    User dbadmin
    ProxyJump admin@bastion.example.com
    IdentityFile ~/.ssh/db_key

# Server dengan X11 forwarding
Host gui-server
    HostName 192.168.1.200
    User user
    ForwardX11 yes
    ForwardX11Trusted yes

# Multiple hosts dengan wildcard
Host 192.168.1.*
    User admin
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null

# AWS EC2
Host aws-*
    User ec2-user
    IdentityFile ~/.ssh/aws_key.pem
    StrictHostKeyChecking no
EOF

# Set permission yang benar
chmod 600 ~/.ssh/config

# Setelah konfigurasi, koneksi lebih singkat:
ssh prod              # Sama dengan: ssh admin@prod.example.com
ssh dev               # Sama dengan: ssh -p 2222 developer@dev.example.com
ssh db                # Otomatis melalui jump host
```

---

## 11.2 Manajemen Kunci SSH

### `ssh-keygen` - Generate SSH Keys
```bash
ssh-keygen [opsi]
```
> Membuat, mengelola, dan mengkonversi kunci autentikasi SSH

**Membuat Key Pair:**
| Perintah | Penjelasan |
|---|---|
| `ssh-keygen` | Buat key pair RSA (interaktif) |
| `ssh-keygen -t ed25519` | Buat key ED25519 (direkomendasikan) |
| `ssh-keygen -t rsa -b 4096` | Buat key RSA 4096-bit |
| `ssh-keygen -t ecdsa -b 521` | Buat key ECDSA 521-bit |
| `ssh-keygen -t ed25519 -C "email@example.com"` | Dengan komentar |
| `ssh-keygen -t ed25519 -f ~/.ssh/custom_key` | Nama file kustom |
| `ssh-keygen -t ed25519 -N "passphrase"` | Dengan passphrase |
| `ssh-keygen -t ed25519 -N ""` | Tanpa passphrase |
| `ssh-keygen -t ed25519 -C "server-key" -f ~/.ssh/server` | Lengkap |

**Mengelola Key:**
| Perintah | Penjelasan |
|---|---|
| `ssh-keygen -l -f ~/.ssh/id_ed25519.pub` | Tampilkan fingerprint |
| `ssh-keygen -l -E md5 -f ~/.ssh/id_ed25519.pub` | Fingerprint MD5 |
| `ssh-keygen -l -E sha256 -f ~/.ssh/id_ed25519.pub` | Fingerprint SHA256 |
| `ssh-keygen -v -l -f ~/.ssh/id_ed25519.pub` | Fingerprint + visual art |
| `ssh-keygen -p -f ~/.ssh/id_ed25519` | Ubah passphrase |
| `ssh-keygen -p -P "lama" -N "baru" -f ~/.ssh/id_ed25519` | Non-interaktif |
| `ssh-keygen -y -f ~/.ssh/id_ed25519` | Tampilkan public key dari private |
| `ssh-keygen -R hostname` | Hapus host dari known_hosts |
| `ssh-keygen -R "[hostname]:port"` | Hapus host dengan port tertentu |
| `ssh-keygen -H -f ~/.ssh/known_hosts` | Hash known_hosts |
| `ssh-keygen -F hostname` | Cari host di known_hosts |

**Konversi Key:**
```bash
# Konversi ke format PEM
ssh-keygen -e -m PEM -f ~/.ssh/id_ed25519.pub > id_ed25519.pem

# Konversi dari OpenSSH ke RFC4716
ssh-keygen -e -f ~/.ssh/id_ed25519.pub

# Konversi PKCS8 ke OpenSSH
ssh-keygen -i -m PKCS8 -f key.pub

# Buat sertifikat SSH
ssh-keygen -s ca_key -I "user_id" -n "username" user_key.pub
ssh-keygen -s ca_key -I "host_id" -h -n "hostname" host_key.pub
```

---

### `ssh-copy-id` - Copy Public Key to Server
```bash
ssh-copy-id [opsi] user@host
```
> Menginstal public key ke server remote

| Perintah | Penjelasan |
|---|---|
| `ssh-copy-id user@hostname` | Copy key default ke server |
| `ssh-copy-id -i ~/.ssh/id_ed25519.pub user@hostname` | Key tertentu |
| `ssh-copy-id -p 2222 user@hostname` | Port SSH tertentu |
| `ssh-copy-id -o "StrictHostKeyChecking=no" user@hostname` | Skip host check |
| `ssh-copy-id -n user@hostname` | Dry run (tidak benar-benar copy) |

**Manual (tanpa ssh-copy-id):**
```bash
# Cara 1: Pipe
cat ~/.ssh/id_ed25519.pub | ssh user@hostname "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"

# Cara 2: Redirect
ssh user@hostname "cat >> ~/.ssh/authorized_keys" < ~/.ssh/id_ed25519.pub

# Cara 3: scp + ssh
scp ~/.ssh/id_ed25519.pub user@hostname:/tmp/
ssh user@hostname "cat /tmp/id_ed25519.pub >> ~/.ssh/authorized_keys && rm /tmp/id_ed25519.pub"
```

---

### `ssh-agent` - SSH Authentication Agent
```bash
# Mulai ssh-agent
eval $(ssh-agent)
eval $(ssh-agent -s)    # Format sh-compatible

# Tambah key ke agent
ssh-add                              # Tambah key default
ssh-add ~/.ssh/id_ed25519            # Tambah key tertentu
ssh-add ~/.ssh/id_rsa ~/.ssh/id_ed25519  # Beberapa key
ssh-add -t 3600 ~/.ssh/id_ed25519   # Timeout 1 jam
ssh-add -c ~/.ssh/id_ed25519        # Konfirmasi setiap penggunaan

# Lihat key yang dimuat
ssh-add -l                           # List dengan fingerprint
ssh-add -L                           # List dengan public key

# Hapus key dari agent
ssh-add -d ~/.ssh/id_ed25519        # Hapus key tertentu
ssh-add -D                           # Hapus semua key

# Stop agent
ssh-agent -k                         # Kill agent saat ini
kill $SSH_AGENT_PID

# Auto-start agent di ~/.bashrc
cat >> ~/.bashrc << 'EOF'
# SSH Agent auto-start
if [ -z "$SSH_AUTH_SOCK" ]; then
    eval $(ssh-agent -s)
    ssh-add ~/.ssh/id_ed25519 2>/dev/null
fi
EOF
```

---

## 11.3 Konfigurasi SSH Server

### `/etc/ssh/sshd_config` - Server Configuration

```bash
# Lihat konfigurasi SSH server
cat /etc/ssh/sshd_config
sudo nano /etc/ssh/sshd_config

# Konfigurasi penting:
cat > /etc/ssh/sshd_config << 'EOF'
# === PORT & NETWORK ===
Port 22                          # Port SSH (ubah untuk keamanan)
# Port 2222                      # Port alternatif
AddressFamily inet               # inet=IPv4, inet6=IPv6, any=keduanya
ListenAddress 0.0.0.0            # Listen di semua interface
# ListenAddress 192.168.1.100    # Listen di IP tertentu

# === AUTENTIKASI ===
PermitRootLogin no               # Larang login root (PENTING!)
# PermitRootLogin prohibit-password  # Root hanya dengan key
PasswordAuthentication no        # Nonaktifkan password auth
PubkeyAuthentication yes         # Aktifkan key auth
AuthorizedKeysFile .ssh/authorized_keys
UsePAM yes

# Batasi user/grup yang bisa login
AllowUsers user1 user2           # Hanya user ini yang bisa login
# AllowGroups ssh-users          # Hanya grup ini
# DenyUsers root                 # Larang user ini
# DenyGroups blocked-users       # Larang grup ini

# === KEAMANAN ===
MaxAuthTries 3                   # Maksimum percobaan auth
MaxSessions 10                   # Maksimum sesi per koneksi
LoginGraceTime 30                # Waktu untuk login (detik)
StrictModes yes                  # Cek mode file yang ketat
HostbasedAuthentication no       # Nonaktifkan host-based auth
IgnoreRhosts yes                 # Abaikan .rhosts
PermitEmptyPasswords no          # Larang password kosong

# === FITUR ===
X11Forwarding no                 # Nonaktifkan X11 forwarding
AllowTcpForwarding yes           # Izinkan TCP forwarding
GatewayPorts no                  # Jangan expose forwarded ports
PermitTunnel no                  # Nonaktifkan tunnel device
AllowAgentForwarding yes         # Izinkan agent forwarding

# === KRYPTOGRAFI ===
# Algoritma yang kuat
KexAlgorithms curve25519-sha256,diffie-hellman-group16-sha512
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com
HostKeyAlgorithms ssh-ed25519,rsa-sha2-512

# === KEEP-ALIVE ===
ClientAliveInterval 300          # Ping client setiap 5 menit
ClientAliveCountMax 2            # Putus setelah 2 kali tidak respons
TCPKeepAlive yes                 # Aktifkan TCP keepalive

# === LOGGING ===
SyslogFacility AUTH
LogLevel INFO                    # QUIET,FATAL,ERROR,INFO,VERBOSE,DEBUG

# === BANNER ===
Banner /etc/ssh/banner.txt       # Tampilkan banner saat login

# === SFTP ===
Subsystem sftp /usr/lib/openssh/sftp-server

# SFTP chroot untuk user tertentu
Match User sftpuser
    ChrootDirectory /var/sftp/%u
    ForceCommand internal-sftp
    AllowTcpForwarding no
    X11Forwarding no
EOF

# Test konfigurasi
sudo sshd -t                     # Test syntax
sudo sshd -T                     # Tampilkan konfigurasi efektif

# Restart SSH server
sudo systemctl restart sshd
sudo systemctl reload sshd       # Reload konfigurasi tanpa restart

# Status SSH server
sudo systemctl status sshd
```

---

### `sshd` - SSH Daemon Commands
```bash
sudo sshd -t                     # Test konfigurasi
sudo sshd -T                     # Dump konfigurasi efektif
sudo sshd -T | grep PermitRoot   # Cek nilai tertentu
sudo sshd -d                     # Debug mode (foreground)
sudo sshd -f /path/sshd_config   # Gunakan file config tertentu

# Monitor koneksi SSH
sudo journalctl -u sshd -f       # Follow log SSH
sudo journalctl -u sshd --since today  # Log hari ini
sudo journalctl -u sshd -p err   # Hanya error
sudo tail -f /var/log/auth.log   # Log autentikasi (Debian/Ubuntu)
sudo tail -f /var/log/secure     # Log auth (RHEL/CentOS)

# Cek siapa yang terkoneksi
who                              # User yang login
w                                # User login + aktivitas
last                             # Riwayat login
lastb                            # Login yang gagal
sudo ss -tnp | grep :22          # Koneksi SSH aktif
```

---

## 11.4 SCP - Secure Copy

### `scp` - Secure File Copy
```bash
scp [opsi] <sumber> <tujuan>
```
> Menyalin file melalui SSH secara aman

**Upload ke Server:**
| Perintah | Penjelasan |
|---|---|
| `scp file.txt user@host:/path/` | Upload file |
| `scp file.txt user@host:~/` | Upload ke home directory |
| `scp file.txt user@host:~/baru.txt` | Upload dengan nama baru |
| `scp file1.txt file2.txt user@host:/path/` | Upload beberapa file |
| `scp *.txt user@host:/path/` | Upload semua .txt |
| `scp -r direktori/ user@host:/path/` | Upload direktori |
| `scp -P 2222 file.txt user@host:/path/` | Port SSH tertentu |
| `scp -i ~/.ssh/key.pem file.txt user@host:/path/` | Key tertentu |
| `scp -C file.txt user@host:/path/` | Kompresi |
| `scp -l 1000 file.txt user@host:/path/` | Batasi bandwidth (Kbps) |
| `scp -p file.txt user@host:/path/` | Pertahankan timestamp & mode |
| `scp -v file.txt user@host:/path/` | Verbose |
| `scp -q file.txt user@host:/path/` | Quiet |

**Download dari Server:**
| Perintah | Penjelasan |
|---|---|
| `scp user@host:/path/file.txt .` | Download file |
| `scp user@host:/path/file.txt ./lokal.txt` | Download dengan nama baru |
| `scp user@host:~/file.txt /lokal/path/` | Download dari home |
| `scp -r user@host:/path/direktori/ .` | Download direktori |
| `scp user@host:"/path/dengan spasi/file.txt" .` | Path dengan spasi |
| `scp user@host:"/path/*.txt" /lokal/` | Download dengan wildcard |

**Remote ke Remote:**
```bash
scp user1@host1:/path/file.txt user2@host2:/path/
```

**Dengan SSH Config:**
```bash
scp file.txt prod:/var/www/     # Gunakan alias dari ~/.ssh/config
scp dev:/home/user/file.txt .  # Download dari alias 'dev'
```

---

## 11.5 SFTP - Secure FTP

### `sftp` - Secure File Transfer Protocol
```bash
sftp [opsi] [user@]host
```
> Transfer file interaktif melalui SSH

**Koneksi:**
```bash
sftp user@hostname               # Koneksi ke server
sftp -P 2222 user@hostname       # Port tertentu
sftp -i ~/.ssh/key user@hostname # Key tertentu
sftp -C user@hostname            # Dengan kompresi
sftp user@hostname:/remote/path  # Langsung ke path tertentu
```

**Perintah dalam SFTP:**

| Perintah | Penjelasan |
|---|---|
| `ls` | Daftar file di remote |
| `ls -la` | Detail listing remote |
| `lls` | Daftar file di lokal |
| `pwd` | Direktori remote saat ini |
| `lpwd` | Direktori lokal saat ini |
| `cd /remote/path` | Ganti direktori remote |
| `lcd /lokal/path` | Ganti direktori lokal |
| `mkdir direktori` | Buat direktori di remote |
| `lmkdir direktori` | Buat direktori di lokal |
| `rmdir direktori` | Hapus direktori remote |
| `rm file.txt` | Hapus file remote |
| `rename lama baru` | Rename file remote |
| `chmod 755 file` | Ubah permission remote |
| `chown 1000 file` | Ubah owner remote |
| `get file.txt` | Download file |
| `get file.txt lokal.txt` | Download dengan nama baru |
| `get -r direktori/` | Download direktori rekursif |
| `get -a file.txt` | Download mode ASCII |
| `mget *.txt` | Download beberapa file |
| `put file.txt` | Upload file |
| `put file.txt remote.txt` | Upload dengan nama baru |
| `put -r direktori/` | Upload direktori rekursif |
| `mput *.txt` | Upload beberapa file |
| `df` | Penggunaan disk remote |
| `df -h` | Human-readable |
| `version` | Versi SFTP |
| `help` | Bantuan |
| `bye` / `quit` / `exit` | Keluar dari SFTP |
| `!perintah` | Jalankan perintah lokal |
| `!` | Buka shell lokal |
| `progress` | Toggle progress bar |
| `reget file` | Lanjutkan download |
| `reput file` | Lanjutkan upload |

**SFTP Non-interaktif (Batch Mode):**
```bash
# Upload file
sftp user@host <<< "put file.txt /remote/path/"

# Download file
sftp user@host <<< "get /remote/file.txt /lokal/"

# Multiple operasi
sftp user@host << 'EOF'
cd /var/www/html
put index.html
put style.css
mkdir backup
get backup/old.html
bye
EOF

# Dari file batch
cat > sftp_commands.txt << EOF
cd /var/www
put *.html
quit
EOF
sftp -b sftp_commands.txt user@host
```

---

## 11.6 RSYNC via SSH

### `rsync` over SSH
```bash
# Sinkronisasi ke remote
rsync -avz sumber/ user@host:/tujuan/

# Sinkronisasi dari remote
rsync -avz user@host:/sumber/ tujuan/

# Dengan port SSH tertentu
rsync -avz -e "ssh -p 2222" sumber/ user@host:/tujuan/

# Dengan key tertentu
rsync -avz -e "ssh -i ~/.ssh/key" sumber/ user@host:/tujuan/

# Delete file yang tidak ada di sumber
rsync -avz --delete sumber/ user@host:/tujuan/

# Dry run
rsync -avzn sumber/ user@host:/tujuan/

# Dengan progress
rsync -avzP sumber/ user@host:/tujuan/

# Kecualikan file
rsync -avz --exclude="*.log" --exclude=".git" sumber/ user@host:/tujuan/

# Batasi bandwidth (100KB/s)
rsync -avz --bwlimit=100 sumber/ user@host:/tujuan/

# Resume transfer yang terputus
rsync -avzP --partial sumber/ user@host:/tujuan/

# Backup dengan timestamp
rsync -avz --backup --backup-dir=/backup/$(date +%Y%m%d) \
    sumber/ user@host:/tujuan/

# Deploy website
rsync -avz --delete \
    --exclude=".git" \
    --exclude="node_modules" \
    --exclude=".env" \
    ./dist/ user@prod:/var/www/html/
```

---

## 11.7 Remote Execution

### Menjalankan Perintah di Multiple Server

```bash
# Loop sederhana
for server in server1 server2 server3; do
    echo "=== $server ==="
    ssh user@$server "uptime && df -h"
done

# Paralel dengan &
for server in server1 server2 server3; do
    ssh user@$server "hostname && uptime" &
done
wait

# Dengan file daftar server
cat servers.txt | xargs -I {} ssh user@{} "uptime"

# Dengan timeout
for server in $(cat servers.txt); do
    timeout 5 ssh -o "ConnectTimeout=3" user@$server "uptime" 2>&1 || \
        echo "$server: UNREACHABLE"
done

# Menggunakan GNU Parallel
parallel -j10 ssh user@{} "uptime" ::: server1 server2 server3
parallel -j10 ssh user@{} "df -h" :::: servers.txt
```

---

### `pssh` - Parallel SSH
```bash
# Install
sudo apt install pssh

# Jalankan perintah di banyak server
pssh -H "server1 server2 server3" -l user -P "uptime"
pssh -h servers.txt -l user -P "df -h"
pssh -h servers.txt -l user -i "hostname && uptime"

# Opsi pssh
-H hosts      # Daftar host
-h file       # File daftar host
-l user       # Username
-p num        # Parallelism (default: 32)
-t timeout    # Timeout
-o dir        # Simpan output ke direktori
-e dir        # Simpan stderr ke direktori
-P            # Print output saat berjalan
-i            # Interactive (tampilkan output langsung)
-A            # Minta password
-x opts       # Extra SSH opsi
-O opts       # SSH option
```

---

### `pdsh` - Parallel Distributed Shell
```bash
# Install
sudo apt install pdsh

# Jalankan di semua server
pdsh -w server1,server2,server3 "uptime"
pdsh -w server[1-10] "df -h"
pdsh -w ^servers.txt "hostname"

# Opsi pdsh
-w hosts      # Daftar host
-R ssh        # Gunakan SSH
-l user       # Username
-t timeout    # Timeout
-f num        # Fan-out (parallelism)
```

---

### `clusterssh` - Cluster SSH
```bash
# Install
sudo apt install clusterssh

# Buka terminal ke banyak server sekaligus
cssh server1 server2 server3
cssh -l user server1 server2
cssh -h servers.txt
```

---

## 11.8 Ansible - Configuration Management

### Perintah Dasar Ansible
```bash
# Install
sudo apt install ansible
pip install ansible

# Konfigurasi inventory
cat > /etc/ansible/hosts << EOF
[web]
web1.example.com
web2.example.com

[db]
db1.example.com ansible_user=dbadmin ansible_port=2222

[all:vars]
ansible_user=admin
ansible_ssh_private_key_file=~/.ssh/id_ed25519
EOF

# Atau inventory file tertentu
cat > inventory.ini << EOF
[production]
prod1 ansible_host=192.168.1.10
prod2 ansible_host=192.168.1.11

[staging]
stage1 ansible_host=192.168.1.20

[all:vars]
ansible_user=admin
ansible_become=yes
EOF
```

**Ansible Ad-hoc Commands:**
```bash
# Ping semua host
ansible all -m ping
ansible all -i inventory.ini -m ping

# Jalankan perintah
ansible all -m command -a "uptime"
ansible web -m command -a "df -h"
ansible all -m shell -a "cat /etc/os-release | head -5"

# Copy file
ansible all -m copy -a "src=file.txt dest=/tmp/file.txt"

# Fetch file dari remote
ansible all -m fetch -a "src=/etc/hostname dest=./hosts/"

# Install paket
ansible all -m apt -a "name=nginx state=present" --become
ansible web -m yum -a "name=httpd state=latest" --become

# Kelola service
ansible all -m service -a "name=nginx state=started" --become
ansible all -m systemd -a "name=nginx state=restarted" --become

# Kelola user
ansible all -m user -a "name=newuser state=present" --become

# Salin file dengan template
ansible all -m template -a "src=nginx.conf.j2 dest=/etc/nginx/nginx.conf"

# Jalankan script
ansible all -m script -a "/local/script.sh"

# Cek fakta sistem
ansible all -m setup
ansible all -m setup -a "filter=ansible_distribution*"
ansible web -m setup -a "filter=ansible_memory_mb"

# Dengan sudo
ansible all -m command -a "id" --become --become-user=root
ansible all -m command -a "uptime" -b -K    # -K = ask for sudo password
```

**Ansible Playbook:**
```yaml
# site.yml
---
- name: Configure web servers
  hosts: web
  become: yes
  vars:
    http_port: 80
    app_dir: /var/www/html

  tasks:
    - name: Update package cache
      apt:
        update_cache: yes
        cache_valid_time: 3600

    - name: Install nginx
      apt:
        name: nginx
        state: present

    - name: Start and enable nginx
      systemd:
        name: nginx
        state: started
        enabled: yes

    - name: Copy website files
      copy:
        src: "{{ item }}"
        dest: "{{ app_dir }}/"
        owner: www-data
        group: www-data
        mode: '0644'
      with_fileglob:
        - files/*.html
        - files/*.css

    - name: Configure nginx
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/sites-available/default
        mode: '0644'
      notify: Reload nginx

  handlers:
    - name: Reload nginx
      systemd:
        name: nginx
        state: reloaded
```

```bash
# Jalankan playbook
ansible-playbook site.yml
ansible-playbook -i inventory.ini site.yml
ansible-playbook site.yml --check           # Dry run
ansible-playbook site.yml --diff            # Tampilkan perbedaan
ansible-playbook site.yml -v               # Verbose
ansible-playbook site.yml -vvv             # Sangat verbose
ansible-playbook site.yml --tags "install" # Hanya task dengan tag
ansible-playbook site.yml --skip-tags "test"  # Skip tag tertentu
ansible-playbook site.yml -l "web1"        # Hanya host tertentu
ansible-playbook site.yml -e "var=value"   # Extra variabel
ansible-playbook site.yml --ask-become-pass # Minta sudo password
ansible-playbook site.yml --vault-ask-pass  # Minta vault password

# Ansible Vault (enkripsi)
ansible-vault create secrets.yml           # Buat file terenkripsi
ansible-vault edit secrets.yml             # Edit file
ansible-vault view secrets.yml             # Lihat file
ansible-vault encrypt file.yml             # Enkripsi file
ansible-vault decrypt file.yml             # Dekripsi file
ansible-vault rekey file.yml               # Ubah password
ansible-playbook site.yml --ask-vault-pass # Jalankan dengan vault

# Ansible Galaxy (role management)
ansible-galaxy install geerlingguy.nginx
ansible-galaxy install -r requirements.yml
ansible-galaxy list
ansible-galaxy init my_role               # Buat role baru
ansible-galaxy search nginx
```

---

## 11.9 Remote Tools Lainnya

### `tmux` - Terminal Multiplexer
```bash
# Install
sudo apt install tmux

# Mulai sesi baru
tmux                             # Sesi tanpa nama
tmux new -s nama_sesi            # Sesi dengan nama
tmux new-session -s "work"       # Sesi bernama

# Operasi sesi
tmux ls                          # Daftar sesi
tmux list-sessions               # Sama
tmux attach -t nama_sesi         # Attach ke sesi
tmux a -t nama_sesi              # Shorthand
tmux a                           # Attach ke sesi terakhir
tmux detach                      # Detach dari sesi
tmux kill-session -t nama_sesi   # Hapus sesi
tmux kill-server                 # Hapus semua sesi
tmux rename-session -t lama baru # Rename sesi

# Prefix key (default: Ctrl+B)
# Semua shortcut dimulai dengan prefix

# Shortcut penting:
Ctrl+B d         # Detach dari sesi
Ctrl+B s         # Daftar sesi
Ctrl+B $         # Rename sesi saat ini
Ctrl+B (         # Pindah ke sesi sebelumnya
Ctrl+B )         # Pindah ke sesi berikutnya

# Window management
Ctrl+B c         # Buat window baru
Ctrl+B ,         # Rename window
Ctrl+B w         # Daftar window
Ctrl+B n         # Window berikutnya
Ctrl+B p         # Window sebelumnya
Ctrl+B 0-9       # Pindah ke window ke-n
Ctrl+B &         # Tutup window

# Pane management
Ctrl+B %         # Split vertikal
Ctrl+B "         # Split horizontal
Ctrl+B arrow     # Pindah antar pane
Ctrl+B o         # Pindah ke pane berikutnya
Ctrl+B q         # Tampilkan nomor pane
Ctrl+B q 0-9     # Pindah ke pane tertentu
Ctrl+B z         # Zoom/unzoom pane
Ctrl+B x         # Tutup pane
Ctrl+B {         # Pindah pane ke kiri
Ctrl+B }         # Pindah pane ke kanan
Ctrl+B Space     # Ganti layout

# Copy mode
Ctrl+B [         # Masuk copy mode (scroll)
q                # Keluar copy mode
Space            # Mulai seleksi
Enter            # Copy seleksi
Ctrl+B ]         # Paste

# Konfigurasi ~/.tmux.conf
cat > ~/.tmux.conf << 'EOF'
# Ubah prefix ke Ctrl+A
set -g prefix C-a
unbind C-b
bind C-a send-prefix

# Reload config
bind r source-file ~/.tmux.conf \; display "Config reloaded!"

# Mulai dari 1
set -g base-index 1
setw -g pane-base-index 1

# Mouse support
set -g mouse on

# Split dengan | dan -
bind | split-window -h
bind - split-window -v

# Pindah pane dengan vim keys
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# Status bar
set -g status-bg colour235
set -g status-fg white
set -g status-left '[#S] '
set -g status-right '%Y-%m-%d %H:%M'

# History
set -g history-limit 50000

# 256 colors
set -g default-terminal "screen-256color"
EOF

# Reload konfigurasi
tmux source-file ~/.tmux.conf

# Script tmux untuk setup lingkungan
cat > ~/setup.sh << 'EOF'
#!/bin/bash
SESSION="mywork"

tmux new-session -d -s $SESSION -n "editor"
tmux send-keys -t $SESSION "vim ." Enter

tmux new-window -t $SESSION -n "server"
tmux send-keys -t $SESSION "npm run dev" Enter

tmux new-window -t $SESSION -n "shell"
tmux split-window -h -t $SESSION
tmux send-keys -t $SESSION "htop" Enter

tmux attach -t $SESSION
EOF
```

---

### `screen` - Terminal Multiplexer (Lama)
```bash
# Install
sudo apt install screen

# Dasar
screen                           # Mulai sesi screen
screen -S nama_sesi              # Sesi dengan nama
screen -ls                       # Daftar sesi
screen -r                        # Reattach ke sesi terakhir
screen -r nama_sesi              # Reattach ke sesi tertentu
screen -d -r nama_sesi           # Detach dari sesi lain lalu attach
screen -X -S nama quit           # Hapus sesi

# Shortcut (prefix: Ctrl+A)
Ctrl+A d         # Detach
Ctrl+A c         # Buat window baru
Ctrl+A n         # Window berikutnya
Ctrl+A p         # Window sebelumnya
Ctrl+A "         # Daftar window
Ctrl+A k         # Tutup window
Ctrl+A |         # Split vertikal
Ctrl+A S         # Split horizontal
Ctrl+A Tab       # Pindah antar split
Ctrl+A Q         # Tutup semua split kecuali yang aktif
Ctrl+A [         # Copy mode (scroll)
Ctrl+A ]         # Paste
Ctrl+A ?         # Bantuan
```

---

### `mosh` - Mobile Shell
```bash
# Install
sudo apt install mosh

# Koneksi (lebih tahan terhadap koneksi yang tidak stabil)
mosh user@hostname               # Koneksi mosh
mosh --port=60001 user@hostname  # Port UDP tertentu
mosh --ssh="ssh -p 2222" user@hostname  # Port SSH tertentu
mosh --predict=always user@hostname     # Prediksi input

# Shortcut
Ctrl+^ .         # Disconnect dari mosh
Ctrl+^ ^         # Kirim Ctrl+^
```

---

### `rdesktop` & `xfreerdp` - Remote Desktop (RDP)
```bash
# rdesktop
sudo apt install rdesktop
rdesktop hostname                # Koneksi ke Windows RDP
rdesktop -u user hostname        # Dengan username
rdesktop -p pass hostname        # Dengan password
rdesktop -g 1920x1080 hostname   # Resolusi tertentu
rdesktop -f hostname             # Fullscreen
rdesktop -r clipboard hostname   # Enable clipboard

# xfreerdp (lebih modern)
sudo apt install freerdp2-x11
xfreerdp /v:hostname             # Koneksi ke server
xfreerdp /v:hostname /u:user /p:pass  # Dengan credentials
xfreerdp /v:hostname /size:1920x1080  # Resolusi
xfreerdp /v:hostname /f              # Fullscreen
xfreerdp /v:hostname /clipboard      # Enable clipboard
xfreerdp /v:hostname /drive:home,/home/user  # Share drive
xfreerdp /v:hostname /tls-seclevel:0  # Kurangi security level
```

---

### `vnc` - Virtual Network Computing
```bash
# VNC Server
sudo apt install tightvncserver
vncserver                        # Mulai VNC server
vncserver :1                     # Display :1 (port 5901)
vncserver -geometry 1920x1080 :1 # Dengan resolusi
vncserver -kill :1               # Stop VNC server
vncpasswd                        # Set password VNC
cat ~/.vnc/*.log                  # Lihat log

# VNC Client
sudo apt install xtightvncviewer
vncviewer hostname:5901           # Koneksi ke VNC server
vncviewer hostname:1              # Via display number
vncviewer -shared hostname:1      # Shared session

# VNC via SSH tunnel (lebih aman)
ssh -L 5901:localhost:5901 user@hostname -N &
vncviewer localhost:5901
```

---

## 11.10 File Transfer Tools Lainnya

### `ftp` - File Transfer Protocol
```bash
# FTP (tidak aman, hindari jika memungkinkan)
ftp hostname                     # Koneksi ke FTP server
ftp -n hostname                  # Tanpa auto-login

# Perintah dalam FTP:
open hostname    # Koneksi
user username    # Login
pass password    # Password
ls               # Daftar file
cd direktori     # Ganti direktori
lcd direktori    # Ganti direktori lokal
get file.txt     # Download file
mget *.txt       # Download beberapa file
put file.txt     # Upload file
mput *.txt       # Upload beberapa file
binary           # Mode binary
ascii            # Mode ASCII
passive          # Mode passive
bye/quit         # Keluar

# lftp (lebih modern)
sudo apt install lftp
lftp ftp://user:pass@hostname
lftp -u user,pass hostname
lftp -e "mirror /remote /lokal; bye" hostname  # Mirror direktori
lftp -e "mirror -R /lokal /remote; bye" hostname  # Upload mirror
```

---

### `ncftp` - Enhanced FTP Client
```bash
sudo apt install ncftp

ncftp hostname                   # Koneksi
ncftp -u user hostname           # Dengan username
ncftpget hostname /remote/file /lokal/  # Download langsung
ncftpput hostname /remote/ /lokal/file  # Upload langsung
ncftpput -r hostname /remote/ /lokal/dir/  # Upload rekursif
```

---

### `axel` - Download Accelerator
```bash
sudo apt install axel

axel https://url/file.zip        # Download dengan percepatan
axel -n 8 https://url/file.zip   # 8 koneksi paralel
axel -o /path/file https://url/  # Nama file output
axel -a https://url/file         # Alternative progress bar
axel -q https://url/file         # Quiet mode
axel --max-speed=1M https://url/ # Batasi kecepatan
```

---

### `aria2` - Download Manager
```bash
sudo apt install aria2

# Download dasar
aria2c https://url/file.zip      # Download file
aria2c -x 8 https://url/file    # 8 koneksi per server
aria2c -s 8 https://url/file    # 8 stream
aria2c -j 5 https://url/        # 5 download paralel
aria2c -o output.file https://url/  # Nama output

# BitTorrent
aria2c file.torrent              # Download torrent
aria2c --seed-time=0 file.torrent  # Tanpa seeding

# Metalink
aria2c file.metalink             # Download via metalink

# Dari file daftar URL
aria2c -i urls.txt               # Download dari file

# Resume download
aria2c -c https://url/file       # Continue download

# FTP
aria2c ftp://user:pass@host/file
```

---

## 11.11 Keamanan SSH

### Hardening SSH Server

```bash
# 1. Ubah port default
sudo sed -i 's/#Port 22/Port 2222/' /etc/ssh/sshd_config

# 2. Nonaktifkan login root
sudo sed -i 's/#PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config

# 3. Nonaktifkan password authentication
sudo sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' \
    /etc/ssh/sshd_config

# 4. Batasi user yang bisa login
echo "AllowUsers admin developer" | sudo tee -a /etc/ssh/sshd_config

# 5. Aktifkan 2FA (Google Authenticator)
sudo apt install libpam-google-authenticator
google-authenticator                  # Setup per user

# Edit /etc/pam.d/sshd
echo "auth required pam_google_authenticator.so" | sudo tee -a /etc/pam.d/sshd

# Edit sshd_config
sudo sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/' \
    /etc/ssh/sshd_config
echo "AuthenticationMethods publickey,keyboard-interactive" | \
    sudo tee -a /etc/ssh/sshd_config

# 6. Fail2ban untuk proteksi brute force
sudo apt install fail2ban

cat > /etc/fail2ban/jail.local << 'EOF'
[sshd]
enabled = true
port = 2222
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 3600
findtime = 600
EOF

sudo systemctl restart fail2ban
sudo fail2ban-client status sshd      # Cek status
sudo fail2ban-client set sshd unbanip 192.168.1.100  # Unban IP

# 7. UFW untuk firewall
sudo ufw allow 2222/tcp               # Izinkan port SSH baru
sudo ufw enable

# 8. Restart SSH
sudo systemctl restart sshd
```

---

### Audit SSH

```bash
# Cek koneksi SSH aktif
sudo ss -tnp | grep :22
sudo ss -tnp | grep :2222

# Monitor login SSH real-time
sudo journalctl -u sshd -f
sudo tail -f /var/log/auth.log

# Cek login gagal
sudo grep "Failed password" /var/log/auth.log | tail -20
sudo grep "Invalid user" /var/log/auth.log | tail -20
sudo grep "Accepted" /var/log/auth.log | tail -20

# Statistik login gagal per IP
sudo grep "Failed password" /var/log/auth.log | \
    awk '{print $(NF-3)}' | sort | uniq -c | sort -rn | head -10

# Cek known_hosts
cat ~/.ssh/known_hosts
ssh-keygen -l -f ~/.ssh/known_hosts    # Fingerprint semua host

# Audit authorized_keys
for user in $(cut -d: -f1 /etc/passwd); do
    home=$(eval echo ~$user)
    auth_keys="$home/.ssh/authorized_keys"
    if [ -f "$auth_keys" ]; then
        echo "=== $user ==="
        cat "$auth_keys"
    fi
done

# Cek permission SSH
ls -la ~/.ssh/
# .ssh harus 700
# authorized_keys harus 600
# private key harus 600
# public key bisa 644
```

---

## Ringkasan Perintah Bagian 11

```
SSH:
  ssh              → Koneksi remote aman
  ssh -L           → Local port forwarding
  ssh -R           → Remote port forwarding
  ssh -D           → Dynamic SOCKS proxy
  ssh -J           → Jump host
  ssh -N -f        → Background tunnel
  ssh-keygen       → Generate key pair
  ssh-copy-id      → Copy public key ke server
  ssh-agent        → SSH authentication agent
  ssh-add          → Tambah key ke agent
  sshd             → SSH server daemon

Transfer File:
  scp              → Secure copy via SSH
  sftp             → Secure FTP via SSH
  rsync            → Sinkronisasi file via SSH
  ftp/lftp         → FTP client
  axel             → Download accelerator
  aria2c           → Download manager

Terminal Multiplexer:
  tmux             → Terminal multiplexer modern
  screen           → Terminal multiplexer klasik
  mosh             → Mobile shell (unstable network)

Remote Desktop:
  xfreerdp         → RDP client (Windows)
  vncviewer        → VNC client

Remote Management:
  ansible          → Configuration management
  pssh             → Parallel SSH
  pdsh             → Parallel distributed shell
  clusterssh       → Cluster SSH (GUI)

Konfigurasi:
  ~/.ssh/config    → SSH client config
  /etc/ssh/sshd_config → SSH server config
  ~/.ssh/authorized_keys → Public keys yang diizinkan
  ~/.ssh/known_hosts → Host yang dikenal

Keamanan:
  fail2ban         → Proteksi brute force
  ufw              → Firewall
  google-auth      → 2FA untuk SSH
```

---

## ✅ Bagian 11 Selesai!

**Lanjut ke Bagian 12: Perintah Monitoring & Log?**
