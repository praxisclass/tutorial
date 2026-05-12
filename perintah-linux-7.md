# Bagian 7: Perintah Disk & Storage

---

## 7.1 Informasi Disk & Partisi

### `df` - Disk Free Space
```bash
df [opsi] [filesystem]
```
> Menampilkan penggunaan ruang disk pada filesystem

| Perintah | Penjelasan |
|---|---|
| `df` | Tampilkan semua filesystem |
| `df -h` | Human-readable (KB, MB, GB) |
| `df -H` | Human-readable dengan basis 1000 (bukan 1024) |
| `df -T` | Tampilkan tipe filesystem |
| `df -t ext4` | Hanya filesystem tipe ext4 |
| `df -x tmpfs` | Kecualikan tipe tmpfs |
| `df -i` | Tampilkan penggunaan inode |
| `df -l` | Hanya filesystem lokal |
| `df -a` | Semua filesystem termasuk dummy |
| `df /home` | Info filesystem di /home |
| `df /dev/sda1` | Info partisi tertentu |
| `df -hT` | Human-readable + tipe filesystem |
| `df --total` | Tampilkan total di akhir |
| `df -P` | Format POSIX (satu baris per filesystem) |
| `df -h --output=source,size,used,avail,pcent,target` | Pilih kolom tertentu |

**Contoh Output df -hT:**
```
Filesystem     Type      Size  Used Avail Use% Mounted on
/dev/sda1      ext4       50G   20G   28G  42% /
/dev/sda2      ext4      100G   50G   45G  53% /home
tmpfs          tmpfs     7.8G  1.2M  7.8G   1% /dev/shm
/dev/sdb1      xfs       200G   80G  120G  40% /data
```

---

### `du` - Disk Usage
```bash
du [opsi] [direktori/file]
```
> Menampilkan penggunaan ruang disk oleh file dan direktori

| Perintah | Penjelasan |
|---|---|
| `du` | Ukuran semua direktori secara rekursif |
| `du -h` | Human-readable |
| `du -s` | Hanya total |
| `du -sh` | Total human-readable |
| `du -sh *` | Ukuran semua item di direktori saat ini |
| `du -sh */ ` | Hanya direktori |
| `du -sh /home/user` | Ukuran direktori tertentu |
| `du -a` | Tampilkan semua file + direktori |
| `du -ah` | Semua file + human-readable |
| `du -c` | Tampilkan grand total |
| `du -ch` | Grand total human-readable |
| `du --max-depth=1` | Kedalaman 1 level |
| `du --max-depth=1 -h /home` | Kedalaman 1 + human-readable |
| `du -sh * \| sort -h` | Urutkan berdasarkan ukuran |
| `du -sh * \| sort -rh` | Urutkan terbesar ke terkecil |
| `du -sh * \| sort -rh \| head -10` | 10 item terbesar |
| `du --exclude="*.log" -sh /var` | Kecualikan file log |
| `du -x /` | Jangan cross filesystem |
| `du -L` | Ikuti symbolic link |
| `du -b` | Ukuran dalam bytes |
| `du -k` | Ukuran dalam KB |
| `du -m` | Ukuran dalam MB |
| `du --time` | Tampilkan waktu modifikasi |
| `du --apparent-size -sh` | Ukuran sebenarnya (bukan disk usage) |
| `du -sh /var/log/*` | Ukuran semua file di /var/log |

**Contoh Mencari File Terbesar:**
```bash
# 10 direktori terbesar di /
sudo du -sh /* 2>/dev/null | sort -rh | head -10

# 20 file terbesar di sistem
sudo find / -type f -printf "%s %p\n" 2>/dev/null | \
  sort -rn | head -20 | \
  awk '{printf "%.2f MB\t%s\n", $1/1048576, $2}'

# Ukuran per subdirektori
du --max-depth=2 -h /var | sort -rh | head -20
```

---

### `lsblk` - List Block Devices
```bash
lsblk [opsi] [device]
```
> Menampilkan semua block device dalam format pohon

| Perintah | Penjelasan |
|---|---|
| `lsblk` | Tampilkan semua block device |
| `lsblk -a` | Tampilkan semua termasuk empty |
| `lsblk -f` | Tampilkan filesystem info (type, UUID, mount) |
| `lsblk -l` | Format list (bukan tree) |
| `lsblk -m` | Tampilkan permission |
| `lsblk -o NAME,SIZE,TYPE,MOUNTPOINT` | Kolom kustom |
| `lsblk -o NAME,SIZE,FSTYPE,LABEL,UUID,MOUNTPOINT` | Lebih detail |
| `lsblk -d` | Hanya device (tanpa partisi) |
| `lsblk -J` | Output JSON |
| `lsblk -P` | Output key=value pairs |
| `lsblk -n` | Tanpa header |
| `lsblk -p` | Path lengkap device |
| `lsblk -s` | Dependensi terbalik (inverse) |
| `lsblk -t` | Tampilkan topology info |
| `lsblk /dev/sda` | Info device tertentu |
| `lsblk -b` | Ukuran dalam bytes |
| `lsblk -i` | Gunakan karakter ASCII (bukan Unicode) |

**Contoh Output lsblk -f:**
```
NAME   FSTYPE   LABEL  UUID                                 FSAVAIL FSUSE% MOUNTPOINT
sda
├─sda1 ext4            a1b2c3d4-e5f6-7890-abcd-ef1234567890   28.2G    42% /
├─sda2 ext4            b2c3d4e5-f6a7-8901-bcde-f12345678901   44.9G    53% /home
└─sda3 swap            c3d4e5f6-a7b8-9012-cdef-123456789012                [SWAP]
sdb
└─sdb1 xfs             d4e5f6a7-b8c9-0123-defa-234567890123  119.9G    40% /data
```

---

### `fdisk` - Partition Table Manipulator
```bash
fdisk [opsi] <device>
```
> Membuat dan mengelola partisi disk (MBR/GPT)

**Mode Interaktif:**
```bash
sudo fdisk /dev/sda       # Buka fdisk untuk /dev/sda
sudo fdisk -l             # List semua partisi
sudo fdisk -l /dev/sda    # List partisi disk tertentu
sudo fdisk -s /dev/sda    # Ukuran disk dalam blok
```

