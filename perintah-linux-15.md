# Bagian 15: Perintah Tambahan & Utilitas Lanjutan

---

## 15.1 Text Processing Lanjutan

### `jq` - JSON Processor
```bash
sudo apt install jq
```
> Tool command-line untuk memproses JSON

```bash
# Dasar
echo '{"name":"John","age":30}' | jq '.'        # Pretty print
echo '{"name":"John","age":30}' | jq '.name'     # Akses field
echo '{"name":"John","age":30}' | jq '.age'      # Akses angka
cat data.json | jq '.'                            # Format file JSON
curl -s https://api.example.com/users | jq '.'   # Format API response

# Akses nested
echo '{"user":{"name":"John","city":"Jakarta"}}' | jq '.user.name'
echo '{"users":[{"name":"John"},{"name":"Jane"}]}' | jq '.users[0].name'
echo '{"users":[{"name":"John"},{"name":"Jane"}]}' | jq '.users[].name'

# Array operations
echo '[1,2,3,4,5]' | jq '.[]'                    # Iterasi array
echo '[1,2,3,4,5]' | jq '.[2]'                   # Elemen ke-3
echo '[1,2,3,4,5]' | jq '.[2:4]'                 # Slice [3,4]
echo '[1,2,3,4,5]' | jq 'length'                 # Panjang array
echo '[1,2,3,4,5]' | jq 'first'                  # Elemen pertama
echo '[1,2,3,4,5]' | jq 'last'                   # Elemen terakhir
echo '[1,2,3,4,5]' | jq 'reverse'                # Balikkan array
echo '[1,2,3,4,5]' | jq 'sort'                   # Urutkan
echo '[1,2,3,4,5]' | jq 'unique'                 # Nilai unik
echo '[3,1,2]' | jq 'sort_by(.)'                 # Sort kompleks

# Filter dan seleksi
echo '[{"name":"John","age":30},{"name":"Jane","age":25}]' | \
    jq '.[] | select(.age > 27)'                  # Filter by kondisi
echo '[{"name":"John","age":30},{"name":"Jane","age":25}]' | \
    jq '[.[] | select(.age > 27)]'                # Hasil sebagai array
echo '[{"name":"John"},{"name":"Jane"}]' | \
    jq '[.[] | .name]'                            # Extract field

# Transformasi
echo '{"name":"John","age":30}' | \
    jq '{nama: .name, umur: .age}'                # Rename fields
echo '[{"n":"John","a":30}]' | \
    jq '[.[] | {name:.n, age:.a}]'                # Transform array

# Operasi string
echo '"Hello World"' | jq 'ascii_downcase'        # Lowercase
echo '"hello world"' | jq 'ascii_upcase'          # Uppercase
echo '"Hello"' | jq 'length'                      # Panjang string
echo '"Hello World"' | jq 'split(" ")'            # Split string
echo '["Hello","World"]' | jq 'join(" ")'         # Join array

# Operasi matematik
echo '{"a":10,"b":3}' | jq '.a + .b'             # Penjumlahan
echo '{"a":10,"b":3}' | jq '.a * .b'             # Perkalian
echo '{"a":10,"b":3}' | jq '.a / .b'             # Pembagian
echo '{"a":10,"b":3}' | jq '.a % .b'             # Modulo
echo '[1,2,3,4,5]' | jq 'add'                    # Sum
echo '[1,2,3,4,5]' | jq 'add / length'           # Average

# Multiple output
echo '{"a":1,"b":2}' | jq '.a, .b'               # Multiple values
echo '{"a":1,"b":2}' | jq '[.a, .b]'             # Sebagai array
echo '{"a":1,"b":2}' | jq 'to_entries'           # Key-value pairs
echo '{"a":1,"b":2}' | jq 'keys'                 # Semua keys
echo '{"a":1,"b":2}' | jq 'values'               # Semua values
echo '{"a":1,"b":2}' | jq 'has("a")'             # Cek field ada

# Format output
cat data.json | jq -c '.'                         # Compact (satu baris)
cat data.json | jq -r '.name'                     # Raw output (tanpa quotes)
cat data.json | jq -r '.[] | .name'               # Raw iterasi
cat data.json | jq --arg name "John" '.[] | select(.name == $name)'  # Variable
cat data.json | jq --argjson limit 5 '[.[] | select(.age > $limit)]'

# CSV output
echo '[{"name":"John","age":30},{"name":"Jane","age":25}]' | \
    jq -r '.[] | [.name, .age] | @csv'

# TSV output
echo '[{"name":"John","age":30}]' | \
    jq -r '.[] | [.name, (.age | tostring)] | @tsv'

# Conditional
echo '{"status":200}' | jq 'if .status == 200 then "OK" else "Error" end'
echo '{"val":null}' | jq '.val // "default"'      # Null coalescing
echo '{"a":1}' | jq '.b? // "tidak ada"'          # Optional field

# Rekursif
echo '{"a":{"b":{"c":1}}}' | jq '.. | numbers'   # Semua angka rekursif
echo '{"a":{"b":1},"c":2}' | jq '[.. | numbers]' # Collect numbers

# Reduce
echo '[1,2,3,4,5]' | jq 'reduce .[] as $x (0; . + $x)'  # Sum

# Env variables
jq -n 'env.HOME'                                  # Akses env var
jq -n --arg home "$HOME" '$home'                  # Pass shell var

# Contoh praktis
# Parse docker ps output
docker inspect $(docker ps -q) | jq '[.[] | {name:.Name, ip:.NetworkSettings.IPAddress}]'

# Parse kubectl
kubectl get pods -o json | jq '.items[].metadata.name'

# Parse systemctl
systemctl show nginx | grep -oP '(?<=^MainPID=)\d+' | xargs -I{} jq -n --argjson pid {} '$pid'

# Format log JSON
tail -f app.log | jq -r '. | "\(.timestamp) [\(.level)] \(.message)"'
```

---

### `yq` - YAML Processor
```bash
sudo snap install yq
# atau
sudo wget https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64 \
    -O /usr/local/bin/yq && sudo chmod +x /usr/local/bin/yq
```
> Tool untuk memproses YAML (mirip jq untuk JSON)

```bash
# Baca YAML
yq '.name' file.yaml                          # Akses field
yq '.database.host' config.yaml              # Nested field
yq '.servers[0]' config.yaml                 # Elemen array
yq '.servers[].name' config.yaml             # Iterasi array

# Edit YAML
yq -i '.version = "2.0"' file.yaml           # Update field
yq -i '.servers += [{"name":"server4"}]' file.yaml  # Tambah ke array
yq -i 'del(.debug)' file.yaml                # Hapus field
yq -i '.env = "production"' config.yaml      # Set nilai

# Format konversi
yq -o json file.yaml                          # YAML ke JSON
yq -o props file.yaml                         # YAML ke properties
cat data.json | yq -P '.'                     # JSON ke YAML (pretty)

# Multiple documents
yq eval '.' multi-doc.yaml                   # Semua dokumen
yq eval-all '.' *.yaml                        # Semua file

# Contoh Kubernetes
yq '.spec.replicas' deployment.yaml           # Baca replicas
yq -i '.spec.replicas = 3' deployment.yaml    # Set replicas
yq '.metadata.labels' pod.yaml               # Baca labels
yq '.spec.containers[].image' deployment.yaml # Semua image
yq -i '.spec.containers[0].image = "nginx:1.25"' deployment.yaml
```

---

### `xmlstarlet` - XML Processor
```bash
sudo apt install xmlstarlet
```
> Tool untuk memproses XML

