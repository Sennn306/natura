# Dokumen Teknis Modul 1 – Lingkungan Pengembangan, Git, dan Lalu Lintas HTTP

Nama/NIM : Adrenalin Syahrobby / 105224030
Repositori : https://github.com/Sennn306/natura

## 1. Lingkungan Pengembangan

Tabel versi sistem operasi, Node.js, npm, Git, dan Visual Studio Code.

| Perangkat / Lingkungan | Versi / Spesifikasi |
| :--------------------- | :------------------ |
| **Sistem Operasi**     | Windows 11 (x64)    |
| **Node.js**            | v24.18.0            |
| **npm**                | 11.16.0             |
| **Git**                | 2.53.0.windows.1    |
| **Visual Studio Code** | 1.136.1             |

## 2. Alur Kerja Git

- Keluaran git log --oneline --graph

* d943a03 (HEAD -> main, origin/main, origin/HEAD) Merge pull request #1 from Sennn306/docs/readme-lengkap
  |\  
  | \* 6dbc7ac (origin/docs/readme-lengkap) docs: lengkapi teknologi
  |/
* d96f3a1 merge: menyelesaikan konflik
  |\  
  | \* 79e462a docs: perjelas desk
* | c520b75 docs: ubah desk 2
  |/
* dcc819d feat: perubahan judul
* 2272138 docs: mengubah isi README
* 7c7377f Initial commit from Create Next App

- Tautan pull request yang telah digabungkan
  https://github.com/Sennn306/natura/pull/1

- Konflik yang terjadi, cara penyelesaian, dan alasan pemilihan isi akhir
  - Konflik yang Terjadi: Konflik terjadi pada berkas README.md saat menggabungkan branch latihan/konflik ke branch main. Penyebabnya adalah modifikasi pada baris kalimat deskripsi produk yang sama oleh commit c520b75 (di branch main) dan commit 79e462a (di branch latihan/konflik).
  - Cara Penyelesaian: Berkas README.md dibuka menggunakan Visual Studio Code. Penanda konflik (<<<<<<< HEAD, =======, dan >>>>>>>) dihapus secara manual, lalu baris deskripsi dirapikan sehingga menjadi deskripsi produk Natura yang utuh. Setelah itu, berkas di-stage dengan git add README.md dan di-commit menggunakan pesan merge: menyelesaikan konflik.
  - Alasan Pemilihan Isi Akhir: Isi akhir dipilih untuk mempertahankan informasi produk kecantikan berbahan alami Natura agar tetap jelas, edukatif, dan mencakup perbaikan deskripsi dari kedua branch secara harmonis.

## 3. Pengamatan Lalu Lintas HTTP

- Lembar kerja pengamatan (Tabel 9) beserta tangkapan layar DevTools

| No  | URL                                                                                 | Metode |      Kode Status      | Content-Type             | Header Lain yang Diamati                                                           |
| :-: | :---------------------------------------------------------------------------------- | :----: | :-------------------: | :----------------------- | :--------------------------------------------------------------------------------- |
|  1  | http://localhost:3000/                                                              |  GET   |        200 OK         | text/html; charset=utf-8 | Remote Address: [::1]:3000, Referrer Policy: strict-origin-when-cross-origin       |
|  2  | http://localhost:3000/halaman-tidak-ada                                             |  GET   |     404 Not Found     | text/html; charset=utf-8 | Remote Address: [::1]:3000, Referrer Policy: strict-origin-when-cross-origin       |
|  3  | http://localhost:3000/_next/static/chunks/%5Broot-of-the-server%5D\_\_0cbk-n2._.css |  GET   |        200 OK         | text/css; charset=utf-8  | Remote Address: [::1]:3000, Referrer Policy: strict-origin-when-cross-origin       |
|  4  | http://github.com (curl)                                                            |  HEAD  | 301 Moved Permanently | Tidak ada body           | Content-Length: 0, Location: https://github.com/                                   |
|  5  | https://developer.mozilla.org/en-US/ (dengan cache)                                 |  GET   |   304 Not Modified    |                          | Remote Address: 146.75.45.91:443, Referrer Policy: strict-origin-when-cross-origin |

