# TP3 : 32°13'34"N 95°03'27"W

## Sommaire

- [TP3 : 32°13'34"N 95°03'27"W](#tp3--321334n-950327w)
  - [Sommaire](#sommaire)
- [0. Prérequis](#0-prérequis)
- [I. ARP basics](#i-arp-basics)
- [II. ARP dans un réseau local](#ii-arp-dans-un-réseau-local)
  - [1. Basics](#1-basics)
  - [2. ARP](#2-arp)
  - [3. Bonus : ARP poisoning](#3-bonus--arp-poisoning)

# 0. Prérequis

 **tout se fait en ligne de commande**, sauf si c'est explicitement précisément autrement
- je tente un truc risqué, ceux du matin, vous servez de crash test
  - **vous allez utiliser vos partages de co dans la partie II**
  - **partie II qui se fait à 3 ou 4** (pas + SVP, c'est déjà le cirque à 4)
  - si ça fonctionne pas ce sera backup plan avec les câbles :c



# I. ARP basics

☀️ **Avant de continuer...**

- affichez l'adresse MAC de votre carte WiFi !

```bsh
cameronlefevre@Ordinateur-portable-de-cameron ~ % ifconfig
ether 50:a6:d8:9b:0b:a7
```

☀️ **Affichez votre table ARP**

- allez vous commencez à devenir grands, je vous donne pas la commande, demande à m'sieur internet !

☀️ **Déterminez l'adresse MAC de la passerelle du réseau de l'école**

- la passerelle, vous connaissez son adresse IP normalement (cf TP1 ;) )
- si vous avez un accès internet, votre PC a forcément l'adresse MAC de la passerelle dans sa table ARP

```bsh
cameronlefevre@Ordinateur-portable-de-cameron ~ % netstat -nr 
default            10.33.79.254       UGScg                 en0       

cameronlefevre@Ordinateur-portable-de-cameron ~ % arp -a
? (10.33.79.254) at 7c:5a:1c:d3:d8:76 on en0 ifscope [ethernet]
```

☀️ **Supprimez la ligne qui concerne la passerelle**

- une commande pour supprimer l'adresse MAC de votre table ARP
- si vous ré-affichez votre table ARP, y'a des chances que ça revienne presque tout de suite !

```bsh
cameronlefevre@Ordinateur-portable-de-cameron ~ % sudo arp -d 10.33.79.254
```

☀️ **Prouvez que vous avez supprimé la ligne dans la table ARP**

- en affichant la table ARP
- si la ligne est déjà revenue, déconnecte-toi temporairement du réseau de l'école, et supprime-la de nouveau

```bsh
cameronlefevre@Ordinateur-portable-de-cameron ~ % arp -a
[...]      
? (10.33.64.125) at 68:7a:64:b0:da:f1 on en0 ifscope [ethernet]
? (10.33.64.143) at 4e:27:88:bf:33:d6 on en0 ifscope [ethernet]
? (10.33.64.144) at b2:26:1b:d3:78:cc on en0 ifscope [ethernet]
? (10.33.64.147) at be:53:a:50:fc:a7 on en0 ifscope [ethernet]
? (10.33.64.180) at 8c:f8:c5:fc:8b:1d on en0 ifscope [ethernet]
? (10.33.64.193) at e2:3f:55:5e:69:3f on en0 ifscope [ethernet]
? (10.33.64.194) at 9e:7a:d1:8a:f7:c9 on en0 ifscope [ethernet]
[...]
```

☀️ **Wireshark**

- capture `arp1.pcap`

 [Capture](arp1.pcap)

  # II. ARP dans un réseau local

  Dans cette situation, le téléphone agit comme une "box" :

- **c'est un switch**
  - il permet à tout le monde d'être connecté à un même réseau local
- **c'est aussi un routeur**
  - il fait passer les paquets du réseau local, à internet
  - et vice-versa
  - on dit que ce routeur est la **passerelle** du réseau local

## 1. Basics

☀️ **Déterminer**

- pour la carte réseau impliquée dans le partage de connexion (carte WiFi ?)
- son adresse IP au sein du réseau local formé par le partage de co
- son adresse MAC

```bsh
cameronlefevre@Ordinateur-portable-de-cameron ~ % ifconfig
inet 172.20.10.8 
ether 50:a6:d8:9b:0b:a7  
```

☀️ **DIY**

- changer d'adresse IP
  - vous pouvez le faire en interface graphique
- faut procéder comme au TP1 :
  - vous respectez les informations que vous connaissez du réseau
  - remettez la même passerelle, le même masque, et le même serveur DNS
  - changez seulement votre adresse IP, ne changez que le dernier nombre
- prouvez que vous avez bien changé d'IP
  - avec une commande !

  ```bsh
  cameronlefevre@Ordinateur-portable-de-cameron ~ % ifconfig

  inet 172.20.10.12
  ```

☀️ **Pingz !**

- vérifiez que vous pouvez tous vous `ping` avec ces adresses IP
- vérifiez avec une commande `ping` que vous avez bien un accès internet

```bsh
cameronlefevre@Ordinateur-portable-de-cameron ~ % ping 172.20.10.9
PING 172.20.10.9 (172.20.10.9): 56 data bytes
64 bytes from 172.20.10.9: icmp_seq=0 ttl=128 time=44.784 ms
64 bytes from 172.20.10.9: icmp_seq=1 ttl=128 time=22.222 ms

cameronlefevre@Ordinateur-portable-de-cameron ~ % ping 172.20.10.11
PING 172.20.10.11 (172.20.10.11): 56 data bytes
64 bytes from 172.20.10.11: icmp_seq=0 ttl=128 time=26.530 ms
64 bytes from 172.20.10.11: icmp_seq=1 ttl=128 time=10.326 ms

cameronlefevre@Ordinateur-portable-de-cameron ~ % ping hentaiheaven.com
PING hentaiheaven.com (34.225.210.0): 56 data bytes
64 bytes from 34.225.210.0: icmp_seq=0 ttl=59 time=82.079 ms
64 bytes from 34.225.210.0: icmp_seq=1 ttl=59 time=56.056 ms
64 bytes from 34.225.210.0: icmp_seq=2 ttl=59 time=61.166 ms
```

## 2. ARP

☀️ **Affichez votre table ARP !**

- normalement, après les `ping`, vous avez appris l'adresse MAC de tous les autres

```bsh
cameronlefevre@Ordinateur-portable-de-cameron ~ % arp -a
? (172.20.10.1) at 8a:1e:5a:a3:4b:64 on en0 ifscope [ethernet]
? (172.20.10.9) at a8:6d:aa:e7:87:40 on en0 ifscope [ethernet]
? (172.20.10.11) at e4:d:36:2f:88:9d on en0 ifscope [ethernet]
? (172.20.10.15) at ff:ff:ff:ff:ff:ff on en0 ifscope [ethernet]
```
➜ **Wireshark that**

- lancez tous Wireshark
- videz tous vos tables ARP
- normalement, y'a ~~presque~~ pas de raisons que vos PCs se contactent entre eux spontanément donc les tables ARP devraient rester vides tant que vous faites rien
- à tour de rôle, envoyez quelques `ping` entre vous
- constatez les messages ARP spontanés qui précèdent vos `ping`
  - ARP request
    - envoyé en broadcast `ff:ff:ff:ff:ff:ff`
    - tout le monde les reçoit donc !
  - ARP reply
    - celui qui a été `ping` répond à celui qui a initié le `pingv
    - il l'informe que l'adresse IP qui a été `ping` correspond à son adresse MAC

☀️ **Capture arp2.pcap**

[Capture](arp2.pcaps)

## 3. Bonus : ARP poisoning

**⭐ Empoisonner la table ARP de l'un des membres de votre réseau**

En ayant utilisé: https://github.com/alandau/arpspoof

```powershell
PS C:\Users\souri\Downloads> .\arpspoof.exe 172.20.10.9
Resolving victim and target...
Redirecting 172.20.10.9 (a8:6d:aa:e7:87:40) ---> 172.20.10.1 (8a:1e:5a:a3:4b:64)
        and in the other direction
```

Sur le pc de la victime:

```powershell
PS C:\WINDOWS\system32> arp -a
Interface : 172.20.10.9 --- 0xe
  Adresse Internet      Adresse physique      Type
  172.20.10.11          e4-0d-36-2f-88-9d     dynamique 
```


**⭐ Mettre en place un MITM**

On peut donc intercepter les données de login a un site (ex: https://github.com/alandau/arpspoof) avec **Wireshark** si le site est en HTTP

[Capture Wireshark des packets HTTP](arppoisoning.pcap)
