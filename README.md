<body>
    <h1>TP-Adm-Linux</h1>
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
