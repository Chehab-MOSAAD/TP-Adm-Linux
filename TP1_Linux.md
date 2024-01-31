# Chehab MOSAAD (1A STRI)
## TP1 Administration Système Linux
### Importation de la configuration OpenVPN
```Powershell
PS C:\Users\hp\Downloads> mv .\ChehabMosaad.tar.gz C:\Users\hp\OpenVPN\config
PS C:\Users\hp\Downloads> cd C:\Users\hp\OpenVPN\config
PS C:\Users\hp\OpenVPN\config> tar xf ./ChehabMosaad.tar.gz
PS C:\Users\hp\OpenVPN\config> ls


    Directory: C:\Users\hp\OpenVPN\config


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        11/24/2023   2:17 PM                ChehabMosaad
d-----        11/24/2023   2:33 PM                pki
-a----         6/14/2018   3:41 PM           1858 ca.crt
-a----         11/5/2023  10:47 AM            285 ChehabMosaad.ovpn
-a----        11/24/2023   2:26 PM           4794 ChehabMosaad.tar.gz
-a----          9/5/2018   4:16 PM            636 ta.key
```
## Test du service VPN
## Interface Réseau IPv4
```Powershell
PS C:\Users\hp> netsh int ipv4 show addr

Configuration for interface "Ethernet"
    DHCP enabled:                         Yes
    InterfaceMetric:                      5

    DHCP enabled:                         No
    InterfaceMetric:                      5

Configuration for interface "vEthernet (Default Switch)"
    DHCP enabled:                         No
    IP Address:                           172.31.112.1
    Subnet Prefix:                        172.31.112.0/20 (mask 255.255.240.0)
    InterfaceMetric:                      15

Configuration for interface "Ethernet 2"
    DHCP enabled:                         No
    IP Address:                           192.168.56.1
    Subnet Prefix:                        192.168.56.0/24 (mask 255.255.255.0)
    InterfaceMetric:                      25

Configuration for interface "OpenVPN TAP-Windows6"
    DHCP enabled:                         Yes
    InterfaceMetric:                      25

Configuration for interface "OpenVPN Data Channel Offload"
    DHCP enabled:                         Yes
    IP Address:                           172.16.196.86
    Subnet Prefix:                        172.16.196.84/30 (mask 255.255.255.252)
    InterfaceMetric:                      25

Configuration for interface "Local Area Connection* 1"
    DHCP enabled:                         Yes
    InterfaceMetric:                      25

Configuration for interface "Local Area Connection* 10"
    DHCP enabled:                         Yes
    InterfaceMetric:                      25

Configuration for interface "Wi-Fi"
    DHCP enabled:                         Yes
    IP Address:                           198.18.16.50
    Subnet Prefix:                        198.18.16.0/22 (mask 255.255.252.0)
    Default Gateway:                      198.18.16.1
    Gateway Metric:                       0
    InterfaceMetric:                      35

Configuration for interface "Loopback Pseudo-Interface 1"
    DHCP enabled:                         No
    IP Address:                           127.0.0.1
    Subnet Prefix:                        127.0.0.0/8 (mask 255.0.0.0)
    InterfaceMetric:                      75
```

