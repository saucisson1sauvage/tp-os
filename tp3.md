
### 🌞 Afficher la ligne du fichier qui concerne votre utilisateur
```powershell
me@Debian11:/$ grep "me:" /etc/passwd
me:x:1000:1000:me,,,:/home/me:/bin/bash
```

### 🌞 Afficher la ligne du fichier qui concerne votre utilisateur ET celle de root en même temps

```powershell

me@Debian11:/$ grep -e "me:" -e "root:" /etc/passwd
root:x:0:0:root:/root:/bin/bash
me:x:1000:1000:me,,,:/home/me:/bin/bash
```

### 🌞 Afficher la liste des groupes d'utilisateurs de la machine


```powershell
me@Debian11:/$ cat /etc/group | cut -d":" -f1
root
daemon
bin
sys
adm
tty
disk
lp
mail
news
uucp
man
proxy
kmem
dialout
fax
voice
cdrom
```


### 🌞 Afficher la ligne du fichier qui concerne votre utilisateur ET celle de root en même temps

```powershell 
me@Debian11:/$ cat /etc/passwd | cut -d":" -f1,6 | grep -e "me:" -e "root:"
root:/root
me:/home/me
```

### 🌞 Afficher la ligne qui contient le hash du mot de passe de votre utilisateur

```powershell
me@Debian11:/$ sudo cat /etc/shadow  | grep "me:"
me:$y$j9T$9jt.uwXQ9njtXmPQmgrmv0$44JIJMmyxvjiIpZ.bIxEmoPQe.yUcIM0OAqb6OTiLCB:20033:0:99999:7:::
```

### 🌞 Faites en sorte que votre utilisateur puisse taper n'importe quelle commande sudo
j'y suis deja

### 🌞 Créer un groupe d'utilisateurs

```powershell
me@Debian11:/etc$ cat ./group | grep "stronk_admins:x:1001:"
stronk_admins:x:1001:
```

### 🌞 Créer un utilisateur

```powershell
me@Debian11:/$ sudo useradd imbo
me@Debian11:/$ sudo passwd imbob
New password:
Retype new password:
passwd: password updated successfully
me@Debian11:/$ sudo usermod -a -G stronk_admins imbob
```

### 🌞 Prouver que l'utilisateur imbob est créé

```powershell
me@Debian11:/$ grep "imbob:" /etc/passwd
imbob:x:1001:1002::/home/imbob:/bin/sh
```

### 🌞 Prouver que l'utilisateur imbob a un password défini

```powershell
me@Debian11:/$ sudo cat /etc/shadow  | grep "imbob:"
imbob:$y$j9T$j4eztGSWnGiJjy34iddAk.$62LKp6QcmF2Mk7cvaZAmznn.LTzU2NxNPOoZmZI8527:20040:0:99999:7:::
```

### 🌞 Prouver que l'utilisateur imbob appartient au groupe stronk_admins

```powershell
me@Debian11:/$ sudo cat /etc/group | grep "stronk"
stronk_admins:x:1001:imbob
```

### 🌞 Créer un deuxième utilisateur

```powershell
me@Debian11:/$ sudo useradd imnotbobsorry
me@Debian11:/$ sudo passwd imnotbobsorry
New password:
Retype new password:
passwd: password updated successfully
me@Debian11:/$ sudo usermod -a -G stronk_admins imnotbobsorry
```

### 🌞 Modifier la configuration de sudo pour que

```
# User privilege specification
root    ALL=(ALL:ALL) ALL
me    ALL=(ALL:ALL) ALL
imbob ALL=(ALL) ALL
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
%stronk_admins ALL= /usr/bin/apt, /usr/bin/apt-get
# See sudoers(5) for more information on "@include" directives:
```
### 🌞 Créer le dossier /home/goodguy
```powershell
me@Debian11:/$ sudo mkdir /home/goodguy
```


### 🌞 Créer le dossier /home/badguy
```powershell
me@Debian11:/$ sudo mkdir /home/badguy
```

