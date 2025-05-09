# 🗓️[Implementasi Layanan Jaringan] - [Pekan 13]

##  💡Anggota Kelompok dan Peran
- Muhammad Novri Aziztra (10231066) - Network Architect
- Zahwa Hanna Dwi Putri (10231092) - Network Engineer
- Indah Nur Fortuna (10231044) - Network Services Spesiaclist
- Putri Rahmawati (10231074) - Security & Documentation Specialist

### Tujuan 
Implementasi layanan jaringan memiliki tujuan utama untuk meningkatkan efisiensi, kemudahan administrasi, dan keamanan dalam pengelolaan jaringan suatu organisasi. Salah satu layanan penting adalah _DHCP (Dynamic Host Configuration Protocol)_, yang memungkinkan otomatisasi pemberian alamat IP kepada perangkat dalam jaringan. Dengan DHCP, tidak perlu konfigurasi manual untuk setiap perangkat, sehingga dapat mengurangi kemungkinan konflik IP dan memastikan bahwa setiap perangkat mendapatkan parameter jaringan yang benar tanpa kendala. Selain itu, _DNS (Domain Name System)_ berperan dalam menerjemahkan nama domain menjadi alamat IP, mempermudah akses ke layanan internal tanpa harus mengingat deretan angka yang kompleks. Dengan adanya DNS, pengguna dapat terhubung ke berbagai sistem dan layanan dengan cara yang lebih intuitif dan efisien. Sementara itu, _NAT (Network Address Translation)_ memungkinkan perangkat dalam jaringan lokal untuk berkomunikasi dengan jaringan eksternal menggunakan satu alamat IP publik. NAT tidak hanya membantu menghemat penggunaan alamat IP publik tetapi juga meningkatkan keamanan dengan menyembunyikan alamat IP internal dari pihak luar. Dengan mengimplementasikan ketiga layanan ini, sistem jaringan menjadi lebih terorganisir, aman, dan mudah dikelola, memastikan konektivitas yang stabil dan optimal bagi seluruh pengguna.

### Konfigurasi CLI untuk DHCP dan DNS
#### Departemen IT
- DHCP untuk Departemen IT (192.168.10.0/24)
- ip dhcp excluded-address 192.168.10.1 192.168.10.10
- ip dhcp pool IT_POOL
- network 192.168.10.0 255.255.255.0
- default-router 192.168.10.1
- dns-server 192.168.10.254
- exit

#### Departemen Keuangan
- DHCP untuk Departemen Keuangan (192.168.20.0/24)
- ip dhcp excluded-address 192.168.20.1 192.168.20.10
- ip dhcp pool KEUANGAN_POOL
- network 192.168.20.0 255.255.255.0
- default-router 192.168.20.1
- dns-server 192.168.20.254
- exit

#### Departemen SDM
- DHCP untuk Departemen SDM (192.168.30.0/24)
- ip dhcp excluded-address 192.168.30.1 192.168.30.10
- ip dhcp pool SDM_POOL
- network 192.168.30.0 255.255.255.0
- default-router 192.168.30.1
- dns-server 192.168.30.254
- exit

#### Server
- DHCP untuk Server (192.168.60.0/24)
- ip dhcp excluded-address 192.168.60.1 192.168.60.10
- ip dhcp pool SERVER_POOL
- network 192.168.60.0 255.255.255.0
- default-router 192.168.60.1
- dns-server 192.168.60.254
- exit

#### Departemen Marketing
- DHCP untuk Departemen Marketing (192.168.40.0/24)
- ip dhcp excluded-address 192.168.40.1 192.168.40.10
- ip dhcp pool MARKETING_POOL
- network 192.168.40.0 255.255.255.0
- default-router 192.168.40.1
- dns-server 192.168.40.254
- exit

#### Departemen Operasional
- DHCP untuk Operasional (192.168.50.0/24)
- ip dhcp excluded-address 192.168.50.1 192.168.50.10
- ip dhcp pool OPERASIONAL_POOL
- network 192.168.50.0 255.255.255.0
- default-router 192.168.50.1
- dns-server 192.168.50.254
- exit

