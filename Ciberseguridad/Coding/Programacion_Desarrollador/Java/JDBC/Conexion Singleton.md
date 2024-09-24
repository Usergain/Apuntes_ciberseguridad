![[jdbc.pdf]]


![[Pasted image 20231012214146.png|850]]

![[Pasted image 20231012180629.png|350]]

En este proyecto (pom.xml) solo importamos la dependencia mysql-connector, https://mvnrepository.com/ : 

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<project xmlns="http://maven.apache.org/POM/4.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
    <modelVersion>4.0.0</modelVersion>  
  
    <groupId>org.aartaraz.java.jdbc</groupId>  
    <artifactId>java_jdbc</artifactId>  
    <version>1.0-SNAPSHOT</version>  
  
	<properties> 
		<maven.compiler.source>17</maven.compiler.source>                                                                                                                       <maven.compiler.target>17</maven.compiler.target>  
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
    </properties>  
    <dependencies>
		<dependency>          
			<groupId>mysql</groupId>                                                                                                                                    
            <artifactId>mysql-connector-java</artifactId>  
            <version>8.0.19</version>  
		</dependency>
	<dependencies>	                                                                                                                                              
</project>
```

```java
import java.sql.Connection;  
import java.sql.DriverManager;  
import java.sql.SQLException;  
  
public class ConexionBaseDatos {  
    //Patron Singleton  
    private static String url = "jdbc:mysql://localhost:3306/java_curso?serverTimezone=Europe/Madrid";  
    private static String username = "root";  
    private static String password = "";  
    private static Connection connection;  
  
    public static Connection getInstance() throws SQLException {  
        if (connection == null) {  
            connection = DriverManager.getConnection(url, username, password);  
        }  
        return connection;  
    }  
}
```

```java
package org.aartaraz.java.jdbc.repositorio;  
  
import java.util.List;  
  
// Interfaz Generica - CRUD para administrar BBD  
public interface Repositorio<T> {  
    List<T> listar();  
  
    T porId(Long id);  
  
     void guardar(T t);  
  
     void eliminar(Long id);  
  
}
```

```java
package org.aartaraz.java.jdbc.repositorio;  
  
import org.aartaraz.java.jdbc.modelo.Categoria;  
import org.aartaraz.java.jdbc.modelo.Producto;  
import org.aartaraz.java.jdbc.util.ConexionBaseDatos;  
  
import java.sql.*;  
import java.util.ArrayList;  
import java.util.List;  
  
// Aqui es donde implementaremos el CRUD del patron repositorio (Create, Read, Update, Delete)  
  
public class ProductoRepositorioImpl implements Repositorio<Producto> {  
  
    private Connection getConnection() throws SQLException {  
        return ConexionBaseDatos.getInstance();  
    }  
  
    @Override  
    public List<Producto> listar() {  
        List<Producto> productos = new ArrayList<>();  
  
        //inner join para establecer relacion entre las tablas: productos y categorias MySQL  
        try (Statement stmt = getConnection().createStatement();  
             ResultSet rs = stmt.executeQuery("SELECT p.*, c.nombre as categoria FROM productos as p " +  
                     "inner join categorias as c ON (p.categoria_id = c.id)")) {  
  
            while (rs.next()) {  
                Producto p = crearProducto(rs);  
                productos.add(p);  
            }  
  
        } catch (SQLException e) {  
            throw new RuntimeException(e);  
        }  
        return productos;  
    }  
  
    @Override  
    public Producto porId(Long id) {  
  
        Producto producto = null;  
  
        try (PreparedStatement stmt = getConnection().  
                prepareStatement("SELECT p.*, c.nombre as categoria FROM productos as p " +  
                        "inner join categorias as c ON (p.categoria_id = c.id) WHERE p.id=?")) {  
  
            stmt.setLong(1, id);  
  
            try (ResultSet rs = stmt.executeQuery()) {  
  
                if (rs.next()) {  
                    producto = crearProducto(rs);  
                }  
            }  
  
        } catch (SQLException e) {  
            e.printStackTrace();  
        }  
        return producto;  
    }  
  
