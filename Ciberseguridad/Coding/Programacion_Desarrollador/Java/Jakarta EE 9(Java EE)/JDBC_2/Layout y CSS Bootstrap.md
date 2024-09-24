El CRUD le daremos estilo:

Visitaremos la página web de Bootstrap y copiaremos el enlace: https://getbootstrap.com/docs/5.3/getting-started/download/

Estos enlaces los copiaremos en la cabecera de cada pagina.jsp, por lo que crearemos una pagina header.jsp y otra footer.jsp como Layout, para reutilizarla en las distintas paginas y usar los estilos Bootstrap.

También incrustaremos un navegador con el estilo Bootstrap para que reconduzca a distintas página y tenga distintas funciones, https://getbootstrap.com/docs/5.3/components/navbar/

![[Pasted image 20231108150049.png]]

*----------------------*
       *LAYOUT*
*----------------------*
```jsp
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>${title}</title>  
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN"  
          crossorigin="anonymous">  
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-C6RzsynM9kWDrMNeT87bh95OGNyZPhcTNXj1NW7RuBCsyN/o0jlpcV8Qyq46cDfL"  
            crossorigin="anonymous"></script>  
</head>  
<body>  
<nav class="navbar navbar-expand-lg bg-body-tertiary">  
    <div class="container-fluid">  
        <a class="navbar-brand" href="#">Navbar</a>  
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false"  
                aria-label="Toggle navigation">  
            <span class="navbar-toggler-icon"></span>  
        </button>  
        <div class="collapse navbar-collapse" id="navbarSupportedContent">  
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">  
                <li class="nav-item">  
                    <a class="nav-link active" aria-current="page" href="${pageContext.request.contextPath}/index.jsp">Home</a>  
                </li>  
                <li class="nav-item">  
                    <a class="nav-link" href="${pageContext.request.contextPath}/productos">Productos</a>  
                </li>  
                <li class="nav-item">  
                    <a class="nav-link" href="${pageContext.request.contextPath}/carro/ver">Ver carro (${carro.items.size()})</a>  
                </li>  
                <li class="nav-item dropdown">  
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">  
                        ${not empty sessionScope.username? sessionScope.username: "Cuenta"}  
                    </a>  
                    <ul class="dropdown-menu">  
                        <li><a class="dropdown-item"  
                               href="${pageContext.request.contextPath}/${not empty sessionScope.username? "logout": "login"}">${not empty sessionScope.username? "logout": "login"}</a></li>  
                    </ul>  
                </li>  
            </ul>  
        </div>  
    </div>  
</nav>  
<div class="container">
```

```jsp
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>${title}</title>  
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN"  
          crossorigin="anonymous">  
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-C6RzsynM9kWDrMNeT87bh95OGNyZPhcTNXj1NW7RuBCsyN/o0jlpcV8Qyq46cDfL"  
            crossorigin="anonymous"></script>  
</head>  
<body>  
<nav class="navbar navbar-expand-lg bg-body-tertiary">  
    <div class="container-fluid">  
        <a class="navbar-brand" href="#">Navbar</a>  
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false"  
                aria-label="Toggle navigation">  
            <span class="navbar-toggler-icon"></span>  
        </button>  
        <div class="collapse navbar-collapse" id="navbarSupportedContent">  
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">  
                <li class="nav-item">  
                    <a class="nav-link active" aria-current="page" href="${pageContext.request.contextPath}/index.jsp">Home</a>  
                </li>  
                <li class="nav-item">  
                    <a class="nav-link" href="${pageContext.request.contextPath}/productos">Productos</a>  
                </li>  
                <li class="nav-item">  
                    <a class="nav-link" href="${pageContext.request.contextPath}/carro/ver">Ver carro (${carro.items.size()})</a>  
                </li>  
                <li class="nav-item dropdown">  
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">  
                        ${not empty sessionScope.username? sessionScope.username: "Cuenta"}  
                    </a>  
                    <ul class="dropdown-menu">  
                        <li><a class="dropdown-item"  
                               href="${pageContext.request.contextPath}/${not empty sessionScope.username? "logout": "login"}">${not empty sessionScope.username? "logout": "login"}</a></li>  
                    </ul>  
                </li>  
            </ul>  
        </div>  
    </div>  
</nav>  
<div class="container">
```

*------------------*
   *WEBAPP*
