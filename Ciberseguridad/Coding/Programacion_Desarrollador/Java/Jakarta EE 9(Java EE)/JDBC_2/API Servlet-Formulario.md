```xml
<?xml version="1.0" encoding="UTF-8"?>  
<project xmlns="http://maven.apache.org/POM/4.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
    <modelVersion>4.0.0</modelVersion>  
  
    <groupId>org.aartaraz.apiservlet.webapp.form</groupId>  
    <artifactId>webapp-form</artifactId>  
    <version>1.0-SNAPSHOT</version>  
  
    <packaging>war</packaging>
    </properties>  
        <maven.compiler.source>17</maven.compiler.source>  
        <maven.compiler.target>17</maven.compiler.target>  
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
        <project.reportingg.outputEncoding>UTF-8</project.reportingg.outputEncoding>
    </properties>
      
	<dependencies>                                                                                                                                                                                            
		<dependency>
			<groupId>jakarta.platform</groupId>  
            <artifactId>jakarta.jakartaee-api</artifactId>  
            <version>10.0.0</version>  
            <scope>provided</scope>  
		</dependency>                                                                                                                                                                                     
	</dependencies>
	<build>
		<finalName>${project.artifactId}</finalName>  
        <plugins>            
	        <plugin>                                                                                                                                                   
		        <artifactId>maven-compiler-plugin</artifactId>  
                <version>3.10.1</version>  
            </plugin>            
            <plugin>                
                <groupId>org.apache.tomcat.maven</groupId>  
                <artifactId>tomcat7-maven-plugin</artifactId>  
                <version>2.2</version>  
                <configuration>                    
	                <url>http://localhost:8080/manager/text</url>  
	                <username>admin</username>  
	                <password>12345</password>  
                </configuration>            
            </plugin>            
            <plugin>                
                <artifactId>maven-war-plugin</artifactId>  
                <version>3.2.3</version>  
                <configuration>                    
	                <failOnMissingWebXml>false</failOnMissingWebXml>  
                </configuration>            
            </plugin>        
        </plugins>    
    </build>  
</project>
```

Copiaremos el POM, con las dependencias instaladas el proyecto anterior (JavaEE -webapp) y crearemos un template(plantilla):

![[Pasted image 20231020112220.png|850]]

Define->Maven->Aplay->OK

![[Pasted image 20231020112722.png|300]]

![[Pasted image 20231024160415.png|350]]

Cuando estemos implementado el index.html, para recibir sugerencia de que hacer (Sorround with)-> ctrl+alt+t :
Anidar con etiqueta "div" por poner un ejemplo.
Metodo Post (FormDate dentro del body):

```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Formulario de usuarios</title>  
</head>  
<body>  
<h3>Formulario de usuarios</h3>  
<form action="/webapp-form/registro" method="post">  
    <div>        
	    <label for="username">Usuario</label>  
	        <div>
		        <input type="text" name="username" id="username">
		    </div>  
    </div>    
	<div>        
	    <label for="password">Password</label>  
	        <div>
		        <input type="password" name="password" id="password">
		    </div>  
    </div>    
    <div>        
	    <label for="email">Email</label>  
	        <div>
				<input type="text" name="email" id="email">
			</div>  
    </div>    
    <div>        
	    <div>            
		    <input type="submit" vaue="Enviar">  
	    </div>    
	</div>  
</form>  
</body>  
</html>
```

```java
package org.aartaraz.apiservlet.webapp.form;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
  
import java.io.IOException;  
import java.io.OutputStream;  
import java.io.PrintWriter;  
  
@WebServlet("/registro")  
public class FormServlet extends HttpServlet {  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        resp.setContentType("text/html");  
  
        String username = req.getParameter("username");  
        String password = req.getParameter("password");  
        String email = req.getParameter("email");  
  
        try (PrintWriter out = resp.getWriter()) {  
  
            out.println("<!DOCTYPE html");  
            out.println("<html>");  
            out.println("    <head>");  
            out.println("        <meta charset=\"UTF-8\">");  
            out.println("        <title>Resultado Form</title>");  
            out.println("    </head>");  
            out.println("    <body>");  
            out.println("        <h1>Resultado Form</h1>");  
  
            out.println("        <ul>");  
            out.println("            <li>UserName: " + username + "</li>");  
            out.println("            <li>Password: " + password + "</li>");  
            out.println("            <li>Email: " + email + "</li>");  
            out.println("        </ul>");  
            out.println("    </body>");  
            out.println("</html>");  
        }  
    }  
}
```


