# Bagian 12: Perintah Monitoring & Log

---

## 12.1 System Monitoring Real-time

### `top` - Process Monitor (Review & Lanjutan)
```bash
top [opsi]
```
> Monitor proses dan sistem secara real-time

**Opsi Lanjutan:**
```bash
top -b -n 1                      # Batch mode sekali jalan
top -b -n 1 > top_output.txt     # Simpan ke file
top -b -d 5 -n 12 > log.txt      # Setiap 5 detik, 12 kali
top -p 1234,5678                 # Monitor PID tertentu
top -u username                  # Filter user tertentu
top -c                           # Tampilkan command lengkap
top -H                           # Tampilkan threads
top -i                           # Sembunyikan proses idle
top -s                           # Secure mode
top -S                           # Cumulative time mode

# Shortcut penting dalam top
M      # Sort by Memory
P      # Sort by CPU
T      # Sort by Time
N      # Sort by PID
R      # Reverse sort order
k      # Kill proses
r      # Renice proses
u      # Filter user
o      # Filter kondisi
f      # Pilih field
W      # Simpan konfigurasi
1      # Tampilkan semua CPU
m      # Toggle memory info
t      # Toggle CPU info
c      # Toggle command line
V      # Forest view (hierarki)
H      # Toggle threads
I      # Toggle Irix mode
z      # Toggle warna
x      # Highlight sort column
b      # Bold/highlight
</>    # Pindah sort column
```

---

### `htop` - Interactive Process Viewer (Lanjutan)
```bash
# Konfigurasi htop
~/.config/htop/htoprc            # File konfigurasi

# Shortcut lengkap htop
F1          # Bantuan
F2          # Setup (konfigurasi tampilan)
F3          # Cari proses
F4          # Filter proses
F5          # Tree view
F6          # Sort by column
F7          # Nice -1 (prioritas lebih tinggi)
F8          # Nice +1 (prioritas lebih rendah)
F9          # Kill (pilih sinyal)
F10         # Quit
u           # Filter by user
U           # Hapus filter user
H           # Toggle user threads
K           # Toggle kernel threads
p           # Toggle path dalam command
Z           # Toggle zswap info
I           # Invert sort
/           # Search
\           # Filter
Space       # Tandai proses
c           # Tandai proses dan anak-anaknya
U           # Hapus semua tanda
a           # Set CPU affinity
e           # Tampilkan environment variables
s           # Strace proses
l           # lsof proses
w           # Tampilkan command wrap
x           # Tampilkan locks
+/-         # Expand/collapse tree
```

---

### `glances` - All-in-one Monitor
```bash
glances [opsi]
```
> Monitor sistem komprehensif dalam satu layar

```bash
glances                          # Monitor lokal
glances -w                       # Web server mode (port 61208)
glances -w -p 8080               # Port kustom
glances --browser                # Browser mode
glances -c hostname              # Client mode (connect ke server)
glances -s                       # Server mode
glances -B 0.0.0.0               # Bind ke semua interface
glances -t 2                     # Refresh setiap 2 detik
glances -1                       # Per CPU core
glances -b                       # Tampilkan dalam bytes
glances --disable-plugin docker  # Nonaktifkan plugin
glances --enable-plugin gpu      # Aktifkan plugin GPU
glances -q                       # Quiet mode
glances -f /path/conf            # File konfigurasi
glances --export influxdb        # Export ke InfluxDB
glances --export csv             # Export ke CSV
glances --export json            # Export ke JSON
glances -r                       # Rekam ke file

# Shortcut dalam glances
h atau ?    # Bantuan
q           # Keluar
1           # Per CPU (global/per-core)
2           # Disable/enable left sidebar
3           # Disable/enable quicklook
4           # Disable all plugins tapi quicklook
0           # Task manager bar
a           # Sort proses otomatis
c           # Sort by CPU
m           # Sort by Memory
u           # Sort by User
p           # Sort by name
i           # Sort by I/O rate
d           # Disk I/O stats on/off
f           # Filesystem on/off
n           # Network on/off
s           # Sensors on/off
y           # Hddtemp on/off
l           # Log on/off
b           # Bytes/bits toggle
t           # View network traffic
w           # Delete warning logs
x           # Delete warning + critical logs
e           # Tampilkan proses extended
g           # Tampilkan mean GPU
j           # Tampilkan alert logs
k           # Koneksi TCP
z           # Proses stats on/off
ENTER       # Refresh
F5          # Refresh tampilan
```

---

### `nmon` - Performance Monitor
```bash
nmon [opsi]
```
> Monitor performa sistem yang komprehensif

```bash
nmon                             # Mode interaktif
nmon -f                          # Mode file (rekam ke file)
nmon -f -s 30 -c 720            # Rekam setiap 30 detik, 720 kali (6 jam)
nmon -f -s 60 -c 1440 -m /var/log/nmon/  # Simpan ke direktori
nmon -F output.nmon              # File output tertentu
nmon -p                          # Tampilkan PID saat rekam

# Shortcut dalam nmon
?           # Bantuan
q           # Keluar
c           # CPU stats
C           # CPU panjang
m           # Memory stats
M           # Memory panjang
n           # Network stats
N           # Network panjang
d           # Disk I/O stats
D           # Disk panjang
k           # Kernel stats
j           # Filesystem stats
t           # Top proses
u           # Top proses (update)
l           # Long-term CPU
r           # System resources
h           # Bantuan
+           # Tambah disk
-           # Hapus disk

# Analisis file nmon
sudo apt install nmon
# File .nmon bisa dianalisis dengan nmon analyser (Excel)
# atau dengan nmon2influxdb untuk InfluxDB
```

---

### `dstat` - System Resource Statistics
```bash
sudo apt install dstat
```
> Statistik resource sistem yang fleksibel

```bash
dstat                            # Default stats
dstat -c                         # CPU stats
dstat -d                         # Disk stats
dstat -n                         # Network stats
dstat -m                         # Memory stats
dstat -s                         # Swap stats
dstat -p                         # Process stats
dstat -y                         # System stats (interrupt, context switch)
dstat -l                         # Load average
dstat -r                         # I/O requests
dstat -f                         # Full stats semua device
dstat -a                         # Default + extra (-g -l -m -s --top-cpu)
dstat -cdnm 2                    # Kombinasi: CPU disk net mem, setiap 2 detik
dstat --top-cpu                  # Proses dengan CPU tertinggi
dstat --top-mem                  # Proses dengan memory tertinggi
dstat --top-io                   # Proses dengan I/O tertinggi
dstat --top-latency              # Proses dengan latency tertinggi
dstat -D sda,sdb                 # Disk tertentu
dstat -N eth0,eth1               # Interface tertentu
dstat --output /tmp/stats.csv    # Simpan ke CSV
dstat 2 10                       # Interval 2 detik, 10 kali
dstat --nocolor                  # Tanpa warna
dstat --noheaders                # Tanpa header berulang
```

