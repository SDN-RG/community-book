AVANT-GUARD: Scalable and Vigilant Switch Flow Management in Software-Defined Networks
===================

*Erlangga Ervansyah (23213302) EL5244 Pemograman Perangkat Jaringan*

----------
1. Pendahuluan
-------------

Sebagai perusahaan jaringan dan pusat kontrol data yang saat ini terus berkembang dalam segi ukuran dan kompleksitas, mereka membuat peranan dalam hal administratif lebih besar dari siapapun, sehingga permintaan layanan harus ditingkatkan atau otomatisasi dalam merancang pada baik sistem komputer maupun sumber daya jaringannya. Komunitas riset jaringan menyatakan bahwa salah satu pendekatan untuk memenuhi tantangan ini terletak dalam prinsip Software-Defined Network (SDN). 
Dengan melakukan proses decoupling logika kontrol dari implementasi tertutup serta eksklusif pada perangkat jaringan konvensional, SDN memungkinkan peneliti dan praktisi sekitar untuk merancang suatu fungsi baru untuk jaringan yang lebih inovatif juga protokol yang menangani jaringannya memproses jauh lebih mudah dan fleksibel. Struktur OpenFlow adalah salah satu yang diwujudkan pada konsep SDN. Dalam beberapa tahun terakhir, OpenFlow(OF) telah melatarbelakangi ide penelitian dalam 
eksplorasi dunia akademik jaringan untuk implementasi referensi standar SDN dimana teknologi ini menjadikan pergerakan momentumnya berperan cukup besar dalam industri saat ini.
OpenFlow (OF) menyediakan banyak tema penelitian baru bagi komunitas keamanan jaringan. Sebagai contoh, OF dapat menawarkan beberapa penyederhanaan yang cukup dramatis dalam proses desain dan integrasi dalam konteks aplikasi keamanan jaringan ke jaringan yang berukuran besar. Sayangnya, potensi OpenFlow untuk memberikan kemajuan berarti bagi negara yang mengandalkan pertahanan jaringannya harus ditahan oleh pernyataan bahwa OpenFlow sendiri memiliki tantangan yang serius dalam bidang keamanan. 
Paper ini, dijelaskan beberapa solusi yang berpotensi baik untuk dua tantangan keamanan. Pertama, jaringan OpenFlow kekurangan skalabilitas antara data plane maupun control plane. Hal ini memungkinkan serangan yang ditargetkan oleh entitas eksternal yang membangun inbound stream pada flow request menyebar di jalur komunikasi antara controller dan switch, yang kita sebut sebagai serangan control plane. Kedua, OpenFlow menawarkan dukungan yang sangat terbatas untuk aplikasi monitoring pada jaringan yang mencari pelacakan halus dari operasi dalam data plane, sehingga membuatnya sulit memperoleh dukungan terhadap banyak aplikasi keamanan yang membutuhkan akses cepat terhadap perubahan penting dalam pola trafik jaringan.

**Tantangan Skalabilitas.** penyebab utama tantangan skalabilitas terletak pada pengoperasian protokol OpenFlow dibawah perangkat switch, yang memisahkan control plane dari data plane untuk mengaktifkan kinerja kontrol yang terpusat. ketika switch OpenFlow menerima paket yang memiliki alur yang baru dan dirinya tidak memiliki kecocokan dengan alur yang ada, maka switch akan meneruskan paket ke kontroler OpenFlow.
Controller merespon dengan satu atau lebih aturan aliran yang menunjukkan bagaimana cara memproses flow yang akan datang yang juga memenuhi kriteria yang cocok pada flow sebelumnya, dan ini dirancang untuk menengahi permintaan flow yang melewati dirinya, sehingga menyebabkan halangan untuk proses scalling jaringan.
Pada saat yang sama, data plane juga menerima serangan karena switch memiliki sumber daya yang terbatas untuk flow inisiasi khusus protokol transport (TCP / UDP) sampai controller menangani flow-nya. Oleh karena itu, control plane yang diserang juga memiliki implikasi langsung untuk kemampuan operasional pada data plane. Jenis serangan seperti DDoS dan network scanning, yang ditangani serius oleh komunitas keamanan, menimbulkan potensi ancaman baru untuk skalabilitas, baik pada lapisan kontrol terpusat OpenFlow maupun SDN pada umumnya.

