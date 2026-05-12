# Bagian 8: Perintah Sistem & Hardware Info

---

## 8.1 Informasi Sistem Umum

### `uname` - System Information
```bash
uname [opsi]
```
> Menampilkan informasi sistem operasi dan kernel

| Perintah | Penjelasan |
|---|---|
| `uname` | Tampilkan nama OS (Linux) |
| `uname -a` | Semua informasi sistem |
| `uname -r` | Versi kernel |
| `uname -v` | Build version kernel |
| `uname -s` | Nama kernel |
| `uname -n` | Hostname jaringan |
| `uname -m` | Arsitektur mesin (x86_64, aarch64, dll.) |
| `uname -p` | Tipe prosesor |
| `uname -i` | Platform hardware |
| `uname -o` | Nama OS |

**Contoh Output uname -a:**
```
Linux hostname 5.15.0-91-generic #101-Ubuntu SMP Tue Nov 14 13:30:08 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
│      │        │                  │                                           │
│      │        │                  └── Tanggal build                           └── OS
│      │        └── Versi kernel
│      └── Hostname
└── Nama kernel
```

---

### `hostnamectl` - System Info (systemd)
```bash
hostnamectl
```
> Menampilkan informasi hostname dan sistem

**Contoh Output:**
```
   Static hostname: myserver
         Icon name: computer-server
           Chassis: server
        Machine ID: abc123def456
           Boot ID: xyz789uvw012
  Operating System: Ubuntu 22.04.3 LTS
            Kernel: Linux 5.15.0-91-generic
      Architecture: x86-64
   Hardware Vendor: Dell Inc.
    Hardware Model: PowerEdge R720
```

---

### `lsb_release` - Linux Standard Base Info
```bash
lsb_release [opsi]
```
> Menampilkan informasi distribusi Linux

| Perintah | Penjelasan |
|---|---|
| `lsb_release -a` | Semua informasi distro |
| `lsb_release -i` | Nama distributor |
| `lsb_release -d` | Deskripsi lengkap |
| `lsb_release -r` | Nomor release |
| `lsb_release -c` | Nama codename |
| `lsb_release -s` | Short output (tanpa label) |
| `lsb_release -is` | Nama distributor (pendek) |
| `lsb_release -rs` | Nomor release (pendek) |

**Contoh Output:**
```
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.3 LTS
Release:        22.04
Codename:       jammy
```

---

### `cat /etc/os-release` - OS Release Info
```bash
cat /etc/os-release
cat /etc/*-release        # Semua file release
cat /etc/issue            # Pesan login
cat /etc/issue.net        # Pesan login remote
```

**Contoh Output:**
```
NAME="Ubuntu"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 22.04.3 LTS"
VERSION_ID="22.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
VERSION_CODENAME=jammy
UBUNTU_CODENAME=jammy
```

---

### `uptime` - System Uptime
```bash
uptime [opsi]
```
> Menampilkan waktu sistem berjalan, jumlah user, dan load average

| Perintah | Penjelasan |
|---|---|
| `uptime` | Tampilkan semua info |
| `uptime -p` | Format pretty (mudah dibaca) |
| `uptime -s` | Waktu sistem mulai |
| `uptime -V` | Versi uptime |

**Contoh Output:**
```
 10:30:00 up 5 days, 12:45,  3 users,  load average: 0.15, 0.20, 0.18
```

---

### `date` - Display/Set Date and Time
```bash
date [opsi] [format]
```
> Menampilkan atau mengatur tanggal dan waktu sistem

| Perintah | Penjelasan |
|---|---|
| `date` | Tampilkan tanggal & waktu saat ini |
| `date -u` | Tampilkan dalam UTC |
| `date +"%Y-%m-%d"` | Format: 2024-01-15 |
| `date +"%H:%M:%S"` | Format: 10:30:00 |
| `date +"%Y-%m-%d %H:%M:%S"` | Format: 2024-01-15 10:30:00 |
| `date +"%d/%m/%Y"` | Format: 15/01/2024 |
| `date +"%A, %B %d %Y"` | Format: Monday, January 15 2024 |
| `date +"%s"` | Unix timestamp (detik sejak epoch) |
| `date +"%s%N"` | Nanosecond timestamp |
| `date -d "2024-01-15"` | Parse tanggal tertentu |
| `date -d "yesterday"` | Kemarin |
| `date -d "tomorrow"` | Besok |
| `date -d "next monday"` | Senin depan |
| `date -d "+7 days"` | 7 hari dari sekarang |
| `date -d "-30 days"` | 30 hari yang lalu |
| `date -d "@1705300200"` | Dari Unix timestamp |
| `sudo date -s "2024-01-15 10:30:00"` | Set tanggal & waktu |
| `date --iso-8601` | Format ISO 8601 |
| `date --rfc-3339=seconds` | Format RFC 3339 |
| `date -R` | Format RFC 2822 (email) |

**Format Specifier date:**
```
%Y → Tahun (2024)
%m → Bulan (01-12)
%d → Hari (01-31)
%H → Jam (00-23)
%M → Menit (00-59)
%S → Detik (00-59)
%A → Nama hari (Monday)
%a → Nama hari pendek (Mon)
%B → Nama bulan (January)
%b → Nama bulan pendek (Jan)
%s → Unix timestamp
%N → Nanosecond
%Z → Timezone name
%z → Timezone offset (+0700)
%j → Day of year (001-366)
%W → Week number
%u → Day of week (1=Monday)
%w → Day of week (0=Sunday)
```

---

### `timedatectl` - Time and Date Control
```bash
timedatectl [opsi] <perintah>
```
> Manajemen waktu dan tanggal sistem menggunakan systemd

| Perintah | Penjelasan |
|---|---|
| `timedatectl` | Tampilkan status waktu |
| `timedatectl status` | Status lengkap |
| `timedatectl list-timezones` | Daftar semua timezone |
| `timedatectl list-timezones \| grep Asia` | Filter timezone Asia |
| `sudo timedatectl set-timezone Asia/Jakarta` | Set timezone |
| `sudo timedatectl set-timezone UTC` | Set ke UTC |
| `sudo timedatectl set-time "2024-01-15 10:30:00"` | Set waktu manual |
| `sudo timedatectl set-ntp true` | Aktifkan NTP sync |
| `sudo timedatectl set-ntp false` | Nonaktifkan NTP sync |
| `timedatectl show` | Output machine-readable |
| `timedatectl show-timesync` | Status NTP sync |

**Contoh Output:**
```
               Local time: Mon 2024-01-15 10:30:00 WIB
           Universal time: Mon 2024-01-15 03:30:00 UTC
                 RTC time: Mon 2024-01-15 03:30:00
                Time zone: Asia/Jakarta (WIB, +0700)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```

---

### `cal` - Calendar
```bash
cal [opsi] [bulan] [tahun]
```
> Menampilkan kalender

