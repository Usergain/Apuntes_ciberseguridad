
https://www.vulnhub.com/ -> Basic Pentesting
### Description


This is a small boot2root VM I created for my university’s cyber security group. It contains multiple remote vulnerabilities and multiple privilege escalation vectors. I did all of my testing for this VM on VirtualBox, so that’s the recommended platform. I have been informed that it also works with VMware, but I haven’t tested this personally.

This VM is specifically intended for newcomers to penetration testing. If you’re a beginner, you should hopefully find the difficulty of the VM to be just right.

Your goal is to remotely attack the VM and gain root privileges. Once you’ve finished, try to find other vectors you might have missed! If you enjoyed the VM or have questions, feel free to contact me at: josiah@vt.edu

If you finished the VM, please also consider posting a writeup! Writeups help you internalize what you worked on and help anyone else who might be struggling or wants to see someone else’s process. I look forward to reading them!



*Ambas máquinas, tanto máquina atacante como máquina victima tienen que estar en NAT*

-> Máquina atacante:
![[Pasted image 20240827115113.png]]


![[Pasted image 20240827115456.png]]
![[Pasted image 20240827115428.png]]

Máquina Linux:
![[Pasted image 20240827115631.png]]

![[Pasted image 20240827115836.png]]

![[Pasted image 20240827122056.png]]

**----
ftp
----**
-> Intentamos conectarnos por ftp -> annonymous
![[Pasted image 20240827121928.png]]
![[Pasted image 20240827121527.png]]

![[Pasted image 20240827121907.png]]


-> Ahora iremos a buscar exploits:
![[Pasted image 20240827122318.png]]

![[Pasted image 20240827122222.png]]

![[Pasted image 20240827122400.png]]

![[Pasted image 20240827122536.png]]

-> Para copiar el exploit y poder verlo:
![[Pasted image 20240827122716.png]]

-> Le damos un nombre más comprensible:
![[Pasted image 20240827122855.png]]

-> El exploit está programado en C

-> Nos metemos en metasploit:
![[Pasted image 20240827123148.png]]

![[Pasted image 20240827123317.png]]

![[Pasted image 20240827123429.png]]

![[Pasted image 20240827123502.png]]

![[Pasted image 20240827123605.png]]

![[Pasted image 20240827123756.png]]

-> Revisar el payload configurado:
![[Pasted image 20240827123924.png]]

![[Pasted image 20240827124053.png]]

-> IP máquina atacante:
![[Pasted image 20240827124248.png]]

![[Pasted image 20240827124357.png]]

-> Para ver los payloads sin necesidad de autocompletar:
![[Pasted image 20240827124534.png]]

![[Pasted image 20240827124651.png]]

![[Pasted image 20240827124842.png]]
![[Pasted image 20240827124910.png]]

![[Pasted image 20240827125203.png]]

Listar todo lo que tengan en carpetas de forma recursiva "*ls -R*"



**-----
ssh
-----**

Muchos exploits pero ninguno funciona bien menos el 40136:
![[Pasted image 20240827130526.png]]

Para ver info:
![[Pasted image 20240827130901.png]]
Para copiar sería con -m
![[Pasted image 20240827131058.png]]

![[Pasted image 20240827131151.png]]

-> National Institute of Standard and Technology (NIST)
-> Repositorios en github
-> Repositorios en Rapid7 (creadores de Metasploit)


-> Ahora nos metemos en metasploit:
![[Pasted image 20240827131734.png]]

![[Pasted image 20240827131759.png]]

![[Pasted image 20240827131908.png]]
![[Pasted image 20240827131944.png]]

Para solo listar las opciones modificables:
![[Pasted image 20240827132042.png]]

![[Pasted image 20240827132324.png]]

![[Pasted image 20240827155922.png]]

![[Pasted image 20240827155940.png]]

Para probar una lista de usuarios se utilizaría:
![[Pasted image 20240827160122.png|700]]


**-------
httpd
-------**
![[Pasted image 20240827160509.png]]

Por exploits en http parece que no hay nada útil

http://192.168.1.53
![[Pasted image 20240827161832.png]]