**Perintah di dalam fdisk:**
| Tombol | Penjelasan |
|---|---|
| **p** | Print tabel partisi |
| **n** | Buat partisi baru |
| **d** | Hapus partisi |
| **t** | Ubah tipe partisi |
| **l** | Daftar tipe partisi |
| **a** | Toggle boot flag |
| **w** | Simpan dan keluar |
| **q** | Keluar tanpa menyimpan |
| **m** | Tampilkan bantuan |
| **v** | Verifikasi tabel partisi |
| **x** | Menu expert |
| **o** | Buat tabel partisi DOS baru |
| **g** | Buat tabel partisi GPT baru |
| **i** | Info partisi |
| **F** | Tampilkan ruang yang tidak dipartisi |

**Contoh Membuat Partisi Baru:**
```bash
sudo fdisk /dev/sdb
# n → new partition
# p → primary
# 1 → partition number
# Enter → default first sector
# +50G → ukuran 50GB
# w → write dan keluar
```

---

### `gdisk` - GPT Partition Table Editor
```bash
gdisk [opsi] <device>
```
> Editor tabel partisi GPT (GUID Partition Table)

```bash
sudo gdisk /dev/sda        # Buka gdisk
sudo gdisk -l /dev/sda     # List partisi GPT
```

**Perintah di dalam gdisk:**
| Tombol | Penjelasan |
|---|---|
| **p** | Print tabel partisi |
| **n** | Buat partisi baru |
| **d** | Hapus partisi |
| **t** | Ubah tipe partisi |
| **l** | Daftar tipe GUID |
| **i** | Info partisi |
| **o** | Buat tabel GPT baru |
| **w** | Simpan dan keluar |
| **q** | Keluar tanpa menyimpan |
| **b** | Backup ke file |
| **r** | Recovery mode |
| **x** | Expert mode |
| **v** | Verifikasi disk |
| **?** | Bantuan |

---

### `parted` - Partition Editor
```bash
parted [opsi] <device> [perintah]
```
> Editor partisi yang mendukung MBR dan GPT

**Non-interaktif:**
```bash
sudo parted /dev/sda print              # Tampilkan info partisi
sudo parted -l                          # List semua disk & partisi
sudo parted /dev/sda unit GB print      # Tampilkan dalam GB
sudo parted /dev/sda mkpart primary ext4 0% 50GB  # Buat partisi
sudo parted /dev/sda rm 3               # Hapus partisi 3
sudo parted /dev/sda resizepart 1 100GB # Resize partisi 1 ke 100GB
sudo parted /dev/sda set 1 boot on      # Set boot flag
sudo parted /dev/sda name 1 "boot"     # Beri nama partisi (GPT)
sudo parted /dev/sda mklabel gpt        # Buat label GPT
sudo parted /dev/sda mklabel msdos      # Buat label MBR
sudo parted /dev/sda align-check optimal 1  # Cek alignment
```

**Mode Interaktif:**
```bash
sudo parted /dev/sda
(parted) print              # Tampilkan tabel partisi
(parted) mkpart             # Buat partisi baru (interaktif)
(parted) resizepart 1 50GB  # Resize partisi
(parted) rm 1               # Hapus partisi
(parted) quit               # Keluar
```

---

### `cfdisk` - Curses-based Partition Editor
```bash
cfdisk [device]
```
> Editor partisi dengan antarmuka semi-grafis

```bash
sudo cfdisk /dev/sda    # Buka cfdisk
sudo cfdisk            # Disk default
```

> Navigasi menggunakan tombol panah dan Enter untuk memilih aksi

---

### `sfdisk` - Scriptable Partition Editor
```bash
sfdisk [opsi] <device>
```
> Editor partisi yang bisa di-script

```bash
sudo sfdisk -l              # List semua partisi
sudo sfdisk -l /dev/sda     # List partisi disk tertentu
sudo sfdisk -d /dev/sda     # Dump tabel partisi
sudo sfdisk -d /dev/sda > partition-backup.txt  # Backup
sudo sfdisk /dev/sdb < partition-backup.txt     # Restore
sudo sfdisk --verify /dev/sda  # Verifikasi
sudo sfdisk -g /dev/sda     # Info geometry disk
```

---

## 7.2 Membuat & Mengelola Filesystem

### `mkfs` - Make Filesystem
```bash
mkfs [opsi] -t <tipe> <device>
```
> Membuat filesystem pada partisi

| Perintah | Penjelasan |
|---|---|
| `sudo mkfs -t ext4 /dev/sdb1` | Buat filesystem ext4 |
| `sudo mkfs.ext4 /dev/sdb1` | Sama, shorthand |
| `sudo mkfs.ext3 /dev/sdb1` | Buat filesystem ext3 |
| `sudo mkfs.ext2 /dev/sdb1` | Buat filesystem ext2 |
| `sudo mkfs.xfs /dev/sdb1` | Buat filesystem XFS |
| `sudo mkfs.btrfs /dev/sdb1` | Buat filesystem Btrfs |
| `sudo mkfs.vfat /dev/sdb1` | Buat filesystem FAT32 |
| `sudo mkfs.vfat -F 16 /dev/sdb1` | Buat filesystem FAT16 |
| `sudo mkfs.ntfs /dev/sdb1` | Buat filesystem NTFS |
| `sudo mkfs.f2fs /dev/sdb1` | Buat filesystem F2FS |
| `sudo mkfs.ext4 -L "Data" /dev/sdb1` | Dengan label |
| `sudo mkfs.ext4 -m 1 /dev/sdb1` | Reserved space 1% (default 5%) |
| `sudo mkfs.ext4 -E lazy_itable_init=0 /dev/sdb1` | Inisialisasi penuh |
| `sudo mkfs.ext4 -b 4096 /dev/sdb1` | Block size 4096 bytes |
| `sudo mkfs.ext4 -n /dev/sdb1` | Dry run (simulasi) |

---

### `e2fsck` - Check ext Filesystem
```bash
e2fsck [opsi] <device>
```
> Memeriksa dan memperbaiki filesystem ext2/ext3/ext4