**Responsiveness Challenge.** Tantangan ini muncul berlandaskan dari kebutuhan untuk akses cepat ke pola aktivitas data plane. Aplikasi network monitoring bertugas mengumpulkan statistik jaringan untuk beberapa kebutuhan seperti pelacakan flow dan jaringan dengan statistik paket yang luas ataupun juga bisa untuk mengukur aktivitas berbagai entitas komunikasi melalui switch (misalnya, untuk mengidentifikasi serangan DoS, yang berdampak pada data plane). 
Teknologi Openflow SDN hanya memungkinkan suatu aplikasi untuk secara eksplisit menerima dan menelaah informasi dari setiap switch. Antarmuka ini tidak cukup untuk aplikasi monitoring yang membutuhkan pemantauan statistik pada data plane untuk melacak dan menanggapi kondisi berbahaya atau penurunan performansi suatu sistem. Selain itu, meskipun aplikasi keamanan sering membutuhkan pemeriksaan isi paket yang cocok dengan beberapa kriteria. Karena hal inilah, OpenFlow menawarkan mekanisme untuk memfasilitasi kebijakan tersebut.
Untuk mengatasi hal-hal tersebut, dikembangkanlah suatu ekstensi keamanan khusus untuk OpenFlow yang disebut AVANT-GUARD. Ada beberapa isu penting yang kita fokuskan melalui proses pengembangan ini. Pertama, menentukan jenis kecerdasan yang akan ditambahkan ke bidang data. Kedua, perlu adanya teknik yang efektif untuk menampilkan jaringan statistik untuk control plane. Ketiga, perlu dikembangkan mekanisme baru yang dapat bereaksi cepat terhadap serangan yang terdeteksi. Akhirnya, implementasi ini harus meminimalkan perubahan pada protokol OpenFlow.

![enter image description here](http://s7.postimg.org/8cmozz1nf/image.jpg)

----------
2. Desain Sistem
-------------

Untuk mengatasi beberapa masalah yang telah dijabarkan sebelumnya, AVANT-GUARD 
dibangun sebagai ekstensi keamanan pada Openflow data plane. Berikut merupakan desain infrastruktur dari sistem AVANT-GUARD.

**Arsitektur Keseluruhan.** AVANT-GUARD memiliki modul jaringan sebagai berikut: 
1) Connection Migration (CM); dan 
2) Actuating Triggers (AT). 
Diagram konseptual untuk AVANT-GUARD pada bidang data. Terinspirasi oleh proxy SYN yang menangani koneksi TCP, perlu diterapkan migrasi koneksi untuk menyaring TCP yang gagal terkirim di bidang data ke kontrol Pesawat. Proses ini bekerja sama dengan tabel akses untuk memelihara informasi sesi TCP pada bidang data yang selanjutnya mengirimkan rincian sesi ke control plane. Proses ini juga memungkinkan koleksi informasi status jaringan  dan informasi payload paket lebih efisien dari data plane sebelumnya. Selain itu, proses ini juga menawarkan flow berupa aktivasi aturan, yaitu, kemampuan untuk mengaktifkan flow aturan ketika beberapa peristiwa terjadi.