---

### `sar` - System Activity Reporter (Lanjutan)
```bash
# Konfigurasi sysstat
sudo apt install sysstat

# Aktifkan pengumpulan data
sudo sed -i 's/ENABLED="false"/ENABLED="true"/' /etc/default/sysstat
sudo systemctl restart sysstat
sudo systemctl enable sysstat

# Data tersimpan di /var/log/sysstat/
ls /var/log/sysstat/

# Melihat data historis
sar -u                           # CPU usage hari ini
sar -u -f /var/log/sysstat/sa15  # CPU usage tanggal 15
sar -r                           # Memory usage
sar -r -f /var/log/sysstat/sa15  # Memory tanggal 15
sar -d                           # Disk activity
sar -n DEV                       # Network traffic
sar -n SOCK                      # Socket statistics
sar -n TCP                       # TCP statistics
sar -n UDP                       # UDP statistics
sar -n IP                        # IP statistics
sar -q                           # Queue dan load average
sar -b                           # I/O dan transfer rate
sar -w                           # Context switch dan proses
sar -v                           # Inode, file, dan tabel kernel
sar -W                           # Swapping statistics
sar -H                           # Huge pages
sar -I ALL                       # Interrupt statistics
sar -P ALL                       # Per processor statistics
sar -u ALL                       # CPU usage detail (semua field)

# Filter waktu
sar -u -s 08:00:00 -e 12:00:00  # Dari jam 8 sampai 12
sar -u --since="10:00"           # Sejak jam 10

# Format output
sar -u -o /tmp/sar.bin 2 10      # Simpan ke binary
sar -u -f /tmp/sar.bin           # Baca dari binary
sadf -d /var/log/sysstat/sa15 -- -u  # Format database
sadf -j /var/log/sysstat/sa15 -- -u  # Format JSON
sadf -x /var/log/sysstat/sa15 -- -u  # Format XML
sadf -g /var/log/sysstat/sa15 -- -u > report.svg  # SVG graph

# Laporan harian otomatis
cat /etc/cron.d/sysstat          # Cek jadwal cron
/usr/lib/sysstat/sa2             # Generate laporan harian
```

---

## 12.2 CPU Monitoring

### `mpstat` - CPU Statistics
```bash
mpstat [opsi] [interval] [count]
```
> Statistik penggunaan CPU per core

```bash
mpstat                           # Statistik semua CPU
mpstat 2                         # Update setiap 2 detik
mpstat 2 10                      # 10 kali, setiap 2 detik
mpstat -P ALL                    # Semua CPU core
mpstat -P ALL 2 5                # Semua core, 2 detik, 5 kali
mpstat -P 0,2,4                  # Core tertentu
mpstat -u                        # CPU utilization (default)
mpstat -I ALL 2                  # Semua interrupt
mpstat -I CPU 2                  # Interrupt per CPU
mpstat -I SUM 2                  # Total interrupt
mpstat -A 2                      # Semua statistik
mpstat -T 2                      # Dengan timestamp
mpstat -o JSON 2                 # Output JSON
mpstat --dec=2                   # 2 desimal

# Field output mpstat:
# %usr    → User space
# %nice   → Nice priority
# %sys    → Kernel
# %iowait → I/O wait
# %irq    → Hardware interrupt
# %soft   → Software interrupt
# %steal  → Stolen (VM)
# %guest  → Guest VM
# %idle   → Idle
```

---

### `pidstat` - Statistics per Process
```bash
pidstat [opsi] [interval] [count]
```
> Statistik per proses (CPU, memori, I/O)

```bash
pidstat                          # Semua proses aktif
pidstat 2                        # Update setiap 2 detik
pidstat 2 10                     # 10 kali
pidstat -u 2                     # CPU usage per proses
pidstat -r 2                     # Memory usage
pidstat -d 2                     # Disk I/O
pidstat -w 2                     # Context switching
pidstat -t 2                     # Per thread
pidstat -l 2                     # Command lengkap
pidstat -p 1234 2                # Proses tertentu
pidstat -p 1234,5678 2           # Beberapa PID
pidstat -C nginx 2               # Proses dengan nama tertentu
pidstat -u -r -d 2               # CPU + memory + disk
pidstat -T ALL 2                 # Semua statistik
pidstat --human 2                # Human-readable
pidstat -G nama_proses 2         # Filter nama proses
pidstat -U username 2            # Filter user
pidstat -I 2                     # Divide by total CPU
```

---

### `perf` - Performance Analysis Tool
```bash
# Monitor CPU performance
sudo perf top                    # CPU profiling real-time
sudo perf top -p 1234            # Profile PID tertentu
sudo perf top -e cache-misses    # Event tertentu
sudo perf top -g                 # Call graph

# Record dan report
sudo perf record ./program       # Record
sudo perf record -g ./program    # Dengan call graph
sudo perf record -F 99 -p 1234  # Sampling frequency 99Hz
sudo perf report                 # Analisis hasil
sudo perf report --sort comm,dso # Sort tertentu

# Statistik
sudo perf stat ./program         # Statistik perintah
sudo perf stat -a sleep 5        # Statistik sistem 5 detik
sudo perf stat -e cache-misses,cache-references ./program

# Benchmark
perf bench mem memcpy            # Benchmark memori
perf bench sched messaging       # Benchmark scheduler
perf bench futex hash            # Benchmark futex

# Event list
perf list                        # Semua event
perf list cache                  # Event cache
perf list hw                     # Hardware events
perf list sw                     # Software events
perf list tracepoint             # Tracepoint events

# Trace
sudo perf trace ./program        # Trace system calls
sudo perf trace -p 1234          # Trace proses
sudo perf trace -e open,read     # Event tertentu

# Flame graph (butuh FlameGraph)
sudo perf record -g -p 1234
sudo perf script > perf.script
./stackcollapse-perf.pl perf.script | ./flamegraph.pl > flame.svg
```

---