#### Hasil Konfigurasi DHCP dan DNS Router A
![Router A](<showipa.jpg>)

#### Hasil Konfigurasi DHCP dan DNS Router B
![Router B](<showipb.jpg>)

### Konfigurasi CLI untuk NAT
! Konfigurasi NAT Overload (PAT) Lengkap

! Langkah 1: Buat Access-List untuk menentukan IP lokal yang di-NAT
access-list 100 permit ip 192.168.0.0 0.0.255.255 any

! Langkah 2: Terapkan NAT Overload menggunakan interface publik
ip nat inside source list 100 interface GigabitEthernet0/1 overload

! Langkah 3: Tentukan interface mana yang 'inside' dan 'outside'
interface GigabitEthernet0/0   ! Interface ke jaringan lokal (Gedung A/B)
 ip address 192.168.1.1 255.255.0.0
 ip nat inside
 no shutdown
exit

interface GigabitEthernet0/1   ! Interface ke ISP
 ip address dhcp
 ip nat outside
 no shutdown
exit

Konfigurasi di atas merupakan langkah-langkah lengkap untuk menerapkan _NAT Overload (PAT)_ pada perangkat router Cisco. Pada langkah pertama, dibuat access-list dengan nomor 100 yang memberikan izin untuk seluruh alamat IP dalam rentang 192.168.0.0/16 agar dapat diteruskan ke jaringan luar. Langkah ini bertujuan untuk mendefinisikan alamat-alamat IP lokal (private) yang akan dikonversi menggunakan NAT.

Selanjutnya, pada langkah kedua, konfigurasi NAT Overload dilakukan dengan perintah ip nat inside source list 100 interface GigabitEthernet0/1 overload. Perintah ini menginstruksikan router untuk menggunakan IP publik dari interface GigabitEthernet0/1 (yang terhubung ke ISP) sebagai alamat asal, dan memungkinkan banyak perangkat internal menggunakan satu alamat IP publik tersebut melalui teknik port address translation.

Pada langkah ketiga, ditentukan arah lalu lintas NAT dengan menandai interface GigabitEthernet0/0 sebagai ip nat inside, karena terhubung ke jaringan lokal (misalnya Gedung A atau B), dan memberikan alamat IP statis 192.168.1.1/16. Sedangkan interface GigabitEthernet0/1 yang mengarah ke ISP diberi perintah ip nat outside serta dikonfigurasi untuk menerima alamat IP publik secara otomatis melalui DHCP. Perintah no shutdown pada kedua interface memastikan bahwa koneksi aktif. Dengan konfigurasi ini, router dapat meneruskan lalu lintas dari jaringan lokal ke internet dengan efisien menggunakan satu IP publik.

### Pengujian Alokasi IP Dinamis
Pengujian dilakukan dengan mengaktifkan DHCP pada perangkat client, sehingga perangkat dapat memperoleh alamat IP secara otomatis dari DHCP Server yang telah dikonfigurasi. 
#### Departemen IT
![IP](<dhcpit.jpg>)
Berikut adalah detail parameter yang diperoleh melalui DHCP:
- Alamat IP yang diterima: 192.168.10.11
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.10.1
- DNS Server: 192.168.10.254
- Status DHCP Request: Berhasil

Hasil konfigurasi DHCP menunjukkan bahwa perangkat klien berhasil memperoleh parameter jaringan secara otomatis dari DHCP Server yang dikelola oleh _Departemen IT_. Perangkat menerima alamat IP 192.168.10.11 dengan subnet mask 255.255.255.0, yang menunjukkan bahwa klien berada dalam jaringan dengan rentang IP 192.168.10.0/24. Default gateway yang diterima adalah 192.168.10.1, yang digunakan sebagai jalur utama untuk komunikasi keluar dari jaringan lokal. Selain itu, DNS Server yang diberikan adalah 192.168.10.254, yang berperan dalam menerjemahkan nama domain menjadi alamat IP untuk akses layanan internet atau internal.

