# 🗓️[Implementasi Topologi Dasar & VLAN] - [Pekan 11]

##  💡Anggota Kelompok dan Peran
- Muhammad Novri Aziztra (10231066) - Network Architect
- Zahwa Hanna Dwi Putri (10231092) - Network Engineer
- Indah Nur Fortuna (10231044) - Network Services Spesiaclist
- Putri Rahmawati (10231074) - Security & Documentation Specialist

### 🔗[Link File Simulasi](https://drive.google.com/drive/folders/1IAHRVcR4TmX3aaW_AtgLcR72CEgTnZ4v)

### Topologi
![Topologi](<newtopologi.png>)
Penjelasan : Topologi yang ditampilkan dalam gambar adalah diagram jaringan yang dibuat menggunakan Cisco Packet Tracer. Diagram ini menggambarkan struktur jaringan dalam sebuah organisasi, terdiri dari beberapa departemen atau area, seperti Departemen IT, Departemen Keuangan, Departemen SDM, Ruang Server, Departemen Marketing, dan Departemen Operasional. 

Setiap departemen memiliki perangkat seperti PC, switch, dan terhubung secara hierarkis. Switch dalam masing-masing departemen dihubungkan ke router pusat (Router1) melalui port Fast Ethernet seperti Fa0/1, Fa0/2, dan sebagainya. Topologi ini menunjukkan bahwa router berperan sebagai pusat komunikasi antar departemen, memastikan konektivitas antara berbagai segmen jaringan.

Di bagian Ruang Server, terdapat perangkat server yang dihubungkan langsung ke switch, memberikan layanan penting ke seluruh departemen. Selain itu, diagram ini memperlihatkan koneksi logis antara router dan switch, yang direpresentasikan oleh garis penghubung. Secara keseluruhan, topologi ini mencerminkan desain jaringan yang terstruktur untuk mendukung komunikasi dan kolaborasi antar berbagai area dalam organisasi. Desain ini mempermudah pengelolaan jaringan serta identifikasi masalah yang mungkin muncul. 

### Konfigurasi VLAN dan Trunking
Switch Departemen IT
![Switch](<switch1it.jpg>)
![Switch](<switch2it.jpg>)
Penjelasan : Gambar tersebut menunjukkan proses konfigurasi dua buah switch pada Cisco Packet Tracer melalui Command Line Interface (CLI). Pada kedua switch, administrator terlebih dahulu masuk ke mode konfigurasi dengan perintah enable dan conf t . Selanjutnya, dibuat dua VLAN, yaitu VLAN 10 dengan nama "VLAN_IT" dan VLAN 99 dengan nama "VLAN_NATIVE". Setelah itu, port FastEthernet dari 0/1 hingga 0/20 dikonfigurasi sebagai access port yang tergabung ke dalam VLAN 10. Sedangkan port FastEthernet 0/21 dikonfigurasi sebagai trunk port untuk menghubungkan antar switch. Pada konfigurasi trunk ini, VLAN 99 ditetapkan sebagai native VLAN. Terakhir, konfigurasi disimpan dengan perintah write memory. Notifikasi yang muncul seperti %LINK-5-CHANGEDdan %LINEPROTO-5-UPDOWN menunjukkan adanya perubahan status pada port, menandakan koneksi antar switch mulai aktif. Konfigurasi ini penting untuk membentuk jaringan VLAN yang tersegmentasi namun tetap dapat saling berkomunikasi melalui trunking.

Departemen Keuangan 
![Switch](<switch1uang.jpg>)
![Switch](<switch2uang.jpg>)
Penjelasan : Konfigurasi pada kedua switch dilakukan untuk membuat jaringan VLAN yang tersegmentasi dan terhubung melalui trunking. Pertama, VLAN 20 dibuat dengan nama VLAN_KEUANGAN dan VLAN 99 sebagai VLAN_NATIVE. Di Switch 1, port fa0/1 sampai fa0/20 dikonfigurasi sebagai akses VLAN 20, sedangkan port fa0/21 digunakan sebagai trunk. Trunk tersebut diset menggunakan perintah switchport mode trunk dan switchport trunk native vlan 99, sehingga VLAN 99 berfungsi sebagai native VLAN untuk komunikasi antar switch.

