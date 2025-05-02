# 🗓️[Implementasi Routing & WAN] - [Pekan 12]

##  💡Anggota Kelompok dan Peran
- Muhammad Novri Aziztra (10231066) - Network Architect
- Zahwa Hanna Dwi Putri (10231092) - Network Engineer
- Indah Nur Fortuna (10231044) - Network Services Spesiaclist
- Putri Rahmawati (10231074) - Security & Documentation Specialist

### 🔗[Link File Simulasi](https://drive.google.com/drive/folders/1IAHRVcR4TmX3aaW_AtgLcR72CEgTnZ4v)

### Konfigurasi Routing Statis Pada Jaringan Intra-Gedung
Pada implementasi jaringan ini, routing statis tidak digunakan karena seluruh konektivitas antar-subnet dan antar-gedung telah dilakukan melalui routing dinamis menggunakan protokol OSPF. Setiap VLAN dalam satu gedung telah dikenali sebagai jaringan yang langsung terhubung oleh masing - masing router, sehingga tidak memerlukan konfigurasi rute statis tambahan. Dalam hal ini, semua VLAN dalam satu gedung sudah langsung terhubung ke router masing - masing, sehingga router otomatis mengenali jaringan-jaringan tersebut sebagai directly connected network. Karena itu, implementasi ini tidak menambahkan rute statis agar antar-VLAN bisa saling terhubung dalam gedung yang sama.

Sementara itu, untuk menghubungkan jaringan antar gedung, digunakan routing dinamis dengan OSPF. Protokol OSPF memungkinkan setiap router bertukar informasi secara otomatis tentang jaringan yang mereka ketahui. Hal ini membuat jalur komunikasi antar gedung dapat terbentuk tanpa perlu konfigurasi rute satu per satu. Selain itu, OSPF juga dapat menyesuaikan rute dengan cepat jika terjadi perubahan pada jaringan, seperti koneksi antar-router terputus atau ada perangkat baru yang ditambahkan.

### Implementasi Routing Dinamis (OSPF) Untuk Koneksi Antar-Gedung
- Konfigurasi IP Sub-Interface Untuk VLAN di Router A
  ![Router A](<routerA.png>)

  Penjelasan : Pada jendela CLI, terdapat berbagai perintah konfigurasi untuk interface GigabitEthernet, seperti pengaturan jenis encapsulation, pemberian alamat IP, dan penanganan kesalahan VLAN. Salah satu perintah yang terlihat adalah penggunaan encapsulation dot1Q untuk menentukan VLAN tertentu, seperti VLAN 10, 20, dan 60. Sub-interface GigabitEthernet0/1.10 dikonfigurasi dengan alamat IP 192.168.10.1 dan subnet mask 255.255.255.0 untuk VLAN 10 (VLAN_IT), GigabitEthernet0/1.20 dengan alamat IP 192.168.20.1 dan subnet mask 255.255.255.0 untuk VLAN 20 (VLAN_KEUANGAN), serta GigabitEthernet0/1.60 dengan alamat IP 192.168.60.1 dan subnet mask 255.255.255.0 untuk VLAN 60 (VLAN_SERVER). Namun, terdapat beberapa kesalahan, termasuk "native VLAN mismatch" pada interface FastEthernet dan tumpang tindih jaringan IP pada interface GigabitEthernet0/0.30. 

- Konfigurasi IP Sub-Interface Untuk VLAN di Router B
  ![Router B](<routerB.png>)

  Penjelasan : Pada jendela CLI, terdapat berbagai perintah konfigurasi untuk interface GigabitEthernet, seperti pengaturan jenis encapsulation, pemberian alamat IP, dan penanganan kesalahan VLAN. Salah satu perintah yang terlihat adalah penggunaan encapsulation dot1Q untuk menentukan VLAN tertentu, seperti VLAN 40 dan VLAN 50. Sub-interface GigabitEthernet0/1.50 dikonfigurasi dengan alamat IP 192.168.50.1 dan subnet mask 255.255.255.0 untuk VLAN 50 (VLAN_OPERASIONAL), sedangkan sub-interface GigabitEthernet0/1.40 dikonfigurasi dengan alamat IP 192.168.40.1 dan subnet mask 255.255.255.0 untuk VLAN 40 (VLAN_MARKETING). Namun, terdapat beberapa kesalahan, termasuk "native VLAN mismatch" pada interface FastEthernet dan tumpang tindih jaringan IP pada interface GigabitEthernet0/1.99. 

