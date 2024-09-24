De esta forma dejaremos de trabajar con scriptless en jsp, facilita mucho todo, es más limpia y amigable -> Lenguanje de Expresión(EL).

Lo primero sería instalar una dependencia en pom:
![[Pasted image 20231107155600.png]]

*---------------------*
     *WEBAPP:*
*---------------------*
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
<%@page contentType="text/html" pageEncoding="UTF-8" %>  
<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>  
  
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Listado de productos</title>  
</head>  
<body>  
<h1>Listado de productos</h1>  
  
<c:if test="${username.present}">  
    <div>Hola ${username.get()}, bienvenido!</div>  
    <p><a href="${pageContext.request.contextPath}/productos/form">crear [+]</a></p>  
</c:if>  
  
<table>  
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
                <td><a href="${pageContext.request.contextPath}/carro/agregar?id=${p.id}">agregar al  
                    carro</a></td>  
                <td><a href="${pageContext.request.contextPath}/productos/form?id=${p.id}">editar</a>  
                </td>  
                <td><a onclick="return confirm('esta seguro que desea eliminar?');"  
                       href="${pageContext.request.contextPath}/productos/eliminar?id=${p.id}">eliminar</a>  
                </td>  
            </c:if>  
        </tr>  
    </c:forEach>  
</table>  
<p>${applicationScope.mensaje}</p>  
<p>${requestScope.mensaje}</p>  
</body>  
</html>
```

```jsp
<%@page contentType="text/html" pageEncoding="UTF-8" import=" java.time.format.*" %>  
<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>  
  
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Formulario productos</title>  
</head>  
<body>  
<h1>Formulario productos</h1>  
<form action="${pageContext.request.contextPath}/productos/form" method="post">  
    <div>  
        <label for="nombre">Nombre</label>  
        <div>  
            <input type="text" name="nombre" id="nombre" value="${producto.nombre}">  
        </div>  
        <c:if test="${errores != null && errores.containsKey('nombre')}">  
            <div style="color:red;">${errores.nombre}</div>  
        </c:if>  
    </div>  
  
    </div>  
    <div>  
        <label for="precio">Precio</label>  
        <div>  
            <input type="number" name="precio" id="precio" value="${producto.precio > 0? producto.precio:""}">  
        </div>  
        <c:if test="${errores != null && !empty errores.precio}">  
            <div style="color:red;">${errores.precio}</div>  
        </c:if>  
    </div>  
    <div>  
        <label for="sku">Sku</label>  
        <div>  
            <input type="text" name="sku" id="sku" value="${producto.sku}">  
        </div>  
        <c:if test="${errores != null && !empty errores.sku}">  
            <div style="color:red;">${errores.sku}</div>  
        </c:if>  
    </div>  
    <div>  
        <label for="fecha_registro">Fecha registro</label>  
        <div>  
            <input type="date" name="fecha_registro" id="fecha_registro" value="${producto.fechaRegistro != null ?  
            producto.fechaRegistro.format(DateTimeFormatter.ofPattern("yyyy-MM-dd")) : ""}">  
        </div>  
        <c:if test="${errores != null && !empty errores.fecha_registro}">  
            <div style="color:red;">${errores.fecha_registro}</div>  
        </c:if>  
    </div>  
    <div>  
        <label for="categoria">Categoria</label>  
        <div>  
            <select name="categoria" id="categoria">  
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
  
    <div><input type="submit" value="${producto.id!=null && producto.id>0?"Editar": "Crear"}"></div>  
  
    <!--El hidden funciona como un flag para saber si tiene que hacer un insert o un update-->  
    <input type="hidden" name="id" value="${producto.id}">  
</form>  
</body>  
</html>
```

```jsp
<%@page contentType="text/html" pageEncoding="UTF-8" %>  
<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>  
  
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Carro de Compras</title>  
</head>  
<body>  
<h1>Carro de Compras</h1>  
  
<c:choose>  
    <c:when test="${sessionScope.carro == null || sessionScope.carro.items.isEmpty()}">  
        <p>Lo sentimos no hay productos en el carro de compras</p>  
    </c:when>  
  
    <c:otherwise>  
        <form name="formcarro" action="${pageContext.request.contextPath}/carro/actualizar" method="post">  
            <table>  
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
                    <td colspan="4" style="text-align: right">Total:</td>  
                    <td>${carro.total}</td>  
                </tr>  
            </table>  
            <a href="javascript:document.formcarro.submit();">Actualizar</a>  
        </form>  
    </c:otherwise>  
</c:choose>  
<p><a href="${pageContext.request.contextPath}/productos">seguir comprando</a></p>  
<p><a href="${pageContext.request.contextPath}/index.html">volver</a></p>  
</body>  
</html>
```