| Perintah | Penjelasan |
|---|---|
| `cal` | Kalender bulan ini |
| `cal 2024` | Kalender tahun 2024 |
| `cal 1 2024` | Kalender Januari 2024 |
| `cal -3` | Bulan lalu, ini, dan depan |
| `cal -y` | Seluruh tahun ini |
| `cal -m` | Mulai dari Senin |
| `cal -j` | Tampilkan Julian day |
| `cal -w` | Tampilkan nomor minggu |
| `ncal` | Format alternatif (vertikal) |
| `ncal -e` | Tanggal Easter |
| `ncal -o` | Tanggal Easter Orthodox |

---

### `clock` & `hwclock` - Hardware Clock
```bash
hwclock [opsi]
```
> Membaca dan mengatur hardware clock (RTC)

| Perintah | Penjelasan |
|---|---|
| `sudo hwclock` | Tampilkan hardware clock |
| `sudo hwclock --show` | Tampilkan dengan detail |
| `sudo hwclock --hctosys` | Sinkronkan hardware clock ke sistem |
| `sudo hwclock --systohc` | Sinkronkan sistem clock ke hardware |
| `sudo hwclock --set --date "2024-01-15 10:30:00"` | Set hardware clock |
| `sudo hwclock --localtime` | Hardware clock dalam waktu lokal |
| `sudo hwclock --utc` | Hardware clock dalam UTC |
| `sudo hwclock -D` | Debug mode |

---

## 8.2 Informasi CPU

### `lscpu` - CPU Information
```bash
lscpu [opsi]
```
> Menampilkan informasi lengkap tentang CPU

| Perintah | Penjelasan |
|---|---|
| `lscpu` | Tampilkan semua info CPU |
| `lscpu -a` | Semua CPU (online + offline) |
| `lscpu -e` | Extended info per CPU |
| `lscpu -p` | Output parseable |
| `lscpu -J` | Output JSON |
| `lscpu -x` | Tampilkan hexadecimal |
| `lscpu --extended=CPU,CORE,SOCKET,CACHE` | Kolom tertentu |

**Contoh Output lscpu:**
```
Architecture:            x86_64
  CPU op-mode(s):        32-bit, 64-bit
  Address sizes:         48 bits physical, 48 bits virtual
  Byte Order:            Little Endian
CPU(s):                  16
  On-line CPU(s) list:   0-15
Vendor ID:               AuthenticAMD
  Model name:            AMD Ryzen 7 5800X 8-Core Processor
    CPU family:          25
    Model:               33
    Thread(s) per core:  2
    Core(s) per socket:  8
    Socket(s):           1
    Stepping:            0
    CPU max MHz:         4700.0000
    CPU min MHz:         550.0000
    BogoMIPS:            6787.81
Caches (sum of all):
  L1d:                   256 KiB (8 instances)
  L1i:                   256 KiB (8 instances)
  L2:                    4 MiB (8 instances)
  L3:                    32 MiB (1 instance)
```

---

### `/proc/cpuinfo` - CPU Details
```bash
cat /proc/cpuinfo                        # Semua info CPU
cat /proc/cpuinfo | grep "model name"   # Model CPU
cat /proc/cpuinfo | grep "processor"    # Jumlah logical CPU
cat /proc/cpuinfo | grep "cpu cores"    # Jumlah core per socket
cat /proc/cpuinfo | grep "siblings"     # Jumlah thread per socket
cat /proc/cpuinfo | grep "MHz"          # Frekuensi CPU
cat /proc/cpuinfo | grep "cache size"   # Ukuran cache
cat /proc/cpuinfo | grep "flags"        # CPU flags/features
grep -c "processor" /proc/cpuinfo       # Hitung jumlah CPU logical
grep "model name" /proc/cpuinfo | uniq  # Model CPU (unik)
```

---

### `mpstat` - CPU Statistics Per Core
```bash
mpstat [opsi] [interval] [count]
```
> Statistik penggunaan CPU per core

| Perintah | Penjelasan |
|---|---|
| `mpstat` | Statistik semua CPU |
| `mpstat -P ALL` | Statistik semua core |
| `mpstat -P ALL 2` | Update setiap 2 detik |
| `mpstat -P 0 2 5` | Core 0, 2 detik, 5 kali |
| `mpstat -u 2` | CPU utilization |
| `mpstat -I ALL 2` | Statistik interrupt |
| `mpstat -I SUM 2` | Ringkasan interrupt |
| `mpstat -A 2` | Semua statistik |
| `mpstat -T 2` | Tampilkan timestamp |
| `mpstat -o JSON 2` | Output JSON |

---

### `cpupower` - CPU Frequency Management
```bash
cpupower [opsi] <perintah>
```
> Manajemen frekuensi dan power CPU

> 📦 Perlu instalasi: `sudo apt install linux-tools-common`

```bash
cpupower frequency-info              # Info frekuensi CPU
cpupower frequency-info -c 0         # Info core tertentu
cpupower frequency-show              # Tampilkan frekuensi
sudo cpupower frequency-set -g performance  # Set governor performance
sudo cpupower frequency-set -g powersave    # Set governor powersave
sudo cpupower frequency-set -f 3GHz         # Set frekuensi tetap
sudo cpupower frequency-set -d 1GHz -u 4GHz # Set range
cpupower idle-info                   # Info CPU idle states
cpupower monitor                     # Monitor CPU
```

**CPU Governor:**
```
performance → Frekuensi selalu maksimum
powersave   → Frekuensi selalu minimum
ondemand    → Naik saat dibutuhkan (default lama)
schedutil   → Berdasarkan scheduler (modern)
conservative → Naik bertahap
userspace   → Diatur manual
```

---

### `stress` & `stress-ng` - CPU/Memory Stress Test
```bash
# stress
sudo apt install stress
stress --cpu 4           # Stress 4 CPU core
stress --cpu 4 --timeout 60  # Selama 60 detik
stress --vm 2 --vm-bytes 1G  # Stress memory 1GB
stress --io 4            # Stress I/O
stress --hdd 2           # Stress disk

# stress-ng (lebih lengkap)
sudo apt install stress-ng
stress-ng --cpu 4 --timeout 60s         # Stress CPU
stress-ng --cpu 4 --cpu-method all -t 30s  # Semua metode
stress-ng --vm 2 --vm-bytes 1G -t 30s  # Stress memory
stress-ng --io 4 -t 30s                 # Stress I/O
stress-ng --all 1 -t 30s               # Semua jenis stress
stress-ng --metrics-brief --cpu 4 -t 30s  # Dengan metrik
```

---

## 8.3 Informasi Memori (RAM)

### `free` - Memory Usage
```bash
free [opsi]
```
> Menampilkan penggunaan RAM dan swap

| Perintah | Penjelasan |
|---|---|
| `free` | Tampilkan info memori dalam KB |
| `free -h` | Human-readable (MB, GB) |
| `free -m` | Dalam MB |
| `free -g` | Dalam GB |
| `free -k` | Dalam KB |
| `free -b` | Dalam bytes |
| `free -t` | Tampilkan total |
| `free -s 2` | Update setiap 2 detik |
| `free -c 5` | Update 5 kali lalu stop |
| `free -s 2 -c 5` | 5 kali update, setiap 2 detik |
| `free -l` | Tampilkan high/low memory |
| `free -w` | Wide output (buff dan cache terpisah) |
| `free -V` | Versi |

