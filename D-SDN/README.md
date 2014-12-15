## **D-SDN : Decentralizing SDN’s Control Plane** ##

*Dwina Fitriyandini Siswanto (23213311)*
*EL5244 Pemrograman Perangkat Jaringan*

**Decentralize-SDN** (D-SDN) adalah suatu kerangka kerja yang memungkinkan distribusi *control plane* pada SDN, baik secara fisik maupun logis, dengan mendefinisikan hirarki kontrol dari *main controller* (MC) dan *secondary controller* (SC). Konsep dari kerangka kerja ini lahir karena konsep SDN saat ini, termasuk *Open-Flow*, lebih cocok untuk “*managed networks*”. SDN mengusung konsep kontrol tunggal dan tersentralisasi yang dinilai tidak cocok untuk internet heterogen yang terdiri dari beragam *autonomously administered network*, seperti contohnya jaringan yang *infrastructure-less* dan *self-organizing*.

###**D-SDN**###
Perbedaan utama antara *main controller* (MC) dan *secondary controller* (SC) adalah dalam hal hirarki. Sebelum SC dapat bertindak sebagai SDN *controller*, MC harus melakukan autorisasi dan delegasi terhadap SC. Sehingga SC diberi tanggung jawab untuk melakukan kontrol terhadap *switch* SDN yang berada dalam sub-domain dari domain MC. Sebagai gambaran dapat dilihat pada ilustrasi berikut ini,

