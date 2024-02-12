La traduction française est ci-dessous. 👇
<body>
    <h1>TP Linux System Administration</h1>
    <ul>
        <li>
            <h2>TP1: Configuration and use of a VPN</h2>
            <p>In this work, I learned how to configure and use a virtual private network (VPN) with OpenVPN, how to manage IPv4 and IPv6 interfaces and routing tables, how to test connectivity and DNS resolution, how to establish secure connections with SSH to different types of machines, how to create and access virtual machines with Debian, Ubuntu and Windows 11, and how to use the SPICE protocol to display the graphical screen of virtual machines. I also set up a working tree and copied a Debian virtual machine image. This work allowed me to develop my skills in Linux system administration and to familiarize myself with the necessary tools and commands.</p>
        </li>
        <li>
            <h2>TP2: Installation and use of Openvswitch and lxd</h2>
            <p>In this work, I learned how to install and use Openvswitch and lxd, two tools that allow to create and manage virtual networks and containers. I accessed a virtual machine and updated it. I edited the /etc/network/interfaces file to configure the network interfaces of the virtual machine. I restarted the virtual machine and checked the status of the interfaces. I installed Openvswitch and lxd on the virtual machine. I configured the C-3P0 switch with Openvswitch and connected it to the virtual machine. I launched two containers with lxd, one with a Debian image and the other with an Ubuntu image. I created a user account on each container. I installed the nginx package on each container to open a web service. I accessed the web page of each container via an SSH tunnel. This work allowed me to develop my skills in network administration and virtualization with Linux.</p>
        </li>
        <li>
            <h2>TP3: Writing and using a checkup.sh script</h2>
            <p>In this work, I learned how to write and use a checkup.sh script that checks the health status of my Linux system. This script displays information about the CPU, memory, disk, network, and processes usage. I also learned how to manage processes with the ps, top, kill, and nice commands. I installed and configured a static web service with Apache and Jekyll. Jekyll is a static site generator that transforms Markdown files into HTML pages. I created a simple website with Jekyll and deployed it on my Apache web server. I also used the HTTP protocol to access my website from a browser.</p>
        </li>
        <li>
            <h2>TP4: Creating and managing user accounts</h2>
            <p>In this work, I learned how to create and manage user accounts, how to configure PAM to modify the default rights, and how to consult the logs of user connections. I created a newuser account belonging to the adm group, who is a developer of a dynamic website. I configured PAM to have a user mask of 027 when logging in, which means that the files and directories created by newuser will not be readable or writable by other users. I used the last command to display the last 50 lines of the user connection log, which is located in the /var/log/wtmp file. This log contains information about the username, terminal, IP address, date and duration of each connection.</p>
        </li>
        <li>
                    <h2>TP5: Consulting the kernel and service logs</h2>
            <p>In this work, I learned how to use the dmesg and journalctl commands to consult the kernel and service logs. I displayed the kernel version and identified the corresponding package with the uname -r and dpkg -S commands. I displayed the last 20 lines of the kernel log with the dmesg -T | tail -n 20 command. I displayed the status of the apache2 service from the kernel log with the journalctl -u apache2 command. I displayed the status of the systemd-networkd, systemd-resolved, and systemd-timesyncd services with the systemctl status command. This work allowed me to develop my skills in managing logs and services with Linux.</p>
    </li>
    </ul>