ctr+U (ver si hay algún comentario en el código fuente de la máquina):
![[Pasted image 20240827161953.png]]

Ver si hay algún archivo interesante:
http://192.168.1.53/robots.txt (archivos que indican a los navegadores que algunas direcciones aparezcan en la búsqueda)

![[Pasted image 20240827162217.png]]

Ahora pasaremos a la búsqueda activa:
![[Pasted image 20240827162626.png]]


https://192.168.1.53/secret/
![[Pasted image 20240827162833.png]]

No es el caso

*A veces al estar direccionada a una u otra página, según. Se puede presentar problemas* -> vtcsec
![[Pasted image 20240827163054.png]]

![[Pasted image 20240827163123.png]]

![[Pasted image 20240827163248.png]]

![[Pasted image 20240827163313.png]]

![[Pasted image 20240827163358.png]]
http
![[Pasted image 20240827163506.png]]

Al ser un wordpress utilizaremos esta herramienta (es de pago pero si son pocas consultas es gratuita):
Antes hay que instalar todas las dependencias necesarias:

```bash
# Intenta forzar la desactualización del paquete libcurl4 para resolver las dependencias
sudo apt-get install -y libcurl4=7.88.1-10+deb12u7 --allow-downgrades

# Después, intenta instalar el paquete libcurl4-openssl-dev nuevamente
sudo apt-get install -y libcurl4-openssl-dev

# Si el problema persiste, intenta corregir las dependencias rotas
sudo apt-get install -f

# Ahora, procede con la instalación de wpscan
sudo gem install wpscan

# Finalmente, verifica que wpscan se haya instalado correctamente
wpscan --version

```


![[Pasted image 20240827165043.png]]
![[Pasted image 20240827165111.png]]

Ya sabiendo que hay un usuario admin (Esto lo sabe por búsqueda  de publicaciones)
![[Pasted image 20240827165339.png]]
![[Pasted image 20240827183332.png]]

![[Pasted image 20240827183510.png]]


![[Pasted image 20240827185728.png]]


-> Ahora ejecutaremos comandos de forma remota en wordpress, pero primero lo haremos por metasploit:

![[Pasted image 20240827191332.png]]
![[Pasted image 20240827191400.png]]

![[Pasted image 20240827191449.png]]

![[Pasted image 20240827191702.png]]

![[Pasted image 20240827191806.png]]


Este Payload está por defecto y no funciona en este servidor, lo cambiaremos por algo más normalito -> shell:
![[Pasted image 20240827191938.png]]

![[Pasted image 20240827192142.png]]

Cambiado:
![[Pasted image 20240827192219.png]]

Ya estamos dentro com www-data -> En Wordpress es el usuario que se utiliza para levantar el servicio con Apache, tiene permisos mínimos -> *habrá que ir escalando privilegios.*


Ahora lo haremos desde el panel de wordpress, sin ayuda de metasploit:
![[Pasted image 20240827193116.png]]

![[Pasted image 20240827193202.png]]

*Aquí inyectaríamos código malicioso*
tema del codio 404 de wordpress:
![[Pasted image 20240827193503.png]]

Escribimos en otro navegador (si al escribir la pantalla sale en blanco sin ningún error es que existe):
![[Pasted image 20240827193649.png]]

![[Pasted image 20240827194314.png]]
![[Pasted image 20240827194339.png]]

![[Pasted image 20240827194420.png]]
![[Pasted image 20240827194431.png]]


![[Pasted image 20240827194634.png]]
Aqui seguido del ? le añadiriamos el comando que queremos ejecutar:
![[Pasted image 20240827194802.png]]

La máquina tiene netcat:
![[Pasted image 20240827194913.png]]

Poner la maquina atacante en escucha:

![[Pasted image 20240827200143.png]]


Visitar página web -> www.revshells.com

Aquí ir jugando con los parámetros y raw/URL Encode
![[Pasted image 20240827200057.png]]


![[Pasted image 20240827202239.png]]

Ya estariamos dentro:
![[Pasted image 20240827202208.png]]