![Gambar 1. Delegasi kontrol dari MC kepada SC](https://lh4.googleusercontent.com/icQxQieddLvC2V47ZVzBFYAJubZaIzNIqxine9KV0S4=s0 "Gambar 1.jpg")

Dalam konsep D-SDN, dengan menggunakan *control plane* terdesentralisasi, GW1 dan GW2 dapat berperan sebagai SC setelah dilakukan autorisasi dan delegasi oleh *Main Controller* yang berperan sebagai MC. Sehingga *flow* yang tiba di *node* dalam *client network* α dan β  tidak perlu melalui MC dan dapat ditangani secara langsung oleh SC.

Proses delegasi kontrol oleh MC terhadap SC dilakukan dengan *Control Delegation message* yang terdiri dari *Check-in Request* dan *Check-in Response*. Pada *Check-in Request*, SC mengirimkan *request* kepada MC agar dapat diautorisasi untuk berperan sebagai *controller*, kemudian MC merespon dengan mengirimkan *Check-in Response* kepada SC yang berisi apakah *request* diterima sehingga autorisasi dan delegasi dapat dilakukan atau *request* ditolak.

 
###**Implementasi**###
 Proses implementasi dilakukan dalam suatu *testbed* dimana *node* yang digunakan merupakan SDN-*enabled mobile nodes*. Implementasi meliputi *server-side* dan *client-side*. Keamanan yang digunakan adalah *Identity Based Criptography* (IBC) yang memerlukan *Trusted Third Party* (TTP) untuk membangkitkan *private key*. Dalam hal ini MC dapat berperan sebagai TTP. Tabel 1 berikut adalah deskripsi protokol-protokol dari komponen D-SDN yang digunakan untuk melakukan komunikasi.
 
 ![Tabel 1. Notasi](https://lh5.googleusercontent.com/TC5AlHpttmKCVTr-Ry3HHHUMLSDlZtobfPWp1ccM95c=s0 "Tabel 1.jpg")

 - *Setup*
Secara public key diturunkan dari identitas, TTP melakukan pemetaan node identity, *IDx*, ke sebuah titik pada kurva elips, *Px*. TTP membangkitkan suatu *master secret key*, *s*, dan mengkalkulasi *private key* dari tiap-tiap node agar memenuhi *Sx=sPx*. 

 - *Authenticated Key Agrrement*
Prosedur *Authenticated Key Agreement*, AKA, memiliki tujuan utama untuk menghindari *public key encryption*. Hal ini berarti, jika dua *node* sepakat menggunakan suatu kunci melalui *public key cryptography* maka kedua *node* tersebut dapat menggunakan *shared key* untuk kerahasiaan dan autentifikasi data.

 - *Handshaking*
Dalam prosedur *handshaking*, setelah *requesting node* (RN) mengirimkan sebuah *request* ke *authenticator*, RN harus merespon *challenge* yang dikirmkan oleh *authenticator* agar identitas perangkat dapat diverifikasi. Proses ini memungkinkan RN dan *authenticator* untuk mengkomputasi *shared key*.

 - *Availability*
*Framework* dapat diunduh di [sini](http://inrg.cse.ucsc.edu/community.)

###**Evaluasi**###
Evaluasi dilakukan dengan menggunakan 2 *use case*, yaitu *Secure Capacity Sharing* dan *Public Safety Networks*. *Secure Capacity Sharing* adalah skenario dimana *node* pada *infrastructure-less network* mencoba terhubung ke internet melalui *node* yang lain. *Public Safety Networks* adalah skenario dilakukannya desentralisasi kontrol pada layanan tanggap darurat.
 
####*Secure Capacity Sharing*####
Merujuk pada Gambar 1 sebuah *node* di *client network*, sebut saja *requesting node* (RN), ingin terkoneksi ke internet dan mengakses *World Wide Web*. Namun karena berada di luar jangkauan AP terdekat maka RN tidak dapat terhubung ke infrastruktur jaringan yang ada. Sebuah *node* yang lain, *gateway node* 1 (GW1) menawarkan *gateway service* kepada RN agar dapat terkoneksi ke internet melaluinya. Langkah-langkah *Secure Network Capacity Sharing* dapat dilihat pada Gambar 2 dibawah ini.

![Gambar 2. Secure Network Capacity Sharing](https://lh5.googleusercontent.com/TkpAA8T129jxkqOkIE24RsU-Eih-nZE6Tpq9R5qjvrQ=s0 "Gambar 2.jpg")

 - *Gateway discovery*
GW mengirimkan pesan periodik yang  berisi info mengenai kapabilitas *gateway*. Kemudian user memilih kandidat GW yang paling sesuai, dan mengirimkan *request* ke GW tersebut.
 - *Handshaking*
GW merespon *request* dari *user* kemudian mulai menginisiasi  prosedur *handshaking* untuk melakukan autentifikasi *node*.
 - *User check-in*
GW mengirimkan *request* ke *main controller* agar dapat diautentifikasi sebagai *secondary controller*. 

Apabila *user* mendapatkan GW yang lebih sesuai maka *user* tersebut dapat mengirimkan *request* ke GW tersebut dan melakukan prosedur *handshaking*. Kemudian MC dapat melakukan *flow creation* di GW baru dan *flow removal* di GW lama. Peristiwa ini dinamakan *secure handover*. Kemulusan dalam *handover* menjadi tujuan dan parameter keberhasilan dalam *Secure Network Capacity Sharing*, selain peningkatan performa diantara *gateway*.

####*Public Safety Networks*####
*Public Safety Networks* (PSN) diciptakan untuk mendeteksi dan/atau menangani bencana. Dengan melakukan desentralisasi control plane, *Public Safety Network* yang mengakomodasi D-SDN menghasilkan fleksibilitas, interoperabilitas, reliabilitas dan deployment yang cepat dalam keadaan krisis. Skenario PSN dapat dilihat pada gambar 3 dibawah ini dimana mobil merupakan GW yang dapat berperan sebagai SC terhadap agen aktor yang berbeda-beda, contohnya seperti pemadam kebakaran, polisi dan lain-lain.

![Gambar 3. Skenario Public Safety Networks](https://lh4.googleusercontent.com/_afy-ejZkmjfVV8aaRDRvDKwyPq2Tq5Q8ItPmFmHj58=s0 "Gambar 3.jpg")

Berikut ini adalah skenario untuk menangani kejadian *link failure* dengan cara mengimplementasikan toleransi pada *failure*. Pada skenario ini *SCi* merupakan *controller* yang aktif dan *SCj* akan mengambil alih peran *SCi* ketika *SCi* mengalami *failure*.

 1. Beberapa SC yang ada mengirimkan pesan **Hello** kepada perangkat-perangkat yang ada.
 2. *SCi* mengalami *failure*.
 3. Protokol *election* diantara SC dibangkitkan, untuk memilih SC yang dapat mengambil alih peran *SCi*.
 4. SC yang terpilih, *SCj*, mengirimkan *request* administrasi ke perangkat-perangkat dan mengambil alih peran sebagai *controller*.
 5. Perangkat-perangkat melakukan konfirmasi dengan mengirimkan pesan **Role Reply** bahwa peran *controller* telah diserahkan ke *SCj.*

Dari hasil eksperimen yang dilakukan, waktu minimum yang dibutuhkan untuk melakukan pemulihan dari suatu *failure* adalah 2,3 detik.

###**Kesimpulan**###
 D-SDN adalah sebuah kerangka kerja yang mendukung distribusi kontrol dengan mendefinisikan hirarki *controller* dimana *main controller* dapat mendelegasikan fungsi *controller* kepada *secondary controller*. Diharapkan dengan fungsi desentralisasi SDN *control plane*, D-SDN dapat mengakomodasi kebutuhan berbagai macam layanan dan aplikasi jaringan baik jaringan eksisting ataupun jaringan di masa depan. 


###**Referensi**###
M.A.S. Santos et al., ''Decentralizing SDN’s Control Plane,'' in *IEEE 39th Conference on Local Computer Networks*, Edmonton, AB, 2014, pp. 402-405.