Status DHCP Request yang ditampilkan adalah berhasil, menandakan bahwa proses pemberian alamat IP berjalan dengan baik tanpa kendala. Hal ini menunjukkan bahwa DHCP Server berfungsi optimal dalam mendistribusikan konfigurasi IP secara otomatis, mengurangi kebutuhan konfigurasi manual pada perangkat klien, serta memastikan efisiensi dan konsistensi konfigurasi jaringan internal.

#### Departemen Keuangan
![IP](<dhcpkeuangan.jpg>)
Berikut adalah detail parameter yang diperoleh melalui DHCP:
- Alamat IP yang diterima: 192.168.20.11
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.20.1
- DNS Server: 192.168.20.254
- Status DHCP Request: Berhasil

DHCP Server di _Departemen Keuangan_ telah berfungsi secara optimal dalam mendistribusikan konfigurasi jaringan kepada perangkat klien. Berdasarkan hasil yang diperoleh, perangkat berhasil menerima alamat IP 192.168.20.11 dengan subnet mask 255.255.255.0, yang menandakan perangkat berada dalam jaringan 192.168.20.0/24. Default gateway yang diberikan adalah 192.168.20.1, memungkinkan perangkat melakukan komunikasi antar jaringan, khususnya akses keluar dari jaringan lokal. Selain itu, DNS Server yang diterima adalah 192.168.20.254, yang berperan penting dalam proses penerjemahan nama domain ke alamat IP.

Status permintaan DHCP tercatat berhasil, menunjukkan bahwa proses pemberian alamat IP berjalan dengan lancar tanpa kendala. Dengan konfigurasi ini, perangkat di Departemen Keuangan dapat langsung terhubung ke jaringan tanpa memerlukan pengaturan manual, sehingga efisiensi dan konsistensi pengelolaan jaringan dapat terjaga. Seluruh parameter yang diberikan sesuai dengan desain jaringan yang telah dirancang khusus untuk departemen ini.

#### Departemen SDM
![IP](<dhcpsdm.jpg>)
Berikut adalah detail parameter yang diperoleh melalui DHCP:
- Alamat IP yang diterima: 192.168.30.12
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.30.1
- DNS Server: 192.168.30.254
- Status DHCP Request: Berhasil

Implementasi DHCP di _Departemen Marketing_ menunjukkan kinerja yang efektif dalam mendistribusikan konfigurasi jaringan secara otomatis kepada perangkat klien. Perangkat berhasil memperoleh alamat IP 192.168.30.12 dengan subnet mask 255.255.255.0, yang menandakan bahwa perangkat berada di dalam jaringan 192.168.30.0/24. Default gateway yang diberikan, yaitu 192.168.30.1, memungkinkan perangkat melakukan komunikasi ke luar jaringan lokal. Selain itu, perangkat menerima DNS Server 192.168.30.254, yang bertugas menerjemahkan nama domain ke alamat IP untuk mendukung akses ke layanan internet maupun sumber daya internal.

Status DHCP Request: Berhasil menunjukkan bahwa proses permintaan konfigurasi berjalan lancar dan DHCP Server mampu merespons dengan baik. Seluruh parameter yang diterima sudah sesuai dengan rancangan topologi jaringan untuk departemen ini, sehingga menjamin konektivitas internal yang stabil dan efisien tanpa perlu konfigurasi manual pada setiap perangkat. Dengan demikian, DHCP Server berperan penting dalam mendukung kelancaran operasional jaringan di Departemen Marketing.

#### Departemen Marketing
![IP](<dhcpmarketing.jpg>)
Berikut adalah detail parameter yang diperoleh melalui DHCP:
- Alamat IP yang diterima: 192.168.40.13
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.40.1
- DNS Server: 192.168.40.254
- Status DHCP Request: Berhasil

Pengujian DHCP di _Departemen Marketing_ menunjukkan bahwa server DHCP berhasil mendistribusikan konfigurasi jaringan secara otomatis kepada perangkat klien. Perangkat menerima alamat IP 192.168.40.13 dengan subnet mask 255.255.255.0, yang menunjukkan bahwa perangkat berada dalam jaringan 192.168.40.0/24. Default gateway yang diberikan adalah 192.168.40.1, memungkinkan perangkat menjalin komunikasi antar jaringan, sementara DNS Server 192.168.40.254 digunakan untuk proses resolusi nama domain ke alamat IP.

