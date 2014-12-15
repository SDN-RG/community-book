*A Security Enforcement Kernel for OpenFlow Networks*
---------------------------------------------------
*Bagus Aditya*

---------------------------------------------------

**SDN Security Challenge**

---------------------------------------------------

SDN memberikan fasilitas pada layer kontrol dengan menyediakan *programmable network* untuk membuat suatu *flow policy* sesuai kemauan. Tantangan keamanan di suatu *programmable network* yang paling utama adalah mendeteksi, efisiensi, dan rekonsiliasi konflik dari *flow rules* yang diakibatkan oleh suatu *Dynamic Open Flow (OF) Application*. *FortNOX* adalah suatu software yang dikembangkan untuk menyediakan *role-based authorization* dan mengatasi kendala keamanan pada *NOX controller* . FortNOX memungkinkan kontroler *NOX* untuk bisa mengetahui adanya kontradiksi pada *flow rule* secara *real time*, dan mengimplementasikan algoritma baru yang lebih baik. 

Aturan keamanan jaringan pada *switch OF* adalah suatu fungsi bagaimana suatu aplikasi OF yang ada akan bereaksi terhadap stream yang datang dari *flow request*. *FortNOX Enforcement Kernel* merupakan suatu *kernel enforcement policy* (disebut FortNOX) adalah sebuah *extension* dari kontroler OpenFlow NOX. FortNOX menggabungkan sebuah *live rule conflict detection engine*, yang akan memediasi semua *request* dari *Flow Rule insertion*. Sebuah *rule conflict* dikatakan ada pada saat *rule candidate* akan mengaktifkan atau menonaktifkan *network flow* setelah diperbolehkan oleh *existing rule*. Analisis *rule conflict* dilakukan menggunakan sebuah algoritma baru, dimana kita sebut *alias set rule reduction*.

Ketika konflik terdeteksi, FortNOX dimungkinkan untuk memilih untuk menerima atau menolak *rule* baru, tergantung pada *rule insertion requester* yang beroperasi pada otorisasi keamanan yang lebih tinggi dari pemilik dari *conflicting rule* yang telah ada. 
FortNOX mengimplementasikan *role-based authentication* untuk menentukan otorisasi keamanan dari setiap aplikasi OF dan memaksa menggunakan prinsip dari *least privilege* untuk memastikan integritas dari proses mediasi.

