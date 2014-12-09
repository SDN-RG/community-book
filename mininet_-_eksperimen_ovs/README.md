# Mininet - Eksperimen OVS

*Bagus Aditya, Hamzah U. Mustakim*


#Macam-macam Flow Matching pada Topologi Mininet Tanpa Controller

Terdapat beberapa macam flow matching yang bisa dilakukan pada mininet. Pada topologi yang akan dibuat, terdapat 3 host dan 1 buah switch ovsk. Kemudian pada switch tersebut ditambahkan flow untuk melakukan komunikasi antar host.
Pada simulasi kali ini dilakukan beberapa kasus flow matching. Yaitu Port based, MAC Address Based, IP Based, dan Transport Protocol Based pada simple HTTP server.

----------

**PORT BASED FLOW MATCHING (L1)**
### Port Based

*Membuat single topologi dengan 3 host tanpa menghubungkan ke controller*
Pada mesin yang sudah terinstall mininet / VM Mininet yang bisa didownload dari website resmi mininet, lakukan perintah di bawah ini :

`sudo mn --mac --topo single,3 --switch ovsk --controller=remote`

```
*** Creating network
*** Adding controller
Unable to contact the remote controller at 127.0.0.1:6633
*** Adding hosts:
h1 h2 h3
*** Adding switches:
s1
*** Adding links:
(h1, s1) (h2, s1) (h3, s1)
*** Configuring hosts
h1 h2 h3
*** Starting controller
*** Starting 1 switches
s1
*** Starting CLI:
```

Kemudian untuk melihat network interface yang terhubung pada host dan switch dengan command seperti berikut :

`mininet> net`

maka muncul tampilan interface network pada tiap node seperti di bawah ini

```
h1 h1-eth0:s1-eth1
h2 h2-eth0:s1-eth2
h3 h3-eth0:s1-eth3
s1 lo:  s1-eth1:h1-eth0 s1-eth2:h2-eth0 s1-eth3:h3-eth0
c0
```

Untuk menampilkan informasi pada setiap node yang ada, dengan comand dibawah ini:

`mininet> dump`

Maka akan muncul tampilan dibawah ini:

```
<Host h1: h1-eth0:10.0.0.1 pid=3814>
<Host h2: h2-eth0:10.0.0.2 pid=3815>
<Host h3: h3-eth0:10.0.0.3 pid=3816>
<OVSSwitch s1: lo:127.0.0.1,s1-eth1:None,s1-eth2:None,s1-eth3:None pid=3820>
<RemoteController c0: 127.0.0.1:6633 pid=3807>
```

Untuk melihat flow-table pada switch1 gunakan comand seperti dibawah ini:

`sudo ovs-ofctl dump-flows s1`

Selanjutnya akan didapat tampilan flow-table seperti dibawah ini.

```
NXST_FLOW reply (xid=0x4):
```
Flow table di atas terlihat masih kosong. Hal ini dikarenakan kita belum menambahkan flow pada switch1 sehingga antar host belum bisa saling berkomunikasi.

Untuk menambahkan flow pada switch-1 berdasarkan port yang tersedia, lakukan command di bawah ini

**Add flow at Port 1**

`sudo ovs-ofctl add-flow s1 in_port=1,action=output:2`

Command di atas adalah menambahkan flow pada port 1 dengan keluaran pada port 2 pada s1

Kemudian kita bisa melihat flow table yang telah dimasukkan,

`sudo ovs-ofctl dump-flows s1`

```
NXST_FLOW reply (xid=0x4):
 cookie=0x0, duration=3.237s, table=0, n_packets=0, n_bytes=0, idle_age=3, in_port=1 actions=output:2
```

*Skenario 1*
Pada skenario 1 ini kita akan mencoba melakukan tes komunikasi dari host 1 ke host 2 dengan mengirimkan sebuah paket ICMP.
Lakukan Xterm pada h1,h2, dan h3. Kemudian untuk menampilkan paket yang berada pada eth0-nya kita gunakan command:

pada h1 : `tcpdump -ne -i h1-eth0`
pada h2 : `tcpdump -ne -i h2-eth0`
pada h3 : `tcpdump -ne -i h3-eth0`