Status DHCP Request: Berhasil menandakan bahwa proses alokasi IP berjalan tanpa kendala. Seluruh parameter yang diberikan telah sesuai dengan skema jaringan yang dirancang untuk Departemen Marketing, mendukung konektivitas internal yang stabil dan efisien tanpa perlu konfigurasi manual. Konfigurasi ini juga memungkinkan perangkat untuk berkomunikasi dengan layanan eksternal secara lancar.

#### Departemen Operasional
![IP](<dhcpoperasional.jpg>)
Berikut adalah detail parameter yang diperoleh melalui DHCP:
- Alamat IP yang diterima: 192.168.50.11
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.50.1
- DNS Server: 192.168.50.254
- Status DHCP Request: Berhasil

Pengujian DHCP pada perangkat PC di _Departemen Operasional_ menunjukkan hasil yang optimal, di mana perangkat berhasil memperoleh konfigurasi jaringan secara otomatis dari DHCP Server. Perangkat menerima alamat IP 192.168.50.11, dengan subnet mask 255.255.255.0, yang mengindikasikan bahwa perangkat berada dalam jaringan 192.168.50.0/24. Selain itu, default gateway 192.168.50.1 telah diterapkan, berperan sebagai jalur keluar menuju jaringan lain di luar subnet lokal.

DNS Server yang diterima adalah 192.168.50.254, yang memungkinkan perangkat untuk melakukan resolusi nama domain. Status DHCP Request: Berhasil menandakan bahwa proses permintaan dan penerimaan konfigurasi jaringan telah dilakukan tanpa hambatan. Dengan konfigurasi ini, perangkat di Departemen Operasional dapat terhubung secara efisien ke jaringan internal, serta dapat mengakses layanan eksternal dengan bantuan DNS publik jika dikonfigurasi lebih lanjut.

#### Server
![IP](<dhcpserver.jpg>)
Berikut adalah detail parameter yang diperoleh melalui DHCP:
- Alamat IP yang diterima: 192.168.60.11
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.60.1
- DNS Server: 192.168.60.254
- Status DHCP Request: Berhasil

Berdasarkan pengujian DHCP, Server0 berhasil menerima konfigurasi jaringan secara otomatis tanpa perlu pengaturan manual. Alamat IP yang diberikan adalah 192.168.60.11, yang menunjukkan bahwa server berada di dalam jaringan 192.168.60.0/24, dengan subnet mask 255.255.255.0, yang menandakan bahwa server dapat berkomunikasi dengan perangkat lain dalam jaringan lokal yang sama. Default gateway 192.168.60.1 berfungsi sebagai jalur keluar dari jaringan lokal menuju jaringan lainnya, termasuk akses ke internet. DNS Server 192.168.60.254 bertugas untuk memfasilitasi proses resolusi nama domain.

Status DHCP Request: Berhasil menandakan bahwa perangkat telah berhasil memperoleh pengaturan jaringan secara otomatis dan siap digunakan dalam lingkungan jaringan tersebut. Server ini siap untuk berkomunikasi dengan perangkat lainnya di jaringan internal maupun mengakses layanan eksternal dengan lancar.

### Perbaikan Trunk
![Trunk](<perbaikantrunk.jpg>)
Gambar di atas menunjukkan proses konfigurasi trunk pada switch jaringan melalui antarmuka baris perintah (CLI). Dalam proses ini, dilakukan sejumlah langkah untuk memastikan koneksi trunk berfungsi dengan baik dan bebas dari gangguan. Pada tahap awal, muncul peringatan terkait ketidaksesuaian VLAN native antara dua switch yang terhubung, yang berpotensi menyebabkan gangguan komunikasi antar perangkat.

Untuk mengatasi permasalahan tersebut, dilakukan pengaturan ulang pada interface FastEthernet, termasuk penetapan mode akses dan trunk, serta pengaturan VLAN yang diizinkan melalui trunk. Selain itu, VLAN native pada port trunk diperbarui menjadi VLAN 99 untuk memastikan kompatibilitas konfigurasi antar perangkat.