## 12.3 Memory Monitoring

### `free` - Memory Usage (Lanjutan)
```bash
# Monitoring berkelanjutan
watch -n 1 free -h               # Setiap 1 detik
free -h -s 2                     # Built-in watch

# Breakdown memori
cat /proc/meminfo
grep -E "MemTotal|MemFree|MemAvailable|Cached|Buffers" /proc/meminfo

# Hitung memori yang benar-benar tersedia
free -m | awk 'NR==2{printf "Available: %s MB (%.2f%%)\n", $7, $7/$2*100}'

# Script monitor memori
cat > /usr/local/bin/memcheck.sh << 'EOF'
#!/bin/bash
THRESHOLD=90
USED=$(free | grep Mem | awk '{printf "%.0f", $3/$2 * 100}')
if [ $USED -gt $THRESHOLD ]; then
    echo "PERINGATAN: Penggunaan memori ${USED}%"
    ps aux --sort=-%mem | head -10
fi
EOF
```

---

### `smem` - Memory Reporter
```bash
sudo apt install smem

smem                             # Laporan memori per proses
smem -r                          # Reverse sort
smem -u                          # Per user
smem -m                          # Per mapping
smem -w                          # Per sistem (wide)
smem -p                          # Persentase
smem -k                          # Tampilkan dalam KB/MB/GB
smem -H                          # Tanpa header
smem -c "name pid pss rss"      # Kolom tertentu
smem -s pss                      # Sort by PSS
smem -r -s pss -k               # Reverse sort PSS human-readable
smem --pie=pss                   # Pie chart
smem --bar=pss                   # Bar chart
smem -P nginx                    # Filter proses nginx
smem -U username                 # Filter user

# PSS (Proportional Set Size): memori yang dimiliki proses
# RSS (Resident Set Size): memori fisik yang digunakan
# USS (Unique Set Size): memori yang hanya digunakan proses ini
```

---

## 12.4 Disk & I/O Monitoring

### `iostat` - I/O Statistics (Lanjutan)
```bash
iostat [opsi] [interval] [count]
```
> Statistik CPU dan I/O

```bash
iostat                           # Statistik dasar
iostat 2                         # Update setiap 2 detik
iostat 2 10                      # 10 kali
iostat -c 2                      # Hanya CPU
iostat -d 2                      # Hanya disk
iostat -x 2                      # Extended stats
iostat -x -d 2                   # Extended disk
iostat -h 2                      # Human-readable
iostat -m 2                      # Dalam MB
iostat -k 2                      # Dalam KB
iostat -p sda 2                  # Disk tertentu
iostat -p sda,sdb 2              # Beberapa disk
iostat -N 2                      # LVM device
iostat -t 2                      # Dengan timestamp
iostat -y 2                      # Skip statistik pertama (hanya delta)
iostat -z 2                      # Sembunyikan device tanpa aktivitas
iostat --pretty 2                # Format yang mudah dibaca
iostat -o JSON 2                 # Output JSON

# Field penting iostat -x:
# r/s      → Read requests per second
# w/s      → Write requests per second
# rMB/s    → Read MB per second
# wMB/s    → Write MB per second
# rrqm/s   → Read requests merged per second
# wrqm/s   → Write requests merged per second
# r_await  → Average wait time read (ms)
# w_await  → Average wait time write (ms)
# aqu-sz   → Average queue size
# %util    → Bandwidth utilization (100% = saturated)
```

---

### `iotop` - I/O Monitor per Process
```bash
sudo apt install iotop

sudo iotop                       # Monitor I/O semua proses
sudo iotop -o                    # Hanya proses dengan I/O
sudo iotop -b                    # Batch mode
sudo iotop -n 5                  # 5 iterasi
sudo iotop -d 2                  # Interval 2 detik
sudo iotop -p 1234               # PID tertentu
sudo iotop -u username           # User tertentu
sudo iotop -q                    # Quiet (tanpa header berulang)
sudo iotop -a                    # Accumulated I/O
sudo iotop -k                    # Dalam KB
sudo iotop --only                # Hanya proses dengan I/O aktif

# Shortcut dalam iotop
r          # Reverse sort
o          # Toggle hanya proses aktif
p          # Toggle thread/process
a          # Toggle accumulated
q          # Keluar
left/right # Ganti sort column
```

---

### `ioping` - I/O Latency
```bash
sudo apt install ioping

ioping /dev/sda                  # Monitor latency disk
ioping .                         # Monitor direktori saat ini
ioping -c 10 /dev/sda            # 10 request
ioping -D /dev/sda               # Direct I/O (bypass cache)
ioping -C /dev/sda               # Cached I/O
ioping -R /dev/sda               # Disk seek rate
ioping -RL /dev/sda              # Sequential read rate
ioping -W /dev/sda               # Write latency
ioping -s 4096 /dev/sda          # Block size 4096 bytes
ioping -i 0.5 /dev/sda           # Interval 0.5 detik
ioping -q /dev/sda               # Quiet
ioping -p 100 /dev/sda           # Period statistics setiap 100 request
```

---

### `blktrace` - Block Layer Tracing
```bash
sudo apt install blktrace

# Trace I/O di level block
sudo blktrace -d /dev/sda -o trace
sudo blkparse -i trace.blktrace.0 | head -50
sudo blkparse -i trace -o trace.txt
sudo btt -i trace.blktrace.0

# Trace real-time
sudo blktrace -d /dev/sda -o - | blkparse -i -
```

---

## 12.5 Network Monitoring

### `nethogs` - Bandwidth per Process (Lanjutan)
```bash
sudo nethogs                     # Monitor semua interface
sudo nethogs eth0                # Interface tertentu
sudo nethogs eth0 wlan0          # Beberapa interface
sudo nethogs -d 2                # Refresh 2 detik
sudo nethogs -t                  # Mode trace
sudo nethogs -b                  # Batch mode
sudo nethogs -v 3                # Verbose level 3
sudo nethogs -c 5                # 5 refresh lalu keluar
sudo nethogs -p                  # Tampilkan PID

# Shortcut dalam nethogs
q atau Q   # Keluar
s          # Sort by sent
r          # Sort by received
m          # Ganti tampilan (KB/s, KB, B)
```

---

