##**Multi-Home Network dan Visual Home Sharing Berbasis SDN**##

*Cokorda Gde Kresna Dhita*

**1. Pendahuluan**
	
Saat ini, digital living berkembang pesat seiring dengan berkembangnya perangkat elektronik cerdas yang semakin pesat. Hal tersebut merubah hidup kita pada semua hal, termasuk kehidupan dirumah. Perkembangan Home-networking  kedepan nya akan memungkinkan anggota keluarga untuk mengakses jaringan rumah dari jarak yang jauh sekalipun. Tetapi keanekaragaman perangkat dan protokol yang digunakan oleh masing masing perangkat yang berbeda – beda timbul sebuah tantangan yaitu masalah interoperability. Pengembangan future home networking kedepannya yang memungkinkan untuk akses jaringan dari jarak jauh akan menimbulkan masalah yaitu  fragmentation challenge. 

Fragmentation challenge tersebut dapat diselesaikan dengan membuat perangkat yang digunakan dalam home network dapat dikonfigurasi maupun diprogram demi fleksibilitas dalam penggunaan perangkat. Koordinasi yang baik akan meningkatkan kepuasan user dalam penggunaan perangkat. Untuk melengkapi fungsi koordinasi tersebut , teknologi yang saat ini dikembangkan , yaitu  Software Defined Networking (SDN) menjadi sebuah pilihan yang tepat. SDN  memisahkan kontrol dengan fungsi koordinasi dari perangkat yang meneruskan paket – paket.

![SDN-enabled future home network environment](http://i61.tinypic.com/2vskyyq.jpg%5B/IMG%5D)

Pada gambar 1 diatas, diillustrasikan perangkat – perangkat  Consumer Electronic (CE) dari prototype SDN-enabled untuk melayani aplikasi multihome multimedia. Pada Home Enviroment diperlukan sedikit konfigurasi , akan tetapi didalam dan diluar dari rumah, akan diperlukan banyak konfigurasi pada dari manajemen home networking, karena terdapat berbagai macam perangkat yang terhubung secara kabel maupun nirkabel.
	Penggunaan perangkat CE (HomeBox dan ControlBox yang ditunjukkan pada gambar 1) memberikan fleksibilitas yang lebih baik pada konfigurasi perangkat  dan packet control dan membebaskan user untuk mengkonfigurasi secara manual berbagai perangkat maupun menginstall aplikasi software untuk memungkinkan interoperabilitas perangkat.

**2.Persyaratan Aplikasi Future – Home Networking dan Solusinya**
Future Home-Networking memerlukan beberapa kriteria dalam pengaplikasiannya. Berikut akan dijabarkan mengenai beberapa kriteria yang diperlukan dalam pengaplikasian SDN-enabled home Network :

**A. Syarat Fungsional**

Perangkat CE harus memenuhi persyaratan untuk mengatasi masalah fragmentation challenge dan untuk meningkatkan User Experience (UX) dirumah. Persyaratan tersebut antara lain : 

 - **Ekstensibilitas Software centric dengan pemrograman perangkat :**
Tantangan manajemen dan interoperabilitas dalam future home environment dapat diatasi dengan memungkinkan perangkat CE untuk dapat diprogram dan dikonfigurasi secara fleksibel. S. Dengan penggunaan software centric programmability, fleksibilitas dari CE dapay dilakukan, dan hal ini berdampak pada peningkatkan kepuasan home user.

 - **Koordinasi yang Fleksibel dari Beberapa Perangkat:** 
Berbagai perangkat CE yang terhubung dalam Future Home Network akan menimbulkan pentingnya untuk mengembangkan manajemen jaringan dari perangkat – perangkat tersebut, termasuk konfigurasi perangkat yang simpel. Hal tesebut juga memicu hadirnya aplikasi home network yang belum pernah ada sebelumnya. Oleh karena itu , koordinasi yang fleksibel dari  perangkat CE penting untuk manajemen dan inovasi aplikasi pada perangkat home network.
 

 - **Interkoneksi Multi-Home dengan Pertimbangan Privasi :**
 Kebutuhan User akan home networking harusnya tidak terbatas pada satu home network, misalkan saat anggota keluarga terpisah untuk bekerja maupun bersekolah diluar lingkungan rumah. Hal tersebut merupakan sebuah tantangan untuk mengintegrasikan beberapa domain home network karena mungkin beberapa home network tersebut memiliki protokol jaringan yang tidak kompatibel dan resource CE yang berbeda beda.Konfigurasi yang tersentralisasi dari perangkat CE dapat menghemat biaya dan waktu  tetapi di saat yang sama terdapat ancaman terhadap privasi karena konfigurasi memerlukan visibilitas dari paket. Jadi menyembunyikan isi paket adalah hal yang harus dilakukan untuk melindungi privasi. Visibilitas paket merepresentasikan informasi raw level packet yang digunakan untuk koordinasi yang tersentralisasi.

**B.	Keuntungan dalam pengaplikasian perangkat  Home CE berbasis SDN**

Dengan adanya software-centric, ekstensibilitas dan fleksibilitas dari SDN-Enabled home CE devices dapat memberikan beberapa keuntungan bagi user , antara lain : 

 - 	**Berkurangnya Biaya Konfigurasi dan Maintenance** 
 Saat ini , perangkat CE harus dipasang, dan dikonfigurasi secara manual. Dengan adanya future home network yang berbasiskan SDN, perangkat CE yang digunakan dapat dikoordinasikan dengan mudah, dalam pemasangan, konfigurasi maupun maintenance dari alat – alat tersebut. Hal ini berdampak pada semakin murahnya biaya untuk pemasangan dan pemeliharaan perangkat.

 - **Meningkatnya User Experience dengan Aplikasi yang Inovatif**
Dengan adanya software-centrik dari jaringan SDN , fungsi – fungsi dari perangkat CE dapat dikonfigurasi sesuai dengan keinginan user. Selain itu, perangkat CE dapat terinterkoneksi ntuk meningkatkan keamanan, performa, maupun adaptasi multimedia. Sebagai contoh, fungsi keamanan pada home gateway dapat dipisahkan dan dipasang pada perangkat CE untuk memastikan keamanan user.


**3.  Koordinasi Home Networking dengan Perangkat Berbasis SDN**

Pada bagian ini, akan dibahas bagaimana arsitektur dari perangkat CE dan contoh dari koordinasi jaringan berbasiskan SDN

**A.	Persyaratan Home-networking yang Memuaskan**

 **-Ekstensibilitas Perangkat dengan Dataplane yang kaya fitur**
 Digunakan sebuah arsitektur modular untuk meningkatkan ekstensibilitas dari perangkat HomeBox yang berbasiskan software-centric. Komponen software yang terpasang berisikan fungsi dari perangkat yang digunakan. Dataplane maupun forwarding plane dari perangkat homebox mampu memproses paket yang kompleks seperti penyisipan, penghapusan maupun memodifikasi IP header untuk mengintegrasikan beberapa home network.

 **-Home Networking Sesuai dengan Keinginan** 
 Jaringan untuk home application dapat dijalankan sesuai dengan keinginan dari user untuk meningkatkan user experience pada extended home network. Penggunaan SDN mampu menyederhanakan paket flow pada jaringan sesuai dengan keinginan user. Interface antara user dengan perangkat CE disediakan oleh sebuah pesan, yaitu Application description (AD) message.

**- Interkoneksi Multi-Home tanpa Visibilitas Paket**
Untuk menangkal permasalahan pada privasi paket menuju ControlBox, digunakan SDN-based two tier messaging yang di sediakan oleh pihak ketiga yang bisa dipercaya. Informasi mentah dari setiap paket IP pada home network hanya boleh dibuka pada perangkat homebox yang ada pada jaringan yang sama.


**B.	Arsitektur Sistem Home Network Berbasis SDN**

![Arsitektur Sistem SDN home network](http://i58.tinypic.com/f4k5ee.jpg)

Pada gambar 2 diatas, diilustrasikan  interkoneski yang terjadi antara 2 home network dengan menggunakan teknologi SDN. ControlBox berada pada level teratas betanggung jawab untuk konfigurasi dari Homebox untuk mengintegrasikan beberapa home network. Arsitektur dalam perangkat Controlbox mirip dengan Gateway Homebox, hanya perbedaannya ControlBox tidak memiliki data-plane support.

AD message diformat untuk menggambarkan packet flow dari field seperti TCP/IP, pemrosesan dan forwarding paket, dan yang lain.AD message dapat memicu komposisi layanan dari home-network dengan megirimkan signal network preferences untuk sebuah packet flow. Semantik dari aplikasi tingkat tinggi , yang dijabarkan pada AD message di terjemahkan kedalam semantic tingkat rendah atau semantic aplikasi yang lebih spesifik. Sebagai contoh, ControlBox dapat membuat message baru yang memiliki semantic aplikasi spesifik yang kemudian dikirmkan ke Gateway Homeboxes untuk dikonfigurasi. Perangkat Gateway Homebox dapat di tafsirkan sebagai controller SDN dengan skala kecil dengan berbagai macam fitur dataplane. Sedangkan pada sisi hardware dari Gateway HomeBox, flow table menyimpan entri dari setiap flow yang memandu “apa dan bagaimana” untuk memproses paket flow.Hal tersebut akan meningkatkan control secara terperinci dan meningkatkan fleksibilitas kontrol.

**C.	Format Message dan SDN based Messaging**

Dengan adanya AD messaging, home user dapat berinteraksi dengan perangkat SDN.  Dan SDN messaging memungkinkan coordinator untuk berinteraksi dengan semua infastrukturnya yaitu semua perangkat yang dikontrol oleh coordinator tersebut. Gateway homebox dapat dianggap sebagai koordinator dari sebuah inrastruktur dan juga merupakan infrastruktur dari Control Box.

![Control and event message](http://i57.tinypic.com/35hil5g.jpg)

Pada Tabel 1 , dijelaskan beberapa jenis message dalam koordinasi centralized home network. Message tersebut dikelompokkan kedalam 2 katagori yaitu **control message** dan **event message**. Sebuah koordinator mengontrol semua infrastrukturnya dengan control messge , sedangkan infrastrukturnya menggenerate event message ketika ia menerima event. SDN messaging memberikan berbagai keuntungan dalam home networking, salah satunya adalah flow lantency akan kecil karena delay propagasi antar semua perangkat CE sangat kecil.

Untuk keamanan informasi dari home user, Gateway homebox tidak boleh mengirimkan event message , karena message tersebut mungkin menyebabkan bocornya paket – level information, yaitu segment dari IP packet.

Untuk pengaturan yang lebih cepat, AD message menggunakan simple character untuk mendeskripsikan perintah. Message tersebut dapat dilihat pada tabel 2 :

![Applcation Descriptiom Message Format](http://i61.tinypic.com/1zedn47.jpg)


![Ilustrasi integrasi 2 home network dengan SDN messaging](http://i61.tinypic.com/106d35u.jpg)

Pada gambar 3 diatas , dapa dilihat contoh dari SDN messaging antar perangkat CE ,dimana tiga homebox digunakan bersama dengan sebuah Control Box. Gambar tersebut menjelaskan fungsi dari AD message untuk menghubungkan dua home network dimana setiap rumah memiliki masing – masing sebuah perangkat gateway homebox. Langkah – langkah dari interkoneksi tersebut antara lain :

1.	User harus mendeskripsikan IP address dari kedua Gateway homebox (dengan AD message( h=Gateway_HomeBox_I:Gateway_HomeBox_II ) dan jenis link virtual antara keduanya yaitu translasi dari network addressnya (AD message : c=ADDR_TRANS)

2. Untuk kontrol Flow based networking , user dapat mengistruksikan bagaimana sebuah flow akan dikontrol secara spesifik. Sebagai contoh, sebuah flow (AD : f=239.2.2) harus melalui 2 waypoint (AD : d=10.1.1.22:10.1.0.22) sebelum mencapai Gateway HomeBox.

Gateway Homebox II akan secara otomatis melakukan koordinasi dalam home network, tanpa adanya interferensi dari Controlbox. Hal tersebut dilakukan dengan pertukaran SDN message dengan cabang - cabang perangkat Homebox. Controlbox masuk kedalam fungsi jaringan saat menerima AD message, dimana message tersebut ditranslasi kedalam message yang lebih spesifik dan kemudian dikirimkan pada kedua Gateway Homebox untuk konfigurasi virtual link.
Masing – masing Gateway Homebox kemudian mentranslasikan message tersebut dan meng-generate flow entries dalam dataplane masing-masing.

**D.	Skenario Eksperimen dari Visual Sharing** 

Untuk menunjukkan kinerja dari perangkat CE dengan basis SDN terutama pada performa multi home visual sharing, dilakukan sebuah eskperimen yang dapat pada gambar : 
![enter image description here](http://i59.tinypic.com/290tuew.jpg)

Pada gambar 4 diatas diilustrasikan sebuah skenario multi home visual sharing yang dikoordinasikan dengan SDN. Seorang home user melakukan multicast setelah mengirimkan AD message kepada ControlBox. Setelah menerima AD message  (h=Gateway_HomeBox_I: Gateway_HomeBox_II ,c=TUNNEL,f=MCAST_ADDR) tersebut , ControlBox kemudian mengkonfigurasi kedua Gateway Homebox untuk menghubungkan  kedua home network. HomeBox II menkonfigurasi semua Homebox dalam home network II dengan menggunakan AD message yang berbeda (h=Gateway_HomeBox_II, c=ADDR_TRANS, f=MCAST_ADDR, d=Transcoder_HomeBox). Transcoder Homebox disini berfungsi sebagai Transcoder. Jalur end to end pada gambar 4 dibagi kedalam 3 bagian berdasarkan perubahan flow information. Perubahan flow information tersebut terjadi karena perangkat Homebox melakukan translasi alamat.

Media adaptation service dikoordinasi oleh Homebox II , yang kemudian mentransmisikan konten multimedia kepada perangkat nirkabel. Transcoder kemudian mengubah paket video dan meneruskannya ke perangkat wireless mobile (handphone, tablet dll). Trnascoding diasumsikan dilakukan sebelum transcoder meng-generate paket video.

![enter image description here](http://i62.tinypic.com/osr5ef.jpg)

Pada gambar 5 diatas ditunjukkan gambar hasil dari eksperimen dengan prototype perangkat berbasis SDN. Komputer mengirimkan video stream ke set-top box yang berada pada home network yang berbeda. Dengan munggunakan koordinasi berbasis SDN, home user dapat mengintegrasikan beberapa home network dengan mudah walaupun perangkat dan protokol yang berbeda beda.

![enter image description here](http://i61.tinypic.com/2e31n4k.jpg)

Gambar 6 menunjukkan perubahan datarate saat video flow melewati Gateway Homebox I.pada gambar tersebut menjelaskan bahwa perangkat Homebox melakukan flow-based packet processing dan forwarding , dan melakukan packet count monitoring terhadap jaringan. Hasilnya, home user dapat memfasilitasi flexibilitas  dan flow level control terhadap pengaturan home network mereka.  


**4. Kesimpulan** 

Dengan hasil eksperimen yang dilakukan, penggunaan koordinasi yang fleksibel berbasis SDN merupak sebuah solusi yang layak untuk menanggulangi fragmentation challenge pada aplikasi future home networking.



**Reference** : Software-defined Home Networking Devices for Multi-home Visual Sharing, Jinyong Jo et al, August 2014

