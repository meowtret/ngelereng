# ⛰️ Ngelereng

**Jalan-jalan di lereng, tebak di mana kamu berdiri.**

Ngelereng adalah game tebak medan berbasis web. Pemain melihat pemandangan perbukitan dari sudut pandang orang yang berdiri di lapangan, lalu mencocokkannya dengan peta kontur untuk menebak arah hadap atau posisi berdirinya.

Game ini dirancang untuk dua kebutuhan sekaligus: permainan santai harian untuk publik, dan alat latihan membaca peta kontur serta orientasi medan (navigasi darat).

🔗 **Main sekarang:** `https://<username>.github.io/ngelereng`

---

## Mode permainan

### Ngelereng Harian
Tiga soal per hari yang sama untuk semua pemain, dengan susunan:

1. Ke mana menghadap? (Landai)
2. Di mana berdiri? (Landai)
3. Soal Tanjakan, jenisnya bergantian setiap hari

Hasil harian dapat dibagikan dalam format teks, misalnya:

```
Ngelereng #12 ⛰️ 2/3
🟩🟥🟩
🔥 streak 5
```

### Main bebas
Soal acak tanpa batas untuk latihan. Pemain bebas memilih jenis soal dan tingkat kesulitan.

### Jenis soal
- **Ke mana menghadap?** Pemain berdiri di titik merah dan memilih satu dari delapan arah mata angin (U, TL, T, Tg, S, BD, B, BL).
- **Di mana berdiri?** Pemain menghadap utara dan memilih titik A, B, atau C.

### Tingkat kesulitan
- **Landai**: 3–4 bukit, pilihan jawaban dibuat cukup berbeda satu sama lain, peta selalu menghadap utara.
- **Tanjakan**: 5–7 bukit, pilihan jawaban lebih mirip, dan pada soal posisi peta diputar 90°, 180°, atau 270° sehingga pemain harus membaca panah utara.

---

## Cara main

1. Amati pemandangan di bagian atas layar.
2. Cocokkan bukit, lembah, dan punggungan yang terlihat dengan garis kontur di peta.
3. Pilih jawaban lewat tombol atau langsung di peta.
4. Setelah menjawab, kerucut pandang muncul di peta. Ketuk pilihan lain atau geser pemandangan ke kiri dan kanan untuk membandingkan skyline dengan kontur.

---

## Cara kerja

Seluruh proses berjalan di browser tanpa server.

| Komponen | Keterangan |
|---|---|
| Pembangkit medan | Grid ketinggian 129 × 129 sel yang mewakili area 2 × 2 km. Bentuk bukit dibuat dari penjumlahan fungsi Gaussian (posisi, tinggi, lebar, dan arah memanjang acak) ditambah *value noise*, lalu ditipiskan ke 0 di tepi peta. |
| Seed | Angka acak memakai *seeded RNG* (mulberry32). Soal harian memakai seed dari tanggal, sehingga semua pemain mendapat medan yang sama. |
| Peta kontur | Dibuat dengan `d3-contour` (marching squares). Interval kontur 10 m, kontur indeks setiap 50 m diberi garis tebal dan label elevasi. |
| Pemandangan 3D | Dirender dengan Three.js. Kamera ditempatkan 2 m di atas permukaan dengan bidang pandang horizontal 60°. |
| Validasi soal | Untuk setiap pilihan jawaban dihitung profil cakrawala (sudut elevasi tertinggi pada 25 sinar di dalam bidang pandang). Soal hanya diterima bila profil jawaban benar berbeda cukup jauh dari pilihan lain (rata-rata ≥ 2,2° untuk Landai dan ≥ 1,3° untuk Tanjakan), titik berdiri berada di dataran yang relatif datar, dan bukit yang terlihat tidak terlalu dekat. |
| Penyimpanan | Statistik dan streak disimpan di `localStorage` browser pemain (kunci `ngelereng:v1`). Tidak ada data yang dikirim ke server. |

---

## Teknologi

- HTML, CSS, dan JavaScript murni dalam satu file
- [Three.js r128](https://threejs.org/) untuk tampilan 3D
- [D3.js v7](https://d3js.org/) untuk peta kontur
- Font Bricolage Grotesque dari Google Fonts
- Hosting statis melalui GitHub Pages

---

## Struktur repo

```
ngelereng/
├── index.html   # seluruh aplikasi (HTML, CSS, JS)
└── README.md
```

---

## Menjalankan secara lokal

Karena berupa satu file statis, cukup buka `index.html` di browser. Jika ingin melalui server lokal:

```bash
git clone https://github.com/<username>/ngelereng.git
cd ngelereng
python -m http.server 8000
```

Lalu buka `http://localhost:8000`. Koneksi internet tetap diperlukan untuk memuat Three.js, D3.js, dan font dari CDN.

---

## Deploy ke GitHub Pages

1. Buat repo baru bernama `ngelereng`.
2. Unggah file game dan ganti namanya menjadi `index.html`, beserta `README.md` ini.
3. Buka **Settings → Pages**.
4. Pada **Source**, pilih **Deploy from a branch**, lalu pilih branch `main` dan folder `/ (root)`.
5. Simpan, tunggu satu sampai dua menit, lalu game dapat diakses di `https://<username>.github.io/ngelereng`.

---

## Rencana pengembangan

- [ ] Tampilan medan 3D yang lebih kaya (tekstur, variasi tutupan lahan, kabut atmosfer)
- [ ] **Ngelereng Latihan**: paket soal bertingkat dari pengenalan bentuk kontur (punggungan, lembah, sadel) sampai penentuan posisi
- [ ] Soal dari DEM nyata (DEMNAS/SRTM) wilayah Sulawesi, diproses dengan Python menjadi heightmap statis
- [ ] Pembahasan skyline: perbandingan profil cakrawala setiap pilihan secara berdampingan
- [ ] Mode ujian dengan timer dan rekap nilai yang dapat diunduh
- [ ] Rekap nilai peserta terpusat melalui Google Sheets (Apps Script)
- [ ] Kartu hasil berbentuk gambar untuk dibagikan di media sosial

---

## Inspirasi

Konsep permainan terinspirasi dari puzzle peta kontur yang dipopulerkan oleh [topopuzzles.io](https://topopuzzles.io) dan [@twoshadowsgames](https://www.instagram.com/twoshadowsgames). Seluruh kode dan medan di Ngelereng dibuat sendiri.

---

## Lisensi

Belum ditentukan.

---

Dibuat oleh **Alfian (Ian)** di Palu, Sulawesi Tengah.
