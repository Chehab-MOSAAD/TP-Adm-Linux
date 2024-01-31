# Chehab MOSAAD (1A STRI)
## TP3 Administration Système Linux
## Partie 1 - Script checkup.sh
###  Créer et tester le script checkup.sh 
```Powershell
PS C:\Users\hp> ssh oscar
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
Last login: Fri Dec  8 13:41:53 2023 from 2a03:7220:8083:c4c4::1012
chehabmosaad@oscar:~$ vim checkup.sh
chehabmosaad@oscar:~$ cat checkup.sh
!/bin/bashvme
set -e

GreenOnBlack='\E[32m'
echo "~> Check-up gestion des machines virtuelles"

# Arborescence
if [ ! -L ~/masters ]; then
    echo "Creation du lien vers le catalogue des images de machines virtuelles."
    ln -s /var/cache/kvm/masters ~
fi

if [ ! -d ~/vm ]; then
    echo "Creation du dossier des images de machines virtuelles."
    mkdir ~/vm
    ln -s ~/masters/scripts ~/vm/
fi

echo -e "${GreenOnBlack}~> Arborescence prete."
tput sgr0

# Analyse des fichiers image
count=0
listFilename="$HOME/image-list-info.json"
echo -n "[" > "$listFilename"
imageList=$(find ~ -type f \( -iname \*.qcow2 -o -iname \*.raw \) -printf '%p ')

for file in $imageList
do
    echo "$((++count)) : $file"
    if ! pgrep -u "${USER}" | grep -q "${file##*/}"; then
        qemu-img info --output=json --backing-chain "$file" | tr -d '[]' >> "$listFilename"
        sed -i '/^$/d' "$listFilename" && sed -i '$s/$/\,/' "$listFilename"
    fi
done

sed -i '$ s/,/\n]/g' "$listFilename"
echo -e "${GreenOnBlack}~> $count fichier(s) image(s) present(s)."
tput sgr0

# Liste des machines virtuelles actives
IFS=$'\n'
count=0
mapfile -t vmArray < <(pgrep -u "${USER}" -f 'qemu-system-x86_64\ ' || true)

if [ ${#vmArray[@]} -gt 0 ]; then
    for vm in "${vmArray[@]}"
    do
        echo -n "$((++count)) : "
        echo "$vm" | grep -Po '(?<=-name\ ).*(?=-m)' | tr -d '\n'
        echo -n "utilise le cordon "
        echo "$vm" | grep -Po 'tap\d{1,3}'
    done
fi

echo -e "${GreenOnBlack}~> $count machine(s) virtuelle(s) active(s)."
tput sgr0

exit 0
chehabmosaad@oscar:~$
chehabmosaad@oscar:~$ bash checkup.sh
checkup.sh: ligne 1: !/bin/bashvme: Aucun fichier ou dossier de ce type
~> Check-up gestion des machines virtuelles
~> Arborescence prete.
1 : /home/chehabmosaad/vm/win11.qcow2
2 : /home/chehabmosaad/vm/ubuntu-23.10-desktop.qcow2
3 : /home/chehabmosaad/myOwnLab.qcow2
qemu-img: Could not open '/home/chehabmosaad/myOwnLab.qcow2': Failed to get shared "write" lock
Is another process using the image [/home/chehabmosaad/myOwnLab.qcow2]?
~> 3 fichier(s) image(s) present(s).
1 : utilise le cordon chehabmosaad@oscar:~$
chehabmosaad@oscar:~$
```
### Gestion des processus
```Powershell
etu@vm0:~$ sudo apt install -y apache2
[sudo] Mot de passe de etu :
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances... Fait
Lecture des informations d'état... Fait
Les paquets supplémentaires suivants seront installés :
  apache2-bin apache2-data apache2-utils libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap libcurl4 liblua5.3-0 ssl-cert
Paquets suggérés :
  apache2-doc apache2-suexec-pristine | apache2-suexec-custom www-browser
Les NOUVEAUX paquets suivants seront installés :
  apache2 apache2-bin apache2-data apache2-utils libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap libcurl4 liblua5.3-0
  ssl-cert
0 mis à jour, 11 nouvellement installés, 0 à enlever et 0 non mis à jour.
Il est nécessaire de prendre 2 729 ko dans les archives.
Après cette opération, 9 271 ko d espace disque supplémentaires seront utilisés.
Réception de :1 http://deb.debian.org/debian testing/main amd64 libapr1 amd64 1.7.2-3 [102 kB]
Réception de :2 http://deb.debian.org/debian testing/main amd64 libaprutil1 amd64 1.6.3-1 [87,8 kB]
Réception de :3 http://deb.debian.org/debian testing/main amd64 libaprutil1-dbd-sqlite3 amd64 1.6.3-1 [13,6 kB]
Réception de :4 http://deb.debian.org/debian testing/main amd64 libaprutil1-ldap amd64 1.6.3-1 [11,8 kB]
Réception de :5 http://deb.debian.org/debian testing/main amd64 libcurl4 amd64 8.4.0-2 [413 kB]
Réception de :6 http://deb.debian.org/debian testing/main amd64 liblua5.3-0 amd64 5.3.6-2 [123 kB]
Réception de :7 http://deb.debian.org/debian testing/main amd64 apache2-bin amd64 2.4.58-1 [1 375 kB]
Réception de :8 http://deb.debian.org/debian testing/main amd64 apache2-data all 2.4.58-1 [160 kB]
Réception de :9 http://deb.debian.org/debian testing/main amd64 apache2-utils amd64 2.4.58-1 [205 kB]
Réception de :10 http://deb.debian.org/debian testing/main amd64 apache2 amd64 2.4.58-1 [217 kB]
Réception de :11 http://deb.debian.org/debian testing/main amd64 ssl-cert all 1.1.2 [21,1 kB]
Sélection du paquet libapr1:amd64 précédemment désélectionné.
(Lecture de la base de données... 34021 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../00-libapr1_1.7.2-3_amd64.deb ...
Dépaquetage de libapr1:amd64 (1.7.2-3) ...
Sélection du paquet libaprutil1:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../01-libaprutil1_1.6.3-1_amd64.deb ...
Dépaquetage de libaprutil1:amd64 (1.6.3-1) ...
Sélection du paquet libaprutil1-dbd-sqlite3:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../02-libaprutil1-dbd-sqlite3_1.6.3-1_amd64.deb ...
Dépaquetage de libaprutil1-dbd-sqlite3:amd64 (1.6.3-1) ...
Sélection du paquet libaprutil1-ldap:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../03-libaprutil1-ldap_1.6.3-1_amd64.deb ...
Dépaquetage de libaprutil1-ldap:amd64 (1.6.3-1) ...
Sélection du paquet libcurl4:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../04-libcurl4_8.4.0-2_amd64.deb ...
Dépaquetage de libcurl4:amd64 (8.4.0-2) ...
Sélection du paquet liblua5.3-0:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../05-liblua5.3-0_5.3.6-2_amd64.deb ...
Dépaquetage de liblua5.3-0:amd64 (5.3.6-2) ...
Sélection du paquet apache2-bin précédemment désélectionné.
Préparation du dépaquetage de .../06-apache2-bin_2.4.58-1_amd64.deb ...
Dépaquetage de apache2-bin (2.4.58-1) ...
Sélection du paquet apache2-data précédemment désélectionné.
Préparation du dépaquetage de .../07-apache2-data_2.4.58-1_all.deb ...
Dépaquetage de apache2-data (2.4.58-1) ...
Sélection du paquet apache2-utils précédemment désélectionné.
Préparation du dépaquetage de .../08-apache2-utils_2.4.58-1_amd64.deb ...
Dépaquetage de apache2-utils (2.4.58-1) ...
Sélection du paquet apache2 précédemment désélectionné.
Préparation du dépaquetage de .../09-apache2_2.4.58-1_amd64.deb ...
Dépaquetage de apache2 (2.4.58-1) ...
Sélection du paquet ssl-cert précédemment désélectionné.
Préparation du dépaquetage de .../10-ssl-cert_1.1.2_all.deb ...
Dépaquetage de ssl-cert (1.1.2) ...
Paramétrage de libapr1:amd64 (1.7.2-3) ...
Paramétrage de ssl-cert (1.1.2) ...
Paramétrage de liblua5.3-0:amd64 (5.3.6-2) ...
Paramétrage de libcurl4:amd64 (8.4.0-2) ...
Paramétrage de apache2-data (2.4.58-1) ...
Paramétrage de libaprutil1:amd64 (1.6.3-1) ...
Paramétrage de libaprutil1-ldap:amd64 (1.6.3-1) ...
Paramétrage de libaprutil1-dbd-sqlite3:amd64 (1.6.3-1) ...
Paramétrage de apache2-utils (2.4.58-1) ...
Paramétrage de apache2-bin (2.4.58-1) ...
Paramétrage de apache2 (2.4.58-1) ...
Enabling module mpm_event.
Enabling module authz_core.
Enabling module authz_host.
Enabling module authn_core.
Enabling module auth_basic.
Enabling module access_compat.
Enabling module authn_file.
Enabling module authz_user.
Enabling module alias.
Enabling module dir.
Enabling module autoindex.####################################################################################################.....]
Enabling module env.
Enabling module mime.
Enabling module negotiation.
Enabling module setenvif.
Enabling module filter.
Enabling module deflate.
Enabling module status.
Enabling module reqtimeout.
Enabling conf charset.
Enabling conf localized-error-pages.
Enabling conf other-vhosts-access-log.
Enabling conf security.
Enabling conf serve-cgi-bin.
Enabling site 000-default.
Created symlink /etc/systemd/system/multi-user.target.wants/apache2.service → /lib/systemd/system/apache2.service.
Scanning processes...
Scanning linux images...

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.
etu@vm0:~$ ps -aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  1.1  22588 10980 ?        Ss   déc.04   0:09 /sbin/init
root           2  0.0  0.0      0     0 ?        S    déc.04   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   déc.04   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   déc.04   0:00 [rcu_par_gp]
root           5  0.0  0.0      0     0 ?        I<   déc.04   0:00 [slub_flushwq]
root           6  0.0  0.0      0     0 ?        I<   déc.04   0:00 [netns]
root           8  0.0  0.0      0     0 ?        I<   déc.04   0:00 [kworker/0:0H-events_highpri]
root          11  0.0  0.0      0     0 ?        I<   déc.04   0:00 [mm_percpu_wq]
root          12  0.0  0.0      0     0 ?        I    déc.04   0:00 [rcu_tasks_kthread]
root          13  0.0  0.0      0     0 ?        I    déc.04   0:00 [rcu_tasks_rude_kthread]
root          14  0.0  0.0      0     0 ?        I    déc.04   0:00 [rcu_tasks_trace_kthread]
root          15  0.0  0.0      0     0 ?        S    déc.04   0:00 [ksoftirqd/0]
root          16  0.0  0.0      0     0 ?        I    déc.04   0:52 [rcu_preempt]
root          17  0.0  0.0      0     0 ?        S    déc.04   0:02 [migration/0]
root          18  0.0  0.0      0     0 ?        S    déc.04   0:00 [idle_inject/0]
root          19  0.0  0.0      0     0 ?        S    déc.04   0:00 [cpuhp/0]
root          20  0.0  0.0      0     0 ?        S    déc.04   0:00 [cpuhp/1]
root          21  0.0  0.0      0     0 ?        S    déc.04   0:00 [idle_inject/1]
root          22  0.0  0.0      0     0 ?        S    déc.04   0:02 [migration/1]
root          23  0.0  0.0      0     0 ?        S    déc.04   0:00 [ksoftirqd/1]
root          25  0.0  0.0      0     0 ?        I<   déc.04   0:00 [kworker/1:0H-events_highpri]
root          26  0.0  0.0      0     0 ?        S    déc.04   0:00 [cpuhp/2]
root          27  0.0  0.0      0     0 ?        S    déc.04   0:00 [idle_inject/2]
root          28  0.0  0.0      0     0 ?        S    déc.04   0:02 [migration/2]
root          29  0.0  0.0      0     0 ?        S    déc.04   0:00 [ksoftirqd/2]
root          32  0.0  0.0      0     0 ?        S    déc.04   0:00 [cpuhp/3]
root          33  0.0  0.0      0     0 ?        S    déc.04   0:00 [idle_inject/3]
root          34  0.0  0.0      0     0 ?        S    déc.04   0:02 [migration/3]
root          35  0.0  0.0      0     0 ?        S    déc.04   0:00 [ksoftirqd/3]
root          37  0.0  0.0      0     0 ?        I<   déc.04   0:01 [kworker/3:0H-ttm]
root          39  0.0  0.0      0     0 ?        S    déc.04   0:00 [kdevtmpfs]
root          40  0.0  0.0      0     0 ?        I<   déc.04   0:00 [inet_frag_wq]
root          42  0.0  0.0      0     0 ?        S    déc.04   0:00 [kauditd]
root          43  0.0  0.0      0     0 ?        S    déc.04   0:00 [khungtaskd]
root          44  0.0  0.0      0     0 ?        S    déc.04   0:00 [oom_reaper]
root          46  0.0  0.0      0     0 ?        I<   déc.04   0:00 [writeback]
root          47  0.0  0.0      0     0 ?        S    déc.04   0:20 [kcompactd0]
root          48  0.0  0.0      0     0 ?        SN   déc.04   0:00 [ksmd]
root          49  0.0  0.0      0     0 ?        SN   déc.04   0:03 [khugepaged]
root          50  0.0  0.0      0     0 ?        I<   déc.04   0:00 [kintegrityd]
root          51  0.0  0.0      0     0 ?        I<   déc.04   0:00 [kblockd]
root          52  0.0  0.0      0     0 ?        I<   déc.04   0:00 [blkcg_punt_bio]
root          56  0.0  0.0      0     0 ?        I<   déc.04   0:00 [tpm_dev_wq]
root          57  0.0  0.0      0     0 ?        I<   déc.04   0:00 [edac-poller]
root          58  0.0  0.0      0     0 ?        I<   déc.04   0:00 [devfreq_wq]
root          59  0.0  0.0      0     0 ?        I<   déc.04   0:00 [kworker/0:1H-kblockd]
root          60  0.0  0.0      0     0 ?        S    déc.04   0:05 [kswapd0]
root          66  0.0  0.0      0     0 ?        I<   déc.04   0:00 [kthrotld]
root          68  0.0  0.0      0     0 ?        I<   déc.04   0:00 [acpi_thermal_pm]
root          69  0.0  0.0      0     0 ?        S    déc.04   0:06 [hwrng]
root          70  0.0  0.0      0     0 ?        I<   déc.04   0:00 [mld]
root          71  0.0  0.0      0     0 ?        I<   déc.04   0:02 [kworker/3:1H-ttm]
root          72  0.0  0.0      0     0 ?        I<   déc.04   0:00 [ipv6_addrconf]
root          77  0.0  0.0      0     0 ?        I<   déc.04   0:00 [kstrp]
root          81  0.0  0.0      0     0 ?        I<   déc.04   0:00 [kworker/u9:0]
root         126  0.0  0.0      0     0 ?        I<   déc.04   0:01 [kworker/2:1H-kblockd]
root         139  0.0  0.0      0     0 ?        I<   déc.04   0:00 [kworker/1:1H-kblockd]
root         141  0.0  0.0      0     0 ?        I    déc.04   0:53 [kworker/3:2-events]
root         149  0.0  0.0      0     0 ?        I<   déc.04   0:00 [ata_sff]
root         150  0.0  0.0      0     0 ?        S    déc.04   0:00 [scsi_eh_0]
root         152  0.0  0.0      0     0 ?        I<   déc.04   0:00 [scsi_tmf_0]
root         153  0.0  0.0      0     0 ?        S    déc.04   0:00 [scsi_eh_1]
root         154  0.0  0.0      0     0 ?        I<   déc.04   0:00 [scsi_tmf_1]
root         155  0.0  0.0      0     0 ?        S    déc.04   0:00 [scsi_eh_2]
root         156  0.0  0.0      0     0 ?        I<   déc.04   0:00 [scsi_tmf_2]
root         157  0.0  0.0      0     0 ?        S    déc.04   0:00 [scsi_eh_3]
root         158  0.0  0.0      0     0 ?        I<   déc.04   0:00 [scsi_tmf_3]
root         159  0.0  0.0      0     0 ?        S    déc.04   0:00 [scsi_eh_4]
root         160  0.0  0.0      0     0 ?        I<   déc.04   0:00 [scsi_tmf_4]
root         161  0.0  0.0      0     0 ?        S    déc.04   0:00 [scsi_eh_5]
root         162  0.0  0.0      0     0 ?        I<   déc.04   0:00 [scsi_tmf_5]
root         166  0.0  0.0      0     0 ?        I<   déc.04   0:00 [nvme-wq]
root         168  0.0  0.0      0     0 ?        I<   déc.04   0:00 [nvme-reset-wq]
root         169  0.0  0.0      0     0 ?        I<   déc.04   0:00 [nvme-delete-wq]
root         203  0.0  0.0      0     0 ?        S    déc.04   0:03 [jbd2/nvme0n1p2-8]
root         204  0.0  0.0      0     0 ?        I<   déc.04   0:00 [ext4-rsv-conver]
root         247  0.0  0.9  66236  9296 ?        Ss   déc.04   0:02 /lib/systemd/systemd-journald
root         299  0.0  0.3  26196  3200 ?        Ss   déc.04   0:01 /lib/systemd/systemd-udevd
root         308  0.0  0.0      0     0 ?        S    déc.04   0:00 [psimon]
root         336  0.0  0.0      0     0 ?        S    déc.04   0:00 [watchdogd]
root         343  0.0  0.0      0     0 ?        I<   déc.04   0:00 [ttm]
root         390  0.0  0.0      0     0 ?        I<   déc.04   0:00 [cryptd]
root         458  0.0  0.0      0     0 ?        I<   déc.04   0:00 [kworker/2:2H]
root         466  0.0  0.0      0     0 ?        I<   déc.04   0:00 [nfit]
root         468  0.0  0.0      0     0 ?        I<   déc.04   0:02 [kworker/3:2H-ttm]
systemd+     503  0.0  0.3  90496  3840 ?        Ssl  déc.04   0:02 /lib/systemd/systemd-timesyncd
root         569  0.0  0.6  15560  6112 ?        S<s  déc.04   1:36 ovsdb-server /etc/openvswitch/conf.db -vconsole:emer -vsyslog:err
root         596  0.0  0.1   6640  1920 ?        Ss   déc.04   0:01 /usr/sbin/cron -f
message+     598  0.0  0.3   9396  3456 ?        Ss   déc.04   0:44 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidf
root         602  0.0  1.6 1467032 15808 ?       Ssl  déc.04   0:36 /usr/lib/snapd/snapd
root         603  0.0  0.6  17612  5888 ?        Ss   déc.04   0:22 /lib/systemd/systemd-logind
root         619  0.0  0.1   5456  1024 ?        Ss   déc.04   0:00 /sbin/rdnssd -u rdnssd -H /etc/rdnssd/merge-hook
rdnssd       620  0.0  0.2   6744  2304 ?        S    déc.04   0:00 /sbin/rdnssd -u rdnssd -H /etc/rdnssd/merge-hook
root         664  0.3  7.7 532168 74092 ?        S<Lsl déc.04  17:00 ovs-vswitchd unix:/var/run/openvswitch/db.sock -vconsole:emer -vs
root         756  0.0  0.1   5880  1664 ?        Ss   déc.04   0:00 dhclient -4 -v -i -pf /run/dhclient.C-3P0.pid -lf /var/lib/dhcp/dh
root         813  0.0  0.4  31876  4224 ?        Ss   déc.04   0:55 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade
root         817  0.0  0.1   5896  1664 tty1     Ss+  déc.04   0:00 /sbin/agetty -o -p -- \u --noclear - linux
root         821  0.0  0.1   5516  1792 ttyS0    Ss+  déc.04   0:00 /sbin/agetty -o -p -- \u --keep-baud 115200,57600,38400,9600 - vt2
root         828  0.0  0.6  15568  6272 ?        Ss   déc.04   0:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root         971  0.0  0.0      0     0 ?        I<   déc.04   0:02 [kworker/3:3H-ttm]
root         977  0.0  0.0      0     0 ?        I<   déc.04   0:02 [kworker/3:4H-ttm]
root         986  0.0  0.0      0     0 ?        I<   déc.04   0:02 [kworker/3:5H-ttm]
root        1018  0.0  0.0      0     0 ?        I<   déc.04   0:02 [kworker/3:7H-ttm]
root        1028  0.0  0.0      0     0 ?        I<   déc.04   0:02 [kworker/3:8H-ttm]
root        1068  0.0  0.0   2888   384 ?        Ss   déc.04   0:00 /bin/sh /snap/lxd/26200/commands/daemon.start
root        1209  0.0  0.0      0     0 ?        I<   déc.04   0:02 [kworker/3:9H-ttm]
root        1225  0.0  0.0 153160   384 ?        Sl   déc.04   0:00 lxcfs /var/snap/lxd/common/var/lib/lxcfs -p /var/snap/lxd/common/l
root        1236  0.0  3.6 6497700 35516 ?       Sl   déc.04   2:44 lxd --logfile /var/snap/lxd/common/lxd/logs/lxd.log --group lxd
root        1268  0.0  0.0      0     0 ?        I<   déc.04   0:00 [dio/nvme0n1p2]
root        1318  0.0  0.0      0     0 ?        I<   déc.04   0:02 [kworker/3:10H-ttm]
root        1319  0.0  0.0      0     0 ?        I<   déc.04   0:02 [kworker/3:11H-ttm]
root        1320  0.0  0.0      0     0 ?        I<   déc.04   0:01 [kworker/3:12H-ttm]
root        1326  0.0  0.0      0     0 ?        I<   déc.04   0:07 [kworker/3:13H-ttm]
root        1327  0.0  0.0      0     0 ?        I<   déc.04   0:02 [kworker/3:14H-ttm]
root        1361  0.0  0.0      0     0 ?        I<   déc.04   0:00 [btrfs-worker]
root        1362  0.0  0.0      0     0 ?        I<   déc.04   0:00 [btrfs-delalloc]
root        1363  0.0  0.0      0     0 ?        I<   déc.04   0:00 [btrfs-flush_del]
root        1364  0.0  0.0      0     0 ?        I<   déc.04   0:00 [btrfs-cache]
root        1365  0.0  0.0      0     0 ?        I<   déc.04   0:00 [btrfs-fixup]
root        1366  0.0  0.0      0     0 ?        I<   déc.04   0:00 [btrfs-endio]
root        1367  0.0  0.0      0     0 ?        I<   déc.04   0:00 [btrfs-endio-met]
root        1368  0.0  0.0      0     0 ?        I<   déc.04   0:00 [btrfs-rmw]
root        1369  0.0  0.0      0     0 ?        I<   déc.04   0:00 [btrfs-endio-wri]
root        1370  0.0  0.0      0     0 ?        I<   déc.04   0:00 [btrfs-compresse]
root        1371  0.0  0.0      0     0 ?        I<   déc.04   0:00 [btrfs-freespace]
root        1372  0.0  0.0      0     0 ?        I<   déc.04   0:00 [btrfs-delayed-m]
root        1373  0.0  0.0      0     0 ?        I<   déc.04   0:00 [btrfs-qgroup-re]
root        1374  0.0  0.0      0     0 ?        S    déc.04   0:00 [btrfs-cleaner]
root        1375  0.0  0.0      0     0 ?        S    déc.04   0:02 [btrfs-transaction]
root        1379  0.0  0.0      0     0 ?        I<   déc.04   0:01 [kworker/3:15H-ttm]
root        1412  0.0  0.0      0     0 ?        I<   déc.04   0:02 [kworker/3:16H-ttm]
root        1527  0.0  0.0 5855428  420 ?        Ss   déc.04   0:00 [lxc monitor] /var/snap/lxd/common/lxd/containers c0
1000000     1538  0.0  0.2 168520  2816 ?        Ss   déc.04   0:06 /sbin/init
1000000     1663  0.0  0.1  32960   980 ?        Ss   déc.04   0:01 /lib/systemd/systemd-journald
1000000     1671  0.0  0.0  22600   768 ?        Ss   déc.04   0:01 /lib/systemd/systemd-udevd
1000997     1677  0.0  0.1  19796  1920 ?        Ss   déc.04   0:06 /lib/systemd/systemd-resolved
1000100     1679  0.0  0.0   9128     0 ?        Ss   déc.04   0:00 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidf
1000000     1681  0.0  0.1  16536  1280 ?        Ss   déc.04   0:01 /lib/systemd/systemd-logind
1000000     1683  0.0  0.0   5496   128 pts/0    Ss+  déc.04   0:00 /sbin/agetty -o -p -- \u --noclear --keep-baud - 115200,38400,9600
1000998     1688  0.0  0.1  17992  1664 ?        Ss   déc.04   0:02 /lib/systemd/systemd-networkd
root        1690  0.0  0.0      0     0 ?        I<   déc.04   0:00 [cfg80211]
root        1789  0.0  0.0 5855428  540 ?        Ss   déc.04   0:00 [lxc monitor] /var/snap/lxd/common/lxd/containers c1
1000000     1800  0.0  0.5 166524  5504 ?        Ss   déc.04   0:04 /sbin/init
1000000     1922  0.0  0.6  31336  5924 ?        Ss   déc.04   0:01 /lib/systemd/systemd-journald
1000000     1957  0.0  0.0  10568   640 ?        Ss   déc.04   0:01 /lib/systemd/systemd-udevd
1000100     1968  0.0  0.1  16120  1536 ?        Ss   déc.04   0:02 /lib/systemd/systemd-networkd
1000000     1971  0.0  0.0   9492   896 ?        Ss   déc.04   0:01 /usr/sbin/cron -f -P
1000102     1972  0.0  0.0   8552   896 ?        Ss   déc.04   0:00 @dbus-daemon --system --address=systemd: --nofork --nopidfile --sy
1000000     1975  0.0  0.0  34300   504 ?        Ss   déc.04   0:00 /usr/bin/python3 /usr/bin/networkd-dispatcher --run-startup-trigge
1000104     1976  0.0  0.0 152764   896 ?        Ssl  déc.04   0:00 /usr/sbin/rsyslogd -n -iNONE
1000000     1977  0.0  0.1  14904  1280 ?        Ss   déc.04   0:01 /lib/systemd/systemd-logind
1000101     1978  0.0  0.1  25532  1636 ?        Ss   déc.04   0:01 /lib/systemd/systemd-resolved
1000000     1986  0.0  0.0   8396   256 pts/0    Ss+  déc.04   0:00 /sbin/agetty -o -p -- \u --noclear --keep-baud console 115200,3840
1000000     4227  0.0  0.0  55184   384 ?        S    déc.04   0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process
1000033     4229  0.0  0.0  55856   128 ?        S    déc.04   0:00 nginx: worker process
1000033     4230  0.0  0.0  55856   380 ?        S    déc.04   0:00 nginx: worker process
1000033     4231  0.0  0.0  55856   508 ?        S    déc.04   0:00 nginx: worker process
1000033     4232  0.0  0.0  55856   380 ?        S    déc.04   0:00 nginx: worker process
root        4326  0.0  0.0      0     0 ?        I<   déc.04   0:05 [kworker/3:17H-ttm]
1000000     4673  0.0  0.0  10348   256 ?        S    déc.04   0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process
1000033     4675  0.0  0.0  11936   172 ?        S    déc.04   0:00 nginx: worker process
1000033     4676  0.0  0.0  11936   300 ?        S    déc.04   0:00 nginx: worker process
1000033     4677  0.0  0.0  11936   300 ?        S    déc.04   0:00 nginx: worker process
1000033     4678  0.0  0.0  11936   300 ?        S    déc.04   0:00 nginx: worker process
1000000     5361  0.0  0.0  15404   512 ?        Ss   déc.04   0:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
1000000     5401  0.0  0.0  15424   384 ?        Ss   déc.04   0:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root       12588  0.0  0.0      0     0 ?        I    01:44   0:04 [kworker/2:2-dio/nvme0n1p2]
root       12933  0.0  0.0      0     0 ?        I    06:05   0:03 [kworker/2:1-events]
root       12996  0.0  0.0      0     0 ?        I    06:43   0:00 [kworker/0:0-events]
root       13061  0.0  0.0      0     0 ?        I    07:02   0:00 [kworker/1:2]
root       13707  0.0  0.0      0     0 ?        I    14:39   0:00 [kworker/3:1-events]
root       13746  0.0  0.0      0     0 ?        I    15:17   0:00 [kworker/u8:2-events_unbound]
root       13795  0.0  0.0      0     0 ?        I    15:43   0:00 [kworker/u8:7-flush-btrfs-2]
root       13798  0.1  0.0      0     0 ?        I    15:43   0:00 [kworker/0:2-cgroup_destroy]
root       13799  0.0  0.0      0     0 ?        I    15:43   0:00 [kworker/1:1-events]
root       13800  0.0  0.0      0     0 ?        I    15:43   0:00 [kworker/u8:10-flush-btrfs-2]
root       13825  0.0  1.1  18316 11332 ?        Ss   15:51   0:00 sshd: etu [priv]
etu        13829  0.0  1.1  19540 10624 ?        Ss   15:51   0:00 /lib/systemd/systemd --user
root       13830  0.1  0.0      0     0 ?        I    15:51   0:00 [kworker/0:1-events]
etu        13831  0.0  0.4  22432  3952 ?        S    15:51   0:00 (sd-pam)
etu        13852  0.0  0.7  18316  6904 ?        S    15:51   0:00 sshd: etu@pts/0
etu        13853  0.0  0.4   8072  4608 pts/0    Ss   15:51   0:00 -bash
root       14375  0.0  0.0      0     0 ?        I    15:54   0:00 [kworker/u8:0-btrfs-endio-meta]
root       14416  0.0  0.2   6572  2272 ?        Ss   15:54   0:00 /usr/sbin/apache2 -k start
www-data   14418  0.0  0.5 1933448 5108 ?        Sl   15:54   0:00 /usr/sbin/apache2 -k start
www-data   14419  0.0  0.9 1998984 9196 ?        Sl   15:54   0:00 /usr/sbin/apache2 -k start
www-data   14514  0.0  0.1   3412  1292 ?        Ss   15:54   0:00 /usr/bin/htcacheclean -d 120 -p /var/cache/apache2/mod_cache_disk -l
root       14578  0.0  0.0      0     0 ?        S    15:54   0:00 [psimon]
etu        14618 50.0  0.5  12228  5120 pts/0    R+   15:54   0:00 ps -aux
etu@vm0:~$ ps auxf | grep apache
etu        14620  0.0  0.2   6364  2304 pts/0    S+   15:55   0:00              \_ grep apache
root       14416  0.0  0.2   6572  2272 ?        Ss   15:54   0:00 /usr/sbin/apache2 -k start
www-data   14418  0.0  0.5 1933448 5108 ?        Sl   15:54   0:00  \_ /usr/sbin/apache2 -k start
www-data   14419  0.0  0.9 1998984 9196 ?        Sl   15:54   0:00  \_ /usr/sbin/apache2 -k start
www-data   14514  0.0  0.1   3412  1292 ?        Ss   15:54   0:00 /usr/bin/htcacheclean -d 120 -p /var/cache/apache2/mod_cache_disk -l 300M -n
etu@vm0:~$ sudo kill -9 14418
etu@vm0:~$ ps auxf | grep apache
etu        14660  0.0  0.2   6364  2304 pts/0    S+   15:59   0:00              \_ grep apache
root       14416  0.0  0.2   6572  2400 ?        Ss   15:54   0:00 /usr/sbin/apache2 -k start
www-data   14419  0.0  0.9 1998984 9196 ?        Sl   15:54   0:00  \_ /usr/sbin/apache2 -k start
www-data   14631  0.1  1.2 1998984 12340 ?       Sl   15:59   0:00  \_ /usr/sbin/apache2 -k start
www-data   14514  0.0  0.1   3412  1292 ?        Ss   15:54   0:00 /usr/bin/htcacheclean -d 120 -p /var/cache/apache2/mod_cache_disk -l 300M -n
etu@vm0:~$ sudo killall apache2
etu@vm0:~$ ps auxf | grep apache
etu        14673  0.0  0.2   6364  2432 pts/0    S+   16:00   0:00              \_ grep apache
www-data   14514  0.0  0.1   3412  1292 ?        Ss   15:54   0:00 /usr/bin/htcacheclean -d 120 -p /var/cache/apache2/mod_cache_disk -l 300M -n
etu@vm0:~$ sudo systemctl status apache2
○ apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; preset: enabled)
     Active: inactive (dead) since Fri 2023-12-08 16:00:18 CET; 1min 37s ago
   Duration: 5min 57.425s
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 14664 ExecStop=/usr/sbin/apachectl graceful-stop (code=exited, status=0/SUCCESS)
   Main PID: 14416 (code=exited, status=0/SUCCESS)
        CPU: 187ms

déc. 08 15:54:21 vm0 systemd[1]: Starting apache2.service - The Apache HTTP Server...
déc. 08 15:54:21 vm0 apachectl[14415]: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using >
déc. 08 15:54:21 vm0 systemd[1]: Started apache2.service - The Apache HTTP Server.
déc. 08 16:00:18 vm0 apachectl[14666]: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using >
déc. 08 16:00:18 vm0 apachectl[14666]: httpd (no pid file) not running
déc. 08 16:00:18 vm0 systemd[1]: apache2.service: Deactivated successfully.
 ESCOC

preset: enabled)
n 37s ago


=exited, status=0/SUCCESS)



che HTTP Server...
reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this>
he HTTP Server.
reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this>
ing
ssfully.

 ESCOD
○ apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; preset: enabled)
     Active: inactive (dead) since Fri 2023-12-08 16:00:18 CET; 1min 37s ago
   Duration: 5min 57.425s
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 14664 ExecStop=/usr/sbin/apachectl graceful-stop (code=exited, status=0/SUCCESS)
   Main PID: 14416 (code=exited, status=0/SUCCESS)
        CPU: 187ms

déc. 08 15:54:21 vm0 systemd[1]: Starting apache2.service - The Apache HTTP Server...
déc. 08 15:54:21 vm0 apachectl[14415]: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using >
déc. 08 15:54:21 vm0 systemd[1]: Started apache2.service - The Apache HTTP Server.
déc. 08 16:00:18 vm0 apachectl[14666]: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using >
déc. 08 16:00:18 vm0 apachectl[14666]: httpd (no pid file) not running
déc. 08 16:00:18 vm0 systemd[1]: apache2.service: Deactivated successfully.
 ESCOD
○ apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; preset: enabled)
     Active: inactive (dead) since Fri 2023-12-08 16:00:18 CET; 1min 37s ago
   Duration: 5min 57.425s
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 14664 ExecStop=/usr/sbin/apachectl graceful-stop (code=exited, status=0/SUCCESS)
   Main PID: 14416 (code=exited, status=0/SUCCESS)
        CPU: 187ms

déc. 08 15:54:21 vm0 systemd[1]: Starting apache2.service - The Apache HTTP Server...
déc. 08 15:54:21 vm0 apachectl[14415]: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using >
déc. 08 15:54:21 vm0 systemd[1]: Started apache2.service - The Apache HTTP Server.
déc. 08 16:00:18 vm0 apachectl[14666]: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using >
déc. 08 16:00:18 vm0 apachectl[14666]: httpd (no pid file) not running
déc. 08 16:00:18 vm0 systemd[1]: apache2.service: Deactivated successfully.
lines 1-15/15 (END)
etu@vm0:~$ sudo systemctl restart apache2
etu@vm0:~$ sudo systemctl status apache2
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; preset: enabled)
     Active: active (running) since Fri 2023-12-08 16:02:44 CET; 10s ago
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 14689 ExecStart=/usr/sbin/apachectl start (code=exited, status=0/SUCCESS)
   Main PID: 14692 (apache2)
      Tasks: 55 (limit: 1079)
     Memory: 19.2M
        CPU: 64ms
     CGroup: /system.slice/apache2.service
             ├─14692 /usr/sbin/apache2 -k start
             ├─14694 /usr/sbin/apache2 -k start
             └─14695 /usr/sbin/apache2 -k start

déc. 08 16:02:44 vm0 systemd[1]: Starting apache2.service - The Apache HTTP Server...
déc. 08 16:02:44 vm0 apachectl[14691]: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using >
déc. 08 16:02:44 vm0 systemd[1]: Started apache2.service - The Apache HTTP Server.
```
## Service web statique - vue 19/29
```Powershell
etu@vm0:~$ sudo apt install task-web-server -y
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances... Fait
Lecture des informations d'état... Fait
Les paquets supplémentaires suivants seront installés :
  analog apache2-doc libabsl20220623 libaom3 libavif16 libdav1d7 libde265-0 libdeflate0 libgav1-1 libgd3 libheif-plugin-aomenc
  libheif-plugin-dav1d libheif-plugin-libde265 libheif-plugin-x265 libheif1 libjbig0 libjpeg62-turbo liblerc4 librav1e0 libsharpyuv0
  libsvtav1enc1d1 libtiff6 libwebp7 libx265-199 libyuv0
Paquets suggérés :
  rmagic libgd-tools libheif-plugin-ffmpegdec libheif-plugin-jpegdec libheif-plugin-jpegenc libheif-plugin-j2kdec
  libheif-plugin-j2kenc libheif-plugin-rav1e libheif-plugin-svtenc
Les NOUVEAUX paquets suivants seront installés :
  analog apache2-doc libabsl20220623 libaom3 libavif16 libdav1d7 libde265-0 libdeflate0 libgav1-1 libgd3 libheif-plugin-aomenc
  libheif-plugin-dav1d libheif-plugin-libde265 libheif-plugin-x265 libheif1 libjbig0 libjpeg62-turbo liblerc4 librav1e0 libsharpyuv0
  libsvtav1enc1d1 libtiff6 libwebp7 libx265-199 libyuv0 task-web-server
0 mis à jour, 26 nouvellement installés, 0 à enlever et 0 non mis à jour.
Il est nécessaire de prendre 14,6 Mo dans les archives.
Après cette opération, 72,9 Mo d'espace disque supplémentaires seront utilisés.
Réception de :1 http://deb.debian.org/debian testing/main amd64 libaom3 amd64 3.7.1-1 [1 861 kB]
Réception de :2 http://deb.debian.org/debian testing/main amd64 libdav1d7 amd64 1.3.0-2 [543 kB]
Réception de :3 http://deb.debian.org/debian testing/main amd64 libabsl20220623 amd64 20220623.1-3 [393 kB]
Réception de :4 http://deb.debian.org/debian testing/main amd64 libgav1-1 amd64 0.18.0-1+b1 [332 kB]
Réception de :5 http://deb.debian.org/debian testing/main amd64 librav1e0 amd64 0.6.6-4 [899 kB]
Réception de :6 http://deb.debian.org/debian testing/main amd64 libsharpyuv0 amd64 1.3.2-0.3 [107 kB]
Réception de :7 http://deb.debian.org/debian testing/main amd64 libsvtav1enc1d1 amd64 1.7.0+dfsg-2 [2 169 kB]
Réception de :8 http://deb.debian.org/debian testing/main amd64 libjpeg62-turbo amd64 1:2.1.5-2 [166 kB]
Réception de :9 http://deb.debian.org/debian testing/main amd64 libyuv0 amd64 0.0~git20231118.e6d5adb-1 [169 kB]
Réception de :10 http://deb.debian.org/debian testing/main amd64 libavif16 amd64 1.0.1-2+b1 [105 kB]
Réception de :11 http://deb.debian.org/debian testing/main amd64 libheif-plugin-dav1d amd64 1.17.1-1+b1 [9 772 B]
Réception de :12 http://deb.debian.org/debian testing/main amd64 libde265-0 amd64 1.0.13-1 [187 kB]
Réception de :13 http://deb.debian.org/debian testing/main amd64 libheif-plugin-libde265 amd64 1.17.1-1+b1 [13,0 kB]
Réception de :14 http://deb.debian.org/debian testing/main amd64 libheif1 amd64 1.17.1-1+b1 [281 kB]
Réception de :15 http://deb.debian.org/debian testing/main amd64 libdeflate0 amd64 1.18-1 [40,9 kB]
Réception de :16 http://deb.debian.org/debian testing/main amd64 libjbig0 amd64 2.1-6.1 [31,7 kB]
Réception de :17 http://deb.debian.org/debian testing/main amd64 liblerc4 amd64 4.0.0+ds-3 [170 kB]
Réception de :18 http://deb.debian.org/debian testing/main amd64 libwebp7 amd64 1.3.2-0.3 [308 kB]
Réception de :19 http://deb.debian.org/debian testing/main amd64 libtiff6 amd64 4.5.1+git230720-1 [322 kB]
Réception de :20 http://deb.debian.org/debian testing/main amd64 libgd3 amd64 2.3.3-9+b1 [125 kB]
Réception de :21 http://deb.debian.org/debian testing/main amd64 analog amd64 2:6.0.17-3 [1 119 kB]
Réception de :22 http://deb.debian.org/debian testing/main amd64 apache2-doc all 2.4.58-1 [4 018 kB]
Réception de :23 http://deb.debian.org/debian testing/main amd64 libheif-plugin-aomenc amd64 1.17.1-1+b1 [19,9 kB]
Réception de :24 http://deb.debian.org/debian testing/main amd64 libx265-199 amd64 3.5-2+b1 [1 150 kB]
Réception de :25 http://deb.debian.org/debian testing/main amd64 libheif-plugin-x265 amd64 1.17.1-1+b1 [18,7 kB]
Réception de :26 http://deb.debian.org/debian testing/main amd64 task-web-server all 3.73 [908 B]
14,6 Mo réceptionnés en 1s (11,1 Mo/s)
Préconfiguration des paquets...
Sélection du paquet libaom3:amd64 précédemment désélectionné.
(Lecture de la base de données... 34753 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../00-libaom3_3.7.1-1_amd64.deb ...
Dépaquetage de libaom3:amd64 (3.7.1-1) ...
Sélection du paquet libdav1d7:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../01-libdav1d7_1.3.0-2_amd64.deb ...
Dépaquetage de libdav1d7:amd64 (1.3.0-2) ...
Sélection du paquet libabsl20220623:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../02-libabsl20220623_20220623.1-3_amd64.deb ...
Dépaquetage de libabsl20220623:amd64 (20220623.1-3) ...
Sélection du paquet libgav1-1:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../03-libgav1-1_0.18.0-1+b1_amd64.deb ...
Dépaquetage de libgav1-1:amd64 (0.18.0-1+b1) ...
Sélection du paquet librav1e0:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../04-librav1e0_0.6.6-4_amd64.deb ...
Dépaquetage de librav1e0:amd64 (0.6.6-4) ...
Sélection du paquet libsharpyuv0:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../05-libsharpyuv0_1.3.2-0.3_amd64.deb ...
Dépaquetage de libsharpyuv0:amd64 (1.3.2-0.3) ...
Sélection du paquet libsvtav1enc1d1:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../06-libsvtav1enc1d1_1.7.0+dfsg-2_amd64.deb ...
Dépaquetage de libsvtav1enc1d1:amd64 (1.7.0+dfsg-2) ...
Sélection du paquet libjpeg62-turbo:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../07-libjpeg62-turbo_1%3a2.1.5-2_amd64.deb ...
Dépaquetage de libjpeg62-turbo:amd64 (1:2.1.5-2) ...
Sélection du paquet libyuv0:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../08-libyuv0_0.0~git20231118.e6d5adb-1_amd64.deb ...
Dépaquetage de libyuv0:amd64 (0.0~git20231118.e6d5adb-1) ...
Sélection du paquet libavif16:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../09-libavif16_1.0.1-2+b1_amd64.deb ...
Dépaquetage de libavif16:amd64 (1.0.1-2+b1) ...
Sélection du paquet libheif-plugin-dav1d:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../10-libheif-plugin-dav1d_1.17.1-1+b1_amd64.deb ...
Dépaquetage de libheif-plugin-dav1d:amd64 (1.17.1-1+b1) ...
Sélection du paquet libde265-0:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../11-libde265-0_1.0.13-1_amd64.deb ...
Dépaquetage de libde265-0:amd64 (1.0.13-1) ...
Sélection du paquet libheif-plugin-libde265:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../12-libheif-plugin-libde265_1.17.1-1+b1_amd64.deb ...
Dépaquetage de libheif-plugin-libde265:amd64 (1.17.1-1+b1) ...
Sélection du paquet libheif1:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../13-libheif1_1.17.1-1+b1_amd64.deb ...
Dépaquetage de libheif1:amd64 (1.17.1-1+b1) ...
Sélection du paquet libdeflate0:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../14-libdeflate0_1.18-1_amd64.deb ...
Dépaquetage de libdeflate0:amd64 (1.18-1) ...
Sélection du paquet libjbig0:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../15-libjbig0_2.1-6.1_amd64.deb ...
Dépaquetage de libjbig0:amd64 (2.1-6.1) ...
Sélection du paquet liblerc4:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../16-liblerc4_4.0.0+ds-3_amd64.deb ...
Dépaquetage de liblerc4:amd64 (4.0.0+ds-3) ...
Sélection du paquet libwebp7:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../17-libwebp7_1.3.2-0.3_amd64.deb ...
Dépaquetage de libwebp7:amd64 (1.3.2-0.3) ...
Sélection du paquet libtiff6:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../18-libtiff6_4.5.1+git230720-1_amd64.deb ...
Dépaquetage de libtiff6:amd64 (4.5.1+git230720-1) ...
Sélection du paquet libgd3:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../19-libgd3_2.3.3-9+b1_amd64.deb ...
Dépaquetage de libgd3:amd64 (2.3.3-9+b1) ...
Sélection du paquet analog précédemment désélectionné.
Préparation du dépaquetage de .../20-analog_2%3a6.0.17-3_amd64.deb ...
Dépaquetage de analog (2:6.0.17-3) ...
Sélection du paquet apache2-doc précédemment désélectionné.
Préparation du dépaquetage de .../21-apache2-doc_2.4.58-1_all.deb ...
Dépaquetage de apache2-doc (2.4.58-1) ...
Sélection du paquet libheif-plugin-aomenc:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../22-libheif-plugin-aomenc_1.17.1-1+b1_amd64.deb ...
Dépaquetage de libheif-plugin-aomenc:amd64 (1.17.1-1+b1) ...
Sélection du paquet libx265-199:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../23-libx265-199_3.5-2+b1_amd64.deb ...
Dépaquetage de libx265-199:amd64 (3.5-2+b1) ...
Sélection du paquet libheif-plugin-x265:amd64 précédemment désélectionné.
Préparation du dépaquetage de .../24-libheif-plugin-x265_1.17.1-1+b1_amd64.deb ...
Dépaquetage de libheif-plugin-x265:amd64 (1.17.1-1+b1) ...
Sélection du paquet task-web-server précédemment désélectionné.
Préparation du dépaquetage de .../25-task-web-server_3.73_all.deb ...
Dépaquetage de task-web-server (3.73) ...
Paramétrage de libsharpyuv0:amd64 (1.3.2-0.3) ...
Paramétrage de libaom3:amd64 (3.7.1-1) ...
Paramétrage de libabsl20220623:amd64 (20220623.1-3) ...
Paramétrage de task-web-server (3.73) ...
Paramétrage de liblerc4:amd64 (4.0.0+ds-3) ...
Paramétrage de libdeflate0:amd64 (1.18-1) ...
Paramétrage de libsvtav1enc1d1:amd64 (1.7.0+dfsg-2) ...
Paramétrage de libjbig0:amd64 (2.1-6.1) ...
Paramétrage de librav1e0:amd64 (0.6.6-4) ...
Paramétrage de libjpeg62-turbo:amd64 (1:2.1.5-2) ...
Paramétrage de apache2-doc (2.4.58-1) ...
Paramétrage de libgav1-1:amd64 (0.18.0-1+b1) ...
Paramétrage de libx265-199:amd64 (3.5-2+b1) ...
Paramétrage de libwebp7:amd64 (1.3.2-0.3) ...
Paramétrage de libdav1d7:amd64 (1.3.0-2) ...
Paramétrage de libtiff6:amd64 (4.5.1+git230720-1) ...
Paramétrage de libde265-0:amd64 (1.0.13-1) ...
Paramétrage de libyuv0:amd64 (0.0~git20231118.e6d5adb-1) ...
Paramétrage de libavif16:amd64 (1.0.1-2+b1) ...
Paramétrage de libheif-plugin-dav1d:amd64 (1.17.1-1+b1) ...
Paramétrage de libheif-plugin-libde265:amd64 (1.17.1-1+b1) ...
Paramétrage de libheif1:amd64 (1.17.1-1+b1) ...
Paramétrage de libgd3:amd64 (2.3.3-9+b1) ...
Paramétrage de analog (2:6.0.17-3) ...
Paramétrage de libheif-plugin-x265:amd64 (1.17.1-1+b1) ...
Paramétrage de libheif-plugin-aomenc:amd64 (1.17.1-1+b1) ...
Traitement des actions différées (« triggers ») pour libc-bin (2.37-12) ...
Traitement des actions différées (« triggers ») pour man-db (2.12.0-1) ...
Scanning processes...
Scanning linux images...

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.
etu@vm0:~$ umask 027
etu@vm0:~$ umask
0027

etu@vm0:~$ cd /var/www
etu@vm0:/var/www$ sudo chmod -R 2750 newhtml/
[sudo] Mot de passe de etu :
etu@vm0:/var/www$ ls -lA
total 8
drwxr-xr-x 2 root     root 4096  8 déc.  15:54 html
drwxr-s--- 2 www-data etu  4096  8 déc.  16:20 newhtml
etu@vm0:/var/www$ sudo chown -R etu:www-data newhtml/
etu@vm0:/var/www$ ls -lA
total 8
drwxr-xr-x 2 root root     4096  8 déc.  15:54 html
drwxr-s--- 2 etu  www-data 4096  8 déc.  16:20 newhtml
etu@vm0:/var/www$
etu@vm0:/var/www$ vim /var/www/newhtml/index.html
etu@vm0:~$ cat /var/www/newhtml/index.html
<html>

        <body>
                <h1> Hello,World </h1>
                <h1> I am Chehab MOSAAD  </h1>
        </body>

</html>
etu@vm0:/var/www$ cat /etc/hosts
127.0.0.1       localhost
127.0.1.1       vm0
127.0.0.2       newsite

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnode
ff02::2 ip6-allrouters
etu@vm0:/var/www$ sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/newsite.conf
etu@vm0:/var/www$ sudo a2ensite newsite
Enabling site newsite.
To activate the new configuration, you need to run:
  systemctl reload apache2
etu@vm0:/var/www$ sudo systemctl reload apache2
etu@vm0:/var/www$ sudo a2ensite newsite
Site newsite already enabled
etu@vm0:/var/www$ cd /etc/apache2/sites-available/
etu@vm0:/etc/apache2/sites-available$ cat newsite.conf
<VirtualHost 127.0.0.2:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/newhtml/

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
etu@vm0:/etc/apache2/sites-available$ cd /var/www
etu@vm0:/var/www$ sudo systemctl restart apache2
etu@vm0:/var/www$ w3m newsite
etu@vm0:/var/www$ cd /var/log/apache2
etu@vm0:/var/log/apache2$ cat access.log
127.0.0.1 - - [11/Dec/2023:17:35:03 +0100] "GET / HTTP/1.0" 200 393 "-" "w3m/0.5.3+git20230121"
```
## Cas jekyll
```Powershell
etu@vm0:~$ sudo apt install openvswitch-switch snapd
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances... Fait
Lecture des informations d'état... Fait
openvswitch-switch est déjà la version la plus récente (3.2.2~git20231029-2+b1).
snapd est déjà la version la plus récente (2.60.2-1+b2).
0 mis à jour, 0 nouvellement installés, 0 à enlever et 0 non mis à jour.
etu@vm0:~$ sudo snap install lxd
snap "lxd" is already installed, see 'snap help refresh'
etu@vm0:~$ sudo adduser etu lxd
info: L'utilisateur « etu » appartient déjà au groupe « lxd ».
etu@vm0:~$ id
),44(video),46(plugdev),100(users),106(netdev),995(lxd)
etu@vm0:~$ cat /etc/network/interfaces
#This file describes the network interfaces available on your system and how to activate them.
#For more information, see interfaces(5).
source /etc/network/interfaces.d/*

#The loopback network interface
auto lo
iface lo inet loopback

#The primary network interface
auto C-3P0
iface C-3P0 inet dhcp
        ovs_type OVSBridge
        ovs_ports enp0s1

allow-C-3P0 enp0s1
iface enp0s1 inet manual
        ovs_bridge C-3P0
        ovs_type OVSPort
        up ip link set dev $IFACE up
        down ip link set dev $IFACE down

etu@vm0:~$ sudo reboot

Broadcast message from root@vm0 on pts/2 (Mon 2023-12-11 17:38:47 CET):

The system will reboot now!

etu@vm0:~$ Connection to 2001:678:3fc:3c:baad:caff:fefe:39 closed by remote host.
Connection to 2001:678:3fc:3c:baad:caff:fefe:39 closed.
PS C:\Users\hp> ssh vm0
ssh: connect to host vm0 port 2222: Connection timed out
PS C:\Users\hp>
PS C:\Users\hp> ssh vm0
etu@2001:678:3fc:3c:baad:caff:fefe:39's password:
Linux vm0 6.5.0-5-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.13-1 (2023-11-29) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Mon Dec 11 16:55:17 2023 from 2a03:7220:8083:c4c4::1012
etu@vm0:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
2: enp0s1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq master ovs-system state UP group default qlen 1000
    link/ether b8:ad:ca:fe:00:39 brd ff:ff:ff:ff:ff:ff
    inet6 fe80::baad:caff:fefe:39/64 scope link proto kernel_ll
       valid_lft forever preferred_lft forever
3: ovs-system: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 9a:a2:da:57:72:90 brd ff:ff:ff:ff:ff:ff
4: C-3P0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN group default qlen 1000
    link/ether b8:ad:ca:fe:00:39 brd ff:ff:ff:ff:ff:ff
    inet 198.18.61.57/23 brd 198.18.61.255 scope global dynamic C-3P0
       valid_lft 86371sec preferred_lft 86371sec
    inet6 2001:678:3fc:3c:baad:caff:fefe:39/64 scope global dynamic mngtmpaddr proto kernel_ra
       valid_lft 86385sec preferred_lft 14385sec
    inet6 fe80::baad:caff:fefe:39/64 scope link proto kernel_ll
       valid_lft forever preferred_lft forever
etu@vm0:~$ lxd init
Would you like to use LXD clustering? (yes/no) [default=no]:
Do you want to configure a new storage pool? (yes/no) [default=yes]:
Name of the new storage pool [default=default]:
Name of the storage backend to use (lvm, btrfs, ceph, dir) [default=btrfs]:
Create a new BTRFS pool? (yes/no) [default=yes]:
Would you like to use an existing empty block device (e.g. a disk or partition)? (yes/no) [default=no]:
Size in GiB of the new loop device (1GiB minimum) [default=11GiB]:
Would you like to connect to a MAAS server? (yes/no) [default=no]:
Would you like to create a new local network bridge? (yes/no) [default=yes]: no
Would you like to configure LXD to use an existing bridge or host interface? (yes/no) [default=no]: yes
Name of the existing bridge or host interface: C-3P0
Would you like the LXD server to be available over the network? (yes/no) [default=no]:
Would you like stale cached images to be updated automatically? (yes/no) [default=yes]:
Would you like a YAML "lxd init" preseed to be printed? (yes/no) [default=no]:
etu@vm0:~$ lxc profile show default
config: {}
description: Default LXD profile
devices:
  eth0:
    name: eth0
    nictype: macvlan
    parent: C-3P0
    type: nic
  root:
    path: /
    pool: default
    type: disk
name: default
used_by: []
etu@vm0:~$ lxc profile device set default eth0 nictype bridged
etu@vm0:~$ lxc profile show default
config: {}
description: Default LXD profile
devices:
  eth0:
    name: eth0
    nictype: bridged
    parent: C-3P0
    type: nic
  root:
    path: /
    pool: default
    type: disk
name: default
used_by: []
etu@vm0:~$ lxc launch images:debian/12 jekyll
Creating jekyll
Starting jekyll
etu@vm0:~$ lxc exec jekyll -- apt update
Hit:1 http://deb.debian.org/debian bookworm InRelease
Get:2 http://deb.debian.org/debian bookworm-updates InRelease [52.1 kB]
Hit:3 http://deb.debian.org/debian-security bookworm-security InRelease
Fetched 52.1 kB in 1s (53.8 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
All packages are up to date.
etu@vm0:~$ lxc exec jekyll -- apt -y install nginx htop ruby-full build-essential
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  binutils binutils-common binutils-x86-64-linux-gnu bzip2 ca-certificates cpp
  cpp-12 dirmngr dpkg-dev fakeroot fontconfig-config fonts-dejavu-core
  fonts-lato g++ g++-12 gcc gcc-12 gnupg gnupg-l10n gnupg-utils gpg gpg-agent
  gpg-wks-client gpg-wks-server gpgconf gpgsm javascript-common libabsl20220623
  libalgorithm-diff-perl libalgorithm-diff-xs-perl libalgorithm-merge-perl
  libaom3 libasan8 libassuan0 libatomic1 libavif15 libbinutils libbrotli1
  libc-dev-bin libc-devtools libc6-dev libcc1-0 libcrypt-dev libctf-nobfd0
  libctf0 libdav1d6 libde265-0 libdeflate0 libdpkg-perl libfakeroot
  libfile-fcntllock-perl libfontconfig1 libfreetype6 libgav1-1 libgcc-12-dev
  libgd3 libgmp-dev libgmpxx4ldbl libgomp1 libgprofng0 libheif1 libisl23 libitm1
  libjansson4 libjbig0 libjpeg62-turbo libjs-jquery libksba8 libldap-2.5-0
  libldap-common liblerc4 liblocale-gettext-perl liblsan0 libmpc3 libmpfr6
  libncurses6 libnl-3-200 libnl-genl-3-200 libnpth0 libnsl-dev libnsl2 libnuma1
  libpng16-16 libquadmath0 librav1e0 libreadline8 libruby libruby3.1 libsasl2-2
  libsasl2-modules libsasl2-modules-db libsqlite3-0 libstdc++-12-dev
  libsvtav1enc1 libtiff6 libtirpc-dev libtsan2 libubsan1 libwebp7 libx265-199
  libxpm4 libyaml-0-2 libyuv0 linux-libc-dev make manpages manpages-dev
  nginx-common openssl patch pinentry-curses rake readline-common ri
  rpcsvc-proto ruby ruby-dev ruby-net-telnet ruby-rubygems ruby-sdbm
  ruby-webrick ruby-xmlrpc ruby3.1 ruby3.1-dev ruby3.1-doc rubygems-integration
  unzip xz-utils zip
Suggested packages:
  binutils-doc bzip2-doc cpp-doc gcc-12-locales cpp-12-doc pinentry-gnome3 tor
  debian-keyring g++-multilib g++-12-multilib gcc-12-doc gcc-multilib autoconf
  automake libtool flex bison gdb gcc-doc gcc-12-multilib parcimonie xloadimage
  scdaemon lm-sensors lsof strace glibc-doc sensible-utils git bzr libgd-tools
  gmp-doc libgmp10-doc libmpfr-dev libsasl2-modules-gssapi-mit
  | libsasl2-modules-gssapi-heimdal libsasl2-modules-ldap libsasl2-modules-otp
  libsasl2-modules-sql libstdc++-12-doc make-doc man-browser fcgiwrap nginx-doc
  ssl-cert ed diffutils-doc pinentry-doc readline-doc bundler
The following NEW packages will be installed:
  binutils binutils-common binutils-x86-64-linux-gnu build-essential bzip2
  ca-certificates cpp cpp-12 dirmngr dpkg-dev fakeroot fontconfig-config
  fonts-dejavu-core fonts-lato g++ g++-12 gcc gcc-12 gnupg gnupg-l10n
  gnupg-utils gpg gpg-agent gpg-wks-client gpg-wks-server gpgconf gpgsm htop
  javascript-common libabsl20220623 libalgorithm-diff-perl
  libalgorithm-diff-xs-perl libalgorithm-merge-perl libaom3 libasan8 libassuan0
  libatomic1 libavif15 libbinutils libbrotli1 libc-dev-bin libc-devtools
  libc6-dev libcc1-0 libcrypt-dev libctf-nobfd0 libctf0 libdav1d6 libde265-0
  libdeflate0 libdpkg-perl libfakeroot libfile-fcntllock-perl libfontconfig1
  libfreetype6 libgav1-1 libgcc-12-dev libgd3 libgmp-dev libgmpxx4ldbl libgomp1
  libgprofng0 libheif1 libisl23 libitm1 libjansson4 libjbig0 libjpeg62-turbo
  libjs-jquery libksba8 libldap-2.5-0 libldap-common liblerc4
  liblocale-gettext-perl liblsan0 libmpc3 libmpfr6 libncurses6 libnl-3-200
  libnl-genl-3-200 libnpth0 libnsl-dev libnsl2 libnuma1 libpng16-16 libquadmath0
  librav1e0 libreadline8 libruby libruby3.1 libsasl2-2 libsasl2-modules
  libsasl2-modules-db libsqlite3-0 libstdc++-12-dev libsvtav1enc1 libtiff6
  libtirpc-dev libtsan2 libubsan1 libwebp7 libx265-199 libxpm4 libyaml-0-2
  libyuv0 linux-libc-dev make manpages manpages-dev nginx nginx-common openssl
  patch pinentry-curses rake readline-common ri rpcsvc-proto ruby ruby-dev
  ruby-full ruby-net-telnet ruby-rubygems ruby-sdbm ruby-webrick ruby-xmlrpc
  ruby3.1 ruby3.1-dev ruby3.1-doc rubygems-integration unzip xz-utils zip
0 upgraded, 133 newly installed, 0 to remove and 0 not upgraded.
Need to get 108 MB of archives.
After this operation, 411 MB of additional disk space will be used.
Get:1 http://deb.debian.org/debian bookworm/main amd64 fonts-lato all 2.0-2.1 [2696 kB]
Get:2 http://deb.debian.org/debian bookworm/main amd64 liblocale-gettext-perl amd64 1.07-5 [15.4 kB]
Get:3 http://deb.debian.org/debian bookworm/main amd64 readline-common all 8.2-1.3 [69.0 kB]
Get:4 http://deb.debian.org/debian bookworm/main amd64 bzip2 amd64 1.0.8-5+b1 [49.8 kB]
Get:5 http://deb.debian.org/debian bookworm/main amd64 openssl amd64 3.0.11-1~deb12u2 [1419 kB]
Get:6 http://deb.debian.org/debian bookworm/main amd64 ca-certificates all 20230311 [153 kB]
Get:7 http://deb.debian.org/debian bookworm/main amd64 manpages all 6.03-2 [1332 kB]
Get:8 http://deb.debian.org/debian bookworm/main amd64 xz-utils amd64 5.4.1-0.2 [471 kB]
Get:9 http://deb.debian.org/debian bookworm/main amd64 binutils-common amd64 2.40-2 [2487 kB]
Get:10 http://deb.debian.org/debian bookworm/main amd64 libbinutils amd64 2.40-2 [572 kB]
Get:11 http://deb.debian.org/debian bookworm/main amd64 libctf-nobfd0 amd64 2.40-2 [153 kB]
Get:12 http://deb.debian.org/debian bookworm/main amd64 libctf0 amd64 2.40-2 [89.8 kB]
Get:13 http://deb.debian.org/debian bookworm/main amd64 libgprofng0 amd64 2.40-2 [812 kB]
Get:14 http://deb.debian.org/debian bookworm/main amd64 libjansson4 amd64 2.14-2 [40.8 kB]
Get:15 http://deb.debian.org/debian bookworm/main amd64 binutils-x86-64-linux-gnu amd64 2.40-2 [2246 kB]
Get:16 http://deb.debian.org/debian bookworm/main amd64 binutils amd64 2.40-2 [65.0 kB]
Get:17 http://deb.debian.org/debian bookworm/main amd64 libc-dev-bin amd64 2.36-9+deb12u3 [45.2 kB]
Get:18 http://deb.debian.org/debian bookworm/main amd64 linux-libc-dev amd64 6.1.66-1 [1905 kB]
Get:19 http://deb.debian.org/debian bookworm/main amd64 libcrypt-dev amd64 1:4.4.33-2 [118 kB]
Get:20 http://deb.debian.org/debian bookworm/main amd64 libnsl2 amd64 1.3.0-2 [39.5 kB]
Get:21 http://deb.debian.org/debian bookworm/main amd64 libtirpc-dev amd64 1.3.3+ds-1 [191 kB]
Get:22 http://deb.debian.org/debian bookworm/main amd64 libnsl-dev amd64 1.3.0-2 [66.4 kB]
Get:23 http://deb.debian.org/debian bookworm/main amd64 rpcsvc-proto amd64 1.4.3-1 [63.3 kB]
Get:24 http://deb.debian.org/debian bookworm/main amd64 libc6-dev amd64 2.36-9+deb12u3 [1898 kB]
Get:25 http://deb.debian.org/debian bookworm/main amd64 libisl23 amd64 0.25-1 [690 kB]
Get:26 http://deb.debian.org/debian bookworm/main amd64 libmpfr6 amd64 4.2.0-1 [701 kB]
Get:27 http://deb.debian.org/debian bookworm/main amd64 libmpc3 amd64 1.3.1-1 [51.5 kB]
Get:28 http://deb.debian.org/debian bookworm/main amd64 cpp-12 amd64 12.2.0-14 [9764 kB]
Get:29 http://deb.debian.org/debian bookworm/main amd64 cpp amd64 4:12.2.0-3 [6836 B]
Get:30 http://deb.debian.org/debian bookworm/main amd64 libcc1-0 amd64 12.2.0-14 [41.7 kB]
Get:31 http://deb.debian.org/debian bookworm/main amd64 libgomp1 amd64 12.2.0-14 [116 kB]
Get:32 http://deb.debian.org/debian bookworm/main amd64 libitm1 amd64 12.2.0-14 [26.1 kB]
Get:33 http://deb.debian.org/debian bookworm/main amd64 libatomic1 amd64 12.2.0-14 [9328 B]
Get:34 http://deb.debian.org/debian bookworm/main amd64 libasan8 amd64 12.2.0-14 [2195 kB]
Get:35 http://deb.debian.org/debian bookworm/main amd64 liblsan0 amd64 12.2.0-14 [969 kB]
Get:36 http://deb.debian.org/debian bookworm/main amd64 libtsan2 amd64 12.2.0-14 [2196 kB]
Get:37 http://deb.debian.org/debian bookworm/main amd64 libubsan1 amd64 12.2.0-14 [883 kB]
Get:38 http://deb.debian.org/debian bookworm/main amd64 libquadmath0 amd64 12.2.0-14 [144 kB]
Get:39 http://deb.debian.org/debian bookworm/main amd64 libgcc-12-dev amd64 12.2.0-14 [2437 kB]
Get:40 http://deb.debian.org/debian bookworm/main amd64 gcc-12 amd64 12.2.0-14 [19.3 MB]
Get:41 http://deb.debian.org/debian bookworm/main amd64 gcc amd64 4:12.2.0-3 [5216 B]
Get:42 http://deb.debian.org/debian bookworm/main amd64 libstdc++-12-dev amd64 12.2.0-14 [2046 kB]
Get:43 http://deb.debian.org/debian bookworm/main amd64 g++-12 amd64 12.2.0-14 [10.7 MB]
Get:44 http://deb.debian.org/debian bookworm/main amd64 g++ amd64 4:12.2.0-3 [1356 B]
Get:45 http://deb.debian.org/debian bookworm/main amd64 make amd64 4.3-4.1 [396 kB]
Get:46 http://deb.debian.org/debian bookworm/main amd64 libdpkg-perl all 1.21.22 [603 kB]
Get:47 http://deb.debian.org/debian bookworm/main amd64 patch amd64 2.7.6-7 [128 kB]
Get:48 http://deb.debian.org/debian bookworm/main amd64 dpkg-dev all 1.21.22 [1353 kB]
Get:49 http://deb.debian.org/debian bookworm/main amd64 build-essential amd64 12.9 [7704 B]
Get:50 http://deb.debian.org/debian bookworm/main amd64 libassuan0 amd64 2.5.5-5 [48.5 kB]
Get:51 http://deb.debian.org/debian bookworm/main amd64 libreadline8 amd64 8.2-1.3 [166 kB]
Get:52 http://deb.debian.org/debian bookworm/main amd64 gpgconf amd64 2.2.40-1.1 [564 kB]
Get:53 http://deb.debian.org/debian bookworm/main amd64 libksba8 amd64 1.6.3-2 [128 kB]
Get:54 http://deb.debian.org/debian bookworm/main amd64 libsasl2-modules-db amd64 2.1.28+dfsg-10 [20.3 kB]
Get:55 http://deb.debian.org/debian bookworm/main amd64 libsasl2-2 amd64 2.1.28+dfsg-10 [59.7 kB]
Get:56 http://deb.debian.org/debian bookworm/main amd64 libldap-2.5-0 amd64 2.5.13+dfsg-5 [183 kB]
Get:57 http://deb.debian.org/debian bookworm/main amd64 libnpth0 amd64 1.6-3 [19.0 kB]
Get:58 http://deb.debian.org/debian bookworm/main amd64 dirmngr amd64 2.2.40-1.1 [792 kB]
Get:59 http://deb.debian.org/debian bookworm/main amd64 libfakeroot amd64 1.31-1.2 [28.3 kB]
Get:60 http://deb.debian.org/debian bookworm/main amd64 fakeroot amd64 1.31-1.2 [66.9 kB]
Get:61 http://deb.debian.org/debian bookworm/main amd64 fonts-dejavu-core all 2.37-6 [1068 kB]
Get:62 http://deb.debian.org/debian bookworm/main amd64 fontconfig-config amd64 2.14.1-4 [315 kB]
Get:63 http://deb.debian.org/debian bookworm/main amd64 gnupg-l10n all 2.2.40-1.1 [1093 kB]
Get:64 http://deb.debian.org/debian bookworm/main amd64 gnupg-utils amd64 2.2.40-1.1 [927 kB]
Get:65 http://deb.debian.org/debian bookworm/main amd64 libsqlite3-0 amd64 3.40.1-2 [837 kB]
Get:66 http://deb.debian.org/debian bookworm/main amd64 gpg amd64 2.2.40-1.1 [949 kB]
Get:67 http://deb.debian.org/debian bookworm/main amd64 pinentry-curses amd64 1.2.1-1 [77.4 kB]
Get:68 http://deb.debian.org/debian bookworm/main amd64 gpg-agent amd64 2.2.40-1.1 [695 kB]
Get:69 http://deb.debian.org/debian bookworm/main amd64 gpg-wks-client amd64 2.2.40-1.1 [541 kB]
Get:70 http://deb.debian.org/debian bookworm/main amd64 gpg-wks-server amd64 2.2.40-1.1 [531 kB]
Get:71 http://deb.debian.org/debian bookworm/main amd64 gpgsm amd64 2.2.40-1.1 [671 kB]
Get:72 http://deb.debian.org/debian bookworm/main amd64 gnupg all 2.2.40-1.1 [846 kB]
Get:73 http://deb.debian.org/debian bookworm/main amd64 libnl-3-200 amd64 3.7.0-0.2+b1 [63.1 kB]
Get:74 http://deb.debian.org/debian bookworm/main amd64 libnl-genl-3-200 amd64 3.7.0-0.2+b1 [21.6 kB]
Get:75 http://deb.debian.org/debian bookworm/main amd64 htop amd64 3.2.2-2 [152 kB]
Get:76 http://deb.debian.org/debian bookworm/main amd64 javascript-common all 11+nmu1 [6260 B]
Get:77 http://deb.debian.org/debian bookworm/main amd64 libabsl20220623 amd64 20220623.1-1 [391 kB]
Get:78 http://deb.debian.org/debian bookworm/main amd64 libalgorithm-diff-perl all 1.201-1 [43.3 kB]
Get:79 http://deb.debian.org/debian bookworm/main amd64 libalgorithm-diff-xs-perl amd64 0.04-8+b1 [11.4 kB]
Get:80 http://deb.debian.org/debian bookworm/main amd64 libalgorithm-merge-perl all 0.08-5 [11.8 kB]
Get:81 http://deb.debian.org/debian bookworm/main amd64 libaom3 amd64 3.6.0-1 [1851 kB]
Get:82 http://deb.debian.org/debian bookworm/main amd64 libdav1d6 amd64 1.0.0-2 [495 kB]
Get:83 http://deb.debian.org/debian bookworm/main amd64 libgav1-1 amd64 0.18.0-1+b1 [332 kB]
Get:84 http://deb.debian.org/debian bookworm/main amd64 librav1e0 amd64 0.5.1-6 [763 kB]
Get:85 http://deb.debian.org/debian bookworm/main amd64 libsvtav1enc1 amd64 1.4.1+dfsg-1 [2121 kB]
Get:86 http://deb.debian.org/debian bookworm/main amd64 libjpeg62-turbo amd64 1:2.1.5-2 [166 kB]
Get:87 http://deb.debian.org/debian bookworm/main amd64 libyuv0 amd64 0.0~git20230123.b2528b0-1 [168 kB]
Get:88 http://deb.debian.org/debian bookworm/main amd64 libavif15 amd64 0.11.1-1 [93.8 kB]
Get:89 http://deb.debian.org/debian bookworm/main amd64 libbrotli1 amd64 1.0.9-2+b6 [275 kB]
Get:90 http://deb.debian.org/debian bookworm/main amd64 libpng16-16 amd64 1.6.39-2 [276 kB]
Get:91 http://deb.debian.org/debian bookworm/main amd64 libfreetype6 amd64 2.12.1+dfsg-5 [399 kB]
Get:92 http://deb.debian.org/debian bookworm/main amd64 libfontconfig1 amd64 2.14.1-4 [386 kB]
Get:93 http://deb.debian.org/debian bookworm/main amd64 libde265-0 amd64 1.0.11-1+deb12u1 [185 kB]
Get:94 http://deb.debian.org/debian bookworm/main amd64 libnuma1 amd64 2.0.16-1 [21.0 kB]
Get:95 http://deb.debian.org/debian bookworm/main amd64 libx265-199 amd64 3.5-2+b1 [1150 kB]
Get:96 http://deb.debian.org/debian bookworm/main amd64 libheif1 amd64 1.15.1-1 [215 kB]
Get:97 http://deb.debian.org/debian bookworm/main amd64 libdeflate0 amd64 1.14-1 [61.4 kB]
Get:98 http://deb.debian.org/debian bookworm/main amd64 libjbig0 amd64 2.1-6.1 [31.7 kB]
Get:99 http://deb.debian.org/debian bookworm/main amd64 liblerc4 amd64 4.0.0+ds-2 [170 kB]
Get:100 http://deb.debian.org/debian bookworm/main amd64 libwebp7 amd64 1.2.4-0.2+deb12u1 [286 kB]
Get:101 http://deb.debian.org/debian bookworm/main amd64 libtiff6 amd64 4.5.0-6+deb12u1 [316 kB]
Get:102 http://deb.debian.org/debian bookworm/main amd64 libxpm4 amd64 1:3.5.12-1.1+deb12u1 [48.6 kB]
Get:103 http://deb.debian.org/debian bookworm/main amd64 libgd3 amd64 2.3.3-9 [124 kB]
Get:104 http://deb.debian.org/debian bookworm/main amd64 libc-devtools amd64 2.36-9+deb12u3 [52.9 kB]
Get:105 http://deb.debian.org/debian bookworm/main amd64 libfile-fcntllock-perl amd64 0.22-4+b1 [34.8 kB]
Get:106 http://deb.debian.org/debian bookworm/main amd64 libgmpxx4ldbl amd64 2:6.2.1+dfsg1-1.1 [338 kB]
Get:107 http://deb.debian.org/debian bookworm/main amd64 libgmp-dev amd64 2:6.2.1+dfsg1-1.1 [641 kB]
Get:108 http://deb.debian.org/debian bookworm/main amd64 libjs-jquery all 3.6.1+dfsg+~3.5.14-1 [326 kB]
Get:109 http://deb.debian.org/debian bookworm/main amd64 libldap-common all 2.5.13+dfsg-5 [29.3 kB]
Get:110 http://deb.debian.org/debian bookworm/main amd64 libncurses6 amd64 6.4-4 [103 kB]
Get:111 http://deb.debian.org/debian bookworm/main amd64 rubygems-integration all 1.18 [6704 B]
Get:112 http://deb.debian.org/debian bookworm/main amd64 ruby3.1 amd64 3.1.2-7 [663 kB]
Get:113 http://deb.debian.org/debian bookworm/main amd64 ruby-rubygems all 3.3.15-2 [293 kB]
Get:114 http://deb.debian.org/debian bookworm/main amd64 ruby amd64 1:3.1 [5868 B]
Get:115 http://deb.debian.org/debian bookworm/main amd64 rake all 13.0.6-3 [83.9 kB]
Get:116 http://deb.debian.org/debian bookworm/main amd64 ruby-net-telnet all 0.2.0-1 [13.1 kB]
Get:117 http://deb.debian.org/debian bookworm/main amd64 ruby-webrick all 1.8.1-1 [51.4 kB]
Get:118 http://deb.debian.org/debian bookworm/main amd64 ruby-xmlrpc all 0.3.2-2 [24.4 kB]
Get:119 http://deb.debian.org/debian bookworm/main amd64 ruby-sdbm amd64 1.0.0-5+b1 [15.4 kB]
Get:120 http://deb.debian.org/debian bookworm/main amd64 libyaml-0-2 amd64 0.2.5-1 [53.6 kB]
Get:121 http://deb.debian.org/debian bookworm/main amd64 libruby3.1 amd64 3.1.2-7 [5401 kB]
Get:122 http://deb.debian.org/debian bookworm/main amd64 libruby amd64 1:3.1 [4972 B]
Get:123 http://deb.debian.org/debian bookworm/main amd64 libsasl2-modules amd64 2.1.28+dfsg-10 [66.6 kB]
Get:124 http://deb.debian.org/debian bookworm/main amd64 manpages-dev all 6.03-2 [2030 kB]
Get:125 http://deb.debian.org/debian bookworm/main amd64 nginx-common all 1.22.1-9 [112 kB]
Get:126 http://deb.debian.org/debian bookworm/main amd64 nginx amd64 1.22.1-9 [527 kB]
Get:127 http://deb.debian.org/debian bookworm/main amd64 ruby3.1-doc all 3.1.2-7 [2449 kB]
Get:128 http://deb.debian.org/debian bookworm/main amd64 ri all 1:3.1 [5016 B]
Get:129 http://deb.debian.org/debian bookworm/main amd64 ruby3.1-dev amd64 3.1.2-7 [1001 kB]
Get:130 http://deb.debian.org/debian bookworm/main amd64 ruby-dev amd64 1:3.1 [5116 B]
Get:131 http://deb.debian.org/debian bookworm/main amd64 ruby-full all 1:3.1 [4952 B]
Get:132 http://deb.debian.org/debian bookworm/main amd64 unzip amd64 6.0-28 [166 kB]
Get:133 http://deb.debian.org/debian bookworm/main amd64 zip amd64 3.0-13 [230 kB]
Fetched 108 MB in 15s (7284 kB/s)
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package fonts-lato.
(Reading database ... 13573 files and directories currently installed.)
Preparing to unpack .../000-fonts-lato_2.0-2.1_all.deb ...
Unpacking fonts-lato (2.0-2.1) ...
Selecting previously unselected package liblocale-gettext-perl.
Preparing to unpack .../001-liblocale-gettext-perl_1.07-5_amd64.deb ...
Unpacking liblocale-gettext-perl (1.07-5) ...
Selecting previously unselected package readline-common.
Preparing to unpack .../002-readline-common_8.2-1.3_all.deb ...
Unpacking readline-common (8.2-1.3) ...
Selecting previously unselected package bzip2.
Preparing to unpack .../003-bzip2_1.0.8-5+b1_amd64.deb ...
Unpacking bzip2 (1.0.8-5+b1) ...
Selecting previously unselected package openssl.
Preparing to unpack .../004-openssl_3.0.11-1~deb12u2_amd64.deb ...
Unpacking openssl (3.0.11-1~deb12u2) ...
Selecting previously unselected package ca-certificates.
Preparing to unpack .../005-ca-certificates_20230311_all.deb ...
Unpacking ca-certificates (20230311) ...
Selecting previously unselected package manpages.
Preparing to unpack .../006-manpages_6.03-2_all.deb ...
Unpacking manpages (6.03-2) ...
Selecting previously unselected package xz-utils.
Preparing to unpack .../007-xz-utils_5.4.1-0.2_amd64.deb ...
Unpacking xz-utils (5.4.1-0.2) ...
Selecting previously unselected package binutils-common:amd64.
Preparing to unpack .../008-binutils-common_2.40-2_amd64.deb ...
Unpacking binutils-common:amd64 (2.40-2) ...
Selecting previously unselected package libbinutils:amd64.
Preparing to unpack .../009-libbinutils_2.40-2_amd64.deb ...
Unpacking libbinutils:amd64 (2.40-2) ...
Selecting previously unselected package libctf-nobfd0:amd64.
Preparing to unpack .../010-libctf-nobfd0_2.40-2_amd64.deb ...
Unpacking libctf-nobfd0:amd64 (2.40-2) ...
Selecting previously unselected package libctf0:amd64.
Preparing to unpack .../011-libctf0_2.40-2_amd64.deb ...
Unpacking libctf0:amd64 (2.40-2) ...
Selecting previously unselected package libgprofng0:amd64.
Preparing to unpack .../012-libgprofng0_2.40-2_amd64.deb ...
Unpacking libgprofng0:amd64 (2.40-2) ...
Selecting previously unselected package libjansson4:amd64.
Preparing to unpack .../013-libjansson4_2.14-2_amd64.deb ...
Unpacking libjansson4:amd64 (2.14-2) ...
Selecting previously unselected package binutils-x86-64-linux-gnu.
Preparing to unpack .../014-binutils-x86-64-linux-gnu_2.40-2_amd64.deb ...
Unpacking binutils-x86-64-linux-gnu (2.40-2) ...
Selecting previously unselected package binutils.
Preparing to unpack .../015-binutils_2.40-2_amd64.deb ...
Unpacking binutils (2.40-2) ...
Selecting previously unselected package libc-dev-bin.
Preparing to unpack .../016-libc-dev-bin_2.36-9+deb12u3_amd64.deb ...
Unpacking libc-dev-bin (2.36-9+deb12u3) ...
Selecting previously unselected package linux-libc-dev:amd64.
Preparing to unpack .../017-linux-libc-dev_6.1.66-1_amd64.deb ...
Unpacking linux-libc-dev:amd64 (6.1.66-1) ...
Selecting previously unselected package libcrypt-dev:amd64.
Preparing to unpack .../018-libcrypt-dev_1%3a4.4.33-2_amd64.deb ...
Unpacking libcrypt-dev:amd64 (1:4.4.33-2) ...
Selecting previously unselected package libnsl2:amd64.
Preparing to unpack .../019-libnsl2_1.3.0-2_amd64.deb ...
Unpacking libnsl2:amd64 (1.3.0-2) ...
Selecting previously unselected package libtirpc-dev:amd64.
Preparing to unpack .../020-libtirpc-dev_1.3.3+ds-1_amd64.deb ...
Unpacking libtirpc-dev:amd64 (1.3.3+ds-1) ...
Selecting previously unselected package libnsl-dev:amd64.
Preparing to unpack .../021-libnsl-dev_1.3.0-2_amd64.deb ...
Unpacking libnsl-dev:amd64 (1.3.0-2) ...
Selecting previously unselected package rpcsvc-proto.
Preparing to unpack .../022-rpcsvc-proto_1.4.3-1_amd64.deb ...
Unpacking rpcsvc-proto (1.4.3-1) ...
Selecting previously unselected package libc6-dev:amd64.
Preparing to unpack .../023-libc6-dev_2.36-9+deb12u3_amd64.deb ...
Unpacking libc6-dev:amd64 (2.36-9+deb12u3) ...
Selecting previously unselected package libisl23:amd64.
Preparing to unpack .../024-libisl23_0.25-1_amd64.deb ...
Unpacking libisl23:amd64 (0.25-1) ...
Selecting previously unselected package libmpfr6:amd64.
Preparing to unpack .../025-libmpfr6_4.2.0-1_amd64.deb ...
Unpacking libmpfr6:amd64 (4.2.0-1) ...
Selecting previously unselected package libmpc3:amd64.
Preparing to unpack .../026-libmpc3_1.3.1-1_amd64.deb ...
Unpacking libmpc3:amd64 (1.3.1-1) ...
Selecting previously unselected package cpp-12.
Preparing to unpack .../027-cpp-12_12.2.0-14_amd64.deb ...
Unpacking cpp-12 (12.2.0-14) ...
Selecting previously unselected package cpp.
Preparing to unpack .../028-cpp_4%3a12.2.0-3_amd64.deb ...
Unpacking cpp (4:12.2.0-3) ...
Selecting previously unselected package libcc1-0:amd64.
Preparing to unpack .../029-libcc1-0_12.2.0-14_amd64.deb ...
Unpacking libcc1-0:amd64 (12.2.0-14) ...
Selecting previously unselected package libgomp1:amd64.
Preparing to unpack .../030-libgomp1_12.2.0-14_amd64.deb ...
Unpacking libgomp1:amd64 (12.2.0-14) ...
Selecting previously unselected package libitm1:amd64.
Preparing to unpack .../031-libitm1_12.2.0-14_amd64.deb ...
Unpacking libitm1:amd64 (12.2.0-14) ...
Selecting previously unselected package libatomic1:amd64.
Preparing to unpack .../032-libatomic1_12.2.0-14_amd64.deb ...
Unpacking libatomic1:amd64 (12.2.0-14) ...
Selecting previously unselected package libasan8:amd64.
Preparing to unpack .../033-libasan8_12.2.0-14_amd64.deb ...
Unpacking libasan8:amd64 (12.2.0-14) ...
Selecting previously unselected package liblsan0:amd64.
Preparing to unpack .../034-liblsan0_12.2.0-14_amd64.deb ...
Unpacking liblsan0:amd64 (12.2.0-14) ...
Selecting previously unselected package libtsan2:amd64.
Preparing to unpack .../035-libtsan2_12.2.0-14_amd64.deb ...
Unpacking libtsan2:amd64 (12.2.0-14) ...
Selecting previously unselected package libubsan1:amd64.
Preparing to unpack .../036-libubsan1_12.2.0-14_amd64.deb ...
Unpacking libubsan1:amd64 (12.2.0-14) ...
Selecting previously unselected package libquadmath0:amd64.
Preparing to unpack .../037-libquadmath0_12.2.0-14_amd64.deb ...
Unpacking libquadmath0:amd64 (12.2.0-14) ...
Selecting previously unselected package libgcc-12-dev:amd64.
Preparing to unpack .../038-libgcc-12-dev_12.2.0-14_amd64.deb ...
Unpacking libgcc-12-dev:amd64 (12.2.0-14) ...
Selecting previously unselected package gcc-12.
Preparing to unpack .../039-gcc-12_12.2.0-14_amd64.deb ...
Unpacking gcc-12 (12.2.0-14) ...
Selecting previously unselected package gcc.
Preparing to unpack .../040-gcc_4%3a12.2.0-3_amd64.deb ...
Unpacking gcc (4:12.2.0-3) ...
Selecting previously unselected package libstdc++-12-dev:amd64.
Preparing to unpack .../041-libstdc++-12-dev_12.2.0-14_amd64.deb ...
Unpacking libstdc++-12-dev:amd64 (12.2.0-14) ...
Selecting previously unselected package g++-12.
Preparing to unpack .../042-g++-12_12.2.0-14_amd64.deb ...
Unpacking g++-12 (12.2.0-14) ...
Selecting previously unselected package g++.
Preparing to unpack .../043-g++_4%3a12.2.0-3_amd64.deb ...
Unpacking g++ (4:12.2.0-3) ...
Selecting previously unselected package make.
Preparing to unpack .../044-make_4.3-4.1_amd64.deb ...
Unpacking make (4.3-4.1) ...
Selecting previously unselected package libdpkg-perl.
Preparing to unpack .../045-libdpkg-perl_1.21.22_all.deb ...
Unpacking libdpkg-perl (1.21.22) ...
Selecting previously unselected package patch.
Preparing to unpack .../046-patch_2.7.6-7_amd64.deb ...
Unpacking patch (2.7.6-7) ...
Selecting previously unselected package dpkg-dev.
Preparing to unpack .../047-dpkg-dev_1.21.22_all.deb ...
Unpacking dpkg-dev (1.21.22) ...
Selecting previously unselected package build-essential.
Preparing to unpack .../048-build-essential_12.9_amd64.deb ...
Unpacking build-essential (12.9) ...
Selecting previously unselected package libassuan0:amd64.
Preparing to unpack .../049-libassuan0_2.5.5-5_amd64.deb ...
Unpacking libassuan0:amd64 (2.5.5-5) ...
Selecting previously unselected package libreadline8:amd64.
Preparing to unpack .../050-libreadline8_8.2-1.3_amd64.deb ...
Unpacking libreadline8:amd64 (8.2-1.3) ...
Selecting previously unselected package gpgconf.
Preparing to unpack .../051-gpgconf_2.2.40-1.1_amd64.deb ...
Unpacking gpgconf (2.2.40-1.1) ...
Selecting previously unselected package libksba8:amd64.
Preparing to unpack .../052-libksba8_1.6.3-2_amd64.deb ...
Unpacking libksba8:amd64 (1.6.3-2) ...
Selecting previously unselected package libsasl2-modules-db:amd64.
Preparing to unpack .../053-libsasl2-modules-db_2.1.28+dfsg-10_amd64.deb ...
Unpacking libsasl2-modules-db:amd64 (2.1.28+dfsg-10) ...
Selecting previously unselected package libsasl2-2:amd64.
Preparing to unpack .../054-libsasl2-2_2.1.28+dfsg-10_amd64.deb ...
Unpacking libsasl2-2:amd64 (2.1.28+dfsg-10) ...
Selecting previously unselected package libldap-2.5-0:amd64.
Preparing to unpack .../055-libldap-2.5-0_2.5.13+dfsg-5_amd64.deb ...
Unpacking libldap-2.5-0:amd64 (2.5.13+dfsg-5) ...
Selecting previously unselected package libnpth0:amd64.
Preparing to unpack .../056-libnpth0_1.6-3_amd64.deb ...
Unpacking libnpth0:amd64 (1.6-3) ...
Selecting previously unselected package dirmngr.
Preparing to unpack .../057-dirmngr_2.2.40-1.1_amd64.deb ...
Unpacking dirmngr (2.2.40-1.1) ...
Selecting previously unselected package libfakeroot:amd64.
Preparing to unpack .../058-libfakeroot_1.31-1.2_amd64.deb ...
Unpacking libfakeroot:amd64 (1.31-1.2) ...
Selecting previously unselected package fakeroot.
Preparing to unpack .../059-fakeroot_1.31-1.2_amd64.deb ...
Unpacking fakeroot (1.31-1.2) ...
Selecting previously unselected package fonts-dejavu-core.
Preparing to unpack .../060-fonts-dejavu-core_2.37-6_all.deb ...
Unpacking fonts-dejavu-core (2.37-6) ...
Selecting previously unselected package fontconfig-config.
Preparing to unpack .../061-fontconfig-config_2.14.1-4_amd64.deb ...
Unpacking fontconfig-config (2.14.1-4) ...
Selecting previously unselected package gnupg-l10n.
Preparing to unpack .../062-gnupg-l10n_2.2.40-1.1_all.deb ...
Unpacking gnupg-l10n (2.2.40-1.1) ...
Selecting previously unselected package gnupg-utils.
Preparing to unpack .../063-gnupg-utils_2.2.40-1.1_amd64.deb ...
Unpacking gnupg-utils (2.2.40-1.1) ...
Selecting previously unselected package libsqlite3-0:amd64.
Preparing to unpack .../064-libsqlite3-0_3.40.1-2_amd64.deb ...
Unpacking libsqlite3-0:amd64 (3.40.1-2) ...
Selecting previously unselected package gpg.
Preparing to unpack .../065-gpg_2.2.40-1.1_amd64.deb ...
Unpacking gpg (2.2.40-1.1) ...
Selecting previously unselected package pinentry-curses.
Preparing to unpack .../066-pinentry-curses_1.2.1-1_amd64.deb ...
Unpacking pinentry-curses (1.2.1-1) ...
Selecting previously unselected package gpg-agent.
Preparing to unpack .../067-gpg-agent_2.2.40-1.1_amd64.deb ...
Unpacking gpg-agent (2.2.40-1.1) ...
Selecting previously unselected package gpg-wks-client.
Preparing to unpack .../068-gpg-wks-client_2.2.40-1.1_amd64.deb ...
Unpacking gpg-wks-client (2.2.40-1.1) ...
Selecting previously unselected package gpg-wks-server.
Preparing to unpack .../069-gpg-wks-server_2.2.40-1.1_amd64.deb ...
Unpacking gpg-wks-server (2.2.40-1.1) ...
Selecting previously unselected package gpgsm.
Preparing to unpack .../070-gpgsm_2.2.40-1.1_amd64.deb ...
Unpacking gpgsm (2.2.40-1.1) ...
Selecting previously unselected package gnupg.
Preparing to unpack .../071-gnupg_2.2.40-1.1_all.deb ...
Unpacking gnupg (2.2.40-1.1) ...
Selecting previously unselected package libnl-3-200:amd64.
Preparing to unpack .../072-libnl-3-200_3.7.0-0.2+b1_amd64.deb ...
Unpacking libnl-3-200:amd64 (3.7.0-0.2+b1) ...
Selecting previously unselected package libnl-genl-3-200:amd64.
Preparing to unpack .../073-libnl-genl-3-200_3.7.0-0.2+b1_amd64.deb ...
Unpacking libnl-genl-3-200:amd64 (3.7.0-0.2+b1) ...
Selecting previously unselected package htop.
Preparing to unpack .../074-htop_3.2.2-2_amd64.deb ...
Unpacking htop (3.2.2-2) ...
Selecting previously unselected package javascript-common.
Preparing to unpack .../075-javascript-common_11+nmu1_all.deb ...
Unpacking javascript-common (11+nmu1) ...
Selecting previously unselected package libabsl20220623:amd64.
Preparing to unpack .../076-libabsl20220623_20220623.1-1_amd64.deb ...
Unpacking libabsl20220623:amd64 (20220623.1-1) ...
Selecting previously unselected package libalgorithm-diff-perl.
Preparing to unpack .../077-libalgorithm-diff-perl_1.201-1_all.deb ...
Unpacking libalgorithm-diff-perl (1.201-1) ...
Selecting previously unselected package libalgorithm-diff-xs-perl:amd64.
Preparing to unpack .../078-libalgorithm-diff-xs-perl_0.04-8+b1_amd64.deb ...
Unpacking libalgorithm-diff-xs-perl:amd64 (0.04-8+b1) ...
Selecting previously unselected package libalgorithm-merge-perl.
Preparing to unpack .../079-libalgorithm-merge-perl_0.08-5_all.deb ...
Unpacking libalgorithm-merge-perl (0.08-5) ...
Selecting previously unselected package libaom3:amd64.
Preparing to unpack .../080-libaom3_3.6.0-1_amd64.deb ...
Unpacking libaom3:amd64 (3.6.0-1) ...
Selecting previously unselected package libdav1d6:amd64.
Preparing to unpack .../081-libdav1d6_1.0.0-2_amd64.deb ...
Unpacking libdav1d6:amd64 (1.0.0-2) ...
Selecting previously unselected package libgav1-1:amd64.
Preparing to unpack .../082-libgav1-1_0.18.0-1+b1_amd64.deb ...
Unpacking libgav1-1:amd64 (0.18.0-1+b1) ...
Selecting previously unselected package librav1e0:amd64.
Preparing to unpack .../083-librav1e0_0.5.1-6_amd64.deb ...
Unpacking librav1e0:amd64 (0.5.1-6) ...
Selecting previously unselected package libsvtav1enc1:amd64.
Preparing to unpack .../084-libsvtav1enc1_1.4.1+dfsg-1_amd64.deb ...
Unpacking libsvtav1enc1:amd64 (1.4.1+dfsg-1) ...
Selecting previously unselected package libjpeg62-turbo:amd64.
Preparing to unpack .../085-libjpeg62-turbo_1%3a2.1.5-2_amd64.deb ...
Unpacking libjpeg62-turbo:amd64 (1:2.1.5-2) ...
Selecting previously unselected package libyuv0:amd64.
Preparing to unpack .../086-libyuv0_0.0~git20230123.b2528b0-1_amd64.deb ...
Unpacking libyuv0:amd64 (0.0~git20230123.b2528b0-1) ...
Selecting previously unselected package libavif15:amd64.
Preparing to unpack .../087-libavif15_0.11.1-1_amd64.deb ...
Unpacking libavif15:amd64 (0.11.1-1) ...
Selecting previously unselected package libbrotli1:amd64.
Preparing to unpack .../088-libbrotli1_1.0.9-2+b6_amd64.deb ...
Unpacking libbrotli1:amd64 (1.0.9-2+b6) ...
Selecting previously unselected package libpng16-16:amd64.
Preparing to unpack .../089-libpng16-16_1.6.39-2_amd64.deb ...
Unpacking libpng16-16:amd64 (1.6.39-2) ...
Selecting previously unselected package libfreetype6:amd64.
Preparing to unpack .../090-libfreetype6_2.12.1+dfsg-5_amd64.deb ...
Unpacking libfreetype6:amd64 (2.12.1+dfsg-5) ...
Selecting previously unselected package libfontconfig1:amd64.
Preparing to unpack .../091-libfontconfig1_2.14.1-4_amd64.deb ...
Unpacking libfontconfig1:amd64 (2.14.1-4) ...
Selecting previously unselected package libde265-0:amd64.
Preparing to unpack .../092-libde265-0_1.0.11-1+deb12u1_amd64.deb ...
Unpacking libde265-0:amd64 (1.0.11-1+deb12u1) ...
Selecting previously unselected package libnuma1:amd64.
Preparing to unpack .../093-libnuma1_2.0.16-1_amd64.deb ...
Unpacking libnuma1:amd64 (2.0.16-1) ...
Selecting previously unselected package libx265-199:amd64.
Preparing to unpack .../094-libx265-199_3.5-2+b1_amd64.deb ...
Unpacking libx265-199:amd64 (3.5-2+b1) ...
Selecting previously unselected package libheif1:amd64.
Preparing to unpack .../095-libheif1_1.15.1-1_amd64.deb ...
Unpacking libheif1:amd64 (1.15.1-1) ...
Selecting previously unselected package libdeflate0:amd64.
Preparing to unpack .../096-libdeflate0_1.14-1_amd64.deb ...
Unpacking libdeflate0:amd64 (1.14-1) ...
Selecting previously unselected package libjbig0:amd64.
Preparing to unpack .../097-libjbig0_2.1-6.1_amd64.deb ...
Unpacking libjbig0:amd64 (2.1-6.1) ...
Selecting previously unselected package liblerc4:amd64.
Preparing to unpack .../098-liblerc4_4.0.0+ds-2_amd64.deb ...
Unpacking liblerc4:amd64 (4.0.0+ds-2) ...
Selecting previously unselected package libwebp7:amd64.
Preparing to unpack .../099-libwebp7_1.2.4-0.2+deb12u1_amd64.deb ...
Unpacking libwebp7:amd64 (1.2.4-0.2+deb12u1) ...
Selecting previously unselected package libtiff6:amd64.
Preparing to unpack .../100-libtiff6_4.5.0-6+deb12u1_amd64.deb ...
Unpacking libtiff6:amd64 (4.5.0-6+deb12u1) ...
Selecting previously unselected package libxpm4:amd64.
Preparing to unpack .../101-libxpm4_1%3a3.5.12-1.1+deb12u1_amd64.deb ...
Unpacking libxpm4:amd64 (1:3.5.12-1.1+deb12u1) ...
Selecting previously unselected package libgd3:amd64.
Preparing to unpack .../102-libgd3_2.3.3-9_amd64.deb ...
Unpacking libgd3:amd64 (2.3.3-9) ...
Selecting previously unselected package libc-devtools.
Preparing to unpack .../103-libc-devtools_2.36-9+deb12u3_amd64.deb ...
Unpacking libc-devtools (2.36-9+deb12u3) ...
Selecting previously unselected package libfile-fcntllock-perl.
Preparing to unpack .../104-libfile-fcntllock-perl_0.22-4+b1_amd64.deb ...
Unpacking libfile-fcntllock-perl (0.22-4+b1) ...
Selecting previously unselected package libgmpxx4ldbl:amd64.
Preparing to unpack .../105-libgmpxx4ldbl_2%3a6.2.1+dfsg1-1.1_amd64.deb ...
Unpacking libgmpxx4ldbl:amd64 (2:6.2.1+dfsg1-1.1) ...
Selecting previously unselected package libgmp-dev:amd64.
Preparing to unpack .../106-libgmp-dev_2%3a6.2.1+dfsg1-1.1_amd64.deb ...
Unpacking libgmp-dev:amd64 (2:6.2.1+dfsg1-1.1) ...
Selecting previously unselected package libjs-jquery.
Preparing to unpack .../107-libjs-jquery_3.6.1+dfsg+~3.5.14-1_all.deb ...
Unpacking libjs-jquery (3.6.1+dfsg+~3.5.14-1) ...
Selecting previously unselected package libldap-common.
Preparing to unpack .../108-libldap-common_2.5.13+dfsg-5_all.deb ...
Unpacking libldap-common (2.5.13+dfsg-5) ...
Selecting previously unselected package libncurses6:amd64.
Preparing to unpack .../109-libncurses6_6.4-4_amd64.deb ...
Unpacking libncurses6:amd64 (6.4-4) ...
Selecting previously unselected package rubygems-integration.
Preparing to unpack .../110-rubygems-integration_1.18_all.deb ...
Unpacking rubygems-integration (1.18) ...
Selecting previously unselected package ruby3.1.
Preparing to unpack .../111-ruby3.1_3.1.2-7_amd64.deb ...
Unpacking ruby3.1 (3.1.2-7) ...
Selecting previously unselected package ruby-rubygems.
Preparing to unpack .../112-ruby-rubygems_3.3.15-2_all.deb ...
Unpacking ruby-rubygems (3.3.15-2) ...
Selecting previously unselected package ruby.
Preparing to unpack .../113-ruby_1%3a3.1_amd64.deb ...
Unpacking ruby (1:3.1) ...
Selecting previously unselected package rake.
Preparing to unpack .../114-rake_13.0.6-3_all.deb ...
Unpacking rake (13.0.6-3) ...
Selecting previously unselected package ruby-net-telnet.
Preparing to unpack .../115-ruby-net-telnet_0.2.0-1_all.deb ...
Unpacking ruby-net-telnet (0.2.0-1) ...
Selecting previously unselected package ruby-webrick.
Preparing to unpack .../116-ruby-webrick_1.8.1-1_all.deb ...
Unpacking ruby-webrick (1.8.1-1) ...
Selecting previously unselected package ruby-xmlrpc.
Preparing to unpack .../117-ruby-xmlrpc_0.3.2-2_all.deb ...
Unpacking ruby-xmlrpc (0.3.2-2) ...
Selecting previously unselected package ruby-sdbm:amd64.
Preparing to unpack .../118-ruby-sdbm_1.0.0-5+b1_amd64.deb ...
Unpacking ruby-sdbm:amd64 (1.0.0-5+b1) ...
Selecting previously unselected package libyaml-0-2:amd64.
Preparing to unpack .../119-libyaml-0-2_0.2.5-1_amd64.deb ...
Unpacking libyaml-0-2:amd64 (0.2.5-1) ...
Selecting previously unselected package libruby3.1:amd64.
Preparing to unpack .../120-libruby3.1_3.1.2-7_amd64.deb ...
Unpacking libruby3.1:amd64 (3.1.2-7) ...
Selecting previously unselected package libruby:amd64.
Preparing to unpack .../121-libruby_1%3a3.1_amd64.deb ...
Unpacking libruby:amd64 (1:3.1) ...
Selecting previously unselected package libsasl2-modules:amd64.
Preparing to unpack .../122-libsasl2-modules_2.1.28+dfsg-10_amd64.deb ...
Unpacking libsasl2-modules:amd64 (2.1.28+dfsg-10) ...
Selecting previously unselected package manpages-dev.
Preparing to unpack .../123-manpages-dev_6.03-2_all.deb ...
Unpacking manpages-dev (6.03-2) ...
Selecting previously unselected package nginx-common.
Preparing to unpack .../124-nginx-common_1.22.1-9_all.deb ...
Unpacking nginx-common (1.22.1-9) ...
Selecting previously unselected package nginx.
Preparing to unpack .../125-nginx_1.22.1-9_amd64.deb ...
Unpacking nginx (1.22.1-9) ...
Selecting previously unselected package ruby3.1-doc.
Preparing to unpack .../126-ruby3.1-doc_3.1.2-7_all.deb ...
Unpacking ruby3.1-doc (3.1.2-7) ...
Selecting previously unselected package ri.
Preparing to unpack .../127-ri_1%3a3.1_all.deb ...
Unpacking ri (1:3.1) ...
Selecting previously unselected package ruby3.1-dev:amd64.
Preparing to unpack .../128-ruby3.1-dev_3.1.2-7_amd64.deb ...
Unpacking ruby3.1-dev:amd64 (3.1.2-7) ...
Selecting previously unselected package ruby-dev:amd64.
Preparing to unpack .../129-ruby-dev_1%3a3.1_amd64.deb ...
Unpacking ruby-dev:amd64 (1:3.1) ...
Selecting previously unselected package ruby-full.
Preparing to unpack .../130-ruby-full_1%3a3.1_all.deb ...
Unpacking ruby-full (1:3.1) ...
Selecting previously unselected package unzip.
Preparing to unpack .../131-unzip_6.0-28_amd64.deb ...
Unpacking unzip (6.0-28) ...
Selecting previously unselected package zip.
Preparing to unpack .../132-zip_3.0-13_amd64.deb ...
Unpacking zip (3.0-13) ...
Setting up libksba8:amd64 (1.6.3-2) ...
Setting up javascript-common (11+nmu1) ...
Setting up libaom3:amd64 (3.6.0-1) ...
Setting up libabsl20220623:amd64 (20220623.1-1) ...
Setting up fonts-lato (2.0-2.1) ...
Setting up liblerc4:amd64 (4.0.0+ds-2) ...
Setting up libxpm4:amd64 (1:3.5.12-1.1+deb12u1) ...
Setting up libfile-fcntllock-perl (0.22-4+b1) ...
Setting up libalgorithm-diff-perl (1.201-1) ...
Setting up libyaml-0-2:amd64 (0.2.5-1) ...
Setting up manpages (6.03-2) ...
Setting up unzip (6.0-28) ...
Setting up libbrotli1:amd64 (1.0.9-2+b6) ...
Setting up libsqlite3-0:amd64 (3.40.1-2) ...
Setting up libsasl2-modules:amd64 (2.1.28+dfsg-10) ...
Setting up binutils-common:amd64 (2.40-2) ...
Setting up libdeflate0:amd64 (1.14-1) ...
Setting up linux-libc-dev:amd64 (6.1.66-1) ...
Setting up libctf-nobfd0:amd64 (2.40-2) ...
Setting up nginx-common (1.22.1-9) ...
Created symlink /etc/systemd/system/multi-user.target.wants/nginx.service → /lib/systemd/system/nginx.service.
Setting up libnpth0:amd64 (1.6-3) ...
Setting up libsvtav1enc1:amd64 (1.4.1+dfsg-1) ...
Setting up libassuan0:amd64 (2.5.5-5) ...
Setting up libgomp1:amd64 (12.2.0-14) ...
Setting up bzip2 (1.0.8-5+b1) ...
Setting up libldap-common (2.5.13+dfsg-5) ...
Setting up libjbig0:amd64 (2.1-6.1) ...
Setting up librav1e0:amd64 (0.5.1-6) ...
Setting up libfakeroot:amd64 (1.31-1.2) ...
Setting up ruby3.1-doc (3.1.2-7) ...
Setting up libjansson4:amd64 (2.14-2) ...
Setting up libsasl2-modules-db:amd64 (2.1.28+dfsg-10) ...
Setting up fakeroot (1.31-1.2) ...
update-alternatives: using /usr/bin/fakeroot-sysv to provide /usr/bin/fakeroot (fakeroot) in auto mode
Setting up zip (3.0-13) ...
Setting up libtirpc-dev:amd64 (1.3.3+ds-1) ...
Setting up libgmpxx4ldbl:amd64 (2:6.2.1+dfsg1-1.1) ...
Setting up rpcsvc-proto (1.4.3-1) ...
Setting up libjpeg62-turbo:amd64 (1:2.1.5-2) ...
Setting up make (4.3-4.1) ...
Setting up libmpfr6:amd64 (4.2.0-1) ...
Setting up gnupg-l10n (2.2.40-1.1) ...
Setting up libncurses6:amd64 (6.4-4) ...
Setting up ruby-net-telnet (0.2.0-1) ...
Setting up xz-utils (5.4.1-0.2) ...
update-alternatives: using /usr/bin/xz to provide /usr/bin/lzma (lzma) in auto mode
Setting up libquadmath0:amd64 (12.2.0-14) ...
Setting up libpng16-16:amd64 (1.6.39-2) ...
Setting up libmpc3:amd64 (1.3.1-1) ...
Setting up libatomic1:amd64 (12.2.0-14) ...
Setting up patch (2.7.6-7) ...
Setting up fonts-dejavu-core (2.37-6) ...
Setting up libgav1-1:amd64 (0.18.0-1+b1) ...
Setting up libdav1d6:amd64 (1.0.0-2) ...
Setting up libdpkg-perl (1.21.22) ...
Setting up libsasl2-2:amd64 (2.1.28+dfsg-10) ...
Setting up libwebp7:amd64 (1.2.4-0.2+deb12u1) ...
Setting up ruby-webrick (1.8.1-1) ...
Setting up libubsan1:amd64 (12.2.0-14) ...
Setting up libnuma1:amd64 (2.0.16-1) ...
Setting up libcrypt-dev:amd64 (1:4.4.33-2) ...
Setting up libtiff6:amd64 (4.5.0-6+deb12u1) ...
Setting up libasan8:amd64 (12.2.0-14) ...
Setting up libnsl2:amd64 (1.3.0-2) ...
Setting up libnl-3-200:amd64 (3.7.0-0.2+b1) ...
Setting up libtsan2:amd64 (12.2.0-14) ...
Setting up libjs-jquery (3.6.1+dfsg+~3.5.14-1) ...
Setting up libbinutils:amd64 (2.40-2) ...
Setting up libisl23:amd64 (0.25-1) ...
Setting up libde265-0:amd64 (1.0.11-1+deb12u1) ...
Setting up libc-dev-bin (2.36-9+deb12u3) ...
Setting up openssl (3.0.11-1~deb12u2) ...
Setting up libyuv0:amd64 (0.0~git20230123.b2528b0-1) ...
Setting up libalgorithm-diff-xs-perl:amd64 (0.04-8+b1) ...
Setting up readline-common (8.2-1.3) ...
Setting up ruby-xmlrpc (0.3.2-2) ...
Setting up libcc1-0:amd64 (12.2.0-14) ...
Setting up liblocale-gettext-perl (1.07-5) ...
Setting up liblsan0:amd64 (12.2.0-14) ...
Setting up libitm1:amd64 (12.2.0-14) ...
Setting up libalgorithm-merge-perl (0.08-5) ...
Setting up libctf0:amd64 (2.40-2) ...
Setting up pinentry-curses (1.2.1-1) ...
Setting up manpages-dev (6.03-2) ...
Setting up cpp-12 (12.2.0-14) ...
Setting up libgmp-dev:amd64 (2:6.2.1+dfsg1-1.1) ...
Setting up libavif15:amd64 (0.11.1-1) ...
Setting up nginx (1.22.1-9) ...
Upgrading binary: nginx.
Setting up fontconfig-config (2.14.1-4) ...
Setting up libreadline8:amd64 (8.2-1.3) ...
Setting up libldap-2.5-0:amd64 (2.5.13+dfsg-5) ...
Setting up ca-certificates (20230311) ...
Updating certificates in /etc/ssl/certs...
140 added, 0 removed; done.
Setting up libgprofng0:amd64 (2.40-2) ...
Setting up libfreetype6:amd64 (2.12.1+dfsg-5) ...
Setting up libgcc-12-dev:amd64 (12.2.0-14) ...
Setting up libx265-199:amd64 (3.5-2+b1) ...
Setting up libnsl-dev:amd64 (1.3.0-2) ...
Setting up cpp (4:12.2.0-3) ...
Setting up libnl-genl-3-200:amd64 (3.7.0-0.2+b1) ...
Setting up gpgconf (2.2.40-1.1) ...
Setting up libc6-dev:amd64 (2.36-9+deb12u3) ...
Setting up libfontconfig1:amd64 (2.14.1-4) ...
Setting up gpg (2.2.40-1.1) ...
Setting up gnupg-utils (2.2.40-1.1) ...
Setting up binutils-x86-64-linux-gnu (2.40-2) ...
Setting up gpg-agent (2.2.40-1.1) ...
Created symlink /etc/systemd/user/sockets.target.wants/gpg-agent-browser.socket → /usr/lib/systemd/user/gpg-agent-browser.socket.
Created symlink /etc/systemd/user/sockets.target.wants/gpg-agent-extra.socket → /usr/lib/systemd/user/gpg-agent-extra.socket.
Created symlink /etc/systemd/user/sockets.target.wants/gpg-agent-ssh.socket → /usr/lib/systemd/user/gpg-agent-ssh.socket.
Created symlink /etc/systemd/user/sockets.target.wants/gpg-agent.socket → /usr/lib/systemd/user/gpg-agent.socket.
Setting up libstdc++-12-dev:amd64 (12.2.0-14) ...
Setting up htop (3.2.2-2) ...
Setting up gpgsm (2.2.40-1.1) ...
Setting up libheif1:amd64 (1.15.1-1) ...
Setting up rubygems-integration (1.18) ...
Setting up binutils (2.40-2) ...
Setting up dpkg-dev (1.21.22) ...
Setting up dirmngr (2.2.40-1.1) ...
Created symlink /etc/systemd/user/sockets.target.wants/dirmngr.socket → /usr/lib/systemd/user/dirmngr.socket.
Setting up gcc-12 (12.2.0-14) ...
Setting up libgd3:amd64 (2.3.3-9) ...
Setting up gpg-wks-server (2.2.40-1.1) ...
Setting up libc-devtools (2.36-9+deb12u3) ...
Setting up g++-12 (12.2.0-14) ...
Setting up gpg-wks-client (2.2.40-1.1) ...
Setting up gcc (4:12.2.0-3) ...
Setting up g++ (4:12.2.0-3) ...
update-alternatives: using /usr/bin/g++ to provide /usr/bin/c++ (c++) in auto mode
Setting up gnupg (2.2.40-1.1) ...
Setting up build-essential (12.9) ...
Setting up rake (13.0.6-3) ...
Setting up libruby3.1:amd64 (3.1.2-7) ...
Setting up libruby:amd64 (1:3.1) ...
Setting up ruby3.1 (3.1.2-7) ...
Setting up ruby3.1-dev:amd64 (3.1.2-7) ...
Setting up ruby-sdbm:amd64 (1.0.0-5+b1) ...
Setting up ruby-dev:amd64 (1:3.1) ...
Setting up ri (1:3.1) ...
Setting up ruby (1:3.1) ...
Setting up ruby-full (1:3.1) ...
Setting up ruby-rubygems (3.3.15-2) ...
Processing triggers for libc-bin (2.36-9+deb12u3) ...
Processing triggers for ca-certificates (20230311) ...
Updating certificates in /etc/ssl/certs...
0 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
etu@vm0:~$ config=$(cat << EOF
server {
listen 80;
listen [::]:80;
location / {
proxy_pass http://127.0.0.1:4000;
}
}
EOF
)
etu@vm0:~$ lxc exec jekyll -- bash -c "echo \"${config}\" | tee /etc/nginx/sites-enabled/reverse.conf"
server {
listen 80;
listen [::]:80;
location / {
proxy_pass http://127.0.0.1:4000;
}
}
etu@vm0:~$ lxc exec jekyll -- rm /etc/nginx/sites-enabled/default
etu@vm0:~$ lxc exec jekyll -- systemctl restart nginx
etu@vm0:~$ lxc exec jekyll -- ss -tan
State    Recv-Q   Send-Q     Local Address:Port     Peer Address:Port   Process
LISTEN   0        4096             0.0.0.0:5355          0.0.0.0:*
LISTEN   0        4096       127.0.0.53%lo:53            0.0.0.0:*
LISTEN   0        511              0.0.0.0:80            0.0.0.0:*
LISTEN   0        4096          127.0.0.54:53            0.0.0.0:*
LISTEN   0        4096                [::]:5355             [::]:*
LISTEN   0        511                 [::]:80               [::]:*
etu@vm0:~$ lxc exec jekyll -- gem install jekyll bundler
Fetching unicode-display_width-2.5.0.gem
Fetching mercenary-0.4.0.gem
Fetching terminal-table-3.0.2.gem
Fetching safe_yaml-1.0.5.gem
Fetching rouge-4.2.0.gem
Fetching forwardable-extended-2.6.0.gem
Fetching pathutil-0.16.2.gem
Fetching liquid-4.0.4.gem
Fetching kramdown-2.4.0.gem
Fetching kramdown-parser-gfm-1.1.0.gem
Fetching ffi-1.16.3.gem
Fetching rb-inotify-0.10.1.gem
Fetching rb-fsevent-0.11.2.gem
Fetching listen-3.8.0.gem
Fetching jekyll-watch-2.2.1.gem
Fetching google-protobuf-3.25.1-x86_64-linux.gem
Fetching sass-embedded-1.69.5.gem
Fetching jekyll-sass-converter-3.0.0.gem
Fetching concurrent-ruby-1.2.2.gem
Fetching i18n-1.14.1.gem
Fetching http_parser.rb-0.8.0.gem
Fetching eventmachine-1.2.7.gem
Fetching em-websocket-0.5.3.gem
Fetching colorator-1.1.0.gem
Fetching jekyll-4.3.2.gem
Fetching public_suffix-5.0.4.gem
Fetching addressable-2.8.6.gem
Successfully installed unicode-display_width-2.5.0
Successfully installed terminal-table-3.0.2
Successfully installed safe_yaml-1.0.5
Successfully installed rouge-4.2.0
Successfully installed forwardable-extended-2.6.0
Successfully installed pathutil-0.16.2
Successfully installed mercenary-0.4.0
Successfully installed liquid-4.0.4
Successfully installed kramdown-2.4.0
Successfully installed kramdown-parser-gfm-1.1.0
Building native extensions. This could take a while...
Successfully installed ffi-1.16.3
Successfully installed rb-inotify-0.10.1
Successfully installed rb-fsevent-0.11.2
Successfully installed listen-3.8.0
Successfully installed jekyll-watch-2.2.1
Successfully installed google-protobuf-3.25.1-x86_64-linux
Building native extensions. This could take a while...
Successfully installed sass-embedded-1.69.5
Successfully installed jekyll-sass-converter-3.0.0
Successfully installed concurrent-ruby-1.2.2
Successfully installed i18n-1.14.1
Building native extensions. This could take a while...
Successfully installed http_parser.rb-0.8.0
Building native extensions. This could take a while...
Successfully installed eventmachine-1.2.7
Successfully installed em-websocket-0.5.3
Successfully installed colorator-1.1.0
Successfully installed public_suffix-5.0.4
Successfully installed addressable-2.8.6
Successfully installed jekyll-4.3.2
Parsing documentation for unicode-display_width-2.5.0
Installing ri documentation for unicode-display_width-2.5.0
Parsing documentation for terminal-table-3.0.2
Installing ri documentation for terminal-table-3.0.2
Parsing documentation for safe_yaml-1.0.5
Installing ri documentation for safe_yaml-1.0.5
Parsing documentation for rouge-4.2.0
Installing ri documentation for rouge-4.2.0
Parsing documentation for forwardable-extended-2.6.0
Installing ri documentation for forwardable-extended-2.6.0
Parsing documentation for pathutil-0.16.2
Installing ri documentation for pathutil-0.16.2
Parsing documentation for mercenary-0.4.0
Installing ri documentation for mercenary-0.4.0
Parsing documentation for liquid-4.0.4
Installing ri documentation for liquid-4.0.4
Parsing documentation for kramdown-2.4.0
Installing ri documentation for kramdown-2.4.0
Parsing documentation for kramdown-parser-gfm-1.1.0
Installing ri documentation for kramdown-parser-gfm-1.1.0
Parsing documentation for ffi-1.16.3
Installing ri documentation for ffi-1.16.3
Parsing documentation for rb-inotify-0.10.1
Installing ri documentation for rb-inotify-0.10.1
Parsing documentation for rb-fsevent-0.11.2
Installing ri documentation for rb-fsevent-0.11.2
Parsing documentation for listen-3.8.0
Installing ri documentation for listen-3.8.0
Parsing documentation for jekyll-watch-2.2.1
Installing ri documentation for jekyll-watch-2.2.1
Parsing documentation for google-protobuf-3.25.1-x86_64-linux
Installing ri documentation for google-protobuf-3.25.1-x86_64-linux
Parsing documentation for sass-embedded-1.69.5
Installing ri documentation for sass-embedded-1.69.5
Parsing documentation for jekyll-sass-converter-3.0.0
Installing ri documentation for jekyll-sass-converter-3.0.0
Parsing documentation for concurrent-ruby-1.2.2
Installing ri documentation for concurrent-ruby-1.2.2
Parsing documentation for i18n-1.14.1
Installing ri documentation for i18n-1.14.1
Parsing documentation for http_parser.rb-0.8.0
unknown encoding name "chunked\r\n\r\n25" for ext/ruby_http_parser/vendor/http-parser-java/tools/parse_tests.rb, skipping
Installing ri documentation for http_parser.rb-0.8.0
Parsing documentation for eventmachine-1.2.7
Installing ri documentation for eventmachine-1.2.7
Parsing documentation for em-websocket-0.5.3
Installing ri documentation for em-websocket-0.5.3
Parsing documentation for colorator-1.1.0
Installing ri documentation for colorator-1.1.0
Parsing documentation for public_suffix-5.0.4
Installing ri documentation for public_suffix-5.0.4
Parsing documentation for addressable-2.8.6
Installing ri documentation for addressable-2.8.6
Parsing documentation for jekyll-4.3.2
Installing ri documentation for jekyll-4.3.2
Done installing documentation for unicode-display_width, terminal-table, safe_yaml, rouge, forwardable-extended, pathutil, mercenary, liquid, kramdown, kramdown-parser-gfm, ffi, rb-inotify, rb-fsevent, listen, jekyll-watch, google-protobuf, sass-embedded, jekyll-sass-converter, concurrent-ruby, i18n, http_parser.rb, eventmachine, em-websocket, colorator, public_suffix, addressable, jekyll after 19 seconds
Fetching bundler-2.4.22.gem
Successfully installed bundler-2.4.22
Parsing documentation for bundler-2.4.22
Installing ri documentation for bundler-2.4.22
Done installing documentation for bundler after 0 seconds
28 gems installed
etu@vm0:~$ lxc exec jekyll -- jekyll new my0wnBl0g
Running bundle install in /root/my0wnBl0g...
  Bundler: Fetching gem metadata from https://rubygems.org/............
  Bundler: Resolving dependencies...
  Bundler: Fetching rake 13.1.0
  Bundler: Installing rake 13.1.0
  Bundler: Fetching rexml 3.2.6
  Bundler: Installing rexml 3.2.6
  Bundler: Fetching jekyll-feed 0.17.0
  Bundler: Fetching jekyll-seo-tag 2.8.0
  Bundler: Installing jekyll-feed 0.17.0
  Bundler: Installing jekyll-seo-tag 2.8.0
  Bundler: Fetching minima 2.5.1
  Bundler: Installing minima 2.5.1
  Bundler: Bundle complete! 7 Gemfile dependencies, 34 gems now installed.
  Bundler: Use `bundle info [gemname]` to see where a bundled gem is installed.Don't run Bundler as root. Installing your bundle as root will break this
  Bundler: application for all non-root users on this machine.
New jekyll site installed in /root/my0wnBl0g.
etu@vm0:~$ lxc exec jekyll -- bash -c "cd my0wnBl0g && bundle exec jekyll serve"
Configuration file: /root/my0wnBl0g/_config.yml
            Source: /root/my0wnBl0g
       Destination: /root/my0wnBl0g/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
Deprecation Warning: Using / for division outside of calc() is deprecated and will be removed in Dart Sass 2.0.0.

Recommendation: math.div($spacing-unit, 2) or calc($spacing-unit / 2)

More info and automated migrator: https://sass-lang.com/d/slash-div

   ╷
40 │   margin-bottom: $spacing-unit / 2;
   │                  ^^^^^^^^^^^^^^^^^
   ╵
    ../../../../minima-2.5.1/_sass/minima/_base.scss 40:18  @import
    minima.scss 48:3                                        @import
    /root/my0wnBl0g/assets/main.scss 1:9                    root stylesheet
Deprecation Warning: Using / for division outside of calc() is deprecated and will be removed in Dart Sass 2.0.0.

Recommendation: math.div($spacing-unit, 2) or calc($spacing-unit / 2)

More info and automated migrator: https://sass-lang.com/d/slash-div

    ╷
134 │   padding-left: $spacing-unit / 2;
    │                 ^^^^^^^^^^^^^^^^^
    ╵
    ../../../../minima-2.5.1/_sass/minima/_base.scss 134:17  @import
    minima.scss 48:3                                         @import
    /root/my0wnBl0g/assets/main.scss 1:9                     root stylesheet
Deprecation Warning: Using / for division outside of calc() is deprecated and will be removed in Dart Sass 2.0.0.

Recommendation: math.div($spacing-unit, 2) or calc($spacing-unit / 2)

More info and automated migrator: https://sass-lang.com/d/slash-div

    ╷
189 │     padding-right: $spacing-unit / 2;
    │                    ^^^^^^^^^^^^^^^^^
    ╵
    ../../../../minima-2.5.1/_sass/minima/_base.scss 189:20  @content
    minima.scss 38:5                                         media-query()
    ../../../../minima-2.5.1/_sass/minima/_base.scss 186:3   @import
    minima.scss 48:3                                         @import
    /root/my0wnBl0g/assets/main.scss 1:9                     root stylesheet
Deprecation Warning: Using / for division outside of calc() is deprecated and will be removed in Dart Sass 2.0.0.

Recommendation: math.div($spacing-unit, 2) or calc($spacing-unit / 2)

More info and automated migrator: https://sass-lang.com/d/slash-div

    ╷
190 │     padding-left: $spacing-unit / 2;
    │                   ^^^^^^^^^^^^^^^^^
    ╵
    ../../../../minima-2.5.1/_sass/minima/_base.scss 190:19  @content
    minima.scss 38:5                                         media-query()
    ../../../../minima-2.5.1/_sass/minima/_base.scss 186:3   @import
    minima.scss 48:3                                         @import
    /root/my0wnBl0g/assets/main.scss 1:9                     root stylesheet
Deprecation Warning: Using / for division outside of calc() is deprecated and will be removed in Dart Sass 2.0.0.

Recommendation: math.div($spacing-unit, 3) or calc($spacing-unit / 3)

More info and automated migrator: https://sass-lang.com/d/slash-div

    ╷
244 │     padding: ($spacing-unit / 3) ($spacing-unit / 2);
    │               ^^^^^^^^^^^^^^^^^
    ╵
    ../../../../minima-2.5.1/_sass/minima/_base.scss 244:15  @import
    minima.scss 48:3                                         @import
    /root/my0wnBl0g/assets/main.scss 1:9                     root stylesheet
Warning: 6 repetitive deprecation warnings omitted.
Run in verbose mode to see all warnings.
                    done in 0.483 seconds.
 Auto-regeneration: enabled for '/root/my0wnBl0g'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
^Cetu@vm0:~$
etu@vm0:~$ lxc ls
+--------+---------+---------------------+-------------------------------------------+-----------+-----------+
|  NAME  |  STATE  |        IPV4         |                   IPV6                    |   TYPE    | SNAPSHOTS |
+--------+---------+---------------------+-------------------------------------------+-----------+-----------+
| jekyll | RUNNING | 198.18.60.76 (eth0) | 2001:678:3fc:3c:216:3eff:fe27:e942 (eth0) | CONTAINER | 0         |
+--------+---------+---------------------+-------------------------------------------+-----------+-----------+
etu@vm0:~$ exit
déconnexion
Connection to 2001:678:3fc:3c:baad:caff:fefe:39 closed.
PS C:\Users\hp> ssh -L 8000:198.18.28.212:80 oscar
Linux oscar 6.5.0-5-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.13-1 (2023-11-29) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
-----
                  __             _
|  ||_  _   o _  /  \ _ _  _  _   )
|/\|| )(_)  |_)  \__/_)(_ (_||   o

        An opponent, similar to Mallory, but not necessarily malicious.

        https://en.wikipedia.org/wiki/Alice_and_Bob

-----
Last login: Tue Dec 12 13:08:23 2023 from 2a03:7220:8083:c4c4::1012
chehabmosaad@oscar:~$ exit
déconnexion
Connection to oscar.infra.stri closed.
PS C:\Users\hp> ssh -L "8000:[2001:678:3fc:1c:216:3eff:feaa:2b68]:80" oscar
Linux oscar 6.5.0-5-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.13-1 (2023-11-29) x86_64

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
Last login: Tue Dec 12 21:21:48 2023 from 2a03:7220:8083:c4c4::1012
chehabmosaad@oscar:~$
```
# Fin