# Bagian 4: Perintah Manajemen Proses

---

## 4.1 Melihat Proses

### `ps` - Process Status
```bash
ps [opsi]
```
> Menampilkan informasi proses yang sedang berjalan

**Opsi Dasar:**
| Opsi | Penjelasan |
|---|---|
| `ps` | Tampilkan proses milik user saat ini di terminal ini |
| `ps -e` atau `ps -A` | Tampilkan semua proses |
| `ps -f` | Format penuh (full format) |
| `ps -l` | Format panjang (long format) |
| `ps -u username` | Proses milik user tertentu |
| `ps -p 1234` | Proses dengan PID tertentu |
| `ps -C nginx` | Proses dengan nama tertentu |
| `ps -t pts/0` | Proses di terminal tertentu |

**Kombinasi Populer:**
| Perintah | Penjelasan |
|---|---|
| `ps aux` | Tampilkan semua proses (format BSD) |
| `ps -ef` | Tampilkan semua proses (format UNIX) |
| `ps -elf` | Format panjang semua proses |
| `ps aux --sort=-%cpu` | Urutkan berdasarkan CPU (tertinggi) |
| `ps aux --sort=-%mem` | Urutkan berdasarkan memori (tertinggi) |
| `ps aux --sort=pid` | Urutkan berdasarkan PID |
| `ps -eo pid,ppid,cmd,%cpu,%mem` | Kolom kustom |
| `ps -eo pid,ppid,user,cmd --forest` | Tampilkan hierarki proses |
| `ps aux \| grep nginx` | Cari proses tertentu |
| `ps -u root -u user` | Proses dari beberapa user |
| `ps -fp $(pgrep nginx)` | Info lengkap proses nginx |

**Kolom Output ps aux:**
```
USER    → Pemilik proses
PID     → Process ID
%CPU    → Penggunaan CPU
%MEM    → Penggunaan memori
VSZ     → Virtual memory size (KB)
RSS     → Resident set size / memori fisik (KB)
TTY     → Terminal yang digunakan
STAT    → Status proses
START   → Waktu proses dimulai
TIME    → Total waktu CPU yang digunakan
COMMAND → Perintah yang menjalankan proses
```

**Status Proses (STAT):**
```
R → Running (sedang berjalan)
S → Sleeping (tidur, menunggu event)
D → Uninterruptible sleep (menunggu I/O)
T → Stopped (dihentikan)
Z → Zombie (sudah selesai, belum di-reap parent)
W → Paging
X → Dead
< → High priority
N → Low priority
L → Terkunci di memori
s → Session leader
l → Multi-threaded
+ → Di foreground process group
```

**Contoh Output ps aux:**
```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0 225948  9380 ?        Ss   09:00   0:01 /sbin/init
user      1234  0.5  1.2 987654 24680 pts/0    S    10:00   0:05 bash
user      5678  2.0  3.5 456789 71234 pts/0    R    10:30   0:15 python3 app.py
```

---

### `top` - Dynamic Process Viewer
```bash
top [opsi]
```
> Menampilkan proses secara real-time dan dinamis

**Opsi Awal:**
| Opsi | Penjelasan |
|---|---|
| `top` | Buka top |
| `top -d 2` | Refresh setiap 2 detik |
| `top -n 5` | Refresh 5 kali lalu keluar |
| `top -p 1234` | Monitor PID tertentu |
| `top -p 1234,5678` | Monitor beberapa PID |
| `top -u username` | Tampilkan proses user tertentu |
| `top -b` | Batch mode (output ke file/pipe) |
| `top -b -n 1 > proses.txt` | Simpan output ke file |
| `top -c` | Tampilkan command line lengkap |
| `top -H` | Tampilkan threads |
| `top -i` | Abaikan proses idle |

**Shortcut di dalam top:**
| Tombol | Penjelasan |
|---|---|
| **q** | Keluar dari top |
| **h** | Bantuan |
| **Space** | Refresh manual |
| **k** | Kill proses (masukkan PID) |
| **r** | Renice proses (ubah prioritas) |
| **u** | Filter berdasarkan user |
| **M** | Urutkan berdasarkan memori |
| **P** | Urutkan berdasarkan CPU |
| **T** | Urutkan berdasarkan waktu |
| **N** | Urutkan berdasarkan PID |
| **R** | Balikkan urutan |
| **1** | Tampilkan semua CPU core |
| **c** | Toggle tampilan command |
| **V** | Toggle tampilan hierarki (forest) |
| **H** | Toggle tampilan threads |
| **i** | Toggle proses idle |
| **l** | Toggle tampilan load average |
| **t** | Toggle tampilan CPU info |
| **m** | Toggle tampilan memori |
| **d** | Ubah interval refresh |
| **s** | Ubah interval refresh |
| **f** | Pilih kolom yang ditampilkan |
| **o** | Filter proses |
| **W** | Simpan konfigurasi |
| **z** | Toggle warna |
| **x** | Highlight kolom urutan |
| **b** | Bold/highlight |
| **<** | Pindah kolom urut ke kiri |
| **>** | Pindah kolom urut ke kanan |

