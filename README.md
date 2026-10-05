# Principios-de-dise-o
Curso de principios de diseño
#SESION 1
# SRP
separacion de responsavilida unica
``` java
// este clas rompe con el pricipio SRP
public class Empledo{
     public void  calcularSueldo();

     public void enviarEmail();

     public void activarBono();
}


// este sera un eje,plo de SRP
public class Empleado{
 // atributos
 double sueldo;
 // constructor

}

public class CalcularBono{
    public double calcular(Empleado empleado){
        return empleado.sueldo*0.1;
    }
}

public class EmviarEmail{
    public void enviar(Empleado empleado){
              System.ut.printl(" enviando email s");

    }
}
```
# LSP
todos los hijos pueden reemplazar a su clase padre  sin romper los comportamientos
``` java

// aca si se cumple con LSP
public class Animal {
     public void comer(){
        System.ut.printl("animal come");
     }
}

public Perro extends Animal{
    @Override
    public void comer(){
        System.ut.printl("animal come");

    }
}

Animal animal= new Perro();
animal.comer(); // OK


// aca no se cumple con LSP
  
 public class  Animal{
    public void volar(){

    }
 }


 public class Pinguino extends Animal{
      @Override
    public void volar(){
        // Error  un pinguino no pude volar

    }

 }

 Animal animal= new Pinguino();
 animal.volar(); // ERROR

 ``` 

 # OCP

 abierto a agregar cerrado a modificar

  ```  java
   public class CalularBono{
     public double calcular(String tipo,sueldo){
        if(tipo.equals("Normal")) return sueldo *0.1;

        if(tipo.equals("Gernte")) return sueldo *0.1;
       if(tipo.equals("ADmisnistrador")) return sueldo *0.1;


     }
   }
   // si creo una nueva  tipo de bono  se modifica la calse CalcularBono   por lo que rompe con OCP
    

    // este es la  que se extendie  y la clase bono no se modifica solo recive el mismo Objeto o Intefas y calcula  lo que nesecita   automaticamente  nno se crean multiples if
    interface Bono{
        double calcular();
    } 
    public class CalcularBono{
        public double calcular(Bono bono){
            return bono.calcular();
        }
    }
    class BonoNormal{
        @Override
        public double calcular(double sueldo){
            return sueldo* 0.1;
        }

    }

    Bono bono= new BonoNormal();
    CalcularBono calcular= new CalcualrBono();
     calcular.calcular(bono)

  ```

# SESION 2
 # Patron singlenton
 consiste en una sola instancia de clase
 ejemplo:

 ```  java
class ConexionDB {
    // 1. La instancia debe ser static para que pertenezca a la clase
    private static ConexionDB instance;

    // 2. El constructor es privado para evitar el 'new ConexionDB()' externo
    private ConexionDB() {
        // Inicialización de la conexión si es necesario
    }

    // 3. El método debe ser static para poder llamarlo sin instanciar la clase
    public static ConexionDB getInstance() {
        if (instance == null) {
            instance = new ConexionDB();
        }
        return instance;
    }
}
 ``` 

# Patron prototype
consiste en clonar una clase ya existente evitando usar el new  para esto se usa la interface Cloneable
ejemplo:
 ```  java
class Persona implements Cloneable {
    String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    // Sobrescribimos el método clone de Object
    @Override
    public Persona clone() {
        try {
            return (Persona) super.clone(); // Realiza una copia superficial (shallow copy)
        } catch (CloneNotSupportedException e) {
            throw new AssertionError(); // No debería ocurrir ya que implementamos Cloneable
        }
    }
}

 ```



