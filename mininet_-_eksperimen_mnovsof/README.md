# Mininet - Eksperimen Topologi

*Dwina Fitriyandini Siswanto, Siti Amatullah Karimah*

Pada Mininet sudah terdapat beberapa topologi bawaan yang dapat langsung digunakan dengan menggunakan perintah (*command*) tertentu. Beberapa topologi bawaan tersebut antara lain topologi *single*, *tree* dan *linear*.

![Mininet Topologi](./assets/mininet_topo.jpg "mininet_topo.jpg")

Pada eksperimen ini dilakukan pembangunan topologi diluar ketiga topologi diatas atau bisa disebut dengan topologi bebas atau *custom topology*.

## Topologi Bebas (*Custom Topo*)
Membuat *custom topology* dapat dilakukan dengan 2 cara, yaitu secara manual dengan menulis kode python atau membuat konfigurasi topologi menggunakan GUI editor seperti *Virtual Network Description* (vnd) kemudian diexport ke bentuk *file* yang dapat di-*running* oleh mininet.

- Membuat custom topology manual pada mininet
   1. Buatlah file *.py* dari topologi yang diinginkan kemudian simpan di folder *mininet/custom/*
   2. Setelah membuat file *.py* kemudian *run* *custom topology* yang telah dibuat menggunakan *command* :
`$ sudo mn --custom <custom_topology> --topo mytopo --mac --switch ovsk --controller remote`

![enter image description here](./assets/mininet_topo.jpg "vnd.jpg")

- Membuat *custom topology* dengan vnd pada via situs http://www.ramonfontes.com/vnd/
  1. Buatlah topologi yang diinginkan
  2. Klik ***File*>*Export*>*Export to mininet***, kemudian ubah format file menjadi .py
  3. Pada mininet, *copy* file ke dalam folder *mininet/examples*
  4. Buat agar file vnd dapat dieksekusi oleh mininet dengan
     `$ chmod +x <file_topology_vnd>`
  5. Jalankan file menggunakan command
	`$ sudo ./<file_topology_vnd>`

Ketika membuat *custom topology* pada vnd, terdapat beberapa fitur pendefinisian yang dibawa oleh vnd. Diantaranya sebagai berikut :

![VND feature 1](./assets/vnd-feature-1.jpg "vnd-feature-1.jpg")

![VND feature 3](./assets/vnd-feature-3.jpg "vnd-feature-3.jpg")

![VND feature 2](./assets/vnd-feature-2.jpg "vnd-feature-2.jpg")


## Menjalankan POX Controller
Sebelum menjalankan *custom topology* jalankan *controller* yang digunakan. Pada eksperimen ini *controller* yang digunakan ialah POX controller.

Berikut contoh cara menjalankan POX controller dalam mininet:
```bash
cd /home/ubuntu/pox && ./pox.py log.level --DEBUG forwarding.tutorial_I2_hub
```

## Eksperimen Custom Topology
Pada eksperimen ini dibuat 5 *custom topology* yang masing-masingnya dibangun menggunakan 2 cara yang dijelaskan sebelumnya yakni secara manual dan juga mengunakan vnd. Berikut 5 jenis topologi yang dibangun,

![enter image description here](./assets/custom_topology.jpg "custom_topology.jpg")


**1. 01_custom_topo**

![01 Custom Topology](./assets/01_custom_topo.jpg "01_custom_topo.jpg")

- Menjalankan custom topology yang telah dibuat di mininet

![01_custom_topo_mininet](./assets/01_custom_topo_mininet.jpg "01_custom_topo_mininet.jpg")

- Menjalankan custom topology yang telah dibuat di vnd

![VND Custom Topology](./assets/01_custom_topo_vnd.jpg "01_custom_topo_vnd.jpg")


**2.	02_custom_topo**

![02_custom_topo](./assets/02_custom_topo.jpg "02_custom_topo.jpg")

**3.	2d_mesh_topo**

![2d_mesh_topo](./assets/2d_mesh_topo.jpg "2d_mesh_topo.jpg")

**4. 2d_torus_topo**

![2d_torus_topo](./assets/2d_torus_topo.jpg "2d_torus_topo.jpg")

**5. full_connect_topo**

![full_connect_topo](./assets/full_connect_topo.jpg "full_connect_topo.jpg")

**Penjelasan Source Code Mininet Topology**

Berikut ini sedikit penjelasan mengenai *source code* pembentukan topologi pada mininet, berdasarkan topologi yang di *generate* oleh vnd.

![enter image description here](https://lh6.googleusercontent.com/-i5zqRWVBC20/VHYJQl5hKVI/AAAAAAAAADk/Q5S5ezzh3s0/s0/Picture1_topo.png "Picture1_topo.png")

Baris pertama diawali dengan simbol #! yang diikuti oleh *pathname* (/usr/bin/python) dari Python *interpreter* agar *script* Python dapat dieksekusi oleh sistem operasi Unix.

'*Statement from'* pada Python memungkinkan kita meng-*import* atribut secara spesifik dari modul kedalam *namespace* yang digunakan.

![enter image description here](https://lh4.googleusercontent.com/-ardypTdFJKk/VHYLKPJB3EI/AAAAAAAAAEc/CtgwVefB2A4/s0/tabel1_topo.png "tabel1_topo.png")

![enter image description here](https://lh5.googleusercontent.com/GeJLIs7BlBFoz6s_4K2IQElhApfvATSfP69S2UIixQ=s0 "Picture2_topo.png")

def topology() : merupakan sintaks yang digunakan untuk mendefinisikan fungsi ‘topology’ dimana pada akhir namespace, fungsi ini dipanggil melalui sintaks ‘topology()’.
 
net = Mininet ( controller=RemoteController, link=TCLink, switch=OVSkernelSwitch )

•**controller=RemoteController**
Mendefinisikan controller yang di kontrol diluar Mininet.

•**link=TCLink**
Menspesifikasikan link sebagai TC yang digunakan untuk memodifikasi parameter link.

•**switch=OVSkernelSwitch**
Penggunaan OVS (Open v Switch) untuk menyediakan fungsi switching pada lingkungan virtualisasi hardware dan juga mendukung berbagai macam protokol standar pada jaringan komputer.

![enter image description here](https://lh6.googleusercontent.com/-eLkwT-BbITo/VHYL1syls5I/AAAAAAAAAEo/nYs388I6Gc4/s0/tabel2_topo.png "tabel2_topo.png")

![enter image description here](https://lh5.googleusercontent.com/-wlLdjNMhZGg/VHYL_ms0tjI/AAAAAAAAAE0/FC_fC_VRLBE/s0/Picture3_topo.png "Picture3_topo.png")

![enter image description here](https://lh5.googleusercontent.com/-emcGq4dWuCc/VHYMHFHeaFI/AAAAAAAAAFA/gcYpa3JlsjU/s0/Picture4_topo.png "Picture4_topo.png")

Setelah jaringan dibentuk, perintah 'start()' digunakan untuk menjalankannya. Kemudian sistem dapat menjalankan beberapa *task* yang berguna diantaranya melakukan tes *basic connectivity*, *bandwidth test* dan juga menjalankan mininet CLI.

Metode 'net.stop()' dipanggil untuk melakukan *shut down* pada jaringan setelah seluruh tes atau aktivitas yang diinginkan dijalankan.

##Referensi

1. TBA
2. ...

##Lisensi
*CC Attribution-NonCommercial-NoDerivatives*
[(Lisensi)](http://creativecommons.org/licenses/by-nc-nd/4.0/)