**Contoh Output free -h:**
```
               total        used        free      shared  buff/cache   available
Mem:            15Gi        4.5Gi       6.2Gi       512Mi       4.8Gi        10Gi
Swap:          4.0Gi          0B        4.0Gi

Keterangan:
total      → Total RAM
used       → RAM yang digunakan
free       → RAM yang benar-benar bebas
shared     → RAM yang dibagi antar proses
buff/cache → RAM untuk buffer dan cache (bisa dibebaskan)
available  → RAM yang tersedia untuk proses baru (free + buff/cache)
```

---

### `/proc/meminfo` - Detailed Memory Info
```bash
cat /proc/meminfo                        # Semua info memori
grep MemTotal /proc/meminfo              # Total RAM
grep MemFree /proc/meminfo               # RAM bebas
grep MemAvailable /proc/meminfo          # RAM tersedia
grep SwapTotal /proc/meminfo             # Total swap
grep SwapFree /proc/meminfo              # Swap bebas
grep Cached /proc/meminfo                # Cache
grep Buffers /proc/meminfo               # Buffer
grep Dirty /proc/meminfo                 # Data kotor (belum ditulis ke disk)
grep HugePages /proc/meminfo             # Huge pages info
```

**Field Penting /proc/meminfo:**
```
MemTotal     → Total fisik RAM
MemFree      → RAM bebas
MemAvailable → RAM tersedia untuk aplikasi baru
Buffers      → Buffer untuk I/O
Cached       → Page cache
SwapCached   → Data swap yang di-cache di RAM
Active       → Memori yang aktif digunakan
Inactive     → Memori yang bisa di-reclaim
Dirty        → Data yang menunggu ditulis ke disk
Writeback    → Data yang sedang ditulis ke disk
HugePages    → Info huge pages
Slab         → Kernel slab allocator
VmallocTotal → Total virtual memory space
```

---

### `vmstat` - Virtual Memory Statistics
```bash
vmstat [opsi] [interval] [count]
```
> Statistik memori virtual, proses, CPU, dan I/O

```bash
vmstat                # Statistik saat ini
vmstat 2              # Update setiap 2 detik
vmstat 2 10           # 10 kali, setiap 2 detik
vmstat -a 2           # Active/inactive memory
vmstat -s             # Tabel statistik lengkap
vmstat -d             # Statistik disk
vmstat -m             # Statistik slab memory
vmstat -t 2           # Dengan timestamp
vmstat -S M 2         # Output dalam MB
vmstat -w 2           # Wide output
```

---

### `dmidecode` - DMI/SMBIOS Information
```bash
dmidecode [opsi]
```
> Menampilkan informasi hardware dari DMI/SMBIOS table

> Perlu hak root

| Perintah | Penjelasan |
|---|---|
| `sudo dmidecode` | Semua informasi DMI |
| `sudo dmidecode -t memory` | Info modul RAM |
| `sudo dmidecode -t 17` | Info RAM (tipe 17 = Memory Device) |
| `sudo dmidecode -t processor` | Info CPU |
| `sudo dmidecode -t bios` | Info BIOS |
| `sudo dmidecode -t system` | Info sistem |
| `sudo dmidecode -t baseboard` | Info motherboard |
| `sudo dmidecode -t chassis` | Info chassis/casing |
| `sudo dmidecode -t cache` | Info cache CPU |
| `sudo dmidecode -t slot` | Info slot ekspansi |
| `sudo dmidecode -s system-manufacturer` | Nama pabrikan |
| `sudo dmidecode -s system-product-name` | Nama produk |
| `sudo dmidecode -s system-serial-number` | Serial number |
| `sudo dmidecode -s bios-version` | Versi BIOS |
| `sudo dmidecode -s processor-frequency` | Frekuensi CPU |
| `sudo dmidecode -q` | Quiet (tanpa unknown) |

**Contoh Melihat Info RAM:**
```bash
sudo dmidecode -t 17 | grep -E \
  "Memory Device|Size:|Type:|Speed:|Manufacturer:|Part Number:|Locator:"
```

---

## 8.4 Informasi Hardware Lengkap

### `lshw` - List Hardware
```bash
lshw [opsi]
```
> Menampilkan informasi hardware yang sangat lengkap

> 📦 Perlu instalasi: `sudo apt install lshw`

| Perintah | Penjelasan |
|---|---|
| `sudo lshw` | Semua informasi hardware |
| `sudo lshw -short` | Format pendek |
| `sudo lshw -class cpu` | Info CPU |
| `sudo lshw -class memory` | Info memori |
| `sudo lshw -class disk` | Info disk |
| `sudo lshw -class network` | Info jaringan |
| `sudo lshw -class display` | Info GPU |
| `sudo lshw -class storage` | Info storage controller |
| `sudo lshw -class bus` | Info bus (PCI, USB, dll.) |
| `sudo lshw -html > hardware.html` | Output HTML |
| `sudo lshw -json > hardware.json` | Output JSON |
| `sudo lshw -xml > hardware.xml` | Output XML |
| `sudo lshw -businfo` | Tampilkan bus info |
| `sudo lshw -numeric` | Tampilkan ID numerik |
| `sudo lshw -sanitize` | Sembunyikan info sensitif |
| `sudo lshw -disable dmi` | Nonaktifkan sumber tertentu |

---

### `inxi` - System Information Tool
```bash
inxi [opsi]
```
> Tool informasi sistem yang komprehensif dan mudah dibaca

> 📦 Perlu instalasi: `sudo apt install inxi`

| Perintah | Penjelasan |
|---|---|
| `inxi` | Info ringkas |
| `inxi -F` | Info lengkap (Full) |
| `inxi -Fxz` | Full + extra + filter privasi |
| `inxi -C` | Info CPU |
| `inxi -m` | Info memori/RAM |
| `inxi -D` | Info disk |
| `inxi -G` | Info GPU/display |
| `inxi -N` | Info jaringan |
| `inxi -A` | Info audio |
| `inxi -S` | Info sistem |
| `inxi -I` | Info umum |
| `inxi -B` | Info baterai |
| `inxi -P` | Info partisi |
| `inxi -d` | Info perangkat optical |
| `inxi -r` | Info repository |
| `inxi -t` | Info proses (top) |
| `inxi -u` | Info USB |
| `inxi -xx` | Extra detail |
| `inxi -xxx` | Maximum detail |
| `inxi --color 2` | Output berwarna |
| `inxi --output json` | Output JSON |
| `inxi --output xml` | Output XML |

**Contoh Output inxi -Fxz:**
```
System:
  Host: myserver Kernel: 5.15.0-91-generic x86_64 bits: 64
  Desktop: N/A Distro: Ubuntu 22.04.3 LTS
CPU:
  Info: 8-core AMD Ryzen 7 5800X [MT MCP] speed (MHz): avg: 3800
  min/max: 550/4700 volts: 1.2V
Memory:
  RAM: total: 15.56 GiB used: 4.52 GiB (29.0%)
  Array-1: capacity: 128 GiB slots: 4 EC: None
  Device-1: DIMM_A1 size: 16 GiB speed: 3200 MT/s
```

