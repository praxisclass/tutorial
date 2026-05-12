# Bagian 3: Perintah Manajemen User & Permission

---

## 3.1 Informasi User

### `whoami` - Display Current User
```bash
whoami
```
> Menampilkan nama user yang sedang login

```bash
whoami
# user

# Contoh penggunaan dalam script
if [ "$(whoami)" != "root" ]; then
    echo "Harus dijalankan sebagai root!"
    exit 1
fi
```

---

### `who` - Show Who is Logged In
```bash
who [opsi]
```
> Menampilkan daftar user yang sedang login

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `who` | Tampilkan user yang sedang login |
| `who -a` | Tampilkan semua informasi |
| `who -b` | Tampilkan waktu boot terakhir |
| `who -d` | Tampilkan proses yang mati |
| `who -H` | Tampilkan header kolom |
| `who -l` | Tampilkan login yang menunggu |
| `who -m` | Tampilkan hanya user saat ini |
| `who -q` | Tampilkan nama user dan jumlahnya saja |
| `who -r` | Tampilkan run level saat ini |
| `who -T` | Tampilkan status terminal (+ = writable, - = tidak) |
| `who -u` | Tampilkan waktu idle |
| `who am i` | Tampilkan info user saat ini |

**Contoh Output:**
```
user     pts/0        2024-01-15 10:30 (192.168.1.100)
root     pts/1        2024-01-15 09:00 (192.168.1.101)
```

---

### `w` - Show Who is Logged In and What They Do
```bash
w [opsi] [user]
```
> Menampilkan user yang login beserta aktivitasnya

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `w` | Tampilkan semua user yang login + aktivitas |
| `w username` | Tampilkan info user tertentu |
| `w -h` | Tanpa header |
| `w -s` | Format pendek (short) |
| `w -f` | Tampilkan/sembunyikan kolom FROM |
| `w -i` | Tampilkan IP address (bukan hostname) |

**Contoh Output:**
```
 10:30:00 up 2 days,  3:45,  2 users,  load average: 0.15, 0.10, 0.09
USER     TTY      FROM             LOGIN@   IDLE JCPU   PCPU WHAT
user     pts/0    192.168.1.100    10:00    1:00  0.05s  0.05s bash
root     pts/1    192.168.1.101    09:00    0.00s 0.10s  0.02s w
```

---

### `id` - Display User and Group ID
```bash
id [opsi] [username]
```
> Menampilkan UID, GID, dan grup dari user

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `id` | Tampilkan info user saat ini |
| `id username` | Tampilkan info user tertentu |
| `id -u` | Tampilkan hanya UID |
| `id -g` | Tampilkan hanya GID utama |
| `id -G` | Tampilkan semua GID grup |
| `id -un` | Tampilkan nama user (bukan angka) |
| `id -gn` | Tampilkan nama grup utama |
| `id -Gn` | Tampilkan nama semua grup |
| `id -r` | Tampilkan real ID (bukan effective ID) |

**Contoh Output:**
```
uid=1000(user) gid=1000(user) groups=1000(user),4(adm),24(cdrom),27(sudo),46(plugdev)
```

---

### `finger` - User Information Lookup
```bash
finger [opsi] [username]
```
> Menampilkan informasi detail tentang user

> 📦 Perlu instalasi: `sudo apt install finger`

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `finger` | Tampilkan info semua user yang login |
| `finger username` | Info user tertentu |
| `finger -l` | Format panjang (long) |
| `finger -s` | Format pendek (short) |
| `finger -m username` | Cocokkan hanya username (bukan nama lengkap) |

---