*------------------*   
```jsp
<jsp:include page="layout/header.jsp"/>  
  
<h3>${title}</h3>  
  
<ul class="list-group">  
    <li class="list-group-item active">Menu de opciones</li>  
    <li class="list-group-item"><a href="${pageContext.request.contextPath}/productos">mostrar productos html</a></li>  
    <li class="list-group-item"><a href="${pageContext.request.contextPath}/login.html">Login</a></li>  
    <li class="list-group-item"><a href="${pageContext.request.contextPath}/logout">Logout</a></li>  
    <li class="list-group-item"><a href="${pageContext.request.contextPath}/carro/ver">Ver carro</a></li>  
</ul>  
  
<jsp:include page="layout/footer.jsp"/>
```

```jsp
<%@page contentType="text/html" pageEncoding="UTF-8" %>  
<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>  
<jsp:include page="layout/header.jsp"/>  
  
<h3>${title}</h3>  
  
<c:if test="${username.present}">  
    <div class="alert alert-info">Hola ${username.get()}, bienvenido!</div>  
    <p><a class="btn btn-primary my-2" href="${pageContext.request.contextPath}/productos/form">crear [+]</a></p>  
</c:if>  
  
<table class="table table-hover table-striped">  
    <tr>  
        <th>id</th>  
        <th>nombre</th>  
        <th>tipo</th>  
        <c:if test="${username.present}">  
            <th>precio</th>  
            <th>agregar</th>  
            <th>editar</th>  
            <th>eliminar</th>  
        </c:if>  
    </tr>  
    <c:forEach items="${productos}" var="p">  
        <tr>  
            <td>${p.id}</td>  
            <td>${p.nombre}</td>  
            <td><${p.categoria.nombre}</td>  
            <c:if test="${username.present}">  
                <td><c:out value="${p.precio}"/></td>  
                <td><a class="btn btn-sm btn-primary"  
                       href="${pageContext.request.contextPath}/carro/agregar?id=${p.id}">agregar al  
                    carro</a></td>  
                <td><a class="btn btn-sm btn-success" href="${pageContext.request.contextPath}/productos/form?id=${p.id}">editar</a>  
                </td>  
                <td><a class="btn btn-sm btn-danger" onclick="return confirm('esta seguro que desea eliminar?');"  
                       href="${pageContext.request.contextPath}/productos/eliminar?id=${p.id}">eliminar</a>  
                </td>  
            </c:if>  
        </tr>  
    </c:forEach>  
</table>  
<p>${applicationScope.mensaje}</p>  
<p>${requestScope.mensaje}</p>  
<jsp:include page="layout/footer.jsp"/>
```

```jsp
<%@page contentType="text/html" pageEncoding="UTF-8" %>  
<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>  
<jsp:include page="layout/header.jsp"/>  
  
<h3>${title}</h3>  
  
<c:choose>  
    <c:when test="${sessionScope.carro == null || sessionScope.carro.items.isEmpty()}">  
        <div class="alert alert-warning">Lo sentimos no hay productos en el carro de compras</div>  
    </c:when>  
  
    <c:otherwise>  
        <form name="formcarro" action="${pageContext.request.contextPath}/carro/actualizar" method="post">  
            <table class="table table-hover table-striped">  
                <tr>  
                    <th>id</th>  
                    <th>nombre</th>  
                    <th>precio</th>  
                    <th>cantidad</th>  
                    <th>total</th>  
                    <th>borrar</th>  
                </tr>  
                <c:forEach items="${carro.items}" var="item">  
                    <tr>  
                        <td>${item.producto.id}</td>  
                        <td>${item.producto.nombre}</td>  
                        <td>${item.producto.precio}</td>  
                        <td><input type="text" size="4" name="cant_${item.producto.id}" value="${item.cantidad}"/></td>  
                        <td>${item.importe}</td>  
                        <td><input type="checkbox" value="${item.producto.id}" name="deleteProductos"/></td>  
                    </tr>  
                </c:forEach>  
                <tr>  
                    <td colspan="5" style="text-align: right">Total:</td>  
                    <td>${carro.total}</td>  
                </tr>  
            </table>  
            <a class="btn btn-primary" href="javascript:document.formcarro.submit();">Actualizar</a>  
        </form>  
    </c:otherwise>  
</c:choose>  
<div class="my-2">  
    <a class="btn btn-secondary" href="${pageContext.request.contextPath}/index.jsp">volver</a>  
    <a class="btn btn-success" href="${pageContext.request.contextPath}/productos">seguir comprando</a>  
  
<jsp:include page="layout/footer.jsp"/>
```

