
https://www.vulnhub.com/ -> Basic Pentesting 2
### Description


This is a boot2root VM and is a continuation of the Basic Pentesting series. This series is designed to help newcomers to penetration testing develop pentesting skills and have fun exploring part of the offensive side of security.

VirtualBox is the recommended platform for this challenge (though it _should_ also work with VMware -- however, I haven’t tested that).

This VM is a moderate step up in difficulty from the first entry in this series. If you’ve solved the first entry and have tried a few other beginner-oriented challenges, this VM should be a good next step. Once again, this challenge contains multiple initial exploitation vectors and privilege escalation vulnerabilities.

Your goal is to remotely attack the VM, gain root privileges, and read the flag located at /root/flag.txt. Once you’ve finished, try to find other vectors you might have missed! If you’d like to send me a link to your writeup, enjoyed the VM or have questions or feedback, feel free to contact me at: josiah@vt.edu

If you finished the VM, please also consider posting a writeup! Writeups help you internalize what you worked on and help anyone else who might be struggling or wants to see someone else’s process. There were lots of wonderful writeups for Basic Pentesting: 1, and I look forward to reading the writeups for this challenge.

-> Desde máquina atacante:
![[Pasted image 20240828163039.png]]
![[Pasted image 20240828163131.png]]

Al estar en red del coworking hay muchas maquinas conectadas, por lo que buscaremos de distinta forma:
![[Pasted image 20240828163502.png]]

![[Pasted image 20240828163651.png]]

![[Pasted image 20240828171131.png]]

![[Pasted image 20240828164148.png]]


![[Pasted image 20240828171409.png]]

-> ctr+u (No obtenemos muchas pistas)
![[Pasted image 20240828171507.png]]

-> Intentamos meter coletillas al final de la dirección:
![[Pasted image 20240828171630.png]]
En este ejemplo no funciona.

![[Pasted image 20240829195818.png]]

![[Pasted image 20240828172252.png]]

-> dev.txt

![[Pasted image 20240828172840.png]]

-> j.txt

![[Pasted image 20240828172923.png]]

-> Ahora tendríamos que conseguir quien ese usuario que empieza por J, y por SSH intentar por fuerza bruta conseguir credenciales. (Por las conversaciones anteriores, al ser muy débil estará en dentro de algún diccionario que usemos)


![[Pasted image 20240828172736.png]]
![[Pasted image 20240828173346.png]]

![[Pasted image 20240828173523.png]]

![[Pasted image 20240828173839.png]]
*Dentro!!*

El archivo descargado:
![[Pasted image 20240828174014.png]]

-> Sabiendo el posible usuario, utilizar hydra (P = listado de contraseñas. Si solo fuera una sola contraseña sería p):

![[Pasted image 20240828174427.png]]

![[Pasted image 20240828175244.png]]

![[Pasted image 20240829181950.png]]

![[Pasted image 20240829182032.png]]
![[Pasted image 20240829182112.png]]


-> **Escalada de privilegios**

![[Pasted image 20240829182431.png]]
![[Pasted image 20240829182551.png]]

![[Pasted image 20240829182616.png]]

Si nos fijamos son la misma clave (La clave pública se encuentra en las authorized_keys, claves autorizadas para conectarse al usuario kay)
![[Pasted image 20240829182755.png]]

![[Pasted image 20240829183013.png]]

Copiamos todo la clave (Desde --BEGIN ....)

->Fuera de ssh en el directorío basicpen2 , crear archivo id_rsa y pegar la clave:
![[Pasted image 20240829183338.png]]

Levantar servidor Python en la máquina comprometida desde el mismo directorio del ssh, y bajartelo con wget o como quieras.
remoto:
```bash
python3 -m http.server 4444
```

atacante:
```bash
wget http://192.168.1.186:4444/id_rsa
```

*Cuando levantamos servidor con python, a veces si intentamos conectarnos por puerto 80, 8080,446 no vamos a poder por tener permisos privilegiado. Por defecto se conecta por el 8000. Cuando tenemos este error probar por otro puerto estilo 4444*

Procederíamos a conectarnos con el usuario jay, pero no sabemos la clave:
![[Pasted image 20240829183537.png]]

Tendremos que crackear hashes con John the Ripper, por lo que primero tendremos que formatear la clave que hemos guardado al formato de John:
![[Pasted image 20240829183909.png]]

![[Pasted image 20240829184520.png]]

![[Pasted image 20240829184833.png]]

![[Pasted image 20240829184932.png]]

-> Intentamos primeramente como root, por si la contraseña es la misma:
![[Pasted image 20240829185041.png]]

-> Como vemos que no funciona, intentaremos conectarnos como usuario kay, ya que está en el grupo:
![[Pasted image 20240829185319.png]]

*Ahora estamos dentro!*
![[Pasted image 20240829185358.png]]

![[Pasted image 20240829185516.png]]