- Routing Dinamis OSPF (Antar-Gedung) di Router A
  ![Router A](<routerAdinamisOSPF.png>)

  Penjelasan : Pada jendela CLI, terlihat konfigurasi routing dinamis menggunakan OSPF yang dilakukan pada Router A. Router ini dikonfigurasi dengan OSPF menggunakan perintah router ospf 1. Beberapa jaringan dimasukkan dalam konfigurasi OSPF, seperti network 192.168.10.0 0.0.0.255 area 0, network 192.168.30.0 0.0.0.255 area 0, dan network 192.168.100.0 0.0.0.3 area 0, yang masing - masing mengkonfigurasi jaringan untuk VLAN 10, VLAN 30, dan koneksi antar router.

  Namun, muncul pesan peringatan %CDP-4-NATIVE_VLAN_MISMATCH, yang menunjukkan adanya ketidakcocokan antara native VLAN pada interface FastEthernet0/0/1 di Router A dan switch yang terhubung pada FastEthernet0/6. Hal ini dapat menyebabkan masalah dalam komunikasi trunk antar perangkat. Setelah konfigurasi selesai, perintah write memory digunakan untuk menyimpan konfigurasi yang telah diterapkan. 

- Routing Dinamis OSPF (Antar-Gedung) di Router B
  ![Router B](<routerBdinamisOSPF.png>)

  Penjelasan : Pada jendela CLI, terlihat konfigurasi routing dinamis menggunakan OSPF yang dilakukan pada Router B. Router ini dikonfigurasi dengan OSPF menggunakan perintah router ospf 1. Beberapa jaringan dimasukkan dalam konfigurasi OSPF, seperti network 192.168.40.0 0.0.0.255 area 0, network 192.168.50.0 0.0.0.255 area 0, dan network 192.168.100.0 0.0.0.3 area 0, yang masing - masing mengkonfigurasi jaringan untuk VLAN 40, VLAN 50, dan koneksi antar router.

  Namun, muncul pesan peringatan %CDP-4-NATIVE_VLAN_MISMATCH, yang menunjukkan adanya ketidakcocokan antara native VLAN pada interface FastEthernet0/0/0 di Router B dan switch yang terhubung pada FastEthernet0/5. Hal ini dapat menyebabkan masalah dalam komunikasi trunk antar perangkat. Setelah konfigurasi selesai, perintah write memory digunakan untuk menyimpan konfigurasi yang telah diterapkan. 

- Hasil Perintah show ip route Pada Router A
  ![Router A](<hasilshowiprouteA.jpg>)

  Penjelasan : Gambar di atas menunjukkan hasil perintah show ip route pada Router A. Tabel routing ini memberikan informasi tentang rute yang digunakan oleh router untuk mengirimkan lalu lintas antar jaringan.

  Prefix O = route dari OSPF :

  Rute dengan prefix O menunjukkan bahwa rute tersebut berasal dari protokol OSPF (Open Shortest Path First). Sebagai contoh, pada rute 192.168.40.0/24 [110/2] via 192.168.100.2, 00:07:40, GigabitEthernet0/1, terlihat bahwa jaringan 192.168.40.0/24 dapat dijangkau melalui OSPF dengan next hop IP 192.168.100.2, dan interface yang digunakan adalah GigabitEthernet0/1.

  Prefix C = directly connected (VLAN lokal) :

  Rute dengan prefix C menunjukkan jaringan yang terhubung langsung (directly connected) ke router. Misalnya, 192.168.10.0/24 is directly connected, GigabitEthernet0/0.10 menunjukkan bahwa jaringan 192.168.10.0/24 terhubung langsung melalui interface GigabitEthernet0/0.10.

  Via IP (Next-Hop) :

  Pada setiap entri, Anda dapat melihat via IP, yang menunjukkan alamat IP tujuan selanjutnya (next-hop) yang harus ditempuh oleh paket untuk mencapai jaringan yang dituju. Contohnya, pada rute 192.168.40.0/24 [110/2] via 192.168.100.2, paket akan dikirimkan ke IP 192.168.100.2 sebagai next-hop.