    @Override  
    public void guardar(Producto producto) {  
  
        String sql;  
  
        if (producto.getId() != null && producto.getId() > 0) {  
  
            sql = "UPDATE productos SET nombre=?, precio=?, categoria_id=? WHERE id=?";  
  
        } else {  
  
            sql = "INSERT INTO productos(nombre, precio, categoria_id,fecha_registro) VALUES(?,?,?,?)";  
        }  
        try (PreparedStatement stmt = getConnection().prepareStatement(sql)) {  
  
            stmt.setString(1, producto.getNombre());  
            stmt.setLong(2, producto.getPrecio());  
            stmt.setLong(3, producto.getCategoria().getId());  
  
            if (producto.getId() != null && producto.getId() > 0) {  
  
                stmt.setLong(4, producto.getId());  
  
            } else {  
  
                stmt.setDate(4, new Date(producto.getFechaRegistro().getTime()));  
            }  
  
            stmt.executeUpdate();  
  
        } catch (SQLException throwables) {  
            throwables.printStackTrace();  
        }  
  
    }  
  
    @Override  
    public void eliminar(Long id) {  
  
        try (PreparedStatement stmt = getConnection().prepareStatement("DELETE FROM productos WHERE id=?")) {  
  
            stmt.setLong(1, id);  
            stmt.executeUpdate();  
  
        } catch (SQLException throwables) {  
  
            throwables.printStackTrace();  
        }  
    }  
  
    private Producto crearProducto(ResultSet rs) throws SQLException {  
        Producto p = new Producto();  
  
        p.setId(rs.getLong("id"));  
        p.setNombre(rs.getString("nombre"));  
        p.setPrecio(rs.getInt("precio"));  
        p.setFechaRegistro(rs.getDate("fecha_registro"));  
        Categoria categoria = new Categoria();  
        categoria.setId(rs.getLong("categoria_id"));  
        categoria.setNombre(rs.getString("categoria"));  
        p.setCategoria(categoria);  
        return p;  
    }  
}
```

```java
package org.aartaraz.java.jdbc.modelo;  
  
import java.util.Date;  
  
// DAO (DTO) - Patron repositorio (uso: Api Rest, JEE, app distribuidas)  
public class Producto {  
    private long id;  
    private String nombre;  
    private Integer precio;  
    private Date fechaRegistro;  
    private Categoria categoria;  
  
    public Producto(long id, String nombre, Integer precio, Date fechaRegistro) {  
        this.id = id;  
        this.nombre = nombre;  
        this.precio = precio;  
        this.fechaRegistro = fechaRegistro;  
    }  
  
    public Producto() {  
  
    }  
  
    public Long getId() {  
        return id;  
    }  
  
    public void setId(long id) {  
        this.id = id;  
    }  
  
    public String getNombre() {  
        return nombre;  
    }  
  
    public void setNombre(String nombre) {  
        this.nombre = nombre;  
    }  
  
    public Integer getPrecio() {  
        return precio;  
    }  
  
    public void setPrecio(Integer precio) {  
        this.precio = precio;  
    }  
  
    public Date getFechaRegistro() {  
        return fechaRegistro;  
    }  
  
    public void setFechaRegistro(Date fechaRegistro) {  
        this.fechaRegistro = fechaRegistro;  
    }  
  
    public Categoria getCategoria() {  
        return categoria;  
    }  
  
    public void setCategoria(Categoria categoria) {  
        this.categoria = categoria;  
    }  
  
    @Override  
    public String toString() {  
        return id +  
                " | " +  
                nombre +  
                " | " +  
                precio +  
                " | " +  
                fechaRegistro +  
                " | " + categoria.getNombre();  
    }  
}
```

```java
package org.aartaraz.java.jdbc.modelo;  
  
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
package org.aartaraz.java.jdbc;  
  
import org.aartaraz.java.jdbc.modelo.Categoria;  
import org.aartaraz.java.jdbc.modelo.Producto;  
import org.aartaraz.java.jdbc.repositorio.ProductoRepositorioImpl;  
import org.aartaraz.java.jdbc.repositorio.Repositorio;  
import org.aartaraz.java.jdbc.util.ConexionBaseDatos;  
  
import java.sql.*;  
import java.util.Date;  
  