Kemudian, lakukan ping dari h1 ke h2 :

`h1 ping -c 1 h2`

Pada h1 akan didapat dumping paket dibawah ini:

```
root@mininet-vm:~# tcpdump -ne -i h1-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h1-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
06:53:04.725955 00:00:00:00:00:01 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
06:53:05.724128 00:00:00:00:00:01 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
06:53:06.724140 00:00:00:00:00:01 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
```

Selanjutnya kita lihat dumping paket pada h2

```
root@mininet-vm:~# tcpdump -ne -i h2-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h2-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
06:53:04.726225 00:00:00:00:00:01 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
06:53:04.726239 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
06:53:05.724183 00:00:00:00:00:01 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
06:53:05.724199 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
06:53:06.724203 00:00:00:00:00:01 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
06:53:06.724220 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
```

dari hasil dumping paket pada kedua host di atas, maka bisa dituliskan sequence diagram seperti di bawah ini:

```sequence
participant h1
participant s1
participant h2
participant h3
h1->s1:ARP Request
s1-->h2:ARP Request
h1->s1:ARP Request
s1-->h2:ARP Request
h1->s1:ARP Request
s1-->h2:ARP Request
h2-->s1:ARP Reply
h2-->s1:ARP Reply
h2-->s1:ARP Reply
```



*Skenario 2*
Masih menggunakan 1 flow table yang sama, kita lakukan tes komunikasi dari h2 ke h1. Jangan lupa untuk xterm h1,h2 dan h3, kemudian amati paket yang datang dan keluar di setiap interface host dengan melakukan dumping.

`mininet> h2 ping -c 1 h1`

Kita hanya akan menemukan paket ICMP Request dan ARP Request pada h2 saja, seperti yang terlihat di bawah ini :
```
root@mininet-vm:~# tcpdump -ne -i h2-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h2-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
06:55:54.707010 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo request, id 4210, seq 1, length 64
06:55:59.720120 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
06:56:00.720102 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
06:56:01.721048 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28

```
Maka bisa dituliskan sequence diagram seperti di bawah ini :

```sequence
participant h1
participant s1
participant h2
participant h3
h2->s1:ICMP Request
h2-->s1:ARP Request
h2-->s1:ARP Request
h2-->s1:ARP Request
```
Terlihat s1 belum bisa meneruskan paket dari h2 ke h1. Hal ini dikarenakan kita belum menambahkan flow dari port2 ke port1

*Tahap selanjutnya kita akan menambahkan flow pada port2 ke port1 pada s1*

**Tambahkan flow pada port2 ke port1 pada s1 menggunakan command berikut**

`sudo ovs-ofctl add-flow s1 in_port=2,action=output:1`

Maka untuk mengecek flow table menggunakan command berikut :

`sudo ovs-ofctl dump-flows s1`

Didapatkan flow table di bawah ini :

```
NXST_FLOW reply (xid=0x4):
 cookie=0x0, duration=376.441s, table=0, n_packets=9, n_bytes=378, idle_age=131, in_port=1 actions=output:2
 cookie=0x0, duration=2.199s, table=0, n_packets=0, n_bytes=0, idle_age=2, in_port=2 actions=output:1
```

*Skenario 1*
Sama seperti skenario 1 sebelumnya, kita akan melakukan tes komunikasi antara h1 ke h2. Jangan lupa untuk melakukan xterm h1, h2 , dan h3 untuk mengamati paket yang melewati interface tiap host.

Setelah itu lakukan command ping :

`h1 ping -c 1 h2`

Maka tcpdump pada h1 diperoleh:

```
root@mininet-vm:~# tcpdump -ne -i h1-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h1-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
06:57:54.375670 00:00:00:00:00:01 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
06:57:54.377215 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
06:57:54.377226 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo request, id 4256, seq 1, length 64
06:57:54.378185 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo reply, id 4256, seq 1, length 64
06:57:59.385383 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
06:57:59.385404 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
```

Dan tcpdump pada h2 diperoleh:

```
root@mininet-vm:~# tcpdump -ne -i h2-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h2-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
06:57:54.375946 00:00:00:00:00:01 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
06:57:54.375975 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
06:57:54.377700 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo request, id 4256, seq 1, length 64
06:57:54.377722 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo reply, id 4256, seq 1, length 64
06:57:59.384967 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
06:57:59.386119 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
```

Kita bisa tuliskan sequence diagram sebagai berikut:

```sequence
participant h1
participant s1
participant h2
participant h3
h1->s1: ARP Request
s1-->h2:ARP Request
h2-->s1:ARP Reply
s1->h1: ARP Reply
h1->s1: ICMP Request
s1-->h2:ICMP Request
h2-->s1: ICMP Reply
s1->h1:ICMP Reply
h2-->s1: ARP Request
s1->h1:ARP Request
h1->s1:ARP Reply
s1-->h2: ARP Reply
```

Kita bisa melihat pada sequence diagram di atas, komunikasi sudah bisa terjadi antara h1 dan h2, paket ICMP Reply dari h2 sudah bisa diteruskan ke h1.

*Skenario 2*
Sama seperti skenario 2 sebelumnya, kita lakukan tes komunikasi dari h2 ke h1:

`h2 ping -c 1 h1`

Pada h1 didapatkan tcpdump berikut :
```
root@mininet-vm:~# tcpdump -ne -i h1-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h1-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
06:59:56.034737 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo request, id 4294, seq 1, length 64
06:59:56.034764 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo reply, id 4294, seq 1, length 64
07:00:01.048952 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
07:00:01.049462 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
07:00:01.049477 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
07:00:01.050306 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
```

Pada h2 didapatkan tcpdump berikut:

```
root@mininet-vm:~# tcpdump -ne -i h2-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h2-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
06:59:56.034503 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo request, id 4294, seq 1, length 64
06:59:56.035333 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo reply, id 4294, seq 1, length 64
07:00:01.048925 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
07:00:01.049321 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
07:00:01.049349 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
07:00:01.050253 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
```

Maka dapat dituliskan sequence diagram berikut:

```sequence
participant h1
participant s1
participant h2
participant h3
h2-->s1: ICMP Request
s1->h1:ICMP Request
h1->s1: ICMP Reply
s1-->h2:ICMP Reply
h1->s1:ARP Request
s1-->h2: ARP Request
h2-->s1: ARP Request
s1->h1:ARP Request
h1->s1:ARP Reply
s1-->h2: ARP Reply
h2-->s1: ARP Reply
s1->h1:ARP Reply
```
Tes komunikasi dari h2 ke h1 juga sudah bisa berhasil. ICMP Reply dari h1 sudah bisa diteruskan ke h2.

----------


**MAC BASED FLOW MATCHING (L2)**
### FLow Matching Based on MAC Address
Selanjutnya, kita akan melakukan flow matching berdasarkan MAC Address.
Jangan lupa lakukan `sudo ovs-ofctl del-flows s1` untuk menghapus flow table sebelumnya yang telah dibuat.

Pada percobaan pertama, kita hanya tambahkan flow dari MAC Address h1 ke port2 pada s1

`sudo ovs-ofctl add-flow s1 dl_src=00:00:00:00:00:01,actions=output:2`

Kita bisa lihat hasil flow table dari command di atas :
`sudo ovs-ofctl dump-flows s1`

```
NXST_FLOW reply (xid=0x4):
 cookie=0x0, duration=3.233s, table=0, n_packets=0, n_bytes=0, idle_age=3, dl_src=00:00:00:00:00:01 actions=output:2
```
----------

*Skenario 1*
Pada skenario 1 ini kita akan mencoba melakukan tes komunikasi dari host 1 ke host 2 dengan mengirimkan sebuah paket ICMP.
Lakukan Xterm pada h1,h2, dan h3. Kemudian untuk menampilkan paket yang berada pada eth0-nya kita gunakan command:

pada h1 : `tcpdump -ne -i h1-eth0`
pada h2 : `tcpdump -ne -i h2-eth0`
pada h3 : `tcpdump -ne -i h3-eth0`

Kemudian, lakukan ping dari h1 ke h2 :