Meskipun pesan peringatan masih muncul setelah konfigurasi diperbarui, langkah-langkah ini bertujuan untuk meningkatkan kestabilan koneksi trunk serta memastikan pengelolaan lalu lintas data antar VLAN dapat berjalan secara optimal. Konfigurasi ini merupakan bagian penting dalam menjaga efisiensi dan stabilitas jaringan secara keseluruhan. Jika diperlukan, analisis tambahan dapat disertakan dalam laporan untuk menjelaskan dampak dari perubahan konfigurasi terhadap kinerja jaringan.

![Trunk](<perbaikantrunk2.jpg>)
Gambar ini menampilkan proses konfigurasi trunk pada switch jaringan menggunakan antarmuka baris perintah (CLI). Dalam gambar tersebut terlihat sejumlah perintah yang digunakan untuk mengatur VLAN serta konektivitas antar perangkat dalam jaringan. Salah satu pesan peringatan yang muncul adalah ketidaksesuaian VLAN native pada port FastEthernet0/21, yang dapat menyebabkan gangguan komunikasi antar switch.

Untuk mengatasi permasalahan tersebut, dilakukan konfigurasi dengan mengubah mode switchport menjadi trunk, menentukan daftar VLAN yang diizinkan, serta menetapkan VLAN native ke VLAN 99. Meskipun setelah konfigurasi dilakukan peringatan VLAN mismatch masih muncul, langkah-langkah ini ditujukan untuk memastikan koneksi trunk dapat berfungsi secara optimal sehingga lalu lintas data antar VLAN berjalan dengan baik.

Laporan ini dapat diperluas dengan analisis tambahan mengenai dampak konfigurasi terhadap performa jaringan serta solusi lebih lanjut yang dapat diterapkan guna meningkatkan stabilitas dan kompatibilitas antar perangkat jaringan.

### Konektivitas ke Jaringan Eksternal melalui NAT
#### Departemen IT
![Ping](<pingit.jpg>)
Pada gambar yang ditampilkan, dilakukan pengujian konektivitas jaringan menggunakan perintah ping di Cisco Packet Tracer. Pengujian pertama dilakukan dengan melakukan ping ke alamat IP 192.168.10.254, namun hasilnya menunjukkan bahwa permintaan ping mengalami "Request timed out" yang mengindikasikan tidak ada respons dari perangkat dengan IP tersebut. Statistik ping menunjukkan bahwa dari 4 paket yang dikirim, 3 di antaranya hilang, dengan tingkat kehilangan paket mencapai 100%. Selanjutnya, dilakukan ping ke IT.server.local, namun perintah ini gagal karena nama host IT.server.local tidak dapat ditemukan, dengan pesan kesalahan "Ping request could not find host", yang kemungkinan disebabkan oleh kesalahan konfigurasi DNS atau penulisan nama host yang tidak tepat.

Kemudian, dilakukan ping ke alamat IP 192.168.20.1 dan 192.168.30.1, yang keduanya berhasil dengan baik. Semua 4 paket yang dikirimkan ke masing-masing alamat IP menerima respons tanpa adanya kehilangan paket, serta waktu respons yang sangat cepat, berkisar antara 1 ms hingga 2 ms. Dengan demikian, dapat disimpulkan bahwa koneksi jaringan ke perangkat dengan IP 192.168.20.1 dan 192.168.30.1 berjalan lancar, sementara masalah terjadi pada konektivitas ke 192.168.10.254 dan dalam pencarian nama host IT.server.local. Masalah tersebut perlu ditindaklanjuti untuk memperbaiki konektivitas jaringan yang tidak stabil pada perangkat tersebut.

#### Departemen Keuangan
![Ping](<pingkeuangan.jpg>)
Pada gambar yang ditampilkan, dilakukan pengujian konektivitas jaringan menggunakan perintah ping di Cisco Packet Tracer. Pengujian pertama dilakukan dengan melakukan ping ke alamat KEUANGAN.server.local, namun hasilnya menunjukkan pesan kesalahan "Ping request could not find host", yang mengindikasikan bahwa perangkat tidak dapat menemukan host dengan nama KEUANGAN.server.local. Hal ini kemungkinan disebabkan oleh kesalahan konfigurasi DNS atau penulisan nama host yang tidak tepat.