## Interface Réseau IPv6
```Powershell
PS C:\Users\hp> netsh int ipv6 show addr "OpenVPN Data Channel Offload"

Address 2a03:7220:8083:c4c4::1014 Parameters
---------------------------------------------------------
Interface Luid     : OpenVPN Data Channel Offload
Scope Id           : 0.0
Valid Lifetime     : infinite
Preferred Lifetime : infinite
DAD State          : Preferred
Address Type       : Manual
Skip as Source     : false

Address fe80::bca3:1bf5:75db:1c7d%72 Parameters
---------------------------------------------------------
Interface Luid     : OpenVPN Data Channel Offload
Scope Id           : 0.72
Valid Lifetime     : infinite
Preferred Lifetime : infinite
DAD State          : Preferred
Address Type       : Other
Skip as Source     : false
```
## Table de routage IPv4
```Powershell
PS C:\Users\hp> netsh int ipv4 show route

Publish  Type      Met  Prefix                    Idx  Gateway/Interface Name
No       Manual    0    0.0.0.0/0                  12  198.18.16.1
No       Manual    256  0.0.0.0/1                  72  172.16.196.85
No       Manual    200  10.0.0.0/8                 72  172.16.196.85
No       Manual    256  89.234.156.196/32          12  198.18.16.1
No       System    256  127.0.0.0/8                 1  Loopback Pseudo-Interface 1
No       System    256  127.0.0.1/32                1  Loopback Pseudo-Interface 1
No       System    256  127.255.255.255/32          1  Loopback Pseudo-Interface 1
No       Manual    256  128.0.0.0/1                72  172.16.196.85
No       Manual    200  172.16.0.0/12              72  172.16.196.85
No       Manual    200  172.16.196.1/32            72  172.16.196.85
No       System    256  172.16.196.84/30           72  OpenVPN Data Channel Offload
No       System    256  172.16.196.86/32           72  OpenVPN Data Channel Offload
No       System    256  172.16.196.87/32           72  OpenVPN Data Channel Offload
No       System    256  172.31.112.0/20            52  vEthernet (Default Switch)
No       System    256  172.31.112.1/32            52  vEthernet (Default Switch)
No       System    256  172.31.127.255/32          52  vEthernet (Default Switch)
No       Manual    200  192.168.0.0/16             72  172.16.196.85
No       System    256  192.168.56.0/24            16  Ethernet 2
No       System    256  192.168.56.1/32            16  Ethernet 2
No       System    256  192.168.56.255/32          16  Ethernet 2
No       Manual    200  198.18.0.0/15              72  172.16.196.85
No       System    256  198.18.16.0/22             12  Wi-Fi
No       System    256  198.18.16.50/32            12  Wi-Fi
No       System    256  198.18.19.255/32           12  Wi-Fi
No       Manual    200  198.51.100.0/24            72  172.16.196.85
No       Manual    200  203.0.113.0/24             72  172.16.196.85
No       System    256  224.0.0.0/4                 1  Loopback Pseudo-Interface 1
No       System    256  224.0.0.0/4                16  Ethernet 2
No       System    256  224.0.0.0/4                20  Ethernet
No       System    256  224.0.0.0/4                12  Wi-Fi
No       System    256  224.0.0.0/4                 9  Local Area Connection* 1
No       System    256  224.0.0.0/4                 7  Local Area Connection* 10
No       System    256  224.0.0.0/4                52  vEthernet (Default Switch)
No       System    256  224.0.0.0/4                64  OpenVPN Wintun
No       System    256  224.0.0.0/4                66  OpenVPN TAP-Windows6
No       System    256  224.0.0.0/4                72  OpenVPN Data Channel Offload
No       System    256  255.255.255.255/32          1  Loopback Pseudo-Interface 1
No       System    256  255.255.255.255/32         16  Ethernet 2
No       System    256  255.255.255.255/32         20  Ethernet
No       System    256  255.255.255.255/32         12  Wi-Fi
No       System    256  255.255.255.255/32          9  Local Area Connection* 1
No       System    256  255.255.255.255/32          7  Local Area Connection* 10
No       System    256  255.255.255.255/32         52  vEthernet (Default Switch)
No       System    256  255.255.255.255/32         66  OpenVPN TAP-Windows6
No       System    256  255.255.255.255/32         72  OpenVPN Data Channel Offload
```
## Table de routage IPv6
```Powershell
PS C:\Users\hp> netsh int ipv6 show route

Publish  Type      Met  Prefix                    Idx  Gateway/Interface Name
-------  --------  ---  ------------------------  ---  ------------------------
No       Manual    16   ::/0                       12  fe80::4c25:9bff:fe95:d940
No       Manual    200  ::/3                       72  fe80::8
No       Manual    200  2000::/4                   72  fe80::8
No       Manual    16   2001:678:3fc:10::/64       12  Wi-Fi
No       System    256  2001:678:3fc:10:f132:bf4:75ad:cd2b/128   12  Wi-Fi
No       System    256  2001:678:3fc:10:f855:8d5e:a21d:d1bc/128   12  Wi-Fi
No       Manual    0    2a03:7220:8083:c4c4::/64   72  fe80::8
No       System    256  2a03:7220:8083:c4c4::1014/128   72  OpenVPN Data Channel Offload
No       Manual    200  3000::/4                   72  fe80::8
No       Manual    200  fc00::/7                   72  fe80::8
No       System    256  fe80::/64                  16  Ethernet 2
No       System    256  fe80::/64                  12  Wi-Fi
No       System    256  fe80::/64                   9  Local Area Connection* 1
No       System    256  fe80::/64                   7  Local Area Connection* 10
No       System    256  fe80::/64                  52  vEthernet (Default Switch)
No       System    256  fe80::/64                  64  OpenVPN Wintun
No       System    256  fe80::/64                  66  OpenVPN TAP-Windows6
No       System    256  fe80::/64                  72  OpenVPN Data Channel Offload
No       System    256  fe80::209c:f148:21e7:fa03/128    7  Local Area Connection* 10
No       System    256  fe80::82ca:7dfd:2dd2:b9e8/128   20  Ethernet
No       System    256  fe80::9877:d841:bc27:4bb3/128   66  OpenVPN TAP-Windows6
No       System    256  fe80::a174:9668:98c0:cd57/128   16  Ethernet 2
No       System    256  fe80::a1fb:3459:9b72:75ea/128   64  OpenVPN Wintun
No       System    256  fe80::bca3:1bf5:75db:1c7d/128   72  OpenVPN Data Channel Offload
No       System    256  fe80::c84d:60da:4b88:60e/128   52  vEthernet (Default Switch)
No       System    256  fe80::db3d:c01a:9006:b849/128   12  Wi-Fi
No       System    256  ff00::/8                    1  Loopback Pseudo-Interface 1
No       System    256  ff00::/8                   16  Ethernet 2
No       System    256  ff00::/8                   20  Ethernet
No       System    256  ff00::/8                   12  Wi-Fi
No       System    256  ff00::/8                    9  Local Area Connection* 1
No       System    256  ff00::/8                    7  Local Area Connection* 10
No       System    256  ff00::/8                   52  vEthernet (Default Switch)
No       System    256  ff00::/8                   64  OpenVPN Wintun
No       System    256  ff00::/8                   66  OpenVPN TAP-Windows6
No       System    256  ff00::/8                   72  OpenVPN Data Channel Offload
```
## Tests de connectivité ICMP
```Powershell
PS C:\Users\hp> ping -n 2 9.9.9.9

Pinging 9.9.9.9 with 32 bytes of data:
Reply from 9.9.9.9: bytes=32 time=36ms TTL=60
Reply from 9.9.9.9: bytes=32 time=37ms TTL=60

Ping statistics for 9.9.9.9:
    Packets: Sent = 2, Received = 2, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 36ms, Maximum = 37ms, Average = 36ms
PS C:\Users\hp> ping -n 2 2620:fe::fe

Pinging 2620:fe::fe with 32 bytes of data:
Reply from 2620:fe::fe: time=37ms
Reply from 2620:fe::fe: time=39ms

Ping statistics for 2620:fe::fe:
    Packets: Sent = 2, Received = 2, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 37ms, Maximum = 39ms, Average = 38ms
```
## Adresse IPv4 Serveur DNS
```Powershell
PS C:\Users\hp> netsh int ipv4 show dnsservers

Configuration for interface "Ethernet"
    DNS servers configured through DHCP:  None
    Register with which suffix:           Primary only

Configuration for interface "OpenVPN Wintun"
    Statically Configured DNS Servers:    None
    Register with which suffix:           Primary only

Configuration for interface "vEthernet (Default Switch)"
    Statically Configured DNS Servers:    None
    Register with which suffix:           None

Configuration for interface "Ethernet 2"
    Statically Configured DNS Servers:    None
    Register with which suffix:           Primary only

Configuration for interface "OpenVPN TAP-Windows6"
    DNS servers configured through DHCP:  None
    Register with which suffix:           Primary only

Configuration for interface "OpenVPN Data Channel Offload"
    Statically Configured DNS Servers:    172.16.0.2
    Register with which suffix:           Primary only

Configuration for interface "Local Area Connection* 1"
    DNS servers configured through DHCP:  None
    Register with which suffix:           Primary only

Configuration for interface "Local Area Connection* 10"
    DNS servers configured through DHCP:  None
    Register with which suffix:           Primary only

Configuration for interface "Wi-Fi"
    DNS servers configured through DHCP:  172.16.0.2
    Register with which suffix:           Primary only

Configuration for interface "Loopback Pseudo-Interface 1"
    Statically Configured DNS Servers:    None
    Register with which suffix:           Primary only
```
## Adresse IPv6 Serveur DNS
```Powershell
PS C:\Users\hp> netsh int ipv6 show dnsservers

Configuration for interface "Ethernet"
    DNS servers configured through DHCP:  fec0:0:0:ffff::1%1
                                          fec0:0:0:ffff::2%1
                                          fec0:0:0:ffff::3%1
    Register with which suffix:           Primary only

Configuration for interface "OpenVPN Wintun"
    Statically Configured DNS Servers:    fec0:0:0:ffff::1%1
                                          fec0:0:0:ffff::3%1
    Register with which suffix:           Primary only

Configuration for interface "vEthernet (Default Switch)"
    Statically Configured DNS Servers:    fec0:0:0:ffff::1%1
                                          fec0:0:0:ffff::2%1
                                          fec0:0:0:ffff::3%1
    Register with which suffix:           None

    Statically Configured DNS Servers:    fec0:0:0:ffff::1%1
                                          fec0:0:0:ffff::2%1
                                          fec0:0:0:ffff::3%1
    Register with which suffix:           Primary only

Configuration for interface "OpenVPN TAP-Windows6"
    DNS servers configured through DHCP:  fec0:0:0:ffff::1%1
                                          fec0:0:0:ffff::3%1
    Register with which suffix:           Primary only

Configuration for interface "OpenVPN Data Channel Offload"
    Statically Configured DNS Servers:    2001:678:3fc::1

Configuration for interface "Local Area Connection* 1"
    DNS servers configured through DHCP:  fec0:0:0:ffff::1%1
                                          fec0:0:0:ffff::2%1
                                          fec0:0:0:ffff::3%1
    Register with which suffix:           Primary only

Configuration for interface "Local Area Connection* 10"
    DNS servers configured through DHCP:  fec0:0:0:ffff::1%1
                                          fec0:0:0:ffff::2%1
                                          fec0:0:0:ffff::3%1
    Register with which suffix:           Primary only

Configuration for interface "Wi-Fi"
    DNS servers configured through DHCP:  None
    Register with which suffix:           Primary only

Configuration for interface "Loopback Pseudo-Interface 1"
    Statically Configured DNS Servers:    fec0:0:0:ffff::1%1
                                          fec0:0:0:ffff::2%1
                                          fec0:0:0:ffff::3%1
    Register with which suffix:           Primary only
```
## Résolution DNS
```Powershell
PS C:\Users\hp> nslookup www.iana.org
Server:  UnKnown
Address:  2001:678:3fc::1

Non-authoritative answer:
Name:    ianawww.vip.icann.org
Addresses:  2620:0:2d0:200::b:8
          192.0.33.8
Aliases:  www.iana.org

PS C:\Users\hp> nslookup alice.infra.stri
Server:  UnKnown
Address:  2001:678:3fc::1

Name:    alice.infra.stri
Addresses:  2001:678:3fc:3::1
          172.16.0.1
```
## Connexion SSH à un hyperviseur
```Powershell
PS C:\Users\hp> ssh -p 2222 chehabmosaad@oscar.infra.stri
The authenticity of host '[oscar.infra.stri]:2222 ([2001:678:3fc:3::7]:2222)' can't be established.
ECDSA key fingerprint is SHA256:Ox09ub1OLTyj3P0OabyFogUBYBFqZMr5d+4SkJkE0Dw.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[oscar.infra.stri]:2222,[2001:678:3fc:3::7]:2222' (ECDSA) to the list of known hosts.
chehabmosaad@oscar.infra.stri's password:
Creating home directory for chehabmosaad.
Linux oscar 6.5.0-4-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.10-1 (2023-11-03) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
-----
                  __             _
|  ||_  _   o _  /  \ _ _  _  _   )
|/\|| )(_)  |_)  \__/_)(_ (_||   o

        An opponent, similar to Mallory, but not necessarily malicious.

        https://en.wikipedia.org/wiki/Alice_and_Bob

-----
chehabmosaad@oscar:~$ exit
déconnexion
Connection to oscar.infra.stri closed.
```
## Connexion SSH à un commutateur
```Powershell
PS C:\Users\hp> ssh -p 2222 etu@asw14-307.infra.stri
The authenticity of host '[asw14-307.infra.stri]:2222 ([2001:678:3fc:3:2a6:caff:fe22:2980]:2222)' can't be established.
RSA key fingerprint is SHA256:OJwwvIEFOxiHRtxBb/6wCHVZUodC6lFNl8iQk21WHCM.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[asw14-307.infra.stri]:2222,[2001:678:3fc:3:2a6:caff:fe22:2980]:2222' (RSA) to the list of known hosts.
Password:
asw14-307#
```
## Connexion passwordless et Configuration client SSH
```Powershell
PS C:\Users\hp> ssh-keygen -t ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\hp/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\hp/.ssh/id_ed25519.
Your public key has been saved in C:\Users\hp/.ssh/id_ed25519.pub.
The key fingerprint is:
SHA256:4B3GaGc0Qe8yltiptrLnNpJ8DSb/5ZPn7XbNLhdLujM hp@Chehab-Laptop
The key's randomart image is:
+--[ED25519 256]--+
|  .=+..+=+.      |
|.oo  ooo+..      |
|=.o. o o o       |
|o=  =Eo + o      |
|..oB.. oS*       |
|. +.=   +..      |
| . o   o ..      |
|   .o o ...      |
|  ..o=. ..       |
+----[SHA256]-----+

PS C:\Users\hp> ssh -p 2222 chehabmosaad@oscar.infra.stri mkdir .ssh
chehabmosaad@oscar.infra.stri's password:

mkdir: impossible de créer le répertoire « .ssh »: Le fichier existe

PS C:\Users\hp> ssh -p 2222 chehabmosaad@oscar.infra.stri "touch .ssh/authorized_keys"
chehabmosaad@oscar.infra.stri's password:

PS C:\Users\hp> ssh -p 2222 chehabmosaad@oscar.infra.stri "chmod 600 .ssh/authorized_keys"
chehabmosaad@oscar.infra.stri's password:

PS C:\Users\hp> type .ssh/id_ed25519.pub | ssh -p 2222 chehabmosaad@oscar.infra.stri "cat >> .ssh/authorized_keys"
>>
chehabmosaad@oscar.infra.stri's password:
```
## Test avec ssh oscar
```Powershell
PS C:\Users\hp> ssh oscar
Warning: Permanently added the ECDSA host key for IP address '[172.16.0.7]:2222' to the list of known hosts.
Linux oscar 6.5.0-4-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.10-1 (2023-11-03) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
-----
                  __             _
|  ||_  _   o _  /  \ _ _  _  _   )
|/\|| )(_)  |_)  \__/_)(_ (_||   o

        An opponent, similar to Mallory, but not necessarily malicious.

        https://en.wikipedia.org/wiki/Alice_and_Bob

-----
Last login: Tue Nov 28 11:20:45 2023 from 2a03:7220:8083:c4c4::1012
chehabmosaad@oscar:~$
chehabmosaad@oscar:~$
```
## Mise en place de l'arborescence de travail suite à la toute première connexion :
```Powershell
chehabmosaad@oscar:~$ ln -s /var/cache/kvm/masters ~
chehabmosaad@oscar:~$ mkdir ~/vm
chehabmosaad@oscar:~$ ln -s~/masters/scripts ~/vm/
ln : option invalide -- '~'
Saisissez « ln --help » pour plus d'informations.
chehabmosaad@oscar:~$ ln -s ~/masters/scripts ~/vm/
chehabmosaad@oscar:~$ cp ~/masters/debian-testing-amd64.{qcow2,qcow2_OVMF_VARS.fd} .

chehabmosaad@oscar:~$
```
## Copie d'une image de machine virtuelle Debian GNU/Linux en une image appelée myOwnLab :
```Powershell
chehabmosaad@oscar:~$ for file in debian-testing-amd64.{qcow2,qcow2_OVMF_VARS.fd}
> do
>  mv $file myOwnLab.${file#*.}
> done
chehabmosaad@oscar:~$ cd vm
chehabmosaad@oscar:~/vm$ ls
scripts
chehabmosaad@oscar:~/vm$ ./scripts/
diff-img.sh                 ovs-nxos.sh                 shrink.sh
iso-install-startup-efi.sh  ovs-startup-bios.sh         standalone-startup.sh
iso-install-startup.sh      ovs-startup-efi.sh          win-install.sh
ovs-csr1k.sh                ovs-startup-hugepages.sh
ovs-iosxe.sh                ovs-startup.sh
```
## Accéder à une machine virtuelle
```Powershell
chehabmosaad@oscar:~/vm$ ./scripts/ovs-startup.sh ../myOwnLab.qcow2 1024 56
tap56 is already in use by another process.
chehabmosaad@oscar:~/vm$ ./scripts/ovs-startup.sh ../myOwnLab.qcow2 1024 57
~> Virtual machine filename   : ../myOwnLab.qcow2
~> RAM size                   : 1024MB
~> SPICE VDI port number      : 5957
~> telnet console port number : 2357
~> MAC address                : b8:ad:ca:fe:00:39
~> Switch port interface      : tap57, access mode
~> IPv6 LL address            : fe80::baad:caff:fefe:39%vlan60
chehabmosaad@oscar:~/vm$
chehabmosaad@oscar:~/vm$ ssh etu@fe80::baad:caff:fefe:39%vlan60
The authenticity of host 'fe80::baad:caff:fefe:39%vlan60 (fe80::baad:caff:fefe:39%vlan60)' can't be established.
ED25519 key fingerprint is SHA256:yFLaZk+OfY7z7bHyHPXgjowRS4KMHjfoMQxracRdG9M.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'fe80::baad:caff:fefe:39%vlan60' (ED25519) to the list of known hosts.
etu@fe80::baad:caff:fefe:39%vlan60's password:
Linux vm0 6.5.0-4-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.10-1 (2023-11-03) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Mon Nov 20 10:27:31 2023 from fe80::9803:fff:fe68:ea4a%enp0s1
etu@vm0:~$
etu@vm0:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
2: enp0s1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether b8:ad:ca:fe:00:39 brd ff:ff:ff:ff:ff:ff
    inet 198.18.61.57/23 brd 198.18.61.255 scope global dynamic enp0s1
       valid_lft 86185sec preferred_lft 86185sec
    inet6 2001:678:3fc:3c:baad:caff:fefe:39/64 scope global dynamic mngtmpaddr proto kernel_ra
       valid_lft 86186sec preferred_lft 14186sec
    inet6 fe80::baad:caff:fefe:39/64 scope link proto kernel_ll
       valid_lft forever preferred_lft forever
etu@vm0:~$ exit
déconnexion
Connection to fe80::baad:caff:fefe:39%vlan60 closed.
chehabmosaad@oscar:~/vm$ exit
déconnexion
Connection to oscar.infra.stri closed.
```
## Test avec ssh vm0
```Powershell
PS C:\Users\hp> ssh vm0
The authenticity of host '[2001:678:3fc:3c:baad:caff:fefe:39]:2222 ([2001:678:3fc:3c:baad:caff:fefe:39]:2222)' can't be established.
ECDSA key fingerprint is SHA256:Nr0aYR2V4PFdXoMCArhqoVALH1xX8TH08qk7tfBOhUM.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[2001:678:3fc:3c:baad:caff:fefe:39]:2222' (ECDSA) to the list of known hosts.
etu@2001:678:3fc:3c:baad:caff:fefe:39's password:
Linux vm0 6.5.0-4-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.10-1 (2023-11-03) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Tue Nov 28 12:03:16 2023 from fe80::64f4:67ff:fed5:a75c%enp0s1
etu@vm0:~$
```
## Accéder à l'écran graphique de la machine virtuelle DEBIAN 
```Powershell
chehabmosaad@Chehab-Laptop:~/.ssh$ ssh -L 5957:localhost:5957 oscar
chehabmosaad@oscar.infra.stri's password:
Linux oscar 6.5.0-4-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.10-1 (2023-11-03) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
-----
                  __             _
|  ||_  _   o _  /  \ _ _  _  _   )
|/\|| )(_)  |_)  \__/_)(_ (_||   o

        An opponent, similar to Mallory, but not necessarily malicious.

        https://en.wikipedia.org/wiki/Alice_and_Bob

-----
Last login: Sun Dec  3 17:10:58 2023 from 172.16.196.78
```