`h1 ping -c 1 h2`

Pada h1 akan didapat dumping paket dibawah ini:

```
root@mininet-vm:~# tcpdump -ne -i h1-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h1-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
17:22:04.953627 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo request, id 2655, seq 1, length 64
17:22:09.960860 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
17:22:10.960859 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
17:22:11.961977 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
```

Selanjutnya kita lihat dumping paket pada h2

```
root@mininet-vm:~# tcpdump -ne -i h2-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h2-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
17:22:04.954317 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo request, id 2655, seq 1, length 64
17:22:04.954365 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo reply, id 2655, seq 1, length 64
17:22:09.960887 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
17:22:09.961260 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
17:22:09.961287 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
17:22:10.960928 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
17:22:10.960948 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
17:22:11.962057 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
17:22:11.962079 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
```

Maka dapat dituliskan sequence diagram berikut:

```sequence
participant h1
participant s1
participant h2
participant h3
h1->s1:ICMP Request
h1->s1:ARP Request
s1-->h2:ARP Request
h1->s1:ARP Request
s1-->h2:ARP Request
h1->s1:ARP Request
s1-->h2:ARP Request
s1-->h2:ICMP Request
h2-->s1:ICMP Reply
h2-->s1:ARP Reply
h2-->s1:ARP Reply
h2-->s1:ARP Reply
```

*Skenario 2*
Masih menggunakan 1 flow table yang sama, kita lakukan tes komunikasi dari h2 ke h1. Jangan lupa untuk xterm h1,h2 dan h3, kemudian amati paket yang datang dan keluar di setiap interface host dengan melakukan dumping.

Maka didapatkan dumping paket hanya pada h2 seperti di bawah ini
```
root@mininet-vm:~# tcpdump -ne -i h2-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h2-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
17:23:37.849256 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo request, id 2687, seq 1, length 64
17:23:42.856869 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
17:23:43.856866 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
17:23:44.856858 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
```

Bisa dituliskan sequence-diagramnya sebagai berikut :

```sequence
participant h1
participant s1
participant h2
participant h3
h2->s1:ICMP Request
h2-->s1:ARP Request
h2-->s1:ARP Request
h2-->s1:ARP Request
```


*Tahap selanjutnya kita akan menambahkan flow pada MAC Address h2 ke port1 pada s1*
**Tambahkan flow menggunakan command berikut**

`sudo ovs-ofctl add-flow s1 dl_src=00:00:00:00:00:02,actions=output:1`

Maka flow-table menjadi seperti di bawah ini:

```
sudo ovs-ofctl dump-flows s1

NXST_FLOW reply (xid=0x4):
 cookie=0x0, duration=3.896s, table=0, n_packets=0, n_bytes=0, idle_age=3, dl_src=00:00:00:00:00:01 actions=output:2
 cookie=0x0, duration=13.064s, table=0, n_packets=0, n_bytes=0, idle_age=13, dl_src=00:00:00:00:00:02 actions=output:1
```

*Skenario 1*
Sama seperti skenario 1 sebelumnya, kita akan melakukan tes komunikasi antara h1 ke h2. Jangan lupa untuk melakukan xterm h1, h2 , dan h3 untuk mengamati paket yang melewati interface tiap host.

Setelah itu lakukan command ping :
`h1 ping -c 1 h2`

Maka pada h1 didapat paket dumping di bawah ini :
```
root@mininet-vm:~# tcpdump -ne -i h1-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h1-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
17:17:24.812247 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo request, id 2561, seq 1, length 64
17:17:24.813084 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo reply, id 2561, seq 1, length 64
17:17:29.817741 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
17:17:29.818296 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
17:17:29.818312 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
17:17:29.819142 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
```

Dan pada h2 didapat paket dumping di bawah ini :

```
root@mininet-vm:~# tcpdump -ne -i h2-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h2-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
17:17:24.812482 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo request, id 2561, seq 1, length 64
17:17:24.812507 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo reply, id 2561, seq 1, length 64
17:17:29.817770 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
17:17:29.818108 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
17:17:29.818164 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
17:17:29.819058 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
```