```jsp
<%@page contentType="text/html" pageEncoding="UTF-8" import=" java.time.format.*" %>  
<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>  
<jsp:include page="layout/header.jsp"/>  
  
<h3>${title}</h3>  
<form action="${pageContext.request.contextPath}/productos/form" method="post">  
    <div class="row mb-2">  
        <label for="nombre" class="col-form-label col-sm-2">Nombre</label>  
        <div class="col-sm-4">  
            <input type="text" name="nombre" id="nombre" value="${producto.nombre}" class="form-control">  
        </div>  
        <c:if test="${errores != null && errores.containsKey('nombre')}">  
            <div style="color:red;">${errores.nombre}</div>  
        </c:if>  
    </div>  
  
    <div class="row mb-2">  
        <label for="precio" class="col-form-label col-sm-2">Precio</label>  
        <div class="col-sm-4">  
            <input type="number" name="precio" id="precio" value="${producto.precio > 0? producto.precio:""}" class="form-control">  
        </div>  
        <c:if test="${errores != null && !empty errores.precio}">  
            <div style="color:red;">${errores.precio}</div>  
        </c:if>  
    </div>  
    <div class="row mb-2">  
        <label for="sku" class="col-form-label col-sm-2">Sku</label>  
        <div class="col-sm-4">  
            <input type="text" name="sku" id="sku" value="${producto.sku}" class="form-control">  
        </div>  
        <c:if test="${errores != null && !empty errores.sku}">  
            <div style="color:red;">${errores.sku}</div>  
        </c:if>  
    </div>  
    <div class="row mb-2">  
        <label for="fecha_registro" class="col-form-label col-sm-2">Fecha registro</label>  
        <div class="col-sm-4">  
            <input class="form-control" type="date" name="fecha_registro" id="fecha_registro"  
                   value="${producto.fechaRegistro != null ?producto.fechaRegistro.format(DateTimeFormatter.ofPattern("yyyy-MM-dd")) : ""}">  
        </div>  
        <c:if test="${errores != null && !empty errores.fecha_registro}">  
            <div style="color:red;">${errores.fecha_registro}</div>  
        </c:if>  
    </div>  
    <div class="row mb-2">  
        <label for="categoria" class="col-form-label col-sm-2">Categoria</label>  
        <div class="col-sm-4">  
            <select name="categoria" id="categoria" class="form-select">  
                <option value="">--- seleccionar ---</option>  
                <c:forEach items="${categorias}" var="c">  
                    <option value="${c.id}" ${c.id.equals(producto.categoria.id) ? "selected" : ""}>${c.nombre}</option>  
                </c:forEach>  
            </select>  
        </div>  
        <c:if test="${errores != null && !empty errores.categoria}">  
            <div style="color:red;">${errores.categoria}</div>  
        </c:if>  
    </div>  
  
    <div class="row mb-2">  
        <div>  
            <input class="btn btn-primary" type="submit" value="${producto.id!=null && producto.id>0?"Editar": "Crear"}">  
        </div>  
    </div>  
  
    <!--El hidden funciona como un flag para saber si tiene que hacer un insert o un update-->  
    <input type="hidden" name="id" value="${producto.id}">  
</form>  
<jsp:include page="layout/footer.jsp"/>
```

```jsp
<%@page contentType="text/html" pageEncoding="UTF-8" %>  
<jsp:include page="layout/header.jsp"/>  
  
<h3>${title}</h3>  
<form action="${pageContext.request.contextPath}/login" method="post">  
    <div class="row my-2">  
        <label for="username" class="form-label">Username</label>  
        <div>  
            <input type="text" name="username" id="username" class="=form-control">  
        </div>  
    </div>  
    <div class="row my-2">  
        <label for="password" class="form-label">Password</label>  
        <div>  
            <input type="password" name="password" id="password" class="=form-control">  
        </div>  
    </div>  
    <div class=row my-2>  
        <input type="submit" value="Login" class="btn btn-primary">  
    </div>  
</form>  
  
<jsp:include page="layout/footer.jsp"/>
```

Como hemos podido apreciar el index.html lo hemos cambiado por index.jsp, y los servlet con ese ruta también.
En *ProductosFormServlet.java*  :
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
        req.setAttribute("title", req.getAttribute("title") + ": Formulario de productos");  
  
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
            req.setAttribute("title", req.getAttribute("title") + ": Formulario de productos");  
            getServletContext().getRequestDispatcher("/form.jsp").forward(req, resp);  
        }  
    }  
}```

Y en *ProductoServlet* :
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
        req.setAttribute("title", req.getAttribute("title") + ": Listado de productos");  
        getServletContext().getRequestDispatcher("/listar.jsp").forward(req, resp);  
    }  
}
```

Asi poder recoger el titulo de cada jsp que abramos.

![[Pasted image 20231108151515.png]]
![[Pasted image 20231108193020.png]]
![[Pasted image 20231108151604.png]]
![[Pasted image 20231108151620.png]]