### `last` - Show Last Logins
```bash
last [opsi] [username]
```
> Menampilkan riwayat login user

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `last` | Tampilkan semua riwayat login |
| `last username` | Riwayat login user tertentu |
| `last -n 10` | Tampilkan 10 login terakhir |
| `last -a` | Tampilkan hostname di kolom terakhir |
| `last -d` | Tampilkan hostname sebagai IP |
| `last -F` | Tampilkan timestamp lengkap |
| `last -i` | Tampilkan IP address |
| `last -x` | Tampilkan shutdown dan runlevel |
| `last -R` | Sembunyikan kolom hostname |
| `last -w` | Tampilkan nama user dan domain lengkap |
| `last reboot` | Tampilkan riwayat reboot |
| `last -f /var/log/wtmp` | Baca dari file log tertentu |

---

### `lastlog` - Show Last Login of All Users
```bash
lastlog [opsi]
```
> Menampilkan login terakhir untuk setiap user

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `lastlog` | Tampilkan login terakhir semua user |
| `lastlog -u username` | Login terakhir user tertentu |
| `lastlog -b 7` | User yang tidak login dalam 7 hari |
| `lastlog -t 7` | User yang login dalam 7 hari terakhir |

---

### `lastb` - Show Bad Login Attempts
```bash
lastb [opsi]
```
> Menampilkan riwayat percobaan login yang gagal

```bash
sudo lastb              # Tampilkan semua percobaan login gagal
sudo lastb -n 20        # 20 percobaan terakhir
sudo lastb username     # Percobaan login user tertentu
```

---

### `logname` - Print Current Login Name
```bash
logname
```
> Menampilkan nama login user saat ini

---

### `users` - Print Logged In Users
```bash
users
```
> Menampilkan nama user yang sedang login (singkat)

```bash
users
# user root user2
```

---

## 3.2 Manajemen User

### `useradd` - Add User
```bash
useradd [opsi] <username>
```
> Membuat user baru (perintah tingkat rendah)

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `useradd username` | Buat user baru dengan pengaturan default |
| `useradd -m username` | Buat user + direktori home |
| `useradd -m -d /home/custom username` | Direktori home kustom |
| `useradd -s /bin/bash username` | Tentukan shell default |
| `useradd -s /sbin/nologin username` | User tanpa akses login |
| `useradd -g groupname username` | Tentukan grup utama |
| `useradd -G sudo,www-data username` | Tambahkan ke grup tambahan |
| `useradd -u 1500 username` | Tentukan UID tertentu |
| `useradd -c "Nama Lengkap" username` | Tambahkan komentar/nama lengkap |
| `useradd -e 2024-12-31 username` | Tanggal kadaluarsa akun |
| `useradd -f 30 username` | Hari setelah password kadaluarsa sebelum nonaktif |
| `useradd -r username` | Buat system account (UID < 1000) |
| `useradd -M username` | Jangan buat direktori home |
| `useradd -k /etc/skel username` | Gunakan skeleton directory tertentu |

**Contoh Lengkap:**
```bash
sudo useradd -m -s /bin/bash -g users -G sudo,www-data \
  -c "John Doe" -u 1500 john
```

---

### `adduser` - Add User (High Level / Interactive)
```bash
adduser [opsi] <username>
```
> Membuat user baru secara interaktif (lebih mudah dari useradd)

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `adduser username` | Buat user baru secara interaktif |
| `adduser --home /path/home username` | Direktori home kustom |
| `adduser --shell /bin/zsh username` | Shell kustom |
| `adduser --no-create-home username` | Tanpa direktori home |
| `adduser --system username` | Buat system user |
| `adduser --uid 1500 username` | UID tertentu |
| `adduser --gid 1000 username` | GID tertentu |
| `adduser --ingroup groupname username` | Masukkan ke grup |
| `adduser --disabled-login username` | Nonaktifkan login |
| `adduser --disabled-password username` | Tanpa password |
| `adduser --gecos "Nama Lengkap" username` | Isi info GECOS otomatis |
| `adduser username groupname` | Tambahkan user ke grup |

---