Maka bisa ditulis sequence-diagram berikut:

```sequence
participant h1
participant s1
participant h2
participant h3
h1->s1: ICMP Request
s1-->h2:ICMP Request
h2-->s1: ICMP Reply
s1->h1:ICMP Reply
h1->s1: ARP Request
s1-->h2:ARP Request
h2-->s1:ARP Reply
s1->h1: ARP Reply
h2-->s1: ARP Request
s1->h1:ARP Request
h1->s1:ARP Reply
s1-->h2: ARP Reply
```
Terlihat ICMP Reply sudah bisa diteruskan ke h1

*Skenario 2*
Sama seperti skenario 2 sebelumnya, kita lakukan tes komunikasi dari h2 ke h1:

`h2 ping -c 1 h1`

Pada h1 didapat dumping paket berikut :
```
root@mininet-vm:~# tcpdump -ne -i h1-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h1-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
17:19:05.147539 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo request, id 2594, seq 1, length 64
17:19:05.147565 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo reply, id 2594, seq 1, length 64
17:19:10.152869 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
17:19:10.153440 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
17:19:10.153466 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
17:19:10.154335 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
```

Pada h2 didapat dumping paket berikut :
```
root@mininet-vm:~# tcpdump -ne -i h2-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h2-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
17:19:05.147295 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo request, id 2594, seq 1, length 64
17:19:05.148178 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo reply, id 2594, seq 1, length 64
17:19:10.152841 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
17:19:10.153258 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
17:19:10.153293 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
17:19:10.153707 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
```

Maka dapat ditulis sequence-diagramnya:

```sequence
participant h1
participant s1
participant h2
participant h3
h2-->s1: ICMP Request
s1->h1:ICMP Request
h1->s1: ICMP Reply
s1-->h2:ICMP Reply
h1->s1:ARP Request
s1-->h2: ARP Request
h2-->s1: ARP Request
s1->h1:ARP Request
h1->s1:ARP Reply
s1-->h2: ARP Reply
h2-->s1: ARP Reply
s1->h1:ARP Reply
```
Terlihat bahwa ICMP Reply sudah bisa diteruskan ke h2

----------

**Flow Matching Based IP (L3)**
### IP Based Flow Matching
Selanjutnya, kita akan melakukan flow matching berdasarkan IP Address
Jangan lupa lakukan `sudo ovs-ofctl del-flows s1` untuk menghapus flow table sebelumnya yang telah dibuat.

Tambahkan flow di bawah ini :

```
sudo ovs-ofctl add-flow s1 priority=500,dl_type=0x800,nw_src=10.0.0.0/24,nw_dst=10.0.0.0/24,actions=normal
sudo ovs-ofctl add-flow s1 arp,nw_dst=10.0.0.1,actions=output:1
sudo ovs-ofctl add-flow s1 arp,nw_dst=10.0.0.2,actions=output:2
```

Maka akan terlihat flow table di bawah ini :

```
mininet@mininet-vm:~$ sudo ovs-ofctl dump-flows s1
NXST_FLOW reply (xid=0x4):
 cookie=0x0, duration=55.291s, table=0, n_packets=0, n_bytes=0, idle_age=12, arp,arp_tpa=10.0.0.2 actions=output:2
 cookie=0x0, duration=59.466s, table=0, n_packets=0, n_bytes=0, idle_age=15, arp,arp_tpa=10.0.0.1 actions=output:1
 cookie=0x0, duration=35.08s, table=0, n_packets=0, n_bytes=0, idle_age=27, priority=500,ip,nw_src=10.0.0.0/24,nw_dst=10.0.0.0/24 actions=NORMAL
```
Pada flow table terlihat bahwa pada s1 akan meneruskan paket arp ke h1 pada port1 dan ke h2 pada port2
dan juga s1 akan bertindak secara normal untuk meneruskan paket dari ip address 10.0.0.0/24 ke 10.0.0.0/24 (melihat berdasarkan ip address)

*Skenario 1*
`h1 ping -c 1 h2`

