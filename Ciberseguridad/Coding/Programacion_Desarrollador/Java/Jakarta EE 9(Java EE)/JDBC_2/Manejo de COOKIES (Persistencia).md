![[1_servlet_cookies.pdf]]

Se crean en el request de las HEADER o cabeceras y se guarda el response en el body. 
En las cookies se guardan valores en el navegador "cliente" no pudiendose guardar objetos completos.

Este ejemplo de app de cookie, te muestra los precios si estas logeado, sino no. Para borrar la cookie log out:

![[Pasted image 20231027121819.png]]

CONTROLLERS:
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.Cookie;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginService;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginServiceImpl;  
  
import javax.swing.text.html.Option;  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.util.Arrays;  
import java.util.Optional;  
import java.util.OptionalInt;  
  
@WebServlet({"/login", "/login.html"})  
public class LoginServlet extends HttpServlet {  
  
    final static String USERNAME = "admin";  
    final static String PASSWORD = "12345";  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        LoginService auth = new LoginServiceImpl();  
        Optional<String> cookieOptional = auth.getUsername(req);  
  
        if (cookieOptional.isPresent()) {  
            resp.setContentType("text/html;charset=UTF-8");  
            try (PrintWriter out = resp.getWriter()) {  
  
                out.println("<!DOCTYPE html");  
                out.println("<html>");  
                out.println("    <head>");  
                out.println("        <meta charset=\"UTF-8\">");  
                out.println("        <title>Hola " + cookieOptional.get() + " </title>");  
                out.println("    </head>");  
                out.println("    <body>");  
                out.println("        <h1>Hola " + cookieOptional.get() + " has iniciado sesión con éxito!</h1>");  
                out.println("<p><a href='" + req.getContextPath() + "/index.html'>volver</a></p>");  
                out.println("<p><a href='" + req.getContextPath() + "/logout'>cerrar sesión</a></p>");  
                out.println("    </body>");  
                out.println("</html>");  
            }  
        } else {  
            getServletContext().getRequestDispatcher("/login.jsp").forward(req, resp); // por primera vez y cuando no exista la cookie  
  
        }  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        String username = req.getParameter("username");  
        String password = req.getParameter("password");  
  
        if (USERNAME.equals(username) && PASSWORD.equals(password)) {  
            Cookie usernameCookie = new Cookie("username", username);  
            resp.addCookie(usernameCookie);  
  
            resp.sendRedirect(req.getContextPath() + "/login.html"); //Para no tener que abrir un request otra vez cuando vamos atras  
        } else {  
            resp.sendError(HttpServletResponse.SC_UNAUTHORIZED, "Lo sentimos no está autorizado para ingresar a esta página");  
        }  
    }  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.Cookie;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginService;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginServiceImpl;  
  
import java.io.IOException;  
import java.util.Optional;  
  
@WebServlet("/logout")  
public class LogoutServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        LoginService auth = new LoginServiceImpl();  
        Optional<String> username = auth.getUsername(req);  
        if (username.isPresent()) {  
            Cookie usernameCookie = new Cookie("username", "");  
            usernameCookie.setMaxAge(0);  
            resp.addCookie(usernameCookie);  
        }  
        resp.sendRedirect(req.getContextPath() + "/login.html");  
    }  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.Cookie;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginService;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginServiceImpl;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoService;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoServiceImpl;  
  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.util.Arrays;  
import java.util.List;  
import java.util.Optional;  
  
@WebServlet({"/productos.html", "/productos"})  
public class ProductoServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        ProductoService service = new ProductoServiceImpl();  
        List<Producto> productos = service.listar();  
  
        LoginService auth = new LoginServiceImpl();  
        Optional<String> cookieOptional = auth.getUsername(req);  
  
        resp.setContentType("text/html;charset=UTF-8");  
        try (PrintWriter out = resp.getWriter()) {  
  
  
            out.println("<!DOCTYPE html");  
            out.println("<html>");  
            out.println("    <head>");  
            out.println("        <meta charset=\"UTF-8\">");  
            out.println("        <title>Listado de productos</title>");  
            out.println("    </head>");  
            out.println("    <body>");  
            out.println("        <h1>Listado de productos</h1>");  
            if (cookieOptional.isPresent()) {  
                out.println("<div style='color:blue'>Hola " + cookieOptional.get() + "Bienvenido! </div>");  
            }  
            out.println("<table>");  
            out.println("<tr>");  
            out.println("<th>id</th>");  
            out.println("<th>nombre</th>");  
            out.println("<th>tipo</th>");  
            if (cookieOptional.isPresent()) {  
                out.println("<th>precio</th>");  
            }  
            out.println("</tr>");  
            productos.forEach(p -> {  
                out.println("<tr>");  
                out.println("<td>" + p.getId() + "</td>");  
                out.println("<td>" + p.getNombre() + "</td>");  
                out.println("<td>" + p.getTipo() + "</td>");  
                if (cookieOptional.isPresent()) {  
                    out.println("<td>" + p.getPrecio() + "</td>");  
                }  
                out.println("</tr>");  
            });  
            out.println("</table>");  
            out.println("    </body>");  
            out.println("</html>");  
  
        }  
    }  
}
```

MODELS:
```java
package org.aartaraz.apiservlet.webapp.headers.models;  
  
