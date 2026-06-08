# Tutorial Lengkap WSL (Windows Subsystem for Linux)

## Daftar Isi

1. Pendahuluan
2. Apa itu WSL?
3. Perbedaan WSL1 dan WSL2
4. Persyaratan Sistem
5. Instalasi WSL
6. Instalasi Distribusi Linux
7. Konfigurasi Awal Linux
8. Struktur File WSL
9. Perintah Dasar Linux
10. Integrasi Windows dan Linux
11. Menggunakan Visual Studio Code
12. Mengelola Distribusi Linux
13. Mengelola Resource WSL
14. Networking pada WSL
15. Menjalankan Docker pada WSL
16. Menjalankan Server Web
17. Backup dan Restore
18. Troubleshooting
19. Best Practice
20. Referensi Belajar Lanjutan

---

# 1. Pendahuluan

Windows Subsystem for Linux (WSL) adalah fitur Windows yang memungkinkan pengguna menjalankan lingkungan Linux secara langsung di Windows tanpa menggunakan virtual machine tradisional.

Dengan WSL Anda dapat:

* Menjalankan Ubuntu, Debian, Kali Linux, dan distro lainnya.
* Menggunakan Bash Shell.
* Menjalankan Python, Node.js, PHP, Go, Rust, Java, dan tool Linux lainnya.
* Mengembangkan aplikasi web.
* Menjalankan Docker.
* Mengakses file Windows dan Linux secara bersamaan.

---

# 2. Apa itu WSL?

Secara sederhana:

```
Windows
│
├── Aplikasi Windows
│
└── WSL
    │
    └── Linux Kernel
        │
        └── Ubuntu / Debian / Kali
```

WSL memungkinkan Linux berjalan langsung di atas Windows dengan performa yang sangat baik.

---

# 3. Perbedaan WSL1 dan WSL2

| Fitur                | WSL1     | WSL2         |
| -------------------- | -------- | ------------ |
| Kernel Linux Asli    | Tidak    | Ya           |
| Kompatibilitas Linux | Sebagian | Hampir 100%  |
| Docker Support       | Terbatas | Penuh        |
| Performa File Linux  | Cepat    | Sangat Cepat |
| Virtualisasi         | Tidak    | Ya           |

Rekomendasi:

**Gunakan WSL2.**

---

# 4. Persyaratan Sistem

Minimal:

* Windows 10 versi 2004+
* Windows 11
* Virtualization Enabled di BIOS
* RAM minimal 4 GB
* Disarankan 8 GB atau lebih

Cek versi Windows:

```powershell
winver
```

---

# 5. Instalasi WSL

Buka PowerShell sebagai Administrator:

```powershell
wsl --install
```

Perintah ini akan:

* Mengaktifkan fitur WSL
* Mengaktifkan Virtual Machine Platform
* Menginstal WSL2
* Menginstal Ubuntu

Restart komputer setelah selesai.

---

# 6. Cek Status Instalasi

```powershell
wsl --status
```

Contoh output:

```text
Default Distribution: Ubuntu
Default Version: 2
```

---

# 7. Melihat Distribusi Linux

```powershell
wsl --list --verbose
```

atau:

```powershell
wsl -l -v
```

Contoh:

```text
NAME      STATE      VERSION
Ubuntu    Running    2
```

---

# 8. Menginstal Distribusi Linux Lain

Lihat daftar distro:

```powershell
wsl --list --online
```

Contoh:

```text
Ubuntu
Debian
Kali-Linux
openSUSE
Ubuntu-24.04
```

Instal:

```powershell
wsl --install -d Ubuntu-24.04
```

atau:

```powershell
wsl --install -d Debian
```

---

# 9. Menjalankan Linux

Dari Start Menu:

```
Ubuntu
```

atau dari terminal:

```powershell
wsl
```

---

# 10. Konfigurasi Awal

Saat pertama kali masuk:

```text
Enter new UNIX username:
```

Misal:

```text
bejo
```

Password:

```text
********
```

Catatan:

Password Linux tidak akan terlihat saat diketik.

---

# 11. Update Sistem Linux

Ubuntu:

```bash
sudo apt update
sudo apt upgrade -y
```

---

# 12. Struktur Direktori Linux

Masuk ke home:

```bash
cd ~
```

Struktur penting:

```text
/
├── home
├── etc
├── var
├── usr
├── tmp
└── root
```

---

# 13. Mengakses Drive Windows

Drive Windows tersedia di:

```text
/mnt/
```

Contoh:

```bash
cd /mnt/c
```

Drive D:

```bash
cd /mnt/d
```

---

# 14. Mengakses File Linux dari Windows

Buka File Explorer:

```text
\\wsl$
```

Contoh:

```text
\\wsl$\Ubuntu
```

---