Di Switch 2, konfigurasi serupa diterapkan dengan port fa0/1 sampai fa0/5 diset sebagai akses untuk VLAN 20 dan port fa0/6 sebagai trunk. Trunk juga dikonfigurasi dengan VLAN 99 sebagai native, memastikan kedua switch bisa bertukar data VLAN 20 dengan benar tanpa tag VLAN tambahan karena penggunaan native VLAN. Setelah konfigurasi selesai, perintah write memory digunakan untuk menyimpan semua pengaturan. Dengan konfigurasi ini, segmentasi jaringan berdasarkan departemen keuangan berjalan optimal dan komunikasi antar switch tetap terjaga melalui trunk yang efisien.

Departemen SDM 
![Switch](<switchsdm.jpg>)
Penjelasan : Pada Switch 1, dibuat dua VLAN yaitu VLAN 30 (VLAN_SDM) untuk akses pengguna dan VLAN 99 (VLAN_NATIVE) sebagai native VLAN. Port fa0/1–fa0/20 diset mode akses ke VLAN 30, sementara fa0/21 diset sebagai trunk dengan native VLAN 99 untuk koneksi antar switch. Konfigurasi disimpan dengan write memory.

Namun, muncul error Native VLAN mismatch dan BPDU error di fa0/21 karena VLAN native di Switch 1 (VLAN 99) tidak sama dengan VLAN native di Switch 2 (kemungkinan masih default VLAN 1). Akibatnya, port trunk diblokir oleh STP untuk mencegah loop. Solusinya, samakan VLAN native di kedua switch. 

Server Farm 
![Switch](<switchserver.jpg>)
Penjelasan : Pada Switch 2, VLAN 60 (SERVER_FARM) dibuat untuk akses pengguna dan VLAN 99 (VLAN_NATIVE) untuk koneksi trunk. Port fa0/1–fa0/10 diset sebagai akses VLAN 60, sedangkan port fa0/11 diset sebagai trunk dengan native VLAN 99. Setelah konfigurasi disimpan, port trunk siap digunakan.

Dengan menyamakan native VLAN 99 pada kedua switch (Switch 1 dan Switch 2), masalah Native VLAN mismatch yang sebelumnya terjadi berhasil diatasi, sehingga koneksi trunk antar switch dapat berjalan normal tanpa konflik. 

Departemen Marketing 
![Switch](<switch1marketing.jpg>)
![Switch](<switch2marketing.jpg>)
Penjelasan : Switch1 dikonfigurasi untuk departemen Server Farm dengan VLAN 60 yang dinamai SERVER_FARM dan VLAN 99 sebagai VLAN_NATIVE. Port Fa0/1 hingga Fa0/10 disetel sebagai akses VLAN 60, sedangkan Fa0/11 disetel sebagai trunk dengan native VLAN 99.

Switch2 dikonfigurasi untuk departemen Marketing dengan VLAN 40 yang dinamai VLAN_MARKETING dan VLAN 99 sebagai VLAN_NATIVE. Port Fa0/1 hingga Fa0/20 diatur sebagai akses VLAN 40, dan port Fa0/21 sebagai trunk dengan native VLAN 99.

Switch3 juga untuk departemen Marketing dengan konfigurasi serupa seperti Switch2. Namun, muncul peringatan Native VLAN mismatch karena terjadi perbedaan native VLAN antara Switch3 (VLAN 99) dan perangkat yang terhubung di Fa0/11 yang kemungkinan masih menggunakan VLAN default (VLAN 1).

Departemen Operasional 
![Switch](<switchoperasional1.jpg>)
![Switch](<switchoperasional2.jpg>)
Penjelasan : Gambar pertama (switchoperasional1.jpg) menampilkan konfigurasi VLAN pada sebuah switch Cisco menggunakan antarmuka CLI. Pada gambar tersebut, administrator jaringan membuat dua VLAN, yaitu VLAN 50 dengan nama "VLAN_OPERASIONAL" dan VLAN 99 dengan nama "VLAN_NATIVE". Selanjutnya, port FastEthernet 0/1 hingga 0/20 dikonfigurasi sebagai mode access dan dimasukkan ke dalam VLAN 50. Port FastEthernet 0/21 dikonfigurasi sebagai mode trunk dengan VLAN native 99. Terjadi beberapa pesan status yang menunjukkan perubahan state pada port tersebut, seperti line protocol yang awalnya down kemudian up. Meskipun konfigurasi berhasil, terdapat pesan error saat mencoba perintah "write memory" yang menunjukkan ketidakvalidan input.  