Maka tampilan dumping paket pada h1 sebagai berikut
```
root@mininet-vm:~# tcpdump -ne -i h1-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h1-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
08:13:59.851916 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo request, id 1772, seq 1, length 64
08:13:59.853213 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo reply, id 1772, seq 1, length 64
08:14:04.858758 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
08:14:04.858780 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
```

Maka tampilan dumping paket pada h2 sebagai berikut
```
root@mininet-vm:~# tcpdump -ne -i h2-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h2-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
08:13:59.852170 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo request, id 1772, seq 1, length 64
08:13:59.852198 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo reply, id 1772, seq 1, length 64
08:14:04.858413 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
08:14:04.859596 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
```
Maka bisa dituliskan sequence diagram di bawah ini :

```sequence
participant h1
participant s1
participant h2
participant h3
h1->s1:ICMP Request
h1->s1:ARP Request
h2-->s1:ICMP Reply
s1->h1:ICMP Reply
h2-->s1:ARP Request
s1->h1:ARP Request
h1->s1:ARP Reply
s1-->h2:ARP Reply
```



*Skenario2*
Kemudian kita akan melakukan tes komunikasi dari h2 ke h1 dengan mengirim sebuah paket ICMP
`h2 ping -c 1 h1`

Maka hasil dumping paket pada h1 sebagai berikut
```
root@mininet-vm:~# tcpdump -ne -i h1-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h1-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
08:15:18.845775 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo request, id 1800, seq 1, length 64
08:15:18.845801 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo reply, id 1800, seq 1, length 64
08:15:23.850420 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
08:15:23.850944 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
08:15:23.850959 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
08:15:23.851765 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
```

Dan hasil dumping paket pada h2 sebagai berikut
```
root@mininet-vm:~# tcpdump -ne -i h2-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h2-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
08:15:18.845473 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo request, id 1800, seq 1, length 64
08:15:18.846388 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo reply, id 1800, seq 1, length 64
08:15:23.850396 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
08:15:23.850803 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
08:15:23.850830 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Reply 10.0.0.2 is-at 00:00:00:00:00:02, length 28
08:15:23.851181 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
```

Maka bisa dituliskan sequence diagram di bawah ini :

```sequence
participant h1
participant s1
participant h2
participant h3
h2-->s1:ICMP Request
s1->h1:ICMP Request
h1->s1:ICMP Reply
s1-->h2:ICMP Reply
h2-->s1:ARP Request
s1->h1:ARP Request
h1->s1:ARP Request
s1-->h2:ARP Request
h2-->s1:ARP Reply
s1->h1:ARP Reply
h1->s1:ARP Reply
s1-->h2:ARP Reply
```

----------

**TRANSPORT BASED FLOW MATCHING WITH SIMPLE HTTP SERVER**
### FLow Matching Based on Transport Based
Selanjutnya, kita akan melakukan flow matching berdasarkan Transport Based dan mencoba mengaplikasikan request paket ke http server yang diinstall di salah satu host.
Jangan lupa lakukan `sudo ovs-ofctl del-flows s1` untuk menghapus flow table sebelumnya yang telah dibuat.

Menambahkan flow data pada switch dari HTTP server.

`sudo ovs-ofctl add-flow s1 arp,actions=normal`
`sudo ovs-ofctl add-flow s1 priority=500,dl_type=0x800,nw_proto=6,tp_dst=80,actions=output:1`
`sudo ovs-ofctl add-flow s1 priority=800,ip,nw_src=10.0.0.1,actions=normal`


```
NXST_FLOW reply (xid=0x4):
 cookie=0x0, duration=336.177s, table=0, n_packets=22, n_bytes=1680, idle_age=168, priority=500,tcp,tp_dst=80 actions=output:1
 cookie=0x0, duration=335.335s, table=0, n_packets=22, n_bytes=3472, idle_age=168, priority=800,ip,nw_src=10.0.0.1 actions=NORMAL
 cookie=0x0, duration=336.237s, table=0, n_packets=0, n_bytes=0, idle_age=336, arp actions=NORMAL
```



mininet> h1 python -m SimpleHTTPServer 80 &
mininet> h2 wget -O - h1
```
--2014-11-04 08:21:54--  http://10.0.0.1/
Connecting to 10.0.0.1:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 848 [text/html]
Saving to: ‘STDOUT’
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 3.2 Final//EN"><html>
<title>Directory listing for /</title>
<body>
<h2>Directory listing for /</h2>
<hr>
<ul>
<li><a href=".bash_history">.bash_history</a>
<li><a href=".bash_logout">.bash_logout</a>
<li><a href=".bashrc">.bashrc</a>
<li><a href=".cache/">.cache/</a>
<li><a href=".gitconfig">.gitconfig</a>
<li><a href=".nano_history">.nano_history</a>
<li><a href=".profile">.profile</a>
<li><a href=".rnd">.rnd</a>
<li><a href=".wireshark/">.wireshark/</a>
<li><a href=".Xauthority">.Xauthority</a>
<li><a href="install-mininet-vm.sh">install-mininet-vm.sh</a>
<li><a href="mininet/">mininet/</a>
<li><a href="of-dissector/">of-dissector/</a>
<li><a href="oflops/">oflops/</a>
<li><a href="oftest/">oftest/</a>
<li><a href="openflow/">openflow/</a>
<li><a href="pox/">pox/</a>
</ul>
<hr>
</body>
</html>

     0K                                                       100% 29.2M=0s

2014-11-04 08:21:54 (29.2 MB/s) - written to stdout [848/848]
```

Kemudian didapat hasil dumping paket pada h1

```
root@mininet-vm:~# tcpdump -ne -i h1-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h1-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
08:21:54.832433 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 74: 10.0.0.2.56089 > 10.0.0.1.80: Flags [S], seq 165414125, win 29200, options [mss 1460,sackOK,TS val 182109 ecr 0,nop,wscale 9], length 0
08:21:54.832461 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 74: 10.0.0.1.80 > 10.0.0.2.56089: Flags [S.], seq 2632645245, ack 165414126, win 28960, options [mss 1460,sackOK,TS val 182109 ecr 182109,nop,wscale 9], length 0
08:21:54.833131 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 1, win 58, options [nop,nop,TS val 182109 ecr 182109], length 0
08:21:54.836282 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 172: 10.0.0.2.56089 > 10.0.0.1.80: Flags [P.], seq 1:107, ack 1, win 58, options [nop,nop,TS val 182110 ecr 182109], length 106
08:21:54.836357 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 66: 10.0.0.1.80 > 10.0.0.2.56089: Flags [.], ack 107, win 57, options [nop,nop,TS val 182110 ecr 182110], length 0
08:21:54.838179 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 83: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 1:18, ack 107, win 57, options [nop,nop,TS val 182110 ecr 182110], length 17
08:21:54.838219 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 18, win 58, options [nop,nop,TS val 182110 ecr 182110], length 0
08:21:54.838487 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 103: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 18:55, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182110], length 37
08:21:54.838524 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 55, win 58, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.838704 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 103: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 55:92, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182111], length 37
08:21:54.838740 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 92, win 58, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.838880 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 106: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 92:132, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182111], length 40
08:21:54.838909 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 132, win 58, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.839000 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 87: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 132:153, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182111], length 21
08:21:54.839023 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 153, win 58, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.839204 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 68: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 153:155, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182111], length 2
08:21:54.839244 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 155, win 58, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.839426 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 914: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 155:1003, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182111], length 848
08:21:54.839463 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 1003, win 61, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.839626 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 66: 10.0.0.1.80 > 10.0.0.2.56089: Flags [F.], seq 1003, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.845386 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [F.], seq 107, ack 1004, win 61, options [nop,nop,TS val 182112 ecr 182111], length 0
08:21:54.845413 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 66: 10.0.0.1.80 > 10.0.0.2.56089: Flags [.], ack 108, win 57, options [nop,nop,TS val 182112 ecr 182112], length 0
```

Dan juga didapat hasil dumping paket pada h2

```
root@mininet-vm:~# tcpdump -ne -i h2-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h2-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
08:21:54.832188 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 74: 10.0.0.2.56089 > 10.0.0.1.80: Flags [S], seq 165414125, win 29200, options [mss 1460,sackOK,TS val 182109 ecr 0,nop,wscale 9], length 0
08:21:54.833081 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 74: 10.0.0.1.80 > 10.0.0.2.56089: Flags [S.], seq 2632645245, ack 165414126, win 28960, options [mss 1460,sackOK,TS val 182109 ecr 182109,nop,wscale 9], length 0
08:21:54.833120 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 1, win 58, options [nop,nop,TS val 182109 ecr 182109], length 0
08:21:54.836260 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 172: 10.0.0.2.56089 > 10.0.0.1.80: Flags [P.], seq 1:107, ack 1, win 58, options [nop,nop,TS val 182110 ecr 182109], length 106
08:21:54.836371 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 66: 10.0.0.1.80 > 10.0.0.2.56089: Flags [.], ack 107, win 57, options [nop,nop,TS val 182110 ecr 182110], length 0
08:21:54.838199 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 83: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 1:18, ack 107, win 57, options [nop,nop,TS val 182110 ecr 182110], length 17
08:21:54.838214 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 18, win 58, options [nop,nop,TS val 182110 ecr 182110], length 0
08:21:54.838501 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 103: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 18:55, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182110], length 37
08:21:54.838518 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 55, win 58, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.838720 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 103: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 55:92, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182111], length 37
08:21:54.838734 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 92, win 58, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.838893 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 106: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 92:132, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182111], length 40
08:21:54.838904 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 132, win 58, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.839009 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 87: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 132:153, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182111], length 21
08:21:54.839018 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 153, win 58, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.839222 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 68: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 153:155, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182111], length 2
08:21:54.839237 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 155, win 58, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.839443 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 914: 10.0.0.1.80 > 10.0.0.2.56089: Flags [P.], seq 155:1003, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182111], length 848
08:21:54.839457 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [.], ack 1003, win 61, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.839690 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 66: 10.0.0.1.80 > 10.0.0.2.56089: Flags [F.], seq 1003, ack 107, win 57, options [nop,nop,TS val 182111 ecr 182111], length 0
08:21:54.845351 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 66: 10.0.0.2.56089 > 10.0.0.1.80: Flags [F.], seq 107, ack 1004, win 61, options [nop,nop,TS val 182112 ecr 182111], length 0
08:21:54.845440 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 66: 10.0.0.1.80 > 10.0.0.2.56089: Flags [.], ack 108, win 57, options [nop,nop,TS val 182112 ecr 182112], length 0
```

Maka, dapat dituliskan flow diagramnya

###Sequence Diagram Http Request Protocol TCP Base flow matching

** h2 wget -O - h1 **

```sequence
participant h2
participant s1
participant h1
participant h3
h2->s1:SYN
s1-->h1:SYN
h1-->s1:ACK
s1->h2:ACK
h2->s1:ACK
s1-->h1:ACK
h2->s1:PUSH
s1-->h1:PUSH
h1-->s1:ACK
s1->h2:ACK
h1-->s1:PUSH
s1->h2:PUSH
h2->s1:ACK
s1-->h1:ACK
h1-->s1:PUSH
s1->h2:PUSH
h2->s1:ACK
s1-->h1:ACK
h1-->s1:PUSH
s1->h2:PUSH
h2->s1:ACK
s1-->h1:ACK
h1-->s1:PUSH
s1->h2:PUSH
h2->s1:ACK
s1-->h1:ACK
h1-->s1:PUSH
s1->h2:PUSH
h2->s1:ACK
s1-->h1:ACK
h1-->s1:PUSH
s1->h2:PUSH
h2->s1:ACK
s1-->h1:ACK
h1-->s1:PUSH
s1->h2:PUSH
h2->s1:ACK
s1-->h1:ACK
h1-->s1:FIN
s1->h2:FIN
h2->s1:FIN
s1-->h1:FIN
h1-->s1:ACK
s1->h2:ACK
```

##Referensi

1. TBA
2. ...

##Lisensi
*CC Attribution-NonCommercial-NoDerivatives*
[(Lisensi)](http://creativecommons.org/licenses/by-nc-nd/4.0/)
