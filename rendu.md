Grand2petit2
'''
serveur
[oui@serveur ~]$ ping routeur
PING Routeur (10.2.0.254) 56(84) bytes of data.
64 bytes from Routeur (10.2.0.254): icmp_seq=1 ttl=64 time=0.477 ms
64 bytes from Routeur (10.2.0.254): icmp_seq=2 ttl=64 time=0.340 ms
64 bytes from Routeur (10.2.0.254): icmp_seq=3 ttl=64 time=0.398 ms
64 bytes from Routeur (10.2.0.254): icmp_seq=4 ttl=64 time=0.367 ms
^C
client

[oui@client ~]$ ping routeur
PING Routeur (10.1.0.254) 56(84) bytes of data.
64 bytes from Routeur (10.1.0.254): icmp_seq=1 ttl=64 time=0.496 ms
64 bytes from Routeur (10.1.0.254): icmp_seq=2 ttl=64 time=0.411 ms
64 bytes from Routeur (10.1.0.254): icmp_seq=3 ttl=64 time=0.380 ms
64 bytes from Routeur (10.1.0.254): icmp_seq=4 ttl=64 time=0.293 ms
^C


'''

petit3

'''

[oui@routeur ~]$ ip route show
10.1.0.0/24 dev enp0s3 proto kernel scope link src 10.1.0.254 metric 100
10.2.0.0/24 dev enp0s8 proto kernel scope link src 10.2.0.254 metric 101

[oui@client ~]$ ping serveur
PING serveur (10.2.0.10) 56(84) bytes of data.
64 bytes from serveur (10.2.0.10): icmp_seq=1 ttl=63 time=0.618 ms
64 bytes from serveur (10.2.0.10): icmp_seq=2 ttl=63 time=0.682 ms
64 bytes from serveur (10.2.0.10): icmp_seq=3 ttl=63 time=0.702 ms
64 bytes from serveur (10.2.0.10): icmp_seq=4 ttl=63 time=0.620 ms
^C
--- serveur ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms

[oui@serveur ~]$ ping client
PING Client (10.1.0.10) 56(84) bytes of data.
64 bytes from Client (10.1.0.10): icmp_seq=1 ttl=63 time=0.593 ms
64 bytes from Client (10.1.0.10): icmp_seq=2 ttl=63 time=0.697 ms
64 bytes from Client (10.1.0.10): icmp_seq=3 ttl=63 time=0.640 ms
64 bytes from Client (10.1.0.10): icmp_seq=4 ttl=63 time=0.645 ms
^C
--- Client ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3001ms

'''

Grand2petitA
ARP

client
'''
[oui@client ~]$ ip n s
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:39 REACHABLE

'''

serveur 
'''
[oui@serveur ~]$ ip n s
10.2.0.1 dev enp0s3 lladdr 0a:00:27:00:00:3d REACHABLE
'''
APRES PING 

client
'''
[oui@client ~]$ ip n s
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:39 REACHABLE
10.1.0.254 dev enp0s3 lladdr 08:00:27:8f:5d:cf REACHABLE

'''

serveur
'''
[oui@serveur ~]$ ip n s
10.2.0.254 dev enp0s3 lladdr 08:00:27:9f:43:44 REACHABLE
10.2.0.1 dev enp0s3 lladdr 0a:00:27:00:00:3d REACHABLE

'''

Petit b

routeur
'''
[oui@routeur ~]$ ip n s
10.2.0.1 dev enp0s8 lladdr 0a:00:27:00:00:3d REACHABLE

'''

APRES PING


routeur
'''
10.1.0.10 dev enp0s3 lladdr 08:00:27:cc:48:69 REACHABLE
10.2.0.1 dev enp0s8 lladdr 0a:00:27:00:00:3d DELAY
10.2.0.10 dev enp0s8 lladdr 08:00:27:f7:16:d3 REACHABLE

'''

Petit c

Table arp hote vide
'''
Interface : 10.1.0.1 --- 0x39
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 10.2.0.1 --- 0x3d
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique
'''
Un peux plus tard 

'''
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
'''

Petit d

Client sans NAT 
'''
[oui@client ~]$ ip n s
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:39 REACHABLE
'''
Avec NAT et apres une recherche

'''
[oui@client ~]$ ip n s
10.0.3.2 dev enp0s8 lladdr 52:54:00:12:35:02 REACHABLE
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:39 REACHABLE

'''