### `usermod` - Modify User
```bash
usermod [opsi] <username>
```
> Mengubah pengaturan user yang sudah ada

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `usermod -l newname oldname` | Ubah username |
| `usermod -d /home/baru username` | Ubah direktori home |
| `usermod -d /home/baru -m username` | Ubah & pindahkan direktori home |
| `usermod -s /bin/zsh username` | Ubah shell default |
| `usermod -g newgroup username` | Ubah grup utama |
| `usermod -G sudo,www-data username` | Set grup tambahan (timpa yang lama) |
| `usermod -aG sudo username` | Tambahkan ke grup tanpa menghapus grup lain |
| `usermod -aG docker username` | Tambahkan user ke grup docker |
| `usermod -u 1500 username` | Ubah UID |
| `usermod -c "Nama Baru" username` | Ubah komentar/nama lengkap |
| `usermod -e 2024-12-31 username` | Set tanggal kadaluarsa |
| `usermod -L username` | Lock (kunci) akun user |
| `usermod -U username` | Unlock akun user |
| `usermod -p 'hash' username` | Set password terenkripsi |
| `usermod -f 30 username` | Ubah inactive period |

---

### `userdel` - Delete User
```bash
userdel [opsi] <username>
```
> Menghapus user

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `userdel username` | Hapus user (direktori home tetap ada) |
| `userdel -r username` | Hapus user beserta direktori home dan mail spool |
| `userdel -f username` | Force hapus meski user masih login |
| `userdel -f -r username` | Force hapus + direktori home |

> ⚠️ **PERINGATAN:** `-r` akan menghapus semua data user secara permanen!

---

### `deluser` - Delete User (High Level)
```bash
deluser [opsi] <username>
```
> Menghapus user (versi lebih mudah)

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `deluser username` | Hapus user |
| `deluser --remove-home username` | Hapus beserta direktori home |
| `deluser --remove-all-files username` | Hapus semua file milik user |
| `deluser --backup username` | Backup file sebelum dihapus |
| `deluser --backup-to /path/ username` | Tentukan lokasi backup |
| `deluser username groupname` | Hapus user dari grup |

---

### `passwd` - Change Password
```bash
passwd [opsi] [username]
```
> Mengubah password user

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `passwd` | Ubah password user saat ini |
| `passwd username` | Ubah password user tertentu (perlu root) |
| `passwd -l username` | Lock (kunci) akun user |
| `passwd -u username` | Unlock akun user |
| `passwd -d username` | Hapus password (akun tanpa password) |
| `passwd -e username` | Paksa user ganti password saat login berikutnya |
| `passwd -n 7 username` | Minimum hari sebelum password bisa diubah |
| `passwd -x 90 username` | Maximum hari sebelum password harus diubah |
| `passwd -w 7 username` | Hari sebelum kadaluarsa untuk memberi peringatan |
| `passwd -i 14 username` | Hari setelah kadaluarsa sebelum akun dinonaktifkan |
| `passwd -S username` | Tampilkan status password |
| `passwd -S -a` | Tampilkan status password semua user |

**Status Password:**
```
P = Password diset
L = Locked
NP = No Password
```

---

### `chpasswd` - Change Passwords in Batch
```bash
chpasswd [opsi]
```
> Mengubah password banyak user sekaligus dari file

```bash
# Format: username:password
echo "user1:password123" | sudo chpasswd
echo "user2:newpass456" | sudo chpasswd

# Dari file
sudo chpasswd < users_passwords.txt

# Isi file users_passwords.txt:
# user1:password1
# user2:password2
# user3:password3
```

---

### `chage` - Change User Password Expiry
```bash
chage [opsi] <username>
```
> Mengatur kebijakan kadaluarsa password user

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `chage username` | Mode interaktif |
| `chage -l username` | Tampilkan info kadaluarsa password |
| `chage -d 0 username` | Paksa ganti password saat login berikutnya |
| `chage -d 2024-01-15 username` | Set tanggal terakhir ganti password |
| `chage -m 7 username` | Minimum hari antar ganti password |
| `chage -M 90 username` | Maximum hari sebelum password kadaluarsa |
| `chage -W 14 username` | Hari peringatan sebelum kadaluarsa |
| `chage -I 30 username` | Hari tidak aktif setelah kadaluarsa |
| `chage -E 2024-12-31 username` | Tanggal kadaluarsa akun |
| `chage -E -1 username` | Nonaktifkan kadaluarsa akun |

