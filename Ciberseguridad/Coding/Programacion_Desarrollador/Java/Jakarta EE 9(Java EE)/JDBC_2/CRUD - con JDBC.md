Instalar el driver jar a la libreria de Tomcat, hay tres maneras de hacerlo:
1- Iremos donde tenemos instalado MySQL.
![[Pasted image 20231031151447.png|900]]

![[Pasted image 20231031151604.png|800]]

![[Pasted image 20231031151834.png|800]]

En mi caso tenía la versión 8.0.29 instalada y no tengo porque bajarme el drive para Java.
![[Pasted image 20231031151948.png|600]]
Este archivo .jar lo copiamos y lo pegamos en la carpeta lib de Tomcat que estamos usando en el proyecto.
![[Pasted image 20231031153043.png|800]]

2- https://dev.mysql.com/downloads/installer/ -> Este archivo .jar lo copiamos y lo pegamos en la carpeta lib de Tomcat que estamos usando en el proyecto.

3- Creariamos una dependencia en el POM, la cual volveriamos a repetir lo mismo anteriormente de archivo .jar lo copiamos y lo pegamos en la carpeta lib de Tomcat que estamos usando en el proyecto. Importante luego borrar la dependencia para que no entre en conflicto con el archivo .jar instalado en el directorio lib.
Alt+Insert -> Add dependencies:
![[Pasted image 20231031153554.png|1100]]

Después de actualizar con Maven -> ir a pestaña derecha:
![[Pasted image 20231031153934.png|550]]
En Libreria externas estaría descargadas.


Esta es la organización de clases del proyecto(las clases de cookies en este caso no servirían para nada ya que esta implementado con HttpSession):
![[Pasted image 20231107124653.png]]

![[Pasted image 20231107124857.png]]
Si no nos logeamos y mostramos producto:
![[Pasted image 20231107124939.png]]
Tiene una capa de seguridad, con la clase filtro, dado que si no estuviesemos logeados no podriamos modificar los productos desde la barra del navegador.
Si nos logeamos (en caso de meter algún campo mal nos reconduciría a una página con el error y mensaje):

![[Pasted image 20231107125021.png]]
![[Pasted image 20231107125201.png]]
![[Pasted image 20231107125253.png]]

agregar al carro:
![[Pasted image 20231107125420.png]]

editar:
![[Pasted image 20231107125453.png]]

eliminar:
![[Pasted image 20231107125528.png]]

crear:
![[Pasted image 20231107125613.png]]
Este formulario esta implementado con sus validaciones, del mismo modo que eliminar.

*---------------------*
 *CONTROLLERS:
---------------------
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
import org.aartaraz.apiservlet.webapp.headers.services.ProductoServiceJdbcImpl;  
  
import java.io.IOException;  
import java.sql.Connection;  
import java.util.Optional;  
  
