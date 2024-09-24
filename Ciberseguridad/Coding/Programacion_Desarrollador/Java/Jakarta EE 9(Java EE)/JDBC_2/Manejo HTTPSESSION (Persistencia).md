
![[1_servlet_session.pdf]]

La información ha diferencia de las cookies no se guarda en el navegador(cliente) ,sino que se guarda en el servidor. Podemos guardar objetos completos.
Yo tengo configurado IntelliJ para que los atajos sean igual que en eclipse, para cambiar todas las variables a otro nombre: Alt+Mayús+R
![[Pasted image 20231030155212.png]]

CONTROLLERS:
```java 
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.*;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginService;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginServiceCookieImpl;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginServiceSessionImpl;  
  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.util.Optional;  
  
@WebServlet({"/login", "/login.html"})  
public class LoginServlet extends HttpServlet {  
  
    final static String USERNAME = "admin";  
    final static String PASSWORD = "12345";  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        LoginService auth = new LoginServiceSessionImpl();  
        Optional<String> usernameOptional = auth.getUsername(req);  
  
        if (usernameOptional.isPresent()) {  
            resp.setContentType("text/html;charset=UTF-8");  
            try (PrintWriter out = resp.getWriter()) {  
  
                out.println("<!DOCTYPE html");  
                out.println("<html>");  
                out.println("    <head>");  
                out.println("        <meta charset=\"UTF-8\">");  
                out.println("        <title>Hola " + usernameOptional.get() + " </title>");  
                out.println("    </head>");  
                out.println("    <body>");  
                out.println("        <h1>Hola " + usernameOptional.get() + " has iniciado sesión con éxito!</h1>");  
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
            HttpSession session = req.getSession(); //ctrl+alt+v  
            session.setAttribute("username", username);  
  
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
import jakarta.servlet.http.*;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginService;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginServiceCookieImpl;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginServiceSessionImpl;  
  
import java.io.IOException;  
import java.util.Optional;  
  
@WebServlet("/logout")  
public class LogoutServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        LoginService auth = new LoginServiceSessionImpl();  
        Optional<String> username = auth.getUsername(req);  
        if (username.isPresent()) {  
            HttpSession session = req.getSession();  
            session.invalidate(); //borrario todo en de la sesion de usuario  
        }  
        resp.sendRedirect(req.getContextPath() + "/login.html");  
    }  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.services.*;  
  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.util.List;  
import java.util.Optional;  
  
@WebServlet({"/productos.html", "/productos"})  
public class ProductoServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        ProductoService service = new ProductoServiceImpl();  
        List<Producto> productos = service.listar();  
  
        LoginService auth = new LoginServiceSessionImpl();  
        Optional<String> usernameOptional = auth.getUsername(req);  
  
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
            if (usernameOptional.isPresent()) {  
                out.println("<div style='color:blue'>Hola " + usernameOptional.get() + "Bienvenido! </div>");  
            }  
            out.println("<table>");  
            out.println("<tr>");  
            out.println("<th>id</th>");  
            out.println("<th>nombre</th>");  
            out.println("<th>tipo</th>");  
            if (usernameOptional.isPresent()) {  
                out.println("<th>precio</th>");  
            }  
            out.println("</tr>");  
            productos.forEach(p -> {  
                out.println("<tr>");  
                out.println("<td>" + p.getId() + "</td>");  
                out.println("<td>" + p.getNombre() + "</td>");  
                out.println("<td>" + p.getTipo() + "</td>");  
                if (usernameOptional.isPresent()) {  
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

```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import jakarta.servlet.http.HttpSession;  
import org.aartaraz.apiservlet.webapp.headers.models.Carro;  
import org.aartaraz.apiservlet.webapp.headers.models.ItemCarro;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoService;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoServiceImpl;  
  
import java.io.IOException;  
import java.util.Optional;  
  
@WebServlet("/agregar-carro")  
public class AgregarCarroServlet extends HttpServlet {  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        Long id = Long.valueOf(req.getParameter("id"));  
        ProductoService service = new ProductoServiceImpl();  
        Optional<Producto> producto = service.porId(id);  
        if (producto.isPresent()) {  
            ItemCarro item = new ItemCarro(1, producto.get());  
            HttpSession session = req.getSession();  
            Carro carro;  
            if (session.getAttribute("carro") == null) {  
                carro = new Carro();  
                session.setAttribute("carro", carro);  
            } else {  
                carro = (Carro) session.getAttribute("carro");  
            }  
            carro.addItemCarro(item);  
        }  
        resp.sendRedirect(req.getContextPath() + "/ver-carro");  
    }  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
  