| Perintah | Penjelasan |
|---|---|
| `sudo e2fsck /dev/sdb1` | Cek filesystem |
| `sudo e2fsck -f /dev/sdb1` | Force check meski terlihat bersih |
| `sudo e2fsck -p /dev/sdb1` | Auto repair tanpa interaksi |
| `sudo e2fsck -y /dev/sdb1` | Jawab yes untuk semua perbaikan |
| `sudo e2fsck -n /dev/sdb1` | Dry run (tidak ubah apapun) |
| `sudo e2fsck -c /dev/sdb1` | Cek bad blocks |
| `sudo e2fsck -v /dev/sdb1` | Verbose |
| `sudo e2fsck -C 0 /dev/sdb1` | Tampilkan progress |
| `sudo fsck /dev/sdb1` | General fsck (bisa untuk semua tipe) |
| `sudo fsck -t ext4 /dev/sdb1` | Tentukan tipe |
| `sudo fsck -A` | Cek semua filesystem di /etc/fstab |
| `sudo fsck -AR -y` | Cek semua + auto repair |

> ⚠️ **PERINGATAN:** Jalankan fsck hanya pada filesystem yang tidak di-mount!

---

### `tune2fs` - Adjust ext Filesystem Parameters
```bash
tune2fs [opsi] <device>
```
> Mengatur parameter filesystem ext2/ext3/ext4

| Perintah | Penjelasan |
|---|---|
| `sudo tune2fs -l /dev/sda1` | Tampilkan superblock info |
| `sudo tune2fs -L "MyDisk" /dev/sda1` | Set label |
| `sudo tune2fs -m 2 /dev/sda1` | Ubah reserved space ke 2% |
| `sudo tune2fs -i 0 /dev/sda1` | Nonaktifkan interval check |
| `sudo tune2fs -c 0 /dev/sda1` | Nonaktifkan mount count check |
| `sudo tune2fs -e remount-ro /dev/sda1` | Error behavior |
| `sudo tune2fs -j /dev/sda1` | Konversi ext2 ke ext3 (tambah journal) |
| `sudo tune2fs -O extent /dev/sda1` | Aktifkan extent feature |
| `sudo tune2fs -U random /dev/sda1` | Generate UUID baru |
| `sudo tune2fs -E max_mount_count=50 /dev/sda1` | Set mount count |

---

### `xfs_info` & `xfs_repair` - XFS Tools
```bash
# Info filesystem XFS
sudo xfs_info /dev/sda1
sudo xfs_info /mount/point

# Repair XFS
sudo xfs_repair /dev/sdb1          # Repair filesystem
sudo xfs_repair -n /dev/sdb1       # Dry run
sudo xfs_repair -L /dev/sdb1       # Reset log (hati-hati!)

# Check XFS
sudo xfs_check /dev/sdb1           # Check filesystem
sudo xfs_db -r /dev/sdb1           # Debug XFS
sudo xfs_db -r -c "freesp" /dev/sdb1  # Cek free space
sudo xfs_admin -l /dev/sdb1        # List label
sudo xfs_admin -L "Data" /dev/sdb1 # Set label
sudo xfs_admin -U generate /dev/sdb1  # Generate UUID baru

# Backup & Restore XFS
sudo xfsdump -f backup.xfsdump /dev/sdb1  # Backup
sudo xfsrestore -f backup.xfsdump /mnt/restore  # Restore

# Defragmentasi XFS
sudo xfs_fsr /dev/sdb1
sudo xfs_fsr /mount/point
```

---

### `btrfs` - Btrfs Filesystem Tools
```bash
btrfs [perintah] [opsi]
```
> Manajemen filesystem Btrfs (B-tree filesystem)

```bash
# Informasi
sudo btrfs filesystem show /dev/sdb1
sudo btrfs filesystem df /mount/point
sudo btrfs filesystem usage /mount/point

# Subvolume
sudo btrfs subvolume create /mount/subvol
sudo btrfs subvolume list /mount/point
sudo btrfs subvolume delete /mount/subvol
sudo btrfs subvolume snapshot /mount/subvol /mount/snapshot
sudo btrfs subvolume snapshot -r /mount/subvol /mount/readonly-snap

# Device management
sudo btrfs device add /dev/sdc1 /mount/point
sudo btrfs device delete /dev/sdc1 /mount/point
sudo btrfs device stats /mount/point

# Balance & Scrub
sudo btrfs balance start /mount/point
sudo btrfs balance status /mount/point
sudo btrfs scrub start /mount/point
sudo btrfs scrub status /mount/point

# Check & Repair
sudo btrfs check /dev/sdb1
sudo btrfs check --repair /dev/sdb1

# RAID
sudo btrfs balance start -dconvert=raid1 /mount/point

# Quota
sudo btrfs quota enable /mount/point
sudo btrfs qgroup show /mount/point
```

---

## 7.3 Mount & Unmount

### `mount` - Mount Filesystem
```bash
mount [opsi] <device> <mount_point>
```
> Me-mount filesystem ke direktori

**Perintah Dasar:**
| Perintah | Penjelasan |
|---|---|
| `mount` | Tampilkan semua filesystem yang di-mount |
| `mount \| column -t` | Format rapi |
| `cat /proc/mounts` | Daftar mount dari kernel |
| `sudo mount /dev/sdb1 /mnt/data` | Mount partisi |
| `sudo mount -t ext4 /dev/sdb1 /mnt/data` | Tentukan tipe filesystem |
| `sudo mount -t vfat /dev/sdb1 /mnt/usb` | Mount USB FAT32 |
| `sudo mount -t ntfs /dev/sdb1 /mnt/windows` | Mount NTFS |
| `sudo mount -t ntfs-3g /dev/sdb1 /mnt/windows` | Mount NTFS (read-write) |
| `sudo mount -o ro /dev/sdb1 /mnt/data` | Mount read-only |
| `sudo mount -o rw /dev/sdb1 /mnt/data` | Mount read-write |
| `sudo mount -o remount,rw /dev/sda1` | Remount dengan opsi baru |
| `sudo mount -o remount,ro /` | Remount root read-only |
| `sudo mount -a` | Mount semua yang ada di /etc/fstab |
| `sudo mount UUID="uuid" /mnt/data` | Mount via UUID |
| `sudo mount LABEL="Data" /mnt/data` | Mount via label |

**Opsi Mount:**
| Opsi | Penjelasan |
|---|---|
| `ro` | Read-only |
| `rw` | Read-write |
| `noexec` | Jangan izinkan eksekusi |
| `exec` | Izinkan eksekusi |
| `nosuid` | Abaikan SUID/SGID bits |
| `suid` | Izinkan SUID/SGID |
| `nodev` | Jangan interpretasikan device files |
| `dev` | Izinkan device files |
| `noatime` | Jangan update access time |
| `relatime` | Update atime relatif (hemat I/O) |
| `nodiratime` | Jangan update directory access time |
| `sync` | I/O sinkron |
| `async` | I/O asinkron (default) |
| `auto` | Mount otomatis saat `mount -a` |
| `noauto` | Jangan mount otomatis |
| `user` | Izinkan user biasa mount |
| `nouser` | Hanya root yang bisa mount |
| `defaults` | rw, suid, dev, exec, auto, nouser, async |
| `loop` | Mount file sebagai loop device |