```bash
# Validasi
xmlstarlet val file.xml                       # Validasi XML
xmlstarlet val --xsd schema.xsd file.xml     # Validasi dengan XSD

# Query (select)
xmlstarlet sel -t -v "//book/title" file.xml  # XPath query
xmlstarlet sel -t -m "//book" -v "title" -n file.xml  # Lebih kompleks
xmlstarlet sel -t -v "count(//book)" file.xml  # Hitung elemen

# Edit
xmlstarlet ed -u "//config/port" -v "8080" file.xml  # Update nilai
xmlstarlet ed -i "//config" -t elem -n "newnode" file.xml  # Insert
xmlstarlet ed -d "//deprecated" file.xml     # Delete node

# Format
xmlstarlet fo file.xml                        # Format/pretty print
xmlstarlet fo -s 2 file.xml                   # Indent 2 spasi

# Transform (XSLT)
xmlstarlet tr stylesheet.xsl file.xml
```

---

### `csvkit` - CSV Processing Tools
```bash
sudo apt install csvkit
# atau
pip install csvkit
```
> Suite tool untuk bekerja dengan CSV

```bash
# csvlook - Tampilkan CSV sebagai tabel
csvlook data.csv                              # Tampilkan tabel
csvlook -d ";" data.csv                      # Delimiter titik koma

# csvstat - Statistik CSV
csvstat data.csv                             # Statistik semua kolom
csvstat -c 2 data.csv                        # Kolom ke-2

# csvcut - Pilih kolom
csvcut -c 1,3 data.csv                       # Kolom 1 dan 3
csvcut -c name,age data.csv                  # Kolom berdasarkan nama
csvcut -C 2 data.csv                         # Semua kecuali kolom 2

# csvgrep - Filter baris
csvgrep -c name -m "John" data.csv           # Filter exact match
csvgrep -c age -r "^[23]\d$" data.csv       # Filter regex
csvgrep -c city -m "Jakarta" -i data.csv    # Case-insensitive

# csvjoin - Join dua CSV
csvjoin -c name file1.csv file2.csv          # Inner join
csvjoin --left -c id file1.csv file2.csv     # Left join

# csvsort - Sort CSV
csvsort -c age data.csv                      # Sort ascending
csvsort -c age -r data.csv                   # Sort descending
csvsort -c name,age data.csv                 # Sort multiple kolom

# csvformat - Format ulang CSV
csvformat -D ";" data.csv                    # Ganti delimiter ke ;
csvformat -U 0 data.csv                      # Hapus quotes tidak perlu

# csv2json - Konversi ke JSON
csvjson data.csv                             # CSV ke JSON
csvjson --indent 2 data.csv                  # JSON indented

# sql2csv - Query dari database
sql2csv --db sqlite:///db.sqlite "SELECT * FROM users"
sql2csv --db postgresql://user:pass@host/db "SELECT * FROM orders"

# csvsql - SQL pada CSV
csvsql --query "SELECT name, age FROM data WHERE age > 25" data.csv
csvsql --query "SELECT * FROM data ORDER BY age" data.csv
```

---

## 15.2 System Administration Tools

### `systemd-analyze` - Boot Analysis
```bash
systemd-analyze                              # Waktu boot total
systemd-analyze blame                        # Waktu per unit
systemd-analyze blame | head -20             # Top 20 lambat
systemd-analyze critical-chain              # Rantai kritis
systemd-analyze critical-chain nginx.service # Untuk unit tertentu
systemd-analyze plot > boot.svg              # Plot SVG
systemd-analyze dot | dot -Tsvg > deps.svg  # Dependency graph
systemd-analyze dump                         # Dump semua unit info
systemd-analyze verify unit.service          # Verifikasi unit
systemd-analyze security                     # Analisis keamanan
systemd-analyze security nginx.service       # Keamanan unit tertentu
systemd-analyze cat-config system.conf       # Tampilkan konfigurasi
systemd-analyze unit-paths                   # Daftar path unit
```

---

### `systemd-cgls` & `systemd-cgtop` - Cgroup Tools
```bash
# Lihat hirarki cgroup
systemd-cgls                                 # Semua cgroup
systemd-cgls /system.slice                   # Slice tertentu
systemd-cgls /user.slice                     # User cgroup

# Monitor resource per cgroup (seperti top untuk cgroup)
systemd-cgtop                                # Monitor real-time
systemd-cgtop -d 2                           # Refresh 2 detik
systemd-cgtop -n 5                           # 5 iterasi
systemd-cgtop -m                             # Sort by memory
systemd-cgtop -c                             # Sort by CPU
systemd-cgtop --cpu=percentage               # CPU dalam persen
```

---

### `cgroups` - Control Groups
```bash
# Install cgroupstools
sudo apt install cgroup-tools

# Buat cgroup
sudo cgcreate -g cpu,memory:mygroup

# Set limit CPU (50% dari satu core)
sudo cgset -r cpu.cfs_quota_us=50000 mygroup
sudo cgset -r cpu.cfs_period_us=100000 mygroup

# Set limit memori (512MB)
sudo cgset -r memory.limit_in_bytes=536870912 mygroup

# Jalankan proses dalam cgroup
sudo cgexec -g cpu,memory:mygroup ./program

# Cek penggunaan
sudo cgget -g memory:mygroup

# Informasi cgroup
ls /sys/fs/cgroup/                           # Semua controller
cat /sys/fs/cgroup/memory/mygroup/memory.usage_in_bytes  # Penggunaan memori
cat /proc/$(pgrep nginx)/cgroup             # Cgroup proses
```

---

### `nsenter` - Enter Namespaces
```bash
# Masuk ke namespace container
sudo nsenter -t $(docker inspect -f '{{.State.Pid}}' container) -n  # Network NS
sudo nsenter -t PID -n -p -m                # Multiple namespace
sudo nsenter -t PID --all                   # Semua namespace
sudo nsenter --net=/var/run/netns/myns      # Named network namespace

# Contoh: debug container network
sudo nsenter -t $(docker inspect -f '{{.State.Pid}}' mycontainer) -n \
    ss -tulpn
```

---

### `unshare` - Run in New Namespaces
```bash
# Buat proses di namespace baru
sudo unshare --mount                         # New mount namespace
sudo unshare --uts                           # New UTS namespace
sudo unshare --ipc                           # New IPC namespace
sudo unshare --pid --fork                    # New PID namespace
sudo unshare --net                           # New network namespace
sudo unshare --user                          # New user namespace

# Contoh: sandbox sederhana
sudo unshare --mount --pid --fork --net bash
```

---

## 15.3 Networking Lanjutan

### `ip` Lanjutan
```bash
# Network namespaces
ip netns list                                # Daftar namespace
sudo ip netns add myns                       # Buat namespace
sudo ip netns del myns                       # Hapus namespace
sudo ip netns exec myns bash                 # Jalankan perintah di namespace
sudo ip netns exec myns ip addr             # Cek interface di namespace
sudo ip link set eth0 netns myns             # Pindahkan interface ke namespace

# Veth pairs (virtual ethernet)
sudo ip link add veth0 type veth peer name veth1
sudo ip link set veth0 up
sudo ip link set veth1 up
sudo ip addr add 10.0.0.1/24 dev veth0
sudo ip addr add 10.0.0.2/24 dev veth1

# Bridge
sudo ip link add br0 type bridge
sudo ip link set br0 up
sudo ip link set eth0 master br0
sudo ip addr add 192.168.1.1/24 dev br0

# VLAN
sudo ip link add link eth0 name eth0.100 type vlan id 100
sudo ip link set eth0.100 up
sudo ip addr add 192.168.100.1/24 dev eth0.100

# Traffic shaping (tc)
sudo tc qdisc add dev eth0 root tbf rate 1mbit burst 32kbit latency 400ms
sudo tc qdisc show dev eth0
sudo tc qdisc del dev eth0 root

# Bandwidth limiting dengan tc
sudo tc qdisc add dev eth0 root handle 1: htb default 12
sudo tc class add dev eth0 parent 1: classid 1:1 htb rate 100mbit
sudo tc class add dev eth0 parent 1:1 classid 1:12 htb rate 5mbit ceil 10mbit

# Routing policy (multiple routing tables)
sudo ip rule add from 192.168.1.0/24 table 100
sudo ip route add default via 192.168.1.1 table 100
sudo ip rule list
sudo ip route show table all
```

---

