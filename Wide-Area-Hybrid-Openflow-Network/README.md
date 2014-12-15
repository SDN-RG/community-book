Deployment and Operation of Wide Area Hybrid Openflow Network
------------------------------------------------------------------------

 
*Galih Nugraha Nurkahfi*
 
 
**Abstrak** Pendekatan untuk membangun hybrid wide-area OpenFlow network adalah sesuatu hal yang penting untuk pembangunan secara bertahap dari teknologi OpenFlow ke WAN yang sudah eksis sebelumnya, karena sangat tidak praktis untuk membangun wide-area network dengan teknologi OpenFlow secara sekaligus. Disatu sisi yang lain, desain, deployment dan operasional sejenis hybrid OpenFlow network sering kali dibuat dengan intuisi tanpa pengetahuan teknis yang mendalam, dalam tulisan ini dibahas aspek teknis dan arsitektur dari hybrid OpenFlow network secara sistematis, berdasarkan pengalaman penulis paper dalam membangun testbed SDN bernama RISE (Research Infrastructure for large-Scale network Experiments)  diatas JGN-X, sebuah testbed jaringan komputer berskala nasional di Jepang.

I. Introduction
------------

Teknologi SDN mempunyai beberapa kemampuan, yang menjadi keunggulan sehingga menjadikan teknologi ini menjadi teknologi jaringan masa depan.
     
 - Teknologi SDN menyediakan fleksibilitas dalam mengatur jaringan,          teknologi ini menyediakan kemampuan untuk mengontrol packet forwarding dalam perangkat-perangkat switch dalam satu jaringan komputer yang berada dalam satu administrasi, secara terpusat melalui satu buah controller yang terpusat.
 - Teknologi SDN juga menyediakan fitur untuk membuat slice logical network yang terpisah satu sama lain,  yang mengakomodasi keberadaan production network dan experimental network dalam satu physical network.
 - Kedua fitur diatas menyediakan kebutuhan bagi researcher dan developer dalam bidang *Future Internet*(FI) dan *New Generation Network*(NwGN) 
 - Tulisan ini menceritakan development dan deployment dari Research Infrastructure for *Large Scale Network Experiment*(RISE) yang dilakukan oleh beberapa institusi pendidikan dan riset di Jepang.

Awalnya establishing testbed ini dilakukan di *campus network*, karena beberapa alasan:

 - Kampus menyediakan *environment* percobaan yang lebih real dibandingkan dengan hanya enviromment laboratorium, dengan keberadaan *traffic real* yang ada.
 - Skala jaringan kampus tidak terlalu besar sehingga lebih mudah mensetup dan mengoperasikannya.

Namun dari sudut pandang deployment teknologi NwGN, network kampus memiliki limitasi dari segi skala, maka experimen ini dijalankan di lingkungan WAN. Maka selanjutnya testbed ini dibangun diatas JGN-X, yang merupakan pengembangan dari JGN network(japan gigabit network) yaitu testbed jaringan skala besar milik National Institute of Information and Communications Technology (NICT), yang dijalankan mulai tahun 1999, JGN-X sendiri merupakan suatu testbed jaringan yang khusus digunakan untuk mengembangkan NwGN. 

