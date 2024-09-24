![[Pasted image 20231006104826.png|300]]

```java
package org.aartaraz.archivos.ejemplos.servicio;  
  
import java.io.*;  
import java.util.Scanner;  
  
public class ArchivoServicio {  
  
    public void crearArchivo(String nombre) {  
  
/*El BufferWritter es de 8192 caracteres de memoria flotante, hasta que consume todo no se vuelve a invocar  
     No como el FileWriter que cada vez se invoca y machaca el disco duro*/        File archivo = new File(nombre);  
  
        try (BufferedWriter buffer = new BufferedWriter(new FileWriter(archivo, true))) {  
  
            buffer.append("Hola que tal amigos!\n").append("Todo bien?, Yo aqui escribiendo un archivo\n")  
                    .append("Hasta luego Lucas !!\n");  
            //buffer.close(); con el BufferedWriter en el try lo va a cerrar directamente  
            System.out.println("El archivo se ha escrito con éxito");  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  
    }  
  
    public void crearArchivo2(String nombre) {  
  
        /* Si queremos manejar desde consola o terminal, de modo interactivo. El Print trabajo por debajo con un BufferWriter */  
        File archivo = new File(nombre);  
  
        try (PrintWriter buffer = new PrintWriter(new FileWriter(archivo, true))) {  //true, pora que no lo machaque-> lo guarda despues de la última
  
            buffer.println("Hola :-D !");  
            buffer.println(":-D\n");  
            buffer.printf("JAAAARR %s!!!", "Lucas");  
            //buffer.close();  
            System.out.println("El archivo se ha escrito con éxito");  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  
    }  
  
    public String leerArchivo(String nombre) {  
        //Guardaremos todo el contenido del archivo linea a linea  
        StringBuilder sb = new StringBuilder();  
        File archivo = new File(nombre);  
  
        try {  
            BufferedReader reader = new BufferedReader(new FileReader(archivo));  
            String linea;  
  
            while ((linea = reader.readLine()) != null) {  
                sb.append(linea).append("\n");  
            }  
            reader.close();  
        } catch (IOException e) {  
            throw new RuntimeException(e);  
        }  
        return sb.toString();  
    }  
  
    public String leerArchivo2(String nombre) {  
        //Guardaremos todo el contenido del archivo linea a linea  
        StringBuilder sb = new StringBuilder();  
        File archivo = new File(nombre);  
  
        try(Scanner s= new Scanner(archivo)) {  
  
            s.useDelimiter("\n");  
            while (s.hasNext()) {  
  
                sb.append(s.next()).append("\n");  
            }  
            //s.close();  
        } catch (IOException e) {  
            throw new RuntimeException(e);  
        }  
        return sb.toString();  
    }  
}
```

```java
package org.aartaraz.archivos.ejemplos;  
  
import org.aartaraz.archivos.ejemplos.servicio.ArchivoServicio;  
  
public class EjemploCrearArchivo {  
    public static void main(String[] args) {  
  
        String nombreArchivo = "C:\\Program Files\\Java\\archivo\\java.txt";  
  
        ArchivoServicio service = new ArchivoServicio();  
        service.crearArchivo(nombreArchivo);  
  
        service.crearArchivo2(nombreArchivo);  
    }  
}
```

```java
package org.aartaraz.archivos.ejemplos;  
  
import org.aartaraz.archivos.ejemplos.servicio.ArchivoServicio;  
  
public class EjemploLeerArchivo {  
    public static void main(String[] args) {  
  
        String nombreArchivo = "C:\\Program Files\\Java\\archivo\\java.txt";  
        ArchivoServicio servicio = new ArchivoServicio();  
  
        System.out.println(servicio.leerArchivo(nombreArchivo));  
  
        System.out.println(servicio.leerArchivo2(nombreArchivo));  
    }  
}
```

