### Description

## Kioptrix VM Image Challenges:

This Kioptrix VM Image are easy challenges. The object of the game is to acquire root access via any means possible (except actually hacking the VM server or player). The purpose of these games are to learn the basic tools and techniques in vulnerability assessment and exploitation. There are more ways then one to successfully complete the challenges.

![[Pasted image 20240829194617.png]]

![[Pasted image 20240829194559.png]]

![[Pasted image 20240830175134.png]]

![[Pasted image 20240829194810.png]]
![[Pasted image 20240829195239.png]]
![[Pasted image 20240829195016.png]]

![[Pasted image 20240829195426.png]]


-> Con ctrl-u , no hay ningún código ofuscado.

![[Pasted image 20240829200225.png]]

**/manual**
![[Pasted image 20240829203501.png]]


**/usage**
![[Pasted image 20240829213231.png]]


**/mrtg**
![[Pasted image 20240829213344.png]]

-> Con ctrl-u , no hay ningún código ofuscado.
-> Revisado ficheros y no hay nada relevante.

**Vulnerabilidades**

*httml (ports: 80, 443)*
![[Pasted image 20240830180640.png]]
![[Pasted image 20240830185615.png]]

![[Pasted image 20240830181945.png]]

![[Pasted image 20240830190400.png]]

En metasploit no he encontrado la manera de configurar el exploit, por lo que he mirado por google para bajarme el exploit de algún github -> Descargar exploit:
https://github.com/heltonWernik/OpenLuck?source=post_page-----ad8d91e7ed63--------------------------------

![[Pasted image 20240830192624.png]]

No me ha funcionado


*samba (139)*

![[Pasted image 20240830175449.png]]

![[Pasted image 20240830182036.png]]

No podemos conectarnos, y para poder explotar vulnerabilidad tendríamos que saber la versión de SMB:

![[Pasted image 20240830183246.png]]

![[Pasted image 20240830183549.png]]

![[Pasted image 20240830184028.png]]

![[Pasted image 20240830183945.png]]

![[Pasted image 20240830184225.png]]

![[Pasted image 20240830184409.png]]

![[Pasted image 20240830185229.png]]

![[Pasted image 20240830185126.png]]

![[Pasted image 20240830185755.png]]


*Todo a sido por Metasploit, he encontrado recursos que te explican como explotar la vulnerabilidad sin necesidad de Metasploit:*

https://medium.com/@bond-o/vulnhub-kioptrix-level-1-d439aa7039b2