## VM Ubuntu Desktop
```Powershell
chehabmosaad@oscar:~$ cd vm
chehabmosaad@oscar:~/vm$ cp ../masters/ubuntu-23.10-desktop.qcow2 2048 57
cp: cible '57': Aucun fichier ou dossier de ce type
chehabmosaad@oscar:~/vm$ cp ../masters/ubuntu-23.10-desktop.qcow2.
cp: opérande de fichier cible manquant après '../masters/ubuntu-23.10-desktop.qcow2.'
Saisissez « cp --help » pour plus d'informations.
chehabmosaad@oscar:~/vm$ cp ../masters/ubuntu-23.10-desktop.qcow2 .
chehabmosaad@oscar:~/vm$ cp ../masters/ubuntu-23.10-desktop.qcow2 2048 57
cp: cible '57': Aucun fichier ou dossier de ce type
chehabmosaad@oscar:~/vm$ ./scripts/ovs-startup.sh ubuntu-23.10-desktop.qcow2 2048 57
tap57 is already in use by another process.
chehabmosaad@oscar:~/vm$ ./scripts/ovs-startup.sh ubuntu-23.10-desktop.qcow2 2048 56
tap56 is already in use by another process.
chehabmosaad@oscar:~/vm$ ./scripts/ovs-startup.sh ubuntu-23.10-desktop.qcow2 2048 55
tap55 is already in use by another process.
chehabmosaad@oscar:~/vm$ ./scripts/ovs-startup.sh ubuntu-23.10-desktop.qcow2 2048 58
~> Virtual machine filename   : ubuntu-23.10-desktop.qcow2
~> RAM size                   : 2048MB
~> SPICE VDI port number      : 5958
~> telnet console port number : 2358
~> MAC address                : b8:ad:ca:fe:00:3a
~> Switch port interface      : tap58, access mode
~> IPv6 LL address            : fe80::baad:caff:fefe:3a%vlan60
chehabmosaad@Chehab-Laptop:~/.ssh$ ssh -L 5958:localhost:5958 oscar
chehabmosaad@oscar.infra.stri's password:
Linux oscar 6.5.0-4-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.10-1 (2023-11-03) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
-----
                  __             _
|  ||_  _   o _  /  \ _ _  _  _   )
|/\|| )(_)  |_)  \__/_)(_ (_||   o

        An opponent, similar to Mallory, but not necessarily malicious.

        https://en.wikipedia.org/wiki/Alice_and_Bob

-----
Last login: Sun Dec  3 17:13:45 2023 from 172.16.196.78
chehabmosaad@oscar:~$ exit
déconnexion
Connection to oscar.infra.stri closed.
```