### `iptables` Lanjutan
```bash
# Struktur iptables
# Tables: filter, nat, mangle, raw, security
# Chains: INPUT, OUTPUT, FORWARD, PREROUTING, POSTROUTING

# Filter table (default)
sudo iptables -L                             # Daftar rules (filter)
sudo iptables -L -n -v --line-numbers        # Detail + verbose
sudo iptables -F                             # Flush semua rules

# NAT
sudo iptables -t nat -L                      # Rules NAT
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE  # NAT
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to 10.0.0.2:80

# Connection tracking
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A INPUT -m conntrack --ctstate INVALID -j DROP

# Rate limiting
sudo iptables -A INPUT -p tcp --dport 22 -m limit --limit 3/min -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j DROP

# Logging
sudo iptables -A INPUT -j LOG --log-prefix "DROPPED: " --log-level 4

# ipset (untuk set IP yang besar)
sudo apt install ipset
sudo ipset create blacklist hash:ip
sudo ipset add blacklist 192.168.1.100
sudo ipset add blacklist 10.0.0.0/8
sudo iptables -A INPUT -m set --match-set blacklist src -j DROP
sudo ipset list                              # Daftar sets
sudo ipset save > /etc/ipset.conf            # Simpan
sudo ipset restore < /etc/ipset.conf         # Restore

# nftables (pengganti iptables modern)
sudo apt install nftables
sudo nft list ruleset                        # Lihat semua rules
sudo nft add table inet filter              # Buat tabel
sudo nft add chain inet filter input { type filter hook input priority 0 \; }
sudo nft add rule inet filter input tcp dport 22 accept
sudo nft add rule inet filter input drop
sudo nft list table inet filter              # Lihat tabel
sudo nft delete rule inet filter input handle N  # Hapus rule
sudo nft flush ruleset                       # Hapus semua
cat /etc/nftables.conf                       # File konfigurasi
```

---

### `tcpdump` Lanjutan
```bash
# Capture dengan filter kompleks
sudo tcpdump -i eth0 'tcp and port 80 and host 192.168.1.100'
sudo tcpdump -i eth0 'not port 22 and not arp'
sudo tcpdump -i eth0 'src net 192.168.1.0/24'
sudo tcpdump -i eth0 'tcp[tcpflags] & (tcp-syn|tcp-fin) != 0'
sudo tcpdump -i eth0 'ip[8] < 5'            # TTL < 5 (routing loop detection)

# HTTP traffic
sudo tcpdump -i eth0 -A -s 0 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'

# DNS queries
sudo tcpdump -i eth0 -n port 53

# Capture ke file dengan rotasi
sudo tcpdump -i eth0 -w /tmp/capture-%Y%m%d-%H%M%S.pcap -G 3600 -C 100

# Baca file pcap
sudo tcpdump -r capture.pcap
sudo tcpdump -r capture.pcap -A | grep "GET\|POST"

# Statistik
sudo tcpdump -i eth0 -ttt -q              # Timestamp relatif + quiet
```

---

## 15.4 Container & Virtualization

### Docker Commands
```bash
# === IMAGE ===
docker images                                # Daftar images
docker pull nginx:latest                     # Download image
docker push user/image:tag                   # Upload image
docker build -t myapp:1.0 .                 # Build image
docker build -t myapp:1.0 -f Dockerfile.prod . # Dockerfile tertentu
docker tag myapp:1.0 myapp:latest            # Tag image
docker rmi nginx:latest                      # Hapus image
docker rmi $(docker images -q)               # Hapus semua image
docker image prune                           # Hapus image tidak terpakai
docker image prune -a                        # Hapus semua image tidak dipakai
docker history nginx                         # Layer history
docker inspect nginx                         # Info detail image
docker save nginx > nginx.tar                # Export image
docker load < nginx.tar                      # Import image

# === CONTAINER ===
docker ps                                    # Container berjalan
docker ps -a                                 # Semua container
docker run nginx                             # Jalankan container
docker run -d nginx                          # Background (detach)
docker run -d --name webserver nginx         # Dengan nama
docker run -p 8080:80 nginx                  # Port mapping
docker run -p 127.0.0.1:8080:80 nginx       # Bind ke IP tertentu
docker run -v /host:/container nginx         # Volume mount
docker run -v $(pwd):/app nginx              # Mount direktori saat ini
docker run -e VAR=value nginx                # Environment variable
docker run --env-file .env nginx             # Dari file .env
docker run --network mynet nginx             # Custom network
docker run --rm nginx ls /                   # Hapus setelah selesai
docker run -it ubuntu bash                   # Interactive + terminal
docker run --memory 512m nginx               # Limit memori
docker run --cpus 0.5 nginx                  # Limit CPU
docker run --restart unless-stopped nginx    # Restart policy

# Container management
docker start container_name                  # Start container
docker stop container_name                   # Stop (SIGTERM)
docker kill container_name                   # Kill (SIGKILL)
docker restart container_name               # Restart
docker pause container_name                  # Pause
docker unpause container_name               # Unpause
docker rm container_name                     # Hapus container
docker rm -f container_name                  # Force hapus
docker rm $(docker ps -aq)                   # Hapus semua
docker container prune                       # Hapus container berhenti

# Container info
docker logs container_name                   # Logs
docker logs -f container_name               # Follow logs
docker logs --tail 50 container_name        # 50 baris terakhir
docker logs --since 1h container_name       # 1 jam terakhir
docker inspect container_name               # Detail info
docker stats                                 # Resource usage real-time
docker stats container_name                 # Stats container tertentu
docker top container_name                   # Proses dalam container
docker diff container_name                  # Perubahan filesystem
docker port container_name                  # Port mapping

# Execute dalam container
docker exec container_name ls /              # Jalankan perintah
docker exec -it container_name bash         # Interactive shell
docker exec -it -u root container_name bash # Sebagai root
docker exec container_name env              # Environment vars

# Copy file
docker cp file.txt container:/path/         # Host ke container
docker cp container:/path/file.txt .        # Container ke host

# === NETWORK ===
docker network ls                            # Daftar network
docker network create mynet                 # Buat network
docker network create --driver bridge mynet # Bridge network
docker network create --driver overlay mynet # Overlay network
docker network inspect mynet                # Detail network
docker network connect mynet container       # Hubungkan container
docker network disconnect mynet container    # Putuskan
docker network rm mynet                      # Hapus network
docker network prune                         # Hapus network tidak terpakai

# === VOLUME ===
docker volume ls                             # Daftar volume
docker volume create myvolume               # Buat volume
docker volume inspect myvolume              # Detail volume
docker volume rm myvolume                   # Hapus volume
docker volume prune                          # Hapus volume tidak terpakai

# === SYSTEM ===
docker info                                  # Info Docker
docker version                               # Versi Docker
docker system df                             # Penggunaan disk
docker system prune                          # Bersihkan semua tidak terpakai
docker system prune -a --volumes            # Bersihkan semua termasuk volume
docker events                               # Event real-time
docker search nginx                          # Cari di Docker Hub

# === DOCKER COMPOSE ===
docker compose up                            # Start semua service
docker compose up -d                         # Background
docker compose up --build                    # Rebuild images
docker compose down                          # Stop dan hapus
docker compose down -v                       # Hapus termasuk volumes
docker compose ps                            # Status services
docker compose logs                          # Logs semua
docker compose logs -f service_name         # Follow logs service
docker compose exec service_name bash       # Shell ke service
docker compose build                         # Build semua
docker compose build service_name           # Build service tertentu
docker compose pull                          # Pull semua images
docker compose restart service_name         # Restart service
docker compose stop                          # Stop semua
docker compose start                         # Start semua
docker compose scale service=3              # Scale service

# Contoh docker-compose.yml
cat > docker-compose.yml << 'EOF'
version: '3.8'

services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./html:/usr/share/nginx/html:ro
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - app
    restart: unless-stopped
    networks:
      - frontend

  app:
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      - NODE_ENV=production
      - DB_HOST=db
    env_file:
      - .env
    volumes:
      - app_data:/app/data
    depends_on:
      - db
    restart: unless-stopped
    networks:
      - frontend
      - backend

  db:
    image: postgres:15
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped
    networks:
      - backend

  redis:
    image: redis:7-alpine
    command: redis-server --requirepass secret
    volumes:
      - redis_data:/data
    restart: unless-stopped
    networks:
      - backend

volumes:
  postgres_data:
  redis_data:
  app_data:

networks:
  frontend:
  backend:
    internal: true
EOF
```

