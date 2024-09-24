Crearemos una BBDD para los usuarios con los datos y contraseña:
![[Pasted image 20231108155316.png|1000]]

*--------------------*
 *CONTROLLERS*
*--------------------*
*LoginServlet.java*
```java
package org.aartaraz.apiservlet.webapp.headers.controllers;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.*;  
import org.aartaraz.apiservlet.webapp.headers.models.Usuario;  
import org.aartaraz.apiservlet.webapp.headers.services.*;  
  
import javax.swing.text.html.Option;  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.sql.Connection;  
import java.util.Optional;  
  
@WebServlet({"/login", "/login.html"})  
public class LoginServlet extends HttpServlet {  
  
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
  
        UsuarioService service = new UsuarioServiceImpl((Connection) req.getAttribute("conn"));  
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

*--------------------*
    *MODELS*
*--------------------*
*Usuario.java*
```java
package org.aartaraz.apiservlet.webapp.headers.models;  
  
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


*-----------------------*
    *REPOSITORIES*
*-----------------------*
*UsuarioRepository.java*
```java
package org.aartaraz.apiservlet.webapp.headers.repositories;  
  
import org.aartaraz.apiservlet.webapp.headers.models.Usuario;  
  
import java.sql.SQLException;  
  
public interface UsuarioRepository extends Repository<Usuario> {  
    Usuario porUsername(String username) throws SQLException;  
}
```

*UsuarioRepository.Impl.java*
```java
import org.aartaraz.apiservlet.webapp.headers.models.Usuario;  
  
import java.sql.Connection;  
import java.sql.PreparedStatement;  
import java.sql.ResultSet;  
import java.sql.SQLException;  
import java.util.List;  
  
public class UsuarioRepositoryImpl implements UsuarioRepository {  
    private Connection conn;  
  
    public UsuarioRepositoryImpl(Connection conn) {  
        this.conn = conn;  
    }  
  
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


*-----------------------*
    *SERVICES*
*-----------------------*
*UsuarioService.java*
```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import org.aartaraz.apiservlet.webapp.headers.models.Usuario;  
  
import java.util.Optional;  
  
public interface UsuarioService {  
    Optional<Usuario> login(String username, String password);  
}
```

```java
package org.aartaraz.apiservlet.webapp.headers.services;  
  
import org.aartaraz.apiservlet.webapp.headers.models.Usuario;  
import org.aartaraz.apiservlet.webapp.headers.repositories.UsuarioRepository;  
import org.aartaraz.apiservlet.webapp.headers.repositories.UsuarioRepositoryImpl;  
  
import java.sql.Connection;  
import java.sql.SQLException;  
import java.util.Optional;  
  
public class UsuarioServiceImpl implements UsuarioService {  
    private UsuarioRepository usuarioRepository;  
  
    public UsuarioServiceImpl(Connection connection) {  
        this.usuarioRepository = new UsuarioRepositoryImpl(connection);  
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