### `iftop` - Network Bandwidth (Lanjutan)
```bash
sudo iftop                       # Monitor interface default
sudo iftop -i eth0               # Interface tertentu
sudo iftop -n                    # Jangan resolve hostname
sudo iftop -N                    # Jangan resolve port
sudo iftop -P                    # Tampilkan port
sudo iftop -B                    # Dalam bytes
sudo iftop -b                    # Tanpa bar grafik
sudo iftop -f "port 80"         # Filter port 80
sudo iftop -F 192.168.1.0/24    # Filter subnet
sudo iftop -G 192.168.1.0/24    # Filter subnet tujuan
sudo iftop -t                    # Mode teks (batch)
sudo iftop -s 5                  # Stop setelah 5 detik
sudo iftop -o 2s                 # Sort 2 detik

# Shortcut dalam iftop
n          # Toggle hostname resolution
N          # Toggle port resolution
t          # Toggle tampilan (send+receive, send, receive, df)
1/2/3      # Sort 2s/10s/40s
</>        # Sort source/destination
p          # Toggle port tampilan
P          # Toggle pause
l          # Set filter
L          # Toggle logarithmic scale
j/k        # Scroll
d          # Toggle direktori
h          # Bantuan
q          # Keluar
```

---

### `vnstat` - Network Traffic Monitor
```bash
sudo apt install vnstat

# Setup interface
sudo vnstat -u -i eth0           # Update/inisialisasi eth0
sudo systemctl enable vnstat
sudo systemctl start vnstat

# Tampilkan statistik
vnstat                           # Ringkasan semua interface
vnstat -i eth0                   # Interface tertentu
vnstat -h                        # Per jam (hourly)
vnstat -d                        # Per hari (daily)
vnstat -m                        # Per bulan (monthly)
vnstat -w                        # Per minggu (weekly)
vnstat -y                        # Per tahun (yearly)
vnstat -5                        # 5 menit terakhir
vnstat -t                        # Top 10 hari
vnstat -l                        # Live monitoring
vnstat -l -i eth0                # Live interface tertentu
vnstat --json                    # Output JSON
vnstat --xml                     # Output XML
vnstat --oneline                 # Satu baris per interface
vnstat --style 0                 # Tanpa grafik
vnstat --style 1                 # Grafik kecil
vnstat --style 4                 # Grafik lengkap
vnstat -ru                       # Rata-rata per jam
vnstat --begin "2024-01-01" --end "2024-01-31"  # Range tanggal

# Database manajemen
vnstat --add -i eth0             # Tambah interface
vnstat --remove -i eth0          # Hapus interface
vnstat -i eth0 --reset           # Reset statistik
sudo vnstatd                     # Daemon manual
```

---

### `bmon` - Bandwidth Monitor
```bash
sudo apt install bmon

bmon                             # Monitor semua interface
bmon -p eth0                     # Interface tertentu
bmon -p eth0,wlan0               # Beberapa interface
bmon -r 2                        # Refresh 2 detik
bmon -b                          # Dalam bytes
bmon -o ascii                    # Output ASCII
bmon -o csv                      # Output CSV
bmon -o html                     # Output HTML

# Shortcut
d          # Detail view
q          # Keluar
←/→        # Ganti interface
↑/↓        # Scroll
g          # General stats
```

---

### `ss` - Socket Statistics (Lanjutan)
```bash
ss                               # Semua socket established
ss -a                            # Semua socket
ss -l                            # Listening socket
ss -t                            # TCP
ss -u                            # UDP
ss -n                            # Numerik
ss -p                            # Tampilkan proses
ss -e                            # Extended info
ss -i                            # Internal TCP info
ss -m                            # Socket memory info
ss -o                            # Timer info
ss -r                            # Resolve hostname
ss -4                            # IPv4
ss -6                            # IPv6
ss -x                            # Unix socket
ss -w                            # Raw socket
ss -s                            # Summary statistics

# Filter canggih
ss -tnp state established        # Hanya ESTABLISHED
ss -tnp state listening          # Hanya LISTENING
ss -tnp state time-wait          # TIME-WAIT
ss -tnp state close-wait         # CLOSE-WAIT
ss dst 192.168.1.1               # Ke IP tujuan
ss dport = :80                   # Port tujuan 80
ss sport = :22                   # Port sumber 22
ss -tnp 'dport = :80 or dport = :443'  # Port 80 atau 443
ss -tnp dst 192.168.1.0/24       # Ke subnet

# Contoh berguna
ss -tulpn                        # Listening TCP+UDP dengan proses
ss -antlp                        # Semua TCP + listen + proses
ss -s                            # Ringkasan semua socket
ss -tp state established         # TCP established
ss -antp | grep LISTEN           # Port yang listening
ss -o state established '( dport = :80 or sport = :80 )'  # Koneksi port 80

# Kill socket
ss -K dst 192.168.1.100          # Kill koneksi ke IP
ss -K dport = :80                # Kill koneksi ke port 80
```

---

## 12.6 Log Management