import java.io.IOException;  
  
@WebServlet("/ver-carro")  
public class VerCarroServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        getServletContext().getRequestDispatcher("/carro.jsp").forward(req, resp);  
    }  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import jakarta.servlet.http.HttpSession;  
import org.aartaraz.apiservlet.webapp.headers.models.Carro;  
  
import java.io.IOException;  
import java.util.Arrays;  
import java.util.Enumeration;  
import java.util.List;  
  
@WebServlet("/carro/actualizar")  
public class ActualizarCarroServlet extends HttpServlet {  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        HttpSession session = req.getSession();  
        if (session.getAttribute("carro") != null) {  
            Carro carro = (Carro) session.getAttribute("carro");  
            updateProductos(req, carro);  
            updateCantidades(req, carro);  
        }  
  
        resp.sendRedirect(req.getContextPath() + "/carro/ver");  
    }  
  
    private void updateProductos(HttpServletRequest request, Carro carro) {  
        String[] deleteIds = request.getParameterValues("deleteProductos");  
  
        if (deleteIds != null && deleteIds.length > 0) {  
            List<String> productoIds = Arrays.asList(deleteIds);  
            // Borramos los productos del carrito.  
            carro.removeProductos(productoIds);  
        }  
  
    }  
  
    private void updateCantidades(HttpServletRequest request, Carro carro) {  
  
        Enumeration<String> enumer = request.getParameterNames();  
  
        /* Iteramos a traves de los parámetros y buscamos los que empiezan con  
        "cant_". El campo cant en la vista fueron nombrados "cant_" + productoId.// Obtenemos el id de cada producto y su correspondiente cantidad ;-). */
         
        while (enumer.hasMoreElements()) {        
            String paramName = enumer.nextElement();  
            if (paramName.startsWith("cant_")) {  
                String id = paramName.substring(5);  
                String cantidad = request.getParameter(paramName);  
                if (cantidad != null) {  
                    carro.updateCantidad(id, Integer.parseInt(cantidad));  
                }  
            }  
        }  
    }  
}
```

SERVICES:
```JAVA
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import jakarta.servlet.http.HttpServletRequest;  
  
import java.util.Optional;  
  
public interface LoginService {  
    Optional<String> getUsername(HttpServletRequest request);  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpSession;  
  
import java.util.Optional;  
  
public class LoginServiceSessionImpl implements LoginService {  
    @Override  
    public Optional<String> getUsername(HttpServletRequest request) {  
        HttpSession session = request.getSession();  
        String username = (String) session.getAttribute("username");  
        if (username != null) {  
            return Optional.of(username);  
        }  
        return Optional.empty();  
    }  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
  
import java.util.List;  
import java.util.Optional;  
  
public interface ProductoService {  
    List<Producto> listar();  
    Optional<Producto> porId(Long id);  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
  
import java.util.Arrays;  
import java.util.List;  
import java.util.Optional;  
  
public class ProductoServiceImpl implements ProductoService {  
  
    @Override  
    public List<Producto> listar() {  
        return Arrays.asList(new Producto(1L, "notebooke", "computacion", 17500),  
                new Producto(2L, "mesa escritorio", "oficina", 10000),  
                new Producto(3L, "teclado mecanico", "computacion", 40000));  
    }  
  