@WebServlet("/carro/agregar")  
public class AgregarCarroServlet extends HttpServlet {  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        Long id = Long.valueOf(req.getParameter("id"));  
        Connection conn = (Connection) req.getAttribute("conn");  
        ProductoService service = new ProductoServiceJdbcImpl(conn);  
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
import org.aartaraz.apiservlet.webapp.headers.services.ProductoService;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoServiceJdbcImpl;  
  
import java.io.IOException;  
import java.sql.Connection;  
import java.util.Optional;  
  
@WebServlet("/productos/eliminar")  
public class ProductoEliminarServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        Connection conn = (Connection) req.getAttribute("conn");  
        ProductoService service = new ProductoServiceJdbcImpl(conn);  
        long id;  
        try {  
            id = Long.parseLong(req.getParameter("id"));  
        } catch (NumberFormatException e) {  
            id = 0L;  
        }  
        if (id > 0) {  
            Optional<Producto> o = service.porId(id);  
            if (o.isPresent()) {  
                service.eliminar(id);  
                resp.sendRedirect(req.getContextPath() + "/productos");  
            } else {  
                resp.sendError(HttpServletResponse.SC_NOT_FOUND, "No existe el producto en la base de datos");  
            }  
        } else {  
            resp.sendError(HttpServletResponse.SC_NOT_FOUND, "Error el id es null, se debe enviar como parametro en la url");  
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
import org.aartaraz.apiservlet.webapp.headers.models.Categoria;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoService;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoServiceJdbcImpl;  
  
import java.io.IOException;  
import java.sql.Connection;  
import java.time.DateTimeException;  
import java.time.LocalDate;  
import java.time.format.DateTimeFormatter;  
import java.util.HashMap;  
import java.util.Map;  
import java.util.Optional;  
  
@WebServlet("/productos/form")  
public class ProductoFormServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        Connection conn = (Connection) req.getAttribute("conn");  
        ProductoService service = new ProductoServiceJdbcImpl(conn);  
        long id;  
        try {  
            id = Long.parseLong(req.getParameter("id"));  
        } catch (NumberFormatException e) {  
            id = 0L;  
        }  
        Producto producto = new Producto();  
        producto.setCategoria(new Categoria());  
        if (id > 0) {  
            Optional<Producto> o = service.porId(id);  
            if (o.isPresent()) {  
                producto = o.get();  
            }  
        }  
        req.setAttribute("categorias", service.listarCategoria());  
        req.setAttribute("producto", producto);  
        getServletContext().getRequestDispatcher("/form.jsp").forward(req, resp);  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        Connection conn = (Connection) req.getAttribute("conn");  
        ProductoService service = new ProductoServiceJdbcImpl(conn);  
        String nombre = req.getParameter("nombre");  
        Integer precio;  
  
        try {  
            precio = Integer.valueOf(req.getParameter(("precio")));  
        } catch (NumberFormatException e) {  
            precio = 0;  
        }  
  
        String sku = req.getParameter("sku");  
        String fechaStr = req.getParameter("fecha_registro");  
        Long categoriaId;  
  
        try {  
            categoriaId = Long.valueOf(req.getParameter("categoria"));  
        } catch (NumberFormatException e) {  
            categoriaId = 0L;  
        }  
  
        Map<String, String> errores = new HashMap<>();  
  
        if (nombre == null || nombre.isBlank()) {  
            errores.put("nombre", "el nombre es requerido");  
        }  
        if (sku == null || sku.isBlank()) {  
            errores.put("sku", "el sku es requerido!");  
        } else if (sku.length() > 10) {  
            errores.put("sku", "el sku debe tener max 10caracteres!");  
        }  
  
        if (fechaStr == null || fechaStr.isBlank()) {  
            errores.put("fecha_registro", "la fecha es requerida");  
        }  
        if (precio.equals(0)) {  
            errores.put("precio", "el precio es requerida");  
        }  
        if (categoriaId.equals(0L)) {  
            errores.put("categoria", "la categoria es requerida!");  
        }  
  
        LocalDate fecha;  
        try {  
            fecha = LocalDate.parse(fechaStr, DateTimeFormatter.ofPattern("yyyy-MM-dd"));  
        } catch (DateTimeException e) {  
            fecha = null;  
        }  
        long id;  
        try {  
            id = Long.parseLong(req.getParameter("id"));  
        } catch (NumberFormatException e) {  
            id = 0L;  
        }  
  
        Producto producto = new Producto();  
        producto.setId(id);  
        producto.setNombre(nombre);  
        producto.setSku(sku);  
        producto.setPrecio(precio);  
        producto.setFechaRegistro(fecha);  
  
        Categoria categoria = new Categoria();  
        categoria.setId(categoriaId);  
        producto.setCategoria(categoria);  
  
        if (errores.isEmpty()) {  
            service.guardar(producto);  
            resp.sendRedirect(req.getContextPath() + "/productos");  
        } else {  
            req.setAttribute("errores", errores);  
            req.setAttribute("categorias", service.listarCategoria());  
            req.setAttribute("producto", producto);  
            getServletContext().getRequestDispatcher("/form.jsp").forward(req, resp);  
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
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.services.*;  
  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.sql.Connection;  
import java.util.List;  
import java.util.Optional;  
  
@WebServlet({"/productos.html", "/productos"})  
public class ProductoServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        Connection conn = (Connection) req.getAttribute("conn");  
        ProductoService service = new ProductoServiceJdbcImpl(conn);  
        List<Producto> productos = service.listar();  
  
        LoginService auth = new LoginServiceSessionImpl();  
        Optional<String> usernameOptional = auth.getUsername(req);  
  
        req.setAttribute("productos", productos);  
        req.setAttribute("username", usernameOptional);  
        getServletContext().getRequestDispatcher("/listar.jsp").forward(req, resp);  
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


*-----------------*
   *FILTERS:*
*-----------------*
```java
package org.aartaraz.apiservlet.webapp.headers.filters;  
  
import jakarta.servlet.*;  
import jakarta.servlet.annotation.WebFilter;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.services.ServiceJdbcException;  
import org.aartaraz.apiservlet.webapp.headers.util.ConexionBaseDatos;  
  
import java.io.IOException;  
import java.sql.Connection;  
import java.sql.SQLException;  
  
@WebFilter("/*")  
public class ConexionFilter implements Filter {  
    @Override  
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {  
        try (Connection conn = ConexionBaseDatos.getConnection()) {  
  
            if (conn.getAutoCommit()) {  
                conn.setAutoCommit(false);  
            }  
            try {  
                servletRequest.setAttribute("conn", conn);  
                filterChain.doFilter(servletRequest, servletResponse);  
                conn.commit();  
            } catch (SQLException | ServiceJdbcException e) {  
                conn.rollback();  
                ((HttpServletResponse) servletResponse).sendError(HttpServletResponse.SC_INTERNAL_SERVER_ERROR, e.getMessage());  
                e.printStackTrace();  
            }  
        } catch (SQLException throwables) {  
            throwables.printStackTrace();  
        }  
    }  
}
```

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
@WebFilter({"/carro/*", "/productos/form/*"})  
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


*---------------------*
    *LISTENERS:*
*---------------------*
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


*---------------------*
      *MODELS:*
*---------------------*
```java
package org.aartaraz.apiservlet.webapp.headers.models;  
  
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
  
    public void removeProductos(List<String> productoIds) {  
        if (productoIds != null) {  
            productoIds.forEach(this::removeProducto);  
            // que es lo mismo a:  
            // productoIds.forEach(productoId -> removeProducto(productoId));        }  
    }  
  
    public void removeProducto(String productoId) {  
        Optional<ItemCarro> producto = findProducto(productoId);  
        producto.ifPresent(itemCarro -> items.remove(itemCarro));  
    }  
  
    public void updateCantidad(String productoId, int cantidad) {  
        Optional<ItemCarro> producto = findProducto(productoId);  
        producto.ifPresent(itemCarro -> itemCarro.setCantidad(cantidad));  
    }  
  
    private Optional<ItemCarro> findProducto(String productoId) {  
        return items.stream()  
                .filter(itemCarro -> productoId.equals(Long.toString(itemCarro.getProducto().getId())))  
                .findAny();  
    }  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.models;  
  
public class Categoria {  
    private Long id;  
    private String nombre;  
  
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

```java
package org.aartaraz.apiservlet.webapp.headers.models;  
  
import java.time.LocalDate;  
  
public class Producto {  
    private Long id;  
    private String nombre;  
    private Categoria categoria;  
    private int precio;  
    private String sku;  
    private LocalDate fechaRegistro;  
  
    public Producto(Long id, String nombre, String tipo, int precio) {  
        this.id = id;  
        this.nombre = nombre;  
        Categoria categoria = new Categoria();  
        categoria.setNombre(tipo);  
        this.categoria = categoria;  
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
  
    public int getPrecio() {  
        return precio;  
    }  
  
    public void setPrecio(int precio) {  
  
        this.precio = precio;  
    }  
  
    public Categoria getCategoria() {  
  
        return categoria;  
    }  
  
    public void setCategoria(Categoria categoria) {  
  
        this.categoria = categoria;  
    }  
  
    public String getSku() {  
  
        return sku;  
    }  
  
    public void setSku(String sku) {  
  
        this.sku = sku;  
    }  
  
    public LocalDate getFechaRegistro() {  
  
        return fechaRegistro;  
    }  
  
    public void setFechaRegistro(LocalDate fechaRegistro) {  
  
        this.fechaRegistro = fechaRegistro;  
    }  
}
```


*---------------------*
  *REPOSITORIES*
*---------------------*
```java
package org.aartaraz.apiservlet.webapp.headers.repositories;  
  
import java.sql.SQLException;  
import java.util.List;  
  
public interface Repository <T>{  
    List<T> listar() throws SQLException;  
    T porId(Long id) throws SQLException;  
    void guardar(T t) throws SQLException;  
    void eliminar(Long id) throws SQLException;  
}
```

```java
import org.aartaraz.apiservlet.webapp.headers.models.Categoria;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
  
import java.net.http.HttpConnectTimeoutException;  
import java.sql.*;  
import java.util.ArrayList;  
import java.util.List;  
  
public class ProductosRepositoryJdbcImpl implements Repository<Producto> {  
    private Connection conn;  
  
    public ProductosRepositoryJdbcImpl(Connection conn) {  
        this.conn = conn;  
    }  
  
    @Override  
    public List<Producto> listar() throws SQLException {  
        List<Producto> productos = new ArrayList<>();  
        try (Statement stmt = conn.createStatement();  
             ResultSet rs = stmt.executeQuery("SELECT p.*, c.nombre as categoria FROM productos as p " +  
                     " inner join categorias as c ON (p.categoria_id = c.id) order by p.id ASC")) {  
            while (rs.next()) {  
                Producto p = getProducto(rs);  
  
                productos.add(p);  
            }  
        }  
        return productos;  
    }  
  
    @Override  
    public Producto porId(Long id) throws SQLException {  
        Producto producto = null;  
        try (PreparedStatement stmt = conn.prepareStatement("SELECT p.*, c.nombre as categoria FROM productos as p" +  
                " inner join categorias as c ON (p.categoria_id = c.id) WHERE p.id = ?")) {  
            stmt.setLong(1, id);  
  
            try (ResultSet rs = stmt.executeQuery()) {  
                if (rs.next()) {  
                    producto = getProducto(rs);  
                }  
            }  
        }  
        return producto;  
    }  
  
    @Override  
    public void guardar(Producto producto) throws SQLException {  
        String sql;  
        if (producto.getId() != null && producto.getId() > 0) {  
            sql = "update productos set nombre=?,precio=?, sku=?, categoria=?, where id=?";  
        } else {  
            sql = "insert into productos (nombre, precio, sku, categoria_id, fecha_registro) values (?,?,?,?,?)";  
        }  
        try (PreparedStatement stmt = conn.prepareStatement(sql)) {  
            stmt.setString(1, producto.getNombre());  
            stmt.setInt(2, producto.getPrecio());  
            stmt.setString(3, producto.getSku());  
            stmt.setLong(4, producto.getCategoria().getId());  
            if (producto.getId() != null && producto.getId() > 0) {  
                stmt.setLong(5, producto.getId());  
            } else {  
                stmt.setDate(5, Date.valueOf(producto.getFechaRegistro()));  
            }  
            stmt.executeUpdate();  
        }  
    }  
  
    @Override  
    public void eliminar(Long id) throws SQLException {  
        String sql = "delete from productos where id=?";  
        try (PreparedStatement stmt = conn.prepareStatement(sql)) {  
            stmt.setLong(1, id);  
            stmt.executeUpdate();  
        }  
    }  
  
    private static Producto getProducto(ResultSet rs) throws SQLException {  
        Producto p = new Producto();  
        p.setId(rs.getLong("id"));  
        p.setNombre(rs.getString("nombre"));  
        p.setPrecio(rs.getInt("precio"));  
        p.setSku(rs.getString("sku"));  
        p.setFechaRegistro(rs.getDate("fecha_registro").toLocalDate());  
        Categoria c = new Categoria();  
        c.setId(rs.getLong("categoria_id"));  
        c.setNombre(rs.getString("categoria"));  
        p.setCategoria(c);  
        return p;  
    }  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.repositories;  
  
import org.aartaraz.apiservlet.webapp.headers.models.Categoria;  
  
import java.sql.*;  
import java.util.ArrayList;  
import java.util.List;  
  
public class CategoriaRepositorioImpl implements Repository<Categoria> {  
    private Connection conn;  
  
    public CategoriaRepositorioImpl(Connection conn) {  
  
        this.conn = conn;  
    }  
  
    @Override  
    public List<Categoria> listar() throws SQLException {  
        List<Categoria> categorias = new ArrayList<>();  
        try (Statement stmt = conn.createStatement();  
             ResultSet rs = stmt.executeQuery("select * from categorias")) {  
            while (rs.next()) {  
                Categoria categoria = getCategoria(rs); //Hemos sacado el metodo fuera para reutilizarlo  
                categorias.add(categoria);  
            }  
        }  
        return categorias;  
    }  
  
    @Override  
    public Categoria porId(Long id) throws SQLException {  
        Categoria categoria = null;  
        try (PreparedStatement stmt = conn.prepareStatement("select * from categorias as c where c.id=?")) {  
            stmt.setLong(1, id);  
            try (ResultSet rs = stmt.executeQuery()) {  
                if (rs.next()) {  
                    categoria = getCategoria(rs);  
                }  
            }  
        }  
        return categoria;  
    }  
  
    @Override  
    public void guardar(Categoria categoria) throws SQLException {  
  
    }  
  
    @Override  
    public void eliminar(Long id) throws SQLException {  
  
    }  
  
    //metodo que mapea  
    private static Categoria getCategoria(ResultSet rs) throws SQLException {  
        Categoria categoria = new Categoria();  
        categoria.setNombre(rs.getString("nombre"));  
        categoria.setId(rs.getLong("id"));  
        return categoria;  
    }  
}
```



*---------------------*
     *SERVICES*
*---------------------*
```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import org.aartaraz.apiservlet.webapp.headers.models.Categoria;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
  
import java.util.List;  
import java.util.Optional;  
  
public interface ProductoService {  
    List<Producto> listar();  
  
    Optional<Producto> porId(Long id);  
  
    void guardar(Producto producto);  
  
    void eliminar(Long id);  
  
    List<Categoria> listarCategoria();  
  
    Optional<Categoria> porIdCategoria(Long id);  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import org.aartaraz.apiservlet.webapp.headers.models.Categoria;  
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
  
    @Override  
    public void guardar(Producto producto) {  
  
    }  
  
    @Override  
    public void eliminar(Long id) {  
  
    }  
  
    @Override  
    public List<Categoria> listarCategoria() {  
        return null;  
    }  
  
    @Override  
    public Optional<Categoria> porIdCategoria(Long id) {  
        return Optional.empty();  
    }  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import org.aartaraz.apiservlet.webapp.headers.models.Categoria;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.repositories.CategoriaRepositorioImpl;  
import org.aartaraz.apiservlet.webapp.headers.repositories.ProductosRepositoryJdbcImpl;  
import org.aartaraz.apiservlet.webapp.headers.repositories.Repository;  
  
import java.sql.Connection;  
import java.sql.SQLException;  
import java.util.List;  
import java.util.Optional;  
  
public class ProductoServiceJdbcImpl implements ProductoService {  
    private Repository<Producto> repositoryJdbc;  
    private Repository<Categoria> repositoryCategoriaJdbc;  
  
    public ProductoServiceJdbcImpl(Connection connection) {  
        this.repositoryJdbc = new ProductosRepositoryJdbcImpl(connection);  
        this.repositoryCategoriaJdbc = new CategoriaRepositorioImpl(connection);  
    }  
  
    @Override  
    public List<Producto> listar() {  
        try {  
            return repositoryJdbc.listar();  
        } catch (SQLException e) {  
            throw new ServiceJdbcException(e.getMessage(), e.getCause());  
        }  
    }  
  
    @Override  
    public Optional<Producto> porId(Long id) {  
        try {  
            return Optional.ofNullable(repositoryJdbc.porId(id)); //Siempre va a devolver un Optional de producto, contenga la instancia o no  
        } catch (SQLException e) {  
            throw new ServiceJdbcException(e.getMessage(), e.getCause());  
        }  
    }  
  
    @Override  
    public void guardar(Producto producto) {  
        try {  
            repositoryJdbc.guardar(producto);  
        } catch (SQLException e) {  
            throw new ServiceJdbcException(e.getMessage(), e.getCause());  
        }  
    }  
  
    @Override  
    public void eliminar(Long id) {  
        try {  
            repositoryJdbc.eliminar(id);  
        } catch (SQLException e) {  
            throw new ServiceJdbcException(e.getMessage(), e.getCause());  
        }  
    }  
  
    @Override  
    public List<Categoria> listarCategoria() {  
        try {  
            return repositoryCategoriaJdbc.listar();  
        } catch (SQLException e) {  
            throw new ServiceJdbcException(e.getMessage(), e.getCause());  
        }  
    }  
  
    @Override  
    public Optional<Categoria> porIdCategoria(Long id) {  
        try {  
            return Optional.ofNullable(repositoryCategoriaJdbc.porId(id));  
        } catch (SQLException e) {  
            throw new ServiceJdbcException(e.getMessage(), e.getCause());  
        }  
    }  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
//Esta clase la implementamos para que la poder tratar la excepcion, recogerla en ProductoServiceJdbcImpl y pasarla a  
// al ConexionFilter para que haga cerrar la BBDD y no casque error. Asi podremos hacer un rollback  
public class ServiceJdbcException extends RuntimeException{  
  
    public ServiceJdbcException(String message) {  
        super(message);  
    }  
  
    public ServiceJdbcException(String message, Throwable cause) {  
        super(message, cause);  
    }  
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



*---------------------*
           *UTIL:*
*---------------------*
```java
package org.aartaraz.apiservlet.webapp.headers.util;  
  
import java.sql.Connection;  
import java.sql.DriverManager;  
import java.sql.SQLException;  
  
public class ConexionBaseDatos {  
  
    private static String url = "jdbc:mysql://localhost:3306/java_curso?serverTimezone=Europe/Madrid";  
    private static String username= "root";  
    private static String password="";  
  
    public static Connection getConnection() throws SQLException {  
        return DriverManager.getConnection(url,username,password);  
    }  
}
```



*---------------------*
     *WEBAPP*
*---------------------*
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

```jsp
<%@page contentType="text/html" pageEncoding="UTF-8"  
        import="java.util.*, org.aartaraz.apiservlet.webapp.headers.models.*" %>  
<%  
    List<Producto> productos = (List<Producto>) request.getAttribute("productos");  
    Optional<String> username = (Optional<String>) request.getAttribute("username");  
    String mensajeRequest = (String) request.getAttribute("mensaje");  
    String mensajeApp = (String) getServletContext().getAttribute("mensaje");  
%>  
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Listado de productos</title>  
</head>  
<body>  
<h1>Listado de productos</h1>  
<% if (username.isPresent()) {%>  
<div>Hola <%=username.get()%> ,bienvenido!</div>  
<p><a href="<%=request.getContextPath()%>/productos/form">crear [+]</a></p>  
<% } %>  
<table>  
    <tr>  
        <th>id</th>  
        <th>nombre</th>  
        <th>tipo</th>  
        <% if (username.isPresent()) {%>  
        <th>precio</th>  
        <th>agregar</th>  
        <th>editar</th>  
        <th>eliminar</th>  
        <% } %>  
    </tr>  
    <%for (Producto p : productos) {%>  
    <tr>  
        <td><%=p.getId()%>  
        </td>  
        <td><%=p.getNombre()%>  
        </td>  
        <td><%=p.getCategoria().getNombre()%>  
        </td>  
        <% if (username.isPresent()) {%>  
        <td><%=p.getPrecio()%>  
        </td>  
        <td><a href="<%=request.getContextPath()%>/carro/agregar?id=<%=p.getId()%>">agregar al carro</a></td>  
        <td><a href="<%=request.getContextPath()%>/productos/form?id=<%=p.getId()%>">editar</a></td>  
        <td><a onclick="return confirm('esta seguro que desea eliminar?');"  
               href="<%=request.getContextPath()%>/productos/eliminar?id=<%=p.getId()%>">eliminar</a></td>  
        <% } %>  
    </tr>  
    <%}%>  
</table>  
<p><%=mensajeApp%>  
</p>  
<p><%=mensajeRequest%>  
</p>  
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

```jsp
<%@page contentType="text/html" pageEncoding="UTF-8"  
        import="java.util.*, org.aartaraz.apiservlet.webapp.headers.models.*, java.time.format.*" %>  
<%  
    List<Categoria> categorias = (List<Categoria>) request.getAttribute("categorias");  
    Map<String, String> errores = (Map<String, String>) request.getAttribute("errores");  
    Producto producto = (Producto) request.getAttribute("producto");  
    String fecha = producto.getFechaRegistro() != null ?  
            producto.getFechaRegistro().format(DateTimeFormatter.ofPattern("yyyy-MM-dd")) : "";  
%>  
  
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Formulario productos</title>  
</head>  
<body>  
<h1>Formulario productos</h1>  
<form action="<%=request.getContextPath()%>/productos/form" method="post">  
    <div>  
        <label for="nombre">Nombre</label>  
        <div>  
            <input type="text" name="nombre" id="nombre"  
                   value="<%=producto.getNombre()!=null? producto.getNombre(): ""%>">  
        </div>  
        <%if (errores != null && errores.containsKey("nombre")) {%>  
        <div style="color:red;"><%=errores.get("nombre")%>  
        </div>  
        <% } %>  
    </div>  
  
    </div>  
    <div>  
        <label for="precio">Precio</label>  
        <div>  
            <input type="number" name="precio" id="precio"  
                   value="<%=producto.getPrecio()>0? producto.getPrecio(): ""%>">  
        </div>  
        <%if (errores != null && errores.containsKey("precio")) {%>  
        <div style="color:red;"><%=errores.get("precio")%>  
        </div>  
        <% } %>  
    </div>  
    <div>  
        <label for="sku">Sku</label>  
        <div>  
            <input type="text" name="sku" id="sku" value="<%=producto.getSku()!=null? producto.getSku(): ""%>">  
        </div>  
        <%if (errores != null && errores.containsKey("sku")) {%>  
        <div style="color:red;"><%=errores.get("sku")%>  
        </div>  
        <% } %>  
    </div>  
    <div>  
        <label for="fecha_registro">Fecha registro</label>  
        <div>  
            <input type="date" name="fecha_registro" id="fecha_registro" value="<%=fecha%>">  
        </div>  
        <%if (errores != null && errores.containsKey("fecha_registro")) {%>  
        <div style="color:red;"><%=errores.get("fecha_registro")%>  
        </div>  
        <% } %>  
    </div>  
    <div>  
        <label for="categoria">Categoria</label>  
        <div>  
            <select name="categoria" id="categoria">  
                <option value="">--- seleccionar ---</option>  
                <% for (Categoria c : categorias) {%>  
                <option value="<%=c.getId()%>" <%=c.getId().equals(producto.getCategoria().getId()) ? "selected" : ""%>><%=c.getNombre()%>  
                    <% }%>                </option>  
            </select>  
        </div>  
        <%if (errores != null && errores.containsKey("categoria")) {%>  
        <div style="color:red;"><%=errores.get("categoria")%>  
        </div>  
        <% } %>  
    </div>  
  
    <div><input type="submit" value="<%=producto.getId()!=null && producto.getId()>0?"Editar": "Crear"%>"></div>  
  
    <!--El hidden funciona como un flag para saber si tiene que hacer un insert o un update-->  
    <input type="hidden" name="id" value="<%=producto.getId()%>">  
</form>  
</body>  
</html>
```


 