### `journalctl` - Systemd Journal (Lanjutan)
```bash
# Navigasi log
journalctl                       # Semua log
journalctl -e                    # Langsung ke akhir
journalctl -f                    # Follow real-time
journalctl -n 100                # 100 baris terakhir
journalctl -r                    # Terbaru dulu

# Filter waktu
journalctl --since "2024-01-15"
journalctl --since "2024-01-15 10:00:00"
journalctl --until "2024-01-15 12:00:00"
journalctl --since "10:00" --until "12:00"
journalctl --since yesterday
journalctl --since today
journalctl --since "1 hour ago"
journalctl --since "30 minutes ago"
journalctl -b                    # Boot saat ini
journalctl -b -1                 # Boot sebelumnya
journalctl -b -2                 # Dua boot sebelumnya
journalctl --list-boots          # Daftar semua boot

# Filter unit dan service
journalctl -u nginx              # Service nginx
journalctl -u nginx -u mysql     # Beberapa service
journalctl -u nginx -f           # Follow nginx
journalctl -u nginx --since today  # Nginx hari ini
journalctl -u nginx --since "1 hour ago" -n 50  # 1 jam terakhir

# Filter level
journalctl -p err                # Hanya error
journalctl -p warning            # Warning dan lebih tinggi
journalctl -p info               # Info dan lebih tinggi
journalctl -p debug              # Debug (semua)
journalctl -p 0                  # Emergency
journalctl -p 0..3               # Emergency sampai Error
journalctl -p "emerg".."err"     # Range priority

# Level priority:
# 0 = emerg   (emergency)
# 1 = alert
# 2 = crit    (critical)
# 3 = err     (error)
# 4 = warning
# 5 = notice
# 6 = info
# 7 = debug

# Filter berdasarkan field
journalctl _PID=1234             # PID tertentu
journalctl _UID=1000             # UID tertentu
journalctl _GID=1000             # GID tertentu
journalctl _COMM=nginx           # Program tertentu
journalctl _EXE=/usr/bin/nginx   # Executable tertentu
journalctl _SYSTEMD_UNIT=nginx.service  # Unit tertentu
journalctl _HOSTNAME=myserver    # Hostname tertentu
journalctl _TRANSPORT=kernel     # Dari kernel
journalctl _TRANSPORT=syslog     # Dari syslog
journalctl PRIORITY=3            # Priority 3 (err)
journalctl MESSAGE_ID=...        # Message ID tertentu

# Format output
journalctl -o short              # Format pendek (default)
journalctl -o short-precise      # Dengan microsecond
journalctl -o short-monotonic    # Monotonic timestamp
journalctl -o short-full         # Full timestamp + zone
journalctl -o verbose            # Semua field
journalctl -o json               # JSON satu baris
journalctl -o json-pretty        # JSON indented
journalctl -o json-sse           # JSON Server-Sent Events
journalctl -o cat                # Hanya pesan
journalctl -o export             # Binary export

# Manajemen journal
journalctl --disk-usage          # Ukuran journal di disk
sudo journalctl --vacuum-size=500M  # Bersihkan hingga 500MB
sudo journalctl --vacuum-time=30d   # Bersihkan log >30 hari
sudo journalctl --vacuum-files=100  # Batas 100 file journal
sudo journalctl --rotate         # Rotasi journal
sudo journalctl --verify         # Verifikasi integritas
sudo journalctl --flush          # Flush volatile journal ke persistent

# Konfigurasi /etc/systemd/journald.conf
cat /etc/systemd/journald.conf
sudo systemctl restart systemd-journald
```

---

### `tail` - Monitor Log File
```bash
# Monitor log real-time
tail -f /var/log/syslog          # Follow syslog
tail -f /var/log/auth.log        # Follow auth log
tail -f /var/log/nginx/access.log  # Follow nginx
tail -f /var/log/apache2/error.log  # Follow Apache error
tail -F /var/log/syslog          # Follow + retry jika file dirotasi

# Monitor beberapa file
tail -f /var/log/syslog /var/log/auth.log

# Monitor dengan filter
tail -f /var/log/nginx/access.log | grep "ERROR"
tail -f /var/log/syslog | grep -E "error|warning|critical"

# Monitor dengan timestamp
tail -f /var/log/syslog | while read line; do
    echo "$(date '+%Y-%m-%d %H:%M:%S') $line"
done

# multitail (monitor beberapa log dengan tampilan split)
sudo apt install multitail
multitail /var/log/syslog /var/log/auth.log
multitail -i /var/log/nginx/access.log -i /var/log/nginx/error.log
```

---

### `grep` untuk Log Analysis
```bash
# Analisis log dengan grep
grep "ERROR" /var/log/app.log                    # Cari ERROR
grep -i "error" /var/log/app.log                 # Case-insensitive
grep -n "ERROR" /var/log/app.log                 # Dengan nomor baris
grep -c "ERROR" /var/log/app.log                 # Hitung ERROR
grep -v "DEBUG" /var/log/app.log                 # Kecuali DEBUG
grep "ERROR" /var/log/app.log | tail -20         # 20 ERROR terakhir
grep -A 5 "CRITICAL" /var/log/app.log            # 5 baris setelah CRITICAL
grep -B 5 "CRITICAL" /var/log/app.log            # 5 baris sebelum
grep -C 5 "CRITICAL" /var/log/app.log            # 5 baris sekitar

# Pattern kompleks
grep -E "ERROR|WARNING|CRITICAL" /var/log/app.log
grep -E "2024-01-15.*ERROR" /var/log/app.log     # Tanggal + ERROR
grep -E "[0-9]{1,3}\.[0-9]{1,3}" /var/log/nginx/access.log  # IP address

# Statistik dari log
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -rn | head -10
grep "404" /var/log/nginx/access.log | awk '{print $7}' | sort | uniq -c | sort -rn
```

---

### `logrotate` - Log Rotation
```bash
# Konfigurasi logrotate
cat /etc/logrotate.conf
ls /etc/logrotate.d/

# Contoh konfigurasi custom
cat > /etc/logrotate.d/myapp << 'EOF'
/var/log/myapp/*.log {
    daily                        # Rotasi harian
    rotate 7                     # Simpan 7 file lama
    compress                     # Kompres file lama
    delaycompress                # Tunda kompres 1 hari
    missingok                    # Tidak error jika file tidak ada
    notifempty                   # Jangan rotasi jika kosong
    create 0640 www-data adm    # Buat file baru dengan permission ini
    sharedscripts                # Jalankan script sekali untuk semua file
    postrotate
        # Reload aplikasi setelah rotasi
        systemctl reload nginx > /dev/null 2>&1 || true
    endscript
}
EOF

# Test konfigurasi
sudo logrotate -d /etc/logrotate.d/myapp    # Debug (dry run)
sudo logrotate -f /etc/logrotate.d/myapp    # Force rotasi sekarang
sudo logrotate -v /etc/logrotate.conf       # Verbose

# Status logrotate
cat /var/lib/logrotate/status

# Opsi logrotate
daily/weekly/monthly/yearly     # Frekuensi rotasi
rotate N                        # Simpan N file
size 100M                       # Rotasi jika ukuran > 100MB
maxsize 200M                    # Ukuran maksimum
minsize 50M                     # Ukuran minimum untuk rotasi
compress                        # Kompres dengan gzip
compresscmd xz                  # Gunakan xz untuk kompres
compressext .xz                 # Ekstensi file terkompresi
dateext                         # Tambahkan tanggal ke nama file
dateformat -%Y%m%d              # Format tanggal
olddir /archive/logs            # Simpan log lama di sini
mail admin@example.com          # Email log lama
nomail                          # Jangan email
prerotate/endscript             # Script sebelum rotasi
postrotate/endscript            # Script setelah rotasi
firstaction/endscript           # Script sebelum semua rotasi
lastaction/endscript            # Script setelah semua rotasi
copytruncate                    # Copy lalu truncate (untuk file yang terus ditulis)
nocopytruncate                  # Default: rename
nocreate                        # Jangan buat file baru
ifempty                         # Rotasi meski file kosong
su root root                    # Jalankan sebagai user/group ini
```