### 🌞 Changer le répertoire personnel de imbob
```powershell
me@Debian11:/$ sudo usermod -d /home/goodguy imbob
me@Debian11:/$ sudo cat /etc/passwd | grep "imbob"
imbob:x:1001:1002::/home/goodguy:/bin/sh
```
### 🌞 Changer le répertoire personnel de imnotbobsorry

```powershell
me@Debian11:/$ sudo usermod -d /home/badguy imnotbobsorry
me@Debian11:/$ sudo cat /etc/passwd | grep "notbob"
imnotbobsorry:x:1002:1003::/home/badguy:/bin/sh
```

### 🌞 Prouver que les permissions du dossier /home/gooduy sont incohérentes

```powershell
me@Debian11:/$ sudo ls -ld /home/goodguy
drwxr-xr-x 2 root root 4096 Nov 13 12:40 /home/goodguy
```

### 🌞 Modifier les permissions de /home/goodguy
```powershell
me@Debian11:/$ sudo chown -R imbob:imbob /home/goodguy
```

### 🌞 Modifier les permissions de /home/badguy

```powershell
me@Debian11:/$ sudo chown -R imnotbobsorry:imnotbobsorry /home/badguy
```

### 🌞 Connectez-vous sur l'utilisateur imbob

```powershell
me@Debian11:/$ su - imbob
Password:
$ pwd
/home/goodguy
$ sudo echo meow
meow
```

### 🌞 Connectez-vous sur l'utilisateur imnotbobsorry

```powershell
me@Debian11:/$ su - imnotbobsorry
Password:
$ pwd
/home/badguy
$ sudo echo meow

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for imnotbobsorry:
Sorry, user imnotbobsorry is not allowed to execute '/usr/bin/echo meow' as root on Debian11.
$ sudo apt update
[sudo] password for imnotbobsorry:
Get:1 http://security.debian.org/debian-security bullseye-security InRelease [27.2 kB]
Hit:2 http://deb.debian.org/debian bullseye InRelease
Get:3 http://deb.debian.org/debian bullseye-updates InRelease [44.1 kB]
Get:4 http://security.debian.org/debian-security bullseye-security/main Sources [213 kB]
Get:5 http://security.debian.org/debian-security bullseye-security/main amd64 Packages [308 kB]
Get:6 http://security.debian.org/debian-security bullseye-security/main Translation-en [198 kB]
Fetched 790 kB in 1s (1,432 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
3 packages can be upgraded. Run 'apt list --upgradable' to see them.
$
```
### 🌞 Affichez les processus bash


```powershell
me@Debian11:/$ ps -ef | grep "bash"
me           883     882  0 Nov13 pts/0    00:00:01 -bash
me          3301     883  0 08:50 pts/0    00:00:00 grep bash
```

### 🌞 Affichez tous les processus lancés par votre utilisateur 

```powershell
me@Debian11:/$ ps -ef | grep "^me"
message+     392       1  0 Nov13 ?        00:00:09 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
me           643       1  0 Nov13 ?        00:00:00 /lib/systemd/systemd --user
me           644     643  0 Nov13 ?        00:00:00 (sd-pam)
me           658     643  0 Nov13 ?        00:00:00 /usr/bin/pipewire
me           659     643  0 Nov13 ?        00:00:00 /usr/bin/pulseaudio --daemonize=no --log-target=journal
me           662     643  0 Nov13 ?        00:00:37 /usr/bin/dbus-daemon --session --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
me           664     658  0 Nov13 ?        00:00:00 /usr/bin/pipewire-media-session
me           669     638  0 Nov13 ?        00:00:00 xfce4-session
me           712     669  0 Nov13 ?        00:00:01 /usr/bin/ssh-agent x-session-manager
```

### 🌞 Affichez le top 5 des processus qui utilisent le plus de RAM

```powershell
me@Debian11:/$ ps -eo comm | head -n 6
COMMAND
systemd
kthreadd
rcu_gp
rcu_par_gp
kworker/0:0H-events_highpri
```

### 🌞 Affichez le PID du processus du service SSH

