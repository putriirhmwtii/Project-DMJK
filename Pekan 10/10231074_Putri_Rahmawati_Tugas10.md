# 🗓️[ Desain Topologi & Skema Pengalamatan] - [Pekan 10]

##  💡Anggota Kelompok dan Peran
- Muhammad Novri Aziztra (10231066) - Network Architect
- Zahwa Hanna Dwi Putri (10231092) - Network Engineer
- Indah Nur Fortuna (10231044) - Network Services Spesiaclist
- Putri Rahmawati (10231074) - Security & Documentation Specialist

#### Diagram Topologi Fisik dan Logis
![Topologi](<Topologi.jpg>)

Penjelasan:
Gambar diatas merupakan desain finalisasi topologi jaringan komputer dari dua gedung (Gedung A dan Gedung B) PT. Nusantara Network yang terkoneksi dengan pusat data (server center), dirancang menggunakan Cisco Packet Tracer. Topologi ini merepresentasikan jaringan perusahaan besar yang terdiri dari beberapa departemen. Yuk kita bedah secara 

#### Struktur Umum Jaringan
Topologi ini dibagi menjadi tiga bagian utama:
1. Gedung A – Terdiri dari Departemen IT, Keuangan, dan SDM.
2. Gedung B – Terdiri dari Departemen Marketing dan Operasional.
3. Ruang Server – Pusat layanan jaringan.

Semua bagian saling terhubung melalui perangkat router dan switch.

#### Gedung A
- Departemen IT
  - Terdiri dari 3 segmen LAN yang masing-masing menggunakan 1 switch (2960-24TT).
  - Tiap switch terhubung ke beberapa PC.
  - Switch terhubung ke router.

- Departemen Keuangan
  - Switch yang masing-masing terhubung ke banyak PC.
  - Terkoneksi ke router 2911 juga, lalu ke core switch.

- Departemen SDM
  - Switch yang masing-masing terhubung ke banyak PC.
  - Terhubung ke router 2911, lalu ke jaringan pusat.

#### Gedung B
- Departemen Marketing
  - Switch yang masing-masing terhubung ke banyak PC.
  - Terkoneksi ke router 2911.

- Departemen Operasional
  - Terdiri dari beberapa switch dan router yang serupa dengan departemen lain.
  - Semua switch terhubung ke router, kemudian ke jaringan pusat.

- Ruang Server (Data Center)
  - Terdapat beberapa server PT.
  - Semua server terhubung ke satu switch utama (2960-24TT).
  - Switch server ini terhubung ke router pusat (2911) dan switch utama dari setiap gedung.

#### 🔀 Topologi Fisik dan Logis
- Topologi Fisik: Menggunakan hierarki bintang (star topology) di masing-masing departemen.
- Topologi Logis:
  - Menggunakan core router dan core switch sebagai penghubung antar departemen.
  - Mungkin menggunakan VLAN untuk memisahkan traffic antar departemen.

#### 🔧 Perangkat Jaringan yang Digunakan
| Komponen      | Tipe             | Jumlah             | Fungsi                                                 |
|---------------|------------------|--------------------|--------------------------------------------------------|
| PC            | PC-PT            | 150                | Representasi pengguna di tiap departemen               |
| Switch        | 2960-24TT        | 12                 | Menghubungkan PC dalam satu LAN                        | 
| Router        | 2911             | 6                  | Interkoneksi antar LAN & server, routing               |
| Server        | Server-PT        | 10                 | Layanan pusat: DHCP, DNS, Web, File, dll               |

#### 📝 Kesimpulan
Topologi ini mencerminkan jaringan enterprise dengan struktur terpusat namun tersegmentasi, cocok untuk perusahaan dengan banyak divisi. Desain ini mendukung efisiensi, pengelolaan trafik, keamanan, dan pengembangan di masa depan.

#### Perencanaan Subnetting IP Privat
| Departemen          | Host Aktif | IP Diperlukan | Subnet Mask       | Prefix | VLAN ID |
|---------------------|------------|---------------|-------------------|--------|---------|
| IT                  | 40         | 64            | 255.255.255.192   | /26    | 10      |
| Keuangan            | 25         | 32            | 255.255.255.224   | /27    | 20      |
| SDM                 | 20         | 32            | 255.255.255.224   | /27    | 30      |
| Server Farm         | 10         | 16            | 255.255.255.240   | /28    | 40      |
| Marketing           | 30         | 64            | 255.255.255.192   | /26    | 50      |
| Operasional         | 35         | 64            | 255.255.255.192   | /26    | 60      |
| Antar Router (trunk)| 2          | 4             | 255.255.255.252   | /30    | 99      |

