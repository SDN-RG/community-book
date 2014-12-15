##SDN integration in LTE mobile backhaul network ##

 *Siti Amatullah Karimah*

**Abstrak**
Tantangan utama pada jaringan mobile masa depan ialah bagaimana meningkatkan *throughput* dalam mendukung meningkatnya trafik jaringan. Software Defined Network merupakan teknologi baru yang diharapkan dapat mengatasi permasalah tersebut, namun sejauh ini SDN hanya di integrasikan pada arsitektur jaringan 4G, tanpa adanya perubahan mendasar. Dalam paper ini dijabarkan mengenai jaringan mobile 4G dimana pendekatan Software Defined Network dapat digunakan untuk mendesain ulang arsitektur saat ini. Dalam paper ini dibahas didalamnya jalur migrasi yang dapat digunakan untuk menjamin terjadinya fase transisi tersebut. Tujuannya bukanlah untuk jangka pendek namun untuk memenuhi kebutuhan *throughput* masa depan dengan membenahi jaringan transport pada *mobile backhaul* dan memindahkan sebagian besar elemen jaringan LTE saat ini ke *cloud*. Pendekatan ini akan menghilangkan adanya *core network* yang dikenal saat ini dan menggantikannya dengan menyederhanakan jaringan akses yang dibentuk oleh *base station* (eNodeB) yang saling terhubung melelui jaringan *backhaul* yang dikendalikan oleh SDN *switch* yang berada di *cloud* yang bekerja bersama dengan elemen jaringan LTE lainnya.

**I.	Introduction**

Diperkirakan bahwa pada tahun 2017 jumlah perangkat *mobile* akan melebihi jumlah keseluruhan penduduk dunia. Dan juga diperkirakan bahwa layanan video akan meningkat signifikan yang berimbas pada permintaan peningkat kapasitas jaringan LTE dengan ukuran yang lebih kecil. Ukuran kecil dari sel LTE ini akan berimbas pada banyaknya jumlah *base station* yang dibutuhkan sehingga membutuhkan jaringan *bakhaul* dengan konektivitas yang lebih tinggi. Peningkatan jumlah *base station* ini berdampak pada kebutuhan akan penyederhanaan jaringan akses. Salah satu solusi penyederhanaan jaringan akses ini ialah menggunakan teknologi SDN. Integrasi dengan teknologi SDN diharapkan dapat meningkatan fleksibilitas yang diperlukan dan manajemen *resource* yang optimal untuk mengakomodasi kebutuhan jaringan *mobile* dimasa depan. 
SDN yang di implementasikan di jaringan *mobile* dinamakan Software Defined Mobile Network (SDMN). Proyek yang sama sebenarnya pernah dilakukan dalam hal pengintegrasian jaringan LTE dan SDN dengan mempertahankan arsitektur LTE yang ada. Yang membedakannya dengan paper ini ialah, paper ini menjabarkan perubahan arsitektur jaringan LTE yang terintegrasi dengan jaringan SDN dengan proses migrasi yang lancar.

**II.	LTE Mobile Networks and SDN Overview**

Jaringan *mobile* terdiri dari layer fisik dan juga logic. Layer fisik dibentuk oleh *switch* jaringan (L2), *router* (L3)dan juga *link* fisik dengan topologi dan teknologi yang berbeda-beda seperti pada gambar 1.

