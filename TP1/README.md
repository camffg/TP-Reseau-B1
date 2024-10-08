# TP1 : Les premiers pas de bébé B1

##Sommaire

- [TP1 : Les premiers pas de bébé B1](#tp1--les-premiers-pas-de-bébé-b1)
  - [Sommaire](#sommaire)
- [I. Récolte d'informations](#i-récolte-dinformations)
- [II. Utiliser le réseau](#ii-utiliser-le-réseau)
- [III. Sniffer le réseau](#iii-sniffer-le-réseau)
- [IV. Network scanning et adresses IP](#iv-network-scanning-et-adresses-ip)

# I. Récolte d'informations

🌞 **Adresses IP de ta machine**

```bsh
ifconfig
	inet 10.33.78.91
    (Mac)
```


🌞 **Si t'as un accès internet normal, d'autres infos sont forcément dispos...**

```bsh
ifconfig
broadcast 10.33.79.254
Server:		8.8.8.8
Address:	8.8.8.8#53
l'adresse IP du serveur DHCP: 10.33.79.254
```

🌟 **BONUS** : Détermine s'il y a un pare-feu actif sur ta machine

```bsh
/usr/libexec/ApplicationFirewall/socketfilterfw --getglobalstate --getblockall --getallowsigned --getstealthmode.

Firewall is disabled. (State = 0)
Block all DISABLED! 
```
# II. Utiliser le réseau

🌞 **Envoie un `ping` vers...**

```bsh
 ping 10.33.78.91
 64 bytes from 10.33.78.91: icmp_seq=0 ttl=64 time=0.202 ms
64 bytes from 10.33.78.91: icmp_seq=1 ttl=64 time=0.179 ms

ping 127.0.0.1
64 bytes from 127.0.0.1: icmp_seq=0 ttl=64 time=0.112 ms
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.117 ms
```

🌞 **On continue avec `ping`.** Envoie un `ping` vers...

```bsh
 PING 10.33.79.254 (10.33.79.254): 56 data bytes
Request timeout for icmp_seq 0

ping 10.33.78.96
64 bytes from 10.33.78.96: icmp_seq=0 ttl=64 time=122.084 ms
64 bytes from 10.33.78.96: icmp_seq=1 ttl=64 time=139.493 ms
```

🌞 **Faire une requête DNS à la main**

```bsh
 nslookup riotgmes.com
 
 Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
Name:	riotgmes.com
Address: 77.247.179.90


 ````


# III. Sniffer le réseau

🌞 **J'attends dans le dépôt git de rendu un fichier `ping.pcap`**

🌞 **Livrez un deuxième fichier : `dns.pcap`**

[Capture Wireshark](ip.pcap)

# IV. Network scanning et adresses IP

🌞 **Effectue un scan du réseau auquel tu es connecté**

 ```bsh
 nmap -sn -PR  10.33.78.91  
Starting Nmap 7.95 ( https://nmap.org ) at 2024-10-07 11:24 CEST
Nmap scan report for 10.33.78.91
Host is up (0.00033s latency).
Nmap done: 1 IP address (1 host up) scanned in 0.17 seconds
cameronlefevre@Ordinateur-portable-de-cameron ~ % 
```

🌞 **Changer d'adresse IP**

```bsh
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=6460<TSO4,TSO6,CHANNEL_IO,PARTIAL_CSUM,ZEROINVERT_CSUM>
	ether 50:a6:d8:9b:0b:a7
	inet6 fe80::18ba:907e:34e9:7acc%en0 prefixlen 64 secured scopeid 0xc 
	inet 10.33.78.92 netmask 0xfffff000 broadcast 10.33.79.255
	nd6 options=201<PERFORMNUD,DAD>
  ```