```powershell
me@Debian11:/$ ps -eo comm,pid | grep "sshd"
sshd                554
sshd                876
sshd                882
```

### 🌞 Affichez le nom du processus avec l'identifiant le plus petit

```powershell
me@Debian11:/$ ps -eo comm,pid --sort=pid |  head -n 2
COMMAND             PID
systemd               1
```

### 🌞 Déterminer le PID de votre shell actuel

```powershell

me@Debian11:/$ ps -eo comm,pid  | grep "bash" | head -n 1
bash                883

```

### 🌞 Déterminer le PPID de votre shell actuel

```powershell

me@Debian11:/$ ps -eo comm,pid,ppid  | grep "bash"
bash                883     882
```

### 🌞 Déterminer le nom de ce processus

```powershell

me@Debian11:/$ ps -eo comm,pid,ppid  | grep "882"
sshd                882     876
bash                883     882

```

### 🌞 Lancer un processus sleep 9999 en tâche de fond


```powershell
me@Debian11:/$ ps -eo comm,pid,ppid  | grep "sleep"
sleep              3436     883
```
### 🌞 Fermez votre session SSH

```powershell

me@Debian11:/$ exit
logout
There are stopped jobs.
me@Debian11:/$ exit
logout
Connection to 192.168.13.102 closed.
PS C:\Users\philipon gouin matys> ssh  me@192.168.13.102
me@192.168.13.102 s password:
Linux Debian11 5.10.0-33-amd64 #1 SMP Debian 5.10.226-1 (2024-10-03) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Nov 13 16:03:25 2024 from 192.168.13.2
me@Debian11:~$ ps -eo comm,pid,ppid  | grep "sleep"
sleep              3436       1
```
### 🌞 S'assurer que le service ssh est démarré

```powershell
me@Debian11:~$ systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2024-11-13 09:42:25 CET; 1 day 23h ago
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 547 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
   Main PID: 554 (sshd)
      Tasks: 1 (limit: 2166)
     Memory: 4.3M
        CPU: 365ms
     CGroup: /system.slice/ssh.service
             └─554 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
```
### 🌞 Isolez la ligne qui indique le nom du programme lancé

```powershell

me@Debian11:~$ systemctl status | grep "ssh"
           │   │ ├─712 /usr/bin/ssh-agent x-session-manager
           │   │ ├─3441 sshd: me [priv]
           │   │ ├─3447 sshd: me@pts/1
           │   │ └─3479 grep ssh
             ├─ssh.service
             │ └─554 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
```

### 🌞 Déterminer le port sur lequel écoute le service SSH

```powershell

me@Debian11:~$ sudo ss -lnpt | grep "sshd"
LISTEN 0      128          0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=554,fd=3))
LISTEN 0      128             [::]:22           [::]:*    users:(("sshd",pid=554,fd=4))

```

### 🌞 Consulter les logs du service SSH

```powershell

me@Debian11:~$ journalctl | grep "ssh"
Hint: You are currently not seeing messages from other users and the system.
      Users in groups 'adm', 'systemd-journal' can see all messages.
      Pass -q to turn off this notice.
Nov 06 18:18:35 Debian11 systemd[669]: Listening on GnuPG cryptographic agent (ssh-agent emulation).
Nov 06 18:18:35 Debian11 gpg-agent[771]: using fd 3 for ssh socket (/run/user/1000/gnupg/S.gpg-agent.ssh)
Nov 06 18:18:35 Debian11 gpg-agent[771]: listening on: std=6 extra=4 browser=5 ssh=3
Nov 06 18:18:35 Debian11 gpg-agent[774]: using fd 3 for ssh socket (/run/user/1000/gnupg/S.gpg-agent.ssh)
Nov 06 18:18:35 Debian11 gpg-agent[774]: listening on: std=6 extra=4 browser=5 ssh=3
Nov 07 08:07:28 Debian11 systemd[594]: Listening on GnuPG cryptographic agent (ssh-agent emulation).
Nov 07 08:07:28 Debian11 gpg-agent[695]: using fd 5 for ssh socket (/run/user/1000/gnupg/S.gpg-agent.ssh)
Nov 07 08:07:28 Debian11 gpg-agent[695]: listening on: std=4 extra=3 browser=6 ssh=5
```