![[Pasted image 20231020124017.png|500]]

![[Pasted image 20231020123950.png|500]]

En cambio con metodo Get:
En html:
![[Pasted image 20231020124717.png|400]]
En Servlet java:
![[Pasted image 20231020124600.png|1200]]

![[Pasted image 20231020124853.png|900]]

Ahora trataremos con los errores:

```java
package org.aartaraz.apiservlet.webapp.form;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
  
import java.io.IOException;  
import java.io.OutputStream;  
import java.io.PrintWriter;  
import java.util.*;  
  
@WebServlet("/registro")  
public class FormServlet extends HttpServlet {  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        resp.setContentType("text/html");  
  
        String username = req.getParameter("username");  
        String password = req.getParameter("password");  
        String email = req.getParameter("email");  
        String pais = req.getParameter("pais");  
        String[] lenguajes = req.getParameterValues("lenguajes");  
        String[] roles = req.getParameterValues("roles");  
        String idioma = req.getParameter("idioma");  
        boolean habilitar = req.getParameter("habilitar") != null &&  
                req.getParameter("habilitar").equals("on");  
        String secreto = req.getParameter("secreto");  
        Map<String, String> errores = new HashMap<>();  
  
        /*Validacion del formulario*/  
        if (username == null || username.isBlank()) {  
            errores.put("username", "el username es requerido!");  
        }  
  
        if (password == null || password.isBlank()) {  
            errores.put("password", "el password es requerido!");  
        }  
  
        if (email == null || !email.contains("@")) {  
            errores.put("email","el email es requerido y debe tener formato de correo.");  
        }  
  
        if (pais == null || pais.equals("") || pais.equals(" ")) { //Lo mismo que el .isBlank  
            errores.put("pais","el pais es requerido!");  
        }  
  
        if (lenguajes == null || lenguajes.length == 0) {  
            errores.put("lenguajes","Debes seleccionar al menos un tema!");  
        }  
  
        if (roles == null || roles.length == 0) {  
            errores.put("roles","Debes seleccionar al menos un role!");  
        }  
  
        if (idioma == null) {  
            errores.put("idioma","debe seleccionar una check");  
        }  
  
        //Printeo en pantalla del formulario:  
  
        if (errores.isEmpty()) {  
  
            try (PrintWriter out = resp.getWriter()) {  
  
                out.println("<!DOCTYPE html");  
                out.println("<html>");  
                out.println("    <head>");  
                out.println("        <meta charset=\"UTF-8\">");  
                out.println("        <title>Resultado Form</title>");  
                out.println("    </head>");  
                out.println("    <body>");  
                out.println("        <h1>Resultado Form</h1>");  
                out.println("        <ul>");  
                out.println("            <li>UserName: " + username + "</li>");  
                out.println("            <li>Password: " + password + "</li>");  
                out.println("            <li>Email: " + email + "</li>");  
                out.println("            <li>Pais: " + pais + "</li>");  
                out.println("            <li>Lenguajes: <ul>");  
                Arrays.asList(lenguajes).forEach(lemguaje -> {  
                    out.println("                <li>" + lemguaje + "</li>");  
                });  
                out.println("            </ul></li>");  
  
                out.println("            <li>Roles: <ul>");  
                Arrays.asList(roles).forEach(role -> {  
                    out.println("                <li>" + role + "</li>");  
                });  
                out.println("            </ul></li>");  
                out.println("            <li>Idioma: " + idioma + "</li>");  
                out.println("            <li>Habilitado: " + habilitar + "</li>");  
                out.println("            <li>Secreto: " + secreto + "</li>");  
                out.println("        </ul>");  
                out.println("    </body>");  
                out.println("</html>");  
            }  
        } else {  
                /*errores.forEach(error -> {  
                    out.println("<li>" + error + "</li>");                });                out.println("<p><a href=\"/webapp-form/index.jsp\">volver</a></p>");*/            req.setAttribute("errores", errores);  
            getServletContext().getRequestDispatcher("/index.jsp").forward(req, resp);  
        }  
    }  
}
```

