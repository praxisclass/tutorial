# Bagian 5: Perintah Manajemen Paket

---

## 5.1 APT - Advanced Package Tool (Debian/Ubuntu)

### `apt` - Modern Package Manager
```bash
apt [opsi] <perintah>
```
> Package manager utama untuk sistem berbasis Debian/Ubuntu

---

### Update & Upgrade

| Perintah | Penjelasan |
|---|---|
| `sudo apt update` | Perbarui daftar paket dari repository |
| `sudo apt upgrade` | Upgrade semua paket yang bisa diupgrade |
| `sudo apt full-upgrade` | Upgrade + hapus paket yang menghalangi |
| `sudo apt dist-upgrade` | Upgrade distribusi (sama dengan full-upgrade) |
| `sudo apt update && sudo apt upgrade -y` | Update + upgrade sekaligus |
| `sudo apt update && sudo apt full-upgrade -y` | Update + full upgrade |

---

### Instalasi Paket

| Perintah | Penjelasan |
|---|---|
| `sudo apt install <paket>` | Instal paket |
| `sudo apt install paket1 paket2 paket3` | Instal beberapa paket |
| `sudo apt install -y <paket>` | Instal tanpa konfirmasi |
| `sudo apt install --no-install-recommends <paket>` | Instal tanpa paket rekomendasi |
| `sudo apt install <paket>=<versi>` | Instal versi tertentu |
| `sudo apt install ./paket.deb` | Instal file .deb lokal |
| `sudo apt install -f` | Perbaiki dependensi yang rusak |
| `sudo apt install --reinstall <paket>` | Instal ulang paket |
| `sudo apt install --dry-run <paket>` | Simulasi instalasi (tidak benar-benar instal) |
| `sudo apt install -s <paket>` | Simulasi (sama dengan --dry-run) |

---

### Menghapus Paket

| Perintah | Penjelasan |
|---|---|
| `sudo apt remove <paket>` | Hapus paket (konfigurasi tetap ada) |
| `sudo apt purge <paket>` | Hapus paket beserta konfigurasinya |
| `sudo apt autoremove` | Hapus paket yang tidak diperlukan lagi |
| `sudo apt autoremove --purge` | Hapus + konfigurasi paket tidak diperlukan |
| `sudo apt remove paket1 paket2` | Hapus beberapa paket |
| `sudo apt purge paket1 paket2` | Purge beberapa paket |

---

### Pencarian & Informasi Paket

| Perintah | Penjelasan |
|---|---|
| `apt search <kata_kunci>` | Cari paket berdasarkan kata kunci |
| `apt show <paket>` | Tampilkan info detail paket |
| `apt list` | Tampilkan semua paket yang tersedia |
| `apt list --installed` | Tampilkan paket yang terinstal |
| `apt list --upgradable` | Tampilkan paket yang bisa diupgrade |
| `apt list --all-versions` | Tampilkan semua versi yang tersedia |
| `apt depends <paket>` | Tampilkan dependensi paket |
| `apt rdepends <paket>` | Tampilkan paket yang membutuhkan paket ini |
| `apt-cache policy <paket>` | Tampilkan versi dan prioritas paket |
| `apt-cache showpkg <paket>` | Info lengkap paket dari cache |
| `apt-cache stats` | Statistik cache paket |
| `apt-cache pkgnames` | Daftar semua nama paket |
| `apt-cache pkgnames <prefix>` | Paket yang diawali prefix tertentu |
| `apt-cache search <kata>` | Cari di cache |
| `apt-cache show <paket>` | Info paket dari cache |
| `apt-cache depends <paket>` | Dependensi dari cache |

---

### Manajemen Cache & Repository

| Perintah | Penjelasan |
|---|---|
| `sudo apt clean` | Hapus semua file paket yang diunduh |
| `sudo apt autoclean` | Hapus file paket lama yang tidak diperlukan |
| `sudo apt-get check` | Cek dependensi yang rusak |
| `sudo apt-mark hold <paket>` | Tandai paket agar tidak diupgrade |
| `sudo apt-mark unhold <paket>` | Hapus tanda hold |
| `sudo apt-mark showhold` | Tampilkan paket yang di-hold |
| `sudo apt-mark auto <paket>` | Tandai sebagai otomatis (bisa dihapus autoremove) |
| `sudo apt-mark manual <paket>` | Tandai sebagai manual (tidak dihapus autoremove) |
| `sudo apt-mark showmanual` | Tampilkan paket yang diinstal manual |
| `sudo apt-mark showauto` | Tampilkan paket yang diinstal otomatis |

---

### File Konfigurasi APT

```bash
# Repository sources
/etc/apt/sources.list
/etc/apt/sources.list.d/

# Format sources.list:
# deb [opsi] url distribusi komponen
# deb http://archive.ubuntu.com/ubuntu jammy main restricted
# deb-src http://archive.ubuntu.com/ubuntu jammy main restricted

# Preferensi
/etc/apt/preferences
/etc/apt/preferences.d/

# Konfigurasi
/etc/apt/apt.conf
/etc/apt/apt.conf.d/

# Trusted keys
/etc/apt/trusted.gpg
/etc/apt/trusted.gpg.d/
```