![Gambar 1. Layer fisik dan logic pada jaringan mobile](https://lh4.googleusercontent.com/7244ClMesrjI_4EvBMQ1Tpk5N8yjuLudMIQdVdJzQw=s0 "Gambar 1. Layer fisik dan logic pada jaringan mobile.png")

 - *Logical Layer*
Terdiri dari elemen jaringan (eNodeB, MME, S/P-GW, HSS, dll) yang menjalankan fungsi :
o	*Attachment*
o	*Mobility*
o	*Transport Data* 
o	Implementasi *Control Plane*

 - *Physical layer*
Menyediakan fungsi konektivitas dan transportasi yang dibutuhkan pada *logical layer*.

 - *Access Network*
Terdiri dari sebagian besar eNodeB yang menyediakan *radio access* kepada UE.

 - *Backhaul*
Terdiri dari seluruh *switch* jaringan yang berfungsi mengagregasi trafik dari *access network* dan juga menyedikan konektivitas dengan *core network*.

 - *Network element*
Mengimplementasikan seluruh koneksi *mobility* dan juga fungsi *billing* yang berlokasi di *core network.*

Mobility merupakan *critical function* pada jaringan mobile. Teknologi baru apapun harus dapat memastikan reabilitas dan *low latency* yang pantas pada proses *handover.* *Mobility* di jaringan LTE dilakukan menggunakan metode yang berbeda-beda bergantung pada apakah target eNodeB baru berada pada Tracking Area ID (TAI) yang berbeda atau tidak.  TAI akan berasosiasi dengan Mobility Management Entitiy (MME) yang sama. Pada umumnya fungsi *mobility* dilakukan menggunakan *interface* S1-MME yang telah didefinisikan antara eNodeB dan MME.
Pada skenario yang dibahas pada *paper* ini, fungsi *mobility* dilakukan menggunakan *interface* X2 yang didefinisikan antar eNodeB. Gambar 2 akan mendeskripsikan *control functionality* yang akan mengatur proses *handover* berdasarkan *interface* S1-MME antar *logical element* (eNodeB, MME dan S/P-GW) dimana perubahan pada MME dibutuhkan.

![Gambar 2. Logical element dan mobility control process](https://lh4.googleusercontent.com/BeIlWcN8EEgYp23uzYk9F_XZXXXVbhQ9k-PGCGFf_g=s0 "Gambar 2. Logical element dan mobility control process.png")

Masalah mendasar pada protocol IP dari sudut pandang *mobility* adalah bahwa alamat IP mengidentifikasi node dan akan menyesuaikan IP yang dimiliki berdasarkan IP subnet ia berada. Solusi yang ditawarkan untuk mengatasi hal ini ialah disedikannya *tunneling* pada alamat IP *user* didalam GTP *tunnel* yang dibangun antara eNodeB dan S/P-GW. GTP *tunnel* akan memberikan identitas unik setiap *traffic flow* yang mendapatkan penanganan QoS yangberbeda-beda antara UE dan PDN GW. Traffic Flow Template (TFT) digunakan untuk melakukan *mapping* atau pemetaan trafik ke EPS *bearer* yang sesuai.
GTP Tunneling Endpoint Identifier (TEID) secara jelas akan mengidentifikasi *tunnel endpoint* termasuk didalamnya tiap paket data *user* ,memisahkan trafik berdasarkan *user* dan juga memisahkan *bearer* dari *user* seperti yang terlihat pada Gambar 3.

![Gambar 3. End user bearer dan mapping pada GTP tunnel](https://lh5.googleusercontent.com/BzoL7WZ7Qop5TJS8VVvI2-3Red5D9lBhtsuffzD5GA=s0 "Gambar 3. End user bearer dan mapping pada GTP tunnel.png")

Kapanpun UE berpindah ke eNodeB baru, GTP *tunnel* harus dibangun ulang antara eNodeB baru dengan S/P-GW dimana *inner* *data flow* akan tetap menjaga alamat IP *user* asli (tidak perlu mengubah-ubah alamat IP).
Proses *handover* di inisiasi dan di atur melalui *interface* S1 seperti pada Gambar 4. MME akan tanggap terhadap proses *mobility* dan akan berkomunikasi dengan S/P-GW untuk membangun ulang GTP tunnel antara eNodeB baru dan S/P-GW.

![Gambar 4. Proses handover diontrol dari MME melalui interface S1 dan berkomnikasi dengan S/P-GW untuk membangun ulang GTP tunnel.](https://lh6.googleusercontent.com/YWlQZS8-WAt45OV-_tuHkMSPAiZKKHYwJvKdOtWLkw=s0 "Gambar 4. Proses handover diontrol dari MME melalui interface S1.png")

**A.	SDN Overview**
Telah dipahami bersama bahwa jaringan pada masa depan akan memerlukan kepekaan yang tinggi terhadap layanan-layanan yang muncul dan juga kepekaan terhadap optimasi penggunaan *resource* jaringan. Semua hal ini dapat dicapai dengan bantuan teknologi Software Defined Network (SDN). SDN diperkirakan akan menjadi *key enabler* pada pengembangan infrastruktur jaringan telekomunikasi dalam menghadapi perkembangan jaringan *mobile* masa depan yang mendasari lahirnya Software Defined Mobile Network (SDMN). SDMN pada dasarnya merupakan pendekatan *networking*/jaringan dimana *control plane* akan dipisahkan dari *hardware* jaringan telekomunikasi secara spesifik untuk kemudian diberikan kepada *software application* yang bernama *controller.* Fitur yang ada pada SDMN akan menyederhanakan kerja *router* maupun *switch* dengan cara memindahkan CP ke server tersentralisasi yang bekerja sebagai *controller.* *Controller* memiliki seluruh kontrol jaringan dimana ia dapat mereduksi kongesti dengan menambahkan fungsi *traffic management* dan *optimized resource allocation* yang akan mengalokasi *resource* secara optimal. 
Gambar 5 memperlihatkan interkasi antara *controller* dan *switch-**switch* pada jaringan berbasis SDN. Interkasi ini berdasarkan API yang telah terdefinisikan dengan baik seperti OpenFlow. SDN membuat perangkat jaringan seperti *switch* dan *router* bersifat *“programmable”* atau dapat di program dengan fungsionalitas tertentu. Oleh karenanya SDN diharapkan dapat meningkatkan data *throughput* akibat dari penyederhanaan kerja *switch.* Pendekatan yang dilakukan pada *paper* ini ialah untuk mempersingkat kerja dari perangkat jaringan agar perangkat-perangkattersebut dapat lebih fokus dalam manajemen *forwarding data* (fungsi-fungsi yang biasa dijalankan oleh *router* diambil alih oleh adanya *controller*). 

![Gambar 5. Jaringan berbasis SDN dengan switch-swich yang diatur oleh controller](https://lh3.googleusercontent.com/fidDlje8O8u9qUelCcruY6eIPH02TGzVlyxekfBnig=s0 "Gambar 5. Jaringan berbasis SDN dengan switch-swich yang diatur oleh controller.png")

Teknologi SDN akan memungkinkan beberapa kemungkinan skenario sebagai berikut :
- Pemisahan *individual traffic flow* untuk membagi *resource* yang tersedia dalam mendukung berbagai macam Mobile Virtual network Operator (MVNO).
- Mengoptimalkan proses *re-direction* flow kepada layanan/aplikasi yang spesifik.
- Mengefisienkan manajemen dan penggunaan *resource.*

Walaupun begitu, SDMN diperkirakan akan memiliki permasalah dalam efisiensi *mobility management* dan juga skalabilitas dalam menangani ratusan ribu *flow user* pada tiap *radio access network* (eNodeB).
Sejauh ini dapat diambil kesimpulan bahwa SDMN memilik fungsionalitas tambahan dibanding SDN yang diperuntukkan untuk *fixed network* dimana SDMN akan mengakomodasi kebutuhan tambahan jaringan *mobile* sepeti halnya *mobility management,* efisiensi proteksi terhadap *air interface* dan perangkat *mobile* dari trafik yang tidak diinginkan, dan juga penggunaan *tunneling* pada proses transportasi paket secara berkala. 

**III.	Software Defined Networks Integration in LTE**

Integrasi SDN menjadi SDMN membutuhkan beberapa desain arsitektur sebagai berikut :
•	Lokasi SDMN *controller*
•	MME : Tanggap terhadap *mobility*
•	S/P-GW : mengkontrol jaringan *transport*
•	Distribusi *controller*
•	*single controller*
•	*many controller*
Kedua model distribusi *controller* diatas akan diletakkan dekat dengan *access network* namun memiliki hierarki topologi tersendiri antar *controller* di *access network* namun akan tetap tersentralisasi di *core network*.
	Integrasi antara SDN dan elemen jaringan LTE baik sebagai bagian dari MME mapun S/P-GW harus dapat mengakomodir efisiensi pada proses *handover*. Tujuannya ialah untuk menjaga basis alamat IP UE dan penggunaan SDMN untuk meningkatkan fleksibilitas arsitektur jaringan LTE. Gambar 6 memperlihatkan arsitektur LTE saat ini dimana terdapat beberapa opsi pengintegrasiannya dengan SDN *controller.*

![Gambar 6. Arsitektur jaringan LTE](https://lh4.googleusercontent.com/G3d6CoMR4nv2e0Q3Zw0KYjTNe4-xQDA5j3kGGC-uBA=s0 "Gambar 6. Arsitektur jaringan LTE.png")

Gambar 7 mendeskripsikan salah satu opsi pengintegrasian SDN ke arsitektur LTE. Opsi ini terdisi dari pemisahan (*decoupling*) S/P-GW menjadi *logical* dan *data plane*. Bagian *logical* pada S/P-GW (S/P-GWc) menyediakan alokasi alamat IP kepada UE dan menjalankan TFT kepada *data flow user*. Bagian data plane dari S/P-GW (S/P-GWu) menyediakan terminasi GTP tunnel dan menahan (*anchoring*) GTP *tunnel* selama proses *handover*. Hal ini dibiarkan berlangsung sementara seluruh elemen jaringan dijaga agar tidak terpengaruh terhadap proses *handover* tersebut dan MME tetap dapat berinteraksi dengan S/P-GWc.

![Gambar 7. Integrasi SDN dan S/P-GW](https://lh6.googleusercontent.com/8K91yQKyhdsHMLwQYe8ZlGgK_ZRgsQ8DkGPa1HghHw=s0 "Gambar 7. Integrasi SDN dan LTE.png")

Gambar 8 menunjukkan integrasi SDN dan arsitektur LTE opsi kedua, dimana opsi ini akan mengizinkan SDN *controller* menerima *mobility event* langsung dari MME agar dapat langsung menerapkan *rule* baru pada *node switching* untuk dapat mengoptimasi *routing path*/jalur perutean. 

![Gambar 8. Integrasi SDN dan MME](https://lh6.googleusercontent.com/XOBgEa2YwfRAOBYObhI9Hf_AGxQt7FPQJdRbHLBWdw=s0 "Gambar 8. Integrasi SDN dan MME.png")

**IV.	Migration Path of SDN Integration in LTE**

Integrasi fungsionalitas SDN *controller* dan MME menyediakan integrasi yang mulus dalam jangka panjang namun menjadi solusi buruk bagi jaringan *mobile* SDN saat ini karena keharusannya menambahkan kebutuhan-kebutuhan spesifik jaringan *mobile* dimana *data plane* harus dioptimasi untuk kecepatan tinggi dan pemrosesan yang dilakukan pada *flow-level* (menggunakan OpenFlow). Pada SDMN, *control plane* dipisahkan dari elemen dasar jaringan ke server yang tersentralisasi. Maka, pada proses migrasi ini, tujuannya ialah memindahkan *controller* dan fungsionalitas S/P-GW kedalam elemen jaringan yang sama dengan MME. Oleh karenanya, fungsionalitas S/P-GW menjadi terhapuskan dan digantikan oleh SDN berbasis jaringan *packet switched*. Penambahan ini akan menambahkan fleksibilitas jaringan dalam menerima trafik dengan *throughput* yang besar, mengoptimalkan manajemen jaringan dan memungkinkan dilakukannya *traffic engineering*. Gambar 9 memperlihatkan integrasi mobility dengan SDN *controller* yang dimasukan ke dalam bagian dari elemen jaringan MME.

![Gambar 9. Integrasi SDN dan MME](https://lh6.googleusercontent.com/4nt7ooLPtEwcobPUKu0Kl512FOimN6kPLpYBZXwfIg=s0 "Gambar 9. Integrasi SDN dan MME.png")

*Mobility* merupakan *critical aspect* dari jaringan mobile yang membutuhkan fungsionalitas tersendiri secara spesifik pada elemen jaringan. Hubungan yang erat antara SDN *controller* dan MME, dalam fungsi waktu akan mengefisienkan penangan proses *mobility* yang dilakukan oleh SDN *controller.* Integrasi ini menyediakan meanajemen *handover* yang efisien pada SDMN. 
OpenFlow *controller* akan menambahkan dan mengurangi *flow* dari *flow table* sesegera mungkin ketika *handover* terjadi untuk kemudian dilaporkan ke MME. Masukan pada **flow* table* diantarany *Packet Header* yang berfungsi mendefinisikan *flow,* *action* untuk mendefinsikan pemrosesan paket dan juga statistiknya. Disamping integrasi MME dan SDN *controller* paper ini pun menggambarkan penggunaan teknologi 802.1ad untuk melakukan proses *double tagging* pada *switch* Ethernet seperti pada Gambar 10.

![Gambar 10. Ethernet 802.1ad – struktur VLAN](https://lh5.googleusercontent.com/0LMZhYWn1qxsu5sNQlnmIdadUT02PgHbTDod2lTSjQ=s0 "Gambar 10. Ethernet 802.1ad – struktur VLAN.png")

*Double tagging* ini akan mengizinkan penggunan lebih dari 2^12 *service tag* sebagai *outer tunnel* dan 2^12 *customer tag* sebagai *inner tunnel.* (total 2096 *inner* dan *outer tunnel*). *Outer* VLAN ini dapat digunakan untuk membangun *tunnel* antar eNodeB dan *router* IP yang berlokasi pada *segmen* Ethernet yang sama menyediakan akses ke internet publik. *Outer* VLAN yang dibangun dari eNodeB dapat dimasukkan ke dalam Mobile Virtual Network Operator (MVNO) yang berbeda pada area dengan 400 eNodeB. 

![Gambar 11. VLAN tunneling antara eNodeB dan IP router](https://lh6.googleusercontent.com/qgGGNTlDYxYVKypgMpEv7Du3Q_eHaM1AKHouEzcDqw=s0 "Gambar 11. VLAN tunneling antara eNodeB dan IP router.png")

Integrasi fungsionalitas *controller* dengan MME dan S/P-GW kedalam satu elemen jaringan yang sama akan menyederhanakan fungsionalitas jaringan. Hal ini akan menyebabkan gangguan selanjutnya dimana *data plane* diatur dari satu elemen MME/*Controller.* Evolusi menuju arsiterktur ini dapat dilakukan dimana MME akan menjaga *interface*nya dalam menerima *signaling* dari *interface* S1-MME. MME akan menangani proses standar (yang biasa dilakukannya) dan membangun GTP *tunnel* antara eNodeB lama dan S/P-GW. Secara bersamaan MME akan memiliki fungsionalitas SDN yang akan membangun komunikasi antara model baru eNodeB dan IP *router* secara langsung pada layer 2 tanpa adanya GTP *tunneling*. Pada skenario yang dipaparkan, pada MME yang sama, ketika menerima *signaling* dari eNodeB berbasis SDN melalui *interface* S1-MME akan membangun koneksi dengan switch SDN yang diterminasi melalui L2 menggunakan *interface* TUN.
*Networking stack* yang digunakan pada *data plane* diperlihatkan pada Gambar 12. *Radio layer* di terminasi pada eNodeB dimana GTP digunakan mengikuti S-GW dan P-GW yang menyediakan jembatan kepada internet publik.

![Gambar 12. User Plane Networking stack](https://lh3.googleusercontent.com/kHFXdweGKdiqC6zQfNfldsfTqr6oyZVIcj42NGLk3w=s0 "Gambar 12. User Plane Networking stack.png")

Penggunaan 802.1ad pada *backhaul* dan integrasi antara MME dan SDN *controller* mengizinkan penghapusan GTP *tunnel*. Hal ini akan menghasilkan penyederhanaan *stack* pada eNodeB yang menterminasi *radio layer* melalui keseluruhan jaringan pada *backhaul* seperti yang digambarkan pada Gambar 13. Maka dari itu, S/P-GW disederhanakan setelah menghilangkan GTP dan terdiri dari Ethernet *switch* sederhana dan IP *router* yang menuju internet publik. Pada arsitektur ini *mobility* dilakukan oleh SDN *controller*. 

![Gambar 13. User Data Plane networking stack](https://lh3.googleusercontent.com/XcqupSwLp8NvVJR7TfUagpeDnv4IhR-jURlvUIY7Sw=s0 "Gambar 13. User Data Plane networking stack.png")

Arsitektur ini mengantarkan pada optimasi pada jaringan transport dan juga menjadikan *control plane* memiliki skalabilitas yang akan mengkonvergensikan kedalam satu elemen jaringan MME dengan fungsionalitas SDN *controller* yang tertanam didalamnya. 
MME pada akan menjaga *networking stack* mereka seperti yang terlihat pada gambar 14. Perubahan pada *data plane* terjadi bersamaan dengan penjagaan proses *signaling* pada MME untuk mendukung eNodeB lama agar dapat melakukan transisi yang baik dan lancar. MME akan dapat mengatur elemen jaringan yang dimilikinya seperti eNodeB dan S/P-GW namun integrasi dengan SDN akan menjadikan fungsi GTP dihilangkan.

![Gambar 14. Signaling networking stack](https://lh3.googleusercontent.com/lLP_TVL2PZZAtTbPOa6f0vySYe8qaVV-STyKW9-w_F8=s0 "Gambar 14. Signaling networking stack.png")

Elemen-elemen pada jaringan mobile LTE pada umumnya berlokasi pada *core network,* dimana pengintegrasian SDN dengan topologi seperti ini membuat jaringan tidak memilki skalabilitas. Maka dari itu, pada arsitektur yang ditawarkan oleh *paper* ini ialah membuat agar elemen-elemen jaringan dapat berlokasi sedekat mungkin dengan eNodeB di *backhaul*. Hal ini membuat *access network* bersifat *standalone* dimana koordinasi terhadap beberapa *acces network* akan dilakukan menggunakan *database* yang tersentralisasi dan *handover* antar MME yang berada pada tiap *access network* tetap dilakukan menggunakan *interface* S1.
*Signaling networking stack* yang dikenal akan tetap sama dalam berinteraksi dengan elemen jaringan LTE yang lama seperti S/P-GW dan juga MME yang ada pada jaringan serti yang terlihat pada Gambar 15.

![Gambar 15. Signaling network stack](https://lh6.googleusercontent.com/w-wPmlUcpJxyj9EV5POUHp9F5-LZl0EDADqODRiUVPw=s0 "Gambar 15. Signaling network stack.png")

Dalam hal penyederhanaan arsitektur jaringan LTE yang terintegrasi dengan SDN, maka permasalahan skalabilitas haruslah dipecahkan. Solusi yang dipaparkan adalah dengan memindahkan elemen jaringan LTE pada arsitektur baru dimana MME akan digabungkan dengan SDN *controller* pad *access network* seperti yang terlihat pada Gambar 16.

![Gambar 16. Arsitektur LTE baru](https://lh6.googleusercontent.com/qDhBU3k43SJ36DneYdS0hlPdTfyTragrkMNNN9If8DI=s0 "Gambar 16. Arsitektur LTE baru.png")

Arsitektur ini mengizinkan penggunaan 802.1ad untuk tiap *access network* dalam menyediakan penyederhanaan arsitektur LTE dimana beberapa pecahan-pecahan fungsi jaringan akan diserahkan kepada *virtual operator* menggunakan VLAN.

**V.	Conclusions**

Paper ini memaparkan integrasi antara teknologi LTE dengan SDN dengan membuat penggabungan fungsionalitas antara MME dengan SDN *controller*. Hal ini mengenalkan arsitektur baru dimana jaringan transport akan disederhanakan dan akan berbasis *switch.* *Control plane* pun akan disederhanakan dengan melakukan penggabungan elemen jaringan LTE seperti MME, S/P-GW  yang digabungkan ke dalam satu komponen jaringan. Selain itu, elemen jaringan LTE baru ini, dimana terdapat fungsi SDN *controller* didalamnya dapat di virtualisasikan dan beberapa *instance* nya dapat di diambil oleh *data center* untuk melakukan penyederhanaan pada *data plane.* Pengimplementasian sistem ini telah diuji cobakan dimana *source code* yang digunakan akan tersedia untuk pengembangan selanjutnya. Hasil dari uji coba menunjukkan kecocokan pengintegrasian elemen jaringan LTE dengan SDN *controller* yang dijalankan diatas *cloud* dapat meningkatkan *throughput* setelah mengurangi adanya *overhead* dan mencegah terjadinya fragmentasi ketika GTP-U dihilangkan dari *user plane* antara eNodeB dan S/P-GW.


**REFERENSI**
---------

Costa-Requena, J. , SDN integration in LTE mobile backhaul network, IEEE International Conference Information Networking (ICOIN), Phuket 2014