---

### `kubectl` - Kubernetes CLI
```bash
# Cluster info
kubectl cluster-info                          # Info cluster
kubectl get nodes                             # Daftar node
kubectl get nodes -o wide                     # Detail node
kubectl describe node nodename               # Detail node
kubectl top nodes                             # Resource usage

# Pods
kubectl get pods                              # Daftar pods (namespace saat ini)
kubectl get pods -A                           # Semua namespace
kubectl get pods -n kube-system              # Namespace tertentu
kubectl get pods -o wide                     # Dengan IP dan node
kubectl get pods --show-labels               # Tampilkan labels
kubectl get pods -l app=nginx                # Filter by label
kubectl describe pod podname                 # Detail pod
kubectl logs podname                          # Logs pod
kubectl logs -f podname                      # Follow logs
kubectl logs podname -c container            # Container tertentu
kubectl exec -it podname -- bash             # Shell ke pod
kubectl exec podname -- ls /app              # Jalankan perintah
kubectl delete pod podname                   # Hapus pod
kubectl delete pod podname --force           # Force hapus

# Deployments
kubectl get deployments                      # Daftar deployment
kubectl describe deployment myapp            # Detail
kubectl create deployment myapp --image=nginx  # Buat deployment
kubectl apply -f deployment.yaml             # Apply manifest
kubectl delete -f deployment.yaml            # Hapus dari manifest
kubectl scale deployment myapp --replicas=3  # Scale
kubectl rollout status deployment/myapp      # Status rollout
kubectl rollout history deployment/myapp     # History rollout
kubectl rollout undo deployment/myapp        # Rollback
kubectl rollout undo deployment/myapp --to-revision=2  # Rollback ke versi
kubectl set image deployment/myapp container=nginx:1.25  # Update image

# Services
kubectl get services                          # Daftar service
kubectl get svc                               # Shorthand
kubectl describe svc myservice               # Detail
kubectl expose deployment myapp --port=80 --type=ClusterIP  # Expose
kubectl expose deployment myapp --port=80 --type=NodePort
kubectl expose deployment myapp --port=80 --type=LoadBalancer
kubectl delete svc myservice                 # Hapus

# Namespaces
kubectl get namespaces                        # Daftar namespace
kubectl get ns                                # Shorthand
kubectl create namespace myns                # Buat namespace
kubectl delete namespace myns                # Hapus
kubectl config set-context --current --namespace=myns  # Ganti default NS

# ConfigMaps & Secrets
kubectl get configmaps                        # Daftar configmap
kubectl get secrets                           # Daftar secret
kubectl create configmap myconfig --from-file=config.conf
kubectl create configmap myconfig --from-literal=KEY=VALUE
kubectl create secret generic mysecret --from-literal=password=secret
kubectl get configmap myconfig -o yaml       # Lihat isi
kubectl describe secret mysecret             # Detail secret
kubectl delete configmap myconfig            # Hapus

# Persistent Volumes
kubectl get pv                                # Persistent Volumes
kubectl get pvc                               # PVC
kubectl describe pvc mypvc                   # Detail PVC

# Resource monitoring
kubectl top pods                              # CPU/Memory pods
kubectl top pods -A                           # Semua namespace
kubectl top nodes                             # CPU/Memory nodes

# Port forwarding
kubectl port-forward pod/podname 8080:80     # Forward port
kubectl port-forward svc/myservice 8080:80   # Dari service
kubectl port-forward deployment/myapp 8080:80

# Ingress
kubectl get ingress                           # Daftar ingress
kubectl describe ingress myingress           # Detail

# Output format
kubectl get pods -o yaml                     # YAML output
kubectl get pods -o json                     # JSON output
kubectl get pods -o jsonpath='{.items[*].metadata.name}'  # JSONPath
kubectl get pods --sort-by='.metadata.name'  # Sort
kubectl get pods --field-selector status.phase=Running    # Filter

# Context management
kubectl config get-contexts                  # Daftar context
kubectl config current-context              # Context saat ini
kubectl config use-context mycontext        # Ganti context
kubectl config set-context mycontext --cluster=mycluster --user=myuser

# Apply dan debug
kubectl apply -f manifest.yaml --dry-run=client  # Dry run
kubectl diff -f manifest.yaml                # Diff dengan yang berjalan
kubectl explain pod                          # Dokumentasi resource
kubectl explain pod.spec.containers          # Nested field
kubectl api-resources                        # Semua resource types
kubectl api-versions                         # Semua API versions
```

---

## 15.5 Version Control (Git) Lanjutan