    @Override  
    public Optional<Producto> porId(Long id) {  
        return listar().stream().filter(p -> p.getId().equals(id)).findAny();  
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

```java
import java.util.ArrayList;  
import java.util.List;  
import java.util.Optional;  
  
public class Carro {  
    private List<ItemCarro> items;  
  
    public Carro() {  
        this.items = new ArrayList<>();  
    }  
  
    public void addItemCarro(ItemCarro itemCarro) {  
        if (items.contains(itemCarro)) {  
            Optional<ItemCarro> optionalItemCarro = items.stream()  
                    .filter(i -> i.equals(itemCarro))  
                    .findAny();  
            if (optionalItemCarro.isPresent()) {  
                ItemCarro i = optionalItemCarro.get();  
                i.setCantidad(i.getCantidad() + 1);  
            }  
        } else {  
            this.items.add(itemCarro);  
        }  
        //this.items.add(itemCarro);  
    }  
  
    public List<ItemCarro> getItems() {  
        return items;  
    }  
  
    public int getTotal() {  
        return items.stream().mapToInt(ItemCarro::getImporte).sum();  
    }  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.models;  
  
import java.util.Objects;  
  
public class ItemCarro {  
    private int cantidad;  
    private Producto producto;  
  
    public ItemCarro() {  
    }  
  
    public ItemCarro(int cantidad, Producto producto) {  
        this.cantidad = cantidad;  
        this.producto = producto;  
    }  
  
    public int getCantidad() {  
  
        return cantidad;  
    }  
  
    public void setCantidad(int cantidad) {  
  
        this.cantidad = cantidad;  
    }  
  
    public Producto getProducto() {  
  
        return producto;  
    }  
  
    public void setProducto(Producto producto) {  
  
        this.producto = producto;  
    }  
  
    public int getImporte() {  
        return cantidad * producto.getPrecio();  
    }  
  
    @Override  
    public boolean equals(Object o) {  
        if (this == o) return true;  
        if (o == null || getClass() != o.getClass()) return false;  
        ItemCarro itemCarro = (ItemCarro) o;  
        return Objects.equals(producto.getId(), itemCarro.producto.getId())  
                && Objects.equals(producto.getNombre(), itemCarro.producto.getNombre());  
    }  
  
}
```

WEBAPP:
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
  
    <li><a href="/webapp-session/productos">mostrar productos html</a></li>  
    <li><a href="/webapp-session/login.html">Login</a></li>  
    <li><a href="/webapp-session/logout">Logout</a></li>  
    <li><a href="/webapp-session/ver-carro">Ver carro</a></li>  
</ul>  
</body>  
</body>  
</html>
```

```java
<%@page contentType="text/html" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Formulario de login</title>
</head>
<body>
<h1>Iniciar sesión</h1>
<form action="/webapp-session/login" method="post">
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

```jsp
<%@page contentType="text/html" pageEncoding="UTF-8" import="org.aartaraz.apiservlet.webapp.headers.models.*" %>  
<%  
    Carro carro = (Carro) session.getAttribute("carro");  
%>  
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Carro de Compras</title>  
</head>  
<body>  
<h1>Carro de Compras</h1>  
<%if (carro == null || carro.getItems().isEmpty()) {%>  
<p>Lo sentimos no hay productos en el carro de compras</p>  
<%} else { %>  
<form action="formcarro" action="<%=request.getContextPath()%>/actualizar-carro" method="post">  
    <table>  
        <tr>  
            <th>id</th>  
            <th>nombre</th>  
            <th>precio</th>  
            <th>cantidad</th>  
            <th>total</th>  
        </tr>  
        <%for (ItemCarro item : carro.getItems()) {%>  
        <tr>  
            <td><%=item.getProducto().getId()%>  
            </td>  
            <td><%=item.getProducto().getNombre()%>  
            </td>  
            <td><%=item.getProducto().getPrecio()%>  
            </td>  
            <td><input type="text" size="4" name="cant_<%=item.getProducto().getId()%>"  
                       value="<%=item.getCantidad()%>"/>  
            </td>  
            <td><%=item.getImporte()%>  
            </td>  
            <td><input type="checkbox" value="<%=item.getProducto().getId()%>" name="deleteProductos"/></td>  
        </tr>  
        <%}%>  
        <tr>  
            <td colspan="4" style="text-align: right">Total</td>  
            <td><%=carro.getTotal()%>  
            </td>  
        </tr>  
    </table>  
    <a href="javascript:document.formcarro.submit();">Actualizar</a>  
</form>  
<%}%>  
<p><a href="<%=request.getContextPath()%>/productos">seguir comprando</a></p>  
<p><a href="<%=request.getContextPath()%>/index.html">volver</a></p>  
</body>  
</html>
```

Persistente hasta que se dé al logout:
![[Pasted image 20231030124935.png]]

![[Pasted image 20231031112328.png]]

![[Pasted image 20231031112436.png|350]]

Después de logearse:
![[Pasted image 20231031112540.png|400]]

![[Pasted image 20231031113825.png|350]]

![[Pasted image 20231031113901.png|350]]