# Patron Factory Metho
consiste en crear objetos sin usar la palabra new
ejemplo:
  ```  java
 interface Transporte {
    void entregar();
}

class Auto implements Transporte {
    public void entregar() {
        System.out.println("Entregado en auto");
    }
}

class Moto implements Transporte {
    public void entregar() {
        System.out.println("Entregado en moto");
    }
}

class TransporteFactory {
    public Transporte getTransporte(String tipo) {
        if (tipo != null && tipo.equalsIgnoreCase("Auto")) {
            return new Auto();
        } else {
            return new Moto();
        }
    }
}
```
# patron abstract factory
 consiste en crear una familia de   objetos
  con relaciones
  ejemplo:
   ```  java
// Interfaces de los productos
interface Silla {
    void crear();
}

interface Mesa {
    void crear();
}

// Productos concretos de la familia "Moderna"
class SillaModerna implements Silla {
    @Override
    public void crear() {
        System.out.println("Creando silla moderna");
    }
}

class MesaModerna implements Mesa {
    @Override
    public void crear() {
        System.out.println("Creando mesa moderna");
    }
}

// Productos concretos de la familia "Clásica"
class SillaClasica implements Silla {
    @Override
    public void crear() {
        System.out.println("Creando silla clásica");
    }
}

class MesaClasica implements Mesa {
    @Override
    public void crear() {
        System.out.println("Creando mesa clásica");
    }
}

// Interfaz de la Abstract Factory
interface FabricaMuebles {
    Silla crearSilla();
    Mesa crearMesa(); // Nota los paréntesis y el tipo de retorno
}

// Fábrica concreta para crear objetos modernos relacionados
class FabricaModerna implements FabricaMuebles {
    @Override
    public Silla crearSilla() {
        return new SillaModerna();
    }
    
    @Override
    public Mesa crearMesa() {
        return new MesaModerna();
    }
}

// Fábrica concreta para crear objetos clásicos relacionados
class FabricaClasica implements FabricaMuebles {
    @Override
    public Silla crearSilla() {
        return new SillaClasica();
    }
    
    @Override
    public Mesa crearMesa() {
        return new MesaClasica();
    }
}

public class Main {
    public static void Main(String[] args) {
        // 1. Decidimos qué familia de muebles queremos (podría ser FabricaModerna o FabricaClasica)
        FabricaMuebles fabrica = new FabricaModerna();
        Silla silla = fabrica.crearSilla();
        Mesa mesa = fabrica.crearMesa();     
        silla.crear(); 
        mesa.crear();  
   
        
        // Si cambiamos de opinión y queremos muebles clásicos, solo cambiamos la fábrica:
        FabricaMuebles fabricaClasica = new FabricaClasica();
        Silla sillaClasica = fabricaClasica.crearSilla();
        Mesa mesaClasica = fabricaClasica.crearMesa();
        
        sillaClasica.crear(); 
        mesaClasica.crear();  
    }
}
```
# Patrón Builder
Consiste en crear un objeto complejo armándolo pieza por pieza.

```java
public class Main {
    public static void main(String[] args) {
        Computadora pc = new ComputadoraBuilder()
                .setCPU("Intel i7")
                .setDisco("512 GB SSD")
                .setRAM("16 GB")
                .build();

        System.out.println(pc); // Imprimirá usando el toString()
    }
}

class Computadora {
    String ram;
    String disco;
    String cpu;

    // Debe ser public porque sobrescribe un método de Object
    @Override
    public String toString() {
        return "CPU: " + cpu + " | RAM: " + ram + " | Disco: " + disco;
    }
}

class ComputadoraBuilder {
    private Computadora comp = new Computadora();

    public ComputadoraBuilder setCPU(String cpu) {
        comp.cpu = cpu;
        return this; // Retorna el builder para encadenar métodos
    }

    public ComputadoraBuilder setRAM(String ram) {
        comp.ram = ram;
        return this;
    }

    public ComputadoraBuilder setDisco(String disco) {
        comp.disco = disco;
        return this;
    }

    public Computadora build() {
        return comp; // Devuelve el objeto ya construido
    }
}
```

# Patrón Adapter
Permite resolver problemas de desajustes de interfaces para que clases con interfaces incompatibles puedan trabajar juntas.

```java
public class Main {
    public static void main(String[] args) {
        ReproductorMultimedia reproductor = new AdaptadorMultimedia(new ReproductorMp4());
        reproductor.reproducir("video.mp4");
    }
}

// Interfaz esperada por el cliente
interface ReproductorMultimedia {
    void reproducir(String archivo);
}

// Clase existente con una interfaz incompatible
class ReproductorMp4 {
    void reproducirMp4(String archivo) {
        System.out.println("Reproduciendo archivo MP4: " + archivo);
    }
}

// El Adaptador implementa la interfaz y traduce la llamada
class AdaptadorMultimedia implements ReproductorMultimedia {
    private ReproductorMp4 reproductorMp4;

    public AdaptadorMultimedia(ReproductorMp4 reproductorMp4) {
        this.reproductorMp4 = reproductorMp4;
    }

    @Override
    public void reproducir(String archivo) {
        reproductorMp4.reproducirMp4(archivo);
    }
}
```