---

### `syslog` - System Log
```bash
# File log utama
/var/log/syslog                 # Log umum sistem (Debian/Ubuntu)
/var/log/messages               # Log umum (RHEL/CentOS)
/var/log/auth.log               # Autentikasi (Debian/Ubuntu)
/var/log/secure                 # Autentikasi (RHEL/CentOS)
/var/log/kern.log               # Kernel log
/var/log/dmesg                  # Boot messages
/var/log/boot.log               # Boot log
/var/log/dpkg.log               # Package manager log (Debian)
/var/log/apt/                   # APT log
/var/log/yum.log                # YUM log
/var/log/dnf.log                # DNF log
/var/log/cron                   # Cron log
/var/log/mail.log               # Mail log
/var/log/nginx/                 # Nginx logs
/var/log/apache2/               # Apache logs
/var/log/mysql/                 # MySQL logs
/var/log/postgresql/            # PostgreSQL logs

# rsyslog konfigurasi
cat /etc/rsyslog.conf
ls /etc/rsyslog.d/

# Kirim log ke server remote
cat >> /etc/rsyslog.conf << 'EOF'
# Kirim semua log ke server remote
*.* @192.168.1.100:514          # UDP
*.* @@192.168.1.100:514         # TCP
EOF

sudo systemctl restart rsyslog

# Logger - kirim pesan ke syslog
logger "Ini pesan test"
logger -t "myapp" "Aplikasi dimulai"
logger -p user.info "Info message"
logger -p user.err "Error message"
logger -p local0.info "Custom facility"
logger -i "PID dalam pesan"
logger -f /path/file.txt        # Kirim isi file
echo "pesan" | logger -t "app"  # Via pipe
```

---

## 12.7 Application Log Monitoring

### Monitoring Nginx Logs
```bash
# Real-time monitoring
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log

# Analisis access log
# Top 10 IP address
awk '{print $1}' /var/log/nginx/access.log | \
    sort | uniq -c | sort -rn | head -10

# Top 10 URL yang diakses
awk '{print $7}' /var/log/nginx/access.log | \
    sort | uniq -c | sort -rn | head -10

# Top 10 User-Agent
awk -F'"' '{print $6}' /var/log/nginx/access.log | \
    sort | uniq -c | sort -rn | head -10

# Status code breakdown
awk '{print $9}' /var/log/nginx/access.log | \
    sort | uniq -c | sort -rn

# Request per jam
awk '{print $4}' /var/log/nginx/access.log | \
    cut -d: -f2 | sort | uniq -c

# Ukuran response rata-rata
awk '{sum+=$10; count++} END {print sum/count}' /var/log/nginx/access.log

# Error 500
grep ' 500 ' /var/log/nginx/access.log | tail -20

# Request dalam 1 menit terakhir
awk -v date="$(date '+%d/%b/%Y:%H:%M')" '$4 ~ date' \
    /var/log/nginx/access.log | wc -l

# GoAccess - analisis log real-time
sudo apt install goaccess
goaccess /var/log/nginx/access.log \
    --log-format=COMBINED -o report.html
goaccess /var/log/nginx/access.log \
    --log-format=COMBINED --real-time-html -o /var/www/html/report.html
```

---

### Monitoring Apache Logs
```bash
# Real-time monitoring
tail -f /var/log/apache2/access.log
tail -f /var/log/apache2/error.log

# Analisis
awk '{print $1}' /var/log/apache2/access.log | \
    sort | uniq -c | sort -rn | head -10

# Top URL
awk '{print $7}' /var/log/apache2/access.log | \
    sort | uniq -c | sort -rn | head -10

# Error terakhir
grep "\[error\]" /var/log/apache2/error.log | tail -20
```

---

### Monitoring MySQL Logs
```bash
# Aktifkan slow query log
mysql -u root -p << 'EOF'
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL slow_query_log_file = '/var/log/mysql/slow.log';
SET GLOBAL long_query_time = 1;
EOF

# Monitor slow queries
tail -f /var/log/mysql/slow.log

# mysqldumpslow - analisis slow query
mysqldumpslow /var/log/mysql/slow.log          # Ringkasan
mysqldumpslow -s t /var/log/mysql/slow.log     # Sort by time
mysqldumpslow -s c -t 10 /var/log/mysql/slow.log  # Top 10 by count

# pt-query-digest (Percona)
sudo apt install percona-toolkit
pt-query-digest /var/log/mysql/slow.log

# Binary log
mysqlbinlog /var/lib/mysql/mysql-bin.000001
mysqlbinlog --start-datetime="2024-01-15 10:00:00" \
    --stop-datetime="2024-01-15 12:00:00" \
    /var/lib/mysql/mysql-bin.000001
```

---

## 12.8 Alerting & Notification

### Monitoring dengan Script
```bash
#!/bin/bash
# monitor.sh - Script monitoring sederhana

# Konfigurasi
CPU_THRESHOLD=90
MEM_THRESHOLD=90
DISK_THRESHOLD=90
EMAIL="admin@example.com"
LOG_FILE="/var/log/monitor.log"

log() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') $*" | tee -a "$LOG_FILE"
}

send_alert() {
    local subject="$1"
    local message="$2"
    echo "$message" | mail -s "$subject" "$EMAIL"
    log "ALERT: $subject"
}

# Cek CPU
check_cpu() {
    local cpu_usage=$(mpstat 1 1 | awk '/Average/ {print 100 - $NF}' | \
        awk '{printf "%.0f", $1}')
    if [ "$cpu_usage" -gt "$CPU_THRESHOLD" ]; then
        send_alert "CPU Tinggi: ${cpu_usage}%" \
            "CPU usage mencapai ${cpu_usage}% di $(hostname)"
    fi
    log "CPU: ${cpu_usage}%"
}

# Cek Memory
check_memory() {
    local mem_usage=$(free | awk '/Mem/ {printf "%.0f", $3/$2 * 100}')
    if [ "$mem_usage" -gt "$MEM_THRESHOLD" ]; then
        send_alert "Memory Tinggi: ${mem_usage}%" \
            "Memory usage mencapai ${mem_usage}% di $(hostname)"
    fi
    log "Memory: ${mem_usage}%"
}

# Cek Disk
check_disk() {
    df -h | grep -vE '^Filesystem|tmpfs|cdrom' | \
    awk '{print $5 " " $6}' | while read output; do
        local usage=$(echo "$output" | awk '{print $1}' | cut -d'%' -f1)
        local partition=$(echo "$output" | awk '{print $2}')
        if [ "$usage" -gt "$DISK_THRESHOLD" ]; then
            send_alert "Disk Penuh: ${partition} ${usage}%" \
                "Disk $partition mencapai ${usage}% di $(hostname)"
        fi
        log "Disk $partition: ${usage}%"
    done
}

# Cek Service
check_service() {
    local services=("nginx" "mysql" "redis")
    for service in "${services[@]}"; do
        if ! systemctl is-active --quiet "$service"; then
            send_alert "Service Down: $service" \
                "Service $service tidak berjalan di $(hostname)"
            log "Service $service: DOWN"
        else
            log "Service $service: OK"
        fi
    done
}

# Cek Konektivitas
check_connectivity() {
    local hosts=("google.com" "8.8.8.8" "internal-server")
    for host in "${hosts[@]}"; do
        if ! ping -c 1 -W 3 "$host" &>/dev/null; then
            send_alert "Host Tidak Terjangkau: $host" \
                "Tidak bisa ping $host dari $(hostname)"
            log "Ping $host: FAIL"
        else
            log "Ping $host: OK"
        fi
    done
}

# Jalankan semua check
log "=== Monitoring dimulai ==="
check_cpu
check_memory
check_disk
check_service
check_connectivity
log "=== Monitoring selesai ==="
```

