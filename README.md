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