### Simulasi Koneksi WAN Antar Gedung (Uji Konektivitas)
- WAN Test Dari Router A ke Router B
  ![Router A](<wantestrouterAkeB.jpg>)

  Penjelasan : Pada jendela CLI, terlihat perintah ping yang digunakan untuk menguji konektivitas antar-gedung (WAN Test) dari Router A ke Router B. Perintah ping 192.168.100.2 digunakan untuk mengirimkan 5 paket ICMP Echo Request dengan ukuran 100-byte ke alamat IP tujuan 192.168.100.2 yang terhubung ke Router B. Tanda "!!!!" menunjukkan bahwa semua paket yang dikirimkan berhasil menerima tanggapan (reply), menandakan bahwa jalur antara Router A dan Router B berjalan dengan baik. Success rate tercatat 100 percent (5/5) yang berarti semua paket yang dikirimkan berhasil diterima dengan baik tanpa ada paket yang hilang. Round-trip min/avg/max = 0/3/15 ms menunjukkan waktu perjalanan pulang-pergi paket, dengan waktu rata - rata 3 ms dan waktu maksimum 15 ms, yang mengindikasikan latensi yang sangat rendah. Ini menunjukkan konektivitas yang cepat dan stabil antar gedung A dan gedung B. Hasil uji konektivitas ini memastikan bahwa jalur WAN antar gedung berfungsi dengan baik dan dapat digunakan untuk komunikasi antara Gedung A dan Gedung B tanpa masalah.

- WAN Test Dari Router B ke Router A
  ![Router B](<wantestrouterBkeA.jpg>)

  Penjelasan : Pada jendela CLI, terlihat perintah ping yang digunakan untuk menguji konektivitas antar-gedung (WAN Test) dari Router B ke Router A. Perintah ping 192.168.100.1 digunakan untuk mengirimkan 5 paket ICMP Echo Request dengan ukuran 100-byte ke alamat IP tujuan 192.168.100.1, yang terhubung ke Router A. Tanda "!!!!" menunjukkan bahwa semua paket yang dikirimkan berhasil menerima tanggapan (reply), menandakan bahwa jalur antara Router B dan Router A berjalan dengan baik. Success rate tercatat 100 percent (5/5) yang berarti semua paket yang dikirimkan berhasil diterima dengan baik tanpa ada paket yang hilang. Round-trip min/avg/max = 0/2/10 ms menunjukkan waktu perjalanan pulang-pergi paket, dengan waktu rata - rata 2 ms dan waktu maksimum 10 ms, yang mengindikasikan latensi yang sangat rendah. Ini menunjukkan konektivitas yang cepat dan stabil antar Gedung B dan Gedung A. Hasil uji konektivitas ini memastikan bahwa jalur WAN antar gedung berfungsi dengan baik dan dapat digunakan untuk komunikasi antara Gedung B dan Gedung A tanpa masalah.

- Wan Test Router A ke Vlan 10
  ![Router A](<wantestrouterAkevlan10.jpg>)

  Penjelasan : Pada jendela CLI, terlihat perintah ping yang digunakan untuk menguji konektivitas antar VLAN lokal di Gedung A. Perintah ping 192.168.10.2 digunakan untuk mengirimkan 5 paket ICMP Echo Request ke alamat IP 192.168.10.2, yang merupakan alamat IP di VLAN 10 di Gedung A. Tanda "!!!!" menunjukkan bahwa semua paket yang dikirimkan berhasil mendapatkan tanggapan (reply), yang berarti jalur komunikasi antar VLAN di Gedung A berfungsi dengan baik. Success rate tercatat 100 percent (5/5) yang berarti tidak ada paket yang hilang. Round-trip min/avg/max = 1/18/46 ms menunjukkan waktu perjalanan pulang-pergi paket, dengan waktu rata - rata 18 ms dan waktu maksimum 46 ms, yang menunjukkan latensi yang relatif rendah.

  Kemudian, perintah ping 192.168.10.1 digunakan untuk mengirimkan 5 paket ICMP Echo Request ke alamat IP 192.168.10.1, yang juga terhubung dengan VLAN 10 di Gedung A. Hasilnya sama, yaitu 100 percent (5/5) berhasil, dengan round-trip min/avg/max = 1/2/4 ms, yang menunjukkan latensi yang sangat rendah antara perangkat dalam VLAN yang sama. Secara keseluruhan, hasil uji ping ini menunjukkan bahwa komunikasi antar perangkat di dalam VLAN 10 di Gedung A berjalan lancar tanpa masalah, dengan latensi yang sangat baik.