# 15. Perintah Dasar Linux

## Lokasi saat ini

```bash
pwd
```

## List file

```bash
ls
```

Detail:

```bash
ls -la
```

## Pindah folder

```bash
cd nama_folder
```

## Buat folder

```bash
mkdir project
```

## Hapus folder

```bash
rm -rf project
```

## Buat file

```bash
touch file.txt
```

## Lihat isi file

```bash
cat file.txt
```

---

# 16. Menginstal Software

## Git

```bash
sudo apt install git -y
```

## Python

```bash
sudo apt install python3 python3-pip -y
```

## Node.js

```bash
sudo apt install nodejs npm -y
```

Cek:

```bash
node -v
npm -v
```

---

# 17. Integrasi dengan Visual Studio Code

Instal VS Code.

Di WSL:

```bash
code .
```

VS Code akan otomatis membuka folder Linux.

Install extension:

```
Remote - WSL
```

Keuntungan:

* Coding langsung di Linux.
* Debug lebih mudah.
* Performa lebih baik.

---

# 18. Menjalankan Python

Buat file:

```bash
nano hello.py
```

Isi:

```python
print("Hello WSL")
```

Jalankan:

```bash
python3 hello.py
```

---

# 19. Menjalankan Web Server

Python:

```bash
python3 -m http.server 8000
```

Akses:

```text
http://localhost:8000
```

---

# 20. Networking WSL

Lihat alamat IP:

```bash
ip addr
```

atau:

```bash
hostname -I
```

---

# 21. Mengubah Versi WSL

Cek:

```powershell
wsl -l -v
```

Konversi:

```powershell
wsl --set-version Ubuntu 2
```

---

# 22. Menjadikan WSL2 Sebagai Default

```powershell
wsl --set-default-version 2
```

---

# 23. Menghentikan WSL

```powershell
wsl --shutdown
```

---

# 24. Restart WSL

```powershell
wsl --shutdown
wsl
```

---

# 25. Menghapus Distribusi Linux

Lihat distro:

```powershell
wsl -l
```

Unregister:

```powershell
wsl --unregister Ubuntu
```

PERINGATAN:

Semua data distro akan hilang.

---

# 26. Backup Distribusi

Export:

```powershell
wsl --export Ubuntu backup.tar
```

---

# 27. Restore Distribusi

```powershell
wsl --import UbuntuBackup D:\WSL\Ubuntu backup.tar
```

---

# 28. Menjalankan Docker di WSL

Instal:

### Docker Desktop

Saat instalasi:

✔ Use WSL2 Backend

Verifikasi:

```bash
docker version
```

Tes:

```bash
docker run hello-world
```

---

# 29. Membatasi Resource WSL

Buat file:

```text
C:\Users\<username>\.wslconfig
```

Isi:

```ini
[wsl2]
memory=4GB
processors=2
swap=2GB
```

Aktifkan:

```powershell
wsl --shutdown
```

---

# 30. Troubleshooting

## WSL tidak bisa start

```powershell
wsl --shutdown
```

Lalu:

```powershell
wsl
```

---

## Virtualization Disabled

Cek Task Manager:

```
Performance
→ CPU
→ Virtualization
```

Harus:

```text
Enabled
```

---

## Error Kernel

Update:

```powershell
wsl --update
```

---

## Cek Versi WSL

```powershell
wsl --version
```

---

# 31. Best Practice

## Simpan project di Linux

Lebih baik:

```text
/home/user/project
```

Daripada:

```text
/mnt/c/project
```

Karena performanya lebih cepat.

---

## Update Berkala

```bash
sudo apt update
sudo apt upgrade
```

---

## Backup Berkala

```powershell
wsl --export Ubuntu backup.tar
```

---

## Gunakan Git

```bash
git init
git add .
git commit -m "first commit"
```

---

# 32. Skenario Penggunaan Nyata

## Web Developer

* WSL2
* Ubuntu
* Git
* Node.js
* Docker
* VS Code

## Data Science

* Python
* Jupyter Notebook
* Pandas
* NumPy
* TensorFlow

## Cyber Security

* Kali Linux
* Nmap
* Wireshark
* Metasploit

## DevOps

* Docker
* Kubernetes
* Terraform
* Ansible

---

# Kesimpulan

WSL adalah solusi ideal untuk menjalankan Linux di Windows tanpa dual boot maupun virtual machine penuh.

Rekomendasi stack modern:

```text
Windows 11
│
├── WSL2
│   └── Ubuntu 24.04
│
├── VS Code
├── Git
├── Docker Desktop
└── Python / Node.js
```

Dengan kombinasi tersebut, Windows dapat berfungsi hampir setara workstation Linux untuk pengembangan perangkat lunak, DevOps, Data Science, dan Cyber Security.