### Git Commands Lanjutan
```bash
# === BASIC ===
git init                                     # Inisialisasi repo
git init --bare                              # Bare repo (server)
git clone https://url/repo.git              # Clone repo
git clone --depth 1 https://url/repo.git   # Shallow clone
git clone --single-branch -b main url      # Clone satu branch
git status                                   # Status repo
git status -s                               # Short format
git add file.txt                             # Stage file
git add .                                    # Stage semua
git add -p                                   # Stage interaktif (patch)
git add -u                                   # Stage modified + deleted
git commit -m "pesan"                       # Commit
git commit -am "pesan"                      # Stage + commit
git commit --amend                           # Amend commit terakhir
git commit --amend --no-edit                # Amend tanpa ganti pesan

# === BRANCH ===
git branch                                   # Daftar branch lokal
git branch -a                               # Semua branch
git branch -r                               # Remote branch
git branch -v                               # Dengan last commit
git branch newbranch                         # Buat branch
git branch -d branch                         # Hapus branch (aman)
git branch -D branch                         # Force hapus branch
git branch -m lama baru                     # Rename branch
git checkout branch                          # Pindah branch
git checkout -b newbranch                   # Buat + pindah
git switch branch                            # Pindah (modern)
git switch -c newbranch                     # Buat + pindah (modern)
git checkout -                               # Kembali ke branch sebelumnya

# === REMOTE ===
git remote -v                               # Daftar remote
git remote add origin https://url/repo.git  # Tambah remote
git remote rename origin backup             # Rename remote
git remote remove backup                    # Hapus remote
git remote set-url origin https://new-url   # Ganti URL
git fetch                                    # Fetch semua remote
git fetch origin                             # Fetch dari origin
git fetch --all                              # Fetch semua remote
git pull                                     # Fetch + merge
git pull --rebase                           # Fetch + rebase
git pull origin main                         # Pull branch tertentu
git push origin main                         # Push ke remote
git push -u origin main                     # Push + set upstream
git push --force-with-lease                 # Safe force push
git push origin --delete branch            # Hapus remote branch
git push --tags                              # Push semua tags

# === MERGE & REBASE ===
git merge branch                             # Merge branch ke saat ini
git merge --no-ff branch                    # No fast-forward merge
git merge --squash branch                   # Squash merge
git merge --abort                           # Batalkan merge
git rebase main                              # Rebase ke main
git rebase -i HEAD~3                        # Interactive rebase (3 commit)
git rebase --abort                           # Batalkan rebase
git rebase --continue                        # Lanjutkan setelah resolve

# === STASH ===
git stash                                    # Simpan perubahan sementara
git stash save "pesan"                      # Stash dengan pesan
git stash push -m "pesan" file.txt          # Stash file tertentu
git stash list                               # Daftar stash
git stash pop                                # Ambil stash terakhir
git stash pop stash@{2}                     # Stash tertentu
git stash apply                              # Apply tanpa hapus
git stash drop stash@{0}                    # Hapus stash
git stash clear                              # Hapus semua stash
git stash show -p                           # Lihat isi stash
git stash branch newbranch                  # Stash ke branch baru

# === LOG & DIFF ===
git log                                      # Riwayat commit
git log --oneline                           # Satu baris per commit
git log --graph --oneline --all             # Graph semua branch
git log -n 10                               # 10 commit terakhir
git log --since="2024-01-01"               # Sejak tanggal
git log --author="John"                     # Oleh author
git log --grep="fix"                        # Pesan mengandung "fix"
git log -S "fungsi()"                       # Commit yang mengubah string
git log --follow file.txt                   # History file (termasuk rename)
git log --stat                               # Dengan statistik file
git log -p                                   # Dengan diff
git log --format="%H %an %s"               # Format kustom
git show abc1234                             # Detail commit
git show HEAD                                # Commit terakhir
git show HEAD~2                              # Dua commit sebelumnya
git diff                                     # Perubahan unstaged
git diff --staged                            # Perubahan staged
git diff main..branch                        # Antar branch
git diff HEAD~3 HEAD                        # 3 commit terakhir
git diff --stat                              # Statistik

# === RESET & REVERT ===
git reset HEAD file.txt                     # Unstage file
git reset --soft HEAD~1                     # Undo commit, keep staged
git reset --mixed HEAD~1                    # Undo commit, keep unstaged
git reset --hard HEAD~1                     # Undo commit, hapus perubahan
git reset --hard origin/main               # Reset ke remote
git revert HEAD                              # Buat commit undo
git revert abc1234                           # Revert commit tertentu
git revert --no-commit HEAD~3..HEAD         # Revert beberapa tanpa commit
git restore file.txt                        # Discard perubahan
git restore --staged file.txt               # Unstage
git restore --source=HEAD~2 file.txt       # Restore dari commit

# === CHERRY-PICK ===
git cherry-pick abc1234                     # Copy commit ke branch saat ini
git cherry-pick abc1234 def5678             # Multiple commits
git cherry-pick abc1234..def5678            # Range commit
git cherry-pick --no-commit abc1234        # Tanpa auto-commit
git cherry-pick --abort                     # Batalkan

# === TAG ===
git tag                                      # Daftar tag
git tag v1.0                                 # Buat lightweight tag
git tag -a v1.0 -m "Release 1.0"           # Annotated tag
git tag -a v1.0 abc1234                     # Tag commit tertentu
git show v1.0                               # Detail tag
git push origin v1.0                        # Push tag
git push origin --tags                      # Push semua tag
git tag -d v1.0                             # Hapus tag lokal
git push origin --delete v1.0              # Hapus tag remote

# === SUBMODULE ===
git submodule add https://url/repo.git lib/repo  # Tambah submodule
git submodule init                           # Inisialisasi
git submodule update                         # Update
git submodule update --init --recursive     # Init + update rekursif
git submodule foreach git pull              # Pull semua submodule
git submodule status                         # Status submodule

# === WORKTREE ===
git worktree add ../hotfix hotfix-branch    # Buat worktree
git worktree list                            # Daftar worktree
git worktree remove ../hotfix               # Hapus worktree

# === BISECT ===
git bisect start                             # Mulai bisect
git bisect bad HEAD                          # Commit saat ini buruk
git bisect good v1.0                         # Tag yang baik
# Test, lalu:
git bisect good                              # Jika ini baik
git bisect bad                               # Jika ini buruk
git bisect reset                             # Selesai bisect

# === BLAME ===
git blame file.txt                           # Siapa yang mengubah setiap baris
git blame -L 10,20 file.txt                 # Baris 10-20
git blame -w file.txt                        # Abaikan whitespace
git blame --since="1 month" file.txt        # Dalam 1 bulan

# === HOOKS ===
ls .git/hooks/                               # Lihat hooks tersedia
cat .git/hooks/pre-commit.sample            # Contoh hook

# Contoh pre-commit hook
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/bash
# Cek syntax Python
git diff --cached --name-only --diff-filter=ACM | \
    grep ".py$" | \
    xargs python3 -m py_compile 2>&1 && \
    echo "Python syntax OK"
EOF
chmod +x .git/hooks/pre-commit

# === MAINTENANCE ===
git gc                                       # Garbage collection
git gc --aggressive                          # Agresif GC
git prune                                    # Hapus unreachable objects
git fsck                                     # Verifikasi integritas
git reflog                                   # Riwayat HEAD
git reflog show HEAD                         # Riwayat HEAD
git reflog expire --expire=90.days           # Hapus reflog lama
git count-objects -v                         # Ukuran repo
git clean -fd                                # Hapus untracked files
git clean -fdx                               # Hapus + ignored files
git clean -n                                 # Dry run
```

---

## 15.6 Database Tools

### MySQL/MariaDB CLI
```bash
# Koneksi
mysql -u root -p                             # Login sebagai root
mysql -u user -p database                    # Login ke database tertentu
mysql -h hostname -u user -p                 # Remote server
mysql -h host -P 3306 -u user -p db         # Dengan port
mysql -u user -p -e "SELECT 1"              # Jalankan query

# Manajemen database
mysql -u root -p -e "SHOW DATABASES;"
mysql -u root -p -e "CREATE DATABASE mydb;"
mysql -u root -p -e "DROP DATABASE mydb;"
mysql -u root -p -e "USE mydb; SHOW TABLES;"

# Import/export
mysqldump -u root -p mydb > backup.sql       # Dump database
mysqldump -u root -p --all-databases > all.sql  # Semua database
mysqldump -u root -p mydb table1 > table.sql   # Table tertentu
mysqldump -u root -p mydb | gzip > backup.sql.gz  # Kompres
mysql -u root -p mydb < backup.sql           # Import
gunzip < backup.sql.gz | mysql -u root -p mydb   # Import compressed

# User management
mysql -u root -p -e "CREATE USER 'user'@'localhost' IDENTIFIED BY 'pass';"
mysql -u root -p -e "GRANT ALL PRIVILEGES ON mydb.* TO 'user'@'localhost';"
mysql -u root -p -e "FLUSH PRIVILEGES;"
mysql -u root -p -e "SHOW GRANTS FOR 'user'@'localhost';"
mysql -u root -p -e "DROP USER 'user'@'localhost';"

# mysqladmin
mysqladmin -u root -p status                # Status server
mysqladmin -u root -p processlist           # Proses aktif
mysqladmin -u root -p kill PID             # Kill query
mysqladmin -u root -p flush-logs           # Flush logs
mysqladmin -u root -p reload               # Reload grant tables
mysqladmin -u root -p shutdown             # Shutdown server
mysqladmin -u root -p create mydb         # Buat database
mysqladmin -u root -p drop mydb           # Hapus database
mysqladmin -u root -p password newpass    # Ganti password root
```

---

### PostgreSQL CLI
```bash
# Koneksi
psql -U postgres                             # Login sebagai postgres
psql -U user -d database                    # User dan database
psql -h host -U user -d db                 # Remote
psql -h host -p 5432 -U user -d db        # Dengan port
psql "postgresql://user:pass@host/db"      # URI format

# Perintah psql (meta-commands)
\l                                           # Daftar database
\c dbname                                    # Connect ke database
\dt                                          # Daftar tabel
\dt schema.*                                 # Tabel dalam schema
\d tablename                                 # Describe tabel
\du                                          # Daftar user/role
\dn                                          # Daftar schema
\df                                          # Daftar fungsi
\dv                                          # Daftar view
\di                                          # Daftar index
\x                                           # Toggle expanded output
\timing                                      # Toggle timing
\i file.sql                                  # Jalankan file SQL
\o output.txt                                # Output ke file
\copy table TO 'file.csv' CSV               # Export CSV
\copy table FROM 'file.csv' CSV            # Import CSV
\q                                           # Keluar
\?                                           # Bantuan

# Dari command line
psql -U postgres -c "SELECT version();"     # Jalankan query
psql -U postgres -f script.sql              # Jalankan file SQL
psql -U postgres -c "\l"                    # Daftar database

# pg_dump - backup
pg_dump -U postgres mydb > backup.sql       # Dump database
pg_dump -U postgres -F c mydb > backup.dump # Custom format
pg_dump -U postgres -t tablename mydb > table.sql  # Table tertentu
pg_dumpall -U postgres > all.sql            # Semua database
pg_dump -U postgres mydb | gzip > backup.sql.gz  # Kompres

# pg_restore - restore
pg_restore -U postgres -d mydb backup.dump  # Restore custom format
psql -U postgres mydb < backup.sql           # Restore SQL format
gunzip < backup.sql.gz | psql -U postgres mydb

# psql variables
psql -U postgres -v ON_ERROR_STOP=1 -f script.sql
```