## VM Windows 11 + VDI SPICE
```Powershell
chehabmosaad@Chehab-Laptop:~/.ssh$ ssh oscar
Linux oscar 6.5.0-4-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.10-1 (2023-11-03) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
-----
                  __             _
|  ||_  _   o _  /  \ _ _  _  _   )
|/\|| )(_)  |_)  \__/_)(_ (_||   o

        An opponent, similar to Mallory, but not necessarily malicious.

        https://en.wikipedia.org/wiki/Alice_and_Bob

-----
Last login: Sun Dec  3 17:40:19 2023 from 172.16.196.78
chehabmosaad@oscar:~$ cd vm
chehabmosaad@oscar:~/vm$ cp ../masters/win11.qcow2 .

chehabmosaad@oscar:~/vm$
chehabmosaad@oscar:~/vm$ ./scripts/ovs-startup.sh win11.qcow2 2048 59
~> Virtual machine filename   : win11.qcow2
~> RAM size                   : 2048MB
~> SPICE VDI port number      : 5959
~> telnet console port number : 2359
~> MAC address                : b8:ad:ca:fe:00:3b
~> Switch port interface      : tap59, access mode
~> IPv6 LL address            : fe80::baad:caff:fefe:3b%vlan60
chehabmosaad@oscar:~/vm$
chehabmosaad@oscar:~/vm$ exit
déconnexion
Connection to oscar.infra.stri closed.
chehabmosaad@Chehab-Laptop:~/.ssh$ ssh -L 5959:localhost:5959 oscar
Linux oscar 6.5.0-4-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.10-1 (2023-11-03) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
-----
                  __             _
|  ||_  _   o _  /  \ _ _  _  _   )
|/\|| )(_)  |_)  \__/_)(_ (_||   o

        An opponent, similar to Mallory, but not necessarily malicious.

        https://en.wikipedia.org/wiki/Alice_and_Bob

-----
Last login: Sun Dec  3 17:49:54 2023 from 172.16.196.78
chehabmosaad@oscar:~$
```
# Fin