Gambar kedua (switchoperasional2.jpg) juga menampilkan konfigurasi VLAN pada switch Cisco dengan langkah-langkah serupa. VLAN 50 dan 99 dibuat dengan nama yang sama seperti pada gambar pertama. Namun, kali ini port FastEthernet 0/1 hingga 0/15 dikonfigurasi sebagai mode access dan dimasukkan ke dalam VLAN 50, sedangkan port FastEthernet 0/16 dikonfigurasi sebagai mode trunk dengan VLAN native 99. Pesan status juga muncul, menunjukkan perubahan state pada port tersebut. Berbeda dengan gambar pertama, perintah "write memory" berhasil dieksekusi dengan pesan "[OK]", menandakan bahwa konfigurasi telah disimpan dengan sukses.  

Kedua gambar tersebut menggambarkan proses konfigurasi VLAN dan trunking pada perangkat switch Cisco, dengan perbedaan pada range port yang dikonfigurasi serta keberhasilan penyimpanan konfigurasi.

Main Switch A
![Switch](<mainswitcha1.jpg>)
![Switch](<mainswitcha2.jpg>)
Penjelasan : Gambar mainswitcha1.jpg menampilkan konfigurasi VLAN pada sebuah switch Cisco, tetapi terdapat beberapa masalah terkait Native VLAN Mismatch yang terdeteksi melalui pesan error CDP (Cisco Discovery Protocol). Pesan tersebut menunjukkan ketidakcocokan VLAN native antara port-port tertentu (Fa0/1, Fa0/3, Fa0/5, dll.) dengan port trunk (Fa0/21 atau Fa0/11) yang memiliki VLAN native 99. Selama proses pembuatan VLAN (10, 20, 30, 40, 60, dan 99), pesan error terus muncul, mengindikasikan bahwa konfigurasi trunk belum sinkron antara switch yang terhubung.

Pada gambar mainswitcha2.jpg, administrator jaringan memperbaiki masalah tersebut dengan mengonfigurasi ulang port Fa0/6 sebagai trunk dan menetapkan VLAN native 99. Setelah konfigurasi, protokol pada port Fa0/6 sempat down kemudian up, menandakan proses negosiasi trunking berhasil. Konfigurasi disimpan dengan perintah write memory, dan pesan "[OK]" menunjukkan bahwa perubahan telah berhasil diterapkan. Gambar ini menggambarkan langkah perbaikan untuk menyelesaikan masalah Native VLAN Mismatch yang muncul sebelumnya.

Main Switch B
![Switch](<mainswitchb1.jpg>)
![Switch](<mainswitchb2.jpg>)
Penjelasan : Gambar mainswitchb1.jpg menunjukkan konfigurasi VLAN pada Switch3 dengan pembuatan tiga VLAN: VLAN 50 (VLAN_OPERASIONAL), VLAN 40 (VLAN_MARKETING), dan VLAN 99 (VLAN_NATIVE). Namun, muncul pesan error Native VLAN Mismatch pada port FastEthernet0/4 yang terhubung ke port FastEthernet0/16 switch lain, karena perbedaan VLAN native (1 vs 99). Pesan ini mengindikasikan ketidaksesuaian konfigurasi trunk antara kedua perangkat, yang dapat menyebabkan masalah komunikasi jaringan.

Pada gambar mainswitchb2.jpg, administrator berusaha memperbaiki masalah tersebut dengan mengonfigurasi port FastEthernet0/1 hingga 0/4 sebagai trunk dan menetapkan VLAN native 99. Selama proses, pesan error Native VLAN Mismatch masih muncul pada beberapa port (Fa0/2 dan Fa0/3), tetapi setelah konfigurasi ulang, sistem memulihkan konsistensi port dengan pesan UNBLOCK_CONSIST_PORT. Port FastEthernet0/5 juga dikonfigurasi sebagai trunk dengan VLAN native 99, yang sempat mengalami down-up pada protokolnya, menandakan proses negosiasi trunking yang berhasil. Gambar ini menggambarkan upaya perbaikan masalah VLAN mismatch melalui sinkronisasi konfigurasi trunk.