---

### Menambah Repository

```bash
# Cara 1: Edit sources.list langsung
sudo nano /etc/apt/sources.list

# Cara 2: Tambahkan file di sources.list.d
sudo nano /etc/apt/sources.list.d/custom.list

# Cara 3: Gunakan add-apt-repository
sudo add-apt-repository ppa:user/repo
sudo add-apt-repository "deb http://url/ubuntu focal main"
sudo add-apt-repository --remove ppa:user/repo

# Cara 4: Menggunakan format baru (.sources)
sudo nano /etc/apt/sources.list.d/custom.sources

# Tambahkan GPG key
curl -fsSL https://url/gpg | sudo gpg --dearmor -o /usr/share/keyrings/key.gpg
wget -qO- https://url/gpg | sudo gpg --dearmor -o /usr/share/keyrings/key.gpg

# Format dengan signed-by:
# deb [signed-by=/usr/share/keyrings/key.gpg] https://url/ubuntu focal main
```

---

## 5.2 APT-GET - Classic Package Tool

### `apt-get` - Classic APT Command
```bash
apt-get [opsi] <perintah>
```
> Versi klasik apt (masih sering digunakan di script)

| Perintah | Penjelasan |
|---|---|
| `sudo apt-get update` | Update daftar paket |
| `sudo apt-get upgrade` | Upgrade paket |
| `sudo apt-get dist-upgrade` | Upgrade distribusi |
| `sudo apt-get install <paket>` | Instal paket |
| `sudo apt-get install -y <paket>` | Instal tanpa konfirmasi |
| `sudo apt-get install --no-upgrade <paket>` | Instal tanpa upgrade jika sudah ada |
| `sudo apt-get install --only-upgrade <paket>` | Hanya upgrade, tidak instal jika belum ada |
| `sudo apt-get remove <paket>` | Hapus paket |
| `sudo apt-get purge <paket>` | Hapus + konfigurasi |
| `sudo apt-get autoremove` | Hapus paket tidak diperlukan |
| `sudo apt-get clean` | Hapus cache |
| `sudo apt-get autoclean` | Hapus cache lama |
| `sudo apt-get check` | Cek dependensi |
| `sudo apt-get -f install` | Perbaiki dependensi rusak |
| `sudo apt-get build-dep <paket>` | Instal dependensi build |
| `sudo apt-get source <paket>` | Unduh source code paket |
| `sudo apt-get download <paket>` | Unduh paket .deb tanpa instal |

---

## 5.3 DPKG - Debian Package Manager

### `dpkg` - Low Level Package Manager
```bash
dpkg [opsi] <file.deb>
```
> Package manager tingkat rendah untuk file .deb

**Instalasi & Penghapusan:**
| Perintah | Penjelasan |
|---|---|
| `sudo dpkg -i paket.deb` | Instal file .deb |
| `sudo dpkg -i *.deb` | Instal semua file .deb di direktori |
| `sudo dpkg -r <paket>` | Hapus paket (konfigurasi tetap) |
| `sudo dpkg -P <paket>` | Purge paket + konfigurasi |
| `sudo dpkg --configure -a` | Konfigurasi semua paket yang belum dikonfigurasi |
| `sudo dpkg --unpack paket.deb` | Unpack tanpa konfigurasi |
| `sudo dpkg-reconfigure <paket>` | Konfigurasi ulang paket |

**Informasi Paket:**
| Perintah | Penjelasan |
|---|---|
| `dpkg -l` | Daftar semua paket terinstal |
| `dpkg -l <paket>` | Status paket tertentu |
| `dpkg -l "nginx*"` | Daftar paket dengan pola nama |
| `dpkg -s <paket>` | Status dan info detail paket |
| `dpkg -L <paket>` | Daftar file yang diinstal oleh paket |
| `dpkg -S /path/file` | Paket mana yang memiliki file ini |
| `dpkg -c paket.deb` | Isi file .deb (sebelum diinstal) |
| `dpkg -I paket.deb` | Info file .deb |
| `dpkg --get-selections` | Daftar semua paket dan statusnya |
| `dpkg --set-selections` | Set status paket |
| `dpkg --print-architecture` | Tampilkan arsitektur sistem |
| `dpkg --print-foreign-architectures` | Arsitektur tambahan |
| `dpkg --add-architecture i386` | Tambah arsitektur |
| `dpkg --remove-architecture i386` | Hapus arsitektur |

**Status dpkg -l:**
```
ii → Terinstal dengan baik
rc → Dihapus tapi konfigurasi masih ada
un → Tidak dikenal
iU → Dipacking tapi belum dikonfigurasi
```

**Contoh Penggunaan dpkg:**
```bash
# Cari paket yang mengandung file tertentu
dpkg -S /usr/bin/python3
# python3-minimal: /usr/bin/python3

# Lihat semua file yang diinstal paket
dpkg -L nginx

# Cek apakah paket terinstal
dpkg -l nginx | grep ^ii

# Export daftar paket untuk backup
dpkg --get-selections > packages.list

# Restore daftar paket
sudo dpkg --set-selections < packages.list
sudo apt-get dselect-upgrade
```