![1. Roadmap pengembangan JGN](http://www.jgn.nict.go.jp/english/info/images/roadmap.png)


 Mengapa RISE memanfaatkan keberadaan testbed JGN-X ini, ada dua alasan untuk hal ini:
 

 - RISE Tidak usah membeli akses WAN yang mahal untuk ‘hanya’ membangun openflow testbed.
 - Menjalankan hybrid network untuk mensimulasikan transisi dari traditional network ke OFN, dimana openflow network dibangun diatas traditional network.


Selanjutnya dari hasil deployment dan operation, maka pihak *developer*   	RISE mensharing pengetahuannya kepada pengelola REN(Research education network) yang lain. Untuk hal yang terkait dengan operasional testbed, Operasional dilakukan dengan memberikan network slice berbasis flowvisor dan controller sendiri-sendiri kepada para *researcher*, untuk menjalankan percobaan layanan.



II. Motivasi untuk mendeploy dan menjalankan RISE
---------------------------------------------

Berikut adalah beberapa isu yang menjadi motivasi untuk menjalankan testbed RISE

 - Keamanan
Membangun testbed yang digunakan bersama oleh *researcher* di kampus harus memperhatikan aspek *security*, mengingat banyak orang yang mengakses sistem untuk berbagai macam kebutuhan development.
 - *Service continuity*
Teknologi baru yang dibangun, biasanya tidak *battle proven* untuk skala besar dan jangka waktu panjang,  teknologi *testbed* yang dideploy harus dipastikan bisa dijalankan secara berkelanjutan di sebuah environment jaringan berskala besar.
 - Sisi ekonomis
Membangun testbed memerlukan pembelian perangkat-perangkat baru yang mensupport teknologi SDN, dalam hal ini *joint research* dengan perusahaan yang memproduksi perangkat jaringan untuk menghemat cost, dengan memberikan benefit bagi kedua belah pihak, pihak vendor mendapatkan keuntungan dengan terujinya perangkat yang mereka produksi secara *scientific*sedangkan pihak researcher mendapatkan perangkat riset secara 'gratis'.
 - Keunggulan Openflow
Menghadirkan fleksibilitas dan kemudahan dalam melakukan implementasi infrastruktur SDN diatas *traditional network*, memudahkan manajemen dari satu titik pusat kontrol dan keberadaan fitur *network slicing* memudahkan virtualisasi jaringan untuk berbagai macam keperluan.

Percobaan *testbed* harus bisa dilakukan dalam skala yang lebih besar dari sekedar network campus, karena kelak teknologi ini, memang akan dijalankan diatas jaringan berskala besar, seperti internet, maka dari itu dibuatlah testbed dengan skala wide area network RISE.


III. Kebutuhan teknologi untuk mendeploy Openflow network di wide area network
---------------------------------------------------------------

Berikut adalah apa saja yang diperlukan untuk mendeploy OFN di skala WAN

 - Fungsi dari openflow
Fungsi dasar dari openflow adalah menjalankan kontrol *packet forwarding* yang sangat fleksibel, namun disatu sisi openflow adalah teknologi yang masih terus berkembang dan memiliki berbagai macam isu, dalam studi kasus pertama yang dilakukan diatas OFN WAN adalah advance traffic engineering. Setiap host di edge OFN menggenerate packet dan *traffic engineeringnya* dioperasikan oleh *Openflow protocol*.
Tiap users di *edge*, diberikan akses satu atau lebih physical ports seperti yang terlihat pada gambar, tiap user diberikan satu *identifier* di layer 2 dengan menggunakan Vlan ID 802.1q vlan tags, dengan keberadaan vlan tags ini kita bisa membuat *slicing network* berdasarkan *users*. Vlan tags ini diatur oleh administrator OFN.
![network slicing di OFN](http://i57.tinypic.com/2ikp2sw.jpg)
 - Teknik deployment pada openflow network
Pada paper ini disebut suatu istilah yaitu *Existing virtual network*(EVN) untuk sebuah virtual network yang mengakomodasi OFN, dengan kata lain ini adalah *tunnel* yang dibuat diatas traditional physical wan untuk menjalankan OFN. Secara teknis tunnel ini yang akan membungkus frame layer 2 dari openflow switch di edge ke openflow switch lain di edge tanpa ada modifikasi frame etherent. Ada berbagai macam teknologi untuk menjalankan teknologi wide area ethernet ini, bisa MPLS *based* atau Ethernet  tunneling based, ip based dan lain sebagainya. Topologi tunnel yang dibuat untuk melakukan separasi adalah sebagai berikut, dimana antar satu buah Openflow switch dibuat satu buah tunnel-tunnel untuk memisahkan traffic user, hal ini untuk mencegah mac address learning yang terlalu banyak, diagramnya bisa dilihat pada  dua gambar di bawah ini.
![enter image description here](http://i62.tinypic.com/mwzlmu.jpg)
![Tunnels no MAC address learning](http://i62.tinypic.com/350vz7n.jpg)
OF packet kemudian diberi identitas tunnel dengan tiga kemungkinan cara seperti terlihat pada gambar dibawah ini.

	![Tunneling di testbed](http://i59.tinypic.com/2rc03ft.jpg)

 - Wide area Ethernet
Ada berbagai macam teknik untuk menerapkan teknologi wide area ethernet, yaitu:

	i. 802.1q Tagged VLAN
	Menambahkan 12 bit vlan tag pada header Ethernet, ini adalah implementasi vlan yang umumnya diterapkan untuk segmentasi jaringan virtual .
	
	ii.	802.1ad Q-in-Q
	Melakukan tagging vlan pada frame yang sudah ditag, jadi dalam satu header frame ada dua buah tag vlan, yaitu tag outer dan tag inner.
	
	iii.	Mac-in-Mac
	Enkapsulasi sebuah frame dengan header frame yang lain.
	
	iv.	EoMPLS
	Ethernet over MPLS, MPLS Layer 2 atau disebut juga *virtual private wire services*.

Dibawah ini adalah perbandingan antara teknologi-teknologi *wide area ethernet*, berdasarkan kondisi apakah user dapat menggunakan vlan tagging atau tidak dan juga berdasarkan apakah EVN menjalankan MAC *learning* atau tidak.
![Perbandingan teknologi wide area ethernet](http://i62.tinypic.com/2cse0s6.jpg) 

IV. Desain dan deployment RISE
--------------------------

 - Desain RISE
Teknologi Ethernet untuk EVN, menggunakan Q-in-Q, 1 vlan untuk mengidentifikasi users ports dan satu lagi untuk mengidentifikasi tunnel ID antara 1 buah switch OF ke EVN switch.

	![Desain RISE](http://i60.tinypic.com/3509tvr.jpg)
	

 - Arsitektur dasar RISE
Dua buah OF switch dan satu switch biasa pada setiap site di JGN, satu dari dua OFS disebut edge openflow(E-OFS) dan yang lain disebut distribution Openflow switch(D-OFS)

 ![Arsitektur dasar RISE](http://i59.tinypic.com/4izgw6.jpg)
E-OFS menyediakan koneksi langsung ke user dan menyediakan vlan tagging per user port, pada D-OFS dilakukan vlan tagging sekali lagi dan terbentuklah tunnel antar user domain antar site.

 ![enter image description here](http://i59.tinypic.com/28sbw38.jpg)
 

 - Topologi Rise
Ini adalah topologi RISE diatas JGN, didalam topologi ini, hanya menampakan OFN di edge saja, dari sini bisa kita lihat site RISE ada diberbagai kota-kota yang ada di jepang, dan satu site lagi ada di Los Angles Amarika serikat.

![Topologi RISE](http://i57.tinypic.com/2qun5w0.jpg)


**VI. Kesimpulan**

Dalam paper ini dikemukakan design, teknologi dan pengembangan RISE,  wide area OpenFlow network testbed
Pengembangan testbed selanjutnya :
   

 - Penggunaan MPLS untuk teknologi Tunneling di WAN.
 - Peningkatan Penggunaan dari masing-masing slices
 - OpenFlow testbed inter-connections


**Referensi**

Yoshihiko Kanaumi, Shu-ichi Saito, Eiji Kawai, Deployment and Operation of Wide-area hybrid Openflow Networks, IEEE/IFIP 4th Workshop on Management of the Future Internet, Maui 2012
 