**Contoh Output `chage -l username`:**
```
Last password change                    : Jan 15, 2024
Password expires                        : Apr 14, 2024
Password inactive                       : never
Account expires                         : never
Minimum number of days between change   : 7
Maximum number of days between change   : 90
Number of days of warning before expiry : 14
```

---

### `su` - Switch User
```bash
su [opsi] [username]
```
> Beralih ke user lain

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `su` | Beralih ke root |
| `su username` | Beralih ke user tertentu |
| `su -` | Beralih ke root dengan environment root |
| `su - username` | Beralih ke user dengan environment lengkap |
| `su -c "perintah" username` | Jalankan perintah sebagai user lain |
| `su -s /bin/bash username` | Gunakan shell tertentu |
| `su -l username` | Login shell (sama dengan `su -`) |
| `su -m username` | Pertahankan environment saat ini |

---

### `sudo` - Execute Command as Another User
```bash
sudo [opsi] <perintah>
```
> Menjalankan perintah dengan hak superuser atau user lain

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `sudo perintah` | Jalankan perintah sebagai root |
| `sudo -u username perintah` | Jalankan sebagai user tertentu |
| `sudo -i` | Login shell sebagai root |
| `sudo -s` | Shell sebagai root |
| `sudo -l` | Tampilkan perintah yang diizinkan untuk user saat ini |
| `sudo -l -U username` | Tampilkan izin untuk user tertentu |
| `sudo -k` | Invalidasi timestamp (perlu password lagi) |
| `sudo -v` | Perpanjang timeout sudo |
| `sudo -n perintah` | Non-interactive (gagal jika perlu password) |
| `sudo -b perintah` | Jalankan di background |
| `sudo !!` | Jalankan perintah sebelumnya dengan sudo |
| `sudo su -` | Beralih ke root shell |
| `sudo -e file` | Edit file sebagai root (sudoedit) |

**Konfigurasi sudoers:**
```bash
# Edit file sudoers
sudo visudo

# Format:
# user/group  host=(runas)  perintah
username ALL=(ALL:ALL) ALL          # User bisa jalankan semua perintah
username ALL=(ALL) NOPASSWD: ALL    # Tanpa password
username ALL=(ALL) /bin/systemctl   # Hanya perintah tertentu
%groupname ALL=(ALL:ALL) ALL        # Izin untuk grup
```

---

## 3.3 Manajemen Grup

### `groupadd` - Add Group
```bash
groupadd [opsi] <groupname>
```
> Membuat grup baru

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `groupadd groupname` | Buat grup baru |
| `groupadd -g 1500 groupname` | Buat grup dengan GID tertentu |
| `groupadd -r groupname` | Buat system group |
| `groupadd -f groupname` | Force (tidak error jika grup sudah ada) |
| `groupadd -p password groupname` | Set password grup |

---

### `groupmod` - Modify Group
```bash
groupmod [opsi] <groupname>
```
> Mengubah pengaturan grup

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `groupmod -n newname oldname` | Ubah nama grup |
| `groupmod -g 1600 groupname` | Ubah GID |
| `groupmod -p password groupname` | Ubah password grup |

---

### `groupdel` - Delete Group
```bash
groupdel <groupname>
```
> Menghapus grup

```bash
sudo groupdel groupname
# Catatan: tidak bisa hapus grup utama user yang masih aktif
```

---

### `groups` - Show Group Membership
```bash
groups [username]
```
> Menampilkan grup yang dimiliki user

```bash
groups                  # Grup user saat ini
groups username         # Grup user tertentu
groups user1 user2      # Grup beberapa user
```

---

