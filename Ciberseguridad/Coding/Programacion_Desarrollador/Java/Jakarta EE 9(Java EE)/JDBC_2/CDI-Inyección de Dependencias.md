![[1_cdi.pdf]]


*-----------*
  *POM*
*-----------*
![[Pasted image 20231108172145.png]]
Pdta. para que funcione las beans trabajaremos con Jakarta 9 la 10 no funciona con Tomcat 10.

Luego para poder trabajar con anotaciones crear fichero *beans.xml* en el directorio *WEB-INF*. Dentro de este archivo pegaremos el siguiente código copiado de la web de jakarta: https://jakarta.ee/specifications/cdi/3.0/jakarta-cdi-spec-3.0, ctrl+f y poner beans.xml para buscar el fragmento de código.

![[Pasted image 20231108175338.png|150]]
```xml
<beans xmlns="https://jakarta.ee/xml/ns/jakartaee"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/beans_3_0.xsd"  
       version="3.0" bean-discovery-mode="annotated">  
  
    <interceptors>  
        <class>org.aartaraz.apiservlet.webapp.headers.interceptor.LoggingInterceptor</class>  
        <class>org.aartaraz.apiservlet.webapp.headers.interceptor.TransactionalInterceptor</class>  
    </interceptors>  
  
</beans>
```

crear un nuevo package *configs*, aquí estarán algunos estereotipos de anotaciones como otras personalizadas, así no tener que dar prioridades sino inyectar directamente:

![[Pasted image 20231109171338.png]]

*----------------*
  *CONFIGS*
*----------------*
*CarroCompra*
```java
package org.aartaraz.apiservlet.webapp.headers.configs;  
  
import jakarta.enterprise.context.SessionScoped;  
import jakarta.enterprise.inject.Stereotype;  
import jakarta.inject.Named;  
  
import java.lang.annotation.ElementType;  
import java.lang.annotation.Retention;  
import java.lang.annotation.Target;  
  
import static java.lang.annotation.RetentionPolicy.RUNTIME;  
  
@SessionScoped  
@Named  
@Stereotype  
@Retention(RUNTIME)  
@Target(ElementType.TYPE)  
public @interface CarroCompra {  
}
```

*MysqlConn*
```java
package org.aartaraz.apiservlet.webapp.headers.configs;  
  
import jakarta.inject.Qualifier;  
  
import java.lang.annotation.Retention;  
import java.lang.annotation.RetentionPolicy;  
import java.lang.annotation.Target;  
  
import static java.lang.annotation.ElementType.*;  
  
@Qualifier  
@Retention(RetentionPolicy.RUNTIME)  
@Target({METHOD, FIELD, PARAMETER, TYPE, CONSTRUCTOR})  
public @interface MysqlConn {  
}
```

*ProducerResources*
```java
package org.aartaraz.apiservlet.webapp.headers.configs;  
  
import jakarta.annotation.Resource;  
import jakarta.enterprise.context.ApplicationScoped;  
import jakarta.enterprise.context.RequestScoped;  
import jakarta.enterprise.inject.Disposes;  
import jakarta.enterprise.inject.Produces;  
import jakarta.enterprise.inject.spi.InjectionPoint;  
import jakarta.inject.Inject;  
  
import javax.naming.NamingException;  
import javax.sql.DataSource;  
import java.sql.Connection;  
import java.sql.SQLException;  
import java.util.logging.Logger;  
  
@ApplicationScoped  
public class ProducerResources {  
  
    @Inject  
    private Logger log;  
  
    @Resource(name = "jdbc/mysqlDB")  
    private DataSource ds;  
  
    @Produces  
    @RequestScoped    
    @MysqlConn    
    private Connection beanConnection() throws NamingException, SQLException {  
	    return ds.getConnection();
    }  
  
    //Info de la metadata  
    @Produces  
    private Logger beanLogger(InjectionPoint injectionPoint) {  
        return Logger.getLogger(injectionPoint.getMember().getDeclaringClass().getName());  
    }  
  
    public void close(@Disposes @MysqlConn Connection connection) throws SQLException {  
        connection.close();  
        log.info("cerrando la conexion a la bbdd mysql");  
    }  
}
```