---

## 5.4 YUM - Yellowdog Updater Modified (RHEL/CentOS 7)

### `yum` - Package Manager RHEL/CentOS 7
```bash
yum [opsi] <perintah>
```
> Package manager untuk RHEL, CentOS 7, dan turunannya

**Update & Upgrade:**
| Perintah | Penjelasan |
|---|---|
| `sudo yum check-update` | Cek update yang tersedia |
| `sudo yum update` | Update semua paket |
| `sudo yum update <paket>` | Update paket tertentu |
| `sudo yum upgrade` | Upgrade semua paket |
| `sudo yum update --security` | Update keamanan saja |

**Instalasi & Penghapusan:**
| Perintah | Penjelasan |
|---|---|
| `sudo yum install <paket>` | Instal paket |
| `sudo yum install -y <paket>` | Instal tanpa konfirmasi |
| `sudo yum install paket1 paket2` | Instal beberapa paket |
| `sudo yum install /path/paket.rpm` | Instal file RPM lokal |
| `sudo yum localinstall paket.rpm` | Instal RPM lokal (resolves deps) |
| `sudo yum remove <paket>` | Hapus paket |
| `sudo yum erase <paket>` | Hapus paket (sama dengan remove) |
| `sudo yum autoremove` | Hapus paket tidak diperlukan |
| `sudo yum reinstall <paket>` | Instal ulang paket |
| `sudo yum downgrade <paket>` | Downgrade ke versi sebelumnya |
| `sudo yum swap <paket-lama> <paket-baru>` | Ganti paket |

**Pencarian & Informasi:**
| Perintah | Penjelasan |
|---|---|
| `yum search <kata>` | Cari paket |
| `yum info <paket>` | Info detail paket |
| `yum list` | Daftar semua paket |
| `yum list installed` | Paket yang terinstal |
| `yum list available` | Paket yang tersedia |
| `yum list updates` | Paket yang bisa diupdate |
| `yum list extras` | Paket tidak ada di repository |
| `yum provides /path/file` | Paket yang menyediakan file |
| `yum provides "*/perintah"` | Paket yang menyediakan perintah |
| `yum deplist <paket>` | Daftar dependensi |
| `yum repolist` | Daftar repository aktif |
| `yum repolist all` | Semua repository |
| `yum repoinfo <repo>` | Info repository |

**Cache & History:**
| Perintah | Penjelasan |
|---|---|
| `sudo yum clean all` | Hapus semua cache |
| `sudo yum clean packages` | Hapus cache paket |
| `sudo yum clean metadata` | Hapus cache metadata |
| `sudo yum makecache` | Buat cache metadata |
| `yum history` | Riwayat transaksi |
| `yum history info <id>` | Detail transaksi tertentu |
| `sudo yum history undo <id>` | Batalkan transaksi |
| `sudo yum history redo <id>` | Ulangi transaksi |
| `sudo yum history rollback <id>` | Rollback ke transaksi tertentu |

**Groups:**
| Perintah | Penjelasan |
|---|---|
| `yum group list` | Daftar grup paket |
| `yum group info <grup>` | Info grup paket |
| `sudo yum group install <grup>` | Instal grup paket |
| `sudo yum group remove <grup>` | Hapus grup paket |
| `sudo yum group update <grup>` | Update grup paket |

**Repository:**
```bash
# Tambah repository
sudo yum-config-manager --add-repo https://url/repo.repo

# Aktifkan/nonaktifkan repository
sudo yum-config-manager --enable <repo>
sudo yum-config-manager --disable <repo>

# File konfigurasi repository
/etc/yum.repos.d/

# Format file .repo:
[repo-id]
name=Nama Repository
baseurl=https://url/repo/
enabled=1
gpgcheck=1
gpgkey=https://url/gpg
```

---

## 5.5 DNF - Dandified YUM (RHEL/CentOS 8+, Fedora)

### `dnf` - Modern Package Manager RPM
```bash
dnf [opsi] <perintah>
```
> Pengganti yum yang lebih modern untuk Fedora, RHEL 8+, CentOS 8+

**Update & Upgrade:**
| Perintah | Penjelasan |
|---|---|
| `sudo dnf check-update` | Cek update yang tersedia |
| `sudo dnf update` | Update semua paket |
| `sudo dnf update <paket>` | Update paket tertentu |
| `sudo dnf upgrade` | Upgrade semua paket |
| `sudo dnf upgrade --security` | Update keamanan saja |
| `sudo dnf system-upgrade download --releasever=9` | Persiapan upgrade versi OS |
| `sudo dnf system-upgrade reboot` | Reboot untuk upgrade OS |

