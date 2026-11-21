# Panduan Online — Daarul Fikr

Website ini statis (HTML + JS) dan datanya disimpan di Firebase. Semua gratis untuk skala desa/TPQ (paket Spark, tanpa kartu kredit).

## Berkas yang di-upload
index.html, admin.html, ortu.html, db.js, konten.js, konten-default.js, firebase-config.js, site-config.js, logo.png, favicon.png (setup.html boleh tidak diupload)

> Cara termudah: buka **setup.html** di browser. Halaman itu memandu langkah A dan membuatkan `firebase-config.js` serta Rules otomatis.

## A. Siapkan Firebase (sekali saja, ±15 menit)
1. Buka https://console.firebase.google.com, login Google, **Add project** (nama bebas, Analytics boleh dimatikan).
2. **Build → Firestore Database → Create database**. Lokasi: `asia-southeast2 (Jakarta)`. Mode: production.
3. **Build → Authentication → Get started → Email/Password → Enable**. Tab **Users → Add user**: isi email dan password kuat untuk admin.
4. Buka berkas `firestore.rules`, ganti `GANTI-DENGAN-EMAIL-ADMIN@gmail.com` dengan email admin tadi. Salin seluruh isinya ke Firestore → tab **Rules** → **Publish**.
5. **Project settings (ikon roda gigi) → Your apps → ikon Web `</>`** → daftarkan app → salin `apiKey`, `authDomain`, `projectId`, `appId` ke `firebase-config.js`.
   (apiKey ini memang publik; keamanan dijaga oleh Rules di langkah 4.)

## B. Online-kan website (pilih salah satu)
**Opsi 1 — Firebase Hosting (satu tempat dengan database)**
1. Pasang Node.js (nodejs.org), lalu di terminal: `npm install -g firebase-tools` dan `firebase login`.
2. Taruh semua berkas di satu folder, buka terminal di folder itu: `firebase init hosting` → pilih project tadi, folder publik `.` , single-page app: **No**. Jangan timpa index.html jika ditanya.
3. `firebase deploy`. Alamatnya `https://NAMA-PROJECT.web.app`.

**Opsi 2 — Netlify / Cloudflare Pages (tanpa terminal)**
Seret folder berisi semua berkas ke halaman deploy Netlify (netlify.com, menu "Add new site → Deploy manually") atau buat proyek di Cloudflare Pages. Anda mendapat alamat gratis. Tampilan menu dapat berubah sewaktu-waktu.

**Opsi 3 — Hosting biasa (cPanel/domain .id)**: upload semua berkas ke folder `public_html`.