![Gambar1](./Cuplikan%20layar%202026-10-02%20031724.png)
![Gambar2](./Cuplikan%20layar%202026-10-02%20031813.png)
![Gambar3](./Cuplikan%20layar%202026-10-02%20031845.png)
![Gambar4](./Cuplikan%20layar%202026-10-02%20031927.png)
![Gambar5](./Cuplikan%20layar%202026-10-02%20032000.png)

- Keluaran curl -I dan curl -v
  - curl.exe -I http://localhost:3000
    HTTP/1.1 200 OK
    Content-Type: text/html; charset=utf-8
    Content-Length: 2845
    Date: Fri, 02 Oct 2026 03:20:00 GMT
    Connection: keep-alive

  - curl.exe -I http://github.com
    HTTP/1.1 301 Moved Permanently
    Content-Length: 0
    Location: https://github.com/

  - curl.exe -v https://example.com
    - Trying 93.184.215.14:443...
    - Connected to example.com (93.184.215.14) port 443
      > GET / HTTP/1.1
      > Host: example.com
      > User-Agent: curl/8.12.1
      > Accept: _/_
      >
      > < HTTP/1.1 200 OK
      > < Content-Type: text/html; charset=utf-8
      > < Content-Length: 1256
      > < Date: Fri, 02 Oct 2026 03:20:00 GMT

- Analisis: perbedaan status dan ukuran antara pemuatan dengan dan tanpa cache, alasan metode curl -I adalah HEAD, dan alasan http://github.com dialihkan
  - Perbedaan Status dan Ukuran Pemuatan Dengan dan Tanpa Cache: Tanpa cache (Disable Cache aktif), peramban mengunduh seluruh berkas dari server (200 OK) dengan ukuran data penuh. Dengan cache, peramban mengirim header validasi dan menerima status 304 Not Modified dari server tanpa transfer body data (0 B), atau langsung mengambilnya dari cache memori/disk peramban.
  - Alasan Metode curl -I Adalah HEAD: Opsi -I pada curl secara khusus hanya meminta header respons dari server tanpa mengunduh isi body dokumen. Menurut spesifikasi HTTP, metode standar yang digunakan untuk meminta header saja tanpa body adalah HEAD.
  - Alasan http://github.com Dialihkan: Dialihkan dengan kode status 301 Moved Permanently ke https://github.com/ untuk menerapkan enkripsi HTTPS dan menjamin keamanan komunikasi data pengguna secara permanen.

## 4. Kendala dan Penyelesaian

1. Kendala: Menentukan lokasi peletakan berkas .env.local dan memastikan berkas rahasia tersebut tidak terlacak oleh Git.
   Penyelesaian: Berkas .env.local dibuat di folder akar (root directory). Dipastikan bahwa pola env\* pada berkas .gitignore membuat berkas tersebut tidak muncul saat mengeksekusi git status.
2. Kendala: Penanganan konflik merge pada README.md saat penggabungan dua branch.
   Penyelesaian: Mengonfigurasi core.editor ke Visual Studio Code (code --wait) sehingga konflik baris kode dapat diperiksa dan diselesaikan secara presisi sebelum di-commit kembali.

## 5. Catatan Pemanfaatan AI

Alat: Gemini.
Perintah utama: menyusun draf deskripsi README.md, membantu panduan alur kerja Git (branching, PR, dan sinkronisasi), serta menstrukturkan Dokumen Teknis Modul 1.
Bagian yang digunakan: teks deskripsi produk Natura, pembahasan analisis lalu lintas HTTP, dan penataan berkas docs/praktikum/modul-01.md.
Cara memverifikasi: setiap perintah diawasi dan dieksekusi manual di terminal, serta keluarannya diverifikasi menggunakan git status, git log --oneline --graph, dan pemeriksaan panel Network pada DevTools peramban.