*ProductoServicePrincipal*
```java
package org.aartaraz.apiservlet.webapp.headers.configs;  
  
import jakarta.inject.Qualifier;  
  
import static java.lang.annotation.ElementType.*;  
  
import java.lang.annotation.Retention;  
import java.lang.annotation.RetentionPolicy;  
import java.lang.annotation.Target;  
  
@Qualifier  
@Retention(RetentionPolicy.RUNTIME)  
@Target({METHOD, FIELD, PARAMETER, TYPE, CONSTRUCTOR})  
public @interface ProductoServicePrincipal {  
}
```

*Repository*
```java
import jakarta.enterprise.context.RequestScoped;  
import jakarta.enterprise.inject.Stereotype;  
import jakarta.inject.Named;  
  
import java.lang.annotation.ElementType;  
import java.lang.annotation.Retention;  
import java.lang.annotation.Target;  
  
import static java.lang.annotation.RetentionPolicy.RUNTIME;  
    
@RequestScoped  
@Named  
@Stereotype  
@Retention(RUNTIME)  
@Target(ElementType.TYPE)  
public @interface Repository {  
}
```

*Service*
```java
package org.aartaraz.apiservlet.webapp.headers.configs;  
  
import jakarta.enterprise.context.ApplicationScoped;  
import jakarta.enterprise.inject.Stereotype;  
import jakarta.inject.Named;  
import org.aartaraz.apiservlet.webapp.headers.interceptor.Logging;  
import org.aartaraz.apiservlet.webapp.headers.interceptor.TransactionalJdbc;  
  
import java.lang.annotation.ElementType;  
import java.lang.annotation.Retention;  
import java.lang.annotation.RetentionPolicy;  
import java.lang.annotation.Target;  
  
@TransactionalJdbc  
@Logging  
@ApplicationScoped  
@Stereotype  
@Named  
@Target(ElementType.TYPE)  
@Retention(RetentionPolicy.RUNTIME)  
public @interface Service {  
}
```

*-------------------*
 *CONTROLLERS*
*-------------------*
*ActualizarCarroServlet*
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.inject.Inject;  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.models.Carro;  
  
import java.io.IOException;  
import java.util.Arrays;  
import java.util.Enumeration;  
import java.util.List;  
  
@WebServlet("/carro/actualizar")  
public class ActualizarCarroServlet extends HttpServlet {  
  
    @Inject  
    private Carro carro;  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        updateProductos(req, carro);  
        updateCantidades(req, carro);    
        resp.sendRedirect(req.getContextPath() + "/carro/ver");  
    }  
  
    private void updateProductos(HttpServletRequest request, Carro carro) {  
        String[] deleteIds = request.getParameterValues("deleteProductos");  
  
        if (deleteIds != null && deleteIds.length > 0) {  
            List<String> productoIds = Arrays.asList(deleteIds);    
            carro.removeProductos(productoIds);  
        }  
    }  
  
    private void updateCantidades(HttpServletRequest request, Carro carro) {  
  
        Enumeration<String> enumer = request.getParameterNames();  
  
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

*AgregarCarroServlet*
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.inject.Inject;  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.configs.ProductoServicePrincipal;  
import org.aartaraz.apiservlet.webapp.headers.models.Carro;  
import org.aartaraz.apiservlet.webapp.headers.models.ItemCarro;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoService;  
  
  
import java.io.IOException;  
import java.util.Optional;  
  
@WebServlet("/carro/agregar")  
public class AgregarCarroServlet extends HttpServlet {  
    @Inject  
    @ProductoServicePrincipal    
    private ProductoService service;   
    @Inject  
    private Carro carro;  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        Long id = Long.valueOf(req.getParameter("id"));  
        Optional<Producto> producto = service.porId(id);  
        if (producto.isPresent()) {  
            ItemCarro item = new ItemCarro(1, producto.get());              
            carro.addItemCarro(item);  
        }  
        resp.sendRedirect(req.getContextPath() + "/carro/ver");  
    }  
}
```

*LonginServlet*
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.inject.Inject;  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.*;  
import org.aartaraz.apiservlet.webapp.headers.models.Usuario;  
import org.aartaraz.apiservlet.webapp.headers.services.*;  
  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.util.Optional;  
  
@WebServlet({"/login", "/login.html"})  
public class LoginServlet extends HttpServlet {  
  
    @Inject  
    private UsuarioService service;  
  
    @Inject  
    private LoginService auth;  
  
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
                out.println("<p><a href='" + req.getContextPath() + "/index.jsp'>volver</a></p>");  
                out.println("<p><a href='" + req.getContextPath() + "/logout'>cerrar sesión</a></p>");  
                out.println("    </body>");  
                out.println("</html>");  
            }  
        } else {  
            req.setAttribute("title", req.getAttribute("title") + ": Login");  
            getServletContext().getRequestDispatcher("/login.jsp").forward(req, resp); // por primera vez y cuando no exista la cookie  
  
        }  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        String username = req.getParameter("username");  
        String password = req.getParameter("password");  
  
        Optional<Usuario> usuarioOptional = service.login(username, password);  
        if (usuarioOptional.isPresent()) {  
            HttpSession session = req.getSession(); //ctrl+alt+v  
            session.setAttribute("username", username);  
  
            resp.sendRedirect(req.getContextPath() + "/login.html"); //Para no tener que abrir un request otra vez cuando vamos atras  
        } else {  
            resp.sendError(HttpServletResponse.SC_UNAUTHORIZED, "Lo sentimos no está autorizado para ingresar a esta página");  
        }  
    }  
}
```