**Connection Migration(CM).** Tujuan dari proses CM adalah untuk meningkatkan mutu data plane untuk membedakan sumber-sumber yang akan mengantarkan koneksi TCP dari sumber yang tidak diterimanya. Untuk melakukan hal ini, handshaking data plane ke proxy TCP diperpanjang, menambahkannya dengan flow request untuk control plane yang telah menyelesaikan proses handshaking. Proses migrasi koneksi dalam diagram terdiri dari empat tahap: (i) klasifikasi, (ii) laporan, (iii) migrasi, dan (iv) relay. 
Setiap tahap dan transisi di antara mereka ditunjukkan dalam Gambar 2. Ketika  sumber memulai sambungan, Modul Connection Migration(CM) terlibat pada sumber  di handshaking stateless TCP menggunakan SYN cookies. Koneksi ditambahkan pada tahap klasifikasi. Setelah proses handshaking selesai, CM memberitahu control plane dari flow request, untuk mentransisi koneksi ke tahap laporan. 
Jika control plane memungkinkan proses migrasi, CM menginisiasi host tujuan dengan TCP handshaking, untuk mentransisi koneksi ke tahap migrasi. Kemudian, jika tujuan merespon, CM memberitahu control plane, dan koneksi akan memasuki tahap laporan. Akhirnya, jika control plane memungkinkan data plane untuk menyampaikan paket, CM akan menyelesaikan langsung hubungan antara sumber dan tujuan, dan koneksi pun beralih ke tahap relay.

![enter image description here](http://s27.postimg.org/kbc099w43/image.jpg)

**Actuating Triggers(AT).** proses ini memungkinkan data plane untuk melaporkan status jaringan secara asinkron dan informasi payload ke control plane. Selain itu, AT dapat digunakan untuk mengaktifkan flow condition di beberapa flow yang telah ditetapkan, yaitu kondisi untuk membantu control plane mengelola aliran data di jaringan tanpa terdapat penundaan. 
Penggerak pemicu AT terdiri dari empat operasi utama. Pertama, control plane perlu menentukan kondisi statistik trafik. Kedua, meregistrasikan kondisi ini dari control plane terhadap bidang data. Ketiga, data plane memeriksa kondisi sambil mengumpulkan aliran data secara lokal dan flow secara statistik untuk menentukan apakah kondisi telah dipenuhi. Keempat, ketika data plane telah menentukan bahwa kondisi ini dipenuhi, mungkin terdapat dua perlakuan 1) memicu call-back event ke control plane untuk menunjukkan bahwa kondisi ini dipenuhi, atau 2) memasukkan flow baru ke dalam flow tabel tertentu.

