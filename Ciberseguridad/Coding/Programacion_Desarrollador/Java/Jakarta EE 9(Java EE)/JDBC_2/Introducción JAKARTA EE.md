![[2_jakarta_ee 1.pdf]]

![[Pasted image 20231019163429.png|700]]

![[Pasted image 20231019164143.png]]

Instalaremos la version 10 de apache-tomcat: https://tomcat.apache.org/ (lo descomprimiremos en la misma raiz donde guardaremos la aplicacion webapp).


![[Pasted image 20231019120637.png|500]]

pom.xml :

```java
<?xml version="1.0" encoding="UTF-8"?>  
<project xmlns="http://maven.apache.org/POM/4.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
    <modelVersion>4.0.0</modelVersion>  
  
    <groupId>org.aartaraz.webapp.servlet</groupId>  
    <artifactId>webapp</artifactId>  
    <version>1.0-SNAPSHOT</version>  
    <packaging>war</packaging>  
  
    <properties>        <maven.compiler.source>17</maven.compiler.source>  
        <maven.compiler.target>17</maven.compiler.target>  
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
    </properties>    <dependencies>        <dependency>            <groupId>jakarta.platform</groupId>  
            <artifactId>jakarta.jakartaee-api</artifactId>  
            <version>10.0.0</version>  
            <scope>provided</scope>  
        </dependency>    </dependencies>    <build>        <plugins>            <plugin>                <artifactId>maven-compiler-plugin</artifactId>  
                <version>3.10.1</version>  
            </plugin>            <plugin>                <groupId>org.apache.tomcat.maven</groupId>  
                <artifactId>tomcat7-maven-plugin</artifactId>  
                <version>2.2</version>  
                <configuration>                    <url>http://localhost:8080/manager/text</url>  
                    <username>admin</username>  
                    <password>12345</password>  
                </configuration>            </plugin>            <plugin>                <artifactId>maven-war-plugin</artifactId>  
                <version>3.2.3</version>  
                <configuration>                    <failOnMissingWebXml>false</failOnMissingWebXml>  
                </configuration>            </plugin>        </plugins>    </build>  
  
</project>
```

```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Hola Mundo Tomcat</title>  
</head>  
<body>  
<h>Hola mundo Tomcat</h>  
</body>  
</html>
```

Antes de compilar(run) tendremos que configurar la conpilación en configuraciones:

![[Pasted image 20231019121037.png|600]]

Y desde consola levantar el apache (el diablo):

Ejecutaremos consola y entrando en la ruta de la carpeta apache guardada en el proyecto, dentro de la carpeta ./bin ejecutaremos el comando:

```windows
./startup.bat
```

![[Pasted image 20231019121210.png|800]]

Para cerrar desde el demonio ctrl+c

Ahora estará listo para ejecutar el JSP, Html, etc..