Main Router A
![Router](<mainroutera.jpg>)
Penjelasan : Pada jendela CLI, terdapat berbagai perintah konfigurasi untuk interface GigabitEthernet, seperti pengaturan jenis encapsulation, pemberian alamat IP, dan penanganan kesalahan VLAN. Salah satu perintah yang terlihat adalah penggunaan encapsulation dot1Q untuk menentukan VLAN tertentu, seperti VLAN 10, 20, 30, dan 40. Namun, terdapat beberapa kesalahan, termasuk "native VLAN mismatch" pada interface FastEthernet dan tumpang tindih jaringan IP pada interface GigabitEthernet0/0.30. Hal ini menunjukkan pentingnya pengelolaan VLAN dan alamat IP yang hati-hati dalam konfigurasi jaringan. Gambar ini relevan untuk administrator jaringan atau pelajar yang mempelajari cara mengonfigurasi router dan menyelesaikan masalah jaringan.

Main Router B
![Router](<mainrouterb.jpg>)
Penjelasan : Pada gambar diatas terdapat sejumlah perintah konfigurasi yang diterapkan pada beberapa interface GigabitEthernet, termasuk konfigurasi VLAN dengan encapsulation dot1Q dan pemberian alamat IP pada sub-interface seperti 0/1.40, 0/1.50, dan 0/1.99. Perintah seperti "no shutdown" digunakan untuk mengaktifkan interface, dan pesan log menunjukkan perubahan status interface, misalnya menjadi aktif ("state to up"). Gambar ini relevan untuk memahami konfigurasi jaringan berbasis VLAN dan manajemen interface pada router, yang penting bagi administrator jaringan atau pelajar dalam bidang jaringan komputer.

Trunking Gedung A
![Trunk](<trunka.jpg>)
Penjelasan : Gambar diatas menunjukkan informasi mengenai konfigurasi trunk pada switch dengan menggunakan perintah show interface trunk. Perintah ini memberikan detail tentang port trunk yang aktif, termasuk mode trunk, tipe encapsulation, native VLAN, VLAN yang diizinkan, dan VLAN yang aktif. Sebagian besar port trunk, seperti FastEthernet0/1 hingga 0/6, berada dalam mode "on" dengan encapsulation 802.1q dan menggunakan native VLAN 99, sedangkan FastEthernet0/7 menggunakan mode "auto" dengan native VLAN 1. Semua port trunk mengizinkan lalu lintas untuk VLAN 1-1005, dan beberapa VLAN seperti 1, 10, 20, 30, 40, 60, dan 99 terdaftar sebagai VLAN aktif dalam domain. Informasi ini berguna bagi administrator jaringan untuk memastikan koneksi antar VLAN berjalan lancar serta untuk mengidentifikasi dan mengatasi potensi masalah dalam konfigurasi trunk. Adanya pesan "--More--" menunjukkan data tambahan yang belum terlihat dalam tampilan ini. 

### Status Trunk
Trunking Gedung B
![Trunk](<trunkb.jpg>)
Penjelasan : Gambar diatas merupakan hasil dari perintah show interface trunk pada antarmuka CLI (Command Line Interface) sebuah switch. Output tersebut memberikan informasi terkait konfigurasi trunk pada beberapa port FastEthernet (Fa0/1 hingga Fa0/5). 

Detail yang ditampilkan meliputi mode trunk yang diatur "on", tipe encapsulation yang menggunakan standar 802.1q, serta native VLAN yang disetel ke VLAN 99 untuk setiap port. Semua port mengizinkan lalu lintas dari VLAN 1 hingga 1005, dan VLAN yang aktif dalam domain manajemen mencakup VLAN 1, 40, 50, dan 99. Selain itu, VLAN yang berada dalam status forwarding pada spanning tree protocol (STP) juga sama, yaitu VLAN 1, 40, 50, dan 99.

Informasi ini penting bagi administrator jaringan untuk memastikan konfigurasi trunk yang benar pada switch, sehingga lalu lintas antar VLAN dapat dikelola dengan baik tanpa konflik. Output ini juga membantu dalam memvalidasi pengaturan spanning tree untuk memastikan kelancaran jaringan.

### Uji Konektivitas
VLAN Gedung A
![VLAN](<vlangedunga.jpg>)
Penjelasan : Gambar diatas menunjukkan hasil perintah show vlan brief pada sebuah switch, yang merangkum konfigurasi VLAN (Virtual Local Area Network). Informasi yang ditampilkan mencakup daftar VLAN, nama VLAN, status VLAN (semuanya aktif), dan port yang terhubung ke masing-masing VLAN. VLAN default (VLAN 1) memiliki banyak port aktif, sedangkan VLAN lain seperti VLAN 10, 20, 30, 40, 60, dan 99 juga terdaftar aktif tanpa port yang terhubung. Selain itu, VLAN sistem seperti 1002 hingga 1005 juga termasuk dalam daftar. Informasi ini berguna untuk memahami pengaturan segmentasi jaringan pada switch tersebut.