*LogoutServlet*
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.inject.Inject;  
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
  
    @Inject  
    private LoginService auth;  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
  
        Optional<String> username = auth.getUsername(req);  
        if (username.isPresent()) {  
            HttpSession session = req.getSession();  
            session.invalidate(); //borrario todo en de la sesion de usuario  
        }  
        resp.sendRedirect(req.getContextPath() + "/login.html");  
    }  
}
```

*ProductoEliminarServlet*
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.inject.Inject;  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.configs.ProductoServicePrincipal;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoService;  
  
import java.io.IOException;  
import java.util.Optional;  
  
@WebServlet("/productos/eliminar")  
public class ProductoEliminarServlet extends HttpServlet {  
    @Inject  
    @ProductoServicePrincipal    
    private ProductoService service;  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
  
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

*ProductoFormServlet*
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.inject.Inject;  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.configs.ProductoServicePrincipal;  
import org.aartaraz.apiservlet.webapp.headers.models.Categoria;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.services.ProductoService;  
  
import java.io.IOException;  
import java.time.DateTimeException;  
import java.time.LocalDate;  
import java.time.format.DateTimeFormatter;  
import java.util.HashMap;  
import java.util.Map;  
import java.util.Optional;  
  
@WebServlet("/productos/form")  
public class ProductoFormServlet extends HttpServlet {  
    @Inject  
    @ProductoServicePrincipal    private ProductoService service;  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
  
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
        req.setAttribute("title", req.getAttribute("title") + ": Formulario de productos");  
  
        getServletContext().getRequestDispatcher("/form.jsp").forward(req, resp);  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
  
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
            req.setAttribute("title", req.getAttribute("title") + ": Formulario de productos");  
            getServletContext().getRequestDispatcher("/form.jsp").forward(req, resp);  
        }  
    }  
}
```

*ProductoServlet*
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.inject.Inject;  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.configs.ProductoServicePrincipal;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.services.*;  
  
import java.io.IOException;  
import java.util.List;  
import java.util.Optional;  
  
@WebServlet({"/productos.html", "/productos"})  
public class ProductoServlet extends HttpServlet {  
  
    @Inject  
    @ProductoServicePrincipal    private ProductoService service;  
  
    @Inject  
    private LoginService auth;  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
  
        List<Producto> productos = service.listar();  
        Optional<String> usernameOptional = auth.getUsername(req);  
  
