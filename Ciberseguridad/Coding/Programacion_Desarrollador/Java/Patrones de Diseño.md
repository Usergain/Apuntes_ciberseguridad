1- **Singleton Method** o **instancia única**, es un [patrón de diseño](https://es.wikipedia.org/wiki/Patr%C3%B3n_de_dise%C3%B1o "Patrón de diseño") que restringe la creación a un único objeto la creación de [objetos](https://es.wikipedia.org/wiki/Programaci%C3%B3n_orientada_a_objetos "Programación orientada a objetos") pertenecientes a una [clase](https://es.wikipedia.org/wiki/Clase_(programaci%C3%B3n_orientada_a_objetos) "Clase (programación orientada a objetos)") y asegura de que sólo haya esta instancia única :

![[Pasted image 20231005193020.png|400]]

![[Pasted image 20231005193144.png]]


```java
package org.aartaraz.patrones.singleton 

public class ConexionBDSingleton {  
  
    //Asi lo podemos ejecutar en cualquier lado  
    private static ConexionBDSingleton instancia;  
  
    //Constructor privado. Solo se crea una sola instancia - SINGLETON  
    private ConexionBDSingleton() {  
        System.out.println("Conectandose algún motor de base de datos");  
    }  
  
    public static ConexionBDSingleton getInstancia() {  
        if (instancia == null) {  
            instancia = new ConexionBDSingleton();  
        }  
        return instancia;  
    }  
}
```

```java
package org.aartaraz.patrones.singleton;  
  
public class EjemploSingleton {  
  
    public static void main(String[] args) {  
  
        ConexionBDSingleton con = null;  
  
        for (int i = 0; i < 10; i++) {  
  
            con = ConexionBDSingleton.getInstancia();  
            System.out.println("con = " + con);  
        }  
  
        ConexionBDSingleton con2 = ConexionBDSingleton.getInstancia();  
        ConexionBDSingleton con3 = ConexionBDSingleton.getInstancia();  
  
        boolean b1 = ((con == con2) && (con2 == con3));  
        System.out.println("b1 = " + b1);  
    }  
  
}
```

2- **Factory** Este patrón, pretende resolver problemas recurrentes con un diseño flexible y reusable para software orientado a objetos. Específicamente el método Factory resuelve como un objeto puede ser creado haciendo que las subclases puedan decidir que clases instanciar y como una clase puede diferir la instanciación de subclases:

![[Pasted image 20231005193446.png]]

![[Pasted image 20231005193519.png|300]]

```java
package org.aartaraz.patrones.factory;  
  
abstract public class PizzeriaZonaAbstractFactory {  
  
    public PizzaProducto ordenarPizza(String tipo) {  
        PizzaProducto pizza = crearPizza(tipo);  
        System.out.println("------------Fabricando la pizza " + pizza.getNombre() + "---------");  
        pizza.cocinar();  
        pizza.cocinar();  
        pizza.empaquetar();  
        return pizza;  
    }  
  
    abstract PizzaProducto crearPizza(String tipo);  
}
```

```java
package org.aartaraz.patrones.factory;  
  
import java.util.ArrayList;  
import java.util.List;  
  
abstract public class PizzaProducto {  
  
    protected String nombre;  
    protected String masa;  
    protected String salsa;  
    protected List<String> ingredientes;  
  
    public PizzaProducto() {  
        this.ingredientes = new ArrayList<>();  
    }  
  
    public void preparar() {  
        System.out.println("Preparando " + nombre);  
        System.out.println("Seleccionando la masa" + masa);  
        System.out.println("Agregando salsa " + salsa);  
        System.out.println("Agregando ingredientes: ");  
        //this.ingredientes.forEach(i-> System.out.println(i)); lo abreviamos como abajo  
        this.ingredientes.forEach(System.out::println);  
    }  
  
    abstract public void cocinar();  
  
    abstract public void cortar();  
  
    public void empaquetar() {  
        System.out.println("Poniendo la piza en una caja de empaque.. ");  
    }  
  
    public String getNombre() {  
        return nombre;  
    }  
  
    @Override  
    public String toString() {  
        return "PizaProducto{" +  
                "nombre='" + nombre + '\'' +  
                ", masa='" + masa + '\'' +  
                ", salsa='" + salsa + '\'' +  
                ", ingredientes=" + ingredientes +  
                '}';  
    }  
}
```

```java
package org.aartaraz.patrones.factory;  
  
import org.aartaraz.patrones.factory.producto.PizzaCaliforniaPepperoni;  
import org.aartaraz.patrones.factory.producto.PizzaCaliforniaQueso;  
import org.aartaraz.patrones.factory.producto.PizzaCaliforniaVegetariana;  
  
import java.sql.SQLOutput;  
  
public class PizzeriaCaliforniaFactory extends PizzeriaZonaAbstractFactory {  
  
    @Override  
    PizzaProducto crearPizza(String tipo) {  
        PizzaProducto producto = null;  
        switch (tipo) {  
            case "queso":  
                producto = new PizzaCaliforniaQueso();  
                break;            case "pepperoni":  
                producto = new PizzaCaliforniaPepperoni();  
                break;            case "vegetariana":  
                producto = new PizzaCaliforniaVegetariana();  
                break;        }  
        return producto;  
    }  
}
```

```java
package org.aartaraz.patrones.factory;  
  
import org.aartaraz.patrones.factory.producto.PizzaNewYorkItaliana;  
import org.aartaraz.patrones.factory.producto.PizzaNewYorkPepperoni;  
import org.aartaraz.patrones.factory.producto.PizzaNewYorkVegetariana;  
  
public class PizzeriaNewYorkFactory extends PizzeriaZonaAbstractFactory {  
  
    @Override  
    PizzaProducto crearPizza(String tipo) {  
  
        return switch (tipo) {  
            case "vegetariana" -> new PizzaNewYorkVegetariana();  
            case "pepperoni" -> new PizzaNewYorkPepperoni();  
            case "italiana" -> new PizzaNewYorkItaliana();  
            default -> null;  
        };  
    }  
}
```

```java
package org.aartaraz.patrones.factory.ejemplo;  
  
import org.aartaraz.patrones.factory.PizzaProducto;  
import org.aartaraz.patrones.factory.PizzeriaCaliforniaFactory;  
import org.aartaraz.patrones.factory.PizzeriaNewYorkFactory;  
import org.aartaraz.patrones.factory.PizzeriaZonaAbstractFactory;  
  
public class EjemploFactory {  
    public static void main(String[] args) {  
        PizzeriaZonaAbstractFactory ny = new PizzeriaNewYorkFactory();  
        PizzeriaZonaAbstractFactory california = new PizzeriaCaliforniaFactory();  
  
        PizzaProducto pizza = california.ordenarPizza("queso");  
        System.out.println("Bruce ordena la pizzaa " + pizza.getNombre());  
  
        pizza = ny.ordenarPizza("pepperoni");  
        System.out.println("Arkaitz ordena una " + pizza.getNombre());  
  
        pizza =california.ordenarPizza("vegetariana");  
        System.out.println("Linus ordena la pizza " + pizza.getNombre());  
  
        pizza = ny.ordenarPizza("vegetariana");  
        System.out.println("James ordena una " + pizza.getNombre());  
  
        pizza =california.ordenarPizza("pepperoni");  
        System.out.println("John ordena la pizza " + pizza.getNombre());  
  
        System.out.println("pizza = " + pizza);  
    }  
}
```

3- **Decorator** , responde a la necesidad de añadir dinámicamente funcionalidad a un Objeto. Esto nos permite no tener que crear sucesivas clases que hereden de la primera incorporando la nueva funcionalidad, sino otras que la implementan y se asocian a la primera.

![[Pasted image 20231005195249.png]]

![[Pasted image 20231005195332.png|300]]

```java
package org.aartaraz.patrones.decorator2;  
  
public interface Configurable {  
  
    float getPrecioBase();  
    String getIngredientes();  
}
```

```java
package org.aartaraz.patrones.decorator2;  
  
public class Cafe implements Configurable{  
    private float precio;  
    private String nombre;  
  
    public Cafe(String nombre, float precio) {  
        this.nombre = nombre;  
        this.precio = precio;  
    }  
  
    @Override  
    public float getPrecioBase() {  
        return this.precio;  
    }  
  
    @Override  
    public String getIngredientes() {  
        return this.nombre;  
    }  
}
```


```java
package org.aartaraz.patrones.decorator2.decorador;  
  
import org.aartaraz.patrones.decorator2.Configurable;  
  
abstract public class CafeDecorador implements Configurable {  
    protected Configurable cafe;  
  
    public CafeDecorador(Configurable cafe) {  
        this.cafe = cafe;  
    }  
}
```

```java
package org.aartaraz.patrones.decorator2.decorador;  
  
import org.aartaraz.patrones.decorator2.Configurable;  
  
public class ConChocolateDecorador extends CafeDecorador{  
  
    public ConChocolateDecorador(Configurable cafe) {  
        super(cafe);  
    }  
  
    @Override  
    public float getPrecioBase() {  
        return cafe.getPrecioBase() + 5f;  
    }  
  
    @Override  
    public String getIngredientes() {  
        return cafe.getIngredientes() + ", Chocolate";  
    }  
}
```

```java
package org.aartaraz.patrones.decorator2.decorador;  
  
import org.aartaraz.patrones.decorator2.Configurable;  
  
public class ConCremaDecorador extends CafeDecorador{  
    public ConCremaDecorador(Configurable cafe) {  
        super(cafe);  
    }  
  
    @Override  
    public float getPrecioBase() {  
        return cafe.getPrecioBase() + 2.5f;  
    }  
  
    @Override  
    public String getIngredientes() {  
        return cafe.getIngredientes() + ", Crema";  
    }  
}
```

```java
package org.aartaraz.patrones.decorator2.decorador;  
  
import org.aartaraz.patrones.decorator2.Configurable;  
  
public class ConLecheDecorador extends CafeDecorador{  
    public ConLecheDecorador(Configurable cafe) {  
        super(cafe);  
    }  
  
    @Override  
    public float getPrecioBase() {  
        return cafe.getPrecioBase() + 3.7f;  
    }  
  
    @Override  
    public String getIngredientes() {  
        return cafe.getIngredientes() + ", Leche";  
    }  
}
```

4- ***Composite*** , es un patrón de diseño de software que permite componer objetos en estructuras de árbol para representar jerarquías de objetos de manera uniforme. Esto permite tratar a objetos individuales y a sus composiciones de manera uniforme, lo que es útil para crear estructuras complejas compuestas por objetos simples o compuestos.

![[Pasted image 20231006101300.png| 450]]

![[Pasted image 20231006101521.png|300]]

```java
package org.aartaraz.patrones.composite;

import java.util.Objects;

abstract public class Componente {
    protected String nombre;

    public Componente(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    abstract public String mostrar(int nivel);
    abstract public boolean buscar(String nombre);

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Componente that = (Componente) o;
        return Objects.equals(nombre, that.nombre);
    }

    @Override
    public int hashCode() {
        return Objects.hash(nombre);
    }
}
```

```java
package org.aartaraz.patrones.composite;

import java.util.ArrayList;
import java.util.List;

public class Directorio extends Componente {

    private List<Componente> hijos;

    public Directorio(String nombre) {
        super(nombre);
        this.hijos = new ArrayList<>();
    }

    public Directorio addComponente(Componente c) {
        hijos.add(c);
        return this;
    }

    public void removeComponente(Componente c) {
        hijos.remove(c);
    }

    @Override
    public String mostrar(int nivel) {

        StringBuilder sb = new StringBuilder("\t".repeat(nivel));
        sb.append(nombre)
                //Los directorios
                .append("/")
                .append("\n");
        for (Componente hijo : this.hijos) {
            //Los archivos
            sb.append(hijo.mostrar(nivel + 1));
            if (hijo instanceof Archivo) {
                sb.append("\n");
            }
        }
        return sb.toString();
    }

    @Override
    public boolean buscar(String nombre) {
        if (this.nombre.equalsIgnoreCase(nombre)) {
            return true;
        }
        return hijos.stream().anyMatch(h -> h.buscar(nombre));
        /*
        for (Componente hijo : this.hijos) {
            if (hijo.buscar(nombre)) {
                return true;
            }
        }
        return false
       */
    }
} 
```

```java
package org.aartaraz.patrones.composite;

public class Archivo extends Componente{
    public Archivo(String nombre) {
        super(nombre);
    }

    @Override
    public String mostrar(int nivel) {
        return "\t".repeat(nivel) + nombre;
    }

    @Override
    public boolean buscar(String nombre) {
        return this.nombre.equalsIgnoreCase(nombre);
    }
}
```

```java
package org.aartaraz.patrones.composite;

import org.aartaraz.patrones.composite.Archivo;
import org.aartaraz.patrones.composite.Directorio;

public class EjemploComposite {
    public static void main(String[] args) {

        //Estructura de arbol
        Directorio doc = new Directorio("Documentos");
        Directorio java = new Directorio("Java");

        java.addComponente(new Archivo("patron-composite.docx"));
        Directorio stream = new Directorio("Api Stream");
        stream.addComponente(new Archivo("strean-mao.docx"));

        java.addComponente(stream);

        doc.addComponente(java);
        doc.addComponente(new Archivo("cv.docx"));
        doc.addComponente(new Archivo("logo.jpeg"));

        System.out.println(doc.mostrar(0));
    }
}
```

```java
org.aartaraz.patrones.composite.ejemplo;

import org.aartaraz.patrones.composite.Archivo;
import org.aartaraz.patrones.composite.Directorio;

public class EjemploCompositeBuscar {
    public static void main(String[] args) {

        //Estructura de arbol
        Directorio doc = new Directorio("Documentos");
        Directorio java = new Directorio("Java");

        java.addComponente(new Archivo("patron-composite.docx"));
        Directorio stream = new Directorio("Api Stream");
        stream.addComponente(new Archivo("strean-mao.docx"));

        java.addComponente(stream);

        doc.addComponente(java);
        doc.addComponente(new Archivo("cv.docx"));
        doc.addComponente(new Archivo("logo.jpeg"));

        boolean encontrado=doc.buscar("patron-composite.docx");
        System.out.println("Encontrado \"patron-composite.docx\": " + encontrado);

        encontrado = doc.buscar("Api Strem");
        System.out.println("Encontrado Api Stream: " + encontrado);

        encontrado = doc.buscar("cv.docx");
        System.out.println("Encontrado cv.docx: " + encontrado);
    }
}
```

5- **Observer** , es un patrón de diseño de software que permite definir una dependencia uno a muchos entre objetos, de manera que cuando un objeto cambia su estado, todos los objetos que dependen de él son notificados y actualizados automáticamente. Este patrón se utiliza para lograr una comunicación eficiente y desacoplada entre objetos, donde un objeto observador (o varios) supervisa y reacciona a los cambios en un objeto sujeto (o publicador).

![[Pasted image 20231006103002.png||500]]

![[Pasted image 20231006103254.png||300]]

```java
package org.aartaraz.patrones.observer;

public interface Observer {
    void update(Observable observable,  Object obj);
} 
```

```java
package org.aartaraz.patrones.observer;

import java.util.ArrayList;
import java.util.List;

public class UsuarioRepositorio extends Observable{

    private List<String> repositorio =new ArrayList<>();

    public void crearUsuario(String usuario){
        repositorio.add(usuario);
        notifyObservers(usuario);
    }
}   
```

```java
package org.aartaraz.patrones.observer;

import java.util.ArrayList;
import java.util.List;

abstract public class Observable {
    protected List<Observer> observers = new ArrayList<>();

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    public void remove(Observer observer) {
        observers.remove(observer);
    }

    public void notifyObservers() {
        /*for (Observer obs : this.observers) {
            obs.update(this);
        }*/
        notifyObservers(null);
    }

    public void notifyObservers(Object obj){
        for(Observer obs: this.observers){
            obs.update(this, obj);
        }
    }
}
```

```java
package org.aartaraz.patrones.observer;

public class Corporacion extends Observable{
    private String nombre;
    private int precio;

    public Corporacion(String nombre, int precio) {
        this.nombre = nombre;
        this.precio = precio;
    }

    public String getNombre() {
        return nombre;
    }

    public int getPrecio() {
        return precio;
    }

    public void modificaPrecio(int precio){
        this.precio=precio;
        notifyObservers();
    }

    @Override
    public String toString() {
        return getNombre() +
                " nuevo precio $ " +
                getPrecio();
    }
}  
```

```java
package org.aartaraz.patrones.observer.ejemplos;

import org.aartaraz.patrones.observer.Corporacion;

public class EjemploObserver {
    public static void main(String[] args) {

        Corporacion google = new Corporacion("Google", 1000);

        google.addObserver((observable, obj) -> {
            System.out.println("John: " + observable);
        });

        google.addObserver((observable, obj) -> {
            System.out.println("Michael: " + observable);
        });

        google.addObserver((observable, obj) -> {
            System.out.println("Jimmy: " + ((Corporacion) observable).getNombre() +
                    " nuevo precio $ " +
                    ((Corporacion) observable).getPrecio());
        });

        google.modificaPrecio(2000);
    }
}  
```

```java
package org.aartaraz.patrones.observer.ejemplos;

import org.aartaraz.patrones.observer.UsuarioRepositorio;

public class EjemploObserver2 {
    public static void main(String[] args) {

        UsuarioRepositorio repo = new UsuarioRepositorio();

        repo.addObserver((o, u)-> System.out.println("Enviando un email al usuario " + u ));
        repo.addObserver((o, u)-> System.out.println("Enviando un email al administrador "));
        repo.addObserver((o, u)-> System.out.println("Guardadno info del usuario en el logs" + u +" en el logs "));
        repo.crearUsuario("Arkaitz");
    }
} 
```



  