**Area Header top:**
```
top - 10:30:00 up 2 days,  3:45,  2 users,  load average: 0.15, 0.10, 0.09
Tasks: 213 total,   1 running, 212 sleeping,   0 stopped,   0 zombie
%Cpu(s):  5.0 us,  2.0 sy,  0.0 ni, 92.0 id,  1.0 wa,  0.0 hi,  0.0 si
MiB Mem :  16384.0 total,   8192.0 free,   4096.0 used,   4096.0 buff/cache
MiB Swap:   4096.0 total,   4096.0 free,      0.0 used.  11264.0 avail Mem

Keterangan CPU:
us = user space
sy = kernel/system
ni = nice (low priority)
id = idle
wa = I/O wait
hi = hardware interrupt
si = software interrupt
st = steal (di VM)
```

---

### `htop` - Interactive Process Viewer
```bash
htop [opsi]
```
> Versi top yang lebih interaktif dan visual

> 📦 Perlu instalasi: `sudo apt install htop`

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `htop` | Buka htop |
| `htop -d 10` | Refresh setiap 1 detik (10 = 10 decis) |
| `htop -u username` | Filter user tertentu |
| `htop -p 1234,5678` | Monitor PID tertentu |
| `htop -t` | Tampilkan thread |
| `htop -s PERCENT_CPU` | Urutkan berdasarkan CPU |
| `htop --no-color` | Tanpa warna |

