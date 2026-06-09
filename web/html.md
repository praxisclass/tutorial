# Tutorial HTML dari A–Z

> Panduan lengkap belajar HTML dari nol hingga mahir — cocok untuk pemula maupun yang ingin memperkuat fondasi.

---

## Daftar Isi

1. [Apa itu HTML?](#1-apa-itu-html)
2. [Struktur Dasar Dokumen HTML](#2-struktur-dasar-dokumen-html)
3. [Tag, Elemen, dan Atribut](#3-tag-elemen-dan-atribut)
4. [Heading dan Paragraf](#4-heading-dan-paragraf)
5. [Pemformatan Teks](#5-pemformatan-teks)
6. [Link (Hyperlink)](#6-link-hyperlink)
7. [Gambar (Image)](#7-gambar-image)
8. [List (Daftar)](#8-list-daftar)
9. [Tabel](#9-tabel)
10. [Form dan Input](#10-form-dan-input)
11. [Div dan Span](#11-div-dan-span)
12. [Semantic HTML5](#12-semantic-html5)
13. [Multimedia (Audio & Video)](#13-multimedia-audio--video)
14. [Meta Tag dan SEO Dasar](#14-meta-tag-dan-seo-dasar)
15. [HTML Entities](#15-html-entities)
16. [Komentar HTML](#16-komentar-html)
17. [Iframe](#17-iframe)
18. [HTML5 API Dasar](#18-html5-api-dasar)
19. [Aksesibilitas (Accessibility)](#19-aksesibilitas-accessibility)
20. [Best Practices & Tips](#20-best-practices--tips)

---

## 1. Apa itu HTML?

**HTML** (HyperText Markup Language) adalah bahasa markup standar untuk membuat halaman web. HTML **bukan** bahasa pemrograman — ia adalah bahasa yang mendefinisikan *struktur* dan *konten* sebuah halaman web.

- **HyperText** = teks yang bisa saling terhubung lewat tautan
- **Markup** = menandai/memberi label pada konten
- **Language** = bahasa yang dipahami oleh browser

Browser (Chrome, Firefox, Edge, dll.) membaca file HTML dan **merender** hasilnya menjadi halaman visual yang kita lihat.

**Ekstensi file:** `.html` atau `.htm`

---

## 2. Struktur Dasar Dokumen HTML

Setiap dokumen HTML yang valid harus memiliki struktur berikut:

```html
<!DOCTYPE html>
<html lang="id">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Judul Halaman</title>
  </head>
  <body>
    <!-- Konten halaman ada di sini -->
    <h1>Halo, Dunia!</h1>
  </body>
</html>
```

### Penjelasan Setiap Bagian

| Bagian | Fungsi |
|---|---|
| `<!DOCTYPE html>` | Deklarasi bahwa dokumen ini adalah HTML5 |
| `<html lang="id">` | Elemen akar; `lang="id"` memberitahu browser bahasa konten adalah Indonesia |
| `<head>` | Berisi informasi meta (tidak tampil di halaman) |
| `<meta charset="UTF-8">` | Mendukung karakter khusus termasuk huruf Indonesia |
| `<meta name="viewport" ...>` | Membuat halaman responsif di perangkat mobile |
| `<title>` | Judul yang muncul di tab browser |
| `<body>` | Semua konten yang tampil di halaman |

---

## 3. Tag, Elemen, dan Atribut

### Tag

Tag adalah penanda yang ditulis dalam tanda kurung siku `< >`.

```html
<p>Ini adalah paragraf.</p>
```

- **Tag pembuka:** `<p>`
- **Tag penutup:** `</p>`
- **Konten:** teks di antara keduanya

### Tag Self-Closing

Beberapa tag tidak memiliki konten dan tidak perlu ditutup:

```html
<br />      <!-- pindah baris -->
<hr />      <!-- garis horizontal -->
<img />     <!-- gambar -->
<input />   <!-- kolom input -->
```

### Atribut

Atribut memberikan informasi tambahan pada sebuah tag:

```html
<a href="https://google.com" target="_blank">Kunjungi Google</a>
```

- `href` → atribut yang menentukan tujuan link
- `target="_blank"` → membuka link di tab baru
- Format: `nama-atribut="nilai"`

### Atribut Global Umum

| Atribut | Fungsi |
|---|---|
| `id` | Pengenal unik untuk elemen |
| `class` | Untuk pemberian gaya CSS |
| `style` | CSS inline langsung di elemen |
| `title` | Tooltip saat hover |
| `hidden` | Menyembunyikan elemen |
| `data-*` | Atribut data kustom |

---

## 4. Heading dan Paragraf

### Heading (Judul)

HTML memiliki 6 level heading, dari yang terbesar hingga terkecil:

```html
<h1>Heading 1 — Judul Utama</h1>
<h2>Heading 2 — Sub Judul</h2>
<h3>Heading 3</h3>
<h4>Heading 4</h4>
<h5>Heading 5</h5>
<h6>Heading 6 — Terkecil</h6>
```

> **Tips SEO:** Gunakan hanya **satu `<h1>`** per halaman. Heading juga penting untuk aksesibilitas.

### Paragraf

```html
<p>Ini adalah sebuah paragraf. Browser akan menambahkan
spasi otomatis di atas dan bawahnya.</p>

<p>Ini paragraf kedua.</p>
```

### Pindah Baris dan Garis Pemisah

```html
<p>Baris pertama.<br />Baris kedua dalam paragraf yang sama.</p>

<hr /> <!-- Garis horizontal pemisah -->
```

---

## 5. Pemformatan Teks

```html
<b>Tebal (Bold)</b>
<strong>Tebal penting (secara semantik)</strong>

<i>Miring (Italic)</i>
<em>Miring penekanan (secara semantik)</em>

<u>Garis bawah</u>
<s>Teks dicoret</s>

<mark>Teks disorot/highlight</mark>
<small>Teks kecil</small>
<big>Teks besar</big>

<sub>Subscript</sub>  → H<sub>2</sub>O
<sup>Superscript</sup> → x<sup>2</sup>

<code>Kode inline</code>
<pre>
  Teks preformatted
  spasi   dipertahankan
</pre>

<blockquote>
  Kutipan panjang dari sumber lain.
</blockquote>

<abbr title="HyperText Markup Language">HTML</abbr>
```

---

## 6. Link (Hyperlink)

### Link Dasar

```html
<a href="https://www.google.com">Kunjungi Google</a>
```

### Variasi Link

```html
<!-- Buka di tab baru -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
  Buka di Tab Baru
</a>

<!-- Link ke halaman lain dalam situs -->
<a href="/tentang.html">Tentang Kami</a>

<!-- Link ke bagian dalam halaman yang sama (anchor) -->
<a href="#kontak">Ke Bagian Kontak</a>
<section id="kontak">...</section>

<!-- Link email -->
<a href="mailto:halo@example.com">Kirim Email</a>

<!-- Link telepon -->
<a href="tel:+6281234567890">Hubungi Kami</a>

<!-- Link download -->
<a href="/file/dokumen.pdf" download>Unduh PDF</a>
```

> **Tips keamanan:** Selalu tambahkan `rel="noopener noreferrer"` pada link `target="_blank"`.

---

## 7. Gambar (Image)

```html
<img
  src="foto.jpg"
  alt="Deskripsi gambar"
  width="600"
  height="400"
/>
```

### Atribut Penting

| Atribut | Wajib? | Fungsi |
|---|---|---|
| `src` | Ya | Path atau URL gambar |
| `alt` | Ya | Teks alternatif (aksesibilitas & SEO) |
| `width` | Opsional | Lebar gambar (piksel atau %) |
| `height` | Opsional | Tinggi gambar |
| `loading` | Opsional | `"lazy"` untuk lazy loading |

### Gambar dari URL Eksternal

```html
<img
  src="https://via.placeholder.com/400x300"
  alt="Gambar placeholder"
  loading="lazy"
/>
```

### Figure dan Figcaption

```html
<figure>
  <img src="pemandangan.jpg" alt="Pemandangan pegunungan" />
  <figcaption>Pemandangan Gunung Merapi saat pagi hari.</figcaption>
</figure>
```

---

## 8. List (Daftar)

### Unordered List (Tidak Berurutan)

```html
<ul>
  <li>Apel</li>
  <li>Mangga</li>
  <li>Jeruk</li>
</ul>
```

### Ordered List (Berurutan)

```html
<ol>
  <li>Cuci tangan</li>
  <li>Siapkan bahan</li>
  <li>Masak</li>
  <li>Hidangkan</li>
</ol>
```

### Atribut Ordered List

```html
<!-- Mulai dari angka tertentu -->
<ol start="5">
  <li>Item kelima</li>
  <li>Item keenam</li>
</ol>

<!-- Urutan terbalik -->
<ol reversed>
  <li>Ketiga</li>
  <li>Kedua</li>
  <li>Pertama</li>
</ol>

<!-- Tipe penomoran -->
<ol type="A">  <!-- A, B, C -->
<ol type="a">  <!-- a, b, c -->
<ol type="I">  <!-- I, II, III -->
<ol type="i">  <!-- i, ii, iii -->
```

### Description List (Daftar Definisi)

```html
<dl>
  <dt>HTML</dt>
  <dd>Bahasa markup untuk membuat struktur halaman web.</dd>

  <dt>CSS</dt>
  <dd>Bahasa untuk mengatur tampilan halaman web.</dd>
</dl>
```

### List Bersarang (Nested)

```html
<ul>
  <li>Buah
    <ul>
      <li>Apel</li>
      <li>Mangga</li>
    </ul>
  </li>
  <li>Sayuran
    <ul>
      <li>Bayam</li>
      <li>Kangkung</li>
    </ul>
  </li>
</ul>
```

---

## 9. Tabel

### Struktur Dasar Tabel

```html
<table border="1">
  <thead>
    <tr>
      <th>Nama</th>
      <th>Umur</th>
      <th>Kota</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Andi</td>
      <td>25</td>
      <td>Jakarta</td>
    </tr>
    <tr>
      <td>Budi</td>
      <td>30</td>
      <td>Surabaya</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="3">Total: 2 orang</td>
    </tr>
  </tfoot>
</table>
```

### Elemen Tabel

| Tag | Fungsi |
|---|---|
| `<table>` | Wadah utama tabel |
| `<thead>` | Bagian kepala tabel |
| `<tbody>` | Bagian isi tabel |
| `<tfoot>` | Bagian kaki tabel |
| `<tr>` | Baris (table row) |
| `<th>` | Sel header (tebal & rata tengah) |
| `<td>` | Sel data biasa |

### Menggabungkan Sel

```html
<!-- Menggabungkan kolom (colspan) -->
<td colspan="2">Menempati 2 kolom</td>

<!-- Menggabungkan baris (rowspan) -->
<td rowspan="3">Menempati 3 baris</td>
```

---

## 10. Form dan Input

Form digunakan untuk mengumpulkan data dari pengguna.

### Struktur Form

```html
<form action="/proses.php" method="POST">

  <!-- Input teks -->
  <label for="nama">Nama:</label>
  <input type="text" id="nama" name="nama" placeholder="Masukkan nama" required />

  <!-- Input email -->
  <label for="email">Email:</label>
  <input type="email" id="email" name="email" required />

  <!-- Input password -->
  <label for="sandi">Kata Sandi:</label>
  <input type="password" id="sandi" name="sandi" minlength="8" />

  <!-- Input angka -->
  <label for="umur">Umur:</label>
  <input type="number" id="umur" name="umur" min="1" max="120" />

  <!-- Textarea -->
  <label for="pesan">Pesan:</label>
  <textarea id="pesan" name="pesan" rows="5" cols="40"></textarea>

  <!-- Select (dropdown) -->
  <label for="kota">Kota:</label>
  <select id="kota" name="kota">
    <option value="">-- Pilih Kota --</option>
    <option value="jakarta">Jakarta</option>
    <option value="yogyakarta" selected>Yogyakarta</option>
    <option value="surabaya">Surabaya</option>
  </select>

  <!-- Radio button -->
  <p>Jenis Kelamin:</p>
  <input type="radio" id="pria" name="jk" value="pria" />
  <label for="pria">Pria</label>
  <input type="radio" id="wanita" name="jk" value="wanita" />
  <label for="wanita">Wanita</label>

  <!-- Checkbox -->
  <input type="checkbox" id="setuju" name="setuju" value="ya" />
  <label for="setuju">Saya setuju dengan syarat dan ketentuan</label>

  <!-- Input file -->
  <label for="foto">Upload Foto:</label>
  <input type="file" id="foto" name="foto" accept="image/*" />

  <!-- Tombol submit -->
  <button type="submit">Kirim</button>
  <button type="reset">Reset</button>

</form>
```

### Semua Tipe Input

```html
<input type="text">        <!-- teks biasa -->
<input type="email">       <!-- validasi format email -->
<input type="password">    <!-- teks tersembunyi -->
<input type="number">      <!-- angka -->
<input type="tel">         <!-- nomor telepon -->
<input type="url">         <!-- URL -->
<input type="date">        <!-- tanggal -->
<input type="time">        <!-- waktu -->
<input type="datetime-local"> <!-- tanggal & waktu -->
<input type="month">       <!-- bulan & tahun -->
<input type="week">        <!-- minggu & tahun -->
<input type="range">       <!-- slider angka -->
<input type="color">       <!-- pemilih warna -->
<input type="search">      <!-- kolom pencarian -->
<input type="checkbox">    <!-- kotak centang -->
<input type="radio">       <!-- pilihan tunggal -->
<input type="file">        <!-- upload file -->
<input type="hidden">      <!-- field tersembunyi -->
<input type="submit">      <!-- tombol kirim -->
<input type="reset">       <!-- tombol reset -->
<input type="button">      <!-- tombol biasa -->
<input type="image">       <!-- tombol gambar -->
```

### Fieldset dan Legend

```html
<fieldset>
  <legend>Informasi Pribadi</legend>
  <label for="fn">Nama Depan:</label>
  <input type="text" id="fn" name="firstName" />
  <label for="ln">Nama Belakang:</label>
  <input type="text" id="ln" name="lastName" />
</fieldset>
```

---

## 11. Div dan Span

### `<div>` — Block Container

`<div>` adalah elemen **blok** (memenuhi lebar penuh) yang digunakan sebagai wadah/kontainer:

```html
<div class="kartu">
  <h2>Judul Kartu</h2>
  <p>Isi konten kartu.</p>
</div>
```

### `<span>` — Inline Container

`<span>` adalah elemen **inline** (hanya selebar kontennya) untuk menandai bagian teks:

```html
<p>Harga produk ini adalah <span style="color: red;">Rp 50.000</span> saja.</p>
```

### Perbedaan Block vs Inline

| Block | Inline |
|---|---|
| Mulai di baris baru | Tetap dalam alur teks |
| Mengambil lebar penuh | Hanya selebar konten |
| Contoh: `div`, `p`, `h1–h6`, `ul`, `table` | Contoh: `span`, `a`, `strong`, `em`, `img` |

---

## 12. Semantic HTML5

HTML5 memperkenalkan tag semantik — tag yang **mendeskripsikan makna** kontennya, bukan hanya tampilannya.

```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <title>Contoh Semantik</title>
</head>
<body>

  <header>
    <nav>
      <ul>
        <li><a href="/">Beranda</a></li>
        <li><a href="/blog">Blog</a></li>
        <li><a href="/kontak">Kontak</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <article>
      <header>
        <h1>Judul Artikel</h1>
        <time datetime="2026-06-09">9 Juni 2026</time>
      </header>

      <section>
        <h2>Bagian Pertama</h2>
        <p>Isi konten bagian pertama...</p>
      </section>

      <section>
        <h2>Bagian Kedua</h2>
        <p>Isi konten bagian kedua...</p>
      </section>

      <footer>
        <p>Ditulis oleh: <address>nama@email.com</address></p>
      </footer>
    </article>

    <aside>
      <h3>Artikel Terkait</h3>
      <ul>
        <li><a href="#">Artikel 1</a></li>
        <li><a href="#">Artikel 2</a></li>
      </ul>
    </aside>
  </main>

  <footer>
    <p>&copy; 2026 Nama Situs. Semua hak dilindungi.</p>
  </footer>

</body>
</html>
```

### Daftar Tag Semantik HTML5

| Tag | Fungsi |
|---|---|
| `<header>` | Kepala halaman atau seksi |
| `<nav>` | Navigasi/menu |
| `<main>` | Konten utama halaman (hanya satu per halaman) |
| `<article>` | Konten mandiri (posting blog, berita) |
| `<section>` | Bagian/seksi dari konten |
| `<aside>` | Konten sampingan (sidebar) |
| `<footer>` | Kaki halaman atau seksi |
| `<figure>` | Gambar/ilustrasi dengan caption |
| `<figcaption>` | Caption untuk `<figure>` |
| `<time>` | Tanggal/waktu |
| `<address>` | Informasi kontak |
| `<details>` | Konten yang bisa dibuka/tutup |
| `<summary>` | Judul untuk `<details>` |
| `<mark>` | Teks yang disorot/relevan |

---

## 13. Multimedia (Audio & Video)

### Audio

```html
<audio controls>
  <source src="lagu.mp3" type="audio/mpeg" />
  <source src="lagu.ogg" type="audio/ogg" />
  Browser Anda tidak mendukung tag audio.
</audio>
```

**Atribut audio:**
- `controls` — tampilkan kontrol play/pause/volume
- `autoplay` — putar otomatis (tidak disarankan)
- `loop` — ulangi terus
- `muted` — mulai dalam kondisi diam
- `preload="auto|metadata|none"` — strategi pra-muat

### Video

```html
<video controls width="640" height="360" poster="thumbnail.jpg">
  <source src="video.mp4" type="video/mp4" />
  <source src="video.webm" type="video/webm" />
  Browser Anda tidak mendukung tag video.
</video>
```

**Atribut tambahan video:**
- `poster` — gambar yang tampil sebelum video diputar
- `width` dan `height` — ukuran pemutar video

---

## 14. Meta Tag dan SEO Dasar

Meta tag ada di dalam `<head>` dan tidak terlihat oleh pengguna, namun penting untuk browser, mesin pencari, dan media sosial.

```html
<head>
  <!-- Charset -->
  <meta charset="UTF-8" />

  <!-- Viewport (responsif) -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <!-- Deskripsi untuk mesin pencari -->
  <meta name="description" content="Tutorial HTML lengkap dari A sampai Z untuk pemula." />

  <!-- Kata kunci (sudah kurang relevan untuk Google) -->
  <meta name="keywords" content="tutorial html, belajar html, html dasar" />

  <!-- Penulis -->
  <meta name="author" content="Nama Anda" />

  <!-- Refresh otomatis setiap 30 detik -->
  <meta http-equiv="refresh" content="30" />

  <!-- Open Graph (tampilan saat dibagikan di media sosial) -->
  <meta property="og:title" content="Tutorial HTML A-Z" />
  <meta property="og:description" content="Pelajari HTML dari dasar hingga mahir." />
  <meta property="og:image" content="https://example.com/thumbnail.jpg" />
  <meta property="og:url" content="https://example.com/tutorial-html" />
  <meta property="og:type" content="article" />

  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:title" content="Tutorial HTML A-Z" />
  <meta name="twitter:description" content="Pelajari HTML dari dasar hingga mahir." />

  <!-- Favicon -->
  <link rel="icon" href="/favicon.ico" type="image/x-icon" />
  <link rel="apple-touch-icon" href="/apple-touch-icon.png" />

  <!-- Canonical URL (hindari konten duplikat) -->
  <link rel="canonical" href="https://example.com/tutorial-html" />

  <!-- CSS eksternal -->
  <link rel="stylesheet" href="style.css" />

  <title>Tutorial HTML A-Z | Nama Situs</title>
</head>
```

---

## 15. HTML Entities

HTML Entities digunakan untuk menampilkan karakter khusus yang memiliki makna dalam HTML, atau karakter yang tidak ada di keyboard.

| Karakter | Entity Name | Entity Number | Keterangan |
|---|---|---|---|
| `<` | `&lt;` | `&#60;` | Less than |
| `>` | `&gt;` | `&#62;` | Greater than |
| `&` | `&amp;` | `&#38;` | Ampersand |
| `"` | `&quot;` | `&#34;` | Tanda kutip |
| `'` | `&apos;` | `&#39;` | Apostrof |
| ` ` (spasi) | `&nbsp;` | `&#160;` | Non-breaking space |
| `©` | `&copy;` | `&#169;` | Copyright |
| `®` | `&reg;` | `&#174;` | Registered trademark |
| `™` | `&trade;` | `&#8482;` | Trademark |
| `€` | `&euro;` | `&#8364;` | Euro |
| `£` | `&pound;` | `&#163;` | Pound |
| `¥` | `&yen;` | `&#165;` | Yen |
| `→` | `&rarr;` | `&#8594;` | Panah kanan |
| `←` | `&larr;` | `&#8592;` | Panah kiri |
| `★` | `&starf;` | `&#9733;` | Bintang penuh |
| `☆` | `&star;` | `&#9734;` | Bintang kosong |

### Contoh Penggunaan

```html
<p>Harga: &euro;10 atau &pound;8</p>
<p>Copyright &copy; 2026 &mdash; Nama Situs</p>
<p>Kode: <code>&lt;p&gt;Paragraf&lt;/p&gt;</code></p>
<p>Kolom&nbsp;&nbsp;&nbsp;dengan&nbsp;spasi&nbsp;khusus</p>
```

---

## 16. Komentar HTML

Komentar tidak ditampilkan di browser namun terlihat di source code:

```html
<!-- Ini adalah komentar satu baris -->

<!--
  Ini komentar
  multi baris
-->

<!-- TODO: Tambahkan form kontak di sini -->

<!-- ==================== HEADER ==================== -->
<header>
  ...
</header>
<!-- ==================== /HEADER ==================== -->
```

> **Perhatian:** Komentar HTML tetap terlihat oleh siapa saja yang melihat source code halaman. Jangan menyimpan informasi sensitif (password, catatan internal) di komentar HTML.

---

## 17. Iframe

`<iframe>` digunakan untuk menyematkan halaman web lain atau konten eksternal ke dalam halaman:

```html
<!-- Embed halaman web lain -->
<iframe
  src="https://example.com"
  width="800"
  height="400"
  title="Halaman example.com"
></iframe>

<!-- Embed Google Maps -->
<iframe
  src="https://www.google.com/maps/embed?pb=!1m18!..."
  width="600"
  height="450"
  style="border:0"
  allowfullscreen
  loading="lazy"
  title="Peta Lokasi"
></iframe>

<!-- Embed YouTube -->
<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/VIDEO_ID"
  title="Judul Video YouTube"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen
></iframe>
```

> **Tips keamanan:** Tambahkan atribut `sandbox` untuk membatasi kemampuan iframe dari sumber yang tidak dipercaya.

---

## 18. HTML5 API Dasar

HTML5 membawa berbagai API bawaan yang bisa diakses melalui JavaScript.

### Geolocation API

```html
<button onclick="getLocation()">Dapatkan Lokasi Saya</button>
<p id="lokasi"></p>

<script>
  function getLocation() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(function(position) {
        document.getElementById("lokasi").textContent =
          "Lat: " + position.coords.latitude +
          ", Long: " + position.coords.longitude;
      });
    }
  }
</script>
```

### Canvas API

```html
<canvas id="myCanvas" width="400" height="200" style="border:1px solid #000;"></canvas>

<script>
  const canvas = document.getElementById("myCanvas");
  const ctx = canvas.getContext("2d");

  ctx.fillStyle = "#4CAF50";
  ctx.fillRect(50, 50, 150, 100);

  ctx.font = "20px Arial";
  ctx.fillStyle = "white";
  ctx.fillText("Halo Canvas!", 70, 105);
</script>
```

### Local Storage

```html
<script>
  // Simpan data
  localStorage.setItem("nama", "Budi");

  // Ambil data
  const nama = localStorage.getItem("nama");

  // Hapus data
  localStorage.removeItem("nama");

  // Hapus semua
  localStorage.clear();
</script>
```

### Details & Summary (Accordion tanpa JavaScript)

```html
<details>
  <summary>Klik untuk melihat jawaban</summary>
  <p>Ini adalah konten tersembunyi yang muncul saat diklik!</p>
</details>
```

---

## 19. Aksesibilitas (Accessibility)

Aksesibilitas memastikan halaman web dapat digunakan oleh semua orang, termasuk pengguna dengan disabilitas.

### Prinsip Utama

```html
<!-- 1. Selalu gunakan atribut alt pada gambar -->
<img src="logo.png" alt="Logo Perusahaan ABC" />
<!-- Untuk gambar dekoratif, gunakan alt kosong -->
<img src="hiasan.png" alt="" />

<!-- 2. Gunakan label untuk setiap input form -->
<label for="email">Alamat Email:</label>
<input type="email" id="email" name="email" />

<!-- 3. Gunakan heading yang terstruktur (h1 → h2 → h3) -->
<h1>Judul Utama Halaman</h1>
  <h2>Sub-bagian</h2>
    <h3>Sub-sub-bagian</h3>

<!-- 4. Gunakan atribut role untuk elemen kustom -->
<div role="button" tabindex="0" onclick="klik()">Tombol Kustom</div>

<!-- 5. ARIA labels untuk elemen tanpa teks -->
<button aria-label="Tutup dialog">✕</button>

<!-- 6. Skip navigation link -->
<a href="#konten-utama" class="skip-link">Langsung ke konten</a>
<main id="konten-utama">...</main>

<!-- 7. Lang attribute pada elemen dengan bahasa berbeda -->
<p>Slogan kami: <span lang="en">"Excellence in Everything"</span></p>
```

### Atribut ARIA Penting

| Atribut | Fungsi |
|---|---|
| `aria-label` | Label teks untuk elemen |
| `aria-labelledby` | Referensi ke elemen yang menjadi label |
| `aria-describedby` | Referensi ke elemen deskripsi |
| `aria-hidden="true"` | Sembunyikan dari screen reader |
| `aria-expanded` | Status buka/tutup (accordion, dropdown) |
| `aria-required` | Tandai field yang wajib diisi |
| `aria-invalid` | Tandai input yang tidak valid |
| `role` | Tentukan peran semantik elemen |

---

## 20. Best Practices & Tips

### Struktur dan Kebersihan Kode

```html
<!-- ✅ BAIK: Indentasi konsisten, tag semantik -->
<article>
  <h2>Judul Artikel</h2>
  <p>Isi artikel...</p>
</article>

<!-- ❌ BURUK: Tidak ada indentasi, semua pakai div -->
<div><div>Judul Artikel</div><div>Isi artikel...</div></div>
```

### Validasi HTML

Selalu validasi HTML Anda di: **https://validator.w3.org**

### Checklist Sebelum Publish

- [ ] `<!DOCTYPE html>` ada di baris pertama
- [ ] Atribut `lang` ada di `<html>`
- [ ] `<meta charset="UTF-8">` ada di `<head>`
- [ ] `<meta name="viewport">` ada untuk responsivitas
- [ ] `<title>` unik dan deskriptif
- [ ] Semua gambar memiliki atribut `alt`
- [ ] Semua input form memiliki `<label>` terhubung
- [ ] Heading terstruktur dengan benar (h1 → h2 → dst)
- [ ] Link memiliki teks yang deskriptif (bukan "klik di sini")
- [ ] HTML sudah divalidasi

### Tips Performa

```html
<!-- Lazy load gambar di luar viewport -->
<img src="foto.jpg" alt="..." loading="lazy" />

<!-- Preload resource penting -->
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin />

<!-- Script di akhir body atau dengan defer -->
<script src="app.js" defer></script>

<!-- Async untuk script independen -->
<script src="analytics.js" async></script>
```

### Urutan Belajar yang Disarankan

```
HTML Dasar → CSS Dasar → JavaScript Dasar
     ↓              ↓              ↓
  Semantik    Flexbox/Grid    DOM Manipulation
     ↓              ↓              ↓
 Aksesibilitas  Responsive     Fetch API
     ↓              ↓              ↓
  Framework    Animasi CSS     Node.js / Backend
```

---

## Referensi Lanjutan

| Sumber | URL |
|---|---|
| MDN Web Docs (terlengkap) | https://developer.mozilla.org |
| W3Schools (mudah dipahami) | https://www.w3schools.com |
| HTML Validator | https://validator.w3.org |
| Can I Use (kompatibilitas browser) | https://caniuse.com |
| HTML Living Standard | https://html.spec.whatwg.org |

---

*Tutorial ini mencakup HTML dari dasar hingga mahir. Praktikkan setiap konsep dengan membuat proyek kecil — itu adalah cara terbaik untuk belajar!*