---

### `hwinfo` - Hardware Information
```bash
hwinfo [opsi]
```
> Menampilkan informasi hardware detail

> 📦 Perlu instalasi: `sudo apt install hwinfo`

```bash
hwinfo                        # Semua hardware
hwinfo --short                # Format pendek
hwinfo --cpu                  # Info CPU
hwinfo --memory               # Info memori
hwinfo --disk                 # Info disk
hwinfo --network              # Info jaringan
hwinfo --gfxcard              # Info GPU
hwinfo --sound                # Info sound card
hwinfo --usb                  # Info USB
hwinfo --pci                  # Info PCI
hwinfo --bios                 # Info BIOS
hwinfo --mouse                # Info mouse
hwinfo --keyboard             # Info keyboard
hwinfo --monitor              # Info monitor
hwinfo --printer              # Info printer
hwinfo --bluetooth            # Info Bluetooth
hwinfo --wlan                 # Info WiFi
hwinfo --cdrom                # Info optical drive
hwinfo --partition            # Info partisi
hwinfo --log /tmp/hwinfo.log  # Simpan ke file
```

---

### `lspci` - List PCI Devices
```bash
lspci [opsi]
```
> Menampilkan semua perangkat PCI/PCIe

| Perintah | Penjelasan |
|---|---|
| `lspci` | Tampilkan semua device PCI |
| `lspci -v` | Verbose |
| `lspci -vv` | Sangat verbose |
| `lspci -vvv` | Maksimum verbose |
| `lspci -k` | Tampilkan driver yang digunakan |
| `lspci -n` | Tampilkan ID numerik |
| `lspci -nn` | ID numerik + nama |
| `lspci -s 00:02.0` | Device tertentu (slot) |
| `lspci -d ::0300` | Filter berdasarkan class (VGA) |
| `lspci -d 10de:` | Filter berdasarkan vendor (NVIDIA) |
| `lspci -t` | Tampilkan tree |
| `lspci -tv` | Tree + verbose |
| `lspci -mm` | Format machine-readable |
| `lspci -mmv` | Machine-readable verbose |
| `sudo lspci -xxx` | Hex dump register |
| `sudo update-pciids` | Update database PCI ID |
| `lspci \| grep -i vga` | Cari GPU |
| `lspci \| grep -i ethernet` | Cari network card |
| `lspci \| grep -i audio` | Cari sound card |
| `lspci \| grep -i usb` | Cari USB controller |

---

### `lsusb` - List USB Devices
```bash
lsusb [opsi]
```
> Menampilkan semua perangkat USB

| Perintah | Penjelasan |
|---|---|
| `lsusb` | Tampilkan semua device USB |
| `lsusb -v` | Verbose (info lengkap) |
| `lsusb -t` | Tampilkan tree |
| `lsusb -s 001:002` | Device tertentu (bus:device) |
| `lsusb -d 046d:c534` | Filter vendor:product ID |
| `lsusb -D /dev/bus/usb/001/002` | Detail dari path |
| `lsusb -V` | Versi |
| `lsusb \| grep -i mouse` | Cari mouse |
| `lsusb \| grep -i keyboard` | Cari keyboard |
| `lsusb \| grep -i hub` | Cari USB hub |
| `watch -n 1 lsusb` | Monitor perubahan USB real-time |

---

### `lsscsi` - List SCSI Devices
```bash
lsscsi [opsi]
```
> Menampilkan perangkat SCSI/SATA/SAS

> 📦 Perlu instalasi: `sudo apt install lsscsi`

```bash
lsscsi                 # Daftar device SCSI
lsscsi -v              # Verbose
lsscsi -l              # Info lengkap
lsscsi -t              # Tampilkan transport info
lsscsi -g              # Tampilkan device file generic
lsscsi -d              # Tampilkan ukuran dan tipe
lsscsi -H              # Info host adapter
lsscsi -p              # Tampilkan protection info
```

---

### `lsdev` - List Devices (IRQ, DMA, I/O)
```bash
cat /proc/interrupts     # Info interrupt
cat /proc/ioports        # Info I/O port
cat /proc/dma            # Info DMA channel
```

---

## 8.5 Informasi GPU

### `nvidia-smi` - NVIDIA GPU Info
```bash
nvidia-smi [opsi]
```
> Manajemen dan monitoring GPU NVIDIA

```bash
nvidia-smi                        # Status GPU ringkas
nvidia-smi -l 2                   # Update setiap 2 detik
nvidia-smi -q                     # Info lengkap
nvidia-smi -q -d MEMORY           # Info memori GPU
nvidia-smi -q -d POWER            # Info power
nvidia-smi -q -d TEMPERATURE      # Info suhu
nvidia-smi -q -d UTILIZATION      # Info utilisasi
nvidia-smi --query-gpu=name,memory.total,memory.free,temperature.gpu \
  --format=csv                    # Output CSV custom
nvidia-smi -L                     # List semua GPU
nvidia-smi topo -m                # Topology antar GPU
nvidia-smi nvlink -s              # Status NVLink
nvidia-smi dmon                   # Device monitoring
nvidia-smi pmon                   # Process monitoring
sudo nvidia-smi -pm 1             # Aktifkan persistence mode
sudo nvidia-smi --power-limit=150 # Set power limit (Watt)
sudo nvidia-smi -r                # Reset GPU
nvidia-smi smi --help             # Bantuan
```

---

### `glxinfo` - OpenGL Information
```bash
glxinfo [opsi]
```
> Info OpenGL dan GPU renderer

> 📦 Perlu instalasi: `sudo apt install mesa-utils`

```bash
glxinfo                           # Semua info OpenGL
glxinfo -B                        # Info ringkas
glxinfo | grep "OpenGL version"   # Versi OpenGL
glxinfo | grep "renderer"         # GPU renderer
glxinfo | grep "vendor"           # Vendor GPU
glxinfo | grep "direct"           # Direct rendering
```

---

### `radeontop` - AMD GPU Monitor
```bash
# Monitor GPU AMD
sudo apt install radeontop
radeontop                         # Monitor real-time
radeontop -c 1                    # Interval 1 detik
radeontop -d /dev/dri/card0      # Device tertentu
```

---

### `vainfo` - VA-API Information
```bash
# Info hardware video acceleration
sudo apt install vainfo
vainfo                            # Info VA-API
vainfo --display drm              # Via DRM
```

---

## 8.6 Monitoring Suhu & Sensor

### `sensors` - Hardware Sensors
```bash
sensors [opsi]
```
> Membaca sensor suhu, voltase, dan kecepatan kipas

> 📦 Perlu instalasi: `sudo apt install lm-sensors`

| Perintah | Penjelasan |
|---|---|
| `sensors` | Tampilkan semua sensor |
| `sensors -A` | Tanpa adapter info |
| `sensors -f` | Tampilkan dalam Fahrenheit |
| `sensors -j` | Output JSON |
| `sensors -u` | Raw output |
| `sensors coretemp-isa-0000` | Sensor tertentu |
| `sensors \| grep -i temp` | Hanya suhu |
| `watch -n 1 sensors` | Monitor real-time |