**Shortcut di dalam htop:**
| Tombol | Penjelasan |
|---|---|
| **F1** | Bantuan |
| **F2** | Setup/konfigurasi |
| **F3** | Cari proses |
| **F4** | Filter proses |
| **F5** | Tampilan tree |
| **F6** | Pilih kolom urut |
| **F7** | Kurangi nice (prioritas lebih tinggi) |
| **F8** | Tambah nice (prioritas lebih rendah) |
| **F9** | Kill proses (pilih sinyal) |
| **F10** | Keluar |
| **Space** | Tandai proses |
| **U** | Hapus semua tanda |
| **u** | Filter berdasarkan user |
| **k** | Kill proses |
| **t** | Toggle tree view |
| **H** | Toggle user threads |
| **K** | Toggle kernel threads |
| **I** | Balikkan urutan |
| **+/-** | Expand/collapse tree |
| **/** | Cari |
| **\\** | Filter |

---

### `atop` - Advanced System Monitor
```bash
atop [opsi] [interval]
```
> Monitor sistem yang sangat detail (CPU, memori, disk, jaringan)

> 📦 Perlu instalasi: `sudo apt install atop`

```bash
atop              # Jalankan atop
atop 5            # Refresh setiap 5 detik
atop -d           # Tampilkan info disk
atop -n           # Tampilkan info jaringan
atop -m           # Tampilkan info memori
atop -c           # Tampilkan command line
atop -r /var/log/atop/atop_20240115  # Baca file log
```

---

### `pgrep` - Find Process by Name
```bash
pgrep [opsi] <pola>
```
> Mencari PID berdasarkan nama proses

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `pgrep nginx` | Tampilkan PID proses nginx |
| `pgrep -l nginx` | Tampilkan PID + nama proses |
| `pgrep -a nginx` | Tampilkan PID + command line lengkap |
| `pgrep -u username` | Proses milik user tertentu |
| `pgrep -u username nginx` | Proses nginx milik user tertentu |
| `pgrep -x nginx` | Cocokkan nama persis (exact match) |
| `pgrep -f "python script.py"` | Cocokkan command line lengkap |
| `pgrep -n nginx` | Proses nginx yang paling baru |
| `pgrep -o nginx` | Proses nginx yang paling lama |
| `pgrep -c nginx` | Hitung jumlah proses nginx |
| `pgrep -P 1234` | Proses anak dari PID 1234 |
| `pgrep -t pts/0` | Proses di terminal tertentu |
| `pgrep -d, nginx` | Gunakan koma sebagai delimiter |

---

### `pidof` - Find PID of Program
```bash
pidof [opsi] <nama_program>
```
> Menemukan PID dari program yang sedang berjalan

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `pidof nginx` | PID semua proses nginx |
| `pidof -s nginx` | Hanya satu PID (single) |
| `pidof -x script.sh` | Termasuk script shell |
| `pidof -o 1234 nginx` | Abaikan PID tertentu |

---

### `pstree` - Display Process Tree
```bash
pstree [opsi] [PID|username]
```
> Menampilkan proses dalam bentuk pohon hierarki

> 📦 Perlu instalasi: `sudo apt install psmisc`

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `pstree` | Tampilkan pohon semua proses |
| `pstree -p` | Tampilkan dengan PID |
| `pstree -u` | Tampilkan dengan nama user |
| `pstree -a` | Tampilkan command line lengkap |
| `pstree -h` | Highlight proses saat ini dan induknya |
| `pstree -H 1234` | Highlight PID tertentu |
| `pstree -l` | Jangan potong baris panjang |
| `pstree -n` | Urutkan berdasarkan PID |
| `pstree -g` | Tampilkan PGID |
| `pstree -s 1234` | Tampilkan proses induk dari PID |
| `pstree 1234` | Pohon dari PID tertentu |
| `pstree username` | Pohon proses milik user |

**Contoh Output:**
```
systemd─┬─NetworkManager───2*[{NetworkManager}]
        ├─apache2───5*[apache2]
        ├─cron
        ├─nginx───4*[nginx]
        ├─sshd───sshd───bash───pstree
        └─systemd───(sd-pam)
```

---

## 4.2 Mengelola Proses

### `kill` - Send Signal to Process
```bash
kill [opsi] <PID>
```
> Mengirim sinyal ke proses

**Daftar Sinyal Penting:**
| Sinyal | Nomor | Penjelasan |
|---|---|---|
| `SIGHUP` | 1 | Hangup - reload konfigurasi |
| `SIGINT` | 2 | Interrupt (Ctrl+C) |
| `SIGQUIT` | 3 | Quit (Ctrl+\\) |
| `SIGKILL` | 9 | Kill paksa (tidak bisa diabaikan) |
| `SIGTERM` | 15 | Terminate (graceful shutdown) - default |
| `SIGSTOP` | 19 | Stop proses (tidak bisa diabaikan) |
| `SIGCONT` | 18 | Lanjutkan proses yang di-stop |
| `SIGUSR1` | 10 | User-defined signal 1 |
| `SIGUSR2` | 12 | User-defined signal 2 |
| `SIGWINCH` | 28 | Window size changed |

**Opsi:**
| Perintah | Penjelasan |
|---|---|
| `kill 1234` | Kirim SIGTERM ke PID 1234 (graceful) |
| `kill -9 1234` | Kirim SIGKILL ke PID 1234 (paksa) |
| `kill -15 1234` | Kirim SIGTERM (eksplisit) |
| `kill -SIGKILL 1234` | Menggunakan nama sinyal |
| `kill -HUP 1234` | Reload konfigurasi |
| `kill -l` | Tampilkan semua sinyal |
| `kill -l SIGKILL` | Tampilkan nomor sinyal tertentu |
| `kill -0 1234` | Cek apakah proses ada (tanpa kirim sinyal) |
| `kill -STOP 1234` | Stop proses |
| `kill -CONT 1234` | Lanjutkan proses |

---

### `killall` - Kill Processes by Name
```bash
killall [opsi] <nama_proses>
```
> Menghentikan semua proses dengan nama tertentu

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `killall nginx` | Kill semua proses nginx (SIGTERM) |
| `killall -9 nginx` | Kill paksa semua proses nginx |
| `killall -HUP nginx` | Reload semua proses nginx |
| `killall -i nginx` | Konfirmasi sebelum kill |
| `killall -u username` | Kill semua proses milik user |
| `killall -u username nginx` | Kill nginx milik user tertentu |
| `killall -e nginx` | Cocokkan nama persis |
| `killall -r "ngin.*"` | Cocokkan dengan regex |
| `killall -w nginx` | Tunggu sampai proses benar-benar mati |
| `killall -q nginx` | Quiet (tidak ada output error) |
| `killall -v nginx` | Verbose |
| `killall -s SIGUSR1 nginx` | Gunakan sinyal tertentu |
| `killall -o 10m nginx` | Kill proses lebih dari 10 menit |
| `killall -y 10m nginx` | Kill proses kurang dari 10 menit |

---

### `pkill` - Kill Processes by Pattern
```bash
pkill [opsi] <pola>
```
> Menghentikan proses berdasarkan pola nama

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `pkill nginx` | Kill proses nginx |
| `pkill -9 nginx` | Kill paksa |
| `pkill -HUP nginx` | Kirim SIGHUP |
| `pkill -u username` | Kill proses milik user tertentu |
| `pkill -u username nginx` | Kill nginx milik user tertentu |
| `pkill -x nginx` | Cocokkan nama persis |
| `pkill -f "python script.py"` | Cocokkan command line lengkap |
| `pkill -n nginx` | Kill proses nginx yang paling baru |
| `pkill -o nginx` | Kill proses nginx yang paling lama |
| `pkill -P 1234` | Kill semua proses anak dari PID 1234 |
| `pkill -t pts/0` | Kill proses di terminal tertentu |
| `pkill -e nginx` | Verbose (tampilkan yang di-kill) |

---

### `nice` - Run Process with Priority
```bash
nice [opsi] <perintah>
```
> Menjalankan proses dengan prioritas tertentu

**Konsep Nice Value:**
```
Nice value range: -20 (tertinggi) hingga 19 (terendah)
Default nice value: 0
Semakin rendah nilai = prioritas lebih tinggi
Hanya root yang bisa set nilai negatif (prioritas tinggi)
```

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `nice perintah` | Jalankan dengan nice value 10 (default) |
| `nice -n 10 perintah` | Jalankan dengan nice value 10 |
| `nice -n -5 perintah` | Jalankan dengan nice value -5 (perlu root) |
| `nice -n 19 perintah` | Prioritas terendah |
| `nice -n -20 perintah` | Prioritas tertinggi (perlu root) |
| `nice -10 perintah` | Shorthand untuk -n 10 |

```bash
# Contoh penggunaan
nice -n 10 ./backup.sh          # Backup dengan prioritas rendah
nice -n 19 find / -name "*.log" # Pencarian prioritas sangat rendah
sudo nice -n -10 ./critical.sh  # Proses kritis prioritas tinggi
```

---

### `renice` - Change Priority of Running Process
```bash
renice [opsi] <nilai> <PID|user|pgrp>
```
> Mengubah prioritas proses yang sedang berjalan

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `renice 10 -p 1234` | Ubah nice value PID 1234 menjadi 10 |
| `renice -5 -p 1234` | Ubah menjadi -5 (perlu root) |
| `renice 10 -u username` | Ubah semua proses milik user |
| `renice 10 -g 1234` | Ubah semua proses dalam process group |
| `renice +5 -p 1234` | Tambah 5 dari nice value saat ini |

---

### `nohup` - Run Command Immune to Hangups
```bash
nohup <perintah> [argumen] &
```
> Menjalankan proses yang tetap berjalan meski terminal ditutup

**Opsi:**
| Perintah | Penjelasan |
|---|---|
| `nohup ./script.sh &` | Jalankan script di background, tahan HUP |
| `nohup python3 app.py &` | Jalankan Python app |
| `nohup ./script.sh > output.log 2>&1 &` | Dengan redirect output |
| `nohup ./script.sh > /dev/null 2>&1 &` | Buang semua output |

```bash
# Output default disimpan di nohup.out
nohup ./backup.sh &
# [1] 12345
# nohup: ignoring input and appending output to 'nohup.out'

# Cek proses
jobs
# [1]+ Running   nohup ./backup.sh &

# Lihat output
tail -f nohup.out
```

---

### `disown` - Remove Job from Shell
```bash
disown [opsi] [job_id]
```
> Melepaskan job dari shell sehingga tidak terpengaruh jika shell ditutup

```bash
./script.sh &          # Jalankan di background
disown                 # Lepas job terakhir dari shell
disown %1              # Lepas job nomor 1
disown -h %1           # Tandai agar tidak menerima SIGHUP
disown -a              # Lepas semua job
disown -r              # Lepas semua job yang running
```

---

## 4.3 Job Control

### `jobs` - List Background Jobs
```bash
jobs [opsi]
```
> Menampilkan daftar job yang berjalan di background

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `jobs` | Tampilkan semua job |
| `jobs -l` | Tampilkan dengan PID |
| `jobs -p` | Tampilkan hanya PID |
| `jobs -r` | Tampilkan hanya job yang running |
| `jobs -s` | Tampilkan hanya job yang stopped |
| `jobs -n` | Tampilkan hanya job yang statusnya berubah |

**Status Job:**
```
Running  → Sedang berjalan di background
Stopped  → Dihentikan sementara (Ctrl+Z)
Done     → Selesai
Killed   → Dihentikan paksa
```

**Contoh Output:**
```
[1]   Running    sleep 100 &
[2]-  Stopped    vim file.txt
[3]+  Running    ./backup.sh &
```

---

### `fg` - Bring Job to Foreground
```bash
fg [job_id]
```
> Memindahkan job dari background ke foreground

```bash
fg          # Pindahkan job terakhir ke foreground
fg %1       # Pindahkan job nomor 1
fg %2       # Pindahkan job nomor 2
fg %nginx   # Pindahkan job bernama nginx
fg %-       # Pindahkan job sebelumnya
```

---

### `bg` - Send Job to Background
```bash
bg [job_id]
```
> Melanjutkan job yang stopped di background

```bash
bg          # Lanjutkan job terakhir di background
bg %1       # Lanjutkan job nomor 1 di background
bg %2       # Lanjutkan job nomor 2

# Workflow umum:
./script.sh     # Jalankan di foreground
# Tekan Ctrl+Z  → job terhenti (Stopped)
bg              # Lanjutkan di background
# atau
fg              # Kembalikan ke foreground
```

---

### Shortcut Job Control
```bash
Ctrl+C    # Kirim SIGINT → hentikan proses foreground
Ctrl+Z    # Kirim SIGTSTP → pause/stop proses foreground
Ctrl+\    # Kirim SIGQUIT → quit proses foreground
Ctrl+D    # EOF → tutup stdin (sering menutup program)

# Menjalankan di background langsung:
./script.sh &         # Tambahkan & di akhir
command1 & command2 & # Jalankan dua perintah di background
```

---

## 4.4 Penjadwalan Proses

### `cron` & `crontab` - Schedule Tasks
```bash
crontab [opsi]
```
> Menjadwalkan tugas yang berjalan otomatis

**Opsi crontab:**
| Opsi | Penjelasan |
|---|---|
| `crontab -e` | Edit crontab user saat ini |
| `crontab -l` | Tampilkan crontab user saat ini |
| `crontab -r` | Hapus crontab user saat ini |
| `crontab -u username -e` | Edit crontab user tertentu (root) |
| `crontab -u username -l` | Tampilkan crontab user tertentu |
| `crontab file.cron` | Instal crontab dari file |

**Format Crontab:**
```
* * * * * perintah
│ │ │ │ │
│ │ │ │ └── Hari dalam seminggu (0-7, 0=Minggu, 7=Minggu)
│ │ │ └──── Bulan (1-12)
│ │ └────── Tanggal (1-31)
│ └──────── Jam (0-23)
└────────── Menit (0-59)

Simbol Khusus:
*  = Setiap nilai
,  = Daftar nilai (1,3,5)
-  = Range (1-5)
/  = Step (*/5 = setiap 5)
```

**Contoh Jadwal Crontab:**
```bash
# Setiap menit
* * * * * /path/script.sh