### `gpasswd` - Administer Group
```bash
gpasswd [opsi] <groupname>
```
> Mengelola keanggotaan grup

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `gpasswd groupname` | Set password grup |
| `gpasswd -a username groupname` | Tambahkan user ke grup |
| `gpasswd -d username groupname` | Hapus user dari grup |
| `gpasswd -A username groupname` | Jadikan user sebagai admin grup |
| `gpasswd -M user1,user2 groupname` | Set anggota grup (timpa semua) |
| `gpasswd -r groupname` | Hapus password grup |
| `gpasswd -R groupname` | Restrict akses grup |

---

### `newgrp` - Change Group ID
```bash
newgrp <groupname>
```
> Berganti ke grup lain dalam sesi saat ini

```bash
newgrp docker    # Berganti ke grup docker
newgrp -        # Reset ke login grup
```

---

## 3.4 File Konfigurasi User & Grup

### File `/etc/passwd`
```bash
cat /etc/passwd
# Format: username:x:UID:GID:GECOS:home:shell
# Contoh:
# root:x:0:0:root:/root:/bin/bash
# user:x:1000:1000:User Name,,,:/home/user:/bin/bash
# nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin

# Lihat semua username
cut -d: -f1 /etc/passwd

# Lihat username, UID, home directory
awk -F: '{print $1, $3, $6}' /etc/passwd | column -t
```

### File `/etc/shadow`
```bash
sudo cat /etc/shadow
# Format: username:password_hash:lastchange:min:max:warn:inactive:expire:reserved
# Contoh:
# root:$6$hash...:19371:0:99999:7:::
# user:$6$hash...:19371:7:90:14:30:19723:

# Hanya root yang bisa membaca file ini
```

### File `/etc/group`
```bash
cat /etc/group
# Format: groupname:x:GID:members
# Contoh:
# root:x:0:
# sudo:x:27:user
# docker:x:998:user

# Lihat semua nama grup
cut -d: -f1 /etc/group

# Lihat grup beserta anggotanya
awk -F: '{print $1, $4}' /etc/group | column -t
```

### File `/etc/gshadow`
```bash
sudo cat /etc/gshadow
# Format: groupname:password:admin:members
```

---

## 3.5 Permission File & Direktori

### `chmod` - Change File Permissions
```bash
chmod [opsi] <mode> <file>
```
> Mengubah permission file atau direktori

#### Mode Simbolik

**Format:** `[who][operator][permission]`

| Who | Penjelasan |
|---|---|
| `u` | User (pemilik) |
| `g` | Group |
| `o` | Others (lainnya) |
| `a` | All (semua) |

| Operator | Penjelasan |
|---|---|
| `+` | Tambah permission |
| `-` | Hapus permission |
| `=` | Set permission tepat |

| Permission | Penjelasan |
|---|---|
| `r` | Read (baca) |
| `w` | Write (tulis) |
| `x` | Execute (eksekusi) |
| `s` | SetUID/SetGID |
| `t` | Sticky bit |

**Contoh Mode Simbolik:**
| Perintah | Penjelasan |
|---|---|
| `chmod u+x file.sh` | Tambah execute untuk pemilik |
| `chmod g+w file.txt` | Tambah write untuk grup |
| `chmod o-r file.txt` | Hapus read untuk others |
| `chmod a+x file.sh` | Tambah execute untuk semua |
| `chmod u=rwx,g=rx,o=r file` | Set permission spesifik |
| `chmod go-w file.txt` | Hapus write dari grup dan others |
| `chmod a-x,u+x file.sh` | Hapus execute semua, tambah untuk user |
| `chmod u+s file` | Set SUID bit |
| `chmod g+s dir/` | Set SGID bit |
| `chmod +t dir/` | Set sticky bit |

#### Mode Oktal

**Nilai Permission:**
```
r = 4
w = 2
x = 1
- = 0

Contoh:
rwx = 4+2+1 = 7
rw- = 4+2+0 = 6
r-x = 4+0+1 = 5
r-- = 4+0+0 = 4
--- = 0+0+0 = 0
```