---

### `monit` - Service Monitor
```bash
sudo apt install monit

# Konfigurasi /etc/monit/monitrc atau /etc/monit/conf.d/
cat > /etc/monit/conf.d/nginx.conf << 'EOF'
check process nginx with pidfile /run/nginx.pid
    start program = "/bin/systemctl start nginx"
    stop program = "/bin/systemctl stop nginx"
    if failed host localhost port 80 protocol http then restart
    if 5 restarts within 5 cycles then timeout
    alert admin@example.com
EOF

cat > /etc/monit/conf.d/system.conf << 'EOF'
check system myserver
    if loadavg (1min) > 4 then alert
    if loadavg (5min) > 3 then alert
    if cpu usage > 90% for 5 cycles then alert
    if memory usage > 85% then alert
    if swap usage > 25% then alert
    alert admin@example.com
EOF

# Manajemen monit
sudo monit start                 # Start monit
sudo monit stop                  # Stop monit
sudo monit restart               # Restart monit
sudo monit status                # Status semua service
sudo monit status nginx          # Status nginx
sudo monit reload                # Reload konfigurasi
sudo monit -t                    # Test konfigurasi
sudo monit summary               # Ringkasan
sudo monit start nginx           # Start service via monit
sudo monit stop nginx            # Stop service via monit
sudo monit restart nginx         # Restart service
monit -v                         # Verbose

# Web interface (jika dikonfigurasi)
# http://localhost:2812
```

---

### `nagios` / `icinga` - Network Monitoring
```bash
# Install Icinga2 (pengganti Nagios yang modern)
sudo apt install icinga2

# Check commands
icinga2 daemon -C               # Check konfigurasi
icinga2 feature list            # Daftar fitur
icinga2 feature enable graphite # Aktifkan graphite
sudo systemctl restart icinga2

# Plugin nagios (compatible dengan icinga)
sudo apt install monitoring-plugins-basic

# Test plugin manual
/usr/lib/nagios/plugins/check_http -H google.com
/usr/lib/nagios/plugins/check_disk -w 80% -c 90% -p /
/usr/lib/nagios/plugins/check_load -w 4,3,2 -c 6,5,4
/usr/lib/nagios/plugins/check_memory -w 80 -c 90
/usr/lib/nagios/plugins/check_procs -c 300 -s Z  # Zombie processes
/usr/lib/nagios/plugins/check_ssh -H hostname
/usr/lib/nagios/plugins/check_ping -H google.com -w 100,5% -c 200,10%
```

---

## 12.9 Centralized Logging

### ELK Stack (Elasticsearch, Logstash, Kibana)
```bash
# Install Elasticsearch
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | \
    sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] \
    https://artifacts.elastic.co/packages/8.x/apt stable main" | \
    sudo tee /etc/apt/sources.list.d/elastic-8.x.list
sudo apt update && sudo apt install elasticsearch

# Konfigurasi elasticsearch
sudo nano /etc/elasticsearch/elasticsearch.yml
# network.host: 0.0.0.0
# discovery.seed_hosts: []
# cluster.initial_master_nodes: ["node-1"]

sudo systemctl start elasticsearch
sudo systemctl enable elasticsearch

# Test elasticsearch
curl -X GET "localhost:9200"
curl -X GET "localhost:9200/_cluster/health?pretty"
curl -X GET "localhost:9200/_cat/indices?v"

# Install Logstash
sudo apt install logstash

# Konfigurasi Logstash pipeline
cat > /etc/logstash/conf.d/nginx.conf << 'EOF'
input {
    file {
        path => "/var/log/nginx/access.log"
        start_position => "beginning"
        type => "nginx_access"
    }
}

filter {
    if [type] == "nginx_access" {
        grok {
            match => { "message" => "%{COMBINEDAPACHELOG}" }
        }
        date {
            match => [ "timestamp", "dd/MMM/yyyy:HH:mm:ss Z" ]
        }
        geoip {
            source => "clientip"
        }
    }
}

output {
    elasticsearch {
        hosts => ["localhost:9200"]
        index => "nginx-access-%{+YYYY.MM.dd}"
    }
}
EOF

sudo systemctl start logstash
sudo systemctl enable logstash

# Install Kibana
sudo apt install kibana
sudo nano /etc/kibana/kibana.yml
# server.host: "0.0.0.0"
# elasticsearch.hosts: ["http://localhost:9200"]

sudo systemctl start kibana
sudo systemctl enable kibana
# Akses: http://localhost:5601
```

---