Estilo Bootstrap:
1- Iremos a la página oficial de Bootstrap5 : https://getbootstrap.com/ 

![[Pasted image 20231024161520.png]]

Copiaremos este enlace y copiarlo en el navegador para luego guardarlo en el proyecto:

![[Pasted image 20231024161554.png]]

Boton derecho y guardar como en la raiz del proyecto JavaEE, además copiar y guardar en carpeta webapp -> css del proyecto.
Para darle a cada etiqueta sin tener que estar repitiendo (ALT+SHIFT+CLICK RATON) asi asignaremos en todas las etiquetas. Claro que estamos asignado en la misma hoja JSP, el enlace a Bootstrap5 lo guardaremos fuera.

```jsp
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Formulario de usuarios</title>  
    <link href="<%=request.getContextPath()%>/css/bootstrap.min.css" rel="stylesheet">  
</head>  
<body>  
<h3>Formulario de usuarios</h3>  
  
<%  
    if (errores != null && errores.size() > 0) {  
%>  
<ul class="alert alert-danger mx-5 px-5">  
    <% for (String error : errores.values()) {%>  
    <li><%=error %>  
    </li><!--Expresiones jsp, sin tener que imprimir con java-->  
    <%}%>  
</ul>  
<%}%>  
<div class="px-5">  
    <form action="/webapp-form/registro" method="post">  
        <div class="row mb-3">  
            <label for="username" class="col-form-label col-sm-2">Usuario</label>  
            <div class="col-sm-4">  
                <input type="text" name="username" id="username" class=form-control" value="${param.username}"></div>  
  
            <%  
                if (errores != null && errores.containsKey("username")) {  
                    out.println("<small class='row mb-3 alert alert-danger' style='color:red;'>" + errores.get("username") + "</small>");  
                }            %>        </div>  
  
        <div class="row mb-3">  
            <label for="password" class="col-form-label col-sm-2">Password</label>  
            <div class="col-sm-4"><input type="password" name="password" id="password" class=form-control"></div>  
            <%  
                if (errores != null && errores.containsKey("password")) {  
                    out.println("<small class='alert alert-danger' style='color:red;'>" + errores.get("password") + "</small>");  
                }            %>        </div>  
  
        <div class="row mb-3">  
            <label for="email" class="col-form-label col-sm-2">Email</label>  
            <div class="col-sm-4"><input type="text" name="email" id="email" class=form-control" value="${param.email}">  
            </div>  
            <%  
                if (errores != null && errores.containsKey("email")) {  
                    out.println("<small class='alert alert-danger' style='color:red;'>" + errores.get("email") + "</small>");  
                }            %>        </div>  
  
        <div class="row mb-3">  
            <label for="pais" class="col-form-label col-sm-2">País</label>  
            <div class="col-sm-4">  
                <select name="pais" id="pais" class="form-select">  
                    <option value="">-- seleccionar --</option>  
                    <option value="ES" ${param.pais.equals("ES")?"selected": ""}>España</option>  
                    <option value="MX" ${param.pais.equals("MX")?"selected": ""}>México</option>  
                    <option value="CL" ${param.pais.equals("CL")?"selected": ""}>Chile</option>  
                    <option value="AR" ${param.pais.equals("<AR>")?"selected": ""}>Argentina</option>  
                    <option value="PE" ${param.pais.equals("PE")?"selected": ""}>Perú</option>  
                    <option value="CO" ${param.pais.equals("CO")?"selected": ""}>Colombia</option>  
                    <option value="VE" ${param.pais.equals("VE")?"selected": ""}>Venezuela</option>  
                </select>  
            </div>  
            <%  
                if (errores != null && errores.containsKey("pais")) {  
                    out.println("<small class='alert alert-danger' style='color:red;'>" + errores.get("pais") + "</small>");  
                }            %>        </div>  
  
        <div class="row mb-3">  
            <label for="lenguajes" class="col-form-label col-sm-2">Lenguajes de programación</label>  
            <div class="col-sm-4">  
                <select name="lenguajes" id="lenguajes" multiple class="form-select">  
                    <option value="java" ${paramValues.lenguajes.stream().anyMatch(v->v.equals("java")).get()?"selected":""} >  
                        Java SE  
                    </option>  
                    <option value="jakartaee" ${paramValues.lenguajes.stream().anyMatch(v->v.equals("jakartaee")).get()?"selected":""} >  
                        JaKARTA EE9  
                    </option>  
                    <option value="spring" ${paramValues.lenguajes.stream().anyMatch(v->v.equals("spring")).get()?"selected":""} >  
                        SpringBoot  
                    </option>  
                    <option value="angular"${paramValues.lenguajes.stream().anyMatch(v->v.equals("angular")).get()?"selected":""} >  
                        Angular  
                    </option>  
                    <option value="react"${paramValues.lenguajes.stream().anyMatch(v->v.equals("react")).get()?"selected":""} >  
                        React  
                    </option>  
                </select>  
            </div>  
            <%  
                if (errores != null && errores.containsKey("lenguajes")) {  
                    out.println("<small class='alert alert-danger' style='color:red;'>" + errores.get("lenguajes") + "</small>");  
                }            %>        </div>  
  
        <div class="row mb-3">  
            <label class="col-form-label col-sm-2">Roles</label>  
            <div class="form-check col-sm-2">  
                <input type="checkbox" name="roles" value="ROLE_ADMIN" class="form-check-input">  
                ${paramValues.roles.stream().anyMatch(v->v.equals("ROLE_ADMIN")).get()?"checked":""}>  
                <label class="form-check-label">Adminstrador</label>  
            </div>  
            <div class="form-check col-sm-2">  
                <input type="checkbox" name="roles" value="ROLE_USER" class="form-check-input"  
                ${paramValues.roles.stream().anyMatch(v->v.equals("ROLE_USER")).get()?"checked":""}>  
                <label class="form-check-label">Usuario</label>  
            </div>  
            <div class="form-check col-sm-2">  
                <input type="checkbox" name="roles" value="ROLE_MODERADOR" checked class="form-check-input"  
                ${paramValues.roles.stream().anyMatch(v->v.equals("ROLE_MODERADOR")).get()?"checked":""}>  
                <label class="form-check-label">Moderador</label>  
            </div>  
            <%  
                if (errores != null && errores.containsKey("roles")) {  
                    out.println("<small class='alert alert-danger col-sm-4' style='color:red;'>" + errores.get("roles") + "</small>");  
                }            %>        </div>  
  
        <div class="row mb-3">  
            <label class="col-form-label col-sm-2">Idiomas</label>  
            <div class="form-check col-sm-2">  
                <input type="radio" name="idioma" value="es"  
                       class="form-check-input" ${param.idioma.equals("es")?"checked": ""}>  
                <label class="form-check-label">Español</label>  
            </div>  
            <div class="form-check col-sm-2">  
                <input type="radio" name="idioma" value="en"  
                       class="form-check-input" ${param.idioma.equals("en")?"checked": ""}>  
                <label class="form-check-label">Ingles</label>  
            </div>  
            <div class="form-check col-sm-2">  
                <input type="radio" name="idioma" value="fr"  
                       class="form-check-input" ${param.idioma.equals("fr")?"checked": ""}>  
                <label class="form-check-label">Frances</label>  
            </div>  
            <%  
                if (errores != null && errores.containsKey("idioma")) {  
                    out.println("<small class='alert alert-danger' style='color:red;'>" + errores.get("idioma") + "</small>");  
                }            %>        </div>  
  
        <div class="row mb-3">  
            <label for="habilitar" class="col-form-label col-sm-2">Habilitar</label>  
            <div class="form-check col-sm-2">  
                <input type="checkbox" name="habilitar" id="habilitar" checked class="form-check-input">  
            </div>  
        </div>  
  
        <div class="row mb-3">  
            <div>  
                <input type="submit" value="Enviar" class="btn btn-primary">  
            </div>  
        </div>  
  
        <input type="hidden" name="secreto" value="12345">  
  
    </form>  
</div>  
</body>  
</html>
```

![[Pasted image 20231024161945.png]]

![[Pasted image 20231024162013.png]]