---

### Redis CLI
```bash
redis-cli                                    # Connect lokal
redis-cli -h hostname -p 6379              # Remote
redis-cli -h host -p 6379 -a password      # Dengan auth
redis-cli -n 1                              # Database ke-1
redis-cli ping                              # Test koneksi
redis-cli info                              # Info server
redis-cli info server                       # Section tertentu
redis-cli monitor                           # Monitor semua perintah
redis-cli --stat                            # Statistik real-time
redis-cli --latency                         # Latency monitoring
redis-cli --bigkeys                         # Cari key besar
redis-cli --memkeys                         # Ukuran memori per key
redis-cli --scan                            # Scan semua key
redis-cli --scan --pattern "user:*"         # Pattern tertentu

# Key management
redis-cli SET key value                     # Set key
redis-cli GET key                           # Get value
redis-cli DEL key                           # Hapus key
redis-cli EXISTS key                        # Cek key ada
redis-cli EXPIRE key 3600                   # Expire dalam 3600 detik
redis-cli TTL key                           # Sisa waktu expire
redis-cli KEYS "*"                          # Semua key (hindari di produksi)
redis-cli SCAN 0                            # Scan (aman)
redis-cli TYPE key                          # Tipe data key

# Backup
redis-cli SAVE                              # Synchronous save
redis-cli BGSAVE                            # Background save
redis-cli BGREWRITEAOF                     # Rewrite AOF
redis-cli CONFIG GET dir                    # Direktori data
redis-cli CONFIG GET dbfilename             # Nama file RDB
redis-cli DEBUG SLEEP 0                     # Test koneksi

# Batch commands
cat commands.txt | redis-cli                # Dari file
redis-cli --pipe < commands.txt            # Pipeline mode
```

---

## 15.7 Text Utilities Lanjutan

### `bat` - Better Cat
```bash
sudo apt install bat
# atau
sudo snap install bat

bat file.txt                                 # Tampilkan dengan syntax highlight
bat file.py                                  # Python dengan highlight
bat -n file.txt                              # Dengan nomor baris
bat -A file.txt                              # Tampilkan karakter tersembunyi
bat -p file.txt                              # Plain output (tanpa header)
bat -l json file.txt                        # Tentukan bahasa
bat --style=plain file.txt                   # Style minimalis
bat --style=full file.txt                    # Style penuh
bat --theme=TwoDark file.txt                 # Tema tertentu
bat --list-themes                            # Daftar tema
bat --list-languages                         # Bahasa yang didukung
bat file1.txt file2.txt                      # Beberapa file
cat file.txt | bat -l json                  # Dari stdin dengan bahasa
bat --diff file.txt                          # Diff mode
bat --map-syntax "*.conf:INI" conf.txt      # Map ekstensi ke bahasa

# Alias (gantikan cat)
alias cat='bat'
alias catp='bat -p'                          # Plain
```

---

### `exa` / `eza` - Better ls
```bash
sudo apt install exa
# atau untuk eza (fork aktif dari exa)
sudo apt install eza

exa                                          # List files
exa -l                                       # Long format
exa -la                                      # Long + all
exa -lah                                     # Long + all + human-readable
exa --tree                                   # Tree view
exa --tree -L 2                             # Tree depth 2
exa -l --git                                 # Tampilkan git status
exa -l --git-ignore                          # Sembunyikan file git-ignored
exa -l --icons                               # Tampilkan icons (butuh nerd font)
exa -l --color-scale                         # Gradient warna ukuran
exa --sort size                              # Sort by size
exa --sort modified                          # Sort by modified
exa --sort extension                         # Sort by extension
exa -r --sort size                           # Reverse sort
exa --group-directories-first               # Direktori dulu
exa --only-dirs                              # Hanya direktori
exa -l --time-style long-iso                # Format waktu ISO
exa --classify                               # Tambah indikator tipe

# Alias
alias ls='exa'
alias ll='exa -l'
alias la='exa -la'
alias lt='exa --tree'
```

---

### `delta` - Better Diff
```bash
# Install
cargo install git-delta
# atau
sudo apt install git-delta

# Gunakan langsung
diff file1.txt file2.txt | delta
git diff | delta

# Konfigurasi di ~/.gitconfig
cat >> ~/.gitconfig << 'EOF'
[core]
    pager = delta

[delta]
    navigate = true
    light = false
    side-by-side = true
    line-numbers = true
    syntax-theme = Dracula

[interactive]
    diffFilter = delta --color-only
EOF

# Opsi delta
delta --side-by-side file1 file2            # Side by side
delta --line-numbers file1 file2            # Dengan nomor baris
delta --diff-so-fancy file1 file2           # diff-so-fancy style
delta --light file1 file2                   # Light mode
delta --list-syntax-themes                  # Daftar tema
```

---

### `zoxide` - Smarter cd
```bash
# Install
curl -sS https://raw.githubusercontent.com/ajeetdsouza/zoxide/main/install.sh | bash

# Tambahkan ke ~/.bashrc
eval "$(zoxide init bash)"

# Penggunaan
z Documents                                  # cd ke direktori mengandung "Documents"
z proj                                       # cd ke direktori mengandung "proj"
z -                                          # cd ke direktori sebelumnya
zi                                           # Interactive selection (butuh fzf)
zoxide query Documents                       # Cari tanpa pindah
zoxide query --list                          # Semua direktori yang diketahui
zoxide add /path/to/dir                     # Tambah manual
zoxide remove /path/to/dir                  # Hapus
zoxide edit                                  # Edit database

# Alias
alias cd='z'
```

---

### `tldr` - Simplified Man Pages
```bash
# Install
sudo apt install tldr
# atau
pip install tldr

# Gunakan
tldr tar                                     # Contoh penggunaan tar
tldr git                                     # Contoh git
tldr find                                    # Contoh find
tldr --update                               # Update database
tldr --list                                 # Daftar semua halaman
tldr --platform linux tar                   # Platform tertentu
tldr --language id tar                      # Bahasa Indonesia

# tealdeer (rust implementation, lebih cepat)
cargo install tealdeer
tldr --update
tldr tar
```

---

## 15.8 Productivity Tools

### `ranger` - Terminal File Manager
```bash
sudo apt install ranger

ranger                                       # Buka ranger
ranger /path/                               # Buka path tertentu
ranger --choosefile=/tmp/chosen              # Output file yang dipilih

# Shortcut ranger
h,j,k,l     # Navigasi (vim-like)
gg / G      # Ke atas/bawah
H / L       # Riwayat direktori
q           # Keluar
Q           # Quit + print path
yy          # Copy
dd          # Cut
pp          # Paste
dD          # Delete
cw          # Rename
/           # Cari
n / N       # Cari berikutnya/sebelumnya
zh          # Toggle hidden files
zf          # Filter
s           # Shell command
S           # Shell di direktori ini
r           # Buka dengan program tertentu
E           # Edit dalam $EDITOR
o           # Ganti sort order
.           # Toggle filter dots
```

---