**Permission Umum:**
| Oktal | Simbolik | Penjelasan |
|---|---|---|
| `777` | `rwxrwxrwx` | Semua bisa baca, tulis, eksekusi |
| `755` | `rwxr-xr-x` | Pemilik penuh, lain bisa baca+eksekusi |
| `644` | `rw-r--r--` | Pemilik baca+tulis, lain baca saja |
| `600` | `rw-------` | Hanya pemilik yang bisa baca+tulis |
| `700` | `rwx------` | Hanya pemilik yang bisa akses penuh |
| `666` | `rw-rw-rw-` | Semua bisa baca+tulis |
| `555` | `r-xr-xr-x` | Semua bisa baca+eksekusi |
| `444` | `r--r--r--` | Semua hanya bisa baca |
| `400` | `r--------` | Hanya pemilik yang bisa baca |
| `000` | `----------` | Tidak ada yang bisa akses |

**Contoh Mode Oktal:**
| Perintah | Penjelasan |
|---|---|
| `chmod 755 file.sh` | rwxr-xr-x |
| `chmod 644 file.txt` | rw-r--r-- |
| `chmod 600 id_rsa` | rw------- (SSH private key) |
| `chmod 777 folder/` | Semua akses penuh (tidak disarankan) |
| `chmod 4755 file` | SUID + rwxr-xr-x |
| `chmod 2755 dir/` | SGID + rwxr-xr-x |
| `chmod 1755 dir/` | Sticky + rwxr-xr-x |

**Opsi chmod:**
| Opsi | Penjelasan |
|---|---|
| `chmod -R 755 folder/` | Rekursif ke semua file & subdirektori |
| `chmod -v 644 file.txt` | Verbose |
| `chmod -c 644 file.txt` | Tampilkan hanya perubahan yang terjadi |
| `chmod --reference=ref.txt file.txt` | Salin permission dari file referensi |

#### Special Permissions

**SUID (Set User ID) - Nilai Oktal: 4000**
```bash
chmod u+s /usr/bin/program
chmod 4755 /usr/bin/program
# File dieksekusi dengan permission pemiliknya
# Contoh: /usr/bin/passwd (harus jalan sebagai root)
```

**SGID (Set Group ID) - Nilai Oktal: 2000**
```bash
chmod g+s /shared/folder
chmod 2755 /shared/folder
# File baru dalam direktori mewarisi grup direktori
```

**Sticky Bit - Nilai Oktal: 1000**
```bash
chmod +t /tmp
chmod 1777 /tmp
# Hanya pemilik yang bisa hapus file miliknya
# Biasa digunakan di /tmp
```

---

### `chown` - Change File Owner
```bash
chown [opsi] <owner>[:<group>] <file>
```
> Mengubah pemilik (owner) dan/atau grup file

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `chown user file.txt` | Ubah pemilik file |
| `chown user:group file.txt` | Ubah pemilik dan grup |
| `chown :group file.txt` | Ubah hanya grup |
| `chown user: file.txt` | Ubah pemilik, grup ke login group user |
| `chown -R user:group folder/` | Rekursif |
| `chown -v user file.txt` | Verbose |
| `chown -c user file.txt` | Tampilkan hanya perubahan |
| `chown --reference=ref.txt file.txt` | Salin owner dari file referensi |
| `chown 1000:1000 file.txt` | Gunakan UID:GID numerik |

---

### `chgrp` - Change Group Ownership
```bash
chgrp [opsi] <group> <file>
```
> Mengubah grup pemilik file

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `chgrp group file.txt` | Ubah grup file |
| `chgrp -R group folder/` | Rekursif |
| `chgrp -v group file.txt` | Verbose |
| `chgrp --reference=ref.txt file.txt` | Salin grup dari file referensi |

---

### `umask` - Set Default Permission Mask
```bash
umask [nilai]
```
> Mengatur permission default untuk file dan direktori yang baru dibuat

