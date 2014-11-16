# SDN & IoT

*Eueung Mulyana*

Emang nyambung? Apa hubungannya?

Untuk me-*refresh* ingatan, atau bagi yg belum mafhum, IoT (*Internet of Things*) adalah konsep Internet (masa depan) yg diperluas dengan memasukan benda-benda (*things*) "cerdas" sebagai sumber dan tujuan informasi. Bahkan, mereka seolah-olah saling "berbicara" satu sama lain.

Sebagai ilustrasi, pada era IoT, skenario berikut adalah hal yg lumrah :-p

> - *otong*: Gan, ane disuruh manasin aer, pakai mode apa nih?
> - *juragan*: Pakei mode moderat tong! beban lagi berat, lagi banyak yg narik .. biar si bos gak kebobolan juga bayar tagihan

Skenario di atas adalah skenario *fiktif* di sebuah **smart home**, otong mewakili *pemanas air*, juragan mewakili *meteran listrik*, kedua-duanya bukan orang :-D ... (pis, mohon maaf untuk yg namanya [mirip-mirip] otong.. :-D..)

Secara konsep, **IoT** lebih luas dari sekedar **M2M** (*Machine to Machine*) seperti dalam contoh. IoT melibatkan **cloud** dalam interaksi antar partisipan, baik itu *things* maupun orang. Dan kalau sudah ada cloud, mestinya **SDN** tidak jauh-jauh amat.

SDN menyediakan *framework* sentralisasi kontrol bagi sistem yg berjaringan, ideal untuk operator atau siapapun yg berkepentingan untuk mengelola *things* yg tersebar posisinya tapi aktif berkomunikasi (satu sama lain). *In other words*, ideal, cocok untuk IoT.

##SDN + IoT
- Implementasi IoT yg umum adalah dengan menyambungkan sensor/aktuator lokal dengan cloud.
- Sensor yg terhubung dengan kontroler lokal merekam dan mengirimkan data ke cloud, melalui *home gateway* dan jaringan operator.
- Cloud dapat melakukan analisis data yg dikirim dan kemudian mengirimkan perintah kepada aktuator. Skema ini sesuai untuk kondisi data yg besar/banyak, keterkaitan antar data ketat atau untuk kondisi dimana konstrain waktu tidak terlalu krusial.
- Cloud dapat pula mendelegasikan (sebagian) fungsi analisis dan *decision making* ke gateway atau *provider edge router*. Skema ini dapat dipakai apabila terdapat konstrain waktu yg cukup ketat. Cara ini juga dikenal sebagai *fog computing* (teknologi cloud *tersebar*)
- ..
- Di sisi lain, SDN menyediakan mekanisme dimana "perintah" dapat dikomunikasikan dari sistem kontrol ke sistem aktuasi, misalnya dari cloud ke gateway.
- Dengan menggunakan fungsi kecerdasan/pemrosesan di cloud, cost untuk gateway dapat direduksi. Misalnya, fungsi *router* dan *dhcp server* dapat di-cloud-kan (e.g. via SDN controller).
- Contoh lain: 2 buah *things* yg terhubung ke 2 gateway berbeda perlu berkomunikasi satu sama lain. Apabila operator mengimplementasikan hirarki secara ketat, proses komunikasi berjalan lewat atas, bisa jadi via cloud. Dengan SDN, cloud dapat mengirimkan perintah untuk menginisiasi link horisontal (i.e. flow entry) antar gateway yg dapat digunakan langsung oleh kedua *things* untuk berkomunikasi.

##Referensi

1. [SDN, IoT Make 1 Soup](http://blogs.freescale.com/iot/2014/06/sdn-iot-make-1-soup/), Joseph Byrne (Freescale), June, 2014

##Lisensi
*CC Attribution-NonCommercial-NoDerivatives*
[(Lisensi)](http://creativecommons.org/licenses/by-nc-nd/4.0/)
