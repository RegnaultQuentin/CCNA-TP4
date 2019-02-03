Tableau des ip
<table>
  <tr>
    <td>Machine</td><td>net1</td><td>net2</td><td>MAC</td>
  </tr>
  <tr>
    <td>client1</td><td>10.1.0.10</td><td>X</td><td>08:00:27:cc:48:69</td>
  </tr>
  <tr>
    <td>routeur1</td><td>10.1.0.254</td><td>10.2.254</td><td>08:00:27:8f:5d:cf<br>08:00:27:9f:43:44</td>
  </tr>
  <tr>
    <td>serveur1</td><td>X</td><td>10.2.0.10</td><td>08:00:27:f7:16:d3</td>
  </tr>
</table>
<b>I. Spéléologie réseau </b><br>
<b>1. Création des réseaux</b><br>
<b>2. Création des VMs</b><br>

Ping du client1 vers routeur1
```
client

[oui@client ~]$ ping routeur
PING Routeur (10.1.0.254) 56(84) bytes of data.
64 bytes from Routeur (10.1.0.254): icmp_seq=1 ttl=64 time=0.496 ms
64 bytes from Routeur (10.1.0.254): icmp_seq=2 ttl=64 time=0.411 ms
64 bytes from Routeur (10.1.0.254): icmp_seq=3 ttl=64 time=0.380 ms
64 bytes from Routeur (10.1.0.254): icmp_seq=4 ttl=64 time=0.293 ms
^C
```
Ping du serveur1 vers routeur1
```
serveur
[oui@serveur ~]$ ping routeur
PING Routeur (10.2.0.254) 56(84) bytes of data.
64 bytes from Routeur (10.2.0.254): icmp_seq=1 ttl=64 time=0.477 ms
64 bytes from Routeur (10.2.0.254): icmp_seq=2 ttl=64 time=0.340 ms
64 bytes from Routeur (10.2.0.254): icmp_seq=3 ttl=64 time=0.398 ms
64 bytes from Routeur (10.2.0.254): icmp_seq=4 ttl=64 time=0.367 ms
^C
```

<b>3. Mise en place du routage statique</b><br>

Les routes sur le routeur déjà connecté au serveur et au client.

```
[oui@routeur ~]$ ip route show
10.1.0.0/24 dev enp0s3 proto kernel scope link src 10.1.0.254 metric 100
10.2.0.0/24 dev enp0s8 proto kernel scope link src 10.2.0.254 metric 101

```
Le client1 ping serveur1
```
[oui@client ~]$ ping serveur
PING serveur (10.2.0.10) 56(84) bytes of data.
64 bytes from serveur (10.2.0.10): icmp_seq=1 ttl=63 time=0.618 ms
64 bytes from serveur (10.2.0.10): icmp_seq=2 ttl=63 time=0.682 ms
64 bytes from serveur (10.2.0.10): icmp_seq=3 ttl=63 time=0.702 ms
64 bytes from serveur (10.2.0.10): icmp_seq=4 ttl=63 time=0.620 ms
^C
--- serveur ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
```
Le serveur1 ping client1
```
[oui@serveur ~]$ ping client
PING Client (10.1.0.10) 56(84) bytes of data.
64 bytes from Client (10.1.0.10): icmp_seq=1 ttl=63 time=0.593 ms
64 bytes from Client (10.1.0.10): icmp_seq=2 ttl=63 time=0.697 ms
64 bytes from Client (10.1.0.10): icmp_seq=3 ttl=63 time=0.640 ms
64 bytes from Client (10.1.0.10): icmp_seq=4 ttl=63 time=0.645 ms
^C
--- Client ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3001ms

```
<b>II. Spéléologie réseau</b><br>
<b>1. ARP</b><br>
<b>A. Manip 1</b><br>

Le client avec une table ARP vide.La ligne visible est la connection SSH
```
[oui@client ~]$ ip n s
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:39 REACHABLE

```

Le serveur avec un table ARP vide. Pareil que pour le client
```
[oui@serveur ~]$ ip n s
10.2.0.1 dev enp0s3 lladdr 0a:00:27:00:00:3d REACHABLE
```
APRES PING 

Le client après le ping du serveur. On peux voir la nouvelle ligne qui correspond à la route du ping serveur.
```
[oui@client ~]$ ip n s
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:39 REACHABLE
10.1.0.254 dev enp0s3 lladdr 08:00:27:8f:5d:cf REACHABLE

```

Le serveur après le ping du client. La même chose mais pour le client.
```
[oui@serveur ~]$ ip n s
10.2.0.254 dev enp0s3 lladdr 08:00:27:9f:43:44 REACHABLE
10.2.0.1 dev enp0s3 lladdr 0a:00:27:00:00:3d REACHABLE

```

<b>B. Manip 2</b><br>
Table ARP vide.<br>
Table du routeur vide. La ligne present correspond au serveur SSH.
```
[oui@routeur ~]$ ip n s
10.2.0.1 dev enp0s8 lladdr 0a:00:27:00:00:3d REACHABLE

```
Le client1 ping serveur1.<br>

La table du routeur. Les nouvelles lignes correspondent aux routes prises par le client1.
```
10.1.0.10 dev enp0s3 lladdr 08:00:27:cc:48:69 REACHABLE
10.2.0.1 dev enp0s8 lladdr 0a:00:27:00:00:3d DELAY
10.2.0.10 dev enp0s8 lladdr 08:00:27:f7:16:d3 REACHABLE

```

<b>C. Manip 3</b><br>

La table ARP de l'hôte vide.
```
Interface : 10.1.0.1 --- 0x39
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 10.2.0.1 --- 0x3d
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique
```
Un peux plus tard, on voit la passerelle se connecté aux autres réseaux tel qu'internet. 

```
Interface : 10.1.0.1 --- 0x39
  Adresse Internet      Adresse physique      Type
  10.1.0.10             08-00-27-cc-48-69     dynamique
  10.1.0.255            ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique

Interface : 10.2.0.1 --- 0x3d
  Adresse Internet      Adresse physique      Type
  10.2.0.10             08-00-27-f7-16-d3     dynamique
  10.2.0.254            08-00-27-9f-43-44     dynamique
  10.2.0.255            ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
```
<b>D. Manip 4</b><br>

Le client1 avec la table ARP vide.
```
[oui@client ~]$ ip n s
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:39 REACHABLE
```
Avec NAT et apres une recherche, on voit une nouvelle ligne qui corespond a l'ip de la NAT pour ce connecter a Internet pour la recherche faite précédemment

```
[oui@client ~]$ ip n s
10.0.3.2 dev enp0s8 lladdr 52:54:00:12:35:02 REACHABLE
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:39 REACHABLE

```
<b>2. Wireshark </b><br>