# Setiap jam (menit ke-0)
0 * * * * /path/script.sh

# Setiap hari jam 02:30
30 2 * * * /path/script.sh

# Setiap Senin jam 08:00
0 8 * * 1 /path/script.sh

# Setiap 5 menit
*/5 * * * * /path/script.sh

# Setiap hari jam 06:00 dan 18:00
0 6,18 * * * /path/script.sh

# Setiap hari kerja (Senin-Jumat) jam 09:00
0 9 * * 1-5 /path/script.sh

# Tanggal 1 setiap bulan jam 00:00
0 0 1 * * /path/script.sh

# Setiap 30 menit
*/30 * * * * /path/script.sh

# Jam 22:00 setiap hari Sabtu dan Minggu
0 22 * * 6,0 /path/script.sh

# Setiap 6 jam
0 */6 * * * /path/script.sh
```

**String Khusus Crontab:**
```bash
@reboot   → Saat sistem boot
@yearly   → Sekali setahun (0 0 1 1 *)
@monthly  → Sekali sebulan (0 0 1 * *)
@weekly   → Sekali seminggu (0 0 * * 0)
@daily    → Sekali sehari (0 0 * * *)
@hourly   → Sekali sejam (0 * * * *)

# Contoh:
@reboot /path/startup.sh
@daily /path/backup.sh
```

**File Crontab Sistem:**
```bash
/etc/crontab           # Crontab sistem (ada kolom user)
/etc/cron.d/           # Direktori crontab tambahan
/etc/cron.hourly/      # Script yang jalan setiap jam
/etc/cron.daily/       # Script yang jalan setiap hari
/etc/cron.weekly/      # Script yang jalan setiap minggu
/etc/cron.monthly/     # Script yang jalan setiap bulan
```

---

### `at` - Schedule One-time Task
```bash
at [opsi] <waktu>
```
> Menjalankan perintah satu kali pada waktu tertentu

> 📦 Perlu instalasi: `sudo apt install at`

**Format Waktu:**
```bash
at 10:30              # Jam 10:30 hari ini
at 10:30 tomorrow     # Jam 10:30 besok
at 10:30 next monday  # Jam 10:30 Senin depan
at now + 1 hour       # 1 jam dari sekarang
at now + 30 minutes   # 30 menit dari sekarang
at now + 2 days       # 2 hari dari sekarang
at midnight           # Tengah malam
at noon               # Siang hari (12:00)
at 2024-01-20 10:30   # Tanggal dan jam tertentu
```

**Contoh Penggunaan:**
```bash
# Interaktif
at 10:30
at> ./backup.sh
at> Ctrl+D    # Selesai