**Setup sensors:**
```bash
# Deteksi sensor secara otomatis
sudo sensors-detect          # Interaktif
sudo sensors-detect --auto   # Non-interaktif

# Load modules
sudo modprobe <module>

# Update konfigurasi
sudo service kmod start
```

**Contoh Output:**
```
coretemp-isa-0000
Adapter: ISA adapter
Package id 0:  +45.0°C  (high = +80.0°C, crit = +100.0°C)
Core 0:        +44.0°C  (high = +80.0°C, crit = +100.0°C)
Core 1:        +45.0°C  (high = +80.0°C, crit = +100.0°C)

it8728-isa-0a00
Adapter: ISA adapter
fan1:        1200 RPM  (min =  600 RPM)
SYSTIN:       +35.0°C  (low  = +127.0°C, high = +127.0°C)
Vcore:         +0.96 V  (min =  +0.00 V, max =  +1.74 V)
```

---

### `hddtemp` - Hard Disk Temperature
```bash
sudo apt install hddtemp
sudo hddtemp /dev/sda              # Suhu disk tertentu
sudo hddtemp /dev/sda /dev/sdb    # Beberapa disk
sudo hddtemp -n /dev/sda          # Hanya angka
sudo hddtemp -u C /dev/sda        # Dalam Celsius
sudo hddtemp -u F /dev/sda        # Dalam Fahrenheit
```

---

### `acpi` - ACPI Information
```bash
sudo apt install acpi
acpi                              # Status baterai
acpi -a                           # Status adapter AC
acpi -b                           # Info baterai
acpi -t                           # Suhu
acpi -f                           # Info kipas
acpi -V                           # Semua info
acpi -i                           # Rata-rata discharge
```

---

### `powertop` - Power Consumption Monitor
```bash
sudo apt install powertop
sudo powertop                     # Monitor power
sudo powertop --time=60           # Monitor 60 detik
sudo powertop --html=report.html  # Output HTML
sudo powertop --csv=report.csv    # Output CSV
sudo powertop --auto-tune         # Otomatis optimasi power
```

---

## 8.7 Kernel & Modul

### `uname -r` - Kernel Version
```bash
uname -r                          # Versi kernel
uname -a                          # Semua info kernel
cat /proc/version                 # Versi kernel dari /proc
cat /boot/config-$(uname -r)     # Konfigurasi kernel
ls /boot/                         # File boot
```

---

### `modprobe` - Load/Unload Kernel Modules
```bash
modprobe [opsi] <module>
```
> Memuat atau menghapus modul kernel

| Perintah | Penjelasan |
|---|---|
| `sudo modprobe <module>` | Load modul |
| `sudo modprobe -r <module>` | Unload modul |
| `sudo modprobe -v <module>` | Verbose |
| `sudo modprobe -n <module>` | Dry run |
| `sudo modprobe --show-depends <module>` | Tampilkan dependensi |
| `sudo modprobe -a <module1> <module2>` | Load beberapa modul |
| `modprobe -c` | Tampilkan konfigurasi |
| `modprobe -c \| grep <module>` | Cek konfigurasi modul |

**Contoh:**
```bash
sudo modprobe bluetooth           # Load modul Bluetooth
sudo modprobe -r bluetooth        # Unload modul Bluetooth
sudo modprobe nfs                 # Load modul NFS
sudo modprobe loop                # Load loop device
sudo modprobe br_netfilter        # Load untuk Docker/K8s
```

---

### `lsmod` - List Kernel Modules
```bash
lsmod [opsi]
```
> Menampilkan modul kernel yang sedang dimuat

```bash
lsmod                            # Daftar semua modul
lsmod | grep bluetooth           # Cari modul tertentu
lsmod | wc -l                    # Hitung jumlah modul
```

**Contoh Output:**
```
Module                  Size  Used by
bluetooth             643072  0
rfkill                 32768  3 bluetooth
joydev                 28672  0
nvidia_drm             77824  1
nvidia_modeset       1175552  1 nvidia_drm
nvidia              56209408  2 nvidia_modeset
```

---

### `insmod` - Insert Module
```bash
sudo insmod /path/to/module.ko   # Load modul dari file
sudo insmod /path/module.ko param=value  # Dengan parameter
```

---

### `rmmod` - Remove Module
```bash
sudo rmmod <module>              # Hapus modul
sudo rmmod -f <module>           # Force hapus
sudo rmmod -v <module>           # Verbose
```

---

### `modinfo` - Module Information
```bash
modinfo <module>
```
> Menampilkan informasi tentang modul kernel

```bash
modinfo bluetooth                # Info modul bluetooth
modinfo nvidia                   # Info modul NVIDIA
modinfo -F version bluetooth     # Hanya versi
modinfo -F author bluetooth      # Hanya author
modinfo -F parm bluetooth        # Parameter yang tersedia
modinfo -F filename bluetooth    # Path file modul
modinfo -k $(uname -r) bluetooth # Untuk kernel tertentu
```

---

### `depmod` - Module Dependencies
```bash
sudo depmod                      # Hitung ulang dependensi
sudo depmod -a                   # Semua modul
sudo depmod -n                   # Dry run
sudo depmod -v                   # Verbose
sudo depmod $(uname -r)          # Kernel tertentu
cat /lib/modules/$(uname -r)/modules.dep  # File dependensi
```

---

### Konfigurasi Modul
```bash
# File konfigurasi modul
/etc/modprobe.d/
/etc/modprobe.conf

# Blacklist modul (mencegah load)
echo "blacklist nouveau" | sudo tee /etc/modprobe.d/blacklist-nouveau.conf

# Tambah parameter modul
echo "options module_name parameter=value" | sudo tee /etc/modprobe.d/module.conf

# Auto-load modul saat boot
echo "module_name" | sudo tee -a /etc/modules

# Lihat modul yang akan di-load saat boot
cat /etc/modules
```

---

### `sysctl` - Kernel Parameters
```bash
sysctl [opsi] [parameter]
```
> Membaca dan mengatur parameter kernel

| Perintah | Penjelasan |
|---|---|
| `sysctl -a` | Tampilkan semua parameter |
| `sysctl -a \| grep net` | Filter parameter jaringan |
| `sysctl net.ipv4.ip_forward` | Baca parameter tertentu |
| `sudo sysctl net.ipv4.ip_forward=1` | Set parameter |
| `sudo sysctl -w net.ipv4.ip_forward=1` | Set dengan -w |
| `sudo sysctl -p` | Load dari /etc/sysctl.conf |
| `sudo sysctl -p /etc/sysctl.d/custom.conf` | Load dari file tertentu |
| `sudo sysctl --system` | Load semua file konfigurasi |
| `sysctl -n net.ipv4.ip_forward` | Hanya nilai (tanpa nama) |

