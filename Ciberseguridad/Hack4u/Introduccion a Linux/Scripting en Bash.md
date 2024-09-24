*-----------------------*
*Comandos básicos I*
*-----------------------*

[https://ciberninjas.com/chuleta-comandos-linux/](https://ciberninjas.com/chuleta-comandos-linux/)

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

-------
ctr+l -> para borrar la pantalla, sin hacer clear y mantener lo que estas escribiendo
------
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

```

*-----------------------*
*Comandos básicos II*
*-----------------------*
- Comandos básicos de Linux: [https://www.bonaval.com/kb/cheats-chuletas/comandos-basicos-linux](https://www.bonaval.com/kb/cheats-chuletas/comandos-basicos-linux)
- Guía en PDF de comandos básicos de Linux:
![[comandos basicos Linux.pdf]]


```linux
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

```

*-------------------------------------------------*
*Control de flujo stderr-stdout, operadores y*
*procesos en segundo plano*
*-------------------------------------------------*
![[bash-redirections-cheat-sheet.pdf]]