**Mount Khusus:**
```bash
# Mount ISO/image file
sudo mount -o loop /path/image.iso /mnt/iso
sudo mount -t iso9660 -o loop image.iso /mnt/iso

# Mount tmpfs (di memori RAM)
sudo mount -t tmpfs -o size=1G tmpfs /mnt/tmpfs

# Mount bind (duplikasi direktori)
sudo mount --bind /source /destination
sudo mount -o bind /source /destination

# Mount NFS
sudo mount -t nfs server:/share /mnt/nfs
sudo mount -t nfs -o rw,soft,intr server:/share /mnt/nfs

# Mount CIFS/Samba
sudo mount -t cifs //server/share /mnt/samba -o user=user,password=pass
sudo mount -t cifs //server/share /mnt/samba -o credentials=/etc/samba/creds

# Mount sshfs (SSH Filesystem)
sshfs user@server:/path /mnt/remote
sshfs -o allow_other user@server:/path /mnt/remote
```

---

### `umount` - Unmount Filesystem
```bash
umount [opsi] <device|mount_point>
```
> Me-unmount filesystem

| Perintah | Penjelasan |
|---|---|
| `sudo umount /mnt/data` | Unmount via mount point |
| `sudo umount /dev/sdb1` | Unmount via device |
| `sudo umount -l /mnt/data` | Lazy unmount (tunggu hingga tidak digunakan) |
| `sudo umount -f /mnt/nfs` | Force unmount (untuk NFS yang hang) |
| `sudo umount -R /mnt/data` | Unmount rekursif |
| `sudo umount -a` | Unmount semua |
| `sudo umount -t ext4` | Unmount semua filesystem ext4 |
| `sudo umount -v /mnt/data` | Verbose |

**Jika Device Busy:**
```bash
# Cari proses yang menggunakan mount point
fuser -m /mnt/data           # Tampilkan PID
fuser -km /mnt/data          # Kill proses + unmount
lsof /mnt/data               # Detail proses

# Gunakan lazy unmount
sudo umount -l /mnt/data
```

---

### `/etc/fstab` - Filesystem Table
```bash
# Lihat fstab
cat /etc/fstab

# Edit fstab
sudo nano /etc/fstab

# Test mount dari fstab
sudo mount -a     # Mount semua yang ada di fstab
sudo mount -fav   # Dry run + verbose

# Format fstab:
# <device>  <mount_point>  <type>  <options>  <dump>  <pass>
```

**Contoh /etc/fstab:**
```
# <device>           <mount_point>  <type>   <options>                    <dump> <pass>
UUID=abc123          /              ext4     defaults,errors=remount-ro   0      1
UUID=def456          /home          ext4     defaults                     0      2
UUID=ghi789          /data          xfs      defaults,noatime             0      2
UUID=jkl012          none           swap     sw                           0      0
/dev/sdc1            /mnt/backup    ext4     defaults,noauto,user         0      0
tmpfs                /tmp           tmpfs    defaults,size=2G             0      0
server:/share        /mnt/nfs       nfs      rw,soft,intr                 0      0
//server/share       /mnt/samba     cifs     credentials=/etc/samba/creds 0      0
```

**Kolom fstab:**
```
1. Device   → UUID=..., /dev/sda1, LABEL=..., server:/share
2. Mount    → /boot, /home, /mnt/data, swap=none, swap=sw
3. Type     → ext4, xfs, btrfs, vfat, ntfs, nfs, cifs, tmpfs, swap, auto
4. Options  → defaults, ro, noatime, user, noauto, dll.
5. Dump     → 0 = tidak backup, 1 = backup dengan dump
6. Pass     → 0 = tidak check, 1 = check pertama (root), 2 = check kedua
```

---

### `findmnt` - Find Mount
```bash
findmnt [opsi] [device|mount_point]
```
> Menampilkan informasi mount dalam format pohon

| Perintah | Penjelasan |
|---|---|
| `findmnt` | Tampilkan semua mount dalam tree |
| `findmnt -l` | Format list |
| `findmnt -t ext4` | Filter tipe filesystem |
| `findmnt /home` | Info mount point tertentu |
| `findmnt /dev/sda1` | Info device tertentu |
| `findmnt --fstab` | Tampilkan dari /etc/fstab |
| `findmnt -o TARGET,SOURCE,FSTYPE,OPTIONS` | Kolom kustom |
| `findmnt -D` | Statistik disk usage |
| `findmnt -P` | Output key=value |
| `findmnt -J` | Output JSON |
| `findmnt -n` | Tanpa header |
| `findmnt --verify` | Verifikasi fstab |
| `findmnt -r` | Raw output |

---

## 7.4 Swap Management

### `swapon` & `swapoff` - Enable/Disable Swap
```bash
# Menampilkan info swap
swapon --show
swapon -s
cat /proc/swaps
free -h

# Aktifkan swap
sudo swapon /dev/sda3              # Aktifkan swap partition
sudo swapon /swapfile              # Aktifkan swap file
sudo swapon -a                     # Aktifkan semua swap di fstab
sudo swapon -p 10 /dev/sda3       # Aktifkan dengan prioritas

# Nonaktifkan swap
sudo swapoff /dev/sda3
sudo swapoff /swapfile
sudo swapoff -a                    # Nonaktifkan semua swap
```

### Membuat Swap File
```bash
# Buat file swap 4GB
sudo fallocate -l 4G /swapfile

# Atau menggunakan dd
sudo dd if=/dev/zero of=/swapfile bs=1M count=4096

# Set permission
sudo chmod 600 /swapfile

# Format sebagai swap
sudo mkswap /swapfile

# Aktifkan
sudo swapon /swapfile

# Tambahkan ke /etc/fstab untuk permanen
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Cek
free -h
swapon --show
```