# Dari pipe
echo "./backup.sh" | at 10:30
echo "systemctl restart nginx" | sudo at 02:00

# Dari file
at 10:30 < perintah.txt
```

**Manajemen Job at:**
| Perintah | Penjelasan |
|---|---|
| `atq` | Tampilkan antrian job at |
| `atrm <job_id>` | Hapus job at |
| `at -l` | Tampilkan antrian (sama dengan atq) |
| `at -d <job_id>` | Hapus job (sama dengan atrm) |
| `at -c <job_id>` | Tampilkan isi job |

---

### `batch` - Run Command When Load is Low
```bash
batch
batch < script.sh
```
> Menjalankan perintah ketika load sistem rendah

---

### `systemd-run` - Run Command as Systemd Unit
```bash
systemd-run [opsi] <perintah>
```
> Menjalankan perintah sebagai unit systemd sementara

```bash
systemd-run --on-active=30s ./script.sh    # Jalankan dalam 30 detik
systemd-run --on-calendar="10:30" ./script.sh  # Jadwal kalender
systemd-run --scope --user ./script.sh     # Jalankan dalam scope
systemd-run -p MemoryLimit=500M ./script.sh  # Dengan batasan memori
```

---

## 4.5 Informasi Proses Lanjutan

### `lsof` - List Open Files
```bash
lsof [opsi]
```
> Menampilkan semua file yang sedang dibuka oleh proses

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `lsof` | Tampilkan semua file yang terbuka |
| `lsof -u username` | File yang dibuka oleh user tertentu |
| `lsof -p 1234` | File yang dibuka oleh PID tertentu |
| `lsof -c nginx` | File yang dibuka oleh proses nginx |
| `lsof /path/file` | Proses yang membuka file tertentu |
| `lsof /mount/point` | File di mount point tertentu |
| `lsof -i` | Semua koneksi jaringan |
| `lsof -i :80` | Proses yang menggunakan port 80 |
| `lsof -i :80 -i :443` | Port 80 dan 443 |
| `lsof -i tcp` | Koneksi TCP |
| `lsof -i udp` | Koneksi UDP |
| `lsof -i @192.168.1.1` | Koneksi ke IP tertentu |
| `lsof -i tcp:80` | TCP port 80 |
| `lsof -n` | Tampilkan IP numerik (tidak resolve hostname) |
| `lsof -t -i :80` | Hanya tampilkan PID (untuk kill) |
| `lsof -r 2` | Refresh setiap 2 detik |
| `lsof +D /path/dir` | File di direktori tertentu (rekursif) |
| `lsof -d 0-2` | File descriptor 0, 1, 2 |

**Contoh Penggunaan:**
```bash
# Temukan proses yang menggunakan port 80
lsof -i :80