# Patrón Facade
Permite resolver el problema de tener múltiples sistemas complejos o libres, agrupándolos y ofreciendo una interfaz unificada y simple en una sola clase.

```java
public class Main {
    public static void main(String[] args) {
        CineFacade cine = new CineFacade();
        cine.verPelicula("Interstellar");
    }
}

class Luces {
    public void atenuar() {
        System.out.println("Luces atenuadas al 10%.");
    }
}

class Proyector {
    public void encender() {
        System.out.println("Proyector encendido.");
    }
    public void setPelicula(String pelicula) {
        System.out.println("Cargando película: " + pelicula);
    }
}

class Sonido {
    public void activar() {
        System.out.println("Sistema de sonido envolvente activado.");
    }
}

// La Fachada agrupa y simplifica todos los subsistemas anteriores
class CineFacade {
    private Luces luces;
    private Proyector proyector;
    private Sonido sonido;

    public CineFacade() {
        this.luces = new Luces();
        this.proyector = new Proyector();
        this.sonido = new Sonido();
    }

    public void verPelicula(String pelicula) {
        System.out.println("--- Preparando el cine en casa ---");
        luces.atenuar();
        proyector.encender();
        proyector.setPelicula(pelicula);
        sonido.activar();
        System.out.println("¡Disfrute su función!\n");
    }
}
```

#Patron Decorator
permite añadir responsabilidades a un objeto  como añadirle capas a un clase 
ejemplo:
``` java
public interface Notificador{
    void enviar(String mensaje);
}

class NofificadorEmail implements Notificador{
   void enviar(String mensaje){
       System.out.println("enviado email"+ mensaje);
   }
}

abstract class NotificatorDecorator implements Notificador{
   protected Notificador wrapper;
   public NotificatorDecorator(Notificator n){
    this.wrapper=n;
   }

   public void enviar(String mensaje){
      wrapper.enviar(mensaje);
   }
}

class NotificadorSMS extends NotificadorDecorator{
    public NotificadordSMS(Notificador n){
        super(n);
    }
    public void enviar(String mensaje){
        super.enviar(mensaje)
        System.out.prinyl("enviando SMS"+ mensaje)
    }
}
class Main {
    static main(String args[]){
        Notificador notificar= new NotificadorSMS(new NotificadorEmail());
        notificar.enviar("Examen el lunes)

    }
}

```
#Patron Composable
su obejtivo es tener una estructura en forma de arbol donde tanto los objetos simples  como los grupos  se traten de la misma manera
ejemplo:
``` java
 interface Empleado{
   void mostrarDetalles();
 }

class Desarrollador implements Empleado{
     private String nombre;
     public Desarrollador(String nombre){
        this.nombre=nombre;
     }

     void mostrarDetalles(){
         System.out.println(" Desarrolador "+ nombre);
     }
}

class Gerente implements Empleado{
    private String nombre;
    public Gerente(String nombre){
   this.nombre=nombre;
    }
      void mostrarDetalles(){
         System.out.println(" Desarrolador "+ nombre);
     }
}

 class Departamento implements Empleado{
    private List<Empleado> empleados= new ArrayList<>();

    public void addEmpleado(Empleado e){
        empleados.add(e);
    }

     void mostrarDetalles(){
        empleados.ForEach(e->{
           System.out.println(" Empleado "+ nombre);
        })
         
     }
 }
 class Main{
    main(){
        Empleado dev1= new Desarrollador("Ana");
         Empleado dev2= new Desarrollador("Juan");
          Empleado gerente= new Desarrollador("Marta");
          Departamento  depto= new Departamento();
          depto.addEmpleado(dev1)
          depto.addEmpleado(dev2)
          depto.addEmpleado(gerente)
          depto.mostrarDetalles();
         
    }
 }
```