**Instalasi & Penghapusan:**
| Perintah | Penjelasan |
|---|---|
| `sudo dnf install <paket>` | Instal paket |
| `sudo dnf install -y <paket>` | Instal tanpa konfirmasi |
| `sudo dnf install paket1 paket2` | Instal beberapa paket |
| `sudo dnf install /path/paket.rpm` | Instal file RPM lokal |
| `sudo dnf remove <paket>` | Hapus paket |
| `sudo dnf autoremove` | Hapus paket tidak diperlukan |
| `sudo dnf reinstall <paket>` | Instal ulang |
| `sudo dnf downgrade <paket>` | Downgrade paket |
| `sudo dnf swap <paket-lama> <paket-baru>` | Ganti paket |
| `sudo dnf mark install <paket>` | Tandai sebagai manual install |
| `sudo dnf mark remove <paket>` | Tandai untuk autoremove |

**Pencarian & Informasi:**
| Perintah | Penjelasan |
|---|---|
| `dnf search <kata>` | Cari paket |
| `dnf search all <kata>` | Cari di semua field |
| `dnf info <paket>` | Info detail paket |
| `dnf list` | Daftar semua paket |
| `dnf list installed` | Paket terinstal |
| `dnf list available` | Paket tersedia |
| `dnf list updates` | Paket yang bisa diupdate |
| `dnf list extras` | Paket tidak ada di repo |
| `dnf list autoremove` | Paket yang akan dihapus autoremove |
| `dnf provides /path/file` | Paket yang menyediakan file |
| `dnf provides "*/perintah"` | Paket yang menyediakan perintah |
| `dnf deplist <paket>` | Daftar dependensi |
| `dnf repoquery <paket>` | Query database paket |
| `dnf repoquery --list <paket>` | File yang diinstal paket |
| `dnf repoquery --requires <paket>` | Dependensi paket |
| `dnf repoquery --whatprovides /path` | Siapa yang menyediakan file |

**Cache & History:**
| Perintah | Penjelasan |
|---|---|
| `sudo dnf clean all` | Hapus semua cache |
| `sudo dnf clean packages` | Hapus cache paket |
| `sudo dnf clean metadata` | Hapus cache metadata |
| `sudo dnf makecache` | Buat cache |
| `dnf history` | Riwayat transaksi |
| `dnf history info <id>` | Detail transaksi |
| `sudo dnf history undo <id>` | Batalkan transaksi |
| `sudo dnf history redo <id>` | Ulangi transaksi |
| `sudo dnf history rollback <id>` | Rollback |

**Groups & Modules:**
| Perintah | Penjelasan |
|---|---|
| `dnf group list` | Daftar grup paket |
| `dnf group info <grup>` | Info grup |
| `sudo dnf group install <grup>` | Instal grup |
| `sudo dnf group remove <grup>` | Hapus grup |
| `dnf module list` | Daftar modul |
| `dnf module info <modul>` | Info modul |
| `sudo dnf module enable <modul>:<stream>` | Aktifkan stream modul |
| `sudo dnf module install <modul>:<stream>/<profil>` | Instal modul |
| `sudo dnf module disable <modul>` | Nonaktifkan modul |
| `sudo dnf module reset <modul>` | Reset modul |

**Repository:**
```bash
# Tambah repository
sudo dnf config-manager --add-repo https://url/repo.repo

# Aktifkan/nonaktifkan
sudo dnf config-manager --enable <repo-id>
sudo dnf config-manager --disable <repo-id>

# Instal EPEL (Extra Packages)
sudo dnf install epel-release

# Daftar repository
dnf repolist
dnf repolist all
dnf repoinfo <repo>
```

---

## 5.6 RPM - RPM Package Manager

### `rpm` - Low Level RPM Package Manager
```bash
rpm [opsi] <file.rpm>
```
> Package manager tingkat rendah untuk file .rpm

**Instalasi & Penghapusan:**
| Perintah | Penjelasan |
|---|---|
| `sudo rpm -i paket.rpm` | Instal paket RPM |
| `sudo rpm -iv paket.rpm` | Instal dengan verbose |
| `sudo rpm -ivh paket.rpm` | Instal + verbose + progress bar |
| `sudo rpm -U paket.rpm` | Upgrade paket (instal jika belum ada) |
| `sudo rpm -Uvh paket.rpm` | Upgrade + verbose + progress |
| `sudo rpm -F paket.rpm` | Freshen (upgrade hanya jika sudah terinstal) |
| `sudo rpm -e <paket>` | Hapus paket |
| `sudo rpm -e --nodeps <paket>` | Hapus tanpa cek dependensi |
| `sudo rpm -e --allmatches <paket>` | Hapus semua versi |
| `sudo rpm --reinstall paket.rpm` | Instal ulang |

**Informasi Paket:**
| Perintah | Penjelasan |
|---|---|
| `rpm -qa` | Daftar semua paket terinstal |
| `rpm -qa \| grep <nama>` | Cari paket terinstal |
| `rpm -qi <paket>` | Info detail paket terinstal |
| `rpm -ql <paket>` | Daftar file dalam paket |
| `rpm -qd <paket>` | Daftar file dokumentasi |
| `rpm -qc <paket>` | Daftar file konfigurasi |
| `rpm -qf /path/file` | Paket yang memiliki file ini |
| `rpm -qR <paket>` | Dependensi paket |
| `rpm -q --provides <paket>` | Apa yang disediakan paket |
| `rpm -q --changelog <paket>` | Changelog paket |
| `rpm -q --scripts <paket>` | Script instalasi paket |
| `rpm -qip paket.rpm` | Info file RPM (belum diinstal) |
| `rpm -qlp paket.rpm` | File dalam RPM (belum diinstal) |
| `rpm --querytags` | Semua tag yang bisa di-query |

