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

##Referensi

1. TBA
2. ...

##Lisensi
*CC Attribution-NonCommercial-NoDerivatives*
[(Lisensi)](http://creativecommons.org/licenses/by-nc-nd/4.0/)