## C. Setelah online
1. Firebase → Authentication → **Settings → Authorized domains** → **Add domain**: alamat website Anda (tanpa https://).
2. Buka `https://alamat-anda/admin.html`, login dengan email admin. Di **Pengaturan Situs** isi WhatsApp, email, biaya. Di tab **Program** dan **Jadwal** klik **Muat data contoh**, lalu edit.
3. Uji: daftar sebagai orang tua dari HP → cek muncul di admin → klik Terima → cek status memakai nomor pendaftaran.
4. Rutin klik **Backup data** di admin (misalnya tiap bulan) dan simpan file-nya.

## Batas gratis (Spark)
Firestore: 50.000 baca/hari, 20.000 tulis/hari, 1 GiB. Hosting: 10 GB simpan, 10 GB transfer/bulan. Lebih dari cukup untuk satu TPQ.
Firebase Storage sekarang berbayar, jadi galeri foto disimpan di database (foto dikecilkan otomatis).

## Keamanan
- Pakai password admin yang panjang dan jangan dibagikan. Hanya email di `firestore.rules` yang bisa mengakses data.
- Data santri tidak bisa dibaca publik; pengunjung hanya bisa mendaftar dan cek status dengan nomor.

## Jika ada masalah
| Gejala | Penyebab & solusi |
|---|---|
| "Firebase gagal dimuat" | Perangkat tidak punya internet atau memblokir gstatic.com. Muat ulang. |
| "Domain website belum didaftarkan" | Firebase → Authentication → Settings → Authorized domains → tambahkan domain. |
| "Akses ditolak" | Email di firestore.rules tidak sama dengan email login, atau Rules belum di-Publish. |
| Pendaftaran tidak masuk | Pastikan firebase-config.js sudah diisi dan diupload. Tanpa itu data hanya tersimpan di browser (mode demo). |
| Pengumuman/program/jadwal tidak tampil | Rules lama belum memuat izin baca publik. Tempel ulang isi firestore.rules. |
| Pop-up rapor/kartu/kwitansi tidak terbuka | Izinkan pop-up untuk situs ini di browser. |

## Fitur panel admin
Dashboard · Pendaftaran & Santri (terima/tolak, detail, rapor, kartu) · Absensi (massal) · Hafalan · Pembayaran (kwitansi) · Laporan Bulanan (kehadiran, setoran, status infaq, CSV/cetak) · Pengumuman · Program · Jadwal · Pengajar · Galeri · Pengaturan Situs · Backup/Restore.

## Menghubungkan website dan admin
Website (`index.html`) dan admin (`admin.html`) terhubung lewat database Firebase yang sama (isi `firebase-config.js`). Cara memastikan:
1. Login admin → **Pengaturan Situs → Tes koneksi**. Semua baris harus bertanda ✅.
2. Buka website di jendela penyamaran, daftar, lalu lihat lencana angka di menu **Pendaftaran & Santri** (diperbarui otomatis tiap 90 detik) dan notifikasi "Ada pendaftar baru".
3. Di Dashboard ada **Tautan pendaftaran** untuk disalin atau dibagikan lewat WhatsApp.
Selama `firebase-config.js` kosong, admin menampilkan banner "Mode demo", dan website di alamat publik menonaktifkan formulir (diganti tombol daftar via WhatsApp) agar tidak ada pendaftaran yang hilang.

## Login dan mengedit isi website
- Di website ada tombol **Login** (menu atas). Setelah login, Anda langsung masuk ke dashboard admin. Jika sudah login, tombolnya berubah menjadi **Dashboard Admin**.
- Menu admin **Isi Website** mengatur semua yang tampil di website:
  - **Teks & Pengaturan Situs**: logo, nama, judul utama, semua paragraf dan judul bagian, visi/misi, banner ajakan, formulir, kontak, peta, media sosial, footer.
  - **Program, Jadwal, Keunggulan, Cara Mendaftar, Tanya Jawab, Sorotan Beranda, Pengajar, Galeri, Pengumuman**: tiap daftar bisa ditambah, diubah, diurutkan (kolom Urutan), disembunyikan (Tampilkan = Tidak), dan dihapus. Klik **Muat data contoh** untuk mengisi dengan isi bawaan lalu edit.
- Ganti logo: unggah di Teks & Pengaturan Situs, atau ganti file `logo.png` di hosting.
- Setelah memperbarui Rules (`firestore.rules`) ke versi baru, klik Publish lagi agar daftar baru (keunggulan, cara mendaftar, tanya jawab, sorotan) bisa dibaca website.

## Logo
Logo sudah tertanam di dalam `index.html` dan `admin.html`, jadi selalu tampil walaupun `logo.png` tidak ikut di-upload. Untuk mengganti logo, unggah gambar baru di admin: **Teks & Pengaturan Situs → Identitas & logo**.

## Akun orang tua dan portal pemantauan anak (ortu.html)
**Alur untuk orang tua**
1. Buka `ortu.html` (atau menu **Orang Tua** di website) → **Daftar Akun** (nama, WhatsApp, email, password).
2. Dua cara menghubungkan anak:
   - **Daftarkan Anak**: anak baru otomatis tertaut ke akun. Pengurus tinggal memverifikasi dan menerima.
   - **Tautkan Anak**: untuk anak yang sudah terdaftar. Masukkan nomor pendaftaran dan tanggal lahir anak. Pengurus akan memeriksa dan menyetujui.
3. Setelah anak diterima, orang tua melihat kehadiran, hafalan dan nilai, pembayaran, status infaq bulan ini, pengumuman, serta bisa **mengajukan izin/sakit**, **mengubah kontak**, **mencetak rapor** dan **kartu santri**.
4. Lupa password: klik **Lupa password?** di halaman masuk (email pemulihan dikirim Firebase).

**Untuk admin**
- Menu **Akun Orang Tua** menampilkan permintaan penautan (dengan penanda *Cocok* bila nomor dan tanggal lahir sama dengan data santri), daftar akun, dan tombol menautkan atau melepas anak. Lencana angka muncul di menu bila ada permintaan baru.
- Izin/sakit dari orang tua langsung masuk ke tab **Absensi** dengan catatannya.
- Tombol **Login** di website memeriksa peran otomatis: admin masuk ke dashboard, orang tua masuk ke portal. Akun orang tua tidak bisa membuka dashboard admin.

**Keamanan** (di `firestore.rules`): orang tua hanya bisa membaca anak yang tertaut ke akunnya, hanya bisa mengubah hp dan alamat anaknya, dan hanya bisa menambah absensi berstatus Izin atau Sakit. Nomor pendaftaran baru kini 6 karakter acak (mis. `DF-26-K7M2QX`) agar tidak mudah ditebak.

**Wajib setelah memperbarui:** tempel ulang `firestore.rules` lalu Publish, dan pastikan **Authentication → Sign-in method → Email/Password** aktif.

**Cara menguji Rules dengan akun orang tua asli** (disarankan sebelum diumumkan):
1. Buat 2 akun orang tua dan 2 anak, tautkan masing-masing.
2. Login sebagai orang tua A: hanya anak A yang tampil. Data anak B tidak boleh terlihat.
3. Jika portal menampilkan "Akses ditolak" untuk riwayat kehadiran/hafalan/pembayaran, buka Firestore → Rules → **Rules Playground**, simulasikan baca `absensi` dengan akun orang tua, lalu kirim teks galatnya.

## Tampilan baru dan fitur tambahan website
- **Tampilan:** latar hijau bergerak dengan ornamen bintang dan partikel emas, halaman buku yang ikut miring mengikuti kursor (desktop), header transparan yang menjadi kaca saat digulir, garis progres di atas, menu yang menandai bagian aktif, animasi muncul saat digulir, dan tombol WhatsApp berdenyut. Pengguna yang mengaktifkan "kurangi gerakan" di perangkatnya otomatis tidak melihat animasi.
- **Jadwal hari ini:** baris jadwal hari ini otomatis disorot dan muncul sebagai kapsul "Hari ini: ..." di beranda.
- **Menu admin baru (Isi Website):**
  - **Agenda Kegiatan**: kegiatan mendatang (yang sudah lewat otomatis disembunyikan).
  - **Angka & Capaian**: angka yang menghitung naik, mis. "120+ Santri aktif". Isi hanya angka yang benar.
  - **Testimoni**: kutipan orang tua, bisa digeser.
  - **Pita pengumuman** di atas halaman: atur di Teks & Pengaturan Situs.
  - Bagian baru ini disembunyikan di website sampai Anda mengisinya.
- Galeri kini berbentuk mosaik dengan tampilan layar penuh yang bisa digeser (panah kiri/kanan atau tombol).
- Tempel ulang `firestore.rules` lalu Publish agar Agenda, Angka, dan Testimoni bisa dibaca website.

## Animasi dan ucapan selamat datang
- Semua animasi tambahan ada di satu berkas, **`anim.js`** (upload bersama berkas lain). Jika berkas ini tidak ada, website, admin, dan portal tetap berfungsi normal tanpa animasi tambahan.
- **Website:** layar pembuka berlogo, ucapan **"Selamat Datang"** di bagian paling atas (Arab, judul besar yang berkilau, salam sesuai waktu: pagi/siang/sore/malam), kata judul muncul satu per satu, dua lentera yang berayun, sorotan cahaya mengikuti kursor, garis judul bagian yang memanjang, efek kilau dan riak pada tombol, dan konfeti saat pendaftaran berhasil. Jika pengunjung sudah login, salam menyebut namanya.
- **Admin:** layar login dengan partikel emas dan logo melayang, menu samping yang masuk bergantian, sapaan "Selamat pagi, Admin" dengan tangan melambai, kartu statistik yang muncul bergantian, angka yang menghitung naik, grafik batang/donat yang tumbuh, baris tabel yang masuk, jendela yang membesar halus, lencana berdenyut, tampilan memuat berkilau, dan konfeti saat santri diterima.
- **Portal orang tua:** sapaan "Selamat pagi, <nama>", cincin persentase kehadiran yang terisi animasi, kartu anak yang muncul bergantian, angka naik, cincin berdenyut di avatar, dan konfeti saat akun berhasil dibuat atau pendaftaran terkirim.
- Teks ucapan di website dapat diubah di admin: **Teks & Pengaturan Situs → Ucapan selamat datang**.
- Pengguna yang mengaktifkan "kurangi gerakan" di perangkatnya otomatis tidak melihat animasi, layar pembuka, atau konfeti.