<hr>
    <h1>TP Administration système en Linux</h1>
    <ul>
    <li>
            <h2>TP1 : Configuration et utilisation d'un VPN</h2>
            <p>Dans ce travail, j’ai appris à configurer et à utiliser un réseau virtuel privé (VPN) avec OpenVPN, à gérer les interfaces et les tables de routage IPv4 et IPv6, à tester la connectivité et la résolution DNS, à établir des connexions sécurisées avec SSH à différents types de machines, à créer et à accéder à des machines virtuelles avec Debian, Ubuntu et Windows 11, et à utiliser le protocole SPICE pour afficher l’écran graphique des machines virtuelles. J’ai également mis en place une arborescence de travail et copié une image de machine virtuelle Debian. Ce travail m’a permis de développer mes compétences en administration système Linux et de me familiariser avec les outils et les commandes nécessaires.</p>
        </li>
        <li>
            <h2>TP2 : Installation et utilisation d'Openvswitch et lxd</h2>
            <p>Dans ce travail, j’ai appris à installer et à utiliser Openvswitch et lxd, deux outils qui permettent de créer et de gérer des réseaux virtuels et des conteneurs. J’ai accédé à une machine virtuelle et je l’ai mise à jour. J’ai édité le fichier /etc/network/interfaces pour configurer les interfaces réseau de la machine virtuelle. J’ai redémarré la machine virtuelle et vérifié l’état des interfaces. J’ai installé Openvswitch et lxd sur la machine virtuelle. J’ai configuré le commutateur C-3P0 avec Openvswitch et je l’ai raccordé à la machine virtuelle. J’ai lancé deux conteneurs avec lxd, un avec une image Debian et l’autre avec une image Ubuntu. J’ai créé un compte utilisateur sur chaque conteneur. J’ai installé le paquet nginx sur chaque conteneur pour ouvrir un service Web. J’ai accédé à la page Web de chaque conteneur via un tunnel SSH. Ce travail m’a permis de développer mes compétences en administration réseau et en virtualisation avec Linux.</p>
        </li>
        <li>
            <h2>TP3 : Écriture et utilisation d'un script checkup.sh</h2>
            <p>Dans ce travail, j’ai appris à écrire et à utiliser un script checkup.sh qui vérifie l’état de santé de mon système Linux. Ce script affiche des informations sur l’utilisation du CPU, de la mémoire, du disque, du réseau, et des processus. J’ai également appris à gérer les processus avec les commandes ps, top, kill, et nice. J’ai installé et configuré un service web statique avec Apache et Jekyll. Jekyll est un générateur de sites statiques qui transforme des fichiers Markdown en pages HTML. J’ai créé un site web simple avec Jekyll et je l’ai déployé sur mon serveur web Apache. J’ai également utilisé le protocole HTTP pour accéder à mon site web depuis un navigateur.</p>
        </li>
        <li>
            <h2>TP4 : Création et gestion des comptes utilisateurs</h2>
            <p>Dans ce travail, j’ai appris à créer et à gérer des comptes utilisateurs, à configurer PAM pour modifier les droits par défaut, et à consulter les journaux des connexions utilisateur. J’ai créé un compte newuser appartenant au groupe adm, qui est un développeur d’un site web dynamique. J’ai configuré PAM pour avoir un masque utilisateur à 027 lors de la connexion, ce qui signifie que les fichiers et les répertoires créés par newuser ne seront pas accessibles en lecture ou en écriture par les autres utilisateurs. J’ai utilisé la commande last pour afficher les 50 dernières lignes du journal des connexions utilisateur, qui se trouve dans le fichier /var/log/wtmp. Ce journal contient les informations sur le nom d’utilisateur, le terminal, l’adresse IP, la date et la durée de chaque connexion.</p>
        </li>
        <li>
            <h2>TP5 : Consultation des journaux du noyau et des services</h2>
            <p>Dans ce travail, j’ai appris à utiliser les commandes dmesg et journalctl pour consulter les journaux du noyau et des services. J’ai affiché la version du noyau et identifié le paquet correspondant avec la commande uname -r et la commande dpkg -S. J’ai affiché les 20 dernières lignes du journal du noyau avec la commande dmesg -T | tail -n 20. J’ai affiché l’état du service apache2 du journal du noyau avec la commande journalctl -u apache2. J’ai affiché l’état des services systemd-networkd, systemd-resolved, et systemd-timesyncd avec la commande systemctl status. Ce travail m’a permis de développer mes compétences en administration des journaux et des services avec Linux.</p>
        </li>
    </ul>
</body>
