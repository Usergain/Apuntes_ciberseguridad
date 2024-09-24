 Ahora configuraremos el CRUD como un pool de conexión, para eso visitaremos la pagina web de apache-Tomcat, este es el enlace: https://tomcat.apache.org/tomcat-10.1-doc/jndi-datasource-examples-howto.html#MySQL_DBCP_2_Example

Tendremos que crear dos carpetas nuevas en Webapp, *META-INF* con el archivo *context.xml* y *WEB-INF* con el archivo *web.xml*, donde pegaremos el código de la pagina de apache-Tomcat encada una de los archivos.

![[Pasted image 20231108163732.png]]

![[Pasted image 20231108163626.png]]

*context.xml*
```xml
<Context>  
  
    <!-- maxTotal: Maximum number of database connections in pool. Make sure you  
         configure your mysqld max_connections large enough to handle         all of your db connections. Set to -1 for no limit.         -->  
    <!-- maxIdle: Maximum number of idle database connections to retain in pool.         Set to -1 for no limit.  See also the DBCP 2 documentation on this         and the minEvictableIdleTimeMillis configuration parameter.         -->  
    <!-- maxWaitMillis: Maximum time to wait for a database connection to become available         in ms, in this example 10 seconds. An Exception is thrown if         this timeout is exceeded.  Set to -1 to wait indefinitely.         -->  
    <!-- username and password: MySQL username and password for database connections  -->  
    <!-- driverClassName: Class name for the old mm.mysql JDBC driver is         org.gjt.mm.mysql.Driver - we recommend using Connector/J though.         Class name for the official MySQL Connector/J driver is com.mysql.jdbc.Driver.         -->  
    <!-- url: The JDBC connection url for connecting to your MySQL database.         -->  
    <Resource name="jdbc/mysqlDB" auth="Container" type="javax.sql.DataSource"  
              maxTotal="100" maxIdle="30" maxWaitMillis="10000"  
              username="root" password="" driverClassName="com.mysql.cj.jdbc.Driver"  
              url="jdbc:mysql://localhost:3306/java_curso?serverTimezone=Europe/Madrid"/>  
  
</Context>
```

*web.xml*
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee  
                      https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"         version="6.0">  
    <description>MySQL Test App</description>  
    <resource-ref>  
        <description>DB Connection</description>  
        <res-ref-name>jdbc/mysqlDB</res-ref-name>  
        <res-type>javax.sql.DataSource</res-type>  
        <res-auth>Container</res-auth>  
    </resource-ref>  
</web-app>
```

*--------------*
  *FILTERS*
*--------------*
Modificaremos la clase *ConexionFilter.java*
```java
package org.aartaraz.apiservlet.webapp.headers.filters;  
  
import jakarta.servlet.*;  
import jakarta.servlet.annotation.WebFilter;  
import jakarta.servlet.http.HttpServletResponse;  
import org.aartaraz.apiservlet.webapp.headers.services.ServiceJdbcException;  
import org.aartaraz.apiservlet.webapp.headers.util.ConexionBaseDatos;  
import org.aartaraz.apiservlet.webapp.headers.util.ConexionBaseDatosDS;  
  
import javax.naming.NamingException;  
import java.io.IOException;  
import java.sql.Connection;  
import java.sql.SQLException;  
  
@WebFilter("/*")  
public class ConexionFilter implements Filter {  
    @Override  
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {  
        try (Connection conn = ConexionBaseDatosDS.getConnection()) {  
  
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
        } catch (SQLException | NamingException throwables) {  
            throwables.printStackTrace();  
        }  
    }  
}
```

*--------------*
     *UTIL*
*--------------*
*ConexionBaseDatosDS.java*
```java
package org.aartaraz.apiservlet.webapp.headers.util;  
  
import javax.naming.Context;  
import javax.naming.InitialContext;  
import javax.naming.NamingException;  
import javax.sql.DataSource;  
import java.sql.Connection;  
import java.sql.DriverManager;  
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