# Kill proses yang menggunakan port 8080
kill $(lsof -t -i :8080)

# Lihat file yang dibuka user tertentu
lsof -u user1

# Cari file yang terhapus tapi masih dibuka
lsof | grep deleted

# Semua koneksi jaringan yang established
lsof -i -s TCP:ESTABLISHED
```

---

### `strace` - Trace System Calls
```bash
strace [opsi] <perintah>
```
> Melacak system call yang dipanggil oleh proses

> 📦 Perlu instalasi: `sudo apt install strace`

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `strace ls` | Trace system call dari perintah ls |
| `strace -p 1234` | Trace proses yang sudah berjalan |
| `strace -e open ls` | Hanya trace system call open |
| `strace -e trace=file ls` | Trace semua system call terkait file |
| `strace -e trace=network ls` | Trace system call jaringan |
| `strace -c ls` | Ringkasan statistik system call |
| `strace -f ls` | Ikuti proses anak (fork) |
| `strace -o output.txt ls` | Simpan output ke file |
| `strace -t ls` | Tampilkan timestamp |
| `strace -T ls` | Tampilkan durasi setiap system call |
| `strace -s 1024 ls` | Panjang string output maksimal |

---

### `ltrace` - Trace Library Calls
```bash
ltrace [opsi] <perintah>
```
> Melacak pemanggilan library oleh program

```bash
ltrace ls
ltrace -p 1234
ltrace -c ls          # Statistik
ltrace -e malloc ls   # Trace fungsi tertentu
```

---

### `/proc` Filesystem
```bash
# Informasi proses dari /proc
cat /proc/1234/status       # Status proses PID 1234
cat /proc/1234/cmdline      # Command line proses
cat /proc/1234/environ      # Environment variables
cat /proc/1234/fd/          # File descriptor yang terbuka
cat /proc/1234/maps         # Memory map
cat /proc/1234/net/tcp      # Koneksi TCP
cat /proc/cpuinfo           # Informasi CPU
cat /proc/meminfo           # Informasi memori
cat /proc/loadavg           # Load average
cat /proc/uptime            # Uptime sistem
cat /proc/version           # Versi kernel
cat /proc/mounts            # Mount point
cat /proc/partitions        # Partisi disk
cat /proc/net/dev           # Statistik jaringan
cat /proc/sys/kernel/pid_max  # Maksimum PID
```

---

### `vmstat` - Virtual Memory Statistics
```bash
vmstat [opsi] [interval] [count]
```
> Menampilkan statistik memori virtual, proses, I/O, CPU

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `vmstat` | Tampilkan statistik saat ini |
| `vmstat 2` | Update setiap 2 detik |
| `vmstat 2 10` | Update setiap 2 detik, 10 kali |
| `vmstat -a` | Tampilkan active/inactive memory |
| `vmstat -d` | Statistik disk |
| `vmstat -D` | Ringkasan statistik disk |
| `vmstat -p /dev/sda1` | Statistik partisi tertentu |
| `vmstat -s` | Tampilkan tabel statistik |
| `vmstat -m` | Statistik memori slab |
| `vmstat -t` | Tambahkan timestamp |
| `vmstat -n` | Jangan print header setiap layar |

**Kolom Output vmstat:**
```
Procs:
  r → Proses menunggu run time
  b → Proses dalam uninterruptible sleep