**Verifikasi:**
| Perintah | Penjelasan |
|---|---|
| `rpm -V <paket>` | Verifikasi integritas paket |
| `rpm -Va` | Verifikasi semua paket |
| `rpm -Vf /path/file` | Verifikasi file tertentu |
| `rpm --checksig paket.rpm` | Cek signature RPM |
| `rpm --import /path/gpg-key` | Import GPG key |
| `rpm -qa gpg-pubkey` | Daftar GPG key yang diimport |

---

## 5.7 PACMAN - Arch Linux Package Manager

### `pacman` - Package Manager Arch Linux
```bash
pacman [opsi] <paket>
```
> Package manager untuk Arch Linux dan turunannya (Manjaro, EndeavourOS, dll.)

**Update & Upgrade:**
| Perintah | Penjelasan |
|---|---|
| `sudo pacman -Sy` | Sinkronisasi database |
| `sudo pacman -Syu` | Update database + upgrade semua paket |
| `sudo pacman -Syuu` | Downgrade jika repo lebih lama |
| `sudo pacman -Su` | Upgrade tanpa sinkronisasi |

**Instalasi & Penghapusan:**
| Perintah | Penjelasan |
|---|---|
| `sudo pacman -S <paket>` | Instal paket |
| `sudo pacman -S paket1 paket2` | Instal beberapa paket |
| `sudo pacman -S --needed <paket>` | Instal hanya jika belum ada |
| `sudo pacman -U paket.pkg.tar.zst` | Instal file lokal |
| `sudo pacman -R <paket>` | Hapus paket |
| `sudo pacman -Rs <paket>` | Hapus + dependensi yang tidak diperlukan |
| `sudo pacman -Rsc <paket>` | Hapus + deps + paket yang tergantung |
| `sudo pacman -Rn <paket>` | Hapus + backup konfigurasi |
| `sudo pacman -Rsn <paket>` | Hapus lengkap |

**Pencarian & Informasi:**
| Perintah | Penjelasan |
|---|---|
| `pacman -Ss <kata>` | Cari paket di database |
| `pacman -Si <paket>` | Info paket dari repository |
| `pacman -Qi <paket>` | Info paket yang terinstal |
| `pacman -Ql <paket>` | File yang diinstal paket |
| `pacman -Qo /path/file` | Paket yang memiliki file ini |
| `pacman -Qm` | Paket yang tidak ada di repo (AUR/manual) |
| `pacman -Qn` | Paket yang ada di repo resmi |
| `pacman -Qe` | Paket yang diinstal secara eksplisit |
| `pacman -Qd` | Paket yang diinstal sebagai dependensi |
| `pacman -Qu` | Paket yang bisa diupdate |
| `pacman -Qdt` | Paket orphan (tidak diperlukan) |
| `pacman -Fl <paket>` | File dari paket di repo |
| `pacman -Fy` | Update database file |
| `pacman -F /path/file` | Cari paket yang punya file ini |

**Cache & Database:**
| Perintah | Penjelasan |
|---|---|
| `sudo pacman -Sc` | Hapus cache paket yang tidak terinstal |
| `sudo pacman -Scc` | Hapus semua cache |
| `sudo pacman -Syy` | Force refresh database |
| `sudo pacman -Dk` | Cek konsistensi database |

**Kunci & Repository:**
```bash
# Inisialisasi keyring
sudo pacman-key --init
sudo pacman-key --populate archlinux

# Tambah kunci
sudo pacman-key --add /path/gpg-key
sudo pacman-key --recv-keys KEY_ID
sudo pacman-key --lsign-key KEY_ID

# Konfigurasi repository
sudo nano /etc/pacman.conf

# Format repository:
[extra]
Include = /etc/pacman.d/mirrorlist

# Mirror
sudo nano /etc/pacman.d/mirrorlist
```

---

## 5.8 YAY / Paru - AUR Helper (Arch Linux)

### `yay` - AUR Helper
```bash
yay [opsi] <paket>
```
> Helper untuk menginstal paket dari AUR (Arch User Repository)

> 📦 Perlu instalasi manual dari AUR

| Perintah | Penjelasan |
|---|---|
| `yay` | Update semua paket termasuk AUR |
| `yay -Syu` | Update sistem + AUR |
| `yay -S <paket>` | Instal paket (repo + AUR) |
| `yay -Ss <kata>` | Cari paket di repo + AUR |
| `yay -Si <paket>` | Info paket |
| `yay -R <paket>` | Hapus paket |
| `yay -Rs <paket>` | Hapus + deps |
| `yay -Qm` | Daftar paket AUR yang terinstal |
| `yay -Ps` | Statistik sistem |
| `yay -Yc` | Hapus paket AUR yang tidak diperlukan |
| `yay --aur` | Operasi hanya pada paket AUR |
| `yay --repo` | Operasi hanya pada paket repo resmi |
| `yay -G <paket>` | Download PKGBUILD dari AUR |