### Membuat Swap Partition
```bash
# Buat partisi di fdisk/gdisk
# Set tipe ke Linux swap (82 di fdisk, 8200 di gdisk)

# Format sebagai swap
sudo mkswap /dev/sda3

# Aktifkan
sudo swapon /dev/sda3

# Tambahkan ke fstab
sudo blkid /dev/sda3   # Dapatkan UUID
# Tambahkan ke /etc/fstab:
# UUID=xxx none swap sw 0 0
```

### `swappiness` - Swap Aggressiveness
```bash
# Lihat nilai swappiness saat ini (default: 60)
cat /proc/sys/vm/swappiness

# Ubah sementara
sudo sysctl vm.swappiness=10

# Ubah permanen
echo "vm.swappiness=10" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

---

## 7.5 LVM - Logical Volume Manager

### Konsep LVM
```
Physical Volume (PV) → partisi atau disk fisik
Volume Group (VG)    → pool dari beberapa PV
Logical Volume (LV)  → partisi virtual dari VG

Physical Disks → PV → VG → LV → Filesystem
```

### Physical Volume

```bash
# Buat PV
sudo pvcreate /dev/sdb1
sudo pvcreate /dev/sdb1 /dev/sdc1    # Beberapa sekaligus

# Tampilkan PV
pvs                         # Ringkasan
pvdisplay                   # Detail lengkap
pvdisplay /dev/sdb1         # PV tertentu
pvscan                      # Scan semua PV

# Hapus PV
sudo pvremove /dev/sdb1

# Resize PV
sudo pvresize /dev/sdb1

# Info PV
pvck /dev/sdb1              # Cek konsistensi
```

---

### Volume Group

```bash
# Buat VG
sudo vgcreate vg_data /dev/sdb1
sudo vgcreate vg_data /dev/sdb1 /dev/sdc1    # Dari beberapa PV

# Tampilkan VG
vgs                         # Ringkasan
vgdisplay                   # Detail
vgdisplay vg_data           # VG tertentu
vgscan                      # Scan semua VG

# Extend VG (tambah PV)
sudo vgextend vg_data /dev/sdd1

# Reduce VG (hapus PV)
sudo vgreduce vg_data /dev/sdd1

# Rename VG
sudo vgrename vg_data vg_newname

# Hapus VG
sudo vgremove vg_data

# Aktivasi/Deaktivasi VG
sudo vgchange -ay vg_data    # Aktifkan
sudo vgchange -an vg_data    # Nonaktifkan

# Merge VG
sudo vgmerge vg_main vg_secondary

# Split VG
sudo vgsplit vg_data vg_new /dev/sdc1

# Export/Import VG
sudo vgexport vg_data
sudo vgimport vg_data

# Backup VG metadata
sudo vgcfgbackup vg_data
sudo vgcfgrestore vg_data
```

---

### Logical Volume

```bash
# Buat LV
sudo lvcreate -L 20G -n lv_home vg_data    # 20GB
sudo lvcreate -l 50%VG -n lv_data vg_data  # 50% dari VG
sudo lvcreate -l 100%FREE -n lv_backup vg_data  # Semua sisa
sudo lvcreate -l +100%FREE -n lv_backup vg_data  # Extend dengan sisa

# Tampilkan LV
lvs                         # Ringkasan
lvdisplay                   # Detail
lvdisplay /dev/vg_data/lv_home  # LV tertentu
lvscan                      # Scan semua LV

# Path LV
/dev/vg_data/lv_home
/dev/mapper/vg_data-lv_home    # Format alternatif

# Extend LV
sudo lvextend -L +10G /dev/vg_data/lv_home     # Tambah 10GB
sudo lvextend -L 50G /dev/vg_data/lv_home      # Set ke 50GB total
sudo lvextend -l +100%FREE /dev/vg_data/lv_home # Gunakan sisa VG

# Extend LV + resize filesystem (ext4)
sudo lvextend -L +10G -r /dev/vg_data/lv_home   # -r = resize filesystem

# Resize filesystem setelah extend (manual)
sudo resize2fs /dev/vg_data/lv_home       # ext4
sudo xfs_growfs /mount/point              # xfs (hanya bisa grow)

# Reduce LV (BERBAHAYA!)
# ext4:
sudo e2fsck -f /dev/vg_data/lv_home
sudo resize2fs /dev/vg_data/lv_home 15G
sudo lvreduce -L 15G /dev/vg_data/lv_home

# Rename LV
sudo lvrename vg_data lv_home lv_new_home

# Hapus LV
sudo lvremove /dev/vg_data/lv_home

# Aktifkan/Nonaktifkan LV
sudo lvchange -ay /dev/vg_data/lv_home    # Aktifkan
sudo lvchange -an /dev/vg_data/lv_home    # Nonaktifkan

# Snapshot LV
sudo lvcreate -L 5G -s -n snap_home /dev/vg_data/lv_home

# Merge snapshot
sudo lvconvert --merge /dev/vg_data/snap_home

# Thin Provisioning
sudo lvcreate -T -L 100G vg_data/thin_pool
sudo lvcreate -V 50G -T vg_data/thin_pool -n lv_thin1

# RAID dengan LVM
sudo lvcreate --type raid1 -m 1 -L 20G -n lv_raid vg_data
sudo lvcreate --type raid5 -i 3 -L 60G -n lv_raid5 vg_data
```

---

## 7.6 RAID - Redundant Array of Independent Disks

### `mdadm` - MD Array Management
```bash
mdadm [opsi] <perintah>
```
> Manajemen software RAID di Linux

**Membuat RAID:**
```bash
# RAID 0 (Striping) - tidak ada redundansi
sudo mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdb1 /dev/sdc1

# RAID 1 (Mirroring) - redundansi penuh
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1

# RAID 5 - stripping + parity
sudo mdadm --create /dev/md0 --level=5 --raid-devices=3 \
  /dev/sdb1 /dev/sdc1 /dev/sdd1

# RAID 6 - stripping + double parity
sudo mdadm --create /dev/md0 --level=6 --raid-devices=4 \
  /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1

# RAID 10 (1+0) - mirroring + striping
sudo mdadm --create /dev/md0 --level=10 --raid-devices=4 \
  /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1

# Dengan spare drive
sudo mdadm --create /dev/md0 --level=5 --raid-devices=3 \
  --spare-devices=1 /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1