        req.setAttribute("productos", productos);  
        req.setAttribute("username", usernameOptional);  
        req.setAttribute("title", req.getAttribute("title") + ": Listado de productos");  
        getServletContext().getRequestDispatcher("/listar.jsp").forward(req, resp);  
    }  
}
```

*VerCarroServlet*
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
        req.setAttribute("title", req.getAttribute("title") + ": Carro de compras");  
        getServletContext().getRequestDispatcher("/carro.jsp").forward(req, resp);  
    }  
}
```

*----------------*
  *FILTERS*
*----------------* 
*ConexionFilter*
```java
package org.aartaraz.apiservlet.webapp.headers.filters;  
  
import jakarta.servlet.*;  
import jakarta.servlet.annotation.WebFilter;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.services.ServiceJdbcException;  
  
import java.io.IOException;  
  
@WebFilter("/*")  
public class ConexionFilter implements Filter {  
  
@Override  
public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {  

    try {       
        filterChain.doFilter(servletRequest, servletResponse);   
    } catch (ServiceJdbcException e) {  
        ((HttpServletResponse) servletResponse).sendError(HttpServletResponse.SC_INTERNAL_SERVER_ERROR, e.getMessage());  
        e.printStackTrace();  
        }  
    }  
}
```

*LoginFilter*
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

Crearemos un package *interceptor*, los interceptor se comportan como los listeners pero para componentes CDI, envuelven los métodos para que hagan algo antes y después. Le da cierta seguridad a la APP.

*-----------------*
*INTERCEPTOR*
*-----------------*
*Logging*
```java
package org.aartaraz.apiservlet.webapp.headers.interceptor;  
  
import jakarta.interceptor.InterceptorBinding;  
  
import java.lang.annotation.ElementType;  
import java.lang.annotation.Retention;  
import java.lang.annotation.RetentionPolicy;  
import java.lang.annotation.Target;  
  
@InterceptorBinding  
@Retention(RetentionPolicy.RUNTIME)  
@Target({ElementType.METHOD, ElementType.TYPE})  
public @interface Logging {  
}
```

*LoggingInterceptor*
```java
package org.aartaraz.apiservlet.webapp.headers.interceptor;  
  
import jakarta.inject.Inject;  
import jakarta.interceptor.AroundInvoke;  
import jakarta.interceptor.Interceptor;  
import jakarta.interceptor.InvocationContext;  
  
import java.util.logging.Logger;  
  
@Logging  
@Interceptor  
public class LoggingInterceptor {  
  
    @Inject  
    private Logger log;  
  
    @AroundInvoke  
    public Object logging(InvocationContext invocation) throws Exception {  
        log.info("***** entrando antes de invocar el metodo " +  
                invocation.getMethod().getName() +  
                " de la clase " + invocation.getMethod().getDeclaringClass());  
        Object resultado = invocation.proceed();  
  
        log.info("***** saliendo de la invocación del método " +  
                invocation.getMethod().getName());  
        return resultado;  
    }  
}
```

*TransactionalJdbc*
```java
package org.aartaraz.apiservlet.webapp.headers.interceptor;  
  
import jakarta.interceptor.InterceptorBinding;  
  
import java.lang.annotation.ElementType;  
import java.lang.annotation.Retention;  
import java.lang.annotation.RetentionPolicy;  
import java.lang.annotation.Target;  
  
@InterceptorBinding  
@Retention(RetentionPolicy.RUNTIME)  
@Target({ElementType.METHOD, ElementType.TYPE})  
public @interface TransactionalJdbc {  
}
```

*TransactionalInterceptor*
```java
package org.aartaraz.apiservlet.webapp.headers.interceptor;  
  
import jakarta.inject.Inject;  
import jakarta.interceptor.AroundInvoke;  
import jakarta.interceptor.Interceptor;  
import jakarta.interceptor.InvocationContext;  
import org.aartaraz.apiservlet.webapp.headers.configs.MysqlConn;  
import org.aartaraz.apiservlet.webapp.headers.services.ServiceJdbcException;  
  
import java.sql.Connection;  
import java.util.logging.Logger;  
  
  
@TransactionalJdbc  
@Interceptor  
public class TransactionalInterceptor {  
  
    @Inject  
    @MysqlConn    private Connection conn;  
  
    @Inject  
    private Logger log;  
  
    @AroundInvoke  
    public Object transactional(InvocationContext invocation) throws Exception {  
        if (conn.getAutoCommit()) {  
            conn.setAutoCommit(false);  
        }  
        try {  
            log.info(" ------> iniciando transacción " + invocation.getMethod().getName() +  
                    " de la clase " + invocation.getMethod().getDeclaringClass());  
            Object resultado = invocation.proceed();  
            conn.commit();  
            log.info(" ------> iniciando transacción " + invocation.getMethod().getName() +  
                    " de la clase " + invocation.getMethod().getDeclaringClass());  
            return resultado;  
        } catch (ServiceJdbcException e) {  
            conn.rollback();  
            throw e;  
        }  
    }  
}
```