---

### `paru` - Modern AUR Helper
```bash
paru [opsi] <paket>
```
> AUR helper modern sebagai pengganti yay

| Perintah | Penjelasan |
|---|---|
| `paru` | Update semua paket |
| `paru -S <paket>` | Instal paket |
| `paru -Ss <kata>` | Cari paket |
| `paru -R <paket>` | Hapus paket |
| `paru -c` | Hapus orphan packages |
| `paru --fm nvim` | Set file manager untuk review PKGBUILD |

---

## 5.9 Zypper - openSUSE Package Manager

### `zypper` - Package Manager openSUSE
```bash
zypper [opsi] <perintah>
```
> Package manager untuk openSUSE dan SUSE Linux Enterprise

| Perintah | Penjelasan |
|---|---|
| `sudo zypper refresh` | Refresh repository |
| `sudo zypper update` | Update semua paket |
| `sudo zypper dup` | Distribution upgrade |
| `sudo zypper install <paket>` | Instal paket |
| `sudo zypper install -y <paket>` | Instal tanpa konfirmasi |
| `sudo zypper remove <paket>` | Hapus paket |
| `sudo zypper remove -u <paket>` | Hapus + deps yang tidak diperlukan |
| `zypper search <kata>` | Cari paket |
| `zypper search -i <kata>` | Cari paket yang terinstal |
| `zypper info <paket>` | Info detail paket |
| `zypper list-updates` | Daftar update yang tersedia |
| `zypper repos` | Daftar repository |
| `sudo zypper addrepo <url> <alias>` | Tambah repository |
| `sudo zypper removerepo <alias>` | Hapus repository |
| `sudo zypper modifyrepo -e <alias>` | Aktifkan repository |
| `sudo zypper modifyrepo -d <alias>` | Nonaktifkan repository |
| `sudo zypper clean` | Hapus cache |
| `zypper verify` | Verifikasi dependensi |
| `zypper what-provides /path/file` | Paket yang menyediakan file |
| `zypper patch` | Instal patch yang tersedia |
| `zypper patch-check` | Cek patch yang diperlukan |

---

## 5.10 Snap Package Manager

### `snap` - Universal Package Manager
```bash
snap [opsi] <perintah>
```
> Package manager universal yang berjalan di berbagai distro Linux

| Perintah | Penjelasan |
|---|---|
| `snap find <kata>` | Cari aplikasi di Snap Store |
| `snap info <paket>` | Info detail snap |
| `sudo snap install <paket>` | Instal snap |
| `sudo snap install <paket> --classic` | Instal dengan akses sistem penuh |
| `sudo snap install <paket> --channel=edge` | Instal dari channel tertentu |
| `sudo snap install <paket> --revision=123` | Instal revisi tertentu |
| `sudo snap remove <paket>` | Hapus snap |
| `sudo snap remove --purge <paket>` | Hapus + data |
| `sudo snap refresh` | Update semua snap |
| `sudo snap refresh <paket>` | Update snap tertentu |
| `sudo snap refresh --hold` | Tahan semua update |
| `snap list` | Daftar snap yang terinstal |
| `snap list --all` | Termasuk revisi lama |
| `snap version` | Versi snapd |
| `snap changes` | Riwayat perubahan |
| `snap connections <paket>` | Koneksi interface snap |
| `sudo snap connect <paket>:<plug> <paket>:<slot>` | Hubungkan interface |
| `sudo snap disconnect <paket>:<plug>` | Putuskan interface |
| `snap services <paket>` | Service dari snap |
| `sudo snap start <paket>` | Start service snap |
| `sudo snap stop <paket>` | Stop service snap |
| `sudo snap enable <paket>` | Aktifkan snap |
| `sudo snap disable <paket>` | Nonaktifkan snap |
| `snap run <paket>` | Jalankan snap |
| `sudo snap revert <paket>` | Kembali ke versi sebelumnya |
| `sudo snap set <paket> key=value` | Set konfigurasi snap |
| `snap get <paket> key` | Baca konfigurasi snap |

---

## 5.11 Flatpak Package Manager

### `flatpak` - Sandboxed Application Manager
```bash
flatpak [opsi] <perintah>
```
> Package manager untuk aplikasi sandboxed lintas distro