Selanjutnya, dilakukan ping ke alamat 192.168.10.1, dan pengujian ini berhasil dengan baik. Semua 4 paket yang dikirimkan menerima respons tanpa adanya kehilangan paket, serta waktu respons yang sangat cepat (kurang dari 1ms). Kemudian, dilakukan ping ke 192.168.30.1, yang juga berhasil. Semua paket yang dikirimkan diterima kembali tanpa ada yang hilang, dengan waktu respons berkisar antara 19ms hingga kurang dari 1ms.

Terakhir, dilakukan ping ke 192.168.40.1, yang juga berhasil dengan baik. Semua paket yang dikirimkan mendapatkan respons tanpa ada kehilangan paket, dengan waktu respons sekitar 2ms. Dengan demikian, dapat disimpulkan bahwa koneksi jaringan ke perangkat dengan alamat IP 192.168.10.1, 192.168.30.1, dan 192.168.40.1 berjalan dengan lancar tanpa masalah. Namun, terdapat masalah dalam pencarian host KEUANGAN.server.local, yang perlu ditindaklanjuti untuk memastikan konfigurasi DNS atau nama host sudah benar.

#### Departemen Marketing
![Ping](<pingmarketing.jpg>)
Pada gambar yang ditampilkan, dilakukan pengujian konektivitas jaringan menggunakan perintah ping di Cisco Packet Tracer. Pengujian pertama dilakukan dengan melakukan ping ke alamat MARKETING.server.local, namun hasilnya menunjukkan pesan kesalahan "Ping request could not find host", yang mengindikasikan bahwa perangkat tidak dapat menemukan host dengan nama MARKETING.server.local. Hal ini kemungkinan disebabkan oleh kesalahan konfigurasi DNS atau penulisan nama host yang tidak tepat.

Selanjutnya, dilakukan ping ke alamat 192.168.10.1, dan pengujian ini berhasil dengan baik. Semua 4 paket yang dikirimkan menerima respons tanpa adanya kehilangan paket, serta waktu respons yang sangat cepat (kurang dari 1ms).Kemudian, dilakukan ping ke 192.168.20.1, yang juga berhasil. Semua paket yang dikirimkan diterima kembali tanpa ada yang hilang, dengan waktu respons yang sangat cepat (kurang dari 1ms).

Terakhir, dilakukan ping ke 192.168.30.1, yang juga berhasil dengan baik. Semua paket yang dikirimkan mendapatkan respons tanpa ada kehilangan paket, dengan waktu respons yang sangat cepat (kurang dari 1ms).Dengan demikian, dapat disimpulkan bahwa koneksi jaringan ke perangkat dengan alamat IP 192.168.10.1, 192.168.20.1, dan 192.168.30.1 berjalan dengan lancar tanpa masalah. Namun, terdapat masalah dalam pencarian host MARKETING.server.local, yang perlu ditindaklanjuti untuk memastikan konfigurasi DNS atau nama host sudah benar.

#### Departemen Operasional
![Ping](<pingoperasional.jpg>)
Pada gambar yang ditampilkan, dilakukan pengujian konektivitas jaringan menggunakan perintah ping di Cisco Packet Tracer. Pengujian pertama dilakukan dengan melakukan ping ke alamat OPERASIONAL.server.local, namun hasilnya menunjukkan pesan kesalahan "Ping request could not find host", yang mengindikasikan bahwa perangkat tidak dapat menemukan host dengan nama OPERASIONAL.server.local. Hal ini kemungkinan disebabkan oleh kesalahan konfigurasi DNS atau penulisan nama host yang tidak tepat.