VLAN Gedung B
![VLAN](<vlangedungb.jpg>)
Penjelasan : Gambar diatas menunjukkan hasil dari perintah show vlan brief yang memberikan gambaran konfigurasi VLAN pada sebuah switch. Output ini mencantumkan beberapa VLAN yang terkonfigurasi, seperti VLAN 1 (default), VLAN 40, 50, dan 99, beserta VLAN sistem seperti 1002 hingga 1005. Informasi yang ditampilkan meliputi nama VLAN, status (semua aktif), dan port yang terhubung. VLAN default memiliki banyak port yang digunakan, termasuk beberapa FastEthernet (Fa0/6 hingga Fa0/24) dan GigabitEthernet (Gi0/1 dan Gi0/2), sementara VLAN lainnya belum memiliki port yang terhubung. Output ini membantu administrator jaringan untuk memahami distribusi port dan pengelolaan VLAN pada perangkat jaringan.

### Konfigurasi Router
![Router](<ujikonek1.jpg>)
![Router](<ujikonek2.jpg>)
Penjelasan : Gambar diatas merupakan hasil perintah show ip interface brief pada CLI router "Router1". Output ini menunjukkan informasi konfigurasi antarmuka jaringan, termasuk nama antarmuka, alamat IP, metode konfigurasi IP, serta status administratif dan protokol. Beberapa antarmuka seperti GigabitEthernet0/0 dan GigabitEthernet0/1 berada dalam kondisi "administratively down" tanpa alamat IP. Sub-antarmuka seperti GigabitEthernet0/1.50 dan GigabitEthernet0/1.51 memiliki alamat IP terkonfigurasi, tetapi protokolnya "down". Sementara itu, antarmuka FastEthernet0/0/0 terlihat aktif dengan status "up" baik secara administratif maupun protokolnya. Pesan "--More--" mengindikasikan adanya informasi tambahan yang belum ditampilkan. Output ini penting untuk memverifikasi konektivitas jaringan dan mengidentifikasi masalah pada antarmuka.

Selain itu, informasi seperti alamat IP terkonfigurasi secara manual dan status protokol yang "down" pada sub-antarmuka menunjukkan adanya potensi kendala dalam koneksi jaringan. Status "administratively down" pada beberapa antarmuka utama juga mengindikasikan bahwa konfigurasi jaringan masih membutuhkan penyesuaian agar seluruh antarmuka dapat berfungsi secara optimal. Output ini menjadi alat penting bagi administrator untuk melakukan troubleshooting dan optimisasi konfigurasi jaringan pada router.

### Kendala dan Solusi
1. Kesusahan untuk mengkonfigurasi karena pada topologi sebelumnya ternyata terlalu tricky/sulit alurnya, jadi menyelesaikan kendala ini dengan menghapus router setiap departemen.
2. Kesalahan dalam penentuan Subnetting, sehingga pada saat konfigurasi terjadi invalid. Memperbaiki kendala ini adalah dengan menyesuaikan pada VLAN yang sudah ditentukan setiap departemennya

### Table Subnetting Terbaru
| VLAN | Nama        | Gedung     | Subnet              | Gateway IP (Router) |
|------|-------------|------------|---------------------|---------------------|
| 10   | IT          | Gedung A   | 192.168.10.0/24     | 192.168.10.1        |
| 20   | Keuangan    | Gedung A   | 192.168.20.0/24     | 192.168.20.1        |
| 30   | SDM         | Gedung A   | 192.168.30.0/24     | 192.168.30.1        |
| 60   | Server      | Gedung A   | 192.168.60.0/24     | 192.168.60.1        |
| 40   | Marketing   | Gedung B   | 192.168.40.0/24     | 192.168.40.1        |
| 50   | Operasional | Gedung B   | 192.168.50.0/24     | 192.168.50.1        |
| 99   | Native VLAN | Semua      | –                   | –                   |
| -    | Link Router A–B | Koneksi Antar Router | 192.168.100.0/30 | Router A: .1, Router B: .2 |

