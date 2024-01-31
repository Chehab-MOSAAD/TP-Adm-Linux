# Chehab MOSAAD (1A STRI)
## TP4 Administration Système Linux
## Partie 1 - Création du compte newuser appartenant au groupe adm
```Powershell
etu@vm0:~$ grep etu /etc/group
adm:x:4:etu
cdrom:x:24:etu
floppy:x:25:etu
sudo:x:27:etu
audio:x:29:etu
dip:x:30:etu
video:x:44:etu
plugdev:x:46:etu
users:x:100:etu
netdev:x:106:etu
etu:x:1000:
lxd:x:995:etu
etu@vm0:~$ grep audio /etc/group
audio:x:29:etu
etu@vm0:~$ id
uid=1000(etu) gid=1000(etu) groupes=1000(etu),4(adm),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),100(users),106(netdev),995(lxd)
etu@vm0:~$ ls -l /dev/snd/
total 0
drwxr-xr-x 2 root root       60 11 déc.  17:39 by-path
crw-rw---- 1 root audio 116,  4 11 déc.  17:39 controlC0
crw-rw---- 1 root audio 116,  3 11 déc.  17:39 hwC0D0
crw-rw---- 1 root audio 116,  2 11 déc.  17:39 pcmC0D0p
crw-rw---- 1 root audio 116,  1 11 déc.  17:39 seq
crw-rw---- 1 root audio 116, 33 11 déc.  17:39 timer
etu@vm0:~$ ps faux | grep apache2
root         859  0.0  0.1   6604  1920 ?        Ss   déc.11   0:06 /usr/sbin/apache2 -k start
www-data    2783  0.0  0.2 1933528 2560 ?        Sl   00:00   0:00  \_ /usr/sbin/apache2 -k start
www-data    2784  0.0  0.2 1999008 2432 ?        Sl   00:00   0:00  \_ /usr/sbin/apache2 -k start
etu        24580  0.0  0.2   6364  2304 pts/0    S+   22:09   0:00              \_ grep apache2
etu@vm0:~$ sudo mkdir /var/www/mywebsite
[sudo] Mot de passe de etu :
etu@vm0:~$ sudo chown www-data:www-data /var/www/mywebsite/
etu@vm0:~$ sudo chmod 2770 /var/www/mywebsite
etu@vm0:~$ ls -l /var/www/ | grep mywebsite
drwxrws--- 2 www-data www-data 4096 12 déc.  22:11 mywebsite
etu@vm0:~$ ls -ln /var/www/ | grep mywebsite
drwxrws--- 2   33 33 4096 12 déc.  22:11 mywebsite
etu@vm0:~$ sudo adduser newuser
info: Ajout de l'utilisateur « newuser » ...
info: Choix d'un UID/GID dans la plage 1000 à 59999 ...
info: Ajout du nouveau groupe « newuser » (1001) ...
info: Ajout du nouvel utilisateur « newuser » (1001) avec le groupe « newuser » (1001) ...
info: Création du répertoire personnel « /home/newuser » ...
info: Copie des fichiers depuis « /etc/skel » ...
Nouveau mot de passe :
Retapez le nouveau mot de passe :
passwd : mot de passe mis à jour avec succès
Modifier les informations associées à un utilisateur pour newuser
Entrer la nouvelle valeur, ou appuyer sur ENTER pour la valeur par défaut
        NOM []: newuser
        Numéro de chambre []:
        Téléphone professionnel []:
        Téléphone personnel []:
        Autre []:
Cette information est-elle correcte ? [O/n]O
info: Ajout du nouvel utilisateur « newuser » aux groupes supplémentaires « users » ...
info: Ajout de l'utilisateur « newuser » au groupe « users » ...
etu@vm0:~$ sudo adduser newuser adm
info: Ajout de l'utilisateur « newuser » au groupe « adm » ...
etu@vm0:~$ id newuser
uid=1001(newuser) gid=1001(newuser) groupes=1001(newuser),4(adm),100(users)
```
## L'utilisateur 'newuser' est développeur d'un site web dynamique
```Powershell
etu@vm0:~$ sudo adduser newuser www-data
info: Ajout de l utilisateur « newuser » au groupe « www-data » ...
etu@vm0:~$ id newuser
uid=1001(newuser) gid=1001(newuser) groupes=1001(newuser),4(adm),33(www-data),100(users)
etu@vm0:~$ sudo chgrp www-data /var/www/html/
etu@vm0:~$ sudo chgrp www-data /var/www/newhtml/
```
## Configurer PAM pour avoir un masque utilisateur à 027 lors de la connexion
```Powershell
etu@vm0:~$ sudo vim login.defs
etu@vm0:~$ etu@vm0:~$
etu@vm0:~$ cat login.defscat login.defs
cat: login.defscat: Aucun fichier ou dossier de ce type
#
# /etc/login.defs - Configuration control definitions for the login package.
#
# Three items must be defined:  MAIL_DIR, ENV_SUPATH, and ENV_PATH.
# If unspecified, some arbitrary (and possibly incorrect) value will
# be assumed.  All other items are optional - if not specified then
# the described action or option will be inhibited.
#
# Comment lines (lines beginning with "#") and blank lines are ignored.
#
# Modified for Linux.  --marekm

# REQUIRED for useradd/userdel/usermod
#   Directory where mailboxes reside, _or_ name of file, relative to the
#   home directory.  If you _do_ define MAIL_DIR and MAIL_FILE,
#   MAIL_DIR takes precedence.
#
#   Essentially:
#      - MAIL_DIR defines the location of users mail spool files
#        (for mbox use) by appending the username to MAIL_DIR as defined
#        below.
#      - MAIL_FILE defines the location of the users mail spool files as the
#        fully-qualified filename obtained by prepending the user home
#        directory before $MAIL_FILE
#
# NOTE: This is no more used for setting up users MAIL environment variable
#       which is, starting from shadow 4.0.12-1 in Debian, entirely the
#       job of the pam_mail PAM modules
#       See default PAM configuration files provided for
#       login, su, etc.
#
# This is a temporary situation: setting these variables will soon
# move to /etc/default/useradd and the variables will then be
# no more supported
MAIL_DIR        /var/mail
#MAIL_FILE      .mail

#
# Enable logging and display of /var/log/faillog login failure info.
# This option conflicts with the pam_tally PAM module.
#
FAILLOG_ENAB            yes

#
# Enable display of unknown usernames when login failures are recorded.
#
# WARNING: Unknown usernames may become world readable.
# See #290803 and #298773 for details about how this could become a security
# concern
LOG_UNKFAIL_ENAB        no

#
# Enable logging of successful logins
#
LOG_OK_LOGINS           no

#
# Enable "syslog" logging of su activity - in addition to sulog file logging.
# SYSLOG_SG_ENAB does the same for newgrp and sg.
#
SYSLOG_SU_ENAB          yes
SYSLOG_SG_ENAB          yes

#
# If defined, all su activity is logged to this file.
#
#SULOG_FILE     /var/log/sulog

#
# If defined, file which maps tty line to TERM environment parameter.
# Each line of the file is in a format something like "vt100  tty01".
#
#TTYTYPE_FILE   /etc/ttytype

#
# If defined, login failures will be logged here in a utmp format
# last, when invoked as lastb, will read /var/log/btmp, so...
#
FTMP_FILE       /var/log/btmp

#
# If defined, the command name to display when running "su -".  For
# example, if this is defined as "su" then a "ps" will display the
# command is "-su".  If not defined, then "ps" would display the
# name of the shell actually being run, e.g. something like "-sh".
#
SU_NAME         su

#
# If defined, file which inhibits all the usual chatter during the login
# sequence.  If a full pathname, then hushed mode will be enabled if the
# user's name or shell are found in the file.  If not a full pathname, then
# hushed mode will be enabled if the file exists in the user's home directory.
#
HUSHLOGIN_FILE  .hushlogin
#HUSHLOGIN_FILE /etc/hushlogins

#
# *REQUIRED*  The default PATH settings, for superuser and normal users.
#
# (they are minimal, add the rest in the shell startup files)
ENV_SUPATH      PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV_PATH        PATH=/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games

#
# Terminal permissions
#
#       TTYGROUP        Login tty will be assigned this group ownership.
#       TTYPERM         Login tty will be set to this permission.
#
# If you have a "write" program which is "setgid" to a special group
# which owns the terminals, define TTYGROUP to the group number and
# TTYPERM to 0620.  Otherwise leave TTYGROUP commented out and assign
# TTYPERM to either 622 or 600.
#
# In Debian /usr/bin/bsd-write or similar programs are setgid tty
# However, the default and recommended value for TTYPERM is still 0600
# to not allow anyone to write to anyone else console or terminal

# Users can still allow other people to write them by issuing
# the "mesg y" command.

TTYGROUP        tty
TTYPERM         0600

#
# Login configuration initializations:
#
#       ERASECHAR       Terminal ERASE character ('\010' = backspace).
#       KILLCHAR        Terminal KILL character ('\025' = CTRL/U).
#       UMASK           Default "umask" value.
#
# The ERASECHAR and KILLCHAR are used only on System V machines.
#
# UMASK is the default umask value for pam_umask and is used by
# useradd and newusers to set the mode of the new home directories.
# 022 is the "historical" value in Debian for UMASK
# 027, or even 077, could be considered better for privacy
# There is no One True Answer here : each sysadmin must make up his/her
# mind.
#
# If USERGROUPS_ENAB is set to "yes", that will modify this UMASK default value
# for private user groups, i. e. the uid is the same as gid, and username is
# the same as the primary group name: for these, the user permissions will be
# used as group permissions, e. g. 022 will become 002.
#
# Prefix these values with "0" to get octal, "0x" to get hexadecimal.
#
ERASECHAR       0177
KILLCHAR        025
UMASK           027

# HOME_MODE is used by useradd(8) and newusers(8) to set the mode for new
# home directories.
# If HOME_MODE is not set, the value of UMASK is used to create the mode.
#HOME_MODE      0700

#
# Password aging controls:
#
#       PASS_MAX_DAYS   Maximum number of days a password may be used.
#       PASS_MIN_DAYS   Minimum number of days allowed between password changes.
#       PASS_WARN_AGE   Number of days warning given before a password expires.
#
PASS_MAX_DAYS   99999
PASS_MIN_DAYS   0
PASS_WARN_AGE   7

#
# Min/max values for automatic uid selection in useradd
#
UID_MIN                  1000
UID_MAX                 60000
# System accounts
#SYS_UID_MIN              100
#SYS_UID_MAX              999
# Extra per user uids
SUB_UID_MIN                100000
SUB_UID_MAX             600100000
SUB_UID_COUNT               65536

#
# Min/max values for automatic gid selection in groupadd
#
GID_MIN                  1000
GID_MAX                 60000
# System accounts
#SYS_GID_MIN              100
#SYS_GID_MAX              999
# Extra per user group ids
SUB_GID_MIN                100000
SUB_GID_MAX             600100000
SUB_GID_COUNT               65536

#
# Max number of login retries if password is bad. This will most likely be
# overriden by PAM, since the default pam_unix module has it's own built
# in of 3 retries. However, this is a safe fallback in case you are using
# an authentication module that does not enforce PAM_MAXTRIES.
#
LOGIN_RETRIES           5

#
# Max time in seconds for login
#
LOGIN_TIMEOUT           60

#
# Which fields may be changed by regular users using chfn - use
# any combination of letters "frwh" (full name, room number, work
# phone, home phone).  If not defined, no changes are allowed.
# For backward compatibility, "yes" = "rwh" and "no" = "frwh".
#
CHFN_RESTRICT           rwh

#
# Should login be allowed if we can't cd to the home directory?
# Default is no.
#
DEFAULT_HOME    yes

#
# If defined, this command is run when removing a user.
# It should remove any at/cron/print jobs etc. owned by
# the user to be removed (passed as the first argument).
#
#USERDEL_CMD    /usr/sbin/userdel_local

#
# If set to yes, userdel will remove the user's group if it contains no
# more members, and useradd will create by default a group with the name
# of the user.
#
# Other former uses of this variable such as setting the umask when
# user==primary group are not used in PAM environments, such as Debian
#
USERGROUPS_ENAB yes

#
# Instead of the real user shell, the program specified by this parameter
# will be launched, although its visible name (argv[0]) will be the shell's.
# The program may do whatever it wants (logging, additional authentification,
# banner, ...) before running the actual shell.
#
# FAKE_SHELL /bin/fakeshell

#
# If defined, either full pathname of a file containing device names or
# a ":" delimited list of device names.  Root logins will be allowed only
# upon these devices.
#
# This variable is used by login and su.
#
#CONSOLE        /etc/consoles
#CONSOLE        console:tty01:tty02:tty03:tty04

#
# List of groups to add to the user's supplementary group set
# when logging in on the console (as determined by the CONSOLE
# setting).  Default is none.
#
# Use with caution - it is possible for users to gain permanent
# access to these groups, even when not logged in on the console.
# How to do it is left as an exercise for the reader...
#
# This variable is used by login and su.
#
#CONSOLE_GROUPS         floppy:audio:cdrom

#
# If set to "yes", new passwords will be encrypted using the MD5-based
# algorithm compatible with the one used by recent releases of FreeBSD.
# It supports passwords of unlimited length and longer salt strings.
# Set to "no" if you need to copy encrypted passwords to other systems
# which don't understand the new algorithm.  Default is "no".
#
# This variable is deprecated. You should use ENCRYPT_METHOD.
#
#MD5_CRYPT_ENAB no

#
# If set to MD5, MD5-based algorithm will be used for encrypting password
# If set to SHA256, SHA256-based algorithm will be used for encrypting password
# If set to SHA512, SHA512-based algorithm will be used for encrypting password
# If set to BCRYPT, BCRYPT-based algorithm will be used for encrypting password
# If set to YESCRYPT, YESCRYPT-based algorithm will be used for encrypting password
# If set to DES, DES-based algorithm will be used for encrypting password (default)
# MD5 and DES should not be used for new hashes, see crypt(5) for recommendations.
# Overrides the MD5_CRYPT_ENAB option
#
# Note: It is recommended to use a value consistent with
# the PAM modules configuration.
#
ENCRYPT_METHOD SHA512

#
# Only works if ENCRYPT_METHOD is set to SHA256 or SHA512.
#
# Define the number of SHA rounds.
# With a lot of rounds, it is more difficult to brute-force the password.
# However, more CPU resources will be needed to authenticate users if
# this value is increased.
#
# If not specified, the libc will choose the default number of rounds (5000),
# which is orders of magnitude too low for modern hardware.
# The values must be within the 1000-999999999 range.
# If only one of the MIN or MAX values is set, then this value will be used.
# If MIN > MAX, the highest value will be used.
#
#SHA_CRYPT_MIN_ROUNDS 5000
#SHA_CRYPT_MAX_ROUNDS 5000

#
# Only works if ENCRYPT_METHOD is set to YESCRYPT.
#
# Define the YESCRYPT cost factor.
# With a higher cost factor, it is more difficult to brute-force the password.
# However, more CPU time and more memory will be needed to authenticate users
# if this value is increased.
#
# If not specified, a cost factor of 5 will be used.
# The value must be within the 1-11 range.
#
#YESCRYPT_COST_FACTOR 5

#
# The pwck(8) utility emits a warning for any system account with a home
# directory that does not exist.  Some system accounts intentionally do
# not have a home directory.  Such accounts may have this string as
# their home directory in /etc/passwd to avoid a spurious warning.
#
NONEXISTENT     /nonexistent

#
# Allow newuidmap and newgidmap when running under an alternative
# primary group.
#
#GRANT_AUX_GROUP_SUBIDS yes

#
# Select the HMAC cryptography algorithm.
# Used in pam_timestamp module to calculate the keyed-hash message
# authentication code.
#
# Note: It is recommended to check hmac(3) to see the possible algorithms
# that are available in your system.
#
#HMAC_CRYPTO_ALGO SHA512

################# OBSOLETED BY PAM ##############
#                                               #
# These options are now handled by PAM. Please  #
# edit the appropriate file in /etc/pam.d/ to   #
# enable the equivelants of them.
#
###############

#MOTD_FILE
#DIALUPS_CHECK_ENAB
#LASTLOG_ENAB
#MAIL_CHECK_ENAB
#OBSCURE_CHECKS_ENAB
#PORTTIME_CHECKS_ENAB
#SU_WHEEL_ONLY
#CRACKLIB_DICTPATH
#PASS_CHANGE_TRIES
#PASS_ALWAYS_WARN
#ENVIRON_FILE
#NOLOGINS_FILE
#ISSUE_FILE
#PASS_MIN_LEN
#PASS_MAX_LEN
#ULIMIT
#ENV_HZ
#CHFN_AUTH
#CHSH_AUTH
#FAIL_DELAY

################# OBSOLETED #######################
#                                                 #
# These options are no more handled by shadow.    #
#                                                 #
# Shadow utilities will display a warning if they #
# still appear.                                   #
#                                                 #
###################################################

# CLOSE_SESSIONS
# LOGIN_STRING
# NO_PASSWORD_CONSOLE
# QMAIL_DIR
etu@vm0:~$ sudo apt install git fonts-powerline
[sudo] Mot de passe de etu :
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances... Fait
Lecture des informations d'état... Fait
fonts-powerline est déjà la version la plus récente (2.8.3-4).
Les paquets supplémentaires suivants seront installés :
  git-man liberror-perl patch
Paquets suggérés :
  git-daemon-run | git-daemon-sysvinit git-doc git-email git-gui gitk gitweb git-cvs git-mediawiki git-svn ed
  diffutils-doc
Les NOUVEAUX paquets suivants seront installés :
  git git-man liberror-perl patch
0 mis à jour, 4 nouvellement installés, 0 à enlever et 0 non mis à jour.
Il est nécessaire de prendre 9 810 ko dans les archives.
Après cette opération, 50,4 Mo d espace disque supplémentaires seront utilisés.
Souhaitez-vous continuer ? [O/n] O
Réception de :1 http://deb.debian.org/debian testing/main amd64 liberror-perl all 0.17029-2 [29,0 kB]
Réception de :2 http://deb.debian.org/debian testing/main amd64 git-man all 1:2.43.0-1 [2 112 kB]
Réception de :3 http://deb.debian.org/debian testing/main amd64 git amd64 1:2.43.0-1 [7 541 kB]
Réception de :4 http://deb.debian.org/debian testing/main amd64 patch amd64 2.7.6-7 [128 kB]
9 810 ko réceptionnés en 1s (8 981 ko/s)
Sélection du paquet liberror-perl précédemment désélectionné.
(Lecture de la base de données... 43558 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../liberror-perl_0.17029-2_all.deb ...
Dépaquetage de liberror-perl (0.17029-2) ...
Sélection du paquet git-man précédemment désélectionné.
Préparation du dépaquetage de .../git-man_1%3a2.43.0-1_all.deb ...
Dépaquetage de git-man (1:2.43.0-1) ...
Sélection du paquet git précédemment désélectionné.
Préparation du dépaquetage de .../git_1%3a2.43.0-1_amd64.deb ...
Dépaquetage de git (1:2.43.0-1) ...
Sélection du paquet patch précédemment désélectionné.
Préparation du dépaquetage de .../patch_2.7.6-7_amd64.deb ...
Dépaquetage de patch (2.7.6-7) ...
Paramétrage de liberror-perl (0.17029-2) ...
Paramétrage de patch (2.7.6-7) ...
Paramétrage de git-man (1:2.43.0-1) ...
Paramétrage de git (1:2.43.0-1) ...
Traitement des actions différées (« triggers ») pour man-db (2.12.0-1) ...
Scanning processes...
Scanning linux images...

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.
etu@vm0:~$ bash -c "$(wget https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh -O -)"
--2023-12-12 22:58:53--  https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh
Résolution de raw.githubusercontent.com (raw.githubusercontent.com)… 2606:50c0:8001::154, 2606:50c0:8002::154, 2606:50c0:8003::154, ...
Connexion à raw.githubusercontent.com (raw.githubusercontent.com)|2606:50c0:8001::154|:443… connecté.
requête HTTP transmise, en attente de la réponse… 200 OK
Taille : 11169 (11K) [text/plain]
Sauvegarde en : « STDOUT »

-                            100%[==============================================>]  10,91K  --.-KB/s    ds 0s

2023-12-12 22:58:53 (47,1 MB/s) — envoi vers sortie standard [11169/11169]

Cloning Oh My Bash...
$ git clone --depth=1 https://github.com/ohmybash/oh-my-bash.git /home/etu/.oh-my-bash
Clonage dans '/home/etu/.oh-my-bash'...
remote: Enumerating objects: 435, done.
remote: Counting objects: 100% (435/435), done.
remote: Compressing objects: 100% (393/393), done.
remote: Total 435 (delta 28), reused 305 (delta 26), pack-reused 0
Réception d'objets: 100% (435/435), 3.31 Mio | 17.92 Mio/s, fait.
Résolution des deltas: 100% (28/28), fait.
Looking for an existing bash config...
Found ~/.bashrc. Backing up to /home/etu/.bashrc.omb-backup-20231212225854
$ mv /home/etu/.bashrc /home/etu/.bashrc.omb-backup-20231212225854
Copying the Oh-My-Bash template file to ~/.bashrc
$ mv -f /home/etu/.bashrc.omb-temp /home/etu/.bashrc
         __                          __               __
  ____  / /_     ____ ___  __  __   / /_  ____ ______/ /_
 / __ \/ __ \   / __ `__ \/ / / /  / __ \/ __ `/ ___/ __ \
/ /_/ / / / /  / / / / / / /_/ /  / /_/ / /_/ (__  ) / / /
\____/_/ /_/  /_/ /_/ /_/\__, /  /_.___/\__,_/____/_/ /_/
                        /____/                            .... is now installed!
Please look over the ~/.bashrc file to select a theme, plugins, completions, aliases, and options
To keep up on the latest news and updates, follow us on GitHub: https://github.com/ohmybash/oh-my-bash
10:58:55 etu@vm0 ~ →
10:59:04 etu@vm0 ~ → echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/snap/bin
11:03:18 etu@vm0 ~ → sudo vim common-session
11:04:29 etu@vm0 ~ → cat common-session
#
# /etc/pam.d/common-session - session-related modules common to all services
#
# This file is included from other service-specific PAM config files,
# and should contain a list of modules that define tasks to be performed
# at the start and end of interactive sessions.
#
# As of pam 1.0.1-6, this file is managed by pam-auth-update by default.
# To take advantage of this, it is recommended that you configure any
# local modules either before or after the default block, and use
# pam-auth-update to manage selection of other modules.  See
# pam-auth-update(8) for details.

# here are the per-package modules (the "Primary" block)
session [default=1]                     pam_permit.so
# here's the fallback if no module succeeds
session requisite                       pam_deny.so
# prime the stack with a positive return value if there isn't one already;
# this avoids us returning an error just because nothing sets a success code
# since the modules above will each just jump around
session required                        pam_permit.so
# and here are more per-package modules (the "Additional" block)
session required        pam_unix.so
session optional        pam_systemd.so
session optional        pam_umask.so
# end of pam-auth-update config
11:04:32 etu@vm0 ~ → umask
0027
11:04:51 etu@vm0 ~ → exit
```
## Afficher les 50 dernières lignes du journal des connexions utilisateur
```Powershell
chehabmosaad@oscar:~$ last -n 50
chehabmo pts/0        2a03:7220:8083:c Tue Dec 12 23:07   still logged in
chehabmo pts/0        2a03:7220:8083:c Tue Dec 12 22:06 - 22:07  (00:00)
pierreha pts/0        2a03:7220:8083:c Tue Dec 12 21:33 - 21:34  (00:01)
chehabmo pts/0        2a03:7220:8083:c Tue Dec 12 21:22 - 21:23  (00:00)
chehabmo pts/0        2a03:7220:8083:c Tue Dec 12 21:21 - 21:22  (00:00)
anthonys pts/0        2a03:7220:8083:c Tue Dec 12 18:44 - 19:22  (00:37)
marieben pts/1        2a03:7220:8083:c Tue Dec 12 17:17 - 17:18  (00:00)
billaich pts/0        2a03:7220:8083:c Tue Dec 12 15:41 - 17:58  (02:16)
billaich pts/0        2a03:7220:8083:c Tue Dec 12 15:39 - 15:41  (00:01)
chehabmo pts/2        2a03:7220:8083:c Tue Dec 12 13:08 - 15:22  (02:13)
chehabmo pts/1        172.16.196.78    Tue Dec 12 12:48 - 15:00  (02:12)
lindabed pts/2        2a03:7220:8083:c Tue Dec 12 10:48 - 13:00  (02:11)
quentino pts/2        2a03:7220:8083:c Tue Dec 12 10:42 - 10:42  (00:00)
lindabed pts/0        2a03:7220:8083:c Tue Dec 12 10:00 - 13:57  (03:57)
hamzazeb pts/1        2a03:7220:8083:c Tue Dec 12 09:03 - 11:57  (02:54)
miniarja pts/0        2a03:7220:8083:c Tue Dec 12 08:50 - 09:11  (00:20)
latu     pts/0        fe80::216:3eff:f Tue Dec 12 06:58 - 06:58  (00:00)
latu     pts/0        fe80::216:3eff:f Tue Dec 12 06:58 - 06:58  (00:00)
mohameds pts/10       2a03:7220:8083:c Mon Dec 11 23:04 - 01:19  (02:15)
mohameds pts/10       2a03:7220:8083:c Mon Dec 11 22:58 - 23:00  (00:01)
mohameds pts/10       2a03:7220:8083:c Mon Dec 11 22:54 - 22:55  (00:01)
mohameds pts/10       2a03:7220:8083:c Mon Dec 11 22:51 - 22:53  (00:02)
mohameds pts/10       2a03:7220:8083:c Mon Dec 11 22:49 - 22:50  (00:01)
mohameds pts/10       2a03:7220:8083:c Mon Dec 11 22:46 - 22:48  (00:01)
amiralab pts/9        2a03:7220:8083:c Mon Dec 11 22:23 - 00:35  (02:12)
amiralab pts/9        2a03:7220:8083:c Mon Dec 11 22:22 - 22:22  (00:00)
lindabed pts/8        2a03:7220:8083:c Mon Dec 11 22:21 - 23:48  (01:26)
lindabed pts/8        2a03:7220:8083:c Mon Dec 11 22:20 - 22:20  (00:00)
amiralab pts/7        2a03:7220:8083:c Mon Dec 11 22:03 - 00:35  (02:32)
amiralab pts/6        2a03:7220:8083:c Mon Dec 11 22:01 - 00:35  (02:34)
amiralab pts/6        2a03:7220:8083:c Mon Dec 11 21:59 - 22:00  (00:00)
amiralab pts/5        2a03:7220:8083:c Mon Dec 11 21:45 - 00:18  (02:32)
nizarsla pts/3        2a03:7220:8083:c Mon Dec 11 21:18 - 23:30  (02:12)
nizarsla pts/3        2a03:7220:8083:c Mon Dec 11 21:15 - 21:17  (00:01)
lindabed pts/5        2a03:7220:8083:c Mon Dec 11 21:12 - 21:16  (00:03)
nizarsla pts/4        2a03:7220:8083:c Mon Dec 11 21:06 - 23:29  (02:23)
nizarsla pts/4        2a03:7220:8083:c Mon Dec 11 21:05 - 21:06  (00:00)
nizarsla pts/4        2a03:7220:8083:c Mon Dec 11 21:03 - 21:05  (00:02)
nizarsla pts/3        2a03:7220:8083:c Mon Dec 11 21:00 - 21:15  (00:14)
nizarsla pts/3        2a03:7220:8083:c Mon Dec 11 20:58 - 21:00  (00:02)
nizarsla pts/3        2a03:7220:8083:c Mon Dec 11 20:57 - 20:58  (00:00)
nizarsla pts/7        2a03:7220:8083:c Mon Dec 11 20:56 - 21:13  (00:17)
lindabed pts/5        2a03:7220:8083:c Mon Dec 11 20:55 - 21:10  (00:14)
nizarsla pts/3        2a03:7220:8083:c Mon Dec 11 20:55 - 20:57  (00:02)
nizarsla pts/1        2a03:7220:8083:c Mon Dec 11 20:54 - 23:34  (02:39)
nizarsla pts/3        2a03:7220:8083:c Mon Dec 11 20:51 - 20:55  (00:03)
nizarsla pts/5        2a03:7220:8083:c Mon Dec 11 20:47 - 20:51  (00:04)
nizarsla pts/4        2a03:7220:8083:c Mon Dec 11 20:47 - 21:03  (00:16)
nizarsla pts/3        2a03:7220:8083:c Mon Dec 11 20:42 - 20:50  (00:07)
nizarsla pts/3        2a03:7220:8083:c Mon Dec 11 20:41 - 20:41  (00:00)

wtmp commence Sun Dec  3 07:10:35 2023
```
# Fin