**Parameter Penting:**
```bash
# Jaringan
sudo sysctl net.ipv4.ip_forward=1           # Enable IP forwarding
sudo sysctl net.ipv4.tcp_syncookies=1       # Enable SYN cookies
sudo sysctl net.core.somaxconn=65535        # Max socket connection
sudo sysctl net.ipv4.tcp_max_syn_backlog=65535

# Memori
sudo sysctl vm.swappiness=10                # Agresivitas swap
sudo sysctl vm.dirty_ratio=15              # Dirty ratio
sudo sysctl vm.dirty_background_ratio=5    # Background dirty ratio
sudo sysctl vm.overcommit_memory=0         # Memory overcommit

# Kernel
sudo sysctl kernel.pid_max=4194304         # Max PID
sudo sysctl kernel.threads-max=2097152     # Max threads
sudo sysctl fs.file-max=2097152            # Max open files
sudo sysctl fs.inotify.max_user_watches=524288  # Inotify watches
```

**File Konfigurasi sysctl:**
```bash
# File konfigurasi
/etc/sysctl.conf
/etc/sysctl.d/

# Contoh /etc/sysctl.d/99-custom.conf:
net.ipv4.ip_forward = 1
vm.swappiness = 10
fs.file-max = 2097152
net.core.somaxconn = 65535
```

---

### `dmesg` - Kernel Ring Buffer
```bash
dmesg [opsi]
```
> Menampilkan pesan dari kernel ring buffer

| Perintah | Penjelasan |
|---|---|
| `dmesg` | Tampilkan semua pesan kernel |
| `dmesg -H` | Human-readable dengan warna |
| `dmesg -T` | Timestamp dalam format manusia |
| `dmesg -h` | Human-readable |
| `dmesg -w` | Follow mode (tunggu pesan baru) |
| `dmesg -W` | Follow hanya pesan baru |
| `dmesg -l err` | Hanya pesan error |
| `dmesg -l warn` | Hanya pesan warning |
| `dmesg -l info` | Hanya pesan info |
| `dmesg -l debug` | Hanya pesan debug |
| `dmesg -l err,warn` | Error dan warning |
| `dmesg -f kern` | Hanya dari kernel |
| `dmesg -f daemon` | Hanya dari daemon |
| `dmesg -c` | Clear ring buffer setelah ditampilkan |
| `sudo dmesg -C` | Clear ring buffer |
| `dmesg -n 1` | Set log level |
| `dmesg \| grep -i usb` | Cari pesan USB |
| `dmesg \| grep -i error` | Cari error |
| `dmesg \| grep -i fail` | Cari kegagalan |
| `dmesg \| grep -i sda` | Pesan disk |
| `dmesg \| grep -i eth0` | Pesan interface jaringan |
| `dmesg \| tail -20` | 20 pesan terakhir |
| `dmesg -T \| grep "Jan 15"` | Filter tanggal |

---

## 8.8 Boot & Systemd

### `systemctl` - Systemd Control
```bash
systemctl [opsi] <perintah> [unit]
```
> Manajemen sistem dan service menggunakan systemd

**Manajemen Service:**
| Perintah | Penjelasan |
|---|---|
| `systemctl status <service>` | Status service |
| `sudo systemctl start <service>` | Mulai service |
| `sudo systemctl stop <service>` | Hentikan service |
| `sudo systemctl restart <service>` | Restart service |
| `sudo systemctl reload <service>` | Reload konfigurasi |
| `sudo systemctl enable <service>` | Aktifkan saat boot |
| `sudo systemctl disable <service>` | Nonaktifkan saat boot |
| `sudo systemctl enable --now <service>` | Aktifkan + start |
| `sudo systemctl disable --now <service>` | Nonaktifkan + stop |
| `systemctl is-active <service>` | Cek apakah aktif |
| `systemctl is-enabled <service>` | Cek apakah enabled |
| `systemctl is-failed <service>` | Cek apakah gagal |
| `sudo systemctl mask <service>` | Mask (tidak bisa distart) |
| `sudo systemctl unmask <service>` | Unmask |
| `sudo systemctl reset-failed` | Reset status failed |
| `sudo systemctl kill <service>` | Kill service |
| `sudo systemctl kill -s SIGKILL <service>` | Force kill |

**Informasi Unit:**
| Perintah | Penjelasan |
|---|---|
| `systemctl list-units` | Daftar semua unit aktif |
| `systemctl list-units --all` | Semua unit |
| `systemctl list-units --type=service` | Hanya service |
| `systemctl list-units --type=socket` | Hanya socket |
| `systemctl list-units --type=timer` | Hanya timer |
| `systemctl list-units --failed` | Unit yang gagal |
| `systemctl list-unit-files` | Daftar file unit |
| `systemctl list-unit-files --type=service` | File service |
| `systemctl list-dependencies <service>` | Dependensi service |
| `systemctl list-dependencies --reverse <service>` | Dependensi terbalik |
| `systemctl list-sockets` | Daftar socket |
| `systemctl list-timers` | Daftar timer |
| `systemctl list-jobs` | Job yang pending |
| `systemctl cat <service>` | Tampilkan file unit |
| `systemctl show <service>` | Semua properti unit |
| `systemctl show -p MainPID <service>` | Properti tertentu |
| `sudo systemctl edit <service>` | Edit override unit |
| `sudo systemctl edit --full <service>` | Edit file unit penuh |

**Manajemen Sistem:**
| Perintah | Penjelasan |
|---|---|
| `sudo systemctl reboot` | Reboot sistem |
| `sudo systemctl poweroff` | Matikan sistem |
| `sudo systemctl halt` | Halt sistem |
| `sudo systemctl suspend` | Suspend (sleep) |
| `sudo systemctl hibernate` | Hibernate |
| `sudo systemctl hybrid-sleep` | Hybrid sleep |
| `sudo systemctl rescue` | Mode rescue |
| `sudo systemctl emergency` | Mode emergency |
| `systemctl get-default` | Target boot default |
| `sudo systemctl set-default multi-user.target` | Set target boot |
| `sudo systemctl set-default graphical.target` | Set ke GUI |
| `sudo systemctl isolate multi-user.target` | Pindah ke target |
| `sudo systemctl daemon-reload` | Reload konfigurasi systemd |
| `sudo systemctl daemon-reexec` | Re-execute systemd |

**Target systemd:**
```
poweroff.target    → Mati
rescue.target      → Rescue mode
multi-user.target  → Multi-user tanpa GUI (runlevel 3)
graphical.target   → Multi-user dengan GUI (runlevel 5)
reboot.target      → Reboot
emergency.target   → Emergency shell
```

---

### `journalctl` - Systemd Journal
```bash
journalctl [opsi]
```
> Melihat log dari systemd journal

| Perintah | Penjelasan |
|---|---|
| `journalctl` | Semua log |
| `journalctl -f` | Follow (real-time) |
| `journalctl -e` | Langsung ke akhir log |
| `journalctl -n 50` | 50 baris terakhir |
| `journalctl -r` | Urutan terbalik (terbaru dulu) |
| `journalctl -u nginx` | Log service nginx |
| `journalctl -u nginx -f` | Follow log nginx |
| `journalctl -u nginx -n 100` | 100 baris terakhir nginx |
| `journalctl -p err` | Hanya error |
| `journalctl -p warning` | Hanya warning |
| `journalctl -p 0..3` | Emergency sampai error |
| `journalctl --since "2024-01-15"` | Sejak tanggal |
| `journalctl --since "10:00"` | Sejak jam 10:00 |
| `journalctl --since "1 hour ago"` | 1 jam terakhir |
| `journalctl --until "2024-01-15 12:00:00"` | Hingga waktu tertentu |
| `journalctl --since yesterday` | Sejak kemarin |
| `journalctl --since today` | Sejak hari ini |
| `journalctl -b` | Log boot saat ini |
| `journalctl -b -1` | Log boot sebelumnya |
| `journalctl -b -2` | Dua boot sebelumnya |
| `journalctl --list-boots` | Daftar semua boot |
| `journalctl _PID=1234` | Log dari PID tertentu |
| `journalctl _UID=1000` | Log dari UID tertentu |
| `journalctl _COMM=nginx` | Log dari program tertentu |
| `journalctl _SYSTEMD_UNIT=nginx.service` | Log unit tertentu |
| `journalctl -k` | Hanya pesan kernel (seperti dmesg) |
| `journalctl -o short` | Format pendek |
| `journalctl -o json` | Format JSON |
| `journalctl -o json-pretty` | JSON yang mudah dibaca |
| `journalctl -o verbose` | Format verbose |
| `journalctl -o cat` | Hanya pesan (tanpa metadata) |
| `journalctl --disk-usage` | Penggunaan disk journal |
| `sudo journalctl --vacuum-size=500M` | Hapus log lama sampai 500MB |
| `sudo journalctl --vacuum-time=7d` | Hapus log lebih dari 7 hari |
| `sudo journalctl --rotate` | Rotasi file journal |
| `journalctl -x` | Tambahkan penjelasan error |
| `journalctl -xe` | Penjelasan + loncat ke akhir |

**Konfigurasi Journal:**
```bash
# /etc/systemd/journald.conf
[Journal]
Storage=persistent      # persistent/volatile/auto/none
Compress=yes            # Kompresi log
SystemMaxUse=500M       # Max ukuran log
SystemKeepFree=100M     # Ruang disk minimum tersisa
MaxRetentionSec=1month  # Retensi maksimum
MaxFileSec=1week        # Rotasi per minggu

# Restart journald setelah konfigurasi
sudo systemctl restart systemd-journald
```

---

### `loginctl` - Login Session Control
```bash
loginctl [opsi] <perintah>
```
> Manajemen sesi login menggunakan systemd-logind

```bash
loginctl                              # Daftar sesi
loginctl list-sessions                # Daftar sesi
loginctl list-users                   # Daftar user yang login
loginctl list-seats                   # Daftar seat
loginctl show-session <id>            # Detail sesi
loginctl show-user <user>             # Detail user
loginctl terminate-session <id>       # Terminate sesi
loginctl terminate-user <user>        # Terminate semua sesi user
loginctl kill-session <id>            # Kill sesi
loginctl lock-session <id>            # Lock sesi
loginctl unlock-session <id>          # Unlock sesi
loginctl lock-sessions                # Lock semua sesi
loginctl enable-linger <user>         # Enable user linger (proses tetap jalan saat logout)
loginctl disable-linger <user>        # Disable linger
```

---

## 8.9 Informasi Boot

### `bootctl` - Boot Manager Control
```bash
bootctl [opsi] <perintah>
```
> Manajemen boot loader systemd-boot

```bash
bootctl status                        # Status boot loader
bootctl list                          # Daftar boot entries
sudo bootctl install                  # Install systemd-boot
sudo bootctl update                   # Update boot loader
bootctl is-installed                  # Cek apakah terinstal
bootctl random-seed                   # Generate random seed
```

---

### `grub-update` & GRUB Configuration
```bash
# Update GRUB
sudo update-grub                     # Debian/Ubuntu
sudo grub2-mkconfig -o /boot/grub2/grub.cfg  # RHEL/CentOS

# Install GRUB
sudo grub-install /dev/sda           # Install ke disk
sudo grub-install --target=x86_64-efi --efi-directory=/boot/efi  # UEFI

# Konfigurasi GRUB
sudo nano /etc/default/grub
# Setelah edit, jalankan:
sudo update-grub

# Parameter penting /etc/default/grub:
GRUB_DEFAULT=0                        # Entry default
GRUB_TIMEOUT=5                        # Timeout (detik)
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"  # Parameter kernel
GRUB_CMDLINE_LINUX=""                 # Parameter kernel (semua mode)
```

---

### `efibootmgr` - EFI Boot Manager
```bash
efibootmgr [opsi]
```
> Manajemen boot entries UEFI

```bash
efibootmgr                           # Tampilkan boot entries
efibootmgr -v                        # Verbose
sudo efibootmgr -n 0003              # Set boot next (sekali)
sudo efibootmgr -o 0001,0003,0002   # Set boot order
sudo efibootmgr -a -b 0001          # Aktifkan entry
sudo efibootmgr -A -b 0001          # Nonaktifkan entry
sudo efibootmgr -B -b 0001          # Hapus entry
sudo efibootmgr -c -d /dev/sda -p 1 -L "Ubuntu" -l '\EFI\ubuntu\shimx64.efi'  # Buat entry baru
```

---

## 8.10 Informasi & Manajemen Sistem Lanjutan

### `strace` - System Call Tracer (Review)
```bash
sudo strace -p $(pidof nginx)          # Trace proses yang berjalan
strace -c ls                           # Statistik system calls
strace -e open,read,write ls           # Filter system calls
```

---

### `ltrace` - Library Call Tracer
```bash
ltrace ls                              # Trace library calls
ltrace -c ls                           # Statistik
ltrace -e malloc ls                    # Filter fungsi tertentu
```

---

### `perf` - Performance Analysis
```bash
sudo apt install linux-perf

perf top                               # CPU profiling real-time
perf top -p 1234                       # Profile PID tertentu
sudo perf record ./program             # Record profiling
perf report                            # Tampilkan hasil
perf stat ./program                    # Statistik performance
perf stat -a sleep 5                   # Statistik sistem 5 detik
perf list                              # Daftar events
perf bench mem memcpy                  # Benchmark memori
perf bench sched messaging             # Benchmark scheduler
```

---

### `sar` - System Activity Reporter
```bash
sudo apt install sysstat

sar                                    # CPU usage hari ini
sar -u 2 5                            # CPU usage, 2 detik, 5 kali
sar -r 2 5                            # Memory usage
sar -d 2 5                            # Disk activity
sar -n DEV 2 5                        # Network traffic
sar -n TCP 2 5                        # TCP statistics
sar -q 2 5                            # Queue length
sar -b 2 5                            # I/O statistics
sar -f /var/log/sysstat/sa15          # Baca file log
sar -s 10:00:00 -e 12:00:00          # Range waktu
sar -o output.sar 2 5                 # Simpan output
sadf /var/log/sysstat/sa15           # Tampilkan data historis
sadf -d /var/log/sysstat/sa15        # Output database-friendly
sadf -j /var/log/sysstat/sa15        # Output JSON
```