### `fzf` Integrasi Lanjutan
```bash
# Fuzzy file open dengan preview
fzf --preview 'bat --style=numbers --color=always {}'

# Fuzzy grep
fzf_grep() {
    local result
    result=$(rg --color=always --line-number '' | \
        fzf --ansi --delimiter ':' \
            --preview 'bat --style=numbers --color=always --highlight-line {2} {1}' \
            --preview-window 'up,60%,border-bottom,+{2}+3/3,~3')
    echo "${result%%:*}"
}

# Git log dengan fzf
git_log_fzf() {
    git log --oneline --color=always | \
        fzf --ansi --preview 'git show --color=always {1}' | \
        awk '{print $1}'
}

# Docker container select
docker_exec() {
    local container
    container=$(docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}" | \
        fzf --header-lines=1 | awk '{print $1}')
    [ -n "$container" ] && docker exec -it "$container" bash
}

# Process kill dengan fzf
fkill() {
    local pid
    pid=$(ps aux | fzf --header-lines=1 | awk '{print $2}')
    [ -n "$pid" ] && kill "$@" "$pid"
}

# SSH host dari config
fssh() {
    local host
    host=$(grep -E "^Host\s+" ~/.ssh/config | awk '{print $2}' | fzf)
    [ -n "$host" ] && ssh "$host"
}

# Port kill
fport() {
    local port pid
    port=$(ss -tulpn | fzf --header-lines=1 | grep -oP ':\K\d+(?=\s)')
    pid=$(ss -tulpn | grep ":$port" | grep -oP 'pid=\K\d+')
    [ -n "$pid" ] && kill "$pid"
}
```

---

### `tmux-sessionizer` - Project Switcher
```bash
# Script untuk switch antara project dengan tmux
cat > ~/.local/bin/tmux-sessionizer << 'EOF'
#!/bin/bash

# Direktori project
PROJECT_DIRS=(
    "$HOME/projects"
    "$HOME/work"
    "$HOME/repos"
)

# Pilih direktori dengan fzf
if [[ $# -eq 1 ]]; then
    selected=$1
else
    selected=$(find "${PROJECT_DIRS[@]}" -mindepth 1 -maxdepth 1 -type d 2>/dev/null | \
        fzf --preview 'ls {}')
fi

[ -z "$selected" ] && exit 0

# Buat session name dari path
selected_name=$(basename "$selected" | tr . _)

# Buat atau switch tmux session
if ! tmux has-session -t "$selected_name" 2>/dev/null; then
    tmux new-session -ds "$selected_name" -c "$selected"
fi

if [ -z "$TMUX" ]; then
    tmux attach -t "$selected_name"
else
    tmux switch-client -t "$selected_name"
fi
EOF

chmod +x ~/.local/bin/tmux-sessionizer

# Bind ke key tmux
echo 'bind-key -r f run-shell "tmux neww ~/.local/bin/tmux-sessionizer"' >> ~/.tmux.conf
```

---

## 15.9 Security Tools

### `fail2ban` - Intrusion Prevention
```bash
sudo apt install fail2ban

# Konfigurasi
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local

# Konfigurasi jail
cat > /etc/fail2ban/jail.local << 'EOF'
[DEFAULT]
bantime = 1h
findtime = 10m
maxretry = 5
ignoreip = 127.0.0.1/8 192.168.1.0/24
backend = systemd

[sshd]
enabled = true
port = 22
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 24h

[nginx-http-auth]
enabled = true
filter = nginx-http-auth
port = http,https
logpath = /var/log/nginx/error.log

[nginx-limit-req]
enabled = true
filter = nginx-limit-req
port = http,https
logpath = /var/log/nginx/error.log

[php-url-fopen]
enabled = true
port = http,https
filter = php-url-fopen
logpath = /var/log/nginx/access.log
maxretry = 2
EOF

# Manajemen fail2ban
sudo systemctl start fail2ban
sudo systemctl enable fail2ban
sudo fail2ban-client status              # Status semua jail
sudo fail2ban-client status sshd        # Status jail tertentu
sudo fail2ban-client set sshd banip 192.168.1.100  # Ban IP manual
sudo fail2ban-client set sshd unbanip 192.168.1.100  # Unban IP
sudo fail2ban-client reload             # Reload konfigurasi
sudo fail2ban-client restart            # Restart
sudo fail2ban-client ping               # Test koneksi
sudo fail2ban-client get sshd bantime   # Lihat konfigurasi
sudo zgrep "Ban" /var/log/fail2ban.log  # Log ban
```

---

### `lynis` - Security Auditing
```bash
sudo apt install lynis

sudo lynis audit system                  # Audit sistem lengkap
sudo lynis audit system --quick         # Audit cepat
sudo lynis audit system --tests-from-group authentication  # Grup tertentu
sudo lynis show tests                    # Daftar semua test
sudo lynis show categories               # Kategori test
sudo lynis show groups                   # Grup test
sudo lynis update info                   # Cek update
sudo lynis --check-update               # Cek versi
cat /var/log/lynis.log                  # Log audit
cat /var/log/lynis-report.dat           # Report
```

---

### `rkhunter` - Rootkit Hunter
```bash
sudo apt install rkhunter

sudo rkhunter --update                   # Update database
sudo rkhunter --check                    # Scan sistem
sudo rkhunter --check --skip-keypress   # Tanpa pause
sudo rkhunter --check --report-warnings-only  # Hanya warning
sudo rkhunter --propupd                  # Update file properties
cat /var/log/rkhunter.log               # Lihat log
```

---

### `chkrootkit` - Check for Rootkits
```bash
sudo apt install chkrootkit

sudo chkrootkit                          # Check semua
sudo chkrootkit -l                       # Daftar test yang tersedia
sudo chkrootkit lkm                      # Check kernel modules
sudo chkrootkit sniffer                  # Check sniffer
sudo chkrootkit -x                       # Expert mode
```

---

### `openssl` - Cryptography Swiss Knife
```bash
# Generate keys
openssl genrsa -out private.key 4096     # RSA private key
openssl rsa -in private.key -pubout -out public.key  # Extract public
openssl genrsa -aes256 -out private.key 4096  # Encrypted private key
openssl ecparam -name prime256v1 -genkey -out ec.key  # EC key

# Certificates
openssl req -new -key private.key -out cert.csr  # CSR
openssl req -x509 -new -nodes -key private.key -sha256 -days 365 -out cert.crt  # Self-signed
openssl x509 -in cert.crt -text -noout  # Lihat sertifikat
openssl x509 -in cert.crt -noout -dates # Masa berlaku
openssl x509 -in cert.crt -noout -subject  # Subject
openssl x509 -in cert.crt -noout -issuer   # Issuer
openssl x509 -in cert.crt -noout -fingerprint  # Fingerprint

# Verifikasi
openssl verify -CAfile ca.crt cert.crt   # Verifikasi dengan CA
openssl s_client -connect host:443       # Test SSL koneksi
openssl s_client -connect host:443 -showcerts  # Lihat semua sertifikat
openssl s_client -connect host:443 </dev/null 2>/dev/null | \
    openssl x509 -noout -dates           # Cek expire

# Enkripsi file
openssl enc -aes-256-cbc -salt -in file.txt -out file.enc
openssl enc -aes-256-cbc -d -in file.enc -out file.txt

# Hash
openssl dgst -sha256 file.txt           # SHA-256
openssl dgst -md5 file.txt              # MD5

# Base64
openssl base64 -in file.bin -out file.b64
openssl base64 -d -in file.b64 -out file.bin

# Random data
openssl rand -hex 32                     # 32 byte hex
openssl rand -base64 32                  # 32 byte base64

# PKCS12 (pfx)
openssl pkcs12 -export -out cert.pfx -inkey private.key -in cert.crt -certfile ca.crt
openssl pkcs12 -in cert.pfx -out cert.pem -nodes

# Test performa
openssl speed aes-256-cbc               # Benchmark AES
openssl speed rsa4096                    # Benchmark RSA
```

---

## 15.10 Miscellaneous Utilities

### `bc` - Calculator
```bash
bc                                       # Mode interaktif
bc -l                                    # Dengan math library

# Kalkulasi dari command line
echo "5 + 3" | bc
echo "10 / 3" | bc                       # Integer division
echo "scale=2; 10 / 3" | bc            # 2 desimal
echo "scale=10; sqrt(2)" | bc          # Sqrt dengan 10 desimal
echo "2^10" | bc                         # Power
echo "obase=16; 255" | bc               # Konversi ke hex
echo "ibase=16; FF" | bc               # Hex ke desimal
echo "obase=2; 10" | bc                 # Desimal ke biner
echo "c=1; for(i=1;i<=10;i++) c*=i; c" | bc  # Faktorial

# Fungsi
cat << 'EOF' | bc -l
define fib(n) {
    if (n <= 1) return n;
    return fib(n-1) + fib(n-2);
}
fib(10)
EOF
```

---

### `units` - Unit Converter
```bash
sudo apt install units

units "1 km" "miles"                     # Konversi km ke miles
units "100 USD" "EUR"                    # Konversi mata uang
units "1 GB" "MB"                        # Konversi storage
units "30 Celsius" "Fahrenheit"          # Konversi suhu
units "1 hour" "minutes"                 # Konversi waktu
units "1 m/s" "km/h"                    # Konversi kecepatan
units "1 bar" "psi"                      # Konversi tekanan
units --list                              # Daftar semua unit
```

---

### `cowsay` & `fortune` - Fun Utilities
```bash
sudo apt install cowsay fortune

fortune                                  # Tampilkan quote acak
cowsay "Hello, Linux!"                  # Sapi berbicara
fortune | cowsay                         # Quote dari sapi
cowsay -f tux "Hello"                   # Karakter tux (Linux penguin)
cowsay -l                               # Daftar karakter tersedia
cowthink "Hmm..."                        # Sapi berpikir
```

---

### `lolcat` - Rainbow Text
```bash
sudo apt install lolcat

echo "Hello World" | lolcat              # Teks pelangi
cat file.txt | lolcat                    # File pelangi
ls | lolcat                              # ls pelangi
lolcat -a file.txt                       # Animasi
fortune | cowsay | lolcat               # Triple fun
```

---

### `figlet` & `toilet` - ASCII Art Text
```bash
sudo apt install figlet toilet

figlet "Hello"                           # ASCII art teks
figlet -f slant "Linux"                 # Font lain
figlet -l                                # Daftar font
toilet "Hello"                           # Toilet (warna)
toilet -f mono9 -F metal "Linux"        # Font + filter
toilet -f big -F rainbow "Hello"        # Rainbow
```

---

### `pv` - Pipe Viewer
```bash
sudo apt install pv

# Monitor progress transfer
pv file.txt | gzip > file.txt.gz        # Progress compress
pv backup.tar.gz | tar xz               # Progress extract
dd if=/dev/sda | pv | dd of=/dev/sdb   # Progress disk clone
cat large_file.sql | pv | mysql -u root -p mydb  # Progress import

# Opsi
pv -b file.txt                           # Hanya bytes
pv -r file.txt                           # Hanya rate
pv -e file.txt                           # Hanya ETA
pv -t file.txt                           # Hanya timer
pv -p file.txt                           # Hanya progress bar
pv -s 100M file.txt                     # Set expected size
pv -l file.txt                           # Hitung baris
pv -q file.txt                           # Quiet (hanya data)
pv -L 1M file.txt                        # Rate limit 1MB/s
```

---

### `parallel` - GNU Parallel
```bash
sudo apt install parallel

# Dasar
parallel echo ::: 1 2 3 4 5             # Paralel print
parallel "echo {}" ::: a b c            # Dengan placeholder
parallel -j4 perintah ::: arg1 arg2 arg3  # 4 job paralel

# Dari file
parallel -a input.txt perintah          # Baca dari file
cat input.txt | parallel perintah       # Dari stdin

# Multiple inputs
parallel echo {1} {2} ::: a b ::: 1 2  # Kombinasi

# Dengan replacement
parallel "convert {} {.}.png" ::: *.jpg  # Konversi image
parallel "ffmpeg -i {} {.}.mp3" ::: *.mp4  # Konversi video
parallel "gzip {}" ::: *.log            # Compress files

# Progress
parallel --progress perintah ::: *.txt  # Tampilkan progress
parallel --bar perintah ::: *.txt       # Progress bar
parallel --eta perintah ::: *.txt       # ETA

# Logging
parallel --results output/ perintah ::: args  # Simpan output

# Retry
parallel --retries 3 perintah ::: args  # Retry 3 kali jika gagal

# SSH
parallel -S user@host1,user@host2 perintah ::: args  # Remote execution
parallel --sshloginfile servers.txt perintah ::: args

# Contoh praktis
# Download paralel
parallel -j5 wget ::: url1 url2 url3 url4 url5

# Test banyak host
parallel ping -c1 {} ::: host1 host2 host3 | grep -E "^PING|packet loss"

# Resize images paralel
find . -name "*.jpg" | parallel -j4 "convert {} -resize 50% small_{/}"

# Backup ke banyak server
parallel "rsync -az /data/ {}:/backup/" ::: server1 server2 server3
```

---

## Ringkasan Bagian 15

```
Text Processing:
  jq               → JSON processing
  yq               → YAML processing
  xmlstarlet       → XML processing
  csvkit           → CSV processing suite

System Admin:
  systemd-analyze  → Boot time analysis
  systemd-cgls     → Cgroup hierarchy
  systemd-cgtop    → Cgroup resource monitor
  nsenter          → Enter namespaces
  unshare          → New namespace isolation

Container:
  docker           → Container management
  docker compose   → Multi-container apps
  kubectl          → Kubernetes CLI

Version Control:
  git (lanjutan)   → Branch, stash, rebase, bisect

Database:
  mysql/mysqldump  → MySQL CLI + backup
  psql/pg_dump     → PostgreSQL CLI + backup
  redis-cli        → Redis CLI

Modern CLI Tools:
  bat              → Better cat
  exa/eza          → Better ls
  delta            → Better diff
  zoxide           → Smarter cd
  tldr             → Simple man pages
  ranger           → Terminal file manager
  fzf              → Fuzzy finder

Security:
  fail2ban         → Intrusion prevention
  lynis            → Security auditing
  rkhunter         → Rootkit detection
  openssl          → Cryptography tools

Utilities:
  bc               → Calculator
  units            → Unit converter
  pv               → Pipe viewer (progress)
  parallel         → GNU parallel execution
  cowsay/figlet    → Fun terminal tools
```

---

## 🎉 SEMUA BAGIAN SELESAI!

---

## Ringkasan Lengkap 15 Bagian

```
Bagian 1  → Navigasi & Manajemen File/Direktori
Bagian 2  → Manajemen Teks & File Content
Bagian 3  → Manajemen User & Permission
Bagian 4  → Manajemen Proses
Bagian 5  → Manajemen Paket
Bagian 6  → Jaringan (Network)
Bagian 7  → Disk & Storage
Bagian 8  → Sistem & Hardware Info
Bagian 9  → Kompresi & Arsip
Bagian 10 → Shell & Scripting
Bagian 11 → SSH & Remote
Bagian 12 → Monitoring & Log
Bagian 13 → Pencarian (Search)
Bagian 14 → Variabel Lingkungan & Konfigurasi
Bagian 15 → Utilitas Tambahan & Lanjutan
```

---

## Tips Belajar Linux

```bash
# 1. Gunakan man untuk dokumentasi
man perintah                     # Manual page
man -k keyword                   # Cari perintah

# 2. Gunakan tldr untuk contoh cepat
tldr perintah

# 3. Gunakan --help
perintah --help

# 4. Praktikkan di lab virtual
# Gunakan VirtualBox, VMware, atau WSL

# 5. Buat alias untuk perintah yang sering digunakan
alias ll='ls -la'

# 6. Gunakan tab completion
# Tekan Tab satu kali = complete
# Tekan Tab dua kali = tampilkan opsi

# 7. Gunakan Ctrl+R untuk history search
# Ctrl+R lalu ketik untuk mencari

# 8. Pelajari pipe dan redirection
command1 | command2 | command3

# 9. Gunakan tmux untuk multi-window
tmux new -s mysession

# 10. Baca log saat ada masalah
journalctl -xe
dmesg | tail -20
```