```

**Informasi RAID:**
```bash
cat /proc/mdstat                  # Status semua RAID
sudo mdadm --detail /dev/md0      # Detail RAID tertentu
sudo mdadm --query /dev/md0       # Query info
sudo mdadm --query /dev/sdb1      # Info member device
sudo mdadm --examine /dev/sdb1    # Examine member device
sudo mdadm --detail --scan        # Detail semua array
```

**Manajemen RAID:**
```bash
# Stop RAID
sudo mdadm --stop /dev/md0

# Start RAID
sudo mdadm --start /dev/md0
sudo mdadm --assemble /dev/md0 /dev/sdb1 /dev/sdc1

# Tambah drive ke RAID
sudo mdadm --add /dev/md0 /dev/sde1

# Hapus drive dari RAID
sudo mdadm --remove /dev/md0 /dev/sde1

# Mark drive sebagai faulty
sudo mdadm --fail /dev/md0 /dev/sdb1

# Re-add drive setelah diganti
sudo mdadm --re-add /dev/md0 /dev/sdb1

# Grow RAID (tambah device)
sudo mdadm --grow /dev/md0 --raid-devices=4 --add /dev/sde1

# Expand RAID array
sudo mdadm --grow /dev/md0 --size=max

# Ubah level RAID
sudo mdadm --grow /dev/md0 --level=6

# Simpan konfigurasi
sudo mdadm --detail --scan >> /etc/mdadm/mdadm.conf
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf
```

---

## 7.7 Enkripsi Disk (LUKS)

### `cryptsetup` - LUKS Encryption
```bash
cryptsetup [opsi] <perintah>
```
> Manajemen enkripsi disk dengan LUKS

**Setup LUKS:**
```bash
# Format partisi dengan LUKS
sudo cryptsetup luksFormat /dev/sdb1
sudo cryptsetup luksFormat --type luks2 /dev/sdb1  # LUKS2
sudo cryptsetup luksFormat -c aes-xts-plain64 -s 512 /dev/sdb1  # Cipher tertentu

# Buka (decrypt) device
sudo cryptsetup luksOpen /dev/sdb1 nama_mapped
# Device tersedia di /dev/mapper/nama_mapped

# Tutup (close) device
sudo cryptsetup luksClose nama_mapped

# Status
sudo cryptsetup status nama_mapped
sudo cryptsetup luksDump /dev/sdb1
```

**Manajemen Keys:**
```bash
# Tambah key baru (LUKS mendukung 8 key slots)
sudo cryptsetup luksAddKey /dev/sdb1

# Hapus key
sudo cryptsetup luksRemoveKey /dev/sdb1

# Ubah passphrase
sudo cryptsetup luksChangeKey /dev/sdb1

# Backup LUKS header
sudo cryptsetup luksHeaderBackup /dev/sdb1 --header-backup-file header.bak

# Restore LUKS header
sudo cryptsetup luksHeaderRestore /dev/sdb1 --header-backup-file header.bak

# Cek apakah device adalah LUKS
sudo cryptsetup isLuks /dev/sdb1 && echo "LUKS" || echo "Bukan LUKS"
```

**Auto-mount LUKS di Boot:**
```bash
# /etc/crypttab
# nama_mapped  device              keyfile  opsi
nama_mapped    /dev/sdb1          none     luks
nama_mapped    UUID=xxx           /path/keyfile  luks

# /etc/fstab
/dev/mapper/nama_mapped  /mnt/data  ext4  defaults  0  2
```

**LUKS dengan LVM:**
```bash
# Enkripsi di atas LVM
sudo cryptsetup luksFormat /dev/vg_data/lv_secure
sudo cryptsetup luksOpen /dev/vg_data/lv_secure secure_data
sudo mkfs.ext4 /dev/mapper/secure_data
sudo mount /dev/mapper/secure_data /mnt/secure
```

---

## 7.8 Alat Disk Tambahan

### `dd` - Data Duplicator
```bash
dd [opsi]
```
> Copy dan konversi data di level byte

| Perintah | Penjelasan |
|---|---|
| `dd if=/dev/sda of=backup.img` | Backup disk ke file |
| `dd if=backup.img of=/dev/sda` | Restore dari backup |
| `dd if=/dev/sda of=/dev/sdb` | Clone disk ke disk lain |
| `dd if=/dev/sda1 of=partition.img` | Backup partisi |
| `dd if=/dev/zero of=/dev/sdb` | Hapus disk (isi dengan nol) |
| `dd if=/dev/urandom of=/dev/sdb` | Hapus aman (isi dengan random) |
| `dd if=/dev/zero of=swapfile bs=1M count=4096` | Buat swap file |
| `dd if=/dev/zero of=file.img bs=1M count=100` | Buat file kosong 100MB |
| `dd if=/dev/sda bs=512 count=1 of=mbr.bin` | Backup MBR |
| `dd if=mbr.bin of=/dev/sda bs=512 count=1` | Restore MBR |
| `dd if=image.iso of=/dev/sdb bs=4M status=progress` | Burn ISO ke USB |
| `dd if=/dev/sda \| gzip > backup.img.gz` | Backup + compress |
| `gzip -dc backup.img.gz \| dd of=/dev/sda` | Restore dari compressed |
| `dd if=/dev/sda \| ssh user@host "dd of=/dev/sdb"` | Clone via SSH |

**Parameter dd:**
| Parameter | Penjelasan |
|---|---|
| `if=` | Input file (sumber) |
| `of=` | Output file (tujuan) |
| `bs=` | Block size (ukuran buffer, misal: 512, 4K, 1M, 4M) |
| `count=` | Jumlah blok yang diproses |
| `skip=` | Lewati N blok di awal input |
| `seek=` | Lewati N blok di awal output |
| `status=progress` | Tampilkan progress |
| `status=none` | Tidak tampilkan output |
| `conv=sync` | Pad blok dengan nol |
| `conv=noerror` | Lanjutkan meski ada error |
| `conv=notrunc` | Jangan truncate output |
| `conv=fsync` | Sync setelah selesai |
| `iflag=direct` | Direct I/O untuk input |
| `oflag=direct` | Direct I/O untuk output |

---

### `smartctl` - S.M.A.R.T. Disk Monitoring
```bash
smartctl [opsi] <device>
```
> Monitor kesehatan disk menggunakan S.M.A.R.T.

> 📦 Perlu instalasi: `sudo apt install smartmontools`

| Perintah | Penjelasan |
|---|---|
| `sudo smartctl -a /dev/sda` | Tampilkan semua info S.M.A.R.T. |
| `sudo smartctl -H /dev/sda` | Cek kesehatan (health check) |
| `sudo smartctl -i /dev/sda` | Info device |
| `sudo smartctl -c /dev/sda` | Capabilities |
| `sudo smartctl -A /dev/sda` | Attribute values |
| `sudo smartctl -l error /dev/sda` | Error log |
| `sudo smartctl -l selftest /dev/sda` | Log self-test |
| `sudo smartctl -t short /dev/sda` | Jalankan short self-test |
| `sudo smartctl -t long /dev/sda` | Jalankan long self-test |
| `sudo smartctl -t conveyance /dev/sda` | Conveyance self-test |
| `sudo smartctl -X /dev/sda` | Abort self-test |
| `sudo smartctl -s on /dev/sda` | Aktifkan S.M.A.R.T. |
| `sudo smartctl -s off /dev/sda` | Nonaktifkan S.M.A.R.T. |
| `sudo smartctl -a /dev/sda -d sat` | SAS/SCSI drive |
| `sudo smartctl -a /dev/sda -d nvme` | NVMe drive |

**Konfigurasi smartd:**
```bash
# Aktifkan smartd
sudo systemctl enable smartd
sudo systemctl start smartd