*--------------*
 *LISTENERS*
*--------------*
*AplicacionListener*
```java
package org.aartaraz.apiservlet.webapp.headers.listeners;  
  
import jakarta.servlet.*;  
import jakarta.servlet.annotation.WebListener;  
import jakarta.servlet.http.HttpSessionEvent;  
import jakarta.servlet.http.HttpSessionListener;  
  
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
    
    @Override  
    public void requestDestroyed(ServletRequestEvent sre) {  
        sre.getServletContext().log("Destruyendo el request!");  
    }  
  
    @Override  
    public void requestInitialized(ServletRequestEvent sre) {  
        servletContext.log("Inicializando el request!");  
        sre.getServletRequest().setAttribute("mensaje", "guardando valor pare el request");  
        sre.getServletRequest().setAttribute("title", "Catálogo Servlet");  
    }  
  
    @Override  
    public void sessionCreated(HttpSessionEvent se) {  
        servletContext.log("Inicializando la sesión http");  
    }  
  
    @Override  
    public void sessionDestroyed(HttpSessionEvent se) {  
        servletContext.log("Destruyendo la sesión http");  
    }  
}
```

*---------------*
 *MODELS*
*--------------*
*Carro*
```java
package org.aartaraz.apiservlet.webapp.headers.models;  
  
import jakarta.annotation.PostConstruct;  
import jakarta.annotation.PreDestroy;  
import jakarta.inject.Inject;  
import org.aartaraz.apiservlet.webapp.headers.configs.CarroCompra;  
  
import java.io.Serializable;  
import java.util.ArrayList;  
import java.util.List;  
import java.util.Optional;  
import java.util.logging.Logger;  
  
@CarroCompra  
public class Carro implements Serializable {  
    private List<ItemCarro> items;  
  
    //transient para poder usarlo aunque no forme parte de la sesión del carro  
    @Inject  
    private transient Logger log;  
  
    @PostConstruct  
    public void inicializar() {  
        this.items = new ArrayList<>();  
        log.info("inicializando el carro de compras!");  
    }  
  
    @PreDestroy  
    public void destruir() {  
        log.info("destruyendo el carro de compras!");  
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

*Categoria*
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

*ItemCarro*
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

*Producto*
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

*Usuario*
```java
ackage org.aartaraz.apiservlet.webapp.headers.models;  
  
public class Usuario {  
    private Long id;  
    private String username;  
    private String password;  
    private String email;  
  
    public Long getId() {  
        return id;  
    }  
  
    public void setId(Long id) {  
        this.id = id;  
    }  
  
    public String getUsername() {  
        return username;  
    }  
  
    public void setUsername(String username) {  
        this.username = username;  
    }  
  
    public String getPassword() {  
        return password;  
    }  
  
    public void setPassword(String password) {  
        this.password = password;  
    }  
  
    public String getEmail() {  
        return email;  
    }  
  
    public void setEmail(String email) {  
        this.email = email;  
    }  
}
```


*-------------------*
 *REPOSITORIES*
*-------------------*

*CrudRepository*
```java
package org.aartaraz.apiservlet.webapp.headers.repositories;  
  
import java.sql.SQLException;  
import java.util.List;  
  
public interface CrudRepository<T>{  
    List<T> listar() throws SQLException;  
    T porId(Long id) throws SQLException;  
    void guardar(T t) throws SQLException;  
    void eliminar(Long id) throws SQLException;  
}
```

*CategoriaRepositorioImpl*
```java
package org.aartaraz.apiservlet.webapp.headers.repositories;  
  
