# 🗓️[Perencanaan Proyek & Desain Awal] - [Pekan 9]

##  💡Anggota Kelompok dan Peran
- Muhammad Novri Aziztra (10231066) - Network Architect
- Zahwa Hanna Dwi Putri (10231092) - Network Engineer
- Indah Nur Fortuna (10231044) - Network Services Spesiaclist
- Putri Rahmawati (10231074) - Security & Documentation Specialist

##  📑Daftar Isi
1. [Pendahuluan](#pendahuluan)
2. [Isi Laporan](#isi-laporan)

## ✏️Pendahuluan
### Latar Belakang
PT. Nusantara Network adalah perusahaan teknologi informasi dengan empat departemen utama yang tersebar di dua lokasi, yaitu Kantor Pusat dan Kantor Cabang. Dengan jumlah perangkat yang cukup banyak di setiap departemen, perusahaan memerlukan infrastruktur jaringan yang aman, efisien, dan mudah dikelola.

Setiap departemen perlu dipisahkan dalam VLAN tersendiri untuk keamanan dan manajemen lalu lintas. Koneksi antar gedung menggunakan teknologi WAN dengan bandwidht terbatas, sehingga diperlukan routing dinamis (OSPF) untuk pengelolaan rute yang efisien. Selain itu, dibutuhkan layanan NAT, DHCP, dan DNS untuk mendukung koneksi internet serta pengalokasian IP secara otomatis.

Untuk menjaga keamanan departemen, Access Control List (ACL) diterapkan, dan seluruh jaringan dikelola secara terpusat melalui sistem monitoring. Perancangan jaringan yang tepat akan memastikan operasional perusahaan berjalan lancar dan terintegrasi dengan baik.

### Tujuan
- Membangun jaringan yang aman dan tersegmentasi melalui implementasi VLAN pada setiap departemen.
- Menghubungkan dua gedung (Kantor Pusat dan Cabang) secara efisien menggunakan teknologi WAN dengan routing dinamis (OSPF).
- Mempermudah pengelolaan alamat IP dengan layanan DHCP di setiap segmen jaringan.
- Mendukung akses internet melalui implementasi NAT yang terpusat.
- Memastikan resolusi nama jaringan berjalan optimal dengan penerapan layanan DNS untuk kebutuhan internal dan eksternal.
- Meningkatkan keamanan antar departemen dengan pembatasan akses menggunakan Access Control List (ACL).
- Menyediakan sistem monitoring dan manajemen jaringan yang terpusat untuk kemudahan pemantauan dan pemeliharaan jaringan.

### Ruang Lingkup
Perancangan jaringan PT. Nusantara Network mencakup segmentasi jaringan menggunakan VLAN untuk setiap departemen di dua lokasi, perancangan koneksi antar gedung menggunakan WAN dengan routing dinamis (OSPF), serta penerapan layanan DHCP, DNS, dan NAT. Selain itu, ruang lingkup juga mencakup pengaturan Access Control List (ACL) untuk keamanan antar departemen, serta pembangunan sistem monitoring dan manajemen jaringan secara terpusat untuk mendukung pengelolaan jaringan yang efisien dan aman.

## 📚Isi Laporan
### Daftar Anggota Kelompok beserta Peran
| Peran                            | Nama                        | NIM        |
|----------------------------------|-----------------------------|------------|
| Network Architect                | Muhammad Novri Aziztra      | 10231066   |
| Network Engineer                 | Zahwa Hanna Dwi Putri       | 10231092   |
| Network Services Specialist      | Indah Nur Fortuna           | 10231044   |
| Security & Documentation Specialist | Putri Rahmawati          | 10231074   |

### Analisis Kebutuhan PT. Nusantara Network
PT. Nusantara Network adalah perusahaan yang bergerak di bidang teknologi informasi dengan 4 departemen utama dan memiliki cabang di 2 lokasi berbeda. Perusahaan ini membutuhkan infrastruktur jaringan yang aman, efisien, dan mudah dikelola. Berikut adalah detail organisasi:

Kantor Pusat (Gedung A):
- Departemen IT (40 komputer)
- Departemen Keuangan (25 komputer)
- Departemen SDM (20 komputer)
- Server Farm (10 server untuk berbagai layanan)
  
Kantor Cabang (Gedung B):
- Departemen Marketing (30 komputer)
- Departemen Operasional (35 komputer)
  
#### Perangkat Jaringan yang dibutuhkan
- Gedung A
  - Departemen IT
      - PC: 30 unit
      - Switch: 2 unit
      - Router: 1 unit
      - VLAN: 10
  - Departemen Keuangan
      - PC: 30 unit
      - Switch: 2 unit
      - Router: 1 unit
      - VLAN: 20
  - Departemen SDM
      - PC: 20 unit
      - Switch: 1 unit
      - Router: 1 unit
      - VLAN: 30
  - Ruang Server
      - Server: 10 unit
      - Switch: 2 unit
      - Router: 1 unit
      - Firewall: 2 unit
      - VLAN: 40

- Gedung B
  - Departemen Marketing
      - PC: 30 unit
      - Switch: 2 unit
      - Router: 1 unit
      - VLAN: 50
  - Departemen Operasional
      - PC: 40 unit
      - Switch: 3 unit
      - Router: 1 unit
      - VLAN: 60
  
#### Kabel yang digunakan
- Straight-Throught Cable
- Cross-Over Cable

#### Kebutuhan IP Address dan DHCP
- VLAN 10 (IT): 192.168.10.0/26
- VLAN 20 (Keuangan): 192.168.20.0/27
- VLAN 30 (SDM): 192.168.30.0/27
- VLAN 40 (Server): 192.168.40.0/28
- VLAN 50 (Marketing): 192.168.50.0/26
- VLAN 60 (Operasional): 192.168.60.0/26

#### Koneksi Antar Gedung
Untuk menghubungkan Gedung A dan Gedung B, digunakan teknologi WAN karena lokasi fisik yang terpisah. Mengingat Bandwith koneksi terbatas, diperlukan konfigurasi agar trafik penting seperti data server atau komunikasi antardepartemen mendapat prioritas dan tidak terganggu oleh trafik lain.

#### Routing Dinamis
Agar Manajemen jaringan efisien dan mudah dikembangkan, digunakan routing dinamis OSPF (Open Shortest Path First). OSPF memungkinkan semua router mengenali jaringan VLAN secara otomatis dan menyesuaikan rute jika ada perubahan. 

#### Layanan NAT dan Akses Internet
Untuk kebutuhan akses internet, semua perangkat di dalam jaringan menggunakan NAT (Network Address Translation). Berarti semua perangkat internal bisa mengakses internet menggunakan satu IP publik dri ISP.

#### Layanan DNS
Disediakan layanan DNS internal yang digunakan untuk mengakses domain-domain lokal. Selain itu, DNS ini juga melakukan forwarding ke DNS eksternal agar tetap bisa mengakses internet dan domain publik.

#### Access Control List (ACL)
Untuk menjaga keamanan dan kontrol antar departemen, digunakan Access Control List (ACL). Misalnya, VLAN keuangan tidak diizinkan mengakses VLAN IT, lalu server hanya bisa diakses oleh VLAN IT, dan VLAN Operasional hanya diberi akses terbatas ke VLAN lain. Hal ini mencegah akses yang tidak diperlukan serta dapat meningkatkan keamanan internal jaringan.

### Timeline Proyek
| Pekan            | Aktivitas                                        | Tenggat Waktu     |
|------------------|--------------------------------------------------|-------------------|
| Pekan 9          | Perencanaan Proyek & Desain Awal                 | 12 April 2025     |
| Pekan 10         | Desain Topologi & Skema Pengalamatan             | 17 April 2025     |
| Pekan 11         | Implementasi Topologi Dasar & VLAN               | 24 April 2025     |
| Pekan 12         | Implementasi Routing & WAN                       | 1 Mei 2025        |
| Pekan 13         | Implementasi Layanan Jaringan                    | 8 Mei 2025        |
| Pekan 14         | Implementasi Keamanan & Pengujian                | 15 Mei 2025       |
| Pekan 15         | Finalisasi Dokumentasi & Presentasi              | 22 Mei 2025       |

🔗[Link Spreadsheet Timeline Proyek](https://docs.google.com/spreadsheets/d/1KvhhvqUZAaUz2ibKxUZO_6EE2K1VYFqbx3yexNJZT6o/edit?gid=0#gid=0)

![Timeline](<Timeline Proyek.png>)

### Sketsa Awal Desain Jaringan
![Sketsa](<Sketsa Awal.png>)
- Gedung A
  - Departemen IT
      - PC: 30 unit
      - Switch: 2 unit
      - Jenis Switch: Cisco Switch 2960
      - Router: 1 unit
      - Jenis Router: Cisco Router 2911
      - VLAN: 10
  - Departemen Keuangan
      - PC: 30 unit
      - Switch: 2 unit
      - Jenis Switch: Cisco Switch 2960
      - Router: 1 unit
      - Jenis Router: Cisco Router 2911
      - VLAN: 20
  - Departemen SDM
      - PC: 20 unit
      - Switch: 1 unit
      - Jenis Switch: Cisco Switch 2960
      - Router: 1 unit
      - Jenis Router: Cisco Router 2911
      - VLAN: 30
  - Ruang Server
      - Server: 10 unit
      - Jenis Server: Generic Server
      - Switch: 2 unit
      - Jenis Switch: Cisco Switch 2960
      - Router: 1 unit
      - Jenis Router: Cisco Router 1941
      - Firewall: 2 unit
      - Jenis Firewall: ASA 5505
      - VLAN: 40

- Gedung B
  - Departemen Marketing
      - PC: 30 unit
      - Switch: 2 unit
      - Jenis Switch: Cisco Switch 2960
      - Router: 1 unit
      - Jenis Router: Cisco Router 2911
      - VLAN: 50
  - Departemen Operasional
      - PC: 40 unit
      - Switch: 3 unit
      - Jenis Switch: Cisco Switch 2960
      - Router: 1 unit
      - Jenis Router: Cisco Router 2911
      - VLAN: 60

- Jenis Kabel
  - Straight-Throught Cable
  - Cross-Over Cable
