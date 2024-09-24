![[3_servlet.pdf]]

![[Pasted image 20231019150853.png|400]]

CTRL+ALT+S : Para entrar en el Settings -> Editor -> Live Templates (para personalizar las plantillas que queramos usar).

![[Pasted image 20231019151130.png]]

```java
ackage org.aartaraz.webapp.servlet;  
  
import jakarta.servlet.ServletException;  
import jakarta.servlet.annotation.WebServlet;  
import jakarta.servlet.http.HttpServlet;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
  
import java.io.IOException;  
import java.io.PrintWriter;  
  
@WebServlet("/parametros/url-get")  
public class ParametroGetServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        resp.setContentType("text/html");  
        PrintWriter out = resp.getWriter();  
  
        String saludo = req.getParameter("saludo");  
        String nombre = req.getParameter("nombre");  
  
        out.println("<!DOCTYPE html");  
        out.println("<html>");  
        out.println("    <head>");  
        out.println("        <meta charset=\"UTF-8\">");  
        out.println("        <title>Parametros Get de la url</title>");  
        out.println("    </head>");  
        out.println("    <body>");  
        out.println("        <h1>Parametros Get de la url</h1>");  
        if (saludo != null && nombre != null) {  
            out.println("        <h2>El saludo enviado es:" + saludo + " " + nombre + "</h2>");  
        } else if (saludo != null) {  
            out.println("        <h2>El saludo enviado es: " + saludo + "</h2>");  
        } else if (nombre != null) {  
            out.println("        <h2>El nombre enviado es: " + nombre + "</h2>");  
        } else {  
            out.println("<h2>No se a pasado ni el nombre ni el saludo </h2>");  
        }  
        try {  
            int codigo = Integer.parseInt(req.getParameter("codigo"));  
            out.println("<h3>El codigo enviado es : " + codigo + "</h3>");  
        } catch (NumberFormatException e) {  
            out.println("<h3>El codigo no se ha enviado, es null</h3>");  
        }  
        out.println("    </body>");  
        out.println("</html>");  
        out.close();  
    }  
}
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
<ul>  
    <li>        <a href="/webapp/parametros/url-get?saludo=Hola Arkaitxiki">enviando parámetro saludo : /url-get?saludo=Hola  
        Arkaitxiki</a>  
    </li>    <li>        <a href="/webapp/parametros/url-get?saludo=Hola&nombre=Arkaitz">/url-get?saludo=Hola&nombre=Arkaitz</a>  
        Arkaitxiki</a>  
    </li>    <li>        <a href="/webapp/parametros/url-get?nombre=Arkaitz">enviando parámetro saludo : /url-get?nombre=Arkaitz</a>  
        Arkaitxiki</a>  
    </li>    <li>        <a href="/webapp/parametros/url-get?saludo=Hola&nombre=Arkaitz&codigo=12345">/url-get?saludo=Hola&nombre=Arkaitz</a>  
        Arkaitxiki</a>  
    </li></ul>  
</body>  
</html>
```