import jakarta.inject.Inject;  
import org.aartaraz.apiservlet.webapp.headers.configs.MysqlConn;  
import org.aartaraz.apiservlet.webapp.headers.configs.Repository;  
import org.aartaraz.apiservlet.webapp.headers.models.Categoria;  
  
import java.sql.*;  
import java.util.ArrayList;  
import java.util.List;  
  
@Repository  
public class CategoriaRepositorioImpl implements CrudRepository<Categoria> {  
    private Connection conn;  
  
    //Inyección via constructor  
    @Inject  
    public CategoriaRepositorioImpl(@MysqlConn Connection conn) {  
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

*ProductoRepositoryJdbcImpl*
```java
package org.aartaraz.apiservlet.webapp.headers.repositories;  
  
import jakarta.annotation.PostConstruct;  
import jakarta.annotation.PreDestroy;  
import jakarta.inject.Inject;  
import org.aartaraz.apiservlet.webapp.headers.configs.MysqlConn;  
import org.aartaraz.apiservlet.webapp.headers.configs.Repository;  
import org.aartaraz.apiservlet.webapp.headers.models.Categoria;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
  
import java.sql.*;  
import java.util.ArrayList;  
import java.util.List;  
import java.util.logging.Logger;  
  
@Repository  
public class ProductosRepositoryJdbcImpl implements CrudRepository<Producto> {  
  
    @Inject  
    private Logger log;  
  
    @Inject  
    @MysqlConn    private Connection conn;    
  
    @PostConstruct  
    public void inicializar() {  
        log.info("inicializando el beans" + this.getClass().getName());  
    }  
  
    @PreDestroy  
    public void destruir() {  
        log.info("destruyendo el beans " + getClass().getName());  
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
            sql = "update productos set nombre=?,precio=?, sku=?, categoria_id=? where id=?";  
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

*UsuarioRepository*
```java
package org.aartaraz.apiservlet.webapp.headers.repositories;  
  
import org.aartaraz.apiservlet.webapp.headers.models.Usuario;  
  
import java.sql.SQLException;  
  
public interface UsuarioRepository extends CrudRepository<Usuario> {  
    Usuario porUsername(String username) throws SQLException;  
}
```

*UsuarioRepositoryImpl*
```java
package org.aartaraz.apiservlet.webapp.headers.repositories;  
  
import jakarta.inject.Inject;  
import org.aartaraz.apiservlet.webapp.headers.configs.MysqlConn;  
import org.aartaraz.apiservlet.webapp.headers.configs.Repository;  
import org.aartaraz.apiservlet.webapp.headers.models.Usuario;  
  
import java.sql.Connection;  
import java.sql.PreparedStatement;  
import java.sql.ResultSet;  
import java.sql.SQLException;  
import java.util.List;  
  
@Repository  
public class UsuarioRepositoryImpl implements UsuarioRepository {  
   
    @Inject  
    @MysqlConn    private Connection conn;  
  
    @Override  
    public List<Usuario> listar() throws SQLException {  
        return null;  
    }  
  
    @Override  
    public Usuario porId(Long id) throws SQLException {  
        return null;  
    }  
  
    @Override  
    public void guardar(Usuario usuario) throws SQLException {  
  
    }  
  
    @Override  
    public void eliminar(Long id) throws SQLException {  
  
    }  
  
    @Override  
    public Usuario porUsername(String username) throws SQLException {  
        Usuario usuario = null;  
        try (PreparedStatement stmt = conn.prepareStatement("select * from usuarios where username=?")) {  
            stmt.setString(1, username);  
            try (ResultSet rs = stmt.executeQuery()) {  
                if (rs.next()) {  
                    usuario = new Usuario();  
                    usuario.setId(rs.getLong("id"));  
                    usuario.setUsername(rs.getString("username"));  
                    usuario.setPassword(rs.getString("password"));  
                    usuario.setEmail(rs.getString("email"));  
                }  
            }  
        }  
        return usuario;  
    }  
}
```


*---------------*
  *SERVICES*
*---------------*
*ProductoService*
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

*ProductoServiceImpl*
```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import jakarta.enterprise.inject.Alternative;  
import org.aartaraz.apiservlet.webapp.headers.models.Categoria;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
  
import java.util.Arrays;  
import java.util.List;  
import java.util.Optional;  
  
//@Alternative  
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

*ProductoServiceJdbImpl*
```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import jakarta.inject.Inject;  
import org.aartaraz.apiservlet.webapp.headers.configs.ProductoServicePrincipal;  
import org.aartaraz.apiservlet.webapp.headers.configs.Service;  
import org.aartaraz.apiservlet.webapp.headers.models.Categoria;  
import org.aartaraz.apiservlet.webapp.headers.models.Producto;  
import org.aartaraz.apiservlet.webapp.headers.repositories.CrudRepository;  
  
  
import java.sql.SQLException;  
import java.util.List;  
import java.util.Optional;  
  
@Service  
@ProductoServicePrincipal  
public class ProductoServiceJdbcImpl implements ProductoService {  
  
    @Inject  
    private CrudRepository<Producto> repositoryJdbc;  
  
    @Inject  
    private CrudRepository<Categoria> repositoryCategoriaJdbc;  
  
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

*ServiceJdbException*
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

*LoginService*
```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import jakarta.servlet.http.HttpServletRequest;  
  
import java.util.Optional;  
  
public interface LoginService {  
    Optional<String> getUsername(HttpServletRequest request);  
}
```

*LoginServiceCookieImpl*
```java
import jakarta.enterprise.inject.Alternative;  
import jakarta.servlet.http.Cookie;  
import jakarta.servlet.http.HttpServletRequest;  
  
import java.util.Arrays;  
import java.util.Optional;  
   
public class LoginServiceCookieImpl implements LoginService {  
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

*LoginServiceSessionImpl*
```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import jakarta.enterprise.context.ApplicationScoped;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpSession;  
  
import java.util.Optional;  
  
@ApplicationScoped  
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

*UsuarioService*
```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import org.aartaraz.apiservlet.webapp.headers.models.Usuario;  
  
import java.util.Optional;  
  
public interface UsuarioService {  
    Optional<Usuario> login(String username, String password);  
}
```

*UsuarioServiceImpl*
```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import jakarta.inject.Inject;  
import org.aartaraz.apiservlet.webapp.headers.configs.Service;  
import org.aartaraz.apiservlet.webapp.headers.models.Usuario;  
import org.aartaraz.apiservlet.webapp.headers.repositories.UsuarioRepository;  
  
import java.sql.SQLException;  
import java.util.Optional;  
  
@Service  
public class UsuarioServiceImpl implements UsuarioService {  
    private UsuarioRepository usuarioRepository;  
  
    //Injeccion via constructor  
    @Inject  
    public UsuarioServiceImpl(UsuarioRepository usuarioRepository) {  
        this.usuarioRepository = usuarioRepository;  
    }  
  
    @Override  
    public Optional<Usuario> login(String username, String password) {  
        try {  
            return Optional.ofNullable(usuarioRepository.porUsername(username)).filter(u -> u.getPassword().equals(password));  
        } catch (SQLException e) {  
            throw new ServiceJdbcException(e.getMessage(), e.getCause());  
        }  
    }  
}
```

*--------------*
      *UTIL*
*-------------*
*ConexionBaseDatosDS*
```java
package org.aartaraz.apiservlet.webapp.headers.util;  
  
import javax.naming.Context;  
import javax.naming.InitialContext;  
import javax.naming.NamingException;  
import javax.sql.DataSource;  
import java.sql.Connection;  
import java.sql.SQLException;  
  
public class ConexionBaseDatosDS {  
  
    public static Connection getConnection() throws SQLException, NamingException {  
        Context initContext = null;  
        initContext = new InitialContext();  
        Context envContext = (Context) initContext.lookup("java:/comp/env");  
        DataSource ds = (DataSource) envContext.lookup("jdbc/mysqlDB");  
        return ds.getConnection();  
    }  
}
```

La parte webapp, con jsp quedaría igual.