Memory:
  swpd  → Memori swap yang digunakan (KB)
  free  → Memori bebas (KB)
  buff  → Buffer (KB)
  cache → Cache (KB)

Swap:
  si → Swap in dari disk (KB/s)
  so → Swap out ke disk (KB/s)

IO:
  bi → Blok yang diterima dari disk (blok/s)
  bo → Blok yang dikirim ke disk (blok/s)

System:
  in → Interrupt per detik
  cs → Context switch per detik

CPU:
  us → User time
  sy → System time
  id → Idle time
  wa → I/O wait
  st → Stolen time (VM)
```

---

### `iostat` - I/O Statistics
```bash
iostat [opsi] [interval] [count]
```
> Menampilkan statistik CPU dan I/O perangkat

> 📦 Perlu instalasi: `sudo apt install sysstat`

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `iostat` | Tampilkan statistik dasar |
| `iostat 2` | Update setiap 2 detik |
| `iostat 2 5` | Update setiap 2 detik, 5 kali |
| `iostat -c` | Hanya statistik CPU |
| `iostat -d` | Hanya statistik disk |
| `iostat -x` | Statistik extended (detail) |
| `iostat -h` | Human-readable |
| `iostat -m` | Output dalam MB |
| `iostat -k` | Output dalam KB |
| `iostat -p sda` | Statistik disk tertentu |
| `iostat -N` | Tampilkan nama device LVM |
| `iostat -t` | Tambahkan timestamp |

---

### `mpstat` - Multiprocessor Statistics
```bash
mpstat [opsi] [interval] [count]
```
> Menampilkan statistik penggunaan CPU per core

> 📦 Perlu instalasi: `sudo apt install sysstat`

```bash
mpstat              # Statistik semua CPU
mpstat 2            # Update setiap 2 detik
mpstat -P ALL 2     # Statistik semua core setiap 2 detik
mpstat -P 0 2       # Hanya core 0
mpstat -u 2         # CPU utilization
mpstat -I ALL 2     # Statistik interrupt
```

---

### `pidstat` - Statistics by Process
```bash
pidstat [opsi] [interval] [count]
```
> Menampilkan statistik penggunaan sumber daya per proses

> 📦 Perlu instalasi: `sudo apt install sysstat`

```bash
pidstat             # Statistik semua proses
pidstat 2           # Update setiap 2 detik
pidstat -u 2        # CPU usage per proses
pidstat -r 2        # Memory usage per proses
pidstat -d 2        # Disk I/O per proses
pidstat -w 2        # Context switching
pidstat -p 1234 2   # Statistik PID tertentu
pidstat -C nginx    # Statistik proses nginx
pidstat -t          # Tampilkan threads
pidstat -l          # Tampilkan command line lengkap
```

---

### `uptime` - System Uptime
```bash
uptime [opsi]
```
> Menampilkan berapa lama sistem sudah berjalan

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `uptime` | Tampilkan waktu, uptime, user, load average |
| `uptime -p` | Format uptime yang mudah dibaca |
| `uptime -s` | Waktu sistem mulai berjalan |

**Contoh Output:**
```
10:30:00 up 2 days, 3:45,  2 users,  load average: 0.15, 0.10, 0.09
         │          │        │         │
         │          │        │         └── Load avg: 1, 5, 15 menit
         │          │        └──────────── Jumlah user login
         │          └───────────────────── Lama uptime
         └──────────────────────────────── Waktu saat ini

