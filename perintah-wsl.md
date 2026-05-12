# Daftar Lengkap Perintah WSL (Windows Subsystem for Linux)

## 1. Perintah Dasar WSL

### Manajemen Distribusi

| Perintah | Penjelasan |
|---|---|
| `wsl` | Menjalankan distribusi Linux default |
| `wsl --install` | Menginstal WSL beserta distribusi default (Ubuntu) |
| `wsl --install -d <distro>` | Menginstal distribusi Linux tertentu |
| `wsl --list` atau `wsl -l` | Menampilkan semua distribusi yang terinstal |
| `wsl --list --verbose` atau `wsl -l -v` | Menampilkan distribusi beserta versi WSL dan status |
| `wsl --list --online` atau `wsl -l -o` | Menampilkan distribusi yang tersedia untuk diinstal |
| `wsl --list --running` | Menampilkan distribusi yang sedang berjalan |
| `wsl --list --quiet` atau `wsl -l -q` | Menampilkan hanya nama distribusi tanpa info tambahan |
| `wsl --set-default <distro>` atau `wsl -s <distro>` | Mengatur distribusi default |
| `wsl --set-default-version <versi>` | Mengatur versi WSL default (1 atau 2) untuk distribusi baru |
| `wsl --set-version <distro> <versi>` | Mengubah versi WSL distribusi tertentu (1 ke 2 atau sebaliknya) |
| `wsl --unregister <distro>` | Menghapus distribusi dan semua datanya |
| `wsl -d <distro>` | Menjalankan distribusi tertentu |

---

### Menjalankan Perintah

| Perintah | Penjelasan |
|---|---|
| `wsl <command>` | Menjalankan perintah Linux dari Windows |
| `wsl -d <distro> <command>` | Menjalankan perintah di distribusi tertentu |
| `wsl -u <username>` | Menjalankan WSL sebagai pengguna tertentu |
| `wsl -u root` | Menjalankan WSL sebagai root |
| `wsl -e <command>` atau `wsl --exec <command>` | Menjalankan perintah tanpa menggunakan shell default |
| `wsl --shell-type standard` | Menjalankan WSL dengan shell standar |
| `wsl --shell-type login` | Menjalankan WSL dengan login shell |
| `wsl --shell-type none` | Menjalankan WSL tanpa shell (langsung eksekusi) |
| `wsl --cd <path>` | Menjalankan WSL di direktori tertentu |
| `wsl -- <command>` | Meneruskan semua argumen setelah `--` sebagai perintah |

---

### Manajemen Siklus Hidup (Lifecycle)

| Perintah | Penjelasan |
|---|---|
| `wsl --shutdown` | Menghentikan semua distribusi dan VM WSL 2 |
| `wsl --terminate <distro>` atau `wsl -t <distro>` | Menghentikan distribusi tertentu |
| `wsl --status` | Menampilkan status WSL (versi kernel, default distro, dll.) |

---

### Update & Sistem

| Perintah | Penjelasan |
|---|---|
| `wsl --update` | Memperbarui kernel WSL ke versi terbaru |
| `wsl --update --rollback` | Mengembalikan kernel WSL ke versi sebelumnya |
| `wsl --version` | Menampilkan informasi versi WSL, kernel, WSLg, dll. |
| `wsl --help` | Menampilkan bantuan perintah WSL |

---

### Ekspor & Impor

| Perintah | Penjelasan |
|---|---|
| `wsl --export <distro> <filename.tar>` | Mengekspor distribusi ke file tar (backup) |
| `wsl --export <distro> <filename.vhdx> --vhd` | Mengekspor distribusi sebagai file VHD |
| `wsl --import <distro> <install_path> <filename.tar>` | Mengimpor distribusi dari file tar |
| `wsl --import <distro> <install_path> <filename.tar> --version <1\|2>` | Mengimpor dengan versi WSL tertentu |
| `wsl --import <distro> <install_path> <filename.vhdx> --vhd` | Mengimpor dari file VHD |
| `wsl --import-in-place <distro> <filename.vhdx>` | Mengimpor VHD di lokasi aslinya tanpa menyalin |

---

### Manajemen Disk & Mount