![Gambar 3: Contoh Skenario Modul AT](http://s30.postimg.org/odyhhsaox/image.jpg)
Gambar 3: Contoh Skenario Modul Actuating Triggers(AT)


----------
3. Implementasi Sistem
-------------

AVANT-GUARD tidak lain adalah sebuah switch berbasis software dengan referensi OpenFlow (atau disebut software OF switch). Acuan ini mengikuti OpenFlow spesifikasi 1.0.0, dan berfungsi sebagai data plane. Sumber kode diubah oleh penyusun materi dalam implementasi ini, untuk mendukung modul CM dan AT. Secara khusus, algoritma packet_receive disusun ulang dalam perangkat lunak switch OF untuk merespon upaya koneksi baru dengan SYN / ACK.
Jika algoritma packet_receive menerima TCP ACK (atau, jika cocok dengan SYN cookies yang dihasilkan sebelumnya), paket tersebut memerlukan izin dari control plane untuk memulai modul CM. Saat proses menerima izin, OF switch yang telah dikonfigurasikan tadi akan memulai proses koneksi TCP ke host tujuan yang sebenarnya. Untuk menyampaikan paket TCP berikutnya melalui CM, ditambahkan pula beberapa fungsi algoritma untuk memodifikasi nomor ACK atau SEQ yang sesuai dengan masing-masing paket TCP.
Terdapat tiga unsur yang ditambahkan. Switch dimodifikasi untuk dapat memeriksa
setiap kali terdapat update counter untuk setiap aliran (atau variabel lain). Jika nilai counter memenuhi suatu kondisi yang didefinisikan oleh control plane, switch menghasilkan sinyal kembali ke control plane. Untuk menerapkan aktivasi aturan pada flow, dibangun sebuah struktur data yang dapat menahan flow aturan yang telah ditetapkan. Untuk membangun fungsi AVANT-GUARD, sepuluh perintah baru OpenFlow ditambahkan, seperti yang tercantum dalam Tabel 1. Perintah-perintah ini dilaksanakan baik perangkat lunak switch OF maupun controller POX.

![Tabel 1: Command OpenFlow baru pada AVANT-GUARD](http://s3.postimg.org/8ax0rq7g3/image.jpg)

Tabel 1: Command OpenFlow Baru pada data plane AVANT-GUARD

![Gambar 4a: Skema Switch SDN OpenFlow ](http://s15.postimg.org/m9tqrhtfv/19a.jpg)
![Gambar 4b: Skema Switch SDN pada AVANT-GUARD](http://s27.postimg.org/yh4s7so2b/19b.jpg)

Gambar 4: Desain infrastrukture hardware untuk (a). Switch OpenFlow pada SDN Tradisional; (b). Switch OpenFlow pada AVANT-GUARD

**Data Plane SDN.** Pertama, perhatikan arsitektur data plane pada SDN seperti pada Gambar 4 (A). Arsitektur ini mengikuti implementasi NetFPGA dari switch OpenFlow oleh penemunya. 
Implementasi ini terdiri dari enam modul utama: (i) input arbiter, yang bertugas meneruskan paket berdasarkan logika yang dijalankan; (ii) parse header, yang mem-parsing-kan header paket; (Iii) exact match lookup , yang mentelusuri aturan pada flow (w/o wildcard) untuk diterjemahkan kedalam paket; (iv) lookup wildcard, mentelusuri aturan pada flow (dengan wildcard) untuk paket; (V) arbiter, yang memutuskan operasi apa yang akan digunakan dalam sebuah paket (forward atau drop); dan (vi) packet editor, yang bertugas meneruskan atau mengubah paket. 
Aturan Flow disimpan dalam TCAM atau SRAM (diluar ASIC), dan counter menyimpan 
nilai-nilai statistik untuk setiap flow aturan yang melekat pada TCAM atau SRAM. 
Implementasi ini meninjau perangkat keras yang dipakai menggunakan skenario berikut. Pertama, jika data plane menerima paket, komponen lookup memeriksa TCAM/SRAM untuk melihat apakah flow aturan untuk penanganan paket ini ada atau tidak. Jika benar ada, data plane meneruskan paket tersebut ke arbiter. Jika tidak, ia meminta control plane melalui antarmuka CPU.

**Implementasi Connection Migration(CM):** Untuk menerapkan CM dalam hardware,  kita perlu mengubah tiga komponen di bidang data dan menambahkan dua struktur data baru ke dalam data plane. Arsitektur data-pesawat baru yang disajikan dengan CM tertera pada Gambar 4 (B). Header parser dimodifikasi untuk mengekstrak TCP flag, dan arbiter dimodifikasi untuk memaksa editor paket untuk memulai inisiasi CM atau membalas dengan paket TCP SYN/ACK. 
Modul ini dapat melakukan CM atau menjawab permintaan sambungan dengan  mengirimkan SYN/ACK. Data plane ini harus bisa mengubah nomor ACK atau SEQ pada masing-masing paket untuk menemukan perbedaan nomor SEQ terhadap SYN (SYN dari inisiator koneksi ke data plane) dan bagian dalam koneksi (SYN dari data plane ke tujuan migrasi). Perbedaan nilai ini akan disimpan dalam satuan delta ACK/SEQ, dan jumlah nilai ini adalah sama dengan jumlah koneksi yang sedang bermigrasi. 

**Implementasi Actuating Triggers(AT):** Untuk menerapkan AT di hardware, penulis menambahkan dua struktur pada data plane, yang telah ditunjukkan pada Gambar 4 (B). Semua kondisi untuk AT ini secara kolektif diberi label "Condition" di gambar. Selain itu, flow aturan yang telah ditetapkan dapat dijalankan dengan menambahkan komponen yang sama untuk flow aturan (TCAM dan SRAM). Untuk implementasi dengan sensitif akan besar biaya, ruang penyimpanan TCAM/SRAM dapat dibagi untuk flow aturan ini (tidak ditunjukkan dalam Gambar).

----------
4. Evaluasi 
-------------

**Network Saturation Attack.** Contoh Skenario: Ruang pengujian untuk percobaan ini ditunjukkan pada Gambar 5. Terdiri atas switch OpenFlow (data plane) dimana AVANT-GUARD telah dijalankan; controller jaringan POX; server yang menjadi host untuk layanan web; user client normal yang terhubung dengan server untuk proses HTTP request; dan penyerang yang melakukan TCP SYN flood attack.

![Gambar 5: Skenario Network Saturation Attack](http://s8.postimg.org/hyvk0ltlh/image.jpg)

Gambar 5: Skenario Network Saturation Attack

![Tabel 2](http://s2.postimg.org/ri9nrpzeh/image.jpg)

![Gambar 6: Persentase paket berhasil terkirim ke webserver ](http://s29.postimg.org/s03sncfuv/image.jpg)

Gambar 6: Persentase Paket Berhasil Terkirim ke Webserver dari Klien Normal

Dalam skenario ini, kita mengukur response time, atau waktu yang dibutuhkan user client normal untuk mengambil halaman data dari server. Proses ini dilakukan dalam dua kondisi, yaitu dengan dan tanpa serangan TCP SYN flood attack dalam jaringan OpenFlow. Penyerang membangkitkan 1.000 koneksi per detik ke server, dan ini diulangi lebih dari 500 detik untuk mengukur waktu respon rata-rata.
Hasil pengujian menunjukkan waktu respon rata-rata dirangkum pada Tabel 2. Klien normal dapat mengambil halaman web dalam 0,4 detik, tetapi tidak mendapatkan respon apapun selama ada TCP SYN flood attack karena muncul efek control plane maupun data plane yang tersaturasi seperti disebutkan sebelumnya. Namun, AVANT-GUARD efektif dapat mempertahankan jaringan dari serangan ini, memungkinkan klien yang normal untuk mengambil halaman web tanpa masalah, karena data plane ini secara otomatis dan transparan mengklasifikasikan dan menghilangkan upaya koneksi TCP asing dari penyerang. 
Pengukuran overhead proses CM pada koneksi TCP normal saat tanpa serangan menggunakan setup eksperimental yang ditunjukkan pada Gambar 12. Dari Tabel 2, kita dapat melihat bahwa biaya overhead yang disebabkan oleh CM pada koneksi TCP yang normal minimal (1,86%). Untuk lebih menunjukkan efek dari serangan saturasi pada trafik normal secara rinci, penyusun mengubah tingkat kirim-terima serangan saturasi jaringan selama 0-800 per detik, dan juga mengirim permintaan dari 10 klien normal ke web server pada saat yang sama. 
Hasilnya ditunjukkan pada Gambar 6, dan kita dapat dengan mudah mengamati  bahwa permintaan dari klien normal yang tidak dikirim ke web server ketika serangan saturasi jaringan yang terjadi dengan switch OpenFlow (hampir 0% ketika serangan banjir mengirimkan lebih dari 100 paket per detik). Namun, dengan AVANT-GUARD, semua permintaan dari klien normal dapat dikirim ke server web, bahkan saat jaringan di bawah serangan saturasi jaringan.

**Network Scanning Attack.** Contoh Skenario: Metode pengujian yang dilakukan adalah bagaimana sistem menangkal jaringan dari serangan pemindaian jaringan, dan ruang lingkup pengujian ini ditunjukkan pada Gambar 7. Pada tes ini, kami menggunakan Nmap [24] untuk memindai semua port jaringan di file server (10.0.0.2) yang hanya membuka jaringan Port 10000.

![Gambar 7: Skenario Network Scanning Attack](http://s12.postimg.org/c9cxgg18d/image.jpg)

Gambar 7: Skenario Network Scanning Attack

Jika AVANT-GUARD dijalankan, data plane secara otomatis mempertahankan informasi tentang upaya koneksi TCP di tabel akses dan informasi sesi laporan ke control plane, yang dapat dengan mudah mendeteksi upaya pemindaian dengan menerapkan algoritma scan-detection sederhana. Aplikasi ini hanya perlu meminta data plane untuk melaporkan informasi tentang upaya sambungan TCP. Hasil deteksi ditandai dengan tanda garis merah pada Gambar 8 dan hasil deteksi pada Nmap tanpa dan dengan AVANT-GUARD masing-masing di gambar 9 dan 10.

![Gambar 8: Hasil Proses Scan-detection ](http://s11.postimg.org/6ipezyaqr/image.jpg)

Gambar 8: Hasil Proses Scan-detection

![Gambar 9: Hasil Deteksi pada Nmap Tanpa Menggunakan AVANT-GUARD](http://s30.postimg.org/k7wh4slk1/image.jpg)

Gambar 9: Hasil Deteksi pada Nmap Tanpa Menggunakan AVANT-GUARD

![Gambar 10: Hasil Deteksi pada Nmap Menggunakan AVANT-GUARD](http://s30.postimg.org/5dch4d1cx/image.jpg)

Gambar 10: Hasil Deteksi pada Nmap Dengan AVANT-GUARD


**Network Intrusion Attack.** skenario serangan ditunjukkan pada Gambar 11. Dalam skenario ini, penyerang (10.0.0.1) mengirimkan serangan RPC buffer overflow ke host lain dalam jaringan (10.0.0.2).

![Gambar 11: Skenario Network Intrusion Attack](http://s27.postimg.org/943mkjbo3/image.jpg)

Gambar 11: Skenario Network Intrusion Attack

Di sini kita mengasumsikan dua hal: (i) control plane sudah diminta oleh data plane untuk memberikan paket yang dikirim ke 10.0.0.2 dan (ii) aplikasi keamanan memiliki pengenal atas serangan itu. Di skenario pengujian, aplikasi menggunakan aturan berbasis snort untuk mendeteksi payload yang bahaya. 
Hasilnya ditunjukkan pada Gambar 12, di ditemukan bahwa aplikasi keamanan telah mendeteksi serangan dengan tepat dan akurat, seperti yang ditunjukkan pada tanda garis merah di gambar.

![Gambar 12: Hasil Deteksi Network Intrusion](http://s9.postimg.org/ir0m5pqyn/image.jpg)

Gambar 12: Hasil Deteksi Network Intrusion

----------
5. Kesimpulan
-------------

Dalam paper ini, AVANT-GUARD dapat diusulkan, menjadi suatu struktur terbaru untuk memajukan keamanan dan ketahanan jaringan OpenFlow dengan keterlibatan yang lebih besar dari lapisan data plane. Tujuan dari AVANTGUARD adalah untuk membuat aplikasi keamanan SDN yang lebih terukur dan responsif terhadap ancaman apapun di jaringan. Tantangan utama yang ditemukan adalah hambatan oleh antarmuka dari control plane oleh data plane yang dapat dimanfaatkan musuh. Metode migrasi koneksi (CM) memungkinkan data plane untuk melindungi control plane dari serangan saturasi tersebut. Tantangan kedua adalah masalah responsif. Aplikasi keamanan SDN membutuhkan akses statistik jaringan yang cepat dari data plane. Maka dari itu, terdapat penggerak pemicu (AT) yang secara otomatis memasukkan aturan aliran ketika jaringan berada di bawah tekanan. Implementasi software AVANT-GUARD menunjukkan bahwa overhead minimal yang dikenakan oleh peranan keamanan AVANT-GUARD, dengan keterlambatan peningkatan koneksi yang jauh lebih sedikit yaitu 1% pada overhead, saat data plane memberikan ketahanan di SDN.

----------
6. Referensi
-------------

Seungwon Shin et al., ''AVANT-GUARD: Scalable and Vigilant Switch Flow Management in Software-Defined Networks,'' in CCS '13 Proceedings of the 2013 ACM SIGSAC conference on Computer & communications security, pp. 413-424 .