Load average: jumlah proses yang menunggu CPU
  < jumlah core CPU = sistem tidak overload
  > jumlah core CPU = sistem overload
```

---

### `time` - Time Command Execution
```bash
time <perintah>
```
> Mengukur waktu eksekusi sebuah perintah

```bash
time ls -la
time ./script.sh
time find / -name "*.log"

# Output:
# real    0m0.015s   → Waktu total (wall clock)
# user    0m0.008s   → Waktu CPU di user space
# sys     0m0.007s   → Waktu CPU di kernel space
```

---

### `timeout` - Run Command with Time Limit
```bash
timeout [opsi] <durasi> <perintah>
```
> Menjalankan perintah dengan batas waktu

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `timeout 10 ./script.sh` | Hentikan setelah 10 detik |
| `timeout 1m ./script.sh` | Hentikan setelah 1 menit |
| `timeout 1h ./script.sh` | Hentikan setelah 1 jam |
| `timeout -k 5 10 ./script.sh` | Kirim SIGKILL jika masih hidup setelah 5 detik |
| `timeout -s SIGTERM 10 ./script.sh` | Gunakan sinyal tertentu |
| `timeout --preserve-status 10 ./script.sh` | Pertahankan exit status |

---

## 4.6 Prioritas & Scheduling

### `taskset` - Set CPU Affinity
```bash
taskset [opsi] <mask> <perintah>
```
> Menentukan CPU core mana yang boleh digunakan proses

```bash
taskset -c 0 ./program          # Jalankan hanya di CPU core 0
taskset -c 0,1 ./program        # Jalankan di core 0 dan 1
taskset -c 0-3 ./program        # Jalankan di core 0 sampai 3
taskset -c -p 1234              # Lihat CPU affinity PID 1234
taskset -c 0,1 -p 1234          # Set CPU affinity PID 1234
taskset 0x3 ./program           # Gunakan bitmask hex (core 0,1)
```

---

### `chrt` - Change Real-time Attributes
```bash
chrt [opsi] <prioritas> <perintah>
```
> Mengatur scheduling policy dan prioritas real-time proses

```bash
chrt -f 50 ./program       # FIFO scheduling, prioritas 50
chrt -r 50 ./program       # Round-robin scheduling
chrt -b 0 ./program        # Batch scheduling
chrt -i 0 ./program        # Idle scheduling
chrt -o 0 ./program        # Other scheduling (default)
chrt -p 1234               # Lihat scheduling proses
chrt -f -p 50 1234         # Set scheduling PID yang berjalan
```

---

## Ringkasan Perintah Bagian 4

```
ps            → Informasi proses
top           → Monitor proses real-time (basic)
htop          → Monitor proses real-time (advanced)
atop          → Monitor sistem detail
pgrep         → Cari PID berdasarkan nama
pidof         → PID program
pstree        → Hierarki proses
kill          → Kirim sinyal ke PID
killall       → Kill berdasarkan nama
pkill         → Kill berdasarkan pola
nice          → Jalankan dengan prioritas
renice        → Ubah prioritas proses berjalan
nohup         → Jalankan proses tahan hangup
disown        → Lepas job dari shell
jobs          → Lihat background job
fg            → Pindah job ke foreground
bg            → Pindah job ke background
crontab       → Jadwalkan tugas berkala
at            → Jadwalkan tugas satu kali
lsof          → Lihat file yang terbuka
strace        → Lacak system call
vmstat        → Statistik memori virtual
iostat        → Statistik I/O
mpstat        → Statistik CPU per core
pidstat       → Statistik per proses
uptime        → Lama sistem berjalan
time          → Ukur waktu eksekusi
timeout       → Jalankan dengan batas waktu
taskset       → Set CPU affinity
chrt          → Set real-time scheduling
```

---

## ✅ Bagian 4 Selesai!

**Lanjut ke Bagian 5: Perintah Manajemen Paket?**