public class Producto {  
    private Long id;  
    private String nombre;  
    private String tipo;  
    private int precio;  
  
    public Producto(Long id, String nombre, String tipo, int precio) {  
        this.id = id;  
        this.nombre = nombre;  
        this.tipo = tipo;  
        this.precio = precio;  
    }  
  
    public Producto() {  
    }  
  
    public Long getId() {  
        return id;  
    }  
  
    public void setId(Long id) {  
        this.id = id;  
    }  
  
    public String getNombre() {  
        return nombre;  
    }  
  
    public void setNombre(String nombre) {  
        this.nombre = nombre;  
    }  
  
    public String getTipo() {  
        return tipo;  
    }  
  
    public void setTipo(String tipo) {  
        this.tipo = tipo;  
    }  
  
    public int getPrecio() {  
        return precio;  
    }  
  
    public void setPrecio(int precio) {  
        this.precio = precio;  
    }  
}
```

SERVICES:
```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
  
import java.util.List;  
  
public interface ProductoService {  
    List<Producto> listar();  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import jakarta.servlet.http.HttpServletRequest;  
  
import java.util.Optional;  
  
public interface LoginService {  
    Optional<String> getUsername(HttpServletRequest request);  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
  
import java.util.Arrays;  
import java.util.List;  
  
public class ProductoServiceImpl implements ProductoService {  
  
    @Override  
    public List<Producto> listar() {  
        return Arrays.asList(new Producto(1L, "notebooke", "computacion", 17500),  
                new Producto(2L, "mesa escritorio", "oficina", 10000),  
                new Producto(3L, "teclado mecanico", "computacion", 40000));  
    }  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import jakarta.servlet.http.Cookie;  
import jakarta.servlet.http.HttpServletRequest;  
  
import java.util.Arrays;  
import java.util.Optional;  
  
public class LoginServiceImpl implements LoginService {  
    @Override  
    public Optional<String> getUsername(HttpServletRequest req) {  
        Cookie[] cookies = req.getCookies() != null ? req.getCookies() : new Cookie[0];  
        return Arrays.stream(cookies)  
                .filter(c -> "username".equals(c.getName()))  
                .map(Cookie::getValue)  
                .findAny();  
    }  
}
```

HTTML Y JSP:
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Cabecera Http</title>  
</head>  
<body>  
<body>  
<h3>Manejo de cookies</h3>  
<ul>  
  
    <li><a href="/webapp-cookie/productos">mostrar productos html</a></li>  
    <li><a href="/webapp-cookie/login.html">Login</a></li>  
    <li><a href="/webapp-cookie/logout">Logout</a></li>  
</ul>  
</body>  
</body>  
</html>
```

```jsp
<%@page contentType="text/html" pageEncoding="UTF-8" %>  
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Formulario de login</title>  
</head>  
<body>  
<h1>Iniciar sesión</h1>  
<form action="/webapp-cookie/login" method="post">  
    <div>  
        <label for="username">Username</label>  
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
        <input type="submit" value="Login">  
    </div>  
</form>  
</body>  
</html>
```

![[Pasted image 20231027122048.png]]
![[Pasted image 20231027122031.png]]
![[Pasted image 20231027121942.png]]
![[Pasted image 20231027122226.png]]
![[Pasted image 20231027122301.png]]
Si clickeasemos logout se borraria cookie de sesión.
Si refrescamos el cookie se guardaría. A menos de eliminarlo. (Aquí vemos la cookie de sesión y la ID de HttpSession)