```bash
umask              # Tampilkan umask saat ini
umask 022          # Set umask ke 022
umask -S           # Tampilkan dalam format simbolik

# Cara kerja umask:
# File default max: 666
# Dir  default max: 777
# umask 022:
#   File: 666 - 022 = 644 (rw-r--r--)
#   Dir:  777 - 022 = 755 (rwxr-xr-x)

# umask 027:
#   File: 666 - 027 = 640 (rw-r-----)
#   Dir:  777 - 027 = 750 (rwxr-x---)
```

**Nilai umask Umum:**
| umask | File | Direktori | Penjelasan |
|---|---|---|---|
| `022` | `644` | `755` | Default umum |
| `027` | `640` | `750` | Lebih ketat |
| `077` | `600` | `700` | Sangat ketat (hanya pemilik) |
| `002` | `664` | `775` | Cocok untuk grup kerja |
| `000` | `666` | `777` | Tidak ada mask |

---

### `lsattr` - List File Attributes
```bash
lsattr [opsi] [file]
```
> Menampilkan atribut file di filesystem ext

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `lsattr file.txt` | Tampilkan atribut file |
| `lsattr -R folder/` | Rekursif |
| `lsattr -a folder/` | Tampilkan termasuk file tersembunyi |
| `lsattr -d folder/` | Tampilkan atribut direktori (bukan isinya) |
| `lsattr -v file.txt` | Tampilkan versi file |

**Contoh Output:**
```
----i--------e-- file.txt
```

---

### `chattr` - Change File Attributes
```bash
chattr [opsi] <mode> <file>
```
> Mengubah atribut file di filesystem ext

**Atribut:**
| Atribut | Penjelasan |
|---|---|
| `i` | Immutable: tidak bisa diubah, dihapus, atau di-rename |
| `a` | Append only: hanya bisa ditambahkan, tidak bisa diubah |
| `e` | Extent format (diset otomatis oleh filesystem) |
| `s` | Secure deletion (data di-overwrite saat dihapus) |
| `u` | Undeletable (data disimpan saat dihapus) |
| `c` | Compressed otomatis |
| `d` | No dump (tidak di-backup dengan dump) |
| `S` | Synchronous updates |
| `A` | No atime update |
| `D` | Synchronous directory updates |
| `j` | Data journaling |

**Contoh:**
| Perintah | Penjelasan |
|---|---|
| `chattr +i file.txt` | Set immutable (file tidak bisa diubah) |
| `chattr -i file.txt` | Hapus immutable |
| `chattr +a log.txt` | Set append only |
| `chattr -a log.txt` | Hapus append only |
| `chattr +i -R folder/` | Set immutable rekursif |
| `chattr =i file.txt` | Set tepat atribut |

---

### `getfacl` & `setfacl` - ACL (Access Control List)
```bash
# Tampilkan ACL
getfacl file.txt
getfacl -R folder/
getfacl -t file.txt       # Format tabel
getfacl -n file.txt       # Tampilkan UID/GID numerik

# Set ACL
setfacl -m u:username:rwx file.txt    # Berikan rwx ke user tertentu
setfacl -m g:groupname:rw file.txt    # Berikan rw ke grup tertentu
setfacl -m o::r file.txt              # Set permission others
setfacl -x u:username file.txt        # Hapus ACL user tertentu
setfacl -b file.txt                   # Hapus semua ACL
setfacl -R -m u:user:rwx folder/     # Rekursif
setfacl -d -m u:user:rwx folder/     # Set default ACL (untuk file baru)

# Salin ACL dari satu file ke file lain
getfacl file1.txt | setfacl --set-file=- file2.txt
```

**Contoh Output getfacl:**
```
# file: file.txt
# owner: user
# group: user
user::rw-
user:john:rwx
group::r--
group:developers:rw-
mask::rwx
other::r--
```

---

## 3.6 Informasi Sesi & Terminal

### `tty` - Print Terminal Name
```bash
tty
```
> Menampilkan nama terminal yang digunakan