- Test Ping Antar Vlan Gedung A ke Gedung B (Pc Vlan 10 ke Pc Vlan 40)
  ![PC Vlan 10](<pingantarvlangedAkegedB.jpg>)

  Penjelasan : Pada gambar di atas, terlihat hasil perintah ping yang digunakan untuk menguji konektivitas antar VLAN di dua gedung yang berbeda, yaitu dari PC di VLAN 10 di Gedung A ke PC di VLAN 40 di Gedung B. Perintah ping 192.168.40.2 digunakan untuk mengirimkan 5 paket ICMP Echo Request ke alamat IP 192.168.40.2 di Gedung B. Pada percobaan pertama, hasilnya menunjukkan 1 paket yang hilang (25% loss), yang berarti hanya 3 dari 4 paket yang berhasil mendapatkan respons. Round-trip min/avg/max = 0ms/4ms/12ms menunjukkan bahwa waktu perjalanan pulang-pergi paket rata - rata adalah 4ms, dengan waktu maksimum 12ms, yang menunjukkan adanya latensi yang masih wajar meskipun ada sedikit kehilangan paket. Pada percobaan kedua, hasilnya menunjukkan 0% packet loss, yang berarti semua paket yang dikirim berhasil mendapatkan respons, dengan round-trip min/avg/max = 1ms/2ms/5ms, yang menunjukkan latensi yang lebih rendah dan sangat baik antara kedua perangkat. Hasil uji ping ini mengindikasikan bahwa meskipun ada sedikit gangguan pada percobaan pertama, konektivitas antar VLAN di dua gedung (Gedung A dan Gedung B) sudah stabil dan dapat berjalan dengan baik pada percobaan kedua.

- Test Ping Antar Vlan Gedung B ke Gedung A (Pc Vlan 40 ke Pc Vlan 10 dan 20)
  ![PC Vlan 40](<pingantarvlangedBkegedA.jpg>)

  Penjelasan : Pada gambar di atas, terlihat hasil perintah ping yang digunakan untuk menguji konektivitas antar VLAN di dua gedung yang berbeda, yaitu dari PC di VLAN 40 di Gedung B ke PC di VLAN 10 dan 20 di Gedung A. Perintah ping 192.168.10.2 digunakan untuk mengirimkan 4 paket ICMP Echo Request ke alamat IP 192.168.10.2 yang berada di Gedung A (VLAN 10). Hasilnya menunjukkan bahwa semua paket berhasil diterima tanpa kehilangan paket (0% loss), dengan round-trip min/avg/max = 0ms/1ms/1ms, yang menunjukkan latensi yang sangat rendah dan koneksi yang stabil antara PC di Gedung B dan PC di VLAN 10 di Gedung A.

  Selanjutnya, perintah ping 192.168.20.2 digunakan untuk mengirimkan 4 paket ICMP Echo Request ke alamat IP 192.168.20.2 yang berada di VLAN 20 Gedung A. Pada percobaan ini, terlihat bahwa 1 paket hilang (25% loss), yang berarti hanya 3 dari 4 paket berhasil mendapatkan respons. Round-trip min/avg/max = 0ms/1ms/5ms menunjukkan waktu perjalanan pulang-pergi paket dengan waktu maksimum 5ms, yang masih tergolong baik meskipun ada sedikit kehilangan paket. Secara keseluruhan, hasil uji ping ini menunjukkan bahwa konektivitas antar VLAN di Gedung A dan Gedung B berjalan dengan baik, meskipun ada sedikit gangguan pada komunikasi dengan VLAN 20 di Gedung A.

### Perbedaan Routing Statis dan Dinamis
Routing statis dan routing dinamis punya perbedaan yang cukup besar dalam cara kerjanya. Routing statis artinya kita harus menentukan jalur ke setiap jaringan secara manual. Ini cocok untuk jaringan kecil yang tidak sering berubah, karena lebih sederhana dan tidak membebani kerja router. Tapi, kalau ada perubahan di jaringan, kita harus mengubah konfigurasinya sendiri. Jadi kurang fleksibel dan cukup merepotkan kalau jaringannya besar atau sering berubah.

Sebaliknya, routing dinamis seperti OSPF ini lebih pintar karena bisa menyesuaikan jalur secara otomatis. Kalau ada perubahan seperti kabel putus atau ada router baru, OSPF bisa mencari jalur bary sendiri tanpa perlu diatur ulang. Ini sangat membantu di jaringan besar atau antar-gedung seperti pada tugas ini. Kekurangannya, routing dinamis sedikit lebjh berat untuk router karena memerlukan lebih banyak proses. Tapi secara umum, untuk jaringan yang kompleks dan butuh fleksibiltas, routing dinamis jauh lebih praktis dan efisien dibanding routing statis.