### Filebeat - Log Shipper
```bash
# Install Filebeat
sudo apt install filebeat

# Konfigurasi /etc/filebeat/filebeat.yml
cat > /etc/filebeat/filebeat.yml << 'EOF'
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /var/log/*.log
    - /var/log/nginx/*.log

output.elasticsearch:
  hosts: ["localhost:9200"]
  index: "filebeat-%{[agent.version]}-%{+yyyy.MM.dd}"

setup.kibana:
  host: "localhost:5601"
EOF

# Enable modules
filebeat modules list            # Daftar modul
filebeat modules enable nginx    # Aktifkan modul nginx
filebeat modules enable system   # Aktifkan modul system
filebeat modules enable mysql    # Aktifkan modul mysql

# Setup dan test
sudo filebeat setup -e           # Setup index dan dashboard
sudo filebeat test config        # Test konfigurasi
sudo filebeat test output        # Test koneksi output

sudo systemctl start filebeat
sudo systemctl enable filebeat
```

---

### Grafana - Visualization
```bash
# Install Grafana
sudo apt install -y apt-transport-https software-properties-common
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
echo "deb https://packages.grafana.com/oss/deb stable main" | \
    sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt update && sudo apt install grafana

sudo systemctl start grafana-server
sudo systemctl enable grafana-server
# Akses: http://localhost:3000 (admin/admin)

# Grafana CLI
grafana-cli plugins list-remote           # Plugin yang tersedia
grafana-cli plugins install grafana-piechart-panel  # Install plugin
grafana-cli plugins ls                    # Plugin terinstal
grafana-cli admin reset-admin-password newpass  # Reset password
```

---

### Prometheus - Metrics Collection
```bash
# Install Prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.45.0/prometheus-2.45.0.linux-amd64.tar.gz
tar xvf prometheus-*.tar.gz
sudo mv prometheus-*/prometheus /usr/local/bin/
sudo mv prometheus-*/promtool /usr/local/bin/

# Konfigurasi
cat > /etc/prometheus/prometheus.yml << 'EOF'
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']

  - job_name: 'nginx'
    static_configs:
      - targets: ['localhost:9113']
EOF

# Jalankan
sudo systemctl start prometheus
# Akses: http://localhost:9090

# Node Exporter (metrics sistem)
wget https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz
tar xvf node_exporter-*.tar.gz
sudo mv node_exporter-*/node_exporter /usr/local/bin/
sudo systemctl start node_exporter
# Akses: http://localhost:9100

# PromQL - Query bahasa Prometheus
# Di browser: http://localhost:9090/graph
# Contoh query:
# node_cpu_seconds_total              → CPU total
# rate(node_cpu_seconds_total[5m])    → CPU rate 5 menit
# node_memory_MemFree_bytes           → Free memory
# node_filesystem_free_bytes          → Disk free
# up                                  → Target yang up
# http_requests_total                 → HTTP requests
```

---

## 12.10 Log Analysis Tools

### `lnav` - Log File Navigator
```bash
sudo apt install lnav

lnav /var/log/syslog             # Buka log
lnav /var/log/*.log              # Beberapa file
lnav /var/log/nginx/access.log   # Nginx log

# Shortcut dalam lnav
/          # Search
q          # Keluar
TAB        # Autocomplete filter
;          # SQL query
:          # Perintah lnav
p          # Toggle parser
i          # Histogram view
?          # Bantuan
e          # Error messages
w          # Warning messages
Ctrl+W     # Toggle word wrap
F          # Field bisect
t          # Top of log
T          # Timestamp jump
0-9        # Bookmark
```

---

### `multitail` - Multiple Log Tails
```bash
sudo apt install multitail

multitail /var/log/syslog /var/log/auth.log
multitail -i /var/log/nginx/access.log -i /var/log/nginx/error.log
multitail -s 2 /var/log/syslog /var/log/auth.log  # 2 kolom
multitail --mergeall /var/log/*.log  # Gabungkan semua

# Shortcut dalam multitail
b          # Scroll up
q          # Keluar satu window
Q          # Keluar semua
a          # Tambah file
d          # Hapus window
f          # Regex filter
/          # Cari
n          # Cari berikutnya
```

---

### `goaccess` - Web Log Analyzer
```bash
sudo apt install goaccess

# Analisis static
goaccess /var/log/nginx/access.log --log-format=COMBINED

# Output HTML
goaccess /var/log/nginx/access.log \
    --log-format=COMBINED \
    -o /var/www/html/report.html

# Real-time HTML dashboard
goaccess /var/log/nginx/access.log \
    --log-format=COMBINED \
    --real-time-html \
    --ws-url=ws://server:7890 \
    -o /var/www/html/report.html &

# Dari stdin
zcat /var/log/nginx/access.log.*.gz | \
    goaccess --log-format=COMBINED -o report.html -

# Custom log format
goaccess access.log \
    --log-format='%h %^[%d:%t %^] "%r" %s %b "%R" "%u"' \
    --date-format='%d/%b/%Y' \
    --time-format='%H:%M:%S'
```

---

## Ringkasan Perintah Bagian 12

```
System Monitoring Real-time:
  top/htop         → Monitor proses real-time
  glances          → Monitor all-in-one
  nmon             → Performance monitor
  dstat            → Resource statistics
  sar              → System activity reporter

CPU Monitoring:
  mpstat           → CPU per core statistics
  pidstat          → Statistik per proses
  perf             → Performance analysis

Memory Monitoring:
  free             → Penggunaan RAM & swap
  smem             → Memory per proses detail
  /proc/meminfo    → Detail info memori kernel

Disk & I/O Monitoring:
  iostat           → I/O statistics
  iotop            → I/O per proses
  ioping           → I/O latency
  blktrace         → Block layer tracing

Network Monitoring:
  nethogs          → Bandwidth per proses
  iftop            → Network bandwidth
  vnstat           → Traffic statistics
  bmon             → Bandwidth monitor
  ss               → Socket statistics

Log Management:
  journalctl       → Systemd journal
  tail -f          → Follow log file
  logrotate        → Rotasi log
  logger           → Kirim ke syslog
  lnav             → Log navigator
  multitail        → Multiple log monitor
  goaccess         → Web log analyzer

Alerting:
  monit            → Service monitoring & alerting
  nagios/icinga    → Network monitoring

Centralized Logging:
  filebeat         → Log shipper
  elasticsearch    → Log storage & search
  logstash         → Log processing
  kibana           → Log visualization
  prometheus       → Metrics collection
  grafana          → Metrics visualization
```

---

## ✅ Bagian 12 Selesai!

**Lanjut ke Bagian 13: Perintah Pencarian (Search)?**

Ketik:
- ✅ **"Lanjut"** → Ke Bagian 13
- 🔄 **"Ulangi"** → Ulangi Bagian 12
- 🎯 **"Langsung ke Bagian X"** → Lompat ke bagian tertentu