| Perintah | Penjelasan |
|---|---|
| `flatpak search <kata>` | Cari aplikasi |
| `flatpak info <app-id>` | Info aplikasi |
| `flatpak install <remote> <app-id>` | Instal aplikasi |
| `flatpak install flathub <app-id>` | Instal dari Flathub |
| `flatpak install --user <app-id>` | Instal hanya untuk user saat ini |
| `flatpak uninstall <app-id>` | Hapus aplikasi |
| `flatpak uninstall --delete-data <app-id>` | Hapus + data aplikasi |
| `flatpak uninstall --unused` | Hapus runtime yang tidak dipakai |
| `flatpak update` | Update semua aplikasi |
| `flatpak update <app-id>` | Update aplikasi tertentu |
| `flatpak list` | Daftar aplikasi terinstal |
| `flatpak list --app` | Hanya aplikasi (bukan runtime) |
| `flatpak list --runtime` | Hanya runtime |
| `flatpak run <app-id>` | Jalankan aplikasi |
| `flatpak run --command=bash <app-id>` | Jalankan shell dalam sandbox |
| `flatpak remotes` | Daftar remote yang dikonfigurasi |
| `flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo` | Tambah Flathub |
| `flatpak remote-delete <remote>` | Hapus remote |
| `flatpak remote-modify --enable <remote>` | Aktifkan remote |
| `flatpak remote-modify --disable <remote>` | Nonaktifkan remote |
| `flatpak permissions` | Tampilkan permission aplikasi |
| `flatpak override --filesystem=home <app-id>` | Override permission |
| `flatpak override --reset <app-id>` | Reset override |
| `flatpak repair` | Perbaiki instalasi flatpak |
| `flatpak create-usb /path <app-id>` | Buat USB installer |
| `flatpak history` | Riwayat transaksi |

---

## 5.12 AppImage

### Menggunakan AppImage
```bash
# AppImage adalah file executable yang berdiri sendiri
# Tidak perlu instalasi, langsung jalankan

# Beri permission execute
chmod +x NamaAplikasi.AppImage

# Jalankan
./NamaAplikasi.AppImage

# Extract isi AppImage
./NamaAplikasi.AppImage --appimage-extract

# Mount AppImage
./NamaAplikasi.AppImage --appimage-mount

# Integrasi dengan desktop (menggunakan appimaged)
# Install appimaged
wget -q https://github.com/probonopd/go-appimage/releases/download/continuous/appimaged-*.AppImage
chmod +x appimaged-*.AppImage
./appimaged-*.AppImage

# AppImageLauncher (integrasi lebih baik)
sudo apt install appimagelauncher
```

---

## 5.13 Pip - Python Package Manager

### `pip` / `pip3` - Python Package Installer
```bash
pip [opsi] <perintah>
```
> Package manager untuk paket Python

| Perintah | Penjelasan |
|---|---|
| `pip install <paket>` | Instal paket Python |
| `pip install <paket>==1.0.0` | Instal versi tertentu |
| `pip install "<paket>>=1.0,<2.0"` | Instal dengan batasan versi |
| `pip install -U <paket>` | Update paket |
| `pip install --upgrade pip` | Update pip itu sendiri |
| `pip install -r requirements.txt` | Instal dari file requirements |
| `pip install --user <paket>` | Instal hanya untuk user |
| `pip install -e .` | Instal dalam mode editable (development) |
| `pip install --no-deps <paket>` | Instal tanpa dependensi |
| `pip uninstall <paket>` | Hapus paket |
| `pip uninstall -y <paket>` | Hapus tanpa konfirmasi |
| `pip uninstall -r requirements.txt` | Hapus semua dari file |
| `pip list` | Daftar paket terinstal |
| `pip list --outdated` | Paket yang bisa diupdate |
| `pip list --uptodate` | Paket yang sudah terbaru |
| `pip show <paket>` | Info detail paket |
| `pip search <kata>` | Cari paket (deprecated) |
| `pip freeze` | Output paket terinstal untuk requirements |
| `pip freeze > requirements.txt` | Simpan ke file |
| `pip check` | Cek kompatibilitas dependensi |
| `pip download <paket>` | Unduh tanpa instal |
| `pip cache list` | Lihat cache |
| `pip cache purge` | Hapus cache |
| `pip config list` | Lihat konfigurasi |

---

## 5.14 NPM & Node Package Manager

### `npm` - Node Package Manager
```bash
npm [opsi] <perintah>
```
> Package manager untuk JavaScript/Node.js

| Perintah | Penjelasan |
|---|---|
| `npm install` | Instal semua dari package.json |
| `npm install <paket>` | Instal paket lokal |
| `npm install -g <paket>` | Instal paket global |
| `npm install <paket>@1.0.0` | Instal versi tertentu |
| `npm install --save-dev <paket>` | Instal sebagai devDependency |
| `npm install --save-exact <paket>` | Instal versi persis |
| `npm uninstall <paket>` | Hapus paket |
| `npm uninstall -g <paket>` | Hapus paket global |
| `npm update` | Update semua paket |
| `npm update <paket>` | Update paket tertentu |
| `npm list` | Daftar paket lokal |
| `npm list -g` | Daftar paket global |
| `npm list --depth=0` | Hanya paket top-level |
| `npm info <paket>` | Info paket |
| `npm search <kata>` | Cari paket |
| `npm outdated` | Paket yang bisa diupdate |
| `npm audit` | Cek kerentanan keamanan |
| `npm audit fix` | Perbaiki kerentanan otomatis |
| `npm init` | Buat package.json baru |
| `npm init -y` | Buat package.json dengan default |
| `npm run <script>` | Jalankan script dari package.json |
| `npm start` | Jalankan script start |
| `npm test` | Jalankan script test |
| `npm build` | Jalankan script build |
| `npm publish` | Publish paket ke npm registry |
| `npm login` | Login ke npm |
| `npm whoami` | Tampilkan user npm saat ini |
| `npm cache clean --force` | Hapus cache |
| `npm config list` | Lihat konfigurasi |
| `npm config set key value` | Set konfigurasi |
| `npx <perintah>` | Jalankan paket tanpa menginstal |

