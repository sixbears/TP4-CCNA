# TP4-CCNA

## 3. Mise en place du routage statique

### sur router1 :

* Activer l'IPv4 Forwarding (= transformer la machine en routeur)

* Désactiver le firewall (pour éviter certaines actions non voulues)

* Affichage des route de la VM router1tp4.b1   
`10.1.0.0/24 dev enp0s8 proto kernel scope link src 10.1.0.254 metric 100
10.2.0.0/24 dev enp0s9 proto kernel scope link src 10.2.0.254 metric 101`

### Sur client1 :

* Affichage des route de la VM client1tp4.b1
10.1.0.0/24 dev enp0s8 proto kernel scope link src 10.1.0.10 metric 100
10.2.0.0/24 via 10.1.0.254 dev enp0s8 proto static metric 100


### Sur server1 :

* Affichage des route de la VM server1tp4.b1
10.1.0.0/24 via 10.2.0.254 dev enp0s8
10.2.0.0/24 dev enp0s8 proto kernel scope link src 10.2.0.10 metric 100


### Test :

* Traceroute depuis la vmclient1   
`[root@client1tp4 ~]# traceroute 10.2.0.10
traceroute to 10.2.0.10 (10.2.0.10), 30 hops max, 60 byte packets
 1  router1tp4.b1 (10.1.0.254)  0.367 ms  0.291 ms  0.155 ms
 2  10.2.0.10 (10.2.0.10)  0.451 ms !X  0.379 ms !X  0.266 ms !X
`

# Spéléologie réseau

## ARP

### Manip 1

* On vide les table arp des machines avec    
`sudo ip neigh flush all`

* Client1
`[root@client1 ~]# ip neigh show
10.1.0.1 dev enp0s8 lladdr 0a:00:27:00:00:08 REACHABLE
`
C'est le réseau auquel appartient la VM.

* Server1
`[root@server1 ~]# ip neigh show
10.2.0.1 dev enp0s8 lladdr 0a:00:27:00:00:04 REACHABLE
`
C'est le réseau auquel appartient la VM.

10.2.0.10<---ping--->10.1.0.10

* Client1
`[root@client1 ~]# ip neigh show
10.1.0.254 dev enp0s8 lladdr 08:00:27:57:90:8c REACHABLE
10.1.0.1 dev enp0s8 lladdr 0a:00:27:00:00:08 REACHABLE
`
Pour ping 10.2.0.10 le Client1 a besoin de passer par le Routeur1.

* Server1
`[root@server1 ~]# ip neigh show
10.2.0.254 dev enp0s8 lladdr 08:00:27:65:02:5b STALE
10.2.0.1 dev enp0s8 lladdr 0a:00:27:00:00:04 DELAY`
Pareil, pour renvoyer le ping, le Server1 a besoin de passer par le Routeur1

### Manip 2

* Routeur1
`[root@router1tp4 ~]# ip neigh show
10.1.0.1 dev enp0s8 lladdr 0a:00:27:00:00:08 DELAY
`

`10.1.0.1 dev enp0s8 lladdr 0a:00:27:00:00:08 DELAY
10.2.0.10 dev enp0s9 lladdr 08:00:27:f0:27:49 REACHABLE
10.1.0.10 dev enp0s8 lladdr 08:00:27:e2:39:c4 REACHABLE`
