
![[sql.pdf]]
En MySQL Workbench:

Lo Primero será crear una BBDD, crear nombre, etc:

![[Pasted image 20231012185036.png]]

Crearemos dos tablas en este ejemplo con cardinalidad 1-n, tabla productos y tabla categorias:

![[Pasted image 20231012185204.png]]
![[Pasted image 20231012185403.png]]
![[Pasted image 20231012185611.png]]

Ahora anidaremos la clave primaria de categorias como clave foranea en productos asi estarán integradas las dos tablas:

![[Pasted image 20231012190046.png]]

![[Pasted image 20231012190238.png]]

A mi personalmente asi no me ha salido asi que hay otras dos maneras de relacionar tablas o incluso crearlas:

1) Con sentencias:

![[Pasted image 20231012191854.png]]

2) Con diagrama de flechas:

![[Pasted image 20231012184548.png]]
Se representará esto:

![[Pasted image 20231012184341.png]]