| Perintah | Penjelasan |
|---|---|
| `wsl --mount <disk>` | Mount disk fisik ke WSL |
| `wsl --mount <disk> --partition <nomor>` | Mount partisi tertentu dari disk |
| `wsl --mount <disk> --type <filesystem>` | Mount disk dengan tipe filesystem tertentu (ext4, fat, dll.) |
| `wsl --mount <disk> --bare` | Menghubungkan disk ke WSL tanpa mount otomatis |
| `wsl --mount <disk> --name <name>` | Mount disk dengan nama mount point kustom |
| `wsl --mount <disk> --options <options>` | Mount disk dengan opsi mount tertentu |
| `wsl --unmount <disk>` | Unmount disk dari WSL |
| `wsl --unmount` | Unmount semua disk yang dimount melalui `--mount` |

---

## 2. Konfigurasi File `.wslconfig` (Global - Windows Side)

> **Lokasi:** `C:\Users\<username>\.wslconfig`
> **Berlaku untuk:** Semua distribusi WSL 2

```ini
[wsl2]
# Jumlah prosesor yang dialokasikan
processors=4

# Ukuran memori maksimum
memory=8GB

# Ukuran swap
swap=4GB

# Lokasi file swap
swapFile=C:\\temp\\wsl-swap.vhdx

# Mengaktifkan/menonaktifkan nested virtualization
nestedVirtualization=true

# Menonaktifkan page reporting (memori tidak dikembalikan ke Windows)
pageReporting=false

# Mengaktifkan/menonaktifkan kernel kustom
kernel=C:\\Users\\username\\custom-kernel

# Menambahkan parameter boot kernel
kernelCommandLine=vsyscall=emulate

# Mengaktifkan/menonaktifkan debug console
debugConsole=false

# Waktu idle sebelum VM dihentikan (ms)
vmIdleTimeout=60000

# Mengaktifkan/menonaktifkan GUI (WSLg)
guiApplications=true

# DNS tunneling
dnsTunneling=true

# Mengaktifkan firewall
firewall=true

# Menggunakan mode jaringan mirrored
networkingMode=mirrored

# Mengaktifkan auto proxy
autoProxy=true

# Mengaktifkan sparse VHD (otomatis mengecilkan ukuran disk)
sparseVhd=true

[experimental]
# Fitur eksperimental
autoMemoryReclaim=gradual  # atau dropcache, disabled
bestEffortDnsParsing=true
useWindowsDnsCache=true
```

---

## 3. Konfigurasi File `wsl.conf` (Per-Distribusi - Linux Side)

> **Lokasi:** `/etc/wsl.conf` di dalam distribusi
> **Berlaku untuk:** Distribusi tempat file berada

```ini
[automount]
# Mengaktifkan auto-mount drive Windows
enabled=true

# Direktori root tempat drive Windows dimount
root=/mnt/

# Opsi mount default
options="metadata,uid=1000,gid=1000,umask=022,fmask=11"

# Memproses /etc/fstab saat startup
mountFsTab=true

[network]
# Membuat /etc/resolv.conf otomatis
generateResolvConf=true

# Membuat /etc/hosts otomatis
generateHosts=true

# Mengatur hostname distribusi
hostname=my-wsl

[interop]
# Mengaktifkan interop (menjalankan program Windows dari Linux)
enabled=true

# Menambahkan path Windows ke $PATH Linux
appendWindowsPath=true

[user]
# Mengatur user default saat login
default=username

[boot]
# Mengaktifkan systemd
systemd=true

# Perintah yang dijalankan saat distribusi dimulai
command="service docker start"
```

---

## 4. Perintah Interop (Interaksi Windows ↔ Linux)

### Dari WSL (Linux) ke Windows

| Perintah | Penjelasan |
|---|---|
| `explorer.exe .` | Membuka File Explorer di direktori saat ini |
| `notepad.exe <file>` | Membuka file dengan Notepad Windows |
| `cmd.exe /c <command>` | Menjalankan perintah CMD dari WSL |
| `powershell.exe -c "<command>"` | Menjalankan perintah PowerShell dari WSL |
| `clip.exe` | Menyalin output ke clipboard Windows (pipe) |
| `/mnt/c/` | Mengakses drive C: Windows dari WSL |
| `wslview <url>` | Membuka URL di browser default Windows (perlu wslu) |
| `wslpath -w <linux_path>` | Mengonversi path Linux ke format Windows |
| `wslpath -u <windows_path>` | Mengonversi path Windows ke format Linux |
| `wslpath -a <path>` | Mengonversi ke path absolut |
| `wslpath -m <linux_path>` | Mengonversi ke format Windows dengan forward slash |

### Dari Windows (CMD/PowerShell) ke WSL

| Perintah | Penjelasan |
|---|---|
| `wsl ls -la` | Menjalankan `ls -la` di distribusi default |
| `wsl -d Ubuntu -- ls -la /home` | Menjalankan perintah di distribusi Ubuntu |
| `\\wsl$\<distro>\` | Mengakses filesystem WSL dari File Explorer |
| `\\wsl.localhost\<distro>\` | Alternatif mengakses filesystem WSL |

---

## 5. Perintah Terkait WSL di PowerShell

| Perintah | Penjelasan |
|---|---|
| `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux` | Mengaktifkan fitur WSL |
| `Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform` | Mengaktifkan Virtual Machine Platform (untuk WSL 2) |
| `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart` | Mengaktifkan WSL via DISM |
| `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart` | Mengaktifkan VM Platform via DISM |
| `Get-AppxPackage *ubuntu*` | Melihat paket WSL yang terinstal |
| `wsl --debug-shell` | Membuka shell debug ke VM WSL 2 (tingkat lanjut) |

---

## 6. Perintah Networking WSL

| Perintah | Penjelasan |
|---|---|
| `wsl hostname -I` | Mendapatkan IP address WSL |
| `cat /etc/resolv.conf` | Melihat DNS server yang digunakan WSL |
| `ip addr show eth0` | Melihat detail interface jaringan di WSL |
| `netsh interface portproxy add v4tov4 listenport=<port> listenaddress=0.0.0.0 connectport=<port> connectaddress=<wsl_ip>` | Port forwarding dari Windows ke WSL (dijalankan di Windows) |
| `netsh interface portproxy show all` | Menampilkan semua port forwarding yang aktif |
| `netsh interface portproxy delete v4tov4 listenport=<port> listenaddress=0.0.0.0` | Menghapus port forwarding |

---

## 7. Tips & Trik

### Mengatur Distribusi Default
```powershell
# Melihat distribusi yang tersedia
wsl -l -v

# Output contoh:
#   NAME            STATE           VERSION
# * Ubuntu-22.04    Running         2
#   Debian          Stopped         2

# Mengubah default
wsl -s Debian
```

### Backup & Restore
```powershell
# Backup
wsl --export Ubuntu-22.04 D:\backup\ubuntu-backup.tar

# Restore
wsl --import Ubuntu-Restore D:\WSL\Ubuntu D:\backup\ubuntu-backup.tar --version 2
```

### Reset Password User
```powershell
# Masuk sebagai root
wsl -u root

# Ubah password di dalam WSL
passwd <username>
```

### Mengecilkan Ukuran Disk VHD
```powershell
# Aktifkan sparse VHD (di .wslconfig)
# Atau manual:
wsl --shutdown
diskpart
# select vdisk file="C:\Users\<user>\AppData\Local\Packages\...\ext4.vhdx"
# compact vdisk
```

### Menjalankan Layanan saat Boot (dengan systemd)
```ini
# /etc/wsl.conf
[boot]
systemd=true
```
```bash
# Kemudian di WSL:
sudo systemctl enable docker
sudo systemctl enable ssh
```

---

## Ringkasan Perintah Paling Sering Digunakan

```
wsl --install              → Instal WSL
wsl                        → Buka terminal Linux
wsl -l -v                  → Lihat semua distro + status
wsl -d <nama>              → Buka distro tertentu
wsl --shutdown              → Matikan semua WSL
wsl -t <nama>              → Matikan distro tertentu
wsl --export / --import    → Backup / Restore
wsl --update               → Update kernel WSL
wsl --set-version          → Ganti versi WSL 1/2
wsl --unregister           → Hapus distro
wsl --status               → Cek status WSL
```

Ini adalah daftar yang mencakup hampir seluruh perintah WSL yang tersedia hingga versi terbaru (WSL 2, dengan dukungan systemd dan WSLg).