### 🌞 Identifier le fichier de configuration du serveur SSH

```powershell

me@Debian11:~$ ls -l /etc/ssh/sshd_config
-rw-r--r-- 1 root root 3289 Dec 21  2023 /etc/ssh/sshd_config

```

### 🌞 Modifier le fichier de conf

```powershell

me@Debian11:~$ cat /etc/ssh/sshd_config | grep "Port"
#Port 1550
```

### 🌞 Redémarrer le service
```powershell
me@Debian11:~$ systemctl restart sshd
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to restart 'ssh.service'.
Authenticating as: me,,, (me)
Password:
==== AUTHENTICATION COMPLETE ===
```
### 🌞 Effectuer une connexion SSH sur le nouveau port
```powershell

PS C:\Users\philipon gouin matys> ssh -p 1550 me@192.168.13.102
me@192.168.13.102's password:
Linux Debian11 5.10.0-33-amd64 #1 SMP Debian 5.10.226-1 (2024-10-03) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Nov 15 09:19:52 2024 from 192.168.13.2
```

### 🌞 Trouver le fichier ssh.service

```powershell

me@Debian11:~$ sudo find / -name "ssh.service"
/etc/systemd/system/multi-user.target.wants/ssh.service
/usr/share/doc/avahi-daemon/examples/ssh.service
/usr/lib/systemd/system/ssh.service
/sys/fs/cgroup/system.slice/ssh.service
/var/lib/systemd/deb-systemd-helper-enabled/multi-user.target.wants/ssh.service

``` 

### 🌞 Déterminer quel est le programme lancé

```powershell
me@Debian11:~$ grep "ExecStart=" /lib/systemd/system/ssh.service
ExecStart=/usr/sbin/sshd -D $SSHD_OPTS
```    

### 🌞 Déterminer le dossier qui contient la commande python3

```powershell

me@Debian11:~$ sudo find / -name python3
/etc/python3
/usr/share/bash-completion/completions/python3
/usr/share/lintian/overrides/python3
/usr/share/python3
/usr/share/doc/python3
/usr/bin/python3
/usr/lib/python3
```
### 🌞 Démarrez votre service

```powershell
sudo systemctl start meow_web
```

### 🌞 Assurez-vous que le service meow_web est actif

```powershell
me@Debian11:~$ sudo systemctl status meow_web
● meow_web.service - Super serveur web MEOW
     Loaded: loaded (/etc/systemd/system/meow_web.service; disabled; vendor preset: enabled)
     Active: active (running) since Fri 2024-11-15 11:12:34 CET; 6s ago
   Main PID: 3775 (python3)
      Tasks: 1 (limit: 2166)
     Memory: 8.2M
        CPU: 74ms
     CGroup: /system.slice/meow_web.service
             └─3775 /usr/bin/python3 -m http.server 8888

Nov 15 11:12:34 Debian11 systemd[1]: Started Super serveur web MEOW.
```


### 🌞 Déterminer le PID du processus Python en cours d'exécution

```powershell
me@Debian11:~$ ps -eo pid,user,cmd | grep "python3 -m http.server 8888"
   3775 root     /usr/bin/python3 -m http.server 8888
   3796 me       grep python3 -m http.server 8888
```

### 🌞 Prouvez que le programme écoute derrière le port 8888

```powershell
me@Debian11:~$ sudo ss -lnpt | grep ":8888"
LISTEN 0      5            0.0.0.0:8888      0.0.0.0:*    users:(("python3",pid=3775,fd=3))
```

### 🌞 Faire en sorte que le service se lance automatiquement au démarrage de la machine

```powershell

me@Debian11:~$ sudo systemctl enable meow_web
Created symlink /etc/systemd/system/multi-user.target.wants/meow_web.service → /etc/systemd/system/meow_web.service.

```