#### Tabel Pengalamatan IP 
| Departemen          | Subnet             | IP Range                         | Broadcast        | Gateway          | VLAN ID |
|---------------------|--------------------|----------------------------------|------------------|------------------|---------|
| IT                  | 192.168.0.0/26     | 192.168.0.1 – 192.168.0.62       | 192.168.0.63     | 192.168.0.1      | 10      |
| Keuangan            | 192.168.0.64/27    | 192.168.0.65 – 192.168.0.94      | 192.168.0.95     | 192.168.0.65     | 20      |
| SDM                 | 192.168.0.96/27    | 192.168.0.97 – 192.168.0.126     | 192.168.0.127    | 192.168.0.97     | 30      |
| Server Farm         | 192.168.0.128/28   | 192.168.0.129 – 192.168.0.142    | 192.168.0.143    | 192.168.0.129    | 40      |
| Marketing           | 192.168.0.144/26   | 192.168.0.145 – 192.168.0.206    | 192.168.0.207    | 192.168.0.145    | 50      |
| Operasional         | 192.168.0.208/26   | 192.168.0.209 – 192.168.0.270    | 192.168.0.271    | 192.168.0.209    | 60      |
| Antar Router        | 192.168.0.272/30   | 192.168.0.273 – 192.168.0.274    | 192.168.0.275    | 192.168.0.273    | 99      |

#### Daftar Perangkat yang dibutuhkan
| Gedung   | Departemen        | PC     | Switch (jenis)              | Router (jenis)              | Server (jenis)       | Firewall (jenis)     | VLAN |
|----------|-------------------|--------|-----------------------------|-----------------------------|----------------------|----------------------|------|
| Gedung A | IT                | 40     | 2 × Cisco Switch 2960       | 1 × Cisco Router 2911       | -                    | -                    | 10   |
| Gedung A | Keuangan          | 25     | 2 × Cisco Switch 2960       | 1 × Cisco Router 2911       | -                    | -                    | 20   |
| Gedung A | SDM               | 20     | 1 × Cisco Switch 2960       | 1 × Cisco Router 2911       | -                    | -                    | 30   |
| Gedung A | Ruang Server      | -      | 2 × Cisco Switch 2960       | 1 × Cisco Router 1941       | 10 × Generic Server  | 2 × ASA 5505         | 40   |
| Gedung B | Marketing         | 30     | 2 × Cisco Switch 2960       | 1 × Cisco Router 2911       | -                    | -                    | 50   |
| Gedung B | Operasional       | 35     | 3 × Cisco Switch 2960       | 1 × Cisco Router 2911       | -                    | -                    | 60   |

| Jenis Kabel         | Pengertian                                                                  | Fungsi Utama                                                                 |
|---------------------|-----------------------------------------------------------------------------|------------------------------------------------------------------------------|
| Straight-Through    | Kabel jaringan dengan susunan ujung ke ujung sama (T568A-T568A / T568B-T568B). Digunakan untuk menghubungkan perangkat berbeda jenis.| Menghubungkan perangkat berbeda jenis seperti PC ke Switch, Router ke Switch. |
| Cross-Over          | Kabel jaringan dengan susunan ujung ke ujung berbeda (T568A-T568B). Digunakan untuk menghubungkan perangkat sejenis. | Menghubungkan perangkat sejenis seperti PC ke PC, Switch ke Switch, atau Router ke Router. |


#### Rencana Penerapan VLAN
| VLAN ID | Nama VLAN        | Departemen/Fungsi                | Tujuan/Deskripsi                                                                       |
|---------|------------------|----------------------------------|----------------------------------------------------------------------------------------|  
| 10      | VLAN_IT          | Departemen IT (Gedung A)         | Memisahkan jaringan IT agar memiliki akses yang lebih luas terhadap sistem & server.   |
| 20      | VLAN_KEUANGAN    | Departemen Keuangan (Gedung A)   | Memberikan keamanan pada komunikasi data keuangan.                                     |
| 30      | VLAN_SDM         | Departemen SDM (Gedung A)        | Melindungi data personal dan operasional terkait kepegawaian.                          |
| 40      | VLAN_SERVER      | Server Farm (Gedung A)           | Menjadi pusat layanan jaringan seperti DNS, DHCP, dsb.                                 |
| 50      | VLAN_MARKETING   | Departemen Marketing (Gedung B)  | Mengatur lalu lintas divisi marketing yang mungkin mengakses data campaign & klien.    |
| 60      | VLAN_OPERASIONAL | Departemen Operasional (Gedung B)| Mengelola konektivitas untuk operasional harian kantor cabang.                         |
| 99      | VLAN_TRUNK       | Antar-router/switch (trunk)      | VLAN native/trunk untuk komunikasi antar perangkat jaringan (router-on-a-stick setup). |