# Konfigurasi /etc/smartd.conf
# Monitor semua disk, email jika ada masalah
DEVICESCAN -m admin@example.com -M exec /usr/share/smartmontools/smartd-runner
```

---

### `hdparm` - Hard Disk Parameters
```bash
hdparm [opsi] <device>
```
> Mengatur parameter hard disk

| Perintah | Penjelasan |
|---|---|
| `sudo hdparm -I /dev/sda` | Info lengkap drive |
| `sudo hdparm -t /dev/sda` | Test kecepatan baca |
| `sudo hdparm -T /dev/sda` | Test kecepatan cache |
| `sudo hdparm -tT /dev/sda` | Test keduanya |
| `sudo hdparm -g /dev/sda` | Tampilkan geometry |
| `sudo hdparm -C /dev/sda` | Status power drive |
| `sudo hdparm -y /dev/sda` | Standby mode |
| `sudo hdparm -Y /dev/sda` | Sleep mode |
| `sudo hdparm -r /dev/sda` | Cek read-only flag |
| `sudo hdparm -r1 /dev/sda` | Set read-only |
| `sudo hdparm -r0 /dev/sda` | Set read-write |
| `sudo hdparm -W /dev/sda` | Cek write caching |
| `sudo hdparm -W1 /dev/sda` | Aktifkan write caching |
| `sudo hdparm -W0 /dev/sda` | Nonaktifkan write caching |
| `sudo hdparm -S 120 /dev/sda` | Spin down setelah 10 menit |
| `sudo hdparm -A1 /dev/sda` | Aktifkan lookahead |

---

### `nvme` - NVMe SSD Tool
```bash
nvme [opsi] <perintah> [device]
```
> Manajemen NVMe SSD

> 📦 Perlu instalasi: `sudo apt install nvme-cli`

| Perintah | Penjelasan |
|---|---|
| `sudo nvme list` | Tampilkan semua NVMe device |
| `sudo nvme id-ctrl /dev/nvme0` | Info controller |
| `sudo nvme id-ns /dev/nvme0n1` | Info namespace |
| `sudo nvme smart-log /dev/nvme0` | S.M.A.R.T. log |
| `sudo nvme error-log /dev/nvme0` | Error log |
| `sudo nvme get-log /dev/nvme0 -i 2` | Log tertentu |
| `sudo nvme format /dev/nvme0n1` | Format NVMe |
| `sudo nvme sanitize /dev/nvme0` | Secure erase |
| `sudo nvme fw-log /dev/nvme0` | Firmware log |
| `sudo nvme fw-activate /dev/nvme0` | Aktifkan firmware |
| `sudo nvme reset /dev/nvme0` | Reset controller |
| `sudo nvme read /dev/nvme0n1 -s 0 -c 1 -z 512` | Baca sektor |

---

### `badblocks` - Check Bad Blocks
```bash
badblocks [opsi] <device>
```
> Mencari bad blocks pada disk

| Perintah | Penjelasan |
|---|---|
| `sudo badblocks -v /dev/sdb` | Scan read-only |
| `sudo badblocks -sv /dev/sdb` | Scan + progress |
| `sudo badblocks -w /dev/sdb` | Write mode (MENGHAPUS DATA!) |
| `sudo badblocks -n /dev/sdb` | Non-destructive write test |
| `sudo badblocks -o badblocks.txt /dev/sdb` | Simpan ke file |
| `sudo badblocks -b 4096 /dev/sdb` | Block size 4096 |
| `sudo badblocks -c 10240 /dev/sdb` | 10240 block per pass |

---

### `e4defrag` - Defragment ext4
```bash
sudo e4defrag /dev/sda1         # Defrag partisi
sudo e4defrag /home             # Defrag mount point
sudo e4defrag /home/user/file   # Defrag file tertentu
sudo e4defrag -c /dev/sda1      # Check fragmentation level
```

---

### `blkid` - Block Device Identifier
```bash
blkid [opsi] [device]
```
> Menampilkan UUID, label, dan tipe filesystem device

| Perintah | Penjelasan |
|---|---|
| `sudo blkid` | Tampilkan semua device |
| `sudo blkid /dev/sda1` | Info device tertentu |
| `sudo blkid -t TYPE=ext4` | Filter berdasarkan tipe |
| `sudo blkid -t LABEL="Data"` | Filter berdasarkan label |
| `sudo blkid -o value -s UUID /dev/sda1` | Hanya tampilkan UUID |
| `sudo blkid -o list` | Format list |
| `sudo blkid -p /dev/sda1` | Probe device |
| `sudo blkid -g` | Garbage collect cache |

**Contoh Output:**
```
/dev/sda1: UUID="abc-def-123" TYPE="ext4" PARTUUID="xyz"
/dev/sda2: UUID="ghi-jkl-456" TYPE="swap" PARTUUID="uvw"
/dev/sdb1: LABEL="Data" UUID="mno-pqr-789" TYPE="xfs"
```

---

### `fstrim` - Discard Unused Blocks (SSD TRIM)
```bash
sudo fstrim [opsi] <mount_point>
```
> Mengirimkan TRIM command ke SSD

| Perintah | Penjelasan |
|---|---|
| `sudo fstrim /` | TRIM root filesystem |
| `sudo fstrim -av` | TRIM semua mounted filesystem yang support |
| `sudo fstrim -v /home` | TRIM dengan verbose |
| `sudo fstrim --minimum=1M /` | Minimum range 1MB |

**Auto TRIM dengan systemd:**
```bash
# Aktifkan fstrim timer (mingguan)
sudo systemctl enable fstrim.timer
sudo systemctl start fstrim.timer

# Cek status
sudo systemctl status fstrim.timer
```

---

### `ioping` - I/O Latency Monitor
```bash
ioping [opsi] <path>
```
> Mengukur latency I/O disk

> 📦 Perlu instalasi: `sudo apt install ioping`

```bash
ioping /dev/sda            # Monitor latency disk
ioping .                   # Monitor direktori saat ini
ioping -c 10 /dev/sda      # 10 request
ioping -D /dev/sda         # Direct I/O
ioping -R /dev/sda         # Disk seek rate
ioping -RL /dev/sda        # Sequential read rate
ioping -W /dev/sda         # Write latency
```

---

### `iostat` - I/O Statistics (Pengingat)
```bash
iostat -x 2                # Extended stats setiap 2 detik
iostat -x -d /dev/sda 2   # Hanya disk tertentu
iostat -m 2                # Output dalam MB/s
```

---

## 7.9 Network Storage

### NFS - Network File System

```bash
# Server NFS
# Install
sudo apt install nfs-kernel-server

# Konfigurasi /etc/exports
/shared 192.168.1.0/24(rw,sync,no_subtree_check)
/public *(ro,sync)
/home/user 192.168.1.100(rw,sync,no_root_squash)

# Opsi exports:
# rw / ro          = read-write / read-only
# sync             = tulis sinkron
# async            = tulis asinkron
# no_subtree_check = meningkatkan reliabilitas
# no_root_squash   = root client = root server
# root_squash      = root client = nobody (default)
# all_squash       = semua user = nobody
# anonuid/anongid  = UID/GID untuk squashed user

# Terapkan konfigurasi
sudo exportfs -a             # Export semua
sudo exportfs -r             # Re-export semua
sudo exportfs -v             # Verbose
sudo exportfs -u 192.168.1.100:/shared  # Unexport

# Start/restart NFS
sudo systemctl start nfs-kernel-server
sudo systemctl enable nfs-kernel-server
sudo systemctl restart nfs-kernel-server

# Lihat export aktif
showmount -e localhost
showmount -e server_ip

# Client NFS
sudo apt install nfs-common

# Mount NFS
sudo mount server:/shared /mnt/nfs
sudo mount -t nfs -o rw,soft server:/shared /mnt/nfs
sudo mount -t nfs4 server:/shared /mnt/nfs

# fstab
server:/shared /mnt/nfs nfs rw,soft,intr,nfsvers=4 0 0

# Info NFS
nfsstat                      # Statistik NFS
nfsstat -c                   # Client stats
nfsstat -s                   # Server stats
```

---

### Samba - Windows File Sharing

```bash
# Install Samba
sudo apt install samba samba-common-bin

# Konfigurasi /etc/samba/smb.conf
[global]
   workgroup = WORKGROUP
   server string = Samba Server
   security = user

[shared]
   comment = Shared Folder
   path = /shared
   read only = no
   browsable = yes
   valid users = user1, @group1

# Test konfigurasi
sudo testparm

# Manage Samba user
sudo smbpasswd -a username    # Tambah user Samba
sudo smbpasswd -e username    # Enable user
sudo smbpasswd -d username    # Disable user
sudo smbpasswd -x username    # Hapus user
sudo pdbedit -L               # List semua user Samba

# Start Samba
sudo systemctl start smbd nmbd
sudo systemctl enable smbd nmbd

# Mount Samba dari Linux
sudo mount -t cifs //server/shared /mnt/samba \
  -o username=user,password=pass,workgroup=WORKGROUP

# fstab
//server/shared /mnt/samba cifs credentials=/etc/samba/credentials,uid=user 0 0

# /etc/samba/credentials
username=user
password=pass

# Cek share
smbclient -L //server -U user
smbclient //server/shared -U user

# Status
sudo smbstatus
```

---

### iSCSI - Internet SCSI

```bash
# Target (Server)
sudo apt install tgt

# Konfigurasi /etc/tgt/targets.conf
<target iqn.2024-01.com.example:storage>
    backing-store /dev/sdb
    initiator-address 192.168.1.0/24
</target>

sudo systemctl start tgtd
sudo tgtadm --mode target --op show   # Tampilkan target

# Initiator (Client)
sudo apt install open-iscsi

# Discovery
sudo iscsiadm -m discovery -t sendtargets -p server_ip

# Login ke target
sudo iscsiadm -m node --login

# Logout
sudo iscsiadm -m node --logout

# List session
sudo iscsiadm -m session

# Status
sudo iscsiadm -m node
```

---

## Ringkasan Perintah Bagian 7

```
df              → Penggunaan ruang disk filesystem
du              → Ukuran file dan direktori
lsblk           → Daftar block device
fdisk           → Editor partisi MBR
gdisk           → Editor partisi GPT
parted          → Editor partisi MBR/GPT
cfdisk          → Editor partisi semi-grafis
mkfs            → Buat filesystem
e2fsck/fsck     → Cek dan repair filesystem
tune2fs         → Atur parameter ext filesystem
xfs_info        → Info filesystem XFS
btrfs           → Manajemen filesystem Btrfs
mount           → Mount filesystem
umount          → Unmount filesystem
findmnt         → Info mount dalam tree format
swapon/swapoff  → Kelola swap
pvcreate        → Buat Physical Volume LVM
vgcreate        → Buat Volume Group LVM
lvcreate        → Buat Logical Volume LVM
lvextend        → Extend Logical Volume
mdadm           → Manajemen software RAID
cryptsetup      → Enkripsi disk LUKS
dd              → Clone/backup disk level byte
smartctl        → Monitor kesehatan disk SMART
hdparm          → Parameter hard disk
nvme            → Manajemen NVMe SSD
badblocks       → Cek bad blocks
blkid           → UUID dan info block device
fstrim          → TRIM SSD
ioping          → Latency I/O disk
```

---

## ✅ Bagian 7 Selesai!

**Lanjut ke Bagian 8: Perintah Sistem & Hardware Info?**