![gambar 1](https://cloud.githubusercontent.com/assets/7939343/5398792/e96a5e58-8197-11e4-92b4-61c393f9fce5.png)

Misal, suatu skenario sederhana , yang kita sebut *dynamic flow tunneling*, pada gambar 1 di atas yang terdiri dari 3 *host*, satu *OF switch* dan satu *OF controller*. Sebuah *firewall* (hal ini bertindak sebagai suatu contoh aplikasi *OF Security*) memiliki *rule* untuk memblok paket dari luar *host* 10.0.0.2 ke *web service* (port 80) yang berjalan di *host* 10.0.0.4. 
Sekarang misalkan bahwa juga terdapat beberapa aplikasi OF lainnya menambahkan menambahkan tiga buah *flow rule* baru ke *OF controller* yang disambungkan oleh petunjuk *GOTO TABLE*. 
*Rule* pertama memodifikasi IP address sumber dari suatu paket ke 10.0.0.1, jika paket dikirimkan dari 10.0.0.2 ke 10.0.0.3 (port80). *Final rule* secara otomatis memperbolehkan *forwarding* paket dari 10.0.0.1 ke 10.0.0.4 pada port 80. Di kasus ini, jika *host* 10.0.0.2 mengirimkan paket ke port 80 pada *host* 10.0.0.3, paket ini dapat *membypass firewall* karena ini tidak secara langsung ke *host* 10.0.0.4 namun ke 10.0.0.3. Namun,paket ini pada akhirnya akan dikirim ke *host* 10.0.0.4 oleh *OF Controller* walaupun ada *firewall* yang melarang trafik ini. 

Hal ini dengan jelas telah menunjukkan bahwa *firewall* yang ada bisa dihindari secara sederhana dengan menambahkan beberapa *OF flow rule*. Ilustrasi di atas adalah hal sederhana, tantangan sebenarnya adalah memastikan bahwa semua aplikasi OF tidak melanggar *security policies* di jaringan nyata yang besar dengan banyak *OF switch*, aplikasi OF yang berbeda-beda, dan *security policies* yang kompleks. Melakukan pekerjaan semacam ini jika dikerjakan secara manual jelas kesalahan yang rawan dan berbahaya.

FortNOX mengembangkan kemampuan dari *NOX OpenFlow Controller* dengan menyediakan *non-bypassable policy-based flow rule enforcement* terhadap *flow rule insertion request* dari aplikasi OpenFlow. Tujuan utamanya adalah untuk meningkatkan kemampuan NOX untuk menerapkan batasan *network flow (flow rule)* yang dihasilkan oleh aplikasi *security OF* untuk memprogram ulang *switch* dalam menanggapi suatu ancaman yang dirasakan.
Sekali *flow rule* di *insert* ke FortNOX oleh aplikasi *security*, tak satupun aplikasi OF lain, yang dapat *menginsert flow rules* ke jaringan OF yang bertentangan dengan rule tersebut. 
FortNOX mengatasi *rule conflict* dengan melihat peran otorisasi menggunakan *flow rule* yang telah *digitally signed*, dimana boleh tidaknya aplikasi yang meminta *insert flow rule* nya berdasarkan pada penetapan *privilege* untuk *flow rule* baru.

---------------------------------------------------

**Implementasi FortNOX**

---------------------------------------------------

![gambar 2](https://cloud.githubusercontent.com/assets/7939343/5398800/f935cdfe-8197-11e4-950e-8f3b39fecfa8.png)


Gambar di atas mengilustrasikan komponen yang membentuk *FortNOX extension* ke NOX. Di tengah NOX layer, terdapat sebuah *interface* yang disebut *send_openflow_command()*, yang bertanggungjawab untuk menyampaikan *flow rule* dari aplikasi OF ke *switch*.  Sebuah *role based source authentication modul* menyediakan validasi *digital signature* untuk tiap *flow rule insertion request*, dan menetapkan prioritas yang sesuai pada *flow rule* baru atau memberikan prioritas terendah jika tidak ada *signature* yang tersedia. 
*Conflict analyzer* bertanggungjawab untuk mengevaluasi tiap  *flow rule* baru terhadap *flow rule* yang lama sesuai *aggregate flow table*. Jika *conflict analyzer* telah memutuskan bahwa calon *flow rule* yang baru konsisten dengan *flow rule* yang telah ada, maka akan diteruskan ke *switch* dan disimpan di *aggregate flow table*, yang dikelola oleh *state table manager*. FortNOX menambahkan *flow rule timeout callback interface* ke NOX, yang akan *mengupdate aggregate flow table* saat *switch* melihat ada *rule* yang kadaluwarsa. 

Terdapat *interface* tambahan yang dapat memungkinkan FortNOX menyediakan *enforced flow rule mediation*. Yang pertama adalah *IPC Proxy*, yang memungkinkan sebuah *legacy native C OF application* dipakai dalam proses yang terpisah, dan secara ideal dioperasikan dari *non-privileged* account yang terpisah. *Interface proxy* menambahkan sebuah ekstensi *digital signature* yang memungkinkan aplikasi ini untuk masuk dan meminta *rule insertion*, yang kemudian memungkinkan FortNOX untuk menentukan pemisahan *rule* berdasarkan *signature*. Melalui proses pemisahan ini, kita dapat menerapkan prinsip *least privilege* pada operasi infrastruktur kontrol. Melalui mekanisme *proksi*, aplikasi OF dimungkinkan untuk meminta *flow rule insertion* baru, namun permintaan ini dimediasi secara terpisah dan independen oleh *conflict resolution service* yang dioperasikan dalam kontroler.

***Role-based Source Authentication***

---------------------------------------------------
FortNOX mengenali 3 peran otorisasi yang membuat *flow rule insertion request*. Peran pertama adalah seorang *administrator*, dimana *rule* yang di *insert* akan mendapatkan prioritas tertinggi pada skema resolusi *conflict* di FortNOX. Kedua, adalah aplikasi *security OF*, yang dimungkinkan membuat suatu *flow rule* yang akan menentang *security policy* di jaringan yang telah dibuat oleh *administrator* berdasarkan ancaman terbaru yang diterima, misal suatu *flow* yang mencurigakan. *Flow insertion* dari aplikasi *security* ini akan berada di bawah prioritas *rule* yang dibuat oleh *administrator*. Dan yang ketiga adalah aplikasi OF yang tak berkaitan dengan *security* berada pada prioritas terakhir.

**Alias set rule reduction**

---------------------------------------------------
Untuk mendeteksi suatu konflik antara *rule* yang baru dengan *rule* yang sudah ada pada OpenFlow, *source & destination IP Adrress*, port dan *wildcard* perlu dikonversi semuanya termasuk *rule candidate* juga yang dijadikan suatu representasi yang disebut *Alias Reduced Rules (ARRs)* dan kemudian baru dianalisis konfliknya. Saat *initial alias set* dibuat, ARR tersebut memuat *IP address rule* pertama, *network mask*, dan port. Jika *action* dari *rule* menyebabkan *field substitution via* sebuah *set action*, maka hasilnya ditambahkan ke *set alias*, yang kemudian digunakan untuk mengganti kriteria dari ARR. Kemudian barulah bisa dilakukan analisis berpasangan antara calon ARR terhadap ARR yang ada. Jika ada *intersection* antara *source* dan *address set*, maka digunakanlah gabungan antara masing-masing *set* tersebut sebagai *rule alias set* selanjutnya. 
Contoh : diberikan suatu *OF security rule* 
(*equation* 1) a --> b *drop packet*,
maka rule turunannya adalah 
(*equation* 2) (a) --> (b) *drop packet*
(*source* alias diubah ke (a) dan *destination alias* menjadi (b) ). 
Untuk calon *rule set* adalah : 
(*equation* 3) 
*1. a --> c set (a --> a')*
*2. a'--> c set (c --> b)*
*3. a'--> b forward packet*
maka *intermediate alias set* nya adalah :
(*equation* 4)
*a --> c set (a-->a') (a,a')(c)*
*a'--> c set (c-->b) (a,a')(c,b)*
*a'-->b forward packet (a,a')(c,b) forward packet*
maka turunan dari *rule* nya adalah
(*equation* 5) *(a,a') --> (c,b) forward packet*

**Rule Set Conflict Evaluation**

---------------------------------------------------
FortNOX telah menunjukkan *alias set rule reduction* pada *kandidat rule*. Validitas pengecekannya ini kemudian ditunjukkan antara kandidat ARR cRule dan set dari ARRs yang merepresentasikan *active flow rule fRule* sebagai berikut :
1. *Skip* semua *cRule/fRule pair* yang tidak cocok dengan prototipe
2. *Skip* semua *cRule/fRule pair* yang *action* keduanya baik *forward* atau *drop* paket.
3. Jika *cRule alias set* berpotongan dengan *fRule*, maka bisa disimpulkan terjadi sebuah konflik.
Dengan demikian, berdasarkan *flow description* di persamaan 2 dan calon *rule set* di persamaan 5 mengasumsikan bahwa kedua *rule* adalah protokol TCP, kandidat rule yang pertama telah melewati 2 pengecekan awal. Namun, untuk pemeriksaan ketiga, karena *source* dan *destination alias set* menghasilkan (a) dan (b), kandidat *rule* dinyatakan bertentangan.

**Penyelesaian Konflik**

---------------------------------------------------
Saat terdeteksi konflik antara *existing rule* di *aggregate flow table* dan *candidate flow rule*, sifat dari *candidate rule* dievaluasi berdasarkan peran otorisasinya. Jika sumber dari *flow rule insertion request* beroperasi pada otorisasi peran yang lebih tinggi daripada rule yang konflik di *aggregate flow table*, kandidat *rule* yang baru akan menimpa *rule* yang ada. *Rule* yang ada dibersihkan dari kedua *aggregate flow table* dan *switch*, dan *candidate rule* dimasukkan ke keduanya. Jika *source* dari *insertion request* adalah *source* yang otorisasi perannya lebih rendah dari *rule* yang bertentangan dalam *aggregate flow table*, maka *candidate rule*( yang baru ditolak, dan kesalahan dikembalikan ke aplikasi. Dan jika *source* dari *insertion requester* beroperasi dengan otorisasi sama dengan *conflicting rule* di *aggregate flow table*, maka FortNOX memungkinkan *administrator* untuk menentukan hasil. Secara *default*, *rule* baru ini akan menggantikan *rule* sebelumnya.

**State Table Manager**

---------------------------------------------------
*State Table Manager* dan modul *Flow Rule Timeout Callback* mengelola keadaan semua *flow rule* yang aktif yang diberlakukan oleh FortNOX, serta sifat dari *rule* itu sehubungan dengan *switch flow table* dan peran otorisasi produser *rule* tersebut. Ketika *flow rule*  berhasil dimasukkan ke dalam *switch*, ARR disimpan dalam *aggregate flow table*. *Rule* dihapus melalui *timer* eksplisit yang disediakan melalui *Security Directives Translator*, atau saat ditemui dalam konflik dengan *candidate rule* yang di *insert* dari produser yang beroperasi level otorisasi yang lebih tinggi. 

**Security Directive Translation**

---------------------------------------------------
*Security Directive Translation* adalah suatu *python interface* yang memediasi suatu *set* dari *high-level threat mitigation directives* ke *flow rules*, yang *digitally signed* dan disampaikan kepada FortNOX.
*Security directive translator* yang ada menerapkan tujuh *security directives* : *block, deny, allow, redirect, quarantine, undo, constraint dan info*. *Block* menerapkan *filter full duplex* di antara *CIDR Block* dan jaringan internal, dimana penggunaan utama untuk perintah ini adalah untuk melakukan *blacklist*.

---------------------------------------------------
**Reference :**
---------------------------------------------------
P. Porras, S. Shin, V. Yegneswaran, M. Fong, M. Tyson, and G. Gu. *A Security Enforcement Kernel for OpenFlow Networks*. In Proceedings ACM SIGCOMM Workshops on Hot Topics in Software Defined Networking (HotSDN), August 2012