---

### `nmon` - Performance Monitor
```bash
sudo apt install nmon

nmon                                   # Interactive mode
nmon -f -s 30 -c 720                  # Log ke file, setiap 30 detik, 720 kali (6 jam)
nmon -f -s 60 -c 1440 -m /var/log/nmon/  # Simpan ke direktori
```

**Shortcut dalam nmon:**
```
c → CPU
m → Memory
d → Disk
n → Network
k → Kernel
j → Filesystem
t → Top processes
q → Quit
h → Help
```

---

### `glances` - System Monitor
```bash
sudo apt install glances

glances                                # Monitor interaktif
glances -w                            # Web server mode
glances -c hostname                   # Client mode
glances --export influxdb             # Export ke InfluxDB
glances -t 5                          # Refresh 5 detik
glances -1                            # Per CPU core
glances -b                            # Tampilkan dalam bytes
glances --disable-plugin docker       # Nonaktifkan plugin
```

---

### `lsof` - List Open Files (System Context)
```bash
# Cek file yang dibuka oleh sistem
lsof | wc -l                          # Total file terbuka
lsof | grep DEL                       # File yang terhapus tapi masih terbuka
lsof -u root                          # File terbuka oleh root
lsof +D /tmp                          # File di /tmp
lsof -i -s TCP:LISTEN                 # Port yang listening
sudo lsof -i -n -P                    # Semua koneksi numerik
```

---

### `stap` - SystemTap
```bash
sudo apt install systemtap

# Contoh script SystemTap
# Monitor syscall tertentu
sudo stap -e 'probe syscall.open { printf("%s opened %s\n", execname(), filename) }'

# Profiling
sudo stap -e 'probe timer.s(1) { printf("uptime: %d s\n", get_seconds()) }'
```

---

### `ausearch` & `auditctl` - Audit System
```bash
sudo apt install auditd

# auditctl - konfigurasi audit rules
sudo auditctl -l                       # List rules
sudo auditctl -w /etc/passwd -p wa -k passwd_changes  # Watch file
sudo auditctl -w /etc/shadow -p wa -k shadow_changes
sudo auditctl -a always,exit -F arch=b64 -S execve -k exec_commands
sudo auditctl -e 1                     # Enable audit
sudo auditctl -e 0                     # Disable audit

# ausearch - cari di audit log
sudo ausearch -f /etc/passwd           # File tertentu
sudo ausearch -m login                 # Event login
sudo ausearch -ua 1000                 # User tertentu
sudo ausearch -sc execve               # System call tertentu
sudo ausearch --start today            # Sejak hari ini
sudo ausearch -k passwd_changes        # Berdasarkan key

# aureport - laporan audit
sudo aureport                          # Ringkasan
sudo aureport --login                  # Laporan login
sudo aureport --failed                 # Laporan kegagalan
sudo aureport --file                   # Laporan akses file
sudo aureport --exec                   # Laporan eksekusi
sudo aureport --user                   # Laporan user
sudo aureport -ts today                # Sejak hari ini
```

---

## 8.11 Environment & Shell Info

### `env` - Display Environment
```bash
env                                    # Tampilkan semua variabel environment
env | sort                             # Urutkan
env | grep PATH                        # Cari variabel PATH
env VAR=value command                  # Jalankan command dengan env kustom
env -i command                         # Jalankan dengan environment bersih
env -u VAR command                     # Hapus variabel tertentu
printenv                               # Sama dengan env
printenv PATH                          # Nilai variabel tertentu
printenv HOME USER SHELL               # Beberapa variabel
```

---

### `set` - Shell Variables
```bash
set                                    # Tampilkan semua variabel dan fungsi shell
set -e                                 # Exit jika ada error
set -x                                 # Debug mode (trace perintah)
set -u                                 # Error jika variabel tidak terdefinisi
set -o pipefail                        # Gagal jika ada perintah dalam pipe yang gagal
set -euxo pipefail                     # Kombinasi untuk scripting yang aman
set +e                                 # Nonaktifkan exit on error
set +x                                 # Nonaktifkan debug mode
```

---

### `export` - Export Variables
```bash
export VAR=value                       # Set dan export variabel
export PATH=$PATH:/usr/local/bin       # Tambah ke PATH
export -n VAR                          # Hapus export (tidak lagi di environment)
export -p                              # Tampilkan semua exported variables
```

---

### `echo` - Display Variables
```bash
echo $PATH                             # Tampilkan PATH
echo $HOME                             # Home directory
echo $USER                             # Username
echo $SHELL                            # Shell yang digunakan
echo $HOSTNAME                         # Hostname
echo $PWD                              # Direktori saat ini
echo $OLDPWD                           # Direktori sebelumnya
echo $?                                # Exit code perintah terakhir
echo $$                                # PID shell saat ini
echo $!                                # PID proses background terakhir
echo $0                                # Nama script/shell
echo $RANDOM                           # Angka random
echo $LINENO                           # Nomor baris saat ini (dalam script)
echo $BASH_VERSION                     # Versi Bash
echo $TERM                             # Tipe terminal
echo $DISPLAY                          # X display
echo $LANG                             # Locale
echo $TZ                               # Timezone
```

---

## Ringkasan Perintah Bagian 8

```
uname           → Info sistem & kernel
hostnamectl     → Info hostname & sistem (systemd)
lsb_release     → Info distribusi Linux
uptime          → Lama sistem berjalan
date            → Tanggal & waktu
timedatectl     → Manajemen waktu systemd
cal             → Kalender
hwclock         → Hardware clock
lscpu           → Info CPU detail
free            → Penggunaan RAM & swap
dmidecode       → Info hardware DMI/SMBIOS
lshw            → Info hardware lengkap
inxi            → Info sistem komprehensif
hwinfo          → Info hardware detail
lspci           → Daftar perangkat PCI
lsusb           → Daftar perangkat USB
lsscsi          → Daftar perangkat SCSI
nvidia-smi      → Monitor GPU NVIDIA
sensors         → Suhu & sensor hardware
hddtemp         → Suhu hard disk
acpi            → Info baterai & power
powertop        → Monitor konsumsi daya
modprobe        → Load/unload modul kernel
lsmod           → Daftar modul kernel
modinfo         → Info modul kernel
sysctl          → Parameter kernel
dmesg           → Pesan kernel ring buffer
systemctl       → Manajemen service systemd
journalctl      → Log systemd journal
loginctl        → Manajemen sesi login
bootctl         → Manajemen boot loader
efibootmgr      → Manajemen boot UEFI
perf            → Analisis performa
sar             → Laporan aktivitas sistem
glances         → Monitor sistem all-in-one
nmon            → Monitor performa interaktif
env             → Variabel environment
sysctl          → Parameter kernel runtime
```

---

## ✅ Bagian 8 Selesai!

**Lanjut ke Bagian 9: Perintah Kompresi & Arsip?**