---

## 5.15 Perintah Manajemen Paket Lainnya

### `gem` - Ruby Package Manager
```bash
gem install <paket>          # Instal gem
gem install <paket> -v 1.0  # Versi tertentu
gem uninstall <paket>        # Hapus gem
gem list                     # Daftar gem terinstal
gem update                   # Update semua gem
gem update <paket>           # Update gem tertentu
gem search <kata>            # Cari gem
gem info <paket>             # Info gem
gem environment              # Info lingkungan gem
gem cleanup                  # Hapus gem lama
```

---

### `cargo` - Rust Package Manager
```bash
cargo new project            # Buat proyek baru
cargo build                  # Build proyek
cargo run                    # Build dan jalankan
cargo test                   # Jalankan tes
cargo install <paket>        # Instal binary Rust
cargo uninstall <paket>      # Hapus binary
cargo update                 # Update dependensi
cargo search <kata>          # Cari paket di crates.io
cargo add <paket>            # Tambah dependensi
cargo remove <paket>         # Hapus dependensi
cargo list                   # Daftar binary terinstal
```

---

### `composer` - PHP Package Manager
```bash
composer install             # Instal dari composer.json
composer require <paket>     # Tambah & instal paket
composer remove <paket>      # Hapus paket
composer update              # Update semua paket
composer update <paket>      # Update paket tertentu
composer search <kata>       # Cari paket
composer show                # Daftar paket terinstal
composer show <paket>        # Info paket
composer create-project <paket> <dir>  # Buat proyek baru
composer dump-autoload       # Update autoloader
composer validate            # Validasi composer.json
composer clear-cache         # Hapus cache
```

---

### `brew` - Homebrew (Linux)
```bash
# Homebrew dapat diinstal di Linux
brew install <paket>         # Instal paket
brew uninstall <paket>       # Hapus paket
brew update                  # Update Homebrew
brew upgrade                 # Upgrade semua paket
brew upgrade <paket>         # Upgrade paket tertentu
brew search <kata>           # Cari paket
brew info <paket>            # Info paket
brew list                    # Daftar paket terinstal
brew doctor                  # Cek masalah
brew cleanup                 # Hapus file lama
brew tap <repo>              # Tambah repository
brew untap <repo>            # Hapus repository
```

---

## Ringkasan Perbandingan Package Manager

```
┌──────────────┬─────────────────────────────────────┐
│ Distro       │ Package Manager                     │
├──────────────┼─────────────────────────────────────┤
│ Ubuntu/Debian│ apt, apt-get, dpkg                  │
│ RHEL/CentOS 7│ yum, rpm                            │
│ RHEL/CentOS 8│ dnf, rpm                            │
│ Fedora       │ dnf, rpm                            │
│ Arch Linux   │ pacman, yay, paru                   │
│ openSUSE     │ zypper, rpm                         │
│ Universal    │ snap, flatpak, appimage             │
└──────────────┴─────────────────────────────────────┘

Operasi Umum:
┌──────────────┬──────────────┬──────────────┬────────────┐
│ Operasi      │ apt          │ dnf/yum      │ pacman     │
├──────────────┼──────────────┼──────────────┼────────────┤
│ Update DB    │ apt update   │ dnf check-up │ pacman -Sy │
│ Upgrade      │ apt upgrade  │ dnf upgrade  │ pacman -Su │
│ Install      │ apt install  │ dnf install  │ pacman -S  │
│ Remove       │ apt remove   │ dnf remove   │ pacman -R  │
│ Search       │ apt search   │ dnf search   │ pacman -Ss │
│ Info         │ apt show     │ dnf info     │ pacman -Si │
│ List files   │ dpkg -L      │ rpm -ql      │ pacman -Ql │
│ Find file    │ dpkg -S      │ rpm -qf      │ pacman -Qo │
│ Clean cache  │ apt clean    │ dnf clean    │ pacman -Sc │
└──────────────┴──────────────┴──────────────┴────────────┘
```

---

## Ringkasan Perintah Bagian 5

```
apt              → Package manager Debian/Ubuntu
apt-get          → Versi klasik apt
dpkg             → Low level .deb manager
yum              → Package manager RHEL/CentOS 7
dnf              → Package manager RHEL 8+/Fedora
rpm              → Low level .rpm manager
pacman           → Package manager Arch Linux
yay/paru         → AUR helper Arch Linux
zypper           → Package manager openSUSE
snap             → Universal package manager
flatpak          → Sandboxed app manager
appimage         → Portable app format
pip              → Python package manager
npm              → Node.js package manager
gem              → Ruby package manager
cargo            → Rust package manager
composer         → PHP package manager
brew             → Homebrew package manager
```

---

## ✅ Bagian 5 Selesai!

**Lanjut ke Bagian 6: Perintah Jaringan (Network)?**
