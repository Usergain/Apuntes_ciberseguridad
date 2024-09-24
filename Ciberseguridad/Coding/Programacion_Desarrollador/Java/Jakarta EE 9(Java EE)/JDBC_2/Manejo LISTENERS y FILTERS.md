Ahora vamos a implementar LISTENER y FILTROS:
Listener -> para manejar un evento antes y después. De forma global para todo
Filtros Http -> Local (PRIVADO), para manejarlo un request (ciclo de vida, para inicializar y para terminar). Podemos mapearlo a un servlet(se ejecutará en un servlet en concreto no en todo) en concreto. Cada cliente tiene una conexión individual. *Por lo general se utiliza este para BBDD, para Logear tb puede ser interesante*
Para trabajar BBDD hay que conocer estos dos conceptos.

Listener:

![[Pasted image 20231030163915.png]]

LISTENERS:
```java
package org.aartaraz.apiservlet.webapp.headers.listeners;  
  
import jakarta.servlet.*;  
import jakarta.servlet.annotation.WebListener;  
import jakarta.servlet.http.HttpSession;  
import jakarta.servlet.http.HttpSessionEvent;  
import jakarta.servlet.http.HttpSessionListener;  
import org.aartaraz.apiservlet.webapp.headers.models.Carro;  
  
@WebListener  
public class AplicacionListener implements ServletContextListener, ServletRequestListener, HttpSessionListener {  
    private ServletContext servletContext;  
  
    // De manera global  
    @Override  
    public void contextInitialized(ServletContextEvent sce) {  
        sce.getServletContext().log("Inicializando la aplicación!");  
        servletContext = sce.getServletContext();  
        servletContext.setAttribute("mensaje", "algún valor global de la app!");  
    }  
  
    @Override  
    public void contextDestroyed(ServletContextEvent sce) {  
        servletContext.log("Destruyendo la aplicación!");  
    }  
  
    // De manera individual  
    @Override  
    public void requestDestroyed(ServletRequestEvent sre) {  
        sre.getServletContext().log("Destruyendo el request!");  
    }  
  
    @Override  
    public void requestInitialized(ServletRequestEvent sre) {  
        servletContext.log("Inicializando el request!");  
        sre.getServletRequest().setAttribute("mensaje", "guardando valor pare el request");  
    }  
  
    @Override  
    public void sessionCreated(HttpSessionEvent se) {  
        servletContext.log("Inicializando la sesión http");  
        Carro carro = new Carro();  
        HttpSession session = se.getSession();  
        session.setAttribute("carro", carro);  
    }  
  
    @Override  
    public void sessionDestroyed(HttpSessionEvent se) {  
        servletContext.log("Destruyendo la sesión http");  
    }  
}
```

FILTERS:
```java
package org.aartaraz.apiservlet.webapp.headers.filters;  
  
  
import jakarta.servlet.*;  
import jakarta.servlet.annotation.WebFilter;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginService;  
import org.aartaraz.apiservlet.webapp.headers.services.LoginServiceSessionImpl;  
  
import java.io.IOException;  
import java.util.Optional;  
  
//Todo lo que sea carro compra va a ser privado  
@WebFilter({"/carro/*"})  
public class LoginFiltro implements Filter {  
    @Override  
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {  
        LoginService service = new LoginServiceSessionImpl();  
        Optional<String> username = service.getUsername((HttpServletRequest) servletRequest);  
        if (username.isPresent()) {  
            filterChain.doFilter(servletRequest, servletResponse);  
        } else {  
            ((HttpServletResponse) servletResponse).sendError(HttpServletResponse.SC_UNAUTHORIZED,  
                    "Lo sentimos no estas autorizado para ingresar a esta pagina!");  
        }  
    }  
}
```

CONTROLLER:

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
  
        // Iteramos a traves de los parámetros y buscamos los que empiezan con  
        // "cant_". El campo cant en la vista fueron nombrados "cant_" + productoId.        // Obtenemos el id de cada producto y su correspondiente cantidad ;-).        while (enumer.hasMoreElements()) {  
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
  
@WebServlet("/carro/agregar")  
public class AgregarCarroServlet extends HttpServlet {  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        Long id = Long.valueOf(req.getParameter("id"));  
        ProductoService service = new ProductoServiceImpl();  
        Optional<Producto> producto = service.porId(id);  
        if (producto.isPresent()) {  
            ItemCarro item = new ItemCarro(1, producto.get());  
            HttpSession session = req.getSession();  
            Carro carro = (Carro) session.getAttribute("carro");  
            carro.addItemCarro(item);  
        }  
        resp.sendRedirect(req.getContextPath() + "/carro/ver");  
    }  
}
```

```java
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
  
        String mensajeRequest = (String) req.getAttribute("mensaje"); //Por cada request  
        String mensajeApp = (String) getServletContext().getAttribute("mensaje"); //Singleton  
  
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
                out.println("<th>agregar</th>");  
            }  
            out.println("</tr>");  
            productos.forEach(p -> {  
                out.println("<tr>");  
                out.println("<td>" + p.getId() + "</td>");  
                out.println("<td>" + p.getNombre() + "</td>");  
                out.println("<td>" + p.getTipo() + "</td>");  
                if (usernameOptional.isPresent()) {  
                    out.println("<td>" + p.getPrecio() + "</td>");  
                    out.println("<td><a href=\""  
                            + req.getContextPath()  
                            + "/carro/agregar?id=" + p.getId()  
                            + "\">agregar a carro</a></td>");  
                }  
                out.println("</tr>");  
            });  
            out.println("</table>");  
            out.println("<p>" + mensajeApp + "</p>");  
            out.println("<p>" + mensajeRequest + "</p>");  
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
  
import java.io.IOException;  
  
@WebServlet("/carro/ver")  
public class VerCarroServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        getServletContext().getRequestDispatcher("/carro.jsp").forward(req, resp);  
    }  
}
```

MODELS:


SERVICES:
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

WEBAPPS:
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
<form name="formcarro" action="<%=request.getContextPath()%>/carro/actualizar" method="post">  
    <table>  
        <tr>  
            <th>id</th>  
            <th>nombre</th>  
            <th>precio</th>  
            <th>cantidad</th>  
            <th>total</th>  
            <th>borrar</th>  
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
            <td colspan="4" style="text-align: right">Total:</td>  
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
    <li><a href="/webapp-session/carro/ver">Ver carro</a></li>  
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


![[Pasted image 20231030162751.png]]

![[Pasted image 20231030164927.png]]
Cada vez que se vea el carro o se haga login, etc se iniciará request y luego se destruirá.

Funcionaría igual que la versión sin Filters ni Listeners -> Con los Listeners la app general se levanta una sola vez desde el Listener. Y con los Filters se crean distintos hilos con el servlet con el que quieras comunicarte, se prepara para conectar y cerrar. Da privacidad y a la vez seguridad (No deja hacer inyeccion en las peticiones por navegador).

Si no estas logeado y quieres ver el carro:
![[Pasted image 20231031115249.png|350]]

Con inyección, por ejemplo petición por id=1:
![[Pasted image 20231031115536.png|400]]



