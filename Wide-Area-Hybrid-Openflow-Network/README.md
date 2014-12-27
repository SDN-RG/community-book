*Deployment and Operation of Wide Area Hybrid Openflow Network*
------------------------------------------------------------------------

 
*Galih Nugraha Nurkahfi*
 
 
**Abstrak** Pendekatan untuk membangun *hybrid wide-area OpenFlow network* adalah sesuatu hal yang penting untuk pembangunan secara bertahap dari teknologi OpenFlow ke WAN yang sudah eksis sebelumnya, karena sangat tidak praktis dan efisien untuk membangun *wide-area network* dengan teknologi OpenFlow secara sekaligus. Disatu sisi yang lain, desain, *deployment* dan operasional sejenis *hybrid OpenFlow network* sering kali dibuat dengan intuisi tanpa pengetahuan teknis yang mendalam, dalam tulisan ini dibahas aspek teknis dan arsitektur dari *hybrid OpenFlow network* secara sistematis, berdasarkan pengalaman penulis paper dalam membangun *testbed* SDN bernama RISE (*Research Infrastructure for large-Scale network Experiments*)  diatas JGN-X, sebuah *testbed* jaringan komputer berskala nasional di Jepang.

I. *Introduction*
------------

Teknologi SDN mempunyai beberapa kemampuan, yang menjadi keunggulan sehingga menjadikan teknologi ini menjadi teknologi jaringan komputer masa depan.
     
 - Teknologi SDN menyediakan fleksibilitas dalam mengatur jaringan,          teknologi ini menyediakan kemampuan untuk mengontrol *packet forwarding* dalam perangkat-perangkat *switch* dalam satu jaringan komputer yang berada dalam satu administrasi, secara terpusat melalui satu buah *controller* yang terpusat.
 - Teknologi SDN juga menyediakan fitur untuk membuat *slice logical network* yang terpisah satu sama lain,  yang mengakomodasi keberadaan *production network* dan *experimental network* dalam satu p*hysical network.*
 - Kedua fitur diatas menyediakan kebutuhan bagi *researcher* dan *developer* dalam bidang *Future Internet*(FI) dan *New Generation Network*(NwGN) 
 - Tulisan ini membahas *development* dan *deployment* dari *Research Infrastructure for Large Scale Network Experiment*(RISE) yang dilakukan oleh beberapa institusi pendidikan dan riset di Jepang.

Awalnya *establishing* testbed ini dilakukan di *campus network*, karena beberapa alasan:

 - Kampus menyediakan *environment* percobaan yang lebih *real* dibandingkan dengan hanya enviromment laboratorium, dengan keberadaan *traffic real* yang ada.
 - Skala jaringan kampus tidak terlalu besar sehingga lebih mudah dibuat dan dioperasikan.

Namun dari sudut pandang *deployment* teknologi NwGN, *network* kampus memiliki keterbatasan dari segi skala, maka selanjutnya eksperimen ini dijalankan di lingkungan WAN yang mempunyai skala lebih besar, yaitu dibangun diatas JGN-X, yang merupakan pengembangan dari JGN *network*(*japan gigabit network*) yaitu *testbed* jaringan skala besar milik *National Institute of Information and Communications Technology* (NICT), yang dijalankan mulai dari tahun 1999, JGN-X sendiri merupakan suatu *testbed* jaringan yang khusus digunakan untuk mengembangkan NwGN. 

