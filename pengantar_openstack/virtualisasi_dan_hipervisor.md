# Virtualisasi dan Hypervisor

Teknologi **Virtualisasi** adalah landasan bagi tumbuh dan berkembangnya ***OpenStack***. Pada hakikatnya OpenStack tidak lain adalah suatu kerangka kerja bagaimana mengatur dan mendayagunakan teknologi virtualisasi yang berkembang terus agar penggunaannya baik dalam skala kecil maupun dalam skala yang luar biasa besar *tetap terkontrol* dan *mudah dilakukan*.

#### Definisi Virtualisasi
Virtualisasi adalah membuat versi maya (*virtual*) dari suatu sumber daya (*resource*) sehingga pada satu sumber daya fisik dapat dijalankan atau disimpan beberapa sumber daya maya sekaligus, dengan syarat unjuk kerja masing-masing sumber daya maya itu tidak berbeda signifikan dengan sumber daya fisiknya.

Hingga saat ini sumber daya yang terlah dapat *divirtualisasikan* antara lain adalah perangkat keras komputer (*hardware*), media penyimpan data (*storage*), operating system (*os*), layanan jaringan (*networking*) dan daftar ini masih bertambah terus.

Virtualisasi ini dimungkinkan karena perkembangan teknologi hardware yang sedemikian pesat sehingga kemampuan sebuah sumber daya fisik berada jauh di atas tuntutan penggunaannya sehinga sebagian besar waktu atau kapasitasnya tidak terpakai (*idle*).

Kapasitas atau kemampuan lebih ini didayagunakan dengan menjalankan atau menyimpan beberapa sumber daya maya (tergantung pada kemampuan dan kapasitas sumber daya tersebut dan beban kerjanya) sehingga dapat menghasilkan efisiensi yang lebih tinggi.

#### Definisi Hypervisor
Secara teknis virtualisasi diwujudkan dengan menambahkan satu bagian (*layer*) perangkat lunak (*software*) yang disebut dengan nama ***hypervisor***.

Hypervisor ini berfungsi sebagai ***Virtual Machine Manager*** (VMM) yaitu bagian yang melakukan abstraksi dari perangkat keras fisik menjadi perangkat keras virtual dalam rangka mendistribusikan beban kerja dari semua mesin virtual (***VM***) ke masing-masing perangkat keras secara proporsional.

## Virtualisasi pada keluarga komputer yang berarsitektur x86
Pada awalnya virtualisasi pada keluarga komputer yang berarsitektur x86 dianggap sulit direalisasikan karena prosesor x86 memiliki arsitektur yang dapat digambarkan dengan bagan seperti di bawah ini :

![Arsitektur x86](./assets/x86arc.png)

Prosesor x86 membagi tingkatan akses menjadi 4 tingkat yang dinamai *Ring 0, 1, 2*, dan *3*. **Ring 0** adalah satu-satunya tingkatan yang memungkinkan dijalankannya perintah-perintah untuk mengakses memori atau perangkat keras lainnya secara langsung.

Ring yang lain tidak dapat menjalankan perintah-perintah untuk mengakses memori dan perangkat keras secara langsung. Software yang dijalankan pada Ring 1, 2, dan 3 dibatasi aksesnya ke hardware untuk alasan keamanan sistem yang ketat. Akses hardware dari Ring selain Ring 0 akan diseleksi terlebih dahulu dan apabila diijinkan akan didelegasikan melalui Ring 0 dengan mendapatkan pengawalan yang sangat ketat.

Idealnya virtualisasi dapat dilakukan dengan mudah dengan cara menempatkan hypervisor pada Ring 0 dan OS minimal di Ring 1. Akan tetapi OS perlu mengakses memori dan perangkat keras lainnya secara langsung. Dalam kondisi biasa OS menempati Ring 0 dan tidak ada masalah. Bila OS harus digeser ke Ring 1 maka timbul masalah akses. Sehingga tantangan terbesar virtualisasi dalam keluarga komputer yang berarsitektur x86 adalah bagaimana membuat sebuah hypervisor yang dapat dijalankan di Ring 0 akan tetapi tidak menghalangi akses dari OS yang digeser dari Ring 0 ke Ring 1.

Ada beberapa cara yang dilakukan untuk mewujudkan hal tersebut yang mengakibatkan pada akhirnya muncul beberapa jenis hypervisor juga. Implikasinya ada tiga macam teknolog virtualisasi yang saat ini bersaing di pasar, yaitu :
* **Full Virtualisasi** dengan cara translasi secara biner
* **Paravirtualisasi** dengan modifikasi OS
* **Virtualisasi** dengan bantuan perangkat keras.