```bash
tty
# /dev/pts/0
```

---

### `stty` - Change Terminal Settings
```bash
stty [opsi]
```
> Mengatur pengaturan terminal

**Opsi:**
| Opsi | Penjelasan |
|---|---|
| `stty` | Tampilkan pengaturan terminal saat ini |
| `stty -a` | Tampilkan semua pengaturan |
| `stty size` | Tampilkan ukuran terminal (baris kolom) |
| `stty rows 50` | Set jumlah baris terminal |
| `stty cols 120` | Set jumlah kolom terminal |
| `stty echo` | Aktifkan echo input |
| `stty -echo` | Nonaktifkan echo input (untuk password) |
| `stty speed` | Tampilkan kecepatan baud |
| `stty sane` | Reset terminal ke pengaturan normal |

---

### `write` - Send Message to User
```bash
write <username> [tty]
```
> Mengirim pesan ke terminal user lain

```bash
write username          # Kirim pesan ke user
write username pts/1    # Kirim ke terminal tertentu
# Ketik pesan, tekan Enter untuk kirim
# Ctrl+D untuk selesai
```

---

### `wall` - Broadcast Message
```bash
wall [opsi] [pesan]
```
> Mengirim pesan ke semua user yang login

```bash
wall "Server akan di-restart dalam 5 menit!"
wall < pesan.txt
sudo wall "Maintenance dimulai!"
```

---

### `mesg` - Control Terminal Messages
```bash
mesg [y|n]
```
> Mengizinkan atau menolak pesan dari user lain

```bash
mesg y      # Izinkan menerima pesan
mesg n      # Tolak pesan dari user lain
mesg        # Cek status saat ini
```

---

## 3.7 Ringkasan Permission

### Tabel Lengkap Permission

```
Format ls -l:
-rwxrwxrwx  1  user  group  size  date  filename
│││││││││└─ others execute
││││││││└── others write
│││││││└─── others read
││││││└──── group execute
│││││└───── group write
││││└────── group read
│││└─────── user execute
││└──────── user write
│└───────── user read
└────────── type: - file, d dir, l symlink, b block, c char, p pipe, s socket
```

### Cheatsheet Permission Oktal

```
7 = rwx (read+write+execute)
6 = rw- (read+write)
5 = r-x (read+execute)
4 = r-- (read only)
3 = -wx (write+execute)
2 = -w- (write only)
1 = --x (execute only)
0 = --- (no permission)

Permission umum:
chmod 755 → script/program (rwxr-xr-x)
chmod 644 → file teks (rw-r--r--)
chmod 600 → file privat (rw-------)
chmod 777 → ⚠️ terlalu terbuka!
chmod 400 → read-only ketat (r--------)
```

---

## Ringkasan Perintah Bagian 3

```
whoami       → Nama user saat ini
who          → User yang sedang login
w            → User login + aktivitas
id           → UID, GID, dan grup user
last         → Riwayat login
lastlog      → Login terakhir semua user
useradd      → Buat user baru (low level)
adduser      → Buat user baru (interaktif)
usermod      → Modifikasi user
userdel      → Hapus user
passwd       → Ubah password
chage        → Atur kadaluarsa password
su           → Ganti user
sudo         → Jalankan perintah sebagai root
groupadd     → Buat grup baru
groupmod     → Modifikasi grup
groupdel     → Hapus grup
groups       → Lihat keanggotaan grup
gpasswd      → Kelola anggota grup
chmod        → Ubah permission file
chown        → Ubah pemilik file
chgrp        → Ubah grup file
umask        → Atur permission default
lsattr       → Lihat atribut file
chattr       → Ubah atribut file
getfacl      → Lihat ACL
setfacl      → Set ACL
tty          → Nama terminal
wall         → Broadcast pesan
write        → Kirim pesan ke user
mesg         → Kontrol penerimaan pesan
```

---

## ✅ Bagian 3 Selesai!

**Lanjut ke Bagian 4: Perintah Manajemen Proses?**
