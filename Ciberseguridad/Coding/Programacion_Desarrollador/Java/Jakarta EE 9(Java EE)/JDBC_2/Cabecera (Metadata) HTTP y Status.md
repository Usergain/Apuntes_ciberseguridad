
(ctrl+alt+v en java para completar la variable->Atajo de IntelliJ)

Resumen de Status:
1. [Informational responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#information_responses) (`100` – `199`)
2. [Successful responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#successful_responses) (`200` – `299`)
3. [Redirection messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#redirection_messages) (`300` – `399`)
4. [Client error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#client_error_responses) (`400` – `499`)
5. [Server error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#server_error_responses) (`500` – `599`)

Para más detalle: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#client_error_responses

![[Pasted image 20231026185246.png|400]]

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

```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Cabecera Http</title>  
</head>  
<body>  
<body>  
<h3>Cabecera Http</h3>  
<ul>  
  
    <li><a href="/webapp-headers/cabeceras-request">cabeceras http request</a></li>  
    <li><a href="/webapp-headers/productos">mostrar productos html</a></li>  
    <li><a href="/webapp-headers/productos.xls">exportar xls</a></li>  
    <li><a href="/webapp-headers/productos.json">mostrar json</a></li>  
    <li><a href="/webapp-headers/hora-actualizada">La hora actualizada!</a></li>  
    <li><a href="/webapp-headers/redirigir">Redirigir</a></li>  
    <li><a href="/webapp-headers/despachar">Despachar</a></li>  
    <li><a href="/webapp-headers/login.html">Login</a></li>  
    <li><a href="/webapp-headers/buscar.html">Buscar producto</a></li>  
</ul>  
</body>  
</body>  
</html>
```

![[Pasted image 20231026190657.png|500]]

Servlet  CabecerasHttpRequestServlet, el cual muestra por pantalla los distintos tipos de cabeceras, request y consultas por navegador:
```java
org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.util.Enumeration;  
  
@WebServlet("/cabeceras-request")  
public class CabecerasHttpRequestServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        resp.setContentType("text/html;charset=UTF-8");  
  
        String metodoHttp = req.getMethod();  
        String requestUri = req.getRequestURI();  
        String requestUrl = req.getRequestURL().toString();  
        String contextPath = req.getContextPath();  
        String servletPath = req.getServletPath();  
        String ipCliente = req.getRemoteAddr(); //cliente  
        String ip = req.getLocalAddr(); //usuario  
        int port = req.getLocalPort();  
        String scheme = req.getScheme();  
        String host = req.getHeader("host");  
        String url = scheme + "://" + host + contextPath + servletPath;  
        String url2 = scheme + "://" + ip + ":" + port + contextPath + servletPath;  
  
        try (PrintWriter out = resp.getWriter()) {  
  
            out.println("<!DOCTYPE html");  
            out.println("<html>");  
            out.println("    <head>");  
            out.println("        <meta charset=\"UTF-8\">");  
            out.println("        <title>Cabecera HTTP Request</title>");  
            out.println("    </head>");  
            out.println("    <body>");  
            out.println("        <h1>Cabecera HTTP Request</h1>");  
            out.println("<ul>");  
            out.println("<li>metodo http: " + metodoHttp + "</li>");  
            out.println("<li>reques uri http: " + requestUri + "</li>");  
            out.println("<li>request url http: " + requestUrl + "</li>");  
            out.println("<li>contextPath http: " + contextPath + "</li>");  
            out.println("<li>Servlet http: " + servletPath + "</li>");  
            out.println("<li>ip local: " + ip + "</li>");  
            out.println("<li>ip cliente: " + ipCliente + "</li>");  
            out.println("<li>puerto local: " + port + "</li>");  
            out.println("<li>scheme: " + scheme + "</li>");  
            out.println("<li>host: " + host + "</li>");  
            out.println("<li>url: " + url + "</li>");  
            out.println("<li>url2: " + url2 + "</li>");  
  
            Enumeration<String> headerNames = req.getHeaderNames(); //cabeceras y las recoreremos en un while  
            while (headerNames.hasMoreElements()) {  
                String cabecera = headerNames.nextElement();  
                out.println("<li>" + cabecera + ": " + req.getHeader(cabecera) + "</li>");  
            }  
  
            out.println("</ul>");  
            out.println("    </body>");  
            out.println("</html>");  
        }  
    }  
}
```
![[Pasted image 20231026190506.png|750]]

Servlet que te da listado de productos.html, productos.xml y productos:
```java
artaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoService;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoServiceImpl;  
  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.util.List;  
  
@WebServlet({"/productos.xls", "/productos.html", "/productos"})  
public class ProductoXlsServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        ProductoService service = new ProductoServiceImpl();  
        List<Producto> productos = service.listar();  
  
        resp.setContentType("text/html;charset=UTF-8");  
        String servletPath = req.getServletPath();  
        boolean esXls = servletPath.endsWith(".xls");  
        if (esXls) {  
            resp.setContentType("application/vnd.ms-excel");  
            resp.setHeader("Content-Disposition", "attachment; filename=productos.xls");  
        }  
        try (PrintWriter out = resp.getWriter()) {  
  
            if (!esXls) {  
                out.println("<!DOCTYPE html");  
                out.println("<html>");  
                out.println("    <head>");  
                out.println("        <meta charset=\"UTF-8\">");  
                out.println("        <title>Listado de productos</title>");  
                out.println("    </head>");  
                out.println("    <body>");  
                out.println("        <h1>Listado de productos</h1>");  
                out.println("<p><a href=\"" + req.getContextPath() + "/productos.xls" + "\">exportar a excel</a></p>");  
                out.println("<p><a href=\"" + req.getContextPath() + "/productos.json" + "\">mostrar json</a></p>");  
            }  
            out.println("<table>");  
            out.println("<tr>");  
            out.println("<th>id</th>");  
            out.println("<th>nombre</th>");  
            out.println("<th>tipo</th>");  
            out.println("<th>precio</th>");  
            out.println("</tr>");  
            productos.forEach(p -> {  
                out.println("<tr>");  
                out.println("<td>" + p.getId() + "</td>");  
                out.println("<td>" + p.getNombre() + "</td>");  
                out.println("<td>" + p.getTipo() + "</td>");  
                out.println("<td>" + p.getPrecio() + "</td>");  
                out.println("</tr>");  
            });  
            out.println("</table>");  
            if (!esXls) {  
                out.println("    </body>");  
                out.println("</html>");  
            }  
  
        }  
    }  
}
```
![[Pasted image 20231026191241.png|500]]

Servlet que muestra por pantalla los producto en JSON:
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import com.fasterxml.jackson.databind.ObjectMapper;  
import jakarta.servlet.ServletException;  
import jakarta.servlet.ServletInputStream;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoService;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoServiceImpl;  
  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.util.List;  
  
@WebServlet("/productos.json")  
public class ProductoJsonServlet extends HttpServlet {  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        ServletInputStream jsonStream = req.getInputStream();  
        ObjectMapper mapper = new ObjectMapper();  
        Producto producto = mapper.readValue(jsonStream, Producto.class);  
        resp.setContentType("text/html;charset=UTF-8");  
        try (PrintWriter out = resp.getWriter()) {  
  
            out.println("<!DOCTYPE html");  
            out.println("<html>");  
            out.println("    <head>");  
            out.println("        <meta charset=\"UTF-8\">");  
            out.println("        <title>Detalle de producto desde json</title>");  
            out.println("    </head>");  
            out.println("    <body>");  
            out.println("        <h1>Detalle de producto desde json</h1>");  
            out.println("<ul>");  
            out.println("<li>Id: " + producto.getId() + "</li>");  
            out.println("<li>Nombre: " + producto.getNombre() + "</li>");  
            out.println("<li>Tipo: " + producto.getTipo() + "</li>");  
            out.println("<li>Precio: " + producto.getPrecio() + "</li>");  
            out.println("</ul>");  
            out.println("    </body>");  
            out.println("</html>");  
        }  
    }  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
  
        ProductoService service = new ProductoServiceImpl();  
        List<Producto> productos = service.listar();  
        ObjectMapper mapper = new ObjectMapper();  
        String json = mapper.writeValueAsString(productos);  
        resp.setContentType("application/json");  
        resp.getWriter().write(json);  
    }  
}
```
![[Pasted image 20231026191711.png|500]]

Enviar un objeto Json en el cuerpo del request (API Rest):
Vamos a un cliente, Postman:

![[Pasted image 20231026164307.png]]

![[Pasted image 20231026164455.png]]

Servlet que te muestra por pantalla hora actualizada:
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.time.LocalTime;  
import java.time.format.DateTimeFormatter;  
  
@WebServlet("/hora-actualizada")  
public class HoraActualizadaServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        resp.setContentType("text/html;charset=UTF-8");  
        resp.setHeader("refresh", "1");  
  
        LocalTime hora = LocalTime.now();  
        DateTimeFormatter df = DateTimeFormatter.ofPattern("hh:mm:ss");  
        try (PrintWriter out = resp.getWriter()) {  
  
            out.println("<!DOCTYPE html");  
            out.println("<html>");  
            out.println("    <head>");  
            out.println("        <meta charset=\"UTF-8\">");  
            out.println("        <title>La hora actualizada</title>");  
            out.println("    </head>");  
            out.println("    <body>");  
            out.println("        <h1>La hora actualizada</h1>");  
            out.println("<h3>" + hora.format(df) + "</h3>");  
            out.println("    </body>");  
            out.println("</html>");  
        }  
    }
```
![[Pasted image 20231026192243.png|500]]

Servlet que abre un nuevo request nuevo de otro ya existente:
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
  
import java.io.IOException;  
  
//Es un request nuevo de otro ya existente  
@WebServlet("/redirigir")  
public class RedirigirServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        //resp.setHeader("Location", req.getContextPath() + "/productos.html");  
        //resp.setStatus(HttpServletResponse.SC_FOUND);        resp.sendRedirect(req.getContextPath() + "/productos.html");  
    }  
}
```
![[Pasted image 20231026192536.png|500]]

servlet que reutiliza un request existente y se fusiona a el:
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
  
import java.io.IOException;  
  
//Es el mismo request de productos.html no es un request nuevo (une los requests), todos los parametros, etc se heredan  
@WebServlet("/despachar")  
public class DespacharSevlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        getServletContext().getRequestDispatcher("/productos.html").forward(req, resp);  
    }  
}
```
![[Pasted image 20231026192753.png|500]]

Servlet buscador de producto:
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoService;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoServiceImpl;  
  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.util.Optional;  
  
@WebServlet("/buscar-producto")  
public class BuscarProductoServlet extends HttpServlet {  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        ProductoService service = new ProductoServiceImpl();  
        String nombre = req.getParameter("producto");  
  
        Optional<Producto> encontrado = service.listar().stream().filter(p -> {  
            if (nombre == null || nombre.isBlank()) {  
                return false;  
            }  
            return p.getNombre().contains(nombre);  
        }).findFirst();  
        if (encontrado.isPresent()) {  
            resp.setContentType("text/html;charset=UTF-8");  
            try (PrintWriter out = resp.getWriter()) {  
  
                out.println("<!DOCTYPE html");  
                out.println("<html>");  
                out.println("    <head>");  
                out.println("        <meta charset=\"UTF-8\">");  
                out.println("        <title>Producto encontrado</title>");  
                out.println("    </head>");  
                out.println("    <body>");  
                out.println("        <h1Producto encontrado</h1>");  
                out.println("        <h3>Producto encontrado: " + encontrado.get().getNombre() +  
                        " el precio $" + encontrado.get().getPrecio() + "</h3>");  
                out.println("    </body>");  
                out.println("</html>");  
            }  
        } else {  
            resp.sendError(HttpServletResponse.SC_NOT_FOUND, "Lo sentimos no se encontró el producto " + nombre);  
        }  
    }  
}
```
![[Pasted image 20231026193359.png|500]]
![[Pasted image 20231026193433.png|500]]
Si metiesemos mal saldría valor de error:
![[Pasted image 20231026193922.png|500]]

Servlet de login:
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
  
import java.io.IOException;  
import java.io.PrintWriter;  
  
@WebServlet("/login")  
public class LoginServlet extends HttpServlet {  
  
    final static String USERNAME = "admin";  
    final static String PASSWORD = "12345";  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        String username = req.getParameter("username");  
        String password = req.getParameter("password");  
  
        if (USERNAME.equals(username) && PASSWORD.equals(password)) {  
            resp.setContentType("text/html;charset=UTF-8");  
            try (PrintWriter out = resp.getWriter()) {  
  
                out.println("<!DOCTYPE html");  
                out.println("<html>");  
                out.println("    <head>");  
                out.println("        <meta charset=\"UTF-8\">");  
                out.println("        <title>Login correcto</title>");  
                out.println("    </head>");  
                out.println("    <body>");  
                out.println("        <h1>Login correcto</h1>");  
                out.println("<h3>Hola " + username + " has iniciado sesión con éxito!</h3>");  
                out.println("    </body>");  
                out.println("</html>");  
            }  
  
        } else {  
            resp.sendError(HttpServletResponse.SC_UNAUTHORIZED, "Lo sentimos no está autorizado para ingresar a esta página");  
        }  
    }  
}
```
![[Pasted image 20231026194217.png|500]]
![[Pasted image 20231026194259.png|500]]
Si metiese mal el usuario o password
![[Pasted image 20231026194342.png|500]]