Selanjutnya, dilakukan ping ke alamat 192.168.10.1, dan pengujian ini berhasil dengan baik. Semua 4 paket yang dikirimkan menerima respons tanpa adanya kehilangan paket, serta waktu respons yang sangat cepat (kurang dari 1ms).Kemudian, dilakukan ping ke 192.168.20.1, yang juga berhasil. Semua paket yang dikirimkan diterima kembali tanpa ada yang hilang, dengan waktu respons yang sangat cepat (kurang dari 1ms).

Terakhir, dilakukan ping ke 192.168.30.1, yang juga berhasil dengan baik. Semua paket yang dikirimkan mendapatkan respons tanpa ada kehilangan paket, dengan waktu respons yang sangat cepat (kurang dari 1ms). Dengan demikian, dapat disimpulkan bahwa koneksi jaringan ke perangkat dengan alamat IP 192.168.10.1, 192.168.20.1, dan 192.168.30.1 berjalan dengan lancar tanpa masalah. Namun, terdapat masalah dalam pencarian host OPERASIONAL.server.local, yang perlu ditindaklanjuti untuk memastikan konfigurasi DNS atau nama host sudah benar.

#### Departemen SDM
![Ping](<pingsdm.jpg>)
Pada gambar yang ditampilkan, dilakukan pengujian konektivitas jaringan menggunakan perintah ping di Cisco Packet Tracer. Pengujian pertama dilakukan dengan melakukan ping ke alamat SDM.server.local, namun hasilnya menunjukkan pesan kesalahan "Ping request could not find host", yang mengindikasikan bahwa perangkat tidak dapat menemukan host dengan nama SDM.server.local. Masalah ini kemungkinan disebabkan oleh kesalahan konfigurasi DNS atau penulisan nama host yang tidak tepat.

Selanjutnya, dilakukan ping ke alamat 192.168.10.1, dan pengujian ini berhasil dengan baik. Semua 4 paket yang dikirimkan menerima respons tanpa adanya kehilangan paket, serta waktu respons yang sangat cepat (kurang dari 1ms). Pengujian yang sama juga dilakukan untuk alamat IP 192.168.20.1 dan 192.168.40.1, yang keduanya berhasil dengan hasil yang sama: tidak ada kehilangan paket dan waktu respons yang cepat, berkisar antara kurang dari 1ms.

Dengan demikian, dapat disimpulkan bahwa koneksi jaringan ke perangkat dengan alamat IP 192.168.10.1, 192.168.20.1, dan 192.168.40.1 berjalan dengan lancar tanpa adanya masalah. Namun, terdapat masalah dalam pencarian host SDM.server.local, yang perlu ditindaklanjuti untuk memastikan konfigurasi DNS atau nama host sudah benar.

#### Server
![Ping](<pingserver.jpg>)
Pada gambar yang ditampilkan, dilakukan pengujian konektivitas jaringan menggunakan perintah ping di Server0. Pengujian pertama dilakukan dengan melakukan ping ke alamat 192.168.100.1, dan pengujian ini berhasil dengan baik. Semua 4 paket yang dikirimkan menerima respons tanpa adanya kehilangan paket, serta waktu respons yang sangat cepat (kurang dari 1ms).

Selanjutnya, dilakukan ping ke alamat 192.168.100.2, yang juga berhasil dengan baik. Semua paket yang dikirimkan diterima kembali tanpa ada yang hilang, dengan waktu respons yang sangat cepat (kurang dari 1ms). Kemudian, dilakukan ping ke 192.168.20.1, yang juga berhasil. Semua paket yang dikirimkan diterima kembali tanpa ada yang hilang, dengan waktu respons yang sangat cepat (kurang dari 1ms).

Dengan demikian, dapat disimpulkan bahwa koneksi jaringan ke perangkat dengan alamat IP 192.168.100.1, 192.168.100.2, dan 192.168.20.1 berjalan dengan lancar tanpa masalah. Semua pengujian menunjukkan 0% packet loss dan waktu respons yang sangat cepat, menandakan tidak adanya masalah dalam konektivitas jaringan antara perangkat-perangkat tersebut.

### 🔗[Link File Simulasi Topologi Cisco Packet Tracer](https://drive.google.com/file/d/1eWPLZPmp67LEDll8b4BmjfgwdYy3U9A7/view?usp=drive_link)