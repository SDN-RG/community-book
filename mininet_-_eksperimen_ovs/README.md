# Mininet - Eksperimen OVS

*Bagus Aditya, Hamzah U. Mustakim*

##Port-Based
###Create Single Topology at Mininet

`sudo mn --mac --topo single,3 --switch ovsk --controller=remote`

```bash
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

`mininet> net`
```bash
h1 h1-eth0:s1-eth1
h2 h2-eth0:s1-eth2
h3 h3-eth0:s1-eth3
s1 lo:  s1-eth1:h1-eth0 s1-eth2:h2-eth0 s1-eth3:h3-eth0
c0
```

`mininet> dump`
```bash
<Host h1: h1-eth0:10.0.0.1 pid=3814>
<Host h2: h2-eth0:10.0.0.2 pid=3815>
<Host h3: h3-eth0:10.0.0.3 pid=3816>
<OVSSwitch s1: lo:127.0.0.1,s1-eth1:None,s1-eth2:None,s1-eth3:None pid=3820>
<RemoteController c0: 127.0.0.1:6633 pid=3807>
```

`sudo ovs-ofctl dump-flows s1`
```bash
NXST_FLOW reply (xid=0x4):
```



###Add flow at Port 1
`sudo ovs-ofctl add-flow s1 in_port=1,action=output:2`
`sudo ovs-ofctl dump-flows s1`
```bash
NXST_FLOW reply (xid=0x4):
 cookie=0x0, duration=3.237s, table=0, n_packets=0, n_bytes=0, idle_age=3, in_port=1 actions=output:2
```


`h1 ping -c 1 h2`

At h1 `tcpdump -ne -i h1-eth0`
```bash
root@mininet-vm:~# tcpdump -ne -i h1-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h1-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
06:53:04.725955 00:00:00:00:00:01 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
06:53:05.724128 00:00:00:00:00:01 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
06:53:06.724140 00:00:00:00:00:01 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.2 tell 10.0.0.1, length 28
```

At h2 `tcpdump -ne -i h2-eth0`
```bash
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


`mininet> h2 ping -c 1 h1`

At h1 `tcpdump -ne -i h1-eth0`


At h2 `tcpdump -ne -i h2-eth0`
```bash
root@mininet-vm:~# tcpdump -ne -i h2-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h2-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
06:55:54.707010 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo request, id 4210, seq 1, length 64
06:55:59.720120 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
06:56:00.720102 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
06:56:01.721048 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28

```


###Add Next flow at Port 2:

`sudo ovs-ofctl add-flow s1 in_port=2,action=output:1`
`sudo ovs-ofctl dump-flows s1`
```bash
NXST_FLOW reply (xid=0x4):
 cookie=0x0, duration=376.441s, table=0, n_packets=9, n_bytes=378, idle_age=131, in_port=1 actions=output:2
 cookie=0x0, duration=2.199s, table=0, n_packets=0, n_bytes=0, idle_age=2, in_port=2 actions=output:1
```

`h1 ping -c 1 h2`
At h1 `tcpdump -ne -i h1-eth0`

```bash
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

At h2 `tcpdump -ne -i h2-eth0`
```bash
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

`h2 ping -c 1 h1`
`tcpdump -ne -i h1-eth0`
```bash
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

at h2
```bash
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

###Sequence Diagram  Port-Based
1st Scenario :
- `h1 ping -c 1 h2`
- `add-flow s1 in_port=1 action=output:2`

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

2nd Scenario :
- `h2 ping -c 1 h1`
- `add-flow s1 in_port=1 action=output:2`

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

1st Scenario :
- `h1 ping -c 1 h2`
- `add-flow s1 in_port=1 action=output:2`
- `add-flow s1 in_port=2,action=output:1`

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

2nd scenario :
- `h2 ping -c 1 h1`
- `add-flow s1 in_port=1 action=output:2`
- `add-flow s1 in_port=2,action=output:1`

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

##IP-Based
###Adding Flows
```bash
sudo ovs-ofctl add-flow s1 priority=500,dl_type=0x800,nw_src=10.0.0.0/24,nw_dst=10.0.0.0/24,actions=normal
sudo ovs-ofctl add-flow s1 arp,nw_dst=10.0.0.1,actions=output:1
sudo ovs-ofctl add-flow s1 arp,nw_dst=10.0.0.2,actions=output:2
```

```bash
mininet@mininet-vm:~$ sudo ovs-ofctl dump-flows s1
NXST_FLOW reply (xid=0x4):
 cookie=0x0, duration=55.291s, table=0, n_packets=8, n_bytes=336, idle_age=12, arp,arp_tpa=10.0.0.2 actions=output:2
 cookie=0x0, duration=59.466s, table=0, n_packets=5, n_bytes=210, idle_age=15, arp,arp_tpa=10.0.0.1 actions=output:1
 cookie=0x0, duration=35.08s, table=0, n_packets=5, n_bytes=490, idle_age=27, priority=500,ip,nw_src=10.0.0.0/24,nw_dst=10.0.0.0/24 actions=NORMAL
```

Display at h1 (`h1 ping -c 1 h2`)

```bash
root@mininet-vm:~# tcpdump -ne -i h1-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h1-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
08:13:59.851916 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo request, id 1772, seq 1, length 64
08:13:59.853213 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo reply, id 1772, seq 1, length 64
08:14:04.858758 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
08:14:04.858780 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
```

Display at h2 (`h1 ping -c 1 h2`)
```bash
root@mininet-vm:~# tcpdump -ne -i h2-eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on h2-eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
08:13:59.852170 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype IPv4 (0x0800), length 98: 10.0.0.1 > 10.0.0.2: ICMP echo request, id 1772, seq 1, length 64
08:13:59.852198 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype IPv4 (0x0800), length 98: 10.0.0.2 > 10.0.0.1: ICMP echo reply, id 1772, seq 1, length 64
08:14:04.858413 00:00:00:00:00:02 > 00:00:00:00:00:01, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.1 tell 10.0.0.2, length 28
08:14:04.859596 00:00:00:00:00:01 > 00:00:00:00:00:02, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at 00:00:00:00:00:01, length 28
```

Display at h1 (`h2 ping -c 1 h1`)

```bash
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

Display at h2 (`h2 ping -c 1 h1`)
```bash
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
###Sequence Diagram  IP-Based
...

##TCP-Based
```bash
sudo ovs-ofctl add-flow s1 arp,actions=normal
sudo ovs-ofctl add-flow s1 priority=500,dl_type=0x800,nw_proto=6,tp_dst=80,actions=output:1
sudo ovs-ofctl add-flow s1 priority=800,ip,nw_src=10.0.0.1,actions=normal
```
```bash
NXST_FLOW reply (xid=0x4):
 cookie=0x0, duration=336.177s, table=0, n_packets=22, n_bytes=1680, idle_age=168, priority=500,tcp,tp_dst=80 actions=output:1
 cookie=0x0, duration=335.335s, table=0, n_packets=22, n_bytes=3472, idle_age=168, priority=800,ip,nw_src=10.0.0.1 actions=NORMAL
 cookie=0x0, duration=336.237s, table=0, n_packets=0, n_bytes=0, idle_age=336, arp actions=NORMAL
```
```bash
mininet> h1 python -m SimpleHTTPServer 80 &
mininet> h2 wget -O - h1
```
```bash
--2014-11-04 08:21:54--  http://10.0.0.1/
Connecting to 10.0.0.1:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 848 [text/html]
Saving to: â€˜STDOUTâ€™
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

Display Xterm at h1
```bash
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

at h2
```bash
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
###Sequence Diagram TCP-Based

`h2 wget -O - h1`

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