![1. Roadmap pengembangan JGN](http://www.jgn.nict.go.jp/english/info/images/roadmap.png)


 Mengapa RISE memanfaatkan keberadaan *testbed* JGN-X ini, ada dua alasan untuk hal ini:
 

 - Pihak *developer testbed* RISE Tidak usah membeli akses WAN yang mahal untuk ‘hanya’ membangun openflow *testbed*.
 - Adanya keperluan untuk menjalankan *hybrid network* untuk mensimulasikan transisi dari *traditional network* ke OpenFlow *network*(OFN), dalam situasi ini openflow *network* dibangun diatas *traditional network* yang sebelumnya sudah ada.


Selanjutnya dari hasil *deployment* dan operasi, maka selanjutnya pihak *developer* RISE membagi pengetahuan yang diperolehnya kepada pengelola REN(*Research education network*) yang lain. Untuk hal yang terkait dengan operasional *testbed*, Operasional dilakukan dengan memberikan *network slice* dan controller masing-masing kepada para *researcher* yang mempergunakan *testbed* ini, untuk menjalankan percobaan layanan.



II. Pertimbangan-pertimbangan untuk *mendeploy* dan menjalankan RISE
---------------------------------------------

Berikut adalah beberapa hal yang menjadi pertimbangan-pertimbangan untuk menjalankan *testbed* RISE

 - Keamanan
Membangun *testbed* yang digunakan bersama oleh para *researcher* di kampus harus memperhatikan aspek *security*, mengingat banyak orang yang mengakses sistem untuk berbagai macam kebutuhan *development*.
 - *Service continuity*
Teknologi baru yang dibangun, biasanya belum bisa dibuktikan kehandalannya untuk penerapan skala besar dan dalam jangka waktu gyanpanjang,  teknologi *testbed* yang *dideploy* harus dipastikan bisa dijalankan secara berkelanjutan di sebuah *environment* jaringan berskala besar.
 - Sisi ekonomi
Membangun *testbed* memerlukan pembelian perangkat-perangkat baru yang mendukung teknologi SDN, dalam hal ini *joint research* dengan perusahaan yang memproduksi perangkat jaringan untuk menghemat pengeluaran dana, bisa dilakukan, cara ini akan memberikan keuntungan bagi kedua belah pihak, pihak *vendor* mendapatkan keuntungan dengan terujinya perangkat yang mereka produksi secara *scientific*sedangkan pihak *researcher* mendapatkan perangkat untuk keperluan riset secara 'gratis'.
 - Keunggulan Openflow
Menghadirkan fleksibilitas dan kemudahan dalam melakukan implementasi infrastruktur jaringan berbasis SDN diatas *traditional network*, memudahkan manajemen dari satu titik pusat kontrol dan keberadaan fitur *network slicing* memudahkan virtualisasi jaringan untuk berbagai macam keperluan.

Pembangunan*testbed* harus bisa dilakukan dalam skala yang lebih besar dari sekedar *network* kampus, karena kelak teknologi ini, memang akan dijalankan diatas jaringan berskala besar, seperti internet ataupun WAN, maka dari itu dibuatlah *testbed* dengan skala *wide area network* yaitu RISE.


III. Kebutuhan teknologi untuk membangun Openflow *network* di wide area *network*
---------------------------------------------------------------

Berikut adalah apa saja yang diperlukan untuk membangun OpenFlow *network* dalam skala WAN

 - Fungsi dari openflow
Fungsi dasar dari openflow adalah menjalankan kontrol *packet forwarding* yang sangat fleksibel, namun disatu sisi openflow adalah teknologi yang masih terus berkembang dan memiliki berbagai macam masalah, dalam studi kasus pertama yang diujicobakan diatas OpenFlow *network* berskala WAN adalah *advance traffic engineering*. Setiap host di edge OFN menggenerate packet dan *traffic engineeringnya* dioperasikan oleh *Openflow protocol*.
Tiap *users* di *edge*, diberikan akses satu atau lebih *physical ports* seperti yang terlihat pada gambar, tiap *user* diberikan satu *identifier* di *layer* 2 dengan menggunakan Vlan ID 802.1q vlan *tags*, dengan keberadaan vlan *tags* ini kita bisa membuat *slicing network* berdasarkan *users*. Vlan *tags* ini diatur oleh administrator OFN.
![network slicing di OFN](http://i57.tinypic.com/2ikp2sw.jpg)
 - Teknik deployment pada openflow network
Pada paper ini disebut suatu istilah yaitu *Existing virtual network*(EVN) untuk sebuah *virtual network* yang mengakomodasi OpenFlow *network*, dengan kata lain ini adalah *tunnel* yang dibuat diatas *traditional physical* WAN untuk menjalankan OpenFlow Network. Secara teknis *tunnel* ini yang akan membungkus *frame layer* 2 dari OpenFlow *switch* di *edge* ke OpenFlow *switch* lain di edge yang bersebrangan tanpa ada modifikasi *frame* etherent. Ada berbagai macam teknologi untuk menjalankan teknologi *wide area ethernet* ini, MPLS *based* atau Ethernet  *tunneling based, ip based* dan lain sebagainya. Topologi *tunnel* yang dibuat untuk melakukan pemisahan adalah sebagai berikut, dimana dalam satu buah OpenFlow *switch* dibuat satu buah *tunnel* untuk memisahkan *traffic* antar *user*, hal ini untuk mencegah MAC *address learning* yang terlalu banyak, diagramnya bisa dilihat pada  dua gambar di bawah ini.
![enter image description here](http://i62.tinypic.com/mwzlmu.jpg)
![Tunnels no MAC address learning](http://i62.tinypic.com/350vz7n.jpg)
OpenFlow *packet* kemudian diberi identitas *tunnel* dengan tiga kemungkinan cara seperti terlihat pada gambar dibawah ini.

![Tunneling di testbed](http://i59.tinypic.com/2rc03ft.jpg)

 - *Wide area Ethernet*
Ada berbagai macam teknik untuk menerapkan teknologi wide area ethernet, yaitu:

	i. 802.1q *Tagged* VLAN
	Menambahkan 12 bit vlan tag pada header Ethernet, ini adalah implementasi vlan yang umumnya diterapkan untuk segmentasi jaringan virtual .
	
	ii.	802.1ad Q-in-Q
	Melakukan tagging vlan pada *frame* yang sudah ditag, jadi dalam satu *header frame* ada dua buah tag vlan, yaitu *tag outer* dan *tag inner*.
	
	iii.	Mac-in-Mac
	Enkapsulasi sebuah *frame* dengan *header frame* yang lain.
	
	iv.	EoMPLS
	*Ethernet over* MPLS, merupakan teknologi (*multi protocol label switching*)MPLS Layer 2 atau disebut juga *virtual private line services*(VPLS).

Dibawah ini adalah perbandingan antara teknologi-teknologi *wide area ethernet*, berdasarkan kondisi apakah seorang *user* dapat menggunakan vlan *tagging* atau tidak dan juga berdasarkan apakah EVN menjalankan MAC *learning* atau tidak.
![Perbandingan teknologi wide area ethernet](http://i62.tinypic.com/2cse0s6.jpg) 

IV. Desain dan deployment RISE
--------------------------

 - Desain RISE
Teknologi Ethernet untuk EVN, menggunakan Q-in-Q, 1 vlan untuk mengidentifikasi *users ports* dan satu lagi untuk mengidentifikasi *tunnel* ID antara 1 buah OpenFlow *switch* ke EVN *switch*.

![Desain RISE](http://i60.tinypic.com/3509tvr.jpg)
	

 - Arsitektur dasar RISE
Dalam arsitektur dasar RISE, terdapat buah OpenFlow *switch* dan satu buah *switch* tradisional pada setiap *site* di *testbed* JGN, satu dari dua OpenFlow *switch* disebut *edge* OpenFlow *switch*(E-OFS) dan yang lain disebut *distribution Openflow switch*(D-OFS)

 ![Arsitektur dasar RISE](http://i59.tinypic.com/4izgw6.jpg)
E-OFS menyediakan koneksi langsung ke *user* dan menyediakan vlan *tagging* per user *port*, *frame* yang keluar dari E-OFS dan masuk ke D-OFS diberikan vlan *tagging* sekali lagi dan terbentuklah *tunnel* antar *user domain* antar *site* yang berbeda.

 ![enter image description here](http://i59.tinypic.com/28sbw38.jpg)
 

 - Topologi Rise
Berikut adalah gambar topologi RISE yang dibangun diatas *testbed* JGN-X, gambar topologi ini, hanya menampakan OpenFlow network di sisi *edge* saja, dari sini bisa kita lihat *site-site* RISE ada diberbagai kota-kota yang ada di jepang, dan satu *site* lagi ada di Los Angles Amarika serikat.

![Topologi RISE](http://i57.tinypic.com/2qun5w0.jpg)


V. Kesimpulan
--------------------------


Dalam paper ini dikemukakan design, teknologi dan pengembangan RISE,  wide area OpenFlow network testbed
Pengembangan testbed selanjutnya :
   

 - Penggunaan MPLS untuk teknologi *Tunneling* di WAN.
 - Peningkatan Penggunaan dari masing-masing *slices* jaringan SDN.
 - OpenFlow *testbed inter-connections*.


VI. Referensi 
--------------------------

[Yoshihiko Kanaumi, Shu-ichi Saito, Eiji Kawai, "*Deployment and Operation of Wide-area hybrid Openflow Networks*", in IEEE/IFIP 4th Workshop on Management of the Future Internet, 2012](http://ieeexplore.ieee.org/xpl/articleDetails.jsp?tp=&arnumber=6212040)
 