public class EjemploJdbc {  
    public static void main(String[] args) {  
  
        /*Patron Singleton, devuelve la misma instancia. El Pool de conexiones  suele estar inplementado en los  
        servidores de app asi que solo habrá que realizar una sola instancia para la conexion*/                                                                         
		  try (Connection conn = ConexionBaseDatos.getInstance()) {
		  
            Repositorio<Producto> repositorio = new ProductoRepositorioImpl();  
            
            System.out.println(" ================= listar =================");  
            repositorio.listar().forEach(System.out::println);  
            System.out.println(" ================= obtener por id =================");  
            System.out.println(repositorio.porId(1L));  
            System.out.println(" ================= insertar nuevo producto =================");  
            Producto producto = new Producto();  
            producto.setNombre("Teclado Razer mecanico");  
            producto.setPrecio(550);  
            producto.setFechaRegistro(new Date());  
            Categoria categoria=new Categoria();  
            categoria.setId(3L);  
            producto.setCategoria(categoria);  
            repositorio.guardar(producto);  
            System.out.println("Producto guardado con exito");  
            repositorio.listar().forEach(System.out::println);  
  
        } catch (SQLException e) {  
            throw new RuntimeException(e);  
        }  
    }  
}
```

```java
package org.aartaraz.java.jdbc;  
  
import org.aartaraz.java.jdbc.modelo.Categoria;  
import org.aartaraz.java.jdbc.modelo.Producto;  
import org.aartaraz.java.jdbc.repositorio.ProductoRepositorioImpl;  
import org.aartaraz.java.jdbc.repositorio.Repositorio;  
import org.aartaraz.java.jdbc.util.ConexionBaseDatos;  
  
import java.sql.Connection;  
import java.sql.SQLException;  
import java.util.Date;  
  
public class EjemploJdbcUpdate {  
    public static void main(String[] args) {  
  
        /*Patron Singleton, devuelve la misma instancia. El Pool de conexiones  suele estar inplementado en los  
        servidores de app asi que solo habrá que realizar una sola instancia para la conexion*/        try (Connection conn = ConexionBaseDatos.getInstance()) {  
  
            Repositorio<Producto> repositorio = new ProductoRepositorioImpl();  
            System.out.println(" ================= listar =================");  
            repositorio.listar().forEach(System.out::println);  
            System.out.println(" ================= obtener por id =================");  
            System.out.println(repositorio.porId(1L));  
            System.out.println(" ================= editar producto =================");  
            Producto producto = new Producto();  
            producto.setId(5L);  
            producto.setNombre("Teclado Corsair k95 mecánico");  
            producto.setPrecio(700);  
            Categoria categoria=new Categoria();  
            categoria.setId(2L);  
            producto.setCategoria(categoria);  
            repositorio.guardar(producto);  
            System.out.println("Producto editado con exito");  
            repositorio.listar().forEach(System.out::println);  
  
        } catch (SQLException e) {  
            throw new RuntimeException(e);  
        }  
    }  
}
```

```java
package org.aartaraz.java.jdbc;  
  
import org.aartaraz.java.jdbc.modelo.Producto;  
import org.aartaraz.java.jdbc.repositorio.ProductoRepositorioImpl;  
import org.aartaraz.java.jdbc.repositorio.Repositorio;  
import org.aartaraz.java.jdbc.util.ConexionBaseDatos;  
  
import java.sql.Connection;  
import java.sql.SQLException;  
  
public class EjemploJdbcDelete {  
    public static void main(String[] args) {  
  
        /*Patron Singleton, devuelve la misma instancia. El Pool de conexiones  suele estar inplementado en los  
        servidores de app asi que solo habrá que realizar una sola instancia para la conexion*/        try (Connection conn = ConexionBaseDatos.getInstance()) {  
  
            Repositorio<Producto> repositorio = new ProductoRepositorioImpl();  
            System.out.println(" ================= listar =================");  
            repositorio.listar().forEach(System.out::println);  
            System.out.println(" ================= obtener por id =================");  
            System.out.println(repositorio.porId(1L));  
            System.out.println(" ================= eliminar producto =================");  
            repositorio.eliminar(3L);  
            System.out.println("Producto eliminado con exito");  
            repositorio.listar().forEach(System.out::println);  
  
        } catch (SQLException e) {  
            throw new RuntimeException(e);  
        }  
    }  
}
```
