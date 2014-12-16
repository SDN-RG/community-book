Resume paper 

TOWARD SOFTWARE-DEFINED CELULLAR NETWORK
==========================
by: Li Erran Li, Z. Moreley Mao, &  Jennifer Rexford
-----------------------------

Hamzah U. Mustakim – 23213367


Pertumbuhan pesat pengguna smart-phone dan tablet diikuti juga oleh perkembangan jaringan seluler. Insfraksttuktur jaringan yang luas dan bentuk topologi yang kompleks memiliki beberapa kendala dalam proses konfigurasi setiap perangkat yang digunakan, karena menggunakan perangkat dari vendor yang berbeda,  sehingga menjadi kurang fleksibel pada proses operasional. Penerapan SDN (Software Difined Network) pada jaringan seluler dapat menyederhanakan desain topologi dan pengelolaan insfrakstuktur jaringan seluler. Selain itu juga memungkinkan operator untuk menambahkan layanan baru kepada pelanggan.

Konsep SDN yang dipaparkan pada paper ini adalah dengan menambahkan kontroler, switch, dan base-station yang dapat mengoperasikan aplikasi kontroler dengan fungsi sebagai berikut:
Mendefinisikan kebijakan dan aturan berdasarkan atribut / identitas pelanggan daripada alamat dan lokasi  pelanggan.
Melakukan proses kontrol secara real-time melalui local switch agent.
Melakukan pemeriksaan terhadap setiap paket data dan header  secara mendalam.
Mengelola pembagian pada alokasi sumber daya base-station  secara remote.

![enter image description here](https://lh6.googleusercontent.com/-1iOVFkwrZJ8/VI_Ghp4kJ7I/AAAAAAAAAM4/IVDB3OkqIco/s0/Capture1.JPG "Capture1.JPG")

  
	Gambar 1: Arsitektur 4G LTE (Long Term Evolution)

Pada arsitertur jaringan LTE (gambar 1) terdapat beberapa bagian (node) yang memiliki control plane dan data plane dengan fungsi tertentu. Diantara bagian-bagian  tersebut adalah:

1. Base-station (eNodeB): berfungsi menghubungkan UE (user equipment) pada jaringan LTE dan meneruskan paket data dari UE kepada jaringan dan sebaliknya.
2. S-GW (Serving Gateway) : sebagai pusat mobilitas UE. Bagian ini selalu memonitor pergerakan UE serta bertugas mengalokasikan frekuensi dan alamat IP.
3. P-GW (Paket Data Gateway) : berfungsi mengatur parameter QoS dan memonitor trafik data yang masuk pada jaringan. Selain itu juga bertugas untuk mengubungkan UE kepada jaringan internet dan jaringan seluler yang lain.

Secara sederhana jaringan LTE dapat disederhanakan seperti  gambar dibawah ini (gambar 2) :
 
 ![enter image description here](https://lh3.googleusercontent.com/-Qgdh0enGU0E/VI_GvWtuq2I/AAAAAAAAANE/VdTau82kQdQ/s0/Capture2.JPG "Capture2.JPG")
 
	Gambar 2 : Arsitertur jaringan LTE secara sederhana

Penerapan SDN pada jaringan seluler akan memberikan solusi dari beberapa masalah utama yang terjadi pada jaringan. Berikut ini adalah beberapa keuntungan yang didapatkan dari penerapan SDN:

1. SDN memberikan proses klasifikasi paket data yang masuk pada jaringan dan memberikan kempuan routing secara fleksibel.
Operator dapat memonitor semua trafik secara real-time dan efektif.
2. SDN memberikan protokol yang dapat bekerja pada teknologi seluler yang berbeda sehingga membuat proses mobility management menjadi jauh lebih mudah.
3. SDN memungkinkan proses distribusi parameter QoS  dan kebijakan keamanan menggunakan firewall.
4. SDN membuat penerapan jaringan virtual pada jaringan seluler menjadi mudah dengan cara membagi flow-space pada paket headers.
5. SDN dapat memusatkan proses kontrol dari beberapa base-station sehingga dapat menghemat daya listrik dan alokasi radio channel yang digunakan.

Dibutuhkan empat ekstensi utama  yang perlu  ditambahan pada jaringan seluler untuk proses penerapan SDN. Penambahan ekstensi tersebut akan mengubah bentuk arsitektur jaringan seluler seperti berikut ini (Gambar 3):

![enter image description here](https://lh5.googleusercontent.com/-89w7jbbgwmw/VI_IArCv_EI/AAAAAAAAANY/Ujyw20Bynj8/s0/Capture3.JPG "Capture3.JPG")
 
	Gambar 3 : Arsiterkur jaringan SDN seluler

Keempat ekstensi tersebut adalah:

1. Kontroler : Membuat parameter aturan dan kebijakan dalam hal atribut pelanggan.
2. Aplikasi Switch : Local control agent.
3. Perangkat Switch : Memproses dan melakukan klasifikasi paket data secara flesibel berdasarkan header.
4. Base Station : BTS harus mendukung remote control untuk mengatur alokasi sumber daya pada jaringan virtual serta  berfungsi untuk manajemen cell secara fleksibel.

Sumber: E. Li et al.., “Toward Software-Defined Cellular Networks,” Proc. EWSDN, Darmstadt, Germany, 2012.
