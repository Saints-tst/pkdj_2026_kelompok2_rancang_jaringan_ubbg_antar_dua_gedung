# Rancang Bangun Jaringan UBBG yang Menghubungkan Dua Gedung Kuliah

Proyek simulasi jaringan komputer yang menghubungkan dua gedung kuliah Universitas Bina Bangsa Getsempena (UBBG) menggunakan **Cisco Packet Tracer**.

**Mata Kuliah:** Pemodelan Komunikasi Data dan Jaringan
**Dosen Pengampu:** Ahmad Mujahid Abdurrahman, S.Kom, M.T
**Program Studi:** Ilmu Komputer, Fakultas Sains Teknologi dan Ilmu Kesehatan
**Universitas:** Universitas Bina Bangsa Getsempena, Banda Aceh

## Anggota Kelompok 2

| Nama | NIM | Tugas |
|---|---|---|
| Rahmad Fadli | 23210129 | Perancangan topologi dan pengalamatan IP |
| Muhammad Fayyadh Alghifari | 23210010 | Konfigurasi router, routing, switch, VLAN, dan link antargedung |
| Maulidzhan Syam Ikraam | 23210095 | Konfigurasi wireless, server, dan keamanan jaringan serta Pengujian sistem |
| Darmawansya | 23210080 | Pengujian Sistem dan Dokumetasi seluruh hasil pengujian |

## Latar Belakang

Kegiatan perkuliahan di lebih dari satu gedung membutuhkan jaringan yang terhubung agar sumber daya dan layanan dapat dipakai bersama. Proyek ini merancang jaringan terpadu untuk dua gedung kuliah UBBG dengan segmentasi VLAN, pengalamatan IP terstruktur, dan keamanan dasar. Server penyimpan data penting ditempatkan di Gedung A dan hanya dapat diakses secara privat dari kedua gedung.

## Tujuan

- Merancang topologi jaringan yang menghubungkan dua gedung kuliah dengan server di Gedung A.
- Menentukan perangkat dan jenis koneksi pada simulasi Cisco Packet Tracer.
- Menyusun pengalamatan IP dan segmentasi VLAN untuk kedua gedung.
- Menerapkan keamanan dasar dan pembatasan akses agar server bersifat privat.
- Menguji konektivitas antargedung dan akses ke server.

## Topologi

Topologi **extended star**: router terhubung ke switch Gedung A, yang terhubung ke switch Gedung B melalui link trunk antargedung.

```
                ROUTER (R-UBBG)
                      |
   SWITCH GEDUNG A (SW-A) <== trunk antargedung ==> SWITCH GEDUNG B (SW-B)
      |                                                  |
 Server UBBG, Access Point,                    Access Point, PC Lab
 PC Lab (Gedung A)                             (Gedung B)
```

## Perangkat

| Perangkat | Jumlah | Fungsi |
|---|---|---|
| Router Cisco 2911 | 1 | Routing antar-VLAN (router-on-a-stick), DHCP, ACL |
| Switch Cisco 2960 | 2 | Satu per gedung, VLAN dan trunk |
| Server-PT | 1 | Server data UBBG (HTTP, FTP, DNS) di Gedung A |
| Access Point-PT | 2 | Wi-Fi, satu per gedung |
| PC / Laptop | Minimal 4 | Pengujian |

## Rancangan VLAN dan Pengalamatan IP

| VLAN | Peruntukan | Jaringan | Gateway | Lokasi |
|---|---|---|---|---|
| 10 | Lab/Kelas Gedung A | 192.168.10.0/24 | 192.168.10.1 | Gedung A |
| 20 | Wi-Fi Gedung A | 192.168.20.0/24 | 192.168.20.1 | Gedung A |
| 30 | Lab/Kelas Gedung B | 192.168.30.0/24 | 192.168.30.1 | Gedung B |
| 40 | Wi-Fi Gedung B | 192.168.40.0/24 | 192.168.40.1 | Gedung B |
| 50 | Server UBBG | 192.168.50.0/24 | 192.168.50.1 | Gedung A |
| 99 | Manajemen perangkat | 192.168.99.0/24 | 192.168.99.1 | Kedua gedung |

IP pengguna diberikan otomatis oleh DHCP di router. Perangkat jaringan dan server memakai IP statis (server: `192.168.50.10`).

## Keamanan

- Password dan akses manajemen dibatasi dari VLAN manajemen (SSH).
- ACL: VLAN Wi-Fi tidak dapat mengakses VLAN manajemen, dan server hanya dapat diakses dari VLAN pengguna.
- Port security pada port switch pengguna.
- Enkripsi WPA2 pada access point.
- Port switch yang tidak dipakai dinonaktifkan.

## Rencana Pengujian

| Pengujian | Skenario | Target |
|---|---|---|
| Konektivitas antargedung | Ping Gedung A ke Gedung B dan sebaliknya | Berhasil tanpa packet loss |
| Akses server dari Gedung A | PC VLAN Gedung A mengakses server | Berhasil |
| Akses server dari Gedung B | PC VLAN Gedung B mengakses server lewat trunk | Berhasil |
| DHCP | Perangkat baru terhubung ke tiap VLAN | IP sesuai segmen VLAN |
| Segmentasi dan ACL | Akses VLAN manajemen dari VLAN Wi-Fi | Ditolak |
| Jalur paket | `tracert` dan mode Simulation | Sesuai rancangan |

## Cara Menjalankan

1. Instal **Cisco Packet Tracer**.
2. Buka file simulasi `.pkt` pada repository ini.
3. Jalankan pengujian ping, web, dan FTP sesuai tabel di atas.

## Batasan

- Seluruh pekerjaan berupa pemodelan dan simulasi, tanpa perangkat fisik.
- Jarak dan jalur kabel antargedung merupakan asumsi.
- Menggunakan IPv4 pada blok alamat privat.
- Akses internet, analisis biaya, dan pemantauan otomatis tidak termasuk cakupan.

## Pengembangan Selanjutnya

Penambahan jalur cadangan (redundansi), akses internet, dan perluasan ke gedung-